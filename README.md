# xia_ce
瞎测为二开xia_sql，主要套了下xia_sql的UI和部分逻辑；
目前存在一个较为明显的BUG，无法识别 json={"xxx":xxx,"xxx":xxx} 这种嵌套的json，暂时未解决；其次是由于默认的检测指令为sleep 3，中间是带空格的，部分系统会因为无法识别空格导致错误，后续会考虑把这个也加上，暂时大家可以在自定义payload里增加这个sleep+3或者是sleep%203,临时更新了一个版本0.1c，可以在第二和第三个日志里看到自定义的超时标记，暂时没有在第一个日志里加，因为这样可能会产生较多的误报
瞎测主要针对iot设备，如：路由器、网关、防火墙的常规命令执行、命令注入、溢出测试。

由于目前市面上大多的设备几乎都支持sleep命令，所以瞎测插件默认使用的payload为;sleep 3和|sleep 3 ；

日志记录根据 md5(url+请求体+请求方法)进行排重，最左侧的日志记录单条URL FUZZ记录和结果，右侧的日志对应每次fuzz的过程和结果，如果原始响应和注入后的响应时间超过3秒，则会出现Time + 超时时间字样。所以如果服务器响应慢，会有延迟，产生误报；

同时，右下角的日志也会记录fuzz记录，包括时间、参数、payload和是否存在异常的记录，方便我们后续排查
![image](https://github.com/user-attachments/assets/d2d25710-d6e5-4858-886a-d9e9d8bf3870)

如果设备不支持sleep指令，我们也可以利用插件的自定义payload配合自定义返回包进行修改，从而实现定制化
![image](https://github.com/user-attachments/assets/4b010550-ae01-4d5a-a6e0-9709ed42ab51)

![image](https://github.com/user-attachments/assets/36be9f6b-ebdb-4f3e-9706-faf365f59653)

除此之外，瞎测插件支持溢出测试，默认插件会对参数值进行 * 256 (0x100) 或者 * 500，此处主要针对程序使用不安全的输出方法(如：strcpy 、sprintf)
![image](https://github.com/user-attachments/assets/a2ee9b18-ac3e-4c29-b2a4-e416835e1f35)

需要注意的是，执行此操作设备可能会挂掉，同时如果参数是空的，是无法复制的，可以手动添加


