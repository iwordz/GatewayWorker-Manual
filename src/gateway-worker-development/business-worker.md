# BusinessWorker类的使用
BusinessWorker类其实也是基于基础的Worker开发的。由于BusinessWorker进程中无法直接操作Gateway进程的连接，也就无法获得连接对象，所以无法使用BusinessWorker的onConnect、onMessage、onClose属性，请开发者不要设置以上回调属性。开发者仍然可以设置onWorkerStart、onWorkerStop属性。

BusinessWorker的相关回调函数在项目中的Event.php中定义，具体内容参见下一节

## BusinessWorker类可以定制的内容

1、name

和Worker一样，可以设置BusinessWorker进程的名称，方便status命令中查看统计

2、count

和Worker一样，可以设置BusinessWorker进程的数量，以便充分利用多cpu资源

3、registerAddress，注册服务地址，只写格式类似于 '127.0.0.1:1236'

4、onWorkerStart

和Worker一样，可以设置BusinessWorker启动后的回调函数，一般在这个回调里面初始化一些全局数据

5、onWorkerStop

和Worker一样，可以设置BusinessWorker关闭的回调函数，一般在这个回调里面做数据清理或者保存数据工作

## 业务处理类 Events

Events类为业务处理的入口文件，当有客户端事件发生时会触发相应的回调如下：
``` (注意：Gateway 2.0.4版本以前业务处理类为Event，为了避免和Event扩展冲突，2.0.4版本以后统一改成Events类) ```

1、每个BusinessWorker进程启动时，都会触发```Events::onWorkerStart($businessworker)```回调```（此特性Gateway版本>=2.0.4才支持）```。

2、当客户端连接到Gateway时，会触发```Events::onConnect($client_id)```回调。

3、当客户端发来数据时，会触发```Events::onMessage($client_id, $data)```回调。

4、当客户端关闭时，会触发```Events::onClose($client_id)```回调。

5、每个BusinessWorker进程退出时，都会触发```Events::onWorkerStop($businessworker)```回调```（此特性Gateway版本>=2.0.4才支持）```。注意如果进程是非正常退出，例如被kill可能无法触发```onWorkerStop```。

**Events详细文档参见下一节**




