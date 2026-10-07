# Web Fuzzing

## What ?

Web fuzzing is a technique used to discover vulnerabilities, hidden resources, and security issues in web applications by automatically injecting a large set of input data into the application and analyzing its response. The goal is to identify unexpected behaviors or errors that could indicate potential security weaknesses or misconfigurations.

Fuzzing is commonly employed in security testing to find:

    Hidden directories and files
    Insecure APIs and endpoints
    SQL injection points
    Cross-site scripting (XSS) vulnerabilities
    Command injection flaws

## Brute-forcing vs Fuzzing

| Criteria    | Bruteforcing                                                                             | Fuzzing                                                                                               |   |   |
|-------------|------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|---|---|
| Definition  | Systematically trying all possible combinations of input data to guess a specific value. | Injecting unexpected or random data into an application to find vulnerabilities and hidden resources. |   |   |
| Purpose     | Crack passwords, keys, or other access credentials.                                      | Discover application vulnerabilities, hidden files, directories, and input validation issues.         |   |   |
| Methodology | Exhaustive search over all possible input combinations.                                  | Dynamic input injection to provoke unexpected application responses.                                  |   |   |
| Focus | Specific input or data, such as passwords or API keys.                                  | General application behavior under various input conditions.                                  |   |   |
| Efficiency | Time-consuming due to exhaustive nature; less efficient for large input spaces.                                  | More efficient in identifying unexpected behaviors and vulnerabilities with varied input.                                  |   |   |
| Tools |  Used	Password crackers, key recovery tools.                                  | Web fuzzers, vulnerability scanners.                                  |   |   |
| Output |  Successful match of the correct input value.                                  |Discovery of vulnerabilities, misconfigurations, and hidden resources.                                  |   |   |

## Miscellaneous Commands

Below are some useful commands that can aid in various tasks related to web fuzzing and testing.

|  Command | Description  |     |   |   |
|-------------|------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|---|---|
| sudo sh -c 'echo "SERVER_IP academy.htb" >> /etc/hosts'  | Add a DNS entry for a specific IP address to the /etc/hosts file. This helps resolve domain names locally. |  |   |   |
|  for i in $(seq 1 1000); do echo $i >> ids.txt; done | Create a sequence wordlist from 1 to 1000. Useful for brute-forcing numerical IDs or similar patterns. |  |   |   |
| curl http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=key' -H 'Content-Type: application/x-www-form-urlencoded'  | Use curl to send a POST request with specific data and headers, simulating form submissions or API calls. |  |   |   |
|   |  |  |   |   |
