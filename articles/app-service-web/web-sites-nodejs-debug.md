---
title: "Postup ladění webové aplikace Node.js ve službě Azure App Service"
description: "Postup ladění webové aplikace Node.js ve službě Azure App Service."
tags: azure-portal
services: app-service\web
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: a48f906c-1a3e-43bc-ae84-7d2dde175b15
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 5e302a4c58a171d40e43a22c34c724e868019ec8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-debug-a-nodejs-web-app-in-azure-app-service"></a><span data-ttu-id="c5325-103">Postup ladění webové aplikace Node.js ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c5325-103">How to debug a Node.js web app in Azure App Service</span></span>
<span data-ttu-id="c5325-104">Azure poskytuje integrované diagnostiky vám pomůže při ladění aplikace Node.js ve [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c5325-104">Azure provides built-in diagnostics to assist with debugging Node.js applications hosted in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps.</span></span> <span data-ttu-id="c5325-105">V tomto článku se dozvíte, jak povolit protokolování stdout a stderr, v prohlížeči zobrazit informace o chybě a jak stáhnout a zobrazit soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="c5325-105">In this article, you will learn how to enable logging of stdout and stderr, display error information in the browser, and how to download and view log files.</span></span>

<span data-ttu-id="c5325-106">Diagnostika pro aplikace Node.js hostované v Azure poskytuje [IISNode].</span><span class="sxs-lookup"><span data-stu-id="c5325-106">Diagnostics for Node.js applications hosted on Azure is provided by [IISNode].</span></span> <span data-ttu-id="c5325-107">Když tento článek popisuje nejběžnější nastavení pro shromažďování diagnostické informace, neposkytuje úplný referenční pro práci s modulu IISNode.</span><span class="sxs-lookup"><span data-stu-id="c5325-107">While this article discusses the most common settings for gathering diagnostics information, it does not provide a complete reference for working with IISNode.</span></span> <span data-ttu-id="c5325-108">Další informace o práci s modulu IISNode, najdete v článku [modulu IISNode Readme] na Githubu.</span><span class="sxs-lookup"><span data-stu-id="c5325-108">For more information on working with IISNode, see the [IISNode Readme] on GitHub.</span></span>

<a id="enablelogging"></a>

## <a name="enable-logging"></a><span data-ttu-id="c5325-109">Povolit protokolování</span><span class="sxs-lookup"><span data-stu-id="c5325-109">Enable logging</span></span>
<span data-ttu-id="c5325-110">Ve výchozím nastavení webové aplikace služby App Service zaznamená pouze diagnostické informace o nasazení, například když nasadíte webovou aplikaci pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="c5325-110">By default, an App Service web app only captures diagnostic information about deployments, such as when you deploy a web app using Git.</span></span> <span data-ttu-id="c5325-111">Tyto informace jsou užitečné, pokud máte potíže při nasazení, například selhání při instalaci modulu, kterou se odkazuje v **package.json**, nebo pokud používáte vlastní nasazení skriptu.</span><span class="sxs-lookup"><span data-stu-id="c5325-111">This information is useful if you are having problems during deployment, such as a failure when installing a module referenced in **package.json**, or if you are using a custom deployment script.</span></span>

<span data-ttu-id="c5325-112">Chcete-li povolit protokolování stdout a stderr datové proudy, musíte vytvořit **IISNode.yml** souboru v kořenovém adresáři aplikace Node.js a přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="c5325-112">To enable the logging of stdout and stderr streams, you must create an **IISNode.yml** file at the root of your Node.js application and add the following:</span></span>

    loggingEnabled: true

<span data-ttu-id="c5325-113">To umožňuje protokolování stderr a stdout z aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="c5325-113">This enables the logging of stderr and stdout from your Node.js application.</span></span>

<span data-ttu-id="c5325-114">**IISNode.yml** soubor lze použít také na ovládací prvek, zda popisný chyby nebo vývojáře se vrátí do prohlížeče při dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="c5325-114">The **IISNode.yml** file can also be used to control whether friendly errors or developer errors are returned to the browser when a failure occurs.</span></span> <span data-ttu-id="c5325-115">Povolit vývojáře chyby, přidejte následující řádek na **IISNode.yml** souboru:</span><span class="sxs-lookup"><span data-stu-id="c5325-115">To enable developer errors, add the following line to the **IISNode.yml** file:</span></span>

    devErrorsEnabled: true

<span data-ttu-id="c5325-116">Jakmile povolíte tuto možnost, modul IISNode vrátí poslední 64 kB informace odesílané do stderr místo popisný chyby, jako je "o vnitřní chybu serveru došlo k chybě".</span><span class="sxs-lookup"><span data-stu-id="c5325-116">Once this option is enabled, IISNode will return the last 64K of information sent to stderr instead of a friendly error such as "an internal server error occurred".</span></span>

> [!NOTE]
> <span data-ttu-id="c5325-117">Zatímco devErrorsEnabled je užitečné při diagnostice problémů během vývoje, povolení v produkčním prostředí může vést k chybám vývoj odesílány koncovým uživatelům.</span><span class="sxs-lookup"><span data-stu-id="c5325-117">While devErrorsEnabled is useful when diagnosing problems during development, enabling it in a production environment may result in development errors being sent to end users.</span></span>
> 
> 

<span data-ttu-id="c5325-118">Pokud **IISNode.yml** soubor v rámci vaší aplikace již neexistoval, je nutné restartovat vaší webové aplikace po publikování aktualizované aplikace.</span><span class="sxs-lookup"><span data-stu-id="c5325-118">If the **IISNode.yml** file did not already exist within your application, you must restart your web app after publishing the updated application.</span></span> <span data-ttu-id="c5325-119">Pokud chcete změnit nastavení v existující jednoduše **IISNode.yml** soubor, který se dříve publikoval, není nutné žádné restartování.</span><span class="sxs-lookup"><span data-stu-id="c5325-119">If you are simply changing settings in an existing **IISNode.yml** file that has previously been published, no restart is required.</span></span>

> [!NOTE]
> <span data-ttu-id="c5325-120">Pokud vaše webová aplikace byla vytvořena pomocí nástroje příkazového řádku Azure nebo rutin prostředí Azure PowerShell, výchozí **IISNode.yml** soubor se automaticky vytvoří.</span><span class="sxs-lookup"><span data-stu-id="c5325-120">If your web app was created using the Azure Command-Line Tools or Azure PowerShell Cmdlets, a default **IISNode.yml** file is automatically created.</span></span>
> 
> 

<span data-ttu-id="c5325-121">Restartování webové aplikace, vyberte webovou aplikaci v [portálu Azure](https://portal.azure.com)a potom klikněte na **RESTARTUJTE** tlačítko:</span><span class="sxs-lookup"><span data-stu-id="c5325-121">To restart the web app, select the web app in the [Azure Portal](https://portal.azure.com), and then click **RESTART** button:</span></span>

![Restartujte tlačítko][restart-button]

<span data-ttu-id="c5325-123">Pokud nástroje příkazového řádku Azure jsou nainstalovány ve vašem vývojovém prostředí, můžete tyto příkazy k restartování webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="c5325-123">If the Azure Command-Line Tools are installed in your development environment, you can use the following command to restart the web app:</span></span>

    azure site restart [sitename]

> [!NOTE]
> <span data-ttu-id="c5325-124">LoggingEnabled a devErrorsEnabled jsou nejčastěji používané IISNode.yml možnosti konfigurace pro shromažďování diagnostických informací, IISNode.yml slouží ke konfiguraci celou řadu možností pro hostitelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="c5325-124">While loggingEnabled and devErrorsEnabled are the most commonly used IISNode.yml configuration options for capturing diagnostic information, IISNode.yml can be used to configure a variety of options for your hosting environment.</span></span> <span data-ttu-id="c5325-125">Úplný seznam možností konfigurace, najdete v článku [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) souboru.</span><span class="sxs-lookup"><span data-stu-id="c5325-125">For a full list of the configuration options, see the [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) file.</span></span>
> 
> 

<a id="viewlogs"></a>

## <a name="accessing-logs"></a><span data-ttu-id="c5325-126">Přístup k protokoly</span><span class="sxs-lookup"><span data-stu-id="c5325-126">Accessing logs</span></span>
<span data-ttu-id="c5325-127">Diagnostické protokoly jsou přístupné třemi způsoby; Pomocí protokol FTP (File Transfer), stahování archivu Zip, nebo jako živé aktualizovat datového proudu protokolu (také označované jako tail).</span><span class="sxs-lookup"><span data-stu-id="c5325-127">Diagnostic logs can be accessed in three ways; Using the File Transfer Protocol (FTP), downloading a Zip archive, or as a live updated stream of the log (also known as a tail).</span></span> <span data-ttu-id="c5325-128">Stahování archivu Zip souborů protokolu nebo zobrazení živý datový proud vyžadují nástroje příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="c5325-128">Downloading the Zip archive of the log files or viewing the live stream require the Azure Command-Line Tools.</span></span> <span data-ttu-id="c5325-129">Lze je instalovat pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="c5325-129">These can be installed by using the following command:</span></span>

    npm install azure-cli -g

<span data-ttu-id="c5325-130">Po instalaci nástroje je přístupný pomocí příkazu "azure".</span><span class="sxs-lookup"><span data-stu-id="c5325-130">Once installed, the tools can be accessed using the 'azure' command.</span></span> <span data-ttu-id="c5325-131">Nástroje příkazového řádku musí nejdřív nakonfigurovat na používání vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="c5325-131">The command-line tools must first be configured to use your Azure subscription.</span></span> <span data-ttu-id="c5325-132">Informace o tom, jak se dá tento úkol provést, najdete v tématu **stahování a import nastavení publikování** části [postup použití nástroje příkazového řádku Azure](../xplat-cli-connect.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c5325-132">For information on how to accomplish this task, see the **How to download and import publish settings** section of the [How to Use The Azure Command-Line Tools](../xplat-cli-connect.md) article.</span></span>

### <a name="ftp"></a><span data-ttu-id="c5325-133">FTP</span><span class="sxs-lookup"><span data-stu-id="c5325-133">FTP</span></span>
<span data-ttu-id="c5325-134">Pro přístup k diagnostické informace pomocí služby FTP, přejděte [portálu Azure](https://portal.azure.com), vyberte webovou aplikaci a pak vyberte **řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="c5325-134">To access the diagnostic information through FTP, visit the [Azure Portal](https://portal.azure.com), select your web app, and then select the **DASHBOARD**.</span></span> <span data-ttu-id="c5325-135">V **rychlé odkazy** části **diagnostické protokoly FTP** a **FTPS diagnostické protokoly** odkazy poskytují přístup k protokolům pomocí protokolu FTP.</span><span class="sxs-lookup"><span data-stu-id="c5325-135">In the **quick links** section, the **FTP DIAGNOSTIC LOGS** and **FTPS DIAGNOSTIC LOGS** links provide access to the logs using the FTP protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="c5325-136">Pokud jste předtím nenakonfigurovali uživatelské jméno a heslo pro nasazení nebo FTP, můžete tak učinit z **rychlý Start** stránce management výběrem **nastavit přihlašovací údaje nasazení**.</span><span class="sxs-lookup"><span data-stu-id="c5325-136">If you have not previously configured user name and password for FTP or deployment, you can do so from the **Quickstart** management page by selecting **Set up deployment credentials**.</span></span>
> 
> 

<span data-ttu-id="c5325-137">Adresa URL FTP, vrátí se v řídicím panelu **LogFiles** adresáři, který bude obsahovat následující dílčí adresáře:</span><span class="sxs-lookup"><span data-stu-id="c5325-137">The FTP URL returned in the dashboard is for the **LogFiles** directory, which will contain the following sub-directories:</span></span>

* <span data-ttu-id="c5325-138">[Metoda nasazení](web-sites-deploy.md) – Pokud používáte metodu nasazení, jako je například Git, se vytvoří adresář se stejným názvem a bude obsahovat informace týkající se nasazení.</span><span class="sxs-lookup"><span data-stu-id="c5325-138">[Deployment Method](web-sites-deploy.md) - If you use a deployment method such as Git, a directory of the same name will be created and will contain information related to deployments.</span></span>
* <span data-ttu-id="c5325-139">nodejs - Stdout a stderr informace zachyceného v všechny instance aplikace (Pokud loggingEnabled hodnotu true.)</span><span class="sxs-lookup"><span data-stu-id="c5325-139">nodejs - Stdout and stderr information captured from all instances of your application (when loggingEnabled is true.)</span></span>

### <a name="zip-archive"></a><span data-ttu-id="c5325-140">Archivu ZIP</span><span class="sxs-lookup"><span data-stu-id="c5325-140">Zip archive</span></span>
<span data-ttu-id="c5325-141">Chcete-li stáhnout archivu Zip diagnostických protokolů, použijte následující příkaz z příkazového řádku nástroje Azure:</span><span class="sxs-lookup"><span data-stu-id="c5325-141">To download a Zip archive of the diagnostic logs, use the following command from the Azure Command-Line Tools:</span></span>

    azure site log download [sitename]

<span data-ttu-id="c5325-142">To bude stahovat **diagnostics.zip** v aktuálním adresáři.</span><span class="sxs-lookup"><span data-stu-id="c5325-142">This will download a **diagnostics.zip** in the current directory.</span></span> <span data-ttu-id="c5325-143">Archivu obsahuje následující adresářovou strukturu:</span><span class="sxs-lookup"><span data-stu-id="c5325-143">This archive contains the following directory structure:</span></span>

* <span data-ttu-id="c5325-144">nasazení – protokolu informace o nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="c5325-144">deployments - A log of information about deployments of your application</span></span>
* <span data-ttu-id="c5325-145">LogFiles</span><span class="sxs-lookup"><span data-stu-id="c5325-145">LogFiles</span></span>
  
  * <span data-ttu-id="c5325-146">[Metoda nasazení](web-sites-deploy.md) – Pokud používáte metodu nasazení, jako je například Git, se vytvoří adresář se stejným názvem a bude obsahovat informace týkající se nasazení.</span><span class="sxs-lookup"><span data-stu-id="c5325-146">[Deployment method](web-sites-deploy.md) - If you use a deployment method such as Git, a directory of the same name will be created and will contain information related to deployments.</span></span>
  * <span data-ttu-id="c5325-147">nodejs - Stdout a stderr informace zachyceného v všechny instance aplikace (Pokud loggingEnabled hodnotu true.)</span><span class="sxs-lookup"><span data-stu-id="c5325-147">nodejs - Stdout and stderr information captured from all instances of your application (when loggingEnabled is true.)</span></span>

### <a name="live-stream-tail"></a><span data-ttu-id="c5325-148">Živý datový proud (tail)</span><span class="sxs-lookup"><span data-stu-id="c5325-148">Live stream (tail)</span></span>
<span data-ttu-id="c5325-149">Chcete-li zobrazit živý datový proud diagnostických informací, protokolu, použijte následující příkaz z příkazového řádku nástroje Azure:</span><span class="sxs-lookup"><span data-stu-id="c5325-149">To view a live stream of diagnostic log information, use the following command from the Azure Command-Line Tools:</span></span>

    azure site log tail [sitename]

<span data-ttu-id="c5325-150">Tato možnost vrátí datový proud protokolu událostí, které jsou aktualizované, kdy k nim dojde na serveru.</span><span class="sxs-lookup"><span data-stu-id="c5325-150">This will return a stream of log events that are updated as they occur on the server.</span></span> <span data-ttu-id="c5325-151">Tento datový proud vrátí informace o nasazení a také stdout a stderr informace (Pokud loggingEnabled hodnotu true.)</span><span class="sxs-lookup"><span data-stu-id="c5325-151">This stream will return deployment information as well as stdout and stderr information (when loggingEnabled is true.)</span></span>

<a id="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="c5325-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c5325-152">Next Steps</span></span>
<span data-ttu-id="c5325-153">V tomto článku jste zjistili, jak povolit a přístup k diagnostické informace pro Azure.</span><span class="sxs-lookup"><span data-stu-id="c5325-153">In this article you learned how to enable and access diagnostics information for Azure.</span></span> <span data-ttu-id="c5325-154">Když tyto informace jsou užitečné v Principy problémy, které se aplikace, mohou odkazovat problém s modulem používáte nebo verze Node.js používané App Service Web Apps je jiný než ten, který používá v prostředí pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="c5325-154">While this information is useful in understanding problems that occur with your application, it may point to a problem with a module you are using or that the version of Node.js used by App Service Web Apps is different than the one used in your deployment environment.</span></span>

<span data-ttu-id="c5325-155">Informace v práci s moduly v Azure najdete v tématu [používání modulů Node.js s aplikacemi Azure](../nodejs-use-node-modules-azure-apps.md).</span><span class="sxs-lookup"><span data-stu-id="c5325-155">For information in working with modules on Azure, see [Using Node.js Modules with Azure Applications](../nodejs-use-node-modules-azure-apps.md).</span></span>

<span data-ttu-id="c5325-156">Informace o určení verze Node.js pro vaši aplikaci najdete v tématu [určení verze Node.js v aplikaci Azure].</span><span class="sxs-lookup"><span data-stu-id="c5325-156">For information on specifying a Node.js version for your application, see [Specifying a Node.js version in an Azure application].</span></span>

<span data-ttu-id="c5325-157">Další informace naleznete také [středisku pro vývojáře Node.js](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="c5325-157">For more information, see also the [Node.js Developer Center](/develop/nodejs/).</span></span>

## <a name="whats-changed"></a><span data-ttu-id="c5325-158">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="c5325-158">What's changed</span></span>
* <span data-ttu-id="c5325-159">Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="c5325-159">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

> [!NOTE]
> <span data-ttu-id="c5325-160">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c5325-160">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="c5325-161">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="c5325-161">No credit cards required; no commitments.</span></span>
> 
> 

<span data-ttu-id="c5325-162">[IISNode]: https://github.com/tjanczuk/iisnode</span><span class="sxs-lookup"><span data-stu-id="c5325-162">[IISNode]: https://github.com/tjanczuk/iisnode</span></span>
<span data-ttu-id="c5325-163">[modulu IISNode Readme]: https://github.com/tjanczuk/iisnode#readme</span><span class="sxs-lookup"><span data-stu-id="c5325-163">[IISNode Readme]: https://github.com/tjanczuk/iisnode#readme</span></span>
[How to Use The Azure Command-Line Interface]:../cli-install-nodejs.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
<span data-ttu-id="c5325-164">[určení verze Node.js v aplikaci Azure]: ../nodejs-specify-node-version-azure-apps.md</span><span class="sxs-lookup"><span data-stu-id="c5325-164">[Specifying a Node.js version in an Azure application]: ../nodejs-specify-node-version-azure-apps.md</span></span>

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png

