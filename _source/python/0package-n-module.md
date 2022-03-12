# python自定义包与模块

- 所有`.py`文件都可当作模块使用。
- 包含`__init__.py`文件的文件夹被视作一个包。
- 包中包含`__init__.py`的文件夹被视作子包。
- 和包在同级目录的文件可以直接导入包。
- 将包加入全局路径的方法：
	- 查看系统路径变量
	```python
	import sys
	sys.path()
	```
	
	- 在任意路径中，创建`[filename].pth`文件，在文件中写入包所在路径，即可全局访问。
- 如果需要读取包中的数据，可以使用模块`pkgutil`实现
```python
from pkgutil import get_data
from io import BytesIO
import pandas as pd

data_bytes = BytesIO(get_data(__package__, file_path))
data: pd.DataFrame = pd.read_csv(data_bytes)
```