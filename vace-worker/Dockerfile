# This image contains ComfyUI and the video I/O node required by the VACE workflow.
# Large model weights deliberately stay on the RunPod Network Volume.
FROM runpod/worker-comfyui:5.8.6-base

RUN comfy-node-install https://github.com/Kosinkadink/ComfyUI-VideoHelperSuite

