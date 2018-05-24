
# Sodility
## Mapping映射
> solidity里的映射可以理解为python里的字典，建立键－值的对应关系，可以通过键来查找值，键必须是唯一的，但值可以重复。
> 定义方式为：mapping（键类型=>值类型），例如mapping(address=>uint)  public  balances，这个映射的名字是balances，权限类型为public，键的类型是地址address，值的类型是整型uint，在solidity中这个映射的作用一般是通过地址查询余额。键的类型允许除映射外的所有类型。
> 例如：映射balances中包括三个键值对（user1:100,user2:145,user3:195）,输入user2即可得到145