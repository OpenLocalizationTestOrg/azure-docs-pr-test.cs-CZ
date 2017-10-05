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
# <a name="use-devops-environments-effectively-for-your-web-apps"></a><span data-ttu-id="d81d0-103">Efektivně používat DevOps prostředí pro webové aplikace</span><span class="sxs-lookup"><span data-stu-id="d81d0-103">Use DevOps environments effectively for your web apps</span></span>
<span data-ttu-id="d81d0-104">Tento článek ukazuje, jak nastavit a spravovat nasazení webové aplikace, pokud několik verzí aplikace jsou v různých prostředích, jako je například vývoj, pracovní, zajištění kvality (QA) a provozní.</span><span class="sxs-lookup"><span data-stu-id="d81d0-104">This article shows you how to set up and manage web application deployments when multiple versions of your application are in various environments, such as development, staging, quality assurance (QA), and production.</span></span> <span data-ttu-id="d81d0-105">Každou verzi vaší aplikace lze považovat za vývojového prostředí pro konkrétní účel procesu nasazení.</span><span class="sxs-lookup"><span data-stu-id="d81d0-105">Each version of your application can be considered as a development environment for the specific purpose of your deployment process.</span></span> <span data-ttu-id="d81d0-106">Například mohou vývojáři prostředí QA pro otestování kvality aplikace před jejich odešlete změny do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="d81d0-106">For example, developers can use the QA environment to test the quality of the application before they push the changes to production.</span></span>
<span data-ttu-id="d81d0-107">Více vývojových prostředí může být složité, protože je potřeba sledovat kódu, spravovat prostředky (výpočetní, webové aplikace, databáze, mezipaměti atd.) a nasazení kódu v rámci prostředí.</span><span class="sxs-lookup"><span data-stu-id="d81d0-107">Multiple development environments can be a challenge because you need to track code, manage resources (compute, web app, database, cache, etc.), and deploy code across environments.</span></span>

## <a name="set-up-a-non-production-environment-stage-dev-qa"></a><span data-ttu-id="d81d0-108">Nastavit mimo produkční prostředí (fáze, vývoj, QA)</span><span class="sxs-lookup"><span data-stu-id="d81d0-108">Set up a non-production environment (stage, dev, QA)</span></span>
<span data-ttu-id="d81d0-109">Po produkční webové aplikace je spuštěná, je dalším krokem vytvoření mimo produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="d81d0-109">After a production web app is up and running, the next step is to create a non-production environment.</span></span> <span data-ttu-id="d81d0-110">Pokud chcete použít nasazovací sloty, ujistěte se, zda jsou spuštěny v režimu plán Standard nebo Premium Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="d81d0-110">To use deployment slots, make sure that you are running in the Standard or Premium Azure App Service plan mode.</span></span> <span data-ttu-id="d81d0-111">Nasazovací sloty jsou za chodu webových aplikací, které mají své vlastní názvy hostitelů.</span><span class="sxs-lookup"><span data-stu-id="d81d0-111">Deployment slots are live web apps that have their own host names.</span></span> <span data-ttu-id="d81d0-112">Webové aplikace obsah a konfiguraci elementy lze vzájemně zaměněny mezi dvěma sloty nasazení, včetně produkční slot.</span><span class="sxs-lookup"><span data-stu-id="d81d0-112">Web app content and configuration elements can be swapped between two deployment slots, including the production slot.</span></span> <span data-ttu-id="d81d0-113">Když nasadíte aplikaci pro slot nasazení, získáte následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d81d0-113">When you deploy your application to a deployment slot, you get the following benefits:</span></span>

- <span data-ttu-id="d81d0-114">Změny webové aplikace ve přípravný slot nasazení můžete ověřit před Prohodit aplikaci pomocí produkčního slotu.</span><span class="sxs-lookup"><span data-stu-id="d81d0-114">You can validate changes to a web app in a staging deployment slot before you swap the app with the production slot.</span></span>
- <span data-ttu-id="d81d0-115">Při nasazení webové aplikace do patice nejprve a Prohodit do produkčního prostředí, jsou všechny instance přihrádky jestli před prohazují do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="d81d0-115">When you deploy a web app to a slot first and swap it into production, all instances of the slot are warmed up before being swapped into production.</span></span> <span data-ttu-id="d81d0-116">Tento proces eliminuje výpadek při nasazení webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-116">This process eliminates downtime when you deploy your web app.</span></span> <span data-ttu-id="d81d0-117">Přesměrování provozu je bezproblémové a žádné požadavky jsou vyřazeny z důvodu operace prohození.</span><span class="sxs-lookup"><span data-stu-id="d81d0-117">The traffic redirection is seamless, and no requests are dropped due to swap operations.</span></span> <span data-ttu-id="d81d0-118">K automatizaci tento celý pracovní postup, nakonfigurujte [Prohodit automaticky](web-sites-staged-publishing.md#configure-auto-swap) při swap předběžné ověření není potřeba.</span><span class="sxs-lookup"><span data-stu-id="d81d0-118">To automate this entire workflow, configure [Auto Swap](web-sites-staged-publishing.md#configure-auto-swap) when pre-swap validation is not needed.</span></span>
- <span data-ttu-id="d81d0-119">Po prohození přihrádku, která má dříve dvoufázové instalace webové aplikace teď má předchozí produkční webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-119">After a swap, the slot that has the previously staged web app now has the previous production web app.</span></span> <span data-ttu-id="d81d0-120">Pokud změny, které jsou vzájemně zaměněny na produkční slot není podle očekávání, můžete provést stejný prohození okamžitě získat webového "poslední známá funkční konfigurace" aplikace zpět.</span><span class="sxs-lookup"><span data-stu-id="d81d0-120">If the changes swapped into the production slot are not as you expected, you can perform the same swap immediately to get your "last known good" web app back.</span></span>

<span data-ttu-id="d81d0-121">Přípravný slot nasazení, naleznete v tématu [nastavení přípravných prostředí pro webové aplikace v Azure App Service](web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="d81d0-121">To set up a staging deployment slot, see [Set up staging environments for web apps in Azure App Service](web-sites-staged-publishing.md).</span></span> <span data-ttu-id="d81d0-122">Každé prostředí by měla obsahovat vlastní sadu prostředků.</span><span class="sxs-lookup"><span data-stu-id="d81d0-122">Every environment should include its own set of resources.</span></span> <span data-ttu-id="d81d0-123">Například pokud vaše webová aplikace používá databázi, pak produkční a pracovní webové aplikace by měl použít jiné databáze.</span><span class="sxs-lookup"><span data-stu-id="d81d0-123">For example, if your web app uses a database, then both production and staging web apps should use different databases.</span></span> <span data-ttu-id="d81d0-124">Přidáte pracovní prostředí prostředky vývoj například databáze, úložiště nebo mezipaměti nastavit pracovní vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="d81d0-124">Add staging development environment resources such as database, storage, or cache to set your staging development environment.</span></span>

## <a name="examples-of-using-multiple-development-environments"></a><span data-ttu-id="d81d0-125">Příklady používání prostředí s více vývoj</span><span class="sxs-lookup"><span data-stu-id="d81d0-125">Examples of using multiple development environments</span></span>
<span data-ttu-id="d81d0-126">Jakýkoli projekt, postupujte podle správu zdrojového kódu s alespoň dvěma prostředími: vývoj a provozní.</span><span class="sxs-lookup"><span data-stu-id="d81d0-126">Any project should follow source code management with at least two environments: development and production.</span></span> <span data-ttu-id="d81d0-127">Pokud používáte systémy správy obsahu (CMSs), aplikační architektury, atd., nemusí podporovat aplikace v tomto scénáři bez úprav.</span><span class="sxs-lookup"><span data-stu-id="d81d0-127">If you use content management systems (CMSs), application frameworks, etc., the application might not support this scenario without customization.</span></span> <span data-ttu-id="d81d0-128">Tato případě platí pro některé z oblíbených rozhraní, které jsou popsané v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="d81d0-128">This eventuality is true for some of the popular frameworks that are discussed in the following sections.</span></span> <span data-ttu-id="d81d0-129">Velké množství otázky se do paměti při práci s CMS nebo rozhraní, jako například:</span><span class="sxs-lookup"><span data-stu-id="d81d0-129">Lots of questions come to mind when you work with CMS/frameworks, such as:</span></span>

- <span data-ttu-id="d81d0-130">Jak vám přerušení obsah do různých prostředí?</span><span class="sxs-lookup"><span data-stu-id="d81d0-130">How do you break the content out into different environments?</span></span>
- <span data-ttu-id="d81d0-131">Jaké soubory můžete změnit bez ovlivnění framework verze aktualizace?</span><span class="sxs-lookup"><span data-stu-id="d81d0-131">What files can you change without affecting framework version updates?</span></span>
- <span data-ttu-id="d81d0-132">Jak budete spravovat konfigurace za prostředí?</span><span class="sxs-lookup"><span data-stu-id="d81d0-132">How do you manage configurations per environment?</span></span>
- <span data-ttu-id="d81d0-133">Jak budete spravovat aktualizace verzí pro moduly, moduly plug-in a rozhraní jádra?</span><span class="sxs-lookup"><span data-stu-id="d81d0-133">How do you manage version updates for modules, plugins, and the core framework?</span></span>

<span data-ttu-id="d81d0-134">Existuje mnoho způsobů, jak nastavit prostředí s více pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="d81d0-134">There are many ways to set up multiple environments for your project.</span></span> <span data-ttu-id="d81d0-135">Následující příklady ukazují jednu metodu pro každou příslušné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d81d0-135">The following examples show one method for each respective application.</span></span>

### <a name="wordpress"></a><span data-ttu-id="d81d0-136">WordPress</span><span class="sxs-lookup"><span data-stu-id="d81d0-136">WordPress</span></span>
<span data-ttu-id="d81d0-137">V této části se dozvíte, jak nastavit pracovní postup nasazení pomocí sloty pro WordPress.</span><span class="sxs-lookup"><span data-stu-id="d81d0-137">In this section, you will learn how to set up a deployment workflow by using slots for WordPress.</span></span> <span data-ttu-id="d81d0-138">WordPress, jako je většina řešení CMS, nepodporuje více vývojových prostředí bez úprav.</span><span class="sxs-lookup"><span data-stu-id="d81d0-138">WordPress, like most CMS solutions, does not support multiple development environments without customization.</span></span> <span data-ttu-id="d81d0-139">Funkce Web Apps služby Azure App Service má několik funkcí, které usnadňují uložit nastavení konfigurace mimo váš kód.</span><span class="sxs-lookup"><span data-stu-id="d81d0-139">The Web Apps feature of Azure App Service has a few features that make it easy to store configuration settings outside your code.</span></span>

1. <span data-ttu-id="d81d0-140">Před vytvořením přípravný slot nastavte na podporu prostředí s více kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-140">Before you create a staging slot, set up your application code to support multiple environments.</span></span> <span data-ttu-id="d81d0-141">Pro podporu prostředí s více v WordPress, budete muset upravit `wp-config.php` na místním vývojovém webu aplikaci a na začátek souboru přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="d81d0-141">To support multiple environments in WordPress, you need to edit `wp-config.php` on your local development web app and add the following code at the beginning of the file.</span></span> <span data-ttu-id="d81d0-142">Tento proces vám umožní aplikaci a vyberte správnou konfiguraci na základě vybraných prostředí.</span><span class="sxs-lookup"><span data-stu-id="d81d0-142">This process will enable your application to pick the correct configuration based on the selected environment.</span></span>

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

2. <span data-ttu-id="d81d0-143">Vytvořte složku v kořenovém adresáři webové aplikace volá `config`a přidejte `wp-config.azure.php` a `wp-config.local.php` soubory, které představují prostředí Azure a místní prostředí, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="d81d0-143">Create a folder under web app root called `config`, and add the `wp-config.azure.php` and `wp-config.local.php` files, which represent your Azure environment and local environment respectively.</span></span>

3. <span data-ttu-id="d81d0-144">Zkopírujte následující `wp-config.local.php`:</span><span class="sxs-lookup"><span data-stu-id="d81d0-144">Copy the following in `wp-config.local.php`:</span></span>

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

    <span data-ttu-id="d81d0-145">Nastavení zabezpečení klíčů, jak je ukázáno v předchozí kód může pomoci zabránit se hacker vaší webové aplikace, takže pomocí jedinečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d81d0-145">Setting the security keys as illustrated in the previous code can help to prevent your web app from being hacked, so use unique values.</span></span> <span data-ttu-id="d81d0-146">Pokud potřebujete vygenerování řetězce pro zabezpečení zkratky uvedené v kódu, můžete [přejděte na automatické generátor](https://api.wordpress.org/secret-key/1.1/salt) k vytvoření nové páry klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="d81d0-146">If you need to generate the string for security keys mentioned in the code, you can [go to the automatic generator](https://api.wordpress.org/secret-key/1.1/salt) to create new key/value pairs.</span></span>

4. <span data-ttu-id="d81d0-147">Zkopírujte následující kód v `wp-config.azure.php`:</span><span class="sxs-lookup"><span data-stu-id="d81d0-147">Copy the following code in `wp-config.azure.php`:</span></span>

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

#### <a name="use-relative-paths"></a><span data-ttu-id="d81d0-148">Použít relativní cesty</span><span class="sxs-lookup"><span data-stu-id="d81d0-148">Use relative paths</span></span>
<span data-ttu-id="d81d0-149">Jednu věc konfigurace v aplikaci WordPress je relativní cesty.</span><span class="sxs-lookup"><span data-stu-id="d81d0-149">One last thing to configure in the WordPress app is relative paths.</span></span> <span data-ttu-id="d81d0-150">WordPress ukládá informace o adresách URL v databázi.</span><span class="sxs-lookup"><span data-stu-id="d81d0-150">WordPress stores URL information in the database.</span></span> <span data-ttu-id="d81d0-151">Toto úložiště umožňuje přesunutí obsah z jednoho prostředí do druhého obtížnější.</span><span class="sxs-lookup"><span data-stu-id="d81d0-151">This storage makes moving content from one environment to another more difficult.</span></span> <span data-ttu-id="d81d0-152">Je potřeba aktualizovat databázi pokaždé, když přejdete z místní fáze nebo fáze do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="d81d0-152">You need to update the database every time you move from local to stage or stage to production environments.</span></span> <span data-ttu-id="d81d0-153">Chcete-li snížit riziko problémů, které může být způsobeno s nasazením databázi pokaždé, když nasazujete z jednoho prostředí do druhého, použijte [kořenové relativní odkazy modulu plug-in](https://wordpress.org/plugins/root-relative-urls/), které můžete nainstalovat pomocí řídicího panelu WordPress správce.</span><span class="sxs-lookup"><span data-stu-id="d81d0-153">To reduce the risk of issues that can be caused with deploying a database every time you deploy from one environment to another, use the [Relative Root links plugin](https://wordpress.org/plugins/root-relative-urls/), which you can install by using the WordPress administrator dashboard.</span></span>

<span data-ttu-id="d81d0-154">Přidejte následující položky do vaší `wp-config.php` soubor před `That's all, stop editing!` komentář:</span><span class="sxs-lookup"><span data-stu-id="d81d0-154">Add the following entries to your `wp-config.php` file before the `That's all, stop editing!` comment:</span></span>

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

<span data-ttu-id="d81d0-155">Aktivovat modulu plug-in prostřednictvím `Plugins` nabídky na řídicím panelu WordPress správce.</span><span class="sxs-lookup"><span data-stu-id="d81d0-155">Activate the plugin through the `Plugins` menu in WordPress administrator dashboard.</span></span> <span data-ttu-id="d81d0-156">Uložte nastavení trvalý odkaz pro aplikaci WordPress.</span><span class="sxs-lookup"><span data-stu-id="d81d0-156">Save your permalink settings for WordPress app.</span></span>

#### <a name="the-final-wp-configphp-file"></a><span data-ttu-id="d81d0-157">Konečné `wp-config.php` souboru</span><span class="sxs-lookup"><span data-stu-id="d81d0-157">The final `wp-config.php` file</span></span>
<span data-ttu-id="d81d0-158">Žádné aktualizace základní WordPress nebude mít vliv na vaše `wp-config.php`, `wp-config.azure.php`, a `wp-config.local.php` soubory.</span><span class="sxs-lookup"><span data-stu-id="d81d0-158">Any WordPress core updates will not affect your `wp-config.php`, `wp-config.azure.php`, and `wp-config.local.php` files.</span></span> <span data-ttu-id="d81d0-159">Tady je finální verzi `wp-config.php` souboru:</span><span class="sxs-lookup"><span data-stu-id="d81d0-159">Here's a final version of the `wp-config.php` file:</span></span>

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

#### <a name="set-up-a-staging-environment"></a><span data-ttu-id="d81d0-160">Nastavit pracovní prostředí</span><span class="sxs-lookup"><span data-stu-id="d81d0-160">Set up a staging environment</span></span>
1. <span data-ttu-id="d81d0-161">Pokud již máte webovou aplikaci WordPress systémem vašeho předplatného Azure, přihlaste se k [portál Azure](http://portal.azure.com)a potom přejděte na webové aplikace WordPress.</span><span class="sxs-lookup"><span data-stu-id="d81d0-161">If you already have a WordPress web app running on your Azure subscription, sign in to the [Azure portal](http://portal.azure.com), and then go to your WordPress web app.</span></span> <span data-ttu-id="d81d0-162">Pokud nemáte webové aplikace WordPress, můžete vytvořit jeden z Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-162">If you don't have a WordPress web app, you can create one from the Azure Marketplace.</span></span> <span data-ttu-id="d81d0-163">Další informace najdete v tématu [vytvořit webovou aplikaci WordPress v Azure App Service](web-sites-php-web-site-gallery.md).</span><span class="sxs-lookup"><span data-stu-id="d81d0-163">To learn more, see [Create a WordPress web app in Azure App Service](web-sites-php-web-site-gallery.md).</span></span>
<span data-ttu-id="d81d0-164">Klikněte na tlačítko **nastavení** > **nasazovací sloty** > **přidat** vytvořit nasazovací slot s názvem *fáze* .</span><span class="sxs-lookup"><span data-stu-id="d81d0-164">Click **Settings** > **Deployment slots** > **Add** to create a deployment slot with the name *stage*.</span></span> <span data-ttu-id="d81d0-165">Slot nasazení je jiné webové aplikace, který sdílí stejné prostředky jako primární webové aplikace, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="d81d0-165">A deployment slot is another web application that shares the same resources as the primary web app that you created previously.</span></span>

    ![Vytvoření fáze nasazovací slot.](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

2. <span data-ttu-id="d81d0-167">Přidat jiné databáze MySQL, například `wordpress-stage-db`, do skupiny prostředků, `wordpressapp-group`.</span><span class="sxs-lookup"><span data-stu-id="d81d0-167">Add another MySQL database, say `wordpress-stage-db`, to your resource group, `wordpressapp-group`.</span></span>

    ![Přidání databáze MySQL do skupiny prostředků](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

3. <span data-ttu-id="d81d0-169">Aktualizovat připojovací řetězce pro slot nasazení vaší fáze tak, aby odkazoval na databázi nové `wordpress-stage-db`.</span><span class="sxs-lookup"><span data-stu-id="d81d0-169">Update the connection strings for your stage deployment slot to point to the new database, `wordpress-stage-db`.</span></span> <span data-ttu-id="d81d0-170">Webové aplikace produkční `wordpressprodapp`a pracovní webové aplikace, `wordpressprodapp-stage`, musíte přejít do různých databází.</span><span class="sxs-lookup"><span data-stu-id="d81d0-170">Your production web app, `wordpressprodapp`, and staging web app, `wordpressprodapp-stage`, must point to different databases.</span></span>

#### <a name="configure-environment-specific-app-settings"></a><span data-ttu-id="d81d0-171">Konfigurovat nastavení konkrétní prostředí aplikace</span><span class="sxs-lookup"><span data-stu-id="d81d0-171">Configure environment-specific app settings</span></span>
<span data-ttu-id="d81d0-172">Vývojářům můžete uložit dvojice klíč/hodnota řetězce v Azure jako součást informace o konfiguraci, nazývá **nastavení aplikace**, který je spojen s webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d81d0-172">Developers can store key/value string pairs in Azure as part of the configuration information, called **App Settings**, that's associated with a web app.</span></span> <span data-ttu-id="d81d0-173">V době běhu webové aplikace automaticky načtení těchto hodnot a zpřístupnit je kód, který běží ve vaší webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d81d0-173">At runtime, web apps automatically retrieve these values and make them available to code that's running in your web app.</span></span> <span data-ttu-id="d81d0-174">Z hlediska zabezpečení je to dobrý straně výhody způsobeno citlivé informace, například databázové připojovací řetězce, které zahrnují hesla, nikdy zobrazí jako nešifrovaný text v souboru, jako `wp-config.php`.</span><span class="sxs-lookup"><span data-stu-id="d81d0-174">From a security perspective, that is a nice side benefit because sensitive information, such as database connection strings that include passwords, never show up as clear text in a file such as `wp-config.php`.</span></span>

<span data-ttu-id="d81d0-175">Tento proces, který je vysvětlené v následujících odstavcích, je užitečné, protože obsahuje soubor změny a změny v databázi pro aplikaci WordPress:</span><span class="sxs-lookup"><span data-stu-id="d81d0-175">This process, which is explained in the following paragraphs, is useful because it includes both file changes and database changes for the WordPress app:</span></span>

* <span data-ttu-id="d81d0-176">Upgrade verze WordPress</span><span class="sxs-lookup"><span data-stu-id="d81d0-176">WordPress version upgrade</span></span>
* <span data-ttu-id="d81d0-177">Přidat nové nebo upravovat nebo upgradovat moduly plug-in</span><span class="sxs-lookup"><span data-stu-id="d81d0-177">Add new or edit or upgrade plugins</span></span>
* <span data-ttu-id="d81d0-178">Přidat nové nebo upravovat nebo upgradovat motivů</span><span class="sxs-lookup"><span data-stu-id="d81d0-178">Add new or edit or upgrade themes</span></span>

<span data-ttu-id="d81d0-179">Konfigurovat nastavení aplikace pro:</span><span class="sxs-lookup"><span data-stu-id="d81d0-179">Configure app settings for:</span></span>

* <span data-ttu-id="d81d0-180">Informace o databázi</span><span class="sxs-lookup"><span data-stu-id="d81d0-180">Database information</span></span>
* <span data-ttu-id="d81d0-181">Zapnutí zapnutí nebo vypnutí protokolování WordPress</span><span class="sxs-lookup"><span data-stu-id="d81d0-181">Turning on/off WordPress logging</span></span>
* <span data-ttu-id="d81d0-182">Nastavení zabezpečení WordPress</span><span class="sxs-lookup"><span data-stu-id="d81d0-182">WordPress security settings</span></span>

![Nastavení aplikace pro webové aplikace Wordpress](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

<span data-ttu-id="d81d0-184">Ujistěte se, přidat následující nastavení aplikace pro slot vaše produkční webové aplikace a fázi.</span><span class="sxs-lookup"><span data-stu-id="d81d0-184">Make sure that you add the following app settings for your production web app and stage slot.</span></span> <span data-ttu-id="d81d0-185">Upozorňujeme, že webové aplikace produkční a pracovní webové aplikace použít jiné databáze.</span><span class="sxs-lookup"><span data-stu-id="d81d0-185">Note that the production web app and staging web app use different databases.</span></span>

1. <span data-ttu-id="d81d0-186">Vymazat **nastavení slotu** zaškrtávací políčko pro všechny parametry nastavení s výjimkou WP_ENV.</span><span class="sxs-lookup"><span data-stu-id="d81d0-186">Clear the **Slot Setting** checkbox for all the settings parameters except WP_ENV.</span></span> <span data-ttu-id="d81d0-187">Tento proces bude odkládacího souboru konfigurace pro vaši webovou aplikaci, obsah souboru a databáze.</span><span class="sxs-lookup"><span data-stu-id="d81d0-187">This process will swap the configuration for your web app, file content, and database.</span></span> <span data-ttu-id="d81d0-188">Pokud **nastavení slotu** je zaškrtnuto, nastavení aplikací webové aplikace a připojovací řetězec konfigurace *není* přesouvat mezi prostředí, když **odkládacího souboru** operaci.</span><span class="sxs-lookup"><span data-stu-id="d81d0-188">If **Slot Setting** is checked, the web app’s app settings and connection string configuration will *not* move across environments when doing a **Swap** operation.</span></span> <span data-ttu-id="d81d0-189">Změny databáze, které jsou k dispozici nebudou porušovat produkční webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-189">Any database changes that are present will not break your production web app.</span></span>

2. <span data-ttu-id="d81d0-190">Nasazení místního vývojového prostředí webové aplikace do fáze webovou aplikaci a databázi pomocí služby WebMatrix nebo nástrojů dle vašeho výběru, jako je například FTP, Git nebo PhpMyAdmin.</span><span class="sxs-lookup"><span data-stu-id="d81d0-190">Deploy the local development environment web app to the stage web app and database by using WebMatrix or tools of your choice, such as FTP, Git, or PhpMyAdmin.</span></span>

    ![Dialog publikování matice webové pro webové aplikace WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

3. <span data-ttu-id="d81d0-192">Procházet a testování vaší pracovní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-192">Browse and test your staging web app.</span></span> <span data-ttu-id="d81d0-193">S ohledem na scénář, kde motivu webové aplikace se mají aktualizovat zde je pracovní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-193">Considering a scenario where the theme of the web app is to be updated, here is the staging web app.</span></span>

    ![Procházet pracovní před odkládací sloty webové aplikace](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)

4. <span data-ttu-id="d81d0-195">Pokud všechny spokojeni, klikněte na tlačítko **Prohodit** tlačítko na pracovní webové aplikace přesunout obsah do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="d81d0-195">If all looks good, click the **Swap** button on your staging web app to move your content to the production environment.</span></span> <span data-ttu-id="d81d0-196">V takovém případě Prohodit webovou aplikaci a databázi prostředích během každé **Swap** operaci.</span><span class="sxs-lookup"><span data-stu-id="d81d0-196">In this case, you swap the web app and the database across environments during every **Swap** operation.</span></span>

    ![Swap – zobrazení náhledu změn WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

    > [!NOTE]
    > <span data-ttu-id="d81d0-198">Pokud váš scénář potřebuje pouze push souborů (bez aktualizace databáze), zkontrolujte **nastavení slotu** pro všechny vztahující se k databázi *nastavení aplikace* a *připojovací řetězce nastavení* v **nastavení webové aplikace** okno v rámci portálu Azure před tím **odkládacího souboru**.</span><span class="sxs-lookup"><span data-stu-id="d81d0-198">If your scenario needs to only push files (no database updates), then check **Slot Setting** for all the database-related *app settings* and *connection strings settings* in the **Web App Settings** blade within the Azure portal before doing the **Swap**.</span></span> <span data-ttu-id="d81d0-199">V takovém případě %{db_name/, DB_HOST, DB_PASSWORD, DB_USER a výchozí nastavení připojovacího řetězce nesmí zobrazí v zobrazení náhledu změn při **odkládacího souboru**.</span><span class="sxs-lookup"><span data-stu-id="d81d0-199">In this case, DB_NAME, DB_HOST, DB_PASSWORD, DB_USER, and default connection string settings should not show up in preview changes when you do a **Swap**.</span></span> <span data-ttu-id="d81d0-200">V tomto okamžiku po dokončení **Prohodit** operace, webové aplikace WordPress budou mít pouze soubory aktualizace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-200">At this time, when you complete the **Swap** operation, the WordPress web app will have the updates files only.</span></span>
    >
    >

    <span data-ttu-id="d81d0-201">Před tím **odkládacího souboru**, zde je webové aplikace WordPress produkční.</span><span class="sxs-lookup"><span data-stu-id="d81d0-201">Before doing a **Swap**, here is the production WordPress web app.</span></span>
    <span data-ttu-id="d81d0-202">![Produkční webové aplikaci před odkládací sloty](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)</span><span class="sxs-lookup"><span data-stu-id="d81d0-202">![Production web app before swapping slots](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)</span></span>

    <span data-ttu-id="d81d0-203">Po **Prohodit** operace, jaký motiv se aktualizovala na produkční webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-203">After the **Swap** operation, the theme has been updated on your production web app.</span></span>

    ![Produkční webové aplikaci po odkládací sloty](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

5. <span data-ttu-id="d81d0-205">Pokud je nutné vrátit zpět, můžete přejít na web produkční **nastavení aplikace**a klikněte na tlačítko **Swap** tlačítko webové aplikace a databáze z výroby přípravný slot prohodit.</span><span class="sxs-lookup"><span data-stu-id="d81d0-205">When you need to roll back, you can go to the production web **App Settings**, and click the **Swap** button to swap the web app and database from production to staging slot.</span></span> <span data-ttu-id="d81d0-206">Pamatujte, že pokud změny databáze jsou součástí **odkládacího souboru** operace, pak při příštím nasazení do webové aplikace pracovní, je třeba nasadit změn databáze do aktuální databáze pro pracovní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d81d0-206">Remember that if database changes are included with a **Swap** operation, then the next time you deploy to your staging web app, you need to deploy the database changes to the current database for your staging web app.</span></span> <span data-ttu-id="d81d0-207">Aktuální databáze může být předchozí produkční databázi nebo databázi fáze.</span><span class="sxs-lookup"><span data-stu-id="d81d0-207">The current database might be the previous production database or the stage database.</span></span>

#### <a name="summary"></a><span data-ttu-id="d81d0-208">Souhrn</span><span class="sxs-lookup"><span data-stu-id="d81d0-208">Summary</span></span>
<span data-ttu-id="d81d0-209">Toto je zobecněný proces pro každou aplikaci, která má databáze:</span><span class="sxs-lookup"><span data-stu-id="d81d0-209">Following is a generalized process for any application that has a database:</span></span>

1. <span data-ttu-id="d81d0-210">Nainstalujte aplikaci na vašem místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d81d0-210">Install the application on your local environment.</span></span>
2. <span data-ttu-id="d81d0-211">Zahrnout konfigurace specifické pro prostředí (místní a Azure Web Apps).</span><span class="sxs-lookup"><span data-stu-id="d81d0-211">Include environment-specific configurations (local and Azure Web Apps).</span></span>
3. <span data-ttu-id="d81d0-212">Nastavte vaše pracovní a provozní prostředí pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-212">Set up your staging and production environments for Web Apps.</span></span>
4. <span data-ttu-id="d81d0-213">Pokud máte produkční aplikace už běží v Azure, synchronizujte svůj produkční obsah (soubory nebo kód a databáze) do místní a pracovní prostředí.</span><span class="sxs-lookup"><span data-stu-id="d81d0-213">If you have a production application already running on Azure, sync your production content (files/code and database) to local and staging environments.</span></span>
5. <span data-ttu-id="d81d0-214">Vyvíjet aplikaci na vašem místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d81d0-214">Develop your application on your local environment.</span></span>
6. <span data-ttu-id="d81d0-215">Převeďte webové aplikace produkční pod údržby nebo uzamčeném režimu a synchronizace databáze obsahu z produkční do prostředí pracovní a vývojářů.</span><span class="sxs-lookup"><span data-stu-id="d81d0-215">Place your production web app under maintenance or locked mode, and sync database content from production to staging and dev environments.</span></span>
7. <span data-ttu-id="d81d0-216">Nasaďte do pracovní prostředí a testování.</span><span class="sxs-lookup"><span data-stu-id="d81d0-216">Deploy to the staging environment and test.</span></span>
8. <span data-ttu-id="d81d0-217">Nasazení do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="d81d0-217">Deploy to production environment.</span></span>
9. <span data-ttu-id="d81d0-218">Opakujte kroky 4 až 6.</span><span class="sxs-lookup"><span data-stu-id="d81d0-218">Repeat steps 4 through 6.</span></span>

### <a name="umbraco"></a><span data-ttu-id="d81d0-219">Umbraco</span><span class="sxs-lookup"><span data-stu-id="d81d0-219">Umbraco</span></span>
<span data-ttu-id="d81d0-220">V této části se dozvíte, jak Umbraco CMS používá vlastní modul pro nasazení ve více prostředích DevOps.</span><span class="sxs-lookup"><span data-stu-id="d81d0-220">In this section, you will learn how the Umbraco CMS uses a custom module to deploy across multiple DevOps environments.</span></span> <span data-ttu-id="d81d0-221">Tento příklad poskytuje jiný přístup ke správě více vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="d81d0-221">This example provides a different approach to managing multiple development environments.</span></span>

<span data-ttu-id="d81d0-222">[Umbraco CMS](http://umbraco.com/) je oblíbených řešení .NET CMS, který je používán celá řada vývojářů.</span><span class="sxs-lookup"><span data-stu-id="d81d0-222">[Umbraco CMS](http://umbraco.com/) is a popular .NET CMS solution that's used by many developers.</span></span> <span data-ttu-id="d81d0-223">Poskytuje [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modulu k nasazení z vývojového do pracovní do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="d81d0-223">It provides the [Courier2](http://umbraco.com/products/more-add-ons/courier-2) module to deploy from development to staging to production environments.</span></span> <span data-ttu-id="d81d0-224">Místní vývojové prostředí pro webové aplikace Umbraco CMS můžete snadno vytvořit pomocí sady Visual Studio nebo služby WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="d81d0-224">You can easily create a local development environment for an Umbraco CMS web app by using Visual Studio or WebMatrix.</span></span>

- [<span data-ttu-id="d81d0-225">Vytvoření webové aplikace Umbraco pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d81d0-225">Create an Umbraco web app with Visual Studio</span></span>](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget)
- [<span data-ttu-id="d81d0-226">Vytvoření webové aplikace Umbraco pomocí služby WebMatrix</span><span class="sxs-lookup"><span data-stu-id="d81d0-226">Create an Umbraco web app with WebMatrix</span></span>](http://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/installation/creating-umbraco-site-from-webmatrix-web-gallery/)

<span data-ttu-id="d81d0-227">Vždy nezapomeňte odebrat `install` ve složce aplikace a nikdy ho nahrát do fáze nebo produkční webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-227">Always remember to remove the `install` folder under your application, and never upload it to stage or production web apps.</span></span> <span data-ttu-id="d81d0-228">Tento kurz používá služba WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="d81d0-228">This tutorial uses WebMatrix.</span></span>

#### <a name="set-up-a-staging-environment"></a><span data-ttu-id="d81d0-229">Nastavit pracovní prostředí</span><span class="sxs-lookup"><span data-stu-id="d81d0-229">Set up a staging environment</span></span>
1. <span data-ttu-id="d81d0-230">Nasazovací slot vytvořte, jak je uvedeno nahoře pro webovou aplikaci Umbraco CMS, za předpokladu, že již máte webové aplikace Umbraco CMS a spuštěna.</span><span class="sxs-lookup"><span data-stu-id="d81d0-230">Create a deployment slot as mentioned previously for the Umbraco CMS web app, assuming you already have an Umbraco CMS web app up and running.</span></span> <span data-ttu-id="d81d0-231">Pokud ho použít nechcete, můžete vytvořit jeden z Marketplace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-231">If you do not, you can create one from the Marketplace.</span></span>
2. <span data-ttu-id="d81d0-232">Aktualizovat připojovací řetězec pro slot nasazení vaší fáze tak, aby odkazoval na nový **umbraco fáze db** databáze.</span><span class="sxs-lookup"><span data-stu-id="d81d0-232">Update the connection string for your stage deployment slot to point to the new **umbraco-stage-db** database.</span></span> <span data-ttu-id="d81d0-233">Produkční webové aplikace (umbraositecms-1) a pracovní webové aplikace (umbracositecms fáze 1) *musí* bodu do různých databází.</span><span class="sxs-lookup"><span data-stu-id="d81d0-233">Your production web app (umbraositecms-1) and staging web app (umbracositecms-1-stage) *must* point to different databases.</span></span>

    ![Aktualizovat připojovací řetězec pro přípravu webové aplikace s novou pracovní databázi](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

3. <span data-ttu-id="d81d0-235">Klikněte na tlačítko **nastavení získat publikování** slotu pro nasazení **fáze**.</span><span class="sxs-lookup"><span data-stu-id="d81d0-235">Click **Get Publish settings** for the deployment slot **stage**.</span></span> <span data-ttu-id="d81d0-236">Tento proces se stáhnout soubor nastavení publikování, který obsahuje všechny informace, který Visual Studio nebo WebMatrix vyžaduje, aby publikovat svoji aplikaci z místní vývoj webové aplikace do webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="d81d0-236">This process will download a publish settings file that stores all the information that Visual Studio or WebMatrix requires to publish your application from the local development web app to the Azure web app.</span></span>

    ![Získat publikovat nastavení pracovní webové aplikace](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)
4. <span data-ttu-id="d81d0-238">Otevřete místní vývoj webové aplikace ve službě WebMatrix nebo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d81d0-238">Open your local development web app in WebMatrix or Visual Studio.</span></span> <span data-ttu-id="d81d0-239">Tento kurz používá služba WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="d81d0-239">This tutorial uses WebMatrix.</span></span> <span data-ttu-id="d81d0-240">Nejprve je třeba importovat soubor nastavení publikování pro pracovní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d81d0-240">First, you need to import the publish settings file for your staging web app.</span></span>

    ![Import nastavení publikování pro Umbraco pomocí Web Matrix](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

5. <span data-ttu-id="d81d0-242">Zkontrolujte změny v dialogovém okně a nasadíte místní webové aplikace do vaší webové aplikace Azure, *umbracositecms-1fáze*.</span><span class="sxs-lookup"><span data-stu-id="d81d0-242">Review changes in the dialog box, and deploy your local web app to your Azure web app, *umbracositecms-1-stage*.</span></span> <span data-ttu-id="d81d0-243">Pokud nasazujete soubory přímo do pracovní webové aplikace, vynecháte soubory v `~/app_data/TEMP/` složky, protože tyto soubory se vygeneruje, pokud je první fázi webové aplikace spuštěna.</span><span class="sxs-lookup"><span data-stu-id="d81d0-243">When you deploy files directly to your staging web app, you will omit files in the `~/app_data/TEMP/` folder because these files will be regenerated when the stage web app is first started.</span></span> <span data-ttu-id="d81d0-244">Musí také vynechejte `~/app_data/umbraco.config` souboru, který se také znovu vygeneruje.</span><span class="sxs-lookup"><span data-stu-id="d81d0-244">You should also omit the `~/app_data/umbraco.config` file, which will also be regenerated.</span></span>

    ![Publikovat změny v matici web](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

6. <span data-ttu-id="d81d0-246">Po úspěšně publikovat místní webové aplikace Umbraco do pracovní webové aplikace, přejděte do pracovní webové aplikace a spustit několik testů vylučte všechny problémy.</span><span class="sxs-lookup"><span data-stu-id="d81d0-246">After you successfully publish the Umbraco local web app to the staging web app, browse to your staging web app, and run a few tests to rule out any issues.</span></span>

#### <a name="set-up-the-courier2-deployment-module"></a><span data-ttu-id="d81d0-247">Nastavit modul Courier2 nasazení</span><span class="sxs-lookup"><span data-stu-id="d81d0-247">Set up the Courier2 deployment module</span></span>
<span data-ttu-id="d81d0-248">Pomocí [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modul, můžete můžete jednoduše klikněte pravým tlačítkem na předávaný obsah, šablony stylů a vývoj modulů z pracovní webové aplikace do produkčního webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-248">With the [Courier2](http://umbraco.com/products/more-add-ons/courier-2) module, you can simply right-click to push content, style sheets, and development modules from a staging web app to a production web app.</span></span> <span data-ttu-id="d81d0-249">Tento proces snižuje riziko nejnovější webové aplikace produkční, když nasazujete aktualizaci nasadit.</span><span class="sxs-lookup"><span data-stu-id="d81d0-249">This process reduces the risk of breaking your production web app when you deploy an update.</span></span>
<span data-ttu-id="d81d0-250">Zakoupit licenci pro Courier2 pro `*.azurewebsites.net` domény a vlastní domény (například http://abc.com).</span><span class="sxs-lookup"><span data-stu-id="d81d0-250">Purchase a license for Courier2 for the `*.azurewebsites.net` domain and your custom domain (say http://abc.com).</span></span> <span data-ttu-id="d81d0-251">Po zakoupení licence umístit stažený licence (. Soubor – licenční smlouva) v `bin` složky.</span><span class="sxs-lookup"><span data-stu-id="d81d0-251">After you purchase the license, place the downloaded license (.LIC file) in the `bin` folder.</span></span>

![Vyřaďte licenční soubor ve složce Koš](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

1. <span data-ttu-id="d81d0-253">[Stáhněte si balíček Courier2](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/).</span><span class="sxs-lookup"><span data-stu-id="d81d0-253">[Download the Courier2 package](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/).</span></span> <span data-ttu-id="d81d0-254">Přihlášení do webové aplikace fázi, například http://umbracocms-site-stage.azurewebsites.net/umbraco, klikněte na tlačítko **vývojáře** nabídce a pak klikněte na tlačítko **balíčky** > **místní instalace balíček**.</span><span class="sxs-lookup"><span data-stu-id="d81d0-254">Sign in to your stage web app, say http://umbracocms-site-stage.azurewebsites.net/umbraco, click the **Developer** menu, and then click **Packages** > **Install local package**.</span></span>

    ![Instalační program balíčku Umbraco](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

2. <span data-ttu-id="d81d0-256">Nahrání balíčku Courier2 pomocí Instalační služby.</span><span class="sxs-lookup"><span data-stu-id="d81d0-256">Upload the Courier2 package by using the installer.</span></span>

    ![Nahrání balíčku pro modul Kurýrní](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

3. <span data-ttu-id="d81d0-258">Ke konfiguraci balíčku, je potřeba aktualizovat soubor do courier.config **konfigurace** složky vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-258">To configure the package, you need to update the courier.config file under the **Config** folder of your web app.</span></span>

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

4. <span data-ttu-id="d81d0-259">V části `<repositories>`, zadejte informace o provozním serveru adresu URL a uživatele.</span><span class="sxs-lookup"><span data-stu-id="d81d0-259">Under `<repositories>`, enter the production site URL and user information.</span></span>
    <span data-ttu-id="d81d0-260">Pokud používáte výchozí zprostředkovatel členství Umbraco, přidejte ID pro uživatele správy v &lt;uživatele&gt; části.</span><span class="sxs-lookup"><span data-stu-id="d81d0-260">If you are using the default Umbraco membership provider, then add the ID for the Administration user in the &lt;user&gt; section.</span></span>
    <span data-ttu-id="d81d0-261">Pokud používáte vlastní zprostředkovatel členství Umbraco, použijte `<login>`,`<password>` v modulu Courier2 pro připojení k produkční lokality.</span><span class="sxs-lookup"><span data-stu-id="d81d0-261">If you are using a custom Umbraco membership provider, use `<login>`,`<password>` in the Courier2 module to connect to the production site.</span></span>
    <span data-ttu-id="d81d0-262">Další podrobnosti [najdete v dokumentaci pro modul Courier2](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).</span><span class="sxs-lookup"><span data-stu-id="d81d0-262">For more details, [review the documentation for the Courier2 module](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).</span></span>

5. <span data-ttu-id="d81d0-263">Podobně nainstalovat modul Courier2 provozního webu a nakonfigurovat ji tak, aby odkazoval na fázi webovou aplikaci ve svém příslušných courier.config souboru, jak je vidět tady.</span><span class="sxs-lookup"><span data-stu-id="d81d0-263">Similarly, install the Courier2 module on your production site, and configure it to point to the stage web app in its respective courier.config file as shown here.</span></span>

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

6. <span data-ttu-id="d81d0-264">Klikněte **Courier2** v řídicím panelu Umbraco CMS webové aplikace a pak klikněte na tlačítko **umístění**.</span><span class="sxs-lookup"><span data-stu-id="d81d0-264">Click the **Courier2** tab in the Umbraco CMS web app dashboard, and then click **Locations**.</span></span> <span data-ttu-id="d81d0-265">Název úložiště byste měli vidět, jak je uvedeno v `courier.config`.</span><span class="sxs-lookup"><span data-stu-id="d81d0-265">You should see the repository name as mentioned in `courier.config`.</span></span> <span data-ttu-id="d81d0-266">Proveďte tento proces na produkční a pracovní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-266">Do this process on both your production and staging web apps.</span></span>

    ![Zobrazení cílové webové aplikace úložiště](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

7. <span data-ttu-id="d81d0-268">Pokud chcete nasadit obsah z webu pracovní na pracoviště, přejděte na **obsah**a zvolte existující stránky nebo vytvořte novou stránku.</span><span class="sxs-lookup"><span data-stu-id="d81d0-268">To deploy content from the staging site to the production site, go to **Content**, and select an existing page or create a new page.</span></span> <span data-ttu-id="d81d0-269">Z mé webové aplikace, kde je název příslušné stránky se vyberte existující stránky **Začínáme – nové**a potom klikněte na **uložit a publikovat**.</span><span class="sxs-lookup"><span data-stu-id="d81d0-269">I will select an existing page from my web app where the title of the page is **Getting Started – new**, and then click **Save and Publish**.</span></span>

    ![Změňte název stránky a publikování](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

8. <span data-ttu-id="d81d0-271">Klikněte pravým tlačítkem na upravené stránky zobrazíte všechny možnosti.</span><span class="sxs-lookup"><span data-stu-id="d81d0-271">Right-click the modified page to view all the options.</span></span> <span data-ttu-id="d81d0-272">Klikněte na tlačítko **Kurýrní** otevřete **nasazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d81d0-272">Click **Courier** to open the **Deployment** dialog box.</span></span> <span data-ttu-id="d81d0-273">Klikněte na tlačítko **nasadit** k inicializaci nasazení.</span><span class="sxs-lookup"><span data-stu-id="d81d0-273">Click **Deploy** to initiate deployment.</span></span>

    ![Dialogové okno nasazení Kurýrní modulu](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

9. <span data-ttu-id="d81d0-275">Zkontrolujte změny a pak klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="d81d0-275">Review the changes, and then click **Continue**.</span></span>

    ![Kurýrní modul nasazení dialogové okno Kontrola změn](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

    <span data-ttu-id="d81d0-277">Nasazení protokolu se zobrazuje, pokud nasazení bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="d81d0-277">The deployment log shows if the deployment was successful.</span></span>

     ![Zobrazit protokoly nasazení z Kurýrní modulu](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

10. <span data-ttu-id="d81d0-279">Procházejte webové aplikace produkční chcete zobrazit, pokud změny se projeví.</span><span class="sxs-lookup"><span data-stu-id="d81d0-279">Browse your production web app to see if the changes are reflected.</span></span>

     ![Procházet produkční webové aplikace](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

<span data-ttu-id="d81d0-281">Další informace o tom, jak používat Kurýrní, zkontrolujte v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="d81d0-281">To learn more about how to use Courier, review the documentation.</span></span>

#### <a name="how-to-upgrade-the-umbraco-cms-version"></a><span data-ttu-id="d81d0-282">Postup upgradu na verzi Umbraco CMS</span><span class="sxs-lookup"><span data-stu-id="d81d0-282">How to upgrade the Umbraco CMS version</span></span>
<span data-ttu-id="d81d0-283">Kurýrní bude není nápovědy, které upgradujete z jedné verze nástroje Umbraco CMS do jiného.</span><span class="sxs-lookup"><span data-stu-id="d81d0-283">Courier will not help you upgrade from one version of Umbraco CMS to another.</span></span> <span data-ttu-id="d81d0-284">Při upgradu verzi Umbraco CMS, je nutné zkontrolovat pro nekompatibilitu s vlastní moduly nebo moduly od partnerů a Umbraco základní knihovny.</span><span class="sxs-lookup"><span data-stu-id="d81d0-284">When you upgrade an Umbraco CMS version, you must check for incompatibilities with your custom modules or modules from partners and the Umbraco Core libraries.</span></span> <span data-ttu-id="d81d0-285">Tady jsou osvědčené postupy:</span><span class="sxs-lookup"><span data-stu-id="d81d0-285">Here are best practices:</span></span>

* <span data-ttu-id="d81d0-286">Vždy zálohování webové aplikace a databáze před upgradem.</span><span class="sxs-lookup"><span data-stu-id="d81d0-286">Always back up your web app and database before you upgrade.</span></span> <span data-ttu-id="d81d0-287">Na webové aplikace v Azure můžete nastavit automatické zálohování pro své weby pomocí funkce zálohování a obnovení vaší lokality v případě potřeby pomocí funkce obnovení.</span><span class="sxs-lookup"><span data-stu-id="d81d0-287">On web apps in Azure, you can set up automatic backups for your websites by using the backup feature and restore your site if needed by using the restore feature.</span></span> <span data-ttu-id="d81d0-288">Další podrobnosti najdete v tématu [zálohování webové aplikace](web-sites-backup.md) a [obnovení webové aplikace](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="d81d0-288">For more details, see [How to back up your web app](web-sites-backup.md) and [How to restore your web app](web-sites-restore.md).</span></span>
* <span data-ttu-id="d81d0-289">Zkontrolujte, jestli jsou kompatibilní s verzí, které provádíte upgrade na balíčky od partnerů.</span><span class="sxs-lookup"><span data-stu-id="d81d0-289">Check if packages from partners are compatible with the version you're upgrading to.</span></span> <span data-ttu-id="d81d0-290">V balíčku stránce pro stažení, zkontrolujte kompatibilitu projektu s Umbraco CMS verzí.</span><span class="sxs-lookup"><span data-stu-id="d81d0-290">On the package's download page, review the project compatibility with Umbraco CMS version.</span></span>

<span data-ttu-id="d81d0-291">Další informace o tom, jak upgradovat webovou aplikaci místně [najdete obecné pokyny upgradu](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).</span><span class="sxs-lookup"><span data-stu-id="d81d0-291">For more details about how to upgrade your web app locally, [see the general upgrade guidance](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).</span></span>

<span data-ttu-id="d81d0-292">Po upgradu místním vývojovém webu, publikujte změny do pracovní webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-292">After your local development site is upgraded, publish the changes to the staging web app.</span></span> <span data-ttu-id="d81d0-293">Testování vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-293">Test your application.</span></span> <span data-ttu-id="d81d0-294">Pokud všechny spokojeni, použijte **Swap** tlačítko Prohodit váš pracovní web na produkční webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d81d0-294">If all looks good, use the **Swap** button to swap your staging site to the production web app.</span></span> <span data-ttu-id="d81d0-295">Při použití **Prohodit** operace, můžete zobrazit změny, které bude mít vliv v konfiguraci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-295">When you use the **Swap** operation, you can view the changes that will be affected in your web app's configuration.</span></span> <span data-ttu-id="d81d0-296">To **Prohodit** operace prohození webové aplikace a databáze.</span><span class="sxs-lookup"><span data-stu-id="d81d0-296">This **Swap** operation swaps the web apps and databases.</span></span> <span data-ttu-id="d81d0-297">Po **odkládacího souboru**, produkční webové aplikace bude odkazovat na databázi umbraco fáze db a pracovní webové aplikace bude přejděte do databáze umbraco produkčnímu db.</span><span class="sxs-lookup"><span data-stu-id="d81d0-297">After the **Swap**, the production web app will point to the umbraco-stage-db database, and the staging web app will point to umbraco-prod-db database.</span></span>

![Swap – náhled pro nasazení systému CMS Umbraco](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

<span data-ttu-id="d81d0-299">Zde jsou výhody prohození webové aplikace a databáze:</span><span class="sxs-lookup"><span data-stu-id="d81d0-299">Here are advantages of swapping both the web app and the database:</span></span>

* <span data-ttu-id="d81d0-300">Můžete se vrátit zpět na předchozí verzi vaší webové aplikace s jinou **Prohodit** Pokud jsou nějaké problémy aplikace.</span><span class="sxs-lookup"><span data-stu-id="d81d0-300">You can roll back to the previous version of your web app with another **Swap** if there are any application issues.</span></span>
* <span data-ttu-id="d81d0-301">Pro účely upgradu musíte nasadit soubory a databází z pracovní webové aplikace na produkční webové aplikaci a databázi.</span><span class="sxs-lookup"><span data-stu-id="d81d0-301">For an upgrade, you need to deploy files and databases from the staging web app to the production web app and database.</span></span> <span data-ttu-id="d81d0-302">Když nasadíte soubory a databáze, můžete přejít nesprávný celou řadu věcí.</span><span class="sxs-lookup"><span data-stu-id="d81d0-302">Many things can go wrong when you deploy files and databases.</span></span> <span data-ttu-id="d81d0-303">Pomocí **Prohodit** funkce přihrádky, můžeme zkrátit dobu prostojů při upgradu a snížilo riziko chyby, ke kterým může dojít, když nasazujete změny.</span><span class="sxs-lookup"><span data-stu-id="d81d0-303">By using the **Swap** feature of slots, we can reduce downtime during an upgrade and reduce the risk of failures that can occur when you deploy changes.</span></span>
* <span data-ttu-id="d81d0-304">Můžete provést **A / B testování** pomocí [testování v produkčním prostředí](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) funkce.</span><span class="sxs-lookup"><span data-stu-id="d81d0-304">You can do **A/B testing** by using the [Testing in production](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) feature.</span></span>

<span data-ttu-id="d81d0-305">Tento příklad ukazuje, flexibilní platformu kde můžete vytvořit vlastní moduly podobná Umbraco Kurýrní modul pro správu nasazení v prostředí.</span><span class="sxs-lookup"><span data-stu-id="d81d0-305">This example shows you the flexibility of the platform where you can build custom modules similar to Umbraco Courier module to manage deployment across environments.</span></span>

## <a name="references"></a><span data-ttu-id="d81d0-306">Odkazy</span><span class="sxs-lookup"><span data-stu-id="d81d0-306">References</span></span>
[<span data-ttu-id="d81d0-307">Agile software development službou Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d81d0-307">Agile software development with Azure App Service</span></span>](app-service-agile-software-development.md)

[<span data-ttu-id="d81d0-308">Nastavení přípravných prostředí pro webové aplikace v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d81d0-308">Set up staging environments for web apps in Azure App Service</span></span>](web-sites-staged-publishing.md)

[<span data-ttu-id="d81d0-309">Blokování webového přístupu k testovacím nasazovací sloty</span><span class="sxs-lookup"><span data-stu-id="d81d0-309">How to block web access to non-production deployment slots</span></span>](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
