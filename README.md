# Bugzilla 简体中文语言包

>目前版本是 `Bugzilla 5.0.4`，之前和之后的版本也可以用 `convert.ipynb` 里面的代码汉化

由于 `5.0.4` 找不到 `简体中文` 的语言包，但是找到了 `繁体中文` 的语言包。所以用 `Python` 写了个脚本翻译了一下，已经在自己服务器上测试了，可用

PS. 如果发现有翻译不合理的地方，还望提交 `issue` ，我会及时修改，谢谢

## 目录说明

|文件名|说 明|
|-----|----|
|default|繁体中文语言包模板文件|
|result|简体中文语言包模板文件|
|bugzilla-cn-5.0.4.zip|标准格式的简体中文语言包|
|convert.ipynb|转换代码|
|langconv.py|繁体转简体包|
|zh_wiki.py|繁体-简体对照表|

## 繁体中文语言包

[repeat/bugzilla-tw >>>](https://github.com/repeat/bugzilla-tw)

## 安装说明

- 下载 `bugzilla-cn-5.0.4.zip`
- 解压并打开得到 `template`
- 把 `zh-CN` 整个复制到所安裝的 `Bugzilla` 的 `template/` 目录下

## 实现原理

遍历目录下所有文件和文件夹，若是 `.tmpl` 则用繁体转简体包翻译，其他格式文件直接拷贝。

``` python
for root,dirs,files in os.walk(target_dir):
    for directory in tqdm(dirs):
        result_src_dir = result_dir + root[len(target_dir):] + "/" + directory
        if not os.path.exists(result_src_dir):  
            os.makedirs(result_src_dir)
            print("mkdir: " + result_src_dir)
    for file in tqdm(files):
        original_src_file = os.path.join(root, file)
        result_src_file = result_dir + root[len(target_dir):] + "/" + file
        if(original_src_file[-5:] == ".tmpl"):
            translate(original_src_file, result_src_file)
        else:
            shutil.copy(original_src_file, result_src_file)
            print("save: " + result_src_file)
```

详细代码见目录下 `convert.ipynb`、`langconv.py` 和 `zh_wiki.py`