# XHTTP
This is a simple command line utility that allows you to check if a given list of domains and ports are open or closed. It uses the aiohttp library to asynchronously check the ports, making it much faster than a traditional synchronous approach.

## Installation
To use this utility, you will need to have Python 3.5 or above and aiohttp library installed. You can install aiohttp by running the following command:

```bash
pip install aiohttp
```

## Usage
You can run the script using the following command:

```bash
python xhttp.py -d domain1.com domain2.com -p 80 443 -t 5 -e json -o results.json
```

### The script accepts the following arguments:

| Argument        | Required  | Description                                                |
| --------------- |:---------:| ----------------------------------------------------------:|
| -d, --domains	  | No        | List of domains to check.                                  |
| -f, --file	    | No        | File containing a list of domains, one per line.           |
| -p, --ports	    | No        | Ports to check. Default: 80 and 443.                       |
| -t, --timeout	  | No        | Timeout for each request. Default: 5 seconds.              |
| -e, --format	  | No        | Format to export the results. Default: "text".             |
| -o, --output		| No        | File to save the results.                                  |
| -s, --secure		| No        | Check for HTTPS connections. Default: False.               |
| -b, --banner		| No        | Get banner from response. Default: False.                  |

You can provide either the -d or -f argument, but not both.



		
## Examples
Check ports 80 and 443 on the domains example1.com and example2.com and export the results as text:

```bash
python xhttp.py -d example1.com example2.com -p 80 443
```
Check ports 80, 443, and 22 on the domains in domains.txt and export the results as json:

```bash
python xhttp.py -f domains.txt -p 80 443 22 -e json
```

Check ports 80 and 443 on the domains example1.com and example2.com and export the results as json and save the result in results.json:

```bash
python xhttp.py -d example1.com example2.com -p 80 443 -e json -o results.json
```

Check ports 80 and 443 on the domains example1.com and example2.com and export the results as json and save the result in results.json and also check for secure flag in http headers

```bash
python xhttp.py -d example1.com example2.com -p 80 443 -e json -o results.json -s 
```

## License
This utility is released under the MIT License.
