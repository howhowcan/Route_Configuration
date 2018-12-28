# Route Configuration

This repository is a lab for NCTU course "Introduction to Computer Networks 2018".

---
## Abstract

In this lab, we are going to write a Python program with Ryu SDN framework to build a simple software-defined network and compare the different between two forwarding rules.

---
## Objectives

1. Learn how to build a simple software-defined networking with Ryu SDN framework
2. Learn how to add forwarding rule into each OpenFlow switch

---
## Execution


1. Connect to a container (For convenience, call it terminal A)
2. Clone my repo by `git clone https://github.com/nctucn/lab3-howhowcan.git Route_Configuration`
3. Start the service of Open vSwitch by `sudo service openvswitch-switch start`
4. Change the directory: `cd ./Route_Configuration/src/`
5. Run `topo.py`: `sudo mn --custom topo.py --topo topo --link tc --controller remote`
   `sudo mn`: use the highest authorization to execute mininet   
     `--custom topo.py`: use `topo.py` for the topology of the internet   
     `--topo topo`: Open the custom topology named `topo` in the .py file   
     `--link tc`: link with symmetric TC interfaces   
     `--controller remote`: use remote controller   
6. If succeed, you will go into CLI mode
7. Set up another connection to the same container (Call it terminal B) and change the directory:   
`cd ./Route_Configuration/src/`
8. Run `SimpleController.py`: `sudo ryu-manager SimpleController.py --observe-links`
    * `sudo ryu-manager`: use the highest authorization to execute ryu-manager  
    `SimpleController.py`: use `SimpleController.py` to control the topology   
    `--observe-links`: observe links and discover events
9. Go to terminal A and input the following iperf commands:
    * `mininet> h1 iperf -s -u -i 1 -p 5566 > ./out/result1 &`    
    `h1`: host 1    
   `iperf`: using iPerf to measure the max achievable bandwidth    
   `-s`: let h1 play the role of server in iPerf    
   `-u`: using UDP connections    
   `-i 1`: set interval to 1s    
   `-p 5566`: set the port to 5566    
   `> ./out/result1 &`: make a file ./out/result1 to save the result of this test 
    * `mininet> h2 iperf -c 10.0.0.1 -u –i 1 -p 5566`
   `h2`: host 2    
   `iperf`: same as above    
   `-c`: let h2 play the role of client in iPerf    
   `10.0.0.1`: connect to host 1     
   `-u`: same as above    
   `-i 1`: same as above     
   `-p 5566`: same as above
10. Use `mininet> exit` to leave CLI mode
11. Go to terminal B and use `ctrl+z` to leave the controller
12. Use `sudo mn -c` to clean the data
13. Go to terminal A and run `topo.py` again
14. Go to terminal B and run `controller.py`: 
`sudo ryu-manager controller.py --observe-links`
15. Go to terminal A and input the following iperf commands:   
`mininet> h1 iperf -s -u -i 1 -p 5566 > ./out/result2 &`   
`mininet> h2 iperf -c 10.0.0.1 -u –i 1 -p 5566`
16. Compare the difference of two results
* Use `SimpleController.py`:
![](https://i.imgur.com/HDWwduI.jpg)
* Use `controller.py`:
![](https://i.imgur.com/LBfzIYl.jpg)


---
## Description

### Tasks

1. Environment Setup    
1.1 Click the link in the slide to join the lab on GitHub Classroom    
    1.2 Open Pietty and connect to the container with IP `140.113.195.69` and Port `612213` (call it terminal A)    
    1.3 Login as `root`, and the password is `cn2018`    
    1.4 Use the command `passwd` to change the password to `z`    
    1.5 Clone my repo by `git clone https://github.com/nctucn/lab3-howhowcan.git Route_Configuration`    
    1.6 Initiate the git infos by the command  
    `git config --global user.name "howhowcan"`      
    `git config --global user.email gktt58@gmail.com`    
    1.7 Run mininet for testing :`sudo mn`    
    1.8 The error mentioned in the slide occurred, so use the command `sudo service openvswitch-switch start`    
        to solve the problem    
    1.9 Run mininet again and check if miniet is work correctly this time  
    1.10 Clean up all the connections: `sudo mn -c`
2. Example of Ryu SDN    
    2.1 After Task 1 was done, go to terminal A and change the directory: 
    `cd ./Route_Configuration/src/`    
    2.2 Run `SimpleTopo.py`: `sudo mn --custom SimpleTopo.py --topo topo --link tc --controller remote`     
    2.3 Open another terminal connecting to the same container (call it terminal B)     
    2.4 Change the directory: `cd ./Route_Configuration/src/`     
    2.5 Run `SimpleController.py`: `sudo ryu-manager SimpleController.py --observe-links`     
    2.6 Go to terminal A and use `mininet> exit` to leave CLI mode     
    2.7 Go to terminal B and use `ctrl+z` to leave the controller     
    2.8 Use `sudo mn -c` to clean the data
3. Mininet Topology     
    3.1 Go to terminal A and enter the command `cp SimpleTopo.py topo.py` to duplicate the example code `SimpleTopo.py` and name it `topo.py`     
    3.2 Use `vim topo.py` to modify `topo.py` by adding some constraints and save the change     
    3.3 Run `topo.py`: `sudo mn --custom topo.py --topo topo --link tc --controller remote`     
    3.4 Go to terminal B and run `SimpleController.py`: `sudo ryu-manager SimpleController.py --observe-links`    
    3.5 Go to terminal A and use `mininet> exit` to leave CLI mode    
    3.6 Go to terminal B and use `ctrl+z` to leave the controller    
    3.7 Use `sudo mn -c` to clean the data
4. Ryu Controller    
    4.1 Go to terminal B and trace the code `SimpleController.py`: `vim SimpleController.py`     
    4.2 After figuring out the meaning of the code, duplicate the example code `SimpleController.py` and name it `controller.py`: 
`cp SimpleController.py controller.py`     
    4.3 Modify `controller.py` according to the fowarding rules in the slide:
`vim controller.py`     
5. Measurement     
    5.1 Go to terminal A and run `topo.py`:
`sudo mn --custom topo.py --topo topo --link tc --controller remote`     
    5.2 Go to terminal B and run `SimpleController.py`:
`sudo ryu-manager SimpleController.py --observe-links`    
    5.3 Go to terminal A and input the iperf commands:
`mininet> h1 iperf -s -u -i 1 –p 5566 > ./out/result1 &`   
`mininet> h2 iperf -c 10.0.0.1 -u –i 1 –p 5566`   
    5.4 Use `mininet> exit` to leave CLI mode   
    5.5 Go to terminal B and use `ctrl+z` to leave the controller   
    5.6 Use `sudo mn -c` to clean the data   
    5.7 Go to terminal A and run `topo.py`:
`sudo mn --custom topo.py --topo topo --link tc --controller remote`   
    5.8 Go to terminal B and run `controller.py`:
`sudo ryu-manager controller.py --observe-links`   
    5.9 Go to terminal A and input the iperf commands:
`mininet> h1 iperf -s -u -i 1 –p 5566 > ./out/result2 &`
`mininet> h2 iperf -c 10.0.0.1 -u –i 1 –p 5566`   
    5.10 Use `mininet> exit` to leave CLI mode   
    5.11 Go to terminal B and use `ctrl+z` to leave the controller   
    5.12 Use `sudo mn -c` to clean the data   
    
### Discussion


1. Describe the difference between packet-in and packet-out in detail.   
   When the switch receive a packet, then it will trigger packet-in and foward it to the controller.   
   After the controller get the information of the packet, it will check the flow table to find out which port the packet should be sent to .(If the flow table has no corresponding info, then do the flooding.)    
2. What is “table-miss” in SDN?   
   If a packet does not match a flow entry in a flow table, it is a table miss.   
3. Why is "`(app_manager.RyuApp)`" adding after the declaration of class in `controller.py`?   
   To inherit the father class `app_manager` for the beginning of the initiaion.   
4. Explain the following code in `controller.py`.
    ```python
    @set_ev_cls(ofp_event.EventOFPPacketIn, MAIN_DISPATCHER)
    ```
   `@set_ev_cls` is a decorator to check which event and state the subprogram stands for   
   `ofp_event.EventOFPPacketIn`: Handling the packet-in event   
   `MAIN_DISPATCHER`: Switch-features message received and sent set-config messages   
5. What is the meaning of “datapath” in `controller.py`?
   The switch in the topology using OpenFlow.   
6. Why need to set "`ip_proto=17`" in the flow entry?
   Because we use UDP connection in this test.   
7. Compare the differences between the iPerf results of `SimpleController.py` and `controller.py` in detail.   
   The loss rate of `SimpleController.py` is 1.7%, whereas the one of `controller.py` is 2.2%.   
8. Which forwarding rule is better? Why?   
   The one use `SimpleController.py`, because the loss rate is lower.   
---
## References

* **Ryu SDN**
    * [Ryubook Documentation](https://osrg.github.io/ryu-book/en/html/)
    * [Ryubook [PDF]](https://osrg.github.io/ryu-book/en/Ryubook.pdf)
    * [Ryu 4.30 Documentation](https://github.com/mininet/mininet/wiki/Introduction-to-Mininet)
    * [Ryu Controller Tutorial](http://sdnhub.org/tutorials/ryu/)
    * [OpenFlow 1.3 Switch Specification](https://www.opennetworking.org/wp-content/uploads/2014/10/openflow-spec-v1.3.0.pdf)
    * [Ryubook 說明文件](https://osrg.github.io/ryu-book/zh_tw/html/)
    * [GitHub - Ryu Controller 教學專案](https://github.com/OSE-Lab/Learning-SDN/blob/master/Controller/Ryu/README.md)
    * [Ryu SDN 指南 – Pengfei Ni](https://feisky.gitbooks.io/sdn/sdn/ryu.html)
    * [OpenFlow 通訊協定](https://osrg.github.io/ryu-book/zh_tw/html/openflow_protocol.html)
    * [深入OpenFlow協定 詳解Flow Table比對機制 - 技術專欄 - 網管人NetAdmin](https://www.netadmin.com.tw/article_content.aspx?sn=1610070003)
    * [深入OpenFlow協定精髓 看懂Pipeline處理流程 - 技術專欄 - 網管人NetAdmin](https://www.netadmin.com.tw/article_content.aspx?sn=1608090003)
* **Python**
    * [Python 2.7.15 Standard Library](https://docs.python.org/2/library/index.html)
    * [Python Tutorial - Tutorialspoint](https://www.tutorialspoint.com/python/)
* **Others**
    * [Cheat Sheet of Markdown Syntax](https://www.markdownguide.org/cheat-sheet)
    * [Vim Tutorial – Tutorialspoint](https://www.tutorialspoint.com/vim/index.htm)
    * [鳥哥的 Linux 私房菜 – 第九章、vim 程式編輯器](http://linux.vbird.org/linux_basic/0310vi.php)

---
## Contributors


* [Jay Tseng](https://github.com/howhowcan)
* [David Lu](https://github.com/yungshenglu)

---
## License

GNU GENERAL PUBLIC LICENSE Version 3
