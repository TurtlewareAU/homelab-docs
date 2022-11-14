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

create file 

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