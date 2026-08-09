## Commands & Setup (Scrapy & ReconSpider)
#### Tool Installation & Execution

```
# Install Scrapy Python framework
pip3 install scrapy

# Download the ReconSpider custom crawler
wget -O ReconSpider.zip https://academy.hackthebox.com/storage/modules/144/ReconSpider.v1.2.zip

# Extract the archive
unzip ReconSpider.zip

# Run ReconSpider against a target domain
python3 ReconSpider.py http://<target_domain>
```

## Key Search Operators

```
site:target.com        // search to a specific domain
inurl:login            // contain a specific keyword in the URL
allinurl:admin panel   // all specified words to appear anywhere in the URL
filetype:pdf           // specific file extensions.
intitle:"index of /"   // Searches for specific words within the HTML page title
allintitle: <text>     // all listed terms to be present in the page title
intext: <text>         // Searches for strings within the body text
allintext: <text>      // Requires all listed terms to appear within the body
"" (Quotes)            // Forces exact phrase matching.
- (Minus Sign)         // Excludes specific terms or domains from the search 
* (Wildcard)           // Represents any character or word.
**                     // accepting matches for any listed term.
AND                    // Narrows results by requiring both conditions
```
## High-Value Google Dorking Queries

#### Finding Administrative & Login Panels
```
site:target.com inurl:login
site:target.com (inurl:login OR inurl:admin OR inurl:portal)
site:target.com intitle:"login"
```
#### Uncovering Exposed Configuration Files & Secrets
```
site:target.com inurl:config.php
site:target.com (ext:conf OR ext:cnf OR ext:env OR ext:yaml OR ext:xml)
site:target.com intext:"DB_PASSWORD"
```
#### Locating Sensitive Documents
```
site:target.com filetype:pdf
site:target.com (filetype:xls OR filetype:xlsx OR filetype:docx OR filetype:csv)
site:target.com intitle:"confidential report"
```
#### Discovering Database Backups & Dumps
```
site:target.com inurl:backup
site:target.com filetype:sql
site:target.com (ext:sql OR ext:db OR ext:bak OR ext:tar.gz)
```
#### Subdomain Discovery via Domain Exclusion
```
site:target.com -inurl:www
site:*.target.com -site:www.target.com
```