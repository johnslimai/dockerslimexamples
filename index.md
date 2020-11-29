
# Welcome to John's DockerSlim examples

## Example 1: Analyzing a container using the DockerSlim xray command  

### Download and test DockerSlim

 - Download DockerSlim from here: [https://dockersl.im/](https://dockersl.im/)

Note: This example was developed on macOs 

- Create a folder and copy the files from the DockerSlim download into the folder. My folder looks like this.

![]({{site.baseurl}}//dockerslim-folder.png)


- Run DockerSlim to check if it's working. 

Type `docker-slim help` into a terminal window.

Note: I added the folder path I created to the /etc/paths file.

![]({{site.baseurl}}//dockerslim-basic-help.png)

- DockerSlim has a cool menu driven command line too. To try that:

Type `docker-slim` into a terminal window then type help at the >>> prompt

![]({{site.baseurl}}//dockerslim-help-menu.png)

We will be using the xray command for this example


### Find a publically available container on DockerHub to examine with DockerSlim xray
 
 I decided to use a container with nuxt.js and node.js for this example. Nuxt.js is gaining popularity as a web app framework for vue.js. My team has been working with nuxt.js lately, so its interesting to me.
 

 - Open [DockerHub](https://hub.docker.com/) and type `nuxt` into the search bar. you can also just click this link:
 
 [https://hub.docker.com/search?q=nuxt&type=image](https://hub.docker.com/search?q=nuxt&type=image)


![docker-hub-search-nuxt.png]({{site.baseurl}}/docker-hub-search-nuxt.png)

I selected the first result for simplicity. Its been donwloaded a bunch of times and seems to have nuxt.js and node.js foundations and a decent GitHub page.

![dockerhub-nuxt-conatiner-target.png]({{site.baseurl}}/dockerhub-nuxt-conatiner-target.png)

- Pull the container using the Docker Pull Command

From a terminal window type `docker pull gerardojunior/nuxtjs`

Note: You need [Docker Desktop](https://www.docker.com/products/docker-desktop) installed on your host to continue this example.

After the pull command completes. Check to see that the image is loaded using the `docker images` command

![docker-pull.png]({{site.baseurl}}/docker-pull.png)

### Run the DockerSlim xray command on the target container

From a terminal window type `xray --target gerardojunior/nuxtjs:latest`

![dockerslim-xray-command.png]({{site.baseurl}}/dockerslim-xray-command.png)


DockerSlim creates a .json report file named `slim.report.json`

![dockerslim-report-directory.png]({{site.baseurl}}/dockerslim-report-directory.png)



### Examine the DockerSlim xray results

- Basic facts about this container foound in the contaier report

Created date = 2/1/2019  
Image Size = 69MB  
Expose Ports = 3000/TCP  

From the slim.report.json file

```JSON
  "source_image": {
    "id": "sha256:602502213c4a3e53ed290235ac955eac6fecd77808942b0e962e83946a62ab70",
    "name": "gerardojunior/nuxtjs:latest",
    "size": 68552646,
    "size_human": "69 MB",
    "create_time": "2019-02-01T04:21:20Z",
    "all_names": [
      "gerardojunior/nuxtjs:latest"
    ],
    "docker_version": "18.03.1-ee-3",
    "architecture": "amd64",
    "user": "node",
    "exposed_ports": [
      "3000/tcp"
    ]
  }

```















