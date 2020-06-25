# MATLAB-
为研究我国民航客运量现状，以民航客运量作为因变量y，以国民收入、铁路客运量、来华旅游人数为主要影响因素。根据《中国统计年鉴》获得统计数据。

【分析】

涉及MATLAB语言三方面功能，包括数值计算、绘图、编程。

1、 利用MATLAB数值计算功能。分析民航客运量随多种因素影响的变化情况，采用线性回归分析得到数据拟合结果，最终得到我国近年民航客运量数据图。
2、 利用MATLAB的编程功能将数据进一步格式化输出。
3、 利用MATLAB绘图功能进行曲线图绘制。

【代码与解释】
% 岭回归
clear;
load('data.mat')
% 第一列：年份    第二列：y（民航客运量）
% x1-x5:国民收入（亿元）、民用汽车拥有量（万辆）、铁路客运量（万人）、民航航线里程（万公里）、来华旅游入境人数（万人）
% x是[x1,x2,x3,x4,x5] 
x = [data(:,3:7)];
y = [data(:,2)];
%样本容量n和自变量的个数p
[n,p] = size(x);
% X是[1,x1,x2,x3,x4,x5]
X = [ones(n,1) x];
% 普通最小二乘估计
B = inv((X'* X))*X'*y;
y_Predictive = X * B;
% 数据标准化
X_ = x;
X_ = (X_ - mean(X_)) ./ sum((X_ - mean(X_)).^2).^0.5;
Y_ = y;
Y_ = (Y_ - mean(Y_)) ./ sum((Y_ - mean(Y_)).^2)^0.5;
% 标准化回归
B_std = inv((X_'* X_))*X_'*Y_;
y_Predictive_std =  X_ * B_std;
% 计算不同k值下的回归系数
k = [0:0.02:0.3];
for i = 1:length(k)
    B_rr(i,:) = inv((X_'* X_ + k(i) * eye(p))) * X_' * Y_;
end
% 绘制岭迹
figure(1); 
plot(k,B_rr,'+')
title('岭迹分析图')
xlabel('k值')
ylabel('回归系数值')
legend()
% k值取0.2，再次计算回归系数
B_ling = inv((X'* X + k(11) * eye(p+1)))*X'*y;
y_Predictive_ling = X * B_ling;
% 岭回归标准化
y_Predictive_ling_std = X_ * B_rr(11,:)';
% 绘制原始数据与普通最小二乘估计值和岭回归估计值
figure(2); 
plot(data(:,1),y,'b',data(:,1),y_Predictive,'r*',data(:,1),y_Predictive_ling,'k+')
title('估计值与真实值')
xlabel('年份')
ylabel('民航客运量y')
legend('真实值','普通最小二乘估计值','岭回归估计值')
% 绘制标准化最小二乘估计值与原数据标准化数据
figure(3); 
plot(data(:,1),Y_,'b',data(:,1),y_Predictive_std,'r*',data(:,1),y_Predictive_ling_std,'k+')
legend('原数据标准化','标准化最小二乘估计','岭回归标准化')
title('标准化数据')
xlabel('年份')
ylabel('标准化后民航客运量y')
【running result】
