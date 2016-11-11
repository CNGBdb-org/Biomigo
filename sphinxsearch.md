#Sphinx search engine
#About
Go to the official website yourself. URI: http://sphinxsearch.com/about/sphinx/
#Example test app
http://172.17.10.19/laboratory/sphinx/search0/
Currently, Sphinx backend of this app is deployed on 4 nodes (192.168.2.31-34, 172.17.10.19), with all meta data of gene and tumour type 
(~40,000 entries) of cancerdb.
A much more previous test query on 1 node with 5 millions variaiton meta data from ensemble variaiton database cost less than 0.05s.
Test will continue on this app for more data.
#Advantage (well, just my personal consider)
##Easy to install
You jsut need to run two or three commands (required root)
on CentOS:
```
yum install postgresql-libs unixODBC
rpm -ivh http://sphinxsearch.com/files/sphinx-2.2.11-2.rhel6.x86_64.rpm
```
on Unbuntu:
```
apt-get install mysql-client unixodbc libpq5
wget http://sphinxsearch.com/files/sphinxsearch_2.2.11-release-1~trusty_amd64.deb
dpkg -i /path/to/sphinxsearch_2.2.11-release-1~trusty_amd64.deb
```
##Easy to deploy
The default config file is `/etc/sphinx/sphinx.conf` on CentOS (`/etc/sphinxsearch/sphinxsearch.conf` on Ubuntu).
The block `searchd` defines how the `searchd` (`sphinxsearch` on Ubuntu) server runs.
```
searchd
{
    listen = 9312 # format is: ( address ":" port | port | path ) [ ":" protocol ]
    listen = 9306:mysql41 # listen to the mysql client connection through 9306, this allows you to use `mysql -h 0 -P 9306`.
    log    = /var/log/sphinx/searchd.log # searchd program log
    query_log = /var/log/sphinx/query.log # query log
    pid_file = /var/run/sphinx/searchd.pid
    binlog_path = /var/lib/sphinx/ # binary log, for real time index only
}
```
The block `common` defines some common settings affect indexing and search serving.
```
common
{
    lemmatizer_base = /usr/local/share # only required when `morphology` in any `index` block is defined.
}
```
The block `indexer` affects resource cost and performance of the program `indexer` while indexing data.
```
indexer
{
    mem_limit = 256M # limit the memory used while indexing data
}
```
