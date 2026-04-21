# Partimos da imagem oficial do PHP 8.3 com FPM
# FPM = FastCGI Process Manager — é o modo que o PHP usa pra se comunicar com o Nginx
FROM php:8.3-fpm

# Instalamos dependências do sistema operacional
# que as extensões PHP precisam para compilar
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    zip \
    unzip

# Instalamos as extensões PHP que o Laravel precisa
# pdo_mysql     → conectar com MySQL
# mbstring      → manipular strings com caracteres especiais (acentos!)
# exif          → metadados de imagens
# pcntl         → controle de processos (usado em filas)
# bcmath        → cálculos precisos — ESSENCIAL para valores financeiros
# zip           → compactar arquivos
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath zip

# Instalamos o Composer (gerenciador de pacotes do PHP)
# Copiamos direto da imagem oficial do Composer — boa prática
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Criamos um usuário não-root para rodar a aplicação
# Por segurança, nunca rode aplicações como root
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# Definimos o diretório de trabalho dentro do container
WORKDIR /var/www

# Trocamos para o usuário não-root
USER www