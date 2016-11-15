#Sphinx search engine
#About
Go to the official website yourself. 
URI: http://sphinxsearch.com/about/sphinx/
#Example test app
http://172.17.10.19/laboratory/sphinx/search0/

The test on large amount entries searching(same query on ensembl_variation_part_dist or ensembl_variation_dist) shows that shard distributed index save half time cost to the non-sharding index. On small amount entries index, this feature is not obvious.

Test index:
- cancerdb_gene_part_dist: shard distributed index of ~36,000 gene entries on 4 nodes
- cancerdb_gene_dist: mirror distributed index of ~36,000 gene entries on 4 nodes
- cancerdb_tumour_type_dist: mirror distributed index of ~30 tumour type entries on 4 nodes
- ensembl_variation_part_dist: shard distributed index of 4,000,000 variation entries on 4 nodes
- ensembl_variation_dist: local index of 4,000,000 variation entries on 1 node

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
    # Here only lists the most required or important settings. Will continue to update.
    listen = 9312 # format is: ( address ":" port | port | path ) [ ":" protocol ]
    listen = 9306:mysql41 # listen to the mysql client connection through 9306, this allows you to use `mysql -h 0 -P 9306`.
    log    = /var/log/sphinx/searchd.log # searchd program log
    query_log   = /var/log/sphinx/query.log # query log
    pid_file    = /var/run/sphinx/searchd.pid
    binlog_path = /var/lib/sphinx/ # binary log, for real time index only
}
```
The block `common` defines some common settings affect indexing and search serving.
```
common
{
    # Here only lists the most required or important settings. Will continue to update.
    lemmatizer_base = /usr/local/share # only required when `morphology` in any `index` block is defined.
}
```
The block `indexer` affects resource cost and performance of the program `indexer` while indexing data.
```
indexer
{
    # Here only lists the most required or important settings. Will continue to update.
    mem_limit = 256M # limit the memory used while indexing data
}
```
##Easy to index
The defining of data source and index method is required for data index. These settings are defined in `source` and `index` block.
```
source source_name
{
    # Here only lists the most required or important settings. Will continue to update.
    type = none # define the source type, values are mysql, pgsql, mssql, xmlpipe2, tsvpipe, csvpipe, odbc
    sql_host = none # for mysql, pgsql, mssql only
    sql_port = none # for mysql, pgsql, mssql only
    sql_user = none # for mysql, pgsql, mssql only
    sql_pass = none # for mysql, pgsql, mssql only
    ql_db    = none # for mysql, pgsql, mssql only
    sql_sock = none # while setting this, leave `sql_host` and `sql_port` blank, for mysql, pgsql, mssql only
    sql_query_pre = none # While using mysql, you may need to set the charset, etc. This is for that purpose. for mysql, pgsql, mssql only
    sql_query = none # This is the most important settings for mysql, pgsql, mssql source data. Defines the main document fetch query.
    sql_range_step = 1024 # This can split the `sql_query` into multi-part-query, it's useful if the total data is so large. For mysql, pgsql, mssql only
    sql_attr_uint, sql_attr_bool, sql_attr_bigint, sql_attr_timestamp, sql_attr_float, sql_attr_multi, sql_attr_string, sql_attr_json = none # for mysql, pgsql, mssql only, declare attributes that will be stored but not be indexed. That means these fields can only be used for sorting and filtering, but not searching.
    sql_field_string = none # for mysql, pgsql, mssql only, declare fields that will bot only be stored, but also be indexed.    
}
index index_name
{
    # Here only lists the most required or important settings. Will continue to update.
    type = plain # available values are plain, distributed, rt, template
    source = source_name
    path = none # the file path stores the index data file
    morphology = none # available values are lemmatize_ru, lemmatize_en, lemmatize_de, lemmatize_ru_all, lemmatize_en_all, lemmatize_de_all, stem_en, stem_ru, stem_enru, stem_cz, stem_ar, soundex, metaphone, rlp_chinese, rlp_chinese_batched; comma-separated; if use this agument, you must set lemmatizer_base in the common block, and put the related lemmatizer file in that path
    dict = keywords # available value is keywords, crc
    stopwords = none # absolute paths of stopword files list; space separated
    min_word_len = 1 # minimum indexed word length
    min_infix_len = 0 # minimum infix prefix length to index
    max_substring_len = 0 # maximum substring (either prefix or infix) length to index; you can use this argument only when dict = crc
    html_strip = 0 # available values are 0, 1; whether to strip HTML markup from incoming full-text data
    index_exact_words = 0 # available values are 0, 1; whether to index the original keywords along with the stemmed/remapped versions
    expand_keywords = 0 # available values are 0, 1; expand keywords with exact forms and/or stars when possible
    # below are for type = distributed only
    ha_strategy = random # available values are random, nodeads, noerrors, roundrobin; agent mirror selection strategy, for load balancing
    local = none # the local index declaration in the distributed index
    agent = none # remote agent declaration in the distributed index, format is address-list:index-list, example:
    # assume there is a config below in a distributed index block:
    # local = index1
    # agent = hostname1:port:index2
    # agent = hostname2:port:index3
    # agent = hostname3:port|hostname4:port:index1
    # We can tell from this config: index1, index2 and index3 are all just a part of an completed index, that means the index has be splited into three pieces; and there are two mirror indexes for index1 on hostname3 and hostname4
}
```
After the settings are completed, run `sudo indexer --all --rotate` to update all indexes while the `searchd` server is already up. Adding `--rotate` is necessary to update the indexes to the `searchd` server without restarting the server. You can just run `sudo indexer --rotate index_name` to update the special index.
##High customizability
What and how will the data be stored and indexed, all depend on your mind.
##High availability, load balancing
Sphinx supports features of distributing and sharding index, and using multi-threads to search. Example:
- Sharding one large index into local/distributed multi-pieces, and using threads to speed up searching (HA)
- deploy mirror indexes on different nodes, sphinx will automatically control the balance (LB)
- combine the two strategies to build a large cluster of HA/LB
#Disadvantage
##Manually sharding
Sphinx does not provide a setting for automatically sharding index into small pieces. All settings of shards must be manually defined. When shards grow or content of indexes vary, it is hard to maintain.
##Hard disk cost of indexing
The Operation of indexing (the `indexer` program) cost a large amount of hard disk. Example: indexing 1,000,000 entries with about 25 fields costs ~15GB space.
