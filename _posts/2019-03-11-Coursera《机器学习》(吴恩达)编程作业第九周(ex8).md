---
layout:     post
title:      Coursera《机器学习》(吴恩达)编程作业第九周(ex8)
subtitle:   吴恩达机器学习课程编程作业 Week9 /exercise8
date:       2019-03-11
author:     王沛
header-img: img/machine-leaining.jpg
catalog: true
tags:
    - 机器学习
    - MATLAB
---

### 目录
- 必做题
1. [estimateGaussian.m](#1) - Estimate the parameters of a Gaussian distribution with a diagonal covariance matrix
2. [selectThreshold.m](#2) - Find a threshold for anomaly detection
3. [cofiCostFunc.m](#3) - Implement the cost function for collaborative filtering

Coursera[课程地址](https://www.coursera.org/learn/machine-learning/)  
本周作业的官方指导文件可以从这里[**下载pdf**](/download/machine-learning-ex8.pdf)
--
---


<h2 id="1">1. estimateGaussian</h2>

{% highlight MATLAB %}
function [mu sigma2] = estimateGaussian(X)
%ESTIMATEGAUSSIAN This function estimates the parameters of a 
%Gaussian distribution using the data in X
%   [mu sigma2] = estimateGaussian(X), 
%   The input X is the dataset with each n-dimensional data point in one row
%   The output is an n-dimensional vector mu, the mean of the data set
%   and the variances sigma^2, an n x 1 vector
% 

% Useful variables
[m, n] = size(X);

% You should return these values correctly
mu = zeros(n, 1);
sigma2 = zeros(n, 1);

% ====================== YOUR CODE HERE ======================
% Instructions: Compute the mean of the data and the variances
%               In particular, mu(i) should contain the mean of
%               the data for the i-th feature and sigma2(i)
%               should contain variance of the i-th feature.
%

mu = mean(X)';

sigma2 = (var(X,1))';

% =============================================================

end
{% endhighlight %}


<h2 id="2">2. selectThreshold</h2>

{% highlight MATLAB %}
function [bestEpsilon bestF1] = selectThreshold(yval, pval)
%SELECTTHRESHOLD Find the best threshold (epsilon) to use for selecting
%outliers
%   [bestEpsilon bestF1] = SELECTTHRESHOLD(yval, pval) finds the best
%   threshold to use for selecting outliers based on the results from a
%   validation set (pval) and the ground truth (yval).
%

bestEpsilon = 0;
bestF1 = 0;
F1 = 0;

stepsize = (max(pval) - min(pval)) / 1000;
for epsilon = min(pval):stepsize:max(pval)
    
    % ====================== YOUR CODE HERE ======================
    % Instructions: Compute the F1 score of choosing epsilon as the
    %               threshold and place the value in F1. The code at the
    %               end of the loop will compare the F1 score for this
    %               choice of epsilon and set it to be the best epsilon if
    %               it is better than the current choice of epsilon.
    %               
    % Note: You can use predictions = (pval < epsilon) to get a binary vector
    %       of 0's and 1's of the outlier predictions

    predictions = (pval < epsilon);
    
    TP = sum((predictions==1)&(yval==1));
    FP = sum((predictions==1)&(yval==0));
    FN = sum((predictions==0)&(yval==1));
% I'm going to add the 'eps' just in order to eliminate the waring of 'dividion by zero' in MATLAB
% 在除数后面加eps以消除‘division by zero’的warning
    prec = TP/(TP+FP+eps);
    rec = TP/(TP+FN+eps);

    F1=2*prec*rec/(prec+rec+eps);

    % =============================================================

    if F1 > bestF1
       bestF1 = F1;
       bestEpsilon = epsilon;
    end
end

end
{% endhighlight %}

<h2 id="3">3. cofiCostFunc</h2>

{% highlight MATLAB %}
function [J, grad] = cofiCostFunc(params, Y, R, num_users, num_movies, ...
                                  num_features, lambda)
%COFICOSTFUNC Collaborative filtering cost function
%   [J, grad] = COFICOSTFUNC(params, Y, R, num_users, num_movies, ...
%   num_features, lambda) returns the cost and gradient for the
%   collaborative filtering problem.
%

% Unfold the U and W matrices from params
X = reshape(params(1:num_movies*num_features), num_movies, num_features);
Theta = reshape(params(num_movies*num_features+1:end), ...
                num_users, num_features);

            
% You need to return the following values correctly
J = 0;
X_grad = zeros(size(X));
Theta_grad = zeros(size(Theta));

% ====================== YOUR CODE HERE ======================
% Instructions: Compute the cost function and gradient for collaborative
%               filtering. Concretely, you should first implement the cost
%               function (without regularization) and make sure it is
%               matches our costs. After that, you should implement the 
%               gradient and use the checkCostFunction routine to check
%               that the gradient is correct. Finally, you should implement
%               regularization.
%
% Notes: X - num_movies  x num_features matrix of movie features
%        Theta - num_users  x num_features matrix of user features
%        Y - num_movies x num_users matrix of user ratings of movies
%        R - num_movies x num_users matrix, where R(i, j) = 1 if the 
%            i-th movie was rated by the j-th user
%
% You should set the following variables correctly:
%
%        X_grad - num_movies x num_features matrix, containing the 
%                 partial derivatives w.r.t. to each element of X
%        Theta_grad - num_users x num_features matrix, containing the 
%                     partial derivatives w.r.t. to each element of Theta
%

J = 1/2 * sum(sum(((X * Theta') .* R - Y .* R) .^ 2)) + lambda/2 * sum(sum(Theta .^ 2)) + lambda/2 * sum(sum(X .^ 2));

X_grad = ((X * Theta') .* R - Y .* R) * Theta + lambda .* X;
Theta_grad = ((X * Theta') .* R - Y .* R)' * X + lambda .* Theta;

% =============================================================

grad = [X_grad(:); Theta_grad(:)];

end
{% endhighlight %}
