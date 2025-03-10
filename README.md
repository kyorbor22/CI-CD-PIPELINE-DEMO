# CI-CD-PIPELINE-DEMO

This is an example ready-to-deploy java web application built for Tomcat using Maven and webapp-runner.
1. download java-11
#yum install java-11*

#cd /usr/lib/jvm

#ls -ltr
 
select this java path

******/usr/lib/jvm/java-11-openjdk****

#cd ~

# vim .bash_profile
copy and paste the script below!!

==================================================

# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs
JAVA_HOME=/usr/lib/jvm/java-11-openjdk
PATH=$PATH:$HOME/bin:$JAVA_HOME

export PATH
===================================================

#echo $JAVA_HOME

#source .bash_profile

#echo $JAVA_HOME

********************Now download and configure jenkins***************************
Before installing jenkis do the following:

#Jenkins Repo and key download command:

sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

*****adding yum install & start commands ******

sudo yum install jenkins

service jenkins status

service jenkins start

#Maven download command:

wget https://mirrors.gigenet.com/apache/maven/maven-3/3.8.1/binaries/apache-maven-3.8.1-bin.tar.gz

#Sonarqube Download Command:

wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-6.7.7.zip

#Sonarqube scanner download command:

wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.1.0.1829-linux.zip

# TOMCAT download command:

wget https://mirrors.ocf.berkeley.edu/apache/tomcat/tomcat-8/v8.5.84/src/apache-tomcat-8.5.84-src.tar.gz

https://mirrors.ocf.berkeley.edu/apache/tomcat/tomcat-8/v8.5.69/bin/apache-tomcat-8.5.69.tar.gz

# TOMCAT user details:
  <role rolename="manager-script"/>
  <role rolename="manager-gui"/>
  <user username="tomcat" password="tomcat" roles="manager-script,manager-gui"/>
  
  
#Sample Pipeline Script.

pipeline {
      agent any
      stages {
            stage('clone') {
                  steps {
                        echo 'cloning GITHUB repository'
                    
                  }
            }
            stage('Build') {
                  steps {
                        echo 'Building Maven Project'
                  }
            }
            stage('Deploy') {
                  steps {
                        echo "Deploying the application"
                  }
            }
          
      }
}

# KUBERNETES INSTALLATION ANS SETUP {URL and Commands}:

Docker installation URL:

yum install -y -q yum-utils device-mapper-persistent-data lvm2 > /dev/null 2>&1

yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo > /dev/null 2>&1

yum install -y -q docker-ce >/dev/null 2>&1

Disable SELinux:

setenforce 0
sed -i --follow-symlinks 's/^SELINUX=enforcing/SELINUX=disabled/' /etc/sysconfig/selinux

Disable swap:
sed -i '/swap/d' /etc/fstab
swapoff -a

Update sysctl settings for Kubernetes networking:

cat >> /etc/sysctl.d/kubernetes.conf <<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system
                                           
Add yum repository for kubernetes packages:
cat >>/etc/yum.repos.d/kubernetes.repo<<EOF
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
        https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
  
Install Kubernetes:
yum install -y kubeadm-1.15.6-0.x86_64 kubelet-1.15.6-0.x86_64 kubectl-1.15.6-0.x86_64
  
Initialize Kubernetes cluster on MASTER node using command:
kubeadm init --apiserver-advertise-address=<master-node-private-ip> --pod-network-cidr=192.168.0.0/16
  
Run the following command to copy kube config file:
mkdir /home/kubeuser/.kube
cp /etc/kubernetes/admin.conf /home/kubeuser/.kube/config
chown -R kubeuser:kubeuser /home/kubeuser/.kube
  
install Calico network connector using command as kubeuser:
su - kubeuser
kubectl create -f https://docs.projectcalico.org/v3.9/manifests/calico.yaml
  
# vim application-deployment.yml
 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: application-deployment
spec:
  selector:
    matchLabels:
      app: application-deployment
  replicas: 2
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1

  template:
    metadata:
      labels:
        app: application-deployment
    spec:
      containers:
      - name: application-deployment
        image: bhaskerrajput/application-image
        imagePullPolicy: Always
        ports:
        - containerPort: 8080

# application-service.yml
  
apiVersion: v1
kind: Service
metadata:
  name: application-service
  labels:
    app: application-deployment
spec:
  selector:
    app: application-deployment
  type: LoadBalancer
  ports:
    - port: 8080
      targetPort: 8080
      nodePort: 30000
  
  
# NAGIOS XI DOWNLOAD URL: on ubuntu-1 and two extra machines: ubuntu and centos

setup your ec2-instance [ec2 t2medium 2 and 4]

yum -y update

apt-get update -y [ubuntu]

curl https://assets.nagios.com/downloads/nagiosxi/install.sh | sh

after Nagio's installation a login URL will appear: copy and paste that to a browser:

copy ec2 nagio's public IP + the url [nagioxi] 

change time zone; enter first name and last name + emailaddress + start trial
copy trial-key to nagio web page
enter username and password; finish installation.
login and accept trial aggrement for 30days.
next, add the other two machines to be monitored.




# Download NCPA package command:To be installed to the servers that are to be monitored.

rpm -Uvh https://assets.nagios.com/downloads/ncpa/ncpa-latest.el6.x86_64.rpm
After installation, navigate to the ncpa file
 
 cd /usr/local/ncpa/etc
 
 ls -ltr
 
 vim ncpa.cfg
 
 community_string = welcome
 
 :wq!
 
 systemctl restart ncpa
 
 netstat -plant |grep ncpa
 
 
 
# Download check_docker plugin command:

wget http://54.209.36.115/nagiosxi/includes/configwizards/docker/plugins/check_docker.py

validating ci/cd settup
This is a test to integrate Jenkins-docker-ansible and automation!!

enjoy!!
I will be having an interview tomorrow and I pray that that interview is a successfull one.


hello git
