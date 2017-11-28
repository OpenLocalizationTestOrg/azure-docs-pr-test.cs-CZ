---
title: "aaaUsing tooDev tooPublish skriptů prostředí PowerShell systému Windows a testovací prostředí | Microsoft Docs"
description: "Zjistěte, jak toouse prostředí Windows PowerShell skriptů z Visual Studio toopublish toodevelopment a testovací prostředí."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5fff1301-5469-4d97-be88-c85c30f837c1
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 491a058f96255576afa74f6156f20ae9559bb9f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-windows-powershell-scripts-toopublish-toodev-and-test-environments"></a><span data-ttu-id="eca2b-103">Pomocí prostředí Windows PowerShell skripty toopublish toodev a testovací prostředí</span><span class="sxs-lookup"><span data-stu-id="eca2b-103">Using Windows PowerShell scripts toopublish toodev and test environments</span></span>
<span data-ttu-id="eca2b-104">Když vytvoříte webovou aplikaci v sadě Visual Studio, můžete vygenerovat skript prostředí Windows PowerShell, které můžete později publikování tooautomate hello vašeho webu tooAzure jako webové aplikace ve službě Azure App Service nebo virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="eca2b-104">When you create a web application in Visual Studio, you can generate a Windows PowerShell script that you can use later tooautomate hello publishing of your website tooAzure as a Web App in Azure App Service or a virtual machine.</span></span> <span data-ttu-id="eca2b-105">Můžete upravit a rozšířit své požadavky na skriptu prostředí Windows PowerShell hello v toosuit editor Visual Studio hello nebo skriptu hello integrovat existující sestavení, testování a publikování skripty.</span><span class="sxs-lookup"><span data-stu-id="eca2b-105">You can edit and extend hello Windows PowerShell script in hello Visual Studio editor toosuit your requirements, or integrate hello script with existing build, test, and publishing scripts.</span></span>

<span data-ttu-id="eca2b-106">Pomocí těchto skriptů, můžete zřídit vlastní verze webu pro dočasné použití (také označované jako vývojářů a testovací prostředí).</span><span class="sxs-lookup"><span data-stu-id="eca2b-106">Using these scripts, you can provision customized versions (also known as dev and test environments) of your site for temporary use.</span></span> <span data-ttu-id="eca2b-107">Může například nastavit na konkrétní verzi vašeho webu na virtuálním počítači Azure nebo na hello pracovní patice na webu toorun sady testů, reprodukujte chyby, testovací oprava chyb, zkušební verze navrhované změny nebo nastavit vlastní prostředí pro ukázku nebo prezentace.</span><span class="sxs-lookup"><span data-stu-id="eca2b-107">For example, you might set up a particular version of your website on an Azure virtual machine or on hello staging slot on a website toorun a test suite, reproduce a bug, test a bug fix, trial a proposed change, or set up a custom environment for a demo or presentation.</span></span> <span data-ttu-id="eca2b-108">Po vytvoření skriptu, který publikuje projektu, můžete znovu vytvořit stejná prostředí znovu spuštěním skriptu hello podle potřeby nebo spusťte skript hello s vlastní sestavení vaší webové aplikace toocreate vlastního prostředí pro testování.</span><span class="sxs-lookup"><span data-stu-id="eca2b-108">After you've created a script that publishes your project, you can recreate identical environments by re-running hello script as needed, or run hello script with your own build of your web application toocreate a custom environment for testing.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="eca2b-109">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="eca2b-109">What you need</span></span>
* <span data-ttu-id="eca2b-110">Azure SDK 2.3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="eca2b-110">Azure SDK 2.3 or later.</span></span> <span data-ttu-id="eca2b-111">V tématu [Visual Studio stáhne](http://go.microsoft.com/fwlink/?LinkID=624384) Další informace.</span><span class="sxs-lookup"><span data-stu-id="eca2b-111">See [Visual Studio Downloads](http://go.microsoft.com/fwlink/?LinkID=624384) for more information.</span></span>

<span data-ttu-id="eca2b-112">Není nutné hello Azure SDK toogenerate hello skripty pro webové projekty.</span><span class="sxs-lookup"><span data-stu-id="eca2b-112">You do not need hello Azure SDK toogenerate hello scripts for web projects.</span></span> <span data-ttu-id="eca2b-113">Tato funkce je pro webové projekty, není webové role v cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="eca2b-113">This feature is for web projects, not web roles in cloud services.</span></span>

* <span data-ttu-id="eca2b-114">Prostředí Azure PowerShell 0.7.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="eca2b-114">Azure PowerShell 0.7.4 or later.</span></span> <span data-ttu-id="eca2b-115">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace.</span><span class="sxs-lookup"><span data-stu-id="eca2b-115">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information.</span></span>
* <span data-ttu-id="eca2b-116">[Prostředí Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="eca2b-116">[Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) or later.</span></span>

## <a name="additional-tools"></a><span data-ttu-id="eca2b-117">Další nástroje</span><span class="sxs-lookup"><span data-stu-id="eca2b-117">Additional tools</span></span>
<span data-ttu-id="eca2b-118">Další nástroje a zdroje pro práci s prostředím PowerShell v sadě Visual Studio pro vývoj pro Azure jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="eca2b-118">Additional tools and resources for working with PowerShell in Visual Studio for Azure development are available.</span></span> <span data-ttu-id="eca2b-119">V tématu [prostředí PowerShell nástroje pro sadu Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).</span><span class="sxs-lookup"><span data-stu-id="eca2b-119">See [PowerShell Tools for Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).</span></span>

## <a name="generating-hello-publish-scripts"></a><span data-ttu-id="eca2b-120">Generování hello publikovat skripty</span><span class="sxs-lookup"><span data-stu-id="eca2b-120">Generating hello publish scripts</span></span>
<span data-ttu-id="eca2b-121">Můžete vygenerovat hello publikovat skripty pro virtuální počítač, který je hostitelem vašeho webu, když vytvoříte nový projekt pomocí následujících [tyto pokyny](virtual-machines/windows/classic/web-app-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="eca2b-121">You can generate hello publish scripts for a virtual machine that hosts your website when you create a new project by following [these instructions](virtual-machines/windows/classic/web-app-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="eca2b-122">Můžete také [generovat publikovat skripty pro webové aplikace v Azure App Service](app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="eca2b-122">You can also [generate publish scripts for web apps in Azure App Service](app-service-web/app-service-web-get-started-dotnet.md).</span></span>

## <a name="scripts-that-visual-studio-generates"></a><span data-ttu-id="eca2b-123">Skripty, které generuje Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eca2b-123">Scripts that Visual Studio generates</span></span>
<span data-ttu-id="eca2b-124">Visual Studio vytvoří řešení úrovni složku s názvem **PublishScripts** obsahující dva soubory prostředí Windows PowerShell, skript publikování pro virtuální počítač nebo webu a modul, který obsahuje funkce, které můžete použít v hello skripty.</span><span class="sxs-lookup"><span data-stu-id="eca2b-124">Visual Studio generates a solution-level folder called **PublishScripts** that contains two Windows PowerShell files, a publish script for your virtual machine or website, and a module that contains functions that you can use in hello scripts.</span></span> <span data-ttu-id="eca2b-125">Visual Studio také vygeneruje soubor ve formátu JSON hello, která určuje podrobnosti hello hello projektu, které nasazujete.</span><span class="sxs-lookup"><span data-stu-id="eca2b-125">Visual Studio also generates a file in hello JSON format that specifies hello details of hello project you are deploying.</span></span>

### <a name="windows-powershell-publish-script"></a><span data-ttu-id="eca2b-126">Publikování skriptu prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="eca2b-126">Windows PowerShell publish script</span></span>
<span data-ttu-id="eca2b-127">Hello publikování skript obsahuje konkrétní publikování kroky pro nasazení webu tooa nebo virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="eca2b-127">hello publish script contains specific publish steps for deploying tooa website or virtual machine.</span></span> <span data-ttu-id="eca2b-128">Visual Studio poskytuje pro prostředí Windows PowerShell vývoj zvýrazňování syntaxe.</span><span class="sxs-lookup"><span data-stu-id="eca2b-128">Visual Studio provides syntax coloring for Windows PowerShell development.</span></span> <span data-ttu-id="eca2b-129">Nápověda pro funkce hello je k dispozici, a můžete volně upravit funkce hello ve skriptu toosuit hello proměnlivé potřeby.</span><span class="sxs-lookup"><span data-stu-id="eca2b-129">Help for hello functions is available, and you can freely edit hello functions in hello script toosuit your changing requirements.</span></span>

### <a name="windows-powershell-module"></a><span data-ttu-id="eca2b-130">Modul prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="eca2b-130">Windows PowerShell module</span></span>
<span data-ttu-id="eca2b-131">Hello prostředí Windows PowerShell modul, který generuje Visual Studio obsahuje funkce, které hello publikovat používá skript.</span><span class="sxs-lookup"><span data-stu-id="eca2b-131">hello Windows PowerShell module that Visual Studio generates contains functions that hello publish script uses.</span></span> <span data-ttu-id="eca2b-132">Tyto jsou funkce Azure PowerShell a nejsou určený toobe upravit.</span><span class="sxs-lookup"><span data-stu-id="eca2b-132">These are Azure PowerShell functions and are not intended toobe modified.</span></span> <span data-ttu-id="eca2b-133">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace.</span><span class="sxs-lookup"><span data-stu-id="eca2b-133">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information.</span></span>

### <a name="json-configuration-file"></a><span data-ttu-id="eca2b-134">Konfigurační soubor JSON</span><span class="sxs-lookup"><span data-stu-id="eca2b-134">JSON configuration file</span></span>
<span data-ttu-id="eca2b-135">soubor JSON Hello se vytvoří v hello **konfigurace** složky a obsahuje konfigurační data, která určuje přesně které tooAzure toodeploy prostředky.</span><span class="sxs-lookup"><span data-stu-id="eca2b-135">hello JSON file is created in hello **Configurations** folder and contains configuration data that specifies exactly which resources toodeploy tooAzure.</span></span> <span data-ttu-id="eca2b-136">Název Hello hello souboru, který generuje Visual Studio je projekt název WAWS-dev.json Pokud jste vytvořili na webu nebo projektu název VM-dev.json, pokud jste vytvořili virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="eca2b-136">hello name of hello file that Visual Studio generates is project-name-WAWS-dev.json if you created a website, or project name-VM-dev.json if you created a virtual machine.</span></span> <span data-ttu-id="eca2b-137">Tady je příklad konfiguračního souboru JSON, který se vygeneruje při vytvoření webu.</span><span class="sxs-lookup"><span data-stu-id="eca2b-137">Here's an example of a JSON configuration file that's generated when you create a website.</span></span> <span data-ttu-id="eca2b-138">Většina hodnot hello je není potřeba vysvětlovat.</span><span class="sxs-lookup"><span data-stu-id="eca2b-138">Most of hello values are self-explanatory.</span></span> <span data-ttu-id="eca2b-139">název webu Hello je generován Azure, takže se nemusí odpovídat název projektu.</span><span class="sxs-lookup"><span data-stu-id="eca2b-139">hello website name is generated by Azure, so it might not match your project name.</span></span>

```json
{
    "environmentSettings": {
        "webSite": {
            "name": "WebApplication26632",
            "location": "West US"
        },
        "databases": [{
            "connectionStringName": "DefaultConnection",
            "databaseName": "WebApplication26632_db",
            "serverName": "YourDatabaseServerName",
            "user": "sqluser2",
            "password": "",
            "edition": "",
            "size": "",
            "collation": "",
            "location": "West US"
        }]
    }
}
```
<span data-ttu-id="eca2b-140">Když vytvoříte virtuální počítač, konfigurační soubor JSON hello vypadá podobně jako následující toohello.</span><span class="sxs-lookup"><span data-stu-id="eca2b-140">When you create a virtual machine, hello JSON configuration file looks similar toohello following.</span></span> <span data-ttu-id="eca2b-141">Všimněte si, že Cloudová služba je vytvořen jako kontejner pro hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="eca2b-141">Note that a cloud service is created as a container for hello virtual machine.</span></span> <span data-ttu-id="eca2b-142">Hello virtuální počítač obsahuje hello obvyklé koncové body pro webový přístup prostřednictvím protokolu HTTP a HTTPS, stejně jako koncové body pro nasazení webu, která umožňuje publikovat toohello webu z vašeho místního počítače, vzdálené plochy a prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="eca2b-142">hello virtual machine contains hello usual endpoints for web access through HTTP and HTTPS, as well as endpoints for Web Deploy, which lets you publish toohello website from your local machine, Remote Desktop, and Windows PowerShell.</span></span>

```json
{
    "environmentSettings": {
        "cloudService": {
            "name": "myusernamevm1",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myusernamevm1",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [{
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [{
            "connectionStringName": "",
            "databaseName": "",
            "serverName": "",
            "user": "",
            "password": ""
        }],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

<span data-ttu-id="eca2b-143">Můžete upravit hello JSON konfigurace toochange co se stane, když spustíte hello publikovat skripty.</span><span class="sxs-lookup"><span data-stu-id="eca2b-143">You can edit hello JSON configuration toochange what happens when you run hello publish scripts.</span></span> <span data-ttu-id="eca2b-144">Hello `cloudService` a `virtualMachine` oddíly jsou povinné, ale můžete odstranit hello `databases` části Pokud tomu tak není.</span><span class="sxs-lookup"><span data-stu-id="eca2b-144">hello `cloudService` and `virtualMachine` sections are required, but you can delete hello `databases` section if you don't need it.</span></span> <span data-ttu-id="eca2b-145">Hello vlastnosti, které jsou prázdné v hello výchozí konfigurační soubor, který generuje Visual Studio jsou volitelné. ty, které mají v konfiguračním souboru na hello výchozí hodnoty jsou povinné.</span><span class="sxs-lookup"><span data-stu-id="eca2b-145">hello properties that are empty in hello default configuration file that Visual Studio generates are optional; those that have values in hello default configuration file are required.</span></span>

<span data-ttu-id="eca2b-146">Pokud máte web, který má několik prostředí nasazení (označované jako sloty) místo jednoho pracoviště v Azure, můžete zahrnout název slotu hello v hello název webu hello v konfiguračním souboru JSON hello.</span><span class="sxs-lookup"><span data-stu-id="eca2b-146">If you have a website that has multiple deployment environments (known as slots) instead of a single production site in Azure, you can include hello slot name in hello name of hello website in hello JSON configuration file.</span></span> <span data-ttu-id="eca2b-147">Například, pokud máte web s názvem **server** a slotu pro něj s názvem **testování** pak hello identifikátor URI je server test.cloudapp.net, ale hello správný název toouse v konfiguračním souboru hello je mysite(test) .</span><span class="sxs-lookup"><span data-stu-id="eca2b-147">For example, if you have a website that's named **mysite** and a slot for it named **test** then hello URI is mysite-test.cloudapp.net, but hello correct name toouse in hello configuration file is mysite(test).</span></span> <span data-ttu-id="eca2b-148">Můžete provést jenom to pokud hello webu a sloty již existují v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="eca2b-148">You can only do this if hello website and slots already exist in your subscription.</span></span> <span data-ttu-id="eca2b-149">Pokud ještě neexistují, vytvoření webu hello spuštěním skriptu hello bez zadání hello slot a pak vytvořit hello slot v hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), a poté spusťte skript hello s názvem hello upravené webové stránky.</span><span class="sxs-lookup"><span data-stu-id="eca2b-149">If they don't exist, create hello website by running hello script without specifying hello slot, then create hello slot in hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), and thereafter run hello script with hello modified website name.</span></span> <span data-ttu-id="eca2b-150">Další informace o nasazovací sloty pro webové aplikace najdete v tématu [nastavení přípravných prostředí pro webové aplikace v Azure App Service](app-service-web/web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="eca2b-150">For more information about deployment slots for web apps, see [Set up staging environments for web apps in Azure App Service](app-service-web/web-sites-staged-publishing.md).</span></span>

## <a name="how-toorun-hello-publish-scripts"></a><span data-ttu-id="eca2b-151">Jak publikovat toorun hello skripty</span><span class="sxs-lookup"><span data-stu-id="eca2b-151">How toorun hello publish scripts</span></span>
<span data-ttu-id="eca2b-152">Pokud jste spustili nikdy skript prostředí Windows PowerShell před, musíte nejprve nastavit hello provádění zásad tooenable skripty toorun.</span><span class="sxs-lookup"><span data-stu-id="eca2b-152">If you have never run a Windows PowerShell script before, you must first set hello execution policy tooenable scripts toorun.</span></span> <span data-ttu-id="eca2b-153">Toto je uživatel tooprevent funkce zabezpečení ve spouštění skriptů prostředí Windows PowerShell, pokud jsou snadno napadnutelný toomalware nebo viry, které zahrnují spouštění skriptů.</span><span class="sxs-lookup"><span data-stu-id="eca2b-153">This is a security feature tooprevent users from running Windows PowerShell scripts if they're vulnerable toomalware or viruses that involve executing scripts.</span></span>

### <a name="run-hello-script"></a><span data-ttu-id="eca2b-154">Spusťte skript hello</span><span class="sxs-lookup"><span data-stu-id="eca2b-154">Run hello script</span></span>
1. <span data-ttu-id="eca2b-155">Vytvořte hello balíčku nástroje nasazení webu pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="eca2b-155">Create hello Web Deploy package for your project.</span></span> <span data-ttu-id="eca2b-156">Balíček nasazení webu je komprimovaný archiv (soubor .zip), které obsahují soubory, které chcete toocopy tooyour webu nebo virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="eca2b-156">A Web Deploy package is a compressed archive (.zip file) that contain files that you want toocopy tooyour website or virtual machine.</span></span> <span data-ttu-id="eca2b-157">Balíčky webového nasazení v sadě Visual Studio můžete vytvořit pro všechny webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="eca2b-157">You can create Web Deploy packages in Visual Studio for any web application.</span></span>

![Vytváření webového nasazení balíčku](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

<span data-ttu-id="eca2b-159">Další informace najdete v tématu [postupy: vytvoření balíčku pro nasazení webu v sadě Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span><span class="sxs-lookup"><span data-stu-id="eca2b-159">For more information, see [How to: Create a Web Deployment Package in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span></span> <span data-ttu-id="eca2b-160">Můžete také automatizovat hello vytvoření vašeho balíčku nástroje nasazení webu, jak je popsáno v části hello **přizpůsobení a rozšíření hello publikovat skripty** dál v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="eca2b-160">You can also automate hello creation of your Web Deploy package, as described in hello section **Customizing and extending hello publish scripts** later in this topic.</span></span>

1. <span data-ttu-id="eca2b-161">V **Průzkumníku řešení**, otevřete hello kontextovou nabídku hello skript a potom zvolte **otevřete pomocí prostředí PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="eca2b-161">In **Solution Explorer**, open hello context menu for hello script, and then choose **Open with PowerShell ISE**.</span></span>
2. <span data-ttu-id="eca2b-162">Pokud je to hello poprvé, co jste na tomto počítači spuštěno skriptů prostředí Windows PowerShell, otevřete okno příkazového řádku s oprávněními správce a hello zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="eca2b-162">If this is hello first time you've run Windows PowerShell scripts on this computer, open a command prompt window with Administrator privileges and type hello following command:</span></span>

    ```powershell
    Set-ExecutionPolicy RemoteSigned
    ```

3. <span data-ttu-id="eca2b-163">Přihlaste se tooAzure pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="eca2b-163">Sign in tooAzure by using hello following command.</span></span>

    ```powershell
    Add-AzureAccount
    ```

    <span data-ttu-id="eca2b-164">Po zobrazení výzvy zadejte své uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="eca2b-164">When prompted, supply your username and password.</span></span>

    <span data-ttu-id="eca2b-165">Všimněte si, že při automatizaci hello skriptu, tato metoda poskytnout přihlašovací údaje Azure nebude fungovat.</span><span class="sxs-lookup"><span data-stu-id="eca2b-165">Note that when you automate hello script, this method of providing Azure credentials won't work.</span></span> <span data-ttu-id="eca2b-166">Místo toho používejte hello přihlašovací údaje tooprovide soubor .publishsettings.</span><span class="sxs-lookup"><span data-stu-id="eca2b-166">Instead, you should use hello .publishsettings file tooprovide credentials.</span></span> <span data-ttu-id="eca2b-167">Jednou pouze, můžete použít příkaz hello **Get-AzurePublishSettingsFile** toodownload hello z Azure a následně použít **Import AzurePublishSettingsFile** tooimport hello souboru.</span><span class="sxs-lookup"><span data-stu-id="eca2b-167">One time only, you use hello command **Get-AzurePublishSettingsFile** toodownload hello file from Azure, and thereafter use **Import-AzurePublishSettingsFile** tooimport hello file.</span></span> <span data-ttu-id="eca2b-168">Podrobné pokyny najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="eca2b-168">For detailed instructions, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

4. <span data-ttu-id="eca2b-169">(Volitelné) Pokud chcete, aby toocreate Azure prostředkům, například hello virtuálního počítače, databáze a webu bez publikování webové aplikace, použijte hello **publikovat WebApplication.ps1** s hello **-konfigurace** argument nastaven toohello JSON konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="eca2b-169">(Optional) If you want toocreate Azure resources such as hello virtual machine, database, and website without publishing your web application, use hello **Publish-WebApplication.ps1** command with hello **-Configuration** argument set toohello JSON configuration file.</span></span> <span data-ttu-id="eca2b-170">Tento příkaz používá hello JSON konfigurační soubor toodetermine které toocreate prostředky.</span><span class="sxs-lookup"><span data-stu-id="eca2b-170">This command line uses hello JSON configuration file toodetermine which resources toocreate.</span></span> <span data-ttu-id="eca2b-171">Protože výchozí nastavení hello používá pro další argumenty příkazového řádku, vytvoří hello prostředků, ale nepodporuje publikování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="eca2b-171">Because it uses hello default settings for other command-line arguments, it creates hello resources, but doesn't publish your web application.</span></span> <span data-ttu-id="eca2b-172">Hello – podrobné možnost získáte další informace o co se děje.</span><span class="sxs-lookup"><span data-stu-id="eca2b-172">hello –Verbose option gives you more information about what's happening.</span></span>

    ```powershell
    Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json
    ```

5. <span data-ttu-id="eca2b-173">Použití hello **publikovat WebApplication.ps1** příkaz, jak je znázorněno v jednom z hello následující příklady tooinvoke hello skriptu a publikování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="eca2b-173">Use hello **Publish-WebApplication.ps1** command as shown in one of hello following examples tooinvoke hello script and publish your web application.</span></span> <span data-ttu-id="eca2b-174">Pokud potřebujete toooverride hello výchozí nastavení pro všechny hello další argumenty, jako je například název odběru hello, publikování název balíčku, přihlašovací údaje virtuálního počítače nebo přihlašovací údaje databáze serveru, můžete určit tyto parametry.</span><span class="sxs-lookup"><span data-stu-id="eca2b-174">If you need toooverride hello default settings for any of hello other arguments, such as hello subscription name, publish package name, virtual machine credentials, or database server credentials, you can specify those parameters.</span></span> <span data-ttu-id="eca2b-175">Použití hello **– podrobný** možnost toosee Další informace o průběhu hello hello procesu publikování.</span><span class="sxs-lookup"><span data-stu-id="eca2b-175">Use hello **–Verbose** option toosee more information about hello progress of hello publishing process.</span></span>

    ```powershell
    Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
    –SubscriptionName Contoso `
    -WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
    -DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
    -Verbose
    ```

    <span data-ttu-id="eca2b-176">Pokud vytváříte virtuální počítač, hello příkaz vypadat jako následující hello.</span><span class="sxs-lookup"><span data-stu-id="eca2b-176">If you're creating a virtual machine, hello command looks like hello following.</span></span> <span data-ttu-id="eca2b-177">Tento příklad také ukazuje, jak toospecify hello přihlašovací údaje pro více databází.</span><span class="sxs-lookup"><span data-stu-id="eca2b-177">This example also shows how toospecify hello credentials for multiple databases.</span></span> <span data-ttu-id="eca2b-178">Hello virtuálních počítačů, které vytvářejí tyto skripty není certifikát SSL hello z důvěryhodnou kořenovou autoritou.</span><span class="sxs-lookup"><span data-stu-id="eca2b-178">For hello virtual machines that these scripts create, hello SSL certificate is not from a trusted root authority.</span></span> <span data-ttu-id="eca2b-179">Proto je nutné toouse hello **– AllowUntrusted** možnost.</span><span class="sxs-lookup"><span data-stu-id="eca2b-179">Therefore, you need toouse hello **–AllowUntrusted** option.</span></span>

    ```powershell
    Publish-WebApplication.ps1 `
    -Configuration C:\Path\ADVM-VM-test.json `
    -SubscriptionName Contoso `
    -WebDeployPackage C:\Path\ADVM.zip `
    -AllowUntrusted `
    -VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
    -DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
    -Verbose
    ```

    <span data-ttu-id="eca2b-180">databáze můžete vytvářet Hello skript, ale nevytvoří databázové servery.</span><span class="sxs-lookup"><span data-stu-id="eca2b-180">hello script can create databases, but it doesn't create database servers.</span></span> <span data-ttu-id="eca2b-181">Pokud chcete, aby toocreate databázový server, můžete použít hello **New-AzureSqlDatabaseServer** funkce v hello Azure modulu.</span><span class="sxs-lookup"><span data-stu-id="eca2b-181">If you want toocreate a database server, you can use hello **New-AzureSqlDatabaseServer** function in hello Azure module.</span></span>

## <a name="customizing-and-extending-hello-publish-scripts"></a><span data-ttu-id="eca2b-182">Přizpůsobení a rozšíření hello publikování skripty</span><span class="sxs-lookup"><span data-stu-id="eca2b-182">Customizing and extending hello publish scripts</span></span>
<span data-ttu-id="eca2b-183">Můžete přizpůsobit hello publikovat skriptu a konfigurační soubor JSON.</span><span class="sxs-lookup"><span data-stu-id="eca2b-183">You can customize hello publish script and JSON configuration file.</span></span> <span data-ttu-id="eca2b-184">Hello funkce v modulu Windows PowerShell hello **AzureWebAppPublishModule.psm1** nejsou určený toobe upravit.</span><span class="sxs-lookup"><span data-stu-id="eca2b-184">hello functions in hello Windows PowerShell module **AzureWebAppPublishModule.psm1** are not intended toobe modified.</span></span> <span data-ttu-id="eca2b-185">Chcete-li právě toospecify jiné databázi nebo změnit některé vlastnosti hello hello virtuálního počítače, upravte konfigurační soubor JSON hello.</span><span class="sxs-lookup"><span data-stu-id="eca2b-185">If you just want toospecify a different database or change some of hello properties of hello virtual machine, edit hello JSON configuration file.</span></span> <span data-ttu-id="eca2b-186">Pokud chcete tooextend hello funkce hello skriptu tooautomate vytváření a testování projektu, můžete implementovat funkce zástupných procedur v **publikovat WebApplication.ps1**.</span><span class="sxs-lookup"><span data-stu-id="eca2b-186">If you want tooextend hello functionality of hello script tooautomate building and testing your project, you can implement function stubs in **Publish-WebApplication.ps1**.</span></span>

<span data-ttu-id="eca2b-187">tooautomate sestavení projektu, přidat kód, který volá MSBuild příliš`New-WebDeployPackage` jak ukazuje tento příklad kódu.</span><span class="sxs-lookup"><span data-stu-id="eca2b-187">tooautomate building your project, add code that calls MSBuild too`New-WebDeployPackage` as shown in this code example.</span></span> <span data-ttu-id="eca2b-188">Hello cesta toohello příkaz MSBuild se liší v závislosti na verzi hello sady Visual Studio, které jste nainstalovali.</span><span class="sxs-lookup"><span data-stu-id="eca2b-188">hello path toohello MSBuild command is different depending on hello version of Visual Studio you have installed.</span></span> <span data-ttu-id="eca2b-189">tooget hello správnou cestu, můžete použít funkce hello **Get-MSBuildCmd**, jak je uvedeno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="eca2b-189">tooget hello correct path, you can use hello function **Get-MSBuildCmd**, as shown in this example.</span></span>

### <a name="tooautomate-building-your-project"></a><span data-ttu-id="eca2b-190">tooautomate sestavení projektu</span><span class="sxs-lookup"><span data-stu-id="eca2b-190">tooautomate building your project</span></span>
1. <span data-ttu-id="eca2b-191">Přidat hello `$ProjectFile` parametru v části globální param hello.</span><span class="sxs-lookup"><span data-stu-id="eca2b-191">Add hello `$ProjectFile` parameter in hello global param section.</span></span>

    ```powershell
    [Parameter(Mandatory = $false)]
    [ValidateScript({Test-Path $_ -PathType Leaf})]
    [String]
    $ProjectFile,
    ```

2. <span data-ttu-id="eca2b-192">Copy – funkce hello `Get-MSBuildCmd` do souboru skriptu.</span><span class="sxs-lookup"><span data-stu-id="eca2b-192">Copy hello function `Get-MSBuildCmd` into your script file.</span></span>

    ```powershell
    function Get-MSBuildCmd
    {
            process
    {

                $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                    Sort-Object {[double]$_.PSChildName} -Descending |
                                    Select-Object -First 1 |
                                    Get-ItemProperty -Name MSBuildToolsPath |
                                    Select -ExpandProperty MSBuildToolsPath

                $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

            return Get-Item $path
        }
    }
    ```

3. <span data-ttu-id="eca2b-193">Nahraďte `New-WebDeployPackage` s hello následující kód a nahraďte zástupné symboly hello při vytváření řádku hello `$msbuildCmd`.</span><span class="sxs-lookup"><span data-stu-id="eca2b-193">Replace `New-WebDeployPackage` with hello following code and replace hello placeholders in hello line constructing `$msbuildCmd`.</span></span> <span data-ttu-id="eca2b-194">Tento kód je pro Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="eca2b-194">This code is for Visual Studio 2015.</span></span> <span data-ttu-id="eca2b-195">Pokud používáte Visual Studio 2013, změňte hello **VisualStudioVersion** vlastnost níže příliš`12.0`.</span><span class="sxs-lookup"><span data-stu-id="eca2b-195">If you're using Visual Studio 2013, change hello **VisualStudioVersion** property below too`12.0`.</span></span>

    ```powershell
    function New-WebDeployPackage
    {
        #Write a function toobuild and package your web application
    ```

    <span data-ttu-id="eca2b-196">toobuild vaší webové aplikace, použijte MsBuild.exe.</span><span class="sxs-lookup"><span data-stu-id="eca2b-196">toobuild your web application, use MsBuild.exe.</span></span> <span data-ttu-id="eca2b-197">Nápovědu najdete v tématu Reference k příkazovému řádku nástroje MSBuild v: [http://go.microsoft.com/fwlink/?LinkId=391339](http://go.microsoft.com/fwlink/?LinkId=391339)</span><span class="sxs-lookup"><span data-stu-id="eca2b-197">For help, see MSBuild Command-Line Reference at: [http://go.microsoft.com/fwlink/?LinkId=391339](http://go.microsoft.com/fwlink/?LinkId=391339)</span></span>

    ```powershell
    Write-VerboseWithTime 'Build-WebDeployPackage: Start'

    $msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=14.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory

    Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
    ```

### <a name="start-execution-of-hello-build-command"></a><span data-ttu-id="eca2b-198">Spustit provádění příkazu hello sestavení</span><span class="sxs-lookup"><span data-stu-id="eca2b-198">Start execution of hello build command</span></span>

```powershell
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru

if ($job.ExitCode -ne 0) {
    throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain hello project name
$projectName = (Get-Item $ProjectFile).BaseName

#Construct hello path tooweb deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName

#Get hello full path for hello web deploy zip package. This is required for MSDeploy toowork
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir

Write-VerboseWithTime 'Build-WebDeployPackage: End'

return $WebDeployPackage
}
```

1. <span data-ttu-id="eca2b-199">Volání hello `New-WebDeployPackage` funkce před tento řádek: `$Config = Read-ConfigFile $Configuration` pro webové aplikace nebo `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="eca2b-199">Call hello `New-WebDeployPackage` function before this line: `$Config = Read-ConfigFile $Configuration` for web apps or `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` for virtual machines.</span></span>

    ```powershell
    if($ProjectFile)
    {
    $WebDeployPackage = New-WebDeployPackage
    }
    ```

2. <span data-ttu-id="eca2b-200">Vyvolání hello přizpůsobit skript z příkazového řádku pomocí předávání hello `$Project` argument jako hello následující příklad příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="eca2b-200">Invoke hello customized script from command line using passing hello `$Project` argument, as in hello following example command line.</span></span>

    ```powershell
    .\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
    -ProjectFile ..\WebApplication5\WebApplication5.csproj `
    -VMPassword @{Name="VMUser";Password="Test.123"} `
    -AllowUntrusted `
    -Verbose
    ```

    <span data-ttu-id="eca2b-201">tooautomate testování vaší aplikace, přidejte kód příliš`Test-WebApplication`.</span><span class="sxs-lookup"><span data-stu-id="eca2b-201">tooautomate testing of your application, add code too`Test-WebApplication`.</span></span> <span data-ttu-id="eca2b-202">Být jisti toouncomment hello řádků v **publikovat WebApplication.ps1** kde jsou tyto funkce volána.</span><span class="sxs-lookup"><span data-stu-id="eca2b-202">Be sure toouncomment hello lines in **Publish-WebApplication.ps1** where these functions are called.</span></span> <span data-ttu-id="eca2b-203">Pokud nezadáte implementace, můžete ručně vytvořit projekt pomocí sady Visual Studio a potom spusťte hello publikovat tooAzure toopublish skriptu.</span><span class="sxs-lookup"><span data-stu-id="eca2b-203">If you don't provide an implementation, you can manually build your project with Visual Studio, and then run hello publish script toopublish tooAzure.</span></span>

## <a name="publishing-function-summary"></a><span data-ttu-id="eca2b-204">Publikování souhrn funkcí</span><span class="sxs-lookup"><span data-stu-id="eca2b-204">Publishing function summary</span></span>
<span data-ttu-id="eca2b-205">tooget nápovědy pro funkce, které můžete použít na příkazovém řádku prostředí Windows PowerShell text hello, použijte příkaz hello `Get-Help function-name`.</span><span class="sxs-lookup"><span data-stu-id="eca2b-205">tooget help for functions you can use at hello Windows PowerShell command prompt, use hello command `Get-Help function-name`.</span></span> <span data-ttu-id="eca2b-206">Nápověda Hello obsahuje příklady a nápovědu parametr.</span><span class="sxs-lookup"><span data-stu-id="eca2b-206">hello help includes parameter help and examples.</span></span> <span data-ttu-id="eca2b-207">Hello stejný text nápovědy je taky v hello skriptu zdrojové soubory, **AzureWebAppPublishModule.psm1** a **publikovat WebApplication.ps1**.</span><span class="sxs-lookup"><span data-stu-id="eca2b-207">hello same help text is also in hello script source files, **AzureWebAppPublishModule.psm1** and **Publish-WebApplication.ps1**.</span></span> <span data-ttu-id="eca2b-208">v jazyce Visual Studio jsou lokalizované Hello skriptu a Nápověda.</span><span class="sxs-lookup"><span data-stu-id="eca2b-208">hello script and help are localized in your Visual Studio language.</span></span>

<span data-ttu-id="eca2b-209">**AzureWebAppPublishModule**</span><span class="sxs-lookup"><span data-stu-id="eca2b-209">**AzureWebAppPublishModule**</span></span>

| <span data-ttu-id="eca2b-210">Název funkce</span><span class="sxs-lookup"><span data-stu-id="eca2b-210">Function name</span></span> | <span data-ttu-id="eca2b-211">Popis</span><span class="sxs-lookup"><span data-stu-id="eca2b-211">Description</span></span> |
| --- | --- |
| <span data-ttu-id="eca2b-212">Přidání azuresqldatabase.</span><span class="sxs-lookup"><span data-stu-id="eca2b-212">Add-AzureSQLDatabase</span></span> |<span data-ttu-id="eca2b-213">Vytvoří novou databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="eca2b-213">Creates a new Azure SQL database.</span></span> |
| <span data-ttu-id="eca2b-214">Přidat AzureSQLDatabases</span><span class="sxs-lookup"><span data-stu-id="eca2b-214">Add-AzureSQLDatabases</span></span> |<span data-ttu-id="eca2b-215">Vytvoří databáze Azure SQL z hodnot hello ve hello konfigurační soubor JSON, který generuje Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eca2b-215">Creates Azure SQL databases from hello values in hello JSON configuration file that Visual Studio generates.</span></span> |
| <span data-ttu-id="eca2b-216">Přidat AzureVM</span><span class="sxs-lookup"><span data-stu-id="eca2b-216">Add-AzureVM</span></span> |<span data-ttu-id="eca2b-217">Vytvoří virtuální počítač Azure a vrátí hodnotu že Hello URL hello nasazení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="eca2b-217">Creates a Azure virtual machine and returns hello URL of hello deployed VM.</span></span> <span data-ttu-id="eca2b-218">Hello funkce nastaví hello požadavků a pak volání hello **New-AzureVM** funkce toocreate (Azure modul) nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="eca2b-218">hello function sets up hello prerequisites and then calls hello **New-AzureVM** function (Azure module) toocreate a new virtual machine.</span></span> |
| <span data-ttu-id="eca2b-219">Přidat AzureVMEndpoints</span><span class="sxs-lookup"><span data-stu-id="eca2b-219">Add-AzureVMEndpoints</span></span> |<span data-ttu-id="eca2b-220">Přidá nový virtuální počítač tooa vstupních koncových bodů a vrátí hello virtuální počítač s hello nový koncový bod.</span><span class="sxs-lookup"><span data-stu-id="eca2b-220">Adds new input endpoints tooa virtual machine and returns hello virtual machine with hello new endpoint.</span></span> |
| <span data-ttu-id="eca2b-221">Přidat AzureVMStorage</span><span class="sxs-lookup"><span data-stu-id="eca2b-221">Add-AzureVMStorage</span></span> |<span data-ttu-id="eca2b-222">Vytvoří nový účet úložiště Azure v aktuálním předplatném hello.</span><span class="sxs-lookup"><span data-stu-id="eca2b-222">Creates a new Azure storage account in hello current subscription.</span></span> <span data-ttu-id="eca2b-223">Hello název účtu hello začíná "devtest", za nímž následuje jedinečný alfanumerický řetězec.</span><span class="sxs-lookup"><span data-stu-id="eca2b-223">hello name of hello account begins with "devtest" followed by a unique alphanumeric string.</span></span> <span data-ttu-id="eca2b-224">Funkce Hello vrací hello název nového účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="eca2b-224">hello function returns hello name of hello new storage account.</span></span> <span data-ttu-id="eca2b-225">Musíte zadat umístění nebo skupina vztahů pro nový účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="eca2b-225">You must specify either a location or an affinity group for hello new storage account.</span></span> |
| <span data-ttu-id="eca2b-226">Přidat AzureWebsite</span><span class="sxs-lookup"><span data-stu-id="eca2b-226">Add-AzureWebsite</span></span> |<span data-ttu-id="eca2b-227">Vytvoří web s hello zadaný název a umístění.</span><span class="sxs-lookup"><span data-stu-id="eca2b-227">Creates a website with hello specified name and location.</span></span> <span data-ttu-id="eca2b-228">Tato funkce volá hello **New-AzureWebsite** funkce v hello Azure modulu.</span><span class="sxs-lookup"><span data-stu-id="eca2b-228">This function calls hello **New-AzureWebsite** function in hello Azure module.</span></span> <span data-ttu-id="eca2b-229">Pokud hello předplatné už neobsahuje web se zadaným názvem hello, tato funkce vytvoří hello web a vrátí objekt webu.</span><span class="sxs-lookup"><span data-stu-id="eca2b-229">If hello subscription doesn't already include a website with hello specified name, this function creates hello website and returns a website object.</span></span> <span data-ttu-id="eca2b-230">Funkce `$null`.</span><span class="sxs-lookup"><span data-stu-id="eca2b-230">Otherwise, it returns `$null`.</span></span> |
| <span data-ttu-id="eca2b-231">Zálohování předplatného</span><span class="sxs-lookup"><span data-stu-id="eca2b-231">Backup-Subscription</span></span> |<span data-ttu-id="eca2b-232">Uloží hello aktuální předplatné Azure v hello `$Script:originalSubscription` proměnné v oboru skriptu. Tato funkce uloží aktuální předplatné Azure hello (jak získat `Get-AzureSubscription -Current`) a jeho účet úložiště a hello předplatné, které mění tímto skriptem (uložené v proměnné hello `$UserSpecifiedSubscription`) a jeho účet úložiště, v oboru skriptu.</span><span class="sxs-lookup"><span data-stu-id="eca2b-232">Saves hello current Azure subscription in hello `$Script:originalSubscription` variable in script scope.This function saves hello current Azure subscription (as obtained by `Get-AzureSubscription -Current`) and its storage account, and hello subscription that is changed by this script (stored in hello variable `$UserSpecifiedSubscription`) and its storage account, in script scope.</span></span> <span data-ttu-id="eca2b-233">Ukládání hello hodnoty, můžete pomocí funkce, jako například `Restore-Subscription`, toorestore hello původní aktuální předplatné a úložiště toocurrent stav účtu pokud došlo ke změně aktuální stav hello.</span><span class="sxs-lookup"><span data-stu-id="eca2b-233">By saving hello values, you can use a function, such as `Restore-Subscription`, toorestore hello original current subscription and storage account toocurrent status if hello current status has changed.</span></span> |
| <span data-ttu-id="eca2b-234">Najít AzureVM</span><span class="sxs-lookup"><span data-stu-id="eca2b-234">Find-AzureVM</span></span> |<span data-ttu-id="eca2b-235">Získá hello zadaný virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="eca2b-235">Gets hello specified Azure virtual machine.</span></span> |
| <span data-ttu-id="eca2b-236">Formát DevTestMessageWithTime</span><span class="sxs-lookup"><span data-stu-id="eca2b-236">Format-DevTestMessageWithTime</span></span> |<span data-ttu-id="eca2b-237">Přidá zprávy tooa hello datum a čas.</span><span class="sxs-lookup"><span data-stu-id="eca2b-237">Prepends hello date and time tooa message.</span></span> <span data-ttu-id="eca2b-238">Tato funkce je určená pro zpráv zapsaných toohello chyba a podrobná datových proudů.</span><span class="sxs-lookup"><span data-stu-id="eca2b-238">This function is designed for messages written toohello Error and Verbose streams.</span></span> |
| <span data-ttu-id="eca2b-239">Get-AzureSQLDatabaseConnectionString</span><span class="sxs-lookup"><span data-stu-id="eca2b-239">Get-AzureSQLDatabaseConnectionString</span></span> |<span data-ttu-id="eca2b-240">Sestaví připojovací řetězec tooconnect tooan Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="eca2b-240">Assembles a connection string tooconnect tooan Azure SQL database.</span></span> |
| <span data-ttu-id="eca2b-241">Get-AzureVMStorage</span><span class="sxs-lookup"><span data-stu-id="eca2b-241">Get-AzureVMStorage</span></span> |<span data-ttu-id="eca2b-242">Vrátí hello název hello první účet úložiště se stejným vzor názvu hello "devtest*" (malá a velká písmena) v zadané umístění nebo vztahů skupiny hello. Pokud hello "devtest*" účet úložiště se neshoduje se hello umístění nebo skupina vztahů, funkce hello ji ignoruje.</span><span class="sxs-lookup"><span data-stu-id="eca2b-242">Returns hello name of hello first storage account with hello name pattern "devtest*" (case insensitive) in hello specified location or affinity group. If hello "devtest*" storage account doesn't match hello location or affinity group, hello function ignores it.</span></span> <span data-ttu-id="eca2b-243">Musíte zadat umístění nebo skupině vztahů.</span><span class="sxs-lookup"><span data-stu-id="eca2b-243">You must specify either a location or an affinity group.</span></span> |
| <span data-ttu-id="eca2b-244">Get-MSDeployCmd</span><span class="sxs-lookup"><span data-stu-id="eca2b-244">Get-MSDeployCmd</span></span> |<span data-ttu-id="eca2b-245">Vrátí příkaz toorun hello MsDeploy.exe nástroj.</span><span class="sxs-lookup"><span data-stu-id="eca2b-245">Returns a command toorun hello MsDeploy.exe tool.</span></span> |
| <span data-ttu-id="eca2b-246">Nové AzureVMEnvironment</span><span class="sxs-lookup"><span data-stu-id="eca2b-246">New-AzureVMEnvironment</span></span> |<span data-ttu-id="eca2b-247">Vyhledá nebo vytvoří virtuální počítač v rámci předplatného hello, který odpovídá hello hodnoty v konfiguračním souboru JSON hello.</span><span class="sxs-lookup"><span data-stu-id="eca2b-247">Finds or creates a virtual machine in hello subscription that matches hello values in hello JSON configuration file.</span></span> |
| <span data-ttu-id="eca2b-248">Publikování WebPackage</span><span class="sxs-lookup"><span data-stu-id="eca2b-248">Publish-WebPackage</span></span> |<span data-ttu-id="eca2b-249">Používá MsDeploy.exe a webové publikování balíčku. ZIP souboru toodeploy prostředky tooa webu.</span><span class="sxs-lookup"><span data-stu-id="eca2b-249">Uses MsDeploy.exe and a web publish package .Zip file toodeploy resources tooa website.</span></span> <span data-ttu-id="eca2b-250">Tato funkce negeneruje žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="eca2b-250">This function doesn't generate any output.</span></span> <span data-ttu-id="eca2b-251">V případě selhání hello volání tooMSDeploy.exe hello funkce vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="eca2b-251">If hello call tooMSDeploy.exe fails, hello function throws an exception.</span></span> <span data-ttu-id="eca2b-252">tooget více podrobný výstup, použijte hello **-Verbose** možnost.</span><span class="sxs-lookup"><span data-stu-id="eca2b-252">tooget more detailed output, use hello **-Verbose** option.</span></span> |
| <span data-ttu-id="eca2b-253">Publikování WebPackageToVM</span><span class="sxs-lookup"><span data-stu-id="eca2b-253">Publish-WebPackageToVM</span></span> |<span data-ttu-id="eca2b-254">Ověří hello hodnoty parametrů a pak zavolá hello **publikovat WebPackage** funkce.</span><span class="sxs-lookup"><span data-stu-id="eca2b-254">Verifies hello parameter values, and then calls hello **Publish-WebPackage** function.</span></span> |
| <span data-ttu-id="eca2b-255">ConfigFile pro čtení</span><span class="sxs-lookup"><span data-stu-id="eca2b-255">Read-ConfigFile</span></span> |<span data-ttu-id="eca2b-256">Ověří hello JSON konfigurační soubor a vrátí hodnotu hash tabulku vybraných hodnot.</span><span class="sxs-lookup"><span data-stu-id="eca2b-256">Validates hello JSON configuration file and returns a hash table of selected values.</span></span> |
| <span data-ttu-id="eca2b-257">Obnovení předplatného</span><span class="sxs-lookup"><span data-stu-id="eca2b-257">Restore-Subscription</span></span> |<span data-ttu-id="eca2b-258">Obnoví hello aktuální předplatné toohello původního předplatného.</span><span class="sxs-lookup"><span data-stu-id="eca2b-258">Resets hello current subscription toohello original subscription.</span></span> |
| <span data-ttu-id="eca2b-259">Test AzureModule</span><span class="sxs-lookup"><span data-stu-id="eca2b-259">Test-AzureModule</span></span> |<span data-ttu-id="eca2b-260">Vrátí `$true` Pokud hello nainstalovaná verze modulu Azure 0.7.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="eca2b-260">Returns `$true` if hello installed Azure module version is 0.7.4 or later.</span></span> <span data-ttu-id="eca2b-261">Vrátí `$false` Pokud hello modul není nainstalován nebo je starší verze.</span><span class="sxs-lookup"><span data-stu-id="eca2b-261">Returns `$false` if hello module isn't installed or is an earlier version.</span></span> <span data-ttu-id="eca2b-262">Tato funkce nemá žádné parametry.</span><span class="sxs-lookup"><span data-stu-id="eca2b-262">This function has no parameters.</span></span> |
| <span data-ttu-id="eca2b-263">Test AzureModuleVersion</span><span class="sxs-lookup"><span data-stu-id="eca2b-263">Test-AzureModuleVersion</span></span> |<span data-ttu-id="eca2b-264">Vrátí `$true` Pokud hello verze hello Azure modulu 0.7.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="eca2b-264">Returns `$true` if hello version of hello Azure module is 0.7.4 or later.</span></span> <span data-ttu-id="eca2b-265">Vrátí `$false` Pokud hello modul není nainstalován nebo je starší verze.</span><span class="sxs-lookup"><span data-stu-id="eca2b-265">Returns `$false` if hello module isn't installed or is an earlier version.</span></span> <span data-ttu-id="eca2b-266">Tato funkce nemá žádné parametry.</span><span class="sxs-lookup"><span data-stu-id="eca2b-266">This function has no parameters.</span></span> |
| <span data-ttu-id="eca2b-267">Test HttpsUrl</span><span class="sxs-lookup"><span data-stu-id="eca2b-267">Test-HttpsUrl</span></span> |<span data-ttu-id="eca2b-268">Převede objekt System.Uri tooa vstupní adresy URL hello.</span><span class="sxs-lookup"><span data-stu-id="eca2b-268">Converts hello input URL tooa System.Uri object.</span></span> <span data-ttu-id="eca2b-269">Vrátí `$True` Pokud adresa URL hello absolutní a její schéma je protokol https.</span><span class="sxs-lookup"><span data-stu-id="eca2b-269">Returns `$True` if hello URL is absolute and its scheme is https.</span></span> <span data-ttu-id="eca2b-270">Vrátí `$false` hello adresa URL je relativní, jeho schématu není HTTPS, nebo hello vstupní řetězec nemůže být převedená tooa adresy URL.</span><span class="sxs-lookup"><span data-stu-id="eca2b-270">Returns `$false` if hello URL is relative, its scheme isn't HTTPS, or hello input string can't be converted tooa URL.</span></span> |
| <span data-ttu-id="eca2b-271">Test člena</span><span class="sxs-lookup"><span data-stu-id="eca2b-271">Test-Member</span></span> |<span data-ttu-id="eca2b-272">Vrátí `$true` Pokud vlastnosti nebo metody je členem objektu hello.</span><span class="sxs-lookup"><span data-stu-id="eca2b-272">Returns `$true` if a property or method is a member of hello object.</span></span> <span data-ttu-id="eca2b-273">Jinak vrátí `$false`.</span><span class="sxs-lookup"><span data-stu-id="eca2b-273">Otherwise, returns `$false`.</span></span> |
| <span data-ttu-id="eca2b-274">Zápis ErrorWithTime</span><span class="sxs-lookup"><span data-stu-id="eca2b-274">Write-ErrorWithTime</span></span> |<span data-ttu-id="eca2b-275">Zapíše chybovou zprávu předponu hello aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="eca2b-275">Writes an error message prefixed with hello current time.</span></span> <span data-ttu-id="eca2b-276">Tato funkce volá hello **formátu DevTestMessageWithTime** funkce tooprepend hello čas před zápisem hello zpráva toohello chybový proud.</span><span class="sxs-lookup"><span data-stu-id="eca2b-276">This function calls hello **Format-DevTestMessageWithTime** function tooprepend hello time before writing hello message toohello Error stream.</span></span> |
| <span data-ttu-id="eca2b-277">Zápis HostWithTime</span><span class="sxs-lookup"><span data-stu-id="eca2b-277">Write-HostWithTime</span></span> |<span data-ttu-id="eca2b-278">Zapíše zpráva toohello hostitele program (**Write-Host**) předponu hello aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="eca2b-278">Writes a message toohello host program (**Write-Host**) prefixed with hello current time.</span></span> <span data-ttu-id="eca2b-279">zápis toohello hostitelského programu Hello účinek se liší.</span><span class="sxs-lookup"><span data-stu-id="eca2b-279">hello effect of writing toohello host program varies.</span></span> <span data-ttu-id="eca2b-280">Většiny programů tohoto hostitele prostředí Windows PowerShell zápisu tyto zprávy toostandard výstupu.</span><span class="sxs-lookup"><span data-stu-id="eca2b-280">Most programs that host Windows PowerShell write these messages toostandard output.</span></span> |
| <span data-ttu-id="eca2b-281">Zápis VerboseWithTime</span><span class="sxs-lookup"><span data-stu-id="eca2b-281">Write-VerboseWithTime</span></span> |<span data-ttu-id="eca2b-282">Zapíše podrobnou zprávu předponu hello aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="eca2b-282">Writes a verbose message prefixed with hello current time.</span></span> <span data-ttu-id="eca2b-283">Vzhledem k tomu, že zavolá **Write-Verbose**, uvítací zprávu zobrazí, jenom když hello skript se spustí s hello **podrobné** parametr nebo když text hello **VerbosePreference** je předvoleb Nastavení příliš**pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="eca2b-283">Because it calls **Write-Verbose**, hello message displays only when hello script runs with hello **Verbose** parameter or when hello **VerbosePreference** preference is set too**Continue**.</span></span> |

<span data-ttu-id="eca2b-284">**Publikovat webovou aplikaci**</span><span class="sxs-lookup"><span data-stu-id="eca2b-284">**Publish-WebApplication**</span></span>

| <span data-ttu-id="eca2b-285">Název funkce</span><span class="sxs-lookup"><span data-stu-id="eca2b-285">Function name</span></span> | <span data-ttu-id="eca2b-286">Popis</span><span class="sxs-lookup"><span data-stu-id="eca2b-286">Description</span></span> |
| --- | --- |
| <span data-ttu-id="eca2b-287">Nové AzureWebApplicationEnvironment</span><span class="sxs-lookup"><span data-stu-id="eca2b-287">New-AzureWebApplicationEnvironment</span></span> |<span data-ttu-id="eca2b-288">Vytvoří prostředky Azure, jako je web nebo virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="eca2b-288">Creates Azure resources, such as a website or virtual machine.</span></span> |
| <span data-ttu-id="eca2b-289">Nové WebDeployPackage</span><span class="sxs-lookup"><span data-stu-id="eca2b-289">New-WebDeployPackage</span></span> |<span data-ttu-id="eca2b-290">Tato funkce není implementována.</span><span class="sxs-lookup"><span data-stu-id="eca2b-290">This function isn't implemented.</span></span> <span data-ttu-id="eca2b-291">Příkazy můžete přidat v této funkce toobuild projektu.</span><span class="sxs-lookup"><span data-stu-id="eca2b-291">You can add commands in this function toobuild your project.</span></span> |
| <span data-ttu-id="eca2b-292">Publikování AzureWebApplication</span><span class="sxs-lookup"><span data-stu-id="eca2b-292">Publish-AzureWebApplication</span></span> |<span data-ttu-id="eca2b-293">Publikuje tooAzure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="eca2b-293">Publishes a web application tooAzure.</span></span> |
| <span data-ttu-id="eca2b-294">Publikovat webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="eca2b-294">Publish-WebApplication</span></span> |<span data-ttu-id="eca2b-295">Vytvoří a nasadí webových aplikací, virtuálních počítačů, databází SQL a účty úložiště pro webový projekt sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eca2b-295">Creates and deploys Web Apps, virtual machines, SQL databases, and storage accounts for a Visual Studio web project.</span></span> |
| <span data-ttu-id="eca2b-296">Test-WebApplication</span><span class="sxs-lookup"><span data-stu-id="eca2b-296">Test-WebApplication</span></span> |<span data-ttu-id="eca2b-297">Tato funkce není implementována.</span><span class="sxs-lookup"><span data-stu-id="eca2b-297">This function isn't implemented.</span></span> <span data-ttu-id="eca2b-298">Příkazy můžete přidat v této funkce tootest vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="eca2b-298">You can add commands in this function tootest your application.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="eca2b-299">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eca2b-299">Next steps</span></span>
<span data-ttu-id="eca2b-300">Další informace o prostředí PowerShell skriptování čtení [skriptování v prostředí Windows PowerShell](https://technet.microsoft.com/library/bb978526.aspx) a jiné skripty prostředí Azure PowerShell v hello [centra skriptů](https://azure.microsoft.com/documentation/scripts/).</span><span class="sxs-lookup"><span data-stu-id="eca2b-300">Learn more about PowerShell scripting by reading [Scripting with Windows PowerShell](https://technet.microsoft.com/library/bb978526.aspx) and see other Azure PowerShell scripts at hello [Script Center](https://azure.microsoft.com/documentation/scripts/).</span></span>
