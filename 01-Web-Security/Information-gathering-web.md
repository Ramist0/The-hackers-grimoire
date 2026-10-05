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

### Certificate Transparency (CT) Logs

Certificate Transparency (CT) logs offer a treasure trove of subdomain information for passive reconnaissance. These publicly accessible logs record SSL/TLS certificates issued for domains and their subdomains, serving as a security measure to prevent fraudulent certificates. 

For reconnaissance, they offer a window into potentially overlooked subdomains.

```
curl -s "https://crt.sh/?q=%25.example.com&output=json" | jq -r '.[].name_value' | sed 's/\*\.//g' | sort -u
```