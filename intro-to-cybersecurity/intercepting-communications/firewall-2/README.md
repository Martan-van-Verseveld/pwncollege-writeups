# Objective
Your host at 10.0.0.1 is receiving traffic on port 31337; block that traffic, but only from the remote host at 10.0.0.3, you must allow traffic from the remote host at 10.0.0.2.

# Solution
The solution is to block the incomming traffic on port 31337 from host 10.0.0.3. I will use iptables.
```bash
iptables -A INPUT -p tcp --dport 31337 -s 10.0.0.3 -j REJECT
```
After blocking the data comming in from the desired port originating from ip 10.0.0.3, it will paste the flag into your terminal.
