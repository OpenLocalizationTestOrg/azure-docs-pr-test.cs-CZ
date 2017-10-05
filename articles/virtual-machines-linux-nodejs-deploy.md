---
title: "Nasazení aplikace Node.js pro virtuální počítače s Linuxem v Azure"
description: "Informace o nasazení aplikace Node.js na virtuální počítače s Linuxem v Azure."
services: 
documentationcenter: nodejs
author: stepro
manager: dmitryr
editor: 
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: /azure
ms.assetid: 857a812d-c73e-4af7-a985-2d0baf8b6f71
ms.service: multiple
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2016
ms.author: stephpr
ms.openlocfilehash: d3d9fecfafb9ba422420230716b9c985481d1e24
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-nodejs-application-to-linux-virtual-machines-in-azure"></a><span data-ttu-id="80f72-103">Nasazení aplikace Node.js pro virtuální počítače s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="80f72-103">Deploy a Node.js application to Linux Virtual Machines in Azure</span></span>
<span data-ttu-id="80f72-104">Tento kurz ukazuje, jak provést aplikace Node.js a nasadíte ho do Linuxové virtuální počítače běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="80f72-104">This tutorial shows how to take a Node.js application and deploy it to Linux virtual machines running in Azure.</span></span> <span data-ttu-id="80f72-105">Pokyny v tomto kurzu platí pro všechny operační systémy, které podporují Node.js.</span><span class="sxs-lookup"><span data-stu-id="80f72-105">The instructions in this tutorial can be followed on any operating system that is capable of running Node.js.</span></span>

<span data-ttu-id="80f72-106">Dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="80f72-106">You'll learn how to:</span></span>

* <span data-ttu-id="80f72-107">Rozvětvení a klonování úložiště GitHub obsahující jednoduchou aplikaci TODO;</span><span class="sxs-lookup"><span data-stu-id="80f72-107">Fork and clone a GitHub repository containing a simple TODO application;</span></span>
* <span data-ttu-id="80f72-108">Vytvořit a nakonfigurovat dvě virtuální počítače s Linuxem v Azure a spusťte aplikaci;</span><span class="sxs-lookup"><span data-stu-id="80f72-108">Create and configure two Linux virtual machines in Azure to run the application;</span></span>
* <span data-ttu-id="80f72-109">Iterate zobrazením aktualizací na virtuální počítač front-endu webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="80f72-109">Iterate on the application by pushing updates to the web frontend virtual machine.</span></span>

> [!NOTE]
> <span data-ttu-id="80f72-110">K dokončení tohoto kurzu, budete potřebovat účet GitHub a účet Microsoft Azure a možnost používat Git z vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="80f72-110">To complete this tutorial, you need a GitHub account and a Microsoft Azure account, and the ability to use Git from a development machine.</span></span>
> 
> <span data-ttu-id="80f72-111">Pokud nemáte účet Githubu, můžete si zaregistrovat [zde](https://github.com/join).</span><span class="sxs-lookup"><span data-stu-id="80f72-111">If you don't have a GitHub account, you can sign up [here](https://github.com/join).</span></span>
> 
> <span data-ttu-id="80f72-112">Pokud nemáte [Microsoft Azure](https://azure.microsoft.com/) účet, můžete si zaregistrovat bezplatnou zkušební verzi [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="80f72-112">If you don't have a [Microsoft Azure](https://azure.microsoft.com/) account, you can sign up for a FREE trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="80f72-113">To také vás provede proces pro přihlášení [Account Microsoft](http://account.microsoft.com) Pokud jste již nemají jeden.</span><span class="sxs-lookup"><span data-stu-id="80f72-113">This will also lead you through the sign up process for a [Microsoft Account](http://account.microsoft.com) if you do not already have one.</span></span> <span data-ttu-id="80f72-114">Případně, pokud jste předplatitele sady Visual Studio, můžete [aktivovat výhody webu MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="80f72-114">Alternatively, if you are a Visual Studio subscriber, you can [activate your MSDN benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>
> 
> <span data-ttu-id="80f72-115">Pokud nemáte git na vývojovém počítači, pokud používáte počítač Macintosh nebo Windows, nainstalujte git z [zde](http://www.git-scm.com).</span><span class="sxs-lookup"><span data-stu-id="80f72-115">If you do not have git on your development machine, then if you are using a Macintosh or Windows machine, install git from [here](http://www.git-scm.com).</span></span> <span data-ttu-id="80f72-116">Pokud používáte Linux, nainstalovat git pomocí mechanismu nejlépe vyhovuje, například `sudo apt-get install git`.</span><span class="sxs-lookup"><span data-stu-id="80f72-116">If you are using Linux, install git using the mechanism most appropriate for you, such as `sudo apt-get install git`.</span></span>
> 
> 

## <a name="forking-and-cloning-the-todo-application"></a><span data-ttu-id="80f72-117">Větvení a klonování aplikace TODO</span><span class="sxs-lookup"><span data-stu-id="80f72-117">Forking and Cloning the TODO Application</span></span>
<span data-ttu-id="80f72-118">Aplikace TODO používané v tomto kurzu implementuje jednoduchý webový front-end přes instance MongoDB, které uchovává informace o seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="80f72-118">The TODO application used by this tutorial implements a simple web frontend over a MongoDB instance that keeps track of a TODO list.</span></span> <span data-ttu-id="80f72-119">Po přihlášení na Githubu, přejděte [sem](https://github.com/stepro/node-todo) můžete najít aplikaci a rozvětvit ho pomocí odkazu v pravé horní.</span><span class="sxs-lookup"><span data-stu-id="80f72-119">After signing in to GitHub, go [here](https://github.com/stepro/node-todo) to find the application and fork it using the link in the top right.</span></span> <span data-ttu-id="80f72-120">To by měl vytvořit úložiště ve vašem účtu s názvem *accountname*/node-todo.</span><span class="sxs-lookup"><span data-stu-id="80f72-120">This should create a repository in your account named *accountname*/node-todo.</span></span>

<span data-ttu-id="80f72-121">Nyní na vývojovém počítači, klonování toto úložiště:</span><span class="sxs-lookup"><span data-stu-id="80f72-121">Now on your development machine, clone this repository:</span></span>

    git clone https://github.com/accountname/node-todo.git

<span data-ttu-id="80f72-122">Použijeme Tenhle klon. místní úložiště trochu později při provádění změn ke zdrojovému kódu.</span><span class="sxs-lookup"><span data-stu-id="80f72-122">We'll use this local clone of the repository a little later when making changes to the source code.</span></span>

## <a name="creating-and-configuring-the-linux-virtual-machines"></a><span data-ttu-id="80f72-123">Vytváření a konfigurace virtuálního počítače se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="80f72-123">Creating and Configuring the Linux Virtual Machines</span></span>
<span data-ttu-id="80f72-124">Azure má podpory pro nezpracovanou výpočet pomocí virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="80f72-124">Azure has great support for raw compute using Linux virtual machines.</span></span> <span data-ttu-id="80f72-125">Tato část kurz ukazuje, jak můžete snadno číselníku až dva virtuální počítače s Linuxem a nasadit aplikace TODO, front-endu webové systémem jednu a instance MongoDB na straně druhé.</span><span class="sxs-lookup"><span data-stu-id="80f72-125">This part of the tutorial shows how you can easily spin up two Linux virtual machines and deploy the TODO application to them, running the web frontend on one and the MongoDB instance on the other.</span></span>

### <a name="creating-virtual-machines"></a><span data-ttu-id="80f72-126">Vytváření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="80f72-126">Creating Virtual Machines</span></span>
<span data-ttu-id="80f72-127">Nejjednodušší způsob, jak vytvořit nový virtuální počítač v Azure je pomocí webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="80f72-127">The easiest way to create a new virtual machine in Azure is to use the Azure Portal.</span></span> <span data-ttu-id="80f72-128">Klikněte na tlačítko [sem](https://portal.azure.com) přihlásit a spuštění portálu Azure ve vašem webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="80f72-128">Click [here](https://portal.azure.com) to sign in and launch the Azure Portal in your web browser.</span></span> <span data-ttu-id="80f72-129">Jakmile načetl portálu Azure, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="80f72-129">Once the Azure Portal has loaded, complete the following steps:</span></span>

* <span data-ttu-id="80f72-130">Klikněte na odkaz "+ nové";</span><span class="sxs-lookup"><span data-stu-id="80f72-130">Click the "+ New" link;</span></span>
* <span data-ttu-id="80f72-131">Vyberte kategorie "Výpočet" a zvolte "Ubuntu Server 14.04 LTS";</span><span class="sxs-lookup"><span data-stu-id="80f72-131">Pick the "Compute" category and choose "Ubuntu Server 14.04 LTS";</span></span>
* <span data-ttu-id="80f72-132">Vyberte model nasazení "Resource Manager" a klikněte na tlačítko "Vytvořit";</span><span class="sxs-lookup"><span data-stu-id="80f72-132">Select the "Resource Manager" deployment model and click "Create";</span></span>
* <span data-ttu-id="80f72-133">Zadejte základní informace o následujících pokynů:</span><span class="sxs-lookup"><span data-stu-id="80f72-133">Fill in the basics following these guidelines:</span></span>
  * <span data-ttu-id="80f72-134">Zadejte název, který vám později mohli snadno identifikovat;</span><span class="sxs-lookup"><span data-stu-id="80f72-134">Specify a name you can easily identify later;</span></span>
  * <span data-ttu-id="80f72-135">V tomto kurzu zvolte ověřování hesla;</span><span class="sxs-lookup"><span data-stu-id="80f72-135">For this tutorial, choose Password authentication;</span></span>
  * <span data-ttu-id="80f72-136">Vytvořte novou skupinu prostředků s názvem Osobní.</span><span class="sxs-lookup"><span data-stu-id="80f72-136">Create a new resource group with an identifiable name.</span></span>
* <span data-ttu-id="80f72-137">Pro velikost virtuálního počítače "A1 standardní" je vhodná volba pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="80f72-137">For the Virtual Machine size, "A1 Standard" is a reasonable choice for this tutorial.</span></span>
* <span data-ttu-id="80f72-138">Další nastavení Ujistěte se, že typ disku "Standard" a přijměte všechny zbývající výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="80f72-138">For additional settings, ensure the disk type is "Standard" and accept all the remaining defaults.</span></span>
* <span data-ttu-id="80f72-139">Ji vytvoření na stránce Souhrn.</span><span class="sxs-lookup"><span data-stu-id="80f72-139">Kick off the creation on the summary page.</span></span>

<span data-ttu-id="80f72-140">Proveďte výše uvedený postup dvakrát vytvořit dva virtuální počítače Linux, jeden pro front-endu webové a jeden pro MongoDB instance.</span><span class="sxs-lookup"><span data-stu-id="80f72-140">Perform the above process twice to create two Linux virtual machines, one for the web frontend and one for the MongoDB instance.</span></span> <span data-ttu-id="80f72-141">Vytváření virtuálních počítačů bude trvat přibližně 5 až 10 minut.</span><span class="sxs-lookup"><span data-stu-id="80f72-141">Creation of the virtual machines will take about 5-10 minutes.</span></span>

### <a name="assigning-a-dns-entry-for-virtual-machines"></a><span data-ttu-id="80f72-142">Přiřazení položky DNS pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="80f72-142">Assigning a DNS entry for Virtual Machines</span></span>
<span data-ttu-id="80f72-143">Virtuální počítače vytvořené v Azure jsou ve výchozím nastavení je dostupné jenom přes veřejnou IP adresu jako 1.2.3.4.</span><span class="sxs-lookup"><span data-stu-id="80f72-143">Virtual machines created in Azure are by default only accessible through a public IP address like 1.2.3.4.</span></span> <span data-ttu-id="80f72-144">Pojďme bylo na počítače snadněji identifikovat přiřazením záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="80f72-144">Let's make the machines more easily identifiable by assigning them DNS entries.</span></span>

<span data-ttu-id="80f72-145">Jakmile na portál označuje, zda byly vytvořeny virtuální počítače, klikněte na odkaz "Virtuální počítače" v levé navigační panel a vyhledejte vašich počítačů.</span><span class="sxs-lookup"><span data-stu-id="80f72-145">Once the portal indicates that the virtual machines have been created, click on the "Virtual machines" link in the left navbar and locate your machines.</span></span> <span data-ttu-id="80f72-146">Pro každý počítač:</span><span class="sxs-lookup"><span data-stu-id="80f72-146">For each machine:</span></span>

* <span data-ttu-id="80f72-147">Vyhledejte kartě základních a klikněte na veřejnou IP adresu;</span><span class="sxs-lookup"><span data-stu-id="80f72-147">Locate the Essentials tab and click on the Public IP Address;</span></span>
* <span data-ttu-id="80f72-148">Ve veřejné konfiguraci IP adresy přiřaďte Popisek názvu DNS a uložit.</span><span class="sxs-lookup"><span data-stu-id="80f72-148">In the public IP address configuration, assign a DNS name label and save.</span></span>

<span data-ttu-id="80f72-149">Na portálu zajistí, že je k dispozici název, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="80f72-149">The portal will ensure that the name you specify is available.</span></span> <span data-ttu-id="80f72-150">Po uložení konfigurace, bude mít virtuální počítače názvy hostitelů, které jsou podobné `machinename.region.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="80f72-150">After saving the configuration, your virtual machines will have host names similar to `machinename.region.cloudapp.azure.com`.</span></span>

### <a name="connecting-to-the-virtual-machines"></a><span data-ttu-id="80f72-151">Připojení k virtuálním počítačům</span><span class="sxs-lookup"><span data-stu-id="80f72-151">Connecting to the Virtual Machines</span></span>
<span data-ttu-id="80f72-152">Pokud vaše virtuální počítače byly zajištěny, byly předem nakonfigurovaný k povolení vzdálených připojení přes protokol SSH.</span><span class="sxs-lookup"><span data-stu-id="80f72-152">When your virtual machines were provisioned, they were pre-configured to allow remote connections over SSH.</span></span> <span data-ttu-id="80f72-153">Toto je mechanismus, který budeme používat ke konfiguraci virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="80f72-153">This is the mechanism we will use to configure the virtual machines.</span></span> <span data-ttu-id="80f72-154">Pokud používáte systém Windows pro váš vývojový, musíte získat klientem SSH, pokud jste již nemají jeden.</span><span class="sxs-lookup"><span data-stu-id="80f72-154">If you are using Windows for your development, you will need to get an SSH client if you do not already have one.</span></span> <span data-ttu-id="80f72-155">Běžné volba je PuTTY, který si můžete stáhnout z [zde](http://www.chiark.greenend.org.uk/~sgtatham/putty/).</span><span class="sxs-lookup"><span data-stu-id="80f72-155">A common choice here is PuTTY, which can be downloaded from [here](http://www.chiark.greenend.org.uk/~sgtatham/putty/).</span></span> <span data-ttu-id="80f72-156">Macintosh a operační systémy Linux obsahují verzi předinstalovaným SSH.</span><span class="sxs-lookup"><span data-stu-id="80f72-156">Macintosh and Linux OSes come with a version of SSH pre-installed.</span></span>

### <a name="configuring-the-web-frontend-virtual-machine"></a><span data-ttu-id="80f72-157">Konfigurace virtuálního počítače webového front-endu</span><span class="sxs-lookup"><span data-stu-id="80f72-157">Configuring the Web Frontend Virtual Machine</span></span>
<span data-ttu-id="80f72-158">SSH k počítači front-endu webové, kterou jste vytvořili pomocí PuTTY, ssh příkazového řádku nebo vaše jiný nástroj Oblíbené SSH.</span><span class="sxs-lookup"><span data-stu-id="80f72-158">SSH to the web frontend machine you created using PuTTY, ssh command line or your other favorite SSH tool.</span></span> <span data-ttu-id="80f72-159">Měli byste vidět uvítací zprávy, za nímž následuje příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="80f72-159">You should see a welcome message followed by a command prompt.</span></span>

<span data-ttu-id="80f72-160">První Ujistěte se, zda git a uzlu jsou oba nainstalovány:</span><span class="sxs-lookup"><span data-stu-id="80f72-160">First, let's make sure that git and node are both installed:</span></span>

    sudo apt-get install -y git
    curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
    sudo apt-get install -y nodejs

<span data-ttu-id="80f72-161">Vzhledem k tomu, že front-endu webové aplikace závisí na některé nativních modulů Node.js, jsme také muset nainstalovat základní sadu nástrojů sestavení:</span><span class="sxs-lookup"><span data-stu-id="80f72-161">Since the application's web frontend relies on some native Node.js modules, we also need to install the essential set of build tools:</span></span>

    sudo apt-get install -y build-essential

<span data-ttu-id="80f72-162">Nakonec nainstalujme aplikaci Node.js s názvem *navždy*, což přispívá ke spouštění aplikací Node.js serveru:</span><span class="sxs-lookup"><span data-stu-id="80f72-162">Finally, let's install a Node.js application called *forever*, which helps to run Node.js server applications:</span></span>

    sudo npm install -g forever

<span data-ttu-id="80f72-163">Toto jsou všechny závislosti nutné na tomto virtuálním počítači, abyste mohli spustit front-endu webové aplikace, takže Pojďme se systémem.</span><span class="sxs-lookup"><span data-stu-id="80f72-163">These are all the dependencies needed on this virtual machine to be able to run the application's web frontend, so let's get that running.</span></span> <span data-ttu-id="80f72-164">Uděláte to tak, nejdřív vytvoříme úplné klon úložiště GitHub dříve forked, aby mohli snadno publikovat aktualizace virtuálního počítače (jsme zaměříme tento scénář aktualizace později) a poté klonovat úplné klon zadejte verzi úložiště, ve skutečnosti mohou být provedeny.</span><span class="sxs-lookup"><span data-stu-id="80f72-164">To do this, we will first create a bare clone of the GitHub repository you previously forked so that you can easily publish updates to the virtual machine (we'll cover this update scenario later), and then clone the bare clone to provide a version of the repository that can actually be executed.</span></span>

<span data-ttu-id="80f72-165">Spouštění z adresáře home (~), spusťte následující příkazy (nahrazení *accountname* pomocí názvu uživatelského účtu Githubu):</span><span class="sxs-lookup"><span data-stu-id="80f72-165">Starting from the home (~) directory, run the following commands (replacing *accountname* with your GitHub user account name):</span></span>

    git clone --bare https://github.com/accountname/node-todo.git
    git clone node-todo.git

<span data-ttu-id="80f72-166">Nyní zadejte adresář, do uzlu úkolů a spusťte tyto příkazy:</span><span class="sxs-lookup"><span data-stu-id="80f72-166">Now enter the node-todo directory and run these commands:</span></span>

    npm install
    forever start server.js

<span data-ttu-id="80f72-167">Front-endu webové aplikace je nyní spuštěna, ale je jeden krok před přístupem k aplikaci z webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="80f72-167">The application's web frontend is now running, however there is one more step before you can access the application from a web browser.</span></span> <span data-ttu-id="80f72-168">Virtuální počítač, který jste vytvořili je chráněn prostředek služby Azure, názvem *skupinu zabezpečení sítě*, který byl vytvořen pro vás při zřizování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="80f72-168">The virtual machine you created is protected by an Azure resource called a *network security group*, which was created for you when you provisioned the virtual machine.</span></span> <span data-ttu-id="80f72-169">Tento prostředek v současné době umožňuje pouze externí požadavky na port 22 k přesměrování na virtuální počítač, který umožňuje komunikaci SSH s počítači, ale nic jiného.</span><span class="sxs-lookup"><span data-stu-id="80f72-169">Currently, this resource only allows external requests to port 22 to be routed to the virtual machine, which enables SSH communication with the machine but nothing else.</span></span> <span data-ttu-id="80f72-170">Chcete-li zobrazit aplikaci úkolů, která je nakonfigurovaná pro spuštění na portu 8080, tento port taky musí být otevřen.</span><span class="sxs-lookup"><span data-stu-id="80f72-170">So in order to view the TODO application, which is configured to run on port 8080, this port also needs to be opened up.</span></span>

<span data-ttu-id="80f72-171">Vraťte se k portálu Azure a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="80f72-171">Return to the Azure Portal and complete the following steps:</span></span>

* <span data-ttu-id="80f72-172">Klikněte na "Prostředku skupiny" v levé navigační panel;</span><span class="sxs-lookup"><span data-stu-id="80f72-172">Click on "Resource groups" in the left navbar;</span></span>
* <span data-ttu-id="80f72-173">Vyberte skupinu prostředků, která obsahuje váš virtuální počítač;</span><span class="sxs-lookup"><span data-stu-id="80f72-173">Select the resource group that contains your virtual machine;</span></span>
* <span data-ttu-id="80f72-174">Výsledný seznam prostředků vyberte skupinu zabezpečení sítě (jeden ikona štítu);</span><span class="sxs-lookup"><span data-stu-id="80f72-174">In the resulting list of resources, select the network security group (the one with a shield icon);</span></span>
* <span data-ttu-id="80f72-175">Ve vlastnostech zvolte "pravidla zabezpečení příchozích";</span><span class="sxs-lookup"><span data-stu-id="80f72-175">In the properties, choose "Inbound security rules";</span></span>
* <span data-ttu-id="80f72-176">Na panelu nástrojů klikněte na tlačítko "Přidat";</span><span class="sxs-lookup"><span data-stu-id="80f72-176">In the toolbar, click "Add";</span></span>
* <span data-ttu-id="80f72-177">Zadejte název jako "výchozí povolit todo";</span><span class="sxs-lookup"><span data-stu-id="80f72-177">Provide a name like "default-allow-todo";</span></span>
* <span data-ttu-id="80f72-178">Nastaví protokol na "TCP";</span><span class="sxs-lookup"><span data-stu-id="80f72-178">Set the protocol to "TCP";</span></span>
* <span data-ttu-id="80f72-179">Nastavit rozsah cílových portů na "8080";</span><span class="sxs-lookup"><span data-stu-id="80f72-179">Set the destination port range to "8080";</span></span>
* <span data-ttu-id="80f72-180">Klikněte na tlačítko OK a počkejte, který se má vytvořit pravidlo zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="80f72-180">Click OK and wait for the security rule to be created.</span></span>

<span data-ttu-id="80f72-181">Po vytvoření tohoto pravidla zabezpečení, je aplikace TODO veřejně viditelný v síti internet a můžete procházet, například pomocí adresy URL, například:</span><span class="sxs-lookup"><span data-stu-id="80f72-181">After creating this security rule, the TODO application is publically visible on the internet and you can browse to it, for instance using a URL such as:</span></span>

    http://machinename.region.cloudapp.azure.com:8080

<span data-ttu-id="80f72-182">Si všimnete, že i když ještě nenakonfigurovali MongoDB virtuálního počítače, aplikace TODO se zdá být poměrně funkční.</span><span class="sxs-lookup"><span data-stu-id="80f72-182">You will notice that even though we have not yet configured the MongoDB virtual machine, the TODO application appears to be quite functional.</span></span> <span data-ttu-id="80f72-183">Je to proto zdrojové úložiště je pevně zakódované použití předem nasazené instance MongoDB.</span><span class="sxs-lookup"><span data-stu-id="80f72-183">This is because the source repository is hardcoded to use a pre-deployed MongoDB instance.</span></span> <span data-ttu-id="80f72-184">Jakmile jsme nakonfigurovali MongoDB virtuální počítač, bude jsme přejděte zpět a změňte zdrojový kód místo využívat naše privátní instance MongoDB.</span><span class="sxs-lookup"><span data-stu-id="80f72-184">Once we have configured the MongoDB virtual machine, we will go back and change the source code to utilize our private MongoDB instance instead.</span></span>

### <a name="configuring-the-mongodb-virtual-machine"></a><span data-ttu-id="80f72-185">Konfigurace virtuálního počítače MongoDB</span><span class="sxs-lookup"><span data-stu-id="80f72-185">Configuring the MongoDB Virtual Machine</span></span>
<span data-ttu-id="80f72-186">SSH do druhé počítače, který jste vytvořili pomocí klienta PuTTY, ssh příkazového řádku nebo vaše jiný nástroj Oblíbené SSH.</span><span class="sxs-lookup"><span data-stu-id="80f72-186">SSH to the second machine you created using PuTTY, ssh command line or your other favorite SSH tool.</span></span> <span data-ttu-id="80f72-187">Po zobrazení uvítací zprávy a příkazového řádku, nainstalujte MongoDB (tyto pokyny, které byly odebrány z [sem](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):</span><span class="sxs-lookup"><span data-stu-id="80f72-187">After seeing the welcome message and command prompt, install MongoDB (these instructions were taken from [here](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):</span></span>

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
    sudo apt-get update
    sudo apt-get install -y mongodb-org

<span data-ttu-id="80f72-188">Ve výchozím nastavení je nakonfigurovaný MongoDB, takže ho lze přistupovat pouze místně.</span><span class="sxs-lookup"><span data-stu-id="80f72-188">By default, MongoDB is configured so it can only be accessed locally.</span></span> <span data-ttu-id="80f72-189">V tomto kurzu jsme se nakonfigurujte MongoDB tak, aby byla přístupná z aplikace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="80f72-189">For this tutorial, we will configure MongoDB so it can be accessed from the application's virtual machine.</span></span> <span data-ttu-id="80f72-190">V kontextu sudo, otevřete soubor /etc/mongod.conf a vyhledejte `# network interfaces` části.</span><span class="sxs-lookup"><span data-stu-id="80f72-190">In a sudo context, open the /etc/mongod.conf file and locate the `# network interfaces` section.</span></span> <span data-ttu-id="80f72-191">Změna `net.bindIp` hodnota konfigurace k `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="80f72-191">Change the `net.bindIp` configuration value to `0.0.0.0`.</span></span>

> [!NOTE]
> <span data-ttu-id="80f72-192">Tato konfigurace je pro účely tohoto kurzu jenom.</span><span class="sxs-lookup"><span data-stu-id="80f72-192">This configuration is for the purposes of this tutorial only.</span></span> <span data-ttu-id="80f72-193">Je **není** doporučené postupy zabezpečení a neměl by být používat v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="80f72-193">It is **NOT** a recommended security practice and should not be used in production environments.</span></span>
> 
> 

<span data-ttu-id="80f72-194">Teď zajistěte MongoDB byla spuštěna služba:</span><span class="sxs-lookup"><span data-stu-id="80f72-194">Now ensure the MongoDB service has been started:</span></span>

    sudo service mongod restart

<span data-ttu-id="80f72-195">MongoDB funguje přes port 27017 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="80f72-195">MongoDB operates over port 27017 by default.</span></span> <span data-ttu-id="80f72-196">Ano v stejným způsobem, který jsme potřebný pro otevření portu 8080 na virtuální počítač webového front-endu, musíme otevřete port 27017 na virtuálním počítači MongoDB.</span><span class="sxs-lookup"><span data-stu-id="80f72-196">So, in the same way that we needed to open port 8080 on the web frontend virtual machine, we need to open port 27017 on the MongoDB virtual machine.</span></span>

<span data-ttu-id="80f72-197">Vraťte se k portálu Azure a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="80f72-197">Return to the Azure Portal and complete the following steps:</span></span>

* <span data-ttu-id="80f72-198">Klikněte na "Prostředku skupiny" v levé navigační panel;</span><span class="sxs-lookup"><span data-stu-id="80f72-198">Click on "Resource groups" in the left navbar;</span></span>
* <span data-ttu-id="80f72-199">Vyberte skupinu prostředků, která obsahuje virtuální počítač MongoDB;</span><span class="sxs-lookup"><span data-stu-id="80f72-199">Select the resource group that contains the MongoDB virtual machine;</span></span>
* <span data-ttu-id="80f72-200">Výsledný seznam prostředků vyberte skupinu zabezpečení sítě (jeden ikona štítu) se stejným názvem, který jste zadali k virtuálnímu počítači MongoDB;</span><span class="sxs-lookup"><span data-stu-id="80f72-200">In the resulting list of resources, select the network security group (the one with a shield icon) with the same name that you gave to the MongoDB virtual machine;</span></span>
* <span data-ttu-id="80f72-201">Ve vlastnostech zvolte "pravidla zabezpečení příchozích";</span><span class="sxs-lookup"><span data-stu-id="80f72-201">In the properties, choose "Inbound security rules";</span></span>
* <span data-ttu-id="80f72-202">Na panelu nástrojů klikněte na tlačítko "Přidat";</span><span class="sxs-lookup"><span data-stu-id="80f72-202">In the toolbar, click "Add";</span></span>
* <span data-ttu-id="80f72-203">Zadejte název jako "výchozí povolit mongo";</span><span class="sxs-lookup"><span data-stu-id="80f72-203">Provide a name like "default-allow-mongo";</span></span>
* <span data-ttu-id="80f72-204">Nastaví protokol na "TCP";</span><span class="sxs-lookup"><span data-stu-id="80f72-204">Set the protocol to "TCP";</span></span>
* <span data-ttu-id="80f72-205">Nastavit rozsah cílových portů na "27017";</span><span class="sxs-lookup"><span data-stu-id="80f72-205">Set the destination port range to "27017";</span></span>
* <span data-ttu-id="80f72-206">Klikněte na tlačítko OK a počkejte, který se má vytvořit pravidlo zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="80f72-206">Click OK and wait for the security rule to be created.</span></span>

## <a name="iterating-on-the-todo-application"></a><span data-ttu-id="80f72-207">Iterace v aplikace TODO</span><span class="sxs-lookup"><span data-stu-id="80f72-207">Iterating on the TODO application</span></span>
<span data-ttu-id="80f72-208">Zatím jsme máte zřízen dva virtuální počítače Linux: ten, který je spuštěna aplikace front-endové webové a jeden, je spuštěna MongoDB instance.</span><span class="sxs-lookup"><span data-stu-id="80f72-208">So far, we have provisioned two Linux virtual machines: one that is running the application's web frontend and one that is running a MongoDB instance.</span></span> <span data-ttu-id="80f72-209">Ale došlo k potížím – front-endu webové nepoužívá ve skutečnosti zřízené instance MongoDB ještě.</span><span class="sxs-lookup"><span data-stu-id="80f72-209">But there is a problem - the web frontend isn't actually using the provisioned MongoDB instance yet.</span></span> <span data-ttu-id="80f72-210">Umožňuje to opravíme aktualizací kód front-endu webové místo je pevně instance používala proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="80f72-210">Let's fix that by updating the web frontend code to use an environment variable instead of a hard-coded instance.</span></span>

### <a name="changing-the-todo-application"></a><span data-ttu-id="80f72-211">Změna aplikace TODO</span><span class="sxs-lookup"><span data-stu-id="80f72-211">Changing the TODO application</span></span>
<span data-ttu-id="80f72-212">Na vývojovém počítači kde nejprve klonovat úložiště uzlu todo, otevřete `node-todo/config/database.js` souboru ve svém oblíbeném editoru a změňte adresu url hodnotu z hodnoty pevně jako `mongodb://...` k `process.env.MONGODB`.</span><span class="sxs-lookup"><span data-stu-id="80f72-212">On your development machine where you first cloned the node-todo repository, open the `node-todo/config/database.js` file in your favorite editor and change the url value from the hard-coded value like `mongodb://...` to `process.env.MONGODB`.</span></span>

<span data-ttu-id="80f72-213">Potvrdit změny a push na Githubu hlavní server:</span><span class="sxs-lookup"><span data-stu-id="80f72-213">Commit your changes and push to the GitHub master:</span></span>

    git commit -am "Get MongoDB instance from env"
    git push origin master

<span data-ttu-id="80f72-214">Bohužel tuto nepodporuje publikování změnu pro virtuální počítač webového front-endu.</span><span class="sxs-lookup"><span data-stu-id="80f72-214">Unfortunately, this doesn't publish the change to the web frontend virtual machine.</span></span> <span data-ttu-id="80f72-215">Umožňuje provést několik více změn k tomuto virtuálnímu počítači povolit jednoduchý, ale efektivní mechanismus pro publikování aktualizací, takže můžete rychle sledovat změny v prostředí za provozu.</span><span class="sxs-lookup"><span data-stu-id="80f72-215">Let's make a few more changes to that virtual machine to enable a simple but effective mechanism for publishing updates so you can quickly observe the effect of the changes in the live environment.</span></span>

### <a name="configuring-the-web-frontend-virtual-machine"></a><span data-ttu-id="80f72-216">Konfigurace virtuálního počítače webového front-endu</span><span class="sxs-lookup"><span data-stu-id="80f72-216">Configuring the Web Frontend Virtual Machine</span></span>
<span data-ttu-id="80f72-217">Odvolat jsme dříve vytvořili úplné klon todo uzlu úložiště na virtuální počítač webového front-endu.</span><span class="sxs-lookup"><span data-stu-id="80f72-217">Recall that we previously created a bare clone of the node-todo repository on the web frontend virtual machine.</span></span> <span data-ttu-id="80f72-218">Ukazuje se, že tato akce vytvořeny nové vzdálené úložiště Git do které se můžou poslat změny.</span><span class="sxs-lookup"><span data-stu-id="80f72-218">It turns out that this action created a new Git remote to which changes can be pushed.</span></span> <span data-ttu-id="80f72-219">Ale vkládání jednoduše na tento vzdálený poměrně nedává rychlé iteraci modelu, který vývojářům hledáte při práci na svůj kód.</span><span class="sxs-lookup"><span data-stu-id="80f72-219">However, simply pushing to this remote doesn't quite give the rapid iteration model that developers are looking for when working on their code.</span></span>

<span data-ttu-id="80f72-220">Co bychom rádi budete moct dělat je, zajistěte, aby při nabízení do vzdáleného úložiště na virtuálním počítači, běžící aplikaci TODO automaticky aktualizovala.</span><span class="sxs-lookup"><span data-stu-id="80f72-220">What we would like to be able to do is ensure that when a push to the remote repository on the virtual machine occurs, the running TODO application is automatically updated.</span></span> <span data-ttu-id="80f72-221">Naštěstí jde snadno dosáhnout pomocí git.</span><span class="sxs-lookup"><span data-stu-id="80f72-221">Fortunately, this is easy to achieve with git.</span></span>

<span data-ttu-id="80f72-222">Git udává počet háky, jež jsou volány v určitou dobu reagování na akce prováděné na úložiště.</span><span class="sxs-lookup"><span data-stu-id="80f72-222">Git exposes a number of hooks that are called at particular times to react to actions taken on the repository.</span></span> <span data-ttu-id="80f72-223">Tyto jsou určeny pomocí skriptů prostředí do úložiště `hooks` složky.</span><span class="sxs-lookup"><span data-stu-id="80f72-223">These are specified using shell scripts in the repository's `hooks` folder.</span></span> <span data-ttu-id="80f72-224">Hák, který je pro tento scénář automatickou aktualizaci tehdy, je `post-update` událostí.</span><span class="sxs-lookup"><span data-stu-id="80f72-224">The hook that is most applicable for the auto-update scenario is the `post-update` event.</span></span>

<span data-ttu-id="80f72-225">V relaci SSH pro virtuální počítač webového front-endu změnit na `~/node-todo.git/hooks` adresáře a přidejte následující obsah do souboru s názvem `post-update` (nahrazení `machinename` a `region` s vaše informace o virtuálním počítači MongoDB):</span><span class="sxs-lookup"><span data-stu-id="80f72-225">In a SSH session to the web frontend virtual machine, change to the `~/node-todo.git/hooks` directory and add the following content to a file named `post-update` (replacing `machinename` and `region` with your MongoDB virtual machine information):</span></span>

    #!/bin/bash

    forever stopall
    unset 'GIT_DIR'
    export MONGODB="mongodb://machinename.region.cloudapp.azure.com:27017/tododb"
    cd ~/node-todo && git fetch origin && git pull origin master && npm install && forever start ~/node-todo/server.js
    exec git update-server-info

<span data-ttu-id="80f72-226">Zkontrolujte, zda že tento soubor je spustitelný soubor tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="80f72-226">Ensure this file is executable by running the following command:</span></span>

    chmod 755 post-update

<span data-ttu-id="80f72-227">Tento skript zajistí, že aktuální server aplikace je zastavena, kód v úložišti klonovaný je aktualizace na nejnovější verzi, žádné aktualizované závislostí a restartování serveru.</span><span class="sxs-lookup"><span data-stu-id="80f72-227">This script ensures that the current server application is stopped, the code in the cloned repository is updated to the latest, any updated dependencies are satisfied, and the server is restarted.</span></span> <span data-ttu-id="80f72-228">Také zajistí, že prostředí nakonfigurovaný během přípravy pro příjem naše první aktualizaci aplikace pro získání MongoDB instance z proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="80f72-228">It also ensures that the environment has been configured in preparation for receiving our first application update to get the MongoDB instance from an environment variable.</span></span>

### <a name="configuring-your-development-machine"></a><span data-ttu-id="80f72-229">Konfigurace počítači pro vývoj</span><span class="sxs-lookup"><span data-stu-id="80f72-229">Configuring your Development Machine</span></span>
<span data-ttu-id="80f72-230">Nyní Pojďme počítači pro vývoj připojených k virtuální počítač webového front-endu.</span><span class="sxs-lookup"><span data-stu-id="80f72-230">Now let's get your development machine hooked up to the web frontend virtual machine.</span></span> <span data-ttu-id="80f72-231">To je jednoduché, přidání úplné úložiště na virtuálním počítači jako Vzdálená.</span><span class="sxs-lookup"><span data-stu-id="80f72-231">This is as simple as adding the bare repository on the virtual machine as a remote.</span></span> <span data-ttu-id="80f72-232">Spusťte následující příkaz k tomu (nahrazení *uživatele* s vaše přihlašovací jméno webového front-endu virtuálního počítače a *machinename* a *oblast* podle potřeby):</span><span class="sxs-lookup"><span data-stu-id="80f72-232">Run the following command to do this (replacing *user* with your web frontend virtual machine login name and *machinename* and *region* as appropriate):</span></span>

    git remote add azure user@machinename.region.cloudapp.azure.com:node-todo.git

<span data-ttu-id="80f72-233">To je všechno, která je potřebná k povolení nabízení nebo publikování platit, změny virtuálního počítače webového front-endu.</span><span class="sxs-lookup"><span data-stu-id="80f72-233">This is all that is needed to enable pushing, or in effect publishing, changes to the web frontend virtual machine.</span></span>

### <a name="publishing-updates"></a><span data-ttu-id="80f72-234">Publikování aktualizací</span><span class="sxs-lookup"><span data-stu-id="80f72-234">Publishing Updates</span></span>
<span data-ttu-id="80f72-235">Umožňuje publikovat jeden změn, který byl změněn, pokud tak, aby aplikace bude používat vlastní instance MongoDB:</span><span class="sxs-lookup"><span data-stu-id="80f72-235">Let's publish the one change that has been made so far so that the application will use our own MongoDB instance:</span></span>

    git push azure master

<span data-ttu-id="80f72-236">Měli byste vidět výstup podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="80f72-236">You should see output similar to this:</span></span>

    Counting objects: 4, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (4/4), 406 bytes | 0 bytes/s, done.
    Total 4 (delta 1), reused 0 (delta 0)
    remote: info:    Forever stopped processes:
    remote: data:        uid  command         script    forever pid  id logfile                         uptime
    remote: data:    [0] 0Lyh /usr/bin/nodejs server.js 9064    9301    /home/username/.forever/0Lyh.log 0:0:3:17.487
    remote: From /home/username/node-todo
    remote:    5f31fd7..5bc7be5  master     -> origin/master
    remote: From /home/username/node-todo
    remote:  * branch            master     -> FETCH_HEAD
    remote: Updating 5f31fd7..5bc7be5
    remote: Fast-forward
    remote:  config/database.js | 2 +-
    remote:  1 file changed, 1 insertion(+), 1 deletion(-)
    remote: npm WARN package.json node-todo@0.0.0 No repository field.
    remote: npm WARN package.json node-todo@0.0.0 No license field.
    remote: warn:    --minUptime not set. Defaulting to: 1000ms
    remote: warn:    --spinSleepTime not set. Your script will exit if it does not stay up for at least 1000ms
    remote: info:    Forever processing file: /home/username/node-todo/server.js
    To username@machinename.region.cloudapp.azure.com:node-todo.git
    5f31fd7..5bc7be5  master -> master

<span data-ttu-id="80f72-237">Po dokončení tohoto příkazu, pokuste se aktualizovat aplikaci ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="80f72-237">After this command completes, try refreshing the application in a web browser.</span></span> <span data-ttu-id="80f72-238">Nyní byste měli mít zjistíte, že seznam úkolů, které jsou uvedeny v tomto tématu je prázdné a už vázanou na sdílené nasazené instance MongoDB.</span><span class="sxs-lookup"><span data-stu-id="80f72-238">You should be able to see that the TODO list presented here is empty and no longer tied to the shared deployed MongoDB instance.</span></span>

<span data-ttu-id="80f72-239">K dokončení tohoto kurzu, můžeme provést jiný, více viditelných změnu.</span><span class="sxs-lookup"><span data-stu-id="80f72-239">To complete the tutorial, let's make another, more visible change.</span></span> <span data-ttu-id="80f72-240">Na vývojovém počítači otevřete soubor node-todo/public/index.html pomocí vašeho oblíbeného editoru.</span><span class="sxs-lookup"><span data-stu-id="80f72-240">On your development machine, open the node-todo/public/index.html file using your favorite editor.</span></span> <span data-ttu-id="80f72-241">Vyhledejte hlavičku jumbotron a změňte název z "Jsem Todo-aholic" na "Jsem aholic Todo v Azure!".</span><span class="sxs-lookup"><span data-stu-id="80f72-241">Locate the jumbotron header and change  the title from "I'm a Todo-aholic" to "I'm a Todo-aholic on Azure!".</span></span>

<span data-ttu-id="80f72-242">Teď umožňuje zapsat:</span><span class="sxs-lookup"><span data-stu-id="80f72-242">Now let's commit:</span></span>

    git commit -am "Azurify the title"

<span data-ttu-id="80f72-243">Tento čas, budeme publikovat změny do Azure před odesláním ho do úložiště GitHub:</span><span class="sxs-lookup"><span data-stu-id="80f72-243">This time, let's publish the change to Azure before pushing it to back to the GitHub repo:</span></span>

    git push azure master

<span data-ttu-id="80f72-244">Po dokončení tohoto příkazu, aktualizujte webové stránky a zobrazí se změny.</span><span class="sxs-lookup"><span data-stu-id="80f72-244">Once this command completes, refresh the web page and you will see the changes.</span></span> <span data-ttu-id="80f72-245">Vzhledem k tomu, že budou vypadat dobře, push změnu zpět k počátku vzdálené:</span><span class="sxs-lookup"><span data-stu-id="80f72-245">Since they look good, push the change back to the origin remote:</span></span> 

    git push origin master

## <a name="next-steps"></a><span data-ttu-id="80f72-246">Další kroky</span><span class="sxs-lookup"><span data-stu-id="80f72-246">Next Steps</span></span>
<span data-ttu-id="80f72-247">Tento článek vám ukázal, jak provést aplikace Node.js a nasadíte ho do Linuxové virtuální počítače běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="80f72-247">This article showed how to take a Node.js application and deploy it to Linux virtual machines running in Azure.</span></span> <span data-ttu-id="80f72-248">Další informace o virtuálních počítačích s Linuxem v Azure najdete v tématu [Úvod do systému Linux v Azure](/documentation/articles/virtual-machines-linux-introduction/).</span><span class="sxs-lookup"><span data-stu-id="80f72-248">To learn more about Linux virtual machines in Azure, see [Introduction to Linux on Azure](/documentation/articles/virtual-machines-linux-introduction/).</span></span>

<span data-ttu-id="80f72-249">Podrobnější informace týkající se postupu při vývoji aplikací Node.js v Azure naleznete ve [Středisku pro vývojáře Node.js](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="80f72-249">For more information about how to develop Node.js applications on Azure, see the [Node.js Developer Center](/develop/nodejs/).</span></span>

