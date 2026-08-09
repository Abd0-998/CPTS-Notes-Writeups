## Manual Banner Grabbing & Header Analysis
```
# Fetch HTTP response headers only (HEAD request)
curl -I http://<target_domain>

# Follow redirects manually to inspect header changes
curl -I https://<target_domain>
curl -I https://www.<target_domain>
```
#### Critical Headers to Inspect
- **`Server`**: Discloses the web server software and OS version (e.g., `Server: Apache/2.4.41 (Ubuntu)`).
    
- **`X-Powered-By`**: Reveals underlying backend technologies or frameworks (e.g., PHP, ASP.NET, Express).
    
- **`X-Redirect-By`**: Often discloses the application handling traffic redirects (e.g., `X-Redirect-By: WordPress`).
    
- **`Link`**: Look for API endpoints or CMS framework signatures (e.g., `[https://www.example.com/wp-json/](https://www.example.com/wp-json/); rel="[https://api.w.org/](https://api.w.org/)"` indicates a WordPress REST API).
## (WAF) Detection
#### wafw00f
```
# Installation via Pip
pip3 install git+https://github.com/EnableSecurity/wafw00f

# Run WAF detection against target
wafw00f <target_domain>
```
## Automated Software Identification
#### Nikto
While Nikto is primarily a web vulnerability scanner, it includes specific tuning flags to isolate software fingerprinting tests.
```
# Installation
git clone https://github.com/sullo/nikto
cd nikto/program
chmod +x ./nikto.pl

# Run software identification modules ONLY
nikto -h <target_domain> -Tuning b
```
#### Key Flags & Outputs
- `-h`: Specifies the target hostname or IP address.
    
- `-Tuning b`: Restricts Nikto to run **Software Identification** modules only (keeps the scan lightweight and focused on technology profiling).
    
- **Key Findings to Look For:**
    
    - Outdated server software versions (e.g., Apache 2.4.41).
        
    - Exposed license or documentation files (e.g., `/license.txt`).
        
    - Discovered CMS login administrative paths (e.g., `/wp-login.php`).
        
    - Missing security headers (e.g., `Strict-Transport-Security`, `X-Content-Type-Options`).

