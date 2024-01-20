# proxychains4:通过命令行进行代理的工具  
- 有时候在执行命令行的一些命令时，会因为网络问题无法执行，即使是开了`clash`中的`TUN mode`也行不通就会用到此工具
- 打开终端执行`sudo apt update`  
- `sudo apt install proxychains4`
- 安装好后打开配置文件`sudo gedit /etc/proxychains4.conf `，拉到最下方
- 加入一行`socks5 ip port`，其中IP为你本机的ip地址，使用`ifconfig`命令查看，如下图`inet`则为本机ip，port为clash的general界面中的端口号
![ip](/pics/ip.png)  
![port](/pics/port.png)  
- 示例为下图所示  
![proxy](/pics/proxychains4.png)  
- 设置好后保存退出，如果在使用命令行的过程中只要遇到网络问题，不管是升级还是安装软件包，都可以在命令行的最前面加入`proxychains4`就可以使用代理  
- 例图如下  
![example](/pics/example.png)  
### 可使用的场景通常有update upgrade rosdep pip install，其它遇到网络问题的都可以采用该工具解决