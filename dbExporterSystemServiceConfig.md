Configure MySQL Exporter as System Service

# <p style="text-align: center;">Add Prometheus system user and group</p>

```
sudo groupadd --system prometheus 
sudo useradd -s /sbin/nologin --system -g prometheus prometheus
```
