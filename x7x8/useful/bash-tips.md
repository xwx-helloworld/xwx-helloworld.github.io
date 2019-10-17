## 常用bash脚本

* 文件夹压缩：zip -r dst.zip src_fd
* 文件夹传输（本地传服务器）：scp local_file remote_username@remote_ip:remote_folder（如果是文件夹，需要 -r ）
* 文件夹传输（服务器传本地）：scp remote_username@remote_ip:remote_folder local_file 

##  python相关库介绍

### os.environ
* 返回的是一个dict
* environ是一个字符串所对应环境的映像对象

### tqdm(show iteration process, third-party)
* pip install tqdm
* from tqdm import tqdm
* tqdm(iterable)

### glob(get all files, builtin)
* from glob import glob
* glob(path) # path contains match character better,eg. '/data/zx**/images/201906*/*.jpg'

### os.walk(similar to glob, builtin)
```
def __get_files_recursive(fd, suffix=None):
    fs, ds = [], []
    for root, dirs, files in os.walk(fd):
        for name in files:
            if suffix is None:
                fs.append(os.path.join(root, name))
            else:
                if name.endswith(suffix):
                    fs.append(os.path.join(root, name))
    return fs
```

### os.rename/remove(similar to name, builtin)
* os.rename(dst_name, os.path.join(dst_fd, new_name))
* os.remove(full_name)

### shutil(operate files, third-party)
* pip install shutil
* shutil.copy(full_name, dst_fd)

### 不生成__cache__文件：
* 程序开始时加上下面代码
`python
import sys
sys.dont_write_bytecode = True
`