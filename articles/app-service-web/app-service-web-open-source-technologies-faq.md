---
title: "Webové aplikace technologie Open source časté otázky k Azure | Microsoft Docs"
description: "Získejte odpovědi na nejčastější dotazy týkající se technologie open-source ve funkci Web Apps služby Azure App Service."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: d37b53242c0b231d83425a59ecbe50216216a95b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="open-source-technologies-faqs-for-web-apps-in-azure"></a><span data-ttu-id="0e1dc-103">Technologie Open source nejčastější dotazy pro webové aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="0e1dc-103">Open-source technologies FAQs for Web Apps in Azure</span></span>

<span data-ttu-id="0e1dc-104">Tento článek obsahuje odpovědi na nejčastější dotazy (FAQ) o problémech s nástrojem technologie open source pro [funkce Web Apps služby Azure App Service](https://azure.microsoft.com/services/app-service/web/).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-104">This article has answers to frequently asked questions (FAQs) about issues with open-source technologies for the [Web Apps feature of Azure App Service](https://azure.microsoft.com/services/app-service/web/).</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-cleardb-database-is-down-how-do-i-resolve-this"></a><span data-ttu-id="0e1dc-105">Databázi ClearDB je mimo provoz.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-105">My ClearDB database is down.</span></span> <span data-ttu-id="0e1dc-106">Jak to lze vyřešit?</span><span class="sxs-lookup"><span data-stu-id="0e1dc-106">How do I resolve this?</span></span>

<span data-ttu-id="0e1dc-107">Databáze s problémy, kontaktujte [ClearDB podporu](https://www.cleardb.com/developers/help/support).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-107">For database-related issues, contact [ClearDB support](https://www.cleardb.com/developers/help/support).</span></span> 

<span data-ttu-id="0e1dc-108">Odpovědi na časté otázky o ClearDB, naleznete v části [cleardb – nejčastější dotazy k](https://docs.microsoft.com/azure/store-cleardb-faq/).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-108">For answers to common questions about ClearDB, see [ClearDB FAQs](https://docs.microsoft.com/azure/store-cleardb-faq/).</span></span>

## <a name="why-isnt-my-cleardb-database-listed-in-the-portal"></a><span data-ttu-id="0e1dc-109">Proč není uvedená Moje databázi ClearDB portálu?</span><span class="sxs-lookup"><span data-stu-id="0e1dc-109">Why isn't my ClearDB database listed in the portal?</span></span>

<span data-ttu-id="0e1dc-110">Pokud vytvoříte databázi ClearDB v [portál Azure](http://portal.azure.com/), funkci se nezobrazí v databázi [portál Azure classic](http://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-110">If you create a ClearDB database in the [Azure portal](http://portal.azure.com/), the database doesn't appear in the [Azure classic portal](http://manage.windowsazure.com/).</span></span> <span data-ttu-id="0e1dc-111">Chcete-li tento problém obejít, můžete ručně propojit databáze do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-111">To work around this, you can manually link your database to the web app.</span></span>

<span data-ttu-id="0e1dc-112">Podobně pokud vytvoříte databázi ClearDB v [portál Azure classic](http://manage.windowsazure.com/), se nezobrazí v databázi [portál Azure](http://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-112">Similarly, if you create a ClearDB database in the [Azure classic portal](http://manage.windowsazure.com/),  you won't see your database in the [Azure portal](http://portal.azure.com/).</span></span> <span data-ttu-id="0e1dc-113">V takovém případě je k dispozici žádné alternativní řešení.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-113">In this case, no workaround is available.</span></span> 

<span data-ttu-id="0e1dc-114">Další informace najdete v tématu [nejčastější dotazy pro databáze ClearDB MySQL službou Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-114">For more information, see [FAQs for ClearDB MySQL databases with Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span></span>

## <a name="why-wasnt-my-cleardb-database-migrated-during-my-subscription-migration"></a><span data-ttu-id="0e1dc-115">Proč nebyl migrován databázi ClearDB během migrace Moje předplatné?</span><span class="sxs-lookup"><span data-stu-id="0e1dc-115">Why wasn't my ClearDB database migrated during my subscription migration?</span></span>

<span data-ttu-id="0e1dc-116">Když provádíte migraci prostředků ve předplatných, platí určitá omezení.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-116">When you perform resource migration across subscriptions, some limitations apply.</span></span> <span data-ttu-id="0e1dc-117">Databáze ClearDB MySQL je služba třetích stran a nemigruje během migrace předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-117">A ClearDB MySQL database is a third-party service and is not migrated during an Azure subscription migration.</span></span>

<span data-ttu-id="0e1dc-118">Pokud nespravujete migraci databáze MySQL před zahájením migrace vašich prostředků Azure, může být databáze MySQL cleardb – není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-118">If you don't manage the migration of your MySQL database before you migrate your Azure resources, your ClearDB MySQL database might be unavailable.</span></span> <span data-ttu-id="0e1dc-119">Abyste tomu předešli, nejprve ručně přenést databázi ClearDB, a potom přenést předplatné Azure pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-119">To avoid this, first, manually migrate your ClearDB database, and then migrate the Azure subscription for your web app.</span></span>

<span data-ttu-id="0e1dc-120">Další informace najdete v tématu [nejčastější dotazy pro databáze ClearDB MySQL službou Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-120">For more information, see [FAQs for ClearDB MySQL databases with Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span></span>

## <a name="how-do-i-turn-on-php-logging-to-troubleshoot-php-issues"></a><span data-ttu-id="0e1dc-121">Jak zapnout na PHP protokolování potíží PHP?</span><span class="sxs-lookup"><span data-stu-id="0e1dc-121">How do I turn on PHP logging to troubleshoot PHP issues?</span></span>

<span data-ttu-id="0e1dc-122">Zapnutí protokolování PHP:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-122">To turn on PHP logging:</span></span>

1. <span data-ttu-id="0e1dc-123">Přihlaste se k vaší [Kudu webu](https://*yourwebsitename*.scm.azurewebsites.net).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-123">Sign in to your [Kudu website](https://*yourwebsitename*.scm.azurewebsites.net).</span></span>
2. <span data-ttu-id="0e1dc-124">V nabídce nahoře vyberte **ladění konzoly** > **CMD**.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-124">In the top menu, select **Debug Console** > **CMD**.</span></span>
3. <span data-ttu-id="0e1dc-125">Vyberte **lokality** složky.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-125">Select the **Site** folder.</span></span>
4. <span data-ttu-id="0e1dc-126">Vyberte **wwwroot** složky.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-126">Select the **wwwroot** folder.</span></span>
5. <span data-ttu-id="0e1dc-127">Vyberte  **+**  ikonu a potom vyberte **nový soubor**.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-127">Select the **+** icon, and then select **New File**.</span></span>
6. <span data-ttu-id="0e1dc-128">Nastavte název souboru na **. user.ini**.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-128">Set the file name to **.user.ini**.</span></span>
7. <span data-ttu-id="0e1dc-129">Vyberte ikonu tužky vedle **. user.ini**.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-129">Select the pencil icon next to **.user.ini**.</span></span>
8. <span data-ttu-id="0e1dc-130">V souboru přidejte tento kód:`log_errors=on`</span><span class="sxs-lookup"><span data-stu-id="0e1dc-130">In the file, add this code: `log_errors=on`</span></span>
9. <span data-ttu-id="0e1dc-131">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-131">Select **Save**.</span></span>
10. <span data-ttu-id="0e1dc-132">Vyberte ikonu tužky vedle **wp-config.php**.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-132">Select the pencil icon next to **wp-config.php**.</span></span>
11. <span data-ttu-id="0e1dc-133">Změňte text na následující kód:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-133">Change the text to the following code:</span></span>
   ```
   //Enable WP_DEBUG modedefine('WP_DEBUG', true);//Enable debug logging to /wp-content/debug.logdefine('WP_DEBUG_LOG', true);
   //Supress errors and warnings to screendefine('WP_DEBUG_DISPLAY', false);//Supress PHP errors to screenini_set('display_errors', 0);
   ```
12. <span data-ttu-id="0e1dc-134">Na portálu Azure, v nabídce webové aplikace restartování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-134">In the Azure portal, in the web app menu, restart your web app.</span></span>

<span data-ttu-id="0e1dc-135">Další informace najdete v tématu [protokoly chyb povolit WordPress](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-135">For more information, see [Enable WordPress error logs](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span></span>

## <a name="how-do-i-log-python-application-errors-in-apps-that-are-hosted-in-app-service"></a><span data-ttu-id="0e1dc-136">Jak se přihlásit chyb aplikací Python v aplikacích, které jsou hostované ve službě App Service?</span><span class="sxs-lookup"><span data-stu-id="0e1dc-136">How do I log Python application errors in apps that are hosted in App Service?</span></span>

<span data-ttu-id="0e1dc-137">K zachycení chyb aplikací Python:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-137">To capture Python application errors:</span></span>

1. <span data-ttu-id="0e1dc-138">Na portálu Azure ve vaší webové aplikaci, vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-138">In the Azure portal, in your web app, select **Settings**.</span></span>
2. <span data-ttu-id="0e1dc-139">Na **nastavení** vyberte **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-139">On the **Settings** tab, select **Application settings**.</span></span>
3. <span data-ttu-id="0e1dc-140">V části **nastavení aplikace**, zadejte následující dvojice klíč/hodnota:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-140">Under **App settings**, enter the following key/value pair:</span></span>
    * <span data-ttu-id="0e1dc-141">Klíč: WSGI_LOG</span><span class="sxs-lookup"><span data-stu-id="0e1dc-141">Key : WSGI_LOG</span></span>
    * <span data-ttu-id="0e1dc-142">Hodnota: D:\home\site\wwwroot\logs.txt (zadejte vaši volbu názvu souboru)</span><span class="sxs-lookup"><span data-stu-id="0e1dc-142">Value : D:\home\site\wwwroot\logs.txt (enter your choice of file name)</span></span>

<span data-ttu-id="0e1dc-143">Teď byste měli vidět chyby v souboru logs.txt ve složce wwwroot.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-143">You should now see errors in the logs.txt file in the wwwroot folder.</span></span>

## <a name="how-do-i-change-the-version-of-the-nodejs-application-that-is-hosted-in-app-service"></a><span data-ttu-id="0e1dc-144">Změna verze aplikace Node.js, který je hostován ve službě App Service</span><span class="sxs-lookup"><span data-stu-id="0e1dc-144">How do I change the version of the Node.js application that is hosted in App Service?</span></span>

<span data-ttu-id="0e1dc-145">Chcete-li změnit verzi aplikace Node.js, můžete použít jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-145">To change the version of the Node.js application, you can use one of the following options:</span></span>

*   <span data-ttu-id="0e1dc-146">Na portálu Azure použijte **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-146">In the Azure portal, use **App settings**.</span></span>
    1. <span data-ttu-id="0e1dc-147">Na portálu Azure přejděte do vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-147">In the Azure portal, go to your web app.</span></span>
    2. <span data-ttu-id="0e1dc-148">Na **nastavení** vyberte **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-148">On the **Settings** blade, select **Application settings**.</span></span>
    3. <span data-ttu-id="0e1dc-149">V **nastavení aplikace**, můžete zahrnout WEBSITE_NODE_DEFAULT_VERSION jako klíč a verze Node.js, které chcete používat jako hodnota.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-149">In **App settings**, you can include WEBSITE_NODE_DEFAULT_VERSION as the key, and the version of Node.js you want as the value.</span></span>
    4. <span data-ttu-id="0e1dc-150">Přejděte na vaše [Kudu konzoly](https://*yourwebsitename*.scm.azurewebsites.net).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-150">Go to your [Kudu console](https://*yourwebsitename*.scm.azurewebsites.net).</span></span>
    5. <span data-ttu-id="0e1dc-151">Kontrola verze Node.js, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-151">To check the Node.js version, enter the following command:</span></span>  
   ```
   node -v
   ```
*   <span data-ttu-id="0e1dc-152">Upravte soubor iisnode.yml.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-152">Modify the iisnode.yml file.</span></span> <span data-ttu-id="0e1dc-153">Změna verze Node.js v souboru iisnode.yml nastaví běhové prostředí pouze při, jemuž modul iisnode používá.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-153">Changing the Node.js version in the iisnode.yml file only sets the runtime environment that iisnode uses.</span></span> <span data-ttu-id="0e1dc-154">Vaše Kudu cmd a jiné dál používat verze Node.js, který je nastavený v **nastavení aplikace** na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-154">Your Kudu cmd and others still use the Node.js version that is set in **App settings** in the Azure portal.</span></span>

    <span data-ttu-id="0e1dc-155">Pokud chcete nastavit iisnode.yml ručně, vytvořte soubor iisnode.yml v kořenové složce aplikace.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-155">To set the iisnode.yml manually, create an iisnode.yml file in your app root folder.</span></span> <span data-ttu-id="0e1dc-156">V souboru zadejte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-156">In the file, include the following line:</span></span>
   ```
   nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
   ```
   
*   <span data-ttu-id="0e1dc-157">Nastavte soubor iisnode.yml pomocí package.json během nasazení zdroj ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-157">Set the iisnode.yml file by using package.json during source control deployment.</span></span>
    <span data-ttu-id="0e1dc-158">Proces řízení nasazení Azure zdroj zahrnuje následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-158">The Azure source control deployment process involves the following steps:</span></span>
    1. <span data-ttu-id="0e1dc-159">Obsah se přesune do webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-159">Moves content to the Azure web app.</span></span>
    2. <span data-ttu-id="0e1dc-160">Vytvoří výchozí skript nasazení, pokud není k dispozici (deploy.cmd, soubory .deployment) v kořenové složce webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-160">Creates a default deployment script, if there isn’t one (deploy.cmd, .deployment files) in the web app root folder.</span></span>
    3. <span data-ttu-id="0e1dc-161">Spustí skript nasazení, ve kterém se vytvoří soubor iisnode.yml Pokud zmínili verze Node.js v souboru package.json > modul`"engines": {"node": "5.9.1","npm": "3.7.3"}`</span><span class="sxs-lookup"><span data-stu-id="0e1dc-161">Runs a deployment script in which it creates an iisnode.yml file if you mention the Node.js version in the package.json file > engine `"engines": {"node": "5.9.1","npm": "3.7.3"}`</span></span>
    4. <span data-ttu-id="0e1dc-162">Soubor iisnode.yml má následující řádek kódu:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-162">The iisnode.yml file has the following line of code:</span></span>
        ```
        nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
        ```

## <a name="i-see-the-message-error-establishing-a-database-connection-in-my-wordpress-app-thats-hosted-in-app-service-how-do-i-troubleshoot-this"></a><span data-ttu-id="0e1dc-163">V mé aplikace WordPress, který je hostován ve službě App Service se zobrazují zpráva "Chyba při navazování připojení k databázi".</span><span class="sxs-lookup"><span data-stu-id="0e1dc-163">I see the message "Error establishing a database connection" in my WordPress app that's hosted in App Service.</span></span> <span data-ttu-id="0e1dc-164">Jak odstranit to?</span><span class="sxs-lookup"><span data-stu-id="0e1dc-164">How do I troubleshoot this?</span></span>

<span data-ttu-id="0e1dc-165">Pokud tato chyba se zobrazí v aplikaci WordPress v Azure, aby php_errors.log a debug.log, dokončení kroky popsané v [protokoly chyb povolit WordPress](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-165">If you see this error in your Azure WordPress app, to enable php_errors.log and debug.log, complete the steps detailed in [Enable WordPress error logs](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span></span>

<span data-ttu-id="0e1dc-166">Pokud jsou povolené protokoly, tomu chybu reprodukovat a v protokolech zjistit, zda jsou spuštěny mimo připojení:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-166">When the logs are enabled, reproduce the error, and then check the logs to see if you are running out of connections:</span></span>
```
[09-Oct-2015 00:03:13 UTC] PHP Warning: mysqli_real_connect(): (HY000/1226): User ‘abcdefghijk79' has exceeded the ‘max_user_connections’ resource (current value: 4) in D:\home\site\wwwroot\wp-includes\wp-db.php on line 1454
```

<span data-ttu-id="0e1dc-167">Pokud se zobrazí tato chyba v souborech debug.log nebo php_errors.log, vaše aplikace překračuje počet připojení.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-167">If you see this error in your debug.log or php_errors.log files, your app is exceeding the number of connections.</span></span> <span data-ttu-id="0e1dc-168">Pokud máte na ClearDB, ověřte počet připojení, které jsou k dispozici ve vaší [plán služby](https://www.cleardb.com/pricing.view).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-168">If you’re hosting on ClearDB, verify the number of connections that are available in your [service plan](https://www.cleardb.com/pricing.view).</span></span>

## <a name="how-do-i-debug-a-nodejs-app-thats-hosted-in-app-service"></a><span data-ttu-id="0e1dc-169">Jak mohu ladit aplikace Node.js, který je hostován ve službě App Service?</span><span class="sxs-lookup"><span data-stu-id="0e1dc-169">How do I debug a Node.js app that's hosted in App Service?</span></span>

1.  <span data-ttu-id="0e1dc-170">Přejděte na vaše [Kudu konzoly](https://*yourwebsitename*.scm.azurewebsites.net/DebugConsole).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-170">Go to your [Kudu console](https://*yourwebsitename*.scm.azurewebsites.net/DebugConsole).</span></span>
2.  <span data-ttu-id="0e1dc-171">Přejděte do složky protokolů aplikace (D:\home\LogFiles\Application).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-171">Go to your application logs folder (D:\home\LogFiles\Application).</span></span>
3.  <span data-ttu-id="0e1dc-172">V souboru logging_errors.txt kontrolovat obsahu.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-172">In the logging_errors.txt file, check for content.</span></span>

## <a name="how-do-i-install-native-python-modules-in-an-app-service-web-app-or-api-app"></a><span data-ttu-id="0e1dc-173">Instalace nativních modulů Python v webové aplikace App Service nebo aplikace API</span><span class="sxs-lookup"><span data-stu-id="0e1dc-173">How do I install native Python modules in an App Service web app or API app?</span></span>

<span data-ttu-id="0e1dc-174">Některé balíčky nemusí nainstalovat pomocí nástroje pip v Azure.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-174">Some packages might not install by using pip in Azure.</span></span> <span data-ttu-id="0e1dc-175">Balíček nemusí být k dispozici v indexu balíčků Pythonu nebo může být potřeba kompilátor (kompilátor není k dispozici v počítači, na kterém běží webová aplikace ve službě App Service).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-175">The package might not be available on the Python Package Index, or a compiler might be required (a compiler is not available on the computer that is running the web app in App Service).</span></span> <span data-ttu-id="0e1dc-176">Informace o instalaci nativních modulů ve webové aplikace aplikační služby a aplikace API najdete v tématu [moduly instalaci jazyka Python ve službě App Service](https://blogs.msdn.microsoft.com/azureossds/2015/06/29/install-native-python-modules-on-azure-web-apps-api-apps/).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-176">For information about installing native modules in App Service web apps and API apps, see [Install Python modules in App Service](https://blogs.msdn.microsoft.com/azureossds/2015/06/29/install-native-python-modules-on-azure-web-apps-api-apps/).</span></span>

## <a name="how-do-i-deploy-a-django-app-to-app-service-by-using-git-and-the-new-version-of-python"></a><span data-ttu-id="0e1dc-177">Jak nasadit aplikace Django do služby App Service pomocí Git a nová verze jazyka Python?</span><span class="sxs-lookup"><span data-stu-id="0e1dc-177">How do I deploy a Django app to App Service by using Git and the new version of Python?</span></span>

<span data-ttu-id="0e1dc-178">Informace o instalaci Django najdete v tématu [nasazení aplikace Django do služby App Service](https://blogs.msdn.microsoft.com/azureossds/2016/08/25/deploying-django-app-to-azure-app-services-using-git-and-new-version-of-python/).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-178">For information about installing Django, see [Deploying a Django app to App Service](https://blogs.msdn.microsoft.com/azureossds/2016/08/25/deploying-django-app-to-azure-app-services-using-git-and-new-version-of-python/).</span></span>

## <a name="where-are-the-tomcat-log-files-located"></a><span data-ttu-id="0e1dc-179">Kde jsou umístěny soubory protokolu Tomcat?</span><span class="sxs-lookup"><span data-stu-id="0e1dc-179">Where are the Tomcat log files located?</span></span>

<span data-ttu-id="0e1dc-180">Pro Azure Marketplace a vlastní nasazení:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-180">For Azure Marketplace and custom deployments:</span></span>

* <span data-ttu-id="0e1dc-181">Umístění složky: D:\home\site\wwwroot\bin\apache-tomcat-8.0.33\logs</span><span class="sxs-lookup"><span data-stu-id="0e1dc-181">Folder location: D:\home\site\wwwroot\bin\apache-tomcat-8.0.33\logs</span></span>
* <span data-ttu-id="0e1dc-182">Soubory, které vás zajímají:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-182">Files of interest:</span></span>
    * <span data-ttu-id="0e1dc-183">catalina. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="0e1dc-183">catalina.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="0e1dc-184">Správce hostitele. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="0e1dc-184">host-manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="0e1dc-185">localhost. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="0e1dc-185">localhost.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="0e1dc-186">správce. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="0e1dc-186">manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="0e1dc-187">site_access_log. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="0e1dc-187">site_access_log.*yyyy-mm-dd*.log</span></span>


<span data-ttu-id="0e1dc-188">Pro portál **nastavení aplikace** nasazení:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-188">For portal **App settings** deployments:</span></span>

* <span data-ttu-id="0e1dc-189">Umístění složky: D:\home\LogFiles</span><span class="sxs-lookup"><span data-stu-id="0e1dc-189">Folder location: D:\home\LogFiles</span></span>
* <span data-ttu-id="0e1dc-190">Soubory, které vás zajímají:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-190">Files of interest:</span></span>
    * <span data-ttu-id="0e1dc-191">catalina. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="0e1dc-191">catalina.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="0e1dc-192">Správce hostitele. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="0e1dc-192">host-manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="0e1dc-193">localhost. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="0e1dc-193">localhost.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="0e1dc-194">správce. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="0e1dc-194">manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="0e1dc-195">site_access_log. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="0e1dc-195">site_access_log.*yyyy-mm-dd*.log</span></span>

## <a name="how-do-i-troubleshoot-jdbc-driver-connection-errors"></a><span data-ttu-id="0e1dc-196">Jak odstranit chyb připojení ovladač JDBC?</span><span class="sxs-lookup"><span data-stu-id="0e1dc-196">How do I troubleshoot JDBC driver connection errors?</span></span>

<span data-ttu-id="0e1dc-197">Tuto zprávu můžete vidět v Tomcat protokolů:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-197">You might see the following message in your Tomcat logs:</span></span>

```
The web application[ROOT] registered the JDBC driver [com.mysql.jdbc.Driver] but failed to unregister it when the web application was stopped. To prevent a memory leak,the JDBC Driver has been forcibly unregistered
```

<span data-ttu-id="0e1dc-198">Chcete-li vyřešit chyby:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-198">To resolve the error:</span></span>

1. <span data-ttu-id="0e1dc-199">Odeberte soubor sqljdbc*.jar ze složky aplikace/lib.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-199">Remove the sqljdbc*.jar file from your app/lib folder.</span></span>
2. <span data-ttu-id="0e1dc-200">Pokud používáte vlastní webový server Tomcat nebo Azure Marketplace Tomcat, zkopírujte tento soubor .jar do složky lib Tomcat.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-200">If you are using the custom Tomcat or Azure Marketplace Tomcat web server, copy this .jar file to the Tomcat lib folder.</span></span>
3. <span data-ttu-id="0e1dc-201">Pokud chcete povolit Java z portálu Azure (vyberte **Java 1.8** > **Tomcat server**), zkopírovat soubor jar sqljdbc.* ve složce, která je paralelní do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-201">If you are enabling Java from the Azure portal (select **Java 1.8** > **Tomcat server**), copy the sqljdbc.* jar file in the folder that's parallel to your app.</span></span> <span data-ttu-id="0e1dc-202">Pak přidejte následující nastavení cesty pro třídy v souboru web.config:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-202">Then, add the following classpath setting to the web.config file:</span></span>

    ```
    <httpPlatform>
    <environmentVariables>
    <environmentVariablename ="JAVA_OPTS" value=" -Djava.net.preferIPv4Stack=true
    -Xms128M -classpath %CLASSPATH%;[Path to the sqljdbc*.jarfile]" />
    </environmentVariables>
    </httpPlatform>
    ```

## <a name="why-do-i-see-errors-when-i-attempt-to-copy-live-log-files"></a><span data-ttu-id="0e1dc-203">Proč se při pokusu o kopírování souborů za provozu protokolu zobrazí chyby?</span><span class="sxs-lookup"><span data-stu-id="0e1dc-203">Why do I see errors when I attempt to copy live log files?</span></span>

<span data-ttu-id="0e1dc-204">Pokud se pokusíte zkopírovat za provozu soubory protokolu pro aplikace v jazyce Java (například Tomcat), může se zobrazit tato chyba FTP:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-204">If you try to copy live log files for a Java app (for example, Tomcat), you might see this FTP error:</span></span>

```
Error transferring file [filename] Copying files from remote side failed.
    
The process cannot access the file because it is being used by another process.
```

<span data-ttu-id="0e1dc-205">Chybová zpráva se může lišit v závislosti na klient FTP.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-205">The error message might vary, depending on the FTP client.</span></span>

<span data-ttu-id="0e1dc-206">Všechny aplikace v jazyce Java mít potíže uzamčení.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-206">All Java apps have this locking issue.</span></span> <span data-ttu-id="0e1dc-207">Pouze Kudu podporuje stahování tohoto souboru, když aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-207">Only Kudu supports downloading this file while the app is running.</span></span>

<span data-ttu-id="0e1dc-208">Zastavení aplikace umožňuje k těmto souborům přístup FTP.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-208">Stopping the app allows FTP access to these files.</span></span>

<span data-ttu-id="0e1dc-209">Jiným řešením je zápis webové úlohy, který spouští podle plánu a zkopíruje tyto soubory do jiného adresáře.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-209">Another workaround is to write a WebJob that runs on a schedule and copies these files to a different directory.</span></span> <span data-ttu-id="0e1dc-210">Ukázkový projekt, najdete v článku [CopyLogsJob](https://github.com/kamilsykora/CopyLogsJob) projektu.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-210">For a sample project, see the [CopyLogsJob](https://github.com/kamilsykora/CopyLogsJob) project.</span></span>

## <a name="where-do-i-find-the-log-files-for-jetty"></a><span data-ttu-id="0e1dc-211">Kde soubory protokolu pro Jetty</span><span class="sxs-lookup"><span data-stu-id="0e1dc-211">Where do I find the log files for Jetty?</span></span>

<span data-ttu-id="0e1dc-212">Pro vlastní nasazení a Marketplace soubor protokolu je ve složce D:\home\site\wwwroot\bin\jetty-distribution-9.1.2.v20140210\logs.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-212">For Marketplace and custom deployments, the log file is in the D:\home\site\wwwroot\bin\jetty-distribution-9.1.2.v20140210\logs folder.</span></span> <span data-ttu-id="0e1dc-213">Všimněte si, že umístění složky závisí na verzi Jetty, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-213">Note that the folder location depends on the version of Jetty you are using.</span></span> <span data-ttu-id="0e1dc-214">Například cesta k dispozici zde je pro Jetty 9.1.2.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-214">For example, the path provided here is for Jetty 9.1.2.</span></span> <span data-ttu-id="0e1dc-215">Vyhledejte jetty_*YYYY_MM_DD*. stderrout.log.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-215">Look for jetty_*YYYY_MM_DD*.stderrout.log.</span></span>

<span data-ttu-id="0e1dc-216">Pro nasazení portálu nastavení aplikace soubor protokolu je v D:\home\LogFiles.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-216">For portal App Setting deployments, the log file is in D:\home\LogFiles.</span></span> <span data-ttu-id="0e1dc-217">Vyhledejte jetty_*YYYY_MM_DD*. stderrout.log</span><span class="sxs-lookup"><span data-stu-id="0e1dc-217">Look for jetty_*YYYY_MM_DD*.stderrout.log</span></span>

## <a name="can-i-send-email-from-my-azure-web-app"></a><span data-ttu-id="0e1dc-218">Můžete odesílat e-maily z Azure webová aplikace?</span><span class="sxs-lookup"><span data-stu-id="0e1dc-218">Can I send email from my Azure web app?</span></span>

<span data-ttu-id="0e1dc-219">Služby App Service nemá předdefinované e-mailové funkce.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-219">App Service doesn't have a built-in email feature.</span></span> <span data-ttu-id="0e1dc-220">Některé funkční alternativami pro odesílání e-mailu z vaší aplikace, najdete v tématu to [Stack Overflow diskusní](http://stackoverflow.com/questions/17666161/sending-email-from-azure).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-220">For some good alternatives for sending email from your app, see this [Stack Overflow discussion](http://stackoverflow.com/questions/17666161/sending-email-from-azure).</span></span>

## <a name="why-does-my-wordpress-site-redirect-to-another-url"></a><span data-ttu-id="0e1dc-221">Proč můj web WordPress přesměrovat na jinou adresu URL?</span><span class="sxs-lookup"><span data-stu-id="0e1dc-221">Why does my WordPress site redirect to another URL?</span></span>

<span data-ttu-id="0e1dc-222">Pokud jste nedávno migrovali do Azure, WordPress může přesměrovat na původní adresu URL domény.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-222">If you have recently migrated to Azure, WordPress might redirect to the old domain URL.</span></span> <span data-ttu-id="0e1dc-223">To je způsobeno nastavení v databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-223">This is caused by a setting in the MySQL database.</span></span>

<span data-ttu-id="0e1dc-224">WordPress kamarád + je rozšíření lokality Azure, které můžete použít k aktualizaci adresu URL pro přesměrování přímo v databázi.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-224">WordPress Buddy+ is an Azure Site Extension that you can use to update the redirection URL directly in the database.</span></span> <span data-ttu-id="0e1dc-225">Další informace o používání WordPress kamarád + najdete v tématu [WordPress nástroje a migraci databáze MySQL s WordPress kamarád +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-225">For more information about using WordPress Buddy+, see [WordPress tools and MySQL migration with WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span></span>

<span data-ttu-id="0e1dc-226">Případně, pokud chcete ručně aktualizovat přesměrování adresu URL pomocí dotazů SQL nebo PHPMyAdmin, najdete v části [WordPress: přesměrování na adresy URL je chybný](https://blogs.msdn.microsoft.com/azureossds/2016/07/12/wordpress-redirecting-to-wrong-url/).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-226">Alternatively, if you prefer to manually update the redirection URL by using SQL queries or PHPMyAdmin, see [WordPress: Redirecting to wrong URL](https://blogs.msdn.microsoft.com/azureossds/2016/07/12/wordpress-redirecting-to-wrong-url/).</span></span>

## <a name="how-do-i-change-my-wordpress-sign-in-password"></a><span data-ttu-id="0e1dc-227">Změna hesla přihlášení WordPress</span><span class="sxs-lookup"><span data-stu-id="0e1dc-227">How do I change my WordPress sign-in password?</span></span>

<span data-ttu-id="0e1dc-228">Pokud jste WordPress přihlašovací heslo zapomněli, můžete WordPress kamarád + jej aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-228">If you have forgotten your WordPress sign-in password, you can use WordPress Buddy+ to update it.</span></span> <span data-ttu-id="0e1dc-229">Pokud chcete resetovat heslo, nainstalujte příponou webu WordPress kamarád + Azure a potom postupujte podle pokynů popsaných v [WordPress nástroje a migraci databáze MySQL s WordPress kamarád +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-229">To reset your password, install the WordPress Buddy+ Azure Site Extension, and then complete the steps described in [WordPress tools and MySQL migration with WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span></span>

## <a name="i-cant-sign-in-to-wordpress-how-do-i-resolve-this"></a><span data-ttu-id="0e1dc-230">Nemůžete se přihlásit k WordPress.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-230">I can't sign in to WordPress.</span></span> <span data-ttu-id="0e1dc-231">Jak to lze vyřešit?</span><span class="sxs-lookup"><span data-stu-id="0e1dc-231">How do I resolve this?</span></span>

<span data-ttu-id="0e1dc-232">Pokud se přistihnete zamknout z WordPress po instalaci nedávno o modul plug-in, můžete mít vadný modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-232">If you find yourself locked out of WordPress after recently installing a plugin, you might have a faulty plugin.</span></span> <span data-ttu-id="0e1dc-233">WordPress kamarád + je rozšíření webu Azure, které vám pomůžou zakažte moduly plug-in v WordPress.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-233">WordPress Buddy+ is an Azure Site Extension that can help you disable plugins in WordPress.</span></span> <span data-ttu-id="0e1dc-234">Další informace najdete v tématu [WordPress nástroje a migraci databáze MySQL s WordPress kamarád +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-234">For more information, see [WordPress tools and MySQL migration with WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span></span>

## <a name="how-do-i-migrate-my-wordpress-database"></a><span data-ttu-id="0e1dc-235">Jak migraci databáze WordPress?</span><span class="sxs-lookup"><span data-stu-id="0e1dc-235">How do I migrate my WordPress database?</span></span>

<span data-ttu-id="0e1dc-236">Máte několik možností pro migraci databáze MySQL, která je připojena k webu WordPress:</span><span class="sxs-lookup"><span data-stu-id="0e1dc-236">You have multiple options for migrating the MySQL database that's connected to your WordPress website:</span></span>

* <span data-ttu-id="0e1dc-237">Vývojáři: Pomocí [příkazový řádek nebo PHPMyAdmin](https://blogs.msdn.microsoft.com/azureossds/2016/03/02/migrating-data-between-mysql-databases-using-kudu-console-azure-app-service/)</span><span class="sxs-lookup"><span data-stu-id="0e1dc-237">Developers: Use the [command prompt or PHPMyAdmin](https://blogs.msdn.microsoft.com/azureossds/2016/03/02/migrating-data-between-mysql-databases-using-kudu-console-azure-app-service/)</span></span>
* <span data-ttu-id="0e1dc-238">Jiný vývojáři: Použít [WordPress kamarád +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/)</span><span class="sxs-lookup"><span data-stu-id="0e1dc-238">Non-developers: Use [WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/)</span></span>

## <a name="how-do-i-help-make-wordpress-more-secure"></a><span data-ttu-id="0e1dc-239">Jak pomoci, zabezpečit WordPress?</span><span class="sxs-lookup"><span data-stu-id="0e1dc-239">How do I help make WordPress more secure?</span></span>

<span data-ttu-id="0e1dc-240">Další informace o doporučeném zabezpečení WordPress najdete v tématu [osvědčené postupy pro zabezpečení WordPress v Azure](https://blogs.msdn.microsoft.com/azureossds/2016/12/26/best-practices-for-wordpress-security-on-azure/).</span><span class="sxs-lookup"><span data-stu-id="0e1dc-240">To learn about security best practices for WordPress, see [Best practices for WordPress security in Azure](https://blogs.msdn.microsoft.com/azureossds/2016/12/26/best-practices-for-wordpress-security-on-azure/).</span></span>

## <a name="i-am-trying-to-use-phpmyadmin-and-i-see-the-message-access-denied-how-do-i-resolve-this"></a><span data-ttu-id="0e1dc-241">Chci používat PHPMyAdmin a zobrazit zpráva "Přístup byl odepřen."</span><span class="sxs-lookup"><span data-stu-id="0e1dc-241">I am trying to use PHPMyAdmin, and I see the message “Access denied.”</span></span> <span data-ttu-id="0e1dc-242">Jak to lze vyřešit?</span><span class="sxs-lookup"><span data-stu-id="0e1dc-242">How do I resolve this?</span></span>

<span data-ttu-id="0e1dc-243">Tomuto problému může dojít, pokud funkci v aplikaci MySQL ještě neběží v této instanci služby App Service.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-243">You might experience this issue if the MySQL in-app feature isn't running yet in this App Service instance.</span></span> <span data-ttu-id="0e1dc-244">Chcete-li vyřešit tento problém, pokusu o přístup k webu.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-244">To resolve the issue, try to access your website.</span></span> <span data-ttu-id="0e1dc-245">Tím se spustí požadované procesů, včetně proces MySQL v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-245">This starts the required processes, including the MySQL in-app process.</span></span> <span data-ttu-id="0e1dc-246">K ověřte, zda je spuštěna MySQL v aplikaci, v Průzkumníka procesů, zajistěte, že mysqld.exe je uvedena v procesy.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-246">To verify that MySQL in-app is running, in Process Explorer, ensure that mysqld.exe is listed in the processes.</span></span>

<span data-ttu-id="0e1dc-247">Po zajištění, že je spuštěn tento MySQL v aplikaci, zkuste použít PHPMyAdmin.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-247">After you ensure that MySQL in-app is running, try to use PHPMyAdmin.</span></span>

## <a name="i-get-an-http-403-error-when-i-try-to-import-or-export-my-mysql-in-app-database-by-using-phpmyadmin-how-do-i-resolve-this"></a><span data-ttu-id="0e1dc-248">Při pokusu o import nebo export databáze MySQL v aplikaci s použitím PHPMyadmin zobrazila chybu HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-248">I get an HTTP 403 error when I try to import or export my MySQL in-app database by using PHPMyadmin.</span></span> <span data-ttu-id="0e1dc-249">Jak to lze vyřešit?</span><span class="sxs-lookup"><span data-stu-id="0e1dc-249">How do I resolve this?</span></span>

<span data-ttu-id="0e1dc-250">Pokud používáte starší verzi Chrome, může se jednat známého problému.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-250">If you are using an older version of Chrome, you might be experiencing a known bug.</span></span> <span data-ttu-id="0e1dc-251">Chcete-li problém vyřešit, upgradujte na novější verzi Chrome.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-251">To resolve the issue, upgrade to a newer version of Chrome.</span></span> <span data-ttu-id="0e1dc-252">Zkuste taky pomocí jiného prohlížeče, jako je Internet Explorer nebo Edge, kde problému nedojde.</span><span class="sxs-lookup"><span data-stu-id="0e1dc-252">Also try using a different browser, like Internet Explorer or Edge, where the issue does not occur.</span></span>
