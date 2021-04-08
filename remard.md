#读取pdf根据内容ip修改文件名
例子：
xxx.pdf提取内容改后127.0.0.1

import os
import pdfplumber
import time
import re

#当前目录下
path="./"
count=0
for directory, dirnames, filenames in os.walk(path):
    for filename in filenames:
        #如果不是pdf为后缀的，继续往下读，如果是的话，就打开读取改名
        if not  filename.endswith(".pdf"):
          
            continue
        count+=1
        origin_path=os.path.join(path,filename)

        #读取为pdf后缀的一个文件
        with pdfplumber.open(origin_path) as pdf:
            for page in pdf.pages:
                # page=pdf.pages[1]
                # text=page.extract_text()
                # print(text)

                print(page.extract_text())
        #test=re.search("IP地址..........................................................................",page.extract_text())
        #print(test)
        time.sleep(3)
        #ip=re.search('((?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?\.){3}(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)))',page.extract_text())

        ipaddress=re.compile(r'(([01]{0,1}\d{0,1}\d|2[0-4]\d|25[0-5])\.){3}([01]{0,1}\d{0,1}\d|2[0-4]\d|25[0-5])')
        #用正则匹配ip规则

        ip=ipaddress.search(page.extract_text())
        #利用规则去寻找ip
        print(ip)

        print(ip.group(0))
        #显示返回的匹配对象

        ip2=ip.group(0)
        filename=filename.split(".")
        prefix=filename[0]
        postfix=filename[1]
        newname=ip2+"."+postfix
        rename_path=os.path.join(path,newname)
        os.rename(origin_path,rename_path)
        print(filename,"重命名为",rename_path)
