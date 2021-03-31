# wordpress環境構築

- Wordpress + Nginx + Mysql の環境です

## WorDock

1. リポジトリをクローン

   ```
   git clone https://github.com/Sodai-circle/Wordpress.git wordpress_app/wordock
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

   同ファイル下部にあるSALTやAUTHを変更するため、[ここに](https://api.wordpress.org/secret-key/1.1/salt/)アクセスしたもので置き換える

6. [ここ](http://localhost/wp-admin/install.php)にアクセスしてwordpressの初期設定をする

## 日本語化

1. [ここ](https://ja.wordpress.org/download/)にアクセスし日本語版をDL

2. DLしたものの中にあるwp-contentのlangagesを元のwp-contentに移動

3. 管理画面のsessings > generalから言語を選択できるようになる

## 自作テーマを作りたいとき

- [sage](https://roots.io/sage/)というものを使えば自作テーマを手軽に作れるようです。(フレームワークの一種かな)
- 以下はsageの環境構築です

### sageの環境構築

1. themes階層でsageをインストール

   your-theme-name部分は自分で名前をつける

   ```
   cd /var/www/html/wp-content/themes && \
   composer create-project roots/sage your-theme-name
   ```

2. 幾つか質問されるが、こんな感じに答える

   ```
   Theme Name [Sage Starter Theme]:
   > 

   Theme URI [https://roots.io/sage/]:
   > 

   Theme Description [Sage is a WordPress starter theme.]:
   > 

   Theme Version [9.0.9]:
   > 

   Theme Author [Roots]:
   > 

   Theme Author URI [https://roots.io/]:
   > 


   Local development URL of WP site [http://example.test]:
   > http://localhost 

   Path to theme directory (e.g., /wp-content/themes/wasecoma-theme) [/app/themes/sage]:
   > /wp-content/themes/wasecoma-theme


   Which framework would you like to load? [Bootstrap]:
   [0] None
   [1] Bootstrap
   [2] Bulma
   [3] Foundation
   [4] Tachyons
   [5] Tailwind
   > 

   Are you sure you want to overwrite the following files?
   - scripts/autoload/_bootstrap.js
   - styles/autoload/_bootstrap.scss
   - styles/common/_variables.scss
   - styles/components/_comments.scss
   - styles/components/_forms.scss
   - styles/components/_wp-classes.scss
   - styles/layouts/_header.scss

   (yes/no) [no]:
   > 

   No actions were taken.
   ```

3. 作成したテーマの階層でコンパイル

   `yarn`

4. 各種コマンド
   
   - assetディレクトリを手動でコンパイルしたい時

      `yarn build`

   - 自動でコンパイルしたい時

      `yarn start`

   - 完成した時

      `yarn build:production`

   - [参照](https://roots.io/docs/sage/9.x/compiling-assets/#available-build-commands)



## 使い方

- 始めるとき wordockの階層で
   ```bash
   docker-compose up -d workspace nginx mysql
   ```
- 終わるとき
   ```bash
   docker-compose down
   ```


  

   
