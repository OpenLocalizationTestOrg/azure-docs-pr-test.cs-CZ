## <a name="install-wordpress"></a>Instalace aplikace WordPress

Pokud chcete tootry do zásobníku, nainstalujte ukázkovou aplikaci. Jako příklad hello následující kroky instalace hello s otevřeným zdrojem [WordPress](https://wordpress.org/) platformy toocreate webů a blogů. Zahrnout další úlohy tootry [Drupal](http://www.drupal.org) a [Moodle](https://moodle.org/). 

Tato instalace WordPress je pro testování konceptu. Další informace a nastavení pro provozní instalace najdete v tématu hello [WordPress dokumentaci](https://codex.wordpress.org/Main_Page). 



### <a name="install-hello-wordpress-package"></a>Instalovat balíček WordPress hello

Spusťte následující příkaz hello:

```bash
sudo apt install wordpress
```

### <a name="configure-wordpress"></a>Konfigurace WordPress

Konfigurace WordPress toouse MySQL a PHP. Spusťte následující příkaz tooopen hello textového editoru podle své volby a vytvoření souboru hello `/etc/wordpress/config-localhost.php`:

```bash
sudo sensible-editor /etc/wordpress/config-localhost.php
```
Kopírování hello následující řádky toohello souboru, nahraďte heslo k databázi pro *yourPassword* (nechte ostatní hodnoty beze změny). Pak hello soubor uložte.

```php
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', 'yourPassword');
define('DB_HOST', 'localhost');
define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
?>
```

V pracovním adresáři, vytvořte textový soubor `wordpress.sql` tooconfigure hello WordPress databáze: 

```bash
sudo sensible-editor wordpress.sql
```

Přidejte následující příkazy, nahraďte heslo k databázi pro hello *yourPassword* (nechte ostatní hodnoty beze změny). Pak hello soubor uložte.

```sql
CREATE DATABASE wordpress;
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
ON wordpress.*
toowordpress@localhost
IDENTIFIED BY 'yourPassword';
FLUSH PRIVILEGES;
```


Spusťte následující příkaz toocreate hello databáze hello:

```bash
cat wordpress.sql | sudo mysql --defaults-extra-file=/etc/mysql/debian.cnf
```

Po dokončení příkazu hello odstranit soubor hello `wordpress.sql`.

Přesunete hello WordPress instalace kořen dokumentu toohello webového serveru:

```bash
sudo ln -s /usr/share/wordpress /var/www/html/wordpress

sudo mv /etc/wordpress/config-localhost.php /etc/wordpress/config-default.php
```

Nyní můžete dokončit nastavení hello WordPress a publikovat na platformě hello. Otevřete prohlížeč a přejděte příliš`http://yourPublicIPAddress/wordpress`. Nahraďte hello veřejnou IP adresu vašeho virtuálního počítače. Měl by vypadat podobně jako toothis bitové kopie.

![Stránka Instalace WordPress](./media/virtual-machines-linux-tutorial-wordpress/wordpressstartpage.png)