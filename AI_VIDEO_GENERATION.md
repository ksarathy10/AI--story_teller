# AI Video Generation Feature 🎥✨

## Overview

This feature generates realistic animated videos from your story using AI text-to-video models. Instead of just static emoji-based animations, you can now create actual video scenes that visualize your story!

## How It Works

### Technology Stack

**Video Generation Models:**
- **Primary**: Zeroscope v2 (576w) - Open-source text-to-video model
- **Fallback**: Simple image sequence generator (for CPU-only systems)
- **Platform**: HuggingFace Diffusers library

### Process Flow

1. **Story Generation** → Your story is created using BLIP + Gemini
2. **Scene Extraction** → Story is split into scenes (paragraphs)
3. **Video Generation** → Each scene is converted to a short video clip (2-3 seconds)
4. **Scene Enhancement** → AI adds cinematic keywords based on story content
5. **Video Export** → MP4 videos ready to view/download

## Features

### ✨ What Gets Generated

- **Multiple Video Scenes**: Up to 5 scenes from your story
- **Cinematic Quality**: AI-enhanced prompts for better visuals
- **Scene-Specific Styling**: 
  - Children's content → Colorful, cheerful style
  - Night scenes → Moonlit, atmospheric
  - Nature scenes → Lush, detailed environments

### 📊 Technical Details

**Video Specifications:**
- Resolution: 576x320 (optimized for speed)
- Frame Rate: 8 FPS
- Duration: ~3 seconds per scene
- Format: MP4 (H.264)
- File Size: ~2-5 MB per video

**Generation Time:**
- GPU: ~30-60 seconds per scene
- CPU: ~2-5 minutes per scene (using fallback)

## Installation & Setup

### 1. Install Dependencies

```bash
cd backend
pip install diffusers>=0.25.0 imageio>=2.31.0 imageio-ffmpeg>=0.4.9
```

### 2. System Requirements

**For GPU Acceleration (Recommended):**
- NVIDIA GPU with 6GB+ VRAM
- CUDA 11.8 or higher
- 16GB+ RAM

**For CPU (Fallback Mode):**
- 8GB+ RAM
- Slower generation times
- Uses simple image sequence generator

### 3. Model Download

The Zeroscope model (~10GB) will auto-download on first use:
- Model: `cerspense/zeroscope_v2_576w`
- Storage Location: `~/.cache/huggingface/`
- First generation: ~5-10 minutes (model download)
- Subsequent: 30-60 seconds per scene

## Usage

### Via Web Interface

1. **Generate Your Story**:
   - Upload an image OR enter text OR use voice input
   - Select language and age category
   - Click "Generate Story"

2. **Create AI Video**:
   - After story appears, click "Generate AI Video Animation"
   - Wait for processing (shows progress)
   - Videos appear in grid below

3. **View & Download**:
   - Play videos directly in browser
   - Download individual scenes
   - Share on social media

### Via API

```python
import requests

# Generate video from story
response = requests.post('http://localhost:8000/generate-video', json={
    'caption': 'Your story text here...'
})

videos = response.json()['videos']
for video in videos:
    print(f"Scene {video['scene_number']}: {video['video_url']}")
```

## API Reference

### POST `/generate-video`

Generate animated videos from story text.

**Request Body:**
```json
{
    "caption": "Once upon a time, there was a little girl named Maya..."
}
```

**Response:**
```json
{
    "videos": [
        {
            "scene_number": 1,
            "scene_text": "Once upon a time, there was a little girl named Maya.",
            "video_url": "http://localhost:8000/videos/{uuid}/scene_1.mp4"
        }
    ],
    "total_scenes": 5,
    "message": "Generated 5 animated videos"
}
```

## Prompt Engineering

The system automatically enhances your story text for better video generation:

### Base Enhancement
```
"cinematic, high quality, beautiful animation, {your_scene_text}"
```

### Content-Based Additions

| Story Content | Enhancement |
|---------------|-------------|
| Children/Kids | ", colorful, cheerful, children's book style" |
| Night/Dark/Moon | ", night time, moonlit, atmospheric" |
| Forest/Trees/Nature | ", lush nature, detailed environment" |

### Example Transformation

**Original Scene:**
> "Maya discovered a magical tree in the forest."

**Enhanced Prompt:**
> "cinematic, high quality, beautiful animation, Maya discovered a magical tree in the forest, lush nature, detailed environment"

## Performance Optimization

### Speed vs Quality Tradeoffs

**Fast Mode (Default):**
```python
num_frames = 24           # 3 seconds at 8fps
num_inference_steps = 20  # Lower quality, faster
```

**High Quality Mode:**
```python
num_frames = 40           # 5 seconds at 8fps
num_inference_steps = 50  # Higher quality, slower
```

### GPU Memory Management

The system uses:
- `enable_model_cpu_offload()` - Moves model parts to CPU when not in use
- `torch.float16` - Half precision for faster computation
- Progressive loading - Only loads model when needed

### CPU Fallback

If no GPU detected, system automatically uses:
- Simple image sequence generator
- Text overlay on colored backgrounds
- Animated text positioning
- Much faster but less realistic

## Troubleshooting

### Common Issues

**1. Out of Memory Error**
```
Solution: Reduce resolution or use CPU fallback
- Lower num_frames to 12-16
- Use height=256, width=448
```

**2. Slow Generation**
```
Solution: Check GPU availability
- Verify CUDA: torch.cuda.is_available()
- Update GPU drivers
- Close other GPU applications
```

**3. Model Download Fails**
```
Solution: Manual download
- Check internet connection
- Verify HuggingFace access
- Use VPN if region-blocked
```

**4. Video Playback Issues**
```
Solution: Check browser compatibility
- Use Chrome/Edge for best support
- Update browser to latest version
- Check video codec support
```

## Limitations

### Current Constraints

1. **Scene Limit**: Maximum 5 scenes per story (to save time)
2. **Resolution**: 576x320 (optimized for speed)
3. **Duration**: 2-3 seconds per scene
4. **Quality**: Dependent on GPU availability
5. **Style**: Limited to Zeroscope model capabilities

### Not Supported Yet

- ❌ Custom character appearances
- ❌ Voice-synced lip movements
- ❌ Scene transitions/merging
- ❌ 4K resolution
- ❌ Longer videos (>5 seconds per scene)
- ❌ Real-time generation

## Future Enhancements

### Planned Features

1. **Advanced Models**:
   - ModelScope integration
   - AnimateDiff for smoother motion
   - Style-specific models

2. **Customization**:
   - Character appearance control
   - Scene camera angles
   - Art style selection (cartoon, realistic, anime)

3. **Performance**:
   - Multi-GPU support
   - Batch processing
   - Progressive streaming

4. **Integration**:
   - TTS audio sync
   - Automatic subtitles
   - Video merging/editing

## Comparison: Emoji vs AI Video

| Feature | Emoji Animation | AI Video |
|---------|----------------|----------|
| **Generation Speed** | Instant | 30-60s per scene |
| **Visual Quality** | Simple/Cute | Realistic/Cinematic |
| **Customization** | Limited | High |
| **File Size** | None (CSS) | 2-5 MB per scene |
| **System Requirements** | Any browser | GPU recommended |
| **Internet Required** | No | First use (model download) |
| **Age Suitability** | All ages | All ages |
| **Shareability** | Screenshots only | Video files |

## Best Practices

### For Best Results

1. **Story Quality**:
   - Clear, descriptive scenes
   - Visual elements mentioned explicitly
   - Action-oriented descriptions

2. **Scene Selection**:
   - Choose most visual scenes
   - Avoid abstract concepts
   - Focus on concrete imagery

3. **Resource Management**:
   - Generate videos during low-usage times
   - Close other applications
   - Monitor GPU temperature

4. **User Experience**:
   - Show generation progress
   - Provide estimated completion time
   - Allow cancellation

## Examples

### Example 1: Children's Story (Age 5-12)

**Story Scene:**
> "Maya found a colorful butterfly in the garden. It had bright blue and yellow wings!"

**Generated Video**:
- Vibrant colors
- Cheerful atmosphere
- Smooth butterfly motion
- Garden environment

### Example 2: Teen Story (Age 13-18)

**Story Scene:**
> "Under the moonlight, Alex discovered an ancient mysterious door hidden in the forest."

**Generated Video**:
- Moonlit atmosphere
- Mysterious ambiance
- Detailed forest environment
- Cinematic camera work

### Example 3: Adult Story (Age 18+)

**Story Scene:**
> "The philosopher walked through the misty mountains, contemplating life's deeper meaning."

**Generated Video**:
- Atmospheric fog
- Majestic mountains
- Contemplative mood
- Sophisticated visuals

## Resources

### Documentation
- [Zeroscope Model Card](https://huggingface.co/cerspense/zeroscope_v2_576w)
- [Diffusers Library](https://huggingface.co/docs/diffusers)
- [Text-to-Video Guide](https://huggingface.co/docs/diffusers/using-diffusers/text-to-video)

### Community
- HuggingFace Forums
- GitHub Issues
- Discord Community

## License & Credits

- **Zeroscope Model**: Apache 2.0 License
- **Diffusers Library**: Apache 2.0 License
- **Your Application**: [Your License]

---

**Note**: AI video generation is resource-intensive. For production use, consider:
- Cloud GPU services (AWS, Google Cloud, RunPod)
- Video caching/CDN
- Queue-based processing
- User credit systems
