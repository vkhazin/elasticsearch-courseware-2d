# Exercise: Setup Kibana #

* Log-in into your sand-box
* Double check elasticsearch service is running:
```
sudo service elasticsearch start
```
* From terminal download and install Public Signing Key:
```
curl https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```
* From terminal add repository definitions:
```
echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list
```
* Update repositories info and install Kibana:
```
sudo apt-get update && sudo apt-get install kibana
```
* Start Kibana service:
```
sudo service kibana start
```
* Populate sample data:
```
curl -O https://s3.us-east-2.amazonaws.com/elasticsearch-courseware/sample-data/logs.json
curl  -XPOST 'localhost:9200/_bulk' \
      -H 'content-type: application/json' \
      --data-binary "@logs.json"
```
* By default Kibana listens to localhost and it won't be really helpful in most environments
* Edit kibana.yml: ```server.host: 0.0.0.0```:
```
sudo nano /etc/kibana/kibana.yml
```
* Restart Kibana service:
```
sudo service kibana restart
```
* Open browser to http://ip-address:5601, where ip address is the same as for ssh connection
* Kibana requires configuration before it can display data: look for 'Index Patterns', it keeps moving between releases
* To find out what indices we have in the cluster:
```
curl localhost:9200/_cat/indices
```
* After typing index name pattern with star as a wildcard tab out from the field to get the fields refreshed
* Kibana (by default) requires a date-time field to filter data on
* By default Kibana displays data for the last 15 minutes and in a simulated environment it is often an empty result set
* Look at the top-right corner to adjust the timeframe
* You should be able to see some data now, if not, common troubles are index pattern configuration and a timeframe selection
* There is a star icon or a maybe another way in the future version to set index pattern as a default
* Head to the 'Discover' link on the left
* Adjust time-frame in the top-right corner
* Use search box to locate some record
* Select 'add' link next to few fields to present selected fields on the results pane
* Select any record and switch between text and json views
* Save search using link on top of the page
* Get comfortable with running Lucene syntax queries using the search box - you are going to need it moving forward
* Notice drill down capabilities using the chart on the top of the page using a mouse
* We will look into other links a bit later...