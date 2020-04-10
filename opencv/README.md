# 项目说明  
在树莓派4B上安装docker，在容器中部署opencv3.4.1，不用再反复安装opencv环境而碰到各种问题烦扰。  

***

## 1、树莓派初始准备  
+ 树莓派第一次启动开启ssh服务  
   `sudo systemctl start ssh.service`  
   `sudo systemctl enable ssh`  
+ 更换国内apt源  
   `vim /etc/apt/sources.list`  
   将原来的内容用#注释，将下面的内容粘贴在最后:  
    `deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main non-free contrib`  
    `deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main non-free contrib`  
   `vim /etc/apt/sources.list.d/raspi.list`  
   将原来的内容用#注释，将下面的内容粘贴在最后  
    `deb http://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ buster main ui`  
   `sudo apt-get update`  
 
 ***
 
## 2、在树莓派上部署docker环境  
 + 安装docker  
   `sudo curl -sSL https://get.docker.com | sh`  
   `sudo usermod pi -aG docker`  
   `echo Adding " cgroup_enable=cpuset cgroup_enable=memory" to /boot/cmdline.txt`  
   `sudo cp /boot/cmdline.txt /boot/cmdline_backup.txt`  
 + 设置 Docker 开机启动  
   `sudo systemctl enable docker`  
 + 安装docker图形化界面portainer  
   `sudo docker pull portainer/portainer`  
   `sudo docker volume create portainer_data`  
   `sudo docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer`
  
***

## 3、opencv  
+ 使用的opencv-3.4.1版本，在opencv-3.4.1/modules/python/src2/cv2.cpp代码中的889行char*前添加const  
+ 在Dockerfile目录下执行  
  `docker build -t opencv:v1 .`  
  生成镜像
  
