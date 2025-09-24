# Objective
Your host at 10.0.0.1 is receiving traffic on port 31337; block that traffic.

# Solution
We have to block incomming data from any ip on port 31337.
```bash
iptables -A INPUT -p tcp --dport 31337 -j REJECT
```
After blocking the desired ip it shouldpaste the flag into your terminal.
