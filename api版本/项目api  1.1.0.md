[TOC]



### 1. 项目功能

* 用户模块, 包括用户的权限, 用户的个人信息管理, 显示和修改, 用户状态配置(封停, 启用)

* 用户收藏商品

* 用户的注册和登录
* 用户间的私信聊天
* 用户间的关注

* 主页, 商城主页以及畲路各页要显示的内容(轮播图)
* 文化元素介绍页要显示的内容(织带, 山歌, 婚俗, 服饰)以及点赞量
* 商城推荐显示的内容, 各个商品展示的内容(包括评论展示)
* 商品分类
* 商品收藏与加入购物车
* 商品下单
* 用户订单状态展示
* 商品评论
* 展示供应商信息
* 课程推荐显示的内容
* 每门课程展示的内容
* 用户报名课程
* 用户提交反馈意见
* app信息展示
* 管理员权限(商品上下架与审核, 用户封停与解封, 评论审核, 商品图片有效无效, 订单状态更改)





### 2. 项目模块划分

​	用户模块   后台管理模块  文化内容展示模块   课程模块   商城模块(商品模块  购物车与收藏模块  订单模块  评论模块)  

* 文化内容展示模块  

  * 用户登录后才能浏览文化展示内容, 
  * 每当用户为文化内容点赞, 则点赞量+1, 且同一用户仅可点赞一次
  * 所有用户都可以看到主页以及畲路的内容

* 用户模块
  * 配置用户权限, 管理员为特殊的用户, 畲族传参人为第二特殊用户
  * 以不同路由区分用户权限
  * 用户可自由编辑自身的资料, 可以查看其它用户资料并发送私信
  * 用户注册使用手机号+验证码返回token的方式, 通过token判断登录状态
  * 用户与用户间的关注
* 商城模块
  * 商品的分类
  * 单个商品的展示
  * 有专门的推荐商品, 展示在显眼位置
  * 供应商/店家的信息展示
  * 商品收藏/加入购物车功能
  * 商品下单生成订单,并实时更新订单状态
  * 商品评论与评论展示
* 课程模块
  * 课程展示
  * 课程推荐
  * 课程报名信息
* 后台管理模块
  * 设置一个超级用户作为管理员
  * 商品上下架与审核, 用户封停与解封, 评论审核, 商品图片有效无效, 订单状态更改



### 3. 后端api定义

​	***说明:*** 

​		**api**:  https://wzctest.wzc520pyf.cn   (还未启用)

​		**dev-api:** https://wzctest.wzc520pyf.cn/90430   (可使用)

​		**version**代表api版本号, 当前版本中其值**必须**是 1

​		发起请求时请在请求头携带参数: fapp : she-window    此参数将被用于识别请求来源

#### 	3.1 无须后端权限就能配置的api

##### 		获取畲路详情的api地址:  get	api/she-lus/:sl-fag?version=1;

```json
# Request
GET api/she-lus/:sl-fag?version=1   HTTP/1.1
	##dev GET dev-api/she-lus?version=1&sl-fag=0    HTTP/1.1
  ### sl-fag值可取4,1,2,3   依次对应畲路下的 简介, 起源, 节日, 非遗
Accept: application/json
fapp: she-window
	####一个请求示例: https://wzctest.wzc520pyf.cn/90430/she-lus?version=1&sl-fag=4
# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "articles":[		#文章
        {
            "ari_id":12345,		#文章id
            "ari_title":"",		#文章标题
            "ari_content":"畲族,是一个古老的民族......",		#文章内容
            "ari_pic":"图片url"		#文章配图
        },
        {
            "ari_id":12346,
            "ari_title":"中国畲乡之窗--大均",
            "ari_content":"大均乡位于......",
            "ari_pic":"图片url"
        }
    ]
}
```



##### 		获取首页轮播图的api地址:  get	api/index-pics?version=1;

```json
# Request
GET api/index-pics?version=1   HTTP/1.1
	##dev GET dev-api/index-pics?version=1    HTTP/1.1
Accept: application/json
fapp: she-window

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"index_pics":[		#轮播图
        {
            "title":"第一张图片",		#图片名
            "src":"http://图片链接",		#图片的跳转链接
            "url":"图片的url"		#图片的url地址
        },
        {
            "title":"第二张图片",
            "src":"http://图片链接",
            "url":"图片的url"
        },
        {
            "title":"第三张图片",
            "src":"http://图片链接",
            "url":"图片的url"
        }
    ]
}
```



##### 		获取文化元素基本信息(标题+描述)的api地址:  get	api/cultures/:culture/infos?version=1;

```json
# Request
GET api/cultures/:culture/infos?version=1   HTTP/1.1
	##dev GET dev-api/cultures/infos?version=1&culture=241;    HTTP/1.1
  ### culture指文化元素的id, 其值取 241, 251, 261, 271时为顶级文化元素  分别对应 畲族织带, 畲族山歌, 畲族婚俗, 畲族服饰     取其他值为子级元素--252:杂歌  253:叙事歌  254:小说歌  255:仪式歌  262:拦赤郎  263:对歌  264:借锅  265:撬蛙  266:夜行嫁  272:凤凰装  273:女子凤冠  274:畲族银饰
Accept: application/json
fapp: she-window

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "culture_name":"山歌",		#文化元素标题
    "culture_desc":"浙江景宁畲族山歌......",		#文化元素描述
    "culture_pic":[		#文化元素的图片地址
    	{
    		"url":"文化元素图片的url"		#图片地址
    		"desc":""		#图片描述
		}
    ]
}
```



##### 		获取文化元素副标题及描述的api地址:  get	api/cultures/:culture/sec-titles?version=1;

```json
# Request
GET api/cultures/:culture/sec-titles?version=1   HTTP/1.1
	##dev GET dev-api/cultures/sec-titles?version=1&culture=251    HTTP/1.1
  ### culture指文化元素的id, 其值可取 251, 261, 271  分别对应 山歌, 婚俗, 服饰
Accept: application/json
fapp: she-window

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"sec_titles":[
        {
            "sec_title":"歌曲种类",		#副标题名
            "sec_desc":"叙述歌、风俗歌......"		#副标题下的内容
        },
        {
            "sec_title":"演唱形式",
            "sec_desc":"分为平讲调、假声唱......"
        }
    ]
}
```



##### 		获取文化元素传承人的api地址:  get	api/cultures/:culture/persons?version=1;

```json
# Request
GET api/cultures/:culture/persons?version=1   HTTP/1.1
	##dev GET dev-api/cultures/persons?version=1&culture=241    HTTP/1.1
  ### culture指文化元素的id, 其值可取 241, 251, 261, 271  分别对应 织带, 山歌, 婚俗, 服饰
Accept: application/json
fapp: she-window

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"culture_persons":[		#传承人
        {
            "u_id":81001		#传承人id
        },
        {
            "u_id":81002
        }
    ]
}
```

##### 获取其他用户基本信息的api地址:  get	api/others/:other?version=1;

```json
# Request
GET api/others/:other?version=1   HTTP/1.1
	##dev GET dev-api/others?version=1&other=15523    HTTP/1.1
  ### other指用户id, 用于标识某一个用户
Accept: application/json
fapp: she-window

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"u_nickname":"蓝仙兰",		#用户昵称
    "u_avatar":"头像图片的url地址",		#用户头像
    "bg_pic":"信息背景展示图的url地址",		#用户背景图
    "signature":"山歌歌唱家"		#用户个性签名
}
```



##### 		获取文化元素视频的api地址:  get	api/cultures/:culture/videos?version=1;

```json
# Request
GET api/cultures/:culture/videos?version=1   HTTP/1.1
	##dev GET dev-api/cultures/videos?version=1&culture=242    HTTP/1.1
  ### culture指文化元素的id, 其值可取 241, 242, 243, 244  分别对应 织带, 山歌, 婚俗, 服饰
Accept: application/json
fapp: she-window

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"culture_video":[		#文化元素视频
        {
            "res_desc":"这是第一个视频",		#视频描述
            "res_url":"图片url地址"		#视频url地址
        },
        {
            "res_desc":"这是第二个视频",
            "res_url":"图片url地址"
        },
        {
            "res_desc":"这是第三个视频",
            "res_url":"图片url地址"
        }
    ]
}
```



##### 		获取商城首页轮播图的api地址:  get	api/shop-index-pics?version=1;

```json
# Request
GET api/shop-index-pics?version=1   HTTP/1.1
	##dev GET dev-api/shop-index-pics?version=1    HTTP/1.1
Accept: application/json
fapp: she-window

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"index_pics":[		#轮播图
        {
            "title":"第一张图片",		#图片名
            "src":"http://图片链接",		#图片的跳转链接
            "url":"图片的url"		#图片的url地址
        },
        {
            "title":"第二张图片",
            "src":"http://图片链接",
            "url":"图片的url"
        },
        {
            "title":"第三张图片",
            "src":"http://图片链接",
            "url":"图片的url"
        }
    ]
}
```



##### 		获取商城推荐商品列表的api地址:  get	api/shop-pushs?page=页数&per_page=每页包含数量&version=1;

```json
# Request
GET api/shop-pushs?page=1&per_page=3&version=1   HTTP/1.1
	##dev GET dev-api/shop-pushs?page=1&per_page=3&version=1   HTTP/1.1
Accept: application/json
fapp: she-window

# Response
HTTP/1.1 Status: 200 OK
Link: <api/shop-pushs?page=2&per_page=3>; rel="next",
	  <api/shop-pushs?page=3&per_page=3>; rel="last"
Content-Type: application/json; charset=utf-8

{
    "shop_pushs":[		#推荐的商品
        {
            "product_id":987771		#商品id
        },
        {
            "product_id":987772
        },
        {
            "product_id":987773
        }
    ]
}
```

##### 获取商城指定分类下的推荐商品列表(已按价格低到高排序)的api地址:  get	api/shop-categorys/:category/shop-pushs?page=页数&per_page=每页包含数量&version=1;

```json
# Request
GET api/shop-categorys/:category/shop-pushs?page=1&per_page=3&version=1   HTTP/1.1
	##dev GET dev-api/shop-categorys/shop-pushs?category=1541&page=1&per_page=3&version=1   HTTP/1.1
  ### category指分类id
Accept: application/json
fapp: she-window

# Response
HTTP/1.1 Status: 200 OK
Link: <api/shop-categorys/:id/shop-pushs?page=2&per_page=3>; rel="next",
	  <api/shop-categorys/:id/shop-pushs?page=3&per_page=3>; rel="last"
Content-Type: application/json; charset=utf-8

{
    "shop_pushs":[		#推荐的商品
        {
            "product_id":987771		#商品id
        },
        {
            "product_id":987772
        },
        {
            "product_id":987773
        }
    ]
}
```



##### 		获取主要的商品分类(分类名+分类标识图)的api地址:  get	api/categorys?version=1;

```json
# Request
GET api/categorys?version=1   HTTP/1.1
	##dev GET dev-api/categorys?version=1    HTTP/1.1
Accept: application/json
fapp: she-window

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"categorys":[		#分类标签
        {
            "category_id":1541,		#分类id
            "category_name":"非遗产品",		#分类名
            "category_pic":"配图的url地址"		#配图的url地址
        },
        {
            "category_id":1551,
            "category_name":"改良服饰",
            "category_pic":"配图的url地址"
        },
        {
            "category_id":1561,
            "category_name":"定制文创",
            "category_pic":"配图的url地址"
        }
    ]
}
```



##### 		获取指定分类的商品列表(已按价格低到高排序)的api地址(仅上架商品可被用户看到, 未上架或未通过审核的商品仅管理员可见):  get	api/shop-categorys/:category/shops?version=1;

```json
# Request
GET api/shop-categorys/:category/shops?page=1&per_page=3&version=1   HTTP/1.1
	##dev GET dev-api/shop-categorys/shops?category=1541&page=1&per_page=3&version=1   HTTP/1.1
  ###category指分类id
Accept: application/json
fapp: she-window

# Response
HTTP/1.1 Status: 200 OK
Link: <api/shop-categorys/:id/shops?page=2&per_page=3>; rel="next",
	  <api/shop-categorys/:id/shops?page=3&per_page=3>; rel="last"
Content-Type: application/json; charset=utf-8

{
    "shops":[		#商品
        {
            "product_id":98277		#商品id
        },
        {
            "product_id":96202
        },
        {
            "product_id":96212
        }
    ]
}
```



##### 		获取指定分类的子分类(分类名+分类标识图)列表的api地址:  get	api/categorys/:category/items?version=1;

```json
# Request
GET api/categorys/:category/items?version=1   HTTP/1.1
	##dev GET dev-api/categorys/items?version=1&category=1541    HTTP/1.1
  ### category指分类id
Accept: application/json
fapp: she-window

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"categorys":[		#分类
        {
            "category_id":2542,		#分类id
            "category_name":"畲族织带",		#分类名
            "category_pic":"配图的url地址"		#配图的url地址
        },
        {
            "category_id":2543,
            "category_name":"畲族酒",
            "category_pic":"配图的url地址"
        }
    ]
}
```



##### 		获取指定商品的信息的api地址:  get	api/shops/:shop?version=1;

```json
# Request
GET api/shops/:shop?version=1   HTTP/1.1
	##dev GET dev-api/shops?version=1&shop=987771    HTTP/1.1
  ### shop指商品id, 用于标识某一个商品
Accept: application/json
fapp: she-window

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"product_name":"畲乡之窗酒",		#商品名
    "product_sn":"1928334822192",		#商品条码
    "store_id":882332,		#供应商id
    "decimal":28.00,		#商品价格
    "product_date":"2021-08-31 23:02:20",		#生成日期
    "reserve":992,		#库存
    "product_desc":"美味的畲酒......",		#商品描述
    "sec_titles":[		#商品副标签
        {
            "sec_title":"满25包邮"		#商品副标签
        }
    ],
    "freight_amount":0,		#运费
    "auto_confirm_day":7,		#服务期
    "product_sku":[		#商品sku
        {
            "product_sku_id":1002983,		#sku的id
            "product_sku_code":"293829382938298329323232",		#sku条码
            "sku_pic":"对应sku下的商品图片url地址",       	#商品图片url地址
            "attribute":"净重",		#商品规格
            "value":"500",		#商品规格参数
            "p_weight":-1,		#重量
            "p_length":-1,		#长度
            "p_height":-1,		#高度
            "p_width":-1,       	#宽度
            "attributes":"{\"重量\":\"25\",\"长度\":\"298\",\"高度\":\"98\",\"宽度\":\"29\",\"尺码\":\"175\",\"颜色\":\"红色\"}"       		#sku属性描述(此处的值仅作展示,实际值请访问接口), 与上述属性值保持一致, 可以利用attributes反向查询sku编号(利用下文的接口)
        },
        {
            "product_sku_id":092211222
        },
        {
            "product_sku_id":092211223
        }
    ],
	"product_pics":[		#商品展示图片
        {
            "pic_desc":"这是一张图片",		#图片描述
            "product_pic":"图片的url地址",		#图片url地址
            "is_main":0,		#是否主图--0:是  1:否
            "pic_num":0        	#图片排序
        },
	    {
            "pic_desc":"",
            "product_pic":"",
            "is_main":1,
            "pic_num":1        
        },
		{
            "pic_desc":"",
            "product_pic":"",
            "is_main":1,
            "pic_num":2        
        }
    ]
}
```



##### 		获取指定商品的评论的api地址:  get	api/shops/:shop/comments?version=1&page=1&per_page=3;

```json
# Request
GET api/shops/:shop/comments?version=1&page=1&per_page=3   HTTP/1.1
	##dev GET dev-api/shops/comments?version=1&shop=1922&page=1&per_page=3    HTTP/1.1
  ### shop指商品id, 用于标识某一个商品
Accept: application/json
fapp: she-window

# Response
HTTP/1.1 Status: 200 OK
Link: <api/shops/:id/comments?version=1&page=2&per_page=3>; rel="next",
	  <api/shops/:id/comments?version=1&page=3&per_page=3>; rel="last"
Content-Type: application/json; charset=utf-8

{
	"comments":[		#评论
         {
         	"c_id":22412,		#评论id
        	"u_id":92211,		#用户id
        	"comment_title":"2021-09-02 分类: 青柠口味 畲乡之窗酒",		#评论标题
        	"comment_content":"宝贝收到了,和卖家描述一样, 味道不错, 很好喝,一直想买",		#评论内容
        	"is_pic":1,		#是否带图--1代表此评论有图,0代表无图
        	"c_pics":[		#评论图片
        		{
        			"pic_url":"评论的图片url地址"		#图片url地址
        		},
				{
        			"pic_url":"评论的图片url地址"
        		}
    		],
			"comment_num":4,		#评论星级
			"like_num":22,		#被点赞数
			"comment_time":"2021-09-02"		#评论时间---与comment_title中的时间是一致的       
         }
    ]
}
```

##### 获取指定评论下的跟评的api地址:  get	api/comments/:comment?version=1&page=1&per_page=3;

```json
# Request
GET api/shops/comments/:comment?version=1&page=1&per_page=3   HTTP/1.1
	##dev GET dev-api/comments?version=1&comment=22412&page=1&per_page=3    HTTP/1.1
  ### comment指评论id, 用于标识某一条评论
Accept: application/json
fapp: she-window

# Response
HTTP/1.1 Status: 200 OK
Link: <api/shops/:id/comments?version=1&page=2&per_page=3>; rel="next",
	  <api/shops/:id/comments?version=1&page=3&per_page=3>; rel="last"
Content-Type: application/json; charset=utf-8

{
	"comments":[		#评论
        {
            "c_id":22412,		#评论id
        	"u_id":92211,		#用户id
        	"comment_content":"是那种纯正的青柠味吗",		#评论内容
        	"is_pic":0,		#是否带图--1代表此评论有图,0代表无图
             "c_pics":[		#评论图片
        		
    		],
			"comment_time":"2017.02.15"		#评论时间
        }
    ]
}
```



##### 		获取指定商品的供应商信息的api地址:  get	api/shops/:shop/stores?version=1;

```json
# Request
GET api/shops/:shop/stores?version=1   HTTP/1.1
	##dev GET dev-api/shops/stores?version=1&shop=987771    HTTP/1.1
  ### shop指商品id, 用于标识某一个商品
Accept: application/json
fapp: she-window

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"store_id":882332,		#供应商id
    "store_code":"23234292932323",		#供应商编码
    "product_store":"畲乡酒业有限责任公司",		#供应商名称
    "product_store_type":"平台",		#供应商所属类型
    "store_manager_name":"张三",		#供应商联系人
    "store_manager_phone":"19234282341",		#联系人电话
    "store_address":"北京市西山区荷西街道22号",		#供应商所在地
    "all_like":4.8,		#供应商评级
    "store_pic":"图片url地址"		#供应商展示图片(头像)
}
```



##### 		获取指定关键词的商品的api地址:  get	api/shops/words/:word="冬季"?version=1&page=1&per_page=3;

```json
# Request
GET api/shops/words/:word="冬季"?version=1&page=1&per_page=3   HTTP/1.1
	##dev GET dev-api/shops/words?version=1&word="冬季"&page=1&per_page=3    HTTP/1.1
  ### word指商品关键词, 用于搜索商品
Accept: application/json
fapp: she-window

# Response
HTTP/1.1 Status: 200 OK
Link: <api/shops/words/:word="冬季"?version=1&page=2&per_page=3>; rel="next",
	  <api/shops/words/:word="冬季"?version=1&page=3&per_page=3>; rel="last"
Content-Type: application/json; charset=utf-8

{
	"shops":[		#符合条件的商品列表
        {
            "shop":309981		#商品id
        },
        {
            "shop":782222
        },
		{
            "shop":990221
        }
    ]
}
```



#### 	3.2 需要用户登录的api

##### 		请求验证码的api地址:  post	api/users/register?version=1;

```json
# Request
POST api/users/register?version=1   HTTP/1.1
	##dev POST dev-api/users/register?version=1   HTTP/1.1
Accept: application/json
fapp: she-window

{
    "u_phone":"17827234128"		#用户登录手机号
}

# Response
HTTP/1.1 Status: 200 OK
Link: 
Content-Type: application/json; charset=utf-8

{
    "message":"已发送验证码,请注意查收"		#返回信息
}
```



##### 		用户登录的api地址:  post	api/users/login?version=1;

```json
# Request
POST api/users/login?version=1   HTTP/1.1
	##dev POST dev-api/users/login?version=1   HTTP/1.1
Accept: application/json
fapp: she-window

{
    "u_phone":"17827234128",		#用户登录手机号
    "ver_code":"299221"		#登录验证码
}

# Response
HTTP/1.1 Status: 200 OK
Link: 
Content-Type: application/json; charset=utf-8

{
    "u_id":19222,		#用户id
    "u_phone":"17827234128",		#用户登录手机号
    "u_fag":0,		#用户标志--0:普通用户  1:畲族传承人  2:超级管理员
    "is_disabled":0,		#禁用状态--0:未禁用  1:被禁用
    "token":"6378bg928a8abbw9e092os9ie2h8df1f",
    "message":"登录成功"		#返回信息
}
```



##### 		获取用户资料的api地址:  get	api/users/:user/info?version=1;

```json
# Request
GET api/users/:user/info?version=1   HTTP/1.1
	##dev GET dev-api/users/info?version=1&user=19222    HTTP/1.1
  ### user指用户id, 用于标识一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"u_phone":"17827234128",		#用户登录手机号
    "u_fag":0,		#用户标志--0:普通用户  1:畲族传承人  2:超级管理员
    "u_nickname":"嬉皮士",		#用户昵称
    "u_avatar":"头像图片的url地址",		#用户头像
    "bg_pic":"个人主页面展示的背景图的url地址",		#用户背景图
    "signature":"划船不用桨,全靠浪!",		#个性签名
    "u_name":"张三",		#真实姓名
    "u_document_type":0,		#证件类型--0:中国大陆居民身份证  1:港澳台居民身份证  2:护照
    "u_id_number":"142202198812302120",		#证件号码
    "u_sex":0,		#性别--0:男  1:女
    "u_reg_time":"2020-09-01"		#注册时间
}
```



##### 		修改用户资料的api地址:  put	api/users/:user?version=1;

```json
# Request
PUT api/users/:user?version=1   HTTP/1.1
	##dev PUT dev-api/users?version=1&user=19222    HTTP/1.1
  ### user指用户id, 用于标识一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    "u_nickname":"嬉皮士",		//用户昵称
    "u_avatar":"头像图片名",		//用户头像
    "bg_pic":"个人主页面展示的背景图片名",		//用户背景图
    "signature":"划船不用桨,全靠浪!",		//个性签名
    "u_name":"张三",		//真实姓名
    "u_document_type":0,		//证件类型--0:中国大陆居民身份证  1:港澳台居民身份证  2:护照
    "u_id_number":"142202198812302120",		//证件号码
    "u_sex":0		//性别--0:男  1:女
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"message":"修改成功"		//返回信息
}
```

##### 用户头像上传的api地址: post	api/users/:user/avatar?version=1;

```json
# Request
POST api/users/:user/avatar?version=1   HTTP/1.1
	##dev POST dev-api/users/avatar?version=1&user=19222    HTTP/1.1
  ### user指用户id,用于标识一个用户
Accept: application/json
Content-Type: multipart/form-data
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "url":"刚刚上传的图片的url地址",		#图片url地址
	"message":"上传成功"		#返回信息
}
```

##### 用户背景图片上传的api地址: post	api/users/:user/bg-pic?version=1;

```json
# Request
POST api/users/:user/bg-pic?version=1   HTTP/1.1
	##dev POST dev-api/users/bg-pic?version=1&user=19222    HTTP/1.1
  ### user指用户id,用于标识一个用户
Accept: application/json
Content-Type: multipart/form-data
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "url":"刚刚上传的图片的url地址",		#图片url地址
	"message":"上传成功"		#返回信息
}
```



##### 获取用户收货地址的api地址: get	api/users/:user/address?version=1;

```json
# Request
GET api/users/:users/address?version=1   HTTP/1.1
	##dev GET dev-api/users/address?version=1&user=19222    HTTP/1.1
  ### user指用户id, 用于标识一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"address":[		#收货地址信息
        {
            "u_address_id":551765,		#收货地址信息id   
            "consignee_name":"张三",		#收货人姓名
            "consignee_phone":"1982287329",		#收货人联系电话
            "consignee_province":330000,		#地区代码--省---330000:浙江省
            "consignee_city":330100,		#地区代码--市---330100:杭州市
            "consignee_region":330106,		#地区代码--区---330106:西湖区
            "consignee_address":"浙江省杭州市西湖区留下街道留和路318号",		#收货详细地址
            "is_first":1		#是否是默认收货地址--0:非默认  1:默认
        },
		{
            "u_address_id":551766,		#收货地址信息id
            "consignee_name":"张三",		#收货人姓名
            "consignee_phone":"1982287329",		#收货人联系电话
            "consignee_province":330000,		#地区代码--省---330000:浙江省
            "consignee_city":330100,		#地区代码--市---330100:杭州市
            "consignee_region":330109,		#地区代码--区---330109:萧山区
            "consignee_address":"浙江省杭州市萧山区九悦江南",		#收货详细地址
            "is_first":0		#是否是默认收货地址--0:非默认  1:默认
        }
    ]
}
```



##### 新增用户收货地址的api地址: post	api/user/:user/address?version=1;

```json
# Request
POST api/user/:user/address?version=1   HTTP/1.1
	##dev POST dev-api/user/address?version=1&user=19222    HTTP/1.1
  ### user指用户id, 用于标识一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    "consignee_name":"张三",		#收货人姓名
    "consignee_phone":"1982287329",		#收货人联系电话
    "consignee_province":330000,		#地区代码--省---330000:浙江省
    "consignee_city":330100,		#地区代码--市---330100:杭州市
    "consignee_region":330109,		#地区代码--区---330109:萧山区
    "consignee_address":"浙江省杭州市萧山区九悦江南",		#收货详细地址
    "is_first":0		#是否是默认收货地址--0:非默认  1:默认
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "u_address_id":551767,		#收货地址信息id
	"message":"创建成功"		#返回信息
}
```



##### 更新用户收货地址的api地址: put	api/user/:user/address?version=1;

```json
# Request
PUT api/user/:user/address?version=1   HTTP/1.1
	##dev PUT dev-api/user/address?version=1&user=19222    HTTP/1.1
  ### user指用户id, 用于标识一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    "u_address_id":551765,		#收货地址信息id
    "consignee_name":"张三",		#收货人姓名
    "consignee_phone":"1982287329",		#收货人联系电话
    "consignee_province":330000,		#地区代码--省---330000:浙江省
    "consignee_city":330100,		#地区代码--市---330100:杭州市
    "consignee_region":330109,		#地区代码--区---330109:萧山区
    "consignee_address":"浙江省杭州市萧山区九悦江南",		#收货详细地址
    "is_first":0		#是否是默认收货地址--0:非默认  1:默认
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"message":"修改成功"
}
```

##### 删除用户收货地址的api地址: delete	api/user/:user/address?version=1;

```json
# Request
DELETE api/user/:user/address?version=1   HTTP/1.1
	##dev DELETE dev-api/user/address?version=1&user=19222    HTTP/1.1
  ### user指用户id, 用于标识一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    "u_address_id":551765,		#收货地址信息id
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"message":"删除成功"
}
```



##### 		发送私信的api地址:  post	api/users/mails?version=1;

```json
# Request
POST api/users/mails?version=1   HTTP/1.1
	##dev POST dev-api/users/mails?version=1    HTTP/1.1
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    "text":"这是发送的消息",		#消息
    "from_user":19222,		#发送者
    "to_user":19224			#接受者
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"message":"发送成功"		#返回信息
}
```



##### 		获取私信列表的api地址(仅仅是对话记录):  get	api/users/:user/mails?version=1;

```json
# Request
GET api/users/:user/mails?version=1   HTTP/1.1
	##dev GET dev-api/users/mails?version=1&user=19222    HTTP/1.1
  ### user指用户id, 用于标识一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"mails":[		#私信
        {
            "m_id":13,		#私信id
            "users":[		#对话者
                "19222",		#用户id
                "19224"			#用户id
            ]
        },
        {
            "m_id":19,
            "users":[
                "19222",
                "19226"
            ]
        }
    ]
}
```



##### 		获取用户私信的api地址(包含对话内容):  post	api/users/:user/mails-2/:mail?version=1;

```json
# Request
GET api/users/:user/mails-2/:mail?version=1   HTTP/1.1
	##dev GET dev-api/users/mails-2?version=1&user=19222&mail=19    HTTP/1.1
  ### user指用户id, 用于标识一个用户
  ### mail指对话id,用于标识一个对话
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"mails":[		#私信--私信类似一个对话,是对话双方唯一特有且共享的,内部可包含N条消息
        {
            "to_user":19224,		#接受者--不是自己则代表是自己给对方发消息
            "text":"你好, 19224",		#消息
            "time":"2021-09-02 20:09:19",		#发送时间
            "read":[]		#已读者--记录对话双方谁已读此消息
        },
        {
            "to_user":19226,		#接受者--是自己则代表是对方给自己发消息
            "text":"想你了, 19226",
            "time":"2021-09-02 20:11:19",
            "read":[]
        }
    ]
}
```



##### 根据手机号查找用户的api地址: get	api/users?version=1&userphone="17827234128";

```json
# Request
GET api/users?version=1&userphone="17827234128"   HTTP/1.1
	##dev GET dev-api/users?version=1&userphone="17827234128"    HTTP/1.1
  ### userphone指用户登录手机号
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"u_id":19222		#用户id
}
```



##### 		为指定文化元素(织带, 山歌, ...)点赞的api地址:  put	api/users/cultures/likes?version=1;

```json
# Request
PUT api/users/cultures/likes?version=1   HTTP/1.1
	##dev PUT dev-api/users/cultures/likes?version=1    HTTP/1.1
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    "culture_id":241,		#文化元素id
    "like":1		#喜欢---0:不喜欢  1:喜欢
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"message":"点赞成功"		#返回信息
}
```

##### 获取文化元素(织带, 山歌, ...)点赞数的api地址: get	api/users/cultures/:culture/likes?version=1;

```json
# Request
GET api/users/cultures/:culture/likes?version=1   HTTP/1.1
	##dev GET dev-api/users/cultures/likes?version=1&culture=241    HTTP/1.1
  ### culture指文化元素id, 用于标识某一个文化元素
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "like_num":2022		#获赞量
}
```



##### 获取指定用户的购物车商品列表的api地址:  get	api/users/:user/shopping-carts?version=1;

```json
# Request
GET api/users/:user/shopping-carts?version=1   HTTP/1.1
	##dev GET dev-api/users/shopping-carts?version=1&user=19222    HTTP/1.1
  ### user指用户id, 用于标识某一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "shops":[		#商品
        {
            "product_id":987771,		#商品id
            "product_sku_id":1002983,		#商品sku编号
            "product_quantity":2		#商品数量
        },
        {
            "product_id":377108,
            "product_sku_id":9977112,
            "product_quantity":1
        }
    ]
}
```

##### 查询商品的sku编号的api地址: post	api/users/shops/:shop?version=1;

```json
# Request
POST api/users/shops/:shop?version=1   HTTP/1.1
	##dev POST dev-api/users/shops?version=1&shop=987771    HTTP/1.1
  ### shop指商品id, 用于标识某一个商品
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    "attribute":"{"重量":"25","长度":"298","高度":"98","宽度":"29","尺码":"175","颜色":"红色"}"
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "shops_sku":987771
}
```



##### 		将指定商品加入购物车的api地址:  post	api/users/:user/shopping-carts?version=1;

```json
# Request
POST api/users/:user/shopping-carts?version=1   HTTP/1.1
	##dev POST dev-api/users/shopping-carts?version=1&user=19222    HTTP/1.1
  ### user指用户id, 用于标识某一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    "product_id":322110,		#商品id
    "product_sku_id":1002983,		#商品sku编号
    "product_quantity":2		#商品数量
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "message":"添加成功"
}
```

##### 修改商品在购物车中的数量的api地址: put	api/users/:user/shopping-carts?version=1;

```json
# Request
PUT api/users/:user/shopping-carts?version=1   HTTP/1.1
	##dev PUT dev-api/users/shopping-carts?version=1&user=19222    HTTP/1.1
  ### user指用户id, 用于标识某一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    "product_id":322110,		#商品id
    "product_sku_id":1002983,		#商品sku编号
    "product_quantity":2		#商品数量--仅可修改数量,其他属性仅作为查询,若查询不到对应商品则修改失败
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "message":"修改成功"
}
```



##### 		将指定商品移除购物车的api地址:  delete	api/users/:user/shopping-carts?version=1;

```json
# Request
DELETE api/users/:user/shopping-carts?version=1   HTTP/1.1
	##dev DELETE dev-api/users/shopping-carts?version=1&user=19222    HTTP/1.1
  ### user指用户id, 用于标识某一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    "product_id":322110,		#商品id
    "product_sku_id":1002983,		#商品sku编号
    "product_quantity":2		#商品数量
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "message":"删除成功"
}
```



##### 获取指定用户的收藏商品列表的api地址:  get	api/users/:user/collects?version=1;

```json
# Request
GET api/users/:user/collects?version=1   HTTP/1.1
	##dev GET dev-api/users/collects?version=1&user=19222    HTTP/1.1
  ### user指用户id, 用于标识某一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "shops":[		#商品
        {
            "product_id":322109,		#商品id
            "product_sku_id":1002223		#商品sku编号
        },
        {
            "product_id":377110,
            "product_sku_id":1002983
        }
    ]
}
```



##### 		将指定商品加入收藏的api地址:  post	api/users/:user/collects?version=1;

```json
# Request
POST api/users/:user/collects?version=1   HTTP/1.1
	##dev POST dev-api/users/collects?version=1&user=19222    HTTP/1.1
  ### user指用户id, 用于标识某一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    "product_id":322109		#商品id
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "message":"添加成功"
}
```



##### 		将指定商品移除收藏列表的api地址:  delete	api/users/:user/collects?version=1;

```json
# Request
DELETE api/users/:user/collects?version=1   HTTP/1.1
	##dev DELETE dev-api/users/collects?version=1&user=19222    HTTP/1.1
  ### user指用户id, 用于标识某一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    "product_id":003221		#商品id
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "message":{"删除成功"}
}
```



##### 		获取用户已拥有优惠券的api地址:  get	api/users/:user/coupons?version=1;

```json
# Request
GET api/users/:user/coupons?version=1   HTTP/1.1
	##dev GET dev-api/users/coupons?version=1&user=19222    HTTP/1.1
  ### user指用户id, 用于标识某一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "coupons":[		#优惠券
        {
            "coupon_id":22211,		#优惠券id
            "is_use":1,		#是否被使用--0:已使用  1:未使用
            "expiration":"2021-10-13"		#过期时间
        },
        {
            "coupon_id":23331,
            "is_use":1,
            "expiration":"2021-10-13"
        }
    ]
}
```



##### 获取优惠券详细信息的api地址: get	api/users/:user/coupons/:coupon/info?version=1;

```json
# Request
GET api/users/:user/coupons/:coupon/info?version=1   HTTP/1.1
	##dev GET dev-api/users/coupons/info?version=1&coupon=23331&user=19222    HTTP/1.1
  ### coupon指优惠券id, 用于标识某一个优惠券
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "product_id":987771,		#优惠券所属的商品id
    "coupon_amount":5.99,		#优惠金额
    "full_reduction":15.00,		#满减要求金额
    "coupon_num":1999		#优惠券剩余数量---商家发行的优惠券剩余数量
}
```



##### 		生成订单的api地址:  post	api/users/:user/orders?version=1;

```json
# Request
POST api/users/:user/orders?version=1   HTTP/1.1
	##dev POST dev-api/users/orders?version=1&user=19222    HTTP/1.1
  ### user指用户id, 用于标识某一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    "products":[		#订单包含的商品
        {
            "product_id":987771,		#商品id
            "product_sku_id":1002983,		#商品sku
            "product_quantity":1,		#商品数量
            "coupon_id":22211		#优惠券id
        },
        {
            "product_id":987771,
            "product_sku_id":1002778,
            "product_quantity":1,
            "coupon_id":23331
        }
    ],
    "bill_type":1,		#电子发票--0:不开发票  1:电子发票  2:纸质发票
    "bill_receiver_phone":"1982223821",		#收票人电话
    "bill_receiver_email":"1823382120@qq.com",		#收票人邮箱
    "bill_receiver_name":"张三",		#收票人姓名
    "receiver_province":330000,		#地区代码--省--330000:浙江省
    "receiver_city":330100,			#地区代码--市--330100:杭州市
    "receiver_region":330109,		#地区代码--区--330109:萧山区
    "receiver_address":"浙江省杭州市萧山区九悦江南",		#收地详细地址
    "order_note":"请发韵达,谢谢",		#订单备注
	"pay_type":1		#支付方式--0:微信  1:支付宝
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "order":1920201,		#订单id
    "orderInfo":"WJJ3SD&DS&S(^SDQW)...很长...",		#对订单关键信息加签并加密后生成的字符串,请利用此字符串向支付宝发起支付请求
    "message":"订单创建成功"
}

#支付接口仅仅以支付宝支付为例,未对微信支付方式规划接口, 在收到加签字符串后就可以向支付宝发起支付请求(但在此之前请先到支付宝网站获取应用公钥,应用私钥,支付宝公钥以及支付宝支付请求api),如果收到支付宝返回的信息是支付失败则应要求用户重新支付,如果收到支付宝返回信息是支付成功则应立即向后端请求订单状态(因为订单状态是后端收到支付宝通知后才依情况更改订单状态而非直接更改,所以不能根据支付宝返回的结果信息直接更改订单状态而必须向后端询问,并且订单状态会有更新延迟,应准备多次向后端请求)
```

##### 获取订单状态的api地址: get	api/users/:user/orders/:order/status?version=1;

```json
# Request
GET api/users/:user/orders/:order/status?version=1   HTTP/1.1
	##dev GET dev-api/users/orders/status?version=1&order=100111&user=19222    HTTP/1.1
  ### order指订单id, 用于标识某一个订单
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "order_status":1		#订单状态--0:待付款 1:待发货 2:已发货 3:已完成 4:已关闭 5:无效订单
}
```

##### 支付宝通知请求的api地址(此api仅为支付宝提供): post	api/get-asynchronous-yan-qian?version=1;

```json
# Request
POST api/get-asynchronous-yan-qian?version=1   HTTP/1.1
	##dev POST dev-api/get-asynchronous-yan-qian?version=1    HTTP/1.1
Accept: application/json

{
    #接收支付宝传来的一切内容
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "msg":"success"
}
```



##### 		获取订单列表的api地址:  get	api/users/:user/orders?version=1;

```json
# Request
GET api/users/:user/orders?version=1   HTTP/1.1
	##dev GET dev-api/users/orders?version=1&user=19222    HTTP/1.1
  ### user都指用户id, 用于标识某一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"orders":[		#订单
        {
            "order_id":1920201,		#订单id
            "order_status":1,	 #订单状态--0:待付款 1:待发货 2:已发货 3:已完成 4:已关闭 5:无效订单
            "pay_amount":992.00		#订单实付金额
        },
        {
            "order_id":1920220,
            "order_status":3,
            "pay_amount":200.00
        }
    ]
}
```



##### 		获取特定订单信息的api地址:  get	api/users/:user/orders/:order/info?version=1;

```json
# Request
GET api/users/:user/orders/:order/info?version=1   HTTP/1.1
	##dev GET dev-api/users/orders/info?version=1&order=1920201&user=19222    HTTP/1.1
  ### order指订单id, 用于标识某一个订单
  ### user指用户id,用于标识某一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"order_id":1920201,		#订单id
    "u_id":19222,		#用户id
    "order_sn":"1020210020102102012",		#订单编号
    "create_time":"2021-09-01 23:20:19",		#创建时间
    "u_nickname":"啦啦",		#用户昵称
    "total_amount":992.00,		#订单总金额
    "pay_amount":980.00,		#订单实付金额
    "freight_amount":10.00,		#运费id
    "coupon_amount":22.00,		#优惠券抵扣金额
    "pay_type":1,		#支付方式--0:微信  1:支付宝
    "order_status":1,		#订单状态--0:待付款 1:待发货 2:已发货 3:已完成 4:已关闭 5:无效订单
    "delivery_company":"韵达",		#物流公司
    "delivery_sn":"20121231313123324222",		#物流单号
    "auto_confirm_day":7,		#自动确认收货时间--7天
    "bill_type":1,		#发票类型--0:不开  1:电子  2:纸质
    "bill_header":"张三",		#发票抬头--即购物人名称
    "bill_content":"畲乡之窗酒,青柠口味,120cm,899元,*1,2021-09-01 20:23:29",	#发票内容
    "bill_receiver_phone":"1922321122",		#收票人电话
    "bill_receiver_email":"292314221@qq.com",		#收票人邮箱
    "receiver_name":"张三",		#收货人姓名
    "receiver_phone":"1922321122",		#收货人电话
    "receiver_province":330000,		#地区代码--省--330000:浙江省
    "receiver_city":330100,			#地区代码--市--330100:杭州市
    "receiver_region":330109,		#地区代码--区--330109:萧山区
    "receiver_address":"浙江省杭州市萧山区九悦江南",		#收地详细地址
    "order_note":"请发韵达,谢谢",		#订单备注
    "confirm_status":0,		#确认收货状态--0:未确认  1:已确认
    "payment_time":"2021-09-01 09:29:20",		#支付时间
    "delivery_time":"2021-09-01 12:00:20",		#发货时间
    "receiver_time":"",		#确认收货时间
    "comment_time":"",		#评价时间
    "modify_time":"2021-09-01 23:29:29"		#订单更新时间
    "shops":[		#订单包含的商品
    	{
    		"oi_id":9920201,		#订单商品id--可利用此id查询订单商品表中的详细信息
    		"product_id":987771,		#商品id
    		"product_pic":"商品图片url地址",		#商品图片
    		"product_name":"畲乡之窗酒柚子口味",		#商品名称
    		"product_price":899.00,		#商品价格
    		"product_quantity":1,		#购买数量
    		"real_amount":890.00		#实付金额
		},
		{
    		"oi_id":9920220,		#订单商品id--可利用此id查询订单商品表中的详细信息
    		"product_id":987772,		#商品id
    		"product_pic":"商品图片url地址",		#商品图片
    		"product_name":"畲乡之窗酒青柠口味",		#商品名称
    		"product_price":899.00,		#商品价格
    		"product_quantity":1,		#购买数量
    		"real_amount":890.00		#实付金额
		}
    ]
}
```

##### 获取订单中商品的详细信息的api地址: get	api/users/:user/orders/:order/shops/:oi-id?version=1;

```json
# Request
GET api/users/:user/orders/:order/shops/:oi-id?version=1   HTTP/1.1
	##dev GET dev-api/users/orders/shops?version=1&user=19222&order=1920201&oi-id=9920201    HTTP/1.1
  ### order指订单id, 用于标识某一个订单
  ### oi-id指订单商品id,用于标识订单中包含的某一个商品
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "order_sn":"00313131342432423",		#订单编号
    "product_id":987771,		#商品id
    "product_pic":"商品图片url",		#商品图片的url地址
    "product_name":"畲乡之窗酒",		#商品名称
    "product_store":"畲乡酒业有限责任公司",		#供应商(店家)名称
    "product_sn":"02932941558578658",		#商品条码
    "product_price":899.00,		#商品销售价格
    "product_quantity":1,		#购买数量
    "product_sku_id":1002983,		#商品sku的id
    "product_sku_code":"29314174104710412",		#商品sku条码
    "product_category_id":4,		#商品分类--1:非遗 2:改良服饰 3:文创产品 4:畲族酒 5:畲元素改良服饰 6:畲族传统服饰 7:工作服 8:文具文创 9:T恤 10:手机壳 11:文创袋子
    "sp1":"{"口味":"柚子口味"}",		#商品属性1
    "sp2":"",		#商品属性2
    "sp3":"",		#商品属性3
    "product_attr":"{"口味":"柚子口味"}",		#商品销售属性,当上诉sp不够用时,将商品属性存放在这里
    "coupon_id":22211,		#优惠券id
    "coupon_amount":10.00,		#优惠券抵扣金额
    "real_amount":15.00		#实付金额(优惠券抵扣后金额)
}
```



##### 		确认收货的api地址:  put	api/users/:user/orders/:order?version=1;

```json
# Request
PUT api/users/:user/orders/:order?version=1   HTTP/1.1
	##dev PUT dev-api/users/orders/?version=1&user=19222&order=1920201    HTTP/1.1
  ### order指订单id, 用于标识某一个订单
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    "u_id":19222,		#用户id
    "order_status":3		#订单状态--0:待付款 1:待发货 2:已发货 3:已完成 4:已关闭 5:无效订单
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "code":9000,		#操作id--9000:操作成功  非9000:操作失败即订单状态更改失败
	"message":"修改成功"	
}
```



##### 		发布指定商品评论的api地址:  post	api/users/:user/shops/:shop/comments?version=1;

```json
# Request
POST api/users/:user/shops/:shop/comments?version=1   HTTP/1.1
	##dev POST dev-api/users/shops/comments?version=1&user=19222&shop=987771    HTTP/1.1
  ### shop指商品id, 用于标识某一个商品
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    "u_id":19222,		#用户id
    "order_id":2313123,			#订单id
    "comment_title":"2021-02-15 颜色分类: 蓝 梦幻蓝咖啡杯",		#评论标题
    "comment_content":"宝贝收到了,和卖家描述一样,质量不错,很漂亮,一直想买",		#评论内容
    "is_pic":1,		#是否带图--0:不带图  1:带图
    "c_pics":[		#评论图片
    	{
    		"pic_url":"评论的图片文件名"		#图片的文件名
		},
		{
    		"pic_url":"评论的图片文件名"
		}
    ],
	"comment_num":4		#评论星级--4星
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "code":9000,		#操作id--9000:操作成功  非9000:操作失败
    "c_id":22415,
	"message":"发布成功"		#返回信息
}
```

##### 评论图片上传的api接口地址: post	api/users/:user/comments/:comment/pics?version=1;

```json
# Request
POST api/users/:user/comments/:comment/pics?version=1   HTTP/1.1
	##dev POST dev-api/users/comments/pics?version=1&user=19222&comment=22415    HTTP/1.1
  ### comment指评论id,用于标识一条评论
Accept: application/json
Content-Type: multipart/form-data
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    
}

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "url":"图片url地址",		#图片url地址
	"message":"上传成功"		#返回信息
}
```



#### 	3.3 畲族传承人专用api

##### 获取传承人个人故事的api地址:  get	api/culture-persons/:person/storys?version=1;

```json
# Request
GET api/culture-persons/:person/storys?version=1   HTTP/1.1
	##dev GET dev-api/culture-persons/storys?version=1&person=18226    HTTP/1.1
  ### person指用户id, 用于标识某一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"story":"1968年, 蓝诞兰出生在......"		#用户自诉故事/个人简介
}
```



##### 		获取传承人个人作品列表的api地址:  get	api/culture-persons/:person/works?version=1;

```json
# Request
GET api/culture-persons/:person/works?version=1   HTTP/1.1
	##dev GET dev-api/culture-persons/works?version=1&person=18226    HTTP/1.1
  ### person指用户id, 用于标识某一个用户
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"works":[		#作品
        {
            "work_id":88798,		#作品id
            "work_title":"彩带王<56个民族>"		#作品名
        },
        {
            "work_id":88799,
            "work_title":"2代彩带王"
        }
    ]
}
```



##### 		获取传承人个人作品资料的api地址:  get	api/culture-persons/:person/works/:work/info?version=1;

```json
# Request
GET api/culture-persons/:person/works/:work/info?version=1   HTTP/1.1
	##dev GET dev-api/culture-persons/works/info?version=1&person=18226&work=88798    HTTP/1.1
  ### work指作品id, 用于标识某一个作品
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
	"work_title":"彩带王<56个民族>",		#作品名
    "work_desc":"一条4米长,10厘米......",		#作品描述
    "work_pic":"作品图片的url地址",		#作品图片url地址
    "work_other_desc":"作品的一些其他描述,多条描述已使用*分割开"		#作品的其他描述/属性
}
```

##### 传承人新建个人作品的api地址: post	api/culture-persons/:person/works?version=1;

```json
# Request
POST  api/culture-persons/:person/works?version=1  HTTP/1.1
	##dev POST dev-api/culture-persons/works?version=1&person=18226
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    "u_id":15523,		#用户id
    "work_title":"三代彩带王<56个民族>",		#作品名
    "work_desc":"一条4米长,10厘米......",		#作品描述
    "work_pic":"作品图片的url地址",		#作品图片url地址
    "work_other_desc":"作品的一些其他描述,多条描述已使用*分割开"		#作品的其他描述/属性
}

# Response
HTTP/1.1 Status: 200 OK
Link: 
Content-Type: application/json; charset=utf-8

{
    "work_id":88799,		#创建的作品id
    "message":"创建成功"		#回调提示
}
```



##### 		传承人编辑个人作品资料的api地址:  put	api/culture-persons/:person/works?version=1;

```json
# Request
PUT  api/culture-persons/:person/works?version=1  HTTP/1.1
	##dev PUT dev-api/culture-persons/works?version=1&person=18226
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    "u_id":15523,		#用户id
    "work_id":88798,		#作品id
    "work_title":"彩带王<56个民族>",		#作品名
    "work_desc":"一条4米长,10厘米......",		#作品描述
    "work_pic":"作品图片的url地址",		#作品图片url地址
    "work_other_desc":"作品的一些其他描述,多条描述已使用*分割开"		#作品的其他描述/属性
}

# Response
HTTP/1.1 Status: 200 OK
Link: 
Content-Type: application/json; charset=utf-8

{
    "work_id":88798,		#创建的作品id
    "message":"修改成功"		#回调提示
}
```

##### 传承人删除个人作品资料的api地址:  delete	culture-persons/:person/works?version=1;

```json
# Request
DELETE  api/culture-persons/:person/works?version=1  HTTP/1.1
	##dev DELETE dev-api/culture-persons/works?version=1&person=18226
Accept: application/json
fapp: she-window
token: cedf3947e1014712e3df9fea4e3c4b96

{
    "u_id":15523,		#用户id
    "work_id":88798,		#作品id
}

# Response
HTTP/1.1 Status: 200 OK
Link: 
Content-Type: application/json; charset=utf-8

{
    "message":"删除成功"		#回调提示
}
```



#### 	3.4 超级用户专用api ---完善中

##### 		获取指定供应商/商家商品列表的api地址:  get	api/admin/stores/:id/shops?version=1;

##### 		获取指定商品审核信息的api地址:  get	api/admin/shops/:id?version=1;

##### 		更改指定商品上架状态的api地址:  put	api/admin/shops/:id?version=1;

##### 		更改指定商品审核状态的api地址:  put	api/admin/shops/:id?version=1;

##### 		获取指定商品评论列表的api地址:  get	api/admin/shops/:id/comments?version=1;

##### 		更改商品评论审核状态的api地址:  put	api/admin/comments/:id?version=1;

##### 		更改商品图片是否有效的api地址:  put	api/admin/shops/:id/pics?version=1;

##### 获取全部用户列表的api地址:   get	api/admin/users?version=1;

##### 		更改用户状态与标志的api地址: put	api/admin/users/用户名?version=1;

##### 		获取指定用户订单列表的api地址:  get	api/admin/users/用户名/orders?version=1;

```json
# Request
GET 	api/admin/users/17816134129/orders?version=1;   HTTP/1.1
	##dev GET dev-api/admin/users/orders?version=1&user=17816134129    HTTP/1.1
  ### user指用户登录手机号
Accept: application/json

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "orders":[		#订单列表
    	{
        	"order_id":12345,		#订单id
            "order_sn":"20210827009130201010970092823888",		#订单编号
            "total_amount":188.2,		#订单总金额
            "coupon_amunt":2.88,		#优惠券抵扣金额
            "pay_amount":185.32,		#实付金额
            "order_status":"已完成"		#订单状态
    	},
    	{
        	"order_id":12322,
            "order_sn":"20210831009130201011170092823899",
            "total_amount":1288.2,
            "coupon_amunt":3.88,
            "pay_amount":1184.32,
            "order_status":"待发货"
    	}
	]
}
```



##### 		获取指定订单的api地址:  get	api/admin/orders/:id?version=1;

```json
# Request
GET 	api/admin/users/17816134129/:id?version=1;   HTTP/1.1
	##dev GET dev-api/admin/users/orders?version=1&order=12345    HTTP/1.1
  ### order指订单id
Accept: application/json

# Response
HTTP/1.1 Status: 200 OK
Content-Type: application/json; charset=utf-8

{
    "order_id":12345,		#订单id
    "order_sn":"20210827009130201010970092823888",		#订单编号
    "u_id":"20221",		#用户id
    "u_nickname":"单身贵族",		#用户昵称
    "total_amount":188.2,		#订单总金额
    "coupon_amunt":2.88,		#优惠券抵扣金额
    "pay_amount":185.32,		#实付金额
    "pay_type":"支付宝",		#支付方式
    "order_status":"已完成",		#订单状态
    "receiver_name":"张三",		#收货人姓名
    "receiver_phone":17817834192,		#收货人手机号
    "receiver_address":"浙江省杭州市咚咚咚去啦啦啦街道555号",		#收货地址
    "order_note":"请发韵达,谢谢",		#订单备注
    "delivery_sn":"29294302182211992244832173",		#物流单号
    "delivery_company":"韵达",		#物流公司
    "create_time":"2021-05-29-00:24:29",		#订单创建时间
    "payment_time":"2021-05-29-00:27:29",		#支付时间
    "delivery_time":"2021-06-01-12:27:29",		#发货时间
    "receiver_time":"2021-06-04-12:27:29",		#确认收货时间
    "products":[		#订单中包含的商品
            {
                "product_id":2223321,		#商品id
                "product_pic":"主图url",		#商品主图url
                "product_name":"新制作畲族围巾男女冬情侣冬",		#商品名
                "product_quantity":2,		#购买数量
                "product_price":50.20,		#单件金额
                "real_amount":98.40		#实付金额
            },
            {
                "product_id":2442221,
                "product_pic":"主图url",
                "product_name":"新制作畲族手套男女冬情侣冬",
                "product_quantity":1,
                "product_price":87.80,
                "real_amount":86.92
            },
    ]
}
```



##### 		更改指定订单的订单状态的api地址:  put	api/admin/orders/:id?version=1;

#### 3.5 出错

```json
# Requset
GET api/  HTTP/1.1
Accept: text/html

# Response
HTTP/1.1 406 Not Acceptable
Content-Type: application/json; charset=utf-8

{"message":"Must ACCEPT application/json: [\"text/xml\"]"}
```



