## <a name="install-wordpress"></a><span data-ttu-id="52399-101">Instalace aplikace WordPress</span><span class="sxs-lookup"><span data-stu-id="52399-101">Install WordPress</span></span>

<span data-ttu-id="52399-102">Pokud chcete tootry do zásobníku, nainstalujte ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="52399-102">If you want tootry your stack, install a sample app.</span></span> <span data-ttu-id="52399-103">Jako příklad hello následující kroky instalace hello s otevřeným zdrojem [WordPress](https://wordpress.org/) platformy toocreate webů a blogů.</span><span class="sxs-lookup"><span data-stu-id="52399-103">As an example, hello following steps install hello open source [WordPress](https://wordpress.org/) platform toocreate websites and blogs.</span></span> <span data-ttu-id="52399-104">Zahrnout další úlohy tootry [Drupal](http://www.drupal.org) a [Moodle](https://moodle.org/).</span><span class="sxs-lookup"><span data-stu-id="52399-104">Other workloads tootry include [Drupal](http://www.drupal.org) and [Moodle](https://moodle.org/).</span></span> 

<span data-ttu-id="52399-105">Tato instalace WordPress je pro testování konceptu.</span><span class="sxs-lookup"><span data-stu-id="52399-105">This WordPress setup is for proof of concept.</span></span> <span data-ttu-id="52399-106">Další informace a nastavení pro provozní instalace najdete v tématu hello [WordPress dokumentaci](https://codex.wordpress.org/Main_Page).</span><span class="sxs-lookup"><span data-stu-id="52399-106">For more information and settings for production installation, see hello [WordPress documentation](https://codex.wordpress.org/Main_Page).</span></span> 



### <a name="install-hello-wordpress-package"></a><span data-ttu-id="52399-107">Instalovat balíček WordPress hello</span><span class="sxs-lookup"><span data-stu-id="52399-107">Install hello WordPress package</span></span>

<span data-ttu-id="52399-108">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="52399-108">Run hello following command:</span></span>

```bash
sudo apt install wordpress
```

### <a name="configure-wordpress"></a><span data-ttu-id="52399-109">Konfigurace WordPress</span><span class="sxs-lookup"><span data-stu-id="52399-109">Configure WordPress</span></span>

<span data-ttu-id="52399-110">Konfigurace WordPress toouse MySQL a PHP.</span><span class="sxs-lookup"><span data-stu-id="52399-110">Configure WordPress toouse MySQL and PHP.</span></span> <span data-ttu-id="52399-111">Spusťte následující příkaz tooopen hello textového editoru podle své volby a vytvoření souboru hello `/etc/wordpress/config-localhost.php`:</span><span class="sxs-lookup"><span data-stu-id="52399-111">Run hello following command tooopen a text editor of your choice and create hello file `/etc/wordpress/config-localhost.php`:</span></span>

```bash
sudo sensible-editor /etc/wordpress/config-localhost.php
```
<span data-ttu-id="52399-112">Kopírování hello následující řádky toohello souboru, nahraďte heslo k databázi pro *yourPassword* (nechte ostatní hodnoty beze změny).</span><span class="sxs-lookup"><span data-stu-id="52399-112">Copy hello following lines toohello file, substituting your database password for *yourPassword* (leave other values unchanged).</span></span> <span data-ttu-id="52399-113">Pak hello soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="52399-113">Then save hello file.</span></span>

```php
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', 'yourPassword');
define('DB_HOST', 'localhost');
define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
?>
```

<span data-ttu-id="52399-114">V pracovním adresáři, vytvořte textový soubor `wordpress.sql` tooconfigure hello WordPress databáze:</span><span class="sxs-lookup"><span data-stu-id="52399-114">In a working directory, create a text file `wordpress.sql` tooconfigure hello WordPress database:</span></span> 

```bash
sudo sensible-editor wordpress.sql
```

<span data-ttu-id="52399-115">Přidejte následující příkazy, nahraďte heslo k databázi pro hello *yourPassword* (nechte ostatní hodnoty beze změny).</span><span class="sxs-lookup"><span data-stu-id="52399-115">Add hello following commands, substituting your database password for *yourPassword* (leave other values unchanged).</span></span> <span data-ttu-id="52399-116">Pak hello soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="52399-116">Then save hello file.</span></span>

```sql
CREATE DATABASE wordpress;
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
ON wordpress.*
toowordpress@localhost
IDENTIFIED BY 'yourPassword';
FLUSH PRIVILEGES;
```


<span data-ttu-id="52399-117">Spusťte následující příkaz toocreate hello databáze hello:</span><span class="sxs-lookup"><span data-stu-id="52399-117">Run hello following command toocreate hello database:</span></span>

```bash
cat wordpress.sql | sudo mysql --defaults-extra-file=/etc/mysql/debian.cnf
```

<span data-ttu-id="52399-118">Po dokončení příkazu hello odstranit soubor hello `wordpress.sql`.</span><span class="sxs-lookup"><span data-stu-id="52399-118">After hello command completes, delete hello file `wordpress.sql`.</span></span>

<span data-ttu-id="52399-119">Přesunete hello WordPress instalace kořen dokumentu toohello webového serveru:</span><span class="sxs-lookup"><span data-stu-id="52399-119">Move hello WordPress installation toohello web server document root:</span></span>

```bash
sudo ln -s /usr/share/wordpress /var/www/html/wordpress

sudo mv /etc/wordpress/config-localhost.php /etc/wordpress/config-default.php
```

<span data-ttu-id="52399-120">Nyní můžete dokončit nastavení hello WordPress a publikovat na platformě hello.</span><span class="sxs-lookup"><span data-stu-id="52399-120">Now you can complete hello WordPress setup and publish on hello platform.</span></span> <span data-ttu-id="52399-121">Otevřete prohlížeč a přejděte příliš`http://yourPublicIPAddress/wordpress`.</span><span class="sxs-lookup"><span data-stu-id="52399-121">Open a browser and go too`http://yourPublicIPAddress/wordpress`.</span></span> <span data-ttu-id="52399-122">Nahraďte hello veřejnou IP adresu vašeho virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="52399-122">Substitute hello public IP address of your VM.</span></span> <span data-ttu-id="52399-123">Měl by vypadat podobně jako toothis bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="52399-123">It should look similar toothis image.</span></span>

![Stránka Instalace WordPress](./media/virtual-machines-linux-tutorial-wordpress/wordpressstartpage.png)