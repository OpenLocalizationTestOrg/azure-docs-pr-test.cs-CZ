---
title: "hello aaaPublish kontejner Docker pomocí nástrojů Azure pro IntelliJ | Microsoft Docs"
description: "Zjistěte, jak hello toopublish tooMicrosoft webové aplikace Azure jako kontejner Docker pomocí nástrojů Azure pro IntelliJ."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: bee94cb269ea707ae7ad55232e23e915aec48c34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-hello-azure-toolkit-for-intellij"></a><span data-ttu-id="4e6e2-103">Publikovat webovou aplikaci pomocí hello Azure Toolkit pro IntelliJ jako kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="4e6e2-103">Publish a web app as a Docker container by using hello Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="4e6e2-104">Kontejnery docker jsou často používaný metodu pro nasazení webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="4e6e2-105">Pomocí Docker kontejnery vývojáři zvládá všechny své soubory projektu a závislosti do jednoho balíčku pro nasazení tooa server.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment tooa server.</span></span> <span data-ttu-id="4e6e2-106">Hello nástrojů Azure pro IntelliJ usnadňuje tento proces pro vývojáře v jazyce Java přidáním *publikovat jako kontejner Docker* funkce pro nasazení tooMicrosoft Azure.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-106">hello Azure Toolkit for IntelliJ simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment tooMicrosoft Azure.</span></span> <span data-ttu-id="4e6e2-107">Tento článek vás provede hello kroky požadované toopublish tooAzure vaší aplikace jako kontejnery Docker.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-107">This article walks you through hello steps required toopublish your applications tooAzure as Docker containers.</span></span>

> [!NOTE]
>
> <span data-ttu-id="4e6e2-108">Další informace o Docker je k dispozici na hello [Docker webu].</span><span class="sxs-lookup"><span data-stu-id="4e6e2-108">More information about Docker is available on hello [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a><span data-ttu-id="4e6e2-109">Publikovat tooAzure vaší webové aplikace pomocí kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="4e6e2-109">Publish your web app tooAzure by using a Docker container</span></span>

> [!NOTE]
> * <span data-ttu-id="4e6e2-110">toopublish vaší webové aplikaci, musíte vytvořit artefakt připravené pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-110">toopublish your web app, you must create a deployment-ready artifact.</span></span> <span data-ttu-id="4e6e2-111">toolearn více, viz hello [Další informace o vytváření artefakty](#artifacts) části.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-111">toolearn more, see hello [Additional information about creating artifacts](#artifacts) section.</span></span>
>
> * <span data-ttu-id="4e6e2-112">Po dokončení Průvodce nasazením hello alespoň jednou, většinu nastavení jsou použity jako výchozí, když znovu spustíte Průvodce hello.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-112">After you have completed hello deployment wizard at least once, most of your settings are used as defaults when you run hello wizard again.</span></span>
>

1. <span data-ttu-id="4e6e2-113">Otevřete projekt webové aplikace v IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-113">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="4e6e2-114">toostart hello **publikovat jako kontejner Docker** průvodce, proveďte jednu z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="4e6e2-114">toostart hello **Publish as Docker Container** wizard, do either of hello following:</span></span>

   * <span data-ttu-id="4e6e2-115">V hello **projektu** nástroj okna, klikněte pravým tlačítkem na projekt, klikněte na **Azure**a potom klikněte na **publikovat jako kontejner Docker**:</span><span class="sxs-lookup"><span data-stu-id="4e6e2-115">In hello **Project** tool window, right-click your project, click **Azure**, and then click **Publish as Docker Container**:</span></span>

      ![Hello publikovat jako příkaz kontejner Docker][PUB01]

   * <span data-ttu-id="4e6e2-117">Na panelu nástrojů IntelliJ hello, klikněte na tlačítko hello **publikovat skupiny** tlačítko a potom klikněte na **publikovat jako kontejner Docker**:</span><span class="sxs-lookup"><span data-stu-id="4e6e2-117">On hello IntelliJ toolbar, click hello **Publish Group** button, and then click **Publish as Docker Container**:</span></span>

      <span data-ttu-id="4e6e2-118">![Hello publikovat jako příkaz kontejner Docker][PUB02]</span><span class="sxs-lookup"><span data-stu-id="4e6e2-118">![hello Publish as Docker Container command][PUB02]</span></span>  
    <span data-ttu-id="4e6e2-119">Hello **nasadit kontejner Docker v Azure** otevře se průvodce.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-119">hello **Deploy Docker Container on Azure** wizard opens.</span></span>

   ![Hello kontejner Docker nasadit na Průvodce Azure][PUB03]

3. <span data-ttu-id="4e6e2-121">V hello **zadejte název bitové kopie, vyberte cestu hello artefaktů a zkontrolujte toobe hostitelů Docker používá** okně hello následující:</span><span class="sxs-lookup"><span data-stu-id="4e6e2-121">In hello **Type an image name, select hello artifact's path and check a Docker host toobe used** window, do hello following:</span></span> 

   <span data-ttu-id="4e6e2-122">a.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-122">a.</span></span> <span data-ttu-id="4e6e2-123">V hello **název bitové kopie Docker** zadejte jedinečný název pro Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-123">In hello **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="4e6e2-124">(hello průvodce automaticky vytvoří název, ale můžete ho upravit.)</span><span class="sxs-lookup"><span data-stu-id="4e6e2-124">(hello wizard automatically creates a name, but you can modify it.)</span></span> 

   <span data-ttu-id="4e6e2-125">b.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-125">b.</span></span> <span data-ttu-id="4e6e2-126">Hello **hostitele** oblasti jsou zobrazeny všechny hostitelů Docker, které jste už vytvořili.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-126">hello **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="4e6e2-127">Proveďte jednu z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="4e6e2-127">Do either of hello following:</span></span> 
      * <span data-ttu-id="4e6e2-128">Pokud máte u existujícího hostitelů Docker, můžete nasadit tooit vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-128">If you have an existing Docker host, you can deploy your web app tooit.</span></span>
      * <span data-ttu-id="4e6e2-129">toocreate Docker hostitele, klikněte na tlačítko hello zelená – znaménko plus (**+**).</span><span class="sxs-lookup"><span data-stu-id="4e6e2-129">toocreate a Docker host, click hello green plus sign (**+**).</span></span>  
       <span data-ttu-id="4e6e2-130">Hello **vytvořit hostitelů Docker** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-130">hello **Create Docker Host** dialog box opens.</span></span> 

      ![Nasadit kontejner Docker na Průvodce Azure][PUB04a]

4. <span data-ttu-id="4e6e2-132">V hello **konfigurace hello nový virtuální počítač** okno, zadejte následující informace o hostiteli Docker hello.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-132">In hello **Configure hello new virtual machine** window, provide hello following information about your Docker host.</span></span> <span data-ttu-id="4e6e2-133">(hello průvodce automaticky generuje většinu hello informace pro vás, ale můžete upravit některé z nich.)</span><span class="sxs-lookup"><span data-stu-id="4e6e2-133">(hello wizard automatically generates most of hello information for you, but you can modify any of them.)</span></span> 

   <span data-ttu-id="4e6e2-134">a.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-134">a.</span></span> <span data-ttu-id="4e6e2-135">V hello **název** zadejte jedinečný název hostitele Docker hello.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-135">In hello **Name** box, enter a unique name for hello Docker host.</span></span> <span data-ttu-id="4e6e2-136">(Není stejný jako název Docker bitové kopie, který jste dřív zadali hello hello.)</span><span class="sxs-lookup"><span data-stu-id="4e6e2-136">(It is not hello same as hello Docker image name that you specified earlier.)</span></span> 
    
   <span data-ttu-id="4e6e2-137">b.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-137">b.</span></span> <span data-ttu-id="4e6e2-138">V hello **předplatné** zadejte hello předplatné Azure, který používáte pro svého hostitele.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-138">In hello **Subscription** box, enter hello Azure subscription that you use for your host.</span></span> 
      
   <span data-ttu-id="4e6e2-139">c.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-139">c.</span></span> <span data-ttu-id="4e6e2-140">V hello **oblast** zadejte hello geografické oblasti, kde se nachází váš hostitel.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-140">In hello **Region** box, enter hello geographical region where your host is located.</span></span>
      
   <span data-ttu-id="4e6e2-141">d.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-141">d.</span></span> <span data-ttu-id="4e6e2-142">Na hello **operačního systému a velikost** kartě, hello následující:</span><span class="sxs-lookup"><span data-stu-id="4e6e2-142">On hello **OS and Size** tab, do hello following:</span></span>      
      * <span data-ttu-id="4e6e2-143">**Hostitelem OS**: Zadejte hello operačního systému pro hello virtuální počítač, který obsahuje váš hostitel.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-143">**Host OS**: Enter hello operating system for hello virtual machine that contains your host.</span></span> 
      * <span data-ttu-id="4e6e2-144">**Velikost**: Zadejte hello velikost virtuálního počítače pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-144">**Size**: Enter hello virtual-machine size for your host.</span></span>   
       
   <span data-ttu-id="4e6e2-145">e.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-145">e.</span></span> <span data-ttu-id="4e6e2-146">Na hello **skupiny prostředků** , vyberte jednu z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="4e6e2-146">On hello **Resource Group** tab, select either of hello following:</span></span>      
      * <span data-ttu-id="4e6e2-147">**Novou skupinu prostředků**: vytvoření skupiny prostředků pro svého hostitele.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-147">**New resource group**: Create a resource group for your host.</span></span>
      * <span data-ttu-id="4e6e2-148">**Existující skupinu prostředků**: Zadejte existující skupinu prostředků z účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-148">**Existing resource group**: Specify an existing resource group from your Azure account.</span></span> 
       
   <span data-ttu-id="4e6e2-149">f.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-149">f.</span></span> <span data-ttu-id="4e6e2-150">Na hello **sítě** , vyberte jednu z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="4e6e2-150">On hello **Network** tab, select either of hello following:</span></span>      
      * <span data-ttu-id="4e6e2-151">**Nové virtuální sítě**: vytvoření virtuální sítě pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-151">**New virtual network**: Create a virtual network for your host.</span></span>
      * <span data-ttu-id="4e6e2-152">**Existující virtuální síť**: Zadejte existující virtuální sítě z účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-152">**Existing virtual network**: Specify an existing virtual network from your Azure account.</span></span> 
       
   <span data-ttu-id="4e6e2-153">g.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-153">g.</span></span> <span data-ttu-id="4e6e2-154">Na hello **úložiště** , vyberte jednu z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="4e6e2-154">On hello **Storage** tab, select either of hello following:</span></span>      
      * <span data-ttu-id="4e6e2-155">**Nový účet úložiště**: vytvoření účtu úložiště pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-155">**New storage account**: Create a storage account for your host.</span></span>
      * <span data-ttu-id="4e6e2-156">**Stávající účet úložiště**: Zadejte existující účet úložiště z účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-156">**Existing storage account**: Specify an existing storage account from your Azure account.</span></span>
       
5. <span data-ttu-id="4e6e2-157">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-157">Click **Next**.</span></span>  
     <span data-ttu-id="4e6e2-158">Hello **konfiguraci protokolu v přihlašovací údaje a nastavení portu** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-158">hello **Configure log in credentials and port settings** window opens.</span></span>

      ![Konfigurace protokolu Hello v okně nastavení port a přihlašovací údaje][PUB05]

6. <span data-ttu-id="4e6e2-160">Vyberte jednu z hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="4e6e2-160">Select one of hello following options:</span></span>

      * <span data-ttu-id="4e6e2-161">**Importovat pověření z Azure Key Vault**: Zadejte dříve uloženou sadu přihlašovacích údajů, které jsou uložené ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-161">**Import credentials from Azure Key Vault**: Specify a previously saved set of credentials that are stored in your Azure subscription.</span></span>

          > [!NOTE]
          > <span data-ttu-id="4e6e2-162">Azure trezoru klíčů, který je vytvořen pomocí určitého účtu nebo instanční objekt není automaticky přístupný pro jiný účet nebo instančního objektu, který sdílí hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-162">An Azure key vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares hello subscription.</span></span> <span data-ttu-id="4e6e2-163">tooallow jiný účet nebo služby toouse hlavní klíč hello trezoru, je nutné použít hello Azure portálu tooadd hello účet nebo instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-163">tooallow another account or service principal toouse hello key vault, you must use hello Azure portal tooadd hello account or service principal.</span></span>

      * <span data-ttu-id="4e6e2-164">**Nový protokol v přihlašovacích údajích**: Vytvořte novou sadu přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-164">**New log in credentials**: Create a new set of login credentials.</span></span> <span data-ttu-id="4e6e2-165">Pokud vyberete tuto možnost, hello následující:</span><span class="sxs-lookup"><span data-stu-id="4e6e2-165">If you select this option, do hello following:</span></span>

        <span data-ttu-id="4e6e2-166">a.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-166">a.</span></span> <span data-ttu-id="4e6e2-167">Na hello **virtuálních počítačů pověření** kartě, zadejte následující informace o virtuálním počítači přihlašovací údaje hello váš hostitel Docker hello: * **uživatelské jméno**: Zadejte hello uživatelské jméno pro přihlášení vaše virtuální počítače přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-167">On hello **VM Credentials** tab, provide hello following information for hello virtual-machine login credentials of your Docker host: * **Username**: Enter hello username for your virtual-machine login credentials.</span></span>
             <span data-ttu-id="4e6e2-168">* **Heslo** a **potvrdit**: Zadejte hello heslo pro své virtuální počítače přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-168">* **Password** and **Confirm**: Enter hello password for your virtual-machine login credentials.</span></span>
             <span data-ttu-id="4e6e2-169">* **SSH**: Zadejte nastavení hello Secure Shell (SSH) pro Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-169">* **SSH**: Enter hello Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="4e6e2-170">Můžete vybrat jednu z hello následující možnosti: * **žádné**: Určuje, že virtuální počítač neumožňuje připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-170">You can select one of hello following options: * **None**: Specifies that your virtual machine does not allow SSH connections.</span></span>
                <span data-ttu-id="4e6e2-171">* **Automaticky generovat**: automaticky vytvoří hello požadavků nastavení pro připojení pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-171">* **Auto-generate**: Automatically creates hello requisite settings for connecting via SSH.</span></span>
                <span data-ttu-id="4e6e2-172">* **Import z adresáře**: vám umožní toospecify adresář, který obsahuje sadu dříve uloženou nastavení SSH.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-172">* **Import from directory**: Allows you toospecify a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="4e6e2-173">Hello directory musí obsahovat hello následující dva soubory:</span><span class="sxs-lookup"><span data-stu-id="4e6e2-173">hello directory must contain hello following two files:</span></span>
                
                  * *id_rsa*: Contains hello RSA identification for a user.
                  * *id_rsa.pub*: Contains hello RSA public key that is used for authentication.
            
        <span data-ttu-id="4e6e2-174">b.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-174">b.</span></span> <span data-ttu-id="4e6e2-175">Na hello **Docker démon přístup** kartě, zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="4e6e2-175">On hello **Docker Daemon Access** tab, provide hello following information:</span></span>

          ![Vytvořit hostitele Docker][PUB06]
    
             * **Docker Daemon port**: Enter hello unique TCP port for your Docker host.
             * **TLS Security**: Enter hello Transport Layer Security settings for your Docker host. You can choose from hello following options:
                * **None**: Specifies that your virtual machine does not allow TLS connections.
                * **Auto-generate**: Automatically creates hello requisite settings for connecting via TLS.
                * **Import from directory**: Specifies a directory that contains a set of previously saved TLS settings. hello directory must contain hello following six files: 
                   * *ca.pem* and *ca-key.pem*: Contain hello certificate and public key for hello TLS Certificate Authority.
                   * *cert.pem* and *key.pem*: Contain client certificate and public key which will be used for TLS authentication.
                   * *server.pem* and *server-key.pem*: Contain hello client certificate and public key that is used for TLS authentication.

7. <span data-ttu-id="4e6e2-177">Po zadání hello požadované informace, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-177">After you have entered hello required information, click **Finish**.</span></span>  
    <span data-ttu-id="4e6e2-178">Hello **nasadit kontejner Docker v Azure** se znovu zobrazí průvodce.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-178">hello **Deploy Docker Container on Azure** wizard reappears.</span></span>

   ![Nasadit kontejner Docker na Průvodce Azure][PUB07]

8. <span data-ttu-id="4e6e2-180">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-180">Click **Next**.</span></span>  
    <span data-ttu-id="4e6e2-181">Hello **konfigurace toobe kontejner Docker hello vytvořit** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-181">hello **Configure hello Docker container toobe created** window opens.</span></span>

   ![Hello konfigurovat hello Docker kontejneru toobe vytvořit okno][PUB08]

9. <span data-ttu-id="4e6e2-183">V hello **konfigurace toobe kontejner Docker hello vytvořit** okno, zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="4e6e2-183">In hello **Configure hello Docker container toobe created** window, provide hello following information:</span></span> 

   <span data-ttu-id="4e6e2-184">a.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-184">a.</span></span> <span data-ttu-id="4e6e2-185">V hello **název kontejneru Docker** zadejte jedinečný název pro váš kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-185">In hello **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="4e6e2-186">b.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-186">b.</span></span> <span data-ttu-id="4e6e2-187">Vyberte jednu z následujících imagí Dockeru hello:</span><span class="sxs-lookup"><span data-stu-id="4e6e2-187">Choose one of hello following Docker images:</span></span> 

      * <span data-ttu-id="4e6e2-188">**Předdefinované image Docker**: Určete existující bitovou kopii Docker z Azure.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-188">**Predefined Docker image**: Specify a pre-existing Docker image from Azure.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="4e6e2-189">Hello seznam imagí Dockeru v tomto poli se skládá z více bitových kopií této hello nástrojů Azure byla nakonfigurovaná toopatch tak, aby vaše artefaktů je nasazená automaticky.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-189">hello list of Docker images in this box consists of several images that hello Azure Toolkit has been configured toopatch so that your artifact is deployed automatically.</span></span> 

      * <span data-ttu-id="4e6e2-190">**Vlastní soubor Docker**: Zadejte předtím uložili soubor Docker ze svého místního počítače.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-190">**Custom Dockerfile**: Specify a previously saved Dockerfile from your local computer.</span></span>

        > [!NOTE]
        > <span data-ttu-id="4e6e2-191">Toto je pokročilejší funkce pro vývojáře, kteří chtějí toodeploy vlastní soubor Docker.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-191">This is a more advanced feature for developers who want toodeploy their own Dockerfile.</span></span> <span data-ttu-id="4e6e2-192">Je však až toodevelopers, kteří používat tuto možnost tooensure vytvořené jejich soubor Docker správně.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-192">However, it is up toodevelopers who use this option tooensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="4e6e2-193">Protože hello Azure Toolkit neověřuje hello obsah vlastní soubor Docker, hello nasazení může selhat, pokud má hello soubor Docker problémy.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-193">Because hello Azure Toolkit does not validate hello content in a custom Dockerfile, hello deployment might fail if hello Dockerfile has issues.</span></span> <span data-ttu-id="4e6e2-194">Kromě toho protože hello nástrojů Azure očekává hello vlastní soubor Docker toocontain artefaktem webové aplikace, pokusí tooopen připojení HTTP.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-194">In addition, because hello Azure Toolkit expects hello custom Dockerfile toocontain a web app artifact, it attempts tooopen an HTTP connection.</span></span> <span data-ttu-id="4e6e2-195">Pokud vývojáři publikovat jiného typu artefaktů, obdrží může neškodné chyby po nasazení.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-195">If developers publish a different type of artifact, they might receive innocuous errors after deployment.</span></span>

   <span data-ttu-id="4e6e2-196">c.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-196">c.</span></span> <span data-ttu-id="4e6e2-197">V hello **nastavení portu** zadejte hello jedinečnou TCP port vazbu pro váš kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-197">In hello **Port settings** box, enter hello unique TCP port binding for your Docker container.</span></span> 

10. <span data-ttu-id="4e6e2-198">Po dokončení předchozích kroků hello, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-198">After you have completed hello preceding steps, click **Finish**.</span></span> 

<span data-ttu-id="4e6e2-199">Hello Azure Toolkit zahájí nasazení vaší webové aplikace tooAzure v kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-199">hello Azure Toolkit begins deploying your web app tooAzure in a Docker container.</span></span> <span data-ttu-id="4e6e2-200">Pokud jste nakonfigurovali IntelliJ toobe nasazené v pozadí hello **nasazení tooAzure** se zobrazí indikátor průběhu.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-200">Unless you have configured IntelliJ toobe deployed in hello background, a **Deploying tooAzure** progress bar appears.</span></span> 

![indikátor průběhu nasazení Hello][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a><span data-ttu-id="4e6e2-202">Další informace o vytváření artefaktů</span><span class="sxs-lookup"><span data-stu-id="4e6e2-202">Additional information about creating artifacts</span></span>

<span data-ttu-id="4e6e2-203">toocreate artefaktem připravené pro nasazení hello následující:</span><span class="sxs-lookup"><span data-stu-id="4e6e2-203">toocreate a deployment-ready artifact, do hello following:</span></span>

1. <span data-ttu-id="4e6e2-204">Otevřete projekt webové aplikace v IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-204">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="4e6e2-205">Klikněte na tlačítko **soubor**a potom klikněte na **strukturu projektu**.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-205">Click **File**, and then click **Project Structure**.</span></span>

   ![Hello příkaz struktura projektu][ART01]

3. <span data-ttu-id="4e6e2-207">tooadd na artefaktů, klikněte na tlačítko hello zelená – znaménko plus (**+**) a potom klikněte na **webové aplikace: archivu**.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-207">tooadd an artifact, click hello green plus sign (**+**), and then click **Web Application: Archive**.</span></span>

   ![Hello příkaz "Archivu webové aplikace:"][ART02]

4. <span data-ttu-id="4e6e2-209">V hello **název** pole, zadejte název vaší artefaktů (nezahrnují hello *.war* rozšíření) a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-209">In hello **Name** box, enter a name for your artifact (do not include hello *.war* extension), and then click **OK**.</span></span>

   ![pole Název artefaktů Hello][ART03]

<span data-ttu-id="4e6e2-211">Další informace o vytváření artefakty v IntelliJ najdete v tématu [konfigurace artefakty] na webu JetBrains hello.</span><span class="sxs-lookup"><span data-stu-id="4e6e2-211">For more information about creating artifacts in IntelliJ, see [Configuring artifacts] on hello JetBrains website.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4e6e2-212">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4e6e2-212">Next steps</span></span>
<span data-ttu-id="4e6e2-213">Další informace o hello sadách Azure pro integrovaného vývojového prostředí Java najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="4e6e2-213">For more information about hello Azure Toolkits for Java IDEs, see hello following resources:</span></span>

* <span data-ttu-id="4e6e2-214">[Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="4e6e2-214">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="4e6e2-215">[Co je nového v hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="4e6e2-215">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="4e6e2-216">[Instalace hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="4e6e2-216">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="4e6e2-217">[Přihlášení pokyny pro hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="4e6e2-217">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="4e6e2-218">[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]</span><span class="sxs-lookup"><span data-stu-id="4e6e2-218">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="4e6e2-219">[Sada Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="4e6e2-219">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="4e6e2-220">[Co je nového v hello nástrojů Azure pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="4e6e2-220">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="4e6e2-221">[Instalace hello Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="4e6e2-221">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="4e6e2-222">[Přihlášení pokyny pro hello Azure Toolkit IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="4e6e2-222">[Sign-in instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="4e6e2-223">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="4e6e2-223">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="4e6e2-224">Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java] a hello [Java nástrojů pro Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="4e6e2-224">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="4e6e2-225">Další zdroje pro Docker najdete v tématu hello oficiální [Docker webu].</span><span class="sxs-lookup"><span data-stu-id="4e6e2-225">For additional resources for Docker, see hello official [Docker website].</span></span>

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace hello Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Přihlášení pokyny pro hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Přihlášení pokyny pro hello Azure Toolkit IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Co je nového v hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co je nového v hello nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/

[Docker webu]: https://www.docker.com/
[konfigurace artefakty]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[PUB01]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB01.png
[PUB02]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB02.png
[PUB03]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB03.png
[PUB04a]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04a.png
[PUB04b]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04b.png
[PUB04c]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04c.png
[PUB04d]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04d.png
[PUB05]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB05.png
[PUB06]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB06.png
[PUB07]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB07.png
[PUB08]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB08.png
[PUB09]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB09.png

[ART01]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART01.png
[ART02]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART02.png
[ART03]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART03.png
