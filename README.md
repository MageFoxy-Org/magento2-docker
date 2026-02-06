Nginx: 1.18
Php: 8.3.19-fpm
Mysql: 8.0.41
Redis: 7.2
elasticsearch: 8.16.6
Magento: 2.4.7


--> DOING
## Steps:
1. Create folders:
    - db_data
    - src
    - mysql-dump
    - logs/nginx
2. Copy Magento 2.4.7 project to src/ folder
    - use composer
    cd src && composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition=2.4.8  --ignore-platform-reqs ./
    then rename nginx.conf.sample to nginx.conf
    - git clone
    - or copy
    - or use scp to download from server.
3. Copy file database to import mysql-dump/ folder and rename it to magento.sql
4. docker-compose build mage248_php --no-cache
5. docker-compose up -d
6. access mysql and change base_url
update core_config_data set value = 'http://mage248.local:8080/' where path = 'web/unsecure/base_url';
update core_config_data set value = 'http://mage248.local:8080/' where path = 'web/secure/base_url';

7. Note:
If install a new Magento site from scratch
step 7.1: docker exec -it mage248_php bash
step 7.2: 
bin/magento setup:install \
--base-url=http://mage248.local \
--db-host=mage248_mysql \
--db-name=magento \
--db-user=magento \
--db-password=magento123 \
--admin-firstname=admin \
--admin-lastname=admin \
--admin-email=admin@admin.com \
--admin-user=admin \
--admin-password=admin123 \
--language=en_US \
--currency=USD \
--timezone=America/Chicago \
--use-rewrites=1 \
--search-engine=opensearch \
--opensearch-host=opensearch \
--opensearch-port=9200 \
--opensearch-index-prefix=magento2 \
--opensearch-timeout=15

