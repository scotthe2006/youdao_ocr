#/usr/bin/env python
#coding=utf8
import sys
import getopt
import urllib.request
import hashlib
import urllib
import random
import json
import base64
from PIL import Image
import datetime

appKey = 'your api key'
secretKey = 'your api secretkey'

httpClient = None
        
#按参数启动程序
def main(argv):    
    file = 'test6.jpg'
    https = False   
    try:
        opts, args = getopt.getopt(argv[1:], 'f:h:', ['file=','https='])
    except getopt.GetoptError:               
        sys.exit()
    #print(opts)
    for o, a in opts:
        if o in ('-f', '--file'):
            #print(a)                       
            file =  a
            #print(file)
        elif o in ('-h', '--https'):
            #print(a) 
            if(len(a) > 0 and int(a) > 0):   
               https = True            
            #print(https)            
        
    return file,https    
 
if __name__ == '__main__':
    curtime = datetime.datetime.now() 
    file,https = main(sys.argv)
    try:        
        #print(appKey.encode(encoding="utf-8"))
        f=open(r''+file,'rb') #二进制方式打开图文件
        bytes = f.read()
        #print(bytes)
        img=base64.b64encode(bytes) #读取文件内容，转换为base64编码
        #print(img)
        f.close()      

        detectType = '10012'
        imageType = '1'
        langType = 'en'
        salt = random.randint(1, 65536)
                
        sign = appKey.encode(encoding="utf-8")+img+ str(salt).encode(encoding="utf-8")+ secretKey.encode(encoding="utf-8")           
        m1 = hashlib.md5()
        m1.update(sign)
        sign = m1.hexdigest()    
        #print(sign)
        data = {'appKey':appKey,'img':img,'detectType':detectType,'imageType':imageType,'langType':langType,'salt':str(salt),'sign':sign}
        #print(data)
        data = urllib.parse.urlencode(data).encode(encoding='UTF8')
        #print(data)
        if(https):
           req = urllib.request.Request('https://openapi.youdao.com/ocrapi',data)
        else:        
           req = urllib.request.Request('http://openapi.youdao.com/ocrapi',data)
        
        response = urllib.request.urlopen(req)
        print(response.read().decode())
    except Exception as e:
        print(e)
    finally:
        if httpClient:
            httpClient.close()
