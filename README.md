# Zeek Cheat Sheet

A quick reference for using **Zeek** to analyze `.pcap` files and extract key data.  
Includes common commands, `zeek-cut` usage, and pipeline examples with UNIX utilities.

---

**# 1. Running Zeek on PCAPs**

    zeek -r file.pcap

    Generates multiple logs (e.g. conn.log, dns.log, http.log, ssl.log, weird.log).

**# 2. Inspecting Logs**

    Use zeek-cut to extract specific fields from logs.

    Example: show all fields → cat conn.log | zeek-cut -d

**#3. Common Log Files**

    conn.log → connection metadata (IPs, ports, services, duration)

    dns.log → DNS queries and responses

    http.log → HTTP requests, URIs, user agents

    ssl.log → SSL/TLS handshake info, certificates

    files.log → file transfers observed

    weird.log → unusual or unexpected activity

**#4. Useful Commands**

    Extract Source and Destination IPs: cat conn.log | zeek-cut id.orig_h id.resp_h

    Count Unique IPs: cat conn.log | zeek-cut id.orig_h | sort | uniq -c | sort -nr

    Show DNS Queries: cat dns.log | zeek-cut query qtype_name

    Count DNS Requests by Domain: cat dns.log | zeek-cut query | sort | uniq -c | sort -nr

    Find Malicious Executable Downloads: cat http.log | zeek-cut id.orig_h host uri | grep ".exe"

    Extract SSL Certificate Subjects: cat ssl.log | zeek-cut id.resp_h subject issuer

**#5. Combining with UNIX Tools**

    sort → order output

    uniq -c → count unique values

    grep → filter patterns

    head → display top results

    Example: Top 10 requested domains: cat dns.log | zeek-cut query | sort | uniq -c | sort -nr | head -10

**#6. Workflow Tips**

    Always start by running zeek -r file.pcap to parse the traffic.

    Focus on conn.log, dns.log, http.log, ssl.log as primary sources.

    Use zeek-cut with pipelines to quickly narrow down IoCs.

    Cross-check suspicious indicators (IPs, domains, URIs) against threat intel feeds.

**References**

Zeek Documentation

TryHackMe: Zeek Room

Summary:

Zeek is a powerful tool for SOC analysts and incident responders.
Mastering zeek-cut and combining it with UNIX commands allows rapid extraction of IoCs and detection of suspicious network activity.

**Made by: Xavier Mota**
**19/08/2025**

Feel free to use!
