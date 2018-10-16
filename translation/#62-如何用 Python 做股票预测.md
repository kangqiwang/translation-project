https://towardsdatascience.com/stock-prediction-in-python-b66555171a2

Stock Prediction in Python

用 python 做股票预测 

Make (and lose) fake fortunes while learning real Python

边学python，边挣虚拟财富

Trying to predict the stock market is an enticing prospect to data scientists motivated not so much as a desire for material gain, but for the challenge.

对于数据科学家来说试图预测股票市场有着一个诱人的前景，对他们来说不仅仅是物质的渴望，更多的是为了挑战。

We see the daily up and downs of the market and imagine there must be patterns we, or our models, can learn in order to beat all those day traders with business degrees. 

我们看到每日起伏的市场想象下一定有模式或者模型我们可以学习为了去击败专业的股票操盘手

Naturally, when I started using additive models for time series prediction, I had to test the method in the proving ground of the stock market with simulated funds. 

当然，当我开始用可加模型为时间序列预测，我不得不测试这个方法在模拟资金的股票市场中。

Inevitably, I joined the many others who have tried to beat the market on a day-to-day basis and failed. However, in the process, I learned a ton of Python including object-oriented programming, data manipulation, modeling, and visualization.

不可避免的，我加入了许多其他人试图去击败市场在日常的基础上并且失败。但是，在过程中，我学到了python包括面向对象编程，数据处理，建模和可视化。

I also found out why we should avoid playing the daily stock market without losing a single dollar (all I can say is play the long game)!

我也弄清楚了为什么我们应该避免在不损失一块钱的情况下进入股票市场。我只能说的是玩一个长期的游戏

![](https://cdn-images-1.medium.com/max/600/1*iUWOsKFHMOY-XhbdyGV45g.png)
![](https://cdn-images-1.medium.com/max/600/1*p6u10H8fxhESbYh0AjTJwg.png)

When we don’t experience immediate success — in any task, not just data science — we have three options:

当我们没有任何经验取得成功-在任何任务中， 不仅仅是数据科学-我们有三个选项：

1.	Tweak the results to make it look like we were successful
1.	调整结果去使他看起来我们是成功的
2.	Hide the results so no one ever notices
2.	隐藏结果没有人会注意到
3.	Show all our results and methods so that others (and ourselves) can learn how to do things better
3.	显示我们所有的结果和方法以便其他人（和自己）能

While option three is the best choice on an individual and community level, it takes the most courage to implement.

虽然三个选项对个人和社区来书是最好的选项，但他们需要鼓起最大的勇气去实现

I can selectively choose ranges when my model delivers a handsome profit, or I can throw it away and pretend I never spent hours working on it. 

我能有选择的选择一个范围当我们的模型获得丰厚的利润时，或者是我能扔掉它并且假装我从没有花费数小时在它上面。

That seems pretty naive! We advance by repeatedly failing and learning rather than by only promoting our success.

这看起来很天真！ 我们预先通过反复的失败和学习而不是宣传我们的成功。

Moreover, Python code written for a difficult task is not Python code written in vain!
而且，为一项艰难任务写的python代码不是徒劳的。


This post documents the prediction capabilities of Stocker, the “stock explorer” tool I developed in Python.

这个文章记录了stocker的预测能力，这个我用python开发的股票探索者工具

[In a previous article](https://towardsdatascience.com/stock-analysis-in-python-a0054e2c1a4c), I showed how to use Stocker for analysis, and the complete code is available on GitHub for anyone wanting to use it themselves or contribute to the project.

[在上一篇文章中](https://towardsdatascience.com/stock-analysis-in-python-a0054e2c1a4c)，我展示了怎么样用stocker进行分析，和[完成的代码在github上](https://github.com/WillKoehrsen/Data-Analysis/tree/master/stocker)可供任何想要用的人使用或者是贡献在这个项目上。

Stocker for Prediction

预测的 stocker
 
Stocker is a Python tool for stock exploration.

stocker 是一个python 工具用来进行股市的探索。

Once we have the required libraries installed (check out the documentation) we can start a Jupyter Notebook in the same folder as the script and import the Stocker class:

一旦我们安装了所需的库（查看文档）我们可以启动Jupyther notebook 在于脚本一样的文件夹中 并且 引用 stocker 类


	from stocker import Stocker


The class is now accessible in our session. We construct an object of the Stocker class by passing it any valid stock ticker (bold is output):

这节课现在可以在我们课程中访问。 我们建立一个stocker类对象通过传递任何有效的股票代码（金色的是输出）

	amazon = Stocker('AMZN')
	AMZN Stocker Initialized. Data covers 1997-05-16 to 2018-01-18.

Just like that we have 20 years of daily Amazon stock data to explore! Stocker is built on the Quandl financial library and with over 3000 stocks to use. We can make a simple plot of the stock history using the plot_stockmethod:

就像是我们有20年的每日亚马逊股票去探索！stocker 被Quandl 金融库建立并且有着超过3000个股票可以使用。我们可以通过用plot_stockmethod去绘制一个简单的股票历史。


	amazon.plot_stock()
	Maximum Adj. Close = 1305.20 on 2018-01-12.
	Minimum Adj. Close = 1.40 on 1997-05-22.
	Current Adj. Close = 1293.32.

![](https://cdn-images-1.medium.com/max/800/1*zUkf-FSxa9CJh-9C9iUWlg.png)

The analysis capabilities of Stocker can be used to find the overall trends and patterns within the data, but we will focus on predicting the future price.

stocker 

Predictions in Stocker are made using an additive model which considers a time series as a combination of an overall trend along with seasonalities on different time scales such as daily, weekly, and monthly. Stocker uses the prophet package developed by Facebook for additive modeling. Creating a model and making a prediction can be done with Stocker in a single line:

