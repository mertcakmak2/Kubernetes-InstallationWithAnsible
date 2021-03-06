# Kubernetes-InstallationWithAnsible


master ubuntu server ip=> 164.92.244.231

worker ubuntu server ip=> 164.92.246.143

# Ansible Kurulumu

`sudo apt update`

`sudo apt install software-properties-common -y`

`sudo apt install ansible -y`

# SSH Keygen oluşturma ve oluşan keygeni Master, Worker Makinelerine kopyalama

`ssh-keygen -q -f ~/.ssh/id_rsa -N ""`

`ssh-copy-id root@164.92.246.143`  Worker sunucusuna kopyalama işlemi

`ssh-copy-id root@164.92.244.231`   Master sunucusuna kopyalama işlemi

# Ansible ile Yaml dosyalarını kullanarak kubernetes kurulumu

`ansible-playbook -i hosts users.yml`

`ansible-playbook -i hosts install-k8s.yml`

`ansible-playbook -i hosts master.yml`

`ansible-playbook -i hosts join-workers.yml`

![image](https://user-images.githubusercontent.com/21373505/142379101-d341b8b8-a125-46da-a059-c4c8c15ed9c1.png)

# Olası hatalar

1) Calico için kubeadm, kubectl ve kubelet versiyonu 1.21 yada üstü olmalı (install-k8s.yml install kubernetes adımı)

2) `kubectl get nodes` sonrası
"The connection to the server localhost:8080 was refused - 
did you specify the right host or port?" 

hatası çözümü:

Kaynak: https://k21academy.com/docker-kubernetes/the-connection-to-the-server-localhost8080-was-refused/

cp /etc/kubernetes/admin.conf $HOME/

chown $(id -u):$(id -g) $HOME/admin.conf

export KUBECONFIG=$HOME/admin.conf

echo 'export KUBECONFIG=$HOME/admin.conf' >> $HOME/.bashrc


## Başka bir alternatif çözüm

https://discuss.kubernetes.io/t/the-connection-to-the-server-localhost-8080-was-refused-did-you-specify-the-right-host-or-port/1464/2

mkdir -p $HOME/.kube

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config

