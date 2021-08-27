 # 容器镜像服务
- Create Dockerfile

  - eg

    ```dockerfile
    FROM cupy/cupy:latest # source image
    
    # install required packages
    COPY ./requirements.txt /requirements.txt   #copy at first so that you don't need to install requirements again when you change your code 
    
    WORKDIR /
    
    RUN pip3 install -r requirements.txt -i https://mirror.baidu.com/pypi/simple  #use baidu source
    
    # change apt rep sources 
    RUN sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list 
    
    # copy your code and model 
    COPY ./ /  
    
    ```

    

- Build docker image (run from local side)

  ```shell
  sudo docker build -t test:latest .  # -t tag ,docker image name . indicate its in current directory
  ```

  

- Push to local server (run from the local side )
  - Add 192.168.8.156 to trusted host( (only need to do this when you push for the first time)

    ```shell
    sudo su  #  switch to god
    
    echo '{ "insecure-registries":["192.168.8.156:5000"] }' > /etc/docker/daemon.json
    
    systemctl daemon-reload
    
    systemctl restart docker
    
    su ubuntu  #  switch back
    
    ```
    
  -  Tag image

    ```shell
    sudo docker tag test:latest 192.168.8.156:5000/test:latest		
    ```

  - Push the image to the local server

    ```shell
    sudo docker push 192.168.8.156:5000/test:latest
    ```

    

- Pull from GPU server or another local server (run from the server-side)

  - Add 192.168.8.156 to trusted host (only need to do this when you pull for the first time)		

    ```shell
    sudo su   #  switch to god
    
    echo '{ "insecure-registries":["192.168.8.156:5000"] }' > /etc/docker/daemon.json
    
    systemctl daemon-reload
    
    systemctl restart docker
    
    su ubuntu  #  switch back
    ```

  - Pull image

    ```SHE
    sudo  docker pull 192.168.8.156:5000/test:latest
    ```

    

- Check available images, visit

​		http://192.168.8.156:5000/v2/_catalog




​			
