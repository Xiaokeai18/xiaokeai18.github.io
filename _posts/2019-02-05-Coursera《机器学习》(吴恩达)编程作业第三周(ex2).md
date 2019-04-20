---
layout:     post
title:      Coursera《机器学习》(吴恩达)编程作业第三周(ex2)
subtitle:   吴恩达机器学习课程编程作业 Week3 /exercise2
date:       2019-02-05
author:     王沛
header-img: img/machine-leaining.jpg
catalog: true
tags:
    - 机器学习
    - MATLAB
---

### 目录
- 必做题
1. [plotData.m](#1) 
2. [sigmoid.m](#2)
3. [costFunction.m](#3)
4. [predict.m](#4)  
5. [costFunctionReg.m](#5)

Coursera[课程地址](https://www.coursera.org/learn/machine-learning/)  
本周作业的官方指导文件可以从这里[**下载pdf**](/download/machine-learning-ex2.pdf)
--
---


<h2 id="1">1. plotData</h2>

{% highlight MATLAB %}
function plotData(X, y)
%PLOTDATA Plots the data points X and y into a new figure 
%   PLOTDATA(x,y) plots the data points with + for the positive examples
%   and o for the negative examples. X is assumed to be a Mx2 matrix.

% Create New Figure
figure; hold on;

% ====================== YOUR CODE HERE ======================
% Instructions: Plot the positive and negative examples on a
%               2D plot, using the option 'k+' for the positive
%               examples and 'ko' for the negative examples.
%
pos = find(y==1);neg = find(y==0);
plot(X(pos,1), X(pos,2), "k+", "LineWidth", 2, "MarkerSize", 7);
plot(X(neg,1), X(neg,2), "ko", "LineWidth", 2, "MarkerSize", 7);

% =========================================================================

hold off;

end
{% endhighlight %}


<h2 id="2">2. sigmoid</h2>

{% highlight MATLAB %}
function g = sigmoid(z)
%SIGMOID Compute sigmoid function
%   g = SIGMOID(z) computes the sigmoid of z.

% You need to return the following variables correctly 
g = zeros(size(z));

% ====================== YOUR CODE HERE ======================
% Instructions: Compute the sigmoid of each value of z (z can be a matrix,
%               vector or scalar).
g = 1./(1+exp(-z));

% =============================================================

end
{% endhighlight %}

<h2 id="3">3. costFunction</h2>

{% highlight MATLAB %}
function [J, grad] = costFunction(theta, X, y)
%COSTFUNCTION Compute cost and gradient for logistic regression
%   J = COSTFUNCTION(theta, X, y) computes the cost of using theta as the
%   parameter for logistic regression and the gradient of the cost
%   w.r.t. to the parameters.

% Initialize some useful values
m = length(y); % number of training examples

% You need to return the following variables correctly 
J = 0;
grad = zeros(size(theta));
% ====================== YOUR CODE HERE ======================
% Instructions: Compute the cost of a particular choice of theta.
%               You should set J to the cost.
%               Compute the partial derivatives and set grad to the partial
%               derivatives of the cost w.r.t. each parameter in theta
%
% Note: grad should have the same dimensions as theta
%

J = (-y'*log(sigmoid(X*theta))-(1-y)'*log(1-sigmoid(X*theta)))/m;
grad = (X'*(sigmoid(X*theta)-y))/m;

% =============================================================

end
{% endhighlight %}


<h2 id="4">4. predict</h2>

{% highlight MATLAB %}
function p = predict(theta, X)
%PREDICT Predict whether the label is 0 or 1 using learned logistic 
%regression parameters theta
%   p = PREDICT(theta, X) computes the predictions for X using a 
%   threshold at 0.5 (i.e., if sigmoid(theta'*x) >= 0.5, predict 1)

m = size(X, 1); % Number of training examples
% You need to return the following variables correctly
p = zeros(m, 1);

% ====================== YOUR CODE HERE ======================
% Instructions: Complete the following code to make predictions using
%               your learned logistic regression parameters. 
%               You should set p to a vector of 0's and 1's
%
pos = find(sigmoid(X*theta) >= 0.5);
neg = find(sigmoid(X*theta) < 0.5);
p(pos) = 1;
p(neg) = 0;

% =========================================================================

end
{% endhighlight %}

<h2 id="5">5. costFunctionReg</h2>

{% highlight MATLAB %}
function [J, grad] = costFunctionReg(theta, X, y, lambda)
%COSTFUNCTIONREG Compute cost and gradient for logistic regression with regularization
%   J = COSTFUNCTIONREG(theta, X, y, lambda) computes the cost of using
%   theta as the parameter for regularized logistic regression and the
%   gradient of the cost w.r.t. to the parameters. 

% Initialize some useful values
m = length(y); % number of training examples

% You need to return the following variables correctly 
J = 0;
grad = zeros(size(theta));

% ====================== YOUR CODE HERE ======================
% Instructions: Compute the cost of a particular choice of theta.
%               You should set J to the cost.
%               Compute the partial derivatives and set grad to the partial
%               derivatives of the cost w.r.t. each parameter in theta

J = (-y'*log(sigmoid(X*theta))-(1-y)'*log(1-sigmoid(X*theta)))/m + lambda/m/2*theta(2:size(theta))'*theta(2:size(theta));
grad = (X'*(sigmoid(X*theta)-y))/m + lambda/m*theta;
grad(1) = X(:,1)'*(sigmoid(X*theta)-y)/m;

% =============================================================

end
{% endhighlight %}
