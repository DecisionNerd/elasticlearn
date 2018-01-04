# elasticlearn
elasticsearch 6.0 and R installation instructions for learning how to use the elasticsearch for analysis of updating data

These instructions are for Ubuntu and prepared using 16.04.03 on VMware Fusion virtual machine with 2 cores and 8g RAM.

## Update System

sudo apt-get update
sudo apt-get upgrade

## java

terminal -> `sudo apt-get install openjdk-8-jre`

## install elasticsearch

### install gdebi

terminal -> `sudo apt-get install gdebi`

### download elasticsearch

firefox -> https://www.elastic.co/downloads/elasticsearch

download the .DEB file

### install elasticsearch

terminal -> 
```
cd ~/Downloads
sudo gdebi elasticsearch-6.0.0.deb
sudo systemctl enable elasticsearch.service
```

### verify installation

terminal ->
```
user@ubuntu:~$ sudo systemctl start elasticsearch.service 
user@ubuntu:~$ systemctl status elasticsearch.service 
● elasticsearch.service - Elasticsearch
   Loaded: loaded (/usr/lib/systemd/system/elasticsearch.service; enabled; vendo
   Active: active (running) since Wed 2017-11-22 14:59:30 PST; 2s ago
     Docs: http://www.elastic.co
 Main PID: 5516 (java)
   CGroup: /system.slice/elasticsearch.service
           └─5516 /usr/bin/java -Xms1g -Xmx1g -XX:+UseConcMarkSweepGC -XX:CMSIni

Nov 22 14:59:30 ubuntu systemd[1]: Started Elasticsearch.
```
## install kibana

### Download Kibana

firefox -> https://www.elastic.co/downloads/kibana

download the DEB 64-BIT file

### Install Kibana

terminal -> 
```
cd ~/Downloads
sudo gdebi kibana-6.0.0-amd64.deb 
sudo systemctl enable kibana.service
sudo systemctl start kibana.service
```

### verify installation

firefox -> http://localhost:5601

there should be a kibana page there telling you that it has no index to show you.

## install logstash

### Download logstash

firefox -> https://www.elastic.co/downloads/logstash

IMPORTANT: It has been important in previous versions to use the TAR.GZ file

download, decompress, and move the resulting logstash-6... folder as su to `/opt/`

I've written a confirguration that pulls JSON files from wunderground api.  The same api can produce the same data in xml.  because the files are not heavily nested and because the data is in json, it works well with elasticsearch.

Moving the data into elasticsearch so that the individual settings are searchable and in a format that can be exported from elasticsearch into the follow on processor as rectangular data (a table) is our major concern and what will make or break the usability of the incoming survey data.

## begin streaming data into logstash

I run logstash from the command line when in development with the debug printed.  this extremely degrades the performance of the ingest, fyi.

terminal ->
```
cd /opt/logstash.../bin
./logstash -f wunderground.conf
```

this runs logstash with the configuration file for pulling the weather data from the wunderground.com api.  this is using my api key, which is fairly limited, so if you increase the frequency of the pulls, it will run out of juice.

now that the elasticsearch stack is complete and up and running.  you can look at the data again in kibana at 
http://localhost:5601 on the discover tab.  making simple data visualizations is straight forward and you can check out elastic.co or youtube for instructions on that for 6.0.0

## Install Rstudio server, and needed R packages

### Add APT entry for CRAN.Rstudio.com and Install R

follow instructions on https://cran.r-project.org/bin/linux/ubuntu/

install `r-base-dev`

### Install Rstudio

download Rstudio Server from https://www.rstudio.com/products/rstudio/download-server/ for Debian/Ubuntu following the instructions on the website while in the ~/Downloads folder.

terminal -> `cd ~/Downloads`

NOTE: You have already installed gdebi

Install the 64bit version NOT 32bit version

### Getting Started with Rstudio Server Open-Source

See https://support.rstudio.com/hc/en-us/articles/200552306-Getting-Started

firefox -> http://localhost:8787/

### Install R packages: ShinyDashboard, elastic, FactoMineR, plotly, leaflet, 

Firefox -> 
- Rstudio > Tools > Install Packages... 
- "shinydashboard" in the package window, it should autopopulate matches
- click install

terminal -> 
```
sudo apt-get install libcurl4-openssl-dev libssl-dev
```
Firefox -> 
- Rstudio > Tools > Install Packages... 
- "elastic" in the package window, it should autopopulate matches
- click install
- repeat for following packages:
  - "FactoMineR"
  - "Plotly"
  - "leaflet"
  - "DT"

#### Resources for more useful R packages
- [Best R packages for data import, data wrangling & data visualization](https://www.computerworld.com/article/2921176/business-intelligence/great-r-packages-for-data-import-wrangling-visualization.html)
