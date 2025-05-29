# 新版客户端Api架构

分三个oss接口，依次检测可用性。

登录界面，提供 在线登录 以及 离线版 选项。（离线版就是常规的导入订阅使用）

### oss接口路径架构：

> https://ossapi.com/fl/api/core.json (核心)

```json
{
    "api": "https://aaa.com/data/sj",
    "api2": "https://bbb.com/data/sj",
    "api3": "https://ccc.com/data/sj",
    "path": {
        "config": "/guest/comm/config",
        "login": "/passport/auth/login",
        "check": "/passport/auth/check",
        "register": "/passport/auth/register",
        "forget": "/passport/auth/forget",
        "sendEmailVerify": "/passport/comm/sendEmailVerify"
    },
    "website": {
        "url": "https://fabu1.com",
        "url2": "https://fabu2.com",
        "url3": "https://fabu3.com"
    },
    "subscription-url": {
        "url": "https://sub1.com/idurl?token=",
        "url2": "https://sub2.com/idurl?token=",
        "url3": "https://sub3.com/idurl?token="
    },
	  "policies": {
		"terms_of_service": "https://website.com/termsofservices.html",
		"privacy_policy": "https://website.com/privacypolicies.html"
	  }
}
```
登录或注册后只取 /user/getSubscribe 的 token 字段，然后通过 subscription-url 来依次拼接，能尽可能的少用api就少用api，只有登录页面会用到v2board api，其余的页面全部走 oss 接口。

如果登录时 3个 api 依次检测都无法使用，提示：远程API数据获取失败，您的网络可能无法连通客户端API，请尝试使用离线版。

获取到 token 来拼接 subscription-url 的时候，如果三个 订阅url都 依次检测都无法使用 ，则提示：远程订阅API数据获取失败，您的网络可能无法连通客户端API，请检查本地网络或联系人工客服。

> https://ossapi.com/fl/api/announcement.json (公告)

```json
{
    "announcement": [
        {
            "id": 1,
            "title": "公告1",
            "time": "2025-01-03",
            "content": "内容1"
        },
        {
            "id": 2,
            "title": "公告2",
            "time": "2025-01-03",
            "content": "内容2"
        },
        {
            "id": 3,
            "title": "公告3",
            "time": "2025-01-03",
            "content": "内容3"
        },
        {
            "id": 4,
            "title": "公告4",
            "time": "2025-01-03",
            "content": "内容4"
        },
        {
            "id": 5,
            "title": "公告5",
            "time": "2025-01-03",
            "content": "内容5"
        }
    ]
}
```

按照 id 大小来进行排序，显示在首页，内容支持 html 

> https://ossapi.com/fl/api/other.json (包含客服、客户端版本等信息)

```json
{
  "version": "2.0.1",
  "ip_api": "https://api.ip.sb/geoip?callback=getgeoip",
  "question_zh": "https://zhq.tgfish.io",
  "customer_help": {
    "name": "crisp",
    "website_id": "23a4c4a3-9cc7-4405-ad1f-d966eabb4c04",
    "crisp_url": "https://go.crisp.chat/chat/embed/?website_id=23a4c4a3-9cc7-4405-ad1f-d966eabb4c04"
  },
  "updates": {
    "android": {
      "down_url": "https://down.com/xxx.apk",
      "down_url2": "https://down2.com/xxx.apk"
    },
    "windows": {
      "down_url": "https://down.com/xxx.exe",
      "down_url2": "https://down2.com/xxx.exe"
    },
    "macos": {
      "down_url": "https://down.com/xxx.dmg",
      "down_url2": "https://down2.com/xxx.dmg"
    },
    "linux": {
      "down_url": "https://down.com/linux_down.html",
      "down_url2": "https://down2.com/linux_down.html"
    }
  }
}
```

当版本号与当前不一致的时候，则弹出 更新提示，crisp那个我没想好到底是调用官方的 sdk 还是直接用url，不知道哪个更好。

> https://ossapi.com/fl/api/store.json (包含商店的内容)

```json
{
    "shop_url_api": "https://aaa.com/url.json",
    "shop_url_api2": "https://bbb.com/url.json",
    "shop_url_api3": "https://ccc.com/url.php",
    "note": [
        {
            "id": 1,
            "color": "blue",
            "content": "<b>提示: </b>所有套餐都支持按年付费，所有节点均为1x节点倍率"
        },
        {
            "id": 2,
            "color": "purple",
            "content": "<b>提示: </b>流量包产品请务必用完再购买，重复购买不会叠加流量！"
        }
    ],
    "store": [
        {
            "id": "1",
            "show": "1",
            "name": "入门",
            "mark_img": "https://xxx.com/img.png",
            "path": "plan/2",
            "describe": {
                "des1_icon_true": "流量：xxxx",
                "des2_icon_true": "线路：xxxx",
                "des3_icon_true": "速率：xxxx",
                "des4_icon_true": "并发: xxxx",
                "des5_icon_true": "解锁：xxxx",
                "des6_icon_true": "特点：xxxx"
            },
            "money": "20",
            "money_symbol": "CNY",
            "time": "月付",
            "type": "cycle"
        },
        {
            "id": "2",
            "show": "1",
            "name": "普通会员",
            "mark_img": "https://xxx.com/img.png",
            "path": "/plan/3",
            "describe": {
                "des1_icon_true": "流量：xxxx",
                "des2_icon_true": "线路：xxxx",
                "des3_icon_true": "速率：xxxx",
                "des4_icon_true": "并发: xxxx",
                "des5_icon_true": "解锁：xxxx",
                "des6_icon_true": "特点：xxxx"
            },
            "money": "30",
            "money_symbol": "CNY",
            "time": "月付",
            "type": "cycle"
        },
        {
            "id": "3",
            "show": "1",
            "name": "高级会员",
            "mark_img": "https://xxx.com/img.png",
            "path": "/plan/4",
            "describe": {
                "des1_icon_true": "流量：xxxx",
                "des2_icon_true": "线路：xxxx",
                "des3_icon_true": "速率：xxxx",
                "des4_icon_true": "并发: xxxx",
                "des5_icon_true": "解锁：xxxx",
                "des6_icon_true": "特点：xxxx"
            },
            "money": "50",
            "money_symbol": "CNY",
            "time": "月付",
            "type": "cycle"
        },
        {
            "id": "4",
            "show": "1",
            "name": "教育优惠",
            "mark_img": "https://xxx.com/img.png",
            "path": "/plan/5",
            "describe": {
                "des1_icon_true": "流量：xxxx",
                "des2_icon_true": "线路：xxxx",
                "des3_icon_true": "速率：xxxx",
                "des4_icon_true": "并发: xxxx",
                "des5_icon_true": "解锁：xxxx",
                "des6_icon_true": "特点：xxxx"
            },
            "money": "45",
            "money_symbol": "CNY",
            "time": "季付",
            "type": "cycle"
        },
        {
            "id": "5",
            "show": "1",
            "name": "进阶",
            "mark_img": "https://xxx.com/img.png",
            "path": "/plan/6",
            "describe": {
                "des1_icon_true": "流量：xxxx",
                "des2_icon_true": "线路：xxxx",
                "des3_icon_true": "速率：xxxx",
                "des4_icon_true": "并发: xxxx",
                "des5_icon_false": "解锁：xxxx",
                "des6_icon_false": "特点：xxxx"
            },
            "money": "80",
            "money_symbol": "CNY",
            "time": "月付",
            "type": "cycle"
        },
        {
            "id": "6",
            "show": "1",
            "name": "期间",
            "mark_img": "https://xxx.com/img.png",
            "path": "/plan/7",
            "describe": {
                "des1_icon_true": "流量：xxxx",
                "des2_icon_true": "线路：xxxx",
                "des3_icon_true": "速率：xxxx",
                "des4_icon_true": "并发: xxxx",
                "des5_icon_true": "解锁：xxxx",
                "des6_icon_true": "特点：xxxx"
            },
            "money": "120",
            "money_symbol": "CNY",
            "time": "月付",
            "type": "cycle"
        },
        {
            "id": "7",
            "show": "1",
            "name": "200G 流量包",
            "mark_img": "https://xxx.com/img.png",
            "path": "/plan/8",
            "describe": {
                "des1_icon_true": "流量：xxxx",
                "des2_icon_true": "线路：xxxx",
                "des3_icon_true": "速率：xxxx",
                "des4_icon_true": "并发: xxxx",
                "des5_icon_false": "解锁：xxxx",
                "des6_icon_false": "特点：xxxx"
            },
            "money": "120",
            "money_symbol": "CNY",
            "time": "一次性",
            "type": "by_flow"
        },
        {
            "id": "8",
            "show": "1",
            "name": "500G 流量包",
            "mark_img": "https://xxx.com/img.png",
            "path": "/plan/9",
            "describe": {
                "des1_icon_true": "流量：xxxx",
                "des2_icon_true": "线路：xxxx",
                "des3_icon_true": "速率：xxxx",
                "des4_icon_true": "并发: xxxx",
                "des5_icon_true": "解锁：xxxx",
                "des6_icon_true": "特点：xxxx"
            },
            "money": "230",
            "money_symbol": "CNY",
            "time": "一次性",
            "type": "by_flow"
        }
    ]
}
```
shop_url_api 是所有可用官网的数据接口，数据格式如下。
```json
[
  "https://aaa.com/#/",
  "https://bbb.com/#/",
  "https://ccc,com/#/"
]
```
通过依次检测可用 shop_url_api ，来获取官网数据，如果都显示失败，用户点击购买时则提示：部分API数据加载失败，请手动登录官网购买套餐。

获取到 shop_url_api 里的数据后，将网址数据缓存到本地（如果有变动，则覆盖本地）用户点击购买的时候，弹出提示框，提示 请选择官网登录线路，依次显示所有网址，并且去除 https:// 只显示域名，然后后面显示访问延迟，可以通过 请求`/QQ%E9%9F%B3%E4%B9%90%E5%AE%A2%E6%88%B7%E7%AB%AF.gif` 来防止跨域，获取延迟，用户点击对应网址，则直接跳转，跳转时 网址后面拼接上 path 的数据。

note 是商店页面顶部的提示框

mark_img 是 商品的头衔

des1_icon_true 显示 ✔ ，des1_icon_false 显示 × 

type 代表：按周期付费的套餐是 cycle，流量包产品是 by_flow，在商店页面分标签。
