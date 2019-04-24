#Version：0.0.0
#Date：2019/04/19

	#2019/04/22
	1. 增加排序功能
	2. 增加高亮功能
	3. 增加题录输出功能
	#2019/04/23
	1. 修复allfield 字段名错误
	2. 修复排序嵌套错误
	3. 增加过滤器统计参数


#基本检索
1. 关键词检索
2. 单篇引文匹配器
3. 单条文献详情
4. 题录输出

##关键词检索
>FMRS最主要的检索功能  
>URL：http://127.0.0.1:8000/api/searcher/results/  
>请求方法：POST  
###传入的Json请求体  

	{
		"query":"lung[ti] AND liver[ad] AND 1800 TO 2019[pdat] AND 0.001 TO 50.236[if]", 
		"aggs":"cluster" //cluster:聚类关联；statistics：统计分析，filters: 过滤器，默认为None
		// 可选参数
		//分页
		"size"：10，//默认每页10条
		"start":0, //默认0
		//排序
		"sort_field":if, //pubyear OR if
		"sort_order":asc, //asc OR desc 
		//高亮
		"highlight":true //true OR false, 目前默认且只支持题名字段高亮
	}

###返回的Json

	{
		"status":"success", // success or fail
		"total":100， //总数
		"results":[
			{
				'journal': 'Chest surgery clinics of North America',
				'au': 'de Perrot M;Keshavjee S',
				'sjr': None,
				'if': None,
				'doi': '',
				'volume': '13',
				'page': '443-62',
				'title': 'Lung transplantation. Lung preservation.',
				'pmid': 13678307,
				'issue': '3',
				'fau': 'de Perrot Marc;Keshavjee Shaf'
			},  {
				'journal': 'Schweizerische Rundschau für Medizin Praxis = Revue suisse de médecine Praxis',
				'au': 'Héritier F;Rochat T',
				'sjr': None,
				'if': None,
				'doi': '',
				'volume': '82',
				'page': '581-2',
				'title': '[Heart-lung transplantation, single lung and both lungs].',
				'pmid': 8506439,
				'issue': '19',
				'fau': 'Héritier F;Rochat T'
			}
		],
		"aggs":[
				'mdi': [{
					'key': 'd008175',
					'doc_count': 55194
				}, {
					'key': 'd002289',
					'doc_count': 15301
				}],
				'mzy': []
		],
        "highlight" : {
          "title" : [
            "Increased 18F-FDG uptake within the reticuloendothelial system in patients with active <a>lung<b> cancer on"
          ]
        }
	}

##单篇引文匹配器
>单篇引文匹配，使用关键词检索接口，不适用单独接口；  
>但需要对该功能需要的参数进行说明

###可选参数
	{
		// 以下均为可选参数， 但至少要有一个参数
		// 强调：检索式中不支持出现逻辑运算符
		"jt":"lung",
		"pubyear":2017,
		"volume":1,
		"issue":2,
		"page"：3,
		"au":"li", //如果勾选第一作者，字段名改为【bau】; 末位，则改为【eau】
		"ti":"lung cancer"
	}
###以上可选参数需要组合成标准检索式，传入关键词检索接口进行处理
>第一作者和末尾作者暂不支持，后续会添加

	{
		"query":"lung[journal] AND li[au]"
	}

##单条文献详情
>查询Mysql，返回单条文献详细信息
>URL:http://127.0.0.1:8000/api/searcher/details/  

###传入Json
	{
		"pmid":20411166
	}

###返回的Json
	{
		"status":"success", //success OR fail
		"pmid":20411166,
		"details":{
			"ddi": "",
			"mzy": "",
			"filters": "TE;EN",
			"country": "Greece",
			"emi": "",
			"chemical": "Radiopharmaceuticals;Fluorodeoxyglucose F18",
			"volume": "13",
			"mdg": "D049268",
			"ptype": "Journal Article",
			"keywords": "",
			"dpr": "",
			"pmid": 20411166,
			"mdi": "D008175",
			"pubyear": 2010,
			"dre": "",
			"title": "Increased 18F-FDG uptake within the reticuloendothelial system in patients with active lung cancer on PET imaging may indicate activation of the systemic immune response.",
			"major": "D019788;D008175;D012157;D049268",
			"issn": "1790-5427",
			"if": 1.008,
			"issue": "1",
			"fau": "Bural Gonca G;Torigian Drew A;Chen Wengen;Houseni Mohamed;Basu Sandip;Alavi Abass",
			"edr": "",
			"id": 22669476,
			"edi": "",
			"pubdate": "2010-01-01",
			"dhs": "D000368;D005260;D006801;D007113;D008175;D008297;D008875;D012157;D049268",
			"subject": null,
			"dps": "",
			"pmcid": "",
			"ddt": "",
			"mds": "",
			"sjr": "0.331",
			"abb": "Hell J Nucl Med",
			"abstract": "The reticuloendothelial system (RES) cells are in the defense against certain pathogens,ing.",
			"mdo": "D000368;D005260;D006801;D007113;D008297;D008875;D012157",
			"au": "Bural GG;Torigian DA;Chen W;Houseni M;Basu S;Alavi A",
			"score": 4015,
			"jid": "101257471",
			"number": "M01.060.116.100;X999999;D09.254.229.500;Y05.060D09.254.229.500;Y05.060WWD09.254.229.500;Y9.871;D27.720.470.410.650;Y05.060D27.720.470.410.650;Y08.030D27.720.470.410.650;",
			"ean": "D012157",
			"msy": "",
			"journal": "Hellenic journal of nuclear medicine",
			"mids": "D000368;D005260;D019788;Q000276;D006801;D007113;Q000276;D008175;Q000000981;Q000276;D008297;D008875;D012157;Q000000981;Q000276;D049268;Q000379;D019275;Q000276",
			"question": null,
			"mesh": "Aged;老年人@@Female;女(雌)性@@Fluorodeoxyglucose F18/immunology*;氟脱氧葡萄糖F18/免疫学*@@Humans;人类@@Immunity, Innate/immunology;免疫, 先天/免疫学@@Lung Neoplasms/diagnostic imaging*;肺肿瘤/影像诊断*@@Lung Neoplasms/immunology*;肺肿瘤/免疫学*@@Male;男(雄)性@@Middle Aged;中年人@@Mononuclear Phagocyte System/diagnostic imaging*;单核巨噬细胞系统/影像诊断*@@Mononuclear Phagocyte System/immunology*;单核巨噬细胞系统/免疫学*@@Positron-Emission Tomography/methods*;正电子发射断层摄影术/方法*@@Radiopharmaceuticals/immunology;放射性药物/免疫学@@",
			"mdr": "D019788;D019275",
			"ad": "Division of Nuclear Medicine, Department of Radiology, University of Pennsylvania, School of Medicine, USA.",
			"dph": "",
			"language": "eng",
			"hindex": "12",
			"page": "23-5",
			"epr": "",
			"doi": "",
			"mdy": "",
			"eph": "D007113",
			"dra": "",
			"conclusion": "",
			"dsu": "",
			"combination": "D019788D008175;D019788D012157;D019788D049268;D008175D012157;D008175D049268;D012157D049268"
		}
	}

##题录输出
>FMRS 题录输出，默认输出1000条
>URL:http://127.0.0.1:8000/api/searcher/exports/  

###传入的Json

	{
		"pmids":"1,2,3,4"  //PMID， 最后不要有逗号
	}

###返回的json 

	{	
		//此为样式，正式返回的字段为全字段
	    "results": [
	        {
	            "title": "Formate assay in body fluids: application in methanol poisoning.",
	            "pmid": 1
	        },
	        {
	            "title": "Delineation of thequeous solution.",
	            "pmid": 2
	        },
	        {
	            "title": "Metal substitutions incarbonic anhydrase: a halide ion probe study.",
	            "pmid": 3
	        },
	        {
	            "title": "Effect of chloroquinke.",
	            "pmid": 4
	        }
	    ],
	    "status": "success"
	}