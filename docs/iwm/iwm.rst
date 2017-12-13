########################################
Overview of Intelligent Workload Manager
########################################

SUNFISH Federation provides automated services for joining and leaving the federation, as well as
an interface to the available Federation services for a Service Consumer with ability to request
optimized service list of services matching Consumer requirements better. A common aspect of all
use cases is requirement to be able to retrieve information about the Federation resources and
optionally schedule execution of a workload on a particular service provider. Within SUNFISH, a
component responsible for delivering such functionality is called **Intelligent Workload Manager**
(IWM).

Optimization model applicable to the scenario of Service provisioning by Service Consumer is
offering an improvement over local scheduling while imposing as little as possible of additional
overhead on the definition of the workload requirements. Improvement means achieving a better
outcome regarding user-defined parameters (e.g. cost) while preserving the strict
requirements for the job payload. The goal of the model is to offer an added value over local scope
of resources by finding and managing a globally optimal target for the Service Consumer’s planned
workload.

Optimisation model is a logical component exposed to the user in form of an optional ordering and
filtering capability used during provider lookup request.

IWM is based on open-source Waldur cloud brokerage platform. The latter is extended to include more
fine-grained optimisation capability. The functionality developed within SUNFISH has been integrated
with the upstream.

###########
Screenshots
###########

Screenshots below are taken from a demo deployment of IWM in a federation.


.. figure:: ../images/screenshots/iwm-login.png
  :width: 400 px

  Login view of IWM frontend, white-labelled to a concrete federation.

.. figure:: ../images/screenshots/iwm-providers.png
  :width: 400 px

  Adding federation service providers to IWM.

.. figure:: ../images/screenshots/iwm-tenants.png
  :width: 400 px

  Listing registered SUNFISH tenants within an IWM.

.. figure:: ../images/screenshots/iwm-plan-1.png
  :width: 400 px

  Visual interface to optimisation API for finding the best option for a planned infrastructure.

.. figure:: ../images/screenshots/iwm-plan-2.png
  :width: 400 px

  Results of the optimisation with 2 service providers in the federation.


###############################
Instructions for deploying IWM
###############################

IWM functionality has been integrated into Waldur. As such, deployment of IWM is done in the same
fashion as upstream. Installation script is below. Deployment requirements are:

- CentOS 7 or other RHEL7-compliant operating system
- At least 8GB of RAM, preferably 2 cores or more.


.. code-block:: bash

    yum clean all
    yum -y update

    # Configure repositories
    yum -y install epel-release
    yum -y install https://download.postgresql.org/pub/repos/yum/9.5/redhat/rhel-7-x86_64/pgdg-centos95-9.5-2.noarch.rpm
    yum -y install https://opennodecloud.com/centos/7/elastic-release.rpm
    yum -y install https://opennodecloud.com/centos/7/waldur-release.rpm

    # Set up PostgreSQL
    yum -y install postgresql95-server
    /usr/pgsql-9.5/bin/postgresql95-setup initdb
    systemctl start postgresql-9.5
    systemctl enable postgresql-9.5

    su - postgres -c "/usr/pgsql-9.5/bin/createdb -EUTF8 waldur"
    su - postgres -c "/usr/pgsql-9.5/bin/createuser waldur"

    # Set up Redis
    yum -y install redis
    systemctl start redis
    systemctl enable redis

    # Set up Elasticsearch
    yum -y install elasticsearch java

    systemctl start elasticsearch
    systemctl enable elasticsearch

    # Set up Logstash
    yum -y install logstash

    cat > /etc/logstash/conf.d/waldur-events.json <<EOF
    input {
      tcp {
        codec => json
        port => 5959
        type => "waldur-event"
      }
    }

    filter {
      if [type] == "waldur-event" {
        json {
          source => "message"
        }

        mutate {
          remove_field => [ "class", "file", "logger_name", "method", "path", "priority", "thread" ]
        }

        grok {
          match => { "host" => "%{IPORHOST:host}:%{POSINT}" }
          overwrite => [ "host" ]
        }
      }
    }

    output {
      elasticsearch { }
    }
    EOF

    systemctl start logstash
    systemctl enable logstash

    # Set up Waldur Core
    yum -y install waldur-core

    su - waldur -c "waldur migrate --noinput"

    systemctl start waldur-uwsgi
    systemctl enable waldur-uwsgi

    systemctl start waldur-celery
    systemctl enable waldur-celery

    systemctl start waldur-celerybeat
    systemctl enable waldur-celerybeat

    su - waldur -c "waldur createstaffuser -u admin -p admin"

    # Set up Waldur MasterMind
    yum -y install centos-release-openstack-pike
    yum -y install waldur-mastermind

    su - waldur -c "waldur migrate --noinput"

    systemctl restart waldur-uwsgi
    systemctl restart waldur-celery
    systemctl restart waldur-celerybeat

    # Set up Waldur HomePort
    yum -y install waldur-homeport

    # Set up Nginx
    yum -y install nginx

    systemctl start nginx
    systemctl enable nginx
