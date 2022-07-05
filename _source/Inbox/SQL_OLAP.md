
# SQL窗口函数（OLAP）

## 1 概述
窗口函数是只能用于`SELECT`子句中的，可以返回基于分组的、与指定的相邻若干行相关的统计值的函数。

- 基本组成：
	```sql
	[函数名] OVER ([分组 PARTITION BY] 
			  		[排列顺序 ORDER BY] 
			  		[窗口])
	```

- 执行顺序：分组-->按顺序排列-->对每个窗口执行函数

例如：
```sql
SELECT product_id, product_name, product_type, sale_price,
    	SUM(sale_price) OVER (PARTITION BY product_type
                            	ORDER BY product_id DESC
                            	ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) sum
FROM Product;
```
![|400](https://cdn.jsdelivr.net/gh/Deruck/Img/Img/20210605200834.png)
这个窗口函数按顺序执行了以下操作：
- `PARTITION BY product_type`：按`product_type`分组
- `ORDER BY product_id DESC`：每组内按`product_id`降序排列
- `ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING`：指定当前行及相邻两行作为窗口
- `SUM(sale_price)`：在每个窗口内执行对`sale_price`的求和

## 2 分组：`PARTITION BY`
选择一个变量作为分组条件，之后的所有操作，包括排序、窗口划分、函数执行都在每个组内进行，组间互不干扰。

## 3 排序：`ORDER BY`
在某一组内，按指定变量排序，以便划分窗口。

## 4 窗口：
指定哪些行参与当前行统计值的计算。

语法：
```sql
RANGE|ROWS BETWEEN 上限 AND 下限
```
- **上下限：**
	```sql
	[CURRENT ROW] | [<num>|UNBOUNDED PRECEDING|FOLLOWING]
	```
	- 当前行：`CURRENT ROW`
	- 上下滑动：
		- `PRECEDING`为当前行向上滑动，`FOLLOWING`为当前行向下滑动。
		- 可以使用`<num>`指定向上/下滑动的单位数（`ROW`和`RANGE`单位含义不同），如`2 PRECEDING`代表包括本行、本行之上一单位与本行之上两单位；或者使用`UNBOUNDED`，代表选中当前行之上/下的所有。

	- 若想表示当前行及其上方若干单位，可省略`BETWEEN AND`，简写为
		```sql
		ROWS|RANGE <num>|UNBOUNDED PRECEDING
		```
- **`ROWS`与`RANGE`**
	- `ROWS`代表每一行在表中的绝对位置，滑动的每一单位为一行，可以脱离`ORDER BY`使用。
	- `RANGE`必须搭配`ORDER BY`使用，表示`ORDER BY`指定变量的值。滑动的单位为值变化1，上下限代表值的区间，如`1 PRECEDING`代表当前行的值向上变动1单位。注意是向”上“变动1单位而不是”加“或”减“1单位，因为这与`ORDER BY`的升降序有关。升序时为减，降序时为加，但不变的是都是朝表的上方向。

	例如：
	```sql
	SELECT s_id, s_score,
			SUM(s_score) OVER (ORDER BY s_score 
							   ROWS BETWEEN 1 PRECEDING AND CURRENT ROW) row_,
			SUM(s_score) OVER (ORDER BY s_score 
							   RANGE BETWEEN 9 PRECEDING AND CURRENT ROW) range_9,    
			SUM(s_score) OVER (ORDER BY s_score 
							   RANGE BETWEEN 10 PRECEDING AND CURRENT ROW) range_10
	FROM Student;          
	```
	![|250](https://cdn.jsdelivr.net/gh/Deruck/Img/Img/20210605200054.png)
- **默认值**
	- 当`ORDER BY`与窗口都缺失时，默认为
		```sql
		ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
		```
	- 指定`ORDER BY`，缺失窗口时，默认为
		```sql
		RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
		```


若触及边界，则将边界外的行省去，如第一个例子中的绿色部分。



## 5 函数
- 专用窗口函数
	- `RANK`：排名，同名次的行算多个
	- `DENSE_RANK`：排名，排名同名次的行算一个
	- `ROW_NUMBER`：按顺序编号
	
	例如：
	```sql
	SELECT s_id, s_score,
			RANK() OVER (ORDER BY s_score) rank_ ,
			DENSE_RANK() OVER (ORDER BY s_score) dense_rank_  ,
			ROW_NUMBER() OVER (ORDER BY s_score) row_num_
	FROM Student;
	```
	![|300](https://cdn.jsdelivr.net/gh/Deruck/Img/Img/20210605202802.png)
- 聚合函数
	- `COUNT()`
	- `SUM()`
	- `AVG()`
	- `MIN()`/`MAX()`


