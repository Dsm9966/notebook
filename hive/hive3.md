
## Hive

* 数据查询
    
    ![image.png](https://upload-images.jianshu.io/upload_images/14466577-afbb6b13348402b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    
* where、having、group by:
    *  1 使用分组就不再逐行执行模式了，会根据我们的分组key以分组模式执行，
    *  一旦有group by子句，那么，在select子句中就不能有 （分组字段，聚合函数） 以外的字段
    *  2 使用聚合函数得到的结果默认的字段名不好使，我们要进行调用可以给它取一个别名，就可以在条件中使用了，
    *  同一层给having使用，作为子查询时可以给外层where用
    *  3 where 执行时间在聚合之前进行数据筛选，而having是在聚合结束之后继续条件处理
* 排序区别：
    * order by: 全局排序，一个reduce
    * sort  by: 不是全局，进入reducer之前排序完成；设置mapred.reduce.tasks>1
    *  distribute by：开启的reducer数量=桶数
    *  cluster by:当分桶和sort by 字段是同一个时，相当于distribute by+sort  by;不是同一个的话，不适用
* join  on:
    *  内连接：inner join
    *  右外连接：right join
    *  左外连接：left join
    *  全连接：full join
    *  左半连接：left semi join(相当于join连接两个表后产生的数据中的左半部分)
* 数据类型
*  1 原子数据类型：
        *  数值型
        *  时间类型：timestamp时间戳；date日期，年月日组成
        *  布尔型
        *  字符串型
*  2 复杂数据类型：
      *  数组（Array)：相同数据类型的元素组成，可以通过下标访问，从0开始
      
      ![image.png](https://upload-images.jianshu.io/upload_images/14466577-12750899cf0b654f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
       
      *  映射（Map）:通过key来访问元素，eg:userlist['username']-->password

      ![image.png](https://upload-images.jianshu.io/upload_images/14466577-912fcc4414cb51a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

      ![image.png](https://upload-images.jianshu.io/upload_images/14466577-3c3013a30344a6c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 

      *  结构体（struct）:可以包含不同类型的元素，获取元素方式‘.’,eg：user.name
      
      ![image.png](https://upload-images.jianshu.io/upload_images/14466577-d99530bca12097fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 函数使用
* 内置函数
    * 类型转换：select cast(数据 as 类型)(hive提供自动转换类型)

    ![image.png](https://upload-images.jianshu.io/upload_images/14466577-355a86938e038192.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 数学运算函数
    * round()四舍五入 
    * ceil()向上取整
    * floor() 向下取整
    * abs()：绝对值
    * greatest():单行函数，取最大值，区别于max（）：分组聚合函数
* 字符串函数
    * substr(string str, int start)   截取子串,注意-->字符串索引从1开始
    * substr(string, int start, int len)
   
     ![11.16.1.png](https://upload-images.jianshu.io/upload_images/14466577-aa6d02fe8b9d94e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    
    * substring 同substr
    * concat()拼接
       
     ![11.16.2.png](https://upload-images.jianshu.io/upload_images/14466577-e9003a5220c41c91.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
    * split(string str, string pat)  ## 切分字符串，返回数组
    
    ![image.png](https://upload-images.jianshu.io/upload_images/14466577-702756fe976f974b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    
*  时间函数  
    * from_unixtimeunix（）时间戳转字符串
    * unix_timestamp（）字符串格式转unix时间戳，返回值是一个长整数类型
    * to_date()字符串转成日期date
* 条件控制函数
    * if(条件，满足条件输出内容，不满足输出内容)
    * CASE [expression]
       WHEN condition1 THEN result1
       WHEN condition2 THEN result2
       ...
       WHEN conditionn THEN resultn
       ELSE result
       END
* 集合函数
    * array(元素) 构造数组
    * array_contains(Array<T>, value)  数组中是否包含value,返回boolean值
    * sort_array(Array<T>) 返回排序后的数组
    * size(Array<T>)  返回一个集合的长度，int值
 * 常见分组聚合函数
    * sum();avg();count();max();min();distinct
    * collect_set():将某个字段在一组中的所有值形成一个集合（数组）返回
    * explode():行转列函数，对数组字段炸裂
    * 