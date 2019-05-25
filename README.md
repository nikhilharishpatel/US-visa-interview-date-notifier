# US Visa Interview
Facing the issue of not being able to find suitable US Visa Interview Date
An Email Notifier program (written in Python) which will send email notifications to you and your friends, if there are any Visa slots available before your desired Visa Interview date
 
  
   
    
    
# IMPORTANT: For your safety and avoiding account freeze create new account on CGI portal!

import smtplib
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.common.keys import Keys
from time import sleep

# Function for sending email to email list
def send_mail(latest_date):
    message = 'Latest Date: ' + latest_date
    email_sender = 'Sender.Email@gmail.com' # Modify Sender Email ID
	email_password = 'Your.Email.Password' # Modify the Password
    receiver = ['Receiver.1@gmail.com', 'Receiver.2@gmail.com', 'Receiver.3@gmail.com'] # Modify Receiver List

    try:
        mailserver = smtplib.SMTP('smtp.gmail.com', '587')
        mailserver.starttls()
        mailserver.login(email_sender, email_password)
        mailserver.sendmail(email_sender, receiver, message)
        mailserver.quit()
    except Exception as e:
        print('Error:',e)


# Setting up Selenium
driver = webdriver.Chrome('C:\\Py\\Scripts\\chrome\\chromedriver.exe') # Update the location of ChromeDriver here
url = 'https://cgifederal.secure.force.com/'
driver.get(url)
wait = WebDriverWait(driver, 900)
sleep(4)

username = 'your.CGIFederal.Email@gmail.com' # Modify Email ID for CGI portal
password = 'your.CGIFederal.Password' # Modify Password for CGI portal

user_input = driver.find_element_by_xpath('//*[@id="loginPage:SiteTemplate:siteLogin:loginComponent:loginForm:username"]')
user_input.send_keys(username)

pass_input = driver.find_element_by_xpath('//*[@id="loginPage:SiteTemplate:siteLogin:loginComponent:loginForm:password"]')
pass_input.send_keys(password)

privacy = driver.find_element_by_xpath('//*[@id="loginPage:SiteTemplate:siteLogin:loginComponent:loginForm:j_id131"]/table/tbody/tr[3]/td/label/input').click()
captcha = input('Please Enter Captcha and Login to CGI portal and then press Enter here')
i = 1

while True:
    print('Running Iteration:',i)
    driver.get('https://cgifederal.secure.force.com/applicanthome')
    wait = WebDriverWait(driver, 900)
    sleep(10)
    content = driver.find_element_by_xpath('//*[@id="ctl00_nav_side1"]/ul/div')
    date = content.text.split(' ')[-3:-1]
    print('Date', ' '.join(str(e) for e in date))
    if date[0] == 'July': # Enter the desired Month here
        if date[1] < '15': #Enter desired Date here
            for _ in range(5):
                send_mail(' '.join(str(e) for e in date))
    if date[0] == 'June': # Enter a month before the desired month
            for _ in range(5):
                send_mail(' '.join(str(e) for e in date))
    sleep(900) # Update the refresh frequency (Currently 15 mins)
    i += 1
 
 
 
Copyright (C) 2019 nikhilharishpatel

This program (visa_date_notifier.py) is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.
