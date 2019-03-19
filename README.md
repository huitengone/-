# -
# 基于Django框架的一段验证码文件
# 登陆验证过程
import sys
from django.http import HttpResponse
from PIL import Image, ImageDraw, ImageFont
import random
import io

def rndColor():
    C1 = random.randrange(0,255)
    C2 = random.randrange(10,255)
    C3 = random.randrange(64,255)
    return (C1,C2,C3)


def verifycode(request):
    # 背景图颜色长度高度
    bgcolor = '#FFF'
    width = 100
    height = 25
    # 创建画布
    im = Image.new('RGB',(width, height), bgcolor)
    # 创建画笔
    draw = ImageDraw.Draw(im)
    # 生成文字
    str1 = "ABCDEFG123HIJ45KLM6NOP78QRS90TUVWXYZ"
    rand_str = ''
    for i in range(0,4):
        rand_str += str1[random.randrange(0, len(str1))]
    font = ImageFont.truetype('/usr/share/fonts/truetype/fonts-japanese-gothic.ttf', 23)
    draw.text((5,2), rand_str, font=font, fill=(random.randrange(0,255),255,random.randrange(0,255)))


    # 生成算式
    # numb_1 = {"1":"壹","2":"贰","3":"叁","4":"肆","5":"伍","6":"陆","7":"柒","8":"捌","9":"玖"}
    # numb_2 = random.randint(1,50)
    # sign = ["+","-"]
    # numb_1_n = random.randrange(1,10)
    # numb_1_s = str(numb_1_n)
    # first_s = numb_1[numb_1_s]
    # thirt_s = str(numb_2)
    # sign_n = random.randrange(0,2)
    # second_s = sign[sign_n]
    # if sign_n == 0:
    #     last = numb_1_n + numb_2
    # else:
    #     last = numb_2 - numb_1_n
    # last_s = str(last)
    # fontcolors = ['yellow', 'green', 'red', 'orange', 'pink']
    # font = ImageFont.truetype('/usr/share/fonts/truetype/fonts-japanese-gothic.ttf', 23)
    #
    # draw.text((5,2), '?', font=font, fill=random.sample(fontcolors,1)[0])
    # draw.text((20,2), second_s, font=font, fill=random.sample(fontcolors,1)[0])
    # draw.text((35,2), first_s, font=font, fill=random.sample(fontcolors,1)[0])
    # draw.text((60,2), '＝', font=font, fill=random.sample(fontcolors,1)[0])
    # draw.text((75,2), last_s, font=font, fill=random.sample(fontcolors,1)[0])


   # 生成点
    for i in range(0,100):
        xy = (random.randrange(0,width), random.randrange(0, height))
        draw.point(xy,fill=rndColor())
    # 生成线
    for i in range(0,5):
        x1 = random.randrange(0, width)
        y1 = random.randrange(0, height)
        x2 = random.randrange(0, width)
        y2 = random.randrange(0, height)
        draw.line((x1,y1,x2,y2), fill=rndColor())
    # 生成圆
    for i in range(0,5):
        x = random.randrange(0, width)
        y = random.randrange(0, height)
        draw.arc((x,y,x+5,y+5),0,360,fill=rndColor())

    request.session['verifycode']=rand_str
    # 文件操作
    buf = io.BytesIO()
    im.save(buf, 'png')
    return HttpResponse(buf.getvalue(), 'image/png')
