# Final Project - Web scraping and Analyzing Christie's auctions house 
Data Analytics final project repository

- Introduction
- Repository index
- A guide for Christie’s website
- Web-scraping
- Tableau
- Slides

## REPOSITORY INDEX
- [Dataframes](https://github.com/GloriaiXIII/christies_final_project/tree/main/0.Extracted_data)
- [Python notebooks](https://github.com/GloriaiXIII/christies_final_project/tree/main/1.Python_notebook)
- [Tableau](https://github.com/GloriaiXIII/christies_final_project/tree/main/3.Tableau)
- [Appendix](https://github.com/GloriaiXIII/christies_final_project/tree/main/Appendix)

## INTRODUCTION
Art auction prices cause controversy in society because they are perceived as erratic. It is believed that art prices are completely unpredictable because of their nature but are they really unpredictable at all? or is pricing art like pricing other market items?

This project focuses on getting and analyzing the sales data from Christie's british Auction house from the 18th century and present worldwide with locations in places such as London, Amsterdam, New York and Hong Kong. They have public auctions and private close-door transactions 

We have to take in account that the art market is extraordinarily small (only very wealthy people can afford it) and that each piece of art is unique and highly individualized. According to some important studies, the human prediction of art prices is believed to be only accurate at 30%. Based on a MIT article, when trying with visual models, they performed surprisingly poorly in contrast to textual and quantitative data. 

For the project purpose, there is no public dataset available but the information can be found on the website. Thus, the dataset for this project has been built extracting the information directly from Christie’s official website through web scraping: a useful technique to gather data from web pages viewable code and store it in different formats for further treatment and analysis.

Due to website size and complexity, being non static and having elements inside javascript scripts, the analysis has been focused on antiquities auction events from january 2009 until december 2021 worldwide. After cleaning the data this equals to more than 10.000 rows, being each row a selled item. 

## A GUIDE FOR CHRISTIE’S WEBSITE 
UNDERSTANDING AND ANALYZING CHRISTIE’S URL :

All the links have an HTTP method. HTTP is the hypertext transfer protocol, used to enable communications between clients and servers as a request-response protocol between both. (definition from w3schools). 

The two most common types of HTTP methods are: GET and POST, followed by PUT, HEAD, DELETE, PATCH and OPTIONS. 

Christie’s URL follows the HTTP method GET. Get method is one of the most common methods to request data from a specific source, but this method can be cached and should never be used when dealing with sensitive data becauses the parameters are stored in browser history, web server logs and is part of the viewable url so is less safer than other methods such as Post. 

On Christie’s website we can consult past auction results and select/filter the content by the following filters: month, year, events (online, live), category and location. The results are from january 1998 until the present.

For this project we want to iterate over months and years in order to scrape and store all the past results. 
https://www.christies.com/en/results?language=en&month=1&year=1998 

Main root url: https://www.christies.com/en/

Results root url: https://www.christies.com/en/results 

We will focus the attention on antiquities but for the moment we will not discard other categories. 

Url with different categories selected:
https://www.christies.com/en/results?filters=|category_10|category_7|category_17|category_13|category_5|category_8|category_6|category_22|category_11|&month=11&year=2020

Most common categories from last year: 
category_5	Asian Art 
https://www.christies.com/en/results?filters=|category_5| 

category_6	Collectibles
https://www.christies.com/en/results?filters=|category_6|

category_7	Fine Art
https://www.christies.com/en/results?filters=|category_7| 

category_8	Furniture & Decorative Art 
https://www.christies.com/en/results?filters=|category_8| 

category_9	Jewellery, Watches & Handbags
https://www.christies.com/en/results?filters=|category_9| 

category_10 	Antiquities
https://www.christies.com/en/results?filters=|category_10|

category_11	Photographs & Prints
https://www.christies.com/en/results?filters=|category_11| 

category_13	Books & Manuscripts 
https://www.christies.com/en/results?filters=|category_13|

category_14	Wines & Spirits 
https://www.christies.com/en/results?filters=|category_14|

category_17	Islamic & Middle Eastern Art 
https://www.christies.com/en/results?filters=|category_17|

category_22 	African, Oceanic & Pre-Columbian Art 
https://www.christies.com/en/results?filters=|category_22|

category_24	Science and Natural History
https://www.christies.com/en/results?filters=|category_24|

## WEB SCRAPING
CHANGING THE IP:
To avoid being blocked by the website when sending requests:
Install URBAN VPN extension for Brave/Tor or your usual navigator
Open the app and select a location
Click play to connect to the selected country VPN
If you get blocked, change the VPN
When finished, click stop. 

CHRISTIE’S RESULTS PAGE STRUCTURE
The Christie’s results page is structured in a tree/hierarchical way with a homepage, main sections and subsections. 

For this project we have the sub-subsection lots (3)  inside the subsection auctions (2) inside the main section results (1)

DEPTH_LIMIT
Increasing the depth limit will increase the running time. According to the Christie’s structure we need a depth limit of 2 taking in account the results page as the mother of all the subpages we want to scrape (0 results - 1 events - 2 lots). 

WHICH WEB SCRAPING LIBRARY SHOULD WE CHOOSE?
Web scraping is a useful technique to gather data from web pages and store it in different formats for further treatment and analysis. Web scraping has to be done ethically, harmless and without harming. In this project web scraping is used in an academic context to analyze the data extracted.

The three most popular libraries for web-scraping are: Beautiful soup vs Scrapy vs Selenium → each one has its own pros and cons

1.SCRAPY
- Open source
- Fast performance 
- Powerful library - one in all library: processes and saves data all on it’s own. 
- Is built using the Twisted Python asynchronous framework (event-driven), allowing us to scrap sending requests without being blocked, using a non-blocking mechanism.
- It consumes low memory and CPU space → low power consumption
- Good option for large/complex projects: if the project needs to be migrated, proxies (intermediary between a client machine and a server) or data pipelines (moving data and storing it from one source to another). 
- We can use different proxies and VPN’s to automate the task + send multiple requests from the multiple proxy addresses.
- Cons: non beginner friendly :(

2.BEAUTIFUL SOUP
- Is only a data parser and extractor
- It depends on the Requests library (synchronous?) and an external parser 
- Good and understandable documentation online
- Easy to use
- Good option for small or low level complex projects

3.SELENIUM
- Designed for browser automation / automate tests (useful for programmers)
- Can be used with Python, C#, Java, Ruby, etc.
- Easily handle AJAX and PJAX requests
- Good option for use when scraping webs with a lot of javascript. 
- Cons: limited data size and hard to use proxies

First I tried to learn Scrapy but due to short time and complexity, I finally used mainly Beautiful Soup. 
![christies_spider](https://github.com/GloriaiXIII/christies_final_project/blob/main/Appendix/scrapy_tutorial/christies%20spider.png)

## TABLEAU
Here you can find the tableau workbook and the tableau public [url](https://public.tableau.com/views/artauctionsanalysis/Avgtotalsalebylocation?:language=en-US&:display_count=n&:origin=viz_share_link).

![avgtotalsalebyloc.png](https://github.com/GloriaiXIII/christies_final_project/blob/main/3.Tableau/avgtotalsalebyloc.png)
![currencies.png](https://github.com/GloriaiXIII/christies_final_project/blob/main/3.Tableau/currencies.png)
![location_total_sales_and_est.png](https://github.com/GloriaiXIII/christies_final_project/blob/main/3.Tableau/location_total_sales_and_est.png)
![auction_events_type.png](https://github.com/GloriaiXIII/christies_final_project/blob/main/3.Tableau/auction_events_type.png)
![prices_info_by_location.png](https://github.com/GloriaiXIII/christies_final_project/blob/main/3.Tableau/prices_info_by_location.png)
![zoom_in.png](https://github.com/GloriaiXIII/christies_final_project/blob/main/3.Tableau/zoom_in.png)
![obs slides second must underrated.png](https://github.com/GloriaiXIII/christies_final_project/blob/main/IMG/obs%20slides%20second%20must%20underrated.png)

## SLIDES
In the following link you can find the [slides](https://slides.com/gloriaixiii/fp_elevator_pitch/fullscreen)

## SUMMARY
- It is possible to get the data from the christies website but it is not easy to navigate through it. 
- This project could serve as a basis for a larger project and it can be expanded.
- A first approach has been made to apply a model to predict the final price based on the estimated price. 
- For the moment humans (although their accuracy is c.40%) are more accurate than machine learning but as AI improves, the models will perform better and it could expand market liquidity and transparency in the art industry.
- Obtaining the whole categories (not only antiquities) and features of the pieces will give us a better basis for predicting the selling price. 

## REFERENCES
Artsy. (2019). The online art collector report 2019. http://files.artsy.net/documents/artsy_2019_onlineartcollectorreport.pdf

Bailey, J. (2020). Can Machine Learning Predict the Price of Art at Auction? Harvard Data Science Review, 2(2). https://doi.org/10.1162/99608f92.7f90ce96

Baumol, W. J. (1985). Unnatural value: Or art investment as floating crap game. Journal of Arts Management and Law, 15(3), 47-60. https://doi.org/10.1080/07335113.1985.9942162

Bjerg, M. (2018). Sotheby's & Christie's hammer price vs estimate for 2016 and 2017. Mearto. https://mearto.com/accuracy-of-sothebys-and-christies-estimations-revealed

Bruno, B., Garcia‐Appendini, E., & Nocera, G. (2016). Experience and brokerage in asset markets: Evidence from art auctions. Financial Management, 47(4), 833–864. https://doi.org/10.1111/fima.12207


Thank you for your time! 






