---
output: html_document
title: Web Scraping with R (part 2)
date: 2023-09-06T13:43:56.972Z
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
From the previous tutorial, we got the chance to web scrape [Toscrape](https://toscrape.com/) and extract the information from the [Quotes to Scrape](https://quotes.toscrape.com/) site into a .csv file using **R programming**. From this exercise, we got the quote texts and author names into the quotes data frame. Let us have a look at the data frame to refresh our minds using the code below.

```r
#view(quotes)
```

**Note:** *If you did not run the codes from the previous tutorial, click [here](https://github.com/mikaranja/R-Programming/blob/main/Web%20Scraping%20with%20R%20(part%201).R) to get the entire code.*

You can see that the previous web scraping exercise only extracted data from the first page of the website. However, there are ten more pages with ten quotes each, which brings us to a total of 100 quotes in the site. In this tutorial, we are going to extract the same information but from all the ten pages. As such, we are going to introduce a loop function in our R code to accomplish this feat and store the data in our quotes data frame.

### Step 1: Load Packages

Assuming that we have the following packages ("rvest"and "dplyr") installed, let us load the packages into the RStudio session. If you do not have the packages installed, revisit the code in Part: 1 of the tutorial [here](https://mikaranja.com/project/web-scraping-with-r/).

##### Code Chunk 1:

```r
library(rvest)
library(dplyr)
```

### Step 2: Preliminaries

Since we are going to initiate a loop in the web scraping process, we need a data frame to store the data. This data frame will start off empty but will get to store the data after every loop, without loosing or overwriting it.

##### Code Chunk 2:

```r
quotes <- data.frame()
```

### Step 3: rvest Loop

Now that we have our empty data frame for storing the scraped data, we are going to initiate a loop with rvest. The main focus of this loop is to instruct rvest to get the urls of all the pages that we want to scrape. While this feat may be daunting, with a keen observation of the changes in the urls from page to page, you will take note of the elements to factor in the loop.

##### Code Chunk 3:

```r
for (page_result in seq(from = 1, to = 10)){
  url <- paste0("http://quotes.toscrape.com/page/", page_result, "/")
  page <- read_html(url)
```

### Step 4: Data Scraping

Just like in the first tutorial, we will need a CSS selector to identify the individual sections and the document using the [SelectorGadget](https://selectorgadget.com/) extension on your browser. With this code, we will get the quotes and their respective author names from all the ten pages.

##### Code Chunk 4:

```r
text <- page %>% html_nodes(".text") %>% html_text()
author <- page %>% html_nodes(".author") %>% html_text()
```

### Step 5: Data Storage

Now that we have successfully extracted the data from the ten pages, we need to store it in the data frame created above. Since every subsequent instance a web page is scraped, already existing data in the data frame is replaced with new data, we need to introduce the rbind() - a row binding function. In this manner, any new data from the subsequent scrapes is added to the already existing data.

##### Code Chunk 5:

```r
quotes <- rbind(quotes, data.frame(text, author, stringsAsFactors = FALSE))
```

Since the above code may take a while to execute, there may be a tendancy to think that the RStudio session has hang. To address this possible concern, let us have a visual to understand what is happening in the runtime. Here, let us print the page numbers of the individual web pages that the scraping is taking place.

```r
print(paste0("Page:", page_result))
}
```

Now that we have scraped all the ten pages and have the data in a data frame, let us store it as a .csv for easier retrieval as and when needed. We can also have a look at the data frame and see the end results of the web scraping exercises.

```r
write.csv(quotes, "quotes.csv")
```

### Conclusion

Eureka! We have successfully scraped the ten pages of 'Quotes to Scrape' site. We can now have a look at what we have extracted from the site. 

```r
View(quotes)
```

Arguably, web scraping identifies as a must have skill for any data analyst who seeks to take on data extraction from the Internet. Through such exercises, one can acquire the necessary data to answer any business question they may identify with. Stay tuned for more tutorials on web scraping.

**Note:** *Click [here](https://github.com/mikaranja/R-Programming/blob/main/Web%20Scraping%20with%20R%20(part%202).R) to get the entire code for this exercise.*