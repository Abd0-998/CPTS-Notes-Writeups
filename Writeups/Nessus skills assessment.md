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

![[Screenshot 2026-08-12 at 9.16.29 AM.png]]

- then in the Discovery toggle i selected the option : Port scan all ports 

![[Screenshot 2026-08-12 at 9.17.17 AM.png]]

- then i added the credentials given to be the auth method in this scan 

![[Screenshot 2026-08-12 at 9.20.25 AM.png]]

*NOTE* : this scan could take 60 min , in the assessment there are a ready results of the scans to do the assessment on it without wating
### First Question : 

1- What is the name of one of the accessible SMB shares from the authenticated Windows scan? (One word)

- in the My Scans Tab i selected the Windows_basic_authed scan results and selected the Vulnerabilities Tab and searched for SMB shares : 

![[Screenshot 2026-08-12 at 9.27.18 AM 1.png]]

- as we see there was only 2 results both of them reveals the names of the accessible SMB shares submit any share name as the answer 

### Second Question

2- What was the target for the authenticated scan?

- get back to the My Scans file and choose the Windows_basic_authed and select the hosts tab it will reveal the target ip of the scan

![[Screenshot 2026-08-12 at 9.34.22 AM.png]]

### Third Question 

3- What is the plugin ID of the highest criticality vulnerability for the Windows authenticated scan?

- again select My Scans tab then choose the  Windows_basic_authed scan and then select the Vulnerabilities tab and click on the score toggle to sort it with the highest CVSS score : 

![[Screenshot 2026-08-12 at 9.38.52 AM.png]]

- then choose the first one at the top and get its plugin id : 

![[Screenshot 2026-08-12 at 9.41.28 AM.png]]

### Fourth Question 

4- What is the name of the vulnerability with plugin ID 26925 from the Windows authenticated scan? (Case sensitive)

- first click on the report button on the top right to make a report withe all the included vulnerabilities  :

![[Screenshot 2026-08-12 at 9.46.56 AM.png]]

- then choose the report format to be .csv and make sure to check true on the plugin ID checkbox 

![[Screenshot 2026-08-12 at 9.48.43 AM.png]]

- then download the report and cd to the directory where it was downloaded and use this command to find the Vulnerability name of the Plugin id : 26925
```
cat Windows_basic_authed_3lk7gh.csv | grep 26925
```

![[Screenshot 2026-08-12 at 9.52.43 AM.png]]

### Fifth Question 

5- What port is the VNC server running on in the authenticated Windows scan?

- the port is 5900 as shown in the appove search results