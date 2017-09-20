# #
# very simple first try at making magnolia using docker
#   latest {centos, java, tomcat, magnolia} + default H2
#   no best practices
#   see README for how to run it
# #

# let's build upon centos
FROM centos:latest

# update the centos system
# add latest java
# add wget
# clean up
RUN yum -y update && yum -y install java wget && yum clean all
# ^ best practice alert: that's a bad idea:
#       imagine this gets run later, and we get a diff version of a library?
#       imagine this gets run later, and we get a diff version of a library, and that breaks the build?

# due to heavy problems with systemd / systemctl inside containers:
# https://github.com/gdraheim/docker-systemctl-replacement
# get tomcat by hand and use that for more control
RUN mkdir -p /tomcat

# where is tomcat
ENV CATALINA_HOME /tomcat
ENV PATH $CATALINA_HOME/bin:$PATH
# ^ critical

# change to where tomcat is
WORKDIR $CATALINA_HOME

# fetch tomcat
ADD get_tomcat.sh $CATALINA_HOME
RUN chmod +x $CATALINA_HOME/get_tomcat.sh
RUN $CATALINA_HOME/get_tomcat.sh
# could combine this and the next step by using bundle
# would have side effect of leveraging magnolia_control.{bat|sh}

# move some things around
RUN mv $CATALINA_HOME/apache-tomcat-8.5.20/* $CATALINA_HOME/
RUN rm -rf $CATALINA_HOME/apache-tomcat-8.5.20
# ^ could be done better

# fetch latest magnolia into tomcat
WORKDIR $CATALINA_HOME/webapps
ADD get_latest_magnolia.sh $CATALINA_HOME/webapps
RUN chmod +x $CATALINA_HOME/webapps/get_latest_magnolia.sh
RUN $CATALINA_HOME/webapps/get_latest_magnolia.sh

# Expose the ports
EXPOSE 8080

CMD ["catalina.sh", "run"]
