---
title: "Nastavit Apache Tomcat na virtuální počítač s Linuxem | Microsoft Docs"
description: "Zjistěte, jak nastavit Apache Tomcat7 pomocí virtuální počítače Azure s Linuxem."
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
ms.openlocfilehash: fa30c78a5a5d458ba8845c3c10b87538427786c9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-tomcat7-on-a-linux-virtual-machine-with-azure"></a><span data-ttu-id="ea611-103">Nastavit Tomcat7 na virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="ea611-103">Set up Tomcat7 on a Linux virtual machine with Azure</span></span>
<span data-ttu-id="ea611-104">Apache Tomcat (nebo jednoduše Tomcat, také dříve se označovaly jako Jakarta Tomcat) je webový server s otevřeným zdrojem a kontejner servlet vyvinuté pomocí softwaru Foundation Apache (amp).</span><span class="sxs-lookup"><span data-stu-id="ea611-104">Apache Tomcat (or simply Tomcat, also formerly called Jakarta Tomcat) is an open source web server and servlet container developed by the Apache Software Foundation (ASF).</span></span> <span data-ttu-id="ea611-105">Tomcat implementuje Java Servlet a specifikace JavaServer stránky (JSP) z Sun Microsystems.</span><span class="sxs-lookup"><span data-stu-id="ea611-105">Tomcat implements the Java Servlet and the JavaServer Pages (JSP) specifications from Sun Microsystems.</span></span> <span data-ttu-id="ea611-106">Tomcat poskytuje čistý Java HTTP prostředí webového serveru ke spouštění kódu v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="ea611-106">Tomcat provides a pure Java HTTP web server environment in which to run Java code.</span></span> <span data-ttu-id="ea611-107">V nejjednodušší konfiguraci Tomcat běží v procesu jednoho operačního systému.</span><span class="sxs-lookup"><span data-stu-id="ea611-107">In the simplest configuration, Tomcat runs in a single operating system process.</span></span> <span data-ttu-id="ea611-108">Tento proces se spustí nástroje Java virtual machine (JVM).</span><span class="sxs-lookup"><span data-stu-id="ea611-108">This process runs a Java virtual machine (JVM).</span></span> <span data-ttu-id="ea611-109">Každý požadavek HTTP z prohlížeče do Tomcat zpracovávány jako samostatné vláken v procesu Tomcat.</span><span class="sxs-lookup"><span data-stu-id="ea611-109">Every HTTP request from a browser to Tomcat is processed as a separate thread in the Tomcat process.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="ea611-110">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="ea611-110">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="ea611-111">Tento článek popisuje postup použití modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="ea611-111">This article covers how to use the classic deployment model.</span></span> <span data-ttu-id="ea611-112">Doporučujeme vám, že většina nových nasazení používala model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="ea611-112">We recommend that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="ea611-113">Použití šablony Resource Manageru k nasazení virtuálního počítače s Ubuntu s otevřete JDK a Tomcat, najdete v části [v tomto článku](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).</span><span class="sxs-lookup"><span data-stu-id="ea611-113">To use a Resource Manager template to deploy an Ubuntu VM with Open JDK and Tomcat, see [this article](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).</span></span>

<span data-ttu-id="ea611-114">V tomto článku bude instalace Tomcat7 na bitovou kopii systému Linux a nasadit v Azure.</span><span class="sxs-lookup"><span data-stu-id="ea611-114">In this article, you will install Tomcat7 on a Linux image and deploy it in Azure.</span></span>  

<span data-ttu-id="ea611-115">Co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="ea611-115">You will learn:</span></span>  

* <span data-ttu-id="ea611-116">Postup vytvoření virtuálního počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="ea611-116">How to create a virtual machine in Azure.</span></span>
* <span data-ttu-id="ea611-117">Postup přípravy Tomcat7 virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ea611-117">How to prepare the virtual machine for Tomcat7.</span></span>
* <span data-ttu-id="ea611-118">Postup instalace Tomcat7.</span><span class="sxs-lookup"><span data-stu-id="ea611-118">How to install Tomcat7.</span></span>

<span data-ttu-id="ea611-119">Předpokládá se, že už máte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="ea611-119">It is assumed that you already have an Azure subscription.</span></span>  <span data-ttu-id="ea611-120">Pokud není, můžete si zaregistrovat bezplatnou zkušební verzi na [webu Azure](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="ea611-120">If not, you can sign up for a free trial at [the Azure website](https://azure.microsoft.com/).</span></span> <span data-ttu-id="ea611-121">Pokud máte předplatné MSDN, najdete v části [Microsoft Azure speciální ceny: MSDN, MPN a výhody BizSpark](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39).</span><span class="sxs-lookup"><span data-stu-id="ea611-121">If you have an MSDN subscription, see [Microsoft Azure Special Pricing: MSDN, MPN, and BizSpark Benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39).</span></span> <span data-ttu-id="ea611-122">Další informace o Azure najdete v tématu [co je Azure?](https://azure.microsoft.com/overview/what-is-azure/).</span><span class="sxs-lookup"><span data-stu-id="ea611-122">To learn more about Azure, see [What is Azure?](https://azure.microsoft.com/overview/what-is-azure/).</span></span>

<span data-ttu-id="ea611-123">Tento článek předpokládá, že máte základní znalosti práce Tomcat a Linux.</span><span class="sxs-lookup"><span data-stu-id="ea611-123">This article assumes that you have a basic working knowledge of Tomcat and Linux.</span></span>  

## <a name="phase-1-create-an-image"></a><span data-ttu-id="ea611-124">Fáze 1: Vytvoření image</span><span class="sxs-lookup"><span data-stu-id="ea611-124">Phase 1: Create an image</span></span>
<span data-ttu-id="ea611-125">V této fázi vytvoříte virtuální počítač pomocí bitové kopie systému Linux v Azure.</span><span class="sxs-lookup"><span data-stu-id="ea611-125">In this phase, you will create a virtual machine by using a Linux image in Azure.</span></span>  

### <a name="step-1-generate-an-ssh-authentication-key"></a><span data-ttu-id="ea611-126">Krok 1: Generovat ověřovací klíč SSH</span><span class="sxs-lookup"><span data-stu-id="ea611-126">Step 1: Generate an SSH authentication key</span></span>
<span data-ttu-id="ea611-127">SSH je důležité nástroj pro správce systému.</span><span class="sxs-lookup"><span data-stu-id="ea611-127">SSH is an important tool for system administrators.</span></span> <span data-ttu-id="ea611-128">Však není doporučeno konfigurace zabezpečení přístupu na základě určit lidské hesla.</span><span class="sxs-lookup"><span data-stu-id="ea611-128">However, configuring access security based on a human-determined password is not recommended.</span></span> <span data-ttu-id="ea611-129">Uživatelé se zlými úmysly může rozdělit na váš systém podle uživatelského jména a vytváření silných hesel.</span><span class="sxs-lookup"><span data-stu-id="ea611-129">Malicious users can break into your system based on a username and a weak password.</span></span>

<span data-ttu-id="ea611-130">Dobrá zpráva je, že je způsob, jak nechte vzdálený přístup otevřené a nestarat se o hesla.</span><span class="sxs-lookup"><span data-stu-id="ea611-130">The good news is that there is a way to leave remote access open and not worry about passwords.</span></span> <span data-ttu-id="ea611-131">Tato metoda se skládá z ověřování s asymetrické šifrování.</span><span class="sxs-lookup"><span data-stu-id="ea611-131">This method consists of authentication with asymmetric cryptography.</span></span> <span data-ttu-id="ea611-132">Privátní klíč uživatele je ten, který uděluje ověřování.</span><span class="sxs-lookup"><span data-stu-id="ea611-132">The user’s private key is the one that grants the authentication.</span></span> <span data-ttu-id="ea611-133">Můžete dokonce uzamčení uživatelského účtu nepovolíte ověřování hesla.</span><span class="sxs-lookup"><span data-stu-id="ea611-133">You can even lock the user’s account to not allow password authentication.</span></span>

<span data-ttu-id="ea611-134">Další výhodou této metody je nepotřebujete různá hesla k přihlášení na jiné servery.</span><span class="sxs-lookup"><span data-stu-id="ea611-134">Another advantage of this method is that you do not need different passwords to sign in to different servers.</span></span> <span data-ttu-id="ea611-135">Můžete ověřovat pomocí osobní privátní klíče ve všech serverech, což zabraňuje si museli pamatovat více hesel.</span><span class="sxs-lookup"><span data-stu-id="ea611-135">You can authenticate by using the personal private key on all servers, which prevents you from having to remember several passwords.</span></span>



<span data-ttu-id="ea611-136">Postupujte podle těchto kroků generovat ověřovací klíč SSH.</span><span class="sxs-lookup"><span data-stu-id="ea611-136">Follow these steps to generate the SSH authentication key.</span></span>

1. <span data-ttu-id="ea611-137">Stáhněte a nainstalujte PuTTYgen z následujícího umístění: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span><span class="sxs-lookup"><span data-stu-id="ea611-137">Download and install PuTTYgen from the following location: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span></span>
2. <span data-ttu-id="ea611-138">Spusťte Puttygen.exe.</span><span class="sxs-lookup"><span data-stu-id="ea611-138">Run Puttygen.exe.</span></span>
3. <span data-ttu-id="ea611-139">Klikněte na tlačítko **generování** ke generování klíče.</span><span class="sxs-lookup"><span data-stu-id="ea611-139">Click **Generate** to generate the keys.</span></span> <span data-ttu-id="ea611-140">V procesu můžete zvýšit náhodnost přesunutím myši nad prázdné místo v okně.</span><span class="sxs-lookup"><span data-stu-id="ea611-140">In the process, you can increase randomness by moving the mouse over the blank area in the window.</span></span>  
   <span data-ttu-id="ea611-141">![PuTTY snímek obrazovky generátor klíč, který ukazuje tlačítko vygenerovat nový klíč][1]</span><span class="sxs-lookup"><span data-stu-id="ea611-141">![PuTTY Key Generator screenshot that shows the generate new key button][1]</span></span>
4. <span data-ttu-id="ea611-142">Po dokončení procesu generování Puttygen.exe zobrazí nový veřejný klíč.</span><span class="sxs-lookup"><span data-stu-id="ea611-142">After the generate process, Puttygen.exe will show your new public key.</span></span>  
   ![PuTTY snímek obrazovky generátor klíč, který ukazuje nový veřejný klíč a uložení privátního klíče tlačítko][2]
5. <span data-ttu-id="ea611-144">Vyberte a zkopírujte veřejný klíč a uložit ho do souboru s názvem publicKey.pem.</span><span class="sxs-lookup"><span data-stu-id="ea611-144">Select and copy the public key, and save it in a file named publicKey.pem.</span></span> <span data-ttu-id="ea611-145">Nemáte klikněte na tlačítko **uložit veřejný klíč**, protože formát souboru uložené veřejný klíč se liší od chceme veřejný klíč.</span><span class="sxs-lookup"><span data-stu-id="ea611-145">Don’t click **Save public key**, because the saved public key’s file format is different from the public key we want.</span></span>
6. <span data-ttu-id="ea611-146">Klikněte na tlačítko **uložit privátní klíč**a uložte ho do souboru s názvem privateKey.ppk.</span><span class="sxs-lookup"><span data-stu-id="ea611-146">Click **Save private key**, and save it in a file named privateKey.ppk.</span></span>

### <a name="step-2-create-the-image-in-the-azure-portal"></a><span data-ttu-id="ea611-147">Krok 2: Vytvoření bitové kopie na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ea611-147">Step 2: Create the image in the Azure portal</span></span>
1. <span data-ttu-id="ea611-148">V [portál](https://portal.azure.com/), klikněte na tlačítko **nový** na hlavním panelu na vytvoření bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="ea611-148">In the [portal](https://portal.azure.com/), click **New** in the task bar to create an image.</span></span> <span data-ttu-id="ea611-149">Zvolte Linux bitovou kopii, která je založena na vašich potřebách.</span><span class="sxs-lookup"><span data-stu-id="ea611-149">Then choose the Linux image that is based on your needs.</span></span> <span data-ttu-id="ea611-150">Následující příklad používá bitovou kopii Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="ea611-150">The following example uses the Ubuntu 14.04 image.</span></span>
<span data-ttu-id="ea611-151">![Snímek obrazovky portálu, který ukazuje tlačítka Nová][3]</span><span class="sxs-lookup"><span data-stu-id="ea611-151">![Screenshot of the portal that shows the New button][3]</span></span>

2. <span data-ttu-id="ea611-152">Pro **název hostitele**, zadejte název pro adresu URL, kterou jste a internetové klienty se bude používat pro přístup k tomuto virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="ea611-152">For **Host Name**, specify the name for the URL that you and Internet clients will use to access this virtual machine.</span></span> <span data-ttu-id="ea611-153">Zadejte poslední část názvu DNS, například tomcatdemo.</span><span class="sxs-lookup"><span data-stu-id="ea611-153">Define the last part of the DNS name, for example, tomcatdemo.</span></span> <span data-ttu-id="ea611-154">Azure pak vygeneruje adresu URL jako tomcatdemo.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="ea611-154">Azure will then generate the URL as tomcatdemo.cloudapp.net.</span></span>  

3. <span data-ttu-id="ea611-155">Pro **SSH ověřovací klíč**, zkopírujte hodnotu klíče ze souboru publicKey.pem, který obsahuje veřejný klíč generované PuTTYgen.</span><span class="sxs-lookup"><span data-stu-id="ea611-155">For **SSH Authentication Key**, copy the key value from the publicKey.pem file, which contains the public key generated by PuTTYgen.</span></span>  
<span data-ttu-id="ea611-156">![Pole ověřovací klíč SSH na portálu][4]</span><span class="sxs-lookup"><span data-stu-id="ea611-156">![SSH Authentication Key box in the portal][4]</span></span>

4. <span data-ttu-id="ea611-157">Podle potřeby nakonfigurujte další nastavení a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ea611-157">Configure other settings as needed, and then click **Create**.</span></span>  

## <a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a><span data-ttu-id="ea611-158">Fáze 2: Příprava virtuálního počítače pro Tomcat7</span><span class="sxs-lookup"><span data-stu-id="ea611-158">Phase 2: Prepare your virtual machine for Tomcat7</span></span>
<span data-ttu-id="ea611-159">V této fázi konfigurace koncového bodu pro provoz Tomcat a potom se připojte k nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ea611-159">In this phase, you will configure an endpoint for Tomcat traffic, and then connect to your new virtual machine.</span></span>

### <a name="step-1-open-the-http-port-to-allow-web-access"></a><span data-ttu-id="ea611-160">Krok 1: Otevřete port HTTP pro povolení webového přístupu</span><span class="sxs-lookup"><span data-stu-id="ea611-160">Step 1: Open the HTTP port to allow web access</span></span>
<span data-ttu-id="ea611-161">Protokol TCP nebo UDP, společně s veřejné a privátní port obsahovat koncové body v Azure.</span><span class="sxs-lookup"><span data-stu-id="ea611-161">Endpoints in Azure consist of a TCP or UDP protocol, along with a public and private port.</span></span> <span data-ttu-id="ea611-162">Privátní port je port, který služba naslouchá na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="ea611-162">The private port is the port that the service is listening to on the virtual machine.</span></span> <span data-ttu-id="ea611-163">Veřejný port je port, který cloudovou službu systému Azure naslouchá externě pro příchozí internetového provozu.</span><span class="sxs-lookup"><span data-stu-id="ea611-163">The public port is the port that the Azure cloud service listens to externally for incoming, Internet-based traffic.</span></span>  

<span data-ttu-id="ea611-164">TCP port 8080 je výchozí číslo portu, který Tomcat používá k naslouchání.</span><span class="sxs-lookup"><span data-stu-id="ea611-164">TCP port 8080 is the default port number that Tomcat uses to listen.</span></span> <span data-ttu-id="ea611-165">Tento port se při otevření s koncový bod Azure, můžete a dalších internetoví klienti mohou přistupovat Tomcat stránky.</span><span class="sxs-lookup"><span data-stu-id="ea611-165">If this port is opened with an Azure endpoint, you and other Internet clients can access Tomcat pages.</span></span>  

1. <span data-ttu-id="ea611-166">Na portálu, klikněte na tlačítko **Procházet** > **virtuální počítače**a potom klikněte na virtuální počítač, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="ea611-166">In the portal, click **Browse** > **Virtual machines**, and then click the virtual machine that you created.</span></span>  
   <span data-ttu-id="ea611-167">![Snímek obrazovky adresáři virtuální počítače][5]</span><span class="sxs-lookup"><span data-stu-id="ea611-167">![Screenshot of the Virtual machines directory][5]</span></span>
2. <span data-ttu-id="ea611-168">Chcete-li přidat koncový bod virtuálního počítače, klikněte na tlačítko **koncové body** pole.</span><span class="sxs-lookup"><span data-stu-id="ea611-168">To add an endpoint to your virtual machine, click the **Endpoints** box.</span></span>
   <span data-ttu-id="ea611-169">![Snímek obrazovky zobrazující pole koncových bodů][6]</span><span class="sxs-lookup"><span data-stu-id="ea611-169">![Screenshot that shows the Endpoints box][6]</span></span>
3. <span data-ttu-id="ea611-170">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="ea611-170">Click **Add**.</span></span>  

   1. <span data-ttu-id="ea611-171">Pro koncový bod, zadejte název koncového bodu v **koncový bod**a pak zadejte 80 v **veřejný Port**.</span><span class="sxs-lookup"><span data-stu-id="ea611-171">For the endpoint, enter a name for the endpoint in **Endpoint**, and then enter 80 in **Public Port**.</span></span>  

      <span data-ttu-id="ea611-172">Pokud je nastavena na 80, nemusíte zahrnovat číslo portu v adrese URL, který se používá pro přístup k Tomcat.</span><span class="sxs-lookup"><span data-stu-id="ea611-172">If you set it to 80, you don’t need to include the port number in the URL that is used to access Tomcat.</span></span> <span data-ttu-id="ea611-173">Například http://tomcatdemo.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="ea611-173">For example, http://tomcatdemo.cloudapp.net.</span></span>    

      <span data-ttu-id="ea611-174">Pokud je nastavena na jinou hodnotu, jako je například 81, budete muset přidat číslo portu na adresu URL pro přístup k Tomcat.</span><span class="sxs-lookup"><span data-stu-id="ea611-174">If you set it to another value, such as 81, you need to add the port number to the URL to access Tomcat.</span></span> <span data-ttu-id="ea611-175">Například http://tomcatdemo.cloudapp.net:81 /.</span><span class="sxs-lookup"><span data-stu-id="ea611-175">For example,  http://tomcatdemo.cloudapp.net:81/.</span></span>
   2. <span data-ttu-id="ea611-176">Zadejte 8080 v **privátní Port**.</span><span class="sxs-lookup"><span data-stu-id="ea611-176">Enter 8080 in **Private Port**.</span></span> <span data-ttu-id="ea611-177">Ve výchozím nastavení Tomcat naslouchá na portu TCP 8080.</span><span class="sxs-lookup"><span data-stu-id="ea611-177">By default, Tomcat listens on TCP port 8080.</span></span> <span data-ttu-id="ea611-178">Pokud jste změnili výchozí naslouchání portu Tomcat, by měl aktualizovat **privátní Port** být stejné jako Tomcat port pro naslouchání.</span><span class="sxs-lookup"><span data-stu-id="ea611-178">If you changed the default listen port of Tomcat, you should update **Private Port** to be the same as the Tomcat listen port.</span></span>  
      <span data-ttu-id="ea611-179">![Snímek obrazovky z uživatelské rozhraní, které se zobrazuje příkaz přidat, veřejný Port a privátní Port][7]</span><span class="sxs-lookup"><span data-stu-id="ea611-179">![Screenshot of UI that shows Add command, Public Port, and Private Port][7]</span></span>
4. <span data-ttu-id="ea611-180">Klikněte na tlačítko **OK** přidat koncový bod k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="ea611-180">Click **OK** to add the endpoint to your virtual machine.</span></span>

### <a name="step-2-connect-to-the-image-you-created"></a><span data-ttu-id="ea611-181">Krok 2: Připojení na bitovou kopii, kterou jste vytvořili</span><span class="sxs-lookup"><span data-stu-id="ea611-181">Step 2: Connect to the image you created</span></span>
<span data-ttu-id="ea611-182">Můžete vybrat jakýkoli SSH nástroj pro připojení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="ea611-182">You can choose any SSH tool to connect to your virtual machine.</span></span> <span data-ttu-id="ea611-183">V tomto příkladu používáme PuTTY.</span><span class="sxs-lookup"><span data-stu-id="ea611-183">In this example, we use PuTTY.</span></span>  

1. <span data-ttu-id="ea611-184">Získáte název DNS virtuálního počítače z portálu.</span><span class="sxs-lookup"><span data-stu-id="ea611-184">Get the DNS name of your virtual machine from the portal.</span></span>
    1. <span data-ttu-id="ea611-185">Klikněte na tlačítko **Procházet** > **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="ea611-185">Click **Browse** > **Virtual machines**.</span></span>
    2. <span data-ttu-id="ea611-186">Vyberte název virtuálního počítače a pak klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="ea611-186">Select the name of your virtual machine, and then click **Properties**.</span></span>
    3. <span data-ttu-id="ea611-187">V **vlastnosti** dlaždici, podívejte se **název domény** pole získat název DNS.</span><span class="sxs-lookup"><span data-stu-id="ea611-187">In the **Properties** tile, look in the **Domain Name** box to get the DNS name.</span></span>  

2. <span data-ttu-id="ea611-188">Získat číslo portu pro SSH připojení z **SSH** pole.</span><span class="sxs-lookup"><span data-stu-id="ea611-188">Get the port number for SSH connections from the **SSH** box.</span></span>  
<span data-ttu-id="ea611-189">![Snímek obrazovky, který se zobrazuje číslo portu SSH připojení][8]</span><span class="sxs-lookup"><span data-stu-id="ea611-189">![Screenshot that shows the SSH connection port number][8]</span></span>

3. <span data-ttu-id="ea611-190">Stáhněte si [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="ea611-190">Download [PuTTY](http://www.putty.org/).</span></span>  

4. <span data-ttu-id="ea611-191">Po stažení, klikněte na spustitelný soubor Putty.exe.</span><span class="sxs-lookup"><span data-stu-id="ea611-191">After downloading, click the executable file Putty.exe.</span></span> <span data-ttu-id="ea611-192">V konfiguraci PuTTY nakonfigurujte základní možnosti s názvem hostitele a portu číslo, které se získávají z vlastností virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ea611-192">In PuTTY configuration, configure the basic options with the host name and port number that is obtained from the properties of your virtual machine.</span></span>   
![Snímek obrazovky, který ukazuje možnosti název a port hostitele PuTTY konfigurace][9]

5. <span data-ttu-id="ea611-194">V levém podokně klikněte na **připojení** > **SSH** > **Auth**a potom klikněte na **Procházet** k určení umístění souboru privateKey.ppk.</span><span class="sxs-lookup"><span data-stu-id="ea611-194">In the left pane, click **Connection** > **SSH** > **Auth**, and then click **Browse** to specify the location of the privateKey.ppk file.</span></span> <span data-ttu-id="ea611-195">Soubor privateKey.ppk obsahuje privátní klíč, který je generovaný PuTTYgen dříve v "fáze 1: vytvoření image" tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="ea611-195">The privateKey.ppk file contains the private key that is generated by PuTTYgen earlier in the "Phase 1: Create an image" section of this article.</span></span>  
<span data-ttu-id="ea611-196">![Snímek obrazovky, který se zobrazuje hierarchie adresářů připojení a tlačítko Procházet][10]</span><span class="sxs-lookup"><span data-stu-id="ea611-196">![Screenshot that shows the Connection directory hierarchy and Browse button][10]</span></span>

6. <span data-ttu-id="ea611-197">Klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="ea611-197">Click **Open**.</span></span> <span data-ttu-id="ea611-198">Může být upozorněni podle okno se zprávou.</span><span class="sxs-lookup"><span data-stu-id="ea611-198">You might be alerted by a message box.</span></span> <span data-ttu-id="ea611-199">Pokud jste nakonfigurovali název DNS a číslo portu správně, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="ea611-199">If you have configured the DNS name and port number correctly, click **Yes**.</span></span>
<span data-ttu-id="ea611-200">![Snímek obrazovky, který se zobrazí oznámení.][11]</span><span class="sxs-lookup"><span data-stu-id="ea611-200">![Screenshot that shows the notification][11]</span></span>

7. <span data-ttu-id="ea611-201">Zobrazí se výzva k zadání vašeho uživatelského jména.</span><span class="sxs-lookup"><span data-stu-id="ea611-201">You are prompted to enter your username.</span></span>  
![Snímek obrazovky, který ukazuje, kde k zadání uživatelského jména][12]

8. <span data-ttu-id="ea611-203">Zadejte uživatelské jméno, které jste použili k vytvoření virtuálního počítače "fáze 1: vytvoření image" dříve v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="ea611-203">Enter the username that you used to create the virtual machine in the "Phase 1: Create an image" section earlier in this article.</span></span> <span data-ttu-id="ea611-204">Zobrazí se přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="ea611-204">You will see something like the following:</span></span>  
![Snímek obrazovky zobrazující potvrzení ověřování][13]

## <a name="phase-3-install-software"></a><span data-ttu-id="ea611-206">Fáze 3: Instalace softwaru</span><span class="sxs-lookup"><span data-stu-id="ea611-206">Phase 3: Install software</span></span>
<span data-ttu-id="ea611-207">V této fázi nainstalujte prostředí Java runtime, Tomcat7 a další součásti Tomcat7.</span><span class="sxs-lookup"><span data-stu-id="ea611-207">In this phase, you install the Java runtime environment, Tomcat7, and other Tomcat7 components.</span></span>  

### <a name="java-runtime-environment"></a><span data-ttu-id="ea611-208">Prostředí Java runtime</span><span class="sxs-lookup"><span data-stu-id="ea611-208">Java runtime environment</span></span>
<span data-ttu-id="ea611-209">Tomcat je napsán v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="ea611-209">Tomcat is written in Java.</span></span> <span data-ttu-id="ea611-210">Existují dva typy sad Java Development Kit (JDKs), OpenJDK a Oracle JDK.</span><span class="sxs-lookup"><span data-stu-id="ea611-210">There are two kinds of Java Development Kits (JDKs), OpenJDK and Oracle JDK.</span></span> <span data-ttu-id="ea611-211">Můžete zvolit ten, který chcete.</span><span class="sxs-lookup"><span data-stu-id="ea611-211">You can choose the one you want.</span></span>  

> [!NOTE]
> <span data-ttu-id="ea611-212">Obě JDKs mít téměř stejný kód pro třídy v rozhraní API Java, ale kód pro virtuální počítač se liší.</span><span class="sxs-lookup"><span data-stu-id="ea611-212">Both JDKs have almost the same code for the classes in the Java API, but the code for the virtual machine is different.</span></span> <span data-ttu-id="ea611-213">OpenJDK obvykle používat otevřené knihovny, Oracle JDK obvykle pro použití uzavřené portů.</span><span class="sxs-lookup"><span data-stu-id="ea611-213">OpenJDK tends to use open libraries, while Oracle JDK tends to use closed ones.</span></span> <span data-ttu-id="ea611-214">Oracle JDK má více tříd a některé vyřešili chyb a Oracle JDK je stabilnější než OpenJDK.</span><span class="sxs-lookup"><span data-stu-id="ea611-214">Oracle JDK has more classes and some fixed bugs, and Oracle JDK is more stable than OpenJDK.</span></span>

#### <a name="install-openjdk"></a><span data-ttu-id="ea611-215">Nainstalujte OpenJDK</span><span class="sxs-lookup"><span data-stu-id="ea611-215">Install OpenJDK</span></span>  

<span data-ttu-id="ea611-216">Chcete-li stáhnout OpenJDK použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="ea611-216">Use the following command to download OpenJDK.</span></span>   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  


* <span data-ttu-id="ea611-217">Chcete-li vytvořit adresář, který bude obsahovat soubory JDK:</span><span class="sxs-lookup"><span data-stu-id="ea611-217">To create a directory to contain the JDK files:</span></span>  

        sudo mkdir /usr/lib/jvm  
* <span data-ttu-id="ea611-218">Extrahujte soubory JDK do adresáře/usr/lib/jvm /:</span><span class="sxs-lookup"><span data-stu-id="ea611-218">To extract the JDK files into the /usr/lib/jvm/ directory:</span></span>  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/

#### <a name="install-oracle-jdk"></a><span data-ttu-id="ea611-219">Nainstalujte Oracle JDK</span><span class="sxs-lookup"><span data-stu-id="ea611-219">Install Oracle JDK</span></span>


<span data-ttu-id="ea611-220">Použijte následující příkaz ke stažení z webu Oracle Oracle JDK.</span><span class="sxs-lookup"><span data-stu-id="ea611-220">Use the following command to download Oracle JDK from the Oracle website.</span></span>  

     wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  
* <span data-ttu-id="ea611-221">Chcete-li vytvořit adresář, který bude obsahovat soubory JDK:</span><span class="sxs-lookup"><span data-stu-id="ea611-221">To create a directory to contain the JDK files:</span></span>  

        sudo mkdir /usr/lib/jvm  
* <span data-ttu-id="ea611-222">Extrahujte soubory JDK do adresáře/usr/lib/jvm /:</span><span class="sxs-lookup"><span data-stu-id="ea611-222">To extract the JDK files into the /usr/lib/jvm/ directory:</span></span>  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  
* <span data-ttu-id="ea611-223">Chcete-li nastavit Oracle JDK jako virtuální počítač Java výchozí:</span><span class="sxs-lookup"><span data-stu-id="ea611-223">To set Oracle JDK as the default Java virtual machine:</span></span>  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  

        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

#### <a name="confirm-that-java-installation-is-successful"></a><span data-ttu-id="ea611-224">Potvrďte, že Java instalace byla úspěšně dokončena</span><span class="sxs-lookup"><span data-stu-id="ea611-224">Confirm that Java installation is successful</span></span>
<span data-ttu-id="ea611-225">Příkaz takto můžete otestovat, pokud je prostředí Java runtime správně nainstalován:</span><span class="sxs-lookup"><span data-stu-id="ea611-225">You can use a command like the following to test if the Java runtime environment is installed correctly:</span></span>  

    java -version  

<span data-ttu-id="ea611-226">Pokud jste nainstalovali OpenJDK, zobrazí se zpráva podobná následující: ![OpenJDK úspěšné instalace zpráv][14]</span><span class="sxs-lookup"><span data-stu-id="ea611-226">If you installed OpenJDK, you should see a message like the following: ![Successful OpenJDK installation message][14]</span></span>

<span data-ttu-id="ea611-227">Pokud jste nainstalovali Oracle JDK, zobrazí se zpráva podobná následující: ![zpráva instalace úspěšná JDK Oracle][15]</span><span class="sxs-lookup"><span data-stu-id="ea611-227">If you installed Oracle JDK, you should see a message like the following: ![Successful Oracle JDK installation message][15]</span></span>

### <a name="install-tomcat7"></a><span data-ttu-id="ea611-228">Instalace Tomcat7</span><span class="sxs-lookup"><span data-stu-id="ea611-228">Install Tomcat7</span></span>
<span data-ttu-id="ea611-229">Použijte následující příkaz k instalaci Tomcat7.</span><span class="sxs-lookup"><span data-stu-id="ea611-229">Use the following command to install Tomcat7.</span></span>  

    sudo apt-get install tomcat7  

<span data-ttu-id="ea611-230">Pokud nepoužíváte Tomcat7, použijte jeho odpovídající variantu tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="ea611-230">If you are not using Tomcat7, use the appropriate variation of this command.</span></span>  

#### <a name="confirm-that-tomcat7-installation-is-successful"></a><span data-ttu-id="ea611-231">Potvrďte, že je instalace Tomcat7 úspěšné</span><span class="sxs-lookup"><span data-stu-id="ea611-231">Confirm that Tomcat7 installation is successful</span></span>
<span data-ttu-id="ea611-232">Kontrola, pokud je úspěšně nainstalována Tomcat7, vyhledejte název DNS Tomcat server.</span><span class="sxs-lookup"><span data-stu-id="ea611-232">To check if Tomcat7 is successfully installed, browse to your Tomcat server’s DNS name.</span></span> <span data-ttu-id="ea611-233">Příklad adresy URL v tomto článku je http://tomcatexample.cloudapp.net/.</span><span class="sxs-lookup"><span data-stu-id="ea611-233">In this article, the example URL is http://tomcatexample.cloudapp.net/.</span></span> <span data-ttu-id="ea611-234">Pokud se zobrazí zpráva podobná následující, Tomcat7 je správně nainstalován.</span><span class="sxs-lookup"><span data-stu-id="ea611-234">If you see a message like the following, Tomcat7 is installed correctly.</span></span>
<span data-ttu-id="ea611-235">![Zpráva úspěšné instalace Tomcat7][16]</span><span class="sxs-lookup"><span data-stu-id="ea611-235">![Successful Tomcat7 installation message][16]</span></span>

### <a name="install-other-tomcat7-components"></a><span data-ttu-id="ea611-236">Instalace součásti Tomcat7</span><span class="sxs-lookup"><span data-stu-id="ea611-236">Install other Tomcat7 components</span></span>
<span data-ttu-id="ea611-237">Existují další volitelné součásti Tomcat, které můžete nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="ea611-237">There are other optional Tomcat components that you can install.</span></span>  

<span data-ttu-id="ea611-238">Použití **sudo výstižný mezipaměti vyhledávání tomcat7** příkazu zobrazte všechny dostupné součásti.</span><span class="sxs-lookup"><span data-stu-id="ea611-238">Use the **sudo apt-cache search tomcat7** command to see all of the available components.</span></span> <span data-ttu-id="ea611-239">Použijte následující příkazy pro instalaci některé užitečné součásti.</span><span class="sxs-lookup"><span data-stu-id="ea611-239">Use the following commands to install some useful components.</span></span>  

    sudo apt-get install tomcat7-admin      #admin web applications

    sudo apt-get install tomcat7-user         #tools to create user instances  

## <a name="phase-4-configure-tomcat7"></a><span data-ttu-id="ea611-240">Fáze 4: Konfigurace Tomcat7</span><span class="sxs-lookup"><span data-stu-id="ea611-240">Phase 4: Configure Tomcat7</span></span>
<span data-ttu-id="ea611-241">V této fázi spravovat Tomcat.</span><span class="sxs-lookup"><span data-stu-id="ea611-241">In this phase, you administer Tomcat.</span></span>

### <a name="start-and-stop-tomcat7"></a><span data-ttu-id="ea611-242">Spuštění a zastavení Tomcat7</span><span class="sxs-lookup"><span data-stu-id="ea611-242">Start and stop Tomcat7</span></span>
<span data-ttu-id="ea611-243">Tomcat7 server se automaticky spustí, když ho nainstalujete.</span><span class="sxs-lookup"><span data-stu-id="ea611-243">The Tomcat7 server automatically starts when you install it.</span></span> <span data-ttu-id="ea611-244">Můžete také spustit pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="ea611-244">You can also start it with the following command:</span></span>   

    sudo /etc/init.d/tomcat7 start

<span data-ttu-id="ea611-245">Chcete-li zastavit Tomcat7:</span><span class="sxs-lookup"><span data-stu-id="ea611-245">To stop Tomcat7:</span></span>

    sudo /etc/init.d/tomcat7 stop

<span data-ttu-id="ea611-246">Chcete-li zobrazit stav Tomcat7:</span><span class="sxs-lookup"><span data-stu-id="ea611-246">To view the status of Tomcat7:</span></span>

    sudo /etc/init.d/tomcat7 status

<span data-ttu-id="ea611-247">K restartu služeb Tomcat:</span><span class="sxs-lookup"><span data-stu-id="ea611-247">To restart Tomcat services:</span></span> 

    sudo /etc/init.d/tomcat7 restart

### <a name="tomcat7-administration"></a><span data-ttu-id="ea611-248">Správa Tomcat7</span><span class="sxs-lookup"><span data-stu-id="ea611-248">Tomcat7 administration</span></span>
<span data-ttu-id="ea611-249">Můžete upravit konfigurační soubor uživatele Tomcat k nastavení přihlašovacích údajů správce.</span><span class="sxs-lookup"><span data-stu-id="ea611-249">You can edit the Tomcat user configuration file to set up your admin credentials.</span></span> <span data-ttu-id="ea611-250">Použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ea611-250">Use the following command:</span></span>  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

<span data-ttu-id="ea611-251">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="ea611-251">Here is an example:</span></span>  
![Snímek obrazovky zobrazující výstupu příkazu sudo vi][17]  

> [!NOTE]
> <span data-ttu-id="ea611-253">Vytvořte silné heslo pro uživatelské jméno správce.</span><span class="sxs-lookup"><span data-stu-id="ea611-253">Create a strong password for the admin username.</span></span>  

<span data-ttu-id="ea611-254">Po úpravě tohoto souboru, musíte restartovat Tomcat7 službám pomocí následujícího příkazu zkontrolujte, že tyto změny začnou platit:</span><span class="sxs-lookup"><span data-stu-id="ea611-254">After editing this file, you should restart Tomcat7 services with the following command to ensure that the changes take effect:</span></span>  

    sudo /etc/init.d/tomcat7 restart  

<span data-ttu-id="ea611-255">Otevřete prohlížeč a zadejte **http://<your tomcat server DNS name>/manager/html** jako adresu URL.</span><span class="sxs-lookup"><span data-stu-id="ea611-255">Open your browser, and enter **http://<your tomcat server DNS name>/manager/html** as the URL.</span></span> <span data-ttu-id="ea611-256">Například v tomto článku adresa URL je http://tomcatexample.cloudapp.net/manager/html.</span><span class="sxs-lookup"><span data-stu-id="ea611-256">For the example in this article, the URL is http://tomcatexample.cloudapp.net/manager/html.</span></span>  

<span data-ttu-id="ea611-257">Po připojení, měli byste vidět něco podobného jako následující:</span><span class="sxs-lookup"><span data-stu-id="ea611-257">After connecting, you should see something similar to the following:</span></span>  
![Snímek obrazovky Správce aplikací webové Tomcat][18]

## <a name="common-issues"></a><span data-ttu-id="ea611-259">Běžné problémy</span><span class="sxs-lookup"><span data-stu-id="ea611-259">Common issues</span></span>
### <a name="cant-access-the-virtual-machine-with-tomcat-and-moodle-from-the-internet"></a><span data-ttu-id="ea611-260">Nelze získat přístup k virtuálnímu počítači s Tomcat a Moodle z Internetu</span><span class="sxs-lookup"><span data-stu-id="ea611-260">Can't access the virtual machine with Tomcat and Moodle from the Internet</span></span>
#### <a name="symptom"></a><span data-ttu-id="ea611-261">Příznaky</span><span class="sxs-lookup"><span data-stu-id="ea611-261">Symptom</span></span>  
  <span data-ttu-id="ea611-262">Tomcat běží, ale nemůžete zobrazit Tomcat výchozí stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ea611-262">Tomcat is running but you can’t see the Tomcat default page with your browser.</span></span>
#### <a name="possible-root-cause"></a><span data-ttu-id="ea611-263">Možné příčiny</span><span class="sxs-lookup"><span data-stu-id="ea611-263">Possible root cause</span></span>   

  * <span data-ttu-id="ea611-264">Port pro naslouchání Tomcat není stejný jako privátní port koncový bod virtuálního počítače pro provoz Tomcat.</span><span class="sxs-lookup"><span data-stu-id="ea611-264">The Tomcat listen port is not the same as the private port of your virtual machine's endpoint for Tomcat traffic.</span></span>  

     <span data-ttu-id="ea611-265">Zkontrolujte veřejný port a privátní port koncového bodu nastavení a ujistěte se, že privátní port je že stejný jako Tomcat port pro naslouchání.</span><span class="sxs-lookup"><span data-stu-id="ea611-265">Check your public port and private port endpoint settings and make sure the private port is the same as the Tomcat listen port.</span></span> <span data-ttu-id="ea611-266">Najdete v části "fáze 1: vytvoření image" tohoto článku Pokyny ke konfiguraci koncových bodů pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="ea611-266">See "Phase 1: Create an image" section of this article for instructions on configuring endpoints for your virtual machine.</span></span>  

     <span data-ttu-id="ea611-267">Ke zjištění portu naslouchání Tomcat, otevřete /etc/httpd/conf/httpd.conf (Red Hat vydání) nebo /etc/tomcat7/server.xml (Debian vydání).</span><span class="sxs-lookup"><span data-stu-id="ea611-267">To determine the Tomcat listen port, open /etc/httpd/conf/httpd.conf (Red Hat release), or /etc/tomcat7/server.xml (Debian release).</span></span> <span data-ttu-id="ea611-268">Ve výchozím nastavení je port pro naslouchání Tomcat 8080.</span><span class="sxs-lookup"><span data-stu-id="ea611-268">By default, the Tomcat listen port is 8080.</span></span> <span data-ttu-id="ea611-269">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="ea611-269">Here is an example:</span></span>  

        <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"   URIEncoding="UTF-8"            redirectPort="8443" />  

     <span data-ttu-id="ea611-270">Pokud používáte virtuální počítač jako Debian a Ubuntu a chcete změnit, výchozí port z Tomcat naslouchání (například 8081), měli byste taky otevřít port pro operační systém.</span><span class="sxs-lookup"><span data-stu-id="ea611-270">If you are using a virtual machine like Debian or Ubuntu and you want to change the default port of Tomcat Listen (for example 8081), you should also open the port for the operating system.</span></span> <span data-ttu-id="ea611-271">První otevřete profil:</span><span class="sxs-lookup"><span data-stu-id="ea611-271">First, open the profile:</span></span>  

        sudo vi /etc/default/tomcat7  

     <span data-ttu-id="ea611-272">Potom Odkomentujte poslední řádek a změňte "žádný" "Ano".</span><span class="sxs-lookup"><span data-stu-id="ea611-272">Then uncomment the last line and change “no” to “yes”.</span></span>  

        AUTHBIND=yes
  2. <span data-ttu-id="ea611-273">Brána firewall zakázal naslouchání portu Tomcat.</span><span class="sxs-lookup"><span data-stu-id="ea611-273">The firewall has disabled the listen port of Tomcat.</span></span>

     <span data-ttu-id="ea611-274">Zobrazí se pouze výchozí stránka Tomcat z místního hostitele.</span><span class="sxs-lookup"><span data-stu-id="ea611-274">You can only see the Tomcat default page from the local host.</span></span> <span data-ttu-id="ea611-275">Problém je velmi pravděpodobné, že je port, který je sleduje Tomcat, blokován branou firewall.</span><span class="sxs-lookup"><span data-stu-id="ea611-275">The problem is most likely that the port, which is listened to by Tomcat, is blocked by the firewall.</span></span> <span data-ttu-id="ea611-276">Můžete použít nástroj w3m a přejděte na webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="ea611-276">You can use the w3m tool to browse the webpage.</span></span> <span data-ttu-id="ea611-277">Následující příkazy instalace w3m a přejděte na stránku výchozího Tomcat:</span><span class="sxs-lookup"><span data-stu-id="ea611-277">The following commands install w3m and browse to the Tomcat default page:</span></span>  


        <span data-ttu-id="ea611-278">sudo yum instalace w3m w3m-img</span><span class="sxs-lookup"><span data-stu-id="ea611-278">sudo yum  install w3m w3m-img</span></span>


        <span data-ttu-id="ea611-279">w3m adrese http://localhost: 8080</span><span class="sxs-lookup"><span data-stu-id="ea611-279">w3m http://localhost:8080</span></span>  
#### <a name="solution"></a><span data-ttu-id="ea611-280">Řešení</span><span class="sxs-lookup"><span data-stu-id="ea611-280">Solution</span></span>

  * <span data-ttu-id="ea611-281">Pokud naslouchat Tomcat port není stejný jako privátní port koncového bodu pro provoz do virtuálního počítače, je nutné změnit privátní port, který má být že stejné jako Tomcat port pro naslouchání.</span><span class="sxs-lookup"><span data-stu-id="ea611-281">If the Tomcat listen port is not the same as the private port of the endpoint for traffic to the virtual machine, you need change the private port to be the same as the Tomcat listen port.</span></span>   
  2. <span data-ttu-id="ea611-282">Pokud tento problém je způsoben brány firewall nebo iptables, přidejte následující řádky do /etc/sysconfig/iptables.</span><span class="sxs-lookup"><span data-stu-id="ea611-282">If the issue is caused by firewall/iptables, add the following lines to /etc/sysconfig/iptables.</span></span> <span data-ttu-id="ea611-283">Druhý řádek je potřeba jenom pro provoz https:</span><span class="sxs-lookup"><span data-stu-id="ea611-283">The second line is only needed for https traffic:</span></span>  

      <span data-ttu-id="ea611-284">-A -p tcp -m tcp – dport 80 -j přijmout vstup</span><span class="sxs-lookup"><span data-stu-id="ea611-284">-A INPUT -p tcp -m tcp --dport 80 -j ACCEPT</span></span>

      <span data-ttu-id="ea611-285">-A -p tcp -m tcp – dport 443 -j přijmout vstup</span><span class="sxs-lookup"><span data-stu-id="ea611-285">-A INPUT -p tcp -m tcp --dport 443 -j ACCEPT</span></span>  

     > [!IMPORTANT]
     > <span data-ttu-id="ea611-286">Zajistěte, aby předchozí řádky jsou umístěny nad všechny řádky, které by globálně omezení přístupu, jako jsou následující: - A -j ODMÍTNĚTE – odmítněte with icmp hostitele zakázáno vstup</span><span class="sxs-lookup"><span data-stu-id="ea611-286">Make sure the previous lines are positioned above any lines that would globally restrict access, such as the following: -A INPUT -j REJECT --reject-with icmp-host-prohibited</span></span>



<span data-ttu-id="ea611-287">Chcete-li znovu načíst iptables, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ea611-287">To reload the iptables, run the following command:</span></span>

    service iptables restart

<span data-ttu-id="ea611-288">To byl otestován v CentOS 6.3.</span><span class="sxs-lookup"><span data-stu-id="ea611-288">This has been tested on CentOS 6.3.</span></span>

### <a name="permission-denied-when-you-upload-project-files-to-varlibtomcat7webapps"></a><span data-ttu-id="ea611-289">Oprávnění byla odepřena při nahrávání souborů projektu do /var/lib/tomcat7/webapps /</span><span class="sxs-lookup"><span data-stu-id="ea611-289">Permission denied when you upload project files to /var/lib/tomcat7/webapps/</span></span>
#### <a name="symptom"></a><span data-ttu-id="ea611-290">Příznaky</span><span class="sxs-lookup"><span data-stu-id="ea611-290">Symptom</span></span>
  <span data-ttu-id="ea611-291">Použijete-li pro připojení k virtuálnímu počítači a přejděte do /var/lib/tomcat7/webapps/publikování webu klientem SFTP (například FileZilla), můžete získat chybová zpráva podobná této:</span><span class="sxs-lookup"><span data-stu-id="ea611-291">When you use an SFTP client (such as FileZilla) to connect to your virtual machine and navigate to /var/lib/tomcat7/webapps/ to publish your site, you get an error message similar to the following:</span></span>  

     status:    Listing directory /var/lib/tomcat7/webapps
     Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
     Error:    /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
     Error:    File transfer failed
#### <a name="possible-root-cause"></a><span data-ttu-id="ea611-292">Možné příčiny</span><span class="sxs-lookup"><span data-stu-id="ea611-292">Possible root cause</span></span>
  <span data-ttu-id="ea611-293">Nemáte oprávnění k přístupu ke složce /var/lib/tomcat7/webapps.</span><span class="sxs-lookup"><span data-stu-id="ea611-293">You have no permissions to access the /var/lib/tomcat7/webapps folder.</span></span>  
#### <a name="solution"></a><span data-ttu-id="ea611-294">Řešení</span><span class="sxs-lookup"><span data-stu-id="ea611-294">Solution</span></span>  
  <span data-ttu-id="ea611-295">Musíte získat oprávnění z kořenového účtu.</span><span class="sxs-lookup"><span data-stu-id="ea611-295">You need to get permission from the root account.</span></span> <span data-ttu-id="ea611-296">Můžete změnit vlastnictví této složky z kořenového adresáře na uživatelské jméno, které jste použili při zřizování na počítač.</span><span class="sxs-lookup"><span data-stu-id="ea611-296">You can change the ownership of that folder from root to the username you used when you provisioned the machine.</span></span> <span data-ttu-id="ea611-297">Tady je příklad s názvem účtu azureuser:</span><span class="sxs-lookup"><span data-stu-id="ea611-297">Here is an example with the azureuser account name:</span></span>  

     sudo chown azureuser -R /var/lib/tomcat7/webapps

  <span data-ttu-id="ea611-298">Použijte parametr -R příliš použít oprávnění pro všechny soubory v adresáři.</span><span class="sxs-lookup"><span data-stu-id="ea611-298">Use the -R option to apply the permissions for all files inside of a directory too.</span></span>  

  <span data-ttu-id="ea611-299">Tento příkaz lze použít také pro adresáře.</span><span class="sxs-lookup"><span data-stu-id="ea611-299">This command also works for directories.</span></span> <span data-ttu-id="ea611-300">-R možnost změny oprávnění pro všechny soubory a adresáře v adresáři.</span><span class="sxs-lookup"><span data-stu-id="ea611-300">The -R option changes the permissions for all files and directories inside the directory.</span></span> <span data-ttu-id="ea611-301">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="ea611-301">Here is an example:</span></span>  

     sudo chown -R username:group directory  

  <span data-ttu-id="ea611-302">Tento příkaz změní vlastnictví (uživatele a skupiny) pro všechny soubory a adresáře, které jsou v adresáři.</span><span class="sxs-lookup"><span data-stu-id="ea611-302">This command changes ownership (both user and group) for all files and directories that are inside the directory.</span></span>  

  <span data-ttu-id="ea611-303">Tento příkaz změní jenom oprávnění adresáři složky.</span><span class="sxs-lookup"><span data-stu-id="ea611-303">The following command only changes the permission of the folder directory.</span></span> <span data-ttu-id="ea611-304">Soubory a složky v adresáři, nebudou změněny.</span><span class="sxs-lookup"><span data-stu-id="ea611-304">The files and folders inside the directory are not changed.</span></span>  

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
