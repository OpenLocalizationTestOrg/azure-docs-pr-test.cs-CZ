## <a name="install-wordpress"></a><span data-ttu-id="37d19-101">Instalace aplikace WordPress</span><span class="sxs-lookup"><span data-stu-id="37d19-101">Install WordPress</span></span>

<span data-ttu-id="37d19-102">Pokud budete chtít zkusit do zásobníku, nainstalujte ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="37d19-102">If you want to try your stack, install a sample app.</span></span> <span data-ttu-id="37d19-103">Jako příklad následující kroky instalace open source [WordPress](https://wordpress.org/) platformu pro vytváření webů a blogů.</span><span class="sxs-lookup"><span data-stu-id="37d19-103">As an example, the following steps install the open source [WordPress](https://wordpress.org/) platform to create websites and blogs.</span></span> <span data-ttu-id="37d19-104">Zahrnout další úlohy a zkuste to [Drupal](http://www.drupal.org) a [Moodle](https://moodle.org/).</span><span class="sxs-lookup"><span data-stu-id="37d19-104">Other workloads to try include [Drupal](http://www.drupal.org) and [Moodle](https://moodle.org/).</span></span> 

<span data-ttu-id="37d19-105">Tato instalace WordPress je pro testování konceptu.</span><span class="sxs-lookup"><span data-stu-id="37d19-105">This WordPress setup is for proof of concept.</span></span> <span data-ttu-id="37d19-106">Další informace a nastavení pro provozní instalace najdete v tématu [WordPress dokumentaci](https://codex.wordpress.org/Main_Page).</span><span class="sxs-lookup"><span data-stu-id="37d19-106">For more information and settings for production installation, see the [WordPress documentation](https://codex.wordpress.org/Main_Page).</span></span> 



### <a name="install-the-wordpress-package"></a><span data-ttu-id="37d19-107">Instalovat balíček WordPress</span><span class="sxs-lookup"><span data-stu-id="37d19-107">Install the WordPress package</span></span>

<span data-ttu-id="37d19-108">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="37d19-108">Run the following command:</span></span>

```bash
sudo apt install wordpress
```

### <a name="configure-wordpress"></a><span data-ttu-id="37d19-109">Konfigurace WordPress</span><span class="sxs-lookup"><span data-stu-id="37d19-109">Configure WordPress</span></span>

<span data-ttu-id="37d19-110">Konfigurace WordPress používání MySQL a PHP.</span><span class="sxs-lookup"><span data-stu-id="37d19-110">Configure WordPress to use MySQL and PHP.</span></span> <span data-ttu-id="37d19-111">Spusťte následující příkaz a otevřete textový editor podle svého výběru vytvoření souboru `/etc/wordpress/config-localhost.php`:</span><span class="sxs-lookup"><span data-stu-id="37d19-111">Run the following command to open a text editor of your choice and create the file `/etc/wordpress/config-localhost.php`:</span></span>

```bash
sudo sensible-editor /etc/wordpress/config-localhost.php
```
<span data-ttu-id="37d19-112">Zkopírujte následující řádky do souboru, nahraďte heslo k databázi pro *yourPassword* (nechte ostatní hodnoty beze změny).</span><span class="sxs-lookup"><span data-stu-id="37d19-112">Copy the following lines to the file, substituting your database password for *yourPassword* (leave other values unchanged).</span></span> <span data-ttu-id="37d19-113">Pak soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="37d19-113">Then save the file.</span></span>

```php
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', 'yourPassword');
define('DB_HOST', 'localhost');
define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
?>
```

<span data-ttu-id="37d19-114">V pracovním adresáři, vytvořte textový soubor `wordpress.sql` konfigurace WordPress databáze:</span><span class="sxs-lookup"><span data-stu-id="37d19-114">In a working directory, create a text file `wordpress.sql` to configure the WordPress database:</span></span> 

```bash
sudo sensible-editor wordpress.sql
```

<span data-ttu-id="37d19-115">Přidejte následující příkazy, nahraďte heslo k databázi pro *yourPassword* (nechte ostatní hodnoty beze změny).</span><span class="sxs-lookup"><span data-stu-id="37d19-115">Add the following commands, substituting your database password for *yourPassword* (leave other values unchanged).</span></span> <span data-ttu-id="37d19-116">Pak soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="37d19-116">Then save the file.</span></span>

```sql
CREATE DATABASE wordpress;
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
ON wordpress.*
TO wordpress@localhost
IDENTIFIED BY 'yourPassword';
FLUSH PRIVILEGES;
```


<span data-ttu-id="37d19-117">Spusťte následující příkaz k vytvoření databáze:</span><span class="sxs-lookup"><span data-stu-id="37d19-117">Run the following command to create the database:</span></span>

```bash
cat wordpress.sql | sudo mysql --defaults-extra-file=/etc/mysql/debian.cnf
```

<span data-ttu-id="37d19-118">Po dokončení příkazu, odstraňte soubor `wordpress.sql`.</span><span class="sxs-lookup"><span data-stu-id="37d19-118">After the command completes, delete the file `wordpress.sql`.</span></span>

<span data-ttu-id="37d19-119">Přesunete instalaci WordPress na kořen dokumentu webového serveru:</span><span class="sxs-lookup"><span data-stu-id="37d19-119">Move the WordPress installation to the web server document root:</span></span>

```bash
sudo ln -s /usr/share/wordpress /var/www/html/wordpress

sudo mv /etc/wordpress/config-localhost.php /etc/wordpress/config-default.php
```

<span data-ttu-id="37d19-120">Nyní můžete dokončit nastavení WordPress a publikovat na platformě.</span><span class="sxs-lookup"><span data-stu-id="37d19-120">Now you can complete the WordPress setup and publish on the platform.</span></span> <span data-ttu-id="37d19-121">Otevřete prohlížeč a přejděte na `http://yourPublicIPAddress/wordpress`.</span><span class="sxs-lookup"><span data-stu-id="37d19-121">Open a browser and go to `http://yourPublicIPAddress/wordpress`.</span></span> <span data-ttu-id="37d19-122">Nahraďte veřejnou IP adresu vašeho virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="37d19-122">Substitute the public IP address of your VM.</span></span> <span data-ttu-id="37d19-123">By měla vypadat podobně jako tento obrázek.</span><span class="sxs-lookup"><span data-stu-id="37d19-123">It should look similar to this image.</span></span>

![Stránka Instalace WordPress](./media/virtual-machines-linux-tutorial-wordpress/wordpressstartpage.png)