# AppleMusic导入全量网易云



便捷小脚本
<!--more-->

辣鸡网易云在歌单的曲目设置了1000首的限制,因此如果歌单超过了1k首,就会无法全量导入

只能通过编写`magic`完成

# 安装Nodejs接口

[NeteaseCloudMusicApi](https://github.com/Binaryify/NeteaseCloudMusicApi)]

```bash
git clone git@github.com:Binaryify/NeteaseCloudMusicApi.git 
npm install # 安装依赖
node app.js # 运行
```

# 获取歌单全部歌曲

调用接口: `/playlist/detail`

> **官方解释: **
>
> 说明 : 歌单能看到歌单名字, 但看不到具体歌单内容 , 调用此接口 , 传入歌单 id, 可 以获取对应歌单内的所有的音乐(未登录状态只能获取不完整的歌单,登录后是完整的)，但是返回的 trackIds 是完整的，tracks 则是不完整的，可拿全部 trackIds 请求一次 `song/detail` 接口获取所有歌曲的详情

因此将返回的的`response`数据保存到本地

# 解析歌曲详细内容

调用接口: `/song/detail?ids=`

```python
import urllib
import requests
import json


headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.186 Safari/537.36',
}


def readJson(filename):
    with open(filename, "r", encoding='utf-8') as f:
        loaded_dic = json.load(f)
    return loaded_dic


json_data = readJson('response.json')
id_list = []
for row in json_data['playlist']['trackIds']:
    tmp_id = row['id']
    id_list.append(tmp_id)
print(len(id_list))
unit = len(id_list)/70
pre_count = 0
count = unit
json_shit = None

writer = open('res.txt', "w", encoding='utf-8')
for a in range(0, 69):
    url = 'http://localhost:3000/song/detail?ids='
    test_list = id_list[int(pre_count):int(count)]
    for i, row in enumerate(test_list):
        if len(test_list) > i > 0:
            url = url + ","+str(row)
        else:
            url = url + str(row)
    get_response = requests.get(url, headers=headers, params=None)
    try:
        json_shit = get_response.json()['songs']
    except Exception as e:
        print(get_response)
        print(str(e))

    for row in json_shit:
        try:
            writer.writelines(row['name'] + "-" + row['ar'][0]['name']+'\n')
        except Exception as e:
            print(str(e))
    pre_count = count
    count += unit

writer.close()
```

得到`res.txt`文件

```markdown
My Imotion (Original Mix)-Sashoook
Alone Again (Radio Edit)-Capcha One
Legendary-Epic Soul Factory
Bring Back the Summer (feat. OLY)-Rain Man
Shine-Canvai
One Shot (Typhoon) (Album Edit)-Julian Calor
Invaders (Original Mix)-Hinkik
...
```

# 导入Apple Music

使用歌曲导入网站`Tunemymusic`

[tunemymusic](https://www.tunemymusic.com/zh-cn/)
