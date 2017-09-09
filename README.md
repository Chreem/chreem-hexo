# hexo based personal blog

博文整得跟自己代码一个德行, 回头检查结构和分类等简直不堪入目(~~现在也好不到哪去~~), 干脆全部重整.

## how this work

主站[github.io](https://chreem.github.io), 不过一些性能优化只能在自己站点测试.  
整站和之前没多大区别, 依然是[hexo主机名](https://hexo.chreem.xyz)通过nginx负载至内部docker的web server上  
去掉了之前shi一样的timing update想法, 改用travis ci

1. github.io配置SSH Key
2. travis ssh 参考<http://blog.csdn.net/u012373815/article/details/53574002>
3. git push

## issue

旧版保留, 新问题直接放在github的issue里

## LICENSE & Thank for

|name|license|
|---|---|
|hexo|[![license](https://img.shields.io/github/license/mashape/apistatus.svg?style=flat-square)](https://github.com/hexojs/hexo/blob/master/LICENSE)|
|hexo-theme-apollo|[![license](https://img.shields.io/github/license/mashape/apistatus.svg?style=flat-square)](https://github.com/pinggod/hexo-theme-apollo)|
