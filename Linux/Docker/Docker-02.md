# 1.volume

```bash
docker volume create v_name

docker run v_name:/app  -d image:tag

如果不确定挂在的位置是不是app，可以如下
docker inpect container_id  观察其json 的volume的Description

```


# 2.inernet

Docker 有四种网络选择，默认为桥接模式(bridge)
