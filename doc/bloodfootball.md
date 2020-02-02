# 热血足球接口文档

 

------

版本：V1.0           

修订时间：2020/2/3

------



## 一 通用接口请求机制

接口请求返回数据为 ***json*** 格式

### 1 接口请求通用格式

```
{ 
   "head"：
   {
 		 "deviceid":"",     //用户设备id
 		 "lcid":"",         //机器语言id 
 		 "context":"",      //本次请求客户端的自定义字段：请求什么，就返回什么
 		 "sign":""		    //本次请求签名 
 		 "timestamp":"",    //时间戳
 	}
 	"user"：                //用户登录信息，如果没有可以为空
 	{
		 "uid"："",           //用户uid：唯一标识
		 "token":""           //用户登录token
	}
 	"data"：{}              //具体接口的请求信息，不同的接口有不同的格式。
}
```



### 2 接口返回通用格式

```
{
	"head":
	{
		"ret":0,               //返回码，0为成功，非0为错误码
		"des":"",              //返回描述信息
		"context":""           //客户端的自定义字段：请求什么，就返回什么
	}
	"data":{}                //具体接口的返回信息，不同的接口有不同的格式。
} 
```



### 3 接口签名机制

加密key：<u>***a9f95526a604cfc7***</u>

签名方式：sign = **<u>*md5（ data内容(json数据包括前后“{}”)  + 加密key + timestamp）</u>***

H5页面的接口，可以不用签名



### 4 接口域名

正式环境：待定

测试环境：待定

开发环境：待定



# 二 首页接口

## 1 视频请求接口

### 接口url路径：/getvideo

### 请求：

```
{
 		"videoid":"",       //视频id
		"count": 1,         //返回该视频videoid之后的视频数
 		"type":0            //0：推荐， 1：新鲜， 2：同城
} 
```

### 返回：

```
{

	   "videolist":
	[
			"userid":"",           //视频发布者id
			"userpic":"",          //视频发布者头像url
			"username":"",         //视频发布者昵称
			"title"："",           //标题
			"videoid":"",          //视频id
			"videourl":"",         //视频url        
			"videopic":"",         //视频展示图片
			"likes"：0,            //点赞数
			"islike":0,            //是否点赞
			"follows":0,           //关注数
			"isfollow":0,          //是否关注
			"watchs":0,            //观看数
			"shares": 0,           //分享数
			"comments":0,          //评论数
			"classify":"",         //分类名称
			"tags":["",""],        //标签列表
			"hotcomments":         //热门评论
			[
				"likes":0,      //点赞数
				"content":"",   //内容
				"commentid":""  //评论id
			]
	]
}
```



## 2 评论请求接口

### 接口url路径：/getcomment

### 请求：

```
{
 		"videoid":"",           //视频id
		"parentcommentid":"",   //父评论id，如果没有父评论，该id为0
		"start":0,              //请求开始的评论行，从0开始
		"count": 1,             //请求返还的评论数  
} 
```



### 返回：

```
{
		 "videoid":"",           //视频id
		 "parentcommentid":"",   //父评论id，如果没有父评论，该id为0
		 "commentlist":
		[
			"likes":0,           //点赞数
			"content":"",        //内容
			"commentid":""       //评论id
			"islike":0,          //是否点赞
		]
}
```



## 3 视频点赞接口

### 接口路径：/setvideolikes

### 请求：

```
{
 		"videoid":"",                      //视频id 
 		"islike":0,                        //是否点赞
} 
```



### 返回：

```
{
}
```



## 4 视频分享接口

### 接口路径：/setvideoshares

### 请求：

```
{
 		"videoid":"",                      //视频id 
} 
```



### 返回：

```
{
}
```



## 5 评论点赞接口

### 接口路径：/setcommentlikes

### 请求：

```
{ 
		"commentid":"",   //评论id 
		"like":1,         //1：点赞， 0：取消点赞
} 
```



### 返回：

```
{
}
```



## 6 评论接口

### 接口路径： /videocomment

### 请求：

```
{
		 "videoid":"",                    //视频id
		 "parentcommentid":"",            //父评论id，如果没有父评论，该id为0
		 "content":""                     //评论内容
}
```



### 返回：

```
{
}
```



# 三 话题接口

## 1 获取话题列表

### 接口路径：/gettopic

### 请求：

```
{
		”start“: 0,                  //话题开始数 
		"count":0,                   //获取请求话题数
}
```



### 返回：

```
{
		"topiclist":              //话题列表
		[
		"topicid":"",          //话题id
		"topictype":1,         //话题type：1 专题， 2 投票 , 3 其他
		"topicpic":"",         //话题展示图片
		"topictitle":""        //话题展示标题
		"other":{              //topictype为3时，该值有效
				"taglist"：
					[
					"tagid":"",                 //视频tagid
					"tagname":"",               //视频tag名称
					"tagpic":"",                //视频tag展示图片
					"videocount":0,             //视频数
					"watchs":0                  //观看数
					]
				"classifylist"：
					[
					"classifyid":"",            //视频分类id
					"classifyname":"",          //视频分类名称
					"classifypic":"",           //视频分类展示图片
					"videocount":0,             //视频数
					"watchs":0                  //观看数
					]		
		}
	]
}
```



## 2 获取分类列表

### 接口路径：/getclassifydetail

### 请求：

```
{
		"classifyid":"",              //视频分类id
		”start“: 0,                  //开始数 
		"count":0,                   //请求数
}
```



### 返回：

```
{
	"videolist":
	[
			"userid":"",           //视频发布者id
			"userpic":"",          //视频发布者头像url
			"username":"",         //视频发布者昵称
			"title"："",           //标题
			"videoid":"",          //视频id
			"videourl":"",         //视频url        
			"videopic":"",         //视频展示图片
			"likes"：0,            //点赞数
			"islike":0,            //是否点赞
			"follows":0,           //关注数
			"isfollow":0,          //是否关注
			"watchs":0,            //观看数
			"shares": 0,           //分享数
			"comments":0,          //评论数
			"classify":"",         //分类名称
			"tags":["",""],        //标签列表
			"hotcomments":         //热门评论
			[
					"likes":0,      //点赞数
					"content":"",   //内容
					"commentid":""  //评论id
			]
	]
}
```





## 3 获取标签列表

### 接口路径：/gettagsdetail

### 请求：

```
{
		"tagid":"",                 //视频tagid
		”start“:0,                 //开始数 
		"count":0,                  //请求数
}
```



### 返回：

```
{

	"videolist":
	[
			"userid":"",           //视频发布者id
			"userpic":"",          //视频发布者头像url
			"username":"",         //视频发布者昵称
			"title"："",           //标题
			"videoid":"",          //视频id
			"videourl":"",         //视频url        
			"videopic":"",         //视频展示图片
			"likes"：0,            //点赞数
			"islike":0,            //是否点赞
			"follows":0,           //关注数
			"isfollow":0,          //是否关注
			"watchs":0,            //观看数
			"shares": 0,           //分享数
			"comments":0,          //评论数
			"classify":"",         //分类名称
			"tags":["",""],        //标签列表
			"hotcomments":         //热门评论
			[
					"likes":0,      //点赞数
					"content":"",   //内容
					"commentid":""  //评论id
			]
	]
}
```





## 4  获取专题

### 接口路径：/getspecial

### 请求：

```
{
	    "topicid":"",             //话题id
		”start“: 0,                  //专题开始数 
		"count":0,                //专题请求话题数
}
```



### 返回：

```
{

	   "videolist":
	[
			"userid":"",           //视频发布者id
			"userpic":"",          //视频发布者头像url
			"username":"",         //视频发布者昵称
			"title"："",           //标题
			"videoid":"",          //视频id
			"videourl":"",         //视频url        
			"videopic":"",         //视频展示图片
			"likes"：0,            //点赞数
			"islike":0,            //是否点赞
			"follows":0,           //关注数
			"isfollow":0,          //是否关注
			"watchs":0,            //观看数
			"shares": 0,           //分享数
			"comments":0,          //评论数
			"classify":"",         //分类名称
			"tags":["",""],        //标签列表
			"hotcomments":         //热门评论
			[
					"likes":0,      //点赞数
					"content":"",   //内容
					"commentid":""  //评论id
			]
	]
}
```



## 5 获取投票

### 接口路径：/getvote

### 请求：

```
{
	    "topicid":"",             //话题id
}
```



### 返回：

```
{
	"votelist":                     //选项
[
	"voteid":"",                  //选项id
	"votecontent":""              //选项描述
  　"votecount":""                // 选项投票数
]
	"votetitle":"",              //投票标题
	"content":"",                //投票内容
	"myvoteid":"",               //我的投票选项
    "voteallcount":""            //总的投票数
}
```



## 6 投票

### 接口路径：/setvote

### 请求：

```
{
	    "topicid":"",             //话题id 
        "voteid":"",              //选项id
}
```



### 返回：

```
{

}
```



# 四 搜索接口

## 1 搜索

### 接口路径：/search

### 请求：

```
{
	"words":"",            //搜索词
	"start":0,             //开始数 
	"count":1              //下拉数
}
```



### 返回：

```
{

	   "videolist":
	[
			"userid":"",           //视频发布者id
			"userpic":"",          //视频发布者头像url
			"username":"",         //视频发布者昵称
			"title"："",           //标题
			"videoid":"",          //视频id
			"videourl":"",         //视频url        
			"videopic":"",         //视频展示图片
			"likes"：0,            //点赞数
			"islike":0,            //是否点赞
			"follows":0,           //关注数
			"isfollow":0,          //是否关注
			"watchs":0,            //观看数
			"shares": 0,           //分享数
			"comments":0,          //评论数
			"classify":"",         //分类名称
			"tags":["",""],        //标签列表
			"hotcomments":         //热门评论
			[
					"likes":0,      //点赞数
					"content":"",   //内容
					"commentid":""  //评论id
			]
	]
}
```



# 五 个人中心

## 1 获取个人资料

### 接口路径：/getuserinfo

### 请求：

```
{

}
```



### 返回：

```
{
		"username":"",         //用户名
		"userpic":"",          //头像
		"userwatches":"",      //观看时长
		"signature":"",        //个性签名
		"likescount":0,        //点赞数 
        "followcount":0,       //关注数

}
```



## 2 获取点赞视频

### 接口路径：/getuserlikes

### 请求：

```
{	
	"start":0,             //开始数 
    "count":1              //下拉数
}
```



### 返回：

```
{

	   "videolist":
	[
			"userid":"",           //视频发布者id
			"userpic":"",          //视频发布者头像url
			"username":"",         //视频发布者昵称
			"title"："",           //标题
			"videoid":"",          //视频id
			"videourl":"",         //视频url        
			"videopic":"",         //视频展示图片
			"likes"：0,            //点赞数
			"islike":0,            //是否点赞
			"follows":0,           //关注数
			"isfollow":0,          //是否关注
			"watchs":0,            //观看数
			"shares": 0,           //分享数
			"comments":0,          //评论数
			"classify":"",         //分类名称
			"tags":["",""],        //标签列表
			"hotcomments":         //热门评论
			[
					"likes":0,      //点赞数
					"content":"",   //内容
					"commentid":""  //评论id
			]
	]
}
```



## 3 获取关注列表

### 接口路径：/getuserfollows

### 请求：

```
{	
	"start":0,              //开始数 
	"count":1               //下拉数
}
```



### 返回：

```
待定
```



## 4 用户编辑

### 接口路径：/edituser

### 请求：

```
{
		"username":"",         //用户名
		"userpic":"",          //头像 
		"signature":"",        //个性签名 
}
```

### 返回：

```
{

}
```



# 六 登录接口

## 1 第三方登录

### 接口路径：/thirdlogin

### 请求：

```
{
		"type": 1,          //第三方登录类型，1：qq，2：微信，3微博
		"auth":"",          //授权code
        "authtype":0        //0：auth为code， 1：auth为accesstoken
}
```



### 返回：

```
{
		"uid":"",              //用户uid
        "cred":"",             //用户登录凭证
        "token":""             //用户业务票据
		”phonemask":""         //用户手机掩码
}
```

## 2 短信登录发送短信

### 接口路径：/loginsendsms

### 请求：

```
{
	    "phone":""                                    //手机号
}
```



### 返回：

```
{

}
```



## 3 短信登录

### 接口路径：/loginsms

### 请求：

```
{
	  "phone":""                                    //手机号
	  "sms":""                                       //短信
}
```



### 返回：

```
{
		"uid":"",              //用户uid
        "cred":"",             //用户登录凭证
        "token":""             //用户业务票据
		”phonemask":""         //用户手机掩码
}
```



## 4 凭证登录

### 接口路径：/logincred

### 请求：

```
{
	  "uid":""                                        //用户uid
	  "cred":""                                       //凭证
}
```



### 返回：

```
{
		"uid":"",              //用户uid
        "cred":"",             //用户登录凭证
        "token":""             //用户业务票据
		”phonemask":""         //用户手机掩码
}
```

