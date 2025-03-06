# Setting up NVIDIA Triton Locally

- Assumptions: Drivers all installed (e.g. NVIDIA GPU, container toolkit for linux based)

1. Pull the Triton image: docker pull nvcr.io/nvidia/tritonserver:25.01-vllm-python-py3
2. Setup the model repository
	- model folder name has to match what is defined in the config.pbtxt. It is also what will be used in the generate api
3. Run the container: docker run --gpus all -it --rm -p 8001:8001 -p 8000:8000 --shm-size=2G --ulimit memlock=-1 --ulimit stack=67108864 --name tritonTestServer -v ${PWD}:/work -w /work nvcr.io/nvidia/tritonserver:25.01-vllm-python-py3 tritonserver --model-store ./model_repository

# Learnings

- Triton and its vLLM engine are able to serve models saved locally. This can be achieved by pointing the model in model.json to the full path of where the model is saved locally. Taking note of where the working directory is set when the docker run is executed. This was tested but not shown in the files uploaded
- If using --network host for docker desktop in windows. Do remember to enable the settings if not the ports would not be open and the triton server can only be accessed from another container.