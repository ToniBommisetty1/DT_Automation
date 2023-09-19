from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

#Launch of the application URL(https://www.bt.com/)
driver = webdriver.Firefox()
driver.get("https://www.bt.com/")

# Close the accept Cookie pop-up if it appears (assuming it's an alert)
try:
    WebDriverWait(driver, 10).until(EC.alert_is_present())
    alert = driver.switch_to.alert
    alert.accept()
except:
    pass  # No alert found, continue

# Hover over the "Mobile" menu item
mobile_menu = driver.find_element(By.XPATH, "//span[contains(text(),'Mobile')]")
ActionChains(driver).move_to_element(mobile_menu).perform()

# Click on the "Mobile phones" sub-menu item
#driver.find_element(By.XPATH, "//a[contains(text(),'Mobil phones')]").click()
driver.get('https://www.bt.com/products/mobile/phones/')


# Verify the number of banners present below "See Handset details" should not be less than 3
banners = driver.find_elements(By.XPATH, "//div[contains(@class, 'product-banner')]")

if len(banners) >= 3:
    print("Banners count is valid.")
else:
    print("Banners count is less than 3.")


# Scroll down and click View SIM only deals
driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
view_sim_deals_button = driver.find_element(By.XPATH, "//a[contains(text(), 'View SIM only deals')]")
view_sim_deals_button.click()

# Validate the title for the new page
new_page_title = driver.title
print(f"Title of the new page: {new_page_title}")


# Validate "30% off and double data" on the 125GB 250GB Essential Plan
offer_text = "30% off and double data"
plan_text = "125GB 250GB Essential Plan"
original_price_text = "was £27"
discounted_price_text = "£18.90 per month"

if offer_text in driver.page_source and plan_text in driver.page_source and original_price_text in driver.page_source and discounted_price_text in driver.page_source:
    print("Validation successful.")
else:
    print("Validation failed.")


# Close the browser and exit
driver.quit()
