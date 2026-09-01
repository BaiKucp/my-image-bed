# Docker Desktop 启动失败与备份恢复：本文参考资料

本页汇总《别急着点 Factory Reset：一次 Docker 镜像丢失复盘》涉及的官方文档和同类故障记录，供读者核对原始资料。

正文已经包含完整的问题分析和处理步骤；阅读这些链接不是解决故障的前置条件。

## Docker 官方文档

### Docker Desktop 故障排查

[Troubleshoot and diagnose](https://docs.docker.com/desktop/troubleshoot-and-support/troubleshoot/)

用于核对 Docker Desktop 的诊断、重启、清理和恢复出厂设置入口。需要注意，`Reset to factory defaults` 是恢复初始状态，不是普通重启。

### Docker Desktop 备份与恢复

[Back up and restore Docker Desktop data](https://docs.docker.com/desktop/settings-and-maintenance/backup-and-restore/)

用于核对 Docker Desktop 无法启动前后的数据备份方式，以及镜像推送、镜像导出和数据盘备份的适用场景。

### 推送镜像到远端仓库

[docker image push](https://docs.docker.com/reference/cli/docker/image/push/)

用于核对 `docker push` 的命令语义。登录 Docker Hub 本身不会自动上传或备份本地镜像。

### 将镜像导出为本地归档

[docker image save](https://docs.docker.com/reference/cli/docker/image/save/)

用于核对 `docker image save` 的命令语义。导出的 TAR 文件应存放在 Docker 数据盘之外，并保留校验值。

### Docker volume 备份、恢复与迁移

[Back up, restore, or migrate data volumes](https://docs.docker.com/engine/storage/volumes/#back-up-restore-or-migrate-data-volumes)

用于核对 volume 数据的备份方法。镜像备份不能代替数据库和 volume 的独立备份。

## Windows 平台的同类故障记录

下面两项来自 Docker 的公开 GitHub 问题区，由用户提交，不代表 Docker 官方已经确认了本文所述故障的唯一根因。它们的价值在于提供了相似的错误路径、触发条件和处理结果。

### GitHub Issue #531

[Backend aborts startup when it cannot remove its own AF_UNIX socket](https://github.com/docker/desktop-feedback/issues/531)

记录了 Windows 上 Docker Desktop 无法移除 `dockerInference` 和 Secrets Engine socket、随后中止启动的现象，并给出了停止 Docker、重命名相关目录后重新启动的处理记录。

### GitHub Issue #554

[Startup crash: cannot remove stale unix socket files after unclean shutdown](https://github.com/docker/desktop-feedback/issues/554)

记录了 Docker Desktop 异常退出或系统待机后，残留 socket 无法删除、重启 Windows 后问题仍存在的相似现象。

## 资料使用边界

- Docker 官方文档用于确认产品操作的语义与备份方法；
- GitHub Issue 用于比对同类症状，不能单独作为官方根因结论；
- 本次事故中能够确认的是错误发生位置和后续 Reset 带来的数据状态变化；
- 故障前一刻究竟发生了休眠、强制结束还是进程崩溃，没有完整日志，因此不作确定判断。

资料核对日期：2026 年 9 月 1 日。
