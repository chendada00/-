## 流程控制

1. if

``` sql
if		表达式 then 操作
elseif 	表达式 then 操作
else	操作
end if
```

2. CASE

``` sql
case 变量
	when 值 then 返回值
    when 值 then 返回值
    when 值 then 返回值
    else 都不符合时执行
end case;
```

3 循环

- loop-------自定义跳出条件

``` sql
declare num int;
lp:loop
	set num = num + 1;
	#如果大于5跳出循环
	if num > 5 then leave lp;
	end if;
end loop;
```



- while---------符合条件时循环

```sql
declare num int;
while num < 5 do
	num = num + 1;
end while;
```

- repeat---------符合条件跳出循环

``` mysql
declare num int;
repeat
	set num = num + 1;
	until i > 5;
end repeat;
```

- 循环控制-------常用于符合某个条件跳出事务或者函数

``` sql
leave_lab:[循环|create|……]
	#跳出到leave_lab标签
	then leave leave_lab;
```

