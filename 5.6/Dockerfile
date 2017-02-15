FROM centos/s2i-base-centos7

# This image provides an Apache+PHP environment for running PHP
# applications.

MAINTAINER SoftwareCollections.org <sclorg@redhat.com>

EXPOSE 8080

ENV PHP_VERSION=5.6 \
    PATH=$PATH:/opt/rh/rh-php56/root/usr/bin \
    ENV NGINX_VERSION="1.8" \
    NGINX_BASE_DIR="/opt/rh/rh-nginx18/root" \
    NGINX_VAR_DIR="/var/opt/rh/rh-nginx18" \
    STI_SCRIPTS_PATH="/usr/libexec/s2i"

LABEL io.k8s.description="Platform for building and running PHP 5.6 applications" \
      io.k8s.display-name="Apache 2.4 with PHP 5.6" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,php,php56,rh-php56" \
      io.k8s.description="Platform for running nginx or building nginx-based application" \
      io.k8s.display-name="Nginx 1.8 builder" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,nginx,nginx18" \
      Name="centos/s2i-nginx" \
      Version="1.8" \
      Release="1" \
      Architecture="x86_64"
      
      
      #nginx
      RUN yum install --setopt=tsflags=nodocs -y centos-release-scl-rh && \
    yum install --setopt=tsflags=nodocs -y bcrypt rh-nginx${NGINX_VERSION/\./} && \
    yum clean all -y && \
    mkdir -p /opt/app-root/etc/nginx.conf.d /opt/app-root/run && \
    chmod -R a+rx  $NGINX_VAR_DIR/lib/nginx && \
    chmod -R a+rwX $NGINX_VAR_DIR/lib/nginx/tmp \
                   $NGINX_VAR_DIR/log \
                   $NGINX_VAR_DIR/run \
                   /opt/app-root/run

COPY ./etc/ /opt/app-root/etc
COPY ./.s2i/bin/ ${STI_SCRIPTS_PATH}
RUN cp /opt/app-root/etc/nginx.server.sample.conf /opt/app-root/etc/nginx.conf.d/default.conf && \
    chown -R 1001:1001 /opt/app-root
    
    EXPOSE 8081

# Install Apache httpd and PHP
RUN yum install -y centos-release-scl && \
    INSTALL_PKGS="rh-php56 rh-php56-php rh-php56-php-mysqlnd rh-php56-php-pgsql rh-php56-php-bcmath \
                  rh-php56-php-gd rh-php56-php-intl rh-php56-php-ldap rh-php56-php-mbstring rh-php56-php-pdo \
                  rh-php56-php-pecl-memcache rh-php56-php-process rh-php56-php-soap rh-php56-php-opcache rh-php56-php-xml \
                  rh-php56-php-pecl-xdebug rh-php56-php-gmp" && \
    yum install -y --setopt=tsflags=nodocs --enablerepo=centosplus $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all -y

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH

COPY ./s2i/bin/ $STI_SCRIPTS_PATH



# Each language image can have 'contrib' a directory with extra files needed to
# run and build the applications.
COPY ./contrib/ /opt/app-root

# In order to drop the root user, we have to make some directories world
# writeable as OpenShift default security model is to run the container under
# random UID.
RUN sed -i -f /opt/app-root/etc/httpdconf.sed /opt/rh/httpd24/root/etc/httpd/conf/httpd.conf && \
    echo "IncludeOptional /opt/app-root/etc/conf.d/*.conf" >> /opt/rh/httpd24/root/etc/httpd/conf/httpd.conf && \
    sed -i '/php_value session.save_path/d' /opt/rh/httpd24/root/etc/httpd/conf.d/rh-php56-php.conf && \
    head -n151 /opt/rh/httpd24/root/etc/httpd/conf/httpd.conf | tail -n1 | grep "AllowOverride All" || exit && \
    mkdir /tmp/sessions && \
    chown -R 1001:0 /opt/app-root /tmp/sessions && \
    chmod -R a+rwx /tmp/sessions && \
    chmod -R ug+rwx /opt/app-root && \
    chmod -R a+rwx /etc/opt/rh/rh-php56 && \
    chmod -R a+rwx /opt/rh/httpd24/root/var/run/httpd

USER 1001

# Set the default CMD to print the usage of the language image
CMD $STI_SCRIPTS_PATH/usage
CMD ["usage"]
