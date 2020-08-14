# FAQs of Cython

- 如果使用Cython调用C++，那么数组最好用Vector而不是int *，否则会导致一些问题，比如前四个值错误。
- pyx文件不能与调用的cpp文件同名，否则根据pyx生成的cpp会覆盖原来的cpp
- 在setup文件中，由pyx生成的库文件，最好和pyx文件同名。
- 测试代码的时候用 python setup.py build -i会方便一些，因为生成的库文件就在当前文件夹下，不用安装就可测试。
```
Test
    - a.pyx
    - c.pxd
    - c.h
    - c.cpp
    - a.pyd      # generated by build_ext -i
    - demo.py    # we can calls functions defined in a.pxd in demo.py
```

