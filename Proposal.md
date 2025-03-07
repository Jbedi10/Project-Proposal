# R Project Proposal
Valerie McIlvaine and Jake Bedi
## Introduction

In our final project, we will investigate superbowl ads from 2000 to 2020 using a data set from Five Thirty Eight. We will make a website or application that displays trends of brands’ advertisement strategies over time and use predictive modeling/ simulations to anticipate which direction superbowl ads will go in the future.
```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
library(tidyverse)
library(readr)
library(tidyverse)
library(highcharter)
library(gapminder)
library(htmlwidgets)
library(kableExtra)
library(fmsb)
library(plotly)
```


```{r upload files}
data_ad_time <- read_csv("~/STA 518/Final Project/Portfolio/Final Proposal/Final Project/Ad  Time _ Category - Sheet1.csv")
data <- read_csv("~/STA 518/Final Project/Portfolio/Final Proposal/Final Project/combined  - Sheet1.csv")
data_ad_cost <- read_csv("~/STA 518/Final Project/Portfolio/Final Proposal/Final Project/Cost per Super bowl ad 2000-2020 - Sheet1 (1).csv")
```
```{r joining two data sets }
# merge data and data_ad_cost together 
total <- inner_join(data, data_ad_cost, by="year")
total
dim(total)
#chr to Numeric cost_per_minute and cost_per_thirty_seconds
total11<-
total%>% as.data.frame %>%
  mutate(cost_per_minute=str_remove_all(cost_per_minute,"[$,]"),
         cost_per_minute=as.numeric(cost_per_minute)) %>%
  mutate(cost_per_thirty_seconds=str_remove_all(cost_per_thirty_seconds,"[$,]"),
        cost_per_thirty_seconds=as.numeric(cost_per_thirty_seconds))%>%
  
  mutate(cost_per_second = cost_per_minute/60 )%>%
  
  mutate(cost_of_ad= cost_per_second*ad_time) %>%
  mutate(cost_of_ad_in_millions =cost_of_ad/1000000 )%>%
  mutate(cost_per_second_in_thousands =cost_per_second/100000)
  
total11
 # mutate(currency = scales::dollar(cost_per_second))
  
 
  
```
```{r }
#total1<- bind_cols(data_ad, data_ad_time %>% select(-year, -brand))
```


```{r exploratory bar charts}
# count of brands use of gender
ggplot(data=total, aes(x=category)) +
  geom_bar(stat= "count", position="dodge",color= "black", aes(fill=celebrity))+ 
   ggtitle("Bar char of Super Bowl Ad Categories and Celebrity Apperances From 2000-2020")
```
```{r line graph}
  ggplot(data=total11, aes(cost_per_second)) +
  geom_histogram(binwidth = 10)
  
  #line graph
  ggplot(data=total11, aes(x=year, y=cost_per_second_in_thousands)) +
  geom_line(linetype = "dashed", color="blue", size=1)+
    theme(legend.position="bottom")+
  geom_point(color="blue")+
  ggtitle("Cost Per Second of Super Bowl Ads from 2000 to 2020")+
    ylab("Cost (Hundreds of Thousands of Dollars)")
  
  
  
  #histogram of distribtuion of cost_of_ad by Year
  
total11 %>%
  group_by(year)
  ggplot(data=total11, aes(y=year, fill=year, color=year)) +
  geom_histogram(alpha=0.5, position="dodge", bins=10)+
       scale_color_brewer(palette="") + 
  theme_classic()+theme(legend.position="top")
 
  
  ggplot(data=total, aes(x=year)) +
  geom_bar( stat="count", position="dodge",colour="black", aes(fill=diversity)) 
  
```
```{r porportions }
 ggplot(data=total11, aes(x= year, fill=use_sex))+
   geom_bar(stat= "count", position="fill")+
   ggtitle("Porportion of SuperBowl Ads that use Sex Appeal from 2000 to 2020")
```


```
```{r boxplot, reorder }
#reordering boxplots based on median cost_of_ad
ggplot(data=total11, aes(x= fct_reorder(category, cost_of_ad_in_millions), y=cost_of_ad_in_millions))+
geom_boxplot(fill="lightblue")+
ggtitle("Boxplots of Cost Per Ad by Category")+
xlab("Ad Category")+
ylab("Ad Cost in Millions")
```



```{r tables}
total11 %>%
group_by(year) %>%
  summarize_at()
```
