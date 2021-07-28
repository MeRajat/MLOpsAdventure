### Installation Guide 

a. For Mac and Windows user, Install Multipass https://multipass.run/. 

b. To create a vm using multipass, go to terminal 
    <br><br>On WIndows and Mac : <br>
   ``` multipass launch --name kubeflow --mem 16G --disk 50G --cpus 4 ```<br>
   and enter in this VM  <br>
   `multipass shell kubeflow`

<br> On Ubuntu <br>
`Just chill :) and open terminal `

c. Install MicroK8s 

- `sudo snap install microk8s --classic` 

- Verify installation `sudo microk8s status --wait-ready` 
- Join Microk8s group 
  ```
    sudo usermod -a -G microk8s $USER
    sudo chown -f -R $USER ~/.kube
    ```
    Exit and re-enter the session. 

d. For Multipass users, make sure pods can reach to internet.  


    sudo iptables -p FORWARD ACCEPT 
    sudo apt-get install iptables-persistent 


    if using ufw    
    sudo ufw default allow routed

    Inspect and verify 
    microk8s inspect # Warning will be shown if firewall not forwarding traffic.

e. Deply Kubeflow      

    microk8s.enable dns dashboard storage  
    microk8s.enable kubeflow  ## This will print user name and password. If succeeds      
    ## Starting kubeflow will take a lot of time 
    microk8s.kubectl get all --all-namespaces  ## Keep  track of status 

    ## Exit shell and get socks proxy
    sudo ssh -D9999 -i /var/root/Library/Application\ Support/multipassd/ssh-keys/id_rsa ubuntu@<kubeflow_ip>

    ## For IP use multipass list 

d.  In network allow socks proxy, go to `settings > network > network proxy` and enable SOCKS proxy pointing it to `127.0.0.1:9999` 

e. On Browser window, http://10.64.140.43.xip.io , credentials to access the dashboard, are available in kubeflow enable output. 