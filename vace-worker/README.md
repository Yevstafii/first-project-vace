# VACE Serverless Worker

This image extends RunPod's ComfyUI Serverless worker with video input/output
nodes. Wan VACE model files are not baked into the image. RunPod mounts them
from the Network Volume at `/runpod-volume/models/`.

Required model layout:

```text
/runpod-volume/models/diffusion_models/wan2.1_vace_14B_fp16.safetensors
/runpod-volume/models/text_encoders/umt5_xxl_fp8_e4m3fn_scaled.safetensors
/runpod-volume/models/vae/wan_2.1_vae.safetensors
```

The API workflow will be exported from ComfyUI after the first interactive VACE
validation, then stored beside this Dockerfile before production deployment.

