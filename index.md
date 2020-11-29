
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


- Examine the container layers information by DockerFile command in the xray report

The report file contains a listing ("image_stack label") of the docker image layers ordered by layer detailing  Dockerfile command(s) that contributed to each layer. 

Find the ``image_stack:`` array in the slim.report.json file. 

This container contains a single image. Each Dockerfile command is listed in the ``"instructions":`` array. In the snippet below notice that the "ADD" instruction contibuted to ``"layer_index": 0,``. Peruse the report to find the total number if layer indices. This container has a total of 9 layers.

```JSON

  "image_stack": [
    {
      "is_top_image": true,
      "id": "sha256:602502213c4a3e53ed290235ac955eac6fecd77808942b0e962e83946a62ab70",
      "full_name": "gerardojunior/nuxtjs:latest",
      "repo_name": "gerardojunior/nuxtjs",
      "version_tag": "latest",
      "raw_tags": [
        "gerardojunior/nuxtjs:latest"
      ],
      "create_time": "2019-02-01T04:21:20Z",
      "new_size": 68552646,
      "new_size_human": "69 MB",
      "instructions": [
        {
          "type": "ADD",
          "time": "2018-12-21T00:21:29Z",
          "is_nop": true,
          "local_image_exits": false,
          "layer_index": 0,
          "layer_id": "5491dce778832e33c284cd8185100e76d6daa18f8cbc32458c706776894127fc",
          "layer_fsdiff_id": "sha256:7bff100f35cb359a368537bb07829b055fe8e0b1cb01085a3a628ae9c187c7b8",
          "size": 4413428,
          "size_human": "4.4 MB",
          "params": "file:2ff00caea4e83dfade726ca47e3c795a1e9acb8ac24e392785c474ecf9a621f2 in /",
          "command_snippet": "ADD file:2ff00caea4e83dfade726ca47e3c795a1e9...",
          "command_all": "ADD file:2ff00caea4e83dfade726ca47e3c795a1e9acb8ac24e392785c474ecf9a621f2 /",
          "target": "/",
          "source_type": "file"
        },
      
```

The most interesting layers in this container are:


**Layer 0**

















