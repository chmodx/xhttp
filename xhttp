import argparse
import asyncio
import json
from aiohttp import ClientSession
import sys

async def check_ports(domains, ports, timeout, export_format, output_file, secure, banner):
    results = {}
    async with ClientSession() as session:
        tasks = []
        for domain in domains:
            for port in ports:
                protocol = "https" if secure else "http"
                url = f"{protocol}://{domain}:{port}"
                tasks.append(asyncio.ensure_future(fetch(url, session, timeout, results, banner)))
        await asyncio.gather(*tasks)

    if export_format == "text":
        print(results)
    elif export_format == "json":
        if output_file:
            with open(output_file, 'w') as f:
                json.dump(results, f)
        else:
            print(json.dumps(results))
    else:
        print("Invalid export format, please choose 'text' or 'json'")


async def fetch(url, session, timeout, results, banner):
    try:
        async with session.get(url, timeout=timeout) as response:
            if response.status == 200:
                if banner:
                    results[f"{url}"] = {"status": "open", "banner": await response.text()}
                else:
                    results[f"{url}"] = "open"
            else:
                results[f"{url}"] = "closed"
    except:
        results[f"{url}"] = "closed"

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("-d", "--domains", nargs='*',  help="List of domains")
    parser.add_argument("-f", "--file",  help="File containing list of domains, one per line")
    parser.add_argument("-p", "--ports", nargs="+", default=[80, 443], type=int, help="Ports to check")
    parser.add_argument("-t", "--timeout", default=5, type=int, help="Timeout for each request")
    parser.add_argument("-e", "--format", default="text", choices=["text","json"], help="Format to export the results")
    parser.add_argument("-o", "--output_file", help="File to save the results")
    parser.add_argument("-s", "--secure", default=False, action='store_true', help="Check for HTTPS connections")
    parser.add_argument("-b", "--banner", default=False, action='store_true', help="Get banner from response")
    args = parser.parse_args()

    domains = []
    if args.domains:
        domains = args.domains
    elif args.file:
        with open(args.file, 'r') as f:
            domains = [line.strip() for line in sys.stdin]
    
    asyncio.run(check_ports(domains, args.ports, args.timeout, args.format, args.output_file, args.secure, args.banner))
