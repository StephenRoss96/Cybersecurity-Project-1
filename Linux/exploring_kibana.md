## Activity File: Exploring Kibana

### Instructions

Part 2:
- In the last 7 days, how many unique visitors were located in India?
	There were 223 unique visitors from India

- In the last 24 hours, of the visitors from China, how many were using Mac OSX?
	13

- In the last 2 days, what percentage of visitors received 404 errors? How about 503 errors?
	404: 21
	503: 10

- In the last 7 days, what country produced the majority of the traffic on the website?
	No one country produces a majority of traffic. China generates the most at about 20%, India 15.7%, and USA 9.4%, with the remainder split between many other nations. 

- Of the traffic that's coming from that country, what time of day had the highest amount of activity?
	9-10am, and 12-1pm are roughly equal at 37 and 38, respectively.

- List all the types of downloaded files that have been identified for the last 7 days, along with a short description of each file type (use Google if you aren't sure about a particular file type).
	| File Type | Name                    | Description                                           |
	|-----------|-------------------------|-------------------------------------------------------|
	| .css      | Cascading Style Sheets  | Define the layout of HTML elements.                   |
	| .deb      | Debian                  | Software package for the Debian Linux distribution.   |
	| .gz       | Gzip                    | Archive file compressed by the GNU zip algorithm      |
	| .rpm      | Red Hat Package Manager | Installation package for Linux. Developed by Red Hat. |
	| .zip      | ZIP Archive             | Commonly-used compression archive format.             |
	| N/A       | Unknown file type       | File type not specified.                              |


3. Now that you have a feel for the data, Let's dive a bit deeper. Look at the chart that shows Unique Visitors Vs. Average Bytes.
- Locate the time frame in the last 7 days with the most amount of bytes (activity).
	Dec 26, 9pm-12pm  -->  Avg.Bytes= 10 735.667 ;   Count = 3
- In your own words, is there anything that seems potentially strange about this activity?
	While this time period has the highest average bytes, it also has one of the lowest number of unique visitors. 
	It appears that the time frames with the highest number of visitors will often, but not always, have an byte count that is average for that day. When there are more visitors, the increased count will average out the visitors with very high or very low byte counts. 
	When unique visitors is low, it increases the influence each visitor has over the average byte count. 

4. Filter the data by this event.
- What is the timestamp for this event?				Dec 26, 2021 @ 21:00:00.0 -> Dec 27, 2021 @ 00:00:00.0
- What kind of file was downloaded?				One .rpm file at 21:55 and one .zip file at 23:05
- From what country did this activity originate?		China and India
- What HTTP response codes were encountered by this visitor?	HTTP response code 200L OK

5. Switch to the Kibana Discover page to see more details about this activity. (event at 21:55)
- What is the source IP address of this activity? 		35.143.166.159 
- What are the geo coordinates of this activity? 		{ "lat": 43.34121, "lon": -73.6103075 }
- What OS was the source machine running?			Windows 8
- What is the full URL that was accessed?			https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.3.2-i686.rpm
- From what website did the visitor's traffic originate?	http://facebook.com/success/jay-c-buckey

6. Finish your investigation with a short overview of your insights. 
- What do you think the user was doing?
	The user was likely downloading metricbeat version 6.3.2
- Was the file they downloaded malicious? If not, what is the file used for?
	The file is unlikely to be malicious because elastic.co is the website of a large corporate, thus more likely to be trustworthy. This is an earlier version of the software we use to monitor system metrics. 
	The file was likely installed in a docker or kubernetes container for the purposes of monitoring system metrics with an ELK stack.
- Is there anything that seems suspicious about this activity?
	The software was an old version of metricbeat and a newer version of the software is available. One could check google and patch notes to see if this version is used for compatibility reasons. 
	It is possible that the files were downloaded with the intention of browsing the source code for vulnerabilities. It is possible that a bad actor is aware their target has not patched their metricbeat installation recently, and wishes to exploit that oversight. 
	Another possibility is that the agent is going to use the code as a cover for deploying malware to a target system. Since the ansible playbooks use python3, the agent could modify the package to execute hidden scripts inserted into the long configuration files or in hidden folders under /etc/ansible/files/. 
- Is any of the traffic you inspected potentially outside of compliance guidlines?
	The third event not mentioned above originates from a machine running Windows XP. This is strange, as long-term support for Windows XP ended in April 2014. While many embedded systems may still run Windows XP, it is not a secure system for consumers. 
	All three events were related to downloading software from elastic, and are thus unlikely to be outside of compliance. 

---
© 2020 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.  