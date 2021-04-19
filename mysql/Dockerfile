FROM centos:centos8

MAINTAINER Vedprakash Nimbalkar <vedprakash.nimbalkar@gmail.com>

# Install basic package
RUN dnf -y update && dnf clean all

RUN dnf -y install mariadb-server mariadb  


COPY docker-entrypoint.sh /

ENTRYPOINT ["/docker-entrypoint.sh"]

# Place VOLUME statement below all changes to /var/lib/mysql
VOLUME /var/lib/mysql

# By default will run as mysql user (27)
USER 27

EXPOSE 3306

CMD ["mysqld_safe"]
