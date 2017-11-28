---
title: "technologie aaaOpen-source časté otázky k Azure webové aplikace | Microsoft Docs"
description: "Získejte odpovědi toofrequently kladené dotazy týkající se technologie open source v hello funkce Web Apps služby Azure App Service."
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
ms.openlocfilehash: 35cff4f322859d25972747cf55aa7c4316381a51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="open-source-technologies-faqs-for-web-apps-in-azure"></a><span data-ttu-id="2f349-103">Technologie Open source nejčastější dotazy pro webové aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="2f349-103">Open-source technologies FAQs for Web Apps in Azure</span></span>

<span data-ttu-id="2f349-104">Tento článek obsahuje odpovědi toofrequently dotazy (FAQ) o problémech s nástrojem technologie open source pro hello [funkce Web Apps služby Azure App Service](https://azure.microsoft.com/services/app-service/web/).</span><span class="sxs-lookup"><span data-stu-id="2f349-104">This article has answers toofrequently asked questions (FAQs) about issues with open-source technologies for hello [Web Apps feature of Azure App Service](https://azure.microsoft.com/services/app-service/web/).</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-cleardb-database-is-down-how-do-i-resolve-this"></a><span data-ttu-id="2f349-105">Databázi ClearDB je mimo provoz.</span><span class="sxs-lookup"><span data-stu-id="2f349-105">My ClearDB database is down.</span></span> <span data-ttu-id="2f349-106">Jak to lze vyřešit?</span><span class="sxs-lookup"><span data-stu-id="2f349-106">How do I resolve this?</span></span>

<span data-ttu-id="2f349-107">Databáze s problémy, kontaktujte [ClearDB podporu](https://www.cleardb.com/developers/help/support).</span><span class="sxs-lookup"><span data-stu-id="2f349-107">For database-related issues, contact [ClearDB support](https://www.cleardb.com/developers/help/support).</span></span> 

<span data-ttu-id="2f349-108">Odpovědi toocommon dotazy týkající se ClearDB, najdete v části [cleardb – nejčastější dotazy k](https://docs.microsoft.com/azure/store-cleardb-faq/).</span><span class="sxs-lookup"><span data-stu-id="2f349-108">For answers toocommon questions about ClearDB, see [ClearDB FAQs](https://docs.microsoft.com/azure/store-cleardb-faq/).</span></span>

## <a name="why-isnt-my-cleardb-database-listed-in-hello-portal"></a><span data-ttu-id="2f349-109">Proč není uvedená Moje databázi ClearDB portálu hello?</span><span class="sxs-lookup"><span data-stu-id="2f349-109">Why isn't my ClearDB database listed in hello portal?</span></span>

<span data-ttu-id="2f349-110">Když vytvoříte databázi ClearDB v hello [portál Azure](http://portal.azure.com/), databáze hello se nezobrazí v hello [portál Azure classic](http://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="2f349-110">If you create a ClearDB database in hello [Azure portal](http://portal.azure.com/), hello database doesn't appear in hello [Azure classic portal](http://manage.windowsazure.com/).</span></span> <span data-ttu-id="2f349-111">toowork tomuto problému vyhnout, můžete ručně propojit vaší databáze toohello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2f349-111">toowork around this, you can manually link your database toohello web app.</span></span>

<span data-ttu-id="2f349-112">Podobně platí když vytvoříte databázi ClearDB v hello [portál Azure classic](http://manage.windowsazure.com/), neuvidíte vaše databáze v hello [portál Azure](http://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="2f349-112">Similarly, if you create a ClearDB database in hello [Azure classic portal](http://manage.windowsazure.com/),  you won't see your database in hello [Azure portal](http://portal.azure.com/).</span></span> <span data-ttu-id="2f349-113">V takovém případě je k dispozici žádné alternativní řešení.</span><span class="sxs-lookup"><span data-stu-id="2f349-113">In this case, no workaround is available.</span></span> 

<span data-ttu-id="2f349-114">Další informace najdete v tématu [nejčastější dotazy pro databáze ClearDB MySQL službou Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span><span class="sxs-lookup"><span data-stu-id="2f349-114">For more information, see [FAQs for ClearDB MySQL databases with Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span></span>

## <a name="why-wasnt-my-cleardb-database-migrated-during-my-subscription-migration"></a><span data-ttu-id="2f349-115">Proč nebyl migrován databázi ClearDB během migrace Moje předplatné?</span><span class="sxs-lookup"><span data-stu-id="2f349-115">Why wasn't my ClearDB database migrated during my subscription migration?</span></span>

<span data-ttu-id="2f349-116">Když provádíte migraci prostředků ve předplatných, platí určitá omezení.</span><span class="sxs-lookup"><span data-stu-id="2f349-116">When you perform resource migration across subscriptions, some limitations apply.</span></span> <span data-ttu-id="2f349-117">Databáze ClearDB MySQL je služba třetích stran a nemigruje během migrace předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="2f349-117">A ClearDB MySQL database is a third-party service and is not migrated during an Azure subscription migration.</span></span>

<span data-ttu-id="2f349-118">Pokud nespravujete hello migraci databáze MySQL před zahájením migrace vašich prostředků Azure, může být databáze MySQL cleardb – není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="2f349-118">If you don't manage hello migration of your MySQL database before you migrate your Azure resources, your ClearDB MySQL database might be unavailable.</span></span> <span data-ttu-id="2f349-119">tooavoid to nejdřív ručně migrovat databázi ClearDB a potom migrovat hello předplatné Azure pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2f349-119">tooavoid this, first, manually migrate your ClearDB database, and then migrate hello Azure subscription for your web app.</span></span>

<span data-ttu-id="2f349-120">Další informace najdete v tématu [nejčastější dotazy pro databáze ClearDB MySQL službou Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span><span class="sxs-lookup"><span data-stu-id="2f349-120">For more information, see [FAQs for ClearDB MySQL databases with Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span></span>

## <a name="how-do-i-turn-on-php-logging-tootroubleshoot-php-issues"></a><span data-ttu-id="2f349-121">Jak zapnout na PHP protokolování tootroubleshoot PHP problémy?</span><span class="sxs-lookup"><span data-stu-id="2f349-121">How do I turn on PHP logging tootroubleshoot PHP issues?</span></span>

<span data-ttu-id="2f349-122">tooturn na PHP protokolování:</span><span class="sxs-lookup"><span data-stu-id="2f349-122">tooturn on PHP logging:</span></span>

1. <span data-ttu-id="2f349-123">Přihlaste se tooyour [Kudu webu](https://*yourwebsitename*.scm.azurewebsites.net).</span><span class="sxs-lookup"><span data-stu-id="2f349-123">Sign in tooyour [Kudu website](https://*yourwebsitename*.scm.azurewebsites.net).</span></span>
2. <span data-ttu-id="2f349-124">V horní nabídce hello, vyberte **ladění konzoly** > **CMD**.</span><span class="sxs-lookup"><span data-stu-id="2f349-124">In hello top menu, select **Debug Console** > **CMD**.</span></span>
3. <span data-ttu-id="2f349-125">Vyberte hello **lokality** složky.</span><span class="sxs-lookup"><span data-stu-id="2f349-125">Select hello **Site** folder.</span></span>
4. <span data-ttu-id="2f349-126">Vyberte hello **wwwroot** složky.</span><span class="sxs-lookup"><span data-stu-id="2f349-126">Select hello **wwwroot** folder.</span></span>
5. <span data-ttu-id="2f349-127">Vyberte hello  **+**  ikonu a potom vyberte **nový soubor**.</span><span class="sxs-lookup"><span data-stu-id="2f349-127">Select hello **+** icon, and then select **New File**.</span></span>
6. <span data-ttu-id="2f349-128">Nastaví název souboru hello příliš**. user.ini**.</span><span class="sxs-lookup"><span data-stu-id="2f349-128">Set hello file name too**.user.ini**.</span></span>
7. <span data-ttu-id="2f349-129">Vyberte ikonu tužky hello vedle příliš**. user.ini**.</span><span class="sxs-lookup"><span data-stu-id="2f349-129">Select hello pencil icon next too**.user.ini**.</span></span>
8. <span data-ttu-id="2f349-130">V souboru hello přidejte tento kód:`log_errors=on`</span><span class="sxs-lookup"><span data-stu-id="2f349-130">In hello file, add this code: `log_errors=on`</span></span>
9. <span data-ttu-id="2f349-131">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2f349-131">Select **Save**.</span></span>
10. <span data-ttu-id="2f349-132">Vyberte ikonu tužky hello vedle příliš**wp-config.php**.</span><span class="sxs-lookup"><span data-stu-id="2f349-132">Select hello pencil icon next too**wp-config.php**.</span></span>
11. <span data-ttu-id="2f349-133">Změna toohello textu hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="2f349-133">Change hello text toohello following code:</span></span>
   ```
   //Enable WP_DEBUG modedefine('WP_DEBUG', true);//Enable debug logging too/wp-content/debug.logdefine('WP_DEBUG_LOG', true);
   //Supress errors and warnings tooscreendefine('WP_DEBUG_DISPLAY', false);//Supress PHP errors tooscreenini_set('display_errors', 0);
   ```
12. <span data-ttu-id="2f349-134">V hello portál Azure, v nabídce webové aplikace hello restartování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2f349-134">In hello Azure portal, in hello web app menu, restart your web app.</span></span>

<span data-ttu-id="2f349-135">Další informace najdete v tématu [protokoly chyb povolit WordPress](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span><span class="sxs-lookup"><span data-stu-id="2f349-135">For more information, see [Enable WordPress error logs](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span></span>

## <a name="how-do-i-log-python-application-errors-in-apps-that-are-hosted-in-app-service"></a><span data-ttu-id="2f349-136">Jak se přihlásit chyb aplikací Python v aplikacích, které jsou hostované ve službě App Service?</span><span class="sxs-lookup"><span data-stu-id="2f349-136">How do I log Python application errors in apps that are hosted in App Service?</span></span>

<span data-ttu-id="2f349-137">chyb aplikací Python toocapture:</span><span class="sxs-lookup"><span data-stu-id="2f349-137">toocapture Python application errors:</span></span>

1. <span data-ttu-id="2f349-138">V hello portál Azure, ve vaší webové aplikaci, vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="2f349-138">In hello Azure portal, in your web app, select **Settings**.</span></span>
2. <span data-ttu-id="2f349-139">Na hello **nastavení** vyberte **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2f349-139">On hello **Settings** tab, select **Application settings**.</span></span>
3. <span data-ttu-id="2f349-140">V části **nastavení aplikace**, zadejte následující dvojice klíč/hodnota hello:</span><span class="sxs-lookup"><span data-stu-id="2f349-140">Under **App settings**, enter hello following key/value pair:</span></span>
    * <span data-ttu-id="2f349-141">Klíč: WSGI_LOG</span><span class="sxs-lookup"><span data-stu-id="2f349-141">Key : WSGI_LOG</span></span>
    * <span data-ttu-id="2f349-142">Hodnota: D:\home\site\wwwroot\logs.txt (zadejte vaši volbu názvu souboru)</span><span class="sxs-lookup"><span data-stu-id="2f349-142">Value : D:\home\site\wwwroot\logs.txt (enter your choice of file name)</span></span>

<span data-ttu-id="2f349-143">Teď byste měli vidět chyby v souboru logs.txt hello ve složce wwwroot hello.</span><span class="sxs-lookup"><span data-stu-id="2f349-143">You should now see errors in hello logs.txt file in hello wwwroot folder.</span></span>

## <a name="how-do-i-change-hello-version-of-hello-nodejs-application-that-is-hosted-in-app-service"></a><span data-ttu-id="2f349-144">Změna hello verzi hello aplikace Node.js, který je hostován ve službě App Service</span><span class="sxs-lookup"><span data-stu-id="2f349-144">How do I change hello version of hello Node.js application that is hosted in App Service?</span></span>

<span data-ttu-id="2f349-145">verze hello toochange hello aplikace Node.js, můžete použít jednu z hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="2f349-145">toochange hello version of hello Node.js application, you can use one of hello following options:</span></span>

*   <span data-ttu-id="2f349-146">V hello portálu Azure, použijte **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2f349-146">In hello Azure portal, use **App settings**.</span></span>
    1. <span data-ttu-id="2f349-147">V hello portálu Azure přejděte tooyour webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2f349-147">In hello Azure portal, go tooyour web app.</span></span>
    2. <span data-ttu-id="2f349-148">Na hello **nastavení** vyberte **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2f349-148">On hello **Settings** blade, select **Application settings**.</span></span>
    3. <span data-ttu-id="2f349-149">V **nastavení aplikace**, můžete zahrnout WEBSITE_NODE_DEFAULT_VERSION jako klíč hello a hello verze Node.js chcete používat jako hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="2f349-149">In **App settings**, you can include WEBSITE_NODE_DEFAULT_VERSION as hello key, and hello version of Node.js you want as hello value.</span></span>
    4. <span data-ttu-id="2f349-150">Přejděte tooyour [Kudu konzoly](https://*yourwebsitename*.scm.azurewebsites.net).</span><span class="sxs-lookup"><span data-stu-id="2f349-150">Go tooyour [Kudu console](https://*yourwebsitename*.scm.azurewebsites.net).</span></span>
    5. <span data-ttu-id="2f349-151">toocheck hello verze Node.js, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="2f349-151">toocheck hello Node.js version, enter hello following command:</span></span>  
   ```
   node -v
   ```
*   <span data-ttu-id="2f349-152">Upravte soubor iisnode.yml hello.</span><span class="sxs-lookup"><span data-stu-id="2f349-152">Modify hello iisnode.yml file.</span></span> <span data-ttu-id="2f349-153">Změna verze Node.js hello v souboru iisnode.yml hello jenom nastaví hello běhového prostředí, jemuž modul iisnode používá.</span><span class="sxs-lookup"><span data-stu-id="2f349-153">Changing hello Node.js version in hello iisnode.yml file only sets hello runtime environment that iisnode uses.</span></span> <span data-ttu-id="2f349-154">Vaše Kudu cmd a jiné dál používat verze Node.js hello, který je nastavený v **nastavení aplikace** v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2f349-154">Your Kudu cmd and others still use hello Node.js version that is set in **App settings** in hello Azure portal.</span></span>

    <span data-ttu-id="2f349-155">tooset hello iisnode.yml ručně, vytvořte soubor iisnode.yml v kořenové složce aplikace.</span><span class="sxs-lookup"><span data-stu-id="2f349-155">tooset hello iisnode.yml manually, create an iisnode.yml file in your app root folder.</span></span> <span data-ttu-id="2f349-156">V souboru hello patří hello následující řádek:</span><span class="sxs-lookup"><span data-stu-id="2f349-156">In hello file, include hello following line:</span></span>
   ```
   nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
   ```
   
*   <span data-ttu-id="2f349-157">Nastavit soubor iisnode.yml poskytovaný rozhraním hello pomocí package.json během nasazení zdroj ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="2f349-157">Set hello iisnode.yml file by using package.json during source control deployment.</span></span>
    <span data-ttu-id="2f349-158">Hello proces řízení nasazení Azure zdroj zahrnuje hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2f349-158">hello Azure source control deployment process involves hello following steps:</span></span>
    1. <span data-ttu-id="2f349-159">Přesune obsahu toohello webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="2f349-159">Moves content toohello Azure web app.</span></span>
    2. <span data-ttu-id="2f349-160">Vytvoří výchozí skript nasazení, pokud není k dispozici (deploy.cmd, soubory .deployment) hello webové aplikace kořenové složky.</span><span class="sxs-lookup"><span data-stu-id="2f349-160">Creates a default deployment script, if there isn’t one (deploy.cmd, .deployment files) in hello web app root folder.</span></span>
    3. <span data-ttu-id="2f349-161">Spustí skript nasazení, ve kterém se vytvoří soubor iisnode.yml Pokud zmínili verze Node.js hello v souboru package.json hello > modul`"engines": {"node": "5.9.1","npm": "3.7.3"}`</span><span class="sxs-lookup"><span data-stu-id="2f349-161">Runs a deployment script in which it creates an iisnode.yml file if you mention hello Node.js version in hello package.json file > engine `"engines": {"node": "5.9.1","npm": "3.7.3"}`</span></span>
    4. <span data-ttu-id="2f349-162">soubor iisnode.yml poskytovaný rozhraním Hello má hello následující řádek kódu:</span><span class="sxs-lookup"><span data-stu-id="2f349-162">hello iisnode.yml file has hello following line of code:</span></span>
        ```
        nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
        ```

## <a name="i-see-hello-message-error-establishing-a-database-connection-in-my-wordpress-app-thats-hosted-in-app-service-how-do-i-troubleshoot-this"></a><span data-ttu-id="2f349-163">V mé aplikace WordPress, který je hostován ve službě App Service se zobrazují hello zpráva "Chyba při navazování připojení k databázi".</span><span class="sxs-lookup"><span data-stu-id="2f349-163">I see hello message "Error establishing a database connection" in my WordPress app that's hosted in App Service.</span></span> <span data-ttu-id="2f349-164">Jak odstranit to?</span><span class="sxs-lookup"><span data-stu-id="2f349-164">How do I troubleshoot this?</span></span>

<span data-ttu-id="2f349-165">Pokud se zobrazí tato chyba v aplikaci Azure WordPress, tooenable php_errors.log a debug.log, dokončení hello kroky popsané v [protokoly chyb povolit WordPress](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span><span class="sxs-lookup"><span data-stu-id="2f349-165">If you see this error in your Azure WordPress app, tooenable php_errors.log and debug.log, complete hello steps detailed in [Enable WordPress error logs](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span></span>

<span data-ttu-id="2f349-166">Pokud jsou povolené protokoly hello, reprodukujte hello chyba a potom zkontrolujte protokoly toosee hello, pokud používáte mimo připojení:</span><span class="sxs-lookup"><span data-stu-id="2f349-166">When hello logs are enabled, reproduce hello error, and then check hello logs toosee if you are running out of connections:</span></span>
```
[09-Oct-2015 00:03:13 UTC] PHP Warning: mysqli_real_connect(): (HY000/1226): User ‘abcdefghijk79' has exceeded hello ‘max_user_connections’ resource (current value: 4) in D:\home\site\wwwroot\wp-includes\wp-db.php on line 1454
```

<span data-ttu-id="2f349-167">Pokud se zobrazí tato chyba v souborech debug.log nebo php_errors.log, vaše aplikace přesahuje hello počet připojení.</span><span class="sxs-lookup"><span data-stu-id="2f349-167">If you see this error in your debug.log or php_errors.log files, your app is exceeding hello number of connections.</span></span> <span data-ttu-id="2f349-168">Pokud máte na ClearDB, ověřte hello počet připojení, které jsou k dispozici ve vaší [plán služby](https://www.cleardb.com/pricing.view).</span><span class="sxs-lookup"><span data-stu-id="2f349-168">If you’re hosting on ClearDB, verify hello number of connections that are available in your [service plan](https://www.cleardb.com/pricing.view).</span></span>

## <a name="how-do-i-debug-a-nodejs-app-thats-hosted-in-app-service"></a><span data-ttu-id="2f349-169">Jak mohu ladit aplikace Node.js, který je hostován ve službě App Service?</span><span class="sxs-lookup"><span data-stu-id="2f349-169">How do I debug a Node.js app that's hosted in App Service?</span></span>

1.  <span data-ttu-id="2f349-170">Přejděte tooyour [Kudu konzoly](https://*yourwebsitename*.scm.azurewebsites.net/DebugConsole).</span><span class="sxs-lookup"><span data-stu-id="2f349-170">Go tooyour [Kudu console](https://*yourwebsitename*.scm.azurewebsites.net/DebugConsole).</span></span>
2.  <span data-ttu-id="2f349-171">Protokoly složce aplikace přejděte tooyour (D:\home\LogFiles\Application).</span><span class="sxs-lookup"><span data-stu-id="2f349-171">Go tooyour application logs folder (D:\home\LogFiles\Application).</span></span>
3.  <span data-ttu-id="2f349-172">V souboru logging_errors.txt hello kontrolovat obsahu.</span><span class="sxs-lookup"><span data-stu-id="2f349-172">In hello logging_errors.txt file, check for content.</span></span>

## <a name="how-do-i-install-native-python-modules-in-an-app-service-web-app-or-api-app"></a><span data-ttu-id="2f349-173">Instalace nativních modulů Python v webové aplikace App Service nebo aplikace API</span><span class="sxs-lookup"><span data-stu-id="2f349-173">How do I install native Python modules in an App Service web app or API app?</span></span>

<span data-ttu-id="2f349-174">Některé balíčky nemusí nainstalovat pomocí nástroje pip v Azure.</span><span class="sxs-lookup"><span data-stu-id="2f349-174">Some packages might not install by using pip in Azure.</span></span> <span data-ttu-id="2f349-175">balíček Hello nemusí být k dispozici na hello indexu balíčků Pythonu nebo může být potřeba kompilátor (kompilátor není k dispozici na hello počítače se systémem hello webové aplikace ve službě App Service).</span><span class="sxs-lookup"><span data-stu-id="2f349-175">hello package might not be available on hello Python Package Index, or a compiler might be required (a compiler is not available on hello computer that is running hello web app in App Service).</span></span> <span data-ttu-id="2f349-176">Informace o instalaci nativních modulů ve webové aplikace aplikační služby a aplikace API najdete v tématu [moduly instalaci jazyka Python ve službě App Service](https://blogs.msdn.microsoft.com/azureossds/2015/06/29/install-native-python-modules-on-azure-web-apps-api-apps/).</span><span class="sxs-lookup"><span data-stu-id="2f349-176">For information about installing native modules in App Service web apps and API apps, see [Install Python modules in App Service](https://blogs.msdn.microsoft.com/azureossds/2015/06/29/install-native-python-modules-on-azure-web-apps-api-apps/).</span></span>

## <a name="how-do-i-deploy-a-django-app-tooapp-service-by-using-git-and-hello-new-version-of-python"></a><span data-ttu-id="2f349-177">Nasazení tooApp aplikace Django služby pomocí Git a hello novou verzi jazyka Python</span><span class="sxs-lookup"><span data-stu-id="2f349-177">How do I deploy a Django app tooApp Service by using Git and hello new version of Python?</span></span>

<span data-ttu-id="2f349-178">Informace o instalaci Django najdete v tématu [nasazení tooApp aplikace Django služby](https://blogs.msdn.microsoft.com/azureossds/2016/08/25/deploying-django-app-to-azure-app-services-using-git-and-new-version-of-python/).</span><span class="sxs-lookup"><span data-stu-id="2f349-178">For information about installing Django, see [Deploying a Django app tooApp Service](https://blogs.msdn.microsoft.com/azureossds/2016/08/25/deploying-django-app-to-azure-app-services-using-git-and-new-version-of-python/).</span></span>

## <a name="where-are-hello-tomcat-log-files-located"></a><span data-ttu-id="2f349-179">Kde jsou umístěny soubory pro protokolu hello Tomcat?</span><span class="sxs-lookup"><span data-stu-id="2f349-179">Where are hello Tomcat log files located?</span></span>

<span data-ttu-id="2f349-180">Pro Azure Marketplace a vlastní nasazení:</span><span class="sxs-lookup"><span data-stu-id="2f349-180">For Azure Marketplace and custom deployments:</span></span>

* <span data-ttu-id="2f349-181">Umístění složky: D:\home\site\wwwroot\bin\apache-tomcat-8.0.33\logs</span><span class="sxs-lookup"><span data-stu-id="2f349-181">Folder location: D:\home\site\wwwroot\bin\apache-tomcat-8.0.33\logs</span></span>
* <span data-ttu-id="2f349-182">Soubory, které vás zajímají:</span><span class="sxs-lookup"><span data-stu-id="2f349-182">Files of interest:</span></span>
    * <span data-ttu-id="2f349-183">catalina. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="2f349-183">catalina.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="2f349-184">Správce hostitele. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="2f349-184">host-manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="2f349-185">localhost. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="2f349-185">localhost.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="2f349-186">správce. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="2f349-186">manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="2f349-187">site_access_log. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="2f349-187">site_access_log.*yyyy-mm-dd*.log</span></span>


<span data-ttu-id="2f349-188">Pro portál **nastavení aplikace** nasazení:</span><span class="sxs-lookup"><span data-stu-id="2f349-188">For portal **App settings** deployments:</span></span>

* <span data-ttu-id="2f349-189">Umístění složky: D:\home\LogFiles</span><span class="sxs-lookup"><span data-stu-id="2f349-189">Folder location: D:\home\LogFiles</span></span>
* <span data-ttu-id="2f349-190">Soubory, které vás zajímají:</span><span class="sxs-lookup"><span data-stu-id="2f349-190">Files of interest:</span></span>
    * <span data-ttu-id="2f349-191">catalina. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="2f349-191">catalina.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="2f349-192">Správce hostitele. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="2f349-192">host-manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="2f349-193">localhost. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="2f349-193">localhost.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="2f349-194">správce. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="2f349-194">manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="2f349-195">site_access_log. *rrrr mm-dd*.log</span><span class="sxs-lookup"><span data-stu-id="2f349-195">site_access_log.*yyyy-mm-dd*.log</span></span>

## <a name="how-do-i-troubleshoot-jdbc-driver-connection-errors"></a><span data-ttu-id="2f349-196">Jak odstranit chyb připojení ovladač JDBC?</span><span class="sxs-lookup"><span data-stu-id="2f349-196">How do I troubleshoot JDBC driver connection errors?</span></span>

<span data-ttu-id="2f349-197">Může zobrazit následující zprávy do protokolů Tomcat hello:</span><span class="sxs-lookup"><span data-stu-id="2f349-197">You might see hello following message in your Tomcat logs:</span></span>

```
hello web application[ROOT] registered hello JDBC driver [com.mysql.jdbc.Driver] but failed toounregister it when hello web application was stopped. tooprevent a memory leak,hello JDBC Driver has been forcibly unregistered
```

<span data-ttu-id="2f349-198">Chyba tooresolve hello:</span><span class="sxs-lookup"><span data-stu-id="2f349-198">tooresolve hello error:</span></span>

1. <span data-ttu-id="2f349-199">Odeberte hello sqljdbc*.jar soubor ze složky aplikace/lib.</span><span class="sxs-lookup"><span data-stu-id="2f349-199">Remove hello sqljdbc*.jar file from your app/lib folder.</span></span>
2. <span data-ttu-id="2f349-200">Pokud používáte hello vlastní Tomcat nebo Azure Marketplace Tomcat webového serveru, zkopírujte složku lib toohello Tomcat tento .jar souboru.</span><span class="sxs-lookup"><span data-stu-id="2f349-200">If you are using hello custom Tomcat or Azure Marketplace Tomcat web server, copy this .jar file toohello Tomcat lib folder.</span></span>
3. <span data-ttu-id="2f349-201">Pokud chcete povolit Java z hello portálu Azure (vyberte **Java 1.8** > **Tomcat server**), kopírovat hello sqljdbc.* jar soubor ve složce hello, který je paralelní tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="2f349-201">If you are enabling Java from hello Azure portal (select **Java 1.8** > **Tomcat server**), copy hello sqljdbc.* jar file in hello folder that's parallel tooyour app.</span></span> <span data-ttu-id="2f349-202">Pak přidejte hello následující soubor web.config toohello nastavení cesty pro třídy:</span><span class="sxs-lookup"><span data-stu-id="2f349-202">Then, add hello following classpath setting toohello web.config file:</span></span>

    ```
    <httpPlatform>
    <environmentVariables>
    <environmentVariablename ="JAVA_OPTS" value=" -Djava.net.preferIPv4Stack=true
    -Xms128M -classpath %CLASSPATH%;[Path toohello sqljdbc*.jarfile]" />
    </environmentVariables>
    </httpPlatform>
    ```

## <a name="why-do-i-see-errors-when-i-attempt-toocopy-live-log-files"></a><span data-ttu-id="2f349-203">Proč se při I pokus soubory za provozu protokolu toocopy zobrazí chyby?</span><span class="sxs-lookup"><span data-stu-id="2f349-203">Why do I see errors when I attempt toocopy live log files?</span></span>

<span data-ttu-id="2f349-204">Pokud se pokusíte toocopy soubory protokolu za provozu pro aplikace v jazyce Java (například Tomcat), zobrazí tato chyba FTP:</span><span class="sxs-lookup"><span data-stu-id="2f349-204">If you try toocopy live log files for a Java app (for example, Tomcat), you might see this FTP error:</span></span>

```
Error transferring file [filename] Copying files from remote side failed.
    
hello process cannot access hello file because it is being used by another process.
```

<span data-ttu-id="2f349-205">Hello chybová zpráva se může lišit v závislosti na hello FTP klienta.</span><span class="sxs-lookup"><span data-stu-id="2f349-205">hello error message might vary, depending on hello FTP client.</span></span>

<span data-ttu-id="2f349-206">Všechny aplikace v jazyce Java mít potíže uzamčení.</span><span class="sxs-lookup"><span data-stu-id="2f349-206">All Java apps have this locking issue.</span></span> <span data-ttu-id="2f349-207">Pouze Kudu podporuje stahování tohoto souboru je spuštěna aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="2f349-207">Only Kudu supports downloading this file while hello app is running.</span></span>

<span data-ttu-id="2f349-208">Ukončení aplikace hello umožňuje přístup FTP toothese soubory.</span><span class="sxs-lookup"><span data-stu-id="2f349-208">Stopping hello app allows FTP access toothese files.</span></span>

<span data-ttu-id="2f349-209">Jiným řešením je toowrite webové úlohy, který spouští podle plánu a zkopíruje tyto soubory tooa jiný adresář.</span><span class="sxs-lookup"><span data-stu-id="2f349-209">Another workaround is toowrite a WebJob that runs on a schedule and copies these files tooa different directory.</span></span> <span data-ttu-id="2f349-210">Ukázkový projekt, najdete v části hello [CopyLogsJob](https://github.com/kamilsykora/CopyLogsJob) projektu.</span><span class="sxs-lookup"><span data-stu-id="2f349-210">For a sample project, see hello [CopyLogsJob](https://github.com/kamilsykora/CopyLogsJob) project.</span></span>

## <a name="where-do-i-find-hello-log-files-for-jetty"></a><span data-ttu-id="2f349-211">Kde hello soubory protokolu pro Jetty</span><span class="sxs-lookup"><span data-stu-id="2f349-211">Where do I find hello log files for Jetty?</span></span>

<span data-ttu-id="2f349-212">Pro vlastní nasazení a Marketplace hello soubor protokolu je ve složce D:\home\site\wwwroot\bin\jetty-distribution-9.1.2.v20140210\logs hello.</span><span class="sxs-lookup"><span data-stu-id="2f349-212">For Marketplace and custom deployments, hello log file is in hello D:\home\site\wwwroot\bin\jetty-distribution-9.1.2.v20140210\logs folder.</span></span> <span data-ttu-id="2f349-213">Všimněte si, že umístění složky hello závisí na verzi hello Jetty, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="2f349-213">Note that hello folder location depends on hello version of Jetty you are using.</span></span> <span data-ttu-id="2f349-214">Například cesta hello zde uvedené pro Jetty 9.1.2.</span><span class="sxs-lookup"><span data-stu-id="2f349-214">For example, hello path provided here is for Jetty 9.1.2.</span></span> <span data-ttu-id="2f349-215">Vyhledejte jetty_*YYYY_MM_DD*. stderrout.log.</span><span class="sxs-lookup"><span data-stu-id="2f349-215">Look for jetty_*YYYY_MM_DD*.stderrout.log.</span></span>

<span data-ttu-id="2f349-216">Pro nasazení portálu nastavení aplikace soubor protokolu hello je v D:\home\LogFiles.</span><span class="sxs-lookup"><span data-stu-id="2f349-216">For portal App Setting deployments, hello log file is in D:\home\LogFiles.</span></span> <span data-ttu-id="2f349-217">Vyhledejte jetty_*YYYY_MM_DD*. stderrout.log</span><span class="sxs-lookup"><span data-stu-id="2f349-217">Look for jetty_*YYYY_MM_DD*.stderrout.log</span></span>

## <a name="can-i-send-email-from-my-azure-web-app"></a><span data-ttu-id="2f349-218">Můžete odesílat e-maily z Azure webová aplikace?</span><span class="sxs-lookup"><span data-stu-id="2f349-218">Can I send email from my Azure web app?</span></span>

<span data-ttu-id="2f349-219">Služby App Service nemá předdefinované e-mailové funkce.</span><span class="sxs-lookup"><span data-stu-id="2f349-219">App Service doesn't have a built-in email feature.</span></span> <span data-ttu-id="2f349-220">Některé funkční alternativami pro odesílání e-mailu z vaší aplikace, najdete v tématu to [Stack Overflow diskusní](http://stackoverflow.com/questions/17666161/sending-email-from-azure).</span><span class="sxs-lookup"><span data-stu-id="2f349-220">For some good alternatives for sending email from your app, see this [Stack Overflow discussion](http://stackoverflow.com/questions/17666161/sending-email-from-azure).</span></span>

## <a name="why-does-my-wordpress-site-redirect-tooanother-url"></a><span data-ttu-id="2f349-221">Proč můj web WordPress přesměrování tooanother URL?</span><span class="sxs-lookup"><span data-stu-id="2f349-221">Why does my WordPress site redirect tooanother URL?</span></span>

<span data-ttu-id="2f349-222">Pokud jste migrovali nedávno tooAzure, může být přesměrování WordPress toohello starou adresu URL domény.</span><span class="sxs-lookup"><span data-stu-id="2f349-222">If you have recently migrated tooAzure, WordPress might redirect toohello old domain URL.</span></span> <span data-ttu-id="2f349-223">To je způsobeno nastavení v databázi MySQL hello.</span><span class="sxs-lookup"><span data-stu-id="2f349-223">This is caused by a setting in hello MySQL database.</span></span>

<span data-ttu-id="2f349-224">WordPress kamarád + je rozšíření lokality Azure, můžete použít adresu URL pro přesměrování tooupdate hello přímo v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="2f349-224">WordPress Buddy+ is an Azure Site Extension that you can use tooupdate hello redirection URL directly in hello database.</span></span> <span data-ttu-id="2f349-225">Další informace o používání WordPress kamarád + najdete v tématu [WordPress nástroje a migraci databáze MySQL s WordPress kamarád +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span><span class="sxs-lookup"><span data-stu-id="2f349-225">For more information about using WordPress Buddy+, see [WordPress tools and MySQL migration with WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span></span>

<span data-ttu-id="2f349-226">Případně, pokud dáváte přednost toomanually aktualizace hello přesměrování URL pomocí dotazů SQL nebo PHPMyAdmin, najdete v části [WordPress: přesměrování toowrong URL](https://blogs.msdn.microsoft.com/azureossds/2016/07/12/wordpress-redirecting-to-wrong-url/).</span><span class="sxs-lookup"><span data-stu-id="2f349-226">Alternatively, if you prefer toomanually update hello redirection URL by using SQL queries or PHPMyAdmin, see [WordPress: Redirecting toowrong URL](https://blogs.msdn.microsoft.com/azureossds/2016/07/12/wordpress-redirecting-to-wrong-url/).</span></span>

## <a name="how-do-i-change-my-wordpress-sign-in-password"></a><span data-ttu-id="2f349-227">Změna hesla přihlášení WordPress</span><span class="sxs-lookup"><span data-stu-id="2f349-227">How do I change my WordPress sign-in password?</span></span>

<span data-ttu-id="2f349-228">Pokud jste WordPress přihlašovací heslo zapomněli, můžete použít WordPress kamarád + tooupdate ho.</span><span class="sxs-lookup"><span data-stu-id="2f349-228">If you have forgotten your WordPress sign-in password, you can use WordPress Buddy+ tooupdate it.</span></span> <span data-ttu-id="2f349-229">tooreset své heslo, instalace hello WordPress kamarád + rozšíření webu Azure a pak dokončení hello kroky popsané v [WordPress nástroje a migraci databáze MySQL s WordPress kamarád +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span><span class="sxs-lookup"><span data-stu-id="2f349-229">tooreset your password, install hello WordPress Buddy+ Azure Site Extension, and then complete hello steps described in [WordPress tools and MySQL migration with WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span></span>

## <a name="i-cant-sign-in-toowordpress-how-do-i-resolve-this"></a><span data-ttu-id="2f349-230">Nelze se přihlásit tooWordPress.</span><span class="sxs-lookup"><span data-stu-id="2f349-230">I can't sign in tooWordPress.</span></span> <span data-ttu-id="2f349-231">Jak to lze vyřešit?</span><span class="sxs-lookup"><span data-stu-id="2f349-231">How do I resolve this?</span></span>

<span data-ttu-id="2f349-232">Pokud se přistihnete zamknout z WordPress po instalaci nedávno o modul plug-in, můžete mít vadný modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="2f349-232">If you find yourself locked out of WordPress after recently installing a plugin, you might have a faulty plugin.</span></span> <span data-ttu-id="2f349-233">WordPress kamarád + je rozšíření webu Azure, které vám pomůžou zakažte moduly plug-in v WordPress.</span><span class="sxs-lookup"><span data-stu-id="2f349-233">WordPress Buddy+ is an Azure Site Extension that can help you disable plugins in WordPress.</span></span> <span data-ttu-id="2f349-234">Další informace najdete v tématu [WordPress nástroje a migraci databáze MySQL s WordPress kamarád +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span><span class="sxs-lookup"><span data-stu-id="2f349-234">For more information, see [WordPress tools and MySQL migration with WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span></span>

## <a name="how-do-i-migrate-my-wordpress-database"></a><span data-ttu-id="2f349-235">Jak migraci databáze WordPress?</span><span class="sxs-lookup"><span data-stu-id="2f349-235">How do I migrate my WordPress database?</span></span>

<span data-ttu-id="2f349-236">Máte několik možností pro migraci databáze MySQL hello, který je připojený tooyour web WordPress:</span><span class="sxs-lookup"><span data-stu-id="2f349-236">You have multiple options for migrating hello MySQL database that's connected tooyour WordPress website:</span></span>

* <span data-ttu-id="2f349-237">Vývojáři: Použití hello [příkazový řádek nebo PHPMyAdmin](https://blogs.msdn.microsoft.com/azureossds/2016/03/02/migrating-data-between-mysql-databases-using-kudu-console-azure-app-service/)</span><span class="sxs-lookup"><span data-stu-id="2f349-237">Developers: Use hello [command prompt or PHPMyAdmin](https://blogs.msdn.microsoft.com/azureossds/2016/03/02/migrating-data-between-mysql-databases-using-kudu-console-azure-app-service/)</span></span>
* <span data-ttu-id="2f349-238">Jiný vývojáři: Použít [WordPress kamarád +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/)</span><span class="sxs-lookup"><span data-stu-id="2f349-238">Non-developers: Use [WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/)</span></span>

## <a name="how-do-i-help-make-wordpress-more-secure"></a><span data-ttu-id="2f349-239">Jak pomoci, zabezpečit WordPress?</span><span class="sxs-lookup"><span data-stu-id="2f349-239">How do I help make WordPress more secure?</span></span>

<span data-ttu-id="2f349-240">toolearn o osvědčené postupy zabezpečení pro WordPress, najdete v části [osvědčené postupy pro zabezpečení WordPress v Azure](https://blogs.msdn.microsoft.com/azureossds/2016/12/26/best-practices-for-wordpress-security-on-azure/).</span><span class="sxs-lookup"><span data-stu-id="2f349-240">toolearn about security best practices for WordPress, see [Best practices for WordPress security in Azure](https://blogs.msdn.microsoft.com/azureossds/2016/12/26/best-practices-for-wordpress-security-on-azure/).</span></span>

## <a name="i-am-trying-toouse-phpmyadmin-and-i-see-hello-message-access-denied-how-do-i-resolve-this"></a><span data-ttu-id="2f349-241">Pokouším toouse PHPMyAdmin a hello zpráva "Přístup byl odepřen".</span><span class="sxs-lookup"><span data-stu-id="2f349-241">I am trying toouse PHPMyAdmin, and I see hello message “Access denied.”</span></span> <span data-ttu-id="2f349-242">Jak to lze vyřešit?</span><span class="sxs-lookup"><span data-stu-id="2f349-242">How do I resolve this?</span></span>

<span data-ttu-id="2f349-243">Tomuto problému může dojít, pokud hello MySQL v aplikaci funkce ještě neběží v této instanci služby App Service.</span><span class="sxs-lookup"><span data-stu-id="2f349-243">You might experience this issue if hello MySQL in-app feature isn't running yet in this App Service instance.</span></span> <span data-ttu-id="2f349-244">tooresolve hello problém, zkuste tooaccess vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="2f349-244">tooresolve hello issue, try tooaccess your website.</span></span> <span data-ttu-id="2f349-245">Tím se spustí hello požadované procesů, včetně procesu hello MySQL v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2f349-245">This starts hello required processes, including hello MySQL in-app process.</span></span> <span data-ttu-id="2f349-246">tooverify, MySQL v aplikace běží v Průzkumníka procesů, ujistěte se, že mysqld.exe je uvedena v hello procesy.</span><span class="sxs-lookup"><span data-stu-id="2f349-246">tooverify that MySQL in-app is running, in Process Explorer, ensure that mysqld.exe is listed in hello processes.</span></span>

<span data-ttu-id="2f349-247">Po zajištění, že je spuštěna MySQL v aplikaci, zkuste toouse PHPMyAdmin.</span><span class="sxs-lookup"><span data-stu-id="2f349-247">After you ensure that MySQL in-app is running, try toouse PHPMyAdmin.</span></span>

## <a name="i-get-an-http-403-error-when-i-try-tooimport-or-export-my-mysql-in-app-database-by-using-phpmyadmin-how-do-i-resolve-this"></a><span data-ttu-id="2f349-248">Zobrazuje chybu HTTP 403 při akci tooimport nebo exportovat pomocí PHPMyadmin databáze MySQL v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2f349-248">I get an HTTP 403 error when I try tooimport or export my MySQL in-app database by using PHPMyadmin.</span></span> <span data-ttu-id="2f349-249">Jak to lze vyřešit?</span><span class="sxs-lookup"><span data-stu-id="2f349-249">How do I resolve this?</span></span>

<span data-ttu-id="2f349-250">Pokud používáte starší verzi Chrome, může se jednat známého problému.</span><span class="sxs-lookup"><span data-stu-id="2f349-250">If you are using an older version of Chrome, you might be experiencing a known bug.</span></span> <span data-ttu-id="2f349-251">tooresolve hello problém, upgradu tooa novější verzi Chrome.</span><span class="sxs-lookup"><span data-stu-id="2f349-251">tooresolve hello issue, upgrade tooa newer version of Chrome.</span></span> <span data-ttu-id="2f349-252">Zkuste taky pomocí jiného prohlížeče, jako je Internet Explorer nebo Edge, kde hello problému nedojde.</span><span class="sxs-lookup"><span data-stu-id="2f349-252">Also try using a different browser, like Internet Explorer or Edge, where hello issue does not occur.</span></span>
