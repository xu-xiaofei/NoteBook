1.  导入模块的搜索路径  
    pip下载后的第三方包都统一放在lib下的site-packages文件夹下

    ```python
    import sys
    print(sys.path)

    ```
2. pip 安装模块的路径

    ```python
    from distutils.sysconfig import get_python_lib
    print(get_python_lib())
    ```

3. pip 和 pip3的区别  

    只有python3  那么pip和pip3是一样的效果  
    只有python2 那么只能使用pip  
    既有python2又有python3，那么使用pip和pip3的安装路径是不一样的，而且模块不相通，也就是pip安装的模块python3调用不到


4. Python __init__.py 
    
    在什么时候执行？这个initial文件？  
    主要是用来进行包的导入，有些变量，如__name__ 和 __all__ 有特殊作用

    ```python
    if __name__ == "__main__":
    __all__ = ['os', 'sys', 're', 'urllib']
    ```