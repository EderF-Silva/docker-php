FROM php:8.2.6-apache

#Pacotes
RUN apt-get update && apt-get install -qqy git unzip libfreetype6-dev \
    autoconf \
    build-essential \
    apt-utils \
    zlib1g-dev \
    libzip-dev \
    unzip \
    zip \
    vim \
    git \
    libmagick++-dev \
    libmagickwand-dev \
    libpq-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    wget \  
    libjpeg62-turbo-dev \
    libpng-dev \
    libaio1 wget && apt-get clean autoclean && apt-get autoremove --yes &&  rm -rf /var/lib/{apt,dpkg,cache,log}/ 

RUN apt-get -y update \
    && apt-get install -y libicu-dev \ 
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl

RUN docker-php-ext-configure gd \
    --with-jpeg=/usr/include/ \
    --with-freetype=/usr/include/

RUN docker-php-ext-configure zip


# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# # ORACLE oci - Libs para Trabalhar com BD Oracle
# RUN mkdir /opt/oracle \
#     && cd /opt/oracle     

# ADD /oracle_oci/instantclient-basic-linux.x64-12.1.0.2.0.zip /opt/oracle
# ADD /oracle_oci/instantclient-sdk-linux.x64-12.1.0.2.0.zip /opt/oracle

# # Install Oracle Instantclient
# RUN  unzip /opt/oracle/instantclient-basic-linux.x64-12.1.0.2.0.zip -d /opt/oracle \
#     && unzip /opt/oracle/instantclient-sdk-linux.x64-12.1.0.2.0.zip -d /opt/oracle \
#     && ln -s /opt/oracle/instantclient_12_1/libclntsh.so.12.1 /opt/oracle/instantclient_12_1/libclntsh.so \
#     && ln -s /opt/oracle/instantclient_12_1/libclntshcore.so.12.1 /opt/oracle/instantclient_12_1/libclntshcore.so \
#     && ln -s /opt/oracle/instantclient_12_1/libocci.so.12.1 /opt/oracle/instantclient_12_1/libocci.so \
#     && rm -rf /opt/oracle/*.zip

# ENV LD_LIBRARY_PATH  /opt/oracle/instantclient_12_1:${LD_LIBRARY_PATH}

# # Install Oracle extensions
# RUN echo 'instantclient,/opt/oracle/instantclient_12_1/' | pecl install oci8 \ 
#     && docker-php-ext-enable \
#     oci8 \ 
#     && docker-php-ext-configure pdo_oci --with-pdo-oci=instantclient,/opt/oracle/instantclient_12_1,12.1 \
#     && docker-php-ext-install \
#     pdo_oci 

RUN apt-get update && \
    apt-get install -y libxml2-dev

# Banco de Dados
RUN docker-php-ext-install gd exif mysqli pdo pdo_mysql && docker-php-ext-enable pdo_mysql

#SOAP
RUN docker-php-ext-install soap

#Instalação Xdebug
RUN pecl install xdebug && docker-php-ext-enable xdebug


#Configurações do PHP
RUN echo "memory_limit = 128M\n" \
    "upload_max_filesize = 100M\n" \
    "post_max_size = 10M\n" \
    "max_execution_time = 30\n" \
    "max_input_vars=1000\n" \
    "log_errors = On\n" \
    "error_reporting = E_ALL\n" \
    "file_uploads = On\n" \
    "expose_php = Off\n" \
    "session.cookie_domain = dev.localhost.com.br\n" \
    "session.cookie_httponly = On\n" \
    "session.sid_bits_per_character = 6\n" \
    "session.sid_length = 48\n" \
    "session.name = NOME_SESSION\n" \
    "session.cookie_secure = On\n" \
    "session.use_only_cookies = On\n" \
    "session.use_strict_mode = On\n" \
    "session.session.use_trans_sid = Off\n" \
> /usr/local/etc/php/conf.d/uploads.ini


# Limpeza de Pacotes
RUN apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*


# Permissões
RUN chown -R root:www-data /var/www/html
RUN chmod u+rwx,g+rx,o+rx /var/www/html
RUN find /var/www/html -type d -exec chmod u+rwx,g+rx,o+rx {} +
RUN find /var/www/html -type f -exec chmod u+rw,g+rw,o+r {} +

#Workdir
WORKDIR /var/www/html

#SSL e Rewrite
RUN a2enmod rewrite && a2enmod ssl && a2enmod socache_shmcb
RUN sed -i '/SSLCertificateFile.*snakeoil\.pem/c\SSLCertificateFile \/etc\/ssl\/certs\/mycert.crt' /etc/apache2/sites-available/default-ssl.conf && sed -i '/SSLCertificateKeyFile.*snakeoil\.key/cSSLCertificateKeyFile /etc/ssl/private/mycert.key\' /etc/apache2/sites-available/default-ssl.conf
RUN a2ensite default-ssl

RUN echo "ServerSignature Off" >> /etc/apache2/apache2.conf
RUN echo "ServerTokens Prod" >> /etc/apache2/apache2.conf

#Portas
EXPOSE 80
EXPOSE 443