import csv
import time
from selenium import webdriver
from selenium.common import NoSuchElementException, StaleElementReferenceException
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.chrome.service import Service

ops = webdriver.ChromeOptions()
ops.add_argument("--disable-notifications")

service_object = Service("../BeautifulShop_tutorial/Install_Driver/chromedriver.exe")
driver = webdriver.Chrome(service=service_object, options=ops)
driver.get("https://discover.gigwell.com/venues")
driver.maximize_window()
driver.implicitly_wait(5)

# Login
driver.find_element(By.XPATH, "//input[@placeholder='Email Address']").send_keys("amy@sunsetsingers.com")
time.sleep(2)
driver.find_element(By.XPATH, "//input[@placeholder='Password']").send_keys("PassWord")
time.sleep(2)
driver.find_element(By.XPATH, "//button[normalize-space()='Log In With Email']").click()
time.sleep(2)
print("Login successful")

# Close the popup
driver.find_element(By.XPATH, "//i[@class='gw-icon-times small']").click()
time.sleep(3)

# Clear search location
search_location = driver.find_element(By.XPATH, "//input[@placeholder='Search for a location']")
current_location = search_location.get_attribute("value")
if current_location:
    search_location.clear()
    print("Location history cleared")
    time.sleep(2)
else:
    print("Location is already clear")

# Search terms
search_terms = ["-bar", "-restaurant", "-nightclub", "-club"]
for term in search_terms:
    search_terms_input = driver.find_element(By.XPATH, "//input[@placeholder='Type to add keyword...']")
    search_terms_input.send_keys(term)
    search_terms_input.send_keys(Keys.ENTER)
    print("filter successful")
    time.sleep(2)

# Open CSV file for writing
with open('venue_data.csv', 'w', newline='', encoding='utf-8') as csvfile:
    csv_writer = csv.writer(csvfile)
    csv_writer.writerow(['urls', 'name', 'Address', 'Email', 'Phone', 'Capacity', 'Tix_Sold', 'First Name', 'Title', 'Directory Phone', 'Directory Email','Second name', 'Second title', 'Second phone', 'Second email', 'Third name', 'Third title', 'Third phone', 'Third email', 'Fourth name', 'Fourth title', 'Fourth phone', 'Fourth email'])

    while True:
        allclick = driver.find_elements(By.XPATH, "//p[@class='strong align-icon-with-text venue-card-name']/span")
        for click in allclick:
            try:
                click.click()
                time.sleep(2)
            except StaleElementReferenceException:
                print("Element is stale, trying again...")
                continue
            row_data = []
            try:
                urls=driver.current_url
            except NoSuchElementException:
                urls="must get urls"
            row_data.append(urls)
            try:
                name=driver.find_element(By.XPATH,"//div[@class='venue-header-content-inner']/div/p[1]")
            except NoSuchElementException:
                name="Data error"
            row_data.append(name)
            try:
                address=driver.find_element(By.XPATH,"//p[@class='text-light']/span").text

            except NoSuchElementException:
                address = "No value"
            row_data.append(address)
            try:
                email=driver.find_element(By.XPATH,"/html/body/div[2]/div[2]/div/div[4]/div/div[1]/div[2]/div/div/p[3]/a").text
            except NoSuchElementException:
                email = "No value"
            row_data.append(email)
            #time.sleep(2)
            try:
                phone=driver.find_element(By.XPATH,"//span[@class='block-element']/a").text
            except NoSuchElementException:
                phone = "No value"
            row_data.append(phone)
            #time.sleep(2)
            try:
                capacity=driver.find_element(By.XPATH,"/html/body/div[2]/div[2]/div/div[4]/div/div[1]/div[2]/div/div/div[2]/div[1]/span[1]").text
            except NoSuchElementException:
                capacity = "No value"
            row_data.append(capacity)
            #time.sleep(2)
            try:
                Tix_Sold=driver.find_element(By.XPATH,"/html/body/div[2]/div[2]/div/div[4]/div/div[1]/div[2]/div/div/div[2]/div[2]/span[1]").text
            except NoSuchElementException:
                Tix_Sold = "No value"
            row_data.append(Tix_Sold)
            #time.sleep(2)
            try:
                first_Name=driver.find_element(By.XPATH,"//ul[@class='list-unstyled venue-directory-list']/li[1]/div/p[1]").text
            except NoSuchElementException:
                first_Name = "No value"
            row_data.append(first_Name)
            #time.sleep(2)
            try:
                directory_title=driver.find_element(By.XPATH,"//ul[@class='list-unstyled venue-directory-list']/li[1]/div/p[3]").text
            except NoSuchElementException:
                directory_title = "No value"
            row_data.append(directory_title)
            #time.sleep(2)
            try:
                directory_phone=driver.find_element(By.XPATH,"//ul[@class='list-unstyled venue-directory-list']/li[1]/div/p[4]").text
            except NoSuchElementException:
                directory_phone = "No value"
            row_data.append(directory_phone)
            #time.sleep(2)
            try:
                directory_email=driver.find_element(By.XPATH,"//ul[@class='list-unstyled venue-directory-list']/li[1]/div/p[5]").text
            except NoSuchElementException:
                directory_email = "No value"
            row_data.append(directory_email)
            #time.sleep(2)
            try:
                directory_second_name=driver.find_element(By.XPATH,"//ul[@class='list-unstyled venue-directory-list']/li[2]/div/p[1]").text
            except NoSuchElementException:
                directory_second_name = "No value"
            row_data.append(directory_second_name)
            #time.sleep(2)
            try:
                directory_second_title=driver.find_element(By.XPATH,"//ul[@class='list-unstyled venue-directory-list']/li[2]/div/p[3]").text
            except NoSuchElementException:
                directory_second_title = "No value"
            row_data.append(directory_second_title)
            #time.sleep(2)
            try:
                directory_second_phone=driver.find_element(By.XPATH,"//ul[@class='list-unstyled venue-directory-list']/li[2]/div/p[4]").text
            except NoSuchElementException:
                directory_second_phone = "No value"
            row_data.append(directory_second_phone)
            #time.sleep(2)
            try:
                directory_second_email=driver.find_element(By.XPATH,"//ul[@class='list-unstyled venue-directory-list']/li[2]/div/p[5]").text
            except NoSuchElementException:
                directory_second_email = "No value"
            row_data.append(directory_second_email)
            #time.sleep(2)
            try:
                directory_third_name=driver.find_element(By.XPATH,"//ul[@class='list-unstyled venue-directory-list']/li[3]/div/p[1]").text
            except NoSuchElementException:
                directory_third_name = "No value"
            row_data.append(directory_third_name)
            #time.sleep(2)
            try:
                directory_third_title=driver.find_element(By.XPATH,"//ul[@class='list-unstyled venue-directory-list']/li[3]/div/p[3]").text
            except NoSuchElementException:
                directory_third_title = "No value"
            row_data.append(directory_third_title)
            #time.sleep(2)
            try:
                directory_third_phone=driver.find_element(By.XPATH,"//ul[@class='list-unstyled venue-directory-list']/li[3]/div/p[4]").text
            except NoSuchElementException:
                directory_third_phone = "No value"
            row_data.append(directory_third_phone)
            #time.sleep(2)
            try:
                directory_third_email=driver.find_element(By.XPATH,"//ul[@class='list-unstyled venue-directory-list']/li[3]/div/p[5]").text
            except NoSuchElementException:
                directory_third_email = "No value"
            row_data.append(directory_third_email)
            #time.sleep(2)
            try:
                directory_fourth_name=driver.find_element(By.XPATH,"//ul[@class='list-unstyled venue-directory-list']/li[4]/div/p[1]").text
            except NoSuchElementException:
                directory_fourth_name = "No value"
            row_data.append(directory_fourth_name)
            #time.sleep(2)
            try:
                directory_fourth_title=driver.find_element(By.XPATH,"//ul[@class='list-unstyled venue-directory-list']/li[4]/div/p[3]").text
            except NoSuchElementException:
                directory_fourth_title = "No value"
            row_data.append(directory_fourth_title)
            #time.sleep(2)
            try:
                directory_fourth_phone=driver.find_element(By.XPATH,"//ul[@class='list-unstyled venue-directory-list']/li[4]/div/p[4]").text
            except NoSuchElementException:
                directory_fourth_phone = "No value"
            row_data.append(directory_fourth_phone)
            #time.sleep(2)
            try:
                directory_fourth_email=driver.find_element(By.XPATH,"//ul[@class='list-unstyled venue-directory-list']/li[4]/div/p[5]").text
            except NoSuchElementException:
                directory_fourth_email = "No value"
            row_data.append(directory_fourth_email)
            csv_writer.writerow(row_data)
            #time.sleep(2)
            url=driver.current_url
            #time.sleep(3)
            print(url,end="  ")
            print()

        next_page = driver.find_element(By.XPATH, "//i[@class='gw-icon-chevron-right-thin']")
        if 'disabled' in next_page.get_attribute('class'):
            print("No more pages available. Exiting loop.")
            break
        next_page.click()
        time.sleep(5)
