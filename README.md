# Create Docker Image containing full ROS code 

## Method 1: Using Dockerfiles

Here, we will create two images. The first one will contain the <strong>ROS melodic distribution</strong> and the second, the <strong>ROS simulation codes</strong>.

### 1) Create folder containg the Dockerfiles

Inside your <strong>catkin_ws</strong>, create a folder named <strong>docker_ros</strong>.

Then create <strong>two Dockerfiles</strong> :
<ul>
  <li><strong>dockerfile_ros_melodic</strong> : Containg the ROS Melodic Image</li>
  <li><strong>dockerfile_cali</strong>: Containg the ROS simulation files/codes</li>
</ul>

```bash
cd ~/catkin_ws
mkdir docker_ros
touch dockerfile_ros_melodic dockerfile_cali
```

### 2) Build the 1st Image - ROS melodic Image

We build the 1st image named <strong>ros_melodic</strong> using the <strong>dockerfile_ros_melodic</strong> Dockerfile and that will contain the ROS Melodic distribution.

```bash
docker build -f dockerfile_ros_melodic -t ros_melodic .
```

### 3) Build the 2nd Image - Simulation codes

Now, we will build the 2nd Image named <strong>cali_base</strong> using the <strong>dockerfile_cali</strong> Dockerfile that is based from the 1st one and that will contain the ROS simulation codes.

```bash
docker build -f dockerfile_cali -t cali_base .
```

We can now run the image by creating a container named <strong>cali_project</strong> :

We will now see <strong>2 methods</strong> to run the final image:

1) <u>Without docker-compose</u> :


    ```bash
    docker run -it --rm --net=host -v /tmp/.X11-unix:/tmp/.X11-unix:rw --name=cali_project --privileged -e DISPLAY -e QT_X11_NO_MITSHM=1 cali_base:latest bash
    ```
    
    This command as parameters allowing Docker to display GUI with X-server.

2) <u>With docker-compose</u> :

    This will use the <strong>docker-compose.yaml</strong> file.

    ```bash
    docker-compose up
    ```





