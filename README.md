# video_effects

Kho hiệu ứng dùng chung cho tính năng "Edit Video" của app Heygen_Auto
(Video_AI_Automation_Pre). Mỗi hiệu ứng là 1 clip animation ngắn nền trong
suốt (WebM, có alpha channel) được AI tạo ra bằng Playwright (render HTML +
CSS + GSAP) rồi encode qua ffmpeg, dùng để chèn lên video tại đúng lúc lời
thoại nói tới nội dung liên quan.

## Cấu trúc

```
video_effects/
├── manifest.json     # Index toàn bộ hiệu ứng: id, tên, từ khóa, thời lượng, đường dẫn file
├── effects/           # Các file clip .webm thực tế
└── README.md
```

## manifest.json

```jsonc
{
  "schemaVersion": 1,
  "effects": [
    {
      "id": "confetti-burst",
      "keywords": ["confetti", "celebration", "success", "achievement"],
      "file": "effects/confetti-burst.webm",
      "durationSec": 2.0,
      "width": 640,
      "height": 340,
      "createdAt": "2026-08-01T00:00:00.000Z",
      "createdBy": "ai"
    }
  ]
}
```

## Quy trình 2 chiều (app <-> kho)

1. Khi cần 1 hiệu ứng cho 1 đoạn video, app tra `manifest.json` (bản cache
   local trước, nếu không có thì `git pull`/tải bản mới nhất từ repo này)
   theo từ khóa.
2. Nếu đã có hiệu ứng khớp: tải file `.webm` tương ứng về, dùng ngay.
3. Nếu chưa có: AI tự tạo hiệu ứng mới (render qua Playwright + GSAP, xuất
   WebM trong suốt), tự kiểm tra kỹ thuật (file hợp lệ, có alpha channel,
   không lỗi khi decode/overlay thử) trước khi commit + push file mới và cập
   nhật `manifest.json` lên repo này, để lần sau (chính máy này hoặc máy
   khác) dùng lại ngay không phải tạo lại.
