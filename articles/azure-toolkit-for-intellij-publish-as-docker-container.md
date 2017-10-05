---
title: "Publikovat kontejner Docker pomocí nástrojů Azure pro IntelliJ | Microsoft Docs"
description: "Zjistěte, jak publikovat webovou aplikaci do služby Microsoft Azure jako kontejner Docker pomocí nástrojů Azure pro IntelliJ."
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
ms.openlocfilehash: 96680319a6c4c0f0a4673cd6303a5b172f428797
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="374f9-103">Publikování webové aplikace pomocí nástrojů Azure pro IntelliJ jako kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="374f9-103">Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="374f9-104">Kontejnery docker jsou často používaný metodu pro nasazení webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="374f9-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="374f9-105">Pomocí Docker kontejnery vývojáři zvládá všechny své soubory projektu a závislosti do jednoho balíčku pro nasazení na server.</span><span class="sxs-lookup"><span data-stu-id="374f9-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="374f9-106">Sada nástrojů Azure pro IntelliJ usnadňuje tento proces pro vývojáře v jazyce Java přidáním *publikovat jako kontejner Docker* funkcí pro nasazení do služby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="374f9-106">The Azure Toolkit for IntelliJ simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment to Microsoft Azure.</span></span> <span data-ttu-id="374f9-107">Tento článek vás provede kroky potřebné k publikování aplikací do Azure jako kontejnery Docker.</span><span class="sxs-lookup"><span data-stu-id="374f9-107">This article walks you through the steps required to publish your applications to Azure as Docker containers.</span></span>

> [!NOTE]
>
> <span data-ttu-id="374f9-108">Další informace o Docker je k dispozici na [Docker webu].</span><span class="sxs-lookup"><span data-stu-id="374f9-108">More information about Docker is available on the [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="374f9-109">Publikování webové aplikace do Azure pomocí kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="374f9-109">Publish your web app to Azure by using a Docker container</span></span>

> [!NOTE]
> * <span data-ttu-id="374f9-110">K publikování webové aplikace, musíte vytvořit artefakt připravené pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="374f9-110">To publish your web app, you must create a deployment-ready artifact.</span></span> <span data-ttu-id="374f9-111">Další informace najdete v tématu [Další informace o vytváření artefakty](#artifacts) části.</span><span class="sxs-lookup"><span data-stu-id="374f9-111">To learn more, see the [Additional information about creating artifacts](#artifacts) section.</span></span>
>
> * <span data-ttu-id="374f9-112">Po dokončení Průvodce nasazením alespoň jednou, většinu nastavení jsou použity jako výchozí, když spustíte průvodce znovu.</span><span class="sxs-lookup"><span data-stu-id="374f9-112">After you have completed the deployment wizard at least once, most of your settings are used as defaults when you run the wizard again.</span></span>
>

1. <span data-ttu-id="374f9-113">Otevřete projekt webové aplikace v IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="374f9-113">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="374f9-114">Spuštění **publikovat jako kontejner Docker** průvodce, proveďte jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="374f9-114">To start the **Publish as Docker Container** wizard, do either of the following:</span></span>

   * <span data-ttu-id="374f9-115">V **projektu** nástroj okna, klikněte pravým tlačítkem na projekt, klikněte na **Azure**a potom klikněte na **publikovat jako kontejner Docker**:</span><span class="sxs-lookup"><span data-stu-id="374f9-115">In the **Project** tool window, right-click your project, click **Azure**, and then click **Publish as Docker Container**:</span></span>

      ![Publikovat jako příkaz kontejner Docker][PUB01]

   * <span data-ttu-id="374f9-117">Na panelu nástrojů IntelliJ, klikněte na tlačítko **publikovat skupiny** tlačítko a potom klikněte na **publikovat jako kontejner Docker**:</span><span class="sxs-lookup"><span data-stu-id="374f9-117">On the IntelliJ toolbar, click the **Publish Group** button, and then click **Publish as Docker Container**:</span></span>

      <span data-ttu-id="374f9-118">![Publikovat jako příkaz kontejner Docker][PUB02]</span><span class="sxs-lookup"><span data-stu-id="374f9-118">![The Publish as Docker Container command][PUB02]</span></span>  
    <span data-ttu-id="374f9-119">**Nasadit kontejner Docker v Azure** otevře se průvodce.</span><span class="sxs-lookup"><span data-stu-id="374f9-119">The **Deploy Docker Container on Azure** wizard opens.</span></span>

   ![Kontejner Docker nasadit na Průvodce Azure][PUB03]

3. <span data-ttu-id="374f9-121">V **zadejte název bitové kopie, vyberte artefaktů cestu a zkontrolujte hostitelů Docker má být použit** okno, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="374f9-121">In the **Type an image name, select the artifact's path and check a Docker host to be used** window, do the following:</span></span> 

   <span data-ttu-id="374f9-122">a.</span><span class="sxs-lookup"><span data-stu-id="374f9-122">a.</span></span> <span data-ttu-id="374f9-123">V **název bitové kopie Docker** zadejte jedinečný název pro Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="374f9-123">In the **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="374f9-124">(Průvodce automaticky vytvoří název, ale můžete ho upravit.)</span><span class="sxs-lookup"><span data-stu-id="374f9-124">(The wizard automatically creates a name, but you can modify it.)</span></span> 

   <span data-ttu-id="374f9-125">b.</span><span class="sxs-lookup"><span data-stu-id="374f9-125">b.</span></span> <span data-ttu-id="374f9-126">**Hostitele** oblasti jsou zobrazeny všechny hostitelů Docker, které jste už vytvořili.</span><span class="sxs-lookup"><span data-stu-id="374f9-126">The **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="374f9-127">Proveďte jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="374f9-127">Do either of the following:</span></span> 
      * <span data-ttu-id="374f9-128">Pokud máte u existujícího hostitelů Docker, můžete nasadit webové aplikace k němu.</span><span class="sxs-lookup"><span data-stu-id="374f9-128">If you have an existing Docker host, you can deploy your web app to it.</span></span>
      * <span data-ttu-id="374f9-129">Chcete-li vytvořit Docker hostitele, klikněte na tlačítko zeleného znaménka (**+**).</span><span class="sxs-lookup"><span data-stu-id="374f9-129">To create a Docker host, click the green plus sign (**+**).</span></span>  
       <span data-ttu-id="374f9-130">**Vytvořit hostitelů Docker** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="374f9-130">The **Create Docker Host** dialog box opens.</span></span> 

      ![Nasadit kontejner Docker na Průvodce Azure][PUB04a]

4. <span data-ttu-id="374f9-132">V **nakonfigurovat nový virtuální počítač** okno, zadejte následující informace o Docker hostiteli.</span><span class="sxs-lookup"><span data-stu-id="374f9-132">In the **Configure the new virtual machine** window, provide the following information about your Docker host.</span></span> <span data-ttu-id="374f9-133">(Průvodce automaticky generuje většinu informací pro vás, ale můžete upravit některé z nich.)</span><span class="sxs-lookup"><span data-stu-id="374f9-133">(The wizard automatically generates most of the information for you, but you can modify any of them.)</span></span> 

   <span data-ttu-id="374f9-134">a.</span><span class="sxs-lookup"><span data-stu-id="374f9-134">a.</span></span> <span data-ttu-id="374f9-135">V **název** zadejte jedinečný název pro Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="374f9-135">In the **Name** box, enter a unique name for the Docker host.</span></span> <span data-ttu-id="374f9-136">(Není stejný jako název Docker bitové kopie, který jste dřív zadali.)</span><span class="sxs-lookup"><span data-stu-id="374f9-136">(It is not the same as the Docker image name that you specified earlier.)</span></span> 
    
   <span data-ttu-id="374f9-137">b.</span><span class="sxs-lookup"><span data-stu-id="374f9-137">b.</span></span> <span data-ttu-id="374f9-138">V **předplatné** zadejte předplatné Azure, který používáte pro svého hostitele.</span><span class="sxs-lookup"><span data-stu-id="374f9-138">In the **Subscription** box, enter the Azure subscription that you use for your host.</span></span> 
      
   <span data-ttu-id="374f9-139">c.</span><span class="sxs-lookup"><span data-stu-id="374f9-139">c.</span></span> <span data-ttu-id="374f9-140">V **oblast** zadejte geografické oblasti, kde se nachází váš hostitel.</span><span class="sxs-lookup"><span data-stu-id="374f9-140">In the **Region** box, enter the geographical region where your host is located.</span></span>
      
   <span data-ttu-id="374f9-141">d.</span><span class="sxs-lookup"><span data-stu-id="374f9-141">d.</span></span> <span data-ttu-id="374f9-142">Na **operačního systému a velikost** kartě, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="374f9-142">On the **OS and Size** tab, do the following:</span></span>      
      * <span data-ttu-id="374f9-143">**Hostitelem OS**: Zadejte operační systém pro virtuální počítač, který obsahuje váš hostitel.</span><span class="sxs-lookup"><span data-stu-id="374f9-143">**Host OS**: Enter the operating system for the virtual machine that contains your host.</span></span> 
      * <span data-ttu-id="374f9-144">**Velikost**: Zadejte velikost virtuálního počítače pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="374f9-144">**Size**: Enter the virtual-machine size for your host.</span></span>   
       
   <span data-ttu-id="374f9-145">e.</span><span class="sxs-lookup"><span data-stu-id="374f9-145">e.</span></span> <span data-ttu-id="374f9-146">Na **skupiny prostředků** , vyberte z následujících:</span><span class="sxs-lookup"><span data-stu-id="374f9-146">On the **Resource Group** tab, select either of the following:</span></span>      
      * <span data-ttu-id="374f9-147">**Novou skupinu prostředků**: vytvoření skupiny prostředků pro svého hostitele.</span><span class="sxs-lookup"><span data-stu-id="374f9-147">**New resource group**: Create a resource group for your host.</span></span>
      * <span data-ttu-id="374f9-148">**Existující skupinu prostředků**: Zadejte existující skupinu prostředků z účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="374f9-148">**Existing resource group**: Specify an existing resource group from your Azure account.</span></span> 
       
   <span data-ttu-id="374f9-149">f.</span><span class="sxs-lookup"><span data-stu-id="374f9-149">f.</span></span> <span data-ttu-id="374f9-150">Na **sítě** , vyberte z následujících:</span><span class="sxs-lookup"><span data-stu-id="374f9-150">On the **Network** tab, select either of the following:</span></span>      
      * <span data-ttu-id="374f9-151">**Nové virtuální sítě**: vytvoření virtuální sítě pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="374f9-151">**New virtual network**: Create a virtual network for your host.</span></span>
      * <span data-ttu-id="374f9-152">**Existující virtuální síť**: Zadejte existující virtuální sítě z účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="374f9-152">**Existing virtual network**: Specify an existing virtual network from your Azure account.</span></span> 
       
   <span data-ttu-id="374f9-153">g.</span><span class="sxs-lookup"><span data-stu-id="374f9-153">g.</span></span> <span data-ttu-id="374f9-154">Na **úložiště** , vyberte z následujících:</span><span class="sxs-lookup"><span data-stu-id="374f9-154">On the **Storage** tab, select either of the following:</span></span>      
      * <span data-ttu-id="374f9-155">**Nový účet úložiště**: vytvoření účtu úložiště pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="374f9-155">**New storage account**: Create a storage account for your host.</span></span>
      * <span data-ttu-id="374f9-156">**Stávající účet úložiště**: Zadejte existující účet úložiště z účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="374f9-156">**Existing storage account**: Specify an existing storage account from your Azure account.</span></span>
       
5. <span data-ttu-id="374f9-157">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="374f9-157">Click **Next**.</span></span>  
     <span data-ttu-id="374f9-158">**Konfiguraci protokolu v přihlašovací údaje a nastavení portu** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="374f9-158">The **Configure log in credentials and port settings** window opens.</span></span>

      ![Konfigurace protokolů v okně nastavení port a přihlašovací údaje][PUB05]

6. <span data-ttu-id="374f9-160">Vyberte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="374f9-160">Select one of the following options:</span></span>

      * <span data-ttu-id="374f9-161">**Importovat pověření z Azure Key Vault**: Zadejte dříve uloženou sadu přihlašovacích údajů, které jsou uložené ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="374f9-161">**Import credentials from Azure Key Vault**: Specify a previously saved set of credentials that are stored in your Azure subscription.</span></span>

          > [!NOTE]
          > <span data-ttu-id="374f9-162">Azure trezoru klíčů, který je vytvořen pomocí určitého účtu nebo instanční objekt není automaticky přístupný pro jiný účet nebo instančního objektu, který sdílí předplatné.</span><span class="sxs-lookup"><span data-stu-id="374f9-162">An Azure key vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares the subscription.</span></span> <span data-ttu-id="374f9-163">Povolit hlavní trezoru klíčů použijte jiný účet nebo služby, vyžaduje použití portálu Azure přidat účet nebo instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="374f9-163">To allow another account or service principal to use the key vault, you must use the Azure portal to add the account or service principal.</span></span>

      * <span data-ttu-id="374f9-164">**Nový protokol v přihlašovacích údajích**: Vytvořte novou sadu přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="374f9-164">**New log in credentials**: Create a new set of login credentials.</span></span> <span data-ttu-id="374f9-165">Pokud vyberete tuto možnost, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="374f9-165">If you select this option, do the following:</span></span>

        <span data-ttu-id="374f9-166">a.</span><span class="sxs-lookup"><span data-stu-id="374f9-166">a.</span></span> <span data-ttu-id="374f9-167">Na **virtuálních počítačů pověření** kartě, zadejte následující informace pro virtuální počítač přihlašovací údaje pro váš hostitel Docker: * **uživatelské jméno**: Zadejte uživatelské jméno pro své virtuální počítače přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="374f9-167">On the **VM Credentials** tab, provide the following information for the virtual-machine login credentials of your Docker host: * **Username**: Enter the username for your virtual-machine login credentials.</span></span>
             <span data-ttu-id="374f9-168">* **Heslo** a **potvrdit**: Zadejte heslo pro své virtuální počítače přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="374f9-168">* **Password** and **Confirm**: Enter the password for your virtual-machine login credentials.</span></span>
             <span data-ttu-id="374f9-169">* **SSH**: Zadejte nastavení Secure Shell (SSH) pro Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="374f9-169">* **SSH**: Enter the Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="374f9-170">Můžete vybrat jednu z následujících možností: * **žádné**: Určuje, že virtuální počítač neumožňuje připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="374f9-170">You can select one of the following options: * **None**: Specifies that your virtual machine does not allow SSH connections.</span></span>
                <span data-ttu-id="374f9-171">* **Automaticky generovat**: automaticky vytvoří požadavků nastavení pro připojení pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="374f9-171">* **Auto-generate**: Automatically creates the requisite settings for connecting via SSH.</span></span>
                <span data-ttu-id="374f9-172">* **Import z adresáře**: můžete zadat adresář, který obsahuje sadu dříve uloženou nastavení SSH.</span><span class="sxs-lookup"><span data-stu-id="374f9-172">* **Import from directory**: Allows you to specify a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="374f9-173">Adresář musí obsahovat následující dva soubory:</span><span class="sxs-lookup"><span data-stu-id="374f9-173">The directory must contain the following two files:</span></span>
                
                  * *id_rsa*: Contains the RSA identification for a user.
                  * *id_rsa.pub*: Contains the RSA public key that is used for authentication.
            
        <span data-ttu-id="374f9-174">b.</span><span class="sxs-lookup"><span data-stu-id="374f9-174">b.</span></span> <span data-ttu-id="374f9-175">Na **Docker démon přístup** kartě, zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="374f9-175">On the **Docker Daemon Access** tab, provide the following information:</span></span>

          ![Vytvořit hostitele Docker][PUB06]
    
             * **Docker Daemon port**: Enter the unique TCP port for your Docker host.
             * **TLS Security**: Enter the Transport Layer Security settings for your Docker host. You can choose from the following options:
                * **None**: Specifies that your virtual machine does not allow TLS connections.
                * **Auto-generate**: Automatically creates the requisite settings for connecting via TLS.
                * **Import from directory**: Specifies a directory that contains a set of previously saved TLS settings. The directory must contain the following six files: 
                   * *ca.pem* and *ca-key.pem*: Contain the certificate and public key for the TLS Certificate Authority.
                   * *cert.pem* and *key.pem*: Contain client certificate and public key which will be used for TLS authentication.
                   * *server.pem* and *server-key.pem*: Contain the client certificate and public key that is used for TLS authentication.

7. <span data-ttu-id="374f9-177">Po zadání požadovaných informací klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="374f9-177">After you have entered the required information, click **Finish**.</span></span>  
    <span data-ttu-id="374f9-178">**Nasadit kontejner Docker v Azure** se znovu zobrazí průvodce.</span><span class="sxs-lookup"><span data-stu-id="374f9-178">The **Deploy Docker Container on Azure** wizard reappears.</span></span>

   ![Nasadit kontejner Docker na Průvodce Azure][PUB07]

8. <span data-ttu-id="374f9-180">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="374f9-180">Click **Next**.</span></span>  
    <span data-ttu-id="374f9-181">**Konfigurace kontejner Docker vytvořit** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="374f9-181">The **Configure the Docker container to be created** window opens.</span></span>

   ![Konfigurovat kontejner Docker vytvořit okno][PUB08]

9. <span data-ttu-id="374f9-183">V **konfigurace kontejner Docker vytvořit** okno, zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="374f9-183">In the **Configure the Docker container to be created** window, provide the following information:</span></span> 

   <span data-ttu-id="374f9-184">a.</span><span class="sxs-lookup"><span data-stu-id="374f9-184">a.</span></span> <span data-ttu-id="374f9-185">V **název kontejneru Docker** zadejte jedinečný název pro váš kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="374f9-185">In the **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="374f9-186">b.</span><span class="sxs-lookup"><span data-stu-id="374f9-186">b.</span></span> <span data-ttu-id="374f9-187">Vyberte jednu z následujících imagí Dockeru:</span><span class="sxs-lookup"><span data-stu-id="374f9-187">Choose one of the following Docker images:</span></span> 

      * <span data-ttu-id="374f9-188">**Předdefinované image Docker**: Určete existující bitovou kopii Docker z Azure.</span><span class="sxs-lookup"><span data-stu-id="374f9-188">**Predefined Docker image**: Specify a pre-existing Docker image from Azure.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="374f9-189">Seznam imagí Dockeru v tomto poli se skládá z několika bitové kopie, které Azure Toolkit nakonfigurován, aby opravit tak, aby vaše artefaktů je nasazená automaticky.</span><span class="sxs-lookup"><span data-stu-id="374f9-189">The list of Docker images in this box consists of several images that the Azure Toolkit has been configured to patch so that your artifact is deployed automatically.</span></span> 

      * <span data-ttu-id="374f9-190">**Vlastní soubor Docker**: Zadejte předtím uložili soubor Docker ze svého místního počítače.</span><span class="sxs-lookup"><span data-stu-id="374f9-190">**Custom Dockerfile**: Specify a previously saved Dockerfile from your local computer.</span></span>

        > [!NOTE]
        > <span data-ttu-id="374f9-191">Toto je pokročilejší funkce pro vývojáře, kteří chtějí nasadit vlastní soubor Docker.</span><span class="sxs-lookup"><span data-stu-id="374f9-191">This is a more advanced feature for developers who want to deploy their own Dockerfile.</span></span> <span data-ttu-id="374f9-192">Je však až vývojáře, kteří tuto možnost použijte k zajištění, že jejich soubor Docker správně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="374f9-192">However, it is up to developers who use this option to ensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="374f9-193">Protože Azure Toolkit neověřuje obsah vlastní soubor Docker, nasazení může selhat, pokud má soubor Docker problémy.</span><span class="sxs-lookup"><span data-stu-id="374f9-193">Because the Azure Toolkit does not validate the content in a custom Dockerfile, the deployment might fail if the Dockerfile has issues.</span></span> <span data-ttu-id="374f9-194">Kromě toho protože sada nástrojů Azure očekává vlastní soubor Docker tak, aby obsahovala artefaktem webové aplikace, se pokusí otevřít připojení HTTP.</span><span class="sxs-lookup"><span data-stu-id="374f9-194">In addition, because the Azure Toolkit expects the custom Dockerfile to contain a web app artifact, it attempts to open an HTTP connection.</span></span> <span data-ttu-id="374f9-195">Pokud vývojáři publikovat jiného typu artefaktů, obdrží může neškodné chyby po nasazení.</span><span class="sxs-lookup"><span data-stu-id="374f9-195">If developers publish a different type of artifact, they might receive innocuous errors after deployment.</span></span>

   <span data-ttu-id="374f9-196">c.</span><span class="sxs-lookup"><span data-stu-id="374f9-196">c.</span></span> <span data-ttu-id="374f9-197">V **nastavení portu** zadejte jedinečnou vazbu port TCP pro váš kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="374f9-197">In the **Port settings** box, enter the unique TCP port binding for your Docker container.</span></span> 

10. <span data-ttu-id="374f9-198">Po dokončení předchozího postupu, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="374f9-198">After you have completed the preceding steps, click **Finish**.</span></span> 

<span data-ttu-id="374f9-199">Sada nástrojů Azure zahájí nasazení webové aplikace do Azure na kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="374f9-199">The Azure Toolkit begins deploying your web app to Azure in a Docker container.</span></span> <span data-ttu-id="374f9-200">Pokud jste nakonfigurovali IntelliJ k nasazení na pozadí **nasazení do Azure** se zobrazí indikátor průběhu.</span><span class="sxs-lookup"><span data-stu-id="374f9-200">Unless you have configured IntelliJ to be deployed in the background, a **Deploying to Azure** progress bar appears.</span></span> 

![Indikátor průběhu nasazení][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a><span data-ttu-id="374f9-202">Další informace o vytváření artefaktů</span><span class="sxs-lookup"><span data-stu-id="374f9-202">Additional information about creating artifacts</span></span>

<span data-ttu-id="374f9-203">Pokud chcete vytvořit artefakt připravené pro nasazení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="374f9-203">To create a deployment-ready artifact, do the following:</span></span>

1. <span data-ttu-id="374f9-204">Otevřete projekt webové aplikace v IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="374f9-204">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="374f9-205">Klikněte na tlačítko **soubor**a potom klikněte na **strukturu projektu**.</span><span class="sxs-lookup"><span data-stu-id="374f9-205">Click **File**, and then click **Project Structure**.</span></span>

   ![Příkaz struktura projektu][ART01]

3. <span data-ttu-id="374f9-207">Chcete-li přidat artefakt, klikněte na tlačítko zeleného znaménka (**+**) a potom klikněte na **webové aplikace: archivu**.</span><span class="sxs-lookup"><span data-stu-id="374f9-207">To add an artifact, click the green plus sign (**+**), and then click **Web Application: Archive**.</span></span>

   ![Příkaz "Archivu webové aplikace:"][ART02]

4. <span data-ttu-id="374f9-209">V **název** pole, zadejte název vaší artefaktů (nezahrnují *.war* rozšíření) a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="374f9-209">In the **Name** box, enter a name for your artifact (do not include the *.war* extension), and then click **OK**.</span></span>

   ![Pole Název artefaktů][ART03]

<span data-ttu-id="374f9-211">Další informace o vytváření artefakty v IntelliJ najdete v tématu [konfigurace artefakty] na webu JetBrains.</span><span class="sxs-lookup"><span data-stu-id="374f9-211">For more information about creating artifacts in IntelliJ, see [Configuring artifacts] on the JetBrains website.</span></span>

## <a name="next-steps"></a><span data-ttu-id="374f9-212">Další kroky</span><span class="sxs-lookup"><span data-stu-id="374f9-212">Next steps</span></span>
<span data-ttu-id="374f9-213">Další informace o sadách Azure pro integrovaného vývojového prostředí Java najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="374f9-213">For more information about the Azure Toolkits for Java IDEs, see the following resources:</span></span>

* <span data-ttu-id="374f9-214">[Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="374f9-214">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="374f9-215">[Co je nového v sadě nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="374f9-215">[What's new in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="374f9-216">[Instalace sady Azure Toolkit pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="374f9-216">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="374f9-217">[Pokyny přihlášení k Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="374f9-217">[Sign-in instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="374f9-218">[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]</span><span class="sxs-lookup"><span data-stu-id="374f9-218">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="374f9-219">[Sada Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="374f9-219">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="374f9-220">[Co je nového v sadě nástrojů Azure pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="374f9-220">[What's new in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="374f9-221">[Instalace sady Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="374f9-221">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="374f9-222">[Přihlášení pokyny pro Azure Toolkit IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="374f9-222">[Sign-in instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="374f9-223">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="374f9-223">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="374f9-224">Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java] a [Java Tools for Visual Studio Team Services] (Nástroje Java pro Visual Studio Team Services).</span><span class="sxs-lookup"><span data-stu-id="374f9-224">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="374f9-225">Další zdroje pro Docker, najdete v oficiální [Docker webu].</span><span class="sxs-lookup"><span data-stu-id="374f9-225">For additional resources for Docker, see the official [Docker website].</span></span>

<!-- URL List -->

<span data-ttu-id="374f9-226">[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="374f9-226">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="374f9-227">[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="374f9-227">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="374f9-228">[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="374f9-228">[Create a Hello World web app for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="374f9-229">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="374f9-229">[Create a Hello World web app for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="374f9-230">[Instalace sady Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="374f9-230">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="374f9-231">[Instalace sady Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="374f9-231">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="374f9-232">[Pokyny přihlášení k Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="374f9-232">[Sign-in instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
<span data-ttu-id="374f9-233">[Přihlášení pokyny pro Azure Toolkit IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="374f9-233">[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="374f9-234">[Co je nového v sadě nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="374f9-234">[What's new in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="374f9-235">[Co je nového v sadě nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="374f9-235">[What's new in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="374f9-236">[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="374f9-236">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="374f9-237">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="374f9-237">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>

<span data-ttu-id="374f9-238">[Docker webu]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="374f9-238">[Docker website]: https://www.docker.com/</span></span>
<span data-ttu-id="374f9-239">[konfigurace artefakty]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html</span><span class="sxs-lookup"><span data-stu-id="374f9-239">[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html</span></span>

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
