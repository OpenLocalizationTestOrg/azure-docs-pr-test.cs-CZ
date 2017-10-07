---
title: "aaaDeploy tooLinux aplikace Node.js virtuálních počítačů v Azure"
description: "Zjistěte, jak toodeploy Node.js aplikace tooLinux virtuálních počítačů v Azure."
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
ms.openlocfilehash: e876c8170a06f102f7f32ec7cb930b876647a707
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-nodejs-application-toolinux-virtual-machines-in-azure"></a><span data-ttu-id="69169-103">Nasazení tooLinux aplikace Node.js virtuálních počítačů v Azure</span><span class="sxs-lookup"><span data-stu-id="69169-103">Deploy a Node.js application tooLinux Virtual Machines in Azure</span></span>
<span data-ttu-id="69169-104">Tento kurz ukazuje, jak tootake aplikace Node.js a nasadit ji tooLinux virtuální počítače běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="69169-104">This tutorial shows how tootake a Node.js application and deploy it tooLinux virtual machines running in Azure.</span></span> <span data-ttu-id="69169-105">Hello pokyny v tomto kurzu platí pro všechny operační systémy, který je schopen spuštění Node.js.</span><span class="sxs-lookup"><span data-stu-id="69169-105">hello instructions in this tutorial can be followed on any operating system that is capable of running Node.js.</span></span>

<span data-ttu-id="69169-106">Dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="69169-106">You'll learn how to:</span></span>

* <span data-ttu-id="69169-107">Rozvětvení a klonování úložiště GitHub obsahující jednoduchou aplikaci TODO;</span><span class="sxs-lookup"><span data-stu-id="69169-107">Fork and clone a GitHub repository containing a simple TODO application;</span></span>
* <span data-ttu-id="69169-108">Vytvořit a nakonfigurovat dvě virtuální počítače s Linuxem v Azure toorun hello aplikace;</span><span class="sxs-lookup"><span data-stu-id="69169-108">Create and configure two Linux virtual machines in Azure toorun hello application;</span></span>
* <span data-ttu-id="69169-109">Iterate hello aplikace nuceným doručením aktualizace toohello webového front-endu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="69169-109">Iterate on hello application by pushing updates toohello web frontend virtual machine.</span></span>

> [!NOTE]
> <span data-ttu-id="69169-110">toocomplete tohoto kurzu potřebujete účet GitHub a účet Microsoft Azure a hello možnost toouse Git z vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="69169-110">toocomplete this tutorial, you need a GitHub account and a Microsoft Azure account, and hello ability toouse Git from a development machine.</span></span>
> 
> <span data-ttu-id="69169-111">Pokud nemáte účet Githubu, můžete si zaregistrovat [zde](https://github.com/join).</span><span class="sxs-lookup"><span data-stu-id="69169-111">If you don't have a GitHub account, you can sign up [here](https://github.com/join).</span></span>
> 
> <span data-ttu-id="69169-112">Pokud nemáte [Microsoft Azure](https://azure.microsoft.com/) účet, můžete si zaregistrovat bezplatnou zkušební verzi [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="69169-112">If you don't have a [Microsoft Azure](https://azure.microsoft.com/) account, you can sign up for a FREE trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="69169-113">To také vás provede hello registrační proces [Account Microsoft](http://account.microsoft.com) Pokud jste již nemají jeden.</span><span class="sxs-lookup"><span data-stu-id="69169-113">This will also lead you through hello sign up process for a [Microsoft Account](http://account.microsoft.com) if you do not already have one.</span></span> <span data-ttu-id="69169-114">Případně, pokud jste předplatitele sady Visual Studio, můžete [aktivovat výhody webu MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="69169-114">Alternatively, if you are a Visual Studio subscriber, you can [activate your MSDN benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>
> 
> <span data-ttu-id="69169-115">Pokud nemáte git na vývojovém počítači, pokud používáte počítač Macintosh nebo Windows, nainstalujte git z [zde](http://www.git-scm.com).</span><span class="sxs-lookup"><span data-stu-id="69169-115">If you do not have git on your development machine, then if you are using a Macintosh or Windows machine, install git from [here](http://www.git-scm.com).</span></span> <span data-ttu-id="69169-116">Pokud používáte Linux, nainstalovat git pomocí hello mechanismus nejlépe vyhovuje, například `sudo apt-get install git`.</span><span class="sxs-lookup"><span data-stu-id="69169-116">If you are using Linux, install git using hello mechanism most appropriate for you, such as `sudo apt-get install git`.</span></span>
> 
> 

## <a name="forking-and-cloning-hello-todo-application"></a><span data-ttu-id="69169-117">Větvení a klonování hello aplikace TODO</span><span class="sxs-lookup"><span data-stu-id="69169-117">Forking and Cloning hello TODO Application</span></span>
<span data-ttu-id="69169-118">Hello aplikace TODO používané v tomto kurzu implementuje jednoduchý webový front-end přes instance MongoDB, které uchovává informace o seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="69169-118">hello TODO application used by this tutorial implements a simple web frontend over a MongoDB instance that keeps track of a TODO list.</span></span> <span data-ttu-id="69169-119">Po přihlášení tooGitHub, přejděte [sem](https://github.com/stepro/node-todo) toofind hello aplikace a rozvětvit pomocí hello odkaz v pravé horní hello.</span><span class="sxs-lookup"><span data-stu-id="69169-119">After signing in tooGitHub, go [here](https://github.com/stepro/node-todo) toofind hello application and fork it using hello link in hello top right.</span></span> <span data-ttu-id="69169-120">To by měl vytvořit úložiště ve vašem účtu s názvem *accountname*/node-todo.</span><span class="sxs-lookup"><span data-stu-id="69169-120">This should create a repository in your account named *accountname*/node-todo.</span></span>

<span data-ttu-id="69169-121">Nyní na vývojovém počítači, klonování toto úložiště:</span><span class="sxs-lookup"><span data-stu-id="69169-121">Now on your development machine, clone this repository:</span></span>

    git clone https://github.com/accountname/node-todo.git

<span data-ttu-id="69169-122">Použijeme Tenhle klon. místní úložiště hello trochu později při provádění změn toohello zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="69169-122">We'll use this local clone of hello repository a little later when making changes toohello source code.</span></span>

## <a name="creating-and-configuring-hello-linux-virtual-machines"></a><span data-ttu-id="69169-123">Vytváření a konfiguraci hello virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="69169-123">Creating and Configuring hello Linux Virtual Machines</span></span>
<span data-ttu-id="69169-124">Azure má podpory pro nezpracovanou výpočet pomocí virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="69169-124">Azure has great support for raw compute using Linux virtual machines.</span></span> <span data-ttu-id="69169-125">Tato část hello kurz ukazuje, jak můžete snadno číselníku až dva virtuální počítače s Linuxem a nasadit toothem aplikace hello TODO, spuštěné hello webového front-endu na jednu a hello instance MongoDB na hello jiné.</span><span class="sxs-lookup"><span data-stu-id="69169-125">This part of hello tutorial shows how you can easily spin up two Linux virtual machines and deploy hello TODO application toothem, running hello web frontend on one and hello MongoDB instance on hello other.</span></span>

### <a name="creating-virtual-machines"></a><span data-ttu-id="69169-126">Vytváření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="69169-126">Creating Virtual Machines</span></span>
<span data-ttu-id="69169-127">Nejjednodušší způsob, jak toocreate Hello nového virtuálního počítače v Azure je toouse hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="69169-127">hello easiest way toocreate a new virtual machine in Azure is toouse hello Azure Portal.</span></span> <span data-ttu-id="69169-128">Klikněte na tlačítko [sem](https://portal.azure.com) toosign v a spouštět hello portálu Azure ve vašem webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="69169-128">Click [here](https://portal.azure.com) toosign in and launch hello Azure Portal in your web browser.</span></span> <span data-ttu-id="69169-129">Jakmile se načetl hello portálu Azure, dokončete hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="69169-129">Once hello Azure Portal has loaded, complete hello following steps:</span></span>

* <span data-ttu-id="69169-130">Klikněte na tlačítko hello "+ nové" odkaz;</span><span class="sxs-lookup"><span data-stu-id="69169-130">Click hello "+ New" link;</span></span>
* <span data-ttu-id="69169-131">Vyberte kategorie "Výpočetní" hello a zvolte "Ubuntu Server 14.04 LTS";</span><span class="sxs-lookup"><span data-stu-id="69169-131">Pick hello "Compute" category and choose "Ubuntu Server 14.04 LTS";</span></span>
* <span data-ttu-id="69169-132">Vyberte model nasazení hello "Resource Manager" a klikněte na tlačítko "Vytvořit";</span><span class="sxs-lookup"><span data-stu-id="69169-132">Select hello "Resource Manager" deployment model and click "Create";</span></span>
* <span data-ttu-id="69169-133">Vyplňte hello základy následujících pokynů:</span><span class="sxs-lookup"><span data-stu-id="69169-133">Fill in hello basics following these guidelines:</span></span>
  * <span data-ttu-id="69169-134">Zadejte název, který vám později mohli snadno identifikovat;</span><span class="sxs-lookup"><span data-stu-id="69169-134">Specify a name you can easily identify later;</span></span>
  * <span data-ttu-id="69169-135">V tomto kurzu zvolte ověřování hesla;</span><span class="sxs-lookup"><span data-stu-id="69169-135">For this tutorial, choose Password authentication;</span></span>
  * <span data-ttu-id="69169-136">Vytvořte novou skupinu prostředků s názvem Osobní.</span><span class="sxs-lookup"><span data-stu-id="69169-136">Create a new resource group with an identifiable name.</span></span>
* <span data-ttu-id="69169-137">Pro hello velikost virtuálního počítače "A1 standardní" je vhodná volba pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="69169-137">For hello Virtual Machine size, "A1 Standard" is a reasonable choice for this tutorial.</span></span>
* <span data-ttu-id="69169-138">Další nastavení Ujistěte se, že typ disku hello "Standard" a přijmout že všechny hello zbývající výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="69169-138">For additional settings, ensure hello disk type is "Standard" and accept all hello remaining defaults.</span></span>
* <span data-ttu-id="69169-139">Ji hello vytvoření na souhrnné stránce hello.</span><span class="sxs-lookup"><span data-stu-id="69169-139">Kick off hello creation on hello summary page.</span></span>

<span data-ttu-id="69169-140">Provedení hello výše zpracovat dvakrát toocreate dva Linuxové virtuální počítače, jeden pro front-end webové hello a jeden pro hello MongoDB instance.</span><span class="sxs-lookup"><span data-stu-id="69169-140">Perform hello above process twice toocreate two Linux virtual machines, one for hello web frontend and one for hello MongoDB instance.</span></span> <span data-ttu-id="69169-141">Vytváření virtuálních počítačů hello bude trvat přibližně 5 až 10 minut.</span><span class="sxs-lookup"><span data-stu-id="69169-141">Creation of hello virtual machines will take about 5-10 minutes.</span></span>

### <a name="assigning-a-dns-entry-for-virtual-machines"></a><span data-ttu-id="69169-142">Přiřazení položky DNS pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="69169-142">Assigning a DNS entry for Virtual Machines</span></span>
<span data-ttu-id="69169-143">Virtuální počítače vytvořené v Azure jsou ve výchozím nastavení je dostupné jenom přes veřejnou IP adresu jako 1.2.3.4.</span><span class="sxs-lookup"><span data-stu-id="69169-143">Virtual machines created in Azure are by default only accessible through a public IP address like 1.2.3.4.</span></span> <span data-ttu-id="69169-144">Přiřazením záznamy DNS vytvoříme snadněji osobní počítače hello.</span><span class="sxs-lookup"><span data-stu-id="69169-144">Let's make hello machines more easily identifiable by assigning them DNS entries.</span></span>

<span data-ttu-id="69169-145">Jakmile hello portál označuje, zda byly vytvořeny hello virtuální počítače, klikněte na odkaz "Virtuální počítače" hello v levé navigační panel hello a vyhledejte vašich počítačů.</span><span class="sxs-lookup"><span data-stu-id="69169-145">Once hello portal indicates that hello virtual machines have been created, click on hello "Virtual machines" link in hello left navbar and locate your machines.</span></span> <span data-ttu-id="69169-146">Pro každý počítač:</span><span class="sxs-lookup"><span data-stu-id="69169-146">For each machine:</span></span>

* <span data-ttu-id="69169-147">Vyhledejte hello Essentials kartě a klikněte na hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="69169-147">Locate hello Essentials tab and click on hello Public IP Address;</span></span>
* <span data-ttu-id="69169-148">V hello veřejné konfiguraci IP adresy přiřaďte Popisek názvu DNS a uložit.</span><span class="sxs-lookup"><span data-stu-id="69169-148">In hello public IP address configuration, assign a DNS name label and save.</span></span>

<span data-ttu-id="69169-149">Hello portálu se ujistěte se, že je k dispozici tento hello název, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="69169-149">hello portal will ensure that hello name you specify is available.</span></span> <span data-ttu-id="69169-150">Po uložení konfigurace hello, virtuální počítače bude mít názvy hostitelů, které jsou podobné příliš`machinename.region.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="69169-150">After saving hello configuration, your virtual machines will have host names similar too`machinename.region.cloudapp.azure.com`.</span></span>

### <a name="connecting-toohello-virtual-machines"></a><span data-ttu-id="69169-151">Připojení toohello virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="69169-151">Connecting toohello Virtual Machines</span></span>
<span data-ttu-id="69169-152">Virtuální počítače byly zajištěny, byly předem nakonfigurovaná tooallow vzdálená připojení přes protokol SSH.</span><span class="sxs-lookup"><span data-stu-id="69169-152">When your virtual machines were provisioned, they were pre-configured tooallow remote connections over SSH.</span></span> <span data-ttu-id="69169-153">Toto je mechanismus hello použijeme tooconfigure hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="69169-153">This is hello mechanism we will use tooconfigure hello virtual machines.</span></span> <span data-ttu-id="69169-154">Pokud používáte systém Windows pro váš vývojový, tooget klientem SSH je nutné, pokud jste již nemají jeden.</span><span class="sxs-lookup"><span data-stu-id="69169-154">If you are using Windows for your development, you will need tooget an SSH client if you do not already have one.</span></span> <span data-ttu-id="69169-155">Běžné volba je PuTTY, který si můžete stáhnout z [zde](http://www.chiark.greenend.org.uk/~sgtatham/putty/).</span><span class="sxs-lookup"><span data-stu-id="69169-155">A common choice here is PuTTY, which can be downloaded from [here](http://www.chiark.greenend.org.uk/~sgtatham/putty/).</span></span> <span data-ttu-id="69169-156">Macintosh a operační systémy Linux obsahují verzi předinstalovaným SSH.</span><span class="sxs-lookup"><span data-stu-id="69169-156">Macintosh and Linux OSes come with a version of SSH pre-installed.</span></span>

### <a name="configuring-hello-web-frontend-virtual-machine"></a><span data-ttu-id="69169-157">Konfigurace hello virtuálním počítačem webového front-endu</span><span class="sxs-lookup"><span data-stu-id="69169-157">Configuring hello Web Frontend Virtual Machine</span></span>
<span data-ttu-id="69169-158">SSH toohello webové front-endu počítače, který jste vytvořili pomocí klienta PuTTY, ssh příkazového řádku nebo vaše jiný nástroj Oblíbené SSH.</span><span class="sxs-lookup"><span data-stu-id="69169-158">SSH toohello web frontend machine you created using PuTTY, ssh command line or your other favorite SSH tool.</span></span> <span data-ttu-id="69169-159">Měli byste vidět uvítací zprávy, za nímž následuje příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="69169-159">You should see a welcome message followed by a command prompt.</span></span>

<span data-ttu-id="69169-160">První Ujistěte se, zda git a uzlu jsou oba nainstalovány:</span><span class="sxs-lookup"><span data-stu-id="69169-160">First, let's make sure that git and node are both installed:</span></span>

    sudo apt-get install -y git
    curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
    sudo apt-get install -y nodejs

<span data-ttu-id="69169-161">Vzhledem k tomu, že front-endu webové aplikace hello závisí na některé nativních modulů Node.js, potřebujeme také tooinstall hello základní sadu nástrojů sestavení:</span><span class="sxs-lookup"><span data-stu-id="69169-161">Since hello application's web frontend relies on some native Node.js modules, we also need tooinstall hello essential set of build tools:</span></span>

    sudo apt-get install -y build-essential

<span data-ttu-id="69169-162">Nakonec nainstalujme aplikaci Node.js s názvem *navždy*, což pomáhá toorun Node.js serverové aplikace:</span><span class="sxs-lookup"><span data-stu-id="69169-162">Finally, let's install a Node.js application called *forever*, which helps toorun Node.js server applications:</span></span>

    sudo npm install -g forever

<span data-ttu-id="69169-163">Toto jsou všechny závislosti hello na tento virtuální počítač toobe možné toorun hello aplikace webového front-endu, takže Pojďme se systémem.</span><span class="sxs-lookup"><span data-stu-id="69169-163">These are all hello dependencies needed on this virtual machine toobe able toorun hello application's web frontend, so let's get that running.</span></span> <span data-ttu-id="69169-164">toodo, nejdřív vytvoříme úplné klon úložiště GitHub hello dříve forked, aby mohli snadno publikovat aktualizace toohello virtuálního počítače (jsme zaměříme tento scénář aktualizace později) a poté klonovat hello úplné klon tooprovide verzi hello úložiště, ve skutečnosti může být spuštěn.</span><span class="sxs-lookup"><span data-stu-id="69169-164">toodo this, we will first create a bare clone of hello GitHub repository you previously forked so that you can easily publish updates toohello virtual machine (we'll cover this update scenario later), and then clone hello bare clone tooprovide a version of hello repository that can actually be executed.</span></span>

<span data-ttu-id="69169-165">Spouštění z adresáře hello home (~), spusťte následující příkazy hello (nahrazení *accountname* pomocí názvu uživatelského účtu Githubu):</span><span class="sxs-lookup"><span data-stu-id="69169-165">Starting from hello home (~) directory, run hello following commands (replacing *accountname* with your GitHub user account name):</span></span>

    git clone --bare https://github.com/accountname/node-todo.git
    git clone node-todo.git

<span data-ttu-id="69169-166">Nyní zadejte adresář hello uzlu úkolů a spusťte tyto příkazy:</span><span class="sxs-lookup"><span data-stu-id="69169-166">Now enter hello node-todo directory and run these commands:</span></span>

    npm install
    forever start server.js

<span data-ttu-id="69169-167">Hello front-endu webové aplikace je nyní spuštěna, ale je jeden krok před přístupem k aplikaci hello z webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="69169-167">hello application's web frontend is now running, however there is one more step before you can access hello application from a web browser.</span></span> <span data-ttu-id="69169-168">Hello vytvoříte virtuální počítač je chráněn prostředek služby Azure, názvem *skupinu zabezpečení sítě*, který byl vytvořen pro případě jste zřídili hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="69169-168">hello virtual machine you created is protected by an Azure resource called a *network security group*, which was created for you when you provisioned hello virtual machine.</span></span> <span data-ttu-id="69169-169">Tento prostředek v současné době umožňuje pouze externí požadavky tooport 22 toobe směrovány toohello virtuálních počítačů umožňující komunikaci SSH s hello počítač, ale nic jiného.</span><span class="sxs-lookup"><span data-stu-id="69169-169">Currently, this resource only allows external requests tooport 22 toobe routed toohello virtual machine, which enables SSH communication with hello machine but nothing else.</span></span> <span data-ttu-id="69169-170">V hello tooview pořadí úkolů aplikace, která je nakonfigurovaná toorun na portu 8080, je proto tento port taky musí toobe otevřeli.</span><span class="sxs-lookup"><span data-stu-id="69169-170">So in order tooview hello TODO application, which is configured toorun on port 8080, this port also needs toobe opened up.</span></span>

<span data-ttu-id="69169-171">Vraťte se toohello portálu Azure a dokončete hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="69169-171">Return toohello Azure Portal and complete hello following steps:</span></span>

* <span data-ttu-id="69169-172">Klikněte na "Prostředku skupiny" v levé navigační panel hello;</span><span class="sxs-lookup"><span data-stu-id="69169-172">Click on "Resource groups" in hello left navbar;</span></span>
* <span data-ttu-id="69169-173">Vyberte skupinu prostředků hello, která obsahuje váš virtuální počítač;</span><span class="sxs-lookup"><span data-stu-id="69169-173">Select hello resource group that contains your virtual machine;</span></span>
* <span data-ttu-id="69169-174">V hello výsledného seznamu prostředků vyberte skupinu zabezpečení sítě hello (hello jeden ikona štítu);</span><span class="sxs-lookup"><span data-stu-id="69169-174">In hello resulting list of resources, select hello network security group (hello one with a shield icon);</span></span>
* <span data-ttu-id="69169-175">V dialogovém okně Vlastnosti hello zvolte "pravidla zabezpečení příchozích";</span><span class="sxs-lookup"><span data-stu-id="69169-175">In hello properties, choose "Inbound security rules";</span></span>
* <span data-ttu-id="69169-176">V panelu nástrojů hello klikněte na tlačítko "Přidat";</span><span class="sxs-lookup"><span data-stu-id="69169-176">In hello toolbar, click "Add";</span></span>
* <span data-ttu-id="69169-177">Zadejte název jako "výchozí povolit todo";</span><span class="sxs-lookup"><span data-stu-id="69169-177">Provide a name like "default-allow-todo";</span></span>
* <span data-ttu-id="69169-178">Nastavení protokolu hello příliš "TCP";</span><span class="sxs-lookup"><span data-stu-id="69169-178">Set hello protocol too"TCP";</span></span>
* <span data-ttu-id="69169-179">Nastavit rozsah cílových portů hello příliš "8080";</span><span class="sxs-lookup"><span data-stu-id="69169-179">Set hello destination port range too"8080";</span></span>
* <span data-ttu-id="69169-180">Klikněte na tlačítko OK a počkejte toobe pravidlo zabezpečení hello vytvořili.</span><span class="sxs-lookup"><span data-stu-id="69169-180">Click OK and wait for hello security rule toobe created.</span></span>

<span data-ttu-id="69169-181">Po vytvoření tohoto pravidla zabezpečení, je veřejně viditelný na hello aplikace TODO hello internet a můžete procházet tooit, například pomocí adresy URL, například:</span><span class="sxs-lookup"><span data-stu-id="69169-181">After creating this security rule, hello TODO application is publically visible on hello internet and you can browse tooit, for instance using a URL such as:</span></span>

    http://machinename.region.cloudapp.azure.com:8080

<span data-ttu-id="69169-182">Si všimnete, že i když ještě nenakonfigurovali hello MongoDB virtuálního počítače, zobrazí hello aplikace TODO toobe poměrně funkční.</span><span class="sxs-lookup"><span data-stu-id="69169-182">You will notice that even though we have not yet configured hello MongoDB virtual machine, hello TODO application appears toobe quite functional.</span></span> <span data-ttu-id="69169-183">Je to proto hello zdrojové úložiště je pevně zakódované toouse předem nasazené instance MongoDB.</span><span class="sxs-lookup"><span data-stu-id="69169-183">This is because hello source repository is hardcoded toouse a pre-deployed MongoDB instance.</span></span> <span data-ttu-id="69169-184">Jakmile jsme nakonfigurovali hello MongoDB virtuálního počítače, jsme se přejděte zpět a změňte hello zdrojového kódu tooutilize naše privátní instance MongoDB místo.</span><span class="sxs-lookup"><span data-stu-id="69169-184">Once we have configured hello MongoDB virtual machine, we will go back and change hello source code tooutilize our private MongoDB instance instead.</span></span>

### <a name="configuring-hello-mongodb-virtual-machine"></a><span data-ttu-id="69169-185">Konfigurace hello MongoDB virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="69169-185">Configuring hello MongoDB Virtual Machine</span></span>
<span data-ttu-id="69169-186">SSH toohello druhý počítač, který jste vytvořili pomocí klienta PuTTY, ssh příkazového řádku nebo vaše jiný nástroj Oblíbené SSH.</span><span class="sxs-lookup"><span data-stu-id="69169-186">SSH toohello second machine you created using PuTTY, ssh command line or your other favorite SSH tool.</span></span> <span data-ttu-id="69169-187">Po zobrazení uvítací zprávy hello a příkazového řádku, nainstalujte MongoDB (tyto pokyny, které byly odebrány z [sem](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):</span><span class="sxs-lookup"><span data-stu-id="69169-187">After seeing hello welcome message and command prompt, install MongoDB (these instructions were taken from [here](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):</span></span>

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
    sudo apt-get update
    sudo apt-get install -y mongodb-org

<span data-ttu-id="69169-188">Ve výchozím nastavení je nakonfigurovaný MongoDB, takže ho lze přistupovat pouze místně.</span><span class="sxs-lookup"><span data-stu-id="69169-188">By default, MongoDB is configured so it can only be accessed locally.</span></span> <span data-ttu-id="69169-189">V tomto kurzu jsme se nakonfigurujte MongoDB tak, aby byla přístupná z aplikace hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="69169-189">For this tutorial, we will configure MongoDB so it can be accessed from hello application's virtual machine.</span></span> <span data-ttu-id="69169-190">V kontextu sudo, otevřete soubor /etc/mongod.conf hello a vyhledejte hello `# network interfaces` části.</span><span class="sxs-lookup"><span data-stu-id="69169-190">In a sudo context, open hello /etc/mongod.conf file and locate hello `# network interfaces` section.</span></span> <span data-ttu-id="69169-191">Změna hello `net.bindIp` konfigurace hodnota příliš`0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="69169-191">Change hello `net.bindIp` configuration value too`0.0.0.0`.</span></span>

> [!NOTE]
> <span data-ttu-id="69169-192">Tato konfigurace je pro účely tohoto kurzu pouze hello.</span><span class="sxs-lookup"><span data-stu-id="69169-192">This configuration is for hello purposes of this tutorial only.</span></span> <span data-ttu-id="69169-193">Je **není** doporučené postupy zabezpečení a neměl by být používat v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="69169-193">It is **NOT** a recommended security practice and should not be used in production environments.</span></span>
> 
> 

<span data-ttu-id="69169-194">Teď zajistěte hello MongoDB byla spuštěna služba:</span><span class="sxs-lookup"><span data-stu-id="69169-194">Now ensure hello MongoDB service has been started:</span></span>

    sudo service mongod restart

<span data-ttu-id="69169-195">MongoDB funguje přes port 27017 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="69169-195">MongoDB operates over port 27017 by default.</span></span> <span data-ttu-id="69169-196">Proto v hello stejným způsobem, že jsme potřeby tooopen port 8080 na virtuální počítač hello webového front-endu, potřebujeme tooopen port 27017 hello MongoDB virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="69169-196">So, in hello same way that we needed tooopen port 8080 on hello web frontend virtual machine, we need tooopen port 27017 on hello MongoDB virtual machine.</span></span>

<span data-ttu-id="69169-197">Vraťte se toohello portálu Azure a dokončete hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="69169-197">Return toohello Azure Portal and complete hello following steps:</span></span>

* <span data-ttu-id="69169-198">Klikněte na "Prostředku skupiny" v levé navigační panel hello;</span><span class="sxs-lookup"><span data-stu-id="69169-198">Click on "Resource groups" in hello left navbar;</span></span>
* <span data-ttu-id="69169-199">Vyberte skupinu prostředků hello, který obsahuje hello MongoDB virtuální počítač;</span><span class="sxs-lookup"><span data-stu-id="69169-199">Select hello resource group that contains hello MongoDB virtual machine;</span></span>
* <span data-ttu-id="69169-200">V hello výsledného seznamu prostředků vyberte skupinu zabezpečení sítě hello (hello jeden ikona štítu) s hello stejný název, který jste zadali v toohello MongoDB virtuálního počítače;</span><span class="sxs-lookup"><span data-stu-id="69169-200">In hello resulting list of resources, select hello network security group (hello one with a shield icon) with hello same name that you gave toohello MongoDB virtual machine;</span></span>
* <span data-ttu-id="69169-201">V dialogovém okně Vlastnosti hello zvolte "pravidla zabezpečení příchozích";</span><span class="sxs-lookup"><span data-stu-id="69169-201">In hello properties, choose "Inbound security rules";</span></span>
* <span data-ttu-id="69169-202">V panelu nástrojů hello klikněte na tlačítko "Přidat";</span><span class="sxs-lookup"><span data-stu-id="69169-202">In hello toolbar, click "Add";</span></span>
* <span data-ttu-id="69169-203">Zadejte název jako "výchozí povolit mongo";</span><span class="sxs-lookup"><span data-stu-id="69169-203">Provide a name like "default-allow-mongo";</span></span>
* <span data-ttu-id="69169-204">Nastavení protokolu hello příliš "TCP";</span><span class="sxs-lookup"><span data-stu-id="69169-204">Set hello protocol too"TCP";</span></span>
* <span data-ttu-id="69169-205">Nastavit rozsah cílových portů hello příliš "27017";</span><span class="sxs-lookup"><span data-stu-id="69169-205">Set hello destination port range too"27017";</span></span>
* <span data-ttu-id="69169-206">Klikněte na tlačítko OK a počkejte toobe pravidlo zabezpečení hello vytvořili.</span><span class="sxs-lookup"><span data-stu-id="69169-206">Click OK and wait for hello security rule toobe created.</span></span>

## <a name="iterating-on-hello-todo-application"></a><span data-ttu-id="69169-207">Iterace v hello aplikace TODO</span><span class="sxs-lookup"><span data-stu-id="69169-207">Iterating on hello TODO application</span></span>
<span data-ttu-id="69169-208">Zatím jsme máte zřízen dva virtuální počítače Linux: ten, který je spuštěna aplikace hello front-endové webové a jeden, je spuštěna MongoDB instance.</span><span class="sxs-lookup"><span data-stu-id="69169-208">So far, we have provisioned two Linux virtual machines: one that is running hello application's web frontend and one that is running a MongoDB instance.</span></span> <span data-ttu-id="69169-209">Ale došlo k potížím – front-endu webové hello nepoužívá ve skutečnosti instance MongoDB hello zřízený ještě.</span><span class="sxs-lookup"><span data-stu-id="69169-209">But there is a problem - hello web frontend isn't actually using hello provisioned MongoDB instance yet.</span></span> <span data-ttu-id="69169-210">Umožňuje vyřešit, aktualizací toouse kód front-endu webové hello proměnné prostředí místo je pevně instance.</span><span class="sxs-lookup"><span data-stu-id="69169-210">Let's fix that by updating hello web frontend code toouse an environment variable instead of a hard-coded instance.</span></span>

### <a name="changing-hello-todo-application"></a><span data-ttu-id="69169-211">Změna aplikace TODO hello</span><span class="sxs-lookup"><span data-stu-id="69169-211">Changing hello TODO application</span></span>
<span data-ttu-id="69169-212">Na vývojovém počítači kde nejprve klonovat úložiště hello uzlu todo, otevřete hello `node-todo/config/database.js` souboru ve svém oblíbeném editoru a změňte hodnotu adresy url hello z hello pevně hodnotu jako `mongodb://...` příliš`process.env.MONGODB`.</span><span class="sxs-lookup"><span data-stu-id="69169-212">On your development machine where you first cloned hello node-todo repository, open hello `node-todo/config/database.js` file in your favorite editor and change hello url value from hello hard-coded value like `mongodb://...` too`process.env.MONGODB`.</span></span>

<span data-ttu-id="69169-213">Potvrďte změny a odešlete hlavní toohello Githubu:</span><span class="sxs-lookup"><span data-stu-id="69169-213">Commit your changes and push toohello GitHub master:</span></span>

    git commit -am "Get MongoDB instance from env"
    git push origin master

<span data-ttu-id="69169-214">Bohužel tuto nepodporuje publikování hello změnu toohello webového front-endu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="69169-214">Unfortunately, this doesn't publish hello change toohello web frontend virtual machine.</span></span> <span data-ttu-id="69169-215">Provedeme pár další tooenable virtuální počítač toothat změny jednoduchý, ale efektivní mechanismus pro publikování aktualizací, takže můžete rychle sledovat účinek hello hello změny v prostředí za provozu hello.</span><span class="sxs-lookup"><span data-stu-id="69169-215">Let's make a few more changes toothat virtual machine tooenable a simple but effective mechanism for publishing updates so you can quickly observe hello effect of hello changes in hello live environment.</span></span>

### <a name="configuring-hello-web-frontend-virtual-machine"></a><span data-ttu-id="69169-216">Konfigurace hello virtuálním počítačem webového front-endu</span><span class="sxs-lookup"><span data-stu-id="69169-216">Configuring hello Web Frontend Virtual Machine</span></span>
<span data-ttu-id="69169-217">Odvolat jsme dříve vytvořili úplné klon hello todo uzlu úložiště na virtuální počítač hello webového front-endu.</span><span class="sxs-lookup"><span data-stu-id="69169-217">Recall that we previously created a bare clone of hello node-todo repository on hello web frontend virtual machine.</span></span> <span data-ttu-id="69169-218">Ukazuje se, že tato akce vytvořeny nové Git vzdálené toowhich změny mohou poslat.</span><span class="sxs-lookup"><span data-stu-id="69169-218">It turns out that this action created a new Git remote toowhich changes can be pushed.</span></span> <span data-ttu-id="69169-219">Ale jednoduše vkládání toothis vzdálené poměrně nedává hello rychlé iterace model, který vývojářům hledáte při práci na svůj kód.</span><span class="sxs-lookup"><span data-stu-id="69169-219">However, simply pushing toothis remote doesn't quite give hello rapid iteration model that developers are looking for when working on their code.</span></span>

<span data-ttu-id="69169-220">Co jsme chcete mít toodo toobe je zajistěte, aby při nabízené toohello vzdáleného úložiště na virtuálním počítači hello, běžící aplikaci TODO hello automaticky aktualizovala.</span><span class="sxs-lookup"><span data-stu-id="69169-220">What we would like toobe able toodo is ensure that when a push toohello remote repository on hello virtual machine occurs, hello running TODO application is automatically updated.</span></span> <span data-ttu-id="69169-221">Naštěstí jde snadno tooachieve s gitem.</span><span class="sxs-lookup"><span data-stu-id="69169-221">Fortunately, this is easy tooachieve with git.</span></span>

<span data-ttu-id="69169-222">Git udává počet háky, jež jsou volány v určitou dobu tooreact tooactions pořízené hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="69169-222">Git exposes a number of hooks that are called at particular times tooreact tooactions taken on hello repository.</span></span> <span data-ttu-id="69169-223">Tyto jsou určeny pomocí skriptů prostředí v úložišti hello `hooks` složky.</span><span class="sxs-lookup"><span data-stu-id="69169-223">These are specified using shell scripts in hello repository's `hooks` folder.</span></span> <span data-ttu-id="69169-224">Hello háku, který je pro scénář automatickou aktualizaci hello tehdy, je hello `post-update` událostí.</span><span class="sxs-lookup"><span data-stu-id="69169-224">hello hook that is most applicable for hello auto-update scenario is hello `post-update` event.</span></span>

<span data-ttu-id="69169-225">SSH relace toohello webového front-endu virtuální počítač, změňte toohello `~/node-todo.git/hooks` adresáře a přidejte následující obsahu tooa soubor s názvem hello `post-update` (nahrazení `machinename` a `region` s vaše informace o virtuálním počítači MongoDB):</span><span class="sxs-lookup"><span data-stu-id="69169-225">In a SSH session toohello web frontend virtual machine, change toohello `~/node-todo.git/hooks` directory and add hello following content tooa file named `post-update` (replacing `machinename` and `region` with your MongoDB virtual machine information):</span></span>

    #!/bin/bash

    forever stopall
    unset 'GIT_DIR'
    export MONGODB="mongodb://machinename.region.cloudapp.azure.com:27017/tododb"
    cd ~/node-todo && git fetch origin && git pull origin master && npm install && forever start ~/node-todo/server.js
    exec git update-server-info

<span data-ttu-id="69169-226">Zkontrolujte, zda že tento soubor je spustitelný soubor spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="69169-226">Ensure this file is executable by running hello following command:</span></span>

    chmod 755 post-update

<span data-ttu-id="69169-227">Tento skript zajišťuje, že aktuální aplikace server hello je zastavena, hello kód v úložišti klonovaný hello je aktualizovaný toohello nejnovější, žádné aktualizované závislostí a restartování serveru hello.</span><span class="sxs-lookup"><span data-stu-id="69169-227">This script ensures that hello current server application is stopped, hello code in hello cloned repository is updated toohello latest, any updated dependencies are satisfied, and hello server is restarted.</span></span> <span data-ttu-id="69169-228">Kromě toho zajišťuje, že byl nakonfigurován hello prostředí v rámci přípravy pro příjem naše první aktualizace tooget hello MongoDB instanci aplikace na proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="69169-228">It also ensures that hello environment has been configured in preparation for receiving our first application update tooget hello MongoDB instance from an environment variable.</span></span>

### <a name="configuring-your-development-machine"></a><span data-ttu-id="69169-229">Konfigurace počítači pro vývoj</span><span class="sxs-lookup"><span data-stu-id="69169-229">Configuring your Development Machine</span></span>
<span data-ttu-id="69169-230">Nyní Pojďme vývojovém počítači připojili toohello webového front-endu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="69169-230">Now let's get your development machine hooked up toohello web frontend virtual machine.</span></span> <span data-ttu-id="69169-231">To je jednoduché, přidání hello úplné úložiště na virtuálním počítači hello jako vzdálené.</span><span class="sxs-lookup"><span data-stu-id="69169-231">This is as simple as adding hello bare repository on hello virtual machine as a remote.</span></span> <span data-ttu-id="69169-232">Spusťte následující příkaz toodo hello to (nahrazení *uživatele* s vaše přihlašovací jméno webového front-endu virtuálního počítače a *machinename* a *oblast* podle potřeby):</span><span class="sxs-lookup"><span data-stu-id="69169-232">Run hello following command toodo this (replacing *user* with your web frontend virtual machine login name and *machinename* and *region* as appropriate):</span></span>

    git remote add azure user@machinename.region.cloudapp.azure.com:node-todo.git

<span data-ttu-id="69169-233">To je vše, který je potřebný tooenable vkládání, nebo v platnosti publikování, změny virtuálního počítače toohello webového front-endu.</span><span class="sxs-lookup"><span data-stu-id="69169-233">This is all that is needed tooenable pushing, or in effect publishing, changes toohello web frontend virtual machine.</span></span>

### <a name="publishing-updates"></a><span data-ttu-id="69169-234">Publikování aktualizací</span><span class="sxs-lookup"><span data-stu-id="69169-234">Publishing Updates</span></span>
<span data-ttu-id="69169-235">Umožňuje publikovat hello jeden změn, který byl změněn, pokud tak, aby hello aplikace bude používat vlastní instance MongoDB:</span><span class="sxs-lookup"><span data-stu-id="69169-235">Let's publish hello one change that has been made so far so that hello application will use our own MongoDB instance:</span></span>

    git push azure master

<span data-ttu-id="69169-236">Měli byste vidět výstup podobný toothis:</span><span class="sxs-lookup"><span data-stu-id="69169-236">You should see output similar toothis:</span></span>

    Counting objects: 4, done.
    Delta compression using up too4 threads.
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
    toousername@machinename.region.cloudapp.azure.com:node-todo.git
    5f31fd7..5bc7be5  master -> master

<span data-ttu-id="69169-237">Po dokončení tohoto příkazu, aktualizujte aplikace hello ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="69169-237">After this command completes, try refreshing hello application in a web browser.</span></span> <span data-ttu-id="69169-238">Musí být schopný toosee, že seznam úkolů hello okomentovat je prázdný a už nasazené vázanou toohello sdílené MongoDB instance.</span><span class="sxs-lookup"><span data-stu-id="69169-238">You should be able toosee that hello TODO list presented here is empty and no longer tied toohello shared deployed MongoDB instance.</span></span>

<span data-ttu-id="69169-239">toocomplete hello kurzu provedeme jiný, více viditelných změnu.</span><span class="sxs-lookup"><span data-stu-id="69169-239">toocomplete hello tutorial, let's make another, more visible change.</span></span> <span data-ttu-id="69169-240">Na vývojovém počítači otevřete soubor node-todo/public/index.html hello pomocí vašeho oblíbeného editoru.</span><span class="sxs-lookup"><span data-stu-id="69169-240">On your development machine, open hello node-todo/public/index.html file using your favorite editor.</span></span> <span data-ttu-id="69169-241">Vyhledejte hello jumbotron záhlaví a změňte název hello z "Jsem Todo-aholic" příliš "jsem aholic Todo v Azure!".</span><span class="sxs-lookup"><span data-stu-id="69169-241">Locate hello jumbotron header and change  hello title from "I'm a Todo-aholic" too"I'm a Todo-aholic on Azure!".</span></span>

<span data-ttu-id="69169-242">Teď umožňuje zapsat:</span><span class="sxs-lookup"><span data-stu-id="69169-242">Now let's commit:</span></span>

    git commit -am "Azurify hello title"

<span data-ttu-id="69169-243">Tento čas, budeme publikovat hello změnu tooAzure před odesláním ji úložiště GitHub toohello tooback:</span><span class="sxs-lookup"><span data-stu-id="69169-243">This time, let's publish hello change tooAzure before pushing it tooback toohello GitHub repo:</span></span>

    git push azure master

<span data-ttu-id="69169-244">Po dokončení tohoto příkazu se zobrazí webovou stránku hello aktualizace a změny hello.</span><span class="sxs-lookup"><span data-stu-id="69169-244">Once this command completes, refresh hello web page and you will see hello changes.</span></span> <span data-ttu-id="69169-245">Vzhledem k tomu, že budou vypadat dobře, push vzdálené hello změnu zpět toohello původu:</span><span class="sxs-lookup"><span data-stu-id="69169-245">Since they look good, push hello change back toohello origin remote:</span></span> 

    git push origin master

## <a name="next-steps"></a><span data-ttu-id="69169-246">Další kroky</span><span class="sxs-lookup"><span data-stu-id="69169-246">Next Steps</span></span>
<span data-ttu-id="69169-247">Tento článek vám ukázal, jak tootake aplikace Node.js a nasadit ji tooLinux virtuální počítače běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="69169-247">This article showed how tootake a Node.js application and deploy it tooLinux virtual machines running in Azure.</span></span> <span data-ttu-id="69169-248">toolearn Další informace o virtuálních počítačích s Linuxem v Azure, najdete v části [tooLinux Úvod v Azure](/documentation/articles/virtual-machines-linux-introduction/).</span><span class="sxs-lookup"><span data-stu-id="69169-248">toolearn more about Linux virtual machines in Azure, see [Introduction tooLinux on Azure](/documentation/articles/virtual-machines-linux-introduction/).</span></span>

<span data-ttu-id="69169-249">Další informace o tom, toodevelop aplikací Node.js v Azure, najdete v části hello [středisku pro vývojáře Node.js](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="69169-249">For more information about how toodevelop Node.js applications on Azure, see hello [Node.js Developer Center](/develop/nodejs/).</span></span>

