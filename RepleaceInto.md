Replease into 可以实现在insert的时候，如果主键冲突，或者Unique Index冲突的时候，先Delete，然后再insert。

引出一个问题点：

如果定义了UniqueIndex，但是其中一个字段的值为空，则不生效。。
