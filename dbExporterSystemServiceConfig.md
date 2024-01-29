# <p style="text-align: center;">Configure MySQL Exporter as System Service</p>


# <p style="text-align: center;">Add Prometheus system user and group</p>

```ruby
sudo groupadd --system prometheus 
sudo useradd -s /sbin/nologin --system -g prometheus prometheus
```
