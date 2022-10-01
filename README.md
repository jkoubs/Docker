# Create Docker Image containing full ROS code 

To get the full ROS code we will create <strong>two images</strong>. We will apply <strong>multi-stage builds</strong>, which use multiple <em>FROM</em> statements in a Dockerfile. The first image contains the <strong>ROS melodic distribution</strong> and the second, the <strong>ROS simulation codes</strong>. Finally we will run the last image by creating a container named <strong>cali_project</strong>.

## 1) Create folder containg the Dockerfiles

Inside your <strong>catkin_ws</strong>, create a folder named <strong>docker_ros</strong>.

Then create <strong>two Dockerfiles</strong> :
<ul>
  <li><strong>dockerfile_ros_melodic</strong> : Containg the ROS Melodic Image</li>
  <li><strong>dockerfile_cali</strong>: Containg the ROS simulation files/codes</li>
</ul>

```bash
cd ~/catkin_ws/src
mkdir docker_ros
cd docker_ros
touch dockerfile_ros_melodic dockerfile_cali
```

## 2) Build the 1st Image - ROS melodic Image

We build the 1st image named <strong>ros_melodic</strong> using the <strong>dockerfile_ros_melodic</strong> Dockerfile which uses the ROS Melodic distribution as a base.


```bash
cd ~/catkin_ws/src/docker_ros
sudo docker build -f dockerfile_ros_melodic -t ros_melodic .
```

## 3) Build the 2nd Image - Simulation codes

Now, we will build the second Image named <strong>cali_base</strong> using the <strong>dockerfile_cali</strong> Dockerfile which uses as base the 1st image <strong>ros_melodic</strong> and that will contain the ROS simulation codes.

```bash
sudo docker build -f dockerfile_cali -t cali_base .
```
## 4) Run the Final Image

<u><strong><em>Requirement</em></strong></u> : To run GUI applications in Docker on Linux hosts, you have to run <strong>"xhost +local:root"</strong>. To disallow, <strong>"xhost -local:root"</strong>. For Windows and Mac hosts please check : [Running GUI applications in Docker on Windows, Linux and Mac hosts](https://cuneyt.aliustaoglu.biz/en/running-gui-applications-in-docker-on-windows-linux-mac-hosts/). Can also found some more information about [Using GUI's with Docker](http://wiki.ros.org/docker/Tutorials/GUI).


```bash
xhost +local:root
```


We can now <strong>run</strong> the image by creating a container named <strong>cali_project</strong> :

We will see <strong>2 methods</strong> to run the final image:

1) <u>Using docker-compose</u> :

    This will use the <strong>docker-compose.yaml</strong> file.

    ```bash
    sudo docker-compose up
    ```

2) <u>Without docker-compose</u> :


    ```bash
    sudo docker run -it --rm --net=host -v /tmp/.X11-unix:/tmp/.X11-unix:rw --name=cali_project --privileged -e DISPLAY -e QT_X11_NO_MITSHM=1 cali_base:latest bash
    ```

    We can now run our simulation codes inside the container.








