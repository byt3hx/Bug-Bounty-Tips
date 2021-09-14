# Bug-Bounty-Tips
I will share my bug bounty tips here

## Nmap
Want to scan port super fast?
This the command that I normally use. Trust me it is the fastest.
```
nmap -sS -T4 -A -p- -iL live_subdomains.txt --min-rate 1000 --max-retries 3
```

## How to run ffuf with multiple urls?

Do you wanna run ffuf with mutliple urls?
Here is what you are looking for.

Note:I saw that rush command from twitter and it works really well but I forget the source link:")


Before we run the ffuf with rush make a directory because you gonna output all the domain from your file.

```
mkdir dir
```
First we will use rush which is the tool that can run parallel. We then use unfurl to extract all the url in domain to output as domain. Finally , we run ffuf.
```
rush -i urls.txt -j 10 'ffuf -o $(echo {} | unfurl domains) -w wordlist.txt -u "{}/FUZZ"'
```
After running above command , you will get a bunch of result with json format. By running the following command, you will get nice list of url with directories , length and status code.
```
cat * | jq -r '.results[] | "\(.length)"+ " " +"\(.url)" + " " +  "\(.status)"' | sort -unt " " -k "1,1"
```

https://github.com/shenwei356/rush
https://github.com/tomnomnom/unfurl


I also  wanna demonstrate the script that I write in python which does same thing to above.
Just copy the following python script and save it to a file.
As above step make a directory and it will print the final result. 
```
python3 ffufer.py urls.txt words.txt 
```

```
import sys
import os
from urllib.parse import urlparse

file = sys.argv[1]
wordlist = sys.argv[2]

with open(file) as file_in:
    lines = []
    for line in file_in:
        lines.append(line.strip('\n'))
    for url in lines:
    	output = urlparse(url).netloc
    	cmd = os.system("ffuf -u {}/FUZZ -w {} -o {} -s -D -ac -H User-Agent: Mediapartners-Google".format(url,wordlist,output))
    	
out = os.system('cat *  | jq -r '"'"'.results[] |   "\(.length)"+ " " +"\(.url)" + " " +  "\(.status)"  '"'"' >> result.txt')
result = open("result.txt", "r")
print(result.read())
```
