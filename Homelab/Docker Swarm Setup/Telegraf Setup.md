

Download Key from Influxdb
```bash
sudo wget https://repos.influxdata.com/influxdb.key
```

Setup key for use in the system
```bash
echo '23a1c8836f0afc5ed24e0486339d7cc8f6790b83886c4c96995b88a061c5bb5d influxdb.key' | sha256sum -c && cat influxdb.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/influxdb.gpg > /dev/null
```

Setup Repository for Telegraf
```bash
echo 'deb [signed-by=/etc/apt/trusted.gpg.d/influxdb.gpg] https://repos.influxdata.com/debian stable main' | sudo tee /etc/apt/sources.list.d/influxdata.list
```

Install Telegraf
```bash
sudo apt-get update && sudo apt-get install telegraf
```