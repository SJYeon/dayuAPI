#dayuAPI
1.功能
 所有接口均可采用GET和POST方式请求。
 所有接口请求和返回数据均采用UTF-8字符编码。
 所有接口返回数据格式为JSON格式。
 所有接口返回数据时间格式为：yyyy-MM-dd HH:mm:ss
 
2.
对外提供一个可访问的url，该url接收一些参数，根据参数进行查询数据，并将查询到的数据以JSON格式返回；

请求的统一地址：http://120.27.111.211:8089/DaYu/interface/

3.返回值

参数说明：

	字段	类型	         可为空	
	flag	Boolean	          false	成功true，否则false
	data	JONSON数组或对象  false	数据体
	info	String	          false	附加信息

4.状态码说明

	状态值	状态值说明
	9000	正常数据
	9001	非正常数据
	1433	数据库链接异常
	404	    无法找到页面
	500	    服务器内部错误
	503	    禁止访问权限

商家部分接口说明：
一、商家登录（店铺登录）

/manage/login.action

	请求参数说明:
	
	字段			类型	可为空	备注
	userName		String	false	用户帐号 
	userPwd			String	false	 密码
	
	返回字段说明:
	adminId		String	 	店铺管理员编号
	userName	String		登录名
	adminState	String		账户的状态（0正常，99禁用）

{测试数据：userName=admin  userPwd=123456}

返回成功数据:
{"flag":true,"data":{"adminId":2,"userName":"admin","adminState":0},"info":"登录成功"}
	备注：此接口只用于登录，因为个人中心页面数据是动态数据，故需要从新请求统计数据接口。

返回失败数据:
{"flag":false,"data":{},"info":"帐号或密码错误"}


二、
商家中心：
/manage/admincenter.action


参数：

	字段		类型	可为空	 备注
	adminId		int		false	店长（商家）id

返回参数：对象JF_AdminCenterDTO

		adminId	 		商家id
		floor			商家所在楼
		shopLogo		商家logo
		moneyOfToday	今日收益
		orderNumOfToday	今日订单数
		shopSiteName	店铺名称
		shopState		店铺状态
		shopId			商铺id
		
{测试数据：adminid=2}
{"flag":true,"data":{"adminId":2,"floor":"西电92号楼 ","shopId":1,"shopSiteName":"大胖超市","shopState":1,"orderNumOfToday":0,
"moneyOfToday":0,"shopLogo":"e:/temp_upload_file/1.jpg"},"info":"正常数据！"}

三、商家修改密码（店铺修改密码）

/users/updatePwd.action

请求参数说明:
	字段		类型	可为空	备注
	
	userName	String	 false	用户帐号
	userPwd		String	 false	密码

返回成功数据:
{"flag":true,"type":9000,"info":"密码修改成功!"}

返回失败数据:
{"flag":false, "type":9001,"info":"密码修改失败"}


四、已下单订单列表查询
/manage/getOrdersPlace.action
请求参数说明:

	字段		类型	可为空	备注
	
	shopId		int		false	商铺id

	  
	
返回参数：对象List(JF_Goods_ProductOrderListDTO)


	字段			类型		可为空	备注
	orderNo			String		false	订单号
	productName		String		False	商品名称
	allNumber		int			False	全部商品数量
	productImage	String		Yes		商品图片
	orderType		int			False	订单类型（0 五分钟购物  1 超市购物 2限时抢购）
	orderState		int			False	订单状态  0待支付 1 待收货 2已完成 3、待发货
	RealPrice		BigDecimal	False	实际付款价格
	OverTime		data		yes		已完成时间
	

返回成功数据：

{"flag":true,"data":[{"orderState":1,"allNumber":11,"orderNo":"1153413513846680","orderType":0,"productImage":"/img/p.png","number":4,"RealPrice":11.5,"productPrice":4,"productName":"泡面9"},
{"orderState":3,"allNumber":8,"orderNo":"1153413513846681","orderType":1,"productImage":"/img/p.png","number":4,"RealPrice":11.5,"productPrice":3.8,"productName":"泡面3"},
{"orderState":3,"allNumber":10,"orderNo":"1153413513846682","orderType":0,"productImage":"/img/p.png","number":2,"RealPrice":11.5,"productPrice":3.8,"productName":"泡面6"},
{"orderState":3,"allNumber":14,"orderNo":"1153413513846683","orderType":0,"productImage":"/img/p.png","number":5,"RealPrice":11.5,"productPrice":3.8,"productName":"泡面2"}],"info":"正常数据！"} 

四-1、全部已完成订单列表查询
/manage/getOrdersOverAll.action
请求参数说明:

	字段		类型	可为空	备注
	shopId		int		false	商铺id
	    
	
返回参数：对象List(JF_Goods_ProductOrderListDTO)


	字段			类型		可为空	备注
	orderNo			String		false	订单号
	productName		String		False	商品名称
	allNumber		int			False	全部商品数量
	productImage	String		Yes		商品图片
	orderType		int			False	订单类型（0 五分钟购物  1 超市购物 2限时抢购）
	orderState		int			False	订单状态  0待支付 1 待收货 2已完成 3、待发货
	RealPrice		BigDecimal	False	实际付款价格
	OverTime		data		yes		已完成时间（刷新操作的时间节点）

返回成功数据：

{"flag":true,"data":[{"orderState":2,"allNumber":5,"orderNo":"1153413513843443","orderType":2,"productImage":"/img/p.png","number":3,"RealPrice":11.5,"productPrice":3.8,"productName":"泡面2"},
{"orderState":2,"allNumber":11,"orderNo":"1153413513843466","orderType":1,"productImage":"/img/p.png","number":6,"RealPrice":11.5,"productPrice":3.8,"productName":"泡面3"},
{"orderState":2,"allNumber":8,"orderNo":"1153413513843455","orderType":0,"productImage":"/img/p.png","number":8,"RealPrice":11.5,"productPrice":4,"productName":"泡面4"},
{"orderState":2,"allNumber":5,"orderNo":"1153413513846678","orderType":0,"productImage":"/img/p.png","number":3,"RealPrice":11.5,"productPrice":4,"productName":"泡面7"}],"info":"正常数据！"} 

四-2、分页已完成订单列表查询
/manage/getOrdersOver.action?
请求参数说明:
	字段		类型	可为空	备注
	
	shopId		int		false	商铺id
	pageNum		inT		False	页数 比如每页12个 需要第2页的 则pangeNum为12*2=24
	
	
返回参数：对象List(JF_Goods_ProductOrderListDTO)


	字段			类型		可为空	备注
	orderNo			String		false	订单号
	productName		String		False	商品名称
	allNumber		int			False	全部商品数量
	productImage	String		Yes		商品图片
	orderType		int			False	订单类型（0 五分钟购物  1 超市购物 2限时抢购）
	orderState		int			False	订单状态  0待支付 1 待收货 2已完成 3、待发货
	RealPrice		BigDecimal	False	实际付款价格
	OverTime		data		yes		已完成时间（刷新操作的时间节点）

返回成功数据：

{"flag":true,"data":[{"orderState":2,"allNumber":5,"orderNo":"1153413513843443","orderType":0,"productImage":"./img/3.jpg","number":3,"RealPrice":11.5,"productPrice":3.8,"productName":"泡面2"},
{"orderState":2,"allNumber":11,"orderNo":"1153413513843466","orderType":0,"productImage":"./img/3.jpg","number":6,"RealPrice":11.5,"productPrice":3.8,"productName":"泡面3"},
{"orderState":2,"allNumber":8,"orderNo":"1153413513843455","orderType":0,"productImage":"./img/3.jpg","number":8,"RealPrice":11.5,"productPrice":4,"productName":"泡面4"}],"info":"正常数据！"}

四-3、已完成订单的刷新操作
/manage/refreshOrdersOver.action
请求参数说明:
	字段		类型	可为空	备注
	
	shopId		int		false	商铺id
	timePoint	String	false	用SimpleDateFormat将日期类型转换为字符串  格式为Type类中的 PATTERN_DATETIME("yyyy-MM-dd HH:mm:ss") 格式
	
	
返回参数：对象List(JF_Goods_ProductOrderListDTO)


	字段			类型		可为空	备注
	orderNo			String		false	订单号
	productName		String		False	商品名称
	allNumber		int			False	全部商品数量
	productImage	String		Yes		商品图片
	orderType		int			False	订单类型（0 五分钟购物  1 超市购物 2限时抢购）
	orderState		int			False	订单状态  0待支付 1 待收货 2已完成 3、待发货
	RealPrice		BigDecimal	False	实际付款价格
	OverTime		data		yes		已完成时间（刷新操作的时间节点）
	
timePoint 为 2016-01-19 12:13:22 返回结果如下
返回成功数据：

{"flag":true,
"data":[{"orderState":2,"allNumber":2,"orderNo":"1153413513846699","overTime":"Tue Jan 19 12:13:26 CST 2016","orderType":0,"productImage":"/img/p.png","number":2,"RealPrice":11.5,"productPrice":3.8,"productName":"泡面5"},
{"orderState":2,"allNumber":2,"orderNo":"1153413513846698","overTime":"Tue Jan 19 12:13:25 CST 2016","orderType":0,"productImage":"/img/p.png","number":2,"RealPrice":11.5,"productPrice":3.8,"productName":"泡面7"},
{"orderState":2,"allNumber":2,"orderNo":"1153413513846697","overTime":"Tue Jan 19 12:13:24 CST 2016","orderType":0,"productImage":"/img/p.png","number":2,"RealPrice":11.5,"productPrice":3.8,"productName":"泡面5"},
{"orderState":2,"allNumber":2,"orderNo":"1153413513846696","overTime":"Tue Jan 19 12:13:23 CST 2016","orderType":0,"productImage":"/img/p.png","number":2,"RealPrice":11.5,"productPrice":3.8,"productName":"泡面4"}],
"info":"正常数据！"}

五、订单详情
/manage/getordersdetail.action
参数：

	字段		类型		可为空		备注
	orderNo 	String		false		订单号

返回参数：对象JF_Goods_ProductOrdersDTO

	字段		类型 		可为空		备注	
	orderNo		String 		false		订单号
	realPrice	BigDecimal	false		实际付款价格
	orderDate	Date		false		订单生成时间
	payDate		data		false		付款时间
	
	
	
（返回数据有待测试）
返回成功数据：

{"flag":true,"data":{"orderPayment":1,"orderState":2,"orderNo":"1153413513843443","orderDate":"Thu Jan 14 12:13:12 CST 2016","orderName":"小二",
"orderAddress":"老科楼了","payDate":"Wed Feb 11 12:13:13 CST 2015","realPrice":null,"orderTel":"18292880890"},"info":"正常数据！"}


六、修改订单状态
/manage/updateorderstate.action
参数说明：
	字段		类型		可为空		备注

	orderNo		String		false		订单号
	orderState	int			false		订单状态

返回成功数据：

{"flag":true,"type":9000,"info":"修改成功"}

七、商铺信息
manage/shopinfo.action
参数说明：

	字段		类型		可为空		备注
	shopId		int 		false		商铺id

返回类型：对象JF_Goods_ShopUserDTO

返回成功数据：

{"flag":true,"data":{"shopAddress":"你猜","shopId":null,"name":"张思","shopSiteName":"大胖超市",
"shopDesc":"大胖超市大胖超市大胖超市大胖超市","shopState":null,"telephone":"18292880897","shopLogo":"e:/temp_upload_file/1.jpg"},"info":"查询成功"}

八、修改商铺信息
manage/updateshopinfo.action
参数说明：
对象：JF_ShopUserPTO

返回成功数据：

{"flag":true,"type":9000,"info":"修改成功！"}


九、商品管理
（1）/manage/addProduct.action                                        添加商品

（2）/manage/modfiyInventory.action                                修改商品库存

（3）/manage/deleteProduct.action                                      删除商品


传入参数
（1）
	字段			类型	可为空	备注

	adminId			int		false	商家id
	productId		int		false	商品id
（2）
	字段			类型	可为空	备注

	adminId			int		false	商家id
	productId		int		false	商品id
	productQuality	int		false	库存数量
（3）
	字段			类型	可为空	备注

	adminId			int		false	商家id
	productId		int		false	商品id

返回成功数据

{"flag":true,"type":9000,"info":"商品添加成功!"}

{"flag":true,"type":9000,"info":"库存修改成功!"}

{"flag":true,"type":9000,"info":"商品删除成功!"}

十、修改店铺状态

/manage/shopState.action

参数说明：
	字段		类型		可为空		备注

	shopId	    int			false		店铺id
	

返回成功数据：

{"flag":true,"type":9000,"info":"店铺状态修改成功!"}


十一、商家商品管理页面
/manage/getmygoods.action
参数说明：
	字段			类型	可为空		备注

	adminId			int	 	false		商家id

返回对象：List (JF_GoodsProductRelationDTO)

返回成功数据：

{"flag":true,"data":[{"ProductQuality":12,"originalPrice":4.5,"productImage":"./img/3.jpg","currentPrice":3.8,"productName":"泡面3","productId":2,"relationId":2}],"info":"正常数据！"}

十二、商品添加页面

/manage/getschoolgoods.action
参数说明：
	字段		类型		可为空		备注
	adminId		int			false		商家id

返回对象：List(JF_GoodsProductDTO)
		
		返回里边有个isAdd  这个表示时候已经添加了  如果是true  标明已添加  

返回成功数据：
{"flag":true,"data":[{"originalPrice":4.5,"isAdd":false,"productImage":"./img/3.jpg","currentPrice":3.8,"productName":"泡面10","productId":10},
{"originalPrice":4.5,"isAdd":false,"productImage":"./img/3.jpg","currentPrice":3.8,"productName":"泡面9","productId":9},
{"originalPrice":4.5,"isAdd":false,"productImage":"./img/3.jpg","currentPrice":3.8,"productName":"泡面8","productId":8}],"info":"正常数据！"}

十三、我的钱包页面
/manage/adminAccount.action

参数对象：
	字段		类型		可为空		备注
	shopId		int			false		商铺id
	
返回对象：JF_AccountDTO

返回成功数据

{"flag":true,"data":{"moneyOfMonth":92,"moneyOfDay":0,"totalCash":470,"totalAmount":450},"info":"正常数据！"}
十四、提现明细

/manage/getAccountDetails.action

参数对象：
	字段		类型		可为空		备注
	shopId		int			false		商铺id
	pageNum		int			false		页面数 同上
	
返回对象：List（JF_AccountDetailsDTO）

{"flag":true,"data":[{"createTime":"Wed Jan 27 12:13:19 CST 2016","serialId":9,"totalCash":0,"totalAmount":0,"cashAmount":50,"amountState":1},
{"createTime":"Wed Jan 27 12:13:18 CST 2016","serialId":8,"totalCash":0,"totalAmount":0,"cashAmount":50,"amountState":1},
{"createTime":"Wed Jan 27 12:13:17 CST 2016","serialId":7,"totalCash":0,"totalAmount":0,"cashAmount":50,"amountState":1},
{"createTime":"Wed Jan 27 12:13:16 CST 2016","serialId":6,"totalCash":0,"totalAmount":0,"cashAmount":50,"amountState":1},
{"createTime":"Wed Jan 27 12:13:15 CST 2016","serialId":5,"totalCash":0,"totalAmount":0,"cashAmount":50,"amountState":1},
{"createTime":"Wed Jan 27 12:13:14 CST 2016","serialId":4,"totalCash":0,"totalAmount":0,"cashAmount":50,"amountState":1},
{"createTime":"Wed Jan 27 00:00:00 CST 2016","serialId":3,"totalCash":0,"totalAmount":0,"cashAmount":50,"amountState":1},
{"createTime":"Fri Jan 15 00:00:00 CST 2016","serialId":2,"totalCash":0,"totalAmount":0,"cashAmount":20,"amountState":1},
{"createTime":"Thu Jan 14 00:00:00 CST 2016","serialId":1,"totalCash":0,"totalAmount":0,"cashAmount":100,"amountState":1}],"info":"正常数据！"}

十五、系统设置页面
/manage/systemSet.action

参数说明：
	字段 		类型 		可为空		备注
	shopId		inT			false		商铺id
	
返回值：
	字段			类型		可为空		备注
	shopState		int			false		商铺的状态
	
返回成功数据：
{"flag":true,"shopState":1}
十六、收入明细
/manage/getIncomeDetail.action
参数说明：
	字段		类型		可为空 		备注
	shopId		inT			false		商铺id
返回值：
	字段			类型		可为空		备注
	overTime		Date		false		时间
	productImage	String		false		图片
	RealPrice		BigDecimal 	false		收入

返回数据:
{"flag":true,
"data":[{"overTime":"Tue Jan 19 12:13:26 CST 2016","productImage":"/img/p.png","RealPrice":11.5},{"overTime":"Tue Jan 19 12:13:25 CST 2016","productImage":"/img/p.png","RealPrice":11.5},
{"overTime":"Tue Jan 19 12:13:24 CST 2016","productImage":"/img/p.png","RealPrice":11.5},{"overTime":"Tue Jan 19 12:13:23 CST 2016","productImage":"/img/p.png","RealPrice":11.5},
{"overTime":"Tue Jan 19 12:13:22 CST 2016","productImage":"/img/p.png","RealPrice":11.5},{"overTime":"Tue Jan 19 12:13:21 CST 2016","productImage":"/img/p.png","RealPrice":11.5},
{"overTime":"Tue Jan 19 12:13:20 CST 2016","productImage":"/img/p.png","RealPrice":11.5},{"overTime":"Tue Jan 19 12:13:19 CST 2016","productImage":"/img/p.png","RealPrice":11.5},
{"overTime":"Tue Jan 19 12:13:18 CST 2016","productImage":"/img/p.png","RealPrice":11.5},{"overTime":"Tue Jan 19 12:13:17 CST 2016","productImage":"/img/p.png","RealPrice":11.5},
{"overTime":"Sat Jan 16 12:13:15 CST 2016","productImage":"/img/p.png","RealPrice":11.5},{"overTime":"Fri Jan 15 12:13:16 CST 2016","productImage":"/img/p.png","RealPrice":11.5},
{"overTime":"Wed Feb 11 12:13:14 CST 2015","productImage":"/img/p.png","RealPrice":11.5}],"info":"正常数据！"}

	
	