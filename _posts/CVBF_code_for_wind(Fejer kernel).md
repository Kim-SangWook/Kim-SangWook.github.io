---
title: "CVBF_wind_data"
author: "2020020335_김상욱"
date: '2021 4 14 '
output:
  pdf_document: default
header-includes: \usepackage{kotex}
---

# wind data 불러오기 및 변환

$$
X=\left\{\begin{array}{ll}
-\frac{\pi}{2}\left(\frac{W}{90}-1\right), & 0 \leq W \leq 270 \\
-\frac{\pi}{2}\left(\frac{W}{90}-5\right), & 270<W \leq 360 .
\end{array}\right.
$$

```{r}
setwd("C:/coding/R/BF/simulation_vonMises/wind-direction")
# data
x1=scan('wind-direction.txt')
w=360*(x1+pi)/(2*pi)

x=-pi/2*(w/90-1)*(w <=270)-pi/2*(w/90-5)*(270 < w)
```
# von Mises density 정의

$$
f(x \mid \mu, \kappa)=\frac{1}{2 \pi I_{0}(\kappa)} e^{\kappa \cos (x-\mu)}, \quad-\pi \leq x \leq \pi
$$
## 2 mixture von Mises density
$$
f\left(x \mid \mu_{1}, \mu_{2}, \kappa_{1}, \kappa_{2}, w\right)=w f\left(x \mid \mu_{1}, \kappa_{1}\right)+(1-w) f\left(x \mid \mu_{2}, \kappa_{2}\right), \quad-\pi \leq x \leq \pi
$$
```{r}
#1
vonMises=function(x,kappa,mu){
f=exp(kappa*cos(x-mu))/(2*pi*besselI(kappa,0))
f
}

#2
vonmix.like=function(par,x){
mu1=par[1]
kappa1=par[2]
mu2=par[3]
kappa2=par[4]
w=par[5]
f1=vonMises(x,kappa1,mu1)
f2=vonMises(x,kappa2,mu2)
f=w*f1+(1-w)*f2
-prod(f)
}
```

# von Mises 주변우도 추정할 때 필요한 함수

```{r}
# von Mises 모수를 제안분포 (다변량정규분포)애서 샘플링 할 때 필요한 다변량 정규분포 모수의 초기값 구하기.
vonMCMC.norm=function(x,theta,nreps,Sigma){
MU=theta
Siginv=solve(Sigma)
Theta=matrix(0,nreps,5)
for(j in 1:nreps){
par=as.vector(rjmvnorm(1,MU,Sigma))
mu1=par[1]
mu2=par[3]
kappa1=par[2]
kappa2=par[4]
w=par[5]
current=-vonmix.like(theta,x)
Indc=(sign(theta[3]-theta[1])+1)/2
postc=2*Indc*current*dexp(theta[2])*dexp(theta[4])/(2*pi)^2
propc=exp(-0.5*matrix(theta-MU,1,5) %*% Siginv %*% matrix(theta-MU,5,1))
gen=-vonmix.like(par,x)
Indg=(sign(mu2-mu1)+1)/2
postg=2*Indg*gen*dexp(kappa1)*dexp(kappa2)/(2*pi)^2
propg=exp(-0.5*matrix(par-MU,1,5) %*% Siginv %*% matrix(par-MU,5,1))
ratio=postg*propc/(postc*propg)
prob=min(1,ratio)
u=runif(1)
if(u <= prob) theta=par
Theta[j,]=theta
}
Theta
}
# 제안분포에서 모수 샘플링 for importance sampling of marginal likelihood
rjmvnorm=function(n,mu,Sigma){
  p=length(mu)
  X=matrix(rnorm(p*n),p,n)
  Sigroot=t(chol(Sigma))
  X=Sigroot %*% X + mu
  t(X)
}
```


# CVBF 함수 정의

```{r}
BF.mix2=function(y,x,kmax,nreps,MU,Sigma){
Siginv=solve(Sigma)
Det=sqrt(det(Sigma))
# Fejer series estimate 아래 코드를 보면 
m=length(y)
n=length(x)
M=t(matrix(x,n,kmax))
M=M*(1:kmax)
MS=sin(M)
MC=cos(M)
out=Fourier(y,kmax) # 푸리에함수 Fejer함수에서 M1
phiC=out[[1]]
phiS=out[[2]]

priork=1/(1+(0:kmax))^2
priork=priork/sum(priork)
mFejer=0:kmax
mFejer[1]=priork[1]/(2*pi)^n

for(k in 1:kmax){
wght1=(1-(1:k)/(k+1))*as.vector(phiC[1,1:k])
wght2=(1-(1:k)/(k+1))*as.vector(phiS[1,1:k])
s1=matrix(wght1,1,k) %*% MC[1:k,]
s2=matrix(wght2,1,k) %*% MS[1:k,]
Fest=(1+2*as.vector(s1+s2))/(2*pi) # Estimate
mFejer[k+1]=prod(Fest)*priork[k+1]
}
num=sum(mFejer) # 분자 : 커널추정으로 구한 주변우도

# 분모 : importance sampling으로 von Mises의 주변우도 구한다.
Theta=rjmvnorm(nreps,MU,Sigma)
like=1:nreps
for(j in 1:nreps){
par=Theta[j,]
like[j]=0
if(par[1]>(-pi) & par[1]<pi & par[3]>(-pi) & par[3]<pi & par[2]>0 & par[4]>0 & par[5]>0 & par[5]<1){

    like[j]=-vonmix.like(par,x) # f(x_i | theta) 다 곱한것
    Ind=(sign(par[3]-par[1])+1)/2# mu1 < mu2 이므로 항상 1
    like[j]=2*Ind*like[j]*dexp(par[2])*dexp(par[4])/(2*pi)^2 # 밀도함수 * theta의 사전분포들
    like[j]=like[j]*Det*(2*pi)^(5/2) # 뒤에 디터민과 2pi는 제안분포 g의 분모의 있는값들임.

g=exp(-0.5*matrix(par-MU,1,5) %*% Siginv %*% matrix(par-MU,5,1))

like[j]=like[j]/g # j개의 샘플을 평균 취해주면 주변우도 추정치 나옴 (importance sampling)
}
}
denom=mean(like)
list(mFejer/num,num/denom,num,denom)
}
```

# CVBF에서 사용된 푸리에 함수

```{r}
Fourier=function(x,k){
n=length(x)
M=t(matrix(x,n,k))
M=M*(1:k)
phiS=sin(M) %*% matrix(1,n,1)/n
phiC=cos(M) %*% matrix(1,n,1)/n
list(t(phiC),t(phiS))
}
```


