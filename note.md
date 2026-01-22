chirpy文档：
https://chirpy.cotes.page/

本地编译部署：
在项目根目录下分别运行：
```bash
bundle install
bundle exec jekyll clean 
bundle exec jekyll serve
```

若本地预览的网页图片缩略不对，需要bundle exec jekyll clean先清理一遍，且清理浏览器缓存后再重新运行打开。

git push到github时出现figure检查错误，原因在于.config_yaml:101中的avatar路径在win中直接复制时用的为反斜杠\，需要改为正斜杠/
