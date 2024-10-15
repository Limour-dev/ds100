# ds100
## 创建环境
1. 访问商店获取 [Windows Subsystem for Linux](https://apps.microsoft.com/detail/9p9tqf7mrm4r?hl=en-us&gl=US)
2. 下载 [Docker Desktop Installer](https://www.docker.com/products/docker-desktop/)
3. 打开 Docker Desktop，然后选择 Settings > Proxies 两个都设置为 `http://127.0.0.1:7890`
4. 在某文件夹下新建 `docker-compose.yml` 文件，填入以下内容
```yml
services:
  notebook:
    image: jupyter/r-notebook
    restart: unless-stopped
    extra_hosts:
      - 'host.docker.internal:host-gateway'
    environment:
      - DOCKER_STACKS_JUPYTER_CMD=nbclassic
      - TZ=Asia/Shanghai
      - JUPYTER_GATEWAY_REQUEST_TIMEOUT=600
      - JUPYTER_GATEWAY_CONNECT_TIMEOUT=600
    ports:
      - '57002:8888'
    volumes:
      - './home:/home/jovyan'
    command: start-notebook.py --NotebookApp.token='***'
```
5. 该文件夹下在终端执行 `docker compose up -d`
6. 访问 `http://127.0.0.1:57002/`
7. 进入 jupyter 内的终端，执行以下命令
```bash
conda init
source activate
conda config --add envs_dirs /home/jovyan/upload/envs
conda create -n mamba conda-forge::mamba
source activate mamba
git clone https://github.com/Limour-dev/ds100.git
```
## python
```bash
conda create -n ds100py conda-forge::pandas conda-forge::matplotlib conda-forge::seaborn conda-forge::plotly conda-forge::ipykernel conda-forge::nbformat
conda run -n ds100py python -m ipykernel install --user --name ds100py
```