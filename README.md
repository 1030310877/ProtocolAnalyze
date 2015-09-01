# ProtocolAnalyze
协议解析框架
============
#用途
使用此框架，可以实现协议数据部分与客户端的解耦。在客户端开发中，发送byte指令将无需进行拼接以及每个byte的计算。<br>
根据协议定义，可直接使用K-V形式传入自定义的Key，框架将自动拼接好byte命令进行回传。使得开发代码的可读性大大提升。

#使用说明
##1.协议定义
以json形式描述协议内容，例如：<br>
{
	"length": 3,
	"body": [
	{
		"type": "constants",
		"value": "0xa5"
	},
	{
		"type": "var"
	},
	{
		"type": "enum",
		"valueMap": {
			"powerup": "0x11",
			"powerdown": "0x22"
		}
	}
	]
}<br>
<br>
##定义保留字
###length
定义协议长度，byte为单位
###body
协议具体内容
####type
该命令类型，有constants常量类型，var变量类型，enum枚举类型
#####constants
与value配对使用，定义协议中的常量值
#####var
定义协议中的变量值，一般用于传递可变数值。
#####enum
定义协议中的枚举类型，必须与valueMap配套使用。
<br>
<br>
##框架使用
1.实例化ProtocolFactory，然后调用initProtocol进行协议解析。
2.调用ProtocolFactory中的方法，按照顺序传入参数（var和enum类型），可获取到拼接完成的不同形式的命令。
3.遇到enum类型，将会自动通过K-V形式，取出Value进行命令拼接。