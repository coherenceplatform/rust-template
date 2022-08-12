FROM rust:buster

RUN set -ex \
    && apt-get update \
    && apt-get install -y curl ca-certificates pkg-config libssl-dev git unzip --no-install-recommends \
    && curl -sL https://deb.nodesource.com/setup_16.x | bash - \
    && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN set -ex \
    && apt-get update \
    && apt-get install -y nodejs yarn --no-install-recommends

RUN rustup target add wasm32-unknown-unknown

#RUN cargo install cargo-generate

RUN cargo install trunk

RUN curl -fsSL https://deno.land/x/install/install.sh | sh

RUN npm cache verify

RUN yarn cache clean --all \
    && rm -rf ${CARGO_HOME}/git/* \
    && rm -rf ${CARGO_HOME}/registry/* \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir /app
WORKDIR /app

ENV PATH="${PATH}:/usr/local/cargo/bin"

COPY . .

RUN ["trunk", "build"]

CMD ["trunk", "serve", "--address", "0.0.0.0", "--port", "8008"]

