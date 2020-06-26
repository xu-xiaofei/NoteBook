1. import
    模块是 .py文件，包是含有.py文件的floder,如果包里面有__.init__.py 文件，那么

2. 包的导入

包就是包含很多模块的文件夹,包内还可以有子包

- 1.利用import直接导入包（仅仅导入__init__.py中的内容）  
    语法：import package_name
    直接导入一个包，仅仅可以使用__init__.py中的全部内容
    使用：package_name.func_name 或者 package_name.class

- 2.导入包中的某一个模块  
    语法：import package_name.module_name
    使用：package_name.module_name.func_name或 package_name.module_name.class_name

3. 模块的导入
    模块就是.py类型的Python文件，导入时不需要.py后缀，直接导入文件名即可
- 1.利用import直接导入：
    语法：import module_name
    使用方式：module_name.class_name或者module.func_name

- 2.利用import导入模块并设置一个别名
    语法：import module_name as XXX  
    使用方式：XXX.class_name或者XXX.funct_name

- 3.利用from复制模块的属性，可以实现只导入模块中的部分类或函数或变量  
    语法：from module_name import class_name， funct_name  
    使用方式：直接调用函数或实例化类即可  
    但要注意，from把变量从模块中导入后，会导致相同名称的变量被覆盖，也就是说不同模块的命名空间会在此处重叠。

- 4.利用from...import *导入模块全部内容  
    语法：from module_name import *
    使用时直接调用函数或实例化类即可
    也会导致相同名称的变量被覆盖

- 5.利用importlib模块实现导入以数字开头的模块  
    语法：import importlib
    XXX = importlib.import_module("module_name")
    使用时XXX.class_name或者XXX.func_name

- 6.python 爬虫的header 网站 http://www.useragentstring.com （需要翻墙）

- 7.How to install moudle by conda

    ```
    conda info --envs
    conda activate my-envs-name
    pip install package name

    ```



