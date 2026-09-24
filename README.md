# day6-networking
Q1. A user reports that a website is not opening. Write the command you would use to verify whether the server is reachable over the network.
 * Command:
   ping google.com

 * Explanation: The ping command sends ICMP Echo Request packets to verify basic network-level reachability and determine if the destination host is responding.
Q3. Ping to a website fails. Which command would you use to identify the router (hop) where the communication is failing?
 * Command:
   traceroute google.com

   (On Windows: tracert google.com)
 * Explanation: traceroute diagnoses the exact intermediary router or gateway where network packets are being dropped or blocked along the path.
Q5. A server is reachable, but users report that the website is still not loading. Which command would you use to verify whether the web server is responding to HTTP requests?
 * Command:
   curl -I https://google.com

 * Explanation: curl -I fetches HTTP response headers to verify if the web service process is active and returning valid status codes (such as 200 OK or 301 Moved).
Q6. Write a command to display only HTTP response headers from google.com.
 * Command:
   curl -I https://google.com

   (Alternative: curl --head [https://google.com](https://google.com))
Q9. Explain the purpose of the options -t, -u, -l and -n.
 * -t : Display only TCP sockets.
 * -u : Display only UDP sockets.
 * -l : Display only Listening sockets (services awaiting incoming connections).
 * -n : Show addresses and ports in Numeric format instead of resolving hostnames and service names.
Q11. Display all listening ports using the modern Linux networking tool that replaces netstat.
 * Command:
   ss -tuln

 * Explanation: ss (Socket Statistics) is the high-performance modern replacement for the legacy netstat utility.
Q12. Compare netstat and ss. Provide any three differences.
| Feature | netstat | ss (Socket Statistics) |
|---|---|---|
| Performance | Slower; parses text files sequentially from the /proc filesystem. | Faster; retrieves socket details directly from kernel space via Netlink. |
| Status | Deprecated in modern Linux distributions. | Standard active networking tool in modern distributions. |
| Package | Shipped inside the legacy net-tools package. | Bundled natively within the modern iproute2 package. |
Q14. Find the IP address associated with google.com using a DNS lookup command.
 * Command:
   nslookup google.com

   (Alternative: dig google.com +short)
Q16. Differentiate between nslookup and dig. Provide at least three differences.
| Feature | nslookup | dig (Domain Information Groper) |
|---|---|---|
| Output Detail | Displays basic name-to-IP mappings. | Provides comprehensive DNS output including TTL, query time, flags, and authority sections. |
| Formatting | Formatted in plain conversational text. | Formatted in technical, standard BIND zone format. |
| Use Case | Quick manual lookups and interactive queries. | System administration, deep debugging, and shell script automation (+short). |
Q17. A company website is reported as down. Perform the complete troubleshooting workflow and list the commands in the correct order.
 * Step 1 (DNS Verification): Verify domain name resolution.
   nslookup example.com

 * Step 2 (Network Reachability): Check basic server connectivity.
   ping example.com

 * Step 3 (Path Diagnostics): Identify failing network hops if ping fails.
   traceroute example.com

 * Step 4 (HTTP Service Validation): Inspect web server response headers.
   curl -I https://example.com

 * Step 5 (Internal Host Checks): Log in via SSH to inspect listening ports and firewall rules.
   ss -tuln
sudo ufw status

Q18. Explain the purpose of ping, traceroute, curl, netstat, ss, nslookup and dig.
 * ping: Checks network connectivity and measures latency to a destination host using ICMP.
 * traceroute: Traces the network path and isolates intermediate router failures.
 * curl: Tests HTTP/HTTPS/FTP connectivity and retrieves web content or response headers.
 * netstat: Legacy utility used to view active connections, routing tables, and open ports.
 * ss: Modern utility used to inspect network sockets and listening ports quickly.
 * nslookup: Basic tool used to query DNS servers for domain name resolution.
 * dig: Advanced DNS exploration tool used to inspect zone records, TTL values, and query times.
Q19. Scenario-Based Troubleshooting: 'google.com is not opening from my machine.' Describe step-by-step troubleshooting using networking tools.
 * Verify DNS Resolution: Run nslookup google.com to confirm your local DNS resolver returns valid IP addresses.
 * Verify Local Gateway: Run ping 192.168.1.1 (or your local router IP) to ensure the local Wi-Fi/LAN link is functional.
 * Verify Internet Reachability: Run ping 8.8.8.8 to rule out DNS failures and verify external Internet routing.
 * Trace Path Failures: Run traceroute google.com to locate the exact upstream hop where packets drop.
 * Verify HTTP/Web Layer: Run curl -I [https://google.com](https://google.com) to determine whether the issue is caused by the web service or a local browser configuration.
Bonus Challenge: The website is reachable through ping, DNS is resolving correctly, but users receive a 500 Internal Server Error. Which networking command confirms the HTTP response status, and what would be your next troubleshooting step?
 * Command:
   curl -I https://example.com

   (Confirms the error by displaying the HTTP/1.1 500 Internal Server Error header).
 * Next Troubleshooting Step:
   * An HTTP 500 status indicates a backend application crash or server-side misconfiguration rather than a network problem.
   * Connect to the server via SSH and inspect web server and application error logs:
     sudo tail -n 100 /var/log/nginx/error.log
# Or inspect application container logs:
docker logs <container_name>

