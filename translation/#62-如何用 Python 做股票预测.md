https://towardsdatascience.com/stock-prediction-in-python-b66555171a2

Stock Prediction in Python

Make (and lose) fake fortunes while learning real Python

Trying to predict the stock market is an enticing prospect to data scientists motivated not so much as a desire for material gain, but for the challenge.We see the daily up and downs of the market and imagine there must be patterns we, or our models, can learn in order to beat all those day traders with business degrees. Naturally, when I started using additive models for time series prediction, I had to test the method in the proving ground of the stock market with simulated funds. Inevitably, I joined the many others who have tried to beat the market on a day-to-day basis and failed. However, in the process, I learned a ton of Python including object-oriented programming, data manipulation, modeling, and visualization. I also found out why we should avoid playing the daily stock market without losing a single dollar (all I can say is play the long game)!

When we don’t experience immediate success — in any task, not just data science — we have three options:

Tweak the results to make it look like we were successful
Hide the results so no one ever notices
Show all our results and methods so that others (and ourselves) can learn how to do things better
While option three is the best choice on an individual and community level, it takes the most courage to implement. I can selectively choose ranges when my model delivers a handsome profit, or I can throw it away and pretend I never spent hours working on it. That seems pretty naive! We advance by repeatedly failing and learning rather than by only promoting our success. Moreover, Python code written for a difficult task is not Python code written in vain!

This post documents the prediction capabilities of Stocker, the “stock explorer” tool I developed in Python. In a previous article, I showed how to use Stocker for analysis, and the complete code is available on GitHub for anyone wanting to use it themselves or contribute to the project.

Stocker for Prediction
Stocker is a Python tool for stock exploration. Once we have the required libraries installed (check out the documentation) we can start a Jupyter Notebook in the same folder as the script and import the Stocker class:

