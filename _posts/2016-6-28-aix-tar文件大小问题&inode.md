每天总会或多或少遇到点事，这不，今天上午，正埋头准备晚上变更时，运行兄弟问我为啥给DC的数据这几天都没生成。

也不是没生成，而是给的压缩包中只有部分数据，特别大的那个数据没有给过去。两人分析了下，觉得奇怪，诡异。

我没有太担心，因为时效性不是很强，影响不是特别大。忙完手头的活，就去运行兄弟那边去查了。

由于每天导出数据量很大，所有脚本每次通过判断ftp返回码成功后，便删除了文件。不过这台aix系统每天不管成功失败，定时任务似乎都会发送一份邮件，通过mail，看到有4003封mail，输入4003看到最后一封，问题一目了然， tar命令报too large file。

原来aix5版本下，tar单个文件最大8G，还好虽然报了这个错，tar的目标文件夹中其它不超过8G的文件打包成功了，不然增量文件重跑也是个麻烦事。大文件DMP是全量的。于是修改脚本，将大一点的表单独做为一个DMP文件导出，这样单个文件就不超过8G了。 DC那边处理也很机智，遍历文件夹下的DMP导入，因此DC那端脚本不用动。

晚上做变更，运行兄弟聊到之前系统报inode预警，因为有个目录下文件特别多，1天4000多个，正常情况应该将一天的文件放在一个日期文件夹下。linux下单个文件夹下文件太多会受限于inode。可以通过 df -i 查看文件系统inode情况。 ulimit -a查看系统限制。


