# ベースイメージ指定
FROM ruby:2.7.2-alpine

# dockerfile内で使用する変数定義
ARG RUNTIME_PACKAGES="nodejs tzdata mysql-dev mysql-client git"
ARG DEV_PACKAGES="build-base curl-dev"

# Dockerコンテナ内で使える変数定義
ENV HOME="/app" \
    LANG=C.UTF-8 \
    TZ=Asia/Tokyo

# 作業ディレクトリ定義
WORKDIR ${HOME}

# ファイルコピー（ホスト -> コンテナ）
COPY Gemfile* ./

# apk => Alpine Linuxのコマンド
# apk update => パッケージの最新リストを取得
RUN apk update && \
    # apk upgrade => インストールパッケージを最新にする
    apk upgrade && \
    apk add --no-cache ${RUNTIME_PACKAGES} && \
    apk add --virtual build-dependencies --no-cache ${DEV_PACKAGES} && \
    # -j4(jobs=4) = Gemインストールの高速化
    bundle install -j4 && \
    # パッケージを削除(Dockerイメージを軽量化)
    apk del build-dependencies

COPY . ./

CMD ["rails", "server", "-b", "0.0.0.0"]