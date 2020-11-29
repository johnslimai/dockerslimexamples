
# Welcome to John's DockerSlim examples

## Example 1: Analyzing a container using the DockerSlim xray command  

 1. **Install and test DockerSlim**

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


 2. **Find a publically available container on DockerHub to examine with DockerSlim xray**
 
 I decided to use a container with nuxt.js and node.js for this example. Nuxt.js is gaining popularity as a web app framework for vue.js. My team has been working with nuxt.js lately, so its interesting to me.
 

 - Open [DockerHub](https://hub.docker.com/) and type `nuxt` into the search bar. you can also just click this link:
 
 [https://hub.docker.com/search?q=nuxt&type=image](https://hub.docker.com/search?q=nuxt&type=image)


