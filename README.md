# 象盒工作台

### 注意事项
##### 1. 请求配置

在services目录下的conf.dart是全局请求配置文件，需要提交git。里面涉及的内容如果只是个人生效的配置，请在该目录新建一个文件local.dart中配置，这个文件不要提交git！！！
例如：请求代理，这个配置每个人不一样，所以在local.dart中配置
```
final String PROXY_URL = 'your proxy ip';
```
打包时要记得将本地代理配置清空，变成：
```
final String PROXY_URL = '';
```

##### 2. 切换正式、开发环境
在services目录下有 test.dart(测试接口)和product.dart(正式接口)文件，这两个文件只能同时存在一个，否则会报错。

##### 3. 版本更新设置
每次发布新版本需要在config/system.json中更新版本号，然后在[运营后台](http://console.xhj.com) 中发布更新信息。

##### 4. 环境设置
开发模式下 将 config/system.json 中的 env 更新为 dev。则开启开发模式。



### 路由跳转
 新路由页面需要在```/lib/router``` 下面的子模块配置新路由名称，如果子模块为新功能，请先新建子模块路由配置文件，然后在 ```/lib/router/index.dart``` 里面引入，在```class RouterConfig with```后加上子模块类名 
<details>
<summary>展开查看</summary>
<h3>Example</h3>

<code>

    import 'package:XHAPP/router.dart';
    import 'package:XHAPP/router/index.dart';

    // 普通跳转页面
    Router.push(
        context,
        name: RouterConfig().PasswordPage(params),
        params: {

        }
    )

    // 关闭当前页面，跳转到新页面
    Router.replace(
        context,
        name: RouterConfig().PasswordPage(params),
        params: {

        }
    )

    // 关闭所有页面栈，跳转到新页面
    Router.replaceAll(
        context,
        name: RouterConfig().PasswordPage(params),
        params: {

        }
    )


    // 页面回退
    Router.pop(
        context,
        name: RouterConfig().PasswordPage(params),
        params: {

        }
    )
</code>
</details>

### 网关封装（网络请求）
<details>
<summary>展开查看</summary>
<h3>Example</h3>
<code>

    import 'package:XHAPP/services/index.dart';
    Service.send(
        context,
        /**
         * 请求地址，前缀配置在 /services/conf.dart里面，
         * 如果路径是以http/https开头的会直接已这个地址去请求，
         * 否则会自动拼接 conf.dart里面的BASE_URLS
         */
        'user/getUserInfo',
        // 接口参数
        data: {

        },
        // 其他配置，同dio的Option，同时支持dio_http_cache数据缓存
        options: options,
        // 是否自动显示loading
        loading: false,
        // 是否自动显示错误信息
        error: false
    )

    // get
    Service.send(
        context,
        'user/hasBindUserPhoneCode/${username}',
    )

    // post
    Service.send(
        context,
        "user/APPLogin",
        data: {
            "account": account,
            "password": pwd,
            "mac": mac,
            "ObjectType": "4"
        },
        error: false,
        options: Options(
            method: 'POST'
        ),
    )
    
    //数据请求 缓存方式
     Service.send(
        context, XHJ_URL + '/msg/remind/pressrelease/findListV2',
        loading: false,
        error: false,
        data: {
          "messageType": "news",
          "page": 1,
          "size": 10,
          "userId": Provider.of<User>(context, listen: false).user["userId"]
        },
        options: buildServiceCacheOptions(
          options: Options(method: 'POST'), //请求类型
          maxStale: Duration(days: 7),  //缓存时效
          forceRefresh: true,   
        ));

    // 支持接口数据缓存
    import 'package:dio_http_cache/dio_http_cache.dart';
    Service.send(
        context,
        "user/APPLogin",
        data: {
            "account": account,
            "password": pwd,
            "mac": mac,
            "ObjectType": "4"
        },
        error: false,
        options: buildServiceCacheOptions(
          options: Options(method: 'POST'),
          maxStale: Duration(days: 7),
          forceRefresh: true,
        ));
    )
</code>
</details>

### 屏幕适配
<details>
<summary>展开查看</summary>
<h6>请涉及到需要使用具体长度和宽度以及字体大小的的都请使用</h6>
<h3>Example</h3>
<code>
    import 'package:flutter_screenutil/flutter_screenutil.dart';
    // 设置宽度
    ScreenUtil().setWidth(100)
    // 设置高度
    ScreenUtil().setHeight(100)
    // 设置字体大小
    ScreenUtil().setSp(26)
</code>

</details>

### 图片查看器
<details>
<summary>展开查看</summary>
<img src='http://i2.tiimg.com/713485/f0d17416b16dca83.gif' height="300">
<h3>Example</h3>
<code>
    
    import 'package:XHAPP/components/PhotoView/index.dart';
    /**
     * 打开公用图片查看器
     * @param {List<String>} urls 图片路径
     * @param {Int} index 当前查看的图片张数，默认为0，查看第一张
     */
    Photos.open(
        context, 
        urls: [
            'http://img.alicdn.com/imgextra/i4/69/O1CN01AaP5rx1CNdufYzkF5_!!69-0-luban.jpg'
        ], 
        index: 0
    );
</code>

</details>

### 本地缓存
<details>
<summary>展开查看</summary>
<h6>该项目本地缓存使用shared_preferences插件，以下记录我们保存在本地的缓存</h6>

<table>
    <tr>
        <td>字段名</td>
        <td>类型</td>
        <td>属性</td>
    </tr>
    <tr>
        <td>userInfo</td>
        <td>StringList</td>
        <td>用户属性</td>
    </tr>
</table>

</details>


### 全局弹框
<details>
<summary>展开查看</summary>
<img src='http://i2.tiimg.com/713485/d89c685df2ff314c.gif' height="300">
<code>
    
    import 'package:XHAPP/components/Alert/index.dart';
    /**
     * 打开共用弹框
     * @param {String} content 提示内容
     * @param {Function} complete 点击回调
     * @param {Function} willPop 监听关闭
     * @param {List<String>} buttons 按钮内容
     */
    AlertPop.open(
        context,
        content: '您确定限时房源是真实有效的吗？否则，一经发现房源虚假商圈经理将面临2000元的乐捐处罚！',
        complete: (int index) {
            print(index);
        },
        willPop:(){

        }
        buttons: ['1', '2']
    );
</code>

</details>

### 图片加载
<details>
<summary>展开查看</summary>
<code>

    import 'package:XHAPP/components/ImageLoader/index.dart';
    ImageLoader(
        'url',
        width: ScreenUtil().setWidth(50),
        height: ScreenUtil().setHeight(59),
        // 自定义占位图
        Widget placeholder;
        // 自定义加载失败图片
        Widget errorWidget;
        // 渲染模式，默认BoxFit.fitWidth
        BoxFit fit;
        // 是否加载适应同等尺寸的图片，默认为true
        bool adapter
    )
</code>

</details>

### 调起图片选择器
<details>
<summary>展开查看</summary>
<img src='http://i2.tiimg.com/713485/5d29793bb80e6c2f.gif' height="300">
<code>

    import 'package:XHAPP/tools/choosePhoto.dart';
    // data返回本地图片路径
    final data = await ChoosePhoto();
</code>

</details>

### 上传图片至服务器
<details>
<summary>展开查看</summary>

<code>

    import 'package:XHAPP/tools/tools.dart';
    final res = await Tools.uploadPhoto(context, 'path');
</code>

</details>

### 图片上传组件
<details>
<summary>展开查看</summary>
<img src='http://i2.tiimg.com/713485/8960c92288a08385.gif' height="300">
<code>

    import 'package:XHAPP/components/UploadPhoto/index.dart';
    /**
     * @param {List<String>} _picList 已选图片列表
     * @param {Function} changed 图片变化回调
     * @param {int} max 最多上传张数，默认为9
     */
    UploadPhoto(
        urls: _picList,
        changed: (List<String>list) {
            _picList = list;
        }
    ),
</code>
</details>

### 上拉加载更多下拉刷新
<details>
<summary>展开查看</summary>
<img src='http://i2.tiimg.com/713485/42649e1039c52485.gif' height="300">
<code>

    
    import 'package:pull_to_refresh/pull_to_refresh.dart';
    import 'package:XHAPP/components/Empty/index.dart';


    SmartRefresher(
        enablePullDown: true,//激活下拉刷新，默认开启
        enablePullUp: true,// 激活上拉加载更多，默认关闭
        controller: _refreshController,
        onRefresh: _onRefresh,
        onLoading: _getMoreData,
        child: _reviewList.length <= 0 && isLast
            ? Empty(
                content: '暂无房源数据',
            )
            : ListView.builder(
                itemCount: _reviewList.length,
                itemBuilder: (BuildContext context, int index) {
                    return _getReviewItem(_reviewList[index]);
                },
            ),
    );
</code>
</details>


### 房源选择组件
<details>
<img src="http://i1.fuimg.com/713485/c3825ddd6dfb8d4c.gif" height="300" />
<summary>展开查看</summary>

<code>

    import 'package:XHAPP/components/HouseSelect/index.dart';
    
    /**
     * 楼盘房源选择
     * @param { dynamic } buildingId 楼盘id
     * @returns Future
     */
    openHouseSelect(_currentHouse['id']).then((data) {
        if (data != null) {
            setState(() {
                _selectHouseList.add(data);
            });
        }
    });
</code>
</details>

### 公用搜索弹框
<details>
<summary>展开查看</summary>
<code>
    import 'package:XHAPP/config/SearchLayer/index.dart';
    /**
     * 公用搜索
     * @param {String} title 标题内容
     * @param {String} actions 右侧显示文字
     * @param {String} placeholder 搜索框默认显示内容
     * @param {Function} keywordChange 搜索内容变化
     * @param {Function} backButton 左上返回键 默认关闭当前页
     * @param {Widget} searchContent 搜索显示内容
     * @param {Widget} content 主体内容
     */
    SearchLayer(
      title: '选择客户',
      actions: '录入',
      placeholder: '搜索',
      keywordChange: (String keyword) {
        
      },
      backButton:(){

      },
      searchContent: Container()
      content: Container()
    )
</code>
</details>


### 分享api
<details>
<summary>展开查看</summary>
<code>
    
    import 'package:XHAPP/tools/share.dart';
    /**
    * 分享页面到微信
    * @param {String} title 分享标题
    * @param {String} text 分享内容
    * @param {String} url 分享链接
    * @param {String} icon 分享图标
    * @param {WechatTypes} type 分享类型
    */
    shareWeixin(
        title: '分享标题',
        text: '分享内容',
        url: 'http://www.xhj.com/',
        icon: 'https://wqimg.jd.com/imgproxy/n7/s150x150_jfs/t1/102605/23/15272/184679/5e6eefb8E5efb8086/e8453aca0bd7f102.jpg',
        type: WechatTypes.timeline
    );

    /**
     * 分享图片到微信
     * @param {String} image 分享图片地址
     * @param {WechatTypes} type 分享类型
     */
    shareWechatImg(
        image: widget.options['urls'][_index-1],
        type: WechatTypes.friends
    );


    /**
     * 分享页面到QQ
     * @param {String} title 分享标题
     * @param {String} text 分享内容
     * @param {String} url 分享链接
     * @param {String} icon 分享图标
     */
    shareQQ(
        title: '分享标题',
        text: '分享内容',
        url: 'http://www.xhj.com/',
        icon: 'https://wqimg.jd.com/imgproxy/n7/s150x150_jfs/t1/102605/23/15272/184679/5e6eefb8E5efb8086/e8453aca0bd7f102.jpg',
    )
    /**
     * 分享图片到QQ
     * @param {String} image 分享图片地址
     */
    shareQQImg(
        image: widget.options['urls'][_index-1],
    )

</code>
</details>


### 全局字体大小、颜色
<details>
<summary>展开查看</summary>
<code>

    // 字体
    import 'package:XHAPP/config/sizes.dart';
    {
        fontSize: Sizes.font20
    }

    // 颜色
    import 'package:XHAPP/config/colors.dart';
    {
        color: AppColors.blue
    }
</code>
</details>

### 分享弹框
<details>
<summary>展开查看</summary>
<code>

    /**
     * @param {int} type 分享类型  1:分享链接 2:分享图片
     * @param {String} name 分享平台名字   friends:微信好友   timeline:朋友圈   qq:QQ   微信小程序:wxminiapp
     * @param {WechatTypes} platform 分享平台类型
     * @param {String} image 分享图片的图片链接
     * @param {String} title 分享标题
     * @param {String} text 分享内容
     * @param {String} url 分享链接
     * @param {String} icon 分享图标
     */
    import 'package:XHAPP/components/shareDialog/index.dart';
    shareList=[
        {
        "type":2, 
        "name":"friends",
        'image':'https://wqimg.jd.com/imgproxy/n7/s150x150_jfs/t1/102605/23/15272/184679/5e6eefb8E5efb8086/e8453aca0bd7f102.jpg',
        "platform": WechatTypes.friends,
        },
        {
        "type":1, 
        "name":"timeline",
        "title":"看房单",
        "text":"精选了一些你喜欢的房源",
        "url":"http://flutter-h5.xhj.com/crm/fl_crm_showingslist?id="+kfdId,
        "icon":'https://wqimg.jd.com/imgproxy/n7/s150x150_jfs/t1/102605/23/15272/184679/5e6eefb8E5efb8086/e8453aca0bd7f102.jpg',
        "platform": WechatTypes.timeline,
        },
        {
        "type":1, 
        "name":"qq",
        "title":"看房单",
        "text":"精选了一些你喜欢的房源",
        "url":"http://flutter-h5.xhj.com/crm/fl_crm_showingslist?id="+kfdId,
        "icon":'https://wqimg.jd.com/imgproxy/n7/s150x150_jfs/t1/102605/23/15272/184679/5e6eefb8E5efb8086/e8453aca0bd7f102.jpg',
        },
        {
        "type":2, 
        "name":"qq",
        'image':'https://wqimg.jd.com/imgproxy/n7/s150x150_jfs/t1/102605/23/15272/184679/5e6eefb8E5efb8086/e8453aca0bd7f102.jpg',
        },
        {
        "type":1,
        "name":"wxminiapp",
        "title":"看房单",
        "text":"精选了一些你喜欢的房源",//分享内容
        "url":"http://flutter-h5.xhj.com/crm/fl_crm_showingslist?id="+kfdId,
        "icon":'https://wqimg.jd.com/imgproxy/n7/s150x150_jfs/t1/102605/23/15272/184679/5e6eefb8E5efb8086/e8453aca0bd7f102.jpg',
        }
    ];
    final res=await shareModalBottomSheet(shareList,title:"标题");

</code>
</details>

### 某一天、某一周、某一月、某一年的时间戳和时间
<details>
<summary>展开查看</summary>
<code>    
    
    import 'package:common_utils/common_utils.dart';
    import 'package:XHAPP/xhPages/commonality/TimeMachineUtil .dart';

    TimeMachineUtil.getTimestampLatest(true, 3));//返回的是时间戳  未来第三天
    TimeMachineUtil.getTimestampLatest(true, -3));//返回的是时间戳  过去的第三天
    DateUtil.formatDate(DateTime.fromMillisecondsSinceEpoch(TimeMachineUtil.getTimestampLatest(true, 3)),format: 'yyyy-MM-dd');
    
    TimeMachineUtil.getWeeksDate(0); //本周
    TimeMachineUtil.getWeeksDate(1); //下一周
    TimeMachineUtil.getWeeksDate(-8); //之前的第8周
    TimeMachineUtil.getWeeksDate(10); //未来的第10周
    
    TimeMachineUtil.getMonthDate(1); //下个月
    TimeMachineUtil.getMonthDate(0); //这个月
    TimeMachineUtil.getMonthDate(4); //未来第四个月
    TimeMachineUtil.getMonthDate(-6); //过去的第六个月
</code>
</details>

### 公用房源筛选组件
<details>
<summary>展开查看</summary>
<code>

    import 'package:XHAPP/components/HouseScreening/screenBar.dart';
    ScreenBar(
        tabList:  [{
            'name': '区域',
            'id': 1,
            'type': ScreenTypes.region,
            'content': null,
            'max': 5,
            'confirm': (Map data) {}
        },{
            'name': '价格',
            'id': 2,
            'type': ScreenTypes.price,
            'confirm': (Map data) {},
            'content': [
                {
                'name': '不限',
                'start': null,
                'end': null,
                },
                {
                'name': '0-50万',
                'start': 0,
                'end': 50,
                },
            ],
        }, {
            'name': '户型',
            'id': 3,
            'type': ScreenTypes.model,
            'max': 3,
            'content': null,
            'confirm': (List list) {}
        },{
            'name': '面积',
            'id': 4,
            'type': ScreenTypes.area,
            'confirm': (Map data) {},
            'content': [
                {
                'name': '不限',
                'start': null,
                'end': null,
                },
                {
                'name': '50m²以下',
                'start': 0,
                'end': 50,
                },
            ],
        }, {
            'name': '更多',
            'id': 5,
            'type': ScreenTypes.more,
            'content': [
                {
                'title': '面积',
                'max': 1,
                'subs': [
                    {'name': '50㎡以下', 'id': '0-50'},
                ]
                },
                {
                'title': '标签',
                'max': 2,
                'subs': [
                    {'name': '钥匙', 'id': '2'},
                ]
                },
            ],
            'confirm': (Map data) {}
        }, {
            'name': '排序',
            'id': 6,
            'type': ScreenTypes.sort,
            'confirm': (Map data) {},
            'content': [
                {'name': '默认', 'id': null},
                {'name': '均价从低到高', 'id': 1},
            ]
        }],
    );
</code>
</details>


### 公用搜索框
<details>
<summary>展开查看</summary>
<code>

    import 'package:XHAPP/components/searchInput/index.dart';
    SearchInput(
        leading: (BuildContext context) {
            return Icon(
                Feather.search,
                color: Color.fromRGBO(106, 109, 112, 1),
                size: ScreenUtil().setWidth(40),
            );
        },
        enable: false,
        hintText: '搜小区',
    ),
</code>
</details>


### 公用房源列表组件
房源组件有新房、租房、二手房、商业地产、海外地产5种类型的组件。组件文件都在 lib/components/houseListItem 下面。


### 其它页面呼起微聊窗口的调用方法：
<details>
<summary>房源消息时：'title', 'price', 'desc', 'img', 'houseid', 'housetype' 消息内容中必须包含这些房源信息。 kf属性与bid，bname，bavatar互斥。kf字段取值范围[ esf   zf  xf] </summary>
<code>   
Router.push(
  context,
  name: RouterConfig().ChatDetailPage({
    'bid': 经纪人ID,
    'bname': 经纪人名称,
    'bavatar': 经纪人头像,
    'kf': 'esf',
    'message':{
          'type': 'text', // text  house
          'content': '文本消息' or {'title', 'price', 'desc', 'img', 'houseid', 'housetype'}
      }
  }),
);
</code>
</details>


### 发送验证码
<details>
<summary>新增验证码统一发送api，后续所有短信发送都需要调用改api，除非有特殊要求</summary>
<code>   

    Tools.sendSms(
        _phoneController.text,
        handler: (bool status) {
            if (status) {
            isSend = false;
            num = 60;
            daojishu();
            }
        },
    );
</code>
</details>








