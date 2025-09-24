# Objective
From your host at 10.0.0.1, connect to the remote host at 10.0.0.2 on port 31337. This time, we have blocked outbound traffic to port 31337, so you'll have to allow it first.

# Solution
We have to access the host 10.0.0.2 over port 31337, when we try and wait for 15 seconds this we get the following:
```bash
root@ip-10-0-0-1:~# nc -w 15 10.0.0.2 31337
root@ip-10-0-0-1:~#
```
the `-w` flag let's us set the timeout for the connection, the manpage of netcat reads the following:
```bash
        -w timeout
               Connections which cannot be established or are idle timeout  af‐
               ter  timeout  seconds.   The -w flag has no effect on the -l op‐
               tion, i.e. nc will listen forever  for  a  connection,  with  or
               without the -w flag.  The default is no timeout.
```
So now that we know and made sure we are unable to access 10.0.0.2 over port 31337...
We first check the current iptables configuration:
```bash
root@ip-10-0-0-1:~# iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination         
DROP       tcp  --  anywhere             anywhere             tcp dpt:31337
```
showing us that all data output via port 31337 is blocked for any ip. 
Now we check which line this rule is identified by,
```bash
root@ip-10-0-0-1:~# iptables -L OUTPUT --line-numbers
Chain OUTPUT (policy ACCEPT)
num  target     prot opt source               destination
1    DROP       tcp  --  anywhere             anywhere             tcp dpt:31337
```
then we delete the rule identified by num 1 and verify our changes:
```bash
root@ip-10-0-0-1:~# iptables -D OUTPUT 1
root@ip-10-0-0-1:~# iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```
We once again try to connect to host 10.0.0.2 over port 31337 and...
```bash
root@ip-10-0-0-1:~# nc -w 15 10.0.0.2 31337
```
and we get the flag in return from host 10.0.0.2.
