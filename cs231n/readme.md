input -> conv -> BN -> relu -> fc -> dropout 


relu在BN后面，经过BN,学习到数据的均值gamma，然后再经过relu激活。防止先经过relu导致失效relu饱和，全激活或者全不激活。


drop 只在fc后面。


test的BN层的batch均值为train的最后一个batch的均值。

train的最后一个batch的均值是该batch均值的0.1和上一个batch均值的0.9的加权平均。
