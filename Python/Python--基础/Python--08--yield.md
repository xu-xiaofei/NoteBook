
1. yield 
    把一个函数变成一个generator

2. e.g

```py
    def dedupe(items):
        seen = set()
        for item in items:
            if item not in seen:
                yield item
                seen.add(item)
```            
3. yield from
    用于 返回一个生成式
```py
    def flatten(item):
        if isinstance(item, (list, tuple)):
            for x in item:
                yield from flatten(x)
        else:
            yield item

```
