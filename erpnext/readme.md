##### 1.0 . 安装centos 7.6 
	更改centos源为国内源: https://www.bbsmax.com/A/q4zVxbpl5K/
	安装 nettools,vim 工具:
`	yum install net-tools vim -y
`	
##### 2.0 . 安装docker, 并且启动docker
	 安装docker: 
`	 curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
`
	 CentOS创建新用户，赋予root权限
```shell
adduser hawk
passwd hawk
chmod -v u+w /etc/sudoers
vi /etc/sudoers
# 关闭写入的权限
chmod -v u-w /etc/sudoers
```

其中 vi /etc/sudoers的更新信息为:
```yaml
## Allow root to run any commands anywhere 
root    ALL=(ALL)       ALL
hawk    ALL=(ALL)       ALL
```

##### 2.1 OS后续工作:
	1, 关闭防火墙/设置VM的快照
	2, 设置docker 的API端口2375,开放给portainer
	3, 设置用户的docker组
	sudo usermod -aG docker hawk && newgrp docker
	

##### 2.2 配置livir
1) https://phoenixnap.com/kb/install-minikube-on-centos
2) 重启 docker

##### 3.0. 启动minikube, 采用docker容器
```shell
minikube start driver=docker

# 如果需要启动dashboard:
minikube dashbard 
```

##### 4.0 . 前提是安装好 helm
https://helm.sh/zh/docs/intro/install/

#### 5.0 安装kubeapps
https://kubeapps.dev/docs/latest/tutorials/getting-started/#using-cmd
读取kubeapps token
```shell
kubectl get --namespace default secret kubeapps-operator-token -o go-template='{{.data.token | base64decode}}'
```


#### 6.0 配置portainer
https://portainer.github.io/k8s/charts/portainer/
只能通过代理转发来访问, 通过代理服务器没法访问
`kubectl port-forward --namespace portainer --address 0.0.0.0 portainer-c875b56d4-qk28b 9002:9000`
