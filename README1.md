

##  ***Install kubernetes(k8s) on ubuntu***
table of content:
Prerequisite
Launch EC2 instances(Ubuntu)
Install docker
Install kind
Install kubectl
Create single node cluster using kind
create multi node note cluster
Delete cluster
Install MYSQL using helm
Run mysql inside the pod
Delete mysql
Run mysql outside the pod
Create user in mysql
Port forwarding
Connect MYSQL to python
Read csv file using python pandas


 





<div id="top"></div>
<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#getting-started">pre requirements :</a>
      <ul>
        <li><a href="#prerequisites">Ec2 or ubuntu </a></li>
        <li><a href="#prerequisites">docker (docker) - no sudo required</a></li>
        <li><a href="#prerequisites">Kind</a></li>
        <li><a href="#prerequisites">Kubectl</a></li>
      </ul>
    </li>
    <li><a href="#Launch EC2 instances(Ubuntu )">Launch EC2 instances(Ubuntu )
</a></li>
    <li><a href="#Install docker">Install docker</a></li>
    <li><a href="#Install kind"> Install kind</a></li>
    <li><a href="#Install kubectl"> Install kubectl</a></li>
    <li><a href="#Create single node cluster using kind"> Create single node cluster using kind</a></li>
        <li><a href="#Create single node cluster using kind"> Create multi node cluster using kind</a></li>
  </ol>
</details>


















### pre requirements :

1. Ec2 or ubuntu
2. docker (docker) - no sudo required
3. Kind
4. Kubectl
<!-- Launch EC2 instances(Ubuntu ) -->
  ## Step 1. Launch EC2 instances(Ubuntu )

<hr>
  <!-- INSTALL DOCKER -->

##  Step 2. Install docker
1. ```sh 
   sudo apt update
   ```

2. ```sh 
   sudo apt install apt-transport-https ca-certificates curl software-properties-common 
   ```
  
3. ```sh 
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```
4. ```sh 
   sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
   ```
5. ```sh 
   apt-cache policy docker-ce
   ```

6. ```sh 
   sudo apt install docker-ce
   ```


<p align="right">(<a href="#top">back to top</a>)</p>

#### Executing the Docker Command Without Sudo (Optional)
1. ```sh
   sudo usermod -aG docker ${USER}
   ``` 

2. ```sh
   su - ${USER}
   ```

3. ```sh 
   groups
  ```
   ```
#### For ubuntu wsl Ref: [https://medium.com/@sebagomez/installing-the-docker-client-on-ubuntus-windows-subsystem-for-linux-612b392a44c4](https://medium.com/@sebagomez/installing-the-docker-client-on-ubuntus-windows-subsystem-for-linux-612b392a44c4)

### IF this type of error comes in ubuntu WSL
```sh
  ERROR: failed to list clusters: command  "docker ps -a --filter label=io.x-k8s.kind.cluster --format '{{.Label "io.x-k8s.kind.cluster"}}'" failed with error: exit status 1  
Command Output: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
  ```
### So run this commands
```sh
 export DOCKER_HOST=localhost:2375`
`echo  "export DOCKER_HOST=localhost:2375" >> ~/.bash_profile
  ```

##### Ref:[https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)

##### With sudo permission: [https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)
##### Check docker version and print “hello word”
1. ```sh
   docker --version
   ``` 

2. ```sh
   docker run hello-world
   ```

<p align="right">(<a href="#top">back to top</a>)</p>

<hr>

## Step 3. Install kind
1. ```sh
   curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.11.1/kind-linux-amd64
  ```

2. ```sh
   chmod +x ./kind
   ```

3. ```sh
    sudo mv kind /usr/local/bin
    ```
##### Ref: [https://www.youtube.com/watch?v=kkW7LNCsK74](https://www.youtube.com/watch?v=kkW7LNCsK74)[https://kind.sigs.k8s.io/docs/user/quick-start/](https://kind.sigs.k8s.io/docs/user/quick-start/)

### Check kind version
```sh
  kind --version
  ```

<p align="right">(<a href="#top">back to top</a>)</p>

<hr>
 <!-- INSTALL KUBECTL -->

## Step 4. Install kubectl
##### Follow given Steps to install Kubectl
1. ```sh
   curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
   ```

2. ```sh
   curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
   ```

3. ```sh
   echo  "$(<kubectl.sha256) kubectl" | sha256sum --check
   ```

4. ```sh
   sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
   ```

##### Check with
```sh
  kubectl
  ```
##### Ref [https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)
#### Check kubectl version
`kubectl version`

<p align="right">(<a href="#top">back to top</a>)</p>
<hr>
<!-- Create single node cluster using kind -->

## Step 5. Create single node cluster using kind

  

##### Default cluster context name is `kind`.
```sh
   kind create cluster
   ```

  

##### create cluster with name
```sh
   kind create cluster --name
   ```
#### Verify with
```sh
   kind get clusters
   ```
##### Ref: [https://www.youtube.com/watch?v=kkW7LNCsK74](https://www.youtube.com/watch?v=kkW7LNCsK74)

## 6.Configuring Your kind Cluster(create multi node cluster)

```sh
   kind create cluster --config <file_name.yml>
   ```

#### .Yaml file to create two node cluster
```yml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
containerdConfigPatches:
  - |-
    [plugins."io.containerd.grpc.v1.cri".registry.mirrors."registry:5000"]
      endpoint = ["http://registry:5000"]
nodes:
- role: control-plane
- role: worker
  kubeadmConfigPatches:
  - |
    kind: JoinConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "has-cpu=true"
```


## 7.Delete cluster
```sh
   kind delete cluster
   ```

<p align="right">(<a href="#top">back to top</a>)</p>
<Br>
<hr>
<h1>---------------Thank You!🤵-------------- </h1>
<Br>

------
  
Credit: [Atul YAdav](https://github.com/atul8122000)
Last Edited on: 01/01/2022