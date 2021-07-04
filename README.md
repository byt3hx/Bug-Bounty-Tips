# Bug-Bounty-Tips
I will share my bug bounty tips here

## Nmap
Want to scan port super fast?
This the command that I normally use. Trust me it is the fastest.
```
nmap -sS -T4 -A -p- -iL live_subdomains.txt --min-rate 1000 --max-retries 3
```
