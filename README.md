# Akile Monitor

![预览图](https://github.com/akile-network/akile_monitor/blob/main/akile_monitor.jpg?raw=true)
前端项目地址 https://github.com/akile-network/akile_monitor_fe

## 前后端集合一键脚本

```
wget -O ak-setup.sh "https://raw.githubusercontent.com/KunBuFenZi/ak-monitor/refs/heads/main/ak-setup.sh" && chmod +x ak-setup.sh && sudo ./ak-setup.sh
```
![image](https://github.com/user-attachments/assets/43379572-4f24-4e7e-9972-f5452c322640)


## 主控后端

```
wget -O setup-monitor.sh "https://raw.githubusercontent.com/KunBuFenZi/ak-monitor/refs/heads/main/setup-monitor.sh" && chmod +x setup-monitor.sh && sudo ./setup-monitor.sh
```

## 被控端

```
wget -O setup-client.sh "https://raw.githubusercontent.com/KunBuFenZi/ak-monitor/refs/heads/main/setup-client.sh" && chmod +x setup-client.sh && sudo ./setup-client.sh <your_secret> <url> <name>
```
如
```
wget -O setup-client.sh "https://raw.githubusercontent.com/KunBuFenZi/ak-monitor/refs/heads/main/setup-client.sh" && chmod +x setup-client.sh && sudo ./setup-client.sh 123321 wss://123.321.123.321/monitor HKLite-One-Akile
```

## 主控前端部署教程(cf pages)

### 1.下载
···
https://github.com/akile-network/akile_monitor_fe/releases/download/v0.0.1/akile_monitor_fe.zip
···

### 2.修改config.json为自己的api地址（公网地址）（如果前端要加ssl 后端也要加ssl 且此处记得改为https和wss）

```
{
  "socket": "ws(s)://192.168.31.64:3000/ws",
  "apiURL": "http(s)://192.168.31.64:3000"
}
```

### 3.直接上传文件夹至cf pages

![image](https://github.com/user-attachments/assets/c9e5a950-045a-4a7f-8b30-00899994c8cf)
![image](https://github.com/user-attachments/assets/c4096133-694d-4c2a-8d90-f92e48de6e9b)

### 4.设置域名（可选）

![image](https://github.com/user-attachments/assets/14adc0cf-2292-4148-a913-7a466e441d71)


## 手动部署

前端项目地址 https://github.com/akile-network/akile_monitor_fe


后端主控部署教程：
```
mkdir /etc/ak_monitor/
cd /etc/ak_monitor/
wget -O ak_monitor https://github.com/akile-network/akile_monitor/releases/download/v0.01/akile_monitor-linux-amd64
chmod 777 ak_monitor

cat > /etc/systemd/system/ak_monitor.service <<EOF
[Unit]
Description=AkileCloud Monitor Service
After=network.target nss-lookup.target
Wants=network.target

[Service]
User=root
Group=root
Type=simple
LimitAS=infinity
LimitRSS=infinity
LimitCORE=infinity
LimitNOFILE=999999999
WorkingDirectory=/etc/ak_monitor/
ExecStart=/etc/ak_monitor/ak_monitor
Restart=on-failure
RestartSec=10

[Install]
WantedBy=multi-user.target

EOF
```


复制配置文件到/etc/ak_monitor/config.json并配置文件

```
启动
systemctl start ak_monitor
关闭
systemctl stop ak_monitor
开机自启
systemctl enable ak_monitor
```


监控端安装教程
```
mkdir /etc/ak_monitor/
cd /etc/ak_monitor/
wget -O client https://github.com/akile-network/akile_monitor/releases/download/v0.01/akile_client-linux-amd64
chmod 777 client

cat > /etc/systemd/system/ak_client.service <<EOF
[Unit]
Description=AkileCloud Monitor Service
After=network.target nss-lookup.target
Wants=network.target

[Service]
User=root
Group=root
Type=simple
LimitAS=infinity
LimitRSS=infinity
LimitCORE=infinity
LimitNOFILE=999999999
WorkingDirectory=/etc/ak_monitor/
ExecStart=/etc/ak_monitor/client
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target

EOF
```
复制配置文件到/etc/ak_monitor/client.json并配置文件

```
启动
systemctl start ak_client
关闭
systemctl stop ak_client
开机自启
systemctl enable ak_client
```
