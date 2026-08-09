## Givne Data :

To complete the skills assessment, answer the questions below. You will need to apply a variety of skills learned in this module, including:

- Using `whois`
- Analysing `robots.txt`
- Performing subdomain bruteforcing
- Crawling and analysing results

Demonstrate your proficiency by effectively utilizing these techniques. Remember to add subdomains to your `hosts` file as you discover them.

**vHosts needed for these questions:**

- inlanefreight.htb

## **What is the IANA ID of the registrar of the [inlanefreight.com](http://inlanefreight.com) domain?**

- to answer this question we will use Whois command to show the registrar of the domain `whois inlanefreight.com`

<img width="874" height="309" alt="image" src="https://github.com/user-attachments/assets/fd1e2e69-372a-4fb1-b15a-788a71737041" />

- as we can see the IANA ID is 468

## **What HTTP server software is powering `inlanefreight.htb` on the target system?**

- The http server software can be revealed in the response header , there are many ways to get the response headers but the fastest is Curl

<img width="676" height="311" alt="image" src="https://github.com/user-attachments/assets/a0385ba5-6c98-457a-a52d-189cd9a3c888" />


- the  web server is **nginx**.

## **What is the API key in the hidden admin directory you discovered?**

- To find hidden directories we need to Brute force directories
- After brute forcing for directories there was no hidden directories for this domain
- So , lets Try first to find a Virtual host in that domain
- 
<img width="1916" height="573" alt="image" src="https://github.com/user-attachments/assets/c9740110-3cc8-48eb-b3a2-f73c94bf1dbd" />


- the results found a vhost called web1337.inlanefreight.htb
- lets add it to the /etc/hosts file

<img width="846" height="336" alt="image" src="https://github.com/user-attachments/assets/bf46d10b-b261-4738-b1ff-b269c216f5be" />


- now lets check the robots.txt file for this vhost

<img width="922" height="440" alt="image" src="https://github.com/user-attachments/assets/4ca01900-e7ea-4e6f-b400-1fa2c7cf3c23" />


- this revealed the hidden admin directory

<img width="1913" height="373" alt="image" src="https://github.com/user-attachments/assets/0889dac9-256d-4fa1-9093-5121c4012646" />


- and here it is the API key

## **After crawling the `inlanefreight.htb` domain, what email address did you find?**

- to answer this question we need to use a automated crawling tool such as reconspider

```
python3 ReconSpider.py http://inlanefreight.htb:30498
```

<img width="521" height="302" alt="image" src="https://github.com/user-attachments/assets/6a07ad53-1f2c-4347-9ef2-e314f454d3e4" />


- there was no result
- lets try the vhost : web1337.inlanefreight.htb

```
python3 ReconSpider.py http://web1337.inlanefreight.htb:30498
cat results.json
```

<img width="355" height="278" alt="image" src="https://github.com/user-attachments/assets/052375e9-2a6e-4d90-aff2-386d17dbc858" />


- there was no results again
- we can try to find a sub vhost for the vhost we found

<img width="1903" height="569" alt="image" src="https://github.com/user-attachments/assets/0dce48a3-e757-40b3-bc30-1c187bb2322e" />


- the results found a new vhost called : dev.web1337.inlanefreight.htb:30498
- lets add it also to the /etc/hosts file and run the reconspider on it

<img width="601" height="158" alt="image" src="https://github.com/user-attachments/assets/e6e00267-3890-426f-bc16-a1050fdcaf5e" />


- and we found the email
- also the crawling results.json file revealed the answer of the last question :

## **What is the API key the inlanefreight.htb developers will be changing too?**

<img width="1150" height="278" alt="image" src="https://github.com/user-attachments/assets/929b27c0-919b-478a-bbdc-34bb4b3a8207" />
