beego patch
===
This is a patch for the **beego version 1.7.1**

## statement
The patch is used to add some filters for the json field of a postgresql, including:

* json_trav_gt
* json_trav_lt
* json_trav_gte
* json_trav_lte
* json_trav_eq
* json_trav_nq

After applying the patch, you can use the filter like this:

```go
func JsonFilterTest() {
    o := orm.NewOrm()
    qs := o.QueryTable(new(Tag))
    var tags []*Tag
    qs = qs.Filter("data__json_trav_gt", "a", 1)
    qs = qs.Filter("data__json_trav_lt", "a", 3)
    qs = qs.Filter("data__json_trav_gte", "b", 2)
    qs = qs.Filter("data__json_trav_lte", "b", 2)
    qs = qs.Filter("data__json_trav_nq", "c", 2)
    qs = qs.Filter("data__json_trav_eq", "c", 1)
    qs.All(&tags)
    fmt.Println(len(tags))
    for _, v := range tags {
        fmt.Println(v)
    }
}
```

## usage
To apply the patch, clone the beego-1.5.2.patch to the ``` $GOPATH/github.com/astaxie/orm/ ``` , and just apply it:

```shell
$ git apply beego-1.5.2.patch
```

## how to add new filter support
The gate of allowing only one argument is opened if the new filter starts with "json_", what we should do is to:

1. simply modifying the ``` operators ``` variable in ``` db.go ``` (this can be treated as a flip, if a new filter is set to ``` true ``` in the map, then the new filter is acceptable)

2. add a new filter map to the variable ``` postgresOperators ``` in file ``` db_postgres.go ``` (this provides the real meaning of the new filter if it is accepted already)

## more introduction
[beego](https://beego.me/)
