---
title: "Efektivně používat DevOps prostředí pro webovou aplikaci | Microsoft Docs"
description: "Další informace o použití nasazovací sloty nastavit a spravovat více vývojové prostředí pro vaši aplikaci"
services: app-service\web
documentationcenter: 
author: sunbuild
manager: yochayk
editor: 
ms.assetid: 16a594dc-61f5-4984-b5ca-9d5abc39fb1e
ms.service: app-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 10/24/2016
ms.author: sumuth
ms.openlocfilehash: 25248411659f6c7b2e386e310428c365c44ea2e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-devops-environments-effectively-for-your-web-apps"></a>Efektivně používat DevOps prostředí pro webové aplikace
Tento článek ukazuje, jak nastavit a spravovat nasazení webové aplikace, pokud několik verzí aplikace jsou v různých prostředích, jako je například vývoj, pracovní, zajištění kvality (QA) a provozní. Každou verzi vaší aplikace lze považovat za vývojového prostředí pro konkrétní účel procesu nasazení. Například mohou vývojáři prostředí QA pro otestování kvality aplikace před jejich odešlete změny do produkčního prostředí.
Více vývojových prostředí může být složité, protože je potřeba sledovat kódu, spravovat prostředky (výpočetní, webové aplikace, databáze, mezipaměti atd.) a nasazení kódu v rámci prostředí.

## <a name="set-up-a-non-production-environment-stage-dev-qa"></a>Nastavit mimo produkční prostředí (fáze, vývoj, QA)
Po produkční webové aplikace je spuštěná, je dalším krokem vytvoření mimo produkční prostředí. Pokud chcete použít nasazovací sloty, ujistěte se, zda jsou spuštěny v režimu plán Standard nebo Premium Azure App Service. Nasazovací sloty jsou za chodu webových aplikací, které mají své vlastní názvy hostitelů. Webové aplikace obsah a konfiguraci elementy lze vzájemně zaměněny mezi dvěma sloty nasazení, včetně produkční slot. Když nasadíte aplikaci pro slot nasazení, získáte následující výhody:

- Změny webové aplikace ve přípravný slot nasazení můžete ověřit před Prohodit aplikaci pomocí produkčního slotu.
- Při nasazení webové aplikace do patice nejprve a Prohodit do produkčního prostředí, jsou všechny instance přihrádky jestli před prohazují do produkčního prostředí. Tento proces eliminuje výpadek při nasazení webové aplikace. Přesměrování provozu je bezproblémové a žádné požadavky jsou vyřazeny z důvodu operace prohození. K automatizaci tento celý pracovní postup, nakonfigurujte [Prohodit automaticky](web-sites-staged-publishing.md#configure-auto-swap) při swap předběžné ověření není potřeba.
- Po prohození přihrádku, která má dříve dvoufázové instalace webové aplikace teď má předchozí produkční webové aplikace. Pokud změny, které jsou vzájemně zaměněny na produkční slot není podle očekávání, můžete provést stejný prohození okamžitě získat webového "poslední známá funkční konfigurace" aplikace zpět.

Přípravný slot nasazení, naleznete v tématu [nastavení přípravných prostředí pro webové aplikace v Azure App Service](web-sites-staged-publishing.md). Každé prostředí by měla obsahovat vlastní sadu prostředků. Například pokud vaše webová aplikace používá databázi, pak produkční a pracovní webové aplikace by měl použít jiné databáze. Přidáte pracovní prostředí prostředky vývoj například databáze, úložiště nebo mezipaměti nastavit pracovní vývojové prostředí.

## <a name="examples-of-using-multiple-development-environments"></a>Příklady používání prostředí s více vývoj
Jakýkoli projekt, postupujte podle správu zdrojového kódu s alespoň dvěma prostředími: vývoj a provozní. Pokud používáte systémy správy obsahu (CMSs), aplikační architektury, atd., nemusí podporovat aplikace v tomto scénáři bez úprav. Tato případě platí pro některé z oblíbených rozhraní, které jsou popsané v následujících částech. Velké množství otázky se do paměti při práci s CMS nebo rozhraní, jako například:

- Jak vám přerušení obsah do různých prostředí?
- Jaké soubory můžete změnit bez ovlivnění framework verze aktualizace?
- Jak budete spravovat konfigurace za prostředí?
- Jak budete spravovat aktualizace verzí pro moduly, moduly plug-in a rozhraní jádra?

Existuje mnoho způsobů, jak nastavit prostředí s více pro váš projekt. Následující příklady ukazují jednu metodu pro každou příslušné aplikaci.

### <a name="wordpress"></a>WordPress
V této části se dozvíte, jak nastavit pracovní postup nasazení pomocí sloty pro WordPress. WordPress, jako je většina řešení CMS, nepodporuje více vývojových prostředí bez úprav. Funkce Web Apps služby Azure App Service má několik funkcí, které usnadňují uložit nastavení konfigurace mimo váš kód.

1. Před vytvořením přípravný slot nastavte na podporu prostředí s více kódu aplikace. Pro podporu prostředí s více v WordPress, budete muset upravit `wp-config.php` na místním vývojovém webu aplikaci a na začátek souboru přidejte následující kód. Tento proces vám umožní aplikaci a vyberte správnou konfiguraci na základě vybraných prostředí.

    ```
    // Support multiple environments
    // set the config file based on current environment
    if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) {
    // local development
     $config_file = 'config/wp-config.local.php';
    }
    elseif ((strpos(getenv('WP_ENV'),'stage') !== false) || (strpos(getenv('WP_ENV'),'prod' )!== false ))
    //single file for all azure development environments
     $config_file = 'config/wp-config.azure.php';
    }
    $path = dirname(__FILE__). '/';
    if (file_exists($path. $config_file)) {
    // include the config file if it exists, otherwise WP is going to fail
    require_once $path. $config_file;
    ```

2. Vytvořte složku v kořenovém adresáři webové aplikace volá `config`a přidejte `wp-config.azure.php` a `wp-config.local.php` soubory, které představují prostředí Azure a místní prostředí, v uvedeném pořadí.

3. Zkopírujte následující `wp-config.local.php`:

    ```
    <?php
    // MySQL settings
    /** The name of the database for WordPress */

    define('DB_NAME', 'yourdatabasename');

    /** MySQL database username */
    define('DB_USER', 'yourdbuser');

    /** MySQL database password */
    define('DB_PASSWORD', 'yourpassword');

    /** MySQL hostname */
    define('DB_HOST', 'localhost');
    /**
     * For developers: WordPress debugging mode.
     * * Change this to true to enable the display of notices during development.
     * It is strongly recommended that plugin and theme developers use WP_DEBUG
     * in their development environments.
     */
    define('WP_DEBUG', true);

    //Security key settings
    define('AUTH_KEY', 'put your unique phrase here');
    define('SECURE_AUTH_KEY','put your unique phrase here');
    define('LOGGED_IN_KEY','put your unique phrase here');
    define('NONCE_KEY', 'put your unique phrase here');
    define('AUTH_SALT', 'put your unique phrase here');
    define('SECURE_AUTH_SALT', 'put your unique phrase here');
    define('LOGGED_IN_SALT', 'put your unique phrase here');
    define('NONCE_SALT', 'put your unique phrase here');

    /**
     * WordPress Database Table prefix.
     *
     * You can have multiple installations in one database if you give each a unique
     * prefix. Only numbers, letters, and underscores please!
     */
    $table_prefix = 'wp_';
    ```

    Nastavení zabezpečení klíčů, jak je ukázáno v předchozí kód může pomoci zabránit se hacker vaší webové aplikace, takže pomocí jedinečné hodnoty. Pokud potřebujete vygenerování řetězce pro zabezpečení zkratky uvedené v kódu, můžete [přejděte na automatické generátor](https://api.wordpress.org/secret-key/1.1/salt) k vytvoření nové páry klíč/hodnota.

4. Zkopírujte následující kód v `wp-config.azure.php`:

    ```    
    <?php
    // MySQL settings
    /** The name of the database for WordPress */

    define('DB_NAME', getenv('DB_NAME'));

    /** MySQL database username */
    define('DB_USER', getenv('DB_USER'));

    /** MySQL database password */
    define('DB_PASSWORD', getenv('DB_PASSWORD'));

    /** MySQL hostname */
    define('DB_HOST', getenv('DB_HOST'));

    /**
    * For developers: WordPress debugging mode.
    *
    * Change this to true to enable the display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    * Turn on debug logging to investigate issues without displaying to end user. For WP_DEBUG_LOG to
    * do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
    * with WP_DEBUG_LOG so that errors are not displayed on the page */

    */
    define('WP_DEBUG', getenv('WP_DEBUG'));
    define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
    define('WP_DEBUG_DISPLAY',false);

    //Security key settings
    /** If you need to generate the string for security keys mentioned above, you can go the automatic generator to create new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
    define('AUTH_KEY',getenv('DB_AUTH_KEY'));
    define('SECURE_AUTH_KEY', getenv('DB_SECURE_AUTH_KEY'));
    define('LOGGED_IN_KEY', getenv('DB_LOGGED_IN_KEY'));
    define('NONCE_KEY', getenv('DB_NONCE_KEY'));
    define('AUTH_SALT', getenv('DB_AUTH_SALT'));
    define('SECURE_AUTH_SALT', getenv('DB_SECURE_AUTH_SALT'));
    define('LOGGED_IN_SALT',  getenv('DB_LOGGED_IN_SALT'));
    define('NONCE_SALT',  getenv('DB_NONCE_SALT'));

    /**
    * WordPress Database Table prefix.
    *
    * You can have multiple installations in one database if you give each a unique
    * prefix. Only numbers, letters, and underscores please!
    */
    $table_prefix = getenv('DB_PREFIX');
    ```

#### <a name="use-relative-paths"></a>Použít relativní cesty
Jednu věc konfigurace v aplikaci WordPress je relativní cesty. WordPress ukládá informace o adresách URL v databázi. Toto úložiště umožňuje přesunutí obsah z jednoho prostředí do druhého obtížnější. Je potřeba aktualizovat databázi pokaždé, když přejdete z místní fáze nebo fáze do produkčního prostředí. Chcete-li snížit riziko problémů, které může být způsobeno s nasazením databázi pokaždé, když nasazujete z jednoho prostředí do druhého, použijte [kořenové relativní odkazy modulu plug-in](https://wordpress.org/plugins/root-relative-urls/), které můžete nainstalovat pomocí řídicího panelu WordPress správce.

Přidejte následující položky do vaší `wp-config.php` soubor před `That's all, stop editing!` komentář:

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

Aktivovat modulu plug-in prostřednictvím `Plugins` nabídky na řídicím panelu WordPress správce. Uložte nastavení trvalý odkaz pro aplikaci WordPress.

#### <a name="the-final-wp-configphp-file"></a>Konečné `wp-config.php` souboru
Žádné aktualizace základní WordPress nebude mít vliv na vaše `wp-config.php`, `wp-config.azure.php`, a `wp-config.local.php` soubory. Tady je finální verzi `wp-config.php` souboru:

```
<?php
/**
 * The base configurations of the WordPress.
 *
 * This file has the following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get the MySQL settings from your web host.
 *
 * This file is used by the wp-config.php creation script during the
 * installation. You don't have to use the web web app, you can just copy this file
 * to "wp-config.php" and fill in the values.
 *
 * @package WordPress
 */

// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
  $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) ||(strpos(getenv('WP_ENV'),'prod' )!== false )){
  $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
  // include the config file if it exists, otherwise WP is going to fail
  require_once $path. $config_file;
}

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__). '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH. 'wp-settings.php');
```

#### <a name="set-up-a-staging-environment"></a>Nastavit pracovní prostředí
1. Pokud již máte webovou aplikaci WordPress systémem vašeho předplatného Azure, přihlaste se k [portál Azure](http://portal.azure.com)a potom přejděte na webové aplikace WordPress. Pokud nemáte webové aplikace WordPress, můžete vytvořit jeden z Azure Marketplace. Další informace najdete v tématu [vytvořit webovou aplikaci WordPress v Azure App Service](web-sites-php-web-site-gallery.md).
Klikněte na tlačítko **nastavení** > **nasazovací sloty** > **přidat** vytvořit nasazovací slot s názvem *fáze* . Slot nasazení je jiné webové aplikace, který sdílí stejné prostředky jako primární webové aplikace, kterou jste vytvořili dříve.

    ![Vytvoření fáze nasazovací slot.](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

2. Přidat jiné databáze MySQL, například `wordpress-stage-db`, do skupiny prostředků, `wordpressapp-group`.

    ![Přidání databáze MySQL do skupiny prostředků](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

3. Aktualizovat připojovací řetězce pro slot nasazení vaší fáze tak, aby odkazoval na databázi nové `wordpress-stage-db`. Webové aplikace produkční `wordpressprodapp`a pracovní webové aplikace, `wordpressprodapp-stage`, musíte přejít do různých databází.

#### <a name="configure-environment-specific-app-settings"></a>Konfigurovat nastavení konkrétní prostředí aplikace
Vývojářům můžete uložit dvojice klíč/hodnota řetězce v Azure jako součást informace o konfiguraci, nazývá **nastavení aplikace**, který je spojen s webovou aplikaci. V době běhu webové aplikace automaticky načtení těchto hodnot a zpřístupnit je kód, který běží ve vaší webové aplikaci. Z hlediska zabezpečení je to dobrý straně výhody způsobeno citlivé informace, například databázové připojovací řetězce, které zahrnují hesla, nikdy zobrazí jako nešifrovaný text v souboru, jako `wp-config.php`.

Tento proces, který je vysvětlené v následujících odstavcích, je užitečné, protože obsahuje soubor změny a změny v databázi pro aplikaci WordPress:

* Upgrade verze WordPress
* Přidat nové nebo upravovat nebo upgradovat moduly plug-in
* Přidat nové nebo upravovat nebo upgradovat motivů

Konfigurovat nastavení aplikace pro:

* Informace o databázi
* Zapnutí zapnutí nebo vypnutí protokolování WordPress
* Nastavení zabezpečení WordPress

![Nastavení aplikace pro webové aplikace Wordpress](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

Ujistěte se, přidat následující nastavení aplikace pro slot vaše produkční webové aplikace a fázi. Upozorňujeme, že webové aplikace produkční a pracovní webové aplikace použít jiné databáze.

1. Vymazat **nastavení slotu** zaškrtávací políčko pro všechny parametry nastavení s výjimkou WP_ENV. Tento proces bude odkládacího souboru konfigurace pro vaši webovou aplikaci, obsah souboru a databáze. Pokud **nastavení slotu** je zaškrtnuto, nastavení aplikací webové aplikace a připojovací řetězec konfigurace *není* přesouvat mezi prostředí, když **odkládacího souboru** operaci. Změny databáze, které jsou k dispozici nebudou porušovat produkční webové aplikace.

2. Nasazení místního vývojového prostředí webové aplikace do fáze webovou aplikaci a databázi pomocí služby WebMatrix nebo nástrojů dle vašeho výběru, jako je například FTP, Git nebo PhpMyAdmin.

    ![Dialog publikování matice webové pro webové aplikace WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

3. Procházet a testování vaší pracovní webové aplikace. S ohledem na scénář, kde motivu webové aplikace se mají aktualizovat zde je pracovní webové aplikace.

    ![Procházet pracovní před odkládací sloty webové aplikace](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)

4. Pokud všechny spokojeni, klikněte na tlačítko **Prohodit** tlačítko na pracovní webové aplikace přesunout obsah do produkčního prostředí. V takovém případě Prohodit webovou aplikaci a databázi prostředích během každé **Swap** operaci.

    ![Swap – zobrazení náhledu změn WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

    > [!NOTE]
    > Pokud váš scénář potřebuje pouze push souborů (bez aktualizace databáze), zkontrolujte **nastavení slotu** pro všechny vztahující se k databázi *nastavení aplikace* a *připojovací řetězce nastavení* v **nastavení webové aplikace** okno v rámci portálu Azure před tím **odkládacího souboru**. V takovém případě %{db_name/, DB_HOST, DB_PASSWORD, DB_USER a výchozí nastavení připojovacího řetězce nesmí zobrazí v zobrazení náhledu změn při **odkládacího souboru**. V tomto okamžiku po dokončení **Prohodit** operace, webové aplikace WordPress budou mít pouze soubory aktualizace.
    >
    >

    Před tím **odkládacího souboru**, zde je webové aplikace WordPress produkční.
    ![Produkční webové aplikaci před odkládací sloty](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

    Po **Prohodit** operace, jaký motiv se aktualizovala na produkční webové aplikace.

    ![Produkční webové aplikaci po odkládací sloty](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

5. Pokud je nutné vrátit zpět, můžete přejít na web produkční **nastavení aplikace**a klikněte na tlačítko **Swap** tlačítko webové aplikace a databáze z výroby přípravný slot prohodit. Pamatujte, že pokud změny databáze jsou součástí **odkládacího souboru** operace, pak při příštím nasazení do webové aplikace pracovní, je třeba nasadit změn databáze do aktuální databáze pro pracovní webovou aplikaci. Aktuální databáze může být předchozí produkční databázi nebo databázi fáze.

#### <a name="summary"></a>Souhrn
Toto je zobecněný proces pro každou aplikaci, která má databáze:

1. Nainstalujte aplikaci na vašem místním prostředí.
2. Zahrnout konfigurace specifické pro prostředí (místní a Azure Web Apps).
3. Nastavte vaše pracovní a provozní prostředí pro webové aplikace.
4. Pokud máte produkční aplikace už běží v Azure, synchronizujte svůj produkční obsah (soubory nebo kód a databáze) do místní a pracovní prostředí.
5. Vyvíjet aplikaci na vašem místním prostředí.
6. Převeďte webové aplikace produkční pod údržby nebo uzamčeném režimu a synchronizace databáze obsahu z produkční do prostředí pracovní a vývojářů.
7. Nasaďte do pracovní prostředí a testování.
8. Nasazení do produkčního prostředí.
9. Opakujte kroky 4 až 6.

### <a name="umbraco"></a>Umbraco
V této části se dozvíte, jak Umbraco CMS používá vlastní modul pro nasazení ve více prostředích DevOps. Tento příklad poskytuje jiný přístup ke správě více vývojové prostředí.

[Umbraco CMS](http://umbraco.com/) je oblíbených řešení .NET CMS, který je používán celá řada vývojářů. Poskytuje [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modulu k nasazení z vývojového do pracovní do produkčního prostředí. Místní vývojové prostředí pro webové aplikace Umbraco CMS můžete snadno vytvořit pomocí sady Visual Studio nebo služby WebMatrix.

- [Vytvoření webové aplikace Umbraco pomocí sady Visual Studio](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget)
- [Vytvoření webové aplikace Umbraco pomocí služby WebMatrix](http://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/installation/creating-umbraco-site-from-webmatrix-web-gallery/)

Vždy nezapomeňte odebrat `install` ve složce aplikace a nikdy ho nahrát do fáze nebo produkční webové aplikace. Tento kurz používá služba WebMatrix.

#### <a name="set-up-a-staging-environment"></a>Nastavit pracovní prostředí
1. Nasazovací slot vytvořte, jak je uvedeno nahoře pro webovou aplikaci Umbraco CMS, za předpokladu, že již máte webové aplikace Umbraco CMS a spuštěna. Pokud ho použít nechcete, můžete vytvořit jeden z Marketplace.
2. Aktualizovat připojovací řetězec pro slot nasazení vaší fáze tak, aby odkazoval na nový **umbraco fáze db** databáze. Produkční webové aplikace (umbraositecms-1) a pracovní webové aplikace (umbracositecms fáze 1) *musí* bodu do různých databází.

    ![Aktualizovat připojovací řetězec pro přípravu webové aplikace s novou pracovní databázi](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

3. Klikněte na tlačítko **nastavení získat publikování** slotu pro nasazení **fáze**. Tento proces se stáhnout soubor nastavení publikování, který obsahuje všechny informace, který Visual Studio nebo WebMatrix vyžaduje, aby publikovat svoji aplikaci z místní vývoj webové aplikace do webové aplikace Azure.

    ![Získat publikovat nastavení pracovní webové aplikace](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)
4. Otevřete místní vývoj webové aplikace ve službě WebMatrix nebo Visual Studio. Tento kurz používá služba WebMatrix. Nejprve je třeba importovat soubor nastavení publikování pro pracovní webovou aplikaci.

    ![Import nastavení publikování pro Umbraco pomocí Web Matrix](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

5. Zkontrolujte změny v dialogovém okně a nasadíte místní webové aplikace do vaší webové aplikace Azure, *umbracositecms-1fáze*. Pokud nasazujete soubory přímo do pracovní webové aplikace, vynecháte soubory v `~/app_data/TEMP/` složky, protože tyto soubory se vygeneruje, pokud je první fázi webové aplikace spuštěna. Musí také vynechejte `~/app_data/umbraco.config` souboru, který se také znovu vygeneruje.

    ![Publikovat změny v matici web](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

6. Po úspěšně publikovat místní webové aplikace Umbraco do pracovní webové aplikace, přejděte do pracovní webové aplikace a spustit několik testů vylučte všechny problémy.

#### <a name="set-up-the-courier2-deployment-module"></a>Nastavit modul Courier2 nasazení
Pomocí [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modul, můžete můžete jednoduše klikněte pravým tlačítkem na předávaný obsah, šablony stylů a vývoj modulů z pracovní webové aplikace do produkčního webové aplikace. Tento proces snižuje riziko nejnovější webové aplikace produkční, když nasazujete aktualizaci nasadit.
Zakoupit licenci pro Courier2 pro `*.azurewebsites.net` domény a vlastní domény (například http://abc.com). Po zakoupení licence umístit stažený licence (. Soubor – licenční smlouva) v `bin` složky.

![Vyřaďte licenční soubor ve složce Koš](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

1. [Stáhněte si balíček Courier2](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/). Přihlášení do webové aplikace fázi, například http://umbracocms-site-stage.azurewebsites.net/umbraco, klikněte na tlačítko **vývojáře** nabídce a pak klikněte na tlačítko **balíčky** > **místní instalace balíček**.

    ![Instalační program balíčku Umbraco](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

2. Nahrání balíčku Courier2 pomocí Instalační služby.

    ![Nahrání balíčku pro modul Kurýrní](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

3. Ke konfiguraci balíčku, je potřeba aktualizovat soubor do courier.config **konfigurace** složky vaší webové aplikace.

    ```xml
    <!-- Repository connection settings -->
     <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
     <repositories>
        <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
        <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
          <url>http://umbracositecms-1.azurewebsites.net</url>
          <user>0</user>
          <!--<login>user@email.com</login> -->
          <!-- <password>user_password</password>-->
          <!-- <passwordEncoding>Clear</passwordEncoding>-->
          </repository>
     </repositories>
     ```

4. V části `<repositories>`, zadejte informace o provozním serveru adresu URL a uživatele.
    Pokud používáte výchozí zprostředkovatel členství Umbraco, přidejte ID pro uživatele správy v &lt;uživatele&gt; části.
    Pokud používáte vlastní zprostředkovatel členství Umbraco, použijte `<login>`,`<password>` v modulu Courier2 pro připojení k produkční lokality.
    Další podrobnosti [najdete v dokumentaci pro modul Courier2](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).

5. Podobně nainstalovat modul Courier2 provozního webu a nakonfigurovat ji tak, aby odkazoval na fázi webovou aplikaci ve svém příslušných courier.config souboru, jak je vidět tady.

    ```xml
     <!-- Repository connection settings -->
     <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
     <repositories>
        <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
        <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
          <url>http://umbracositecms-1-stage.azurewebsites.net</url>
          <user>0</user>
          </repository>
     </repositories>
    ```

6. Klikněte **Courier2** v řídicím panelu Umbraco CMS webové aplikace a pak klikněte na tlačítko **umístění**. Název úložiště byste měli vidět, jak je uvedeno v `courier.config`. Proveďte tento proces na produkční a pracovní webové aplikace.

    ![Zobrazení cílové webové aplikace úložiště](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

7. Pokud chcete nasadit obsah z webu pracovní na pracoviště, přejděte na **obsah**a zvolte existující stránky nebo vytvořte novou stránku. Z mé webové aplikace, kde je název příslušné stránky se vyberte existující stránky **Začínáme – nové**a potom klikněte na **uložit a publikovat**.

    ![Změňte název stránky a publikování](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

8. Klikněte pravým tlačítkem na upravené stránky zobrazíte všechny možnosti. Klikněte na tlačítko **Kurýrní** otevřete **nasazení** dialogové okno. Klikněte na tlačítko **nasadit** k inicializaci nasazení.

    ![Dialogové okno nasazení Kurýrní modulu](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

9. Zkontrolujte změny a pak klikněte na tlačítko **pokračovat**.

    ![Kurýrní modul nasazení dialogové okno Kontrola změn](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

    Nasazení protokolu se zobrazuje, pokud nasazení bylo úspěšné.

     ![Zobrazit protokoly nasazení z Kurýrní modulu](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

10. Procházejte webové aplikace produkční chcete zobrazit, pokud změny se projeví.

     ![Procházet produkční webové aplikace](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

Další informace o tom, jak používat Kurýrní, zkontrolujte v dokumentaci.

#### <a name="how-to-upgrade-the-umbraco-cms-version"></a>Postup upgradu na verzi Umbraco CMS
Kurýrní bude není nápovědy, které upgradujete z jedné verze nástroje Umbraco CMS do jiného. Při upgradu verzi Umbraco CMS, je nutné zkontrolovat pro nekompatibilitu s vlastní moduly nebo moduly od partnerů a Umbraco základní knihovny. Tady jsou osvědčené postupy:

* Vždy zálohování webové aplikace a databáze před upgradem. Na webové aplikace v Azure můžete nastavit automatické zálohování pro své weby pomocí funkce zálohování a obnovení vaší lokality v případě potřeby pomocí funkce obnovení. Další podrobnosti najdete v tématu [zálohování webové aplikace](web-sites-backup.md) a [obnovení webové aplikace](web-sites-restore.md).
* Zkontrolujte, jestli jsou kompatibilní s verzí, které provádíte upgrade na balíčky od partnerů. V balíčku stránce pro stažení, zkontrolujte kompatibilitu projektu s Umbraco CMS verzí.

Další informace o tom, jak upgradovat webovou aplikaci místně [najdete obecné pokyny upgradu](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).

Po upgradu místním vývojovém webu, publikujte změny do pracovní webové aplikace. Testování vaší aplikace. Pokud všechny spokojeni, použijte **Swap** tlačítko Prohodit váš pracovní web na produkční webové aplikaci. Při použití **Prohodit** operace, můžete zobrazit změny, které bude mít vliv v konfiguraci webové aplikace. To **Prohodit** operace prohození webové aplikace a databáze. Po **odkládacího souboru**, produkční webové aplikace bude odkazovat na databázi umbraco fáze db a pracovní webové aplikace bude přejděte do databáze umbraco produkčnímu db.

![Swap – náhled pro nasazení systému CMS Umbraco](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

Zde jsou výhody prohození webové aplikace a databáze:

* Můžete se vrátit zpět na předchozí verzi vaší webové aplikace s jinou **Prohodit** Pokud jsou nějaké problémy aplikace.
* Pro účely upgradu musíte nasadit soubory a databází z pracovní webové aplikace na produkční webové aplikaci a databázi. Když nasadíte soubory a databáze, můžete přejít nesprávný celou řadu věcí. Pomocí **Prohodit** funkce přihrádky, můžeme zkrátit dobu prostojů při upgradu a snížilo riziko chyby, ke kterým může dojít, když nasazujete změny.
* Můžete provést **A / B testování** pomocí [testování v produkčním prostředí](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) funkce.

Tento příklad ukazuje, flexibilní platformu kde můžete vytvořit vlastní moduly podobná Umbraco Kurýrní modul pro správu nasazení v prostředí.

## <a name="references"></a>Odkazy
[Agile software development službou Azure App Service](app-service-agile-software-development.md)

[Nastavení přípravných prostředí pro webové aplikace v Azure App Service](web-sites-staged-publishing.md)

[Blokování webového přístupu k testovacím nasazovací sloty](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
