1. Shell 中单双引号的作用

    单引号：所见即所得

    双引号：所见非所得，它会先把变量解析之后，再输出

```sh
[envuser@beta ~]$  echo "$HOME"
/home/envuser
[envuser@beta ~]$ echo '$HOME'
$HOME
[envuser@beta ~]$ echo ${HOME}
/home/envuser
[envuser@beta ~]$ echo '${HOME}'
${HOME}
[envuser@beta ~]$ echo "${HOME}"
/home/envuser
[envuser@beta ~]$ echo "'${HOME}'"
'/home/envuser'
[envuser@beta ~]$  

```

2. 其他  
    反引号（``） ：命令替换，通常用于把命令输出结果传给入变量中,也可以使用$(cmd)

    反斜杠( \ ) ：转义字符/逃脱字符，Linux如果echo要让转义字符发生作用，就要使用-e选项，且转义字符要使用双引号

```sh
[envuser@beta ~]$ echo $(ls)  无序，无格式 =  echo `ls`
apache-tomcat-8.5.32.tar.gz certificate-service create.sh create_temp_view_table.sh demoCA Dockerfile 
[envuser@beta ~]$ echo "$(ls)"  有序，有格式 =  echo "`ls`"
apache-tomcat-8.5.32.tar.gz
certificate-service
create.sh
create_temp_view_table.sh
demoCA
```

```sh

[envuser@beta ~]$ echo "\n"
\n
[envuser@beta ~]$ echo -e \n
n
[envuser@beta ~]$
```
