# corevec


## Gaussian Core Vector Regression

A method that uses parameters to reweight both samples and features.

Ideas based off of Vector Generalized Linear Models. See https://en.wikipedia.org/wiki/Vector_generalized_linear_model


More information can be found at:
https://www.askexplain.com/unifying-models-with-interpretable-parameters


Please address any questions to ask@explain.group





```
Example code for log and scale transform with library normalisation, without covariates

library(corevec)
core.v_model <- corevec::corevec(x = x,
                 y = y,
                 covariate_x = rep(0,dim(x)[2]),
                 covariate_y = rep(0,dim(y)[2]),
                 k_dim = 2,
                 max_iter = 150,
                 min_iter = 100,
                 tol = 1e-5,
                 log = T,
                 factor = T,
                 scale = T,
                 select_run = NULL)

# Assign variables for easy access
x <- core.v_model$statistics$x
y <- core.v_model$statistics$y
alpha.L <- core.v_model$statistics$alpha.L
beta <- core.v_model$statistics$beta

# Plotting dimensionality reduction
par(mfrow=c(2,2))
plot(t(alpha.L%*%y),col = as.factor(colnames(y)))
plot(t(alpha.L%*%x),col = as.factor(colnames(x)))
plot(t(cbind(alpha.L%*%y,alpha.L%*%x)),col = as.factor(c(colnames(y),colnames(x))))
plot(t(cbind(alpha.L%*%y,alpha.L%*%x)),col = as.factor(c(rep(5,length(colnames(y))),rep(6,length(colnames(x))))))

# Alignment & Imputation
align.impute_x <- t(alpha.L)%*%alpha.L%*%x
align.impute_y <- t(alpha.L)%*%alpha.L%*%y

# Label projection

# Binary K matrix for labels by reference cell
K <- do.call('rbind',lapply(unique(as.factor(colnames(y))),function(cell.type){
  as.integer(as.factor(colnames(y)) == cell.type) 
}))

label_projection <- t(K%*%t(beta))
colnames(label_projection) <- unique(as.factor(colnames(y)))


```
