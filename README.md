# Splunk-demo-notes
Notes / Demo on Splunk

## 1. Splunk on ec2

- Provision an AML ec2 instance 
```
sudo wget -O splunk-8.2.5-77015bc7a462-Linux-x86_64.tgz "https://download.splunk.com/products/splunk/releases/8.2.5/linux/splunk-8.2.5-77015bc7a462-Linux-x86_64.tgz"
sudo mv splunk-8.2.5-77015bc7a462-Linux-x86_64.tgz /opt
cd /opt
sudo tar -xvzf splunk-8.2.5-77015bc7a462-Linux-x86_64.tgz 
ls /opt
cd /opt/splunk/bin
sudo ./splunk start --accept-license
```
<img width="826" alt="image" src="https://user-images.githubusercontent.com/49971693/167541066-810ff5c0-0ab2-49f8-a589-60e85c237280.png">

- Test at ec2-public-ip:8000 using the username and password created while installing 

## 2. Search Processing Language SPL

- To query, calculate, transform, organize, visualize & manipulate data using Search & reporting app
- Index data, build reports & visualizations, configure alerts, create dashboards
- timestamps are converted into UNIX time and stored in the _time field and it is a **default and essential** field.
- Real time - like a refresh rate, relative - as upto right now and other.
- Time ranges using SPL 
- <img width="522" alt="image" src="https://user-images.githubusercontent.com/49971693/167684806-05e82c63-93ee-48e7-bc72-f0fcd499679e.png">
<img width="307" alt="image" src="https://user-images.githubusercontent.com/49971693/167684856-fcdc4c5f-2f17-4ac6-8a44-22c1798e648e.png">
- Date Variables
<img width="503" alt="image" src="https://user-images.githubusercontent.com/49971693/167685464-698ef436-1171-4304-b3df-83d01e04a918.png">

- Time Variables
 <img width="498" alt="image" src="https://user-images.githubusercontent.com/49971693/167685060-f00e15b5-5d94-4efa-9b9a-bcdd2fe38295.png">
<img width="616" alt="image" src="https://user-images.githubusercontent.com/49971693/167685238-6c3b872b-402b-48c1-b5e4-0d5a88715c23.png">

## Basic Searching
 - Broad search terms - metadata
   - index
     - index=main, index=default
   - host
     - host = server.com, host = 111.111.111.111
   - source, sourcetype
     - source = /var/lib, sourcetype = csv
   - Keywords
     - failed, error
   - Phrases
     - "failed login" 
   - Fields
     - user=user1.domain.com
   - Wildcards
     - *ailed, fail*
   - Booleans 
     - AND OR NOT
 - Basic search commands
   - chart / timechart
   - rename
   - sort
   - stats
   - eval
   - dedup
   - Table
 - **Search Terms | Commands**
 - host=myhost.lcc source=hstlogs user=* (message=fail* OR message=lock*) | table _time user message | sort -_time
 - the space in the search term is a boolean AND
