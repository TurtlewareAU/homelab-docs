The best place to start is inside a repository click on the acitons link. Here you will see a landing page for getting started. I entered the below search items and found a publish to ghcr.io job.

![](Pasted%20image%2020221204104307.png)

---
### Initial Action Setup

Check your action variables and setup information. Here I named it after my website and the action its performing. Building the react app for Integrate. 

`name:` The name displayed in the actions tab when running, and completed.
`on:` Is the trigger section 
`push:` Listens for push events on a specific branch
`branches:` All of the branches we will listen to. so could have multiple
`tags:` Version details for the current build.
`env:` This sets up where the information will be stored to. 
`REGISTRY:` ghcr.io (Github Container Registry)
`IMAGE_NAME:` will be the name of the container stored in your repository.

```yml
name: Build Integrate UI Container
on:
  push:
    branches: [ "main" ]
    tags: [ 'v*.*.*' ]
env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}
```


---
### Job Setup

`jobs:` Initial base step (always here).  
`build:` Setup the build environment and location.  
`runs-on:` This is used to tell the action where you want it to run. `dev` is a tag for my local ubunut runner, I wanted to keep my actions running at home, as at a time I was initiating docker run commands on remote network connected machines.  
`permissions:` These are base permissions setup by the template selected at the start.  

```yml
jobs:
  build:
    runs-on: dev
    permissions:
      contents: read
      packages: write
      id-token: write
```

---
### Action Steps

`steps:` Container which holds all of the steps necessary to perform your action.  
`name:` Step name displayed in the action output.  
`uses:` This is the action (predefined) which is run.  
`with:` Parameters that change the way the specific select action needs to perform its work.  
`if:` Conditional step when the event triggering this job is a certain event it will run this step.  

`login-uuid` this is auto generated at the time of action creation  
`metada-uuid` this is auto generated at the time of action creation  
`build-uuid` this is auto generated at the time of action creation  

```yml
steps:
      - name: Checkout repository
        uses: actions/checkout@v3
    ...
      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: 16
    ...
      - name: Log into registry ${{ env.REGISTRY }}
        if: github.event_name != 'pull_request'
        uses: docker/login-action@<login-uuid>
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
    ...
      - name: Extract Docker metadata
        id: meta
        uses: docker/metadata-action@<metada-uuid>
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
    ...
      - name: Build and push Docker image
        id: build-and-push
        uses: docker/build-push-action@<build-uuid>
        with:
          context: .
          push: ${{ github.event_name != 'pull_request' }}
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```

---
### All Actions - Usage

`uses: actions/checkout@v3` This action does what it says checks out to the agent all of the repository code.  
`uses: actions/setup-node@v3` This action will setup node.js on the agent so you can perform node/npm commands successfully.  
`run: npm install` running npm install from the supplied package.json file in the repository.  
`run: npm run build` build my react website (Typescript).  
`uses: docker/setup-buildx-action@<buildx-uuid>` setup the buildx on the agent. This will give the agent access to build a new docker container.  
`uses: docker/login-action@<login-uuid>` Login to the repository. I use this because these actions are triggered and stored into a private github repository.  
`uses: docker/metadata-action@<metada-uuid>` This action gathers information about the docker container, for tagging and metadata storage.  
`uses: docker/build-push-action@<build-uuid>` Action for building the docker container from the supplied Dockerfile. The dockerfile needs to be in the root of the repository, and needs to reference any and all folders explicitly.

---
### Deploying Containers

To deploy containers first check to make sure you have the container built correctly and showing in your project. The image below shows the packages and container name. Here you can click on the container and see when it was last built.

![](Pasted%20image%2020221204104507.png)

---
#### Container Details

![](Pasted%20image%2020221204111648.png)

With my repository being private the containers are only available when you auth to github.

On your docker host login to ghcr.io as your username. You will need to have generated a PAT token to login which has package read access at a bare minimum. PAT can be generated here https://github.com/settings/tokens. 

![](Pasted%20image%2020221204112026.png)


Docker Login command:
```bash
docker login ghcr.io -u <username>
```

Docker Pull command:

```bash
docker pull ghcr.io/<username|organization>/<repositoty>:main
```


