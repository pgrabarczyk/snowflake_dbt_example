# snowflake-dbt-example
Example usage of Snowflake with dbt

## My configuration
```bash
$ uname -a
Linux pgrabarczyk 5.13.0-52-generic #59~20.04.1-Ubuntu SMP Thu Jun 16 21:21:28 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
$ cat /etc/*-release | grep DISTRIB_DESCRIPTION
DISTRIB_DESCRIPTION="Ubuntu 20.04.4 LTS"
$ bash --version
GNU bash, version 5.0.17(1)-release (x86_64-pc-linux-gnu)
...
$ pip --version
pip 20.0.2 from /usr/lib/python3/dist-packages/pip (python 3.8)
```

## Create Snowflake account
https://www.snowflake.com/login/

## Install SnowSQL (Optional)

```bash
# https://docs.snowflake.com/en/user-guide/snowsql-install-config.html
curl -O https://sfc-repo.azure.snowflakecomputing.com/snowsql/bootstrap/1.2/linux_x86_64/snowsql-1.2.22-linux_x86_64.bash
bash snowsql-linux_x86_64.bash
snowsql -v # reboot may be needed
```

## Create warehouse, database (May be done via Snowsight(Web Interface) or SnowSQL)

```sql
CREATE WAREHOUSE IF NOT EXISTS PGRABARCZYK_WH
WITH
  WAREHOUSE_SIZE = XSMALL
  MAX_CLUSTER_COUNT = 1
  MIN_CLUSTER_COUNT = 1
  SCALING_POLICY = STANDARD
  AUTO_SUSPEND = 600
  AUTO_RESUME = TRUE
  INITIALLY_SUSPENDED = FALSE
  COMMENT = 'pgrabarczyk warehouse for example usage'
```

```sql
CREATE DATABASE IF NOT EXISTS PGRABARCZYK_DB
    COMMENT = 'pgrabarczyk db for example usage'
```

## Install DBT CLI

```bash
# https://docs.getdbt.com/dbt-cli/install/pip
sudo apt-get install git libpq-dev python-dev python3-pip
sudo apt-get remove python-cffi
sudo pip install --upgrade cffi
pip install --upgrade pip wheel setuptools
python3 -m venv venv
source venv/bin/activate
pip install cryptography~=3.4
pip install dbt-snowflake
dbt --version
```

## Create DBT profile file

```bash
$ dbt init snowflake_dbt_example
16:49:20  Running with dbt=1.2.0
Which database would you like to use?
[1] snowflake

(Don't see the one you want? https://docs.getdbt.com/docs/available-adapters)

Enter a number: 1
account (https://<this_value>.snowflakecomputing.com): XXXXXXXXXXXXX
user (dev username): pgrabarczyk
[1] password
[2] keypair
[3] sso
Desired authentication type option (enter a number): 1
password (dev password): 
role (dev role): ACCOUNTADMIN
warehouse (warehouse name): PGRABARCZYK_WH
database (default database that dbt will build objects in): PGRABARCZYK_DB
schema (default schema that dbt will build objects in): PUBLIC
threads (1 or more) [1]: 
16:50:08  Profile snowflake_dbt_example written to /home/pgrabarczyk/.dbt/profiles.yml using target's profile_template.yml and your supplied values. Run 'dbt debug' to validate the connection.
16:50:08  
Your new dbt project "snowflake_dbt_example" was created!

For more information on how to configure the profiles.yml file,
please consult the dbt documentation here:

  https://docs.getdbt.com/docs/configure-your-profile

One more thing:

Need help? Don't hesitate to reach out to us via GitHub issues or on Slack:

  https://community.getdbt.com/

Happy modeling!
```

## Check connection
```bash
dbt debug
```
