# wordpress環境構築

- Wordpress + Nginx + Mysql の環境です
- dockerを使ったWordpressの環境構築なのでwordockと名付けています



## WorDock

1. リポジトリをクローン

   ```
   git clone https://gitlab.com/welcome-to-sodai/wordpress.git wordpress_app/wordock
   ```

2. wordock階層で

   ```
   cp env-example .env
   ```
   .env内のAPP_NAMEを適当に変更する

3. 同じくwordock階層で

   ```
   docker-compose build workspace nginx mysql
   ```

4. コンテナを構築、起動

   ```
   docker-compose up -d workspace nginx mysql
   ```

5. workspaceのbashに入りwordpressアプリを作成

   `docker-compose exec workspace bash`

   入ったら、以下を全てコピペ

   ```
   cd /var/www && \
   wget https://wordpress.org/latest.tar.gz && \
   tar -xzvf latest.tar.gz -C /var/www/html --strip-components=1 && \
   cd html && \
   cp wp-config-sample.php wp-config.php
   ```

   `wp-config.php`がホストPCのsrc内にあると思うので、DBの設定を以下に変更

   ```
   // ** MySQL settings - You can get this info from your web host ** //
   /** The name of the database for WordPress */
   define( 'DB_NAME', 'default' );
   
   /** MySQL database username */
   define( 'DB_USER', 'default' );
   
   /** MySQL database password */
   define( 'DB_PASSWORD', 'secret' );
   
   /** MySQL hostname */
   define( 'DB_HOST', 'mysql' );
   
   /** Database Charset to use in creating database tables. */
   define( 'DB_CHARSET', 'utf8' );
   
   /** The Database Collate type. Don't change this if in doubt. */
   define( 'DB_COLLATE', '' );
   ```

   SALTやAUTHを変更するため、[ここに](https://api.wordpress.org/secret-key/1.1/salt/)アクセスしたもので置き換える

6. [ここ](http://localhost/wp-admin/install.php)にアクセスしてwordpressの初期設定をする

## 使い方

- 始めるとき wordockの階層で
   ```bash
   docker-compose up -d workspace nginx mysql
   ```
- 終わるとき
   ```bash
   docker-compose down
   ```



## 自作テーマを作りたいとき

- [sage](https://roots.io/sage/)というものを使えば自作テーマを手軽に作れるようです。(フレームワークなのかな)
- 以下さらにsageの環境構築です

### sageの環境構築

1. themes階層でsageをインストール

   your-theme-name部分は自分で名前をつける

   ```
   cd /var/www/html/wp-content/themes && \
   composer create-project roots/sage your-theme-name
   ```

2. sageを立ち上げる

   `yarn`

3. 幾つか質問されるが、全てEnterでOK

## まとめ

- Wordpressの環境構築ができた
- 自作テーマを作るための環境構築ができた

## Next Step

- wordpressを勉強していくのみ!

- [sage](https://roots.io/docs/sage/9.x/installation/#browsersync-configuration)

- 面倒でwordpressの日本語化はやらなかったんですが、できた人いたらpull requestしてもらえると助かります。

  

   