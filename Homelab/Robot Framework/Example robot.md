Create file names init.robot
```
*** Settings ***
Documentation As a User
... I want to find and verify a product name
... When I view a list of product prices
Resource base_definitions.robot
Resource home_page_step_definitions.robot
Resource product_category_page_step_definitions.robot
Resource web_base_keywords.robot
Library BuiltIn
Test Setup web_base_keywords.Set Up For Browser Tests
Test Teardown web_base_keywords.Tear Down For Browser Tests
Metadata Feature Product names can be verified


*** Test Cases ***
I open the Advantage Online website
I open the Advantage Online website
Go To ${production_url}
wait for home page to load
```

create file  base_definitions.robot

```
*** Settings ***
Documentation Implement steps for base operations
Library SeleniumLibrary
Variables configs/environment.py
Resource adv_home_page.robot

*** Keywords ***
I open the Advantage Online website
Go To ${production_url}
wait for home page to load
```

home_page_step_definitions.robot

```
*** Settings ***
Documentation Implement steps for the home page
Resource adv_home_page.robot

*** Keywords ***
I open the speakers page
open speakers page

I open the mice page
open mice page

I open the headphones page
open headphones page
```

product_category_page_step_definitions.robot

```
*** Settings ***
Documentation Implements steps for the product category page
Library Collections
Library BuiltIn
Resource adv_product_category_page.robot

  

*** Variables ***
${testProduct} ${EMPTY}
${testAccordian} ${EMPTY}

  

*** Keywords ***
I find a product with the name ${productName}
${product} find product Match By Title ${productName}
set test variable ${testProduct} ${product}
the price should be ${productPrice}
${price} get product price ${testProduct}
should be equal as strings ${price} ${productPrice}

I find a product with the price ${productPrice}
${product} find product Match By Price ${productPrice}
set test variable ${testProduct} ${product}
the name should be ${productName}
${name} get product title ${testProduct}
should be equal as strings ${name} ${productName}

  

I expand the accordian ${filterTitle}
${accordian} Find And Expand Filter ${filterTitle}
set test variable ${testAccordian} ${accordian}
I select the filter item ${filterItem}
Select Filter Item ${testAccordian} ${filterItem}
the filter items count should be ${filterCount}
${count} Get Filter Item Count
Should be equal as integers ${count} ${filterCount}
the page should contain ${productCount} products
${containerTimeoutSec}= Set Variable 5
${containerCount} Get Count Of Product Containers ${productCount} ${containerTimeoutSec}
should be equal as integers ${containerCount} ${productCount}
the products sold out status should be ${soldOutStatus}
${soldOutDisplayed} Is Product Sold Out Displayed ${testProduct}
should be equal as strings ${soldOutDisplayed} ${soldOutStatus}
```

Create web_base_keywords.robot
```
**** Settings ***
Documentation Base keywords for opening and closing a browser session
Library SeleniumLibrary
Library OperatingSystem
Variables configs/environment.py

*** Variables ***
${browser} headlesschrome
${defaultURL} about:blank
${pageSpinner} css:body>div.loader
${modalSpinner} css:div.PopUp div.loader

*** Keywords ***
Set Up For Browser Tests
[Documentation] Pre test setup. Opens the browser, maximizes the window, sets the implicit wait and clears cookies
${driverpath}= Get Environment Variable CHROME_DRIVER
open browser ${defaultURL} ${browser} options=add_argument("--headless");add_argument("--no-sandbox");add_argument("--window-size=1920,1080");add_argument("--ignore-certificate-errors") executable_path=${driverpath}
#open browser ${defaultURL} ${browser} options=add_argument("--ignore-certificate-errors") executable_path=${driverpath}
maximize browser window
set selenium implicit wait 2
delete all cookies

Tear Down For Browser Tests
[Documentation] Post test tear down. Closes the open browser
close browser
Wait Until Loading Spinners Disappear
wait until element is not visible ${pageSpinner} timeout=5
wait until element is not visible ${pageSpinner} timeout=5
```
`