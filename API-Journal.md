# Version：0.0.1
# CreateDate：2019/04/22

	#2019/04/22
	1. 修改首字母排序，需要传入小写参数
	2. 修改期刊主题词导航检索字段，修改为c_keywords
	3. 修复c_keywords中文分词
	4. 期刊详细信息中添加影响因子字段
	#2019/04/28
	1. 修复分区处理参数细节错误
	2. 修复五个可选过滤参数，默认赋值问题

# 期刊接口
1. 获取期刊详情
2. 自动提示
3. 检索

# 主要参数简介
>请求方式：POST  
>请求体：JSON  
>检索词: query  
>起始ID号：start  
>每页条数： size  


## 获取期刊详情
> 通过期刊ID，获取期刊详情  
> URL：http://127.0.0.1:8000/api/journal/details/  
> 传入的Json参数  

    {
		"uid":"0370647"
	}

> 返回的Json参数  

    {
		"status":"success", //成功Success，失败Fail
		"total":1,
		"uid":"0370647",
		"details":{
			"ename" : "CA: a cancer journal for clinicians",
			"country" : "United States",
			"cname" : "癌",
			.........
		},
		"included":["SCIE", "Medline"],
		"dvi":null //暂时未做，后续添加
		"rank":{
			"wos":[
				{
					"subject":"CHEMISTRY, PHYSICAL",
					"ranking":1,
					"total":147,
					"quartile":1
				},
				{
					"subject":"PHYSICS, APPLIED",
					"ranking":1,
					"total":146,
					"quartile":1
				}
			],
			"cas":{
				"major":[
					{
						"subject":"工程技术",
						"section":1
					},
					{
						"subject":"工程技术",
						"section":1
					}
				],
				"sub":[
					{
						"subject":"CHEMISTRY, PHYSICAL",
						"cname":"物理化学",
						"section":1
					},
					{
						"subject":"PHYSICS, CONDENSED MATTER",
						"cname":"物理：凝聚态物理",
						"section":1
					}
				]
			}
		}
	}

## 自动提示
>根据用户输入字母，自动提示用户可能想输入的词，也称自动补全   
>URL: http://127.0.0.1:8000/api/journal/suggests/  
>传入的Json参数    

	{
	    "query":"lun"，
	    "size": 10 //可选参数， 默认返回前十条
	}
    
> 返回的Json参数  

	{
	    "status": "correct", //correct: 正常的补全，rectify：未找到该词，纠正后补全, wrong:空字符
	    "suggestions":['lung', "lung cancer"] //返回的为列表形式的建议词
	}

## 检索
>通过中图分类号、文本框以及主题词分类号，检索期刊  
>URL：http://127.0.0.1:8000/api/journal/results/ 
>传入的Json参数   

	{
	    "query":"lun"，//必要参数，传入空可返回全部
		"szie":10, //可选参数， 每页大小
		"start":0,  //可选参数， 起始值
		"sort_field":if,  //可选参数， 排序字段
		"sort_rank":asc //asc或desc
		//以下五个参数为可选参数
		"alpha_order":"0 a", //首字母需小写
		"language":"eng",
		"filters":"SC",
		"classification":"R5*",
		"c_keywords":"妇科学"
	}

> 返回的Json参数  

	{
		"status": "success", //成功Success，失败Fail
		"total":100,
		"text":"lun",
		"results":[
			{
	          "abb" : "Bull Inst Natl Sante Rech Med",
			  "uid" : "0001640",
	          "ename" : "Bulletin de l'Institut national de la santé et de la recherche médicale",
	          "cname" : "桑德勒国家研究所公报",
	          "filters" : null,
	          "if" : null,
	          "ig" : null
	        },
			{
	          "abb" : "Bull Inst Natl Sante Rech Med",
			  "uid" : "0001640",
	          "ename" : "Bulletin de l'Institut national de la santé et de la recherche médicale",
	          "cname" : "桑德勒国家研究所公报",
	          "filters" : null,
	          "if" : null,
	          "ig" : null
	        }
		],
		"statistics":[
				{
		          "key" : "cr",
		          "doc_count" : 117
		        },
		        {
		          "key" : "pa",
		          "doc_count" : 2617
		        },
		        {
		          "key" : "md",
		          "doc_count" : 5230
		        },
		        {
		          "key" : "sc",
		          "doc_count" : 6468
		        }
		]

	}


