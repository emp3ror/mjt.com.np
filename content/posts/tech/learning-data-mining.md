+++
title = "Learning Data Mining "
date = "2019-03-27T14:54:42Z"

tags = ["R", "Data Mining"]
draft = false
author = "admin"
+++

learning data mining

## Nature of data

After observation, we can get 3 types of data

- Nominal :
  which has label or tags, cant take mean, we can take percentage (frequecy) though
  eg, sex, taste

- Ordinal :
  Rank, satisfactions, mean may not be good...

- Interval/Ratio :
  Measure, intervals like age, time, couts

| Customer |  Age | Sex    | Groceries | Chocobars | Type  | Satisfied | Bulk |
| -------: | ---: | ------ | --------: | --------: | ----- | --------: | ---: |
|        1 |   18 | Male   |    Rs 140 |         2 | Milk  |         1 |    2 |
|        1 |   23 | Female |    Rs 270 |         2 | Dark  |         3 |    3 |
|        1 |   21 | Female |    Rs 160 |         2 | White |         2 |    4 |
|        1 |   27 | Male   |    Rs 180 |         2 | Milk  |         1 |    2 |

- `Type` is `Nominal` and can be displayed in pie chart or bar chart
- `Satisfied` is `Ordinal` and can't be displayed in pie chart. The difference in satisfaction and unsatisfaction feeling can be high so mean may not be preferred.
- `Age` is `Interval` can be displayed in histogram
  
Good video to watch on youtube about nature of data [https://youtu.be/hZxnzfnt5v8](https://youtu.be/hZxnzfnt5v8) 


## Classification - trees
Also known as decision tree 
Divide and conquer 

## Naive Bayes