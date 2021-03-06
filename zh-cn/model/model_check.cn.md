## 数据模型的检测


数据模型的检测主要是针对创建好的数据模型进行一系列的检查，包括检查维表的主键里是否存在重复的值，是否存在数据倾斜的可能性，连接的条件是否合理。这些信息能够帮助用户在构建Cube前就判断出数据模型是否合理，以免在构建Cube时发现问题再修改模型。同时，模型检测还会进行单列以及两列的基数计算，用于辅助优化Cube。这一章将介绍如何在KAP中进行模型检测。

**Step1.** 打开KAP界面，选取一个项目，比如: learn_kylin,  点击左侧导航栏的**建模**，再点击右侧**模型**标签页。

![](images/model_check/24_model_diagnose_1.png)

**Step2.** 对于一个已存在的模型，比如：kylin_sales_model, 点击右上角的菜单，选择**检测**

![](images/model_check/24_model_diagnose_2.png)

**Step3. 设置**

时间范围。要求设置一个获取数据的时间框，因为如果模型有时间分区的列，可以设置起始－终止时间范围来限定获取的数据范围，避免检测全量数据。默认不选择就是检测全量数据。

采样比例。表示在选定的时间段内的数据只进行部分检测，比如，50%比例表示每两条数据只检测一条，以此类推。

![](images/model_check/25_model_check.png)

**Step4.** 设置都完成后，点击提交按钮，则开始检测任务，可以点击左侧导航栏的**监控**观察任务进度。

![](images/model_check/24_model_diagnose_4.png)



另外，如果是创建或者编辑一个模型，在点击**保存**后，会弹出一个对话框询问是否进行模型检测，默认会勾选，提交后将启动一个模型检测任务，同样可以在监控面板观察到进度。

![](images/model_check/25_model_save.png)

如果模型检测顺利完成，建模－模型页面可以看到检测后的状态，本例中模型状态为健康，如果模型有问题将出现对应的状态及详细信息。

![](images/model_check/24_model_diagnose_6.png)

模型检测的主要是检查如下三点：

1. **模型状态检查**：检查连接条件。用连接的生成平表，根据平表的结果来判断连接条件是否合理。如果检查结果有问题，用户要检查连接的条件是否有错，或者源数据是否存在问题。

2. **事实表采样**：检查模型的事实表的数据是否分布均匀。如果数据分布不均匀，在生成平表的时候造成数据处理卡在少数的reducer上。如果检查存在数据分布不均匀的情况，用户要采取必要的手段避免数据倾斜。 

3. **维度表采样**：检查维度表的主键值是否有重复的值。如果有重复的值且超过设定的容忍度将直接终止模型检查。用户考虑是否要进行去重工作。

   ​

结合以上的检查点，模型检测会给出五种状态：

1. 未检测。创建模型的默认状态。
2. 正在检测。表示正在跑检测任务。
3. 健康状态。表示模型检测不存在以上三个错误。
4. 警告状态。表示模型检测存在上述错误的一种。
5. 错误状态。表示模型检测存在上述错误的两种及以上。
