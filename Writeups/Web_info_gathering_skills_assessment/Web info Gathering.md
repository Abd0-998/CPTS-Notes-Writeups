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
![[Pasted image 20260809191729.png]]

- as we can see the IANA ID is 468

## **What HTTP server software is powering `inlanefreight.htb` on the target system?**

- The http server software can be revealed in the response header , there are many ways to get the response headers but the fastest is Curl

![[Pasted image 20260809191815.png]]

- the  web server is **nginx**.

## **What is the API key in the hidden admin directory you discovered?**

- To find hidden directories we need to Brute force directories
- After brute forcing for directories there was no hidden directories for this domain
- So , lets Try first to find a Virtual host in that domain
- 
![[Pasted image 20260809191835.png]]

- the results found a vhost called web1337.inlanefreight.htb
- lets add it to the /etc/hosts file

![[Pasted image 20260809191912.png]]

- now lets check the robots.txt file for this vhost

![[Pasted image 20260809191933.png]]

- this revealed the hidden admin directory

![[Pasted image 20260809192002.png]]

- and here it is the API key

## **After crawling the `inlanefreight.htb` domain, what email address did you find?**

- to answer this question we need to use a automated crawling tool such as reconspider

```
python3 ReconSpider.py http://inlanefreight.htb:30498
```

![[Pasted image 20260809192055.png]]

- there was no result
- lets try the vhost : web1337.inlanefreight.htb

```
python3 ReconSpider.py http://web1337.inlanefreight.htb:30498
cat results.json
```

![[Pasted image 20260809192121.png]]

- there was no results again
- we can try to find a sub vhost for the vhost we found

![[Pasted image 20260809192145.png]]

- the results found a new vhost called : dev.web1337.inlanefreight.htb:30498
- lets add it also to the /etc/hosts file and run the reconspider on it

![[Pasted image 20260809192205.png]]

- and we found the email
- also the crawling results.json file revealed the answer of the last question :

## **What is the API key the inlanefreight.htb developers will be changing too?**

![[Pasted image 20260809192229.png]]