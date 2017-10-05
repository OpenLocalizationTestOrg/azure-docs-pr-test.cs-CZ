---
title: "Nasazení virtuálního počítače Azure s Chef | Microsoft Docs"
description: "Další informace o použití Chef provedete nasazení automatizované virtuálního počítače a konfigurace v Microsoft Azure"
services: virtual-machines-windows
documentationcenter: 
author: diegoviso
manager: timlt
tags: azure-service-management,azure-resource-manager
editor: 
ms.assetid: 0b82ca70-89ed-496d-bb49-c04ae59b4523
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: diviso
ms.openlocfilehash: b6db0fbb4e0de896994954974ddcc39daad9c125
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a><span data-ttu-id="e7905-103">Automatizace nasazení virtuálních počítačů Azure pomocí Chefu</span><span class="sxs-lookup"><span data-stu-id="e7905-103">Automating Azure virtual machine deployment with Chef</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="e7905-104">Chef je skvělý nástroj pro doručení automatizace a požadovaného stavu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e7905-104">Chef is a great tool for delivering automation and desired state configurations.</span></span>

<span data-ttu-id="e7905-105">Naše nejnovější verzi api cloudu Chef poskytuje bezproblémovou integraci s Azure, což vám umožní zřídit a nasadit stavům konfigurace prostřednictvím jeden příkaz.</span><span class="sxs-lookup"><span data-stu-id="e7905-105">With our latest cloud-api release, Chef provides seamless integration with Azure, giving you the ability to provision and deploy configuration states through a single command.</span></span>

<span data-ttu-id="e7905-106">V tomto článku I budete ukazují, jak nastavit prostředí Chef vás provede procesem vytvoření zásadu nebo "Kuchařka" a pak tento kuchařka nasazení pro virtuální počítač Azure a zřizovat virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="e7905-106">In this article, I’ll show you how to set up your Chef environment to provision Azure virtual machines and walk you through creating a policy or “CookBook” and then deploying this cookbook to an Azure virtual machine.</span></span>

<span data-ttu-id="e7905-107">Můžeme začít!</span><span class="sxs-lookup"><span data-stu-id="e7905-107">Let’s begin!</span></span>

## <a name="chef-basics"></a><span data-ttu-id="e7905-108">Základy Chef</span><span class="sxs-lookup"><span data-stu-id="e7905-108">Chef basics</span></span>
<span data-ttu-id="e7905-109">Než začnete, naznačují, že zkontrolujte se základními koncepty Chef.</span><span class="sxs-lookup"><span data-stu-id="e7905-109">Before you begin, I suggest you review the basic concepts of Chef.</span></span> <span data-ttu-id="e7905-110">Je skvělým materiálu <a href="http://www.chef.io/chef" target="_blank">zde</a> a I doporučujeme mít Rychlé čtení před provedením tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="e7905-110">There is great material <a href="http://www.chef.io/chef" target="_blank">here</a> and I recommend you have a quick read before you attempt this walkthrough.</span></span> <span data-ttu-id="e7905-111">Základy I bude recap, ale než začneme.</span><span class="sxs-lookup"><span data-stu-id="e7905-111">I will however recap the basics before we get started.</span></span>

<span data-ttu-id="e7905-112">Následující diagram znázorňuje architekturu Chef vysoké úrovně.</span><span class="sxs-lookup"><span data-stu-id="e7905-112">The following diagram depicts the high-level Chef architecture.</span></span>

![][2]

<span data-ttu-id="e7905-113">Chef má třemi hlavními komponentami architektury: Chef, Chef klienta (uzel), Chef pracovní stanici a serveru.</span><span class="sxs-lookup"><span data-stu-id="e7905-113">Chef has three main architectural components: Chef Server, Chef Client (node), and Chef Workstation.</span></span>

<span data-ttu-id="e7905-114">Je Chef Server naše bod správy a existují dvě možnosti pro Chef Server: hostované řešení nebo v případě místních řešení.</span><span class="sxs-lookup"><span data-stu-id="e7905-114">The Chef Server is our management point and there are two options for the Chef Server: a hosted solution or an on-premises solution.</span></span> <span data-ttu-id="e7905-115">Použijeme hostované řešení.</span><span class="sxs-lookup"><span data-stu-id="e7905-115">We will be using a hosted solution.</span></span>

<span data-ttu-id="e7905-116">Klient Chef (uzel) je agenta, která se nachází na serverech, které spravujete.</span><span class="sxs-lookup"><span data-stu-id="e7905-116">The Chef Client (node) is the agent that sits on the servers you are managing.</span></span>

<span data-ttu-id="e7905-117">Pracovní stanice Chef je naše správce pracovní stanice, kde budeme vytvářet naše zásady a spustit příkazy pro správu.</span><span class="sxs-lookup"><span data-stu-id="e7905-117">The Chef Workstation is our admin workstation where we create our policies and execute our management commands.</span></span> <span data-ttu-id="e7905-118">Jsme spustit **nůž** příkazu z Chef pracovní stanice ke správě naše infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="e7905-118">We run the **knife** command from the Chef Workstation to manage our infrastructure.</span></span>

<span data-ttu-id="e7905-119">Je také koncepci "Cookbooks" a "Recepty".</span><span class="sxs-lookup"><span data-stu-id="e7905-119">There is also the concept of “Cookbooks” and “Recipes”.</span></span> <span data-ttu-id="e7905-120">Toto jsou efektivně zásady jsme definovat a aplikovat na našich serverech.</span><span class="sxs-lookup"><span data-stu-id="e7905-120">These are effectively the policies we define and apply to our servers.</span></span>

## <a name="preparing-the-workstation"></a><span data-ttu-id="e7905-121">Příprava pracovní stanice</span><span class="sxs-lookup"><span data-stu-id="e7905-121">Preparing the workstation</span></span>
<span data-ttu-id="e7905-122">Umožňuje nejprve rychlá příprava pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="e7905-122">First, lets prep the workstation.</span></span> <span data-ttu-id="e7905-123">Používám standardní pracovní stanice systému Windows.</span><span class="sxs-lookup"><span data-stu-id="e7905-123">I’m using a standard Windows workstation.</span></span> <span data-ttu-id="e7905-124">Je potřeba vytvořit adresář k uložení naše konfigurační soubory a cookbooks.</span><span class="sxs-lookup"><span data-stu-id="e7905-124">We need to create a directory to store our config files and cookbooks.</span></span>

<span data-ttu-id="e7905-125">Nejprve vytvořte adresář s názvem C:\chef.</span><span class="sxs-lookup"><span data-stu-id="e7905-125">First create a directory called C:\chef.</span></span>

<span data-ttu-id="e7905-126">Potom vytvořte druhý adresář s názvem c:\chef\cookbooks.</span><span class="sxs-lookup"><span data-stu-id="e7905-126">Then create a second directory called c:\chef\cookbooks.</span></span>

<span data-ttu-id="e7905-127">Nyní potřebujeme Stáhnout naše souborů nastavení Azure, Chef může komunikovat s naše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="e7905-127">We now need to download our Azure settings file so Chef can communicate with our Azure subscription.</span></span>

<!--Download your publish settings from [here](https://manage.windowsazure.com/publishsettings/).-->
<span data-ttu-id="e7905-128">Stáhněte si vaše nastavení pomocí PowerShell Azure publikování [Get-AzurePublishSettingsFile](https://docs.microsoft.com/en-us/powershell/module/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) příkaz.</span><span class="sxs-lookup"><span data-stu-id="e7905-128">Download your publish settings using the PowerShell Azure [Get-AzurePublishSettingsFile](https://docs.microsoft.com/en-us/powershell/module/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) command.</span></span> 

<span data-ttu-id="e7905-129">Uložte soubor nastavení publikování v C:\chef.</span><span class="sxs-lookup"><span data-stu-id="e7905-129">Save the publish settings file in C:\chef.</span></span>

## <a name="creating-a-managed-chef-account"></a><span data-ttu-id="e7905-130">Vytvoření spravované Chef účtu</span><span class="sxs-lookup"><span data-stu-id="e7905-130">Creating a managed Chef account</span></span>
<span data-ttu-id="e7905-131">Zaregistrujte si účet hostované Chef [zde](https://manage.chef.io/signup).</span><span class="sxs-lookup"><span data-stu-id="e7905-131">Sign up for a hosted Chef account [here](https://manage.chef.io/signup).</span></span>

<span data-ttu-id="e7905-132">Během procesu registrace se zobrazí výzva k vytvoření nové organizace.</span><span class="sxs-lookup"><span data-stu-id="e7905-132">During the signup process, you will be asked to create a new organization.</span></span>

![][3]

<span data-ttu-id="e7905-133">Po vytvoření vaší organizace, stáhněte si starter kit.</span><span class="sxs-lookup"><span data-stu-id="e7905-133">Once your organization is created, download the starter kit.</span></span>

![][4]

> [!NOTE]
> <span data-ttu-id="e7905-134">Pokud se zobrazí výzva, upozornění, že vaše klíče bude restartován, je ok pokračovat, protože máme nakonfigurovanou zatím žádná existující infrastrukturu.</span><span class="sxs-lookup"><span data-stu-id="e7905-134">If you receive a prompt warning you that your keys will be reset, it’s ok to proceed as we have no existing infrastructure configured as yet.</span></span>
> 
> 

<span data-ttu-id="e7905-135">Tento soubor zip starter kit obsahuje vaše organizace konfigurační soubory a klíče.</span><span class="sxs-lookup"><span data-stu-id="e7905-135">This starter kit zip file contains your organization config files and keys.</span></span>

## <a name="configuring-the-chef-workstation"></a><span data-ttu-id="e7905-136">Konfigurace Chef pracovní stanice</span><span class="sxs-lookup"><span data-stu-id="e7905-136">Configuring the Chef workstation</span></span>
<span data-ttu-id="e7905-137">Extrahujte obsah chef-starter.zip k C:\chef.</span><span class="sxs-lookup"><span data-stu-id="e7905-137">Extract the content of the chef-starter.zip to C:\chef.</span></span>

<span data-ttu-id="e7905-138">Zkopírujte všechny soubory pod chef. starter\chef úložišti\.chef do vašeho adresáře c:\chef.</span><span class="sxs-lookup"><span data-stu-id="e7905-138">Copy all files under chef-starter\chef-repo\.chef to your c:\chef directory.</span></span>

<span data-ttu-id="e7905-139">Adresáře by měl nyní vypadat podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="e7905-139">Your directory should now look something like the following example.</span></span>

![][5]

<span data-ttu-id="e7905-140">Teď byste měli mít čtyři soubory včetně Azure publikování v kořenovém c:\chef.</span><span class="sxs-lookup"><span data-stu-id="e7905-140">You should now have four files including the Azure publishing file in the root of c:\chef.</span></span>

<span data-ttu-id="e7905-141">Soubory PEM obsahují vaší organizace a správy privátních klíčů pro komunikaci při knife.rb soubor obsahuje konfiguraci nůž.</span><span class="sxs-lookup"><span data-stu-id="e7905-141">The PEM files contain your organization and admin private keys for communication while the knife.rb file contains your knife configuration.</span></span> <span data-ttu-id="e7905-142">Je potřeba upravit soubor knife.rb.</span><span class="sxs-lookup"><span data-stu-id="e7905-142">We will need to edit the knife.rb file.</span></span>

<span data-ttu-id="e7905-143">Otevřete soubor ve svém editoru volba a upravit "cookbook_path" odebráním nebo... / z cesty, zdá se, jak ukazuje následující.</span><span class="sxs-lookup"><span data-stu-id="e7905-143">Open the file in your editor of choice and modify the “cookbook_path” by removing the /../ from the path so it appears as shown next.</span></span>

    cookbook_path  ["#{current_dir}/cookbooks"]

<span data-ttu-id="e7905-144">Také přidejte následující řádek odrážející název vaší Azure soubor nastavení publikování.</span><span class="sxs-lookup"><span data-stu-id="e7905-144">Also add the following line reflecting the name of your Azure publish settings file.</span></span>

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

<span data-ttu-id="e7905-145">Váš soubor knife.rb by teď měl vypadat podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="e7905-145">Your knife.rb file should now look similar to the following example.</span></span>

![][6]

<span data-ttu-id="e7905-146">Tyto řádky zajišťují, že odkazuje na adresář cookbooks v c:\chef\cookbooks nůž a také naše soubor s nastavením publikování Azure používá během operace v Azure.</span><span class="sxs-lookup"><span data-stu-id="e7905-146">These lines will ensure that Knife references the cookbooks directory under c:\chef\cookbooks, and also uses our Azure Publish Settings file during Azure operations.</span></span>

## <a name="installing-the-chef-development-kit"></a><span data-ttu-id="e7905-147">Instalace Chef Development Kit</span><span class="sxs-lookup"><span data-stu-id="e7905-147">Installing the Chef Development Kit</span></span>
<span data-ttu-id="e7905-148">Další [stáhněte a nainstalujte](http://downloads.getchef.com/chef-dk/windows) ChefDK (Chef Development Kit) nastavit pracovní stanice Chef.</span><span class="sxs-lookup"><span data-stu-id="e7905-148">Next [download and install](http://downloads.getchef.com/chef-dk/windows) the ChefDK (Chef Development Kit) to set up your Chef Workstation.</span></span>

![][7]

<span data-ttu-id="e7905-149">Ve výchozím umístěním c:\opscode nainstalujte.</span><span class="sxs-lookup"><span data-stu-id="e7905-149">Install in the default location of c:\opscode.</span></span> <span data-ttu-id="e7905-150">Tato instalace bude trvat přibližně 10 minut.</span><span class="sxs-lookup"><span data-stu-id="e7905-150">This install will take around 10 minutes.</span></span>

<span data-ttu-id="e7905-151">Potvrďte, že vaše proměnná PATH obsahuje položky pro C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin</span><span class="sxs-lookup"><span data-stu-id="e7905-151">Confirm your PATH variable contains entries for C:\opscode\chefdk\bin;C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin</span></span>

<span data-ttu-id="e7905-152">Pokud nejsou existuje, ujistěte se, že přidáte tyto cesty!</span><span class="sxs-lookup"><span data-stu-id="e7905-152">If they are not there, make sure you add these paths!</span></span>

<span data-ttu-id="e7905-153">*VŠIMNĚTE SI, ŽE JE DŮLEŽITÉ POŘADÍ CESTY!*</span><span class="sxs-lookup"><span data-stu-id="e7905-153">*NOTE THE ORDER OF THE PATH IS IMPORTANT!*</span></span> <span data-ttu-id="e7905-154">Pokud vaše opscode cesty nejsou ve správném pořadí budete mít problémy.</span><span class="sxs-lookup"><span data-stu-id="e7905-154">If your opscode paths are not in the correct order you will have issues.</span></span>

<span data-ttu-id="e7905-155">Než budete pokračovat, restartujte počítač pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="e7905-155">Reboot your workstation before you continue.</span></span>

<span data-ttu-id="e7905-156">V dalším kroku se nainstaluje rozšíření nůž Azure.</span><span class="sxs-lookup"><span data-stu-id="e7905-156">Next, we will install the Knife Azure extension.</span></span> <span data-ttu-id="e7905-157">To poskytuje nůž s "Modul plug-in Azure".</span><span class="sxs-lookup"><span data-stu-id="e7905-157">This provides Knife with the “Azure Plugin”.</span></span>

<span data-ttu-id="e7905-158">Spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="e7905-158">Run the following command.</span></span>

    chef gem install knife-azure ––pre

> [!NOTE]
> <span data-ttu-id="e7905-159">– Před argument zajistí, že se vám nejnovější verzi RC sad Azure nůž modul plug-in, který poskytuje přístup k nejnovější sadu rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e7905-159">The –pre argument ensures you are receiving the latest RC version of the Knife Azure Plugin which provides access to the latest set of APIs.</span></span>
> 
> 

<span data-ttu-id="e7905-160">Je pravděpodobné, že počet závislostí, budou také instalována ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="e7905-160">It’s likely that a number of dependencies will also be installed at the same time.</span></span>

![][8]

<span data-ttu-id="e7905-161">K zajištění, že je všechno správně nastavené, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="e7905-161">To ensure everything is configured correctly, run the following command.</span></span>

    knife azure image list

<span data-ttu-id="e7905-162">Pokud se vše správně nakonfigurovaná, zobrazí se seznam dostupných imagí Azure procházení.</span><span class="sxs-lookup"><span data-stu-id="e7905-162">If everything is configured correctly, you will see a list of available Azure images scroll through.</span></span>

<span data-ttu-id="e7905-163">Blahopřejeme.</span><span class="sxs-lookup"><span data-stu-id="e7905-163">Congratulations.</span></span> <span data-ttu-id="e7905-164">Pracovní stanici je nastavený!</span><span class="sxs-lookup"><span data-stu-id="e7905-164">The workstation is set up!</span></span>

## <a name="creating-a-cookbook"></a><span data-ttu-id="e7905-165">Vytváření kuchařka</span><span class="sxs-lookup"><span data-stu-id="e7905-165">Creating a Cookbook</span></span>
<span data-ttu-id="e7905-166">Kuchařka slouží Chef definovat sadu příkazů, které chcete spustit na spravovaného klienta.</span><span class="sxs-lookup"><span data-stu-id="e7905-166">A Cookbook is used by Chef to define a set of commands that you wish to execute on your managed client.</span></span> <span data-ttu-id="e7905-167">Vytváření kuchařka je jednoduchý a používáme **chef generovat kuchařka** příkazu vygenerujte naše kuchařka šablony.</span><span class="sxs-lookup"><span data-stu-id="e7905-167">Creating a Cookbook is straightforward and we use the **chef generate cookbook** command to generate our Cookbook template.</span></span> <span data-ttu-id="e7905-168">I bude možné volání Moje kuchařka webový server jako nastavit jako zásadu, která automaticky nasadí služby IIS.</span><span class="sxs-lookup"><span data-stu-id="e7905-168">I will be calling my Cookbook web server as I would like a policy that automatically deploys IIS.</span></span>

<span data-ttu-id="e7905-169">V části adresáře C:\Chef spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="e7905-169">Under your C:\Chef directory run the following command.</span></span>

    chef generate cookbook webserver

<span data-ttu-id="e7905-170">Tím se vygeneruje určitou sadu souborů v adresáři C:\Chef\cookbooks\webserver.</span><span class="sxs-lookup"><span data-stu-id="e7905-170">This will generate a set of files under the directory C:\Chef\cookbooks\webserver.</span></span> <span data-ttu-id="e7905-171">Nyní potřebujeme definovat sadu příkazů, že rádi bychom znali našeho Chef klienta ke spouštění na našem spravované virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e7905-171">We now need to define the set of commands we would like our Chef client to execute on our managed virtual machine.</span></span>

<span data-ttu-id="e7905-172">Příkazy jsou uloženy v souboru default.rb.</span><span class="sxs-lookup"><span data-stu-id="e7905-172">The commands are stored in the file default.rb.</span></span> <span data-ttu-id="e7905-173">V tomto souboru I budete definování sadu příkazů, který nainstaluje službu IIS, spustí službu IIS a zkopíruje soubor šablony do složky wwwroot.</span><span class="sxs-lookup"><span data-stu-id="e7905-173">In this file, I’ll be defining a set of commands that installs IIS, starts IIS and copies a template file to the wwwroot folder.</span></span>

<span data-ttu-id="e7905-174">Upravte soubor C:\chef\cookbooks\webserver\recipes\default.rb a přidejte následující řádky.</span><span class="sxs-lookup"><span data-stu-id="e7905-174">Modify the C:\chef\cookbooks\webserver\recipes\default.rb file and add the following lines.</span></span>

    powershell_script 'Install IIS' do
         action :run
         code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
         action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
         source 'Default.htm.erb'
         rights :read, 'Everyone'
    end

<span data-ttu-id="e7905-175">Až budete hotoví, uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="e7905-175">Save the file once you are done.</span></span>

## <a name="creating-a-template"></a><span data-ttu-id="e7905-176">Vytvoření šablony</span><span class="sxs-lookup"><span data-stu-id="e7905-176">Creating a template</span></span>
<span data-ttu-id="e7905-177">Jak jsme už bylo zmíněno dříve, je potřeba generovat soubor šablony, který se použije jako naší stránce s default.html.</span><span class="sxs-lookup"><span data-stu-id="e7905-177">As we mentioned previously, we need to generate a template file which will be used as our default.html page.</span></span>

<span data-ttu-id="e7905-178">Spusťte následující příkaz pro vytvoření šablony.</span><span class="sxs-lookup"><span data-stu-id="e7905-178">Run the following command to generate the template.</span></span>

    chef generate template webserver Default.htm

<span data-ttu-id="e7905-179">Nyní přejděte k souboru C:\chef\cookbooks\webserver\templates\default\Default.htm.erb.</span><span class="sxs-lookup"><span data-stu-id="e7905-179">Now navigate to the C:\chef\cookbooks\webserver\templates\default\Default.htm.erb file.</span></span> <span data-ttu-id="e7905-180">Upravte soubor přidáním jednoduchý "Hello, World" HTML kód a pak soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="e7905-180">Edit the file by adding some simple “Hello World” HTML code, and then save the file.</span></span>

## <a name="upload-the-cookbook-to-the-chef-server"></a><span data-ttu-id="e7905-181">Nahrát na Server Chef kuchařka</span><span class="sxs-lookup"><span data-stu-id="e7905-181">Upload the Cookbook to the Chef Server</span></span>
<span data-ttu-id="e7905-182">V tomto kroku jsme jsou pořízení kopie Cookbook, který jsme vytvořili na našem místního počítače a pak ho nahrát na Server hostované Chef.</span><span class="sxs-lookup"><span data-stu-id="e7905-182">In this step, we are taking a copy of the Cookbook that we have created on our local machine and uploading it to the Chef Hosted Server.</span></span> <span data-ttu-id="e7905-183">Po nahrání kuchařka se zobrazí v části **zásad** kartě.</span><span class="sxs-lookup"><span data-stu-id="e7905-183">Once uploaded, the Cookbook will appear under the **Policy** tab.</span></span>

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a><span data-ttu-id="e7905-184">Nasazení virtuálního počítače s nůž Azure</span><span class="sxs-lookup"><span data-stu-id="e7905-184">Deploy a virtual machine with Knife Azure</span></span>
<span data-ttu-id="e7905-185">Nyní změníme nasadit virtuální počítač Azure a použít kuchařka "Webový server", který nainstaluje naše IIS webové služby a výchozí webové stránky.</span><span class="sxs-lookup"><span data-stu-id="e7905-185">We will now deploy an Azure virtual machine and apply the “Webserver” Cookbook which will install our IIS web service and default web page.</span></span>

<span data-ttu-id="e7905-186">Chcete-li to provést, použijte **server nůž azure vytvořit** příkaz.</span><span class="sxs-lookup"><span data-stu-id="e7905-186">In order to do this, use the **knife azure server create** command.</span></span>

<span data-ttu-id="e7905-187">Se, že příklad příkazu se zobrazí další.</span><span class="sxs-lookup"><span data-stu-id="e7905-187">Am example of the command appears next.</span></span>

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

<span data-ttu-id="e7905-188">Parametry jsou není potřeba vysvětlovat.</span><span class="sxs-lookup"><span data-stu-id="e7905-188">The parameters are self-explanatory.</span></span> <span data-ttu-id="e7905-189">Nahraďte konkrétní proměnných a spustit.</span><span class="sxs-lookup"><span data-stu-id="e7905-189">Substitute your particular variables and run.</span></span>

> [!NOTE]
> <span data-ttu-id="e7905-190">Pomocí příkazového řádku, I se také automatizaci Moje pravidla filtru koncového bodu sítě pomocí parametru koncové body – tcp.</span><span class="sxs-lookup"><span data-stu-id="e7905-190">Through the the command line, I’m also automating my endpoint network filter rules by using the –tcp-endpoints parameter.</span></span> <span data-ttu-id="e7905-191">I otevřeli porty 80 a 3389 k poskytování přístupu k Moje webová stránka a relaci protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="e7905-191">I’ve opened up ports 80 and 3389 to provide access to my web page and RDP session.</span></span>
> 
> 

<span data-ttu-id="e7905-192">Po spuštění příkazu, přejděte na portál Azure a zobrazí se váš počítač začít zřizování.</span><span class="sxs-lookup"><span data-stu-id="e7905-192">Once you run the command, go to the Azure portal and you will see your machine begin to provision.</span></span>

![][13]

<span data-ttu-id="e7905-193">Do příkazového řádku se zobrazí na další.</span><span class="sxs-lookup"><span data-stu-id="e7905-193">The command prompt appears next.</span></span>

![][10]

<span data-ttu-id="e7905-194">Po dokončení nasazení jsme byste měli mít připojení k webové službě přes port 80, jako jsme byl otevřen port při jsme zřízení virtuálního počítače pomocí příkazu nůž Azure.</span><span class="sxs-lookup"><span data-stu-id="e7905-194">Once the deployment is complete, we should be able to connect to the web service over port 80 as we had opened the port when we provisioned the virtual machine with the Knife Azure command.</span></span> <span data-ttu-id="e7905-195">Tento virtuální počítač je pouze virtuální počítač v mé cloudové služby, budete ho připojení s adresou url cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="e7905-195">As this virtual machine is the only virtual machine in my cloud service, I’ll connect it with the cloud service url.</span></span>

![][11]

<span data-ttu-id="e7905-196">Jak vidíte, se zobrazí chybové tvůrčí s vlastního kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="e7905-196">As you can see, I got creative with my HTML code.</span></span>

<span data-ttu-id="e7905-197">Nezapomeňte, že jsme také můžete připojit přes relaci protokolu RDP z portálu Azure přes port 3389.</span><span class="sxs-lookup"><span data-stu-id="e7905-197">Don’t forget we can also connect through an RDP session from the Azure portal via port 3389.</span></span>

<span data-ttu-id="e7905-198">Ať, že tento byl užitečné!</span><span class="sxs-lookup"><span data-stu-id="e7905-198">I hope this has been helpful!</span></span> <span data-ttu-id="e7905-199">Přejděte a spusťte infrastruktury jako cesty kódu s Azure ještě dnes!</span><span class="sxs-lookup"><span data-stu-id="e7905-199">Go  and start your infrastructure as code journey with Azure today!</span></span>

<!--Image references-->
[2]: media/chef-automation/2.png
[3]: media/chef-automation/3.png
[4]: media/chef-automation/4.png
[5]: media/chef-automation/5.png
[6]: media/chef-automation/6.png
[7]: media/chef-automation/7.png
[8]: media/chef-automation/8.png
[9]: media/chef-automation/9.png
[10]: media/chef-automation/10.png
[11]: media/chef-automation/11.png
[13]: media/chef-automation/13.png


<!--Link references-->
