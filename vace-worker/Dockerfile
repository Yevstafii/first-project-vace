# This image contains the native ComfyUI nodes required by the VACE inpainting
# workflow. Large model weights deliberately stay on the RunPod Network Volume.
FROM runpod/worker-comfyui:5.8.6-base

# SAM3_Detect was added to ComfyUI core after the worker image's original core
# snapshot. Pin a stable core release that includes it rather than relying on a
# moving "latest" checkout.
ARG COMFYUI_VERSION=v0.24.0

RUN git -C /comfyui fetch --depth 1 origin "refs/tags/${COMFYUI_VERSION}" \
    && git -C /comfyui checkout --detach FETCH_HEAD \
    && python -m pip install --no-cache-dir -r /comfyui/requirements.txt

# Keep VHS available for future workflows, while the checked-in first workflow
# uses the native LoadVideo/CreateVideo/SaveVideo nodes from ComfyUI core.
RUN comfy-node-install https://github.com/Kosinkadink/ComfyUI-VideoHelperSuite \
    && if [ -f /comfyui/custom_nodes/ComfyUI-VideoHelperSuite/requirements.txt ]; then \
        python -m pip install --no-cache-dir -r /comfyui/custom_nodes/ComfyUI-VideoHelperSuite/requirements.txt; \
    fi

# VideoHelperSuite imports this package dynamically.  It is not present in the
# base image and without it ComfyUI skips the video node module at startup.
RUN python -m pip install --no-cache-dir imageio-ffmpeg

COPY extra_model_paths.yaml /comfyui/extra_model_paths.yaml
COPY official-vace-inpainting.workflow.json /opt/vace/official-vace-inpainting.workflow.json

# The base worker starts ComfyUI through /start.sh and executes /handler.py.
# Replace only the handler so the standard launch and network-volume support
# remain intact while SaveVideo MP4 output is returned to the API.
COPY handler.py /handler.py

# Fail the image build if a future ComfyUI or VideoHelperSuite update prevents
# the worker from importing the VACE/SAM3 node graph.
RUN cd /comfyui && python main.py --quick-test-for-ci --cpu
