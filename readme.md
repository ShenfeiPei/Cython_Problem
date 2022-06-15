# FAQs of Cython

- 如果使用Cython调用C++，那么数组最好用Vector而不是int *，否则会导致一些问题，比如前四个值错误。
- pyx文件不能与调用的cpp文件同名，否则根据pyx生成的cpp会覆盖原来的cpp
- 在setup文件中，由pyx生成的库文件，最好和pyx文件同名。
- 测试代码的时候用 python setup.py build -i会方便一些，因为生成的库文件就在当前文件夹下，不用安装就可测试。
- 调用cpp文件定义类时，cpp的头文件c.h一定要写ifdef，endif等，否则会重定义
- 在pyx文件中返回vector<vector<type>> 时，要加上np.array(ret)，否则不是2维的array，而是list（list）。
- setup中定义"NPY_1_7_API_VERSION" 会导致dimensions is not a member of 'tagPyArrayObject'，暂时只能去掉
- c++使用eigen库，默认的矩阵时列优先存储的，拷贝到vector时也要列优先拷贝，否则会出错，后续考虑使用eigency转换/返回，避免自己转换。
- _ZN5错误，h文件中有~ClassName，cpp文件用也必须要有ClassName::~ClassName(){}否则会引发undefined symbol _ZN5ClassName
- _ZN2错误，undefined symbol，可能是pxd文件里面没有pass cpp文件。
- 顶层setup.py中的包的名字和包含的唯一子包要一致，
```
文件命名如下
xx_code
    - xx (dir)
        - a.h
        - a.cpp
        - a_.pyx
        - a_pxd  
    - setup.py (name=xx)
    - demo.py    # we can calls functions defined in a.pxd in demo.py
```
# 引用公共函数，建议目录如下, 所有包（目录）不能有下划线。
    - pack
        - algo
            - a.h
            - a.cpp
            - a_.pyx  (reference Public by top_pack = __import__(__name__.split(".")[0], fromlist=["Public"]), P = top_pack.Public
            - a_.pxd
            - setup.py (reference Public by top_pack = __import__(__name__.split("_")[0], fromlist=["Public"]), P = top_pack.Public
            - __init__.py
        - Public
        - __init__.py (empty， 编译时阻止import algo，如果编译时import algo，而此时algo还没有完成编译，就会出错)
        - setup.py
    - demo.py
    - setup.py
