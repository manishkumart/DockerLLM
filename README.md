# DockerLLM

- [Dockerhub]([https://hub.docker.com/r/joshxt/local-llm/tags](https://hub.docker.com/r/mainshkumar/docker-llm/tags))


Local-LLM is a [llama.cpp](https://github.com/ggerganov/llama.cpp) server in Docker with OpenAI Style Endpoints that allows you to send the model name as the name of the model as it appears in the model list, for example `Mistral-7B-OpenOrca`. It will automatically download the model from Hugging Face if it isn't already downloaded and configure the server for you. It automatically configures the server based on your CPU, RAM, and GPU. It is designed to be as easy as possible to get started with running local models.

## Table of Contents 📖

- [DockerLLM](#local-llm)
  - [Table of Contents 📖](#table-of-contents-)
  - [Run with Docker](#run-with-docker)
    - [Prerequisites](#prerequisites)
    - [Run without NVIDIA GPU support](#run-without-nvidia-gpu-support)
    - [Run with NVIDIA GPU support](#run-with-nvidia-gpu-support)
  - [Run with Docker Compose](#run-with-docker-compose)
    - [Run without NVIDIA GPU support with Docker Compose](#run-without-nvidia-gpu-support-with-docker-compose)
    - [Run with NVIDIA GPU support with Docker Compose](#run-with-nvidia-gpu-support-with-docker-compose)
  - [OpenAI Style Endpoint Usage](#openai-style-endpoint-usage)

## Run with Docker

You can choose to run with Docker or [Docker Compose](DockerCompose.md). Both are not needed. Instructions to run with Docker Compose can be found [here](DockerCompose.md).

Replace the environment variables with your desired settings. Assumptions will be made on all of these values if you choose to accept the defaults.

- `LOCAL_LLM_API_KEY` - The API key to use for the server. If not set, the server will not require an API key.
- `THREADS` - The number of threads to use. Default is `your CPU core count minus 1`.

The following are only applicable to NVIDIA GPUs:

- `GPU_LAYERS` - The number of layers to use on the GPU. Default is `0`.
- `MAIN_GPU` - The GPU to use for the main model. Default is `0`.

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/)

### Run without NVIDIA GPU support

Modify the `THREADS` environment variable to your desired settings. Assumptions will be made on all of these values if you choose to accept the defaults.

```bash
docker pull manishkumart/dockerllm:cpu  
docker run -d --name docker-llm -p 8091:8091 manishkumart/dockerllm:cpu -e THREADS="10" -e LOCAL_LLM_API_KEY="" -v ./models:/app/models
```

### Run with NVIDIA GPU support

If you're using an NVIDIA GPU, you can use the CUDA version of the server. You must have the [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) installed if using NVIDIA GPU.

Modify the `GPU_LAYERS`, `MAIN_GPU`, and `THREADS` environment variables to your desired settings. Assumptions will be made on all of these values if you choose to accept the defaults.

```bash
docker pull manishkumart/dockerllm:cuda
docker run -d --name docker-llm -p 8091:8091 --gpus all manishkumart/dockerllm:cuda -e THREADS="10" -e GPU_LAYERS="20" -e MAIN_GPU="0" -e LOCAL_LLM_API_KEY="" -v ./models:/app/models
```

## Run with Docker Compose

You can choose to run with Docker Compose or Docker. Both are not needed.

Update the `.env` file with your desired settings. Assumptions will be made on all of these values if you choose to accept the defaults.

### Run without NVIDIA GPU support with Docker Compose

```bash
docker-compose pull
docker-compose up
```

### Run with NVIDIA GPU support with Docker Compose

```bash
docker-compose -f docker-compose-cuda.yml pull
docker-compose -f docker-compose-cuda.yml up
```

## OpenAI Style Endpoint Usage

OpenAI Style endpoints available at `http://<YOUR LOCAL IP ADDRESS>:8091/v1` by default. Documentation can be accessed at that <http://localhost:8091> when the server is running. There are examples for each of the endpoints in the [Examples Jupyter Notebook](DockerLLM_Testing.ipynb).


