# base-container-template
Ubuntu 18.04 container template with RDP and VNC enabled which can be extended for more specialised applications.

This is a modified version of the container created by consol (https://hub.docker.com/r/consol/ubuntu-xfce-vnc/). 

## Usage
The container can be run by both root and a user with a non-root UID and GID.
A default user has been created with the credentials default:default.
The default user has passwordless sudo priviledges.

To run this container using Python 3, the following script can be used:

```python
import docker

client = docker.from_env()
groups = [100] # Needed as this group allows for sudo access
image = 'aicampbell/vnc-ubuntu18-xfce'
volume = {"/host/path/to/nfs/directory" : {"bind": "/container/path/to/nfs/directory", "mode": "rw"}, # NFS - this is acting as the data directory which is stored on NFS
}
user = '<UID of user>:<GID of user>' # Needed for read/write permissions using NFS

client.containers.run(image=image, detach=True, publish_all_ports=True, volumes=volume, user=user, group_add=groups)
```
## Accessing the container using VNC/RDP viewer
Once the container has been created, a VNC/RDP client can be used to test or use the container.
To see the open ports, use the command
```
sudo docker ps - a
```
Example ouput:
PORTS
0.0.0.0:32901->3389/tcp, 0.0.0.0:32900->5901/tcp, 0.0.0.0:32899->6901/tcp

### RDP
Using an RDP client, use the port which maps to 3389. From the example above, use 0.0.0.0:32901

### VNC
Using a VNC client, use the port which maps to 5901. From the example above, use 0.0.0.0:32900
The vnc password is: vncpassword

### NoVNC
Using a NoVNC client, use the port which maps to 6901. From the example above, use 0.0.0.0:32899
The vnc password is: vncpassword
