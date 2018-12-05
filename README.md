# Network Topology with Mininet

This repository is lab for NCTU course "Introduction to Computer Networks 2018".

---
## Abstract

In this lab, we are going to write a Python program which can generate a network topology using Mininet and use iPerf to measure the bandwidth of the topology.

---
## Objectives

1. Learn how to create a network topology with Mininet
2. Learn how to measure the bandwidth in your network topology with iPerf

---
## Execution

> TODO: 
> * Describe how to execute your program
> * Show the screenshot of using iPerf command in Mininet

進到有topology.py的src資料夾中  
`cd /root/Network_Topology/src/`  
讓要跑的檔案topology.py進入executable mode，輸入指令  
`sudo chmod +x example.py`  
進入executable mode後，執行topology.py  
`sudo ./example.py`  
如果出現錯誤訊息*Exception: Error creating interface pair (s1-eth1,s2-eth1): RTNETLINK answers: File exists*的話，就先清除先前的檔案，用指令`sudo mn -c`，在重新輸入兩行sudo指令執行topology.py  
當進入到mininet環境時，就可以用topo0.png的iPerf commands來測量建造的topology.py  
`h2 iperf -s -u -i 1 > ./out/result &`  
`h6 iperf -c 10.0.0.2 -u –i 1`  
看執行loss的結果是否在21% ~ 26%，如果沒有就先退出mininet，`exit`後，輸入`sudo mn -c`重新執行一次，有在範圍內就是成功了   
  
![topology result](https://github.com/nctucn/lab2-kuanchiehwu/blob/master/topology%20result.png)

---
## Description

### Mininet API in Python

> TODO:
> * Describe the meaning of Mininet API in Python you used in detail

* Topo：Data center network representation for structured multi-trees  
* OVSController：Open vSwitch controlle  
* TCLink：Link with symmetric TC interfaces configured via opts  
* dumpNodeConnections()：dumps connections to/from a set of nodes  
* setLogLevel( 'info' | 'debug' | 'output' )：set Mininet's default output level; 'info' is recommended as it provides useful information  
* CLI：命令行服務的文檔  
* Host, Switch, and Link (consists of the base node and link classes) which can actually be instantiated individually and used to create a network.  
* addHost(), addSwitch(), and addLink() for adding nodes and links to a network (adds the Mininet object which serves as a container for nodes and links)  
* pingAll()：通過各個節點的互相ping來檢測網絡連通性  
* self.addLink(node1, node2, bw = 10, delay = '5ms', loss = 10)：添加雙向鏈路、延遲和損失。bw表示頻寬的數值；delay表示一串計量字符，例如‘5ms'，’1s‘；loss表示丟失百分比（數值在0~100之間)  
* net：包含了最重要的類，Mininet類，為定義一個網絡的類  
* net.start()：啟動net  
* CLI(net)：等待鍵入命令  

### iPerf Commands

> TODO:
> * Describe the meaning of iPerf command you used in detail

`h2 iperf -s -u -i 1 > ./out/result &`  
`h6 iperf -c 10.0.0.2 -u –i 1`  
* -s 啟動server模式  
* -u 使用udp通訊規格  
* -i sec 報告顯示的時間間隔(以秒為單位)  
* -c 啟動client模式，命令後接server的位址  
* The above commands will dump the result of iPerf’s measurement into the file result  
  * /Network_Topology/src/out/result  

### Tasks

> TODO:
> * Describe how you finish this work step-by-step in detail

1. **Environment Setup**

   1) 加入GitHub Classroom，拿到Lab2的資料
   2) 用SSH指令進入資料夾  
      `ssh root@140.113.195.69 –p 16206`  
      (密碼為cn2018)
   3) 用clone指令登入GitHub帳號，進入資料夾中  
      裡面有一個資料Network_Topology
   4) 下載Mininet環境，並測試Mininet  
      `sudo mn`
    

2. **Example of Mininet**

   1) 先進到有example.py的src資料夾  
     `cd /root/Network_Topology/src/`  
   2) 測試example.py  
     `sudo chmod +x example.py`*(Change to the executable mode)*  
     `sudo ./example.py`  
   3) 確認example.py成功後清除連線，避免之後要再跑檔案失敗  
      或是檔案在第二步測試時失敗可輸入指令  
     `sudo mn -c`


3. **Topology Generator**

   1) 確認要用的topology  
      學號後五碼16206%3=0 --> 用topo0.png  
      ![topo0](https://github.com/nctucn/lab2-kuanchiehwu/blob/master/src/topo/topo0.png)
   2) 在src資料夾中建立topology.py  
      `touch topology.py`  
   3) 參照example.py  
   4) 在topology.py中照著topo0.png建立switch、host、link  
      * 建立switch  
        `switch = self.addSwitch('s')`  
      * 建立host  
        `host = self.assHost('h')`  
      * 建立link  
        `self.addLink(host, switch, bw, delay, loss)`  
   5) Dump every hosts’ connections  
      `from mininet.util import dumpNodeConnections`  
      \# *Dump every hosts’ and switches’ connections*  
      `dumpNodeConnections(net.hosts)`  
      `dumpNodeConnections(net.switches)`  
   6) Enter in the Mininet’s CLI mode  
      `from mininet.cli import CLI`
      * 把example.py中的 net.stop()改成  
      `CLI(net)`  
   7) 跑topology.py  
      `sudo chmod +x topology.py`*(Change to the executable mode)*  
      `sudo ./topology.py`  


4. **Measurement**  

   1) 等到topology.py跑完，會看到 mininet> 後面可以輸指令  
   2) 在 mininet> 後用topo0.png適用的指令  
      `h2 iperf -s -u -i 1 > ./out/result &`  
      `h6 iperf -c 10.0.0.2 -u –i 1`  
   3) 看packet loss有沒有在21% ~ 26%中  
   4) 有在範圍內就可以截圖  
      沒有的話就輸入`sudo mn -c`重跑一次topology.py

---
## References

> TODO: 
> * Please add your references in the following
* **Reference**
    * [Mininet實驗 命令延伸實驗擴展](https://www.cnblogs.com/qq952693358/p/5882931.html)
    * [自定義topo文檔解析](https://hk.saowen.com/a/bec15fcc5f83404bce2c3724394fa8e8e9404d8d52f049a925517f770619ff80)
    * [Mininet 網路拓墣模擬](https://ithelp.ithome.com.tw/articles/10197633)
    * [R Markdown 介紹](https://bookdown.org/tpemartin/rmarkdown_intro/markdown-syntax.html)  
    * [在GitHub的README.md加入圖片及gif的方法](https://medium.com/@stephyang/在github的readme-md加入圖片及gif的方法-7282a4a63141)
    * [利用 iperf 測試網路效能](https://cms.35g.tw/coding/利用-iperf-測試網路效能/?fbclid=IwAR3QFgxlRzWOfvmHopmvGCQ3FxGIjye0CJ1vZxY97Zc6aEL7SOO62tz4MSw)
* **Mininet**
    * [Mininet Walkthrough](http://mininet.org/walkthrough/)
    * [Introduction to Mininet](https://github.com/mininet/mininet/wiki/Introduction-to-Mininet)
    * [Mininet Python API Reference Manual](http://mininet.org/api/annotated.html)
    * [A Beginner's Guide to Mininet](https://opensourceforu.com/2017/04/beginners-guide-mininet/)
    * [GitHub/OSE-Lab - 熟悉如何使用 Mininet](https://github.com/OSE-Lab/Learning-SDN/blob/master/Mininet/README.md)
    * [菸酒生的記事本 – Mininet 筆記](https://blog.laszlo.tw/?p=81)
    * [Hwchiu Learning Note – 手把手打造仿 mininet 網路](https://hwchiu.com/setup-mininet-like-environment.html)
    * [阿寬的實驗室 – Mininet 指令介紹](https://ting-kuan.blog/2017/11/09/%E3%80%90mininet%E6%8C%87%E4%BB%A4%E4%BB%8B%E7%B4%B9%E3%80%91/)
    * [Mininet 學習指南](https://www.sdnlab.com/11495.html)
* **Python**
    * [Python 2.7.15 Standard Library](https://docs.python.org/2/library/index.html)
    * [Python Tutorial - Tutorialspoint](https://www.tutorialspoint.com/python/)
* **Others**
    * [iPerf3 User Documentation](https://iperf.fr/iperf-doc.php#3doc)
    * [Cheat Sheet of Markdown Syntax](https://www.markdownguide.org/cheat-sheet)
    * [Vim Tutorial – Tutorialspoint](https://www.tutorialspoint.com/vim/index.htm)
    * [鳥哥的 Linux 私房菜 – 第九章、vim 程式編輯器](http://linux.vbird.org/linux_basic/0310vi.php)

---
## Contributors

> TODO:
> * Please replace "YOUR_NAME" and "YOUR_GITHUB_LINK" into yours

* [吳冠潔](https://github.com/)
* [David Lu](https://github.com/yungshenglu)

---
## License

GNU GENERAL PUBLIC LICENSE Version 3
