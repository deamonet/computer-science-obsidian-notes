# K-Nearest Neighbor Classification(KNN)[](https://www.heywhale.com/api/notebooks/5a5592c64cd36f1bbf3411ed/RenderedContent?cellcomment=1#K-Nearest-Neighbor-Classification(KNN))

# K最近邻分类算法[](https://www.heywhale.com/api/notebooks/5a5592c64cd36f1bbf3411ed/RenderedContent?cellcomment=1#K最近邻分类算法)

**引例：**下面图片中只有三种豆，有三个豆是未知的种类，如何判定他们的种类？  
<img src="https://cdn.kesci.com/images/lab_upload/1515551752589_57434.png" width="500">  
最简单的想法是：未知的豆离哪种豆最近就认为未知豆和该豆是同一种类。  
由此，我们引出最近邻算法的定义：**为了判定未知样本的类别，以全部训练样本作为代表点，计算未知样本与所有训练样本的距离，并以最近邻者的类别作为决策未知样本类别的唯一依据。**  
但是，最近邻算法明显是存在缺陷的，我们来看一个例子。

**问题：**有一个未知形状X(图中绿色的圆点)，如何判断X是什么形状?  
<img src="https://cdn.kesci.com/images/lab_upload/1515551967402_56560.png" width="400">  
显然，通过上面的例子我们可以明显发现最近邻算法的缺陷——**对噪声数据过于敏感。**  
为了解决这个问题，我们可以可以把位置样本周边的多个最近样本计算在内，扩大参与决策的样本量，以避免个别数据直接决定决策结果。  
由此，我们引进**K-最近邻算法（KNN）**。

K-最近邻算法是最近邻算法的一个延伸。基本思路是：**选择未知样本一定范围内确定个数的K个样本，该K个样本大多数属于某一类型，则未知样本判定为该类型。**  
下面借助图形解释一下。  
<img src="https://cdn.kesci.com/images/lab_upload/1515552155971_60074.png" width="400">

KNN算法的实现步骤:

-   step.1---初始化距离为最大值
-   step.2---计算未知样本和每个训练样本的距离dist
-   step.3---得到目前K个最临近样本中的最大距离maxdist
-   step.4---如果dist小于maxdist，则将该训练样本作为K-最近 邻样本
-   step.5---重复步骤2、3、4，直到未知样本和所有训练样本的 距离都算完
-   step.6---统计K个最近邻样本中每个类别出现的次数
-   step.7---选择出现频率最大的类别作为未知样本的类别

接下来，我们展示一个KNN的实际应用案例：**用KNN根据身高体重判断是否肥胖**

import numpy as np
from sklearn import neighbors
from sklearn.metrics import precision_recall_curve
from sklearn.metrics import classification_report
from sklearn.cross_validation import train_test_split

#17人的身高体重数据data[身高m,体重kg]
data= [[1.5,40],[1.5,50],[1.5,60],
       [1.6,40],[1.6,50],[1.6,60],[1.6,70],
       [1.7,50],[1.7,60],[1.7,70],[1.7,80],
       [1.8,60],[1.8,70],[1.8,80],[1.8,90],
      [1.9,80],[1.9,90]]
#labels记录该17人是否肥胖（1表示肥胖）
labels=[0,1,1,0,0,1,1,0,0,1,1,0,0,1,1,0,1]

x = np.array(data)
y = np.array(labels)

#拆分训练数据与测试数据 
x_train, x_test, y_train, y_test = train_test_split(x, y, test_size = 0.2)

#训练KNN分类器
clf = neighbors.KNeighborsClassifier(algorithm='auto')
clf.fit(x_train, y_train)

KNeighborsClassifier(algorithm='auto', leaf_size=30, metric='minkowski',
           metric_params=None, n_jobs=1, n_neighbors=5, p=2,
           weights='uniform')

这样，我们的分类器clf就训练完成了，在上行的输出中，我们可以看到分类器的一些参数，笔者解释几个重要的：

-   algorithm：分类器的算法，sklearn的KNN分类器可以设置3种算法：‘brute’，‘kd_tree’，‘ball_tree’。如果不知道用哪个好，设置‘auto’让KNeighborsClassifier自己根据输入去决定。
-   n_neighbors：默认值为5，表示查询k个最近邻的数目
-   p：属性（parameter）的个数，默认为2，本例子中为身高和体重两个属性

其他参数的解释请参阅[KNN文档](http://scikit-learn.org/dev/modules/generated/sklearn.neighbors.NearestNeighbors.html#sklearn.neighbors.NearestNeighbors)

#测试结果的打印
answer = clf.predict(x)
print("分类结果:",answer)
print("正确结果:",y)
print("准确率:",np.mean( answer == y))

分类结果: [1 1 1 1 1 1 1 1 0 1 1 0 0 1 1 1 1]
正确结果: [0 1 1 0 0 1 1 0 0 1 1 0 0 1 1 0 1]
准确率: 0.705882352941

当然KNN算法也有缺陷，请看下例：  
![image description](media/image_description.png)  
我们看到，对于位置样本X，通过KNN算法，我们显然可以得到X应属于红点，但对于位置样本Y，通过KNN算法我们似乎得到了Y应属于蓝点的结论，而这个结论直观来看并没有说服力。

KNN算法的改进：**分组快速搜索近邻法（K-Means）**  
其基本思想是：将样本集按近邻关系分解成组，给出每组质心的位置，以质心作为代表点，和未知样本计算距离，选出距离最近的一个或若干个组，再在组的范围内应用一般的knn算法。由于并不是将未知样本与所有样本计算距离，故该改进算法可以减少计算量，但并不能减少存储量。  
<img src="https://cdn.kesci.com/images/lab_upload/1515555520507_7402.png" width="500">  
关于K-Means算法的案例，可以参考这个项目[K-Means控卫聚类](https://www.kesci.com/apps/home/project/58acf053d2445916845b4012)  
，笔者在此就不再举例了。

KNN和K-Means的区别主要体现在以下方面：  
![image description](media/image_description.png)