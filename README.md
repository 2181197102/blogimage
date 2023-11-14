# 我的个人图床

![image](https://github.com/2181197102/blogimage/assets/128360566/c66cafee-65b4-4f38-93e4-8ff697e11661)


自定义域名：https://cdn.jsdelivr.net/gh/2181197102/blogimage@main

**jsDelivr CDN使用**

CDN的两个功能：

基础功能：可以提供静态文件内容 主要功能：加速内容获取，提高访问速度

设定自定义域名：`https://cdn.jsdelivr.net/gh/用户名/仓库名@分支名` 或 `https://fastly.jsdelivr.net/gh/用户名/仓库名@分支名`

例如：

```
https://cdn.jsdelivr.net/gh/wefashe/git-images@master
```



**返回URL**

```
https://cdn.jsdelivr.net/gh/用户名/仓库名@分支名/指定的存储路径/文件名
```

例如

```
https://cdn.jsdelivr.net/gh/wefashe/git-images@master/images/20210112153045.png
```

![image-20231114195621514](C:\Users\21811\AppData\Roaming\Typora\typora-user-images\image-20231114195621514.png)

**补充：**
现在`jsdelivr`改为只能访问release的内容即只能加速访问发版后的内容 可以右侧随便创建一个新的 relaese

现在提交的内容只有发版后才能加速访问，所以可以使用github action 来实现自动发布：

.github/workflow/publish.yml

```
# This is a basic workflow to help you get started with Actions

name: release CI

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the main branch
  push:
    branches:
      - main

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - name: Checkout
        uses: actions/checkout@v2

      - name: 读取当前版本号
        id: version
        uses: ashley-taylor/read-json-property-action@v1.0
        with:
          path: ./package.json
          property: version

      # Runs a set of commands using the runners shell
      - name: Release
        # uses: softprops/action-gh-release@v1
        # if: startsWith(github.ref, 'refs/tags/')
        uses: actions/create-release@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: v${{steps.version.outputs.value}}
          release_name: v${{steps.version.outputs.value}}
          body: Release v${{steps.version.outputs.value}}
          draft: false
          prerelease: false
```



参考文章：[Typora+PicGo+GitHub图床的搭建和常见问题的个人解决方法_picgo图床github图床搭建上传失败_墨叶OvO的博客-CSDN博客](https://blog.csdn.net/qq_62229338/article/details/126661273?ops_request_misc=%7B%22request%5Fid%22%3A%22169996133616800215099583%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=169996133616800215099583&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-5-126661273-null-null.142^v96^pc_search_result_base8&utm_term=picgo上传图片失败github&spm=1018.2226.3001.4187)
