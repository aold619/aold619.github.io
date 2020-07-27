---
title: "用PIL生成封面"
date: 2020-07-25T19:12:19+08:00
---

### 题目

Python 题目，要求给指定的图片按照指定的格式合成文字，并且只能使用 PIL 和标准库。

模板一：
  * 尺寸：封面宽度 80%，高度 20%
  * 位置：垂直居中封面，水平起始于封面高度 60% 处

模板二：
  * 尺寸：
  * 位置：


#### 思路

图片长宽已知，字体长宽已知，文字区域大小固定，根据以上条件可以计算出文字的排列，并修行数和改字体大小。

```python
def make_font(text, top_left, buttom_right):
    x_top_left, y_top_left = top_left
    x_buttom_right, y_buttom_right = buttom_right
    text_area_width = x_buttom_right - x_top_left
    text_area_height = y_buttom_right - y_top_left
    max_font_size = text_area_width if text_area_width < text_area_height else text_area_height
    max_len = text_area_width if text_area_width > text_area_height else text_area_height

    font_size = max_font_size
    lines = 1
    while (max_len < font_size * len(text)):
        font_size = font_size - (max_font_size / (lines + 1) / 2)
        if max_len < font_size * len(text):
            font_size = font_size - (max_font_size / (lines + 1) / 2)
            lines += 1

    font = ImageFont.truetype("FONT.TTF", font_size)
    return font
```

模板一：

```python
def template_1(text):
    """
    尺寸：封面宽度 80%，高度 20%
    位置：垂直居中封面，水平起始于封面高度 60% 处
    字体：FONT.TTF
    颜色：#CBDEFF
    版式：垂直左对齐，水平顶端对齐
    """
    img = Image.open("1.png").convert("RGBA")
    x, y = img.size
    text_top_left = (x * 0.1, y * 0.6)
    text_buttom_right = (x * 0.9, y * 0.8)
    ipdb.set_trace()
    font = make_font(text, text_top_left, text_buttom_right)
    img_layout = ImageDraw.Draw(img)
    # img_layout.textsize(text, font=font)
    img_layout.multiline_text(text_top_left, text, font=font, fill="#CBDEFF")

    img.show()
    return img
```
