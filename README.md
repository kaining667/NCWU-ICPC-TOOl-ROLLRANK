# PTA滚榜使用说明

## 1.前言

​	为了更好的服务于学生，补全华北水利水电大学在算法竞赛方面的一些不足的地方，本人（ChiefNing）在网上学习如何将pta结合于resolver（ICPC官方滚榜工具）。由于无法把pta上的比赛部署至Domjudge（官方滚榜工具所配合的测评平台），因此在配置奖项的时候无法使用套接字进行数据导入，只能使用xml文件来进行滚榜文件的配置。则需要对赛时的“提交记录”进行处理分析，但是pta官方并未给出导出“提交记录”的模块，因此需要第三方工具进行对网页数据的爬取，在这里我推荐使用浙江大学开发的一款爬虫软件EasySpider，该软件上手简单，爬取数据也比较方便，下面会详细介绍该软件。



## 2.对于数据的爬取

​	上面我们提到，需要EasySpider进行对数据的爬取，该软件可以在github上找到，也可也从本目录中找到（但是不是最新的，可能存在一系列bug，在下面我会详细说明其中一个影响比较大的bug的处理方法，先说一下配置一个爬虫项目的步骤。

* 点击EasySpider.exe，进入软件主页面，选择中文之后会有“设计/修改任务选项”，点击进入选择设计模式，这里有三个设计模式，推荐使用”使用纯净版浏览器设计“。

* 进入到任务列表中，点击左上角的”创建新任务“，会进行新任务设计，在这里必须输入pta的登录时候的url，具体操作如下：

  ​	随便打开一个浏览器进入到pta的主页之下，然后点击”登出“

  ![image-20250331095425539](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20250331095425539.png)

  ​	之后复制url到爬虫软件上即可使用，即下述

  ![image-20250331095544012](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20250331095544012.png)

  或者直接输入`https://pintia.cn/auth/login?`即可，如下图所示：
  
  ![image-20250331161905626](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20250331161905626.png)

之后点击开始设计即可在网站上开始设计。



* 需要输入自己的账号与密码，右键点击文字框，会弹出如右下角所示的界面，之后点击输入文字即可，你需要在其中输入你的账号密码，之后的每一步都是模仿人的行为进行。

![image-20250331162105234](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20250331162105234.png)

* 在输入完之后，点击“登录”，有几率跳转到验证界面，这个验证界面需要人手动进行验证，因此其逻辑是--点击“登录”--等待一定时间（在这个时间内需要真人进行手动验证），如下所示：

  ![image-20250331162610630](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20250331162610630.png)



点击之后会跳转到主界面，此时需要点击下方“鹿头”的这个东西

![image-20250331163019954](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20250331163019954.png)

正常的情况应该如下所示：

![image-20250331163050119](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20250331163050119.png)



此时需要设计点击登录之后需要等待的时间，这段时间用来进行认证，这里我设计了10秒，设计多久都行，要保证自己可以完成验证阶段。

![image-20250331163203903](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20250331163203903.png)

* 之后返回到网站的那个界面，以此按照上述设计方式点击对应的比赛 --> ”管理“ --> ”提交列表“(注意，这里都是需要进行点击配置，不要自己点击了忘记进行配置了！！！！！！！！！！！)

* 现在应该到达了正确的提交列表的界面了，下一步比较关键，需要爬取对应的元素

  * ![](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20250331163912426.png)
  * ![image-20250331164350681](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20250331164350681.png)

  * ![image-20250331164434038](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20250331164434038.png)

    把没用的元素删除，只留下”时间“，"答案结果”，“题目名称”，“提交人”即可

![image-20250331165107714](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20250331165107714.png)

​			最后点击“采集数据”就可以了。

![image-20250331165241171](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20250331165241171.png)

​	哦对还有要设计翻页，这个就不多说了。



* 点击完保存，返回至主界面，点击“查看，管理，执行任务“，点击对应的任务信息，最后执行任务就可以了，这里推荐点击本地直接执行（纯净模式）

  ![image-20250331165501602](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20250331165501602.png)



* 注意，执行完的结果会保存在文件目录中的Data文件夹中，并且是csv文件，此时我们需要把csv文件转为xlsx文件，并且按照下述把对应的列起名字，如下所示：

  | 提交时间        | 提交状态 | 题目名称 | 用户id          | 提交语言 | 提交id |
  | --------------- | -------- | -------- | --------------- | -------- | ------ |
  | 2025/3/29 18:09 | 答案错误 | G        | 2002231133 王丽 | c++      | 1      |
  | 2025/3/29 18:09 | 答案错误 | B        | 202301126 徐雅  | c++      | 2      |
  | 2025/3/29 18:09 | 运行超时 | K        | 20222909 张云   | c++      | 3      |
  | 2025/3/29 18:09 | 答案错误 | I        | 202332812 张智  | c++      | 4      |
  | 2025/3/29 18:09 | 答案错误 | G        | 202233614 张冰  | c++      | 5      |

把名字改成SubData就完成了数据采集这一步。



## 3.对于数据的处理

### 3.1文件转化：

需要把xlsx文件转为xml文件，之后导入到solver中进行json的配置

```python
import os
import pandas as pd
from lxml import etree
from datetime import datetime

# **************function**************** #
# 转化成xml的提交状态
def 提交状态(status_str):
    if status_str == "答案正确":
        return "AC"
    if status_str == "编译错误":
        return "CE"
    return "WA"

# 获取指定时间的时间戳
def 时间戳(date_string):
    # 定义日期时间的格式
    date_format = "%Y-%m-%d %H:%M:%S"

    # 将日期字符串转换为datetime对象
    dt_object = datetime.strptime(date_string, date_format)

    # 将datetime对象转换为时间戳
    timestamp = dt_object.timestamp()
    return timestamp

new_root = etree.Element('contest')
# 方便模块化写入xml
def write_xml(child_elem_str, cfg_list, val_list):
    elem = etree.SubElement(new_root, child_elem_str)
    for i in range(len(cfg_list)):
        etree.SubElement(elem, cfg_list[i]).text = val_list[i]

# ********************************** #


# **************参数设置**************** #
data_path = "SubData.xlsx" # oj导出来的提交数据
start_time = "2025-03-29 14:10:00" # 比赛开始时间
end_time = "2025-03-29 18:10:00"   # 比赛结束时间
continue_time = "4:00:00"          # 比赛时长
fz_size = "01:30:00"                # 封榜时长
problem_num = 13                   # 题目数量
题目名称 = {                        # 题目名称对应id
    'A': "1",
    'B': "2",
    'C': "3",
    'D': "4",
    'E': "5",
    'F': "6",
    'G': "7",
    'H': "8",
    'I': "9",
    'J': "10",
    'K': "11",
    'L': "12",
    'M': "13"
}
num = 0
# ************************************** #

# info
def set_info():
    info_elem = etree.SubElement(new_root, 'info')

    etree.SubElement(info_elem, 'length').text = continue_time
    etree.SubElement(info_elem, 'penalty').text = "20"
    etree.SubElement(info_elem, 'started').text = "False"
    etree.SubElement(info_elem, 'starttime').text = str(时间戳(start_time))
    etree.SubElement(info_elem, 'title').text = "2025年华北水利水电大学ACM校赛"
    etree.SubElement(info_elem, 'short-title').text = "2025年华北水利水电大学ACM校赛"
    etree.SubElement(info_elem, 'scoreboard-freeze-length').text = fz_size
    etree.SubElement(info_elem, 'contest-id').text = "default--3"


# region
def set_region():
    write_xml(
        "region",
        ["external-id", "name"],
        ["1", "ncwu"]
    )


# judgement
def set_judgement():
    write_xml(
        "judgement",
        ["id", "acronym", "name", "solved", "penalty"],
        ["1", "AC", "Yes", "true", "false"]
    )
    write_xml(
        "judgement",
        ["id", "acronym", "name", "solved", "penalty"],
        ["2", "WA", "No - Wrong Answer", "false", "true"]
    )
    write_xml(
        "judgement",
        ["id", "acronym", "name", "solved", "penalty"],
        ["3", "CE", "No - Compile Error", "false", "false"]
    )


# language
def set_language():
    write_xml(
        "language",
        ["id", "name"],
        ["1", "c++"]
    )

# problem
def set_problem():
    for i in range(problem_num):
        write_xml(
            "problem",
            ["id", "letter", "name"],
            [str(i + 1), chr(ord('A') + i), chr(ord('A') + i)]
        )


# team
def set_team():
    global num
    name = dict()
    ok = 0
    try:
        df = pd.read_excel("SubData.xls")
        ok = 1
    except:
        pass
    if ok:
        for index, row in df.iterrows():
            name[str(row['用户ID'])] = str(row['昵称'])

    df = pd.read_excel(data_path)
    # 假设 '用户id' 是用户 ID 列
    df_unique_sorted = df['用户id'].drop_duplicates().sort_values()
    if not ok:
        for value in df_unique_sorted:
            name[str(value)] = str(value)
    # 如果想要每个字典是 {'user_id': value} 的形式
    user_dict_list = [{'user_id': value, 'name': name[str(value)]} for value in df_unique_sorted]

    # 遍历每个 <user> 元素
    for idx, user_elem in enumerate(user_dict_list, start=1):
        print(user_elem['name'])
        write_xml(
            "team",
            ["id", "external-id", "region", "name", "university"],
            [str(idx), "1", "ncwu", user_elem['name'], "ncwu"]
        )
    num = len(user_dict_list) # 为最后一项设置获奖做准备


# run
def set_run():
    df = pd.read_excel(data_path)
    team = list(set([row['用户id'] for index, row in df.iterrows()]))
    team.sort()
    i = 1
    teamlist = dict()
    for u in team:
        teamlist[u] = str(i)
        i = i + 1
    ok = 1
    # 遍历每一行数据，并将其转换为XML的子节点
    for index, row in df.iterrows():
        # try:
            write_xml(
                "run",
                ["id", "judged", "language", "problem", "status", "team", "time", "timestamp", "result"],
                [str(index + 1), "True", "c++", 题目名称[str(row["题目名称"])], "done", teamlist[row['用户id']],
                str(max(0, int(时间戳(str(row['提交时间'])) - 时间戳(start_time)))), str(时间戳(str(row['提交时间']))),
                提交状态(str(row['提交状态']))]
            )
        # except:
        #     if ok:
        #         print(row)
        #         ok = 0


# finalized
def set_finalized():
    write_xml(
        "finalized",
        ["last_gold", "last_silver", "last_bronze", "time", "timestamp"],
        [str(int(num * 0.1)), str(int(num * 0.25)), str(int(num * 0.45)), "0", str(时间戳(end_time))]
    )


if __name__ == "__main__":
    set_info()
    set_region()
    set_judgement()
    set_language()
    set_problem()
    set_team()
    set_run()
    set_finalized()
    new_tree = etree.ElementTree(new_root)
    new_tree.write('events.xml', encoding='utf-8', xml_declaration=True, pretty_print=True)
```

这个就不多对其进行说明，大家看注释就行了。注意，一定要匹配到目标项，并且把需要变化的数据全部修改！！！

### 3.2 对于xml文件的处理

打开solver的目录，找到awards，对于win是.bat，mac是.sh

![image-20250331170712406](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20250331170712406.png)



选择disk，然后选择对应的文件

![image-20250331170858231](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20250331170858231.png)



打开之后是这个界面

![image-20250331171134052](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20250331171134052.png)

直接点击save，其他自己配置！！！！

### 3.3 配置json文件

#### 3.3.1 配置首刀

```json
{"type":"awards","id":"icpc6795","op":"create","data":{"id":"first-to-solve-A","citation":"First to solve problem A","team_ids":["7"],"display_mode":"pause"}}
```

注意：

* id要唯一，可以不连续

* team_ids是刚才界面的id

* 如果该题为被写出来则

  ```xml
  {"type":"awards","id":"icpc6806","op":"create","data":{"id":"first-to-solve-L","team_ids":[]}}
  ```

  

#### 3.1.2 配置金银铜

```
{"type":"awards","id":"icpc6808","op":"create","data":{"id":"gold-medal","citation":"Gold Medalist","team_ids":[]}}
{"type":"awards","id":"icpc6809","op":"create","data":{"id":"silver-medal","citation":"Silver Medalist","team_ids":[],"display_mode":"list"}}
{"type":"awards","id":"icpc6810","op":"create","data":{"id":"bronze-medal","citation":"Bronze Medalist","team_ids":[],"display_mode":"list"}}
```

注意：

* team_ids没有顺序，但是要包含所有的对应牌子的id
* display_mode如果不写就是默认的暂停展示个人，如果是`"display_mode":"list"`就是不显示个人，显示对应牌子的所有人，在滚完之后显示



#### 3.1.3 配置其他

```xml
{"type":"awards","id":"icpc6794","op":"create","data":{"id":"rank-1","citation":"1st place","team_ids":[""]}}
{"type":"awards","id":"icpc6794","op":"create","data":{"id":"rank-2","citation":"2nd place","team_ids":[""]}}
{"type":"awards","id":"icpc6794","op":"create","data":{"id":"rank-3","citation":"3rd place","team_ids":[""]}}
```

这里举一个例子，其他可以举一反三



## 4. 项目启动

打开resolver的目录，点开终端，输入

```cmd
./resolver.bat c:\events.json --singleStep 999
```

注意：我把json文件放到c盘的根目录了，其他举一反三，一般来说输入这行命令就行了，其他的没太大必要，但是我也稍微介绍一下：

```cmd
键盘快捷键：
Ctrl-Q - 退出
r - 倒回
0 - 重新开始（跳转到开头）
2 - 快进（无延迟地跳进一步）
1 - 快退（无延迟地倒退一步）
+/ 上箭头 - 加速（减少解析延迟）
-/ 下箭头 - 减速（增加解析延迟）
j - 重置解析速度
p - 暂停 / 恢复滚动
i - 切换额外信息显示状态
```

```cmd
常规选项：
--info
向展示客户端显示额外信息。
--speed 速度系数
解析延迟倍数。例如，0.5 表示速度会加快一倍，2 则表示速度会减慢一倍。
--singleStep 起始行
从特定行开始，每一步都需要点击操作；若未指定行，则针对整个竞赛都需要如此操作。
--rowDisplayOffset 行数
将屏幕显示向上移动指定的行数（默认是 4 行）。
--display #
使用指定的显示屏。
1 = 主显示屏，2 = 次显示屏，依此类推。
--multi-display p@wxh
将展示内容扩展到多个客户端。使用 “2@3x2” 表示此客户端在 3×2 的网格中处于第 2 个位置（顶部中间）。
--display_name 模板
使用模板更改团队显示的方式。参数：{团队。显示名称}、{团队。名称}、{组织。正式名称} 以及 {组织。名称}。
--groups
仅解析给定正则表达式模式中对应 ID 的分组。如果给出了多个分组，则分别对每个分组进行解析。
--pause #
从指定的暂停点开始。对测试 / 预览很有用。
--judgeQueue
使用评审队列开始解析。必须至少有一个奖项列表。
--test
在未完成的竞赛上进行测试。忽略（移除）所有未评审的运行记录。
--light
使用浅色模式。
--help
显示此帮助信息。
--version
显示版本信息。
```

注意：如果出现乱码，请查scdn，这里不赘述



## 5. 写在最后

该项目是2022届学生邢凯宁所整理，仅供华北水利水电大学人员浏览使用，作者联系方式QQ：769472984。