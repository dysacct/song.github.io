---
title: Python 爬取番茄小说
date: 2025-07-25 14:55:41
tags:
  - Python
categories:
  - Python
---

### 首先我们先需要准备所需模块

```txt
pip install parsel, html/xml、数据提取库
pip install fontTools, 文字处理模块
pip install ddddocr, 验证码识别模块
pip install brotli, 解压缩数据模块
pip install lxml, html解析模块
pip install tqdm, 进度条模块
pip install requests, 网络请求模块
pip install --upgrade pillow, 图像请求模块
```

### 爬取思路

1. 首先我们先找到对应的小说网站
   > https://fanqienovel.com/page/7075952796322761739

注意：后面的网页数字，这可能是页面唯一变换的地方，可以尝试多个章节的网页对比

2. 通过 F12 查看网页源代码，找到对应的小说内容,通过正则提取出小说内容

3. 通过正则提取出小说章节对应的 ID 号

### 代码

<./fanqie_xiaoshuo.py>

```python
# -*- coding: utf-8 -*- 目前版本不需要
import requests
from tqdm import tqdm
import parsel
import re

font_dic = { 58344: 'd', 58345: '在', 58346: '主', 58347: '特', 58348: '家', 58349: '军', 58350: '然', 58351: '表', 58352: '场', 58353: '4', 58354: '要', 58355: '只', 58356: 'v', 58357: '和', 58359: '6', 58360: '别', 58361: '还', 58362: 'g', 58363: '现', 58364: '儿L', 58365: '岁', 58368: '此', 58369: '象', 58370: '月', 58371: '3', 58372: '出', 58373: '战', 58374: '工', 58375: '相', 58376: 'o', 58377: '男', 58378: '直', 58379: '失', 58380: '世', 58381: 'F', 58382: '都', 58383: '平', 58384: '文', 58385: '什', 58386: 'v', 58387: 'o', 58388: '将', 58389: '真', 58390: 't', 58391: '那', 58392: '当', 58394: '会', 58395: '立', 58396: '些', 58397: 'u', 58398: '是', 58399: '十', 58400: '张', 58401: '学', 58402: '气', 58403: '大', 58404: '爱', 58405: '两', 58406: '命', 58407: '全', 58408: '后', 58409: '东', 58410: '性', 58411: '通', 58412: '被', 58413: '1', 58414: '它', 58415: '乐', 58416: '接', 58417: '而', 58418: '感', 58419: '车', 58420: '山山', 58421: '公', 58422: '了', 58423: '常', 58424: '以', 58425: '何', 58426: '可', 58427: '话', 58428: '先', 58429: 'p', 58430: 'i', 58431: '叫', 58432: '轻', 58433: 'm', 58434: '上', 58435: 'w', 58436: '着', 58437: '变', 58438: '尔', 58439: '快', 58440: 'l', 58441: '个', 58442: '说', 58443: '少', 58444: '色', 58445: '里', 58446: '安', 58447: '花', 58448: '远', 58449: '7', 58450: '难', 58451: '师', 58452: '放', 58453: 't', 58454: '报', 58455: '认', 58456: '面', 58457: '道', 58458: 's', 58460: '克', 58461: '地', 58462: '度', 58463: 'l', 58464: '好', 58465: '机', 58466: 'u', 58467: '民', 58468: '写', 58469: '把', 58470: '万', 58471: '同', 58472: '水', 58473: '新', 58474: '没', 58475: '书', 58476: '电', 58477: '吃', 58478: '像', 58479: '斯', 58480: '5', 58481: '为', 58482: 'y', 58483: '白', 58484: '几', 58485: '日', 58486: '教', 58487: '看', 58488: '但', 58489: '第', 58490: '加', 58491: '候', 58492: '作', 58493: '上', 58494: '拉', 58495: '住', 58496: '有', 58497: '法', 58498: 'r', 58499: '事', 58500: '应', 58501: '位', 58502: '利', 58503: '你', 58504: '声', 58505: '身', 58506: '国', 58507: '问', 58508: '马', 58509: '女', 58510: '他', 58511: 'y', 58512: '比', 58513: '父', 58514: 'x', 58515: 'a', 58516: 'h', 58517: 'n', 58518: 's', 58519: 'x', 58520: '边', 58521: '美', 58522: '对', 58523: '所', 58524: '金', 58525: '活', 58526: '回', 58527: '意', 58528: '到', 58529: 'z', 58530: '从', 58531: 'j', 58532: '知', 58533: '又', 58534: '内', 58535: '因', 58536: '点', 58537: 'q', 58538: '三', 58539: '定', 58540: '8', 58541: 'r', 58542: 'b', 58543: '正', 58544: '或', 58545: '夫', 58546: '向', 58547: '德', 58548: '听', 58549: '更', 58551: '得', 58552: '告', 58553: '并', 58554: '本', 58555: 'q', 58556: '过', 58557: '记', 58558: 'L', 58559: '让', 58560: '打', 58561: 'f', 58562: '人', 58563: '就', 58564: '者', 58565: '去', 58566: '原', 58567: '满', 58568: '体', 58569: '做', 58570: '经', 58571: 'k', 58572: '走', 58573: '如', 58574: '孩', 58575: 'c', 58576: 'g', 58577: '给', 58578: '使', 58579: '物', 58581: '最', 58582: '笑', 58583: '部', 58585: '员', 58586: '等', 58587: '受', 58588: 'k', 58589: '行', 58590: '一', 58591: '条', 58592: '果', 58593: '动', 58594: '光', 58595: '门', 58596: '头', 58597: '见', 58598: '往', 58599: '自', 58600: '解', 58601: '成', 58602: '处', 58603: '天', 58604: '能', 58605: '于', 58606: '名', 58607: '其', 58608: '发', 58609: '总', 58610: '母', 58611: '的', 58612: '死', 58613: '手', 58614: '入', 58615: '路', 58616: '进', 58617: '心', 58618: '来', 58619: 'h', 58620: '时', 58621: '力', 58622: '多', 58623: '开', 58624: '已', 58625: '许', 58626: 'd', 58627: '至', 58628: '由', 58629: '很', 58630: '界', 58631: 'n', 58632: '小', 58633: '与', 58634: 'z', 58635: '想', 58636: '代', 58637: '么', 58638: '分', 58639: '生', 58640: '口', 58641: '再', 58642: '妈', 58643: '望', 58644: '次', 58645: '西', 58646: '风', 58647: '种', 58648: '带', 58649: 'J', 58651: '实', 58652: '情', 58653: '才', 58654: '这', 58656: 'e', 58657: '我', 58658: '神', 58659: '格', 58660: '长', 58661: '觉', 58662: '问', 58663: '年', 58664: '眼', 58665: '无', 58666: '不', 58667: '亲', 58668: '关', 58669: '结', 58670: '0', 58671: '友', 58672: '信', 58673: '下', 58674: '却', 58675: '重', 58676: '己', 58677: '老', 58678: '2', 58679: '音', 58680: '字', 58681: 'm', 58682: '呢', 58683: '明', 58684: '之', 58685: '前', 58686: '高', 58687: 'p', 58688: 'b', 58689: '目', 58690: '太', 58691: 'e', 58692: '9', 58693: '起', 58694: '棱', 58695: '她', 58696: '也', 58697: 'w', 58698: '用', 58699: '方', 58700: '子', 58701: '英', 58702: '每', 58703: '理', 58704: '便', 58705: '四', 58706: '数', 58707: '期', 58708: '中', 58709: 'c', 58710: '外', 58711: '样', 58712: 'a', 58713: '海', 58714: '们', 58715: '任'}

url = "https://fanqienovel.com/page/7075952796322761739"

headers = {
  "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36 Edg/137.0.0.0",
  "Connection": "keep-alive",
}

r = requests.get(url, headers=headers).text

# 利用re正则表达式提取章节ID数据
# ID_list = re.findall('<a href="/reader/(\d+)" class="chapter-item-title" target="_blank">', r)
ID_list = re.findall('"itemId":"(\d+)"', r)
# with open('fanqie.txt', mode='a', encoding='utf-8') as f:
#   f.write(r)
# 获取小说名
title_name = re.findall('"bookName":"(.*?)"', r)[0]
print(f'正在爬取{title_name}请稍等...')
# print(ID_list)

for i in tqdm(ID_list):
  # 获取所有ID章节链接
  ID_url = f'https://fanqienovel.com/reader/{i}?enter_from=page'
  # print(ID_url)
  # 对连接发送请求
  ID_r = requests.get(ID_url, headers=headers)
  # 获取小说章节页面的html代码
  html = ID_r.text
  # 用css选择器取值，但是css不能对字符串取值，所以需要用parsel模块解析后再取值
  selector = parsel.Selector(html)
  # 利用css选择器提取小说标题
  title = selector.css('.muye-reader-title::text').get()
  # 利用css选择器提取小说的文本内容
  text_content = selector.css('.muye-reader-content p::text').getall()
  # print(text_content)
  text = "\n\n".join(text_content)
  # print(text)
  # 定义一个空字符novel
  novel = ''
# 对加密文字进行替换
  for t in text:
    try:
      font = font_dic[ord(t)]
      # print(font, end="")
      novel += font
    except:
      font = t
      novel += font
      # print(font, end="")
  with open(title_name + '.txt', mode='a', encoding='utf-8') as f:
    f.write(title)
    f.write("\n\n")
    f.write(novel)
    f.write("\n\n")
```

<./font.py>

```python
import sys
from io import BytesIO
from PIL import Image, ImageDraw, ImageFont
from fontTools.ttLib import TTFont
import ddddocr
import brotli
try:
    import brotli
except ImportError:
    print("错误：缺少 'brotli' 模块，WOFF2 解码需要此模块。请使用以下命令安装：pip install brotli")
    sys.exit(1)

def font_to_img(_code, font_path):
    """将 Unicode 字符转换为使用指定字体的图像"""
    img_size = 1024
    img = Image.new('1', (img_size, img_size), 255)  # 创建二值图像
    draw = ImageDraw.Draw(img)
    try:
        font = ImageFont.truetype(font_path, int(img_size * 0.7))
    except OSError as e:
        print(f"错误：无法加载字体文件 '{font_path}'：{e}")
        sys.exit(1)

    txt = chr(_code)
    bbox = draw.textbbox((0, 0), txt, font=font)
    x = bbox[2] - bbox[0]
    y = bbox[3] - bbox[1]
    draw.text(((img_size - x) // 2, (img_size - y) // 7), txt, font=font, fill=0)
    return img

def identify_word(font_path):
    """使用 OCR 识别字体文件中的字符并创建映射"""
    font_mapping = {}
    try:
        font = TTFont(font_path)
    except Exception as e:
        print(f"错误：无法处理字体文件 '{font_path}'：{e}")
        sys.exit(1)

    ocr = ddddocr.DdddOcr()  # 初始化 OCR
    for cmap_code, glyph_name in font.getBestCmap().items():
        bytes_io = BytesIO()
        pil = font_to_img(cmap_code, font_path)
        pil.save(bytes_io, format='PNG')
        try:
            word = ocr.classification(bytes_io.getvalue())
            print(f"Unicode: {cmap_code} - Glyph: {glyph_name} - OCR 结果: {word}")
            if word:  # 仅将有效 OCR 结果添加到映射
                font_mapping[cmap_code] = word
        except Exception as e:
            print(f"处理 Unicode {cmap_code} 时出错：{e}")
            continue

    return font_mapping

if __name__ == '__main__':
    font_file = 'dc027189e0ba4cd.woff2'
    mapping = identify_word(font_file)
    print('最终字体映射：', mapping)
```
