
1. yield 
    把一个函数变成一个generator

2. e.g

```python
    def dedupe(items):
        seen = set()
        for item in items:
            if item not in seen:
                yield item
                seen.add(item)
```            