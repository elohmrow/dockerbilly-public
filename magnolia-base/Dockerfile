# #
# very simple first try at making magnolia using docker
#   latest {centos, java, tomcat, magnolia} + default H2
# #

# let's build upon centos
FROM centos:latest

# update the centos system
# add latest java
# add latest tomcat
# add wget
# clean up
RUN yum -y update && yum -y install java tomcat wget && yum clean all

# where is tomcat
ENV CATALINA_BASE /var/lib/tomcat

# change to where tomcat is
WORKDIR $CATALINA_BASE/webapps

# fetch latest magnolia into tomcat
ADD get_latest_magnolia.sh /usr/local/bin/get_latest_magnolia.sh
RUN chmod +x /usr/local/bin/get_latest_magnolia.sh
CMD ["/usr/local/bin/get_latest_magnolia.sh"]

# start tomcat with default settings
ENTRYPOINT ["/usr/share/tomcat/bin/catalina.sh", "run"]
CMD [""]

# Expose the ports
EXPOSE 8080
