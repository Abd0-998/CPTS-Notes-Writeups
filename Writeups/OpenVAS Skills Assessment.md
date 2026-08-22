
# OpenVAS Skills Assessment

---

You have been contracted by the company `Inlanefreight` to perform an internal vulnerability assessment against one of their servers. They have asked for a cursory assessment to be performed to identify any significant vulnerabilities as they do not have the budget for a full-scale penetration test this year. The results of this vulnerability assessment may enable the CISO to push for additional funding from the Board of Directors to perform more in-depth security testing.

The target server is a Linux Server host.

---

## Requirements

Navigate to the OpenVAS web interface at the server below and log in with the provided credentials.

Once logged in, create a new task with the `OpenVAS Default` Scanner and use the `Full and Fast` config against the target: `172.16.16.160`. Additionally, ensure you have the scan set up to run as an authenticated user using the credentials: `root:HTB_@cademy_admin!`.

The scan will take up to 60 minutes to finish.

_Note: It may take 1-2 minutes for your target instance to spawn._

Alternatively, use the pre-populated scan data to answer the questions below without having to wait for the scan to finish but feel free to practice configuring and running it.


## Step By Step 

### Q1 

- What type of operating system is the Linux host running? (one word)

- Navigate to the Assets Tab --> select hosts --> select the linux host ip --> click show all identifiers --> check the linux os distru 
### Q2

- What type of FTP vulnerability is on the Linux host? (Case Sensitive, four words)

- navigate Scans tab --> select tasks --> click on the report of linux basic --> select results tab --> find the FTP vulnerability 

### Q3

- What is the IP of the Linux host targeted for the scan?

- the answer was given in the task 

### Q4 

- What vulnerability is associated with the HTTP server? (Case-sensitive)

- follow same steps of question 2
