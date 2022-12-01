# 实用脚本命名

## shell超时自动终止进程
```bash
timeout 3s sh 1.sh  # 执行1.sh脚本,执行时间超过三秒自动终止
```
或者
```bash
sh 1.sh & { sleep 3; kill $! & } # 执行1.sh, 超过3s自动杀死进程，未超过3s则执行不结束待3s后自动杀死后结束
```