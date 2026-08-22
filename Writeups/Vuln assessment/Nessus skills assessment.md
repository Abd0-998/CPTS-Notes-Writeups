## Given Data :

---

You have been contracted by the company `Inlanefreight` to perform an internal vulnerability assessment against one of their servers. They have asked for a cursory assessment to be performed to identify any significant vulnerabilities as they do not have the budget for a full-scale penetration test this year. The results of this vulnerability assessment may enable the CISO to push for additional funding from the Board of Directors to perform more in-depth security testing.

The target server is a Windows Server host used as a development server.

#### Requirements

Navigate to the web interface at the end of this section and log in with the provided credentials.

Once logged in, perform a `BASIC NETWORK SCAN` (modify the scan template to scan `ALL` ports, leave all other options the same) against the target: `172.16.16.100`. Additionally, set up the scan to be authenticated using `administrator:Academy_VA_adm1!` as the credentials.

The scan will take up to 60 minutes to finish.

_Note: It may take 1-2 minutes for your target instance to spawn. Additionally, it may take up to an hour for the scan to run_

Alternatively, use the pre-populated scan data to answer the questions below without having to wait for the scan to finish but feel free to practice configuring and running it.
## Step By Step

- First as required in the assessment i created a new Basic Network scan and named it Network scan and then added the target ip : `172.16.16.100`

<img width="1068" height="553" alt="image" src="https://github.com/user-attachments/assets/08ab4a76-93b6-4e47-b5b3-32b8712c04d8" />


- then in the Discovery toggle i selected the option : Port scan all ports 

<img width="1062" height="540" alt="image" src="https://github.com/user-attachments/assets/e78f669a-b8a3-4889-8866-753511f809dc" />


- then i added the credentials given to be the auth method in this scan 

<img width="1056" height="380" alt="image" src="https://github.com/user-attachments/assets/5f4476e3-f2da-47de-82b6-c33d71e8d741" />


*NOTE* : this scan could take 60 min , in the assessment there are a ready results of the scans to do the assessment on it without wating
### First Question : 

1- What is the name of one of the accessible SMB shares from the authenticated Windows scan? (One word)

- in the My Scans Tab i selected the Windows_basic_authed scan results and selected the Vulnerabilities Tab and searched for SMB shares : 

<img width="1266" height="710" alt="image" src="https://github.com/user-attachments/assets/b83146f6-2eb6-4571-82d2-1d3e3af80221" />


- as we see there was only 2 results both of them reveals the names of the accessible SMB shares submit any share name as the answer 

### Second Question

2- What was the target for the authenticated scan?

- get back to the My Scans file and choose the Windows_basic_authed and select the hosts tab it will reveal the target ip of the scan

<img width="1255" height="568" alt="image" src="https://github.com/user-attachments/assets/207fbaa5-065d-4030-8dd6-97f42e2bcb99" />


### Third Question 

3- What is the plugin ID of the highest criticality vulnerability for the Windows authenticated scan?

- again select My Scans tab then choose the  Windows_basic_authed scan and then select the Vulnerabilities tab and click on the score toggle to sort it with the highest CVSS score : 

<img width="1224" height="693" alt="image" src="https://github.com/user-attachments/assets/9a8b74d3-fa7f-49f5-9d99-a5285a67d17f" />


- then choose the first one at the top and get its plugin id : 

<img width="1079" height="335" alt="image" src="https://github.com/user-attachments/assets/ca147a96-4668-4762-b4c0-bb7d5388c9e9" />


### Fourth Question 

4- What is the name of the vulnerability with plugin ID 26925 from the Windows authenticated scan? (Case sensitive)

- first click on the report button on the top right to make a report withe all the included vulnerabilities  :

<img width="1089" height="361" alt="image" src="https://github.com/user-attachments/assets/f5da58cf-ce96-47c7-bf85-b71223a3309c" />


- then choose the report format to be .csv and make sure to check true on the plugin ID checkbox 

<img width="622" height="447" alt="image" src="https://github.com/user-attachments/assets/08655c30-2e48-4bcf-8154-e8d22e310b3c" />


- then download the report and cd to the directory where it was downloaded and use this command to find the Vulnerability name of the Plugin id : 26925
```
cat Windows_basic_authed_3lk7gh.csv | grep 26925
```

<img width="728" height="92" alt="image" src="https://github.com/user-attachments/assets/a0f38fd3-e6ab-41ab-9615-6cb5a45ced09" />


### Fifth Question 

5- What port is the VNC server running on in the authenticated Windows scan?

- the port is 5900 as shown in the appove search results
