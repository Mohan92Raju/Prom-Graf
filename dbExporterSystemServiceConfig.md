# <p style="text-align: center;">Configure MySQL Exporter as System Service</p>

>Add Prometheus system user and group

```ruby
sudo groupadd --system prometheus 
sudo useradd -s /sbin/nologin --system -g prometheus prometheus
```
>Configure the Database Credentials
```ruby
sudo vi /etc/.mysqld_exporter.cnf
 
# Add correct username and password for user create 
[client] 
user=mysqld_exporter 
password=StrongPassword 
 
# Set ownership permissions: 
sudo chown root:prometheus /etc/.mysqld_exporter.cnf
```

>Create systemd Unit File
```ruby
sudo vim /etc/systemd/system/mysql_exporter.service
 
#Add the following content
[Unit]
	Description=Prometheus MySQL Exporter
	After=network.target
	User=prometheus
	Group=prometheus
 
	[Service]
	Type=simple
	Restart=always
	ExecStart=/usr/local/bin/mysqld_exporter \
	--config.my-cnf /etc/.mysqld_exporter.cnf \
	--collect.global_status \
	--collect.info_schema.innodb_metrics \
	--collect.auto_increment.columns \
	--collect.info_schema.processlist \
	--collect.binlog_size \
	--collect.info_schema.tablestats \
	--collect.global_variables \
	--collect.info_schema.query_response_time \
	--collect.info_schema.userstats \
	--collect.info_schema.tables \
	--collect.perf_schema.tablelocks \
	--collect.perf_schema.file_events \
	--collect.perf_schema.eventswaits \
	--collect.perf_schema.indexiowaits \
	--collect.perf_schema.tableiowaits \
	--collect.slave_status \
	--web.listen-address=0.0.0.0:9104
	
	[Install]
	WantedBy=multi-user.target
```
>Reload systemd and start mysql_exporter service
```ruby
$sudo systemctl daemon-reload
$sudo systemctl enable mysql_exporter
$sudo systemctl start mysql_exporter
```

>Configure MySQL Endpoint to be Scraped by Prometheus
```ruby
scrape_configs: 
- job_name: mysql_server1
  static_configs: 
   - targets: ['<Machine_IP>:9104'] 
     labels: 
       alias: db1
```
> Reload Prometheus Configuration
```ruby
curl -x -XPOST localhost:9090/-/reload
```
