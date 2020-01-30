# Sephora and Ulta ETL 

## Project Proposal
		
Our group set out to create a user-friendly tool/database to address the need of consumers to identify the different ingredients that make up the personal hygiene/beauty products that we as consumers use daily. Whether the consumer has sensitive skin, dry skin, oily hair, allergies to a specific ingredient or simply wants to search by price or peer recommendations, our tool seeks to empower the user to be able to navigate the various product offerings with the confidence that this tool has done the homework for them and has narrowed the products to those that fit the customer’s needs. 
		

### Initial Project Set-Up
		

When we visualized what we wanted to accomplish with the project and what the timeline was, we initially set out to obtain various variables (skin type, top grossing products, etc.) on a wider set of products/product categories (mascara, eye brows etc.), on two websites (sephora.com and ulta.com). Given our time constraints however, we decided to downscale the project to a narrower product and variable set. Importantly, however given the way that we set up the code, the project can easily be modified and scaled to extract any additional products and variables that the user may need/want by changing the search list.
		

![image1](https://github.com/nawdah/cosmetics-proj/blob/master/pictures/schema1.PNG)
![image2](https://github.com/nawdah/cosmetics-proj/blob/master/pictures/schema2.PNG)
		

Given the quick turnaround (five days from start to finish), our timeline was compressed. As a group we agreed that all the web-scrapping needed to be done by Monday (day - 2), all the data transformation completed by Tuesday (day - 3), to go live and complete documentation by Wednesday (day - 4), and on Thursday analyze, submit and present our findings to the class.
		

## Technical write up of project:
		

With the end project result in mind of being able to make product recommendations to consumers based on their preferences, the group set out to get the most relevant and up to date product information in order to accomplish this goal. Next we transformed the data and later loaded to data in a usable format by the end consumer. We followed the Extract Transform Load (ETL) methodology. One can find a more detailed explanation on each step below, but in brief, we extracted the data through web scrapping from www.sephora.com and www.ulta.com websites, we then cleaned up the data and created dataframes in order to transform the data to a usuable format and then loaded the data to PostreSQL database for easy access. 
		

### Extraction:
		

Our objective for scraping was to extract the brand name, product name, price, rating, details such as skin type, and ingredient list. We assumed that the script needed to scrape both Sephora and Ulta would be similar, since they are both grid formatted e-commerce sites. However, we eventually realized both sites utilized Javascript, so scraping with just the BeautifulSoup library was simply not enough. 
		

#### Sephora
		

Before working on crawling through the Sephora website, we need to familiarize ourselves with the framework of their HTML & Javascript. Using the Inspect tool on Google Chrome’s browser, we can browse through the entire profile’s code. Highlighting and inspecting specific text we need gives us the attributes required for scraping. Initially we used BeautifulSoup to extract all attributes needed. We were able to get brand name ('a', class_='css-es084o'), product name ('span', class_='css-0'), price ('div', class_='css-slwsq8'), ratings ('div', class_= 'css-ychh9y'), url ('a', class_='css-ix8km1'), details and ingredients ('div', class_='css-1rny024'). At first we were only able to extract 12 products per page. That's when we realized that Sephora ran the rest of the grid through JS. We imported the selenium library to extract data and added a delay and scroll to allow the page to fully load. For more information, check the ipython notebook sephora_scraping.ipynb
		

#### Ulta
		

Similarly to Sephora, we were initially stumped to why some of our data was able to be extracted and the rest was hidden. Our "Eureka!" moment was realized when we disabled JS on our browsers and realized most of the page did not load. That's when we use selenium in conjucture to bs4 to extract the data. We added a delay as chromedriver.exe was loading up each page. In total, extraction took over 2 hours for nearly 800 product. 
		

### Transformation
		

Once we were able to create dataframes of both Ulta and Sephora products, we cleaned each dataframe individually. We needed to determine data integrity and interpret the data. This included adding the store that the product was available on (in this case Sephora and Ulta), rename the product types, change the price variable from a string to a numeric object, make the ratings numeric as well and append the domain URL to the URLs for the products. 
		

Code for this can be seen here and also in the respectful ipython notebooks:
		

![image3](https://github.com/nawdah/cosmetics-proj/blob/master/pictures/schema5.PNG)
		

### Loading
		

We used the transformed dataframes and merged the data both on postgres and using a pandas dataframe. Some product and brand names were named the same so we merged those values together. Now we could filter through the data however we wanted to, whether we wanted to look for a specific brand or not. 
		

A snippet of the loaded merged dataframe is included here: 
		

![image4](![image3](https://github.com/nawdah/cosmetics-proj/blob/master/pictures/schema6.PNG)
		

We were able to analyze the average consumer rating related to product type. Both stores visualizations are shown below.
		

![image5](https://github.com/nawdah/cosmetics-proj/blob/master/pictures/sephora.PNG)
![image6](https://github.com/nawdah/cosmetics-proj/blob/master/pictures/ulta.PNG)
		

Interestingly enough, eyeliner was the least favorable product among consumers. This data is insightful to the cosmetic industry to illustrate the demand and quality preference of consumers in this market. 

## Conclusion

Our object to begin with was to create an application that would allow consumers to filter out products based on their skin type and allergies. However, due to the time constraint, this seemd very improbable. The application would have required using JavaScript which we haven't gotten to yet. In the future, this is definitely a project worth pursuing further as there is no current application that parses through ingredients and details of products. 

