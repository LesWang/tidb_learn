## tidb
### 下载源码
之前已经下载过tidb的源码，使用```git pull```更新至最新
### 编译
最开始使用Goland打开了项目，直接进入tidb-server文件夹下go build编译，但由于本地的go版本是go1.12，编译时某些依赖包提示需要go1.13，随后下载go1.15，切换至1.15后编译成功

后来直接使用make编译，能够正常编译成功并生成产物
### 运行
./bin/tidb-server直接能够运行起来

## pd
### 下载源码
从github直接```git clone```即可
### 编译
使用make编译，pd的编译遇到过一些问题，基本是网络问题。开始直接编译了一段时间后直接报Error 56（错误码不太确定了），没有任何错误信息。有点忘记如何操作了，可能是切换过网络，后来重试能够显示出编译信息，某些依赖包无法下载，切换至翻墙网络后，go的依赖包能够正常下载完成，最后下载依赖时又非常慢，切换至国内网络后正常编译通过
### 运行
按照github上[tikv部署文档](https://github.com/tikv/tikv/blob/master/docs/how-to/deploy/using-binary.md#deploy-the-tikv-cluster-on-a-single-machine)能够成功运行pd

## tikv
### 下载源码
从github直接```git clone```即可
### 编译
由于tikv用rust编写，先是安装了rust，按照[Contributing to tikv](https://github.com/tikv/tikv/blob/master/CONTRIBUTING.md#building-and-setting-up-a-development-workspace)文档尝试编译tikv，但是一直卡在下载依赖上，切换中科大的源还是非常慢，编译了一晚上，最后中科大的源连不上了……

### 运行
由于暂时无法解决编译问题，尝试从官网下载最新的tidb版本，直接使用官网的二进制文件，但由于官网提供的linux版，mac上无法运行，最后没有运行tikv

## 作业
### 作业要求
改写tidb，使tidb启动事务时，会打出"hello transaction"的日志
### 完成过程
从tidb-server/main.go开始，查看tidb启动过程，并查看tidb执行sql语句过程。最终在kv/kv.go找到Storage接口，里面定义了```Begin() (Transaction, error)```方法，为开始事务的方法。在store/tikv/kv.go中tikvStore的实现中添加了```logutil.BgLogger().Info("hello transaction")``` 