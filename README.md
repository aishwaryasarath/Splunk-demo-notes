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
```
host=splunkmain backupduration=* domain=* | table _time backupduration domain
```
 <img width="1395" alt="image" src="https://user-images.githubusercontent.com/49971693/167697784-9b782ef1-269c-4040-8ced-935d1c3eea84.png">

```
host="splunkmain" | eval new_time=strftime(_time, "%m-%d-%y  %I:%M%p") | table _time new_time
```
- Extract field
  - regex
  - delimiter

## Search Pilepine

**Broadsearch Keywords/Booleans/fields | Commands | Table/Viz**

Commands: top, rare, stats
- top automatically builds a table with count & percent columns
- rare is least common
- stats <function(fields) by fields eg. stats avg(kbps) BY host 
- stats count(failed_logins) BY user

```
host="splunkmain"  state=* usr=* | stats count(usr) AS cuser BY state | sort -cuser
```
<img width="700" alt="image" src="https://user-images.githubusercontent.com/49971693/167710852-64bb37a4-0499-4b7d-b614-950258a04a19.png">

```
host="splunkmain" state=* level=critical | rare state by level
```
<img width="691" alt="image" src="https://user-images.githubusercontent.com/49971693/167711203-c68957da-2155-42cc-b735-8ac1aadebf89.png">
```
host="splunkmain" state=* level=critical | top state by level
```

<img width="684" alt="image" src="https://user-images.githubusercontent.com/49971693/167711448-f20452fc-9c99-4db8-a8af-985b83cc2f49.png">
