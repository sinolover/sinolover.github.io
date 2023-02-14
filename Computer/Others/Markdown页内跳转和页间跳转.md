# 跳转
## 页内跳转

### 方法一：

```scss
// 使用锚点
[锚点描述](#锚点id)

// 设置锚点
<div id = '锚点id'>锚点文字</div>
```

例子：

1. 此处起跳
```scss
[此处起跳](#1) 
```

2. 此处落地
```scss
<div id = '1'>此处落地</div> 
```

### 方法二：注脚(不那么通用)，两点间可互相跳转

注脚尽量为数字，注脚内容可不写，但英文的冒号不能省略

```less
// 使用注脚
[^注脚]

// 设置注脚
[^注脚]:注脚内容
```

例子

注脚起跳

```less
注脚起跳[^1] //上一行内容
```

```markdown
[^1]:注脚落地 //上一行内容（注脚显示在页面底部）
```


## 页间跳转

### 方法一：

```less
// 使用锚点到网站
[起跳到百度](http://www.baidu.com)

//使用锚点到网站的某个锚点
[起跳到百度](http://www.baidu.com#锚点id)
```

例子(使用锚点到网站)

[起跳到百度](http://www.baidu.com/)

```less
[起跳到百度](http://www.baidu.com) //上一行内容
```

1.  注脚落地 [↩︎](https://www.cnblogs.com/moyutime/p/14300484.html#fnref1)
    
# 插入音视频
## 插入视频
1. 方法一

*   html中的video标签

```html
<!-- mp4格式 -->
<video id="video" controls="" preload="none" poster="封面">
      <source id="mp4" src="mp4格式视频" type="video/mp4">
</videos>

<!-- webm格式 -->
<video id="video" controls="" preload="none" poster="封面">
      <source id="webm" src="webm格式视频" type="video/webm">
</videos>

<!-- ovg格式 -->
<video id="video" controls="" preload="none" poster="封面">
      <source id="ogv" src="ogv格式视频" type="video/ogv">
</videos>
```

2.方法二

*   html中的iframe标签

```html
<iframe 
src="视频或者网页路径" 
scrolling="no" 
border="0" 
frameborder="no" 
framespacing="0" 
allowfullscreen="true" 
height=600 
width=800> 
</iframe>
<!-- 相当于是子网页 -->
<!-- B站分享链接提供 -->
```

3.方法三

*   视频转化为gif格式, 直接当作图片插入markdown中

```html
![](https://gitee.com/turbo-studio/image/raw/master/image/20210215225951.gif)
```

PS:

*   三种方式 各有千秋 选择适当的即可

# markdown如何嵌入视频、音频
## 嵌入本地视频

```
<video src="./test.mp4" width="800px" height="600px" controls="controls"></video>
```

## 嵌入网络视频

```
<video src="https://apd-05d336a1b4772da1e283799cf0c551fa.v.smtcdns.com/vhot2.qqvideo.tc.qq.com/A2MpZpoZp960E9gqArnh3G34Wi_9zUANpgm3UGU29zgM/uwMROfz2r5zEIaQXGdGnC2dfJ6norVr71SyOzMWdO4L-7R5f/o09294fum1s.p701.1.mp4?sdtfrom=v1104&guid=a4106cc96ce1444bd03862a74a82d1b6&vkey=7A77CC2285DCB33DDA884E80CDE79957D9548DE12C9CCE43F39EEB0C01B140E1771ECDA0177F9476DB13D06EDA7B0049E9109BFBAEA1828B3D9E706BBC58BBAFCE7F334627C48533FF03B35EEF0026DD89E21E82C4212743C8B4D4A564E6DAB58B308B8956A6F375E161343E37297896B11B8C7D315AE83FC4ED6DDE0D54357F" width="800px" height="600px" controls="controls"></video>
```

## 嵌入哔哩哔哩(bilibili)视频

```
<iframe src="http://player.bilibili.com/player.html?aid=24931813&cid=42084760&page=1" scrolling="no" width="800px" height="600px" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
```

注：替换视频地址就可以了

  

替换视频地址就可以了.png

## 嵌入音频

```
<iframe name="music" src="http://music.163.com/song/media/outer/url?id=1382359170.mp3" marginwidth="1px" marginheight="20px" width=100% height="80px" frameborder=1 　scrolling="yes">
</iframe>
```

推荐一个获取各大音乐MP3格式的网站：[链接](https://links.jianshu.com/go?to=http%3A%2F%2Fmusic.aitao456.com%2F)

## 嵌入音频网易云音乐

```
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=34341360&auto=1&height=66"></iframe>
```

## 嵌入git动画

```
<iframe height=500 width=500 src="https://i0.hdslb.com/bfs/archive/39047c5cca133d93beb3b5a99713ff6eb589341c.gif"></iframe>
```



# 语法指导

这是一篇讲解如何正确使用 **Markdown** 的排版示例，学会这个很有必要，能让你的文章有更佳清晰的排版。

> 引用文本：Markdown is a text formatting syntax inspired
## 普通内容

这段内容展示了在内容里面一些小的格式，比如：
~~~
- **加粗** - `**加粗**`
- _倾斜_ - `*倾斜*`
- ~~删除线~~ - `~~删除线~~`
- `Code 标记` - `` `Code 标记` ``
- [超级链接](http://github.com) - `[超级链接](http://github.com)`
- [username@gmail.com](mailto:username@gmail.com) - `[username@gmail.com](mailto:username@gmail.com)`
~~~
效果：
- **加粗** - `**加粗**`
- _倾斜_ - `*倾斜*`
- ~~删除线~~ - `~~删除线~~`
- `Code 标记` - `` `Code 标记` ``
- [超级链接](http://github.com) - `[超级链接](http://github.com)`
- [username@gmail.com](mailto:username@gmail.com) - `[username@gmail.com](mailto:username@gmail.com)`

## 提及用户

@foo @bar @someone ... 通过 @ 可以在发帖和回帖里面提及用户，信息提交以后，被提及的用户将会收到系统通知。以便让他来关注这个帖子或回帖。

## 表情符号 Emoji

支持表情符号，你可以用系统默认的 Emoji 符号（无法支持 Windows 用户）。
也可以用图片的表情，输入 `:` 将会出现智能提示。

### 一些表情例子
~~~
:smile: :laughing: :dizzy_face: :sob: :cold_sweat: :sweat_smile: :cry: :triumph: :heart_eyes: :relaxed: :sunglasses: :weary:

:+1: :-1: :100: :clap: :bell: :gift: :question: :bomb: :heart: :coffee: :cyclone: :bow: :kiss: :pray: :sweat_drops: :hankey: :exclamation: :anger:

~~~
效果：
:smile: :laughing: :dizzy_face: :sob: :cold_sweat: :sweat_smile: :cry: :triumph: :heart_eyes: :relaxed: :sunglasses: :weary:

:+1: :-1: :100: :clap: :bell: :gift: :question: :bomb: :heart: :coffee: :cyclone: :bow: :kiss: :pray: :sweat_drops: :hankey: :exclamation: :anger:

## 大标题 - Heading 3

你可以选择使用 H2 至 H6，使用 ##(N) 打头，H1 不能使用，会自动转换成 H2。

> NOTE: 别忘了 # 后面需要有空格！

### Heading 4

#### Heading 5

##### Heading 6

## 图片

```
![alt 文本](http://image-path.png)
![alt 文本](http://image-path.png "图片 Title 值")
![设置图片宽度高度](http://image-path.png =300x200)
![设置图片宽度](http://image-path.png =300x)
![设置图片高度](http://image-path.png =x200)
```


## 代码块

### 普通

```
*emphasize*    **strong**
_emphasize_    __strong__
@a = 1
```

### 语法高亮支持

如果在 \`\`\` 后面更随语言名称，可以有语法高亮的效果哦，比如:

#### 演示 Ruby 代码高亮

```ruby
class PostController < ApplicationController
  def index
    @posts = Post.last_actived.limit(10)
  end
end
```

#### 演示 Rails View 高亮

```erb
<%= @posts.each do |post| %>
<div class="post"></div>
<% end %>
```

#### 演示 YAML 文件

```yml
zh-CN:
  name: 姓名
  age: 年龄
```

> Tip: 语言名称支持下面这些: `ruby`, `python`, `js`, `html`, `erb`, `css`, `coffee`, `bash`, `json`, `yml`, `xml` ...

## 有序、无序列表

### 无序列表
~~~
- Ruby
  - Rails
    - ActiveRecord
- Go
  - Gofmt
  - Revel
- Node.js
  - Koa
  - Express
~~~
效果：

- Ruby
  - Rails
    - ActiveRecord
- Go
  - Gofmt
  - Revel
- Node.js
  - Koa
  - Express

### 有序列表

1. Node.js
1. Express
1. Koa
1. Sails
1. Ruby
1. Rails
1. Sinatra
1. Go

## 表格

如果需要展示数据什么的，可以选择使用表格哦

| header 1 | header 3 |
| -------- | -------- |
| cell 1   | cell 2   |
| cell 3   | cell 4   |
| cell 5   | cell 6   |

## 段落

留空白的换行，将会被自动转换成一个段落，会有一定的段落间距，便于阅读。

请注意后面 Markdown 源代码的换行留空情况。

## 视频插入

目前支持 Youtube 和 Youku 两家的视频插入，你只需要复制视频播放页面，浏览器地址栏的网页 URL 地址，并粘贴到话题／回复文本框，提交以后将自动转换成视频播放器。

**例如**

**Youtube**

https://www.youtube.com/watch?v=52AMJwF7P0w

**Vimeo**

https://vimeo.com/460511888

**Youku**

https://v.youku.com/v_show/id_XNDU4Mzg4Mjc2OA==.html

**BiliBili**

https://www.bilibili.com/video/BV1uv411B7MK
···
