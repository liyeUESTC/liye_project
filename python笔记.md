## 语法规则 ##
- 下列语法中的“if”、“elif”以及“else”要保持对齐。 相对于正文向外超出两个空格。
   {
    if
    elif
    else
    }


- 函数定义后面要加“：”

- “引号”  代表字符串

**np.random产生随机数**

```
import numpy as np
（1）浮点数
A = np.random.random(3)
# 产生[0,1]内的随机浮点数
A1 = np.random.random((2,3))
# 在[0,1]内随机产生2行3列的矩阵。
# 调用函数时，后面加“()”,括号内的数据才是真正需要设置的。所以会出现重括号。
B = np.random.uniform(20,30，5)
# 在[20,30]之间产生5个随机浮点数

（2）整数
D = np.random.randint(10,20，5)
# 在[10，20]之间产生5个随机整数
E = np.random.permutation(10)
# 产生[0,9]的随机顺序的整数
E1 = np.random.permutation(10)[:3]
# 从[0,9]中选取3个随机整数
E2 = np.random.permutation([9,3,4,2,8,6])
# 输出所给数据的随机顺序

（3）在一定范围内按顺序生成数字
x = np.mgird[-1:1:complex(0,n)]
# 在[-1,1]内按从小到大的顺序产生n个数字
```


###列表(list)、元组(tuple)以及数组(array)###
- array [1,2,3].shape  (3L,)
- array [[1,2,3]].shape  (1L,3L)
- array  [[1],[2],[3]].shape   (3L,1L)
- array [1,2,3].reshape(3,1).shape   (3L,1L)
- tuple：代表元组   numpy.ndarray：numpy中的n维元组
- list 没有shape或者reshape等属性，数组(array)操作更加方便，array输出为list的形式。
- list定义利用'[]'，tuple定义利用'()'


### pca类的理解
- pca类有parameters和attributes，调用pca类的attributes时不需要加参数，例：pca.components_.reshape ，但在调用pca类中的其他函数时需要加参数，例：pca.fit(X)。根据需求制定。


**zip函数**

```
 zip函数举例：
 a = [1,2,3]
 b = [4,5,6]
 print zip(a,b)
 [(1, 4), (2, 5), (3, 6)]
```

**plot画图的一般流程**

- `plt.figure()`
- `plt.xlabel('') 横坐标`
- `plt.ylabel('')  纵坐标`
-	`plt.legend(loc = 'lower right')  显示图中各线条意义`
	# 画出各线段所表示的含义（每条曲线表示的意义）
-	`plt.title(title)   标题`
-	`plt.show()   显示图片`

实例如下：
```
plt.figure()
	plt.plot(n_components, pca_scores, 'b', label = 'PCA scores')
	plt.plot(n_components, fa_scores, 'r', label = 'FA scores')
	#plt.axvline and plt.axhline 分别是竖直划线和水平划线的含义
	plt.axvline(rank, color='g', label='TRUTH:%d' %rank, linestyle='-')
	plt.axvline(n_components_pca, color = 'b',
				label = 'PCA CV:%d' % n_components_pca,linestyle='--')
	plt.axvline(n_components_fa, color = 'r',
				label='FactorAnalysis CV:%d' % n_components_fa,
				linestyle='--')
	plt.axvline(n_components_pca_mle, color='k',
				label='PCA MLE:%d' % n_components_pca_mle,linestyle='--')
	
	#compare with other covariance estimators
	plt.axhline(shrunk_cov_score(X), color='violet',
				label='Shrunk Covariance MLE', linestyle='-.')
				
	plt.axhline(lw_score(X), color='orange',
				label='LedoitWolf MLE' % n_components_pca_mle, linestyle='-.')
	
	plt.xlabel('nb of components')
	plt.ylabel('CV scores')
	plt.legend(loc = 'lower right')
	# 画出各线段所表示的含义（每条曲线表示的意义）
	plt.title(title)
	plt.show()
```

**range、np.arange以及xrange区别**




- range多用作循环，range（0,10）返回一个list对象
```
print range(3)  [0, 1, 2]
print range(1,4)  [1, 2]

for i in range(n_row * n_col):
		plt.subplot(n_row, n_col, i+1) # 在每个区域画图

```

- arange是numpy模块中的函数，使用前需要先导入此模块，arange(3):返回array类型对象。
【注：range()中的步长不能为小数，但是np.arange()中的步长可以为小数】
```
n_components = np.arange(0, n_features, 5)  
# n_components   array ([0,5,10,15,...,45]) 
```

- xrange()也是用作循环，只是xrang(0,10)不返回list，返回xrange对象。每次调用返回其中的一个值。 
返回很大的数的时候或者频繁的需要break时候，xrange性能更好。

【注意：python3.x中把xrange()取消了】

**time计时函数**

```
import time as time
t0 = time()
#中间加需要计算时间的代码内容
print time()-t0

```

**reshape函数**

```
faces_centered -= faces_centered.mean(axis=1).reshape(n_samples, -1)
#  faces_centered.mean(axis = 1) 按列求平均值，得到一列
# reshape.(n_samples, -1) 中的‘-1’表示由实际情况而定，不具体指定是多少
```

**enumerate函数**
用于遍历(images)中的索引以及值（下标以及数据）

```
seq = ['one','two','three']
for i ,j in enumerate(seq):
	print i,j
output:
0   one
1   two
2   three

for i, comp in enumerate(images):
	# enumerate 用于遍历images中的index以及value
	# i用于存储索引(index)，comp用于存储值(value)
	plt.subplot(n_row, n_col, i + 1)
```

**fetch_lfw_people获取人脸数据集各参数作用**

```
# 获取人脸数据集
lfw_people = fetch_lfw_people(min_faces_per_person = 70, resize = 0.4)
# print lfw_people 函数把符合条件的照片专为一个(n_samples,n_features)的矩阵
# fetch_lfw_people 函数用于提取lfw(人脸数据集)数据集中的数据
# min_face_per_person 保留文件夹中照片数大于70的人脸数据集
# Each row corresponds to a ravelled face image of original size 62*47 pixels
# 每一行对应一个原始尺寸为62*47像素的脸部图像，默认resize为0.5，如果变为0.4后，size为50*37
n_samples, h, w, = lfw_people.images.shape
# n_samples = 1288, h = 50, w = 37
# 挑选了1288张人脸，其中每个人的人脸照片都超过70张，picture.shape()=50*37
```

**random_state = 42，代表每次产生相同的随机分组**
```
# split train and test data
X_train, X_test, y_train, y_test = train_test_split(
	X, y, test_size = 0.25, random_state = 42)
# 利用train_test_split函数将数据分为训练集和测试集
# random_state 代表是随机数的种子，填写数字，代表每次产生相同的随机数组。
# 数字为0或者不填，每次产生的随机数组都不同 
	
```
**%d、%s以及%.3f**
```
print('Extracting the top %d eigenfaces from %d faces' 
		%(n_components, X_train.shape[0]))
print（'predicted: %s\ntrue:     %s' %(pred_name, true_name)）
print("done in %0.3fs" % train_time)
```

**rsplit函数用法**

```
def title(y_pred, y_test, target_names, i):
	# 返回预测人脸姓和测试人脸姓的对比title
	pred_name = target_names[y_pred[i]].rsplit(' ', 1)[-1]
	# rsplit('',1) 从右边开始，以右边第一个空格为分界，分成两个字符
	#组成一个list
	# 此处代表把'姓'和'名'分开，然后把后面的'姓'提取出来
	# 末尾加[-1]代表引用分割后的列表的最后一个元素
	true_name = target_names[y_test[i]].rsplit(' ', 1)[-1]
	return 'predicted: %s\ntrue:     %s' %(pred_name, true_name)
```

**np.logspace(-2, 0, 30)**

```
shrinkages = np.logspace(-2, 0, 30)
# [-2,0]中挑选30个点,然后取以10为底的对数，
# x = np.random.uniform(-2,0,30)， shrinkages = log(10)(x) 
```

