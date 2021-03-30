---
title: Add new node in jenkins
---
## Prepare Node
1. Create new linux machine and config network, storage, security group and etc.
2. Update system
```shell
sudo yum update -y
sudo reboot
```
3. Clear old package
```shell
sudo package-cleanup --oldkernels --count=1 -y
```
4. Run command to create user
```shell
sudo yum install java-1.8.0-openjdk -y
sudo useradd jenkins
sudo passwd jenkins
su jenkins
ssh-keygen -t rsa -C "Jenkins agent key"
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
cat ~/.ssh/id_rsa
mkdir ~/data
exit
```
5. Install git
`sudo yum install -y git`
6. Install maven
```shell
sudo yum install -y wget
wget https://archive.apache.org/dist/maven/maven-3/3.6.1/binaries/apache-maven-3.6.1-bin.tar.gz -P /tmp
sudo tar xf /tmp/apache-maven-3.6.1-bin.tar.gz -C /usr/share
sudo ln -s /usr/share/apache-maven-3.6.1 /usr/share/maven
sudo chmod 755 -R /usr/share/apache-maven-3.6.1/
sudo bash -c 'cat << EOF > /etc/profile.d/maven.sh
export PATH="/usr/share/maven/bin:$PATH"
EOF'
sudo chmod 755 /etc/profile.d/maven.sh
source /etc/profile.d/maven.sh
mvn -v
```
7. Install docker
```shell
sudo yum remove  -y docker \
                     docker-client \
                     docker-client-latest \
                     docker-common \
                     docker-latest \
                     docker-latest-logrotate \
                     docker-logrotate \
                     docker-engine
sudo yum install -y amazon-linux-extras \
                      yum-utils \
                      device-mapper-persistent-data \
                      lvm2
sudo amazon-linux-extras install -y docker=18.06.1
docker -v
sudo systemctl enable docker
sudo systemctl start docker
sudo chown jenkins:docker /var/run/docker.sock
sudo usermod -aG docker jenkins
```
8. Config file open limit
```shell
sudo bash -c 'echo "jenkins          hard nofile 5000" >> /etc/security/limits.conf'
sudo bash -c 'echo "jenkins          soft nofile 5000" >> /etc/security/limits.conf'
```
## Add Node
1. Add credential in jenkins
   1. "Manage Jenkins" -> "Manage Credentials" -> Click any credential name -> "Back to Global credentials(unrestricted)" -> "Add Credentials" -> Select kind = "SSH Username with private key" -> Fill the private key which generate in node machine
2. Add node in jenkins
   1. "Manage Jenkins" -> "Manage Nodes and Clouds" -> "New Node"
   2. Set "Node Name", select "Permanent Agent" and setting the node