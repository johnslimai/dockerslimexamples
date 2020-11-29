
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

- Exploring the container's layers listed in the xray report

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

The image layer information listed in the ``"image_layers":`` array is useful for understanding what files and consequesntly what software and files are in the container. The layer informaion is organized by layer index indicated by the ``"index":`` tag. Search for index 0 in the image layers list. 

**Layer 0**

Seek the ``"top":`` tag and you can view the files contained in layer 0. Layer 0 is comprised of common Linux OS files included for the Alpine base image. 

```JSON
"top": [
        {
          "change": "A",
          "name": "lib/libcrypto.so.43.0.1",
          "size": 1734840,
          "mode": 493,
          "mod_time": "2018-06-14T02:30:35-04:00",
          "change_time": "0001-01-01T00:00:00Z"
        },
        {
          "change": "A",
          "name": "bin/busybox",
          "size": 796312,
          "mode": 493,
          "mod_time": "2018-12-06T10:13:58-05:00",
          "change_time": "0001-01-01T00:00:00Z"
        },
        {
          "change": "A",
          "name": "lib/ld-musl-x86_64.so.1",
          "size": 584168,
          "mode": 493,
          "mod_time": "2018-06-19T03:48:15-04:00",
          "change_time": "0001-01-01T00:00:00Z"
        },

```

**Layer 1**

Layer 1 containes the files associated with node.js. It is the largest layer (56.8MB) and single file (usr/local/bin/node), the node.js executable is 36.4MB

```JSON

    {
      "id": "eaabbaf2e7fe2603c758103f3f9397a82ea9c1f39e421735753afef370323a1f",
      "index": 1,
      "path": "eaabbaf2e7fe2603c758103f3f9397a82ea9c1f39e421735753afef370323a1f/layer.tar",
      "stats": {
        "all_size": 56810567,
        "object_count": 4178,
        "dir_count": 711,
        "file_count": 3463,
        "link_count": 4,
        "max_file_size": 36468360,
        "max_dir_size": 0,
        "deleted_count": 1,
        "deleted_dir_count": 0,
        "deleted_file_count": 1,
        "deleted_link_count": 0,
        "deleted_size": 0,
        "added_size": 56786658,
        "modified_size": 23909
      },
      "changes": {
        "deleted": 1,
        "added": 4146,
        "modified": 31
      },
      "top": [
        {
          "change": "A",
          "name": "usr/local/bin/node",
          "size": 36468360,
          "mode": 493,
          "mod_time": "2018-12-26T20:40:30-05:00",
          "change_time": "0001-01-01T00:00:00Z"
        },
```

**Layer 2**

Layer 2 containes the files associated with the popular package manager yarn. Yarn is used to configure this container for nuxt.js and also allows developers using this container to easily add and manage packages.

Note: Package managers are good to include in containers for development but should generally be stripped out of production containers to ensure immutability and adhere to production-ready container best practices.

```JSON

      {
          "change": "A",
          "name": "opt/yarn-v1.12.3/README.md",
          "size": 3174,
          "mode": 420,
          "mod_time": "2018-11-07T10:13:08-05:00",
          "change_time": "0001-01-01T00:00:00Z"
        },
        {
          "change": "A",
          "name": "opt/yarn-v1.12.3/LICENSE",
          "size": 1355,
          "mode": 420,
          "mod_time": "2018-11-07T10:13:08-05:00",
          "change_time": "0001-01-01T00:00:00Z"
        },
        {
          "change": "M",
          "name": "root/.gnupg/trustdb.gpg",
          "size": 1200,
          "mode": 384,
          "mod_time": "2018-12-26T20:22:33-05:00",
          "change_time": "0001-01-01T00:00:00Z"
        },
        {
          "change": "A",
          "name": "opt/yarn-v1.12.3/bin/yarn",
          "size": 1025,
          "mode": 493,
          "mod_time": "2018-11-07T10:13:08-05:00",
          "change_time": "0001-01-01T00:00:00Z"
        }

```

***Layers 6,7 and 8***

Layers 6,7 and 8 contain the container entrypoint definition and the .sh scripts for nuxt.js.

Layer 6 (command listing) snippet showing the ENTRYPOINT command 

```JSON
{
          "type": "USER",
          "time": "2019-01-17T18:19:19Z",
          "is_nop": true,
          "local_image_exits": false,
          "layer_index": 6,
          "layer_id": "ee754c9068ed8fccbbd95b642f4f4c069d54e94229a0595ce02b91e16c240c03",
          "layer_fsdiff_id": "sha256:a7f1fa57d58b715c742a983028810777b521e878812e61f26cab0290f967feeb",
          "size": 0,
          "params": "node",
          "command_snippet": "USER node",
          "command_all": "USER node",
          "empty_layer": true
        },
        {
          "type": "ENTRYPOINT",
          "time": "2019-01-17T18:19:19Z",
          "is_nop": true,
          "is_exec_form": true,
          "local_image_exits": false,
          "layer_index": 6,
          "layer_id": "ee754c9068ed8fccbbd95b642f4f4c069d54e94229a0595ce02b91e16c240c03",
          "layer_fsdiff_id": "sha256:a7f1fa57d58b715c742a983028810777b521e878812e61f26cab0290f967feeb",
          "size": 0,
          "params": "[\"/bin/sh\",\"/opt/tools/entrypoint.sh\"]",
          "command_snippet": "ENTRYPOINT [\"/bin/sh\",\"/opt/tools/entrypoint...",
          "command_all": "ENTRYPOINT [\"/bin/sh\",\"/opt/tools/entrypoint.sh\"]",
          "empty_layer": true
        },
```

Layer 7 (layer listing) snippet showing the file 

  {
      "id": "a38c549dca9e68603ab2a47a06b1cdfe3d9d733f5e29d3efb953e36b9e17d8a9",
      "index": 7,
      "path": "a38c549dca9e68603ab2a47a06b1cdfe3d9d733f5e29d3efb953e36b9e17d8a9/layer.tar",
      "stats": {
        "all_size": 180,
        "object_count": 3,
        "dir_count": 2,
        "file_count": 1,
        "link_count": 0,
        "max_file_size": 180,
        "max_dir_size": 0,
        "deleted_count": 0,
        "deleted_dir_count": 0,
        "deleted_file_count": 0,
        "deleted_link_count": 0,
        "deleted_size": 0,
        "added_size": 180,
        "modified_size": 0
      },
      "changes": {
        "deleted": 0,
        "added": 1,
        "modified": 2
      },
      "top": [
        {
          "change": "A",
          "name": "opt/tools/entrypoint-nuxtjs.sh",
          "size": 180,
          "mode": 420,
          "mod_time": "2019-01-31T23:20:58-05:00",
          "change_time": "0001-01-01T00:00:00Z"
        },







