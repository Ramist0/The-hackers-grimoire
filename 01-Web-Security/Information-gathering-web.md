# Information gathering web edition

## WHOIS

WHOIS is a query and response protocol used to retrieve information about domain names, IP addresses, and other internet resources. It's essentially a directory service that details who owns a domain, when it was registered, contact information, and more.

In the context of web reconnaissance, WHOIS lookups can be a valuable source of information, potentially revealing the identity of the website owner, their contact information, and other details that could be used for further investigation or social engineering attacks : 

```
whois example.com
```

## DNS

The Domain Name System (DNS) functions as the internet's GPS, translating user-friendly domain names into the numerical IP addresses computers use to communicate. Like GPS converting a destination's name into coordinates, DNS ensures your browser reaches the correct website by matching its name with its IP address.

This eliminates memorizing complex numerical addresses, making web navigation seamless and efficient.

```
dig example.com A -> Maps a hostname to an IPv4 address.
dig example.com AAA -> Maps a hostname to an IPv6 address.
dig example.com CNAME -> Creates an alias for a hostname, pointing it to another hostname.
dig example.com MX -> Specifies mail servers responsible for handling email for the domain.
dig example.com NS -> Delegates a DNS zone to a specific authoritative name server.
dig example.com TXT -> Stores arbitrary text information.
dig example.com SOA -> Contains administrative information about a DNS zone.
dig example.com ANY -> Contains any informations.
```

## Subdomains

Subdomains are essentially extensions of a primary domain name, often used to organize different sections or services within a website. For example, a company might use mail.example.com for their email server or blog.example.com for their blog.

From a reconnaissance perspective, subdomains are incredibly valuable. They can expose additional attack surfaces, reveal hidden services, and provide clues about the internal structure of a target's network. Subdomains might host development servers, staging environments, or even forgotten applications that haven't been properly secured.

### Subdomains brute-forcing

```
dnsenum example.com -f subdomains.txt
```

### Zone transfers

A zone transfer is a mechanism for replicating DNS data across servers. When a zone transfer is successful, it provides a complete copy of the DNS zone file, which contains a wealth of details about the target domain.

```
dig @ns1.example.com example.com axfr
```

### Virtual host

Virtual hosting is a technique that allows multiple websites to share a single IP address. Each website is associated with a unique hostname, which is used to direct incoming requests to the correct site. This can be a cost-effective way for organizations to host multiple websites on a single server, but it can also create a challenge for web reconnaissance.

```
gobuster vhost -u http://192.0.2.1 -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt
```

### Certificate transparency (CT) logs

Certificate Transparency (CT) logs offer a treasure trove of subdomain information for passive reconnaissance. These publicly accessible logs record SSL/TLS certificates issued for domains and their subdomains, serving as a security measure to prevent fraudulent certificates. 

For reconnaissance, they offer a window into potentially overlooked subdomains.

```
curl -s "https://crt.sh/?q=%25.example.com&output=json" | jq -r '.[].name_value' | sed 's/\*\.//g' | sort -u
```

### Web crawling

Web crawling is the automated exploration of a website's structure. A web crawler, or spider, systematically navigates through web pages by following links, mimicking a user's browsing behavior. This process maps out the site's architecture and gathers valuable information embedded within the pages.

A crucial files : 

```
robots.txt
.well-know
.git
sitemap.xml
security.txt
```

Scrapy is a powerful and efficient Python framework for large-scale web crawling and scraping projects. It provides a structured approach to defining crawling rules, extracting data, and handling various output formats.

```python
import scrapy

class ExampleSpider(scrapy.Spider):
    name = "example"
    start_urls = ['http://example.com/']

    def parse(self, response):
        for link in response.css('a::attr(href)').getall():
            if any(link.endswith(ext) for ext in self.interesting_extensions):
                yield {"file": link}
            elif not link.startswith("#") and not link.startswith("mailto:"):
                yield response.follow(link, callback=self.parse)
```

After running the Scrapy spider, you'll have a file containing scraped data (e.g., example_data.json). You can analyze these results using standard command-line tools. For instance, to extract all links:

```
jq -r '.[] | select(.file != null) | .file' example_data.json | sort -u
```

## Search Engine Discovery

Leveraging search engines for reconnaissance involves utilizing their vast indexes of web content to uncover information about your target. This passive technique, often referred to as Open Source Intelligence (OSINT) gathering, can yield valuable insights without directly interacting with the target's systems.

```
site: -> Restricts search results to a specific website.	site:example.com "password reset"
inurl: -> Searches for a specific term in the URL of a page.	inurl:admin login
filetype: -> Limits results to files of a specific type.	filetype:pdf "confidential report"
intitle: -> Searches for a term within the title of a page.	intitle:"index of" /backup
cache: -> Shows the cached version of a webpage.	
"search term" ->	Searches for the exact phrase within quotation marks.	
OR	-> Combines multiple search terms.	
-intext: -> Excludes specific terms from search results.
```

## Web Archives

Web archives are digital repositories that store snapshots of websites across time, providing a historical record of their evolution. Among these archives, the Wayback Machine is the most comprehensive and accessible resource for web reconnaissance.

```
pip install waybackpack
waybackpack http://example.com -d ~/Downloads/example-wayback (download all copies of specific URL)
```