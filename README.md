

解除文件限制  

`echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p`  

`sudo sysctl --system`
