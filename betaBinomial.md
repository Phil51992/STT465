
## Example: Effect of Sample Size on Likelihood and Posterior Inferences

The following example displays the likelihood and posterior density (both up to a constant)
of the Beta-Binomial model, assuming that a sample has render a sample mean of 0.3 (average
number of successes in the sample) with sample size being varied from 10 to 100.

```R
# Data
  N=c(5,10,20,50,100);
  cols=c('black','darkgreen','blue','orange','red') # colors I will use for each sample size
  xBar=.1
  
# Prior
  shape1=3
  shape2=3
    
# Grid of values for theta
 theta=seq(from=1/1000,to=999/1000,by=1/1000)
 

# Likelihood
  L=matrix(nrow=length(theta),ncol=length(N))
  
  logLik=function(xBar,n,theta){
  	nSuc=round(xBar*n)
  	nFail=(n-nSuc)
  	log_lik=nSuc*log(theta)+nFail*log(1-theta)
  	return(log_lik)
  }
  
  for(i in 1:length(N)){
  	L[,i]=exp(logLik(xBar=xBar,n=N[i],theta))
  }
  
# Posterior density
  PD=L
  for(i in 1:length(N)){
  	PD[,i]=dbeta(x=theta,shape1=xBar*N[i]+shape1,shape2=(1-xBar)*N[i]+shape2)
  }

 # Scaling to get nice plots
   K=max(c(as.vector(PD),as.vector(L)))
   for(i in 1:length(N)){
   	  PD[,i]=PD[,i]/K
   	  L[,i]=L[,i]/K
   }

 plot(numeric()~numeric(),col=1,lty=1,ylim=c(0,1),xlim=range(theta),xlab=expression('theta'))
 
 for(i in 1:length(N)){
 	lines(x=theta,y=PD[,i],col=cols[i],lty=1)
 }
 # Adding the prior
 lines(x=theta,y=dbeta(x=theta,shape1=shape1,shape2=shape2)/K,col='grey',lwd=2)
 
 # Adding prior mean and the maximum likelihood estimator
  abline(v=c(xBar,shape1/(shape1+shape2)),col=1,lty=2)
```
 

