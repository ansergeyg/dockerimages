FROM php:8.3-apache

# Install ssh to use git in the container

RUN apt-get update
RUN apt-get -y install openssh-server

# Install git

RUN apt-get install -y git

# Install Composer

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer


# Composer shows warnings that it cannot unzip packages. So we install zip/unzip

RUN apt-get install -y \
    zlib1g-dev \
    libzip-dev \
    unzip
RUN docker-php-ext-install zip

# By default docker image is looking at the /var/www/html folder
# We need to change it to our directory, so next time we log in to container shell
# We will see our working directory from host
# You can replace it in the dockre-compose file with working_dir property

# Document root variable expose 

ENV DOCUMENT_ROOT ${PWD}

# Replace default /var/www/html with DOCUMENT_ROOT variable to
# be able to set it from host

RUN sed -ri -e 's!/var/www/html!${DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf


# We resolve container/host file permission issues by feeding the host user to container

ARG USER_ID
ARG GROUP_ID

RUN addgroup --gid $GROUP_ID user

RUN adduser --disabled-password --gecos '' --uid $USER_ID --gid $GROUP_ID user

USER user

