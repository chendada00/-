# Shell

## 自定义用户变量

``` shell
#可以直接赋值，中间不能加空格，如果需要空格要使用引号
myvar=123
#测试输出
echo $myvar

#删除变量
export 变量名

#获取用户输入参数
/root/test.sh
$0				#获取参数前输入的字符，例如/root/test.sh就是$0
$1 - $9			#可以获取用户输入的参数

#获取当前命令行的全部参数
$*			#把参数看成一个整体
$@			#把参数看成一个数组

# 相当于返回值，正常退出时返回0,直接写时返回上一条命令的执行结果
$?
```

## 运算符

``` shell
#进行数学运算，空格必须存在
expr 1 + 2
expr 1 /* 2			#转义
#或者
$[1+2]

#变量替换，将运算结果赋值给变量，使用expr时需要用$()或者‘’，使用$[]时不需要
```




## 流程控制
### if判断

``` shell
#判断文件权限
-r			  #读
-w			  #写
-x 文件名		#判断是否有执行权限

#判断文件类型
-e				#是否存在
-f				#是否为文件
-d				#是否为目录

#技能
[ 条件表达式 ] && echo ok || echo notok

#if判断
if [ 条件表达式 ]
then
	#代码
elif [条件表达式]
then
	#代码
else
	#代码
fi	#结束

#-------------例子---------------
if [ a -gt 18 ] && [ a -lt 35 ]
then
	echo ok
fi
#-------------等价----------------
# -a（and）表示与，-o(or)表示或
if [ a -gt 18 -a a -lt 35 ]
then
	echo ok
fi
```

- -eq : 相等判断(equal)
- -ne: 不等于判断(not equal)
- -lt: 小于判断(less than)
- -gt: 大于判断(greater than)
- -le: 小于等于（less than or equal)
- -ge: 大于等于(greater than or equal)

### case语句

``` shell
case 变量 in
值1)
	代码
;;
值2)
	代码
;;
*)
	如果都不符合则执行此语句
;;
esac#结束
```

### for循环

``` shell
# 1
for (( 初始值;循环条件;变量变化))
do
	代码
done

# 2
for 变量 in 值1 2 3 或者 {1..100}
do
	代码
done

#例
for (( i = 0 ; i < 10 ; i++))
do
    num=$[ $num + $i ]
    或者
    let num+=i
    echo $num
done
```

### while循环

``` shell
while [ 判断条件 ]
do
	代码
	
done
```

## 从控制台读取用户输入

``` shell
read -p(输入的提示语) -t(指定读取等待时间,超过时间则执行下面的语句) 变量名
```

## 函数

``` shell
#调用系统内置函数
$(函数名称)


#自定义函数
[function可以省略] 函数名称 ()
{
	代码
	[return int0-255;]如果没有返回值则自动返回函数最后一句的返回值
}
```



## 数组

``` shell
# 定义数组
数组名=(a b c)
# 或者
数组名[0]=值

# 读取
${数组名[0]}
```



## 输出

``` sh
# 原样输出
echo
# 使用占位%符输出
# %s 字符串
# %c 一个字符
# %d 整数
# %f 小数
printf "姓名%d，年龄%f" cc 20
```



## 案例

``` shell
#将指定目录中的文件定时归档

if [ $# -ne 1]
then
	echo "输入错误"
	exit
fi




```

