
FROM php:8.3-cli-alpine

WORKDIR /app

# Install system dependencies and PHP extensions
RUN apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS \
        postgresql-dev \
        curl-dev \
    && docker-php-ext-install -j"$(nproc)" pdo_pgsql curl \
    && apk add --no-cache postgresql-libs libcurl \
    && apk del .build-deps

COPY router.php .

# Run as non-root user
RUN addgroup -S appgroup \
    && adduser -S appuser -G appgroup \
    && chown -R appuser:appgroup /app

USER appuser

EXPOSE 8087

CMD ["php", "-S", "0.0.0.0:8087", "router.php"]
