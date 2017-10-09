---
title: "aaaSet až Apache Tomcat na virtuální počítač s Linuxem | Microsoft Docs"
description: "Zjistěte, jak tooset až Apache Tomcat7 pomocí virtuální počítače Azure s Linuxem."
services: virtual-machines-linux
documentationcenter: 
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 45ecc89c-1cb0-4e80-8944-bd0d0bbedfdc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: ningk
ms.openlocfilehash: b837a73e91fcb25d5459d993a0e93ceef1a1fc8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-tomcat7-on-a-linux-virtual-machine-with-azure"></a><span data-ttu-id="cae5f-103">Nastavit Tomcat7 na virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="cae5f-103">Set up Tomcat7 on a Linux virtual machine with Azure</span></span>
<span data-ttu-id="cae5f-104">Apache Tomcat (nebo jednoduše Tomcat, také dříve se označovaly jako Jakarta Tomcat) je webový server s otevřeným zdrojem a kontejner servlet vyvinuté hello Apache Software Foundation (amp).</span><span class="sxs-lookup"><span data-stu-id="cae5f-104">Apache Tomcat (or simply Tomcat, also formerly called Jakarta Tomcat) is an open source web server and servlet container developed by hello Apache Software Foundation (ASF).</span></span> <span data-ttu-id="cae5f-105">Tomcat implementuje hello Java Servlet a specifikace hello JavaServer stránky (JSP) z Sun Microsystems.</span><span class="sxs-lookup"><span data-stu-id="cae5f-105">Tomcat implements hello Java Servlet and hello JavaServer Pages (JSP) specifications from Sun Microsystems.</span></span> <span data-ttu-id="cae5f-106">Tomcat poskytuje čistý Java HTTP prostředí webového serveru v které toorun kódu v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="cae5f-106">Tomcat provides a pure Java HTTP web server environment in which toorun Java code.</span></span> <span data-ttu-id="cae5f-107">V nejjednodušší konfiguraci hello Tomcat běží v procesu jednoho operačního systému.</span><span class="sxs-lookup"><span data-stu-id="cae5f-107">In hello simplest configuration, Tomcat runs in a single operating system process.</span></span> <span data-ttu-id="cae5f-108">Tento proces se spustí nástroje Java virtual machine (JVM).</span><span class="sxs-lookup"><span data-stu-id="cae5f-108">This process runs a Java virtual machine (JVM).</span></span> <span data-ttu-id="cae5f-109">Každý požadavek HTTP z prohlížeče tooTomcat zpracovávány jako samostatné vláken v procesu Tomcat hello.</span><span class="sxs-lookup"><span data-stu-id="cae5f-109">Every HTTP request from a browser tooTomcat is processed as a separate thread in hello Tomcat process.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="cae5f-110">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="cae5f-110">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="cae5f-111">Tento článek popisuje, jak toouse hello modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="cae5f-111">This article covers how toouse hello classic deployment model.</span></span> <span data-ttu-id="cae5f-112">Doporučujeme vám, že většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="cae5f-112">We recommend that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="cae5f-113">toouse toodeploy správce prostředků šablony virtuálního počítače s Ubuntu s otevřete JDK a Tomcat, najdete v části [v tomto článku](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).</span><span class="sxs-lookup"><span data-stu-id="cae5f-113">toouse a Resource Manager template toodeploy an Ubuntu VM with Open JDK and Tomcat, see [this article](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).</span></span>

<span data-ttu-id="cae5f-114">V tomto článku bude instalace Tomcat7 na bitovou kopii systému Linux a nasadit v Azure.</span><span class="sxs-lookup"><span data-stu-id="cae5f-114">In this article, you will install Tomcat7 on a Linux image and deploy it in Azure.</span></span>  

<span data-ttu-id="cae5f-115">Co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="cae5f-115">You will learn:</span></span>  

* <span data-ttu-id="cae5f-116">Jak toocreate virtuálního počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="cae5f-116">How toocreate a virtual machine in Azure.</span></span>
* <span data-ttu-id="cae5f-117">Jak tooprepare hello virtuálního počítače pro Tomcat7.</span><span class="sxs-lookup"><span data-stu-id="cae5f-117">How tooprepare hello virtual machine for Tomcat7.</span></span>
* <span data-ttu-id="cae5f-118">Jak tooinstall Tomcat7.</span><span class="sxs-lookup"><span data-stu-id="cae5f-118">How tooinstall Tomcat7.</span></span>

<span data-ttu-id="cae5f-119">Předpokládá se, že už máte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="cae5f-119">It is assumed that you already have an Azure subscription.</span></span>  <span data-ttu-id="cae5f-120">Pokud není, můžete si zaregistrovat bezplatnou zkušební verzi na [hello webu Azure](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="cae5f-120">If not, you can sign up for a free trial at [hello Azure website](https://azure.microsoft.com/).</span></span> <span data-ttu-id="cae5f-121">Pokud máte předplatné MSDN, najdete v části [Microsoft Azure speciální ceny: MSDN, MPN a výhody BizSpark](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39).</span><span class="sxs-lookup"><span data-stu-id="cae5f-121">If you have an MSDN subscription, see [Microsoft Azure Special Pricing: MSDN, MPN, and BizSpark Benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39).</span></span> <span data-ttu-id="cae5f-122">toolearn Další informace o Azure, najdete v části [co je Azure?](https://azure.microsoft.com/overview/what-is-azure/).</span><span class="sxs-lookup"><span data-stu-id="cae5f-122">toolearn more about Azure, see [What is Azure?](https://azure.microsoft.com/overview/what-is-azure/).</span></span>

<span data-ttu-id="cae5f-123">Tento článek předpokládá, že máte základní znalosti práce Tomcat a Linux.</span><span class="sxs-lookup"><span data-stu-id="cae5f-123">This article assumes that you have a basic working knowledge of Tomcat and Linux.</span></span>  

## <a name="phase-1-create-an-image"></a><span data-ttu-id="cae5f-124">Fáze 1: Vytvoření image</span><span class="sxs-lookup"><span data-stu-id="cae5f-124">Phase 1: Create an image</span></span>
<span data-ttu-id="cae5f-125">V této fázi vytvoříte virtuální počítač pomocí bitové kopie systému Linux v Azure.</span><span class="sxs-lookup"><span data-stu-id="cae5f-125">In this phase, you will create a virtual machine by using a Linux image in Azure.</span></span>  

### <a name="step-1-generate-an-ssh-authentication-key"></a><span data-ttu-id="cae5f-126">Krok 1: Generovat ověřovací klíč SSH</span><span class="sxs-lookup"><span data-stu-id="cae5f-126">Step 1: Generate an SSH authentication key</span></span>
<span data-ttu-id="cae5f-127">SSH je důležité nástroj pro správce systému.</span><span class="sxs-lookup"><span data-stu-id="cae5f-127">SSH is an important tool for system administrators.</span></span> <span data-ttu-id="cae5f-128">Však není doporučeno konfigurace zabezpečení přístupu na základě určit lidské hesla.</span><span class="sxs-lookup"><span data-stu-id="cae5f-128">However, configuring access security based on a human-determined password is not recommended.</span></span> <span data-ttu-id="cae5f-129">Uživatelé se zlými úmysly může rozdělit na váš systém podle uživatelského jména a vytváření silných hesel.</span><span class="sxs-lookup"><span data-stu-id="cae5f-129">Malicious users can break into your system based on a username and a weak password.</span></span>

<span data-ttu-id="cae5f-130">Dobrá zpráva Hello je způsob tooleave vzdáleného přístupu otevřete a nestarat se o hesla.</span><span class="sxs-lookup"><span data-stu-id="cae5f-130">hello good news is that there is a way tooleave remote access open and not worry about passwords.</span></span> <span data-ttu-id="cae5f-131">Tato metoda se skládá z ověřování s asymetrické šifrování.</span><span class="sxs-lookup"><span data-stu-id="cae5f-131">This method consists of authentication with asymmetric cryptography.</span></span> <span data-ttu-id="cae5f-132">Hello soukromého klíče uživatele je hello ten, který uděluje hello ověřování.</span><span class="sxs-lookup"><span data-stu-id="cae5f-132">hello user’s private key is hello one that grants hello authentication.</span></span> <span data-ttu-id="cae5f-133">Můžete dokonce uzamknout hello uživatelský účet toonot povolit ověřování hesla.</span><span class="sxs-lookup"><span data-stu-id="cae5f-133">You can even lock hello user’s account toonot allow password authentication.</span></span>

<span data-ttu-id="cae5f-134">Další výhodou této metody je nepotřebujete toosign různá hesla v toodifferent servery.</span><span class="sxs-lookup"><span data-stu-id="cae5f-134">Another advantage of this method is that you do not need different passwords toosign in toodifferent servers.</span></span> <span data-ttu-id="cae5f-135">Můžete ověřovat pomocí hello osobní privátní klíče ve všech serverech, což zabraňuje s tooremember několik hesel.</span><span class="sxs-lookup"><span data-stu-id="cae5f-135">You can authenticate by using hello personal private key on all servers, which prevents you from having tooremember several passwords.</span></span>



<span data-ttu-id="cae5f-136">Postupujte podle těchto kroků toogenerate hello SSH ověřovací klíč.</span><span class="sxs-lookup"><span data-stu-id="cae5f-136">Follow these steps toogenerate hello SSH authentication key.</span></span>

1. <span data-ttu-id="cae5f-137">Stáhněte a nainstalujte PuTTYgen z hello následující umístění: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span><span class="sxs-lookup"><span data-stu-id="cae5f-137">Download and install PuTTYgen from hello following location: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span></span>
2. <span data-ttu-id="cae5f-138">Spusťte Puttygen.exe.</span><span class="sxs-lookup"><span data-stu-id="cae5f-138">Run Puttygen.exe.</span></span>
3. <span data-ttu-id="cae5f-139">Klikněte na tlačítko **generování** toogenerate hello klíče.</span><span class="sxs-lookup"><span data-stu-id="cae5f-139">Click **Generate** toogenerate hello keys.</span></span> <span data-ttu-id="cae5f-140">V procesu hello můžete zvýšit náhodnost tím přesunutí myši hello přes hello prázdné místo v okně hello.</span><span class="sxs-lookup"><span data-stu-id="cae5f-140">In hello process, you can increase randomness by moving hello mouse over hello blank area in hello window.</span></span>  
   <span data-ttu-id="cae5f-141">![PuTTY snímek obrazovky generátor klíč, který ukazuje hello nového klíče tlačítko Generovat][1]</span><span class="sxs-lookup"><span data-stu-id="cae5f-141">![PuTTY Key Generator screenshot that shows hello generate new key button][1]</span></span>
4. <span data-ttu-id="cae5f-142">Po hello generovat proces, Puttygen.exe se zobrazí nový veřejný klíč.</span><span class="sxs-lookup"><span data-stu-id="cae5f-142">After hello generate process, Puttygen.exe will show your new public key.</span></span>  
   ![PuTTY snímek obrazovky generátor klíč, který ukazuje hello nový veřejný klíč a hello uložení privátního klíče tlačítko][2]
5. <span data-ttu-id="cae5f-144">Vyberte a zkopírujte hello veřejný klíč a uložit ho do souboru s názvem publicKey.pem.</span><span class="sxs-lookup"><span data-stu-id="cae5f-144">Select and copy hello public key, and save it in a file named publicKey.pem.</span></span> <span data-ttu-id="cae5f-145">Nemáte klikněte na tlačítko **uložit veřejný klíč**, protože se liší od hello veřejný klíč chceme hello uloženého formátu souboru veřejného klíče.</span><span class="sxs-lookup"><span data-stu-id="cae5f-145">Don’t click **Save public key**, because hello saved public key’s file format is different from hello public key we want.</span></span>
6. <span data-ttu-id="cae5f-146">Klikněte na tlačítko **uložit privátní klíč**a uložte ho do souboru s názvem privateKey.ppk.</span><span class="sxs-lookup"><span data-stu-id="cae5f-146">Click **Save private key**, and save it in a file named privateKey.ppk.</span></span>

### <a name="step-2-create-hello-image-in-hello-azure-portal"></a><span data-ttu-id="cae5f-147">Krok 2: Vytvoření bitové kopie hello v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cae5f-147">Step 2: Create hello image in hello Azure portal</span></span>
1. <span data-ttu-id="cae5f-148">V hello [portál](https://portal.azure.com/), klikněte na tlačítko **nový** v hello úlohy panelu toocreate bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="cae5f-148">In hello [portal](https://portal.azure.com/), click **New** in hello task bar toocreate an image.</span></span> <span data-ttu-id="cae5f-149">Potom vyberte image hello Linux, která je založena na vašich potřebách.</span><span class="sxs-lookup"><span data-stu-id="cae5f-149">Then choose hello Linux image that is based on your needs.</span></span> <span data-ttu-id="cae5f-150">Hello následující příklad používá image Ubuntu 14.04 hello.</span><span class="sxs-lookup"><span data-stu-id="cae5f-150">hello following example uses hello Ubuntu 14.04 image.</span></span>
<span data-ttu-id="cae5f-151">![Snímek obrazovky hello portál, který se zobrazí tlačítko Nový hello][3]</span><span class="sxs-lookup"><span data-stu-id="cae5f-151">![Screenshot of hello portal that shows hello New button][3]</span></span>

2. <span data-ttu-id="cae5f-152">Pro **název hostitele**, zadejte název hello hello adresu URL, které a internetové klienty použijete tooaccess tohoto virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cae5f-152">For **Host Name**, specify hello name for hello URL that you and Internet clients will use tooaccess this virtual machine.</span></span> <span data-ttu-id="cae5f-153">Definujte hello poslední část hello název DNS, například tomcatdemo.</span><span class="sxs-lookup"><span data-stu-id="cae5f-153">Define hello last part of hello DNS name, for example, tomcatdemo.</span></span> <span data-ttu-id="cae5f-154">Azure pak vygeneruje adresu URL hello jako tomcatdemo.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="cae5f-154">Azure will then generate hello URL as tomcatdemo.cloudapp.net.</span></span>  

3. <span data-ttu-id="cae5f-155">Pro **SSH ověřovací klíč**, zkopírujte hodnotu klíče hello z hello publicKey.pem souboru, který obsahuje veřejný klíč hello generované PuTTYgen.</span><span class="sxs-lookup"><span data-stu-id="cae5f-155">For **SSH Authentication Key**, copy hello key value from hello publicKey.pem file, which contains hello public key generated by PuTTYgen.</span></span>  
<span data-ttu-id="cae5f-156">![Ověřovací klíč SSH pole hello portálu][4]</span><span class="sxs-lookup"><span data-stu-id="cae5f-156">![SSH Authentication Key box in hello portal][4]</span></span>

4. <span data-ttu-id="cae5f-157">Podle potřeby nakonfigurujte další nastavení a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cae5f-157">Configure other settings as needed, and then click **Create**.</span></span>  

## <a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a><span data-ttu-id="cae5f-158">Fáze 2: Příprava virtuálního počítače pro Tomcat7</span><span class="sxs-lookup"><span data-stu-id="cae5f-158">Phase 2: Prepare your virtual machine for Tomcat7</span></span>
<span data-ttu-id="cae5f-159">V této fázi konfigurace koncového bodu pro provoz Tomcat a potom se připojte tooyour nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cae5f-159">In this phase, you will configure an endpoint for Tomcat traffic, and then connect tooyour new virtual machine.</span></span>

### <a name="step-1-open-hello-http-port-tooallow-web-access"></a><span data-ttu-id="cae5f-160">Krok 1: Otevřete hello HTTP port tooallow webový přístup</span><span class="sxs-lookup"><span data-stu-id="cae5f-160">Step 1: Open hello HTTP port tooallow web access</span></span>
<span data-ttu-id="cae5f-161">Protokol TCP nebo UDP, společně s veřejné a privátní port obsahovat koncové body v Azure.</span><span class="sxs-lookup"><span data-stu-id="cae5f-161">Endpoints in Azure consist of a TCP or UDP protocol, along with a public and private port.</span></span> <span data-ttu-id="cae5f-162">Hello privátní port je port hello, služba hello naslouchá tooon hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cae5f-162">hello private port is hello port that hello service is listening tooon hello virtual machine.</span></span> <span data-ttu-id="cae5f-163">Hello veřejný port je port hello hello cloudové služby Azure naslouchá tooexternally pro příchozí, internetový provoz.</span><span class="sxs-lookup"><span data-stu-id="cae5f-163">hello public port is hello port that hello Azure cloud service listens tooexternally for incoming, Internet-based traffic.</span></span>  

<span data-ttu-id="cae5f-164">TCP port 8080 je hello výchozí číslo portu, Tomcat používá toolisten.</span><span class="sxs-lookup"><span data-stu-id="cae5f-164">TCP port 8080 is hello default port number that Tomcat uses toolisten.</span></span> <span data-ttu-id="cae5f-165">Tento port se při otevření s koncový bod Azure, můžete a dalších internetoví klienti mohou přistupovat Tomcat stránky.</span><span class="sxs-lookup"><span data-stu-id="cae5f-165">If this port is opened with an Azure endpoint, you and other Internet clients can access Tomcat pages.</span></span>  

1. <span data-ttu-id="cae5f-166">Hello portálu, klikněte na tlačítko **Procházet** > **virtuální počítače**a potom klikněte na hello virtuální počítač, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="cae5f-166">In hello portal, click **Browse** > **Virtual machines**, and then click hello virtual machine that you created.</span></span>  
   <span data-ttu-id="cae5f-167">![Snímek obrazovky directory hello virtuální počítače][5]</span><span class="sxs-lookup"><span data-stu-id="cae5f-167">![Screenshot of hello Virtual machines directory][5]</span></span>
2. <span data-ttu-id="cae5f-168">tooadd koncový bod tooyour virtuální počítač, klikněte na tlačítko hello **koncové body** pole.</span><span class="sxs-lookup"><span data-stu-id="cae5f-168">tooadd an endpoint tooyour virtual machine, click hello **Endpoints** box.</span></span>
   <span data-ttu-id="cae5f-169">![Snímek obrazovky zobrazující hello koncové body pole][6]</span><span class="sxs-lookup"><span data-stu-id="cae5f-169">![Screenshot that shows hello Endpoints box][6]</span></span>
3. <span data-ttu-id="cae5f-170">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="cae5f-170">Click **Add**.</span></span>  

   1. <span data-ttu-id="cae5f-171">Pro koncový bod hello, zadejte název pro koncový bod hello v **koncový bod**a pak zadejte 80 v **veřejný Port**.</span><span class="sxs-lookup"><span data-stu-id="cae5f-171">For hello endpoint, enter a name for hello endpoint in **Endpoint**, and then enter 80 in **Public Port**.</span></span>  

      <span data-ttu-id="cae5f-172">Pokud je nastavena too80, nepotřebujete číslo portu hello tooinclude hello adresu URL, která je použité tooaccess Tomcat.</span><span class="sxs-lookup"><span data-stu-id="cae5f-172">If you set it too80, you don’t need tooinclude hello port number in hello URL that is used tooaccess Tomcat.</span></span> <span data-ttu-id="cae5f-173">Například http://tomcatdemo.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="cae5f-173">For example, http://tomcatdemo.cloudapp.net.</span></span>    

      <span data-ttu-id="cae5f-174">Pokud je nastavena hodnota tooanother, jako je například 81, je nutné tooadd hello port číslo toohello URL tooaccess Tomcat.</span><span class="sxs-lookup"><span data-stu-id="cae5f-174">If you set it tooanother value, such as 81, you need tooadd hello port number toohello URL tooaccess Tomcat.</span></span> <span data-ttu-id="cae5f-175">Například http://tomcatdemo.cloudapp.net:81 /.</span><span class="sxs-lookup"><span data-stu-id="cae5f-175">For example,  http://tomcatdemo.cloudapp.net:81/.</span></span>
   2. <span data-ttu-id="cae5f-176">Zadejte 8080 v **privátní Port**.</span><span class="sxs-lookup"><span data-stu-id="cae5f-176">Enter 8080 in **Private Port**.</span></span> <span data-ttu-id="cae5f-177">Ve výchozím nastavení Tomcat naslouchá na portu TCP 8080.</span><span class="sxs-lookup"><span data-stu-id="cae5f-177">By default, Tomcat listens on TCP port 8080.</span></span> <span data-ttu-id="cae5f-178">Pokud jste změnili hello výchozí naslouchání portu Tomcat, by měl aktualizovat **privátní Port** toobe hello stejná hodnota jako hello port pro naslouchání Tomcat.</span><span class="sxs-lookup"><span data-stu-id="cae5f-178">If you changed hello default listen port of Tomcat, you should update **Private Port** toobe hello same as hello Tomcat listen port.</span></span>  
      <span data-ttu-id="cae5f-179">![Snímek obrazovky z uživatelské rozhraní, které se zobrazuje příkaz přidat, veřejný Port a privátní Port][7]</span><span class="sxs-lookup"><span data-stu-id="cae5f-179">![Screenshot of UI that shows Add command, Public Port, and Private Port][7]</span></span>
4. <span data-ttu-id="cae5f-180">Klikněte na tlačítko **OK** tooadd hello koncový bod tooyour virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cae5f-180">Click **OK** tooadd hello endpoint tooyour virtual machine.</span></span>

### <a name="step-2-connect-toohello-image-you-created"></a><span data-ttu-id="cae5f-181">Krok 2: Připojení toohello bitovou kopii, kterou jste vytvořili</span><span class="sxs-lookup"><span data-stu-id="cae5f-181">Step 2: Connect toohello image you created</span></span>
<span data-ttu-id="cae5f-182">Můžete použít jakéhokoli SSH nástroj tooconnect tooyour virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cae5f-182">You can choose any SSH tool tooconnect tooyour virtual machine.</span></span> <span data-ttu-id="cae5f-183">V tomto příkladu používáme PuTTY.</span><span class="sxs-lookup"><span data-stu-id="cae5f-183">In this example, we use PuTTY.</span></span>  

1. <span data-ttu-id="cae5f-184">Získání názvu DNS hello virtuálního počítače z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="cae5f-184">Get hello DNS name of your virtual machine from hello portal.</span></span>
    1. <span data-ttu-id="cae5f-185">Klikněte na tlačítko **Procházet** > **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="cae5f-185">Click **Browse** > **Virtual machines**.</span></span>
    2. <span data-ttu-id="cae5f-186">Vyberte hello název virtuálního počítače a pak klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="cae5f-186">Select hello name of your virtual machine, and then click **Properties**.</span></span>
    3. <span data-ttu-id="cae5f-187">V hello **vlastnosti** dlaždici, podívejte se v hello **název domény** název DNS hello tooget pole.</span><span class="sxs-lookup"><span data-stu-id="cae5f-187">In hello **Properties** tile, look in hello **Domain Name** box tooget hello DNS name.</span></span>  

2. <span data-ttu-id="cae5f-188">Získat hello číslo portu pro připojení SSH z hello **SSH** pole.</span><span class="sxs-lookup"><span data-stu-id="cae5f-188">Get hello port number for SSH connections from hello **SSH** box.</span></span>  
<span data-ttu-id="cae5f-189">![Snímek obrazovky, který zobrazuje číslo portu pro připojení SSH hello][8]</span><span class="sxs-lookup"><span data-stu-id="cae5f-189">![Screenshot that shows hello SSH connection port number][8]</span></span>

3. <span data-ttu-id="cae5f-190">Stáhněte si [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="cae5f-190">Download [PuTTY](http://www.putty.org/).</span></span>  

4. <span data-ttu-id="cae5f-191">Po stažení, klikněte na spustitelný soubor hello Putty.exe.</span><span class="sxs-lookup"><span data-stu-id="cae5f-191">After downloading, click hello executable file Putty.exe.</span></span> <span data-ttu-id="cae5f-192">V PuTTY konfiguraci konfigurovat základní možnosti hello hello názvem hostitele a portu číslo, které se získávají z hello vlastnosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cae5f-192">In PuTTY configuration, configure hello basic options with hello host name and port number that is obtained from hello properties of your virtual machine.</span></span>   
![Snímek obrazovky, který ukazuje možnosti název a port hostitele PuTTY konfigurace hello][9]

5. <span data-ttu-id="cae5f-194">V levém podokně hello, klikněte na **připojení** > **SSH** > **Auth**a potom klikněte na **Procházet** toospecify Hello umístění souboru privateKey.ppk hello.</span><span class="sxs-lookup"><span data-stu-id="cae5f-194">In hello left pane, click **Connection** > **SSH** > **Auth**, and then click **Browse** toospecify hello location of hello privateKey.ppk file.</span></span> <span data-ttu-id="cae5f-195">Hello privateKey.ppk soubor obsahuje hello privátní klíč, který je generovaný PuTTYgen dříve v hello "fáze 1: vytvoření image" tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="cae5f-195">hello privateKey.ppk file contains hello private key that is generated by PuTTYgen earlier in hello "Phase 1: Create an image" section of this article.</span></span>  
<span data-ttu-id="cae5f-196">![Snímek obrazovky zobrazující hierarchie adresářů hello připojení a tlačítko Procházet][10]</span><span class="sxs-lookup"><span data-stu-id="cae5f-196">![Screenshot that shows hello Connection directory hierarchy and Browse button][10]</span></span>

6. <span data-ttu-id="cae5f-197">Klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="cae5f-197">Click **Open**.</span></span> <span data-ttu-id="cae5f-198">Může být upozorněni podle okno se zprávou.</span><span class="sxs-lookup"><span data-stu-id="cae5f-198">You might be alerted by a message box.</span></span> <span data-ttu-id="cae5f-199">Pokud jste nakonfigurovali hello DNS název a číslo portu správně, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="cae5f-199">If you have configured hello DNS name and port number correctly, click **Yes**.</span></span>
<span data-ttu-id="cae5f-200">![Snímek obrazovky, který se zobrazí oznámení o hello][11]</span><span class="sxs-lookup"><span data-stu-id="cae5f-200">![Screenshot that shows hello notification][11]</span></span>

7. <span data-ttu-id="cae5f-201">Můžete se výzvami tooenter vaše uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="cae5f-201">You are prompted tooenter your username.</span></span>  
![Snímek obrazovky, který ukazuje, kde tooenter uživatelské jméno][12]

8. <span data-ttu-id="cae5f-203">Zadejte uživatelské jméno hello jste použili toocreate hello virtuálního počítače v hello "fáze 1: vytvoření image" dříve v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="cae5f-203">Enter hello username that you used toocreate hello virtual machine in hello "Phase 1: Create an image" section earlier in this article.</span></span> <span data-ttu-id="cae5f-204">Zobrazí se něco podobného jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="cae5f-204">You will see something like hello following:</span></span>  
![Snímek obrazovky zobrazující hello ověřování potvrzení][13]

## <a name="phase-3-install-software"></a><span data-ttu-id="cae5f-206">Fáze 3: Instalace softwaru</span><span class="sxs-lookup"><span data-stu-id="cae5f-206">Phase 3: Install software</span></span>
<span data-ttu-id="cae5f-207">V této fázi nainstalujte prostředí Java runtime hello, Tomcat7 a další součásti Tomcat7.</span><span class="sxs-lookup"><span data-stu-id="cae5f-207">In this phase, you install hello Java runtime environment, Tomcat7, and other Tomcat7 components.</span></span>  

### <a name="java-runtime-environment"></a><span data-ttu-id="cae5f-208">Prostředí Java runtime</span><span class="sxs-lookup"><span data-stu-id="cae5f-208">Java runtime environment</span></span>
<span data-ttu-id="cae5f-209">Tomcat je napsán v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="cae5f-209">Tomcat is written in Java.</span></span> <span data-ttu-id="cae5f-210">Existují dva typy sad Java Development Kit (JDKs), OpenJDK a Oracle JDK.</span><span class="sxs-lookup"><span data-stu-id="cae5f-210">There are two kinds of Java Development Kits (JDKs), OpenJDK and Oracle JDK.</span></span> <span data-ttu-id="cae5f-211">Můžete zvolit hello, kterou potřebujete.</span><span class="sxs-lookup"><span data-stu-id="cae5f-211">You can choose hello one you want.</span></span>  

> [!NOTE]
> <span data-ttu-id="cae5f-212">Jak JDKs mít prakticky hello stejný kód hello třídy v hello Java API, ale hello kód pro virtuální počítač hello se liší.</span><span class="sxs-lookup"><span data-stu-id="cae5f-212">Both JDKs have almost hello same code for hello classes in hello Java API, but hello code for hello virtual machine is different.</span></span> <span data-ttu-id="cae5f-213">OpenJDK obvykle otevřené knihovny toouse, zatímco Oracle JDK obvykle toouse uzavřený těch, které jsou.</span><span class="sxs-lookup"><span data-stu-id="cae5f-213">OpenJDK tends toouse open libraries, while Oracle JDK tends toouse closed ones.</span></span> <span data-ttu-id="cae5f-214">Oracle JDK má více tříd a některé vyřešili chyb a Oracle JDK je stabilnější než OpenJDK.</span><span class="sxs-lookup"><span data-stu-id="cae5f-214">Oracle JDK has more classes and some fixed bugs, and Oracle JDK is more stable than OpenJDK.</span></span>

#### <a name="install-openjdk"></a><span data-ttu-id="cae5f-215">Nainstalujte OpenJDK</span><span class="sxs-lookup"><span data-stu-id="cae5f-215">Install OpenJDK</span></span>  

<span data-ttu-id="cae5f-216">Použijte následující příkaz toodownload OpenJDK hello.</span><span class="sxs-lookup"><span data-stu-id="cae5f-216">Use hello following command toodownload OpenJDK.</span></span>   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  


* <span data-ttu-id="cae5f-217">toocreate directory toocontain hello JDK soubory:</span><span class="sxs-lookup"><span data-stu-id="cae5f-217">toocreate a directory toocontain hello JDK files:</span></span>  

        sudo mkdir /usr/lib/jvm  
* <span data-ttu-id="cae5f-218">tooextract hello JDK soubory do hello/usr/lib/jvm/directory:</span><span class="sxs-lookup"><span data-stu-id="cae5f-218">tooextract hello JDK files into hello /usr/lib/jvm/ directory:</span></span>  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/

#### <a name="install-oracle-jdk"></a><span data-ttu-id="cae5f-219">Nainstalujte Oracle JDK</span><span class="sxs-lookup"><span data-stu-id="cae5f-219">Install Oracle JDK</span></span>


<span data-ttu-id="cae5f-220">Použijte následující příkaz toodownload Oracle JDK z webu Oracle hello hello.</span><span class="sxs-lookup"><span data-stu-id="cae5f-220">Use hello following command toodownload Oracle JDK from hello Oracle website.</span></span>  

     wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  
* <span data-ttu-id="cae5f-221">toocreate directory toocontain hello JDK soubory:</span><span class="sxs-lookup"><span data-stu-id="cae5f-221">toocreate a directory toocontain hello JDK files:</span></span>  

        sudo mkdir /usr/lib/jvm  
* <span data-ttu-id="cae5f-222">tooextract hello JDK soubory do hello/usr/lib/jvm/directory:</span><span class="sxs-lookup"><span data-stu-id="cae5f-222">tooextract hello JDK files into hello /usr/lib/jvm/ directory:</span></span>  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  
* <span data-ttu-id="cae5f-223">tooset Oracle JDK jako virtuální počítač Java výchozí hello:</span><span class="sxs-lookup"><span data-stu-id="cae5f-223">tooset Oracle JDK as hello default Java virtual machine:</span></span>  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  

        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

#### <a name="confirm-that-java-installation-is-successful"></a><span data-ttu-id="cae5f-224">Potvrďte, že Java instalace byla úspěšně dokončena</span><span class="sxs-lookup"><span data-stu-id="cae5f-224">Confirm that Java installation is successful</span></span>
<span data-ttu-id="cae5f-225">Můžete použít příkaz jako hello následující tootest, pokud je prostředí Java runtime hello správně nainstalován:</span><span class="sxs-lookup"><span data-stu-id="cae5f-225">You can use a command like hello following tootest if hello Java runtime environment is installed correctly:</span></span>  

    java -version  

<span data-ttu-id="cae5f-226">Pokud jste nainstalovali OpenJDK, zobrazí se zpráva podobná následující hello: ![OpenJDK úspěšné instalace zpráv][14]</span><span class="sxs-lookup"><span data-stu-id="cae5f-226">If you installed OpenJDK, you should see a message like hello following: ![Successful OpenJDK installation message][14]</span></span>

<span data-ttu-id="cae5f-227">Pokud jste nainstalovali Oracle JDK, zobrazí zpráva podobná následující hello: ![zpráva instalace úspěšná JDK Oracle][15]</span><span class="sxs-lookup"><span data-stu-id="cae5f-227">If you installed Oracle JDK, you should see a message like hello following: ![Successful Oracle JDK installation message][15]</span></span>

### <a name="install-tomcat7"></a><span data-ttu-id="cae5f-228">Instalace Tomcat7</span><span class="sxs-lookup"><span data-stu-id="cae5f-228">Install Tomcat7</span></span>
<span data-ttu-id="cae5f-229">Použijte následující příkaz tooinstall Tomcat7 hello.</span><span class="sxs-lookup"><span data-stu-id="cae5f-229">Use hello following command tooinstall Tomcat7.</span></span>  

    sudo apt-get install tomcat7  

<span data-ttu-id="cae5f-230">Pokud nepoužíváte Tomcat7, použijte odpovídající variantu hello tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="cae5f-230">If you are not using Tomcat7, use hello appropriate variation of this command.</span></span>  

#### <a name="confirm-that-tomcat7-installation-is-successful"></a><span data-ttu-id="cae5f-231">Potvrďte, že je instalace Tomcat7 úspěšné</span><span class="sxs-lookup"><span data-stu-id="cae5f-231">Confirm that Tomcat7 installation is successful</span></span>
<span data-ttu-id="cae5f-232">toocheck Pokud Tomcat7 je úspěšně nainstalován, vyhledejte název DNS serveru Tomcat tooyour.</span><span class="sxs-lookup"><span data-stu-id="cae5f-232">toocheck if Tomcat7 is successfully installed, browse tooyour Tomcat server’s DNS name.</span></span> <span data-ttu-id="cae5f-233">V tomto článku je příklad adresy URL hello http://tomcatexample.cloudapp.net/.</span><span class="sxs-lookup"><span data-stu-id="cae5f-233">In this article, hello example URL is http://tomcatexample.cloudapp.net/.</span></span> <span data-ttu-id="cae5f-234">Pokud se zobrazí zpráva podobná následující hello, Tomcat7 je správně nainstalován.</span><span class="sxs-lookup"><span data-stu-id="cae5f-234">If you see a message like hello following, Tomcat7 is installed correctly.</span></span>
<span data-ttu-id="cae5f-235">![Zpráva úspěšné instalace Tomcat7][16]</span><span class="sxs-lookup"><span data-stu-id="cae5f-235">![Successful Tomcat7 installation message][16]</span></span>

### <a name="install-other-tomcat7-components"></a><span data-ttu-id="cae5f-236">Instalace součásti Tomcat7</span><span class="sxs-lookup"><span data-stu-id="cae5f-236">Install other Tomcat7 components</span></span>
<span data-ttu-id="cae5f-237">Existují další volitelné součásti Tomcat, které můžete nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="cae5f-237">There are other optional Tomcat components that you can install.</span></span>  

<span data-ttu-id="cae5f-238">Použití hello **sudo výstižný mezipaměti vyhledávání tomcat7** příkaz toosee všech součástí hello k dispozici.</span><span class="sxs-lookup"><span data-stu-id="cae5f-238">Use hello **sudo apt-cache search tomcat7** command toosee all of hello available components.</span></span> <span data-ttu-id="cae5f-239">Použijte následující příkazy tooinstall hello některé užitečné součásti.</span><span class="sxs-lookup"><span data-stu-id="cae5f-239">Use hello following commands tooinstall some useful components.</span></span>  

    sudo apt-get install tomcat7-admin      #admin web applications

    sudo apt-get install tomcat7-user         #tools toocreate user instances  

## <a name="phase-4-configure-tomcat7"></a><span data-ttu-id="cae5f-240">Fáze 4: Konfigurace Tomcat7</span><span class="sxs-lookup"><span data-stu-id="cae5f-240">Phase 4: Configure Tomcat7</span></span>
<span data-ttu-id="cae5f-241">V této fázi spravovat Tomcat.</span><span class="sxs-lookup"><span data-stu-id="cae5f-241">In this phase, you administer Tomcat.</span></span>

### <a name="start-and-stop-tomcat7"></a><span data-ttu-id="cae5f-242">Spuštění a zastavení Tomcat7</span><span class="sxs-lookup"><span data-stu-id="cae5f-242">Start and stop Tomcat7</span></span>
<span data-ttu-id="cae5f-243">Hello Tomcat7 server se automaticky spustí, když ho nainstalujete.</span><span class="sxs-lookup"><span data-stu-id="cae5f-243">hello Tomcat7 server automatically starts when you install it.</span></span> <span data-ttu-id="cae5f-244">Můžete také spustit s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cae5f-244">You can also start it with hello following command:</span></span>   

    sudo /etc/init.d/tomcat7 start

<span data-ttu-id="cae5f-245">toostop Tomcat7:</span><span class="sxs-lookup"><span data-stu-id="cae5f-245">toostop Tomcat7:</span></span>

    sudo /etc/init.d/tomcat7 stop

<span data-ttu-id="cae5f-246">Stav hello tooview Tomcat7:</span><span class="sxs-lookup"><span data-stu-id="cae5f-246">tooview hello status of Tomcat7:</span></span>

    sudo /etc/init.d/tomcat7 status

<span data-ttu-id="cae5f-247">toorestart Tomcat services:</span><span class="sxs-lookup"><span data-stu-id="cae5f-247">toorestart Tomcat services:</span></span> 

    sudo /etc/init.d/tomcat7 restart

### <a name="tomcat7-administration"></a><span data-ttu-id="cae5f-248">Správa Tomcat7</span><span class="sxs-lookup"><span data-stu-id="cae5f-248">Tomcat7 administration</span></span>
<span data-ttu-id="cae5f-249">Můžete upravit hello Tomcat uživatele konfigurační soubor tooset až přihlašovacích údajů správce.</span><span class="sxs-lookup"><span data-stu-id="cae5f-249">You can edit hello Tomcat user configuration file tooset up your admin credentials.</span></span> <span data-ttu-id="cae5f-250">Hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cae5f-250">Use hello following command:</span></span>  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

<span data-ttu-id="cae5f-251">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="cae5f-251">Here is an example:</span></span>  
![Snímek obrazovky zobrazující výstupu příkazu vi sudo hello][17]  

> [!NOTE]
> <span data-ttu-id="cae5f-253">Vytvořte silné heslo pro uživatelské jméno správce hello.</span><span class="sxs-lookup"><span data-stu-id="cae5f-253">Create a strong password for hello admin username.</span></span>  

<span data-ttu-id="cae5f-254">Po úpravě tohoto souboru, musíte restartovat služby Tomcat7 s následující příkaz tooensure hello změny projeví hello:</span><span class="sxs-lookup"><span data-stu-id="cae5f-254">After editing this file, you should restart Tomcat7 services with hello following command tooensure that hello changes take effect:</span></span>  

    sudo /etc/init.d/tomcat7 restart  

<span data-ttu-id="cae5f-255">Otevřete prohlížeč a zadejte **http://<your tomcat server DNS name>/manager/html** jako hello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="cae5f-255">Open your browser, and enter **http://<your tomcat server DNS name>/manager/html** as hello URL.</span></span> <span data-ttu-id="cae5f-256">Například hello v tomto článku adresa URL hello je http://tomcatexample.cloudapp.net/manager/html.</span><span class="sxs-lookup"><span data-stu-id="cae5f-256">For hello example in this article, hello URL is http://tomcatexample.cloudapp.net/manager/html.</span></span>  

<span data-ttu-id="cae5f-257">Po připojení, měli byste vidět něco podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="cae5f-257">After connecting, you should see something similar toohello following:</span></span>  
![Snímek obrazovky hello správce Tomcat webových aplikací][18]

## <a name="common-issues"></a><span data-ttu-id="cae5f-259">Běžné problémy</span><span class="sxs-lookup"><span data-stu-id="cae5f-259">Common issues</span></span>
### <a name="cant-access-hello-virtual-machine-with-tomcat-and-moodle-from-hello-internet"></a><span data-ttu-id="cae5f-260">Nelze získat přístup hello virtuální počítač s Tomcat a Moodle z hello Internetu</span><span class="sxs-lookup"><span data-stu-id="cae5f-260">Can't access hello virtual machine with Tomcat and Moodle from hello Internet</span></span>
#### <a name="symptom"></a><span data-ttu-id="cae5f-261">Příznaky</span><span class="sxs-lookup"><span data-stu-id="cae5f-261">Symptom</span></span>  
  <span data-ttu-id="cae5f-262">Tomcat běží, ale nemůžete zobrazit hello Tomcat výchozí stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="cae5f-262">Tomcat is running but you can’t see hello Tomcat default page with your browser.</span></span>
#### <a name="possible-root-cause"></a><span data-ttu-id="cae5f-263">Možné příčiny</span><span class="sxs-lookup"><span data-stu-id="cae5f-263">Possible root cause</span></span>   

  * <span data-ttu-id="cae5f-264">port pro naslouchání Tomcat Hello není hello stejné jako hello privátní port koncového bodu virtuálního počítače pro provoz Tomcat.</span><span class="sxs-lookup"><span data-stu-id="cae5f-264">hello Tomcat listen port is not hello same as hello private port of your virtual machine's endpoint for Tomcat traffic.</span></span>  

     <span data-ttu-id="cae5f-265">Port pro naslouchání zkontrolujte veřejný port a privátní port nastavení koncového bodu a zajistěte, aby privátní port hello je hello stejná hodnota jako hello Tomcat.</span><span class="sxs-lookup"><span data-stu-id="cae5f-265">Check your public port and private port endpoint settings and make sure hello private port is hello same as hello Tomcat listen port.</span></span> <span data-ttu-id="cae5f-266">Najdete v části "fáze 1: vytvoření image" tohoto článku Pokyny ke konfiguraci koncových bodů pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="cae5f-266">See "Phase 1: Create an image" section of this article for instructions on configuring endpoints for your virtual machine.</span></span>  

     <span data-ttu-id="cae5f-267">toodetermine hello Tomcat port pro naslouchání, otevřete /etc/httpd/conf/httpd.conf (Red Hat vydání) nebo /etc/tomcat7/server.xml (Debian vydání).</span><span class="sxs-lookup"><span data-stu-id="cae5f-267">toodetermine hello Tomcat listen port, open /etc/httpd/conf/httpd.conf (Red Hat release), or /etc/tomcat7/server.xml (Debian release).</span></span> <span data-ttu-id="cae5f-268">Ve výchozím nastavení je hello port pro naslouchání Tomcat 8080.</span><span class="sxs-lookup"><span data-stu-id="cae5f-268">By default, hello Tomcat listen port is 8080.</span></span> <span data-ttu-id="cae5f-269">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="cae5f-269">Here is an example:</span></span>  

        <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"   URIEncoding="UTF-8"            redirectPort="8443" />  

     <span data-ttu-id="cae5f-270">Pokud používáte virtuální počítač jako Debian a Ubuntu a chcete, aby toochange hello výchozí port z Tomcat naslouchání (například 8081), měli byste taky otevřít port hello hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="cae5f-270">If you are using a virtual machine like Debian or Ubuntu and you want toochange hello default port of Tomcat Listen (for example 8081), you should also open hello port for hello operating system.</span></span> <span data-ttu-id="cae5f-271">První, otevřete hello profil:</span><span class="sxs-lookup"><span data-stu-id="cae5f-271">First, open hello profile:</span></span>  

        sudo vi /etc/default/tomcat7  

     <span data-ttu-id="cae5f-272">Potom Odkomentujte hello poslední řádek a změňte "žádný" příliš "Ano".</span><span class="sxs-lookup"><span data-stu-id="cae5f-272">Then uncomment hello last line and change “no” too“yes”.</span></span>  

        AUTHBIND=yes
  2. <span data-ttu-id="cae5f-273">brány firewall Hello zakázal hello naslouchání portu z Tomcat.</span><span class="sxs-lookup"><span data-stu-id="cae5f-273">hello firewall has disabled hello listen port of Tomcat.</span></span>

     <span data-ttu-id="cae5f-274">Zobrazí se pouze hello Tomcat výchozí stránky z místního hostitele hello.</span><span class="sxs-lookup"><span data-stu-id="cae5f-274">You can only see hello Tomcat default page from hello local host.</span></span> <span data-ttu-id="cae5f-275">problém Hello je velmi pravděpodobné, že hello port, který je naslouchali tooby Tomcat, je blokována bránou hello firewall.</span><span class="sxs-lookup"><span data-stu-id="cae5f-275">hello problem is most likely that hello port, which is listened tooby Tomcat, is blocked by hello firewall.</span></span> <span data-ttu-id="cae5f-276">Můžete vytvořit webovou stránku hello w3m nástroj toobrowse hello.</span><span class="sxs-lookup"><span data-stu-id="cae5f-276">You can use hello w3m tool toobrowse hello webpage.</span></span> <span data-ttu-id="cae5f-277">Hello následujících příkazů nainstalujte w3m a procházet toohello Tomcat výchozí stránky:</span><span class="sxs-lookup"><span data-stu-id="cae5f-277">hello following commands install w3m and browse toohello Tomcat default page:</span></span>  


        <span data-ttu-id="cae5f-278">sudo yum instalace w3m w3m-img</span><span class="sxs-lookup"><span data-stu-id="cae5f-278">sudo yum  install w3m w3m-img</span></span>


        <span data-ttu-id="cae5f-279">w3m adrese http://localhost: 8080</span><span class="sxs-lookup"><span data-stu-id="cae5f-279">w3m http://localhost:8080</span></span>  
#### <a name="solution"></a><span data-ttu-id="cae5f-280">Řešení</span><span class="sxs-lookup"><span data-stu-id="cae5f-280">Solution</span></span>

  * <span data-ttu-id="cae5f-281">Pokud hello port pro naslouchání Tomcat není hello stejné jako privátní port hello hello koncového bodu pro provoz toohello virtuální počítač, můžete potřebovat změnit privátní port hello toobe hello stejná hodnota jako hello port pro naslouchání Tomcat.</span><span class="sxs-lookup"><span data-stu-id="cae5f-281">If hello Tomcat listen port is not hello same as hello private port of hello endpoint for traffic toohello virtual machine, you need change hello private port toobe hello same as hello Tomcat listen port.</span></span>   
  2. <span data-ttu-id="cae5f-282">Pokud je hello problém způsoben brány firewall nebo iptables, přidejte následující řádky příliš/etc/sysconfig/iptables hello.</span><span class="sxs-lookup"><span data-stu-id="cae5f-282">If hello issue is caused by firewall/iptables, add hello following lines too/etc/sysconfig/iptables.</span></span> <span data-ttu-id="cae5f-283">druhý řádek Hello je potřeba jenom pro provoz https:</span><span class="sxs-lookup"><span data-stu-id="cae5f-283">hello second line is only needed for https traffic:</span></span>  

      <span data-ttu-id="cae5f-284">-A -p tcp -m tcp – dport 80 -j přijmout vstup</span><span class="sxs-lookup"><span data-stu-id="cae5f-284">-A INPUT -p tcp -m tcp --dport 80 -j ACCEPT</span></span>

      <span data-ttu-id="cae5f-285">-A -p tcp -m tcp – dport 443 -j přijmout vstup</span><span class="sxs-lookup"><span data-stu-id="cae5f-285">-A INPUT -p tcp -m tcp --dport 443 -j ACCEPT</span></span>  

     > [!IMPORTANT]
     > <span data-ttu-id="cae5f-286">Zajistěte, aby hello předchozí řádky jsou umístěny nad všechny řádky, které by globálně omezení přístupu, jako je například následující hello: - A -j ODMÍTNĚTE – odmítněte with icmp hostitele zakázáno vstup</span><span class="sxs-lookup"><span data-stu-id="cae5f-286">Make sure hello previous lines are positioned above any lines that would globally restrict access, such as hello following: -A INPUT -j REJECT --reject-with icmp-host-prohibited</span></span>



<span data-ttu-id="cae5f-287">tooreload hello iptables, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="cae5f-287">tooreload hello iptables, run hello following command:</span></span>

    service iptables restart

<span data-ttu-id="cae5f-288">To byl otestován v CentOS 6.3.</span><span class="sxs-lookup"><span data-stu-id="cae5f-288">This has been tested on CentOS 6.3.</span></span>

### <a name="permission-denied-when-you-upload-project-files-toovarlibtomcat7webapps"></a><span data-ttu-id="cae5f-289">Oprávnění byla odepřena při nahrávání projektu soubory příliš/var/lib/tomcat7/webapps /</span><span class="sxs-lookup"><span data-stu-id="cae5f-289">Permission denied when you upload project files too/var/lib/tomcat7/webapps/</span></span>
#### <a name="symptom"></a><span data-ttu-id="cae5f-290">Příznaky</span><span class="sxs-lookup"><span data-stu-id="cae5f-290">Symptom</span></span>
  <span data-ttu-id="cae5f-291">Při použití protokolu SFTP klienta (například FileZilla) tooconnect tooyour virtuálního počítače a přejděte příliš/var/lib/tomcat7/webapps/toopublish vašeho webu, dojde k chybě zprávy podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="cae5f-291">When you use an SFTP client (such as FileZilla) tooconnect tooyour virtual machine and navigate too/var/lib/tomcat7/webapps/ toopublish your site, you get an error message similar toohello following:</span></span>  

     status:    Listing directory /var/lib/tomcat7/webapps
     Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
     Error:    /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
     Error:    File transfer failed
#### <a name="possible-root-cause"></a><span data-ttu-id="cae5f-292">Možné příčiny</span><span class="sxs-lookup"><span data-stu-id="cae5f-292">Possible root cause</span></span>
  <span data-ttu-id="cae5f-293">Nemáte žádná oprávnění tooaccess hello /var/lib/tomcat7/webapps složka.</span><span class="sxs-lookup"><span data-stu-id="cae5f-293">You have no permissions tooaccess hello /var/lib/tomcat7/webapps folder.</span></span>  
#### <a name="solution"></a><span data-ttu-id="cae5f-294">Řešení</span><span class="sxs-lookup"><span data-stu-id="cae5f-294">Solution</span></span>  
  <span data-ttu-id="cae5f-295">Potřebujete oprávnění tooget z hello kořenového účtu.</span><span class="sxs-lookup"><span data-stu-id="cae5f-295">You need tooget permission from hello root account.</span></span> <span data-ttu-id="cae5f-296">Toohello uživatelské jméno kořenové, které jste použili při zřízení hello počítače, můžete změnit vlastnictví hello této složky.</span><span class="sxs-lookup"><span data-stu-id="cae5f-296">You can change hello ownership of that folder from root toohello username you used when you provisioned hello machine.</span></span> <span data-ttu-id="cae5f-297">Tady je příklad s názvem účtu azureuser hello:</span><span class="sxs-lookup"><span data-stu-id="cae5f-297">Here is an example with hello azureuser account name:</span></span>  

     sudo chown azureuser -R /var/lib/tomcat7/webapps

  <span data-ttu-id="cae5f-298">Příliš pomocí hello -R – možnost tooapply hello oprávnění pro všechny soubory v adresáři.</span><span class="sxs-lookup"><span data-stu-id="cae5f-298">Use hello -R option tooapply hello permissions for all files inside of a directory too.</span></span>  

  <span data-ttu-id="cae5f-299">Tento příkaz lze použít také pro adresáře.</span><span class="sxs-lookup"><span data-stu-id="cae5f-299">This command also works for directories.</span></span> <span data-ttu-id="cae5f-300">Hello -R – možnost změny hello oprávnění pro všechny soubory a adresáře v rámci adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="cae5f-300">hello -R option changes hello permissions for all files and directories inside hello directory.</span></span> <span data-ttu-id="cae5f-301">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="cae5f-301">Here is an example:</span></span>  

     sudo chown -R username:group directory  

  <span data-ttu-id="cae5f-302">Tento příkaz změní vlastnictví (uživatele a skupiny) pro všechny soubory a adresáře, které jsou uvnitř hello adresář.</span><span class="sxs-lookup"><span data-stu-id="cae5f-302">This command changes ownership (both user and group) for all files and directories that are inside hello directory.</span></span>  

  <span data-ttu-id="cae5f-303">Hello následující příkaz změní jenom oprávnění hello hello složky adresáře.</span><span class="sxs-lookup"><span data-stu-id="cae5f-303">hello following command only changes hello permission of hello folder directory.</span></span> <span data-ttu-id="cae5f-304">Hello soubory a složky v adresáři hello, nebudou změněny.</span><span class="sxs-lookup"><span data-stu-id="cae5f-304">hello files and folders inside hello directory are not changed.</span></span>  

     sudo chown username:group directory

[1]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png
