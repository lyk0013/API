Hello API！



	{
		# 2019/05/17---重要提示
		* API开启用户验证功能，POST请求时，需要额外增加【user_info】参数 
		* 如果用户已存在，只需增加手机号信息
		* 如果为新增用户，需要额外提交邮箱、姓名、科室等详细信息
		示例一，用户已存在：
		API：http://127.0.0.1/api/journal/details
		{
			"uid":"1111",
			"user_info":{
				"phone":"13233543654"，
				"ip":"1.1.1.12"
			}
		}
		示例二，新增用户：
		{
			"uid":"1111",
			"user_info":{
				"phone":"13233543654"，
				"name":"姓名"，
				"email":"邮箱"，
				"organization":"医院名称",
				"department":"科室名称",
				"ip":"1.1.1.12"
			}
		}
	}



