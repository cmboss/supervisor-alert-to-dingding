# Supervisor 事件发送到钉钉

这是一个Supervisor的事件监听插件，用于把Supervisor产生的事件发送到钉钉。通过捕获不同Supervisor事件，达到实时告警的功能；

## 如何使用

1、下载 [supervior_alert_to_ding.sh](https://github.com/cmboss/supervisor-alert-to-dingding/blob/master/supervior_alert_to_ding.sh) 脚本；

​		把它放到你认为合适的位置，并给赋予可执行权限；

```
chmod +x supervior_alert_to_ding.sh
```

2、修改supervior_alert_to_ding.sh 参数;

```
# 日志存放的目录；
LOG_DIR=/data/logs/supervisor

# 日志文件名
LOG_FILENAME=supervior_all_alert_to_ding.log

# 修改hook url
DING_HOOK_URL='your hook url'

# 钉钉消息模板，默认为 markdown 格式 ，根据需求修改以下字段信息；
"{ \"msgtype\": \"markdown\", \"markdown\": { \"title\": \"Supervior alert\", \"text\": '![](https://xxx.com/xxx.png)\n ### 告警服务: ${alert_name}\n ### 告警类型: ${alert_type}\n ### 告警主机: ${alert_host}\n ### 告警摘要:\n ${alert_summary} \n ### 告警详情:\n ${alert_details}\n ### [点击详情]( @13888888888)' }, \"at\": { \"atMobiles\": [ \"13888888888\" ], \"isAtAll\": false } }" 
```

3、supervisord.conf文件添加如下区段；

```
[eventlistener:all_alert_to_ding]
command=sh -c /opt/scripts/supervior_alert_to_ding.sh
events=PROCESS_STATE_EXITED
user=root
autostart=true
stderr_logfile=/tmp/supervisor_to_ding_error.log
stdout_logfile=/tmp/supervisor_to_ding_app.log
```

4、重新加载修改后的supervisor.conf配置文件或重新启动supervisord;

```
supervisorctl update
```

5、测试

​	使用kill命令杀死supervisor托管的任意服务，消息机会发送到钉钉；



# Supervisor event to dingding

This is a Supervisor event listener plugin that sends events generated by the Supervisor to dingding. Real-time alarms are achieved by capturing different Supervisor events;

##  how to use

1、Download  [supervior_alert_to_ding.sh](https://github.com/cmboss/supervisor-alert-to-dingding/blob/master/supervior_alert_to_ding.sh) ；

​		Put it where you think it is and give it executable permissions;；

```
chmod +x supervior_alert_to_ding.sh
```

2、Modify the supervior_alert_to_ding.sh parameter.;

```
# directory where the log is stored；
LOG_DIR=/data/logs/supervisor

# Name of the log file;
LOG_FILENAME=supervior_all_alert_to_ding.log

# Modify hook url;
DING_HOOK_URL='your hook url'

# dingdingMessage template, the default is markdown format, modify the following field information according to requirements;；

"{ \"msgtype\": \"markdown\", \"markdown\": { \"title\": \"Supervior alert\", \"text\": '![](https://xxx.com/xxx.png)\n ### 告警服务: ${alert_name}\n ### 告警类型: ${alert_type}\n ### 告警主机: ${alert_host}\n ### 告警摘要:\n ${alert_summary} \n ### 告警详情:\n ${alert_details}\n ### [点击详情]( @13888888888)' }, \"at\": { \"atMobiles\": [ \"13888888888\" ], \"isAtAll\": false } }" 
```

3、Add the following section to the supervisord.conf file;；

```
[eventlistener:all_alert_to_ding]
command=sh -c /opt/scripts/supervior_alert_to_ding.sh
events=PROCESS_STATE_EXITED
user=root
autostart=true
stderr_logfile=/tmp/supervisor_to_ding_error.log
stdout_logfile=/tmp/supervisor_to_ding_app.log
```

4、Reload the modified supervisor.conf configuration file or restart supervisord;;

```
supervisorctl update
```

5, test it

Use the kill command to kill any service hosted by the supervisor, and the message is sent to dingding ;