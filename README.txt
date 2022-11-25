// create forder : airflow_tutorial
// use terminal create project :
pwd

python3 --version

python3 -m venv py_env

source py_env/bin/activate
// use python3 --version to map 

# install from pypi using pip
pip install 'apache-airflow==2.4.2' \
 --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.4.2/constraints-3.10.txt"

# airflow needs a home, ~/airflow is the default,
# but you can lay foundation somewhere else if you prefer
# (optional)
export AIRFLOW_HOME=~/airflow

# initialize the database
airflow db init

#account setting
airflow user create --username admin --firstname firstname --lastname lastname --role Admin --email admin@domain.com

# start the web server, default port is 8080
airflow webserver -p 8080

# start the scheduler
airflow scheduler

# visit localhost:8080 in the browser and enable the example dag in the home page

NOTE : 

On November 2020, new version of PIP (20.3) has been released with a new, 2020 resolver. This resolver does not yet work with Apache Airflow and might leads to errors in installation - depends on your choice of extras. In order to install Airflow you need to either downgrade pip to version 20.2.4 pip upgrade --pip==20.2.4 or, in case you use Pip 20.3, you need to add option --use-deprecated legacy-resolver to your pip install command.

Upon running these commands, Airflow will create the $AIRFLOW_HOME folder and lay an “airflow.cfg” file with defaults that get you going fast. You can inspect the file either in $AIRFLOW_HOME/airflow.cfg, or through the UI in the Admin->Configuration menu. The PID file for the webserver will be stored in $AIRFLOW_HOME/airflow-webserver.pid or in /run/airflow/webserver.pid if started by systemd.

Out of the box, Airflow uses a sqlite database, which you should outgrow fairly quickly since no parallelization is possible using this database backend. It works in conjunction with the airflow.executors.sequential_executor.SequentialExecutor which will only run task instances sequentially. While this is very limiting, it allows you to get up and running quickly and take a tour of the UI and the command line utilities.

# run your first task instance
airflow run example_bash_operator runme_0 2015-01-01
# run a backfill over 2 days
airflow backfill example_bash_operator -s 2015-01-01 -e 2015-01-02