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
# <a name="use-devops-environments-effectively-for-your-web-apps"></a><span data-ttu-id="84838-103">Efektivně používat DevOps prostředí pro webové aplikace</span><span class="sxs-lookup"><span data-stu-id="84838-103">Use DevOps environments effectively for your web apps</span></span>
<span data-ttu-id="84838-104">Tento článek ukazuje, jak tooset a Správa nasazení webové aplikace, pokud několik verzí aplikace jsou v různých prostředích, jako je například vývoj, pracovní, zajištění kvality (QA) a provozní.</span><span class="sxs-lookup"><span data-stu-id="84838-104">This article shows you how tooset up and manage web application deployments when multiple versions of your application are in various environments, such as development, staging, quality assurance (QA), and production.</span></span> <span data-ttu-id="84838-105">Každou verzi vaší aplikace lze považovat za vývojového prostředí pro konkrétní účel hello procesu nasazení.</span><span class="sxs-lookup"><span data-stu-id="84838-105">Each version of your application can be considered as a development environment for hello specific purpose of your deployment process.</span></span> <span data-ttu-id="84838-106">Například mohou vývojáři hello QA prostředí tootest hello kvalitu hello aplikací před jejich push tooproduction změny hello.</span><span class="sxs-lookup"><span data-stu-id="84838-106">For example, developers can use hello QA environment tootest hello quality of hello application before they push hello changes tooproduction.</span></span>
<span data-ttu-id="84838-107">Více vývojových prostředí může být složité protože potřebujete tootrack kód, spravovat prostředky (výpočetní, webové aplikace, databáze, mezipaměti atd.) a nasazení kódu v rámci prostředí.</span><span class="sxs-lookup"><span data-stu-id="84838-107">Multiple development environments can be a challenge because you need tootrack code, manage resources (compute, web app, database, cache, etc.), and deploy code across environments.</span></span>

## <a name="set-up-a-non-production-environment-stage-dev-qa"></a><span data-ttu-id="84838-108">Nastavit mimo produkční prostředí (fáze, vývoj, QA)</span><span class="sxs-lookup"><span data-stu-id="84838-108">Set up a non-production environment (stage, dev, QA)</span></span>
<span data-ttu-id="84838-109">Po produkční webové aplikace je spuštěná, je dalším krokem hello toocreate mimo produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="84838-109">After a production web app is up and running, hello next step is toocreate a non-production environment.</span></span> <span data-ttu-id="84838-110">toouse nasazovací sloty, ujistěte se, že používáte v režimu plán Standard nebo Premium Azure App Service hello.</span><span class="sxs-lookup"><span data-stu-id="84838-110">toouse deployment slots, make sure that you are running in hello Standard or Premium Azure App Service plan mode.</span></span> <span data-ttu-id="84838-111">Nasazovací sloty jsou za chodu webových aplikací, které mají své vlastní názvy hostitelů.</span><span class="sxs-lookup"><span data-stu-id="84838-111">Deployment slots are live web apps that have their own host names.</span></span> <span data-ttu-id="84838-112">Webové aplikace obsah a konfiguraci elementy lze vzájemně zaměněny mezi dvěma sloty nasazení, včetně hello produkční slot.</span><span class="sxs-lookup"><span data-stu-id="84838-112">Web app content and configuration elements can be swapped between two deployment slots, including hello production slot.</span></span> <span data-ttu-id="84838-113">Při nasazení vaší aplikace tooa nasazovací slot získáte hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="84838-113">When you deploy your application tooa deployment slot, you get hello following benefits:</span></span>

- <span data-ttu-id="84838-114">Můžete ověřit změny tooa webové aplikace ve přípravný slot nasazení před Prohodit hello aplikace pomocí hello produkční slot.</span><span class="sxs-lookup"><span data-stu-id="84838-114">You can validate changes tooa web app in a staging deployment slot before you swap hello app with hello production slot.</span></span>
- <span data-ttu-id="84838-115">Při nasazení slot webové aplikace tooa nejprve a Prohodit do produkčního prostředí, jsou všechny instance hello slotu jestli před prohazují do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="84838-115">When you deploy a web app tooa slot first and swap it into production, all instances of hello slot are warmed up before being swapped into production.</span></span> <span data-ttu-id="84838-116">Tento proces eliminuje výpadek při nasazení webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="84838-116">This process eliminates downtime when you deploy your web app.</span></span> <span data-ttu-id="84838-117">přesměrování provozu Hello je bezproblémové a žádné požadavky jsou vyřadit z důvodu tooswap operace.</span><span class="sxs-lookup"><span data-stu-id="84838-117">hello traffic redirection is seamless, and no requests are dropped due tooswap operations.</span></span> <span data-ttu-id="84838-118">tooautomate tento celý pracovní postup konfigurace [Prohodit automaticky](web-sites-staged-publishing.md#configure-auto-swap) při není potřeba před swap ověření.</span><span class="sxs-lookup"><span data-stu-id="84838-118">tooautomate this entire workflow, configure [Auto Swap](web-sites-staged-publishing.md#configure-auto-swap) when pre-swap validation is not needed.</span></span>
- <span data-ttu-id="84838-119">Po prohození hello slotu, který má hello dříve dvoufázové instalace webové aplikace teď má hello předchozí produkční webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="84838-119">After a swap, hello slot that has hello previously staged web app now has hello previous production web app.</span></span> <span data-ttu-id="84838-120">Pokud jsou tyto změny hello vzájemně zaměněny do produkčního slotu. hello není podle očekávání, můžete to udělat hello stejné Prohodit okamžitě tooget vaší "poslední známé dobré" back webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="84838-120">If hello changes swapped into hello production slot are not as you expected, you can perform hello same swap immediately tooget your "last known good" web app back.</span></span>

<span data-ttu-id="84838-121">tooset až přípravný slot nasazení, najdete v části [nastavení přípravných prostředí pro webové aplikace v Azure App Service](web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="84838-121">tooset up a staging deployment slot, see [Set up staging environments for web apps in Azure App Service](web-sites-staged-publishing.md).</span></span> <span data-ttu-id="84838-122">Každé prostředí by měla obsahovat vlastní sadu prostředků.</span><span class="sxs-lookup"><span data-stu-id="84838-122">Every environment should include its own set of resources.</span></span> <span data-ttu-id="84838-123">Například pokud vaše webová aplikace používá databázi, pak produkční a pracovní webové aplikace by měl použít jiné databáze.</span><span class="sxs-lookup"><span data-stu-id="84838-123">For example, if your web app uses a database, then both production and staging web apps should use different databases.</span></span> <span data-ttu-id="84838-124">Přidáte pracovní prostředky prostředí vývoj například databáze, úložiště nebo mezipaměti tooset pracovní vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="84838-124">Add staging development environment resources such as database, storage, or cache tooset your staging development environment.</span></span>

## <a name="examples-of-using-multiple-development-environments"></a><span data-ttu-id="84838-125">Příklady používání prostředí s více vývoj</span><span class="sxs-lookup"><span data-stu-id="84838-125">Examples of using multiple development environments</span></span>
<span data-ttu-id="84838-126">Jakýkoli projekt, postupujte podle správu zdrojového kódu s alespoň dvěma prostředími: vývoj a provozní.</span><span class="sxs-lookup"><span data-stu-id="84838-126">Any project should follow source code management with at least two environments: development and production.</span></span> <span data-ttu-id="84838-127">Pokud používáte systémy správy obsahu (CMSs), aplikační architektury, atd., nemusí podporovat aplikace hello tento scénář bez úprav.</span><span class="sxs-lookup"><span data-stu-id="84838-127">If you use content management systems (CMSs), application frameworks, etc., hello application might not support this scenario without customization.</span></span> <span data-ttu-id="84838-128">Tato případě platí pro některé hello oblíbených rozhraní, které jsou popsané v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="84838-128">This eventuality is true for some of hello popular frameworks that are discussed in hello following sections.</span></span> <span data-ttu-id="84838-129">Velký počet otázek pocházet toomind při práci s CMS nebo rozhraní, jako například:</span><span class="sxs-lookup"><span data-stu-id="84838-129">Lots of questions come toomind when you work with CMS/frameworks, such as:</span></span>

- <span data-ttu-id="84838-130">Jak vám přerušení hello obsah do různých prostředí?</span><span class="sxs-lookup"><span data-stu-id="84838-130">How do you break hello content out into different environments?</span></span>
- <span data-ttu-id="84838-131">Jaké soubory můžete změnit bez ovlivnění framework verze aktualizace?</span><span class="sxs-lookup"><span data-stu-id="84838-131">What files can you change without affecting framework version updates?</span></span>
- <span data-ttu-id="84838-132">Jak budete spravovat konfigurace za prostředí?</span><span class="sxs-lookup"><span data-stu-id="84838-132">How do you manage configurations per environment?</span></span>
- <span data-ttu-id="84838-133">Jak budete spravovat aktualizace verzí pro moduly, moduly plug-in a hello core framework?</span><span class="sxs-lookup"><span data-stu-id="84838-133">How do you manage version updates for modules, plugins, and hello core framework?</span></span>

<span data-ttu-id="84838-134">Existuje mnoho způsobů tooset více prostředí pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="84838-134">There are many ways tooset up multiple environments for your project.</span></span> <span data-ttu-id="84838-135">Hello následující příklady ukazují jednu metodu pro každou příslušné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="84838-135">hello following examples show one method for each respective application.</span></span>

### <a name="wordpress"></a><span data-ttu-id="84838-136">WordPress</span><span class="sxs-lookup"><span data-stu-id="84838-136">WordPress</span></span>
<span data-ttu-id="84838-137">V této části se dozvíte, jak tooset si pracovní postup nasazení pomocí přihrádek WordPress.</span><span class="sxs-lookup"><span data-stu-id="84838-137">In this section, you will learn how tooset up a deployment workflow by using slots for WordPress.</span></span> <span data-ttu-id="84838-138">WordPress, jako je většina řešení CMS, nepodporuje více vývojových prostředí bez úprav.</span><span class="sxs-lookup"><span data-stu-id="84838-138">WordPress, like most CMS solutions, does not support multiple development environments without customization.</span></span> <span data-ttu-id="84838-139">Hello funkce Web Apps služby Azure App Service má několik funkcí, které umožňují snadno toostore nastavení konfigurace mimo váš kód.</span><span class="sxs-lookup"><span data-stu-id="84838-139">hello Web Apps feature of Azure App Service has a few features that make it easy toostore configuration settings outside your code.</span></span>

1. <span data-ttu-id="84838-140">Před vytvořením přípravný slot nastavte na vaše aplikace kód toosupport prostředí s více.</span><span class="sxs-lookup"><span data-stu-id="84838-140">Before you create a staging slot, set up your application code toosupport multiple environments.</span></span> <span data-ttu-id="84838-141">toosupport prostředí s více WordPress, je nutné tooedit `wp-config.php` na lokální vývoj webové aplikace a přidejte následující kód na začátku hello hello souboru hello.</span><span class="sxs-lookup"><span data-stu-id="84838-141">toosupport multiple environments in WordPress, you need tooedit `wp-config.php` on your local development web app and add hello following code at hello beginning of hello file.</span></span> <span data-ttu-id="84838-142">Tento proces vám umožní toopick hello správná konfigurace aplikace na základě vybraných prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="84838-142">This process will enable your application toopick hello correct configuration based on hello selected environment.</span></span>

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

2. <span data-ttu-id="84838-143">Vytvořte složku v kořenovém adresáři webové aplikace volá `config`a přidejte hello `wp-config.azure.php` a `wp-config.local.php` soubory, které představují prostředí Azure a místní prostředí, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="84838-143">Create a folder under web app root called `config`, and add hello `wp-config.azure.php` and `wp-config.local.php` files, which represent your Azure environment and local environment respectively.</span></span>

3. <span data-ttu-id="84838-144">Zkopírujte následující hello `wp-config.local.php`:</span><span class="sxs-lookup"><span data-stu-id="84838-144">Copy hello following in `wp-config.local.php`:</span></span>

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

    <span data-ttu-id="84838-145">Nastavení hello zabezpečení klíčů, jak je ukázáno v předchozí kód hello může pomoct tooprevent vaší webové aplikace z se hacker, proto použijte jedinečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="84838-145">Setting hello security keys as illustrated in hello previous code can help tooprevent your web app from being hacked, so use unique values.</span></span> <span data-ttu-id="84838-146">Pokud potřebujete toogenerate hello řetězec pro zabezpečení klíče uvedená v hello kódu, můžete [automatické generátor přejděte toohello](https://api.wordpress.org/secret-key/1.1/salt) toocreate nové dvojice klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="84838-146">If you need toogenerate hello string for security keys mentioned in hello code, you can [go toohello automatic generator](https://api.wordpress.org/secret-key/1.1/salt) toocreate new key/value pairs.</span></span>

4. <span data-ttu-id="84838-147">Kopírování hello následující kód v `wp-config.azure.php`:</span><span class="sxs-lookup"><span data-stu-id="84838-147">Copy hello following code in `wp-config.azure.php`:</span></span>

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

#### <a name="use-relative-paths"></a><span data-ttu-id="84838-148">Použít relativní cesty</span><span class="sxs-lookup"><span data-stu-id="84838-148">Use relative paths</span></span>
<span data-ttu-id="84838-149">Jeden poslední věcí tooconfigure v aplikaci WordPress hello je relativní cesty.</span><span class="sxs-lookup"><span data-stu-id="84838-149">One last thing tooconfigure in hello WordPress app is relative paths.</span></span> <span data-ttu-id="84838-150">WordPress ukládá informace o adrese URL hello databáze.</span><span class="sxs-lookup"><span data-stu-id="84838-150">WordPress stores URL information in hello database.</span></span> <span data-ttu-id="84838-151">Toto úložiště umožňuje přesunutí obsah z jednoho prostředí tooanother obtížnější.</span><span class="sxs-lookup"><span data-stu-id="84838-151">This storage makes moving content from one environment tooanother more difficult.</span></span> <span data-ttu-id="84838-152">Pokaždé, když přejdete z místní toostage nebo fáze tooproduction prostředí musíte tooupdate hello databáze.</span><span class="sxs-lookup"><span data-stu-id="84838-152">You need tooupdate hello database every time you move from local toostage or stage tooproduction environments.</span></span> <span data-ttu-id="84838-153">riziko hello tooreduce problémy, které může být způsobeno s nasazením databázi pokaždé, když se nasadit z jednoho prostředí tooanother, použijte hello [kořenové relativní odkazy modulu plug-in](https://wordpress.org/plugins/root-relative-urls/), které můžete nainstalovat pomocí Správce WordPress hello řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="84838-153">tooreduce hello risk of issues that can be caused with deploying a database every time you deploy from one environment tooanother, use hello [Relative Root links plugin](https://wordpress.org/plugins/root-relative-urls/), which you can install by using hello WordPress administrator dashboard.</span></span>

<span data-ttu-id="84838-154">Přidejte následující položky tooyour hello `wp-config.php` soubor před hello `That's all, stop editing!` komentář:</span><span class="sxs-lookup"><span data-stu-id="84838-154">Add hello following entries tooyour `wp-config.php` file before hello `That's all, stop editing!` comment:</span></span>

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

<span data-ttu-id="84838-155">Aktivovat modulu plug-in hello prostřednictvím hello `Plugins` nabídky na řídicím panelu WordPress správce.</span><span class="sxs-lookup"><span data-stu-id="84838-155">Activate hello plugin through hello `Plugins` menu in WordPress administrator dashboard.</span></span> <span data-ttu-id="84838-156">Uložte nastavení trvalý odkaz pro aplikaci WordPress.</span><span class="sxs-lookup"><span data-stu-id="84838-156">Save your permalink settings for WordPress app.</span></span>

#### <a name="hello-final-wp-configphp-file"></a><span data-ttu-id="84838-157">Hello konečné `wp-config.php` souboru</span><span class="sxs-lookup"><span data-stu-id="84838-157">hello final `wp-config.php` file</span></span>
<span data-ttu-id="84838-158">Žádné aktualizace základní WordPress nebude mít vliv na vaše `wp-config.php`, `wp-config.azure.php`, a `wp-config.local.php` soubory.</span><span class="sxs-lookup"><span data-stu-id="84838-158">Any WordPress core updates will not affect your `wp-config.php`, `wp-config.azure.php`, and `wp-config.local.php` files.</span></span> <span data-ttu-id="84838-159">Tady je finální verzi hello `wp-config.php` souboru:</span><span class="sxs-lookup"><span data-stu-id="84838-159">Here's a final version of hello `wp-config.php` file:</span></span>

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

#### <a name="set-up-a-staging-environment"></a><span data-ttu-id="84838-160">Nastavit pracovní prostředí</span><span class="sxs-lookup"><span data-stu-id="84838-160">Set up a staging environment</span></span>
1. <span data-ttu-id="84838-161">Pokud již máte webovou aplikaci WordPress systémem vašeho předplatného Azure, přihlaste se toohello [portál Azure](http://portal.azure.com), a potom přejděte na webovou aplikaci WordPress tooyour.</span><span class="sxs-lookup"><span data-stu-id="84838-161">If you already have a WordPress web app running on your Azure subscription, sign in toohello [Azure portal](http://portal.azure.com), and then go tooyour WordPress web app.</span></span> <span data-ttu-id="84838-162">Pokud nemáte webové aplikace WordPress, můžete vytvořit jeden z hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="84838-162">If you don't have a WordPress web app, you can create one from hello Azure Marketplace.</span></span> <span data-ttu-id="84838-163">Další, najdete v části toolearn [vytvořit webovou aplikaci WordPress v Azure App Service](web-sites-php-web-site-gallery.md).</span><span class="sxs-lookup"><span data-stu-id="84838-163">toolearn more, see [Create a WordPress web app in Azure App Service](web-sites-php-web-site-gallery.md).</span></span>
<span data-ttu-id="84838-164">Klikněte na tlačítko **nastavení** > **nasazovací sloty** > **přidat** toocreate nasazovací slot s názvem hello *fáze*.</span><span class="sxs-lookup"><span data-stu-id="84838-164">Click **Settings** > **Deployment slots** > **Add** toocreate a deployment slot with hello name *stage*.</span></span> <span data-ttu-id="84838-165">Slot nasazení je jiné webové aplikace sdílené složky hello stejné prostředky jako hello primární webovou aplikaci, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="84838-165">A deployment slot is another web application that shares hello same resources as hello primary web app that you created previously.</span></span>

    ![Vytvoření fáze nasazovací slot.](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

2. <span data-ttu-id="84838-167">Přidat jiné databáze MySQL, například `wordpress-stage-db`, tooyour skupinu prostředků, `wordpressapp-group`.</span><span class="sxs-lookup"><span data-stu-id="84838-167">Add another MySQL database, say `wordpress-stage-db`, tooyour resource group, `wordpressapp-group`.</span></span>

    ![Přidat skupinu tooresource databáze MySQL](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

3. <span data-ttu-id="84838-169">Aktualizovat hello připojovací řetězce pro fáze nasazení slotu toopoint toohello novou databázi, `wordpress-stage-db`.</span><span class="sxs-lookup"><span data-stu-id="84838-169">Update hello connection strings for your stage deployment slot toopoint toohello new database, `wordpress-stage-db`.</span></span> <span data-ttu-id="84838-170">Webové aplikace produkční `wordpressprodapp`a pracovní webové aplikace, `wordpressprodapp-stage`, musí bodu toodifferent databáze.</span><span class="sxs-lookup"><span data-stu-id="84838-170">Your production web app, `wordpressprodapp`, and staging web app, `wordpressprodapp-stage`, must point toodifferent databases.</span></span>

#### <a name="configure-environment-specific-app-settings"></a><span data-ttu-id="84838-171">Konfigurovat nastavení konkrétní prostředí aplikace</span><span class="sxs-lookup"><span data-stu-id="84838-171">Configure environment-specific app settings</span></span>
<span data-ttu-id="84838-172">Vývojářům můžete uložit dvojice klíč/hodnota řetězce v Azure jako součást informace o konfiguraci hello názvem **nastavení aplikace**, který je spojen s webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="84838-172">Developers can store key/value string pairs in Azure as part of hello configuration information, called **App Settings**, that's associated with a web app.</span></span> <span data-ttu-id="84838-173">V době běhu webové aplikace automaticky načtení těchto hodnot a aby byly k dispozici toocode, který běží ve vaší webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="84838-173">At runtime, web apps automatically retrieve these values and make them available toocode that's running in your web app.</span></span> <span data-ttu-id="84838-174">Z hlediska zabezpečení je to dobrý straně výhody způsobeno citlivé informace, například databázové připojovací řetězce, které zahrnují hesla, nikdy zobrazí jako nešifrovaný text v souboru, jako `wp-config.php`.</span><span class="sxs-lookup"><span data-stu-id="84838-174">From a security perspective, that is a nice side benefit because sensitive information, such as database connection strings that include passwords, never show up as clear text in a file such as `wp-config.php`.</span></span>

<span data-ttu-id="84838-175">Tento proces, který je vysvětlené v následujících odstavcích hello, je užitečné, protože obsahuje soubor změny a změny v databázi pro aplikaci WordPress hello:</span><span class="sxs-lookup"><span data-stu-id="84838-175">This process, which is explained in hello following paragraphs, is useful because it includes both file changes and database changes for hello WordPress app:</span></span>

* <span data-ttu-id="84838-176">Upgrade verze WordPress</span><span class="sxs-lookup"><span data-stu-id="84838-176">WordPress version upgrade</span></span>
* <span data-ttu-id="84838-177">Přidat nové nebo upravovat nebo upgradovat moduly plug-in</span><span class="sxs-lookup"><span data-stu-id="84838-177">Add new or edit or upgrade plugins</span></span>
* <span data-ttu-id="84838-178">Přidat nové nebo upravovat nebo upgradovat motivů</span><span class="sxs-lookup"><span data-stu-id="84838-178">Add new or edit or upgrade themes</span></span>

<span data-ttu-id="84838-179">Konfigurovat nastavení aplikace pro:</span><span class="sxs-lookup"><span data-stu-id="84838-179">Configure app settings for:</span></span>

* <span data-ttu-id="84838-180">Informace o databázi</span><span class="sxs-lookup"><span data-stu-id="84838-180">Database information</span></span>
* <span data-ttu-id="84838-181">Zapnutí zapnutí nebo vypnutí protokolování WordPress</span><span class="sxs-lookup"><span data-stu-id="84838-181">Turning on/off WordPress logging</span></span>
* <span data-ttu-id="84838-182">Nastavení zabezpečení WordPress</span><span class="sxs-lookup"><span data-stu-id="84838-182">WordPress security settings</span></span>

![Nastavení aplikace pro webové aplikace Wordpress](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

<span data-ttu-id="84838-184">Ujistěte se, abyste přidali hello následující nastavení aplikace pro slot vaše produkční webové aplikace a fázi.</span><span class="sxs-lookup"><span data-stu-id="84838-184">Make sure that you add hello following app settings for your production web app and stage slot.</span></span> <span data-ttu-id="84838-185">Všimněte si, že hello produkční webové aplikace a pracovní webovou aplikaci pomocí různých databází.</span><span class="sxs-lookup"><span data-stu-id="84838-185">Note that hello production web app and staging web app use different databases.</span></span>

1. <span data-ttu-id="84838-186">Vymazat hello **nastavení slotu** zaškrtávací políčko pro všechny parametry nastavení hello kromě WP_ENV.</span><span class="sxs-lookup"><span data-stu-id="84838-186">Clear hello **Slot Setting** checkbox for all hello settings parameters except WP_ENV.</span></span> <span data-ttu-id="84838-187">Tento proces bude Prohodit hello konfigurace pro vaši webovou aplikaci, obsah souboru a databáze.</span><span class="sxs-lookup"><span data-stu-id="84838-187">This process will swap hello configuration for your web app, file content, and database.</span></span> <span data-ttu-id="84838-188">Pokud **nastavení slotu** je zaškrtnuto, nastavení aplikací a konfigurace řetězec připojení hello webové aplikace se *není* přesouvat mezi prostředí, když **Prohodit** operaci.</span><span class="sxs-lookup"><span data-stu-id="84838-188">If **Slot Setting** is checked, hello web app’s app settings and connection string configuration will *not* move across environments when doing a **Swap** operation.</span></span> <span data-ttu-id="84838-189">Změny databáze, které jsou k dispozici nebudou porušovat produkční webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="84838-189">Any database changes that are present will not break your production web app.</span></span>

2. <span data-ttu-id="84838-190">Nasazení pomocí služby WebMatrix nebo nástrojů dle vašeho výběru, jako je například FTP, Git nebo PhpMyAdmin hello místním vývojovém prostředí webové aplikace toohello fáze webovou aplikaci a databázi.</span><span class="sxs-lookup"><span data-stu-id="84838-190">Deploy hello local development environment web app toohello stage web app and database by using WebMatrix or tools of your choice, such as FTP, Git, or PhpMyAdmin.</span></span>

    ![Dialog publikování matice webové pro webové aplikace WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

3. <span data-ttu-id="84838-192">Procházet a testování vaší pracovní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="84838-192">Browse and test your staging web app.</span></span> <span data-ttu-id="84838-193">S ohledem na scénář, kde hello motiv hello webové aplikace je toobe aktualizovat zde je hello pracovní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="84838-193">Considering a scenario where hello theme of hello web app is toobe updated, here is hello staging web app.</span></span>

    ![Procházet pracovní před odkládací sloty webové aplikace](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)

4. <span data-ttu-id="84838-195">Pokud všechny spokojeni, klikněte na tlačítko hello **Prohodit** tlačítko na vaše pracovní aplikace toomove webového obsahu toohello provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="84838-195">If all looks good, click hello **Swap** button on your staging web app toomove your content toohello production environment.</span></span> <span data-ttu-id="84838-196">V takovém případě Prohodit hello webovou aplikaci a databázi hello prostředích během každé **Swap** operaci.</span><span class="sxs-lookup"><span data-stu-id="84838-196">In this case, you swap hello web app and hello database across environments during every **Swap** operation.</span></span>

    ![Swap – zobrazení náhledu změn WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

    > [!NOTE]
    > <span data-ttu-id="84838-198">Pokud váš scénář potřebuje tooonly nabízené souborů (bez aktualizace databáze), zkontrolujte **nastavení slotu** pro všechny hello vztahující se k databázi *nastavení aplikace* a *připojovací řetězce nastavení*v hello **nastavení webové aplikace** okno v rámci hello portál Azure před provedením hello **odkládacího souboru**.</span><span class="sxs-lookup"><span data-stu-id="84838-198">If your scenario needs tooonly push files (no database updates), then check **Slot Setting** for all hello database-related *app settings* and *connection strings settings* in hello **Web App Settings** blade within hello Azure portal before doing hello **Swap**.</span></span> <span data-ttu-id="84838-199">V takovém případě %{db_name/, DB_HOST, DB_PASSWORD, DB_USER a výchozí nastavení připojovacího řetězce nesmí zobrazí v zobrazení náhledu změn při **odkládacího souboru**.</span><span class="sxs-lookup"><span data-stu-id="84838-199">In this case, DB_NAME, DB_HOST, DB_PASSWORD, DB_USER, and default connection string settings should not show up in preview changes when you do a **Swap**.</span></span> <span data-ttu-id="84838-200">V tomto okamžiku po dokončení hello **Prohodit** operace, budou mít hello webové aplikace WordPress hello aktualizuje pouze soubory.</span><span class="sxs-lookup"><span data-stu-id="84838-200">At this time, when you complete hello **Swap** operation, hello WordPress web app will have hello updates files only.</span></span>
    >
    >

    <span data-ttu-id="84838-201">Před tím **odkládacího souboru**, zde je webové aplikace WordPress produkční hello.</span><span class="sxs-lookup"><span data-stu-id="84838-201">Before doing a **Swap**, here is hello production WordPress web app.</span></span>
    <span data-ttu-id="84838-202">![Produkční webové aplikaci před odkládací sloty](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)</span><span class="sxs-lookup"><span data-stu-id="84838-202">![Production web app before swapping slots](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)</span></span>

    <span data-ttu-id="84838-203">Po hello **Prohodit** operace, motiv hello se aktualizovala na produkční webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="84838-203">After hello **Swap** operation, hello theme has been updated on your production web app.</span></span>

    ![Produkční webové aplikaci po odkládací sloty](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

5. <span data-ttu-id="84838-205">Pokud budete potřebovat tooroll zpět, můžete přejít toohello produkční webové **nastavení aplikace**a klikněte na tlačítko hello **Prohodit** tlačítko tooswap hello webovou aplikaci a databázi z produkčního slotu. toostaging.</span><span class="sxs-lookup"><span data-stu-id="84838-205">When you need tooroll back, you can go toohello production web **App Settings**, and click hello **Swap** button tooswap hello web app and database from production toostaging slot.</span></span> <span data-ttu-id="84838-206">Pamatujte, že pokud jsou součástí změny v databázi **odkládacího souboru** operace a pak hello příštím nasazení tooyour pracovní webové aplikace, musíte toodeploy hello změny toohello aktuální databáze pro pracovní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="84838-206">Remember that if database changes are included with a **Swap** operation, then hello next time you deploy tooyour staging web app, you need toodeploy hello database changes toohello current database for your staging web app.</span></span> <span data-ttu-id="84838-207">Hello aktuální databáze může být hello předchozí produkční databázi nebo databázi fáze hello.</span><span class="sxs-lookup"><span data-stu-id="84838-207">hello current database might be hello previous production database or hello stage database.</span></span>

#### <a name="summary"></a><span data-ttu-id="84838-208">Souhrn</span><span class="sxs-lookup"><span data-stu-id="84838-208">Summary</span></span>
<span data-ttu-id="84838-209">Toto je zobecněný proces pro každou aplikaci, která má databáze:</span><span class="sxs-lookup"><span data-stu-id="84838-209">Following is a generalized process for any application that has a database:</span></span>

1. <span data-ttu-id="84838-210">Instalace aplikace hello v místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="84838-210">Install hello application on your local environment.</span></span>
2. <span data-ttu-id="84838-211">Zahrnout konfigurace specifické pro prostředí (místní a Azure Web Apps).</span><span class="sxs-lookup"><span data-stu-id="84838-211">Include environment-specific configurations (local and Azure Web Apps).</span></span>
3. <span data-ttu-id="84838-212">Nastavte vaše pracovní a provozní prostředí pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="84838-212">Set up your staging and production environments for Web Apps.</span></span>
4. <span data-ttu-id="84838-213">Pokud máte produkční aplikace už běží v Azure, synchronizujte vaše produkční obsahu (soubory nebo kód a databáze) toolocal a pracovní prostředí.</span><span class="sxs-lookup"><span data-stu-id="84838-213">If you have a production application already running on Azure, sync your production content (files/code and database) toolocal and staging environments.</span></span>
5. <span data-ttu-id="84838-214">Vyvíjet aplikaci na vašem místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="84838-214">Develop your application on your local environment.</span></span>
6. <span data-ttu-id="84838-215">Převeďte webové aplikace produkční pod údržby nebo uzamčeném režimu a synchronizace databáze obsahu z provozní prostředí toostaging a vývojářů.</span><span class="sxs-lookup"><span data-stu-id="84838-215">Place your production web app under maintenance or locked mode, and sync database content from production toostaging and dev environments.</span></span>
7. <span data-ttu-id="84838-216">Nasaďte toohello pracovní prostředí a testování.</span><span class="sxs-lookup"><span data-stu-id="84838-216">Deploy toohello staging environment and test.</span></span>
8. <span data-ttu-id="84838-217">Nasazení tooproduction prostředí.</span><span class="sxs-lookup"><span data-stu-id="84838-217">Deploy tooproduction environment.</span></span>
9. <span data-ttu-id="84838-218">Opakujte kroky 4 až 6.</span><span class="sxs-lookup"><span data-stu-id="84838-218">Repeat steps 4 through 6.</span></span>

### <a name="umbraco"></a><span data-ttu-id="84838-219">Umbraco</span><span class="sxs-lookup"><span data-stu-id="84838-219">Umbraco</span></span>
<span data-ttu-id="84838-220">V této části se dozvíte, jak hello Umbraco CMS využívá toodeploy vlastní modul v prostředí s více DevOps.</span><span class="sxs-lookup"><span data-stu-id="84838-220">In this section, you will learn how hello Umbraco CMS uses a custom module toodeploy across multiple DevOps environments.</span></span> <span data-ttu-id="84838-221">Tento příklad poskytuje jiný přístup toomanaging více vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="84838-221">This example provides a different approach toomanaging multiple development environments.</span></span>

<span data-ttu-id="84838-222">[Umbraco CMS](http://umbraco.com/) je oblíbených řešení .NET CMS, který je používán celá řada vývojářů.</span><span class="sxs-lookup"><span data-stu-id="84838-222">[Umbraco CMS](http://umbraco.com/) is a popular .NET CMS solution that's used by many developers.</span></span> <span data-ttu-id="84838-223">Poskytuje hello [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modulu toodeploy z vývojových toostaging tooproduction prostředí.</span><span class="sxs-lookup"><span data-stu-id="84838-223">It provides hello [Courier2](http://umbraco.com/products/more-add-ons/courier-2) module toodeploy from development toostaging tooproduction environments.</span></span> <span data-ttu-id="84838-224">Místní vývojové prostředí pro webové aplikace Umbraco CMS můžete snadno vytvořit pomocí sady Visual Studio nebo služby WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="84838-224">You can easily create a local development environment for an Umbraco CMS web app by using Visual Studio or WebMatrix.</span></span>

- [<span data-ttu-id="84838-225">Vytvoření webové aplikace Umbraco pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="84838-225">Create an Umbraco web app with Visual Studio</span></span>](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget)
- [<span data-ttu-id="84838-226">Vytvoření webové aplikace Umbraco pomocí služby WebMatrix</span><span class="sxs-lookup"><span data-stu-id="84838-226">Create an Umbraco web app with WebMatrix</span></span>](http://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/installation/creating-umbraco-site-from-webmatrix-web-gallery/)

<span data-ttu-id="84838-227">Vždycky mějte na paměti, tooremove hello `install` ve složce aplikace a nikdy nahrajte ho toostage nebo produkční webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="84838-227">Always remember tooremove hello `install` folder under your application, and never upload it toostage or production web apps.</span></span> <span data-ttu-id="84838-228">Tento kurz používá služba WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="84838-228">This tutorial uses WebMatrix.</span></span>

#### <a name="set-up-a-staging-environment"></a><span data-ttu-id="84838-229">Nastavit pracovní prostředí</span><span class="sxs-lookup"><span data-stu-id="84838-229">Set up a staging environment</span></span>
1. <span data-ttu-id="84838-230">Jak už bylo zmíněno dříve pro hello za předpokladu, že již máte webové aplikace Umbraco CMS a spuštění webové aplikace Umbraco CMS, vytvořte nasazovací slot.</span><span class="sxs-lookup"><span data-stu-id="84838-230">Create a deployment slot as mentioned previously for hello Umbraco CMS web app, assuming you already have an Umbraco CMS web app up and running.</span></span> <span data-ttu-id="84838-231">Pokud ho použít nechcete, můžete vytvořit jeden z hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="84838-231">If you do not, you can create one from hello Marketplace.</span></span>
2. <span data-ttu-id="84838-232">Aktualizovat hello připojovací řetězec pro vaše fáze nasazení slotu toopoint toohello nové **umbraco fáze db** databáze.</span><span class="sxs-lookup"><span data-stu-id="84838-232">Update hello connection string for your stage deployment slot toopoint toohello new **umbraco-stage-db** database.</span></span> <span data-ttu-id="84838-233">Produkční webové aplikace (umbraositecms-1) a pracovní webové aplikace (umbracositecms fáze 1) *musí* bodu toodifferent databáze.</span><span class="sxs-lookup"><span data-stu-id="84838-233">Your production web app (umbraositecms-1) and staging web app (umbracositecms-1-stage) *must* point toodifferent databases.</span></span>

    ![Aktualizovat připojovací řetězec pro přípravu webové aplikace s novou pracovní databázi](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

3. <span data-ttu-id="84838-235">Klikněte na tlačítko **nastavení získat publikování** pro slot nasazení hello **fáze**.</span><span class="sxs-lookup"><span data-stu-id="84838-235">Click **Get Publish settings** for hello deployment slot **stage**.</span></span> <span data-ttu-id="84838-236">Tento proces se stáhnout soubor nastavení publikování, který obsahuje všechny informace hello, Visual Studio nebo WebMatrix vyžaduje toopublish aplikace hello místní vývoj webové aplikace toohello webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="84838-236">This process will download a publish settings file that stores all hello information that Visual Studio or WebMatrix requires toopublish your application from hello local development web app toohello Azure web app.</span></span>

    ![Get publikovat nastavení hello pracovní webové aplikace](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)
4. <span data-ttu-id="84838-238">Otevřete místní vývoj webové aplikace ve službě WebMatrix nebo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="84838-238">Open your local development web app in WebMatrix or Visual Studio.</span></span> <span data-ttu-id="84838-239">Tento kurz používá služba WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="84838-239">This tutorial uses WebMatrix.</span></span> <span data-ttu-id="84838-240">Nejprve je třeba tooimport hello pro webové aplikace pracovního souboru s nastavením publikování.</span><span class="sxs-lookup"><span data-stu-id="84838-240">First, you need tooimport hello publish settings file for your staging web app.</span></span>

    ![Import nastavení publikování pro Umbraco pomocí Web Matrix](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

5. <span data-ttu-id="84838-242">Zkontrolujte změny v dialogovém okně hello a nasadit Azure webové aplikace místní webové aplikace tooyour *umbracositecms-1fáze*.</span><span class="sxs-lookup"><span data-stu-id="84838-242">Review changes in hello dialog box, and deploy your local web app tooyour Azure web app, *umbracositecms-1-stage*.</span></span> <span data-ttu-id="84838-243">Pokud nasazujete soubory přímo tooyour pracovní webová aplikace, vynecháte soubory v hello `~/app_data/TEMP/` složky, protože tyto soubory se vygeneruje, když hello fáze webová aplikace je první spuštění.</span><span class="sxs-lookup"><span data-stu-id="84838-243">When you deploy files directly tooyour staging web app, you will omit files in hello `~/app_data/TEMP/` folder because these files will be regenerated when hello stage web app is first started.</span></span> <span data-ttu-id="84838-244">Musí také vynechejte hello `~/app_data/umbraco.config` souboru, který se také znovu vygeneruje.</span><span class="sxs-lookup"><span data-stu-id="84838-244">You should also omit hello `~/app_data/umbraco.config` file, which will also be regenerated.</span></span>

    ![Publikovat změny v matici web](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

6. <span data-ttu-id="84838-246">Po úspěšném publikování hello Umbraco místní webové aplikace toohello pracovní webové aplikace, procházet tooyour pracovní webové aplikace a spusťte několik toorule testy na všechny problémy.</span><span class="sxs-lookup"><span data-stu-id="84838-246">After you successfully publish hello Umbraco local web app toohello staging web app, browse tooyour staging web app, and run a few tests toorule out any issues.</span></span>

#### <a name="set-up-hello-courier2-deployment-module"></a><span data-ttu-id="84838-247">Nastavit modul nasazení Courier2 hello</span><span class="sxs-lookup"><span data-stu-id="84838-247">Set up hello Courier2 deployment module</span></span>
<span data-ttu-id="84838-248">S hello [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modul, můžete jednoduše pravým tlačítkem toopush obsahu, šablony stylů a vývoj modulů z pracovní webové aplikace tooa produkční webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="84838-248">With hello [Courier2](http://umbraco.com/products/more-add-ons/courier-2) module, you can simply right-click toopush content, style sheets, and development modules from a staging web app tooa production web app.</span></span> <span data-ttu-id="84838-249">Tento proces snižuje riziko hello nejnovější webové aplikace produkční, když nasazujete aktualizaci nasadit.</span><span class="sxs-lookup"><span data-stu-id="84838-249">This process reduces hello risk of breaking your production web app when you deploy an update.</span></span>
<span data-ttu-id="84838-250">Zakoupit licenci pro Courier2 pro hello `*.azurewebsites.net` domény a vlastní domény (například http://abc.com).</span><span class="sxs-lookup"><span data-stu-id="84838-250">Purchase a license for Courier2 for hello `*.azurewebsites.net` domain and your custom domain (say http://abc.com).</span></span> <span data-ttu-id="84838-251">Po zakoupení licence hello místní hello stáhnout licenci (. Soubor – licenční smlouva) v hello `bin` složky.</span><span class="sxs-lookup"><span data-stu-id="84838-251">After you purchase hello license, place hello downloaded license (.LIC file) in hello `bin` folder.</span></span>

![Vyřaďte licenční soubor ve složce Koš](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

1. <span data-ttu-id="84838-253">[Stáhněte si balíček hello Courier2](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/).</span><span class="sxs-lookup"><span data-stu-id="84838-253">[Download hello Courier2 package](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/).</span></span> <span data-ttu-id="84838-254">Přihlášení tooyour fáze webovou aplikaci, například http://umbracocms-site-stage.azurewebsites.net/umbraco, klikněte na tlačítko hello **vývojáře** nabídce a pak klikněte na tlačítko **balíčky** > **instalace místní balíček**.</span><span class="sxs-lookup"><span data-stu-id="84838-254">Sign in tooyour stage web app, say http://umbracocms-site-stage.azurewebsites.net/umbraco, click hello **Developer** menu, and then click **Packages** > **Install local package**.</span></span>

    ![Instalační program balíčku Umbraco](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

2. <span data-ttu-id="84838-256">Nahrání balíčku hello Courier2 pomocí instalačního programu hello.</span><span class="sxs-lookup"><span data-stu-id="84838-256">Upload hello Courier2 package by using hello installer.</span></span>

    ![Nahrání balíčku pro modul Kurýrní](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

3. <span data-ttu-id="84838-258">balíček hello tooconfigure, budete potřebovat tooupdate hello courier.config soubor pod hello **konfigurace** složky vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="84838-258">tooconfigure hello package, you need tooupdate hello courier.config file under hello **Config** folder of your web app.</span></span>

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

4. <span data-ttu-id="84838-259">V části `<repositories>`, zadejte hello produkční lokality adresy URL a uživatelské informace.</span><span class="sxs-lookup"><span data-stu-id="84838-259">Under `<repositories>`, enter hello production site URL and user information.</span></span>
    <span data-ttu-id="84838-260">Pokud používáte hello výchozí zprostředkovatel členství Umbraco, přidejte v hello hello ID pro uživatele správy hello &lt;uživatele&gt; části.</span><span class="sxs-lookup"><span data-stu-id="84838-260">If you are using hello default Umbraco membership provider, then add hello ID for hello Administration user in hello &lt;user&gt; section.</span></span>
    <span data-ttu-id="84838-261">Pokud používáte vlastní zprostředkovatel členství Umbraco, použijte `<login>`,`<password>` v hello Courier2 modulu tooconnect toohello pracoviště.</span><span class="sxs-lookup"><span data-stu-id="84838-261">If you are using a custom Umbraco membership provider, use `<login>`,`<password>` in hello Courier2 module tooconnect toohello production site.</span></span>
    <span data-ttu-id="84838-262">Další podrobnosti [hello v dokumentaci pro modul hello Courier2](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).</span><span class="sxs-lookup"><span data-stu-id="84838-262">For more details, [review hello documentation for hello Courier2 module](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).</span></span>

5. <span data-ttu-id="84838-263">Podobně nainstalujte modul Courier2 hello na produkční lokality a nakonfigurujte ho toopoint toohello fáze webové aplikace ve svém příslušných courier.config souboru, jak je vidět tady.</span><span class="sxs-lookup"><span data-stu-id="84838-263">Similarly, install hello Courier2 module on your production site, and configure it toopoint toohello stage web app in its respective courier.config file as shown here.</span></span>

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

6. <span data-ttu-id="84838-264">Klikněte na tlačítko hello **Courier2** v hello řídicí panel Umbraco CMS webové aplikace a pak klikněte na tlačítko **umístění**.</span><span class="sxs-lookup"><span data-stu-id="84838-264">Click hello **Courier2** tab in hello Umbraco CMS web app dashboard, and then click **Locations**.</span></span> <span data-ttu-id="84838-265">Název úložiště hello byste měli vidět, jak je uvedeno v `courier.config`.</span><span class="sxs-lookup"><span data-stu-id="84838-265">You should see hello repository name as mentioned in `courier.config`.</span></span> <span data-ttu-id="84838-266">Proveďte tento proces na produkční a pracovní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="84838-266">Do this process on both your production and staging web apps.</span></span>

    ![Zobrazení cílové webové aplikace úložiště](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

7. <span data-ttu-id="84838-268">toodeploy obsah z hello pracovní lokality toohello pracoviště, přejděte příliš**obsah**a zvolte existující stránky nebo vytvořte novou stránku.</span><span class="sxs-lookup"><span data-stu-id="84838-268">toodeploy content from hello staging site toohello production site, go too**Content**, and select an existing page or create a new page.</span></span> <span data-ttu-id="84838-269">Z mé webové aplikace, kde je název hello stránku hello bude vyberte existující stránky **Začínáme – nové**a potom klikněte na **uložit a publikovat**.</span><span class="sxs-lookup"><span data-stu-id="84838-269">I will select an existing page from my web app where hello title of hello page is **Getting Started – new**, and then click **Save and Publish**.</span></span>

    ![Změňte název stránky a publikování](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

8. <span data-ttu-id="84838-271">Klikněte pravým tlačítkem na hello upravit stránku tooview všechny možnosti hello.</span><span class="sxs-lookup"><span data-stu-id="84838-271">Right-click hello modified page tooview all hello options.</span></span> <span data-ttu-id="84838-272">Klikněte na tlačítko **Kurýrní** tooopen hello **nasazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="84838-272">Click **Courier** tooopen hello **Deployment** dialog box.</span></span> <span data-ttu-id="84838-273">Klikněte na tlačítko **nasadit** tooinitiate nasazení.</span><span class="sxs-lookup"><span data-stu-id="84838-273">Click **Deploy** tooinitiate deployment.</span></span>

    ![Dialogové okno nasazení Kurýrní modulu](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

9. <span data-ttu-id="84838-275">Zkontrolujte hello změny a pak klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="84838-275">Review hello changes, and then click **Continue**.</span></span>

    ![Kurýrní modul nasazení dialogové okno Kontrola změn](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

    <span data-ttu-id="84838-277">nasazení protokolu Hello se zobrazuje, pokud hello nasazení proběhlo úspěšně.</span><span class="sxs-lookup"><span data-stu-id="84838-277">hello deployment log shows if hello deployment was successful.</span></span>

     ![Zobrazit protokoly nasazení z Kurýrní modulu](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

10. <span data-ttu-id="84838-279">Procházet vaše produkční webové aplikace toosee, pokud se projeví hello.</span><span class="sxs-lookup"><span data-stu-id="84838-279">Browse your production web app toosee if hello changes are reflected.</span></span>

     ![Procházet produkční webové aplikace](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

<span data-ttu-id="84838-281">Další informace o tom, jak toouse Kurýrní, zkontrolujte hello dokumentaci toolearn.</span><span class="sxs-lookup"><span data-stu-id="84838-281">toolearn more about how toouse Courier, review hello documentation.</span></span>

#### <a name="how-tooupgrade-hello-umbraco-cms-version"></a><span data-ttu-id="84838-282">Jak tooupgrade hello verze Umbraco CMS</span><span class="sxs-lookup"><span data-stu-id="84838-282">How tooupgrade hello Umbraco CMS version</span></span>
<span data-ttu-id="84838-283">Kurýrní bude není nápovědy, které upgradujete z jedné verze nástroje tooanother Umbraco CMS.</span><span class="sxs-lookup"><span data-stu-id="84838-283">Courier will not help you upgrade from one version of Umbraco CMS tooanother.</span></span> <span data-ttu-id="84838-284">Při upgradu verzi Umbraco CMS, musíte vyhledat nekompatibilitu s vlastní moduly nebo moduly od partnerů a hello Umbraco základní knihovny.</span><span class="sxs-lookup"><span data-stu-id="84838-284">When you upgrade an Umbraco CMS version, you must check for incompatibilities with your custom modules or modules from partners and hello Umbraco Core libraries.</span></span> <span data-ttu-id="84838-285">Tady jsou osvědčené postupy:</span><span class="sxs-lookup"><span data-stu-id="84838-285">Here are best practices:</span></span>

* <span data-ttu-id="84838-286">Vždy zálohování webové aplikace a databáze před upgradem.</span><span class="sxs-lookup"><span data-stu-id="84838-286">Always back up your web app and database before you upgrade.</span></span> <span data-ttu-id="84838-287">Na webové aplikace v Azure můžete nastavit automatické zálohování pro své weby pomocí funkce zálohování hello a v případě potřeby pomocí funkce obnovení hello obnovit váš web.</span><span class="sxs-lookup"><span data-stu-id="84838-287">On web apps in Azure, you can set up automatic backups for your websites by using hello backup feature and restore your site if needed by using hello restore feature.</span></span> <span data-ttu-id="84838-288">Další podrobnosti najdete v tématu [jak tooback až vaše webová aplikace](web-sites-backup.md) a [jak toorestore vaší webové aplikace](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="84838-288">For more details, see [How tooback up your web app](web-sites-backup.md) and [How toorestore your web app](web-sites-restore.md).</span></span>
* <span data-ttu-id="84838-289">Zkontrolujte, jestli jsou kompatibilní s verzí hello, kterou provádíte upgrade na balíčky od partnerů.</span><span class="sxs-lookup"><span data-stu-id="84838-289">Check if packages from partners are compatible with hello version you're upgrading to.</span></span> <span data-ttu-id="84838-290">V balíčku hello stránce pro stažení, zkontrolujte hello projektu kompatibilitu s verzí Umbraco CMS.</span><span class="sxs-lookup"><span data-stu-id="84838-290">On hello package's download page, review hello project compatibility with Umbraco CMS version.</span></span>

<span data-ttu-id="84838-291">Další informace o tooupgrade vaši webovou aplikaci místně, [najdete obecné pokyny upgradu hello](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).</span><span class="sxs-lookup"><span data-stu-id="84838-291">For more details about how tooupgrade your web app locally, [see hello general upgrade guidance](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).</span></span>

<span data-ttu-id="84838-292">Po upgradu místním vývojovém webu, publikujte toohello změny hello pracovní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="84838-292">After your local development site is upgraded, publish hello changes toohello staging web app.</span></span> <span data-ttu-id="84838-293">Testování vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="84838-293">Test your application.</span></span> <span data-ttu-id="84838-294">Pokud všechny spokojeni, použijte hello **Prohodit** tlačítko tooswap pracovní lokality toohello produkční webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="84838-294">If all looks good, use hello **Swap** button tooswap your staging site toohello production web app.</span></span> <span data-ttu-id="84838-295">Při použití hello **Prohodit** operace, hello změny, které bude mít vliv na si můžete prohlédnout v konfiguraci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="84838-295">When you use hello **Swap** operation, you can view hello changes that will be affected in your web app's configuration.</span></span> <span data-ttu-id="84838-296">To **Prohodit** operace prohození hello webové aplikace a databáze.</span><span class="sxs-lookup"><span data-stu-id="84838-296">This **Swap** operation swaps hello web apps and databases.</span></span> <span data-ttu-id="84838-297">Po hello **Prohodit**hello produkční webové aplikace se bod toohello umbraco fáze db databázi a hello pracovní webové aplikace se bod tooumbraco-produkčnímu db databáze.</span><span class="sxs-lookup"><span data-stu-id="84838-297">After hello **Swap**, hello production web app will point toohello umbraco-stage-db database, and hello staging web app will point tooumbraco-prod-db database.</span></span>

![Swap – náhled pro nasazení systému CMS Umbraco](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

<span data-ttu-id="84838-299">Zde jsou výhody odkládací hello webové aplikace a databáze hello:</span><span class="sxs-lookup"><span data-stu-id="84838-299">Here are advantages of swapping both hello web app and hello database:</span></span>

* <span data-ttu-id="84838-300">Můžete se vrátit zpět předchozí verze vaší webové aplikace s jinou toohello **Prohodit** Pokud jsou nějaké problémy aplikace.</span><span class="sxs-lookup"><span data-stu-id="84838-300">You can roll back toohello previous version of your web app with another **Swap** if there are any application issues.</span></span>
* <span data-ttu-id="84838-301">Pro účely upgradu musíte toodeploy soubory a databáze z hello pracovní webové aplikace toohello produkční webové aplikace a databáze.</span><span class="sxs-lookup"><span data-stu-id="84838-301">For an upgrade, you need toodeploy files and databases from hello staging web app toohello production web app and database.</span></span> <span data-ttu-id="84838-302">Když nasadíte soubory a databáze, můžete přejít nesprávný celou řadu věcí.</span><span class="sxs-lookup"><span data-stu-id="84838-302">Many things can go wrong when you deploy files and databases.</span></span> <span data-ttu-id="84838-303">Pomocí hello **Prohodit** funkce přihrádky, můžeme zkrátit dobu prostojů při upgradu a snížilo riziko chyby, ke kterým může dojít, když nasazujete změny hello.</span><span class="sxs-lookup"><span data-stu-id="84838-303">By using hello **Swap** feature of slots, we can reduce downtime during an upgrade and reduce hello risk of failures that can occur when you deploy changes.</span></span>
* <span data-ttu-id="84838-304">Můžete provést **A / B testování** pomocí hello [testování v produkčním prostředí](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) funkce.</span><span class="sxs-lookup"><span data-stu-id="84838-304">You can do **A/B testing** by using hello [Testing in production](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) feature.</span></span>

<span data-ttu-id="84838-305">Tento příklad ukazuje hello flexibilitu hello platformy, kde můžete vytvořit vlastní moduly podobné tooUmbraco Kurýrní modulu toomanage nasazení prostředích.</span><span class="sxs-lookup"><span data-stu-id="84838-305">This example shows you hello flexibility of hello platform where you can build custom modules similar tooUmbraco Courier module toomanage deployment across environments.</span></span>

## <a name="references"></a><span data-ttu-id="84838-306">Odkazy</span><span class="sxs-lookup"><span data-stu-id="84838-306">References</span></span>
[<span data-ttu-id="84838-307">Agile software development službou Azure App Service</span><span class="sxs-lookup"><span data-stu-id="84838-307">Agile software development with Azure App Service</span></span>](app-service-agile-software-development.md)

[<span data-ttu-id="84838-308">Nastavení přípravných prostředí pro webové aplikace v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="84838-308">Set up staging environments for web apps in Azure App Service</span></span>](web-sites-staged-publishing.md)

[<span data-ttu-id="84838-309">Jak tooblock webový přístup k produkční toonon nasazovací sloty</span><span class="sxs-lookup"><span data-stu-id="84838-309">How tooblock web access toonon-production deployment slots</span></span>](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
