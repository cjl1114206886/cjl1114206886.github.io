## Wireshark

```C++
1.下载tcpdump
sudo apt update
sudo apt install tcpdump
export PATH=$PATH:/usr/sbin
2.查看某个进程使用的端口：
sudo netstat -tulnp | grep [进程ID]
3.设置监听端口参数并生成.pcap:-G是监听时间
./tcpdump -i any port 44718 or port 44719 -G 300 -w /home/nvidia/cjl/output.pcap
4.导入wireshark
5.wireshark:ctrl+A选中全部包->statistics：IO Graph->Y Axis设置为Bytes->查看峰值：点击峰值/1024/1024得到MB
```