---
title: Web Scraping with R (part 3)
date: 2023-10-28T18:55:19.620Z
draft: false
featured: false
tags:
  - data_analytics
  - web_scraping
image:
  filename: web-scraping.png
  focal_point: Smart
  preview_only: false
  caption: Web Scraping with R
---
Welcome to the third part of the web scraping tutorial series. So far, we have managed to the web scrape [Quotes to Scrape](https://quotes.toscrape.com/) site and extract the 100 quote texts and author names into a data frame and thereafter into a .csv file. This feat was achieved by incorporating a loop function in the R code, which made it easy to exploit the page numbering sequence in the site and web scrape their contents effectively. For a refresher on this exercise, visit [Web Scraping with R (part 2)](https://mikaranja.com/project/web-scraping-with-r-2/) to learn more. 

For this web scraping tutorial, we are going to create functions that will open the 'about' page of every author and extract their bio information. This bio information will identify with the authors' date of birth, birth location, and description, adding it to the main data frame, which has the respective quote text and author name. With this understanding, let's get to it.

## **Step 1: Load Packages**

We will start by loading the necessary packages into the RStudio session. (This step assumes that you already have the packages installed).

```r
library(rvest)
library(dplyr)
```

## **Step 2: Create Functions**

In R programming, a function serves as a block of code that, when run, passes data as a result of the defined parameters. In other words, a function serves as code chunk that identifies with organized statements to perform a specific task. Here, the function will be created to extract the authors' bio data from their individual 'about' pages. This function will be tasked to use the URL from the 'about' link, scrape using "rvest" and pass the output for the data frame. 

#### **Function 1:**

```r
get_born = function(author_link) {
  author_born <- read_html(author_link)
  born <- author_born %>%
    html_node(".author-title+ p") %>%
    html_text()
  return(born)
}
```

This function will serve to extract the author's date of birth and location of birth as captured under 'Born'. 

#### **Function 2:**

```r
get_bio = function(author_link) {
  author_bio <- read_html(author_link)
  bio <- author_bio %>%
    html_node(".author-description") %>%
    html_text()
  return(bio)
}
```

This function will serve to extract the author's bio as captured under 'Description'. 

## **Step 3: Preliminaries**

Just as in the previous tutorial, we are going to create an empty data frame to store the web scraped data. The data frame will serve to temporarily hold web scraped data after every executed loop thereby negating issues of lost or overwritten data.

```r
quotes <- data.frame()
```

## **Step 4: rvest Loop**

Borrowing from the structure of the previous tutorial, this loop structure will not be so different. The main focus remains to read the HTML of every page as per the sequence of changes in the URLs using "rvest" and use [SelectorGadget](https://selectorgadget.com/) to identify and extract the HTML text. 

```r
for (page_result in seq(from = 1, to = 10)){
  url <- paste0("http://quotes.toscrape.com/page/", page_result, "/")
  page <- read_html(url)
```

While a greater part of the loop remains the same, some parameters have been introduced to accommodate the functions listed above. To start, there is a need to identify the URL to the authors' 'about' page. It is from this page that the functions "get_born" and "get_bio" come in to extract the authors' bio data.

```r
  author_link <-page %>%
    html_nodes(".quote span a") %>% html_attr("href") %>%
    paste0("https://quotes.toscrape.com/", .)
  
  text <- page %>% html_nodes(".text") %>% html_text()
  author <- page %>% html_nodes(".author") %>% html_text()
```

*Take note of the "author_link" variable that stores the absolute URL string, which originates from the concatenation of the extracted html attribute and URL text.*

The following code chunk deserves some explanation. The two lines of code use the sapply() function to apply the "get_born"/"get_bio" function to each element in the "author_link" vector, and the results are stored in the "born"/"bio" variable. 

```r
  born <- sapply(author_link, FUN = get_born, USE.NAMES = FALSE)
  bio <- sapply(author_link, FUN = get_bio, USE.NAMES = FALSE)
```

Having extracted the necessary information as values, it is now time to store them in the 'quotes' data frame. Just as from the previous tutorial, we need to factor in a row binding function - rbind(). This function will ensure that any new data extracted in a loop does not overwrite the previous loop data.

```r
  quotes <-rbind(quotes, data.frame(text, author, author_link, born, bio,
                                    stringsAsFactors = FALSE))
```

To get a sense of what is taking place in the RStudio session or where the web scraping process has reached, let us print the page numbers of the completed loops.

```r
  print(paste("Page:", page_result))
}
```

With the finalization of the loops, we will have the quote text, author name, 'about' link, author date of birth & location, and author description from the 'Quotes to Scrape’ site. Let us store this data as a .csv as we take a look at the 'quotes' data frame.

```r
write.csv(quotes, "quotes.csv")
View(quotes)
```

# **Conclusion**

Awesome! We have managed to web scrap 100 quotes, including author bio data, from the 10 pages of the 'Quotes to Scrape’ site. Looking back at the steps taken to achieve this feat, it becomes obvious that any other way of getting the same information would have been laborious. However, web scraping with R makes it all easy with a few lines of code. In the next post, we will be analyzing web scraped data from the Internet. Stay tuned!

Note: Click [here](https://github.com/mikaranja/r-programming/blob/main/Web%20Scraping%20with%20R%20(part%203).R) to get the entire code for this exercise.