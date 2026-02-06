# Magento 2.4.8 Docker Environment - Fresh Installation

A complete Docker-based environment for Magento 2 development, configured with Nginx, PHP-FPM, MySQL, Valkey, and OpenSearch.

## 🛠 Technology Stack

This environment is built with the following components:

| Component | Version | Service Name |
|-----------|---------|--------------|
| **Magento** | 2.4.8 | - |
| **Nginx** | 1.18 | `mage248_nginx` |
| **PHP** | 8.3.19-fpm | `mage248_php` |
| **MySQL** | 8.0.41 | `mage248_mysql` |
| **Valkey** (Redis) | 8.0 | `mage248_valkey` |
| **OpenSearch** | 3.0 | `mage248_opensearch` |
| **PhpMyAdmin** | 5.2 | `mage248_phpmyadmin` |

## 🚀 Installation & Setup

### 1. Prepare Directory Structure
Create the necessary directories for persistence and logs:

```bash
mkdir -p db_data src logs/nginx osdata
```

### 2. Start Containers
Start the environment:

```bash
docker-compose up -d
```

### 3. Setup Source Code
Install the Magento 2 source code into the `src/` directory.

**Option A: Using Composer (Recommended)**

***Access the PHP container***:

```bash
docker exec -it mage248_php bash
```

```bash
composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition=2.4.8 ./
```

```bash
mv nginx.conf.sample nginx.conf
```
> **Note**: This requires valid keys from [repo.magento.com](https://repo.magento.com/).

**Option B: Existing Source**
Clone your repository or copy your existing Magento files into the `src/` folder.

***Access the PHP container***:

```bash
docker exec -it mage248_php bash
```

```bash
composer install
```

```bash
mv nginx.conf.sample nginx.conf
```

### 4. Fresh Installation
Once the environment is running, update the base URLs in the database to match the local environment.

**Database Connection**
- **Host**: `127.0.0.1` (Port: `3307`)
- **User**: `magento`
- **Password**: `magento123`
- **Database**: `magento`

**Run the installation command**:

```bash
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
```

## 📝 Usage Notes

- **PhpMyAdmin** is available at `http://localhost:81/index.php`
- **Nginx** listens on `http://localhost:80` (or configured port)
- **MySQL** is exposed on port `3307`
- **Valkey** is exposed on port `6379`
- **OpenSearch** is exposed on port `9200`
- **Magento 2.4.8** is exposed on `http://mage248.local`


## Build Images

```bash
docker-compose build mage248_php --no-cache
```
