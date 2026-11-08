# Image to video AI on your own PC: settings and prompts (ToolProof AI, Ep18)

Free, local, no credits. Tested on one RTX 3090 (24 GB), 7 Nov 2026. Your times will differ with your card.

## What you need
- ComfyUI (free, open source) with the built-in LTX-2 and Wan 2.2 image-to-video templates.
- LTX-2 19B distilled (fp8) + Gemma 3 12B text encoder + LTX-2 spatial upscaler x2. Free for businesses under $10M yearly revenue (LTX-2 Community License; check the current wording).
- Wan 2.2 I2V A14B (we used the Q5_K_M GGUF high + low noise files) + the lightx2v 4-step LoRAs + umT5 text encoder + Wan 2.1 VAE. Apache 2.0.
- One product photo: plain background, product centred, good light. The video is only as sharp as the photo.

## Settings that worked
| | LTX-2 distilled | Wan 2.2 |
|---|---|---|
| Template | video_ltx2_i2v_distilled (ComfyUI) | Wan 2.2 I2V + 4-step LoRAs |
| Size | 704x1280 (9:16) or 1280x704 (16:9) | 720x1280 |
| Length | 121 frames @ 25 fps = 4.8 s (153 frames = 6.1 s) | 81 frames @ 16 fps = 5 s |
| Steps | 8 sigmas at half size, x2 upscale, 3 refine sigmas | 4 steps (2 high + 2 low), cfg 1, shift 8 |
| Our time per clip (GPU) | ~3.6 min | ~11 min |
| Our energy per clip (GPU) | ~8 Wh | ~42 Wh |

## Prompts (describe the MOTION, not the product)
1. Vertical ad: "Slow cinematic orbit around the amber glass candle jar on the oak table. The small candle flame flickers gently and a thin line of smoke rises and curls upward. Warm window light. The jar, its label and the wooden lid stay exactly the same. Calm, premium product commercial, shallow depth of field."
2. Wide hero: "Wide product hero shot. The camera slowly pushes in toward the candle jar. The flame flickers softly, warm light drifts across the wall, dust glows in the light. The jar and its label stay exactly the same."
3. 6 s bumper: "The camera holds still, then makes a gentle push-in on the candle jar. The flame grows slightly brighter, a soft warm glow spreads over the table. The label stays sharp and unchanged."
4. Unboxing (Wan 2.2 first + last frame: closed gift box -> candle): "The twine bow on the kraft-paper gift box loosens and the box opens and folds away, revealing the candle jar with a lit flame on the table. Smooth unboxing reveal, static camera, warm window light."
5. Continue a clip: take the last frame of the clip, use it as the next start image, same prompt. Good for about 10 s; by 20 s colours and contrast drift.

Negative prompt: "text changes, warped label, melting letters, misspelled text, extra jars, people, hands, logo, watermark, subtitles, flicker, morphing, blurry"

## Tips from our run
- Keep text on the product big and simple: neither model melted our label in 15 clips.
- LTX-2 tends to push in even when you ask for an orbit. Wan 2.2 moves the camera less.
- Make the photo variants (16:9, gift box) by EDITING the one product photo, not by re-generating it.
- Free still means your electricity and your time: check your own tariff per kWh.

## Workflow files
- `ltx2_distilled_image_to_video_api.json`: LTX-2 19B distilled, 9:16, 121 frames.
- `wan22_image_to_video_api.json`: Wan 2.2 I2V A14B (GGUF) + lightx2v 4-step LoRAs, 81 frames.
- `wan22_start_end_frame_api.json`: Wan 2.2 first + last frame (the unboxing).
They are ComfyUI API-format files: drag one onto ComfyUI, put your photo in ComfyUI/input as `your_product_photo.png` (or pick yours in Load Image), check the model names match your files, then Run.

Video generated with free, local AI models (LTX-2 by Lightricks, Wan 2.2 by Wan-Video) in ComfyUI. Made by ToolProof AI.
