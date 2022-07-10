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

- loop

``` sql
declare num int;
lp:loop
	set num = num + 1;
	#如果大于5跳出
	if num > 5 then leave lp;
	end if;
end loop;
```



- while

```sql
```

