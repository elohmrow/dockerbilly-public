# #
# very simple first try at making magnolia using docker
#   latest {centos, java, tomcat, magnolia} + default H2
# #

# let's build upon centos
FROM centos:latest

# update the centos system
# add latest java
# add wget
# clean up
RUN yum -y update && yum -y install java wget && yum clean all

# due to heavy problems with systemd / systemctl inside containers
# get tomcat by hand and use that for more control
RUN mkdir -p /tomcat

# where is tomcat
ENV CATALINA_HOME /tomcat

# change to where tomcat is
WORKDIR $CATALINA_HOME

# fetch tomcat
ADD get_tomcat.sh $CATALINA_HOME
RUN chmod +x $CATALINA_HOME/get_tomcat.sh
CMD ["$CATALINA_HOME/get_tomcat.sh"]

# move some things around
COPY $CATALINA_HOME/apache-tomcat-8.5.20/* $CATALINA_HOME
RUN rm -rf $CATALINA_HOME/apache-tomcat-8.5.20

# fetch latest magnolia into tomcat
WORKDIR $CATALINA_HOME/webapps
ADD get_latest_magnolia.sh $CATALINA_HOME/webapps
RUN chmod +x $CATALINA_HOME/webapps/get_latest_magnolia.sh
CMD ["$CATALINA_HOME/webapps/get_latest_magnolia.sh"]

# start tomcat with default settings
CMD ["$CATALINA_HOME/bin/catalina.sh run"]

# Expose the ports
EXPOSE 8080
