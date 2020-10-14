# init
Optimize the system with rc-local.service

#使用rc-local服务在启动阶段优化系统参数
cat /usr/lib/systemd/system/rc-local.service
[Unit]
Description=/etc/rc.d/rc.local Compatibility
ConditionFileIsExecutable=/etc/rc.d/rc.local
Requires=network-online.target  #新加
After=network-online.target  #修改

[Service]
Type=forking
ExecStart=/etc/rc.d/rc.local start
TimeoutSec=0
RemainAfterExit=yes

systemctl daemon-reload #修改后生效

chmod +x /etc/rc.d/rc.local #增加执行权限
cat /etc/rc.d/rc.local #开机自启动脚本内容

touch /var/lock/subsys/local
/bin/bash  <path>/set_irq_affinity.sh eth0 eth1 eth2 > /tmp/start_1.log 2>&1  #根据网卡数量配置
/bin/bash  <path>/set_mem_page.sh  #关闭透明大页
/bin/su - pg -c "/bin/sh /home/pg/start.sh > /tmp/start_pg.log  2>&1"  #以pg用户启动postgres


