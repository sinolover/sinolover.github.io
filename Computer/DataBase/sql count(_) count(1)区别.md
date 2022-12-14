sql count(*) count(1)区别
 
效果：两者的返回结果是一样的。 
意义：当count的参数是具体值时（如count(1)，count('a')），
count的参数已没有实际意义了。
 
范围：在统计范围，count(*)和count(1) 一样，都包括对NULL的统计；
           count(column) 是不包括NULL的统计。
 
速度：表沒有主键(Primary key)，count(1)比count(*)快；
           否则，主键作为count的参数时，count(主键)比count(1)和count(*)都快；
           表只有一个字段，count(*)，count(1)和count(主键)速度一样。
