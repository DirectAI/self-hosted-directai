# Self-Hosted DirectAI

**WARNING: THIS CODE IS NOT AUTHORIZED FOR USE BY A THIRD PARTY WITHOUT A LICENSE FROM DIRECTAI**

[DirectAI Self-Hosted EULA](https://docs.google.com/document/d/1sYmDYExFCIvMqo9ImbZW1GnhbQq8y6W-w_0VkJ6f_l4/edit) is linked for your convenience.

### Startup
We expect DirectAI's services to be run on an Ubuntu machine with access to an Nvidia GPU. See `Deep Learning OSS Nvidia Driver AMI GPU PyTorch 2.1.0 (Ubuntu 20.04) 20240326` AMI and `g5.2xlarge`/`g4dn.xlarge` instance types. We recommend allocating **256GB** of disk space.

- Follow [docker install instructions](https://docs.docker.com/engine/install/ubuntu/)
- Install docker-compose: `sudo apt-get update && sudo apt-get install docker-compose`
- Configure NVIDIA runtime for docker: `sudo nvidia-ctk runtime configure --runtime=docker` 
    - If you don't have the correct AMI, you may have to [install the toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) and drivers `sudo apt-get install -y nvidia-driver-535 nvidia-utils-535-server` beforehand. Don't forget to reboot in this case.
- Restart Docker: `sudo systemctl restart docker`
- Authenticate via DirectAI-provided Docker Access Token: `docker login -u directai`
    - Provide the access token when prompted for a password.
    - This is not the access token provided by `https://api.alpha.directai.io/token`. Please reach out to `ben@directai.io` for that token.

### Running DirectAI on your Machine
- Pull the server image: `docker pull directai/directai:server`
- Run the image as a container: `docker-compose up`

### Integration Testing DirectAI on your Machine
- Pull the testing image: `docker pull directai/directai:testing`
- Run the image as a container: `docker-compose -f docker-compose-testing.yml up`
- This will not be necessary in a standard run although DirectAI engineers may ask for the log results in the event of a debug session.

### Notes
- You are able to choose whether telemetry is sent to DirectAI via `ENABLE_TELEMETRY` (environment variable). This is set in `self-hosted-directai/directai_fastapi/.env`. It is `True` by default as it enables easy debugging on our side.
- The majority of the logs are encrypted. If you experience an error, send relevant log files to `ben@directai.io` - the names of those files are printed when you run `docker-compose up`.
- After launching the DirectAI service, you should be able to view the server documentation at `http://localhost:8000/docs`. This includes OpenAPI configurations for all the available endpoints (expected request and response types).
- The service will be ready to stream inference when you see a print statement indicating that a `TrackerPoolWorker` (at least one) is ready for a task.