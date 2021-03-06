# upwork-scraping
Project aimed to provide set of utilities to scrape some useful information from UpWork. I develop it according to my personal need but if you have an idea feel free to create an issue.

## full_data_parser.ipynb
It's a ipython notebook with some data analysis. I am planning to add more analysis here, but at this point correlation analysis was made (the bigger the absolute value of the coefficient the better):

```
data.combinedTotalEarnings            0.015510
data.combinedTotalRevenue             0.016040
data.hourlyRate.amount               -0.418419
data.nss100                          -0.622634
data.recno                            0.262488
data.totalFpJobs                     -0.209804
data.totalHourlyJobs                 -0.363204
data.totalHoursBilled                -0.297878
data.totalPassedTests                -0.291060
data.totalPortfolioItems             -0.055847
page                                  0.999421
rank                                  1.000000
```
As you can see your earnings (`data.combinedTotalEarnings` and `data.combinedTotalRevenue`) and number of items in your protfolio (`data.totalPortfolioItems`) do not affect your rank a lot. Your success score (`data.nss100`) is the most valuable parameter in terms of your rank.

UpWork values fixed-price jobs (`data.totalFpJobs`)  more than hourly-paid jobs (`data.totalHourlyJobs`). And the number of your hourly-paid jobs (`data.totalHourlyJobs`) is more imortant than how many hours you worked (`data.totalHoursBilled`).

The surprise to me that the number of tests passed (`data.totalPassedTests`) is as important as your hours billed (`data.totalHoursBilled`). But when the correlation coefficient module is so small we cannot be sure enough. Nevertheless I am going to try to pass more tests.

Your hourly rate (`data.hourlyRate.amount`) also has a big correlation but it is a consequence of your rank not a reason (I hope). It is fun to notice that your id (`data.recno`) also matters, but I believe in the least.

### Visualisation
There are scatter plots and histograms for all columns correlation of rank was calculated with. Here is the example of scatter plot (rank is on Y axis and your success score (`data.nss100 `) is on X axis):

![Success score scatter plot](/images/success_score_scatter.png)

Here is the histogram of the number of passed tests:

![Passed tests histogram](/images/passed_tests_hist.png)

## my_rank_for_query.py
It's a simle scraper that helps you to determine your place (rank) in [UpWork freelancers search](https://www.upwork.com/o/profiles/browse/) in terms of particular query. Here is an example of how to check the rank of my agency on UpWork ([blue underlined link](https://www.upwork.com/companies/~0140676ee0e4006401)):

```
> scrapy runspider -a profile_id="~0140676ee0e4006401" -a query="telegram bot" -a page_limit=10 -o telegram_bot.json my_rank_for_query.py
2018-10-23 19:45:55 [scrapy.utils.log] INFO: Scrapy 1.5.1 started (bot: scrapybot)
...
2018-10-23 19:45:58 [scrapy.core.engine] INFO: Spider closed (Your profile rank is 26. You are at page 3: https://www.upwork.com/o/profiles/browse/?q=telegram+bot&page=3)
```

Let's go to the link and check the results (I recomend you to be logged out and use incognito mode of your browser): https://www.upwork.com/o/profiles/browse/?q=telegram+bot&page=3


### profile_id
You should use your agency `profile_id` if your are agency freelancer otherwise you should use your personal `profile_id`. You can get profile id from these links:

#### Personal link
Here is my personal profile: https://www.upwork.com/freelancers/~01b101e9259c8483ee  
![Personal link](/images/personal_link.png)

#### Agency link
Here is my agency profile: https://www.upwork.com/companies/~0140676ee0e4006401  
Since I am an agency freelancer I am using it as a parameter for the scraper.  
![Agency link](/images/agency_link.png)

### query
`query` is a search query what you want to check, `telegram bot` e.g.

### page_limit
UpWork limits the page number at 500 although there maybe more freelancers. But if you don't want to scrape so many pages you can use `page_limit` parameter.

### Ouput
In addition to determining your place in the search results scraper also saves some data about all scraped freelancers in the output file. Here it is `telegram_bot.json`.
