# copilot-at-home
Instructions to install self hosted code assistant for your IDE

Works best if you have a GPU for the server. 
The tabby server can be installed anywhere in you organisation.

## Prepare server host

The server must have Nvidia drivers already installed : https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/index.html

### Install Docker on host.
```Bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo usermod -aG docker $USER
```
### Install Nvidia container toolkit
```Bash
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```
### Install Tabby Server

Reference documentation : https://tabby.tabbyml.com/docs/welcome/

Clone this repository on the server, or add the docker-compose.yaml file 

Run the container with `docker compose up`

### Develloper Usage

Have devellopers register themselves to the following link, http://[tabby host]:8080

Install a compatible addon for your IDE, example : https://marketplace.visualstudio.com/items?itemName=TabbyML.vscode-tabby

Add your authentication token from the web UI.

Code!


