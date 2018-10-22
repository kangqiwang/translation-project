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

stocker 的分析能力能被用作发现数据中的总趋势和模式，但是我们将关注在预测未来的价格


Predictions in Stocker are made using an additive model which considers a time series as a combination of an overall trend along with seasonalities on different time scales such as daily, weekly, and monthly.

在stocker中的预测是被用作一个加性模型，此模型视作时间序列作为整体趋势的组合与不同时间尺度的组合（如每日，每周，每月）

Stocker uses the prophet package developed by Facebook for additive modeling. Creating a model and making a prediction can be done with Stocker in a single line:

Stocker 用被Facebook开发的开发包为了加性模型。stocker可以用一行创造了一个模型并进行预测。

	# predict days into the future
	model, model_data = amazon.create_prophet_model(days=90)
	Predicted Price on 2018-04-18 = $1336.98

![](https://cdn-images-1.medium.com/max/1600/1*mzWix3jo4sqEj17LPD8KyA.png)


Notice that the prediction, the green line, contains a confidence interval.

注意这个预测，绿线包含了置信区间。

This represents the model’s uncertainty in the forecast. 

这代表了模型的不确定性。

In this case, the confidence interval width is set at 80%, meaning we expect that this range will contain the actual value 80% of the time. 

在这个例子中，置信区间宽度被设置成80%，意味着我们期望这个区间内将包含。

The confidence interval grows wide further out in time because the estimate has more uncertainty as it gets further away from the data. 

这个置信区间随着时间的流逝宽度变大因为估计值随着时间的越来越远而具有更多的不确定性。

Any time we make a prediction we must include a confidence interval. 

每当我们做出预测时，我们必须包含置信区间。

Although most people tend to want a simple answer about the future, our forecast must reflect that we live in an uncertain world!

虽然对于未来更多的人想要一个简单答案，但是我们的预测必须展现出我们生活在一个不确定的世界中.

Anyone can make stock predictions: simply pick a number and that’s your estimate (I might be wrong, but I’m pretty sure this is all people on Wall Street do). 

任何人可以进行股票预测：简单的选一个数字，然后说这是我的预测（我可能错了，但是我非常确定这个就是所有在华尔街的人做的事情）。

For us to trust our model we need to evaluate it for accuracy.There are a number of methods in Stocker for assessing model accuracy.

为了我们去相信我们的模型，我们需要去评估他的准确率。Stocker有一系列的方法去评估模型的准确度。


##Evaluate Predictions
##评估预测


To calculate accuracy, we need a test set and a training set. We need to know the answers — the actual stock price — for the test set, so we will use the past one year of historical data (2017 in our case).

我们需要一个测试集和一个训练集去计算精确度。我们需要知道答案作为测试集-实际的股票价格，因此我们将要用过去一年的历史数据（2017 年）

When training, we do not let our model see the answers to the test set, so we use three years of data previous to the testing time frame (2014–2016).

当训练时，我们不能让我们的模型知道测试集，因此我们用此前三年的数据作为训练集（2014-2016）。

The basic idea of supervised learning is the model learns the patterns and relationships in the data from the training set and then is able to correctly reproduce them for the test data.

监督学习的基本思想是模型从训练集中学习数据的模式和关系，然后模型能够正确的从测试集中找到答案。

We need to quantify our accuracy, so we using the predictions for the test set and the actual values, we calculate metrics including average dollar error on the testing and training set, the percentage of the time we correctly predicted the direction of a price change, and the percentage of the time the actual price fell within the predicted 80% confidence interval.

我们需要量化我们的精准度，因此我们用预测值和实际值，我们计算矩阵包括测试集和训练集的平均美元错误率，我们正确预测的价格变动百分比，和在80%置信区间的时间价格下降的时间百分比。

All of these calculations are automatically done by Stocker with a nice visual:
所有这些计算都由stocker自动完成，并且给出不错的数据可视化。


	amazon.evaluate_prediction()
	Prediction Range: 2017-01-18 to 2018-01-18.

	Predicted price on 2018-01-17 = $814.77.
	Actual price on    2018-01-17 = $1295.00.

	Average Absolute Error on Training Data = $18.21.
	Average Absolute Error on Testing  Data = $183.86.

	When the model predicted an increase, the price increased 57.66% of the time.
	When the model predicted a  decrease, the price decreased  44.64% of the time.

	The actual value was within the 80% confidence interval 20.00% of the time.

![](https://cdn-images-1.medium.com/max/1600/1*B-1irHXJVouQAQYKgw6rTw.png)


Those are abysmal stats! We might as well have flipped a coin. If we were using this to invest, we would probably be better off buying something sensible like lottery tickets. However, don’t give up on the model just yet. We usually expect a first model to be rather bad because we are using the default settings (called hyperparameters). If our initial attempts are not successful, we can turn these knobs to make a better model. There are a number of different settings to adjust in a Prophet model, with the most important the changepoint prior scale which controls the amount of weight the model places on shifts in the trend of the data.


