# 查询json数组中某个键的方法
jdata字段的`origin`键的值是数组,使用`json_array_elements`可以进行数组查询,再指定`id`查询数组中的id值
```sql
<!--查询json数组的值-->
select json_array_elements(jdata ->'origin')->'id' attr from  mylog  where usetime >= '2021-01-19' and code = 'logger' and requestType  = 'POST';
<!--使用json数组的值再查询数组中某些值,id前边的两个箭头是为了让输出为文本,可看参考三-->
select json_array_elements(myjdata) ->> 'id' from (select thedata -> 'data' myjdata from mylog where updatetime >= '2021-04-18' and "action" = 'PUT'
and code = 'test') as t1 ;
```
[参考一](https://blog.csdn.net/weixin_45893488/article/details/108000953)  
[参考二](https://blog.csdn.net/eff666/article/details/105253974)  
[参考三](https://blog.csdn.net/xupeng874395012/article/details/72883483)  