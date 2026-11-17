
# Stage 1: Build dependencies
FROM ruby:3.3-alpine AS builder

WORKDIR /app

RUN apk add --no-cache build-base postgresql-dev

COPY Gemfile Gemfile.lock ./

RUN bundle config set without 'development test' \
    && bundle install

# Stage 2: Runtime
FROM ruby:3.3-alpine

WORKDIR /app

RUN apk add --no-cache libpq

COPY --from=builder /usr/local/bundle /usr/local/bundle

COPY Gemfile Gemfile.lock ./
COPY app.rb .

RUN addgroup -S appgroup \
    && adduser -S appuser -G appgroup \
    && chown -R appuser:appgroup /app

ENV PATH="/usr/local/bundle/bin:${PATH}"

USER appuser

EXPOSE 8086

CMD ["bundle", "exec", "ruby", "app.rb"]
