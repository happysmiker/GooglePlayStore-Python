import csv
import time

import pandas as pd
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
#in the next line inset the address of your own geckodriver file
driver = webdriver.Firefox(executable_path=r'F:\Documents\geckodriver.exe')
# I started with the first category called Art & Deign and this new commit is scraping the augmented reality category :
url = 'https://play.google.com/store/apps/collection/cluster?clp=6gsvCi0KJ3Byb21vdGlvbl9kYXlkcmVhbV9zdG9yZWNhdGVnb3J5X2FyYXBwcxBKGAM%3D:S:ANO1ljLDFOc&gsr=CjLqCy8KLQoncHJvbW90aW9uX2RheWRyZWFtX3N0b3JlY2F0ZWdvcnlfYXJhcHBzEEoYAw%3D%3D:S:ANO1ljKjMrE'
driver.get(url)
time.sleep(5)

# Getting the app links
app_links = []
elems = driver.find_elements_by_xpath("//a[@href]")    #getting all the links in the page
for elem in elems:  #separating the links to applications and saving them in app_links list
    if "details?id" in elem.get_attribute("href"):
        app_links.append((elem.get_attribute("href")))



app_links = list(dict.fromkeys(app_links))  ######turning our list into dictionary and back to list


# getting these data from the links :'URL', 'Name', 'Stars', 'Comments', 'Installs', 'Size' , 'Updated' , 'Current Version', 'Required Android' , 'Content Rating' , 'Cost Type' , 'Email Address'
list_all_elements = []
list_all_elements2 = []

for iteration in app_links:
    try:
        app = {'URL': None, 'name': None, 'stars': None, 'comments': None, "installs": None, "size": None,
               "updated": None, "gamegenre": None, 'gamedesc': None, 'price': None, "allreviewtext": None,
               "current_version": None, "requires_android": None, 'in_app_products': None, 'offered_by': None,
               "content_rating": None}
        driver.get(iteration)
        print(iteration)
        time.sleep(3)
        try:
            header1 = driver.find_element_by_tag_name("h1")
        except Exception as e:
            header1 = "not found"
        try:
            star = driver.find_element_by_class_name("BHMmbe")
        except Exception as e:
            star = "not found"
        try:
            gamegenre = driver.find_element_by_css_selector("a[itemprop='genre']")
        except Exception as e:
            gamegenre = "not found"
        try:
            gamedesc = driver.find_element_by_css_selector("div[jsname='sngebd']")
        except Exception as e:
            gamedesc = "not found"
        try :
            price = driver.find_element_by_class_name("oocvOe")
        except Exception as e:
            price = "not found"

        others = driver.find_elements_by_class_name("htlgb")
        try:
            reviewtext = driver.find_elements_by_css_selector("div[jscontroller='LVJlx']")
        except Exception as e:
            reviewtext = "not found"
        try:
            allreviewtext = reviewtext[0].text + reviewtext[1].text + reviewtext[2].text + \
                                               reviewtext[3].text
        except Exception as e:
            allreviewtext = "not found"
        if allreviewtext != "not found":
            app['allreviewtext'] = allreviewtext
        if price != "not found":
            app["price"] = price.text
        if gamedesc != "not found":
            app["gamedesc"] = gamedesc.text.replace("<br>", "")
        if gamegenre != "not found":
            app["gamegenre"] = gamegenre.text
        if star != "not found":
            app["stars"] = float(star.text.replace(",", "."))
        app["name"] = header1.text
        app["URL"] = iteration
        list_others = []
        for x in range(len(others)):
            if x % 2 == 0:
                list_others.append(others[x].text)

        titles = driver.find_elements_by_class_name("BgcNfc")
        try:
            comments = driver.find_element_by_class_name("EymY4b")
            app['comments'] = comments.text.split()[0]
        except Exception as e:
            comments = "not found"

        # list_elements = [iteration, header1.text, float(star.text.replace(",", ".")), gamegenre.text, price.text,
        #                  gamedesc.text.replace("<br>", ""), allreviewtext, comments.text.split()[0]]
        for x in range(len(titles)):
            if titles[x].text == "Installs":
                app["installs"] = list_others[x]
                # list_elements.append(list_others[x])
            if titles[x].text == "Size":
                app["size"] = list_others[x]
                # list_elements.append(list_others[x])
            if titles[x].text == "Updated":
                # list_elements.append(list_others[x])
                app["updated"] = list_others[x]
            if titles[x].text == "Current Version":
                app["current_version"] = list_others[x]
                # list_elements.append(list_others[x])
            if titles[x].text == "Requires Android":
                app["requires_android"] = list_others[x]
                # list_elements.append(list_others[x])
            if titles[x].text == "Content Rating":
                app["content_rating"] = list_others[x]
                # list_elements.append(list_others[x])
            if titles[x].text == "In-app Products":
                app["in_app_products"] = list_others[x]
                # list_elements.append(list_others[x])
            if titles[x].text == "Offered By":
                app["offered_by"] = list_others[x]
                # list_elements.append(list_others[x])
                # print(list_elements)
        # list_all_elements.append(list_elements)
        list_all_elements2.append(app)
    except Exception as e:
        print(e)

# print(list_all_elements)

driver.quit()

fieldnames = ['URL', 'name', 'stars', 'comments', "installs", "size",
       "updated", "gamegenre", 'gamedesc', 'price', "allreviewtext",
       "current_version", "requires_android", 'in_app_products', 'offered_by',
       "content_rating"]

df = pd.DataFrame(list_all_elements2,columns=fieldnames)
print(df)
df.to_excel('ll.xlsx',columns=fieldnames)
