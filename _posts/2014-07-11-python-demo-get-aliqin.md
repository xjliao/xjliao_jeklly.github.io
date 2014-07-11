---
layout: post
title: "Python 抓取某通信号码"
categories:
- Python
tags:
- Python


---
Python抓取某通信号码  

>code_zh.txt 内容格式：省编码-城市编码:省名-城市名 每行一条记录  
code_en.txt 内容格式和code_zh一样，中文改为汉语拼音即可,例如:11-1101:BeiJing-BeiJing  

{% highlight python linenos%}
11-1101:北京市-北京市
31-3101:上海市-上海市
32-3201:江苏省-南京市
32-3202:江苏省-无锡市
32-3203:江苏省-徐州市
32-3205:江苏省-苏州市
32-3209:江苏省-盐城市
33-3301:浙江省-杭州市
33-3302:浙江省-宁波市
33-3303:浙江省-温州市
33-3307:浙江省-金华市
33-3310:浙江省-台州市
44-4401:广东省-广州市
44-4403:广东省-深圳市
44-4404:广东省-珠海市
44-4406:广东省-佛山市
44-4413:广东省-惠州市
44-4419:广东省-东莞市
44-4420:广东省-中山市
50-5001:重庆市-重庆市
{% endhighlight %}

send.py

{% highlight objective-c linenos%}
#!/usr/bin/python
# encoding: utf-8
import os
import json

# Test 数据
# proDics = {"32":"JiangSu"};
# codeDics = [{"32":"3201"} ,{"32":"3202"}]
# cityDics = {"323201":"NanJing", "323202":"XuZhou"}

# 从文件读取省，城市编码
# 格式为：省编码-城市编码：省名称-城市名称
codeFile = open('code_zh.txt', 'r')
#print f.readlines()

# 存放省编码，地市编码，省包含的地市编码
# 格式为如下
# 省：proDics = {"省编码":"省名称"}
# 省包含的城市编码,一个省有多个地市：codeDics = [{"省编码":"城市编码1"}，{"省编码":"城市编码2"}]
# 城市：cityDics = {"省编码 + 城市编码":"城市名称"}
proDics = {}
codeDics = []
cityDics = {}

# 遍历文件文件中的数据
# code 为文件中的每行数据, 格式:省编码-城市编码:省名称-城市名称
for code in codeFile.readlines():
	# f为将省编码-城市编码:省名称-城市名称以':'分割
	f = code.split(':')
	# f1为省编码-城市编码以'-'分割的数组，0为省编码，1为城市编码
	f1 = f[0].split('-')
	# f1为省名称-城市名称以'-'分割的数组，0为省名称，1为城市名称
	f2 = f[1].split('-')
	# 获取f1里的省编码
	proCode = f1[0]
	# 获取f2里面的省名称
	proName = f2[0]

	# 存入省字典
	proDics[proCode] = proName
	# 城市编码为f里0下标去掉-
	cityCode = f1[1]
	# 城市名称为f2里1下标的内容，\n为文件内容的换行符，需去掉
	cityName = f2[1].replace('\n', '')

	# 存入城市字典
	cityDics[cityCode] = cityName

	# 省编码包含的城市编码
	codeDic = {}
	codeDic[proCode] = cityCode
	codeDics.append(codeDic)

#print proDics
#print cityDics
#print codeDics	
if (1 < 2):
	# codeDic为某个省包含的城市编码字典
	for codeDic in codeDics:
		# codeKey=省编码，codeVal=城市编码
		for (codeKey, codeVal) in codeDic.items():
			# 省名称
			proName = proDics[codeKey]
			# 获取城市名称
			city = cityDics[codeVal]
			# fName存放原始内容
			# newFName存放去重后的内容
			# allFname存放去重后的排序内容
			fName = proName + '-' + city + '-1.txt'
			newFName = proName + '-' + city + '-2.txt'
			allFName = proName + '-' + city + '.txt'

			if (os.path.isfile(allFName)):
				continue

			# 循环次数为n次，所以要追加到文件内容末尾
			f = open(fName, 'ar')
			print '  ' + city
			# curl为shell命令，访问链接
			# 靓号链接
			cmd = "curl 'http://detailskip.taobao.com/json/dyn_combo.do?provCode=" + codeKey + "&regionCode=" + codeVal + "&model=WCDMA&like=&_tb_token_=07VtqGi1U9Va&_input_charset=UTF-8&_ksTS=1405003247995_1220&callback=jsonp1221&itemId=39865087221&databiz=telenum&numtype=contract&telecomId=39865087221' -H 'Host: detailskip.taobao.com' -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:30.0) Gecko/20100101 Firefox/30.0' -H 'Accept: */*' -H 'Accept-Language: zh-cn,zh;q=0.8,en-us;q=0.5,en;q=0.3' -H 'Accept-Encoding: gzip, deflate' -H 'Referer: http://detail.aliqin.tmall.com/item.htm?spm=a1z2p.7360257.1997908701.12.FhZgKg&id=39865087221' -H 'Cookie: t=8176bd32cdb49af344b553b788b17784; cna=X3sXDFeu8WkCAZkDKBPAs7u8; uc3=nk2=&id2=&lg2=; tracknick=; _cc_=WqG3DMC9EA%3D%3D; tg=0; l=suplxj::1402643332571::11; lzstat_uv=241936811430198200|2185014; mt=ci=0_0; ck1=; _tb_token_=07VtqGi1U9Va; cookie2=99aedc28164de90d0fc6cda889928a8b; uc1=cookie14=UoW3uiiZyW5nQg%3D%3D; v=0' -H 'Connection: keep-alive'"
			
			# 普通号
			#cmd = "curl 'http://detailskip.taobao.com/json/dyn_combo.do?provCode=" + codeKey + "&regionCode=" + codeVal + "&model=WCDMA&like=&_tb_token_=07VtqGi1U9Va&_input_charset=UTF-8&_ksTS=1404996627874_3462&callback=jsonp3463&itemId=39990583842&databiz=telenum&numtype=main&telecomId=39990583842&offerId=111050001001' -H 'Accept: */*' -H 'Accept-Encoding: gzip, deflate' -H 'Accept-Language: zh-cn,zh;q=0.8,en-us;q=0.5,en;q=0.3' -H 'Connection: keep-alive' -H 'Cookie: t=8176bd32cdb49af344b553b788b17784; cna=X3sXDFeu8WkCAZkDKBPAs7u8; uc3=nk2=&id2=&lg2=; tracknick=; _cc_=WqG3DMC9EA%3D%3D; tg=0; l=suplxj::1402643332571::11; lzstat_uv=241936811430198200|2185014; mt=ci=0_0; ck1=; _tb_token_=07VtqGi1U9Va; cookie2=99aedc28164de90d0fc6cda889928a8b; uc1=cookie14=UoW3u5tb66%2BGhg%3D%3D; v=0' -H 'Host: detailskip.taobao.com' -H 'Referer: http://detail.aliqin.tmall.com/item.htm?spm=a1z2p.7360289.0.0.P5qmke&id=39990583842' -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:30.0) Gecko/20100101 Firefox/30.0'"
						
			# 访问次链接n次
			n = 1000

			if proName == '北京市':
				n = n / 2
			elif proName == '上海市':
				n = n / 2

			for x in xrange(1, n):
				print x
				print proName
				print '  ' + city
				# 执行访问命令, popen返回执行结果内容文件对象，system只返回执行后的状态0，1等	
				returnFile = os.popen(cmd)
				# 读取返回的内容
				fileContent = returnFile.read();
				# 处理返回内容
				beginIndex = fileContent.index('[')
				endIndex = fileContent.index(']')

				phoneNumbers = fileContent[beginIndex + 1:endIndex]
				phoneNumbers = phoneNumbers.replace('"', "")
				phoneNumbers = phoneNumbers.split(',')

				# 排序
				phoneNumbers.sort()

				# 遍历每个号码，并存文件
				for phoneNumber in phoneNumbers:
					f.write(phoneNumber + '\n')
					print phoneNumber

			f.close()

			# 去掉重复的号码， 使用set， 集合是无重复，无序的数据结果，并保存到另外一个文件
			newF = open(newFName, 'w')
			fr = open(fName, 'r')
			newF.write(''.join(set(fr.readlines())))
			fr.close()
			newF.close()

			# 对去重之后的数据进行排序， 并存入一个新文件
			newFr = open(newFName, 'r')	
			allNumber =  newFr.readlines()
			allNumber.sort()
			fAllNumber = open(allFName, 'w')
			newFr.close()

			# 遍历去重之后的号码，写入文件
			for num in allNumber:
				fAllNumber.write(num)

			fAllNumber.close()
			# 删除去重后的文件，只保留去重后，排序过的文件
			os.remove(newFName)
			# 删除原始内容文件
			os.remove(fName)
{% endhighlight %}

最后结果的出入文件名为每个省，城市独立文件存储，格式:省名称-城市名称.txt，例如:江苏省-南京市