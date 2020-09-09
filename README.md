# k8



==========COMMON FOR MASTER & SLAVES START ====

sudo apt-get update -y
sudo apt-get install -y apt-transport-https
sudo su -

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

apt-get update -y

swapoff -a
sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab


apt install docker.io -y
usermod -aG docker ubuntu



systemctl restart docker
systemctl enable docker.service


apt-get install -y kubelet kubeadm kubectl 

systemctl daemon-reload 
systemctl start kubelet 
systemctl enable kubelet.service 


==========COMMON FOR MASTER & SLAVES END=====



===========In Master Node Start====================
# Execute below commond as root user
kubeadm init

#exit root user & execute as normal user
  
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"

kubectl get nodes

kubectl get pods --all-namespaces


# Get token

kubeadm token create --print-join-command

=========In Master Node End====================



=========In Worker Nodes Start===================
sudo su

Copy kubeadm join token and execute in   Worker Nodes to join to cluster

=========In Worker Nodes End===================
