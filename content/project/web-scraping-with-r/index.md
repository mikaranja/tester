---
title: Web Scraping with R (part 1)
date: 2023-08-30T09:35:44.865Z
draft: false
featured: false
tags:
  - data_analytics
  - web_scraping
links: []
image:
  filename: web-scraping.png
  focal_point: Smart
  preview_only: false
  caption: Web Scraping with R
---
There are numerous ways that one can get information from the Internet. While some of these ways are quite straight forward e.g., copying and pasting, extracting great volumes of data may prove to be cumbersome. It is in such situations that web scraping stands out. Web scraping identifies as the process in which one can extract great volumes of information and data from a webpage/website with the aid of automated processes. With the adoption of capable tools and programming languages, web scraping suffices as a breeze when it comes to getting loads of information and data from the Internet efficiently.

In this tutorial, we are going to use **R programming** to web scrape [Toscrape](https://toscrape.com/) and extract the information from the [Quotes to Scrape](https://quotes.toscrape.com/) site into a .csv file.

### Step 1: Install Packages

The first step when it comes to web scraping With R entails installing the necessary packages in RStudio. Here, we are going to work with "rvest" and "dplyr".

```r
install.packages("rvest")
install.packages("dplyr")
```

**Note**:*"dplyr" is one of the packages in the core tidyverse packages. You can go ahead and install the "tidyverse" package instead.*

### Step 2: Load Packages

Following the installation of the above packages, we have to load the packages in our RStudio session.

```r
library(rvest)
library(dplyr)
```

### Step 3: rvest Commands

Once we have loaded the necessary packages, we can start scraping the information from the website. We can tackle this extraction in parts, which are going to build on each other.  

In this first part, we will scrape the first page of the [Quotes to Scrape](http://quotes.toscrape.com/) site and extract the 10 quotes and their authors.

##### Code Chunk 1:

```r
url <- "http://quotes.toscrape.com/"
page <- read_html(url)
```

*The first line of the code chunk assigns the website link to the url variable; the second line commands "rvest" the get the html document/source code of the url.*

##### Code Chunk 2:

For the code chunk that follows, we will need a CSS selector to identify the individual sections and the document. Here, I recommend installing [SelectorGadget](https://selectorgadget.com/) extension on your browser.

```r
text <- page %>% html_nodes(".text") %>% html_text()
author <- page %>% html_nodes(".author") %>% html_text()
```

*The first line of the code chunk commands "rvest" to get the html node of the quote from the CSS selection/tag offered by SelectorGadget and pass it as the html text into the text variable. The second line does the same for the author variable.*

##### Code Chunk 3:

Now that we have the information we need, we are going to create a data frame and store it as a .csv.

```r
quotes <- data.frame(text, author, stringsAsFactors = FALSE)
write.csv(quotes, "quotes.csv")
```

*The first line of the code chunk creates a data frame with the column names 'text' and 'author' and stores the observations in rows; the stringsAsFactors argument instructs R to treat the strings as plain strings as opposed to the default, which is as factor variables.*

### Conclusion

Great! We have concluded our web scraping exercise. Now, let us have a view of what we have scraped from the 'Quotes to Scrape' site.

```r
View(quotes)
```

It is evident that web scraping is a great skill for data analysts and more so, data scientists. With the power to extract information and data from webpages and websites fast, one can save time for what really matters - data analysis. In the tutorials that follow, we will dive deeper into advancing our web scraping skills. See you then.

**Note:** *Click [here](https://github.com/mikaranja/R-Programming/blob/main/Web%20Scraping%20with%20R%20(part%201).R) to get the entire code for this exercise.*