---
title: "hello aaaPublish kontejner Docker pomocí nástrojů Azure pro Eclipse | Microsoft Docs"
description: "Zjistěte, jak hello toopublish tooMicrosoft webové aplikace Azure jako kontejner Docker pomocí nástrojů Azure pro prostředí Eclipse."
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
ms.openlocfilehash: 53ec3a7f7a171691024e03622fd48d6f1e257b50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-hello-azure-toolkit-for-eclipse"></a><span data-ttu-id="960a9-103">Publikovat webovou aplikaci jako kontejner Docker pomocí hello Azure Toolkit pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="960a9-103">Publish a web app as a Docker container by using hello Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="960a9-104">Kontejnery docker jsou často používaný metodu pro nasazení webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="960a9-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="960a9-105">Pomocí Docker kontejnery vývojáři zvládá všechny své soubory projektu a závislosti do jednoho balíčku pro nasazení tooa server.</span><span class="sxs-lookup"><span data-stu-id="960a9-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment tooa server.</span></span> <span data-ttu-id="960a9-106">Hello nástrojů Azure pro prostředí Eclipse usnadňuje tento proces pro vývojáře v jazyce Java přidáním *publikovat jako kontejner Docker* funkce pro nasazení tooMicrosoft Azure.</span><span class="sxs-lookup"><span data-stu-id="960a9-106">hello Azure Toolkit for Eclipse simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment tooMicrosoft Azure.</span></span> <span data-ttu-id="960a9-107">Tento článek vás provede hello kroky požadované toopublish tooAzure vaší aplikace jako kontejnery Docker.</span><span class="sxs-lookup"><span data-stu-id="960a9-107">This article walks you through hello steps required toopublish your applications tooAzure as Docker containers.</span></span>

> [!NOTE]
> <span data-ttu-id="960a9-108">Další informace o Docker je k dispozici na hello [Docker webu].</span><span class="sxs-lookup"><span data-stu-id="960a9-108">More information about Docker is available on hello [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a><span data-ttu-id="960a9-109">Publikovat tooAzure vaší webové aplikace pomocí kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="960a9-109">Publish your web app tooAzure by using a Docker container</span></span>

1. <span data-ttu-id="960a9-110">Otevření projektu webové aplikace v prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="960a9-110">Open your web app project in Eclipse.</span></span>

2. <span data-ttu-id="960a9-111">toostart hello **publikovat jako kontejner Docker** průvodce, proveďte jednu z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="960a9-111">toostart hello **Publish as Docker Container** wizard, do either of hello following:</span></span>

   * <span data-ttu-id="960a9-112">V hello **Navigátor** zobrazení, klikněte pravým tlačítkem na projekt, klikněte na tlačítko **Azure**a potom klikněte na **publikovat jako kontejner Docker**.</span><span class="sxs-lookup"><span data-stu-id="960a9-112">In hello **Navigator** view, right-click your project, click **Azure**, and then click **Publish as Docker Container**.</span></span>

      ![Navigátor zobrazení publikovat jako příkaz kontejner Docker][PUB01]

   * <span data-ttu-id="960a9-114">Na panelu nástrojů Eclipse hello, klikněte na tlačítko hello **publikovat** tlačítko a potom klikněte na **publikovat jako kontejner Docker**.</span><span class="sxs-lookup"><span data-stu-id="960a9-114">On hello Eclipse toolbar, click hello **Publish** button, and then click **Publish as Docker Container**.</span></span>

      ![Panel nástrojů Eclipse publikovat jako příkaz kontejner Docker][PUB02]
      
    <span data-ttu-id="960a9-116">Hello **nasadit kontejner Docker v Azure** otevře se průvodce.</span><span class="sxs-lookup"><span data-stu-id="960a9-116">hello **Deploy Docker Container on Azure** wizard opens.</span></span>

    ![Hello kontejner Docker nasadit na Průvodce Azure][PUB03]

3. <span data-ttu-id="960a9-118">V hello **zadejte název bitové kopie, vyberte cestu hello artefaktů a zkontrolujte toobe hostitelů Docker používá** okně hello následující:</span><span class="sxs-lookup"><span data-stu-id="960a9-118">In hello **Type an image name, select hello artifact's path and check a Docker host toobe used** window, do hello following:</span></span>

    <span data-ttu-id="960a9-119">a.</span><span class="sxs-lookup"><span data-stu-id="960a9-119">a.</span></span> <span data-ttu-id="960a9-120">V hello **název bitové kopie Docker** zadejte jedinečný název pro Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="960a9-120">In hello **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="960a9-121">(hello průvodce automaticky vytvoří název, ale můžete ho upravit.)</span><span class="sxs-lookup"><span data-stu-id="960a9-121">(hello wizard automatically creates a name, but you can modify it.)</span></span>

    <span data-ttu-id="960a9-122">b.</span><span class="sxs-lookup"><span data-stu-id="960a9-122">b.</span></span> <span data-ttu-id="960a9-123">Hello **hostitele** oblasti jsou zobrazeny všechny hostitelů Docker, které jste už vytvořili.</span><span class="sxs-lookup"><span data-stu-id="960a9-123">hello **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="960a9-124">Proveďte jednu z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="960a9-124">Do either of hello following:</span></span>

    * <span data-ttu-id="960a9-125">Pokud máte u existujícího hostitelů Docker, můžete nasadit tooit vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="960a9-125">If you have an existing Docker host, you can deploy your web app tooit.</span></span>
    * <span data-ttu-id="960a9-126">Klikněte na tlačítko toocreate na nového hostitele Docker **přidat**.</span><span class="sxs-lookup"><span data-stu-id="960a9-126">toocreate a new Docker host, click **Add**.</span></span>  
      
    <span data-ttu-id="960a9-127">Hello **vytvořit hostitelů Docker** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="960a9-127">hello **Create Docker Host** dialog box opens.</span></span>

    ![Nasadit kontejner Docker na Průvodce Azure][PUB04a]

4. <span data-ttu-id="960a9-129">V hello **konfigurace hello nový virtuální počítač** okno, zadejte následující možnosti pro svého hostitele Docker hello.</span><span class="sxs-lookup"><span data-stu-id="960a9-129">In hello **Configure hello new virtual machine** window, specify hello following options for your Docker host.</span></span> <span data-ttu-id="960a9-130">(hello průvodce automaticky generuje většina možností hello za vás, ale můžete upravit některé z nich.)</span><span class="sxs-lookup"><span data-stu-id="960a9-130">(hello wizard automatically generates most of hello options for you, but you can modify any of them.)</span></span>

   <span data-ttu-id="960a9-131">a.</span><span class="sxs-lookup"><span data-stu-id="960a9-131">a.</span></span> <span data-ttu-id="960a9-132">**Název**: Zadejte jedinečný název pro hello Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="960a9-132">**Name**: Enter a unique name for hello Docker host.</span></span> <span data-ttu-id="960a9-133">(Není stejný jako název Docker bitové kopie, který jste dřív zadali hello hello.)</span><span class="sxs-lookup"><span data-stu-id="960a9-133">(It is not hello same as hello Docker image name that you specified earlier.)</span></span>

   <span data-ttu-id="960a9-134">b.</span><span class="sxs-lookup"><span data-stu-id="960a9-134">b.</span></span> <span data-ttu-id="960a9-135">**Předplatné**: Zadejte hello předplatné Azure, který používáte pro svého hostitele.</span><span class="sxs-lookup"><span data-stu-id="960a9-135">**Subscription**: Enter hello Azure subscription that you use for your host.</span></span>

   <span data-ttu-id="960a9-136">c.</span><span class="sxs-lookup"><span data-stu-id="960a9-136">c.</span></span> <span data-ttu-id="960a9-137">**Oblast**: Zadejte hello geografické oblasti, kde se nachází váš hostitel.</span><span class="sxs-lookup"><span data-stu-id="960a9-137">**Region**: Enter hello geographical region where your host is located.</span></span>

   <span data-ttu-id="960a9-138">d.</span><span class="sxs-lookup"><span data-stu-id="960a9-138">d.</span></span> <span data-ttu-id="960a9-139">Na hello **hostitelským operačním systémem a velikost** karty:</span><span class="sxs-lookup"><span data-stu-id="960a9-139">On hello **Host OS and Size** tab:</span></span>
     * <span data-ttu-id="960a9-140">**Hostitelem OS**: Zadejte hello operačního systému pro hello virtuální počítač, který obsahuje váš hostitel.</span><span class="sxs-lookup"><span data-stu-id="960a9-140">**Host OS**: Enter hello operating system for hello virtual machine that contains your host.</span></span>
     * <span data-ttu-id="960a9-141">**Velikost**: Zadejte hello velikost virtuálního počítače pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="960a9-141">**Size**: Enter hello virtual-machine size for your host.</span></span>

   <span data-ttu-id="960a9-142">e.</span><span class="sxs-lookup"><span data-stu-id="960a9-142">e.</span></span> <span data-ttu-id="960a9-143">Na hello **skupiny prostředků** karty:</span><span class="sxs-lookup"><span data-stu-id="960a9-143">On hello **Resource Group** tab:</span></span>
     * <span data-ttu-id="960a9-144">**Novou skupinu prostředků**: Vytvořte novou skupinu prostředků pro svého hostitele.</span><span class="sxs-lookup"><span data-stu-id="960a9-144">**New resource group**: Create a new resource group for your host.</span></span>
     * <span data-ttu-id="960a9-145">**Existující skupinu prostředků**: Zadejte existující skupinu prostředků z účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="960a9-145">**Existing resource group**: Enter an existing resource group from your Azure account.</span></span>

   <span data-ttu-id="960a9-146">f.</span><span class="sxs-lookup"><span data-stu-id="960a9-146">f.</span></span> <span data-ttu-id="960a9-147">Na hello **sítě** karty:</span><span class="sxs-lookup"><span data-stu-id="960a9-147">On hello **Network** tab:</span></span>
     * <span data-ttu-id="960a9-148">**Nové virtuální sítě**: vytvoření nové virtuální sítě pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="960a9-148">**New virtual network**: Create a new virtual network for your host.</span></span>
     * <span data-ttu-id="960a9-149">**Existující virtuální síť**: Zadejte existující virtuální síť od účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="960a9-149">**Existing virtual network**: Enter an existing virtual network from your Azure account.</span></span>

   <span data-ttu-id="960a9-150">g.</span><span class="sxs-lookup"><span data-stu-id="960a9-150">g.</span></span> <span data-ttu-id="960a9-151">Na hello **úložiště** karty:</span><span class="sxs-lookup"><span data-stu-id="960a9-151">On hello **Storage** tab:</span></span>
     * <span data-ttu-id="960a9-152">**Nový účet úložiště**: Vytvořte nový účet úložiště pro hostitele.</span><span class="sxs-lookup"><span data-stu-id="960a9-152">**New storage account**: Create a new storage account for your host.</span></span>
     * <span data-ttu-id="960a9-153">**Stávající účet úložiště**: Zadejte existující účet úložiště z účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="960a9-153">**Existing storage account**: Enter an existing storage account from your Azure account.</span></span>

5. <span data-ttu-id="960a9-154">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="960a9-154">Click **Next**.</span></span>

6. <span data-ttu-id="960a9-155">V hello **konfiguraci protokolu v přihlašovací údaje a nastavení portu** okno, vyberte jednu z hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="960a9-155">In hello **Configure log in credentials and port settings** window, select one of hello following options:</span></span>

    * <span data-ttu-id="960a9-156">**Importovat pověření z Azure Key Vault**: Určuje dříve uloženou sadu přihlašovacích údajů, které jsou uložené ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="960a9-156">**Import credentials from Azure Key Vault**: Specifies a previously saved set of credentials that are stored in your Azure subscription.</span></span>

      >[!NOTE]
      ><span data-ttu-id="960a9-157">Azure Key Vault, vytvořený pomocí určitého účtu nebo instanční objekt není automaticky přístupný pro jiný účet nebo instančního objektu, který sdílí hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="960a9-157">An Azure Key Vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares hello subscription.</span></span> <span data-ttu-id="960a9-158">tooallow jiný účet nebo služby hlavní toouse hello Key Vault, je nutné použít účet Azure portálu tooadd hello hello nebo instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="960a9-158">tooallow another account or service principal toouse hello Key Vault, you must use hello Azure portal tooadd hello account or service principal.</span></span>

    * <span data-ttu-id="960a9-159">**Nový protokol v přihlašovacích údajích**: vytvoří novou sadu přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="960a9-159">**New log in credentials**: Creates a new set of login credentials.</span></span> <span data-ttu-id="960a9-160">Pokud vyberete tuto možnost, hello následující:</span><span class="sxs-lookup"><span data-stu-id="960a9-160">If you select this option, do hello following:</span></span>
    
      * <span data-ttu-id="960a9-161">Na hello **virtuálních počítačů pověření** , zvolte jednu z následujících hello možností hello virtuálnímu počítači přihlašovací údaje vaší Docker hostitele:</span><span class="sxs-lookup"><span data-stu-id="960a9-161">On hello **VM Credentials** tab, choose one of hello following options for hello virtual-machine login credentials of your Docker host:</span></span>

          * <span data-ttu-id="960a9-162">**Uživatelské jméno**: Zadejte uživatelské jméno hello pro své přihlašovací údaje virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="960a9-162">**Username**: Enter hello username for your virtual machine login credentials.</span></span>
          * <span data-ttu-id="960a9-163">**Heslo** a **potvrdit**: Zadejte hello heslo pro své přihlašovací údaje virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="960a9-163">**Password** and **Confirm**: Enter hello password for your virtual machine login credentials.</span></span>
          * <span data-ttu-id="960a9-164">**SSH**: Zadejte nastavení hello Secure Shell (SSH) pro Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="960a9-164">**SSH**: Enter hello Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="960a9-165">Můžete vybrat z hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="960a9-165">You can choose from hello following options:</span></span>
            * <span data-ttu-id="960a9-166">**Žádný**: Určuje, že virtuální počítač nebude povolovat připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="960a9-166">**None**: Specifies that your virtual machine will not allow SSH connections.</span></span>
            * <span data-ttu-id="960a9-167">**Automaticky generovat**: automaticky vytvoří hello požadavků nastavení pro připojení pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="960a9-167">**Auto-generate**: Automatically creates hello requisite settings for connecting via SSH.</span></span>
            * <span data-ttu-id="960a9-168">**Import z adresáře**: Určuje adresář, který obsahuje sadu dříve uloženou nastavení SSH.</span><span class="sxs-lookup"><span data-stu-id="960a9-168">**Import from directory**: Specifies a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="960a9-169">Hello directory musí obsahovat hello následující dva soubory:</span><span class="sxs-lookup"><span data-stu-id="960a9-169">hello directory must contain hello following two files:</span></span>
                * <span data-ttu-id="960a9-170">*id_rsa*: obsahuje identifikační hello RSA pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="960a9-170">*id_rsa*: Contains hello RSA identification for a user.</span></span>
                * <span data-ttu-id="960a9-171">*id_rsa.pub*: obsahuje hello veřejným klíčem RSA používaný pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="960a9-171">*id_rsa.pub*: Contains hello RSA public key that is used for authentication.</span></span>
        
        ![Vytvořit hostitele Docker][PUB05]

      * <span data-ttu-id="960a9-173">Na hello **Docker démon pověření** zadejte hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="960a9-173">On hello **Docker Daemon Credentials** tab, specify hello following options:</span></span>

          * <span data-ttu-id="960a9-174">**Port démon docker**: Zadejte hello jedinečný port TCP pro Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="960a9-174">**Docker Daemon port**: Enter hello unique TCP port for your Docker host.</span></span>
          * <span data-ttu-id="960a9-175">**Zabezpečení TLS**: Zadejte hello Transport Layer Security nastavení pro Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="960a9-175">**TLS Security**: Enter hello Transport Layer Security settings for your Docker host.</span></span> <span data-ttu-id="960a9-176">Můžete vybrat z hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="960a9-176">You can choose from hello following options:</span></span>
            * <span data-ttu-id="960a9-177">**Žádný**: Určuje, že virtuální počítač nebude povolovat připojení protokol TLS.</span><span class="sxs-lookup"><span data-stu-id="960a9-177">**None**: Specifies that your virtual machine will not allow TLS connections.</span></span>
            * <span data-ttu-id="960a9-178">**Automaticky generovat**: automaticky vytvoří hello požadavků nastavení pro připojení prostřednictvím protokolu TLS.</span><span class="sxs-lookup"><span data-stu-id="960a9-178">**Auto-generate**: Automatically creates hello requisite settings for connecting via TLS.</span></span>
            * <span data-ttu-id="960a9-179">**Import z adresáře**: Určuje adresář, který obsahuje sadu dříve uloženou nastavení TLS.</span><span class="sxs-lookup"><span data-stu-id="960a9-179">**Import from directory**: Specifies a directory that contains a set of previously saved TLS settings.</span></span> <span data-ttu-id="960a9-180">Přesněji řečeno hello directory musí obsahovat následující soubory šesti hello:</span><span class="sxs-lookup"><span data-stu-id="960a9-180">More specifically, hello directory must contain hello following six files:</span></span>
                * <span data-ttu-id="960a9-181">*CA.pem* a *certifikační autority key.pem*: obsahovat hello certifikát a veřejný klíč pro hello TLS certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="960a9-181">*ca.pem* and *ca-key.pem*: Contain hello certificate and public key for hello TLS Certificate Authority.</span></span>
                * <span data-ttu-id="960a9-182">*cert.pem* a *key.pem*: obsahovat hello klientský certifikát a veřejný klíč, který se používá k ověřování TLS.</span><span class="sxs-lookup"><span data-stu-id="960a9-182">*cert.pem* and *key.pem*: Contain hello client certificate and public key that is used for TLS authentication.</span></span>
                * <span data-ttu-id="960a9-183">*Server.pem* a *server key.pem*: obsahovat hello serveru certifikát a veřejný klíč pro hostitele hello.</span><span class="sxs-lookup"><span data-stu-id="960a9-183">*server.pem* and *server-key.pem*: Contain hello server certificate and public key for hello host.</span></span>

        ![Vytvořit hostitele Docker][PUB06]

7. <span data-ttu-id="960a9-185">Po zadání všech hello předcházející informace, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="960a9-185">After you have entered all of hello preceding information, click **Finish**.</span></span>

8. <span data-ttu-id="960a9-186">V hello **nasadit kontejner Docker v Azure** průvodce, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="960a9-186">In hello **Deploy Docker Container on Azure** wizard, click **Next**.</span></span>

   ![Hello kontejner Docker nasadit na Průvodce Azure][PUB07]

9. <span data-ttu-id="960a9-188">V hello **konfigurace toobe kontejner Docker hello vytvořit** okně hello následující:</span><span class="sxs-lookup"><span data-stu-id="960a9-188">In hello **Configure hello Docker container toobe created** window, do hello following:</span></span>

   <span data-ttu-id="960a9-189">a.</span><span class="sxs-lookup"><span data-stu-id="960a9-189">a.</span></span> <span data-ttu-id="960a9-190">V hello **název kontejneru Docker** zadejte jedinečný název pro váš kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="960a9-190">In hello **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="960a9-191">b.</span><span class="sxs-lookup"><span data-stu-id="960a9-191">b.</span></span> <span data-ttu-id="960a9-192">Vyberte jednu z následujících imagí Dockeru hello:</span><span class="sxs-lookup"><span data-stu-id="960a9-192">Choose one of hello following Docker images:</span></span>
     * <span data-ttu-id="960a9-193">**Předdefinované image Docker**: Určuje existující bitovou kopii Docker z Azure.</span><span class="sxs-lookup"><span data-stu-id="960a9-193">**Predefined Docker image**: Specifies a pre-existing Docker image from Azure.</span></span>

       >[!NOTE]
       ><span data-ttu-id="960a9-194">Hello seznam imagí Dockeru v tomto poli se skládá z více bitových kopií této hello nástrojů Azure byla nakonfigurovaná toopatch tak, aby vaše artefaktů je nasazená automaticky.</span><span class="sxs-lookup"><span data-stu-id="960a9-194">hello list of Docker images in this box consists of several images that hello Azure Toolkit has been configured toopatch so that your artifact is deployed automatically.</span></span>

     * <span data-ttu-id="960a9-195">**Vlastní soubor Docker**: Určuje předtím uložili soubor Docker ze svého místního počítače.</span><span class="sxs-lookup"><span data-stu-id="960a9-195">**Custom Dockerfile**: Specifies a previously saved Dockerfile from your local computer.</span></span>

       >[!NOTE]
       ><span data-ttu-id="960a9-196">Toto je pokročilejší funkce pro vývojáře, kteří chtějí toodeploy vlastní soubor Docker.</span><span class="sxs-lookup"><span data-stu-id="960a9-196">This is a more advanced feature for developers who want toodeploy their own Dockerfile.</span></span> <span data-ttu-id="960a9-197">Je však až toodevelopers, kteří používat tuto možnost tooensure vytvořené jejich soubor Docker správně.</span><span class="sxs-lookup"><span data-stu-id="960a9-197">However, it is up toodevelopers who use this option tooensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="960a9-198">Hello nástrojů Azure neověřuje hello obsah vlastní soubor Docker, takže hello nasazení může selhat, pokud hello soubor Docker má problémy.</span><span class="sxs-lookup"><span data-stu-id="960a9-198">hello Azure Toolkit does not validate hello content in a custom Dockerfile, so hello deployment might fail if hello Dockerfile has issues.</span></span> <span data-ttu-id="960a9-199">Kromě toho hello nástrojů Azure očekává hello vlastní soubor Docker toocontain artefaktem webové aplikace, a pokusí tooopen připojení HTTP.</span><span class="sxs-lookup"><span data-stu-id="960a9-199">In addition, hello Azure Toolkit expects hello custom Dockerfile toocontain a web app artifact, and it will attempt tooopen an HTTP connection.</span></span> <span data-ttu-id="960a9-200">Pokud vývojáři publikovat jiného typu artefaktů, můžou získat neškodné chyby po nasazení.</span><span class="sxs-lookup"><span data-stu-id="960a9-200">If developers publish a different type of artifact, they may receive innocuous errors after deployment.</span></span>

   <span data-ttu-id="960a9-201">c.</span><span class="sxs-lookup"><span data-stu-id="960a9-201">c.</span></span> <span data-ttu-id="960a9-202">**Nastavení portu**: Zadejte hello jedinečnou TCP port vazbu pro váš kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="960a9-202">**Port settings**: Enter hello unique TCP port binding for your Docker container.</span></span>

     ![Hello konfigurovat hello Docker kontejneru toobe vytvořit okno][PUB08]

10. <span data-ttu-id="960a9-204">Po dokončení všech předchozích kroků hello, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="960a9-204">After you have completed all of hello preceding steps, click **Finish**.</span></span>

<span data-ttu-id="960a9-205">Hello Azure Toolkit zahájí nasazení vaší webové aplikace tooAzure v kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="960a9-205">hello Azure Toolkit begins deploying your web app tooAzure in a Docker container.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="960a9-206">Další kroky</span><span class="sxs-lookup"><span data-stu-id="960a9-206">Next steps</span></span>
<span data-ttu-id="960a9-207">Další informace o hello sadách Azure pro integrovaného vývojového prostředí Java najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="960a9-207">For more information about hello Azure Toolkits for Java IDEs, see hello following resources:</span></span>

* <span data-ttu-id="960a9-208">[Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="960a9-208">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="960a9-209">[Co je nového v hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="960a9-209">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="960a9-210">[Instalace hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="960a9-210">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="960a9-211">[Přihlášení pokyny pro hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="960a9-211">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="960a9-212">[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]</span><span class="sxs-lookup"><span data-stu-id="960a9-212">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="960a9-213">[Sada Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="960a9-213">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="960a9-214">[Co je nového v hello nástrojů Azure pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="960a9-214">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="960a9-215">[Instalace hello Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="960a9-215">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="960a9-216">[Přihlášení pokyny pro hello Azure Toolkit IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="960a9-216">[Sign-in instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="960a9-217">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="960a9-217">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="960a9-218">Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java] a [Java nástrojů pro Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="960a9-218">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="960a9-219">Další zdroje pro Docker najdete v tématu hello oficiální [Docker webu].</span><span class="sxs-lookup"><span data-stu-id="960a9-219">For additional resources for Docker, see hello official [Docker website].</span></span>

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