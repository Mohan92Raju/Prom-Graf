# <p style="text-align: center;">Configure MySQL Exporter as System Service</p>

>Add Prometheus system user and group

```ruby
sudo groupadd --system prometheus 
sudo useradd -s /sbin/nologin --system -g prometheus prometheus
```
Configure the Database Credentials
