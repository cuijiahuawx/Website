@def title = "systemd "
@def tags = ["systemd "]

\toc
# systemd配置

## 通用命令
```
systemctl daemon-reload
systemctl status jupyterlab.service 
```
## JupyterLab
```
jupyter notebook --generate-config
jupyter notebook password
sudo vim /etc/systemd/system/jupyterlab.service

[Unit]
Description=JupyterLabService
After=network.service
[Service]
User=ubuntu
ExecStart=/home/ubuntu/anaconda3/bin/jupyter-lab --ip 0.0.0.0 --port 8888 --no-browser --notebook-dir /home/ubuntu/jupyterlab                                                                       
```
## Excalidraw
```
git clone https://github.com/excalidraw/excalidraw.git
npm install --global yarn 
yarn

sudo vim /etc/systemd/system/excalidraw.service

[Unit]
Description=Excalidraw
After=network.service
[Service]
WorkingDirectory=/home/ubuntu/excalidraw
ExecStart=yarn start
```
