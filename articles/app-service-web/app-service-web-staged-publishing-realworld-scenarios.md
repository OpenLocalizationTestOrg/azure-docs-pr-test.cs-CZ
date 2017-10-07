---
title: "prostředí DevOps aaaUse efektivně pro vaši webovou aplikaci | Microsoft Docs"
description: "Zjistěte, jak nasazení toouse přihrádek tooset nahoru a spravovat více vývojové prostředí pro vaši aplikaci"
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
ms.openlocfilehash: 61a552e735a4ad9769b661d7c988744074ba2962
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-devops-environments-effectively-for-your-web-apps"></a>Efektivně používat DevOps prostředí pro webové aplikace
Tento článek ukazuje, jak tooset a Správa nasazení webové aplikace, pokud několik verzí aplikace jsou v různých prostředích, jako je například vývoj, pracovní, zajištění kvality (QA) a provozní. Každou verzi vaší aplikace lze považovat za vývojového prostředí pro konkrétní účel hello procesu nasazení. Například mohou vývojáři hello QA prostředí tootest hello kvalitu hello aplikací před jejich push tooproduction změny hello.
Více vývojových prostředí může být složité protože potřebujete tootrack kód, spravovat prostředky (výpočetní, webové aplikace, databáze, mezipaměti atd.) a nasazení kódu v rámci prostředí.

## <a name="set-up-a-non-production-environment-stage-dev-qa"></a>Nastavit mimo produkční prostředí (fáze, vývoj, QA)
Po produkční webové aplikace je spuštěná, je dalším krokem hello toocreate mimo produkční prostředí. toouse nasazovací sloty, ujistěte se, že používáte v režimu plán Standard nebo Premium Azure App Service hello. Nasazovací sloty jsou za chodu webových aplikací, které mají své vlastní názvy hostitelů. Webové aplikace obsah a konfiguraci elementy lze vzájemně zaměněny mezi dvěma sloty nasazení, včetně hello produkční slot. Při nasazení vaší aplikace tooa nasazovací slot získáte hello následující výhody:

- Můžete ověřit změny tooa webové aplikace ve přípravný slot nasazení před Prohodit hello aplikace pomocí hello produkční slot.
- Při nasazení slot webové aplikace tooa nejprve a Prohodit do produkčního prostředí, jsou všechny instance hello slotu jestli před prohazují do produkčního prostředí. Tento proces eliminuje výpadek při nasazení webové aplikace. přesměrování provozu Hello je bezproblémové a žádné požadavky jsou vyřadit z důvodu tooswap operace. tooautomate tento celý pracovní postup konfigurace [Prohodit automaticky](web-sites-staged-publishing.md#configure-auto-swap) při není potřeba před swap ověření.
- Po prohození hello slotu, který má hello dříve dvoufázové instalace webové aplikace teď má hello předchozí produkční webové aplikace. Pokud jsou tyto změny hello vzájemně zaměněny do produkčního slotu. hello není podle očekávání, můžete to udělat hello stejné Prohodit okamžitě tooget vaší "poslední známé dobré" back webové aplikace.

tooset až přípravný slot nasazení, najdete v části [nastavení přípravných prostředí pro webové aplikace v Azure App Service](web-sites-staged-publishing.md). Každé prostředí by měla obsahovat vlastní sadu prostředků. Například pokud vaše webová aplikace používá databázi, pak produkční a pracovní webové aplikace by měl použít jiné databáze. Přidáte pracovní prostředky prostředí vývoj například databáze, úložiště nebo mezipaměti tooset pracovní vývojové prostředí.

## <a name="examples-of-using-multiple-development-environments"></a>Příklady používání prostředí s více vývoj
Jakýkoli projekt, postupujte podle správu zdrojového kódu s alespoň dvěma prostředími: vývoj a provozní. Pokud používáte systémy správy obsahu (CMSs), aplikační architektury, atd., nemusí podporovat aplikace hello tento scénář bez úprav. Tato případě platí pro některé hello oblíbených rozhraní, které jsou popsané v následující části hello. Velký počet otázek pocházet toomind při práci s CMS nebo rozhraní, jako například:

- Jak vám přerušení hello obsah do různých prostředí?
- Jaké soubory můžete změnit bez ovlivnění framework verze aktualizace?
- Jak budete spravovat konfigurace za prostředí?
- Jak budete spravovat aktualizace verzí pro moduly, moduly plug-in a hello core framework?

Existuje mnoho způsobů tooset více prostředí pro váš projekt. Hello následující příklady ukazují jednu metodu pro každou příslušné aplikaci.

### <a name="wordpress"></a>WordPress
V této části se dozvíte, jak tooset si pracovní postup nasazení pomocí přihrádek WordPress. WordPress, jako je většina řešení CMS, nepodporuje více vývojových prostředí bez úprav. Hello funkce Web Apps služby Azure App Service má několik funkcí, které umožňují snadno toostore nastavení konfigurace mimo váš kód.

1. Před vytvořením přípravný slot nastavte na vaše aplikace kód toosupport prostředí s více. toosupport prostředí s více WordPress, je nutné tooedit `wp-config.php` na lokální vývoj webové aplikace a přidejte následující kód na začátku hello hello souboru hello. Tento proces vám umožní toopick hello správná konfigurace aplikace na základě vybraných prostředí hello.

    ```
    // Support multiple environments
    // set hello config file based on current environment
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
    // include hello config file if it exists, otherwise WP is going toofail
    require_once $path. $config_file;
    ```

2. Vytvořte složku v kořenovém adresáři webové aplikace volá `config`a přidejte hello `wp-config.azure.php` a `wp-config.local.php` soubory, které představují prostředí Azure a místní prostředí, v uvedeném pořadí.

3. Zkopírujte následující hello `wp-config.local.php`:

    ```
    <?php
    // MySQL settings
    /** hello name of hello database for WordPress */

    define('DB_NAME', 'yourdatabasename');

    /** MySQL database username */
    define('DB_USER', 'yourdbuser');

    /** MySQL database password */
    define('DB_PASSWORD', 'yourpassword');

    /** MySQL hostname */
    define('DB_HOST', 'localhost');
    /**
     * For developers: WordPress debugging mode.
     * * Change this tootrue tooenable hello display of notices during development.
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

    Nastavení hello zabezpečení klíčů, jak je ukázáno v předchozí kód hello může pomoct tooprevent vaší webové aplikace z se hacker, proto použijte jedinečné hodnoty. Pokud potřebujete toogenerate hello řetězec pro zabezpečení klíče uvedená v hello kódu, můžete [automatické generátor přejděte toohello](https://api.wordpress.org/secret-key/1.1/salt) toocreate nové dvojice klíč/hodnota.

4. Kopírování hello následující kód v `wp-config.azure.php`:

    ```    
    <?php
    // MySQL settings
    /** hello name of hello database for WordPress */

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
    * Change this tootrue tooenable hello display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    * Turn on debug logging tooinvestigate issues without displaying tooend user. For WP_DEBUG_LOG to
    * do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
    * with WP_DEBUG_LOG so that errors are not displayed on hello page */

    */
    define('WP_DEBUG', getenv('WP_DEBUG'));
    define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
    define('WP_DEBUG_DISPLAY',false);

    //Security key settings
    /** If you need toogenerate hello string for security keys mentioned above, you can go hello automatic generator toocreate new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
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
Jeden poslední věcí tooconfigure v aplikaci WordPress hello je relativní cesty. WordPress ukládá informace o adrese URL hello databáze. Toto úložiště umožňuje přesunutí obsah z jednoho prostředí tooanother obtížnější. Pokaždé, když přejdete z místní toostage nebo fáze tooproduction prostředí musíte tooupdate hello databáze. riziko hello tooreduce problémy, které může být způsobeno s nasazením databázi pokaždé, když se nasadit z jednoho prostředí tooanother, použijte hello [kořenové relativní odkazy modulu plug-in](https://wordpress.org/plugins/root-relative-urls/), které můžete nainstalovat pomocí Správce WordPress hello řídicí panel.

Přidejte následující položky tooyour hello `wp-config.php` soubor před hello `That's all, stop editing!` komentář:

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

Aktivovat modulu plug-in hello prostřednictvím hello `Plugins` nabídky na řídicím panelu WordPress správce. Uložte nastavení trvalý odkaz pro aplikaci WordPress.

#### <a name="hello-final-wp-configphp-file"></a>Hello konečné `wp-config.php` souboru
Žádné aktualizace základní WordPress nebude mít vliv na vaše `wp-config.php`, `wp-config.azure.php`, a `wp-config.local.php` soubory. Tady je finální verzi hello `wp-config.php` souboru:

```
<?php
/**
 * hello base configurations of hello WordPress.
 *
 * This file has hello following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get hello MySQL settings from your web host.
 *
 * This file is used by hello wp-config.php creation script during the
 * installation. You don't have toouse hello web web app, you can just copy this file
 * too"wp-config.php" and fill in hello values.
 *
 * @package WordPress
 */

// Support multiple environments
// set hello config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
  $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) ||(strpos(getenv('WP_ENV'),'prod' )!== false )){
  $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
  // include hello config file if it exists, otherwise WP is going toofail
  require_once $path. $config_file;
}

/** Database Charset toouse in creating database tables. */
define('DB_CHARSET', 'utf8');

/** hello Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path toohello WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__). '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH. 'wp-settings.php');
```

#### <a name="set-up-a-staging-environment"></a>Nastavit pracovní prostředí
1. Pokud již máte webovou aplikaci WordPress systémem vašeho předplatného Azure, přihlaste se toohello [portál Azure](http://portal.azure.com), a potom přejděte na webovou aplikaci WordPress tooyour. Pokud nemáte webové aplikace WordPress, můžete vytvořit jeden z hello Azure Marketplace. Další, najdete v části toolearn [vytvořit webovou aplikaci WordPress v Azure App Service](web-sites-php-web-site-gallery.md).
Klikněte na tlačítko **nastavení** > **nasazovací sloty** > **přidat** toocreate nasazovací slot s názvem hello *fáze*. Slot nasazení je jiné webové aplikace sdílené složky hello stejné prostředky jako hello primární webovou aplikaci, kterou jste vytvořili dříve.

    ![Vytvoření fáze nasazovací slot.](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

2. Přidat jiné databáze MySQL, například `wordpress-stage-db`, tooyour skupinu prostředků, `wordpressapp-group`.

    ![Přidat skupinu tooresource databáze MySQL](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

3. Aktualizovat hello připojovací řetězce pro fáze nasazení slotu toopoint toohello novou databázi, `wordpress-stage-db`. Webové aplikace produkční `wordpressprodapp`a pracovní webové aplikace, `wordpressprodapp-stage`, musí bodu toodifferent databáze.

#### <a name="configure-environment-specific-app-settings"></a>Konfigurovat nastavení konkrétní prostředí aplikace
Vývojářům můžete uložit dvojice klíč/hodnota řetězce v Azure jako součást informace o konfiguraci hello názvem **nastavení aplikace**, který je spojen s webovou aplikaci. V době běhu webové aplikace automaticky načtení těchto hodnot a aby byly k dispozici toocode, který běží ve vaší webové aplikaci. Z hlediska zabezpečení je to dobrý straně výhody způsobeno citlivé informace, například databázové připojovací řetězce, které zahrnují hesla, nikdy zobrazí jako nešifrovaný text v souboru, jako `wp-config.php`.

Tento proces, který je vysvětlené v následujících odstavcích hello, je užitečné, protože obsahuje soubor změny a změny v databázi pro aplikaci WordPress hello:

* Upgrade verze WordPress
* Přidat nové nebo upravovat nebo upgradovat moduly plug-in
* Přidat nové nebo upravovat nebo upgradovat motivů

Konfigurovat nastavení aplikace pro:

* Informace o databázi
* Zapnutí zapnutí nebo vypnutí protokolování WordPress
* Nastavení zabezpečení WordPress

![Nastavení aplikace pro webové aplikace Wordpress](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

Ujistěte se, abyste přidali hello následující nastavení aplikace pro slot vaše produkční webové aplikace a fázi. Všimněte si, že hello produkční webové aplikace a pracovní webovou aplikaci pomocí různých databází.

1. Vymazat hello **nastavení slotu** zaškrtávací políčko pro všechny parametry nastavení hello kromě WP_ENV. Tento proces bude Prohodit hello konfigurace pro vaši webovou aplikaci, obsah souboru a databáze. Pokud **nastavení slotu** je zaškrtnuto, nastavení aplikací a konfigurace řetězec připojení hello webové aplikace se *není* přesouvat mezi prostředí, když **Prohodit** operaci. Změny databáze, které jsou k dispozici nebudou porušovat produkční webové aplikace.

2. Nasazení pomocí služby WebMatrix nebo nástrojů dle vašeho výběru, jako je například FTP, Git nebo PhpMyAdmin hello místním vývojovém prostředí webové aplikace toohello fáze webovou aplikaci a databázi.

    ![Dialog publikování matice webové pro webové aplikace WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

3. Procházet a testování vaší pracovní webové aplikace. S ohledem na scénář, kde hello motiv hello webové aplikace je toobe aktualizovat zde je hello pracovní webové aplikace.

    ![Procházet pracovní před odkládací sloty webové aplikace](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)

4. Pokud všechny spokojeni, klikněte na tlačítko hello **Prohodit** tlačítko na vaše pracovní aplikace toomove webového obsahu toohello provozním prostředí. V takovém případě Prohodit hello webovou aplikaci a databázi hello prostředích během každé **Swap** operaci.

    ![Swap – zobrazení náhledu změn WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

    > [!NOTE]
    > Pokud váš scénář potřebuje tooonly nabízené souborů (bez aktualizace databáze), zkontrolujte **nastavení slotu** pro všechny hello vztahující se k databázi *nastavení aplikace* a *připojovací řetězce nastavení*v hello **nastavení webové aplikace** okno v rámci hello portál Azure před provedením hello **odkládacího souboru**. V takovém případě %{db_name/, DB_HOST, DB_PASSWORD, DB_USER a výchozí nastavení připojovacího řetězce nesmí zobrazí v zobrazení náhledu změn při **odkládacího souboru**. V tomto okamžiku po dokončení hello **Prohodit** operace, budou mít hello webové aplikace WordPress hello aktualizuje pouze soubory.
    >
    >

    Před tím **odkládacího souboru**, zde je webové aplikace WordPress produkční hello.
    ![Produkční webové aplikaci před odkládací sloty](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

    Po hello **Prohodit** operace, motiv hello se aktualizovala na produkční webové aplikace.

    ![Produkční webové aplikaci po odkládací sloty](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

5. Pokud budete potřebovat tooroll zpět, můžete přejít toohello produkční webové **nastavení aplikace**a klikněte na tlačítko hello **Prohodit** tlačítko tooswap hello webovou aplikaci a databázi z produkčního slotu. toostaging. Pamatujte, že pokud jsou součástí změny v databázi **odkládacího souboru** operace a pak hello příštím nasazení tooyour pracovní webové aplikace, musíte toodeploy hello změny toohello aktuální databáze pro pracovní webovou aplikaci. Hello aktuální databáze může být hello předchozí produkční databázi nebo databázi fáze hello.

#### <a name="summary"></a>Souhrn
Toto je zobecněný proces pro každou aplikaci, která má databáze:

1. Instalace aplikace hello v místním prostředí.
2. Zahrnout konfigurace specifické pro prostředí (místní a Azure Web Apps).
3. Nastavte vaše pracovní a provozní prostředí pro webové aplikace.
4. Pokud máte produkční aplikace už běží v Azure, synchronizujte vaše produkční obsahu (soubory nebo kód a databáze) toolocal a pracovní prostředí.
5. Vyvíjet aplikaci na vašem místním prostředí.
6. Převeďte webové aplikace produkční pod údržby nebo uzamčeném režimu a synchronizace databáze obsahu z provozní prostředí toostaging a vývojářů.
7. Nasaďte toohello pracovní prostředí a testování.
8. Nasazení tooproduction prostředí.
9. Opakujte kroky 4 až 6.

### <a name="umbraco"></a>Umbraco
V této části se dozvíte, jak hello Umbraco CMS využívá toodeploy vlastní modul v prostředí s více DevOps. Tento příklad poskytuje jiný přístup toomanaging více vývojové prostředí.

[Umbraco CMS](http://umbraco.com/) je oblíbených řešení .NET CMS, který je používán celá řada vývojářů. Poskytuje hello [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modulu toodeploy z vývojových toostaging tooproduction prostředí. Místní vývojové prostředí pro webové aplikace Umbraco CMS můžete snadno vytvořit pomocí sady Visual Studio nebo služby WebMatrix.

- [Vytvoření webové aplikace Umbraco pomocí sady Visual Studio](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget)
- [Vytvoření webové aplikace Umbraco pomocí služby WebMatrix](http://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/installation/creating-umbraco-site-from-webmatrix-web-gallery/)

Vždycky mějte na paměti, tooremove hello `install` ve složce aplikace a nikdy nahrajte ho toostage nebo produkční webové aplikace. Tento kurz používá služba WebMatrix.

#### <a name="set-up-a-staging-environment"></a>Nastavit pracovní prostředí
1. Jak už bylo zmíněno dříve pro hello za předpokladu, že již máte webové aplikace Umbraco CMS a spuštění webové aplikace Umbraco CMS, vytvořte nasazovací slot. Pokud ho použít nechcete, můžete vytvořit jeden z hello Marketplace.
2. Aktualizovat hello připojovací řetězec pro vaše fáze nasazení slotu toopoint toohello nové **umbraco fáze db** databáze. Produkční webové aplikace (umbraositecms-1) a pracovní webové aplikace (umbracositecms fáze 1) *musí* bodu toodifferent databáze.

    ![Aktualizovat připojovací řetězec pro přípravu webové aplikace s novou pracovní databázi](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

3. Klikněte na tlačítko **nastavení získat publikování** pro slot nasazení hello **fáze**. Tento proces se stáhnout soubor nastavení publikování, který obsahuje všechny informace hello, Visual Studio nebo WebMatrix vyžaduje toopublish aplikace hello místní vývoj webové aplikace toohello webové aplikace Azure.

    ![Get publikovat nastavení hello pracovní webové aplikace](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)
4. Otevřete místní vývoj webové aplikace ve službě WebMatrix nebo Visual Studio. Tento kurz používá služba WebMatrix. Nejprve je třeba tooimport hello pro webové aplikace pracovního souboru s nastavením publikování.

    ![Import nastavení publikování pro Umbraco pomocí Web Matrix](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

5. Zkontrolujte změny v dialogovém okně hello a nasadit Azure webové aplikace místní webové aplikace tooyour *umbracositecms-1fáze*. Pokud nasazujete soubory přímo tooyour pracovní webová aplikace, vynecháte soubory v hello `~/app_data/TEMP/` složky, protože tyto soubory se vygeneruje, když hello fáze webová aplikace je první spuštění. Musí také vynechejte hello `~/app_data/umbraco.config` souboru, který se také znovu vygeneruje.

    ![Publikovat změny v matici web](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

6. Po úspěšném publikování hello Umbraco místní webové aplikace toohello pracovní webové aplikace, procházet tooyour pracovní webové aplikace a spusťte několik toorule testy na všechny problémy.

#### <a name="set-up-hello-courier2-deployment-module"></a>Nastavit modul nasazení Courier2 hello
S hello [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modul, můžete jednoduše pravým tlačítkem toopush obsahu, šablony stylů a vývoj modulů z pracovní webové aplikace tooa produkční webové aplikace. Tento proces snižuje riziko hello nejnovější webové aplikace produkční, když nasazujete aktualizaci nasadit.
Zakoupit licenci pro Courier2 pro hello `*.azurewebsites.net` domény a vlastní domény (například http://abc.com). Po zakoupení licence hello místní hello stáhnout licenci (. Soubor – licenční smlouva) v hello `bin` složky.

![Vyřaďte licenční soubor ve složce Koš](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

1. [Stáhněte si balíček hello Courier2](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/). Přihlášení tooyour fáze webovou aplikaci, například http://umbracocms-site-stage.azurewebsites.net/umbraco, klikněte na tlačítko hello **vývojáře** nabídce a pak klikněte na tlačítko **balíčky** > **instalace místní balíček**.

    ![Instalační program balíčku Umbraco](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

2. Nahrání balíčku hello Courier2 pomocí instalačního programu hello.

    ![Nahrání balíčku pro modul Kurýrní](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

3. balíček hello tooconfigure, budete potřebovat tooupdate hello courier.config soubor pod hello **konfigurace** složky vaší webové aplikace.

    ```xml
    <!-- Repository connection settings -->
     <!-- For each site, a custom repository must be configured, so Courier knows how tooconnect and authenticate-->
     <repositories>
        <!-- If a custom Umbraco Membership provider is used, specify login & password + set hello passwordEncoding tooclear: -->
        <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
          <url>http://umbracositecms-1.azurewebsites.net</url>
          <user>0</user>
          <!--<login>user@email.com</login> -->
          <!-- <password>user_password</password>-->
          <!-- <passwordEncoding>Clear</passwordEncoding>-->
          </repository>
     </repositories>
     ```

4. V části `<repositories>`, zadejte hello produkční lokality adresy URL a uživatelské informace.
    Pokud používáte hello výchozí zprostředkovatel členství Umbraco, přidejte v hello hello ID pro uživatele správy hello &lt;uživatele&gt; části.
    Pokud používáte vlastní zprostředkovatel členství Umbraco, použijte `<login>`,`<password>` v hello Courier2 modulu tooconnect toohello pracoviště.
    Další podrobnosti [hello v dokumentaci pro modul hello Courier2](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).

5. Podobně nainstalujte modul Courier2 hello na produkční lokality a nakonfigurujte ho toopoint toohello fáze webové aplikace ve svém příslušných courier.config souboru, jak je vidět tady.

    ```xml
     <!-- Repository connection settings -->
     <!-- For each site, a custom repository must be configured, so Courier knows how tooconnect and authenticate-->
     <repositories>
        <!-- If a custom Umbraco Membership provider is used, specify login & password + set hello passwordEncoding tooclear: -->
        <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
          <url>http://umbracositecms-1-stage.azurewebsites.net</url>
          <user>0</user>
          </repository>
     </repositories>
    ```

6. Klikněte na tlačítko hello **Courier2** v hello řídicí panel Umbraco CMS webové aplikace a pak klikněte na tlačítko **umístění**. Název úložiště hello byste měli vidět, jak je uvedeno v `courier.config`. Proveďte tento proces na produkční a pracovní webové aplikace.

    ![Zobrazení cílové webové aplikace úložiště](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

7. toodeploy obsah z hello pracovní lokality toohello pracoviště, přejděte příliš**obsah**a zvolte existující stránky nebo vytvořte novou stránku. Z mé webové aplikace, kde je název hello stránku hello bude vyberte existující stránky **Začínáme – nové**a potom klikněte na **uložit a publikovat**.

    ![Změňte název stránky a publikování](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

8. Klikněte pravým tlačítkem na hello upravit stránku tooview všechny možnosti hello. Klikněte na tlačítko **Kurýrní** tooopen hello **nasazení** dialogové okno. Klikněte na tlačítko **nasadit** tooinitiate nasazení.

    ![Dialogové okno nasazení Kurýrní modulu](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

9. Zkontrolujte hello změny a pak klikněte na tlačítko **pokračovat**.

    ![Kurýrní modul nasazení dialogové okno Kontrola změn](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

    nasazení protokolu Hello se zobrazuje, pokud hello nasazení proběhlo úspěšně.

     ![Zobrazit protokoly nasazení z Kurýrní modulu](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

10. Procházet vaše produkční webové aplikace toosee, pokud se projeví hello.

     ![Procházet produkční webové aplikace](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

Další informace o tom, jak toouse Kurýrní, zkontrolujte hello dokumentaci toolearn.

#### <a name="how-tooupgrade-hello-umbraco-cms-version"></a>Jak tooupgrade hello verze Umbraco CMS
Kurýrní bude není nápovědy, které upgradujete z jedné verze nástroje tooanother Umbraco CMS. Při upgradu verzi Umbraco CMS, musíte vyhledat nekompatibilitu s vlastní moduly nebo moduly od partnerů a hello Umbraco základní knihovny. Tady jsou osvědčené postupy:

* Vždy zálohování webové aplikace a databáze před upgradem. Na webové aplikace v Azure můžete nastavit automatické zálohování pro své weby pomocí funkce zálohování hello a v případě potřeby pomocí funkce obnovení hello obnovit váš web. Další podrobnosti najdete v tématu [jak tooback až vaše webová aplikace](web-sites-backup.md) a [jak toorestore vaší webové aplikace](web-sites-restore.md).
* Zkontrolujte, jestli jsou kompatibilní s verzí hello, kterou provádíte upgrade na balíčky od partnerů. V balíčku hello stránce pro stažení, zkontrolujte hello projektu kompatibilitu s verzí Umbraco CMS.

Další informace o tooupgrade vaši webovou aplikaci místně, [najdete obecné pokyny upgradu hello](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).

Po upgradu místním vývojovém webu, publikujte toohello změny hello pracovní webové aplikace. Testování vaší aplikace. Pokud všechny spokojeni, použijte hello **Prohodit** tlačítko tooswap pracovní lokality toohello produkční webové aplikace. Při použití hello **Prohodit** operace, hello změny, které bude mít vliv na si můžete prohlédnout v konfiguraci webové aplikace. To **Prohodit** operace prohození hello webové aplikace a databáze. Po hello **Prohodit**hello produkční webové aplikace se bod toohello umbraco fáze db databázi a hello pracovní webové aplikace se bod tooumbraco-produkčnímu db databáze.

![Swap – náhled pro nasazení systému CMS Umbraco](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

Zde jsou výhody odkládací hello webové aplikace a databáze hello:

* Můžete se vrátit zpět předchozí verze vaší webové aplikace s jinou toohello **Prohodit** Pokud jsou nějaké problémy aplikace.
* Pro účely upgradu musíte toodeploy soubory a databáze z hello pracovní webové aplikace toohello produkční webové aplikace a databáze. Když nasadíte soubory a databáze, můžete přejít nesprávný celou řadu věcí. Pomocí hello **Prohodit** funkce přihrádky, můžeme zkrátit dobu prostojů při upgradu a snížilo riziko chyby, ke kterým může dojít, když nasazujete změny hello.
* Můžete provést **A / B testování** pomocí hello [testování v produkčním prostředí](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) funkce.

Tento příklad ukazuje hello flexibilitu hello platformy, kde můžete vytvořit vlastní moduly podobné tooUmbraco Kurýrní modulu toomanage nasazení prostředích.

## <a name="references"></a>Odkazy
[Agile software development službou Azure App Service](app-service-agile-software-development.md)

[Nastavení přípravných prostředí pro webové aplikace v Azure App Service](web-sites-staged-publishing.md)

[Jak tooblock webový přístup k produkční toonon nasazovací sloty](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
