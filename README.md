## Amazon Price Tracker for iPhone 15
This project is a web scraper built in Python that checks the price of the iPhone 15 on Amazon and sends an email alert when the price drops below $1,000. The price data is collected daily and stored in a CSV file.

### Amazon link:
[Iphone15](https://www.amazon.com.au/Apple-iPhone-15-128-GB/dp/B0CHXCQPF8/ref=sr_1_2?crid=2Z7TSNW7UFEMP&dib=eyJ2IjoiMSJ9.V3DelHIRA8hjMC3riN0FBw65gpSxJZRgczynnK8MQ8j0TiLcHspq0Wi33klvS1dfeOaDZC6EODaVWvYYyEz4AHibAi6bRPl7xVmF3pjtyIsdkG-XOU4y5ZKMvnpwiSQQGOV1Yp60jR-N74FyisJrkq7uhRfzbHnHUBUPCoJgJ-CN9wjOrw2daZrLboZXQF2wQhWupCs5NNXYMyd6q-ZanJ_0SQq_wjCsPj3ipUgu3FNwp9G9bHP6HvLgPF0ccPBuw0oakUSfdqF2oCxmi4FkvY1NmVCK5g32A4a4WlVi2m0.CvArOIA634dGky7MszBaWBIyk5KfCUTX5XED-04WtfQ&dib_tag=se&keywords=iphone+15&qid=1719481924&sprefix=iphone+15%2Caps%2C448&sr=8-2)

### Resources:
- Python (Jupyter)
- pandas
- BeautifulSoup
- Requests
- smtplib
- time
- datetime

### Step 1: import libraries
```python
from bs4 import BeautifulSoup
import requests
import time
import datetime
import smtplib
```

### Step 2: Connect to Website and pull in data
```python
URL = 'https://www.amazon.com.au/Apple-iPhone-15-128-GB/dp/B0CHXCQPF8/ref=sr_1_2?crid=2Z7TSNW7UFEMP&dib=eyJ2IjoiMSJ9.V3DelHIRA8hjMC3riN0FBw65gpSxJZRgczynnK8MQ8j0TiLcHspq0Wi33klvS1dfeOaDZC6EODaVWvYYyEz4AHibAi6bRPl7xVmF3pjtyIsdkG-XOU4y5ZKMvnpwiSQQGOV1Yp60jR-N74FyisJrkq7uhRfzbHnHUBUPCoJgJ-CN9wjOrw2daZrLboZXQF2wQhWupCs5NNXYMyd6q-ZanJ_0SQq_wjCsPj3ipUgu3FNwp9G9bHP6HvLgPF0ccPBuw0oakUSfdqF2oCxmi4FkvY1NmVCK5g32A4a4WlVi2m0.CvArOIA634dGky7MszBaWBIyk5KfCUTX5XED-04WtfQ&dib_tag=se&keywords=iphone+15&qid=1719481924&sprefix=iphone+15%2Caps%2C448&sr=8-2'

headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.108 Safari/537.36", "Accept-Encoding":"gzip, deflate", "Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", "DNT":"1","Connection":"close", "Upgrade-Insecure-Requests":"1"}

page = requests.get(URL, headers=headers)

soup1 = BeautifulSoup(page.content, "html.parser")

soup2 = BeautifulSoup(soup1.prettify(), "html.parser")

title = soup2.find(id='productTitle').get_text().strip()

price = soup2.find('span', {'class':'aok-offscreen'}).get_text().strip()

print(title)
print(price)
#check title and price.
```
### Step 3: Create a Timestamp for your output to track when data was collected
```python
import datetime

today = datetime.date.today()

print(today)
```

### Step 4: Create CSV and Write Headers and Data
```python
import csv 

header = ['Title', 'Price', 'Date']
data = [title, price, today]


with open('Amazonpricecheckip15.csv', 'w', newline='', encoding='UTF8') as f:
    writer = csv.writer(f)
    writer.writerow(header)
    writer.writerow(data)

import pandas as pd

df = pd.read_csv(r'your file path\Amazonpricecheckip15.csv')

print(df)
```
### Step 5: Append Data to the CSV
```python
with open('Amazonpricecheckip15.csv', 'a+', newline='', encoding='UTF8') as f:
    writer = csv.writer(f)
    writer.writerow(data)
```
### Step 6: Automate Price Checking and Email Notifications
This step combines all previous steps into a single function that automates the price checking process. The function will scrape the iPhone 15 price from Amazon daily and append the data to a CSV file. If the price drops below $1,000, it will trigger an email notification to alert you.
```python
def check_price():
    URL = 'https://www.amazon.com.au/Apple-iPhone-15-128-GB/dp/B0CHXCQPF8/ref=sr_1_2?crid=2Z7TSNW7UFEMP&dib=eyJ2IjoiMSJ9.V3DelHIRA8hjMC3riN0FBw65gpSxJZRgczynnK8MQ8j0TiLcHspq0Wi33klvS1dfeOaDZC6EODaVWvYYyEz4AHibAi6bRPl7xVmF3pjtyIsdkG-XOU4y5ZKMvnpwiSQQGOV1Yp60jR-N74FyisJrkq7uhRfzbHnHUBUPCoJgJ-CN9wjOrw2daZrLboZXQF2wQhWupCs5NNXYMyd6q-ZanJ_0SQq_wjCsPj3ipUgu3FNwp9G9bHP6HvLgPF0ccPBuw0oakUSfdqF2oCxmi4FkvY1NmVCK5g32A4a4WlVi2m0.CvArOIA634dGky7MszBaWBIyk5KfCUTX5XED-04WtfQ&dib_tag=se&keywords=iphone+15&qid=1719481924&sprefix=iphone+15%2Caps%2C448&sr=8-2'

    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.108 Safari/537.36", "Accept-Encoding":"gzip, deflate", "Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", "DNT":"1","Connection":"close", "Upgrade-Insecure-Requests":"1"}

    page = requests.get(URL, headers=headers)

    soup1 = BeautifulSoup(page.content, "html.parser")

    soup2 = BeautifulSoup(soup1.prettify(), "html.parser")

    title = soup2.find(id='productTitle').get_text().strip()

    price = soup2.find('span', {'class':'aok-offscreen'}).get_text().strip()
    
    import datetime

    today = datetime.date.today()

    import csv 

    header = ['Title', 'Price', 'Date']
    data = [title, price, today]

    with open('Amazonpricecheckip15.csv', 'a+', newline='', encoding='UTF8') as f:
        writer = csv.writer(f)
        writer.writerow(data)

    if(price<1,000.00):
       send_mail()
```
### Step 7: Send Email Notification
```python
while(True):
    check_price()
    time.sleep(86400)
import pandas as pd

df = pd.read_csv(r'C:\Users\dinht\OneDrive\Desktop\học data\Học Python\Amazonpricecheckip15.csv')

print(df)
```
### Step 8: send email when we get this price (setup an email)
```python
def send_mail():
    server = smtplib.SMTP_SSL('smtp.gmail.com',465)
    server.ehlo()
    #server.starttls()
    server.ehlo()
    server.login('your email account ex: dinhthanhtrung2011@gmail.com','your password')
    
    subject = "Price Alert: iPhone 15 below $1000!"
    body = "The price of the iPhone 15 has dropped below $1000. Check the Amazon link :https://www.amazon.com.au/Apple-iPhone-15-128-GB/dp/B0CHXCQPF8/ref=sr_1_2?crid=2Z7TSNW7UFEMP&dib=eyJ2IjoiMSJ9.V3DelHIRA8hjMC3riN0FBw65gpSxJZRgczynnK8MQ8j0TiLcHspq0Wi33klvS1dfeOaDZC6EODaVWvYYyEz4AHibAi6bRPl7xVmF3pjtyIsdkG-XOU4y5ZKMvnpwiSQQGOV1Yp60jR-N74FyisJrkq7uhRfzbHnHUBUPCoJgJ-CN9wjOrw2daZrLboZXQF2wQhWupCs5NNXYMyd6q-ZanJ_0SQq_wjCsPj3ipUgu3FNwp9G9bHP6HvLgPF0ccPBuw0oakUSfdqF2oCxmi4FkvY1NmVCK5g32A4a4WlVi2m0.CvArOIA634dGky7MszBaWBIyk5KfCUTX5XED-04WtfQ&dib_tag=se&keywords=iphone+15&qid=1719481924&sprefix=iphone+15%2Caps%2C448&sr=8-2"
   
    msg = f"Subject: {subject}\n\n{body}"
    
    server.sendmail(
        'dinhthanhtrung2011@gmail.com',
        msg
     
    )
```
## Contact Information

For further information, collaboration, or inquiries, feel free to contact [Phil Dinh](mailto:dinhthanhtrung2011@gmail.com).
