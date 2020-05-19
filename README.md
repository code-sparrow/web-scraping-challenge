# **_The Mars Report_**

A web application that scrapes multiple websites for Mars-related information using a Flask powered backend that stores the data in a mongoDB collection and displays the latest information in a single HTML page.

<img src="Images/mission_to_mars.png" width="60%">  


## Data Sources and Technologies  
* <a href="https://mars.nasa.gov/news/" target="_blank">NASA Mars News Site</a>  
* <a href="https://www.jpl.nasa.gov/spaceimages/?search=&category=Mars" target="_blank">JPL Mars Space Images</a>  
* <a href="https://twitter.com/marswxreport?lang=en" target="_blank">Mars Weather Twitter Account</a> 
* <a href="https://space-facts.com/mars/" target="_blank">Mars Facts Table from Space-Facts site</a> 
* <a href="https://astrogeology.usgs.gov/search/results?q=hemisphere+enhanced&k1=target&v1=Mars" target="_blank">Mars Hemispheres Images from USGS Astrogeology site</a>  
* MongoDB, Python, Pandas, PyMongo, BeautifulSoup, Splinter, Flask, ChromeDriver, HTML, CSS, Bootstrap  

## Backend  
A [Flask App](Missions_to_Mars/app.py)  
* connects to the Mongo database using PyMongo  
* The home route gets the latest mongoDB collection and renders the [HTML template](Missions_to_Mars/templates/index.html)     
* injecting the data into the HTML  
* A scrape route runs the web scraping functions, updates the mongoDB collection, and redirects back to the home route  

## Web Scraping
* Initial testing of the web scraping was done in a [Jupyter Notebook](Missions_to_Mars/mission_to_mars.ipynb)
* The [web scraping module](Missions_to_Mars/scrape_mars.py) is ran when the user clicks the button at the top of the page
* First, a Browser istance is created with Splinter using the Chrome driver 
* As each URL is visited with the browser instance, the HTML is passed to and parsed with BeautifulSoup  
&nbsp;&nbsp;&nbsp;&nbsp;`browser.visit(url)`&nbsp;&nbsp;&nbsp;&nbsp;`html = browser.html`&nbsp;&nbsp;&nbsp;&nbsp;`soup = bs(html, "html.parser")`  
* In each case, the HTML in the "soup" object is navigated, and the appropriate text is extracted
* typically based on tags and classes, for example `soup.find('div', class_="article_teaser_body").text`
* This approach failed only with Twitter, where strictly Splinter and regular expressions were used. Such that:

```python
con = browser.find_by_tag('div')
for s in con:
        x = re.findall('InSight[^<]*hPa', s.html, flags=re.S)
        if x:
                mars_weather = x[0]
                break
```
	





Complete your initial scraping using Jupyter Notebook, BeautifulSoup, Pandas, and Requests/Splinter.

* Create a Jupyter Notebook file called `mission_to_mars.ipynb` and use this to complete all of your scraping and analysis tasks. The following outlines what you need to scrape.

### NASA Mars News

* Scrape the [NASA Mars News Site](https://mars.nasa.gov/news/) and collect the latest News Title and Paragraph Text. Assign the text to variables that you can reference later.

```python
# Example:
news_title = "NASA's Next Mars Mission to Investigate Interior of Red Planet"

news_p = "Preparation of NASA's next spacecraft to Mars, InSight, has ramped up this summer, on course for launch next May from Vandenberg Air Force Base in central California -- the first interplanetary launch in history from America's West Coast."
```

### JPL Mars Space Images - Featured Image

* Visit the url for JPL Featured Space Image [here](https://www.jpl.nasa.gov/spaceimages/?search=&category=Mars).

* Use splinter to navigate the site and find the image url for the current Featured Mars Image and assign the url string to a variable called `featured_image_url`.

* Make sure to find the image url to the full size `.jpg` image.

* Make sure to save a complete url string for this image.

```python
# Example:
featured_image_url = 'https://www.jpl.nasa.gov/spaceimages/images/largesize/PIA16225_hires.jpg'
```

### Mars Weather

* Visit the Mars Weather twitter account [here](https://twitter.com/marswxreport?lang=en) and scrape the latest Mars weather tweet from the page. Save the tweet text for the weather report as a variable called `mars_weather`.

```python
# Example:
mars_weather = 'Sol 1801 (Aug 30, 2017), Sunny, high -21C/-5F, low -80C/-112F, pressure at 8.82 hPa, daylight 06:09-17:55'
```

### Mars Facts

* Visit the Mars Facts webpage [here](https://space-facts.com/mars/) and use Pandas to scrape the table containing facts about the planet including Diameter, Mass, etc.

* Use Pandas to convert the data to a HTML table string.

### Mars Hemispheres

* Visit the USGS Astrogeology site [here](https://astrogeology.usgs.gov/search/results?q=hemisphere+enhanced&k1=target&v1=Mars) to obtain high resolution images for each of Mar's hemispheres.

* You will need to click each of the links to the hemispheres in order to find the image url to the full resolution image.

* Save both the image url string for the full resolution hemisphere image, and the Hemisphere title containing the hemisphere name. Use a Python dictionary to store the data using the keys `img_url` and `title`.

* Append the dictionary with the image url string and the hemisphere title to a list. This list will contain one dictionary for each hemisphere.

```python
# Example:
hemisphere_image_urls = [
    {"title": "Valles Marineris Hemisphere", "img_url": "..."},
    {"title": "Cerberus Hemisphere", "img_url": "..."},
    {"title": "Schiaparelli Hemisphere", "img_url": "..."},
    {"title": "Syrtis Major Hemisphere", "img_url": "..."},
]
```

- - -

## Step 2 - MongoDB and Flask Application

Use MongoDB with Flask templating to create a new HTML page that displays all of the information that was scraped from the URLs above.

* Start by converting your Jupyter notebook into a Python script called `scrape_mars.py` with a function called `scrape` that will execute all of your scraping code from above and return one Python dictionary containing all of the scraped data.

* Next, create a route called `/scrape` that will import your `scrape_mars.py` script and call your `scrape` function.

  * Store the return value in Mongo as a Python dictionary.

* Create a root route `/` that will query your Mongo database and pass the mars data into an HTML template to display the data.

* Create a template HTML file called `index.html` that will take the mars data dictionary and display all of the data in the appropriate HTML elements. Use the following as a guide for what the final product should look like, but feel free to create your own design.

![final_app_part1.png](Images/final_app_part1.png)
![final_app_part2.png](Images/final_app_part2.png)

- - -

## Step 3 - Submission

To submit your work to BootCampSpot, create a new GitHub repository and upload the following:

1. The Jupyter Notebook containing the scraping code used.

2. Screenshots of your final application.

3. Submit the link to your new repository to BootCampSpot.

## Hints

* Use Splinter to navigate the sites when needed and BeautifulSoup to help find and parse out the necessary data.

* Use Pymongo for CRUD applications for your database. For this homework, you can simply overwrite the existing document each time the `/scrape` url is visited and new data is obtained.

* Use Bootstrap to structure your HTML template.

### Copyright

Trilogy Education Services © 2019. All Rights Reserved.
# web-scraping-challenge