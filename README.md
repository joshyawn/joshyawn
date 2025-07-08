<a href="https://www.linkedin.com/in/joshua-yawn-3620aa288">Joshua Yawn</a>'s IT and Cybersecurity Project Portfolio 🔐

I'm passionate about cybersecurity and I love tackling complex challenges through hands-on projects. From vulnerability management to threat detection, these projects allow me to dive deep into the ever-evolving landscape of cybersecurity. Please feel free to check them out and see the work I’ve put into enhancing security operations and processes!


## ⚠️ Vulnerability Management Projects

- **[Vulnerability Management Program Implementation](https://github.com/joshcybertest/vulnerability-management-program)**
- **[Programmatic Vulnerability Remediations (PowerShell and BASH)](https://github.com/joshcybertest/programmatic-vulnerability-remediations)**

## 🚨 Threat Hunting and Security Operations

- **[Threat Hunting Scenario (Tor Browser Usage)](https://github.com/joshyawn/threat-hunting-scenario-tor)**
- **[Threat Hunting Scenario (The Credential Stuffing Nightmare)](https://github.com/joshyawn/Threat-Hunting-Scenario-The-Credential-Stuffing-Nightmare/tree/main)**

<hr/>

## 🤳 Connect With Me

[<img align="left" alt="___________ | YouTube" width="22px" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/youtube.svg" />][youtube]
[<img align="left" alt="___________ | Twitter" width="22px" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/twitter.svg" />][twitter]
[<img align="left" alt="___________ | LinkedIn" width="22px" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/linkedin.svg" />][linkedin]
[<img align="left" alt="___________ | Instagram" width="22px" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/instagram.svg" />][instagram]

[twitter]: https://twitter.com/___________
[youtube]: https://www.youtube.com/c/___________
[instagram]: https://www.instagram.com/___________
[linkedin]: https://linkedin.com/in/joshua-yawn-3620aa288

<!--
<img width="35" alt="image" src="https://github.com/user-attachments/assets/2f41c7cd-5ea8-4475-b451-a37161b6c3fb"> 
<img width="35" alt="image" src="https://github.com/user-attachments/assets/77649969-9910-4994-8b96-74a116cfb2a8">
-->
#!/usr/bin/env python3
"""
Generate header.gif – a smooth-scrolling banner for a GitHub README.

▶  Requirements
    pip install pillow
"""

from PIL import Image, ImageDraw, ImageFont
import math

# ──────────────────────────────────────────────────────────────
# Customize these values
WIDTH, HEIGHT   = 1200, 160                 # overall GIF size
BG_COLOR        = "#0d1117"                 # GitHub dark-mode background
FG_COLOR        = "#ffffff"                 # text color
TEXT            = "Joshua Yawn  |  Cybersecurity Engineer  "  # pad with spaces!
FONT_PATH       = "DejaVuSans-Bold.ttf"     # add to repo or use any .ttf
FONT_SIZE       = 56
FPS             = 50                        # frames per second
SCROLL_SPEED    = 2                         # pixels/frame
# ──────────────────────────────────────────────────────────────

font   = ImageFont.truetype(FONT_PATH, FONT_SIZE)
text_w = font.getlength(TEXT)               # true pixel width of the string
frames = []
total_frames = math.ceil((text_w + WIDTH) / SCROLL_SPEED)

for i in range(total_frames):
    img = Image.new("RGB", (WIDTH, HEIGHT), BG_COLOR)
    draw = ImageDraw.Draw(img)

    # Draw TEXT twice, side-by-side, so the loop is seamless
    x1 = WIDTH - i * SCROLL_SPEED
    x2 = x1 + text_w
    y  = (HEIGHT - FONT_SIZE) // 2          # vertically center text

    draw.text((x1, y), TEXT, font=font, fill=FG_COLOR)
    draw.text((x2, y), TEXT, font=font, fill=FG_COLOR)

    frames.append(img)

# Save as looping GIF
frames[0].save(
    "header.gif",
    save_all=True,
    append_images=frames[1:],
    duration=int(1000 / FPS),               # ms per frame
    loop=0
)

print("✅  header.gif generated")
name: Regenerate animated header

on:
  workflow_dispatch:          # run manually
  schedule:
    - cron: '0 0 * * 1'       # every Monday at 00:00 UTC

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.x'

      - name: Install Pillow
        run: pip install pillow

      - name: Generate header.gif
        run: python generate_header.py

      - name: Commit & push if GIF changed
        run: |
          git config user.name  "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add header.gif
          git diff --cached --quiet || git commit -m "🔄 Update animated header"
          git push
