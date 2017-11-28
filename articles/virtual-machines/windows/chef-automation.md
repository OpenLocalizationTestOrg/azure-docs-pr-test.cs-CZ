---
title: "nasazení virtuálního počítače aaaAzure s Chef | Microsoft Docs"
description: "Zjistěte, jak toouse Chef toodo automatizované nasazení virtuálního počítače a konfigurace v Microsoft Azure"
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
ms.openlocfilehash: c5ea98c673b2ee75dd4cedf27e50330af05230d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a><span data-ttu-id="fac5e-103">Automatizace nasazení virtuálních počítačů Azure pomocí Chefu</span><span class="sxs-lookup"><span data-stu-id="fac5e-103">Automating Azure virtual machine deployment with Chef</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="fac5e-104">Chef je skvělý nástroj pro doručení automatizace a požadovaného stavu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fac5e-104">Chef is a great tool for delivering automation and desired state configurations.</span></span>

<span data-ttu-id="fac5e-105">Naše nejnovější verzi api cloudu Chef zajišťuje bezproblémovou integraci s Azure, budete hello možnost tooprovision a nasadit stavům konfigurace prostřednictvím jeden příkaz.</span><span class="sxs-lookup"><span data-stu-id="fac5e-105">With our latest cloud-api release, Chef provides seamless integration with Azure, giving you hello ability tooprovision and deploy configuration states through a single command.</span></span>

<span data-ttu-id="fac5e-106">V tomto článku I vám ukáže, jak tooset až tooprovision prostředí vaší Chef Azure virtuální počítače, a provede vás vytvořením zásady nebo "Kuchařka" a pak nasazení této kuchařka tooan virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="fac5e-106">In this article, I’ll show you how tooset up your Chef environment tooprovision Azure virtual machines and walk you through creating a policy or “CookBook” and then deploying this cookbook tooan Azure virtual machine.</span></span>

<span data-ttu-id="fac5e-107">Můžeme začít!</span><span class="sxs-lookup"><span data-stu-id="fac5e-107">Let’s begin!</span></span>

## <a name="chef-basics"></a><span data-ttu-id="fac5e-108">Základy Chef</span><span class="sxs-lookup"><span data-stu-id="fac5e-108">Chef basics</span></span>
<span data-ttu-id="fac5e-109">Než začnete, naznačují, že zkontrolujte hello základní koncepty Chef.</span><span class="sxs-lookup"><span data-stu-id="fac5e-109">Before you begin, I suggest you review hello basic concepts of Chef.</span></span> <span data-ttu-id="fac5e-110">Je skvělým materiálu <a href="http://www.chef.io/chef" target="_blank">zde</a> a I doporučujeme mít Rychlé čtení před provedením tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="fac5e-110">There is great material <a href="http://www.chef.io/chef" target="_blank">here</a> and I recommend you have a quick read before you attempt this walkthrough.</span></span> <span data-ttu-id="fac5e-111">Základy hello I bude recap, ale než začneme.</span><span class="sxs-lookup"><span data-stu-id="fac5e-111">I will however recap hello basics before we get started.</span></span>

<span data-ttu-id="fac5e-112">Hello následující diagram znázorňuje Chef Architektura vysoké úrovně hello.</span><span class="sxs-lookup"><span data-stu-id="fac5e-112">hello following diagram depicts hello high-level Chef architecture.</span></span>

![][2]

<span data-ttu-id="fac5e-113">Chef má třemi hlavními komponentami architektury: Chef, Chef klienta (uzel), Chef pracovní stanici a serveru.</span><span class="sxs-lookup"><span data-stu-id="fac5e-113">Chef has three main architectural components: Chef Server, Chef Client (node), and Chef Workstation.</span></span>

<span data-ttu-id="fac5e-114">Hello Chef serveru je naše bod správy a existují dvě možnosti pro hello Chef serveru: hostované řešení nebo v případě místních řešení.</span><span class="sxs-lookup"><span data-stu-id="fac5e-114">hello Chef Server is our management point and there are two options for hello Chef Server: a hosted solution or an on-premises solution.</span></span> <span data-ttu-id="fac5e-115">Použijeme hostované řešení.</span><span class="sxs-lookup"><span data-stu-id="fac5e-115">We will be using a hosted solution.</span></span>

<span data-ttu-id="fac5e-116">Hello Chef klienta (uzel) je hello agenta, která se nachází na hello servery, které spravujete.</span><span class="sxs-lookup"><span data-stu-id="fac5e-116">hello Chef Client (node) is hello agent that sits on hello servers you are managing.</span></span>

<span data-ttu-id="fac5e-117">Hello Chef pracovní stanici je naše správce pracovní stanice, kde budeme vytvářet naše zásady a spustit příkazy pro správu.</span><span class="sxs-lookup"><span data-stu-id="fac5e-117">hello Chef Workstation is our admin workstation where we create our policies and execute our management commands.</span></span> <span data-ttu-id="fac5e-118">Spustíme hello **nůž** příkaz z hello pracovní stanice Chef toomanage naše infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="fac5e-118">We run hello **knife** command from hello Chef Workstation toomanage our infrastructure.</span></span>

<span data-ttu-id="fac5e-119">Je také hello konceptu "Cookbooks" a "Recepty".</span><span class="sxs-lookup"><span data-stu-id="fac5e-119">There is also hello concept of “Cookbooks” and “Recipes”.</span></span> <span data-ttu-id="fac5e-120">Toto jsou efektivně hello zásady jsme definovat a použít tooour servery.</span><span class="sxs-lookup"><span data-stu-id="fac5e-120">These are effectively hello policies we define and apply tooour servers.</span></span>

## <a name="preparing-hello-workstation"></a><span data-ttu-id="fac5e-121">Příprava pracovní stanice hello</span><span class="sxs-lookup"><span data-stu-id="fac5e-121">Preparing hello workstation</span></span>
<span data-ttu-id="fac5e-122">Nejprve umožňuje přípravný hello pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="fac5e-122">First, lets prep hello workstation.</span></span> <span data-ttu-id="fac5e-123">Používám standardní pracovní stanice systému Windows.</span><span class="sxs-lookup"><span data-stu-id="fac5e-123">I’m using a standard Windows workstation.</span></span> <span data-ttu-id="fac5e-124">Naše konfigurační soubory a cookbooks potřebujeme toocreate toostore adresáře.</span><span class="sxs-lookup"><span data-stu-id="fac5e-124">We need toocreate a directory toostore our config files and cookbooks.</span></span>

<span data-ttu-id="fac5e-125">Nejprve vytvořte adresář s názvem C:\chef.</span><span class="sxs-lookup"><span data-stu-id="fac5e-125">First create a directory called C:\chef.</span></span>

<span data-ttu-id="fac5e-126">Potom vytvořte druhý adresář s názvem c:\chef\cookbooks.</span><span class="sxs-lookup"><span data-stu-id="fac5e-126">Then create a second directory called c:\chef\cookbooks.</span></span>

<span data-ttu-id="fac5e-127">Nyní potřebujeme toodownload naše souborů nastavení Azure, Chef může komunikovat s naše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="fac5e-127">We now need toodownload our Azure settings file so Chef can communicate with our Azure subscription.</span></span>

<!--Download your publish settings from [here](https://manage.windowsazure.com/publishsettings/).-->
<span data-ttu-id="fac5e-128">Stáhnout vaše nastavení pomocí PowerShell Azure hello publikování [Get-AzurePublishSettingsFile](https://docs.microsoft.com/en-us/powershell/module/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fac5e-128">Download your publish settings using hello PowerShell Azure [Get-AzurePublishSettingsFile](https://docs.microsoft.com/en-us/powershell/module/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) command.</span></span> 

<span data-ttu-id="fac5e-129">Uložte hello soubor nastavení publikování v C:\chef.</span><span class="sxs-lookup"><span data-stu-id="fac5e-129">Save hello publish settings file in C:\chef.</span></span>

## <a name="creating-a-managed-chef-account"></a><span data-ttu-id="fac5e-130">Vytvoření spravované Chef účtu</span><span class="sxs-lookup"><span data-stu-id="fac5e-130">Creating a managed Chef account</span></span>
<span data-ttu-id="fac5e-131">Zaregistrujte si účet hostované Chef [zde](https://manage.chef.io/signup).</span><span class="sxs-lookup"><span data-stu-id="fac5e-131">Sign up for a hosted Chef account [here](https://manage.chef.io/signup).</span></span>

<span data-ttu-id="fac5e-132">Během procesu registrace hello, nebudete vyzváni toocreate nové organizace.</span><span class="sxs-lookup"><span data-stu-id="fac5e-132">During hello signup process, you will be asked toocreate a new organization.</span></span>

![][3]

<span data-ttu-id="fac5e-133">Po vytvoření vaší organizace stáhněte hello starter kit.</span><span class="sxs-lookup"><span data-stu-id="fac5e-133">Once your organization is created, download hello starter kit.</span></span>

![][4]

> [!NOTE]
> <span data-ttu-id="fac5e-134">Pokud se zobrazí výzva, upozornění, že vaše klíče bude restartován, je ok tooproceed jako máme nakonfigurovanou zatím žádná existující infrastrukturu.</span><span class="sxs-lookup"><span data-stu-id="fac5e-134">If you receive a prompt warning you that your keys will be reset, it’s ok tooproceed as we have no existing infrastructure configured as yet.</span></span>
> 
> 

<span data-ttu-id="fac5e-135">Tento soubor zip starter kit obsahuje vaše organizace konfigurační soubory a klíče.</span><span class="sxs-lookup"><span data-stu-id="fac5e-135">This starter kit zip file contains your organization config files and keys.</span></span>

## <a name="configuring-hello-chef-workstation"></a><span data-ttu-id="fac5e-136">Konfigurace hello Chef pracovní stanice</span><span class="sxs-lookup"><span data-stu-id="fac5e-136">Configuring hello Chef workstation</span></span>
<span data-ttu-id="fac5e-137">Extrahujte obsah hello tooC:\chef chef starter.zip hello.</span><span class="sxs-lookup"><span data-stu-id="fac5e-137">Extract hello content of hello chef-starter.zip tooC:\chef.</span></span>

<span data-ttu-id="fac5e-138">Zkopírujte všechny soubory pod chef. starter\chef úložišti\.chef tooyour c:\chef adresáře.</span><span class="sxs-lookup"><span data-stu-id="fac5e-138">Copy all files under chef-starter\chef-repo\.chef tooyour c:\chef directory.</span></span>

<span data-ttu-id="fac5e-139">Adresáře by měl nyní vypadat podobně jako následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="fac5e-139">Your directory should now look something like hello following example.</span></span>

![][5]

<span data-ttu-id="fac5e-140">Teď byste měli mít čtyři soubory včetně hello Azure publikování hello kořenové c:\chef.</span><span class="sxs-lookup"><span data-stu-id="fac5e-140">You should now have four files including hello Azure publishing file in hello root of c:\chef.</span></span>

<span data-ttu-id="fac5e-141">soubory PEM Hello obsahovat vaší organizace a správy privátních klíčů pro komunikaci, zatímco hello knife.rb soubor obsahuje konfiguraci nůž.</span><span class="sxs-lookup"><span data-stu-id="fac5e-141">hello PEM files contain your organization and admin private keys for communication while hello knife.rb file contains your knife configuration.</span></span> <span data-ttu-id="fac5e-142">Potřebujeme tooedit hello knife.rb souboru.</span><span class="sxs-lookup"><span data-stu-id="fac5e-142">We will need tooedit hello knife.rb file.</span></span>

<span data-ttu-id="fac5e-143">Otevřete soubor hello ve svém editoru volba a upravit hello "cookbook_path" odebráním hello nebo... z hello cestu, zdá se, jak ukazuje následující.</span><span class="sxs-lookup"><span data-stu-id="fac5e-143">Open hello file in your editor of choice and modify hello “cookbook_path” by removing hello /../ from hello path so it appears as shown next.</span></span>

    cookbook_path  ["#{current_dir}/cookbooks"]

<span data-ttu-id="fac5e-144">Také přidat hello následující řádek odrážející hello název vaší Azure soubor nastavení publikování.</span><span class="sxs-lookup"><span data-stu-id="fac5e-144">Also add hello following line reflecting hello name of your Azure publish settings file.</span></span>

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

<span data-ttu-id="fac5e-145">Váš soubor knife.rb by teď měl vypadat podobně jako toohello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="fac5e-145">Your knife.rb file should now look similar toohello following example.</span></span>

![][6]

<span data-ttu-id="fac5e-146">Tyto řádky zajišťují, že odkazuje na adresář cookbooks hello c:\chef\cookbooks nůž a také naše soubor s nastavením publikování Azure používá během operace v Azure.</span><span class="sxs-lookup"><span data-stu-id="fac5e-146">These lines will ensure that Knife references hello cookbooks directory under c:\chef\cookbooks, and also uses our Azure Publish Settings file during Azure operations.</span></span>

## <a name="installing-hello-chef-development-kit"></a><span data-ttu-id="fac5e-147">Instalace hello Chef Development Kit</span><span class="sxs-lookup"><span data-stu-id="fac5e-147">Installing hello Chef Development Kit</span></span>
<span data-ttu-id="fac5e-148">Další [stáhněte a nainstalujte](http://downloads.getchef.com/chef-dk/windows) hello tooset ChefDK (Chef Development Kit) do pracovní stanice Chef.</span><span class="sxs-lookup"><span data-stu-id="fac5e-148">Next [download and install](http://downloads.getchef.com/chef-dk/windows) hello ChefDK (Chef Development Kit) tooset up your Chef Workstation.</span></span>

![][7]

<span data-ttu-id="fac5e-149">Ve výchozím umístěním hello c:\opscode nainstalujte.</span><span class="sxs-lookup"><span data-stu-id="fac5e-149">Install in hello default location of c:\opscode.</span></span> <span data-ttu-id="fac5e-150">Tato instalace bude trvat přibližně 10 minut.</span><span class="sxs-lookup"><span data-stu-id="fac5e-150">This install will take around 10 minutes.</span></span>

<span data-ttu-id="fac5e-151">Potvrďte, že vaše proměnná PATH obsahuje položky pro C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin</span><span class="sxs-lookup"><span data-stu-id="fac5e-151">Confirm your PATH variable contains entries for C:\opscode\chefdk\bin;C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin</span></span>

<span data-ttu-id="fac5e-152">Pokud nejsou existuje, ujistěte se, že přidáte tyto cesty!</span><span class="sxs-lookup"><span data-stu-id="fac5e-152">If they are not there, make sure you add these paths!</span></span>

<span data-ttu-id="fac5e-153">*Všimněte si hello pořadí z hello cesta je důležité!*</span><span class="sxs-lookup"><span data-stu-id="fac5e-153">*NOTE hello ORDER OF hello PATH IS IMPORTANT!*</span></span> <span data-ttu-id="fac5e-154">Pokud vaše opscode cesty nejsou ve správném pořadí hello budete mít problémy.</span><span class="sxs-lookup"><span data-stu-id="fac5e-154">If your opscode paths are not in hello correct order you will have issues.</span></span>

<span data-ttu-id="fac5e-155">Než budete pokračovat, restartujte počítač pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="fac5e-155">Reboot your workstation before you continue.</span></span>

<span data-ttu-id="fac5e-156">V dalším kroku se nainstaluje rozšíření Azure nůž hello.</span><span class="sxs-lookup"><span data-stu-id="fac5e-156">Next, we will install hello Knife Azure extension.</span></span> <span data-ttu-id="fac5e-157">To poskytuje nůž hello "Modul plug-in Azure".</span><span class="sxs-lookup"><span data-stu-id="fac5e-157">This provides Knife with hello “Azure Plugin”.</span></span>

<span data-ttu-id="fac5e-158">Spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="fac5e-158">Run hello following command.</span></span>

    chef gem install knife-azure ––pre

> [!NOTE]
> <span data-ttu-id="fac5e-159">argument – před Hello zajistí, že se vám hello nejnovější verzi RC sad hello nůž modul plug-in Azure, který poskytuje přístup toohello nejnovější sadu rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fac5e-159">hello –pre argument ensures you are receiving hello latest RC version of hello Knife Azure Plugin which provides access toohello latest set of APIs.</span></span>
> 
> 

<span data-ttu-id="fac5e-160">Je pravděpodobné, počet závislosti budou také instalována na hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="fac5e-160">It’s likely that a number of dependencies will also be installed at hello same time.</span></span>

![][8]

<span data-ttu-id="fac5e-161">tooensure všechno, co je nakonfigurována správně, spusťte hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="fac5e-161">tooensure everything is configured correctly, run hello following command.</span></span>

    knife azure image list

<span data-ttu-id="fac5e-162">Pokud se vše správně nakonfigurovaná, zobrazí se seznam dostupných imagí Azure procházení.</span><span class="sxs-lookup"><span data-stu-id="fac5e-162">If everything is configured correctly, you will see a list of available Azure images scroll through.</span></span>

<span data-ttu-id="fac5e-163">Blahopřejeme.</span><span class="sxs-lookup"><span data-stu-id="fac5e-163">Congratulations.</span></span> <span data-ttu-id="fac5e-164">pracovní stanice Hello nastavení!</span><span class="sxs-lookup"><span data-stu-id="fac5e-164">hello workstation is set up!</span></span>

## <a name="creating-a-cookbook"></a><span data-ttu-id="fac5e-165">Vytváření kuchařka</span><span class="sxs-lookup"><span data-stu-id="fac5e-165">Creating a Cookbook</span></span>
<span data-ttu-id="fac5e-166">Používá kuchařka Chef toodefine sadu příkazů na vašeho klienta spravovaného chcete tooexecute.</span><span class="sxs-lookup"><span data-stu-id="fac5e-166">A Cookbook is used by Chef toodefine a set of commands that you wish tooexecute on your managed client.</span></span> <span data-ttu-id="fac5e-167">Vytváření kuchařka je jednoduchý a používáme hello **chef generovat kuchařka** příkaz toogenerate naše kuchařka šablony.</span><span class="sxs-lookup"><span data-stu-id="fac5e-167">Creating a Cookbook is straightforward and we use hello **chef generate cookbook** command toogenerate our Cookbook template.</span></span> <span data-ttu-id="fac5e-168">I bude možné volání Moje kuchařka webový server jako nastavit jako zásadu, která automaticky nasadí služby IIS.</span><span class="sxs-lookup"><span data-stu-id="fac5e-168">I will be calling my Cookbook web server as I would like a policy that automatically deploys IIS.</span></span>

<span data-ttu-id="fac5e-169">V části adresáře C:\Chef spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="fac5e-169">Under your C:\Chef directory run hello following command.</span></span>

    chef generate cookbook webserver

<span data-ttu-id="fac5e-170">Tím se vygeneruje určitou sadu souborů v adresáři hello C:\Chef\cookbooks\webserver.</span><span class="sxs-lookup"><span data-stu-id="fac5e-170">This will generate a set of files under hello directory C:\Chef\cookbooks\webserver.</span></span> <span data-ttu-id="fac5e-171">Nyní potřebujeme toodefine hello sadu příkazů, že naše tooexecute klienta Chef rádi bychom znali na našem spravované virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fac5e-171">We now need toodefine hello set of commands we would like our Chef client tooexecute on our managed virtual machine.</span></span>

<span data-ttu-id="fac5e-172">Hello příkazy jsou uloženy v souboru default.rb hello.</span><span class="sxs-lookup"><span data-stu-id="fac5e-172">hello commands are stored in hello file default.rb.</span></span> <span data-ttu-id="fac5e-173">V tomto souboru I budete definování sadu příkazů, který nainstaluje službu IIS, spustí službu IIS a zkopíruje složku wwwroot toohello souboru šablony.</span><span class="sxs-lookup"><span data-stu-id="fac5e-173">In this file, I’ll be defining a set of commands that installs IIS, starts IIS and copies a template file toohello wwwroot folder.</span></span>

<span data-ttu-id="fac5e-174">Upravte soubor C:\chef\cookbooks\webserver\recipes\default.rb hello a přidejte následující řádky hello.</span><span class="sxs-lookup"><span data-stu-id="fac5e-174">Modify hello C:\chef\cookbooks\webserver\recipes\default.rb file and add hello following lines.</span></span>

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

<span data-ttu-id="fac5e-175">Až budete hotoví, uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="fac5e-175">Save hello file once you are done.</span></span>

## <a name="creating-a-template"></a><span data-ttu-id="fac5e-176">Vytvoření šablony</span><span class="sxs-lookup"><span data-stu-id="fac5e-176">Creating a template</span></span>
<span data-ttu-id="fac5e-177">Jak jsme už bylo zmíněno dříve, potřebujeme toogenerate soubor šablony, který se použije jako naší stránce s default.html.</span><span class="sxs-lookup"><span data-stu-id="fac5e-177">As we mentioned previously, we need toogenerate a template file which will be used as our default.html page.</span></span>

<span data-ttu-id="fac5e-178">Spusťte následující příkaz toogenerate hello šablony hello.</span><span class="sxs-lookup"><span data-stu-id="fac5e-178">Run hello following command toogenerate hello template.</span></span>

    chef generate template webserver Default.htm

<span data-ttu-id="fac5e-179">Nyní přejděte toohello C:\chef\cookbooks\webserver\templates\default\Default.htm.erb souboru.</span><span class="sxs-lookup"><span data-stu-id="fac5e-179">Now navigate toohello C:\chef\cookbooks\webserver\templates\default\Default.htm.erb file.</span></span> <span data-ttu-id="fac5e-180">Upravte soubor hello přidáním jednoduchý "Hello, World" HTML kód a potom uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="fac5e-180">Edit hello file by adding some simple “Hello World” HTML code, and then save hello file.</span></span>

## <a name="upload-hello-cookbook-toohello-chef-server"></a><span data-ttu-id="fac5e-181">Nahrát hello kuchařka toohello Chef serveru</span><span class="sxs-lookup"><span data-stu-id="fac5e-181">Upload hello Cookbook toohello Chef Server</span></span>
<span data-ttu-id="fac5e-182">V tomto kroku jsme jsou pořízení kopie hello Cookbook, který jsme vytvořili na našem místního počítače a pak ho nahrát toohello Chef Hosted serveru.</span><span class="sxs-lookup"><span data-stu-id="fac5e-182">In this step, we are taking a copy of hello Cookbook that we have created on our local machine and uploading it toohello Chef Hosted Server.</span></span> <span data-ttu-id="fac5e-183">Po nahrání hello kuchařka se zobrazí v části hello **zásad** kartě.</span><span class="sxs-lookup"><span data-stu-id="fac5e-183">Once uploaded, hello Cookbook will appear under hello **Policy** tab.</span></span>

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a><span data-ttu-id="fac5e-184">Nasazení virtuálního počítače s nůž Azure</span><span class="sxs-lookup"><span data-stu-id="fac5e-184">Deploy a virtual machine with Knife Azure</span></span>
<span data-ttu-id="fac5e-185">Nyní změníme nasadit virtuální počítač Azure a použít hello kuchařka "Webový server", který nainstaluje naše IIS webové služby a výchozí webové stránky.</span><span class="sxs-lookup"><span data-stu-id="fac5e-185">We will now deploy an Azure virtual machine and apply hello “Webserver” Cookbook which will install our IIS web service and default web page.</span></span>

<span data-ttu-id="fac5e-186">V pořadí toodo to, použijte hello **server nůž azure vytvořit** příkaz.</span><span class="sxs-lookup"><span data-stu-id="fac5e-186">In order toodo this, use hello **knife azure server create** command.</span></span>

<span data-ttu-id="fac5e-187">Se, že příklad hello příkazu se zobrazí další.</span><span class="sxs-lookup"><span data-stu-id="fac5e-187">Am example of hello command appears next.</span></span>

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

<span data-ttu-id="fac5e-188">Parametry Hello je zřejmých.</span><span class="sxs-lookup"><span data-stu-id="fac5e-188">hello parameters are self-explanatory.</span></span> <span data-ttu-id="fac5e-189">Nahraďte konkrétní proměnných a spustit.</span><span class="sxs-lookup"><span data-stu-id="fac5e-189">Substitute your particular variables and run.</span></span>

> [!NOTE]
> <span data-ttu-id="fac5e-190">Prostřednictvím hello hello příkazového řádku I se také automatizaci Moje pravidla filtru koncového bodu sítě pomocí parametru koncové body – tcp hello.</span><span class="sxs-lookup"><span data-stu-id="fac5e-190">Through hello hello command line, I’m also automating my endpoint network filter rules by using hello –tcp-endpoints parameter.</span></span> <span data-ttu-id="fac5e-191">Jste otevřené porty 80 a 3389 tooprovide přístup toomy webové stránky a relaci protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="fac5e-191">I’ve opened up ports 80 and 3389 tooprovide access toomy web page and RDP session.</span></span>
> 
> 

<span data-ttu-id="fac5e-192">Po spuštění příkazu hello přejděte toohello Azure portal a můžete se zobrazí váš počítač začít tooprovision.</span><span class="sxs-lookup"><span data-stu-id="fac5e-192">Once you run hello command, go toohello Azure portal and you will see your machine begin tooprovision.</span></span>

![][13]

<span data-ttu-id="fac5e-193">Hello příkazového řádku se zobrazí na další.</span><span class="sxs-lookup"><span data-stu-id="fac5e-193">hello command prompt appears next.</span></span>

![][10]

<span data-ttu-id="fac5e-194">Po dokončení nasazení hello jsme musí být schopný tooconnect toohello webové služby přes port 80 při jsme zřízení hello virtuální počítač s hello nůž Azure příkaz jsme měl otevřít hello port.</span><span class="sxs-lookup"><span data-stu-id="fac5e-194">Once hello deployment is complete, we should be able tooconnect toohello web service over port 80 as we had opened hello port when we provisioned hello virtual machine with hello Knife Azure command.</span></span> <span data-ttu-id="fac5e-195">Tento virtuální počítač je hello pouze virtuální počítač v mé cloudové služby, budete ho připojení s adresou url služby cloud hello.</span><span class="sxs-lookup"><span data-stu-id="fac5e-195">As this virtual machine is hello only virtual machine in my cloud service, I’ll connect it with hello cloud service url.</span></span>

![][11]

<span data-ttu-id="fac5e-196">Jak vidíte, se zobrazí chybové tvůrčí s vlastního kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="fac5e-196">As you can see, I got creative with my HTML code.</span></span>

<span data-ttu-id="fac5e-197">Nezapomeňte, že jsme také můžete připojit přes relaci protokolu RDP z portálu Azure přes port 3389 hello.</span><span class="sxs-lookup"><span data-stu-id="fac5e-197">Don’t forget we can also connect through an RDP session from hello Azure portal via port 3389.</span></span>

<span data-ttu-id="fac5e-198">Ať, že tento byl užitečné!</span><span class="sxs-lookup"><span data-stu-id="fac5e-198">I hope this has been helpful!</span></span> <span data-ttu-id="fac5e-199">Přejděte a spusťte infrastruktury jako cesty kódu s Azure ještě dnes!</span><span class="sxs-lookup"><span data-stu-id="fac5e-199">Go  and start your infrastructure as code journey with Azure today!</span></span>

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
