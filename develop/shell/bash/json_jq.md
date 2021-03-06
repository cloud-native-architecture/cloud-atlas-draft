在维护系统中，经常会遇到需要处理一些程序的json格式配置文件，并且在curl脚本处理RESTful返回数据。

如果输出内容没有很好格式化，则大量的json字符串非常不利于检查。此时，使用python模块 `json.tool` 过滤就可以输出标准格式json内容：

```
container_id=f4810fec374a096895c90e1c74574a7d69e5ec306ea8fe0817e461c28b3da716
cat /var/run/docker/libcontainerd/${container_id}/config.json | python -m json.tool
```

输出内容是非常清晰的json格式:

```
{
    "annotations": {
        "__BlkBufferWriteBps": "0",
        "__BlkBufferWriteSwitch": "0",
        ...
```

有一个处理脚本案例结合了python json.tool和sed等:

```bash
curl -s 'http://api.icndb.com/jokes/random' \
| python -m json.tool \
| grep '\"joke\"' \
| cut -d ':' -f 2 \
| sed 's/&quot;/\"/g'
```

虽然我们可以用shell脚本来处理输出的json数据，但是，如果要依赖组合的 `awk` `grep` `cut` 显然非常繁琐，效率低下。

实际上工具 [jq](https://stedolan.github.io/jq/) 可以帮助我们在shell脚本中处理json数据，特别适合解析一些REST返回数据。

举例

```
curl -s "http://api.icndb.com/jokes/random" | jq '.value.joke'
```

举例，json数据如下

```json
{ "foo": 123, "bar": 456 }
```

我们要提取 foo 键的数据，则使用命令

```bash
echo '{ "foo": 123, "bar": 456 }' | jq '.foo'
```

输出结果就是 `123`

```bash
echo '{ "Version Number": "1.2.3" }' | jq '."Version Number"'
```

# jq处理array

* jq 可以处理数组，使用 `.[]` 方法:

```bash
echo '[1,2,3]' | jq '.[]'
```

对于数组的对象，采用如下方法:

```bash
echo '[ {"id": 1}, {"id": 2}  ]' | jq '.[].id'
```

也可以用来获取 `key/value` 对的值:

```bash
echo '{ "a": 1, "b": 2  }' | jq '.[]'
```

可以按照索引来获得array的值：

```bash
echo '["foo", "bar"]' | jq '.[1]'
```

jq还包含了一些内建功能：

- 返回array的key:

```
echo '{ "a": 1, "b": 2  }' | jq 'keys | .[]'
```

则返回值是a和b。请注意，在jq中也可以使用管道`|`，这里就是表示提取出keys以后在通过索引`.[]`把所有key都输出出来。

- 可以返回array的长度

```bash
echo '[1,2,3]' | jq 'length'
```

# 使用jq创建对象

可以对json进行提取重整，例如:

```bash
echo '{"user": {"id": 1, "name": "Cameron"}}' | jq '{ name: .user.name  }'
```

则输出就是

```json
{
  "name": "Cameron"
}
```

# 参考

* [Working with JSON in bash using `jq`](https://medium.com/cameron-nokes/working-with-json-in-bash-using-jq-13d76d307c4)
* [Working with JSON in bash using jq](https://cameronnokes.com/blog/working-with-json-in-bash-using-jq/)
