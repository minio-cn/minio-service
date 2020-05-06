# Systemd service 运行 MinIO

Systemd 脚本为 MinIO服务器进行设计和使用的方式.

## 安装

- 将Systemd脚本配置为从/usr/local/bin /运行二进制文件。
- 下载二进制文件。 在http://www.minio.org.cn/ 中找到二进制文件的相关链接。

## 创建默认配置

该文件用作MinIO系统服务的输入。 使用此文件可添加具有正确路径的“ MINIO_VOLUMES”，可使用“ MINIO_OPTS”来添加MinIO服务器选项，例如“ certs-dir”，“ address”。 在此文件中，MinIO凭据也可以是“ MINIO_ACCESS_KEY”和“ MINIO_SECRET_KEY”。


```sh
$ cat <<EOT >> /etc/default/minio
# Volume to be used for MinIO server.
MINIO_VOLUMES="/tmp/minio/"
# Use if you want to run MinIO on a custom port.
MINIO_OPTS="--address :9199"
# Access Key of the server.
MINIO_ACCESS_KEY=Server-Access-Key
# Secret key of the server.
MINIO_SECRET_KEY=Server-Secret-Key

EOT
```

## Systemctl

下载 `minio.service` 在  `/etc/systemd/system/`
```
( cd /etc/systemd/system/; curl -O https://raw.githubusercontent.com/minio/minio-service/master/linux-systemd/minio.service )
```
Note: If you want to bind to a port < 1024 with the service running as a regular user, you will need to add bind capability via the AmbientCapabilities directive in the minio.service file:

```
[Service]
AmbientCapabilities=CAP_NET_BIND_SERVICE
WorkingDirectory=/usr/local/
```
### enable开机启动
```
systemctl enable minio.service
```

### 关闭 MinIO 服务
```
systemctl disable minio.service
```

## 提示

- 替换 ``User=minio-user`` and ``Group=minio-user`` in minio.service file with your local setup.
- Ensure that ``MINIO_VOLUMES`` source has appropirate write access.
