# 😂 Meme Generator Online | تولید میم آنلاین

یک اپلیکیشن وب با Streamlit که کاربران می‌توانند متن‌های خنده‌دار به دو زبان فارسی و انگلیسی وارد کنند و روی قالب‌های میم محبوب نمایش داده شود.  
کاربر می‌تواند میم‌ها را بلافاصله مشاهده کرده و دانلود کند.

An online web app built with Streamlit where users can input funny text in Farsi and English and render it on popular meme templates.  
Users can instantly view and download their memes.

---

## 🧠 Technologies Used | تکنولوژی‌های استفاده‌شده

- Python 3.10+  
- Streamlit (ساخت وب اپلیکیشن)  
- Pillow (PIL) (پردازش تصویر و نوشتن متن روی تصویر)  
- Requests (دریافت تصاویر آنلاین)  
- Arabic-Reshaper + python-bidi (نمایش درست متن فارسی)  

---

## ⚙️ How It Works | نحوه کار

1. User enters top and bottom text in Persian or English.  
2. App fetches meme templates from online URLs.  
3. Text is processed and reshaped for proper display in Farsi.  
4. Text is drawn on the top and bottom of each meme template.  
5. User can view memes and download them instantly.

مراحل کار:  
1. کاربر متن بالای میم و متن پایین میم را وارد می‌کند.  
2. قالب‌های میم از لینک‌های آنلاین دریافت می‌شوند.  
3. متن فارسی با استفاده از arabic_reshaper و python-bidi آماده می‌شود.  
4. متن روی بالای و پایین تصویر میم رسم می‌شود.  
5. کاربر می‌تواند میم‌ها را مشاهده و دانلود کند.

---

## 🧩 Key Code Structure | ساختار اصلی کد

```python
#User text input
top_text = st.text_input("متن بالای میم / Top Text")
bottom_text = st.text_input("متن پایین میم / Bottom Text")

#List of templates (direct link)
templates = {...}  #Template Dictionary

#Local Persian font
font_path = "fonts/Vazirmatn-Regular.ttf"

#Persian text preparation
def prepare_text(text):
    reshaped_text = arabic_reshaper.reshape(text)
    bidi_text = get_display(reshaped_text)
    return bidi_text

#Meme generation button
if st.button("تولید میم‌ها / Generate Memes"):
    for name, url in templates.items():
        #taking a picture
        response = requests.get(url)
        img = Image.open(io.BytesIO(response.content)).convert("RGB")
        draw = ImageDraw.Draw(img)

        #font size
        font = ImageFont.truetype(font_path, img.width // 15)

        #Drawing text on the image
        draw.text((img.width//2, 10), prepare_text(top_text), font=font,
                  fill="white", anchor="ma", stroke_width=2, stroke_fill="black")
        draw.text((img.width//2, img.height - 10), prepare_text(bottom_text), font=font,
                  fill="white", anchor="ms", stroke_width=2, stroke_fill="black")

        #Display image and download button 
        st.image(img, caption=name, width="stretch")
        buf = io.BytesIO()
        img.save(buf, format="PNG")
        st.download_button("دانلود / Download", buf, file_name=f"meme_{name}.png", mime="image/png")