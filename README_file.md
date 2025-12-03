## 本仓库核心更新
1. api对话结束后，自动删除会话（强迫症）
2. 支持视频+图片，单会话沟通

### 带云上视频聊天对话（需要提前上传视频到类似s3的地方）

```bash
curl --location 'http://127.0.0.1:8000/v1/chat/completions' \
--header 'Authorization: Bearer 3dee725a-75ad-42e0-9a69-c4f514ea2e21' \
--header 'Content-Type: application/json' \
--data '{
  "model": "gemini-3-pro-preview",
  "temperature": 0,
  "stream": true,
  "messages": [
    {
      "role": "user",
      "content": [
        {
          "type": "text",
          "text": "请你完整反推这个视频：\n从镜头语言、运镜方式、分镜节奏、色彩风格、剪辑手法、旁白文案、音效设计、背景音乐情绪等方面进行1:1拆解。\n然后基于同样风格，写一段全新的10秒视频生成提示词（适合Sora / Runway / Luma / Kling使用），要求极度详细、可直接复制生成。\n请按以下结构输出：\n1. 视频完整拆解（带时间戳）\n2. 1:1复刻分镜脚本\n3. 全新10秒视频的终极Prompt（中英文双语）"
        },
        {
          "type": "file",
          "file_url": "https://video-r2.cgssoccer.com/lipsync_auto/user-CtuVfK3pCH0zsgj1ww4vg8JP_gen_01kadvq2w6f9q82jk3btt2q5zz.mp4"
        }
      ]
    }
  ]
}'
```

### 生成图片 or 视频

```bash
curl --location 'http://127.0.0.1:8000/v1/chat/completions' \
--header 'Authorization: Bearer 3dee725a-75ad-42e0-9a69-c4f514ea2e21' \
--header 'Content-Type: application/json' \
--data '{
    "model": "gemini-3-pro-preview",
    "messages": [
      {
        "role": "user",
        "content": "視覺風格: 超現實主義，電影感，8K畫質，由Unreal Engine 5渲染。畫面充滿神秘和莊嚴的氣氛。\n主體與情感: 一個巨大、發光的金色蓮花座懸浮在黑暗的宇宙空間中。蓮花的花瓣是由鋒利、冰冷的金屬柵欄構成的，形成一個華麗但無可逃脫的鳥籠。一個模糊的人影被困在蓮花籠的中心，表情迷茫而無助。\n構圖與焦點: 特寫鏡頭，聚焦於蓮花牢籠的中心。構圖遵循三分法，視覺焦點是人影的迷茫與蓮花的華麗之間的矛盾感。\n封面文字（**简体中文**）: 靈性陷阱\n字體風格: 巨大、醒目的白色書法字體，略帶破碎或飛濺的墨跡效果，疊加在畫面的正中央，帶有輕微的發光外描邊，以確保在任何背景下都清晰可見。\n燈光與色彩: 強烈的戲劇性照明。背景是深邃的暗紫色和黑色，只有蓮花牢籠本身發出強烈的、溫暖的金色光芒，光線從內部透出，形成高飽和度的光影對比。"
      }
    ],
    "stream": false
  }'
  

```
```markdown
{
    "choices": [
        {
            "finish_reason": "stop",
            "index": 0,
            "message": {
                "content": [
                    {
                        "image_url": {
                            "url": "https://upload.xxxx/file/1764746092905_image_1764746075128319.png"
                        },
                        "type": "image_url"
                    }
                ],
                "role": "assistant"
            }
        }
    ],
    "created": 1764746107,
    "id": "chatcmpl-42b1661f",
    "model": "gemini-3-pro-preview",
    "object": "chat.completion",
    "usage": {
        "completion_tokens": 0,
        "prompt_tokens": 364,
        "total_tokens": 364
    }
}
```

#### 方式2：内联 base64 图片（自动上传）

**OpenAI 标准格式**

```bash
curl -X POST http://127.0.0.1:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gemini-enterprise",
    "messages": [
      {
        "role": "user",
        "content": [
          {"type": "text", "text": "描述这张图片"},
          {"type": "image_url", "image_url": {"url": "data:image/png;base64,..."}}
        ]
      }
    ]
  }'
```
