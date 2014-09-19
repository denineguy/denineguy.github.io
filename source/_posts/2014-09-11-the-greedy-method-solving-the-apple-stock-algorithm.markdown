---
layout: post
title: "The Greedy Method - Solving the Apple Stock Algorithm"
date: 2014-09-11 17:30:27 -0400
comments: true
categories: ruby algorithms greedy method
---

Last week I had the pleasure of attending a Women Who Code NYC event focusing on Algorithms &Interview Prep. The meetup provided a lot of great resources for interview prep and gave us all a chance to work on our whiteboarding skills. One of the questions we came across was regarding   Apple Stock.
<!-- more -->

##Apple Stock Question
I have an array of stock_prices where the keys are the number of minutes into the day (starting with midnight) and the values are the price of Apple stock at that time.  For example, the stock cost $500 at 1am, so stockPricesYesterday[60] = 500.
Write an efficient algorithm for computing the best profit I could have made from 1 purchase and 1 sale of an Apple stock yesterday.

##First Solution
My first attempt at solving this was the brute force way, which iterates through all of the possible solutions.  While this will give me the answer the issue is that it there are n^2 combinations (where n represents each time and stock price pair, for the day). This result will take O(n^2) time to get to the solution. Below is an example of my solution.


```ruby Brute Force Apple Stock Solution
  class StockMarket

    def apple_stock(stock_prices)
      max_profit = 0

      stock_prices.each_with_index do |buy_price, buy_time|
        stock_prices.each_with_index do |sell_price, sell_time|
          profit = sell_price - buy_price
          if buy_time < sell_time && profit > max_profit
            max_profit = profit
          end
        end x
      end
      return max_profit
      
    end
  end
```

##Second Solution - The Greedy Method
My next solution was iterating through the stock prices only once to look for the optimal solution.  This is done using the greedy method. A greedy algorithm iterates through the problem space taking the optimal solution "so far," until it reaches the end. This method leads to only iterating through the problem n times and gives a solution that is O(n) times fast.

```ruby Greedy Method Apple Stock Solution
class StockMarket
  def apple_stock(stock_prices)
    max_profit = 0,
    min_price = stock_prices[0]

    0.upto(stock_prices.length - 1) do |time|   
      current_price = stock_prices[time]
      min_price = current_price  if current_price < min_price

      current_profit = current_price - min_price
      max_profit = current_profit if current_profit > max_profit[0]
    end
    max_profit
  end
end
```

##More About the Greedy Method
In general, greedy algorithms have five components:

1. A candidate set, from which a solution is created
2. A selection function, which chooses the best candidate to be added to the solution
3. A feasibility function, that is used to determine if a candidate can be used to contribute to a solution
4. An objective function, which assigns a value to a solution, or a partial solution, and
5. A solution function, which will indicate when we have discovered a complete solution


Greedy algorithms help find the optimal solution for some mathematical problems, but not all. This algorithm works best on problems that have two properties:

###Greedy choice property 
We can make whatever choice seems best at the moment and then solve the subproblems that 
arise later. The choice made by a greedy algorithm may depend on choices made so far but 
not on future choices or all the solutions to the subproblem. It iteratively makes one 
greedy choice after another, reducing each given problem into a smaller one. In other words,
a greedy algorithm never reconsiders its choices. 

###Optimal substructure 
"A problem exhibits optimal substructure if an optimal solution to the problem contains 
ptimal solutions to the sub-problems."


Below are a list of more resources to prepare for interviews:

* [http://www.ocf.berkeley.edu/~kelu/interviews/questions.html](http://www.ocf.berkeley.edu/~kelu/interviews/questions.html)
* [http://katemats.com/interview-questions/](http://katemats.com/interview-questions/)
* [http://meetupresources.herokuapp.com/index.html](http://meetupresources.herokuapp.com/index.html)
* [http://codingforinterviews.com/practice](http://codingforinterviews.com/practice)
* [https://www.interviewcake.com/](https://www.interviewcake.com/)

Learn more about [Women Who Code](https://www.womenwhocode.com/ "Women Who Code website") and if you are in NYC come checkout a meetup and [Women Who Code NYC](http://www.meetup.com/WomenWhoCodeNYC/ "meetup site").  The next Algorithm and Interview Prep session is sched
