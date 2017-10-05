---
title: "Pomocí skriptů prostředí PowerShell systému Windows k publikování pro vývojáře a testovací prostředí | Microsoft Docs"
description: "Další informace o použití skriptů prostředí Windows PowerShell ze sady Visual Studio pro publikování pro vývoj a testování prostředí."
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
ms.openlocfilehash: d4c39eb7a8bc97a980061872ba0f32f375e6976f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="using-windows-powershell-scripts-to-publish-to-dev-and-test-environments"></a><span data-ttu-id="80cdb-103">Použití skriptů Windows PowerShellu k publikování do vývojových a testovacích prostředí</span><span class="sxs-lookup"><span data-stu-id="80cdb-103">Using Windows PowerShell scripts to publish to dev and test environments</span></span>
<span data-ttu-id="80cdb-104">Když vytvoříte webovou aplikaci v sadě Visual Studio, můžete vygenerovat skript prostředí Windows PowerShell, který můžete později použít k automatizaci publikování webu do Azure jako webové aplikace ve službě Azure App Service nebo virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="80cdb-104">When you create a web application in Visual Studio, you can generate a Windows PowerShell script that you can use later to automate the publishing of your website to Azure as a Web App in Azure App Service or a virtual machine.</span></span> <span data-ttu-id="80cdb-105">Můžete upravit a rozšířit skript prostředí Windows PowerShell v editoru Visual Studio podle svých potřeb nebo skript integrovat existující sestavení, testování a publikování skripty.</span><span class="sxs-lookup"><span data-stu-id="80cdb-105">You can edit and extend the Windows PowerShell script in the Visual Studio editor to suit your requirements, or integrate the script with existing build, test, and publishing scripts.</span></span>

<span data-ttu-id="80cdb-106">Pomocí těchto skriptů, můžete zřídit vlastní verze webu pro dočasné použití (také označované jako vývojářů a testovací prostředí).</span><span class="sxs-lookup"><span data-stu-id="80cdb-106">Using these scripts, you can provision customized versions (also known as dev and test environments) of your site for temporary use.</span></span> <span data-ttu-id="80cdb-107">Může například nastavit na konkrétní verzi vašeho webu na virtuálním počítači Azure nebo na přípravný slot na web, který chcete spustit testovací sadu, reprodukujte chyby, testovací oprava chyb, zkušební verze navrhované změny nebo nastavit vlastní prostředí pro ukázku nebo prezentace.</span><span class="sxs-lookup"><span data-stu-id="80cdb-107">For example, you might set up a particular version of your website on an Azure virtual machine or on the staging slot on a website to run a test suite, reproduce a bug, test a bug fix, trial a proposed change, or set up a custom environment for a demo or presentation.</span></span> <span data-ttu-id="80cdb-108">Po vytvoření skriptu, který publikuje projektu, můžete znovu vytvořit stejná prostředí tak, že znovu spustíte skript podle potřeby nebo spusťte skript s vlastní sestavení webové aplikace k vytvoření vlastního prostředí pro testování.</span><span class="sxs-lookup"><span data-stu-id="80cdb-108">After you've created a script that publishes your project, you can recreate identical environments by re-running the script as needed, or run the script with your own build of your web application to create a custom environment for testing.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="80cdb-109">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="80cdb-109">What you need</span></span>
* <span data-ttu-id="80cdb-110">Azure SDK 2.3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="80cdb-110">Azure SDK 2.3 or later.</span></span> <span data-ttu-id="80cdb-111">V tématu [Visual Studio stáhne](http://go.microsoft.com/fwlink/?LinkID=624384) Další informace.</span><span class="sxs-lookup"><span data-stu-id="80cdb-111">See [Visual Studio Downloads](http://go.microsoft.com/fwlink/?LinkID=624384) for more information.</span></span>

<span data-ttu-id="80cdb-112">Není nutné sadu Azure SDK ke generování skriptů pro webové projekty.</span><span class="sxs-lookup"><span data-stu-id="80cdb-112">You do not need the Azure SDK to generate the scripts for web projects.</span></span> <span data-ttu-id="80cdb-113">Tato funkce je pro webové projekty, není webové role v cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="80cdb-113">This feature is for web projects, not web roles in cloud services.</span></span>

* <span data-ttu-id="80cdb-114">Prostředí Azure PowerShell 0.7.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="80cdb-114">Azure PowerShell 0.7.4 or later.</span></span> <span data-ttu-id="80cdb-115">V tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace.</span><span class="sxs-lookup"><span data-stu-id="80cdb-115">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for more information.</span></span>
* <span data-ttu-id="80cdb-116">[Prostředí Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="80cdb-116">[Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) or later.</span></span>

## <a name="additional-tools"></a><span data-ttu-id="80cdb-117">Další nástroje</span><span class="sxs-lookup"><span data-stu-id="80cdb-117">Additional tools</span></span>
<span data-ttu-id="80cdb-118">Další nástroje a zdroje pro práci s prostředím PowerShell v sadě Visual Studio pro vývoj pro Azure jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="80cdb-118">Additional tools and resources for working with PowerShell in Visual Studio for Azure development are available.</span></span> <span data-ttu-id="80cdb-119">V tématu [prostředí PowerShell nástroje pro sadu Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).</span><span class="sxs-lookup"><span data-stu-id="80cdb-119">See [PowerShell Tools for Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).</span></span>

## <a name="generating-the-publish-scripts"></a><span data-ttu-id="80cdb-120">Generování skriptů publikování</span><span class="sxs-lookup"><span data-stu-id="80cdb-120">Generating the publish scripts</span></span>
<span data-ttu-id="80cdb-121">Můžete vygenerovat skripty publikování pro virtuální počítač, který je hostitelem vašeho webu, když vytvoříte nový projekt pomocí následujících [tyto pokyny](virtual-machines/windows/classic/web-app-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="80cdb-121">You can generate the publish scripts for a virtual machine that hosts your website when you create a new project by following [these instructions](virtual-machines/windows/classic/web-app-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="80cdb-122">Můžete také [generovat publikovat skripty pro webové aplikace v Azure App Service](app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="80cdb-122">You can also [generate publish scripts for web apps in Azure App Service](app-service-web/app-service-web-get-started-dotnet.md).</span></span>

## <a name="scripts-that-visual-studio-generates"></a><span data-ttu-id="80cdb-123">Skripty, které generuje Visual Studio</span><span class="sxs-lookup"><span data-stu-id="80cdb-123">Scripts that Visual Studio generates</span></span>
<span data-ttu-id="80cdb-124">Visual Studio vytvoří řešení úrovni složku s názvem **PublishScripts** obsahující dva soubory prostředí Windows PowerShell, skript publikování pro virtuální počítač nebo webu a modul, který obsahuje funkce, které můžete použít v skripty.</span><span class="sxs-lookup"><span data-stu-id="80cdb-124">Visual Studio generates a solution-level folder called **PublishScripts** that contains two Windows PowerShell files, a publish script for your virtual machine or website, and a module that contains functions that you can use in the scripts.</span></span> <span data-ttu-id="80cdb-125">Visual Studio také vygeneruje soubor ve formátu JSON, který určuje podrobnosti projektu, který nasazujete.</span><span class="sxs-lookup"><span data-stu-id="80cdb-125">Visual Studio also generates a file in the JSON format that specifies the details of the project you are deploying.</span></span>

### <a name="windows-powershell-publish-script"></a><span data-ttu-id="80cdb-126">Publikování skriptu prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="80cdb-126">Windows PowerShell publish script</span></span>
<span data-ttu-id="80cdb-127">Skript publikování obsahuje konkrétní publikovat kroky pro nasazení na webu nebo virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="80cdb-127">The publish script contains specific publish steps for deploying to a website or virtual machine.</span></span> <span data-ttu-id="80cdb-128">Visual Studio poskytuje pro prostředí Windows PowerShell vývoj zvýrazňování syntaxe.</span><span class="sxs-lookup"><span data-stu-id="80cdb-128">Visual Studio provides syntax coloring for Windows PowerShell development.</span></span> <span data-ttu-id="80cdb-129">Nápověda pro funkce je k dispozici, a můžete volně upravit funkce ve skriptu podle svých potřeb změny.</span><span class="sxs-lookup"><span data-stu-id="80cdb-129">Help for the functions is available, and you can freely edit the functions in the script to suit your changing requirements.</span></span>

### <a name="windows-powershell-module"></a><span data-ttu-id="80cdb-130">Modul prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="80cdb-130">Windows PowerShell module</span></span>
<span data-ttu-id="80cdb-131">Modul Windows PowerShell, který generuje Visual Studio obsahuje funkce, které používá skript publikovat.</span><span class="sxs-lookup"><span data-stu-id="80cdb-131">The Windows PowerShell module that Visual Studio generates contains functions that the publish script uses.</span></span> <span data-ttu-id="80cdb-132">Tyto jsou funkce Azure PowerShell a nejsou určeny má být změněn.</span><span class="sxs-lookup"><span data-stu-id="80cdb-132">These are Azure PowerShell functions and are not intended to be modified.</span></span> <span data-ttu-id="80cdb-133">V tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace.</span><span class="sxs-lookup"><span data-stu-id="80cdb-133">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for more information.</span></span>

### <a name="json-configuration-file"></a><span data-ttu-id="80cdb-134">Konfigurační soubor JSON</span><span class="sxs-lookup"><span data-stu-id="80cdb-134">JSON configuration file</span></span>
<span data-ttu-id="80cdb-135">Vytvoření souboru JSON v **konfigurace** složky a obsahuje konfigurační data, která určuje přesně prostředky, ke kterým chcete nasadit do Azure.</span><span class="sxs-lookup"><span data-stu-id="80cdb-135">The JSON file is created in the **Configurations** folder and contains configuration data that specifies exactly which resources to deploy to Azure.</span></span> <span data-ttu-id="80cdb-136">Název souboru, který generuje Visual Studio je projekt název WAWS-dev.json Pokud jste vytvořili na webu nebo projektu název VM-dev.json, pokud jste vytvořili virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="80cdb-136">The name of the file that Visual Studio generates is project-name-WAWS-dev.json if you created a website, or project name-VM-dev.json if you created a virtual machine.</span></span> <span data-ttu-id="80cdb-137">Tady je příklad konfiguračního souboru JSON, který se vygeneruje při vytvoření webu.</span><span class="sxs-lookup"><span data-stu-id="80cdb-137">Here's an example of a JSON configuration file that's generated when you create a website.</span></span> <span data-ttu-id="80cdb-138">Většina hodnot je není potřeba vysvětlovat.</span><span class="sxs-lookup"><span data-stu-id="80cdb-138">Most of the values are self-explanatory.</span></span> <span data-ttu-id="80cdb-139">Název webu je generován Azure, takže se nemusí odpovídat název projektu.</span><span class="sxs-lookup"><span data-stu-id="80cdb-139">The website name is generated by Azure, so it might not match your project name.</span></span>

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
<span data-ttu-id="80cdb-140">Když vytvoříte virtuální počítač, bude vypadat podobně jako následující konfigurační soubor JSON.</span><span class="sxs-lookup"><span data-stu-id="80cdb-140">When you create a virtual machine, the JSON configuration file looks similar to the following.</span></span> <span data-ttu-id="80cdb-141">Všimněte si, že Cloudová služba je vytvořen jako kontejner pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="80cdb-141">Note that a cloud service is created as a container for the virtual machine.</span></span> <span data-ttu-id="80cdb-142">Virtuální počítač obsahuje obvyklé koncové body pro webový přístup prostřednictvím protokolu HTTP a HTTPS, jakož i koncové body pro nasazení webu, která umožňuje publikovat na web z vašeho místního počítače, vzdálené plochy a prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="80cdb-142">The virtual machine contains the usual endpoints for web access through HTTP and HTTPS, as well as endpoints for Web Deploy, which lets you publish to the website from your local machine, Remote Desktop, and Windows PowerShell.</span></span>

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

<span data-ttu-id="80cdb-143">Můžete upravit JSON konfiguraci chcete změnit, co se stane, když spustíte skripty publikování.</span><span class="sxs-lookup"><span data-stu-id="80cdb-143">You can edit the JSON configuration to change what happens when you run the publish scripts.</span></span> <span data-ttu-id="80cdb-144">`cloudService` a `virtualMachine` oddíly jsou povinné, ale můžete odstranit `databases` části Pokud tomu tak není.</span><span class="sxs-lookup"><span data-stu-id="80cdb-144">The `cloudService` and `virtualMachine` sections are required, but you can delete the `databases` section if you don't need it.</span></span> <span data-ttu-id="80cdb-145">Vlastnosti, které jsou prázdné v výchozí konfigurační soubor, který generuje Visual Studio jsou volitelné. ty, které mají v konfiguračním souboru na výchozí hodnoty jsou povinné.</span><span class="sxs-lookup"><span data-stu-id="80cdb-145">The properties that are empty in the default configuration file that Visual Studio generates are optional; those that have values in the default configuration file are required.</span></span>

<span data-ttu-id="80cdb-146">Pokud máte web, který má několik prostředí nasazení (označované jako sloty) místo jednoho pracoviště v Azure, můžete zahrnout název slotu názvu webu v konfiguračním souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="80cdb-146">If you have a website that has multiple deployment environments (known as slots) instead of a single production site in Azure, you can include the slot name in the name of the website in the JSON configuration file.</span></span> <span data-ttu-id="80cdb-147">Například, pokud máte web s názvem **server** a slotu pro něj s názvem **testování** pak server test.cloudapp.net je identifikátor URI, ale mysite(test) je správný název pro použití v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="80cdb-147">For example, if you have a website that's named **mysite** and a slot for it named **test** then the URI is mysite-test.cloudapp.net, but the correct name to use in the configuration file is mysite(test).</span></span> <span data-ttu-id="80cdb-148">Můžete provést jenom Pokud tento web a sloty, které již existují v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="80cdb-148">You can only do this if the website and slots already exist in your subscription.</span></span> <span data-ttu-id="80cdb-149">Pokud ještě neexistují, vytvořit web tak, že spustíte skript bez zadání přihrádky, pak vytvořte slot v [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), a poté spusťte skript s názvem upravené webové stránky.</span><span class="sxs-lookup"><span data-stu-id="80cdb-149">If they don't exist, create the website by running the script without specifying the slot, then create the slot in the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), and thereafter run the script with the modified website name.</span></span> <span data-ttu-id="80cdb-150">Další informace o nasazovací sloty pro webové aplikace najdete v tématu [nastavení přípravných prostředí pro webové aplikace v Azure App Service](app-service-web/web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="80cdb-150">For more information about deployment slots for web apps, see [Set up staging environments for web apps in Azure App Service](app-service-web/web-sites-staged-publishing.md).</span></span>

## <a name="how-to-run-the-publish-scripts"></a><span data-ttu-id="80cdb-151">Jak spustit skripty publikování</span><span class="sxs-lookup"><span data-stu-id="80cdb-151">How to run the publish scripts</span></span>
<span data-ttu-id="80cdb-152">Pokud jste spustili nikdy skript prostředí Windows PowerShell před, musíte nejprve nastavit zásady spouštění umožňující spouštění skriptů.</span><span class="sxs-lookup"><span data-stu-id="80cdb-152">If you have never run a Windows PowerShell script before, you must first set the execution policy to enable scripts to run.</span></span> <span data-ttu-id="80cdb-153">Toto je funkce zabezpečení, chcete-li zabránit uživatelům ve spouštění skriptů prostředí Windows PowerShell, pokud jsou snadno napadnutelný malwaru nebo viry, které zahrnují spouštění skriptů.</span><span class="sxs-lookup"><span data-stu-id="80cdb-153">This is a security feature to prevent users from running Windows PowerShell scripts if they're vulnerable to malware or viruses that involve executing scripts.</span></span>

### <a name="run-the-script"></a><span data-ttu-id="80cdb-154">Spusťte skript</span><span class="sxs-lookup"><span data-stu-id="80cdb-154">Run the script</span></span>
1. <span data-ttu-id="80cdb-155">Vytvořte balíček nasazení webu pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="80cdb-155">Create the Web Deploy package for your project.</span></span> <span data-ttu-id="80cdb-156">Balíček nasazení webu je komprimovaný archiv (soubor .zip), které obsahují soubory, které chcete zkopírovat do svého webu nebo virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="80cdb-156">A Web Deploy package is a compressed archive (.zip file) that contain files that you want to copy to your website or virtual machine.</span></span> <span data-ttu-id="80cdb-157">Balíčky webového nasazení v sadě Visual Studio můžete vytvořit pro všechny webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="80cdb-157">You can create Web Deploy packages in Visual Studio for any web application.</span></span>

![Vytváření webového nasazení balíčku](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

<span data-ttu-id="80cdb-159">Další informace najdete v tématu [postupy: vytvoření balíčku pro nasazení webu v sadě Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span><span class="sxs-lookup"><span data-stu-id="80cdb-159">For more information, see [How to: Create a Web Deployment Package in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).</span></span> <span data-ttu-id="80cdb-160">Můžete také automatizovat vytvoření vašeho balíčku nástroje nasazení webu, jak je popsáno v části **přizpůsobení a rozšíření skripty publikování** dál v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="80cdb-160">You can also automate the creation of your Web Deploy package, as described in the section **Customizing and extending the publish scripts** later in this topic.</span></span>

1. <span data-ttu-id="80cdb-161">V **Průzkumníku řešení**, otevřete v místní nabídce pro skript a potom zvolte **otevřete pomocí prostředí PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="80cdb-161">In **Solution Explorer**, open the context menu for the script, and then choose **Open with PowerShell ISE**.</span></span>
2. <span data-ttu-id="80cdb-162">Pokud je to poprvé, kterou jste spustili skriptů prostředí Windows PowerShell na tomto počítači, otevřete okno příkazového řádku s oprávněními správce a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="80cdb-162">If this is the first time you've run Windows PowerShell scripts on this computer, open a command prompt window with Administrator privileges and type the following command:</span></span>

    ```powershell
    Set-ExecutionPolicy RemoteSigned
    ```

3. <span data-ttu-id="80cdb-163">Přihlaste se k Azure pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="80cdb-163">Sign in to Azure by using the following command.</span></span>

    ```powershell
    Add-AzureAccount
    ```

    <span data-ttu-id="80cdb-164">Po zobrazení výzvy zadejte své uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="80cdb-164">When prompted, supply your username and password.</span></span>

    <span data-ttu-id="80cdb-165">Všimněte si, že při automatizaci skript, tato metoda poskytnout přihlašovací údaje Azure nebude fungovat.</span><span class="sxs-lookup"><span data-stu-id="80cdb-165">Note that when you automate the script, this method of providing Azure credentials won't work.</span></span> <span data-ttu-id="80cdb-166">Místo toho používejte soubor .publishsettings k zadání přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="80cdb-166">Instead, you should use the .publishsettings file to provide credentials.</span></span> <span data-ttu-id="80cdb-167">Jednou, můžete použít příkaz **Get-AzurePublishSettingsFile** ke stažení souboru z Azure a následně použít **Import AzurePublishSettingsFile** se soubor naimportovat.</span><span class="sxs-lookup"><span data-stu-id="80cdb-167">One time only, you use the command **Get-AzurePublishSettingsFile** to download the file from Azure, and thereafter use **Import-AzurePublishSettingsFile** to import the file.</span></span> <span data-ttu-id="80cdb-168">Podrobné pokyny najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="80cdb-168">For detailed instructions, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

4. <span data-ttu-id="80cdb-169">(Volitelné) Pokud chcete vytvořit prostředky Azure, jako je například virtuální počítač, databáze a webu bez publikování webové aplikace, použijte **publikovat WebApplication.ps1** s **-konfigurace**argument nastaven na hodnotu konfiguračního souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="80cdb-169">(Optional) If you want to create Azure resources such as the virtual machine, database, and website without publishing your web application, use the **Publish-WebApplication.ps1** command with the **-Configuration** argument set to the JSON configuration file.</span></span> <span data-ttu-id="80cdb-170">Tento příkaz používá k určení, které prostředky pro vytvoření konfiguračního souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="80cdb-170">This command line uses the JSON configuration file to determine which resources to create.</span></span> <span data-ttu-id="80cdb-171">Protože používá výchozí nastavení pro další argumenty příkazového řádku, vytvoří prostředky, ale nepodporuje publikování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="80cdb-171">Because it uses the default settings for other command-line arguments, it creates the resources, but doesn't publish your web application.</span></span> <span data-ttu-id="80cdb-172">-Verbose – možnost získáte další informace o co se děje.</span><span class="sxs-lookup"><span data-stu-id="80cdb-172">The –Verbose option gives you more information about what's happening.</span></span>

    ```powershell
    Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json
    ```

5. <span data-ttu-id="80cdb-173">Použití **publikovat WebApplication.ps1** příkaz, jak je znázorněno v jednom z následujících příkladech vyvolání skriptu a publikování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="80cdb-173">Use the **Publish-WebApplication.ps1** command as shown in one of the following examples to invoke the script and publish your web application.</span></span> <span data-ttu-id="80cdb-174">Pokud potřebujete přepíší výchozí nastavení pro žádné další argumenty, jako je například název odběru, publikujte název balíčku, přihlašovací údaje virtuálního počítače nebo přihlašovací údaje databáze serveru, můžete určit tyto parametry.</span><span class="sxs-lookup"><span data-stu-id="80cdb-174">If you need to override the default settings for any of the other arguments, such as the subscription name, publish package name, virtual machine credentials, or database server credentials, you can specify those parameters.</span></span> <span data-ttu-id="80cdb-175">Použití **– podrobný** možnost zobrazit další informace o průběhu procesu publikování.</span><span class="sxs-lookup"><span data-stu-id="80cdb-175">Use the **–Verbose** option to see more information about the progress of the publishing process.</span></span>

    ```powershell
    Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
    –SubscriptionName Contoso `
    -WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
    -DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
    -Verbose
    ```

    <span data-ttu-id="80cdb-176">Pokud vytváříte virtuální počítač, příkaz vypadá takto.</span><span class="sxs-lookup"><span data-stu-id="80cdb-176">If you're creating a virtual machine, the command looks like the following.</span></span> <span data-ttu-id="80cdb-177">Tento příklad také ukazuje, jak zadat pověření pro více databází.</span><span class="sxs-lookup"><span data-stu-id="80cdb-177">This example also shows how to specify the credentials for multiple databases.</span></span> <span data-ttu-id="80cdb-178">Pro virtuální počítače, které tyto skripty vytvořit není certifikát SSL od důvěryhodné kořenové autority.</span><span class="sxs-lookup"><span data-stu-id="80cdb-178">For the virtual machines that these scripts create, the SSL certificate is not from a trusted root authority.</span></span> <span data-ttu-id="80cdb-179">Proto je nutné použít **– AllowUntrusted** možnost.</span><span class="sxs-lookup"><span data-stu-id="80cdb-179">Therefore, you need to use the **–AllowUntrusted** option.</span></span>

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

    <span data-ttu-id="80cdb-180">Databáze můžete vytvářet skript, ale nevytvoří databázové servery.</span><span class="sxs-lookup"><span data-stu-id="80cdb-180">The script can create databases, but it doesn't create database servers.</span></span> <span data-ttu-id="80cdb-181">Pokud chcete vytvořit databázový server, můžete použít **New-AzureSqlDatabaseServer** funkce v modulu Azure.</span><span class="sxs-lookup"><span data-stu-id="80cdb-181">If you want to create a database server, you can use the **New-AzureSqlDatabaseServer** function in the Azure module.</span></span>

## <a name="customizing-and-extending-the-publish-scripts"></a><span data-ttu-id="80cdb-182">Přizpůsobení a rozšíření skripty publikování</span><span class="sxs-lookup"><span data-stu-id="80cdb-182">Customizing and extending the publish scripts</span></span>
<span data-ttu-id="80cdb-183">Můžete upravit skript publikování a konfigurační soubor JSON.</span><span class="sxs-lookup"><span data-stu-id="80cdb-183">You can customize the publish script and JSON configuration file.</span></span> <span data-ttu-id="80cdb-184">Funkce v modulu Windows PowerShell **AzureWebAppPublishModule.psm1** nejsou určeny má být změněn.</span><span class="sxs-lookup"><span data-stu-id="80cdb-184">The functions in the Windows PowerShell module **AzureWebAppPublishModule.psm1** are not intended to be modified.</span></span> <span data-ttu-id="80cdb-185">Pokud chcete zadat jinou databázi nebo změnit některé vlastnosti virtuálního počítače, upravte konfigurační soubor JSON.</span><span class="sxs-lookup"><span data-stu-id="80cdb-185">If you just want to specify a different database or change some of the properties of the virtual machine, edit the JSON configuration file.</span></span> <span data-ttu-id="80cdb-186">Pokud chcete rozšířit funkce skript, který chcete automatizovat vytváření a testování projektu, můžete implementovat funkce zástupných procedur v **publikovat WebApplication.ps1**.</span><span class="sxs-lookup"><span data-stu-id="80cdb-186">If you want to extend the functionality of the script to automate building and testing your project, you can implement function stubs in **Publish-WebApplication.ps1**.</span></span>

<span data-ttu-id="80cdb-187">K automatizaci vytváření projektu, přidat kód, který volá MSBuild `New-WebDeployPackage` jak ukazuje tento příklad kódu.</span><span class="sxs-lookup"><span data-stu-id="80cdb-187">To automate building your project, add code that calls MSBuild to `New-WebDeployPackage` as shown in this code example.</span></span> <span data-ttu-id="80cdb-188">Cesta k příkazu MSBuild se liší v závislosti na verzi sady Visual Studio, které jste nainstalovali.</span><span class="sxs-lookup"><span data-stu-id="80cdb-188">The path to the MSBuild command is different depending on the version of Visual Studio you have installed.</span></span> <span data-ttu-id="80cdb-189">Chcete-li získat správnou cestu, můžete použít funkci **Get-MSBuildCmd**, jak je uvedeno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="80cdb-189">To get the correct path, you can use the function **Get-MSBuildCmd**, as shown in this example.</span></span>

### <a name="to-automate-building-your-project"></a><span data-ttu-id="80cdb-190">K automatizaci vytváření projektu</span><span class="sxs-lookup"><span data-stu-id="80cdb-190">To automate building your project</span></span>
1. <span data-ttu-id="80cdb-191">Přidat `$ProjectFile` parametru v části globální param.</span><span class="sxs-lookup"><span data-stu-id="80cdb-191">Add the `$ProjectFile` parameter in the global param section.</span></span>

    ```powershell
    [Parameter(Mandatory = $false)]
    [ValidateScript({Test-Path $_ -PathType Leaf})]
    [String]
    $ProjectFile,
    ```

2. <span data-ttu-id="80cdb-192">Zkopírujte funkci `Get-MSBuildCmd` do souboru skriptu.</span><span class="sxs-lookup"><span data-stu-id="80cdb-192">Copy the function `Get-MSBuildCmd` into your script file.</span></span>

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

3. <span data-ttu-id="80cdb-193">Nahraďte `New-WebDeployPackage` s následující kód a nahraďte zástupné symboly v řádku vytváření `$msbuildCmd`.</span><span class="sxs-lookup"><span data-stu-id="80cdb-193">Replace `New-WebDeployPackage` with the following code and replace the placeholders in the line constructing `$msbuildCmd`.</span></span> <span data-ttu-id="80cdb-194">Tento kód je pro Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="80cdb-194">This code is for Visual Studio 2015.</span></span> <span data-ttu-id="80cdb-195">Pokud používáte Visual Studio 2013, změňte **VisualStudioVersion** vlastnost níže na `12.0`.</span><span class="sxs-lookup"><span data-stu-id="80cdb-195">If you're using Visual Studio 2013, change the **VisualStudioVersion** property below to `12.0`.</span></span>

    ```powershell
    function New-WebDeployPackage
    {
        #Write a function to build and package your web application
    ```

    <span data-ttu-id="80cdb-196">Chcete-li vytvořit webovou aplikaci, použijte MsBuild.exe.</span><span class="sxs-lookup"><span data-stu-id="80cdb-196">To build your web application, use MsBuild.exe.</span></span> <span data-ttu-id="80cdb-197">Nápovědu najdete v tématu Reference k příkazovému řádku nástroje MSBuild v: [http://go.microsoft.com/fwlink/?LinkId=391339](http://go.microsoft.com/fwlink/?LinkId=391339)</span><span class="sxs-lookup"><span data-stu-id="80cdb-197">For help, see MSBuild Command-Line Reference at: [http://go.microsoft.com/fwlink/?LinkId=391339](http://go.microsoft.com/fwlink/?LinkId=391339)</span></span>

    ```powershell
    Write-VerboseWithTime 'Build-WebDeployPackage: Start'

    $msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=14.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory

    Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
    ```

### <a name="start-execution-of-the-build-command"></a><span data-ttu-id="80cdb-198">Spustit provádění příkazu sestavení</span><span class="sxs-lookup"><span data-stu-id="80cdb-198">Start execution of the build command</span></span>

```powershell
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru

if ($job.ExitCode -ne 0) {
    throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain the project name
$projectName = (Get-Item $ProjectFile).BaseName

#Construct the path to web deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName

#Get the full path for the web deploy zip package. This is required for MSDeploy to work
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir

Write-VerboseWithTime 'Build-WebDeployPackage: End'

return $WebDeployPackage
}
```

1. <span data-ttu-id="80cdb-199">Volání `New-WebDeployPackage` funkce před tento řádek: `$Config = Read-ConfigFile $Configuration` pro webové aplikace nebo `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="80cdb-199">Call the `New-WebDeployPackage` function before this line: `$Config = Read-ConfigFile $Configuration` for web apps or `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` for virtual machines.</span></span>

    ```powershell
    if($ProjectFile)
    {
    $WebDeployPackage = New-WebDeployPackage
    }
    ```

2. <span data-ttu-id="80cdb-200">Vyvolání vlastní skript z příkazového řádku pomocí předávání `$Project` argument, stejně jako na příkazovém řádku následující příklad.</span><span class="sxs-lookup"><span data-stu-id="80cdb-200">Invoke the customized script from command line using passing the `$Project` argument, as in the following example command line.</span></span>

    ```powershell
    .\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
    -ProjectFile ..\WebApplication5\WebApplication5.csproj `
    -VMPassword @{Name="VMUser";Password="Test.123"} `
    -AllowUntrusted `
    -Verbose
    ```

    <span data-ttu-id="80cdb-201">Pro automatizaci testování vaší aplikace, přidejte kód, který `Test-WebApplication`.</span><span class="sxs-lookup"><span data-stu-id="80cdb-201">To automate testing of your application, add code to `Test-WebApplication`.</span></span> <span data-ttu-id="80cdb-202">Nezapomeňte zrušte komentář u řádků v **publikovat WebApplication.ps1** kde jsou tyto funkce volána.</span><span class="sxs-lookup"><span data-stu-id="80cdb-202">Be sure to uncomment the lines in **Publish-WebApplication.ps1** where these functions are called.</span></span> <span data-ttu-id="80cdb-203">Pokud nezadáte implementace, můžete ručně vytvořit projekt pomocí sady Visual Studio a pak spusťte skript publikování, který chcete publikovat do Azure.</span><span class="sxs-lookup"><span data-stu-id="80cdb-203">If you don't provide an implementation, you can manually build your project with Visual Studio, and then run the publish script to publish to Azure.</span></span>

## <a name="publishing-function-summary"></a><span data-ttu-id="80cdb-204">Publikování souhrn funkcí</span><span class="sxs-lookup"><span data-stu-id="80cdb-204">Publishing function summary</span></span>
<span data-ttu-id="80cdb-205">Chcete-li získat nápovědu pro funkce, které můžete použít na příkazovém řádku prostředí Windows PowerShell, použijte příkaz `Get-Help function-name`.</span><span class="sxs-lookup"><span data-stu-id="80cdb-205">To get help for functions you can use at the Windows PowerShell command prompt, use the command `Get-Help function-name`.</span></span> <span data-ttu-id="80cdb-206">V nápovědě zahrnuje parametr nápovědy a příklady.</span><span class="sxs-lookup"><span data-stu-id="80cdb-206">The help includes parameter help and examples.</span></span> <span data-ttu-id="80cdb-207">Stejný text nápovědy je také ve zdrojových souborech skriptu **AzureWebAppPublishModule.psm1** a **publikovat WebApplication.ps1**.</span><span class="sxs-lookup"><span data-stu-id="80cdb-207">The same help text is also in the script source files, **AzureWebAppPublishModule.psm1** and **Publish-WebApplication.ps1**.</span></span> <span data-ttu-id="80cdb-208">V jazyce Visual Studio jsou lokalizované skriptu a Nápověda.</span><span class="sxs-lookup"><span data-stu-id="80cdb-208">The script and help are localized in your Visual Studio language.</span></span>

<span data-ttu-id="80cdb-209">**AzureWebAppPublishModule**</span><span class="sxs-lookup"><span data-stu-id="80cdb-209">**AzureWebAppPublishModule**</span></span>

| <span data-ttu-id="80cdb-210">Název funkce</span><span class="sxs-lookup"><span data-stu-id="80cdb-210">Function name</span></span> | <span data-ttu-id="80cdb-211">Popis</span><span class="sxs-lookup"><span data-stu-id="80cdb-211">Description</span></span> |
| --- | --- |
| <span data-ttu-id="80cdb-212">Přidání azuresqldatabase.</span><span class="sxs-lookup"><span data-stu-id="80cdb-212">Add-AzureSQLDatabase</span></span> |<span data-ttu-id="80cdb-213">Vytvoří novou databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="80cdb-213">Creates a new Azure SQL database.</span></span> |
| <span data-ttu-id="80cdb-214">Přidat AzureSQLDatabases</span><span class="sxs-lookup"><span data-stu-id="80cdb-214">Add-AzureSQLDatabases</span></span> |<span data-ttu-id="80cdb-215">Vytvoří databáze Azure SQL z hodnot v konfiguračním souboru JSON, který generuje Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="80cdb-215">Creates Azure SQL databases from the values in the JSON configuration file that Visual Studio generates.</span></span> |
| <span data-ttu-id="80cdb-216">Přidat AzureVM</span><span class="sxs-lookup"><span data-stu-id="80cdb-216">Add-AzureVM</span></span> |<span data-ttu-id="80cdb-217">Vytvoří virtuální počítač Azure a vrátí adresu URL nasazené virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="80cdb-217">Creates a Azure virtual machine and returns the URL of the deployed VM.</span></span> <span data-ttu-id="80cdb-218">Funkce nastaví požadavky a pak zavolá **New-AzureVM** – funkce (Azure modul) k vytvoření nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="80cdb-218">The function sets up the prerequisites and then calls the **New-AzureVM** function (Azure module) to create a new virtual machine.</span></span> |
| <span data-ttu-id="80cdb-219">Přidat AzureVMEndpoints</span><span class="sxs-lookup"><span data-stu-id="80cdb-219">Add-AzureVMEndpoints</span></span> |<span data-ttu-id="80cdb-220">Přidá nový vstupní koncové body k virtuálnímu počítači a vrátí virtuální počítač s nový koncový bod.</span><span class="sxs-lookup"><span data-stu-id="80cdb-220">Adds new input endpoints to a virtual machine and returns the virtual machine with the new endpoint.</span></span> |
| <span data-ttu-id="80cdb-221">Přidat AzureVMStorage</span><span class="sxs-lookup"><span data-stu-id="80cdb-221">Add-AzureVMStorage</span></span> |<span data-ttu-id="80cdb-222">Vytvoří nový účet úložiště Azure v aktuálním předplatném.</span><span class="sxs-lookup"><span data-stu-id="80cdb-222">Creates a new Azure storage account in the current subscription.</span></span> <span data-ttu-id="80cdb-223">Název účtu začíná řetězcem "devtest", za nímž následuje jedinečný alfanumerický řetězec.</span><span class="sxs-lookup"><span data-stu-id="80cdb-223">The name of the account begins with "devtest" followed by a unique alphanumeric string.</span></span> <span data-ttu-id="80cdb-224">Vrátí název nového účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="80cdb-224">The function returns the name of the new storage account.</span></span> <span data-ttu-id="80cdb-225">Musíte zadat umístění nebo skupina vztahů pro nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="80cdb-225">You must specify either a location or an affinity group for the new storage account.</span></span> |
| <span data-ttu-id="80cdb-226">Přidat AzureWebsite</span><span class="sxs-lookup"><span data-stu-id="80cdb-226">Add-AzureWebsite</span></span> |<span data-ttu-id="80cdb-227">Vytvoří web se zadaným názvem a umístěním.</span><span class="sxs-lookup"><span data-stu-id="80cdb-227">Creates a website with the specified name and location.</span></span> <span data-ttu-id="80cdb-228">Tato funkce volá **New-AzureWebsite** funkce v modulu Azure.</span><span class="sxs-lookup"><span data-stu-id="80cdb-228">This function calls the **New-AzureWebsite** function in the Azure module.</span></span> <span data-ttu-id="80cdb-229">Pokud předplatné už neobsahuje web se zadaným názvem, tato funkce vytvoří web a vrátí objekt webu.</span><span class="sxs-lookup"><span data-stu-id="80cdb-229">If the subscription doesn't already include a website with the specified name, this function creates the website and returns a website object.</span></span> <span data-ttu-id="80cdb-230">Funkce `$null`.</span><span class="sxs-lookup"><span data-stu-id="80cdb-230">Otherwise, it returns `$null`.</span></span> |
| <span data-ttu-id="80cdb-231">Zálohování předplatného</span><span class="sxs-lookup"><span data-stu-id="80cdb-231">Backup-Subscription</span></span> |<span data-ttu-id="80cdb-232">Uloží aktuální předplatné Azure v `$Script:originalSubscription` proměnné v oboru skriptu. Tato funkce uloží aktuální předplatné Azure (jak získat `Get-AzureSubscription -Current`) a jeho účet úložiště a předplatné, které mění tímto skriptem (uložené v proměnné `$UserSpecifiedSubscription`) a jeho účet úložiště, v oboru skriptu.</span><span class="sxs-lookup"><span data-stu-id="80cdb-232">Saves the current Azure subscription in the `$Script:originalSubscription` variable in script scope.This function saves the current Azure subscription (as obtained by `Get-AzureSubscription -Current`) and its storage account, and the subscription that is changed by this script (stored in the variable `$UserSpecifiedSubscription`) and its storage account, in script scope.</span></span> <span data-ttu-id="80cdb-233">Ukládání hodnot, můžete pomocí funkce, jako například `Restore-Subscription`, pokud chcete obnovit původní aktuální předplatné a účet úložiště pro aktuální stav, pokud došlo ke změně aktuálního stavu.</span><span class="sxs-lookup"><span data-stu-id="80cdb-233">By saving the values, you can use a function, such as `Restore-Subscription`, to restore the original current subscription and storage account to current status if the current status has changed.</span></span> |
| <span data-ttu-id="80cdb-234">Najít AzureVM</span><span class="sxs-lookup"><span data-stu-id="80cdb-234">Find-AzureVM</span></span> |<span data-ttu-id="80cdb-235">Získá zadaný virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="80cdb-235">Gets the specified Azure virtual machine.</span></span> |
| <span data-ttu-id="80cdb-236">Formát DevTestMessageWithTime</span><span class="sxs-lookup"><span data-stu-id="80cdb-236">Format-DevTestMessageWithTime</span></span> |<span data-ttu-id="80cdb-237">Přidá k datu a času na zprávy.</span><span class="sxs-lookup"><span data-stu-id="80cdb-237">Prepends the date and time to a message.</span></span> <span data-ttu-id="80cdb-238">Tato funkce je určená pro zpráv zapsaných do datové proudy chyba a podrobná.</span><span class="sxs-lookup"><span data-stu-id="80cdb-238">This function is designed for messages written to the Error and Verbose streams.</span></span> |
| <span data-ttu-id="80cdb-239">Get-AzureSQLDatabaseConnectionString</span><span class="sxs-lookup"><span data-stu-id="80cdb-239">Get-AzureSQLDatabaseConnectionString</span></span> |<span data-ttu-id="80cdb-240">Sestaví připojovacího řetězce pro připojení k databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="80cdb-240">Assembles a connection string to connect to an Azure SQL database.</span></span> |
| <span data-ttu-id="80cdb-241">Get-AzureVMStorage</span><span class="sxs-lookup"><span data-stu-id="80cdb-241">Get-AzureVMStorage</span></span> |<span data-ttu-id="80cdb-242">Vrací název první účet úložiště se stejným názvem vzor "devtest*" (malá a velká písmena) v zadaném umístění nebo skupina vztahů. Pokud "devtest*" účet úložiště neodpovídá umístění nebo skupina vztahů, funkce se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="80cdb-242">Returns the name of the first storage account with the name pattern "devtest*" (case insensitive) in the specified location or affinity group. If the "devtest*" storage account doesn't match the location or affinity group, the function ignores it.</span></span> <span data-ttu-id="80cdb-243">Musíte zadat umístění nebo skupině vztahů.</span><span class="sxs-lookup"><span data-stu-id="80cdb-243">You must specify either a location or an affinity group.</span></span> |
| <span data-ttu-id="80cdb-244">Get-MSDeployCmd</span><span class="sxs-lookup"><span data-stu-id="80cdb-244">Get-MSDeployCmd</span></span> |<span data-ttu-id="80cdb-245">Vrátí příkaz ke spuštění nástroje MsDeploy.exe.</span><span class="sxs-lookup"><span data-stu-id="80cdb-245">Returns a command to run the MsDeploy.exe tool.</span></span> |
| <span data-ttu-id="80cdb-246">Nové AzureVMEnvironment</span><span class="sxs-lookup"><span data-stu-id="80cdb-246">New-AzureVMEnvironment</span></span> |<span data-ttu-id="80cdb-247">Vyhledá nebo vytvoří virtuální počítač v rámci předplatného, které se shodují s hodnotami v konfiguračním souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="80cdb-247">Finds or creates a virtual machine in the subscription that matches the values in the JSON configuration file.</span></span> |
| <span data-ttu-id="80cdb-248">Publikování WebPackage</span><span class="sxs-lookup"><span data-stu-id="80cdb-248">Publish-WebPackage</span></span> |<span data-ttu-id="80cdb-249">Používá MsDeploy.exe a webové publikování balíčku. Soubor ZIP k nasazení prostředků na webu.</span><span class="sxs-lookup"><span data-stu-id="80cdb-249">Uses MsDeploy.exe and a web publish package .Zip file to deploy resources to a website.</span></span> <span data-ttu-id="80cdb-250">Tato funkce negeneruje žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="80cdb-250">This function doesn't generate any output.</span></span> <span data-ttu-id="80cdb-251">Pokud volání MSDeploy.exe selže, funkce vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="80cdb-251">If the call to MSDeploy.exe fails, the function throws an exception.</span></span> <span data-ttu-id="80cdb-252">Chcete-li získat podrobnější výstup, použijte **-Verbose** možnost.</span><span class="sxs-lookup"><span data-stu-id="80cdb-252">To get more detailed output, use the **-Verbose** option.</span></span> |
| <span data-ttu-id="80cdb-253">Publikování WebPackageToVM</span><span class="sxs-lookup"><span data-stu-id="80cdb-253">Publish-WebPackageToVM</span></span> |<span data-ttu-id="80cdb-254">Ověřuje hodnoty parametru a potom zavolá **publikovat WebPackage** funkce.</span><span class="sxs-lookup"><span data-stu-id="80cdb-254">Verifies the parameter values, and then calls the **Publish-WebPackage** function.</span></span> |
| <span data-ttu-id="80cdb-255">ConfigFile pro čtení</span><span class="sxs-lookup"><span data-stu-id="80cdb-255">Read-ConfigFile</span></span> |<span data-ttu-id="80cdb-256">Ověří konfiguračního souboru JSON a vrátí hodnotu hash tabulku vybraných hodnot.</span><span class="sxs-lookup"><span data-stu-id="80cdb-256">Validates the JSON configuration file and returns a hash table of selected values.</span></span> |
| <span data-ttu-id="80cdb-257">Obnovení předplatného</span><span class="sxs-lookup"><span data-stu-id="80cdb-257">Restore-Subscription</span></span> |<span data-ttu-id="80cdb-258">Obnoví aktuální předplatné na původního předplatného.</span><span class="sxs-lookup"><span data-stu-id="80cdb-258">Resets the current subscription to the original subscription.</span></span> |
| <span data-ttu-id="80cdb-259">Test AzureModule</span><span class="sxs-lookup"><span data-stu-id="80cdb-259">Test-AzureModule</span></span> |<span data-ttu-id="80cdb-260">Vrátí `$true` Pokud je nainstalovaný modul Azure verze 0.7.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="80cdb-260">Returns `$true` if the installed Azure module version is 0.7.4 or later.</span></span> <span data-ttu-id="80cdb-261">Vrátí `$false` Pokud modul není nainstalován nebo je starší verze.</span><span class="sxs-lookup"><span data-stu-id="80cdb-261">Returns `$false` if the module isn't installed or is an earlier version.</span></span> <span data-ttu-id="80cdb-262">Tato funkce nemá žádné parametry.</span><span class="sxs-lookup"><span data-stu-id="80cdb-262">This function has no parameters.</span></span> |
| <span data-ttu-id="80cdb-263">Test AzureModuleVersion</span><span class="sxs-lookup"><span data-stu-id="80cdb-263">Test-AzureModuleVersion</span></span> |<span data-ttu-id="80cdb-264">Vrátí `$true` Pokud je verze modulu Azure 0.7.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="80cdb-264">Returns `$true` if the version of the Azure module is 0.7.4 or later.</span></span> <span data-ttu-id="80cdb-265">Vrátí `$false` Pokud modul není nainstalován nebo je starší verze.</span><span class="sxs-lookup"><span data-stu-id="80cdb-265">Returns `$false` if the module isn't installed or is an earlier version.</span></span> <span data-ttu-id="80cdb-266">Tato funkce nemá žádné parametry.</span><span class="sxs-lookup"><span data-stu-id="80cdb-266">This function has no parameters.</span></span> |
| <span data-ttu-id="80cdb-267">Test HttpsUrl</span><span class="sxs-lookup"><span data-stu-id="80cdb-267">Test-HttpsUrl</span></span> |<span data-ttu-id="80cdb-268">Vstupní adresa URL převede na objekt System.Uri.</span><span class="sxs-lookup"><span data-stu-id="80cdb-268">Converts the input URL to a System.Uri object.</span></span> <span data-ttu-id="80cdb-269">Vrátí `$True` Pokud se absolutní adresu URL a jeho schéma https.</span><span class="sxs-lookup"><span data-stu-id="80cdb-269">Returns `$True` if the URL is absolute and its scheme is https.</span></span> <span data-ttu-id="80cdb-270">Vrátí `$false` Pokud adresa URL je relativní, jeho schématu není HTTPS nebo vstupní řetězec nelze převést na adresu URL.</span><span class="sxs-lookup"><span data-stu-id="80cdb-270">Returns `$false` if the URL is relative, its scheme isn't HTTPS, or the input string can't be converted to a URL.</span></span> |
| <span data-ttu-id="80cdb-271">Test člena</span><span class="sxs-lookup"><span data-stu-id="80cdb-271">Test-Member</span></span> |<span data-ttu-id="80cdb-272">Vrátí `$true` Pokud vlastnosti nebo metody je členem objektu.</span><span class="sxs-lookup"><span data-stu-id="80cdb-272">Returns `$true` if a property or method is a member of the object.</span></span> <span data-ttu-id="80cdb-273">Jinak vrátí `$false`.</span><span class="sxs-lookup"><span data-stu-id="80cdb-273">Otherwise, returns `$false`.</span></span> |
| <span data-ttu-id="80cdb-274">Zápis ErrorWithTime</span><span class="sxs-lookup"><span data-stu-id="80cdb-274">Write-ErrorWithTime</span></span> |<span data-ttu-id="80cdb-275">Zapíše chybovou zprávu s předponou aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="80cdb-275">Writes an error message prefixed with the current time.</span></span> <span data-ttu-id="80cdb-276">Tato funkce volá **formátu DevTestMessageWithTime** funkce pro předřazení čas před zápisu do datového proudu chyba se zprávou.</span><span class="sxs-lookup"><span data-stu-id="80cdb-276">This function calls the **Format-DevTestMessageWithTime** function to prepend the time before writing the message to the Error stream.</span></span> |
| <span data-ttu-id="80cdb-277">Zápis HostWithTime</span><span class="sxs-lookup"><span data-stu-id="80cdb-277">Write-HostWithTime</span></span> |<span data-ttu-id="80cdb-278">Zapíše zprávu do hostitelského programu (**Write-Host**) s předponou aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="80cdb-278">Writes a message to the host program (**Write-Host**) prefixed with the current time.</span></span> <span data-ttu-id="80cdb-279">Zápis do hostitelského programu účinek se liší.</span><span class="sxs-lookup"><span data-stu-id="80cdb-279">The effect of writing to the host program varies.</span></span> <span data-ttu-id="80cdb-280">Většina programů tohoto hostitele prostředí Windows PowerShell zápisu tyto zprávy standardním výstupu.</span><span class="sxs-lookup"><span data-stu-id="80cdb-280">Most programs that host Windows PowerShell write these messages to standard output.</span></span> |
| <span data-ttu-id="80cdb-281">Zápis VerboseWithTime</span><span class="sxs-lookup"><span data-stu-id="80cdb-281">Write-VerboseWithTime</span></span> |<span data-ttu-id="80cdb-282">Zapíše podrobnou zprávu s předponou aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="80cdb-282">Writes a verbose message prefixed with the current time.</span></span> <span data-ttu-id="80cdb-283">Vzhledem k tomu, že zavolá **Write-Verbose**, ve zprávě zobrazí, jenom když bude skript spuštěn s **podrobné** parametr nebo když **VerbosePreference** předvoleb je nastaven na  **Pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="80cdb-283">Because it calls **Write-Verbose**, the message displays only when the script runs with the **Verbose** parameter or when the **VerbosePreference** preference is set to **Continue**.</span></span> |

<span data-ttu-id="80cdb-284">**Publikovat webovou aplikaci**</span><span class="sxs-lookup"><span data-stu-id="80cdb-284">**Publish-WebApplication**</span></span>

| <span data-ttu-id="80cdb-285">Název funkce</span><span class="sxs-lookup"><span data-stu-id="80cdb-285">Function name</span></span> | <span data-ttu-id="80cdb-286">Popis</span><span class="sxs-lookup"><span data-stu-id="80cdb-286">Description</span></span> |
| --- | --- |
| <span data-ttu-id="80cdb-287">Nové AzureWebApplicationEnvironment</span><span class="sxs-lookup"><span data-stu-id="80cdb-287">New-AzureWebApplicationEnvironment</span></span> |<span data-ttu-id="80cdb-288">Vytvoří prostředky Azure, jako je web nebo virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="80cdb-288">Creates Azure resources, such as a website or virtual machine.</span></span> |
| <span data-ttu-id="80cdb-289">Nové WebDeployPackage</span><span class="sxs-lookup"><span data-stu-id="80cdb-289">New-WebDeployPackage</span></span> |<span data-ttu-id="80cdb-290">Tato funkce není implementována.</span><span class="sxs-lookup"><span data-stu-id="80cdb-290">This function isn't implemented.</span></span> <span data-ttu-id="80cdb-291">Můžete přidat příkazy v této funkci můžete sestavit projekt.</span><span class="sxs-lookup"><span data-stu-id="80cdb-291">You can add commands in this function to build your project.</span></span> |
| <span data-ttu-id="80cdb-292">Publikování AzureWebApplication</span><span class="sxs-lookup"><span data-stu-id="80cdb-292">Publish-AzureWebApplication</span></span> |<span data-ttu-id="80cdb-293">Publikuje webovou aplikaci do Azure.</span><span class="sxs-lookup"><span data-stu-id="80cdb-293">Publishes a web application to Azure.</span></span> |
| <span data-ttu-id="80cdb-294">Publikovat webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="80cdb-294">Publish-WebApplication</span></span> |<span data-ttu-id="80cdb-295">Vytvoří a nasadí webových aplikací, virtuálních počítačů, databází SQL a účty úložiště pro webový projekt sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="80cdb-295">Creates and deploys Web Apps, virtual machines, SQL databases, and storage accounts for a Visual Studio web project.</span></span> |
| <span data-ttu-id="80cdb-296">Test-WebApplication</span><span class="sxs-lookup"><span data-stu-id="80cdb-296">Test-WebApplication</span></span> |<span data-ttu-id="80cdb-297">Tato funkce není implementována.</span><span class="sxs-lookup"><span data-stu-id="80cdb-297">This function isn't implemented.</span></span> <span data-ttu-id="80cdb-298">Můžete přidat příkazy v této funkci můžete testování vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="80cdb-298">You can add commands in this function to test your application.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="80cdb-299">Další kroky</span><span class="sxs-lookup"><span data-stu-id="80cdb-299">Next steps</span></span>
<span data-ttu-id="80cdb-300">Další informace o prostředí PowerShell skriptování čtení [skriptování v prostředí Windows PowerShell](https://technet.microsoft.com/library/bb978526.aspx) a jiné skripty prostředí Azure PowerShell v [centra skriptů](https://azure.microsoft.com/documentation/scripts/).</span><span class="sxs-lookup"><span data-stu-id="80cdb-300">Learn more about PowerShell scripting by reading [Scripting with Windows PowerShell](https://technet.microsoft.com/library/bb978526.aspx) and see other Azure PowerShell scripts at the [Script Center](https://azure.microsoft.com/documentation/scripts/).</span></span>
