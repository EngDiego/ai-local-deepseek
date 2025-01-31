# Era: Modelo Companheiro Sem Censura

🌎 Idiomas Disponíveis:
🇺🇸 [Inglês](README.md) | 🇧🇷 [Português](README.pt-br.md)

Este é um modelo companheiro sem censura usando Llama3.1.

Para a melhor experiência, use o Open-WebUI, a maneira mais simples de executar o LLM com um serviço web.

---

## Pré-requisitos

1. **Verifique os Drivers da GPU**
   - Certifique-se de que sua máquina tenha os drivers mais recentes instalados para **Nvidia, AMD ou Intel GPU**.

2. **Usuários do Windows: Instale o WSL (Subsistema Windows para Linux)**
   - Abra o Terminal do Windows e execute:
     ```bash
     wsl -l -o
     wsl --install Ubuntu
     wsl -l -v
     ```

3. **Configure o WSL para Suporte à GPU**
   - **Para Nvidia**:
     - Siga o [guia de instalação do CUDA](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local).
     - Instale as dependências:
       ```bash
       sudo apt install build-essential
       ```
     - Instale o CUDA:
       ```bash
       wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
       sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
       wget https://developer.download.nvidia.com/compute/cuda/12.8.0/local_installers/cuda-repo-wsl-ubuntu-12-8-local_12.8.0-1_amd64.deb
       sudo dpkg -i cuda-repo-wsl-ubuntu-12-8-local_12.8.0-1_amd64.deb
       sudo cp /var/cuda-repo-wsl-ubuntu-12-8-local/cuda-*-keyring.gpg /usr/share/keyrings/
       sudo apt-get update
       sudo apt-get -y install cuda-toolkit-12-8
       ```
     - Verifique o Driver:
       ```bash
       nvidia-smi
       ```

   - **Para AMD Radeon** (Ubuntu 22.04 obrigatório):
     - Siga o [guia de instalação do ROCm](https://rocm.docs.amd.com/projects/radeon/en/latest/docs/install/wsl/install-radeon.html).

---

## Etapas de Instalação

### 1. Instale o Docker
- **Linux**: Siga [este guia](https://docs.docker.com/engine/install/ubuntu/).
- **Windows**: Siga [este guia](https://docs.docker.com/desktop/setup/install/windows-install/).

- **Para WSL (baseado em Ubuntu)**:
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

### 2. (Opcional) Instalar o Portainer para Gerenciamento do Docker
```bash
sudo docker volume create portainer_data
sudo docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:2.21.5
```
- Access [https://localhost:9443/](https://localhost:9443/).

### 3. Implantar o Ollama (Motor LLM)
```bash
sudo docker run -d --gpus=all -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```

### 4. Implantar o Open-WebUI
```bash
sudo docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

### 5. Acessar a Interface Web
- Abra [http://localhost:8080/](http://localhost:8080/).

### 6. Configurar o Open-WebUI
1.  **Crie e faça login como Admin**.
2.  **Configuração de Admin → Conexão**: Certifique-se de que ele se conecta a [http://host.docker.internal:11434](http://host.docker.internal:11434).
3.  **Configuração de Admin → Modelos**:
    - Extraia o modelo Ollama.com.
    - Insira `deepseek-r1:7b` e faça o download.
4.  **Carregar o Modelo Era**:
    - Vá para [Modelo Companheira Mika](https://openwebui.com/m/digo/mika/) e adicione-o ao Open-WebUI.

### 7. Pronto para Usar!

---