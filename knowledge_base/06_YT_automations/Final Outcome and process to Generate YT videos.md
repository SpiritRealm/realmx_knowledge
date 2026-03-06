Step-by-step guide** you can share with your team to set up **local AI image and video generation** on laptops like yours (RTX 4050, 16 GB RAM). I’ve made it **practical, one-to-one, and easy to follow**.

---

# **Local AI Image & Video Generation Guide**

**Goal:** Enable team members to generate high-quality **images (1080p)** and **short videos** locally using **ComfyUI + Stable Diffusion XL**.

---

## **Prerequisites**

|Requirement|Recommended|
|---|---|
|GPU|NVIDIA RTX 4050 (6 GB) or better|
|RAM|16 GB or more|
|OS|Windows / Linux / Mac|
|Disk|20 GB free (for models & output)|

---

# **Step 1: Install the Portable ComfyUI**

1. Go to the official ComfyUI release page:  
    [ComfyUI Releases](https://github.com/comfyanonymous/ComfyUI)
    
2. Download **ComfyUI_windows_portable.zip** (or equivalent for your OS).
    
3. Extract to a folder, e.g.:
    

```
D:\AI\ComfyUI
```

4. Inside, you should see:
    

```
ComfyUI_windows_portable/
 ├─ ComfyUI/
 ├─ models/
 ├─ run_nvidia_gpu.bat
 └─ update/
```

5. Double-click **`run_nvidia_gpu.bat`** to launch ComfyUI.
    
6. Open the browser:
    

```
http://127.0.0.1:8188
```

You now have the **UI ready**.

---

# **Step 2: Download AI Models**

### **A. Stable Diffusion XL (for images & video)**

- Free checkpoints available on **Civitai**: [https://civitai.com/](https://civitai.com/)
    
- Recommended models:
    
    - **Stable Diffusion XL Base 1.0**
        
    - **Juggernaut XL** (high realism)
        
- Place downloaded `.safetensors` or `.ckpt` in:
    

```
ComfyUI/models/checkpoints/
```

---

### **B. Optional Add-ons**

- **LoRA models** for style or cinematic effect → `ComfyUI/models/loras/`
    
- **RealESRGAN** for upscaling → included in ComfyUI Video Nodes
    

---

# **Step 3: Generate High-Quality Images**

1. Launch ComfyUI.
    
2. Open the default workflow.
    
3. Add nodes:
    
    - **Checkpoint Loader** → select your SDXL model
        
    - **CLIP Text Encode** → enter prompt
        
    - **KSampler** → sampling
        
    - **VAE Decode** → latent → image
        
    - **Save Image** → output folder
        
4. **Settings for 1080p output:**
    
    - Width: 1920
        
    - Height: 1080
        
    - Steps: 20–30
        
    - CFG Scale: 7–12
        
5. Example prompt:
    

```
Cinematic cyberpunk city at night, neon reflections, ultra detailed, dramatic lighting, 1080p
```

6. Execute the graph → image appears in `ComfyUI/output/`.
    

---

# **Step 4: Install Video Nodes for ComfyUI**

1. Clone the Video Nodes repo inside ComfyUI folder:
    

```bash
cd ComfyUI
git clone https://github.com/comfyanonymous/ComfyUI_VideoNodes
```

2. Restart ComfyUI → Video nodes now appear.
    

---

# **Step 5: Generate Frame-by-Frame Video**

1. Create a new workflow for video:
    
    - **Checkpoint Loader** → SDXL model
        
    - **Video nodes** → set frames and motion
        
    - **CLIP Text Encode** → prompt
        
    - **KSampler** → sample frames
        
    - **VAE Decode** → latent → image
        
    - **Save Image** → output folder
        
2. **Recommended video settings**:
    
    - Resolution: 1920×1080
        
    - FPS: 15–30
        
    - Frames: 30–60 for 1–2 sec video
        
3. Example prompt:
    

```
Cinematic forest sunrise, camera slowly pans, soft lighting, ultra detailed, 1080p
```

4. Execute the graph → frames saved in `ComfyUI/output/video/frames/`.
    

---

# **Step 6: Compile Frames into Video**

1. Install FFmpeg (free): [https://ffmpeg.org/download.html](https://ffmpeg.org/download.html)
    
2. Open terminal in frames folder:
    

```bash
ffmpeg -r 30 -i frame_%04d.png -c:v libx264 -pix_fmt yuv420p output.mp4
```

- `-r 30` → 30 FPS
    
- `frame_%04d.png` → input frames
    
- `output.mp4` → final video
    

3. Done → your cinematic 1080p video is ready.
    

---

# **Step 7: Optional Enhancements**

|Enhancement|How to apply|
|---|---|
|Upscaling|Add **RealESRGAN node**|
|Face restoration|Add **CodeFormer node**|
|Style / aesthetics|Use **LoRA models** in workflow|
|Motion / camera control|Use **keyframes in video nodes**|
|Reduce flicker|Enable **temporal consistency** in Video nodes|

---

# **Step 8: Tips for Smooth Workflow**

- Start with **short sequences (10 frames)** for testing
    
- Lower sampling steps for VRAM-intensive workflows
    
- Save workflow templates for reuse
    
- Keep output folder organized for images vs videos
    

---

# **Step 9: Recommended Model Setup for RTX 4050**

|Task|Model|
|---|---|
|High-quality images|Stable Diffusion XL / Juggernaut XL|
|Video frames|Same SDXL model|
|Style / cinematic effect|LoRA models|
|Upscale|RealESRGAN|

---

# **Outcome**

- Local AI pipeline for **images (1080p)**
    
- Local AI pipeline for **short cinematic videos**
    
- Fully offline, GPU-accelerated
    
- Easily shareable workflow across your team
    

---

