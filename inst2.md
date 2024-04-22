
---

# Documentation for Managing the Dashboard

This documentation provides step-by-step instructions for setting up and managing your dashboard environment, including installing Ruby with rbenv, setting up MySQL, and deploying with Docker.

## Installing Ruby with rbenv

Ruby is a dynamic, open-source programming language with a focus on simplicity and productivity. This section covers the installation of Ruby using rbenv on Unix-based systems.

### Step 1: Install rbenv and Dependencies

Before installing Ruby, you need to install its dependencies. Begin by updating your package list and installing the necessary packages:

```bash
sudo apt update
sudo apt install git curl libssl-dev libreadline-dev zlib1g-dev autoconf bison build-essential libyaml-dev libreadline-dev libncurses5-dev libffi-dev libgdbm-dev
```

Then, install rbenv using the following commands:

```bash
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc
type rbenv
```

### Step 2: Install Ruby Using ruby-build

With rbenv and ruby-build installed, you can now install Ruby:

```bash
rbenv install -l  # List all available versions of Ruby
rbenv install 3.3.0  # Install Ruby 3.3.0
rbenv global 3.3.0  # Set Ruby 3.3.0 as the default version
ruby -v  # Verify the installation
```

### Step 3: Installing Dashboard Dependencies

Install essential build tools and the `smashing` gem, a framework for building dashboards:

```bash
sudo apt install build-essential
gem install bundler
gem install smashing
```

## Setting Up MySQL

This section guides you through the installation of MySQL and configuring it to accept connections for your dashboard.

### Step 4: Install MySQL

Follow the instructions provided at the [Guide to install MySQL on Ubuntu](https://www.devart.com/dbforge/mysql/how-to-install-mysql-on-ubuntu/) for versions 18.04, 20.04, and 22.04.

### Step 5: Configure MySQL Environment

Set the MySQL environment for your Ubuntu system:

```bash
export DB_HOST=192.168.0.125
export DB_USERNAME=root
export DB_PASSWORD=<Password>
export DB_NAME=RPA_Dashboard
export DB_PORT=3306
```

### Step 6: Configure MySQL to Accept Connections

Modify the MySQL configuration to accept connections from any IP:

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

In the file, change `bind-address` to `0.0.0.0` and restart MySQL:

```bash
sudo systemctl restart mysql
```

### Step 7: Adjust MySQL User Permissions

Ensure the MySQL user can connect from any host and manage user credentials:

```sql
-- Check existing users
SELECT user, host FROM mysql.user WHERE user = 'root';

-- Modify or create user
CREATE USER 'root'@'%' IDENTIFIED BY 'Password@123';
GRANT ALL PRIVILEGES ON RPA_Dashboard.* TO 'root'@'%';
FLUSH PRIVILEGES;

-- Verify changes
SHOW GRANTS FOR 'root'@'%';

-- Test connection
mysql -h 192.168.0.125 -u root -pPassword@123
```

## Deploying the Dashboard with Docker

### Step 8: Download dashboad template

```bash
https://github.com/arun12341234/my-dashing-dashboard.git
````
In docker-compose <.yml> file set your mysql credential.



## Deploying the Dashboard with Docker

### Step 9: Build and Run the Docker Container

Build your Docker image and run your dashboard locally:

```bash
sudo docker build -t my-dashing-dashboard .
sudo docker run -d -p 3030:3030 --name my-dashboard my-dashing-dashboard
```

Manage your Docker container:

```bash
-- List Container:
sudo docker ps -a

-- Stop Container:
sudo docker stop my-dashboard

-- Start Container:
sudo docker start my-dashboard

-- Remove Container (if you need to delete it and clean up):
sudo docker rm -f my-dashboard
```

## Updating Dashboard Data

For updating the dashboard data, refer to the [API Dashboard plugin on GitHub](https://github.com/arun12341234/Api-dashboard).

---

