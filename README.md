智能垃圾分类系统作业
1.设计思路
我们项目的实现决定使用pytorch将resnext神经网络用于垃圾分类的实现，利用已经成熟的resnext神经网络算法去实现对垃圾识别与分类，我们需要去着重学习与理解resnext的算法，通过先识别出垃圾图片中物品的类别（比如易拉罐、果皮等），然后查询垃圾分类规则，输出该垃圾图片中物品属于可回收物、厨余垃圾、有害垃圾和其他垃圾中的哪一种。垃圾类别共42类。

2.代码结构
 {repo_root}
  ├── models	//模型文件夹
  ├── utils		//一些函数包
  |   ├── eval.py		// 求精度
  │   ├── misc.py		// 模型保存，参数初始化，优化函数选择
  │   ├── radam.py
  │   └── ...
  ├── args.py		//参数配置文件
  ├── build_net.py		//搭建模型
  ├── dataset.py		//数据批量加载文件
  ├── preprocess.py		//数据预处理文件，生成坐标标签
  ├── train.py		//训练运行文件
  ├── transform.py		//数据增强文件
文件拓扑图

3. 环境设置
python版本为3.6，具体的函数包如下（其实只要是最新版本的就好）：

pytorch>=1.0.1
torchvision==0.2.2
matplotlib>=3.1.0
numpy>=1.16.4
scikit-image
pandas
sklearn
4.运行步骤
建立文件夹data，把garbage_classify全部解压缩到data下 这里附上数据集的下载地址：链接：https://pan.baidu.com/s/1ZI1SNIJGKF1QD8AN3ggfeg 提取码：m7dt

运行preprocess.py，生成训练集和测试集运行文

单张显卡的话，修改arg.py 85行 parser.add_argument('--gpu-id', default='0, 1, 2, 3' 为'--gpu-id', default='0'，同时修改 '--train-batch'，'--test-batch'为适当的数字

你还需要注意除了train_batch以外还要对epoch即训练的总次数设置成一个适当的值。

这里可以给出一个数据：

GTX 1070显卡，8G显存，16G电脑内存，单GPU

train_batch = 2 ( 若是 > 2 ，会报GPU内存不足的问题 )，基本上1个 epoch 大概 45分钟左右

运行train.py

其中args.py文件存放了读取数据集的路径，以及保存模型结果的路径，在本地运行的时候需要修改自身配置的路径

5.遇到的问题及一些注意事项
（1）首先就是，所有的路径必须是纯英文、纯英文、纯英文，重要的事情说三遍！！！
（2）在运行 preprocess.py 的时候要注意生成文件的路径。
（3）如何去查看你电脑有记得GPU呢？访问电脑路径 C:\Program Files\NVIDIA Corporation\NVSMI 通过 cmd 进入此路径，然后运行 nvidia-smi.exe 之后就会给出你电脑的GPU运行情况，以下给出我电脑的 GPU 运行情况，如果有多个的话，会如橘黄色框框中显示的一样，按照 0-n 的序号向下排列，然后根据自己的情况，操作 运行步骤-3。
检查系统gpu

（4）在运行train.py的过程中你有可能会遇到一下问题，提示你说你的 GPU 的内存不足。这时候你得尝试将args.py 中的 train_batch（即 batch_size） 的值设置的小一点，但这也意味着训练一个 epoch 的时间将会非常的漫长，并且一旦值设置的还不够小:
轻则训练第一轮的时候就会提示你 GPU 容量不足并中断程序；
重则他在训练中途会突然中断并对你进行嘲讽（Buy a new RAM），等于需要再花费更多的时间从新训练！！！！
训练一开始就被中断

训练过程中突然中断

这里是 需要这设置的地方
修改train_batch

修改epoch

（5）需要注意生成文件的路径。
