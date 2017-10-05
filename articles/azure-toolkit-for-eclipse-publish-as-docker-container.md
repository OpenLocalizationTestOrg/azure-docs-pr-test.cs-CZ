---
title: "Publikovat kontejner Docker pomocí nástrojů Azure pro Eclipse | Microsoft Docs"
description: "Zjistěte, jak publikovat webovou aplikaci do služby Microsoft Azure jako kontejner Docker pomocí nástrojů Azure pro Eclipse."
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
ms.openlocfilehash: 746bb0a073396b84fa8592adda6748a2a5692ee8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="aa68d-103">Publikovat webovou aplikaci jako kontejner Docker pomocí nástrojů Azure pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="aa68d-103">Publish a web app as a Docker container by using the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="aa68d-104">Kontejnery docker jsou často používaný metodu pro nasazení webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="aa68d-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="aa68d-105">Pomocí Docker kontejnery vývojáři zvládá všechny své soubory projektu a závislosti do jednoho balíčku pro nasazení na server.</span><span class="sxs-lookup"><span data-stu-id="aa68d-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="aa68d-106">Sada nástrojů Azure pro Eclipse usnadňuje tento proces pro vývojáře v jazyce Java přidáním *publikovat jako kontejner Docker* funkcí pro nasazení do služby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="aa68d-106">The Azure Toolkit for Eclipse simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment to Microsoft Azure.</span></span> <span data-ttu-id="aa68d-107">Tento článek vás provede kroky potřebné k publikování aplikací do Azure jako kontejnery Docker.</span><span class="sxs-lookup"><span data-stu-id="aa68d-107">This article walks you through the steps required to publish your applications to Azure as Docker containers.</span></span>

> [!NOTE]
> <span data-ttu-id="aa68d-108">Další informace o Docker je k dispozici na [Docker webu].</span><span class="sxs-lookup"><span data-stu-id="aa68d-108">More information about Docker is available on the [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="aa68d-109">Publikování webové aplikace do Azure pomocí kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="aa68d-109">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="aa68d-110">Otevření projektu webové aplikace v prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="aa68d-110">Open your web app project in Eclipse.</span></span>

2. <span data-ttu-id="aa68d-111">Spuštění **publikovat jako kontejner Docker** průvodce, proveďte jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="aa68d-111">To start the **Publish as Docker Container** wizard, do either of the following:</span></span>

   * <span data-ttu-id="aa68d-112">V **Navigátor** zobrazení, klikněte pravým tlačítkem na projekt, klikněte na tlačítko **Azure**a potom klikněte na **publikovat jako kontejner Docker**.</span><span class="sxs-lookup"><span data-stu-id="aa68d-112">In the **Navigator** view, right-click your project, click **Azure**, and then click **Publish as Docker Container**.</span></span>

      ![Navigátor zobrazení publikovat jako příkaz kontejner Docker][PUB01]

   * <span data-ttu-id="aa68d-114">Na panelu nástrojů Eclipse klikněte na **publikovat** tlačítko a potom klikněte na **publikovat jako kontejner Docker**.</span><span class="sxs-lookup"><span data-stu-id="aa68d-114">On the Eclipse toolbar, click the **Publish** button, and then click **Publish as Docker Container**.</span></span>

      ![Panel nástrojů Eclipse publikovat jako příkaz kontejner Docker][PUB02]
      
    <span data-ttu-id="aa68d-116">**Nasadit kontejner Docker v Azure** otevře se průvodce.</span><span class="sxs-lookup"><span data-stu-id="aa68d-116">The **Deploy Docker Container on Azure** wizard opens.</span></span>

    ![Kontejner Docker nasadit na Průvodce Azure][PUB03]

3. <span data-ttu-id="aa68d-118">V **zadejte název bitové kopie, vyberte artefaktů cestu a zkontrolujte hostitelů Docker má být použit** okno, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="aa68d-118">In the **Type an image name, select the artifact's path and check a Docker host to be used** window, do the following:</span></span>

    <span data-ttu-id="aa68d-119">a.</span><span class="sxs-lookup"><span data-stu-id="aa68d-119">a.</span></span> <span data-ttu-id="aa68d-120">V **název bitové kopie Docker** zadejte jedinečný název pro Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="aa68d-120">In the **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="aa68d-121">(Průvodce automaticky vytvoří název, ale můžete ho upravit.)</span><span class="sxs-lookup"><span data-stu-id="aa68d-121">(The wizard automatically creates a name, but you can modify it.)</span></span>

    <span data-ttu-id="aa68d-122">b.</span><span class="sxs-lookup"><span data-stu-id="aa68d-122">b.</span></span> <span data-ttu-id="aa68d-123">**Hostitele** oblasti jsou zobrazeny všechny hostitelů Docker, které jste už vytvořili.</span><span class="sxs-lookup"><span data-stu-id="aa68d-123">The **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="aa68d-124">Proveďte jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="aa68d-124">Do either of the following:</span></span>

    * <span data-ttu-id="aa68d-125">Pokud máte u existujícího hostitelů Docker, můžete nasadit webové aplikace k němu.</span><span class="sxs-lookup"><span data-stu-id="aa68d-125">If you have an existing Docker host, you can deploy your web app to it.</span></span>
    * <span data-ttu-id="aa68d-126">Chcete-li vytvořit nového hostitele Docker, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="aa68d-126">To create a new Docker host, click **Add**.</span></span>  
      
    <span data-ttu-id="aa68d-127">**Vytvořit hostitelů Docker** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="aa68d-127">The **Create Docker Host** dialog box opens.</span></span>

    ![Nasadit kontejner Docker na Průvodce Azure][PUB04a]

4. <span data-ttu-id="aa68d-129">V **nakonfigurovat nový virtuální počítač** okně určete následující možnosti pro Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="aa68d-129">In the **Configure the new virtual machine** window, specify the following options for your Docker host.</span></span> <span data-ttu-id="aa68d-130">(Průvodce automaticky vygeneruje většina možností pro vás, ale můžete upravit některé z nich.)</span><span class="sxs-lookup"><span data-stu-id="aa68d-130">(The wizard automatically generates most of the options for you, but you can modify any of them.)</span></span>

   <span data-ttu-id="aa68d-131">a.</span><span class="sxs-lookup"><span data-stu-id="aa68d-131">a.</span></span> <span data-ttu-id="aa68d-132">**Název**: Zadejte jedinečný název pro Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="aa68d-132">**Name**: Enter a unique name for the Docker host.</span></span> <span data-ttu-id="aa68d-133">(Není stejný jako název Docker bitové kopie, který jste dřív zadali.)</span><span class="sxs-lookup"><span data-stu-id="aa68d-133">(It is not the same as the Docker image name that you specified earlier.)</span></span>

   <span data-ttu-id="aa68d-134">b.</span><span class="sxs-lookup"><span data-stu-id="aa68d-134">b.</span></span> <span data-ttu-id="aa68d-135">**Předplatné**: Zadejte předplatné Azure, který používáte pro svého hostitele.</span><span class="sxs-lookup"><span data-stu-id="aa68d-135">**Subscription**: Enter the Azure subscription that you use for your host.</span></span>

   <span data-ttu-id="aa68d-136">c.</span><span class="sxs-lookup"><span data-stu-id="aa68d-136">c.</span></span> <span data-ttu-id="aa68d-137">**Oblast**: Zadejte geografické oblasti, kde se nachází váš hostitel.</span><span class="sxs-lookup"><span data-stu-id="aa68d-137">**Region**: Enter the geographical region where your host is located.</span></span>

   <span data-ttu-id="aa68d-138">d.</span><span class="sxs-lookup"><span data-stu-id="aa68d-138">d.</span></span> <span data-ttu-id="aa68d-139">Na **hostitelským operačním systémem a velikost** karty:</span><span class="sxs-lookup"><span data-stu-id="aa68d-139">On the **Host OS and Size** tab:</span></span>
     * <span data-ttu-id="aa68d-140">**Hostitelem OS**: Zadejte operační systém pro virtuální počítač, který obsahuje váš hostitel.</span><span class="sxs-lookup"><span data-stu-id="aa68d-140">**Host OS**: Enter the operating system for the virtual machine that contains your host.</span></span>
     * <span data-ttu-id="aa68d-141">**Velikost**: Zadejte velikost virtuálního počítače pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="aa68d-141">**Size**: Enter the virtual-machine size for your host.</span></span>

   <span data-ttu-id="aa68d-142">e.</span><span class="sxs-lookup"><span data-stu-id="aa68d-142">e.</span></span> <span data-ttu-id="aa68d-143">Na **skupiny prostředků** karty:</span><span class="sxs-lookup"><span data-stu-id="aa68d-143">On the **Resource Group** tab:</span></span>
     * <span data-ttu-id="aa68d-144">**Novou skupinu prostředků**: Vytvořte novou skupinu prostředků pro svého hostitele.</span><span class="sxs-lookup"><span data-stu-id="aa68d-144">**New resource group**: Create a new resource group for your host.</span></span>
     * <span data-ttu-id="aa68d-145">**Existující skupinu prostředků**: Zadejte existující skupinu prostředků z účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="aa68d-145">**Existing resource group**: Enter an existing resource group from your Azure account.</span></span>

   <span data-ttu-id="aa68d-146">f.</span><span class="sxs-lookup"><span data-stu-id="aa68d-146">f.</span></span> <span data-ttu-id="aa68d-147">Na **sítě** karty:</span><span class="sxs-lookup"><span data-stu-id="aa68d-147">On the **Network** tab:</span></span>
     * <span data-ttu-id="aa68d-148">**Nové virtuální sítě**: vytvoření nové virtuální sítě pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="aa68d-148">**New virtual network**: Create a new virtual network for your host.</span></span>
     * <span data-ttu-id="aa68d-149">**Existující virtuální síť**: Zadejte existující virtuální síť od účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="aa68d-149">**Existing virtual network**: Enter an existing virtual network from your Azure account.</span></span>

   <span data-ttu-id="aa68d-150">g.</span><span class="sxs-lookup"><span data-stu-id="aa68d-150">g.</span></span> <span data-ttu-id="aa68d-151">Na **úložiště** karty:</span><span class="sxs-lookup"><span data-stu-id="aa68d-151">On the **Storage** tab:</span></span>
     * <span data-ttu-id="aa68d-152">**Nový účet úložiště**: Vytvořte nový účet úložiště pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="aa68d-152">**New storage account**: Create a new storage account for your host.</span></span>
     * <span data-ttu-id="aa68d-153">**Stávající účet úložiště**: Zadejte existující účet úložiště z účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="aa68d-153">**Existing storage account**: Enter an existing storage account from your Azure account.</span></span>

5. <span data-ttu-id="aa68d-154">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="aa68d-154">Click **Next**.</span></span>

6. <span data-ttu-id="aa68d-155">V **konfiguraci protokolu v přihlašovací údaje a nastavení portu** okno, vyberte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="aa68d-155">In the **Configure log in credentials and port settings** window, select one of the following options:</span></span>

    * <span data-ttu-id="aa68d-156">**Importovat pověření z Azure Key Vault**: Určuje dříve uloženou sadu přihlašovacích údajů, které jsou uložené ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="aa68d-156">**Import credentials from Azure Key Vault**: Specifies a previously saved set of credentials that are stored in your Azure subscription.</span></span>

      >[!NOTE]
      ><span data-ttu-id="aa68d-157">Azure Key Vault, vytvořený pomocí určitého účtu nebo instanční objekt není automaticky přístupný pro jiný účet nebo instančního objektu, který sdílí předplatné.</span><span class="sxs-lookup"><span data-stu-id="aa68d-157">An Azure Key Vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares the subscription.</span></span> <span data-ttu-id="aa68d-158">Povolit jiný účet nebo služby hlavní používat Key Vault, vyžaduje použití portálu Azure přidat účet nebo instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="aa68d-158">To allow another account or service principal to use the Key Vault, you must use the Azure portal to add the account or service principal.</span></span>

    * <span data-ttu-id="aa68d-159">**Nový protokol v přihlašovacích údajích**: vytvoří novou sadu přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="aa68d-159">**New log in credentials**: Creates a new set of login credentials.</span></span> <span data-ttu-id="aa68d-160">Pokud vyberete tuto možnost, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="aa68d-160">If you select this option, do the following:</span></span>
    
      * <span data-ttu-id="aa68d-161">Na **pověření virtuálních počítačů** , zvolte jednu z následujících možností pro virtuální počítač přihlašovací údaje vaší Docker hostitele:</span><span class="sxs-lookup"><span data-stu-id="aa68d-161">On the **VM Credentials** tab, choose one of the following options for the virtual-machine login credentials of your Docker host:</span></span>

          * <span data-ttu-id="aa68d-162">**Uživatelské jméno**: Zadejte uživatelské jméno pro své přihlašovací údaje virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="aa68d-162">**Username**: Enter the username for your virtual machine login credentials.</span></span>
          * <span data-ttu-id="aa68d-163">**Heslo** a **potvrdit**: Zadejte heslo pro své přihlašovací údaje virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="aa68d-163">**Password** and **Confirm**: Enter the password for your virtual machine login credentials.</span></span>
          * <span data-ttu-id="aa68d-164">**SSH**: Zadejte nastavení Secure Shell (SSH) pro Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="aa68d-164">**SSH**: Enter the Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="aa68d-165">Můžete zvolit z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="aa68d-165">You can choose from the following options:</span></span>
            * <span data-ttu-id="aa68d-166">**Žádný**: Určuje, že virtuální počítač nebude povolovat připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="aa68d-166">**None**: Specifies that your virtual machine will not allow SSH connections.</span></span>
            * <span data-ttu-id="aa68d-167">**Automaticky generovat**: automaticky vytvoří požadavků nastavení pro připojení pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="aa68d-167">**Auto-generate**: Automatically creates the requisite settings for connecting via SSH.</span></span>
            * <span data-ttu-id="aa68d-168">**Import z adresáře**: Určuje adresář, který obsahuje sadu dříve uloženou nastavení SSH.</span><span class="sxs-lookup"><span data-stu-id="aa68d-168">**Import from directory**: Specifies a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="aa68d-169">Adresář musí obsahovat následující dva soubory:</span><span class="sxs-lookup"><span data-stu-id="aa68d-169">The directory must contain the following two files:</span></span>
                * <span data-ttu-id="aa68d-170">*id_rsa*: obsahuje identifikační RSA pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="aa68d-170">*id_rsa*: Contains the RSA identification for a user.</span></span>
                * <span data-ttu-id="aa68d-171">*id_rsa.pub*: obsahuje veřejný klíč RSA, který se používá k ověřování.</span><span class="sxs-lookup"><span data-stu-id="aa68d-171">*id_rsa.pub*: Contains the RSA public key that is used for authentication.</span></span>
        
        ![Vytvořit hostitele Docker][PUB05]

      * <span data-ttu-id="aa68d-173">Na **Docker démon pověření** určete následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="aa68d-173">On the **Docker Daemon Credentials** tab, specify the following options:</span></span>

          * <span data-ttu-id="aa68d-174">**Port démon docker**: Zadejte jedinečný port TCP pro Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="aa68d-174">**Docker Daemon port**: Enter the unique TCP port for your Docker host.</span></span>
          * <span data-ttu-id="aa68d-175">**Zabezpečení TLS**: Zadejte Transport Layer Security nastavení pro Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="aa68d-175">**TLS Security**: Enter the Transport Layer Security settings for your Docker host.</span></span> <span data-ttu-id="aa68d-176">Můžete zvolit z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="aa68d-176">You can choose from the following options:</span></span>
            * <span data-ttu-id="aa68d-177">**Žádný**: Určuje, že virtuální počítač nebude povolovat připojení protokol TLS.</span><span class="sxs-lookup"><span data-stu-id="aa68d-177">**None**: Specifies that your virtual machine will not allow TLS connections.</span></span>
            * <span data-ttu-id="aa68d-178">**Automaticky generovat**: automaticky vytvoří požadavků nastavení pro připojení prostřednictvím protokolu TLS.</span><span class="sxs-lookup"><span data-stu-id="aa68d-178">**Auto-generate**: Automatically creates the requisite settings for connecting via TLS.</span></span>
            * <span data-ttu-id="aa68d-179">**Import z adresáře**: Určuje adresář, který obsahuje sadu dříve uloženou nastavení TLS.</span><span class="sxs-lookup"><span data-stu-id="aa68d-179">**Import from directory**: Specifies a directory that contains a set of previously saved TLS settings.</span></span> <span data-ttu-id="aa68d-180">Přesněji řečeno adresář musí obsahovat následující šesti soubory:</span><span class="sxs-lookup"><span data-stu-id="aa68d-180">More specifically, the directory must contain the following six files:</span></span>
                * <span data-ttu-id="aa68d-181">*CA.pem* a *certifikační autority key.pem*: obsahovat certifikát a veřejný klíč pro certifikační autority TLS.</span><span class="sxs-lookup"><span data-stu-id="aa68d-181">*ca.pem* and *ca-key.pem*: Contain the certificate and public key for the TLS Certificate Authority.</span></span>
                * <span data-ttu-id="aa68d-182">*cert.pem* a *key.pem*: obsahovat klientský certifikát a veřejný klíč, který se používá k ověřování TLS.</span><span class="sxs-lookup"><span data-stu-id="aa68d-182">*cert.pem* and *key.pem*: Contain the client certificate and public key that is used for TLS authentication.</span></span>
                * <span data-ttu-id="aa68d-183">*Server.pem* a *server key.pem*: obsahovat serverový certifikát a veřejný klíč pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="aa68d-183">*server.pem* and *server-key.pem*: Contain the server certificate and public key for the host.</span></span>

        ![Vytvořit hostitele Docker][PUB06]

7. <span data-ttu-id="aa68d-185">Po zadání všechny předchozí informace, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="aa68d-185">After you have entered all of the preceding information, click **Finish**.</span></span>

8. <span data-ttu-id="aa68d-186">V **nasadit kontejner Docker v Azure** průvodce, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="aa68d-186">In the **Deploy Docker Container on Azure** wizard, click **Next**.</span></span>

   ![Kontejner Docker nasadit na Průvodce Azure][PUB07]

9. <span data-ttu-id="aa68d-188">V **konfigurace kontejner Docker vytvořit** okno, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="aa68d-188">In the **Configure the Docker container to be created** window, do the following:</span></span>

   <span data-ttu-id="aa68d-189">a.</span><span class="sxs-lookup"><span data-stu-id="aa68d-189">a.</span></span> <span data-ttu-id="aa68d-190">V **název kontejneru Docker** zadejte jedinečný název pro váš kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="aa68d-190">In the **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="aa68d-191">b.</span><span class="sxs-lookup"><span data-stu-id="aa68d-191">b.</span></span> <span data-ttu-id="aa68d-192">Vyberte jednu z následujících imagí Dockeru:</span><span class="sxs-lookup"><span data-stu-id="aa68d-192">Choose one of the following Docker images:</span></span>
     * <span data-ttu-id="aa68d-193">**Předdefinované image Docker**: Určuje existující bitovou kopii Docker z Azure.</span><span class="sxs-lookup"><span data-stu-id="aa68d-193">**Predefined Docker image**: Specifies a pre-existing Docker image from Azure.</span></span>

       >[!NOTE]
       ><span data-ttu-id="aa68d-194">Seznam imagí Dockeru v tomto poli se skládá z několika bitové kopie, které Azure Toolkit nakonfigurován, aby opravit tak, aby vaše artefaktů je nasazená automaticky.</span><span class="sxs-lookup"><span data-stu-id="aa68d-194">The list of Docker images in this box consists of several images that the Azure Toolkit has been configured to patch so that your artifact is deployed automatically.</span></span>

     * <span data-ttu-id="aa68d-195">**Vlastní soubor Docker**: Určuje předtím uložili soubor Docker ze svého místního počítače.</span><span class="sxs-lookup"><span data-stu-id="aa68d-195">**Custom Dockerfile**: Specifies a previously saved Dockerfile from your local computer.</span></span>

       >[!NOTE]
       ><span data-ttu-id="aa68d-196">Toto je pokročilejší funkce pro vývojáře, kteří chtějí nasadit vlastní soubor Docker.</span><span class="sxs-lookup"><span data-stu-id="aa68d-196">This is a more advanced feature for developers who want to deploy their own Dockerfile.</span></span> <span data-ttu-id="aa68d-197">Je však až vývojáře, kteří tuto možnost použijte k zajištění, že jejich soubor Docker správně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="aa68d-197">However, it is up to developers who use this option to ensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="aa68d-198">Sada nástrojů Azure neověřuje obsah vlastní soubor Docker, tak nasazení může selhat, pokud soubor Docker má problémy.</span><span class="sxs-lookup"><span data-stu-id="aa68d-198">The Azure Toolkit does not validate the content in a custom Dockerfile, so the deployment might fail if the Dockerfile has issues.</span></span> <span data-ttu-id="aa68d-199">Kromě toho sada nástrojů Azure očekává vlastní soubor Docker tak, aby obsahovala artefaktem webové aplikace a pokusí se otevřít připojení HTTP.</span><span class="sxs-lookup"><span data-stu-id="aa68d-199">In addition, the Azure Toolkit expects the custom Dockerfile to contain a web app artifact, and it will attempt to open an HTTP connection.</span></span> <span data-ttu-id="aa68d-200">Pokud vývojáři publikovat jiného typu artefaktů, můžou získat neškodné chyby po nasazení.</span><span class="sxs-lookup"><span data-stu-id="aa68d-200">If developers publish a different type of artifact, they may receive innocuous errors after deployment.</span></span>

   <span data-ttu-id="aa68d-201">c.</span><span class="sxs-lookup"><span data-stu-id="aa68d-201">c.</span></span> <span data-ttu-id="aa68d-202">**Nastavení portu**: Zadejte jedinečnou vazbu port TCP pro váš kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="aa68d-202">**Port settings**: Enter the unique TCP port binding for your Docker container.</span></span>

     ![Konfigurovat kontejner Docker vytvořit okno][PUB08]

10. <span data-ttu-id="aa68d-204">Po dokončení všech předchozích kroků, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="aa68d-204">After you have completed all of the preceding steps, click **Finish**.</span></span>

<span data-ttu-id="aa68d-205">Sada nástrojů Azure zahájí nasazení webové aplikace do Azure na kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="aa68d-205">The Azure Toolkit begins deploying your web app to Azure in a Docker container.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="aa68d-206">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aa68d-206">Next steps</span></span>
<span data-ttu-id="aa68d-207">Další informace o sadách Azure pro integrovaného vývojového prostředí Java najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="aa68d-207">For more information about the Azure Toolkits for Java IDEs, see the following resources:</span></span>

* <span data-ttu-id="aa68d-208">[Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="aa68d-208">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="aa68d-209">[Co je nového v sadě nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="aa68d-209">[What's new in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="aa68d-210">[Instalace sady Azure Toolkit pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="aa68d-210">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="aa68d-211">[Pokyny přihlášení k Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="aa68d-211">[Sign-in instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="aa68d-212">[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]</span><span class="sxs-lookup"><span data-stu-id="aa68d-212">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="aa68d-213">[Sada Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="aa68d-213">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="aa68d-214">[Co je nového v sadě nástrojů Azure pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="aa68d-214">[What's new in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="aa68d-215">[Instalace sady Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="aa68d-215">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="aa68d-216">[Přihlášení pokyny pro Azure Toolkit IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="aa68d-216">[Sign-in instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="aa68d-217">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="aa68d-217">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="aa68d-218">Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java] a [Java nástrojů pro Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="aa68d-218">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="aa68d-219">Další zdroje pro Docker, najdete v oficiální [Docker webu].</span><span class="sxs-lookup"><span data-stu-id="aa68d-219">For additional resources for Docker, see the official [Docker website].</span></span>

<!-- URL List -->

<span data-ttu-id="aa68d-220">[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="aa68d-220">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="aa68d-221">[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="aa68d-221">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="aa68d-222">[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="aa68d-222">[Create a Hello World web app for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="aa68d-223">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="aa68d-223">[Create a Hello World web app for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="aa68d-224">[Instalace sady Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="aa68d-224">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="aa68d-225">[Instalace sady Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="aa68d-225">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="aa68d-226">[Pokyny přihlášení k Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="aa68d-226">[Sign-in instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
<span data-ttu-id="aa68d-227">[Přihlášení pokyny pro Azure Toolkit IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="aa68d-227">[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="aa68d-228">[Co je nového v sadě nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="aa68d-228">[What's new in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="aa68d-229">[Co je nového v sadě nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="aa68d-229">[What's new in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="aa68d-230">[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="aa68d-230">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="aa68d-231">[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="aa68d-231">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>

<span data-ttu-id="aa68d-232">[Docker webu]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="aa68d-232">[Docker website]: https://www.docker.com/</span></span>

<!-- IMG List -->

[PUB01]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB01.png
[PUB02]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB02.png
[PUB03]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB03.png
[PUB04a]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04a.png
[PUB04b]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04b.png
[PUB04c]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04c.png
[PUB04d]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04d.png
[PUB05]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB05.png
[PUB06]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB06.png
[PUB07]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB07.png
[PUB08]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB08.png