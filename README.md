# Github link

[Step by step use deepseek AI model locally](https://github.com/EngDiego/ai-local-deepseek).

# Era: Uncensored Companion Model

🌎 Available Languages:  
🇺🇸 [English](README.md) | 🇧🇷 [Português](README.pt-br.md)

This is an uncensored companion model using Llama3.1.

For the best experience, use Open-WebUI, the simplest way to run the LLM with a web service.

---

## Prerequisites

1. **Check GPU Drivers**
   - Ensure your machine has the latest drivers installed for **Nvidia, AMD, or Intel GPU**.

2. **Windows Users: Install WSL (Windows Subsystem for Linux)**
   - Open Windows Terminal and run:
     ```bash
     wsl -l -o
     wsl --install Ubuntu
     wsl -l -v
     ```

3. **Configure WSL for GPU Support**
   - **For Nvidia**:
     - Follow the [CUDA installation guide](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local).
     - Install dependencies:
       ```bash
       sudo apt install build-essential
       ```
     - Install CUDA:
       ```bash
       wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
       sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
       wget https://developer.download.nvidia.com/compute/cuda/12.8.0/local_installers/cuda-repo-wsl-ubuntu-12-8-local_12.8.0-1_amd64.deb
       sudo dpkg -i cuda-repo-wsl-ubuntu-12-8-local_12.8.0-1_amd64.deb
       sudo cp /var/cuda-repo-wsl-ubuntu-12-8-local/cuda-*-keyring.gpg /usr/share/keyrings/
       sudo apt-get update
       sudo apt-get -y install cuda-toolkit-12-8
       ```
     - Check Driver:
       ```bash
       nvidia-smi
       ```

   - **For AMD Radeon** (Ubuntu 22.04 required):
     - Follow the [ROCm installation guide](https://rocm.docs.amd.com/projects/radeon/en/latest/docs/install/wsl/install-radeon.html).

---

## Installation Steps

### 1. Install Docker
- **Linux**: Follow [this guide](https://docs.docker.com/engine/install/ubuntu/).
- **Windows**: Follow [this guide](https://docs.docker.com/desktop/setup/install/windows-install/).

- **For WSL (Ubuntu-based)**:
  ```bash
  sudo apt-get update
  sudo apt-get install ca-certificates curl
  sudo install -m 0755 -d /etc/apt/keyrings
  sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
  sudo chmod a+r /etc/apt/keyrings/docker.asc
  echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
    $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  sudo apt-get update
  sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
  sudo docker run hello-world
  ```

### 2. (Optional) Install Portainer for Docker Management
```bash
sudo docker volume create portainer_data
sudo docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:2.21.5
```
- Access [https://localhost:9443/](https://localhost:9443/).

### 3. Deploy Ollama (LLM Engine)
```bash
sudo docker run -d --gpus=all -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```

### 4. Deploy Open-WebUI
```bash
sudo docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

### 5. Access the Web Interface
- Open [http://localhost:8080/](http://localhost:8080/).

### 6. Configure Open-WebUI
1. **Create and log in as Admin**.
2. **Admin Config → Connection**: Ensure it connects to [http://host.docker.internal:11434](http://host.docker.internal:11434).
3. **Admin Config → Models**:
   - Extract Ollama.com model.
   - Insert `deepseek-r1:7b` and download it.
4. **Load Era Model**:
   - Go to [Mika Companion Model](https://openwebui.com/m/digo/mika/) and add it to Open-WebUI.

### 7. Ready to Use!

---