# -
import re
import requests

def save_content(res,path):     # 保存内容
    with open(path, 'wb') as textfile:     # 爬取的内容保存为文本形式
        # textfile.write(bytes(text, "utf-8"))
        textfile.write(res.content)

def parse_str(text_str,target_str):       # 利用正则表达式找到爬取内容中的相应字符串
    urls = re.findall(target_str, text_str, re.S)  # re.S 把文本信息转换成1行匹配
    return urls
#字母前加r表示raw string，也叫原始字符串常量。阻止了转义字符的作用

if __name__ == '__main__':
    video_path = 'C:\\Users\\Charlescai\\Desktop\\video.mp4'
    txt_path = 'C:\\Users\\Charlescai\\Desktop\\website_address.txt'
    target_url = 'http://'  # 要爬取的网站

    target_str1 = r'class="items".*?href="(.*?)"'   # 第一个str
    target_str2 = r'id="media".*?src="(.*?)"'   # 第二个str
    text1 = requests.get(target_url).text
    save_content(requests.get(target_url), txt_path)
    urls = parse_str(text1, target_str1)
    text2 = requests.get(urls[5]).text
    mp4_urls = parse_str(text2, target_str2)
    video = requests.get(mp4_urls[0])
    save_content(video, video_path)
