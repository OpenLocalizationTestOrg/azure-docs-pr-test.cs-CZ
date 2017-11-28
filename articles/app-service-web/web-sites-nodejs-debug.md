---
title: "aaaHow toodebug webové aplikace Node.js ve službě Azure App Service"
description: "Zjistěte, jak toodebug Node.js webové aplikace v Azure App Service."
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
ms.openlocfilehash: 888ec5c3f92cfc3aeea4ea86005b9b6a0d1306ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodebug-a-nodejs-web-app-in-azure-app-service"></a><span data-ttu-id="bccd5-103">Jak toodebug Node.js webové aplikace v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="bccd5-103">How toodebug a Node.js web app in Azure App Service</span></span>
<span data-ttu-id="bccd5-104">Azure poskytuje integrované diagnostiky tooassist s ladění aplikace Node.js ve [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="bccd5-104">Azure provides built-in diagnostics tooassist with debugging Node.js applications hosted in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps.</span></span> <span data-ttu-id="bccd5-105">V tomto článku se dozvíte, jak protokolování tooenable stdout a stderr, informace o chybě zobrazení v prohlížeči hello a jak toodownload a zobrazit soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="bccd5-105">In this article, you will learn how tooenable logging of stdout and stderr, display error information in hello browser, and how toodownload and view log files.</span></span>

<span data-ttu-id="bccd5-106">Diagnostika pro aplikace Node.js hostované v Azure poskytuje [IISNode].</span><span class="sxs-lookup"><span data-stu-id="bccd5-106">Diagnostics for Node.js applications hosted on Azure is provided by [IISNode].</span></span> <span data-ttu-id="bccd5-107">Když tento článek popisuje hello nejběžnější nastavení pro shromažďování diagnostické informace, neposkytuje úplný referenční pro práci s modulu IISNode.</span><span class="sxs-lookup"><span data-stu-id="bccd5-107">While this article discusses hello most common settings for gathering diagnostics information, it does not provide a complete reference for working with IISNode.</span></span> <span data-ttu-id="bccd5-108">Další informace o práci s modulu IISNode, najdete v části hello [modulu IISNode Readme] na Githubu.</span><span class="sxs-lookup"><span data-stu-id="bccd5-108">For more information on working with IISNode, see hello [IISNode Readme] on GitHub.</span></span>

<a id="enablelogging"></a>

## <a name="enable-logging"></a><span data-ttu-id="bccd5-109">Povolit protokolování</span><span class="sxs-lookup"><span data-stu-id="bccd5-109">Enable logging</span></span>
<span data-ttu-id="bccd5-110">Ve výchozím nastavení webové aplikace služby App Service zaznamená pouze diagnostické informace o nasazení, například když nasadíte webovou aplikaci pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="bccd5-110">By default, an App Service web app only captures diagnostic information about deployments, such as when you deploy a web app using Git.</span></span> <span data-ttu-id="bccd5-111">Tyto informace jsou užitečné, pokud máte potíže při nasazení, například selhání při instalaci modulu, kterou se odkazuje v **package.json**, nebo pokud používáte vlastní nasazení skriptu.</span><span class="sxs-lookup"><span data-stu-id="bccd5-111">This information is useful if you are having problems during deployment, such as a failure when installing a module referenced in **package.json**, or if you are using a custom deployment script.</span></span>

<span data-ttu-id="bccd5-112">tooenable hello protokolování stdout a stderr datové proudy, musíte vytvořit **IISNode.yml** souboru v kořenovém adresáři aplikace Node.js hello a přidejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="bccd5-112">tooenable hello logging of stdout and stderr streams, you must create an **IISNode.yml** file at hello root of your Node.js application and add hello following:</span></span>

    loggingEnabled: true

<span data-ttu-id="bccd5-113">To umožňuje protokolování hello stderr a stdout z aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="bccd5-113">This enables hello logging of stderr and stdout from your Node.js application.</span></span>

<span data-ttu-id="bccd5-114">Hello **IISNode.yml** soubor může být také použít toocontrol zda popisný chyby nebo vývojáře jsou vráceny toohello prohlížeče, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="bccd5-114">hello **IISNode.yml** file can also be used toocontrol whether friendly errors or developer errors are returned toohello browser when a failure occurs.</span></span> <span data-ttu-id="bccd5-115">chyby tooenable developer, přidejte následující řádek toohello hello **IISNode.yml** souboru:</span><span class="sxs-lookup"><span data-stu-id="bccd5-115">tooenable developer errors, add hello following line toohello **IISNode.yml** file:</span></span>

    devErrorsEnabled: true

<span data-ttu-id="bccd5-116">Jakmile povolíte tuto možnost, modul IISNode vrátí hello poslední 64 tisíc informací odesílaných toostderr místo popisný chyby, jako je "o vnitřní chybu serveru došlo k chybě".</span><span class="sxs-lookup"><span data-stu-id="bccd5-116">Once this option is enabled, IISNode will return hello last 64K of information sent toostderr instead of a friendly error such as "an internal server error occurred".</span></span>

> [!NOTE]
> <span data-ttu-id="bccd5-117">Zatímco devErrorsEnabled je užitečné při diagnostice problémů během vývoje, povolení v produkčním prostředí může vést k chybám vývoj odesílány tooend uživatele.</span><span class="sxs-lookup"><span data-stu-id="bccd5-117">While devErrorsEnabled is useful when diagnosing problems during development, enabling it in a production environment may result in development errors being sent tooend users.</span></span>
> 
> 

<span data-ttu-id="bccd5-118">Pokud hello **IISNode.yml** souboru v rámci vaší aplikace již neexistuje, je nutné restartovat po publikování aplikace hello aktualizovat vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bccd5-118">If hello **IISNode.yml** file did not already exist within your application, you must restart your web app after publishing hello updated application.</span></span> <span data-ttu-id="bccd5-119">Pokud chcete změnit nastavení v existující jednoduše **IISNode.yml** soubor, který se dříve publikoval, není nutné žádné restartování.</span><span class="sxs-lookup"><span data-stu-id="bccd5-119">If you are simply changing settings in an existing **IISNode.yml** file that has previously been published, no restart is required.</span></span>

> [!NOTE]
> <span data-ttu-id="bccd5-120">Pokud vaše webová aplikace byla vytvořena pomocí nástroje příkazového řádku Azure hello nebo rutin prostředí Azure PowerShell, výchozí **IISNode.yml** soubor se automaticky vytvoří.</span><span class="sxs-lookup"><span data-stu-id="bccd5-120">If your web app was created using hello Azure Command-Line Tools or Azure PowerShell Cmdlets, a default **IISNode.yml** file is automatically created.</span></span>
> 
> 

<span data-ttu-id="bccd5-121">toorestart hello webovou aplikaci, vyberte hello webové aplikace ve hello [portálu Azure](https://portal.azure.com)a potom klikněte na **RESTARTUJTE** tlačítko:</span><span class="sxs-lookup"><span data-stu-id="bccd5-121">toorestart hello web app, select hello web app in hello [Azure Portal](https://portal.azure.com), and then click **RESTART** button:</span></span>

![Restartujte tlačítko][restart-button]

<span data-ttu-id="bccd5-123">Pokud nástroje příkazového řádku Azure hello jsou nainstalovány ve vašem vývojovém prostředí, můžete použít následující příkaz toorestart hello webové aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="bccd5-123">If hello Azure Command-Line Tools are installed in your development environment, you can use hello following command toorestart hello web app:</span></span>

    azure site restart [sitename]

> [!NOTE]
> <span data-ttu-id="bccd5-124">LoggingEnabled a devErrorsEnabled jsou možnosti konfigurace IISNode.yml hello nejčastěji používaná pro shromažďování diagnostických informací, může být IISNode.yml použité tooconfigure celou řadu možností pro hostitelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="bccd5-124">While loggingEnabled and devErrorsEnabled are hello most commonly used IISNode.yml configuration options for capturing diagnostic information, IISNode.yml can be used tooconfigure a variety of options for your hosting environment.</span></span> <span data-ttu-id="bccd5-125">Úplný seznam možností konfigurace hello najdete v tématu hello [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) souboru.</span><span class="sxs-lookup"><span data-stu-id="bccd5-125">For a full list of hello configuration options, see hello [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) file.</span></span>
> 
> 

<a id="viewlogs"></a>

## <a name="accessing-logs"></a><span data-ttu-id="bccd5-126">Přístup k protokoly</span><span class="sxs-lookup"><span data-stu-id="bccd5-126">Accessing logs</span></span>
<span data-ttu-id="bccd5-127">Diagnostické protokoly jsou přístupné třemi způsoby; Pomocí hello protokol FTP (File Transfer), stahování archivu Zip, nebo jako živé aktualizovat datový proud protokolu hello (také označované jako tail).</span><span class="sxs-lookup"><span data-stu-id="bccd5-127">Diagnostic logs can be accessed in three ways; Using hello File Transfer Protocol (FTP), downloading a Zip archive, or as a live updated stream of hello log (also known as a tail).</span></span> <span data-ttu-id="bccd5-128">Stahování archivu Zip hello hello protokolových souborů nebo zobrazení živý datový proud hello vyžadují hello nástroje příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="bccd5-128">Downloading hello Zip archive of hello log files or viewing hello live stream require hello Azure Command-Line Tools.</span></span> <span data-ttu-id="bccd5-129">Lze je instalovat pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="bccd5-129">These can be installed by using hello following command:</span></span>

    npm install azure-cli -g

<span data-ttu-id="bccd5-130">Po instalaci nástrojů hello je přístupný pomocí příkazu "azure" hello.</span><span class="sxs-lookup"><span data-stu-id="bccd5-130">Once installed, hello tools can be accessed using hello 'azure' command.</span></span> <span data-ttu-id="bccd5-131">Hello nástroje příkazového řádku musí být nakonfigurované toouse nejprve při vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="bccd5-131">hello command-line tools must first be configured toouse your Azure subscription.</span></span> <span data-ttu-id="bccd5-132">Informace o tom, jak se tooaccomplish to úloh najdete v tématu hello **jak toodownload a import nastavení publikování** části hello [jak tooUse hello nástroje příkazového řádku Azure](../xplat-cli-connect.md) článku.</span><span class="sxs-lookup"><span data-stu-id="bccd5-132">For information on how tooaccomplish this task, see hello **How toodownload and import publish settings** section of hello [How tooUse hello Azure Command-Line Tools](../xplat-cli-connect.md) article.</span></span>

### <a name="ftp"></a><span data-ttu-id="bccd5-133">FTP</span><span class="sxs-lookup"><span data-stu-id="bccd5-133">FTP</span></span>
<span data-ttu-id="bccd5-134">tooaccess hello diagnostických informací pomocí služby FTP, navštivte hello [portálu Azure](https://portal.azure.com), vyberte webovou aplikaci a pak vyberte hello **řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="bccd5-134">tooaccess hello diagnostic information through FTP, visit hello [Azure Portal](https://portal.azure.com), select your web app, and then select hello **DASHBOARD**.</span></span> <span data-ttu-id="bccd5-135">V hello **rychlé odkazy** části hello **diagnostické protokoly FTP** a **FTPS diagnostické protokoly** odkazy poskytují přístup k protokolům toohello pomocí protokolu hello FTP.</span><span class="sxs-lookup"><span data-stu-id="bccd5-135">In hello **quick links** section, hello **FTP DIAGNOSTIC LOGS** and **FTPS DIAGNOSTIC LOGS** links provide access toohello logs using hello FTP protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="bccd5-136">Pokud jste předtím nenakonfigurovali uživatelské jméno a heslo pro nasazení nebo FTP, můžete tak učinit z hello **rychlý Start** stránce management výběrem **nastavit přihlašovací údaje nasazení**.</span><span class="sxs-lookup"><span data-stu-id="bccd5-136">If you have not previously configured user name and password for FTP or deployment, you can do so from hello **Quickstart** management page by selecting **Set up deployment credentials**.</span></span>
> 
> 

<span data-ttu-id="bccd5-137">Hello adresy URL FTP, vrátí se v řídicím panelu hello je pro hello **LogFiles** adresáři, který bude obsahovat hello následující dílčí adresáře:</span><span class="sxs-lookup"><span data-stu-id="bccd5-137">hello FTP URL returned in hello dashboard is for hello **LogFiles** directory, which will contain hello following sub-directories:</span></span>

* <span data-ttu-id="bccd5-138">[Metoda nasazení](web-sites-deploy.md) – Pokud používáte metodu nasazení, jako je například Git, adresář hello stejným názvem bude vytvořen a bude obsahují informace související s toodeployments.</span><span class="sxs-lookup"><span data-stu-id="bccd5-138">[Deployment Method](web-sites-deploy.md) - If you use a deployment method such as Git, a directory of hello same name will be created and will contain information related toodeployments.</span></span>
* <span data-ttu-id="bccd5-139">nodejs - Stdout a stderr informace zachyceného v všechny instance aplikace (Pokud loggingEnabled hodnotu true.)</span><span class="sxs-lookup"><span data-stu-id="bccd5-139">nodejs - Stdout and stderr information captured from all instances of your application (when loggingEnabled is true.)</span></span>

### <a name="zip-archive"></a><span data-ttu-id="bccd5-140">Archivu ZIP</span><span class="sxs-lookup"><span data-stu-id="bccd5-140">Zip archive</span></span>
<span data-ttu-id="bccd5-141">toodownload archivu Zip hello diagnostických protokolů, hello použijte následující příkaz z nástroje příkazového řádku Azure hello:</span><span class="sxs-lookup"><span data-stu-id="bccd5-141">toodownload a Zip archive of hello diagnostic logs, use hello following command from hello Azure Command-Line Tools:</span></span>

    azure site log download [sitename]

<span data-ttu-id="bccd5-142">To bude stahovat **diagnostics.zip** v aktuálním adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="bccd5-142">This will download a **diagnostics.zip** in hello current directory.</span></span> <span data-ttu-id="bccd5-143">Archivu obsahuje hello následující adresářovou strukturu:</span><span class="sxs-lookup"><span data-stu-id="bccd5-143">This archive contains hello following directory structure:</span></span>

* <span data-ttu-id="bccd5-144">nasazení – protokolu informace o nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="bccd5-144">deployments - A log of information about deployments of your application</span></span>
* <span data-ttu-id="bccd5-145">LogFiles</span><span class="sxs-lookup"><span data-stu-id="bccd5-145">LogFiles</span></span>
  
  * <span data-ttu-id="bccd5-146">[Metoda nasazení](web-sites-deploy.md) – Pokud používáte metodu nasazení, jako je například Git, adresář hello stejným názvem bude vytvořen a bude obsahují informace související s toodeployments.</span><span class="sxs-lookup"><span data-stu-id="bccd5-146">[Deployment method](web-sites-deploy.md) - If you use a deployment method such as Git, a directory of hello same name will be created and will contain information related toodeployments.</span></span>
  * <span data-ttu-id="bccd5-147">nodejs - Stdout a stderr informace zachyceného v všechny instance aplikace (Pokud loggingEnabled hodnotu true.)</span><span class="sxs-lookup"><span data-stu-id="bccd5-147">nodejs - Stdout and stderr information captured from all instances of your application (when loggingEnabled is true.)</span></span>

### <a name="live-stream-tail"></a><span data-ttu-id="bccd5-148">Živý datový proud (tail)</span><span class="sxs-lookup"><span data-stu-id="bccd5-148">Live stream (tail)</span></span>
<span data-ttu-id="bccd5-149">tooview živý datový proud protokolů diagnostiky informací, hello použijte následující příkaz z nástroje příkazového řádku Azure hello:</span><span class="sxs-lookup"><span data-stu-id="bccd5-149">tooview a live stream of diagnostic log information, use hello following command from hello Azure Command-Line Tools:</span></span>

    azure site log tail [sitename]

<span data-ttu-id="bccd5-150">Tato možnost vrátí datový proud protokolu událostí, které jsou aktualizované, kdy k nim dojde na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="bccd5-150">This will return a stream of log events that are updated as they occur on hello server.</span></span> <span data-ttu-id="bccd5-151">Tento datový proud vrátí informace o nasazení a také stdout a stderr informace (Pokud loggingEnabled hodnotu true.)</span><span class="sxs-lookup"><span data-stu-id="bccd5-151">This stream will return deployment information as well as stdout and stderr information (when loggingEnabled is true.)</span></span>

<a id="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="bccd5-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bccd5-152">Next Steps</span></span>
<span data-ttu-id="bccd5-153">V tomto článku jste se naučili jak tooenable a přístup diagnostické informace pro Azure.</span><span class="sxs-lookup"><span data-stu-id="bccd5-153">In this article you learned how tooenable and access diagnostics information for Azure.</span></span> <span data-ttu-id="bccd5-154">Když tyto informace jsou užitečné v Principy problémy, které ke kterým dochází u vaší aplikace, už může ukazovat tooa problém s modul, který používáte nebo že hello tato verze Node.js používané App Service Web Apps je jiný než hello používá v nasazení prostředí.</span><span class="sxs-lookup"><span data-stu-id="bccd5-154">While this information is useful in understanding problems that occur with your application, it may point tooa problem with a module you are using or that hello version of Node.js used by App Service Web Apps is different than hello one used in your deployment environment.</span></span>

<span data-ttu-id="bccd5-155">Informace v práci s moduly v Azure najdete v tématu [používání modulů Node.js s aplikacemi Azure](../nodejs-use-node-modules-azure-apps.md).</span><span class="sxs-lookup"><span data-stu-id="bccd5-155">For information in working with modules on Azure, see [Using Node.js Modules with Azure Applications](../nodejs-use-node-modules-azure-apps.md).</span></span>

<span data-ttu-id="bccd5-156">Informace o určení verze Node.js pro vaši aplikaci najdete v tématu [určení verze Node.js v aplikaci Azure].</span><span class="sxs-lookup"><span data-stu-id="bccd5-156">For information on specifying a Node.js version for your application, see [Specifying a Node.js version in an Azure application].</span></span>

<span data-ttu-id="bccd5-157">Další informace najdete v tématu taky hello [středisku pro vývojáře Node.js](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="bccd5-157">For more information, see also hello [Node.js Developer Center](/develop/nodejs/).</span></span>

## <a name="whats-changed"></a><span data-ttu-id="bccd5-158">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="bccd5-158">What's changed</span></span>
* <span data-ttu-id="bccd5-159">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="bccd5-159">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

> [!NOTE]
> <span data-ttu-id="bccd5-160">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="bccd5-160">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="bccd5-161">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="bccd5-161">No credit cards required; no commitments.</span></span>
> 
> 

[IISNode]: https://github.com/tjanczuk/iisnode
[modulu IISNode Readme]: https://github.com/tjanczuk/iisnode#readme
[How tooUse hello Azure Command-Line Interface]:../cli-install-nodejs.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[určení verze Node.js v aplikaci Azure]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png

