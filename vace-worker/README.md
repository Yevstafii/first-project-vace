# VACE Serverless Worker

This image runs the VACE inpainting video workflow on RunPod Serverless with
ComfyUI. Large model files stay on the mounted Network Volume instead of the
container image.

## Required Network Volume layout

```text
/runpod-volume/models/diffusion_models/wan2.1_vace_14B_fp16.safetensors
/runpod-volume/models/text_encoders/umt5_xxl_fp8_e4m3fn_scaled.safetensors
/runpod-volume/models/vae/wan_2.1_vae.safetensors
```

At first execution, the handler also downloads the SAM3 detector checkpoint to
`/runpod-volume/models/checkpoints/`, where it is reused by later workers.

## Input contract

The handler accepts a JSON payload with:

```json
{
  "input": {
    "video_url": "https://example.com/source.mp4",
    "prompt": "red dress"
  }
}
```

It downloads the video, runs the bundled official VACE inpainting workflow and
returns the finished MP4 as base64 in `output.video.data`. The API service
persists that output and exposes it through its job download endpoint.
