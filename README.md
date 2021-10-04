# Deploy Flask-Selenium app on Heroku
Deploying python Flask-Selenium project on Heroku with 8 simple steps.

# Instructions

## Step 1: Create a virtual environment.

## Step 2: Install necessary dependencies.
  
  * Flask
  * Selenium 
  * Gunicorn
  
  ```
  pip install Flask
  pip install selenium
  pip install gunicorn
  ```
## Step 3: Create a requirements.txt file.
  
  ```
  pip freeze > requirements.txt
  ```

## Step 4: Create a Procfile file.
  
  ```
  echo "web: gunicorn wsgi:app" > Procfile
  ```
 
## Step 5: Edit your python driver code to be like the below one.
  
  ```python
  chrome_options = webdriver.ChromeOptions()
  chrome_options.binary_location = os.environ.get("GOOGLE_CHROME_BIN")
  chrome_options.add_argument("--headless")
  chrome_options.add_argument("--disable-dev-shm-usage")
  chrome_options.add_argument("--no-sandbox")
  driver = webdriver.Chrome(executable_path=os.environ.get("CHROMEDRIVER_PATH"), options=chrome_options)
  ```
 
## Step 6: Set Heroku environment variables.
 
  * CHROMEDRIVER_PATH = /app/.chromedriver/bin/chromedriver
  * GOOGLE_CHROME_BIN = /app/.apt/usr/bin/google-chrome

  ![](https://github.com/RNogales94/Selenium-Heroku-Python-POC/blob/master/imgs/environment_variables.png)
 
## Step 7: Set Heroku Buildpacks.

  * https://github.com/heroku/heroku-buildpack-google-chrome
  * https://github.com/heroku/heroku-buildpack-chromedriver
  * Python

  ![](https://github.com/RNogales94/Selenium-Heroku-Python-POC/blob/master/imgs/buildpacks.png)

## Step 8: Now push your project and deploy on Heroku.

# Sample code to deploy

```python
from flask import Flask
from selenium import webdriver
import os

chrome_options = webdriver.ChromeOptions()
chrome_options.binary_location = os.environ.get("GOOGLE_CHROME_BIN")
chrome_options.add_argument("--headless")
chrome_options.add_argument("--disable-dev-shm-usage")
chrome_options.add_argument("--no-sandbox")
driver = webdriver.Chrome()

app = Flask(__name__)


@app.route('/')
def root():
    return 'Yea ! It\'s working try to go to /example'


@app.route('/example')
def example():
    driver.get('http://example.com/')
    return driver.page_source


if __name__ == '__main__':
    app.run()

```

