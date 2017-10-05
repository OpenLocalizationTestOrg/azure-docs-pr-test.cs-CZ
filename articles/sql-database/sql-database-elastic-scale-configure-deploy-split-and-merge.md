---
title: "Nasazení služby rozdělení sloučení | Microsoft Docs"
description: "Rozdělování a slučování pomocí nástroje elastické databáze"
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: 9a993c0f-7052-46cd-aa59-073bea8d535a
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 6e2fea882c248fa095a9d450ed54a7b4e64b45e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-split-merge-service"></a><span data-ttu-id="c5484-103">Nasazení služby dělení a slučování</span><span class="sxs-lookup"><span data-stu-id="c5484-103">Deploy a split-merge service</span></span>
<span data-ttu-id="c5484-104">Nástroj pro rozdělení sloučení umožňuje přesun dat mezi horizontálně dělené databáze.</span><span class="sxs-lookup"><span data-stu-id="c5484-104">The split-merge tool lets you move data between sharded databases.</span></span> <span data-ttu-id="c5484-105">V tématu [přesouvání dat mezi instancemi cloudu databáze](sql-database-elastic-scale-overview-split-and-merge.md)</span><span class="sxs-lookup"><span data-stu-id="c5484-105">See [Moving data between scaled-out cloud databases](sql-database-elastic-scale-overview-split-and-merge.md)</span></span>

## <a name="download-the-split-merge-packages"></a><span data-ttu-id="c5484-106">Stáhnout balíčky rozdělení sloučení</span><span class="sxs-lookup"><span data-stu-id="c5484-106">Download the Split-Merge packages</span></span>
1. <span data-ttu-id="c5484-107">Stažení nejnovější verze NuGet z [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="c5484-107">Download the latest NuGet version from [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>
2. <span data-ttu-id="c5484-108">Otevřete příkazový řádek a přejděte do adresáře, kam jste stáhli nuget.exe.</span><span class="sxs-lookup"><span data-stu-id="c5484-108">Open a command prompt and navigate to the directory where you downloaded nuget.exe.</span></span> <span data-ttu-id="c5484-109">Stahování zahrnuje commmands prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c5484-109">The download includes PowerShell commmands.</span></span>
3. <span data-ttu-id="c5484-110">Stáhněte si nejnovější balíček sloučení rozdělení do aktuální adresář se následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c5484-110">Download the latest Split-Merge package into the current directory with the below command:</span></span>
   ```
   nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge
   ```  

<span data-ttu-id="c5484-111">Soubory jsou umístěny v adresáři s názvem **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** kde *x.x.xxx.x* zobrazuje číslo verze.</span><span class="sxs-lookup"><span data-stu-id="c5484-111">The files are placed in a directory named **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** where *x.x.xxx.x* reflects the version number.</span></span> <span data-ttu-id="c5484-112">Hledání souborů služby rozdělení sloučení v **content\splitmerge\service** podadresář a rozdělení sloučení PowerShell skripty (a knihoven DLL klienta) v **content\splitmerge\powershell** podadresář.</span><span class="sxs-lookup"><span data-stu-id="c5484-112">Find the split-merge Service files in the **content\splitmerge\service** sub-directory, and the Split-Merge PowerShell scripts (and required client .dlls) in the **content\splitmerge\powershell** sub-directory.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c5484-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c5484-113">Prerequisites</span></span>
1. <span data-ttu-id="c5484-114">Vytvořte databázi Azure SQL DB, který se použije jako rozdělení sloučení stavu databáze.</span><span class="sxs-lookup"><span data-stu-id="c5484-114">Create an Azure SQL DB database that will be used as the split-merge status database.</span></span> <span data-ttu-id="c5484-115">Přejděte na [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c5484-115">Go to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="c5484-116">Vytvořte novou **SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="c5484-116">Create a new **SQL Database**.</span></span> <span data-ttu-id="c5484-117">Zadejte název databáze a vytvořte nový správce a heslo.</span><span class="sxs-lookup"><span data-stu-id="c5484-117">Give the database a name and create a new administrator and password.</span></span> <span data-ttu-id="c5484-118">Ujistěte se, že záznam jméno a heslo pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="c5484-118">Be sure to record the name and password for later use.</span></span>
2. <span data-ttu-id="c5484-119">Zajistěte, aby váš server Azure SQL DB povolovalo Azure Services pro připojení k němu.</span><span class="sxs-lookup"><span data-stu-id="c5484-119">Ensure that your Azure SQL DB server allows Azure Services to connect to it.</span></span> <span data-ttu-id="c5484-120">Na portálu v **nastavení brány Firewall**, ujistěte se, že **povolit přístup ke službám Azure** nastavení je **na**.</span><span class="sxs-lookup"><span data-stu-id="c5484-120">In the portal, in the **Firewall Settings**, ensure that the **Allow access to Azure Services** setting is set to **On**.</span></span> <span data-ttu-id="c5484-121">Klikněte na ikonu "uložit".</span><span class="sxs-lookup"><span data-stu-id="c5484-121">Click the "save" icon.</span></span>
   
   ![Povolené služby][1]
3. <span data-ttu-id="c5484-123">Vytvoření účtu úložiště Azure, který se použije pro výstup diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="c5484-123">Create an Azure Storage account that will be used for diagnostics output.</span></span> <span data-ttu-id="c5484-124">Přejděte na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c5484-124">Go to the Azure Portal.</span></span> <span data-ttu-id="c5484-125">V levém panelu klikněte na **nový**, klikněte na tlačítko **Data + úložiště**, pak **úložiště**.</span><span class="sxs-lookup"><span data-stu-id="c5484-125">In the left bar, click **New**, click **Data + Storage**, then **Storage**.</span></span>
4. <span data-ttu-id="c5484-126">Vytvoření cloudové služby Azure, která bude obsahovat služby rozdělení sloučení.</span><span class="sxs-lookup"><span data-stu-id="c5484-126">Create an Azure Cloud Service that will contain your Split-Merge service.</span></span>  <span data-ttu-id="c5484-127">Přejděte na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c5484-127">Go to the Azure Portal.</span></span> <span data-ttu-id="c5484-128">V levém panelu klikněte na **nový**, pak **výpočetní**, **Cloudová služba**, a **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c5484-128">In the left bar, click **New**, then **Compute**, **Cloud Service**, and **Create**.</span></span> 

## <a name="configure-your-split-merge-service"></a><span data-ttu-id="c5484-129">Konfigurace služby rozdělení sloučení</span><span class="sxs-lookup"><span data-stu-id="c5484-129">Configure your Split-Merge service</span></span>
### <a name="split-merge-service-configuration"></a><span data-ttu-id="c5484-130">Konfigurace služby rozdělení sloučení</span><span class="sxs-lookup"><span data-stu-id="c5484-130">Split-Merge service configuration</span></span>
1. <span data-ttu-id="c5484-131">Ve složce, kam jste stáhli sestavení rozdělení sloučení, vytvořit kopii **ServiceConfiguration.Template.cscfg** soubor, který se dodává spolu s **SplitMergeService.cspkg** a přejmenujte ji **souboru ServiceConfiguration.cscfg**.</span><span class="sxs-lookup"><span data-stu-id="c5484-131">In the folder where you downloaded the Split-Merge assemblies, create a copy of the **ServiceConfiguration.Template.cscfg** file that shipped alongside **SplitMergeService.cspkg** and rename it **ServiceConfiguration.cscfg**.</span></span>
2. <span data-ttu-id="c5484-132">Otevřete **souboru ServiceConfiguration.cscfg** v textovém editoru, jako je například Visual Studio, která ověřuje vstupy například formát kryptografické otisky certifikátu.</span><span class="sxs-lookup"><span data-stu-id="c5484-132">Open **ServiceConfiguration.cscfg** in a text editor such as Visual Studio that validates inputs such as the format of certificate thumbprints.</span></span>
3. <span data-ttu-id="c5484-133">Vytvořit novou databázi nebo vyberte existující databázi sloužit jako databázi stavu pro operace sloučení rozdělení a načtení připojovacího řetězce databáze.</span><span class="sxs-lookup"><span data-stu-id="c5484-133">Create a new database or choose an existing database to serve as the status database for Split-Merge operations and retrieve the connection string of that database.</span></span> 
   
   > [!IMPORTANT]
   > <span data-ttu-id="c5484-134">V tomto okamžiku stav databáze musí používat Latin kolace (SQL\_Latin1\_Obecné\_CP1\_CI\_AS).</span><span class="sxs-lookup"><span data-stu-id="c5484-134">At this time, the status database must use the Latin  collation (SQL\_Latin1\_General\_CP1\_CI\_AS).</span></span> <span data-ttu-id="c5484-135">Další informace najdete v tématu [název kolace systému Windows (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx).</span><span class="sxs-lookup"><span data-stu-id="c5484-135">For more information, see [Windows Collation Name (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx).</span></span>
   >

   <span data-ttu-id="c5484-136">S Azure SQL DB připojovací řetězec je obvykle ve formátu:</span><span class="sxs-lookup"><span data-stu-id="c5484-136">With Azure SQL DB, the connection string typically is of the form:</span></span>
      ```
      Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
      ```

4. <span data-ttu-id="c5484-137">Zadejte tento připojovací řetězec v soubor .cscfg v obou **SplitMergeWeb** a **SplitMergeWorker** role části ElasticScaleMetadata nastavení.</span><span class="sxs-lookup"><span data-stu-id="c5484-137">Enter this connection string in the cscfg file in both the **SplitMergeWeb** and **SplitMergeWorker** role sections in the ElasticScaleMetadata setting.</span></span>
5. <span data-ttu-id="c5484-138">Pro **SplitMergeWorker** role, zadejte platný připojovací řetězec do úložiště Azure pro **WorkerRoleSynchronizationStorageAccountConnectionString** nastavení.</span><span class="sxs-lookup"><span data-stu-id="c5484-138">For the **SplitMergeWorker** role, enter a valid connection string to Azure storage for the **WorkerRoleSynchronizationStorageAccountConnectionString** setting.</span></span>

### <a name="configure-security"></a><span data-ttu-id="c5484-139">Konfigurace zabezpečení</span><span class="sxs-lookup"><span data-stu-id="c5484-139">Configure security</span></span>
<span data-ttu-id="c5484-140">Podrobné pokyny ke konfiguraci zabezpečení služby, najdete v části [konfigurace zabezpečení rozdělení sloučení](sql-database-elastic-scale-split-merge-security-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="c5484-140">For detailed instructions to configure the security of the service, refer to the [Split-Merge security configuration](sql-database-elastic-scale-split-merge-security-configuration.md).</span></span>

<span data-ttu-id="c5484-141">Pro účely jednoduchá testovací nasazení pro tento kurz minimální sadu konfiguračních kroků bude provádět ke zprovoznění služby a spuštěná.</span><span class="sxs-lookup"><span data-stu-id="c5484-141">For the purposes of a simple test deployment for this tutorial, a minimal set of configuration steps will be performed to get the service up and running.</span></span> <span data-ttu-id="c5484-142">Tyto kroky aktivují jenom jeden nebo účet počítače, aby mohla komunikovat se službou provádění.</span><span class="sxs-lookup"><span data-stu-id="c5484-142">These steps enable only the one machine/account executing them to communicate with the service.</span></span>

### <a name="create-a-self-signed-certificate"></a><span data-ttu-id="c5484-143">Vytvořit certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="c5484-143">Create a self-signed certificate</span></span>
<span data-ttu-id="c5484-144">Vytvořte nový adresář a z tohoto adresáře spusťte následující příkaz pomocí [příkazový řádek vývojáře pro sadu Visual Studio](http://msdn.microsoft.com/library/ms229859.aspx) okno:</span><span class="sxs-lookup"><span data-stu-id="c5484-144">Create a new directory and from this directory execute the following command using a [Developer Command Prompt for Visual Studio](http://msdn.microsoft.com/library/ms229859.aspx) window:</span></span>

   ```
    makecert ^
    -n "CN=*.cloudapp.net" ^
    -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2" ^
    -a sha1 -len 2048 ^
    -sr currentuser -ss root ^
    -sv MyCert.pvk MyCert.cer
   ```

<span data-ttu-id="c5484-145">Zobrazí se výzva k zadání hesla k ochraně soukromého klíče.</span><span class="sxs-lookup"><span data-stu-id="c5484-145">You are asked for a password to protect the private key.</span></span> <span data-ttu-id="c5484-146">Zadejte silné heslo a potvrďte ho.</span><span class="sxs-lookup"><span data-stu-id="c5484-146">Enter a strong password and confirm it.</span></span> <span data-ttu-id="c5484-147">Zobrazí se výzva k zadání hesla pro použití jednou po který.</span><span class="sxs-lookup"><span data-stu-id="c5484-147">You are then prompted for the password to be used once more after that.</span></span> <span data-ttu-id="c5484-148">Klikněte na tlačítko **Ano** na konci ho importovat do úložiště Důvěryhodné kořenové certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="c5484-148">Click **Yes** at the end to import it to the Trusted Certification Authorities Root store.</span></span>

### <a name="create-a-pfx-file"></a><span data-ttu-id="c5484-149">Vytvořte soubor PFX</span><span class="sxs-lookup"><span data-stu-id="c5484-149">Create a PFX file</span></span>
<span data-ttu-id="c5484-150">Spusťte následující příkaz z téhož okna, kde byl proveden makecert; Použijte stejné heslo, které jste použili k vytvoření certifikátu:</span><span class="sxs-lookup"><span data-stu-id="c5484-150">Execute the following command from the same window where makecert was executed; use the same password that you used to create the certificate:</span></span>

    pvk2pfx -pvk MyCert.pvk -spc MyCert.cer -pfx MyCert.pfx -pi <password>

### <a name="import-the-client-certificate-into-the-personal-store"></a><span data-ttu-id="c5484-151">Importujte certifikát klienta do osobního úložiště</span><span class="sxs-lookup"><span data-stu-id="c5484-151">Import the client certificate into the personal store</span></span>
1. <span data-ttu-id="c5484-152">V Průzkumníku Windows, klikněte dvakrát na **MyCert.pfx**.</span><span class="sxs-lookup"><span data-stu-id="c5484-152">In Windows Explorer, double-click **MyCert.pfx**.</span></span>
2. <span data-ttu-id="c5484-153">V **Průvodce importem certifikátu** vyberte **aktuální uživatel** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c5484-153">In the **Certificate Import Wizard** select **Current User** and click **Next**.</span></span>
3. <span data-ttu-id="c5484-154">Zkontrolujte cestu k souboru a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c5484-154">Confirm the file path and click **Next**.</span></span>
4. <span data-ttu-id="c5484-155">Zadejte heslo, nechte **obsahovat všechny rozšířené vlastnosti** zaškrtnutí a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c5484-155">Type the password, leave **Include all extended properties** checked and click **Next**.</span></span>
5. <span data-ttu-id="c5484-156">Nechte **automaticky vybrat úložiště certifikátů [...]**  zaškrtnutí a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c5484-156">Leave **Automatically select the certificate store[…]** checked and click **Next**.</span></span>
6. <span data-ttu-id="c5484-157">Klikněte na tlačítko **Dokončit** a **OK**.</span><span class="sxs-lookup"><span data-stu-id="c5484-157">Click **Finish** and **OK**.</span></span>

### <a name="upload-the-pfx-file-to-the-cloud-service"></a><span data-ttu-id="c5484-158">Nahrát soubor PFX do cloudové služby</span><span class="sxs-lookup"><span data-stu-id="c5484-158">Upload the PFX file to the cloud service</span></span>
1. <span data-ttu-id="c5484-159">Přejděte na [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c5484-159">Go to the [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="c5484-160">Vyberte **cloudových služeb**.</span><span class="sxs-lookup"><span data-stu-id="c5484-160">Select **Cloud Services**.</span></span>
3. <span data-ttu-id="c5484-161">Vyberte cloudovou službu, kterou jste vytvořili výše pro službu rozdělení či sloučení.</span><span class="sxs-lookup"><span data-stu-id="c5484-161">Select the cloud service you created above for the Split/Merge service.</span></span>
4. <span data-ttu-id="c5484-162">Klikněte na tlačítko **certifikáty** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="c5484-162">Click **Certificates** on the top menu.</span></span>
5. <span data-ttu-id="c5484-163">Klikněte na tlačítko **nahrát** v dolním panelu.</span><span class="sxs-lookup"><span data-stu-id="c5484-163">Click **Upload** in the bottom bar.</span></span>
6. <span data-ttu-id="c5484-164">Vyberte soubor PFX a zadejte stejné heslo, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="c5484-164">Select the PFX file and enter the same password as above.</span></span>
7. <span data-ttu-id="c5484-165">Po dokončení, zkopírujte kryptografický otisk certifikátu z nové položky v seznamu.</span><span class="sxs-lookup"><span data-stu-id="c5484-165">Once completed, copy the certificate thumbprint from the new entry in the list.</span></span>

### <a name="update-the-service-configuration-file"></a><span data-ttu-id="c5484-166">Aktualizovat konfigurační soubor služby</span><span class="sxs-lookup"><span data-stu-id="c5484-166">Update the service configuration file</span></span>
<span data-ttu-id="c5484-167">Vložte kryptografický otisk certifikátu výše zkopírují do atribut kryptografický otisk nebo hodnotu z těchto nastavení.</span><span class="sxs-lookup"><span data-stu-id="c5484-167">Paste the certificate thumbprint copied above into the thumbprint/value attribute of these settings.</span></span>
<span data-ttu-id="c5484-168">Pro roli pracovního procesu:</span><span class="sxs-lookup"><span data-stu-id="c5484-168">For the worker role:</span></span>
   ```
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

<span data-ttu-id="c5484-169">Pro webovou roli:</span><span class="sxs-lookup"><span data-stu-id="c5484-169">For the web role:</span></span>

   ```
    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />
    <Setting name="AllowedClientCertificateThumbprints" value="" />
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

<span data-ttu-id="c5484-170">Všimněte si, že pro produkční nasazení samostatné certifikáty prosím by měl použít pro certifikační Autoritu pro šifrování, certifikát serveru a klientské certifikáty.</span><span class="sxs-lookup"><span data-stu-id="c5484-170">Please note that for production deployments separate certificates should be used for the CA, for encryption, the Server certificate and client certificates.</span></span> <span data-ttu-id="c5484-171">Podrobné pokyny v tomto najdete v tématu [konfigurace zabezpečení](sql-database-elastic-scale-split-merge-security-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="c5484-171">For detailed instructions on this, see [Security Configuration](sql-database-elastic-scale-split-merge-security-configuration.md).</span></span>

## <a name="deploy-your-service"></a><span data-ttu-id="c5484-172">Nasazení služby</span><span class="sxs-lookup"><span data-stu-id="c5484-172">Deploy your service</span></span>
1. <span data-ttu-id="c5484-173">Přejděte na [portál Azure](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="c5484-173">Go to the [Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="c5484-174">Klikněte **cloudové služby** na levé straně a vyberte cloudovou službu, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="c5484-174">Click the **Cloud Services** tab on the left, and select the cloud service that you created earlier.</span></span>
3. <span data-ttu-id="c5484-175">Klikněte na tlačítko **řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="c5484-175">Click **Dashboard**.</span></span>
4. <span data-ttu-id="c5484-176">Vyberte pracovní prostředí a potom klikněte na **nahrát nový pracovní nasazení**.</span><span class="sxs-lookup"><span data-stu-id="c5484-176">Choose the staging environment, then click **Upload a new staging deployment**.</span></span>
   
   ![Pracovní][3]
5. <span data-ttu-id="c5484-178">V dialogovém okně zadejte označení nasazení.</span><span class="sxs-lookup"><span data-stu-id="c5484-178">In the dialog box, enter a deployment label.</span></span> <span data-ttu-id="c5484-179">Pro 'Balíček' i 'konfiguraci, klikněte na tlačítko 'Z Local' a vyberte **SplitMergeService.cspkg** Souborová služba a který jste dříve nakonfigurovali souboru .cscfg.</span><span class="sxs-lookup"><span data-stu-id="c5484-179">For both 'Package' and 'Configuration', click 'From Local' and choose the **SplitMergeService.cspkg** file and your .cscfg file that you configured earlier.</span></span>
6. <span data-ttu-id="c5484-180">Ujistěte se, že na zaškrtávací políčko s názvem bez přípony **nasadit, i když jedna nebo víc rolí obsahuje jednu instanci** je zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="c5484-180">Ensure that the checkbox labeled **Deploy even if one or more roles contain a single instance** is checked.</span></span>
7. <span data-ttu-id="c5484-181">Klikněte na tlačítko značek v pravém dolním zahájíte nasazení.</span><span class="sxs-lookup"><span data-stu-id="c5484-181">Hit the tick button in the bottom right to begin the deployment.</span></span> <span data-ttu-id="c5484-182">Jeho trvat několik minut na dokončení očekávejte.</span><span class="sxs-lookup"><span data-stu-id="c5484-182">Expect it to take a few minutes to complete.</span></span>

   ![Odeslat][4]

## <a name="troubleshoot-the-deployment"></a><span data-ttu-id="c5484-184">Řešení potíží s nasazení</span><span class="sxs-lookup"><span data-stu-id="c5484-184">Troubleshoot the deployment</span></span>
<span data-ttu-id="c5484-185">Pokud vaši webovou roli selže do režimu online, je pravděpodobně problém s konfigurací zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="c5484-185">If your web role fails to come online, it is likely a problem with the security configuration.</span></span> <span data-ttu-id="c5484-186">Zkontrolujte, jestli je nakonfigurované SSL, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="c5484-186">Check that the SSL is configured as described above.</span></span>

<span data-ttu-id="c5484-187">Pokud své role pracovního procesu selže do režimu online, ale vaši webovou roli úspěšné, je pravděpodobně problém s připojením k databázi stav, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="c5484-187">If your worker role fails to come online, but your web role succeeds, it is most likely a problem connecting to the status database that you created earlier.</span></span>

* <span data-ttu-id="c5484-188">Ujistěte se, že je připojovací řetězec do vaší .cscfg přesná.</span><span class="sxs-lookup"><span data-stu-id="c5484-188">Make sure that the connection string in your .cscfg is accurate.</span></span>
* <span data-ttu-id="c5484-189">Zkontrolujte, zda server a databázi existují, a zda jsou správné id uživatele a heslo.</span><span class="sxs-lookup"><span data-stu-id="c5484-189">Check that the server and database exist, and that the user id and password are correct.</span></span>
* <span data-ttu-id="c5484-190">Pro databáze SQL Azure musí mít připojovací řetězec ve tvaru:</span><span class="sxs-lookup"><span data-stu-id="c5484-190">For Azure SQL DB, the connection string should be of the form:</span></span>

   ```  
   Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
   ```

* <span data-ttu-id="c5484-191">Ujistěte se, že název serveru nemá na začátku **https://**.</span><span class="sxs-lookup"><span data-stu-id="c5484-191">Ensure that the server name does not begin with **https://**.</span></span>
* <span data-ttu-id="c5484-192">Zajistěte, aby váš server Azure SQL DB povolovalo Azure Services pro připojení k němu.</span><span class="sxs-lookup"><span data-stu-id="c5484-192">Ensure that your Azure SQL DB server allows Azure Services to connect to it.</span></span> <span data-ttu-id="c5484-193">K tomuto účelu otevřete https://manage.windowsazure.com, na levé straně klikněte na tlačítko "Databází SQL", klikněte na tlačítko "Servery" v horní části a vyberte svůj server.</span><span class="sxs-lookup"><span data-stu-id="c5484-193">To do this, open https://manage.windowsazure.com, click “SQL Databases” on the left, click “Servers” at the top, and select your server.</span></span> <span data-ttu-id="c5484-194">Klikněte na tlačítko **konfigurace** v horní části a ujistěte se, že **služeb Azure** nastavení je "Ano".</span><span class="sxs-lookup"><span data-stu-id="c5484-194">Click **Configure** at the top and ensure that the **Azure Services** setting is set to “Yes”.</span></span> <span data-ttu-id="c5484-195">(Viz část požadavky v horní části v tomto článku).</span><span class="sxs-lookup"><span data-stu-id="c5484-195">(See the Prerequisites section at the top of this article).</span></span>

## <a name="test-the-service-deployment"></a><span data-ttu-id="c5484-196">Testovací nasazení služby</span><span class="sxs-lookup"><span data-stu-id="c5484-196">Test the service deployment</span></span>
### <a name="connect-with-a-web-browser"></a><span data-ttu-id="c5484-197">Připojení s webovým prohlížečem</span><span class="sxs-lookup"><span data-stu-id="c5484-197">Connect with a web browser</span></span>
<span data-ttu-id="c5484-198">Zjistěte koncový bod webové služby rozdělení sloučení.</span><span class="sxs-lookup"><span data-stu-id="c5484-198">Determine the web endpoint of your Split-Merge service.</span></span> <span data-ttu-id="c5484-199">Tento nástroj naleznete na portálu Azure Classic přechodem na **řídicí panel** cloudové služby a vyhledávání v části **adresa URL webu** na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="c5484-199">You can find this in the Azure Classic Portal by going to the **Dashboard** of your cloud service and looking under **Site URL** on the right side.</span></span> <span data-ttu-id="c5484-200">Nahraďte **http://** s **https://** vzhledem k tomu, že výchozí nastavení zabezpečení zakázat koncový bod HTTP.</span><span class="sxs-lookup"><span data-stu-id="c5484-200">Replace **http://** with **https://** since the default security settings disable the HTTP endpoint.</span></span> <span data-ttu-id="c5484-201">Načtení stránky pro tuto adresu URL do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="c5484-201">Load the page for this URL into your browser.</span></span>

### <a name="test-with-powershell-scripts"></a><span data-ttu-id="c5484-202">Testování pomocí skriptů prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c5484-202">Test with PowerShell scripts</span></span>
<span data-ttu-id="c5484-203">Nasazení a prostředí může být testována spuštěním zahrnuty vzorové skripty prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c5484-203">The deployment and your environment can be tested by running the included sample PowerShell scripts.</span></span>

<span data-ttu-id="c5484-204">Zahrnuté soubory skriptů jsou:</span><span class="sxs-lookup"><span data-stu-id="c5484-204">The script files included are:</span></span>

1. <span data-ttu-id="c5484-205">**SetupSampleSplitMergeEnvironment.ps1** -nastaví datovou vrstvu testu pro rozdělení/Merge (v tabulce níže najdete podrobný popis)</span><span class="sxs-lookup"><span data-stu-id="c5484-205">**SetupSampleSplitMergeEnvironment.ps1** - sets up a test data tier for Split/Merge (see table below for detailed description)</span></span>
2. <span data-ttu-id="c5484-206">**ExecuteSampleSplitMerge.ps1** -provádí operace test na testovací datové vrstvy (v tabulce níže najdete podrobný popis)</span><span class="sxs-lookup"><span data-stu-id="c5484-206">**ExecuteSampleSplitMerge.ps1** - executes test operations on the test data tier (see table below for detailed description)</span></span>
3. <span data-ttu-id="c5484-207">**GetMappings.ps1** – nejvyšší úrovně ukázkový skript, který vytiskne aktuální stav mapování horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="c5484-207">**GetMappings.ps1** - top-level sample script that prints out the current state of the shard mappings.</span></span>
4. <span data-ttu-id="c5484-208">**ShardManagement.psm1** -skriptu pomocné rutiny, které zabaluje rozhraní API ShardManagement</span><span class="sxs-lookup"><span data-stu-id="c5484-208">**ShardManagement.psm1**  - helper script that wraps the ShardManagement API</span></span>
5. <span data-ttu-id="c5484-209">**SqlDatabaseHelpers.psm1** – Pomocník skriptu pro vytváření a správu databází SQL</span><span class="sxs-lookup"><span data-stu-id="c5484-209">**SqlDatabaseHelpers.psm1** - helper script for creating and managing SQL databases</span></span>
   
   <table style="width:100%">
     <tr>
       <th><span data-ttu-id="c5484-210">Soubor PowerShell</span><span class="sxs-lookup"><span data-stu-id="c5484-210">PowerShell file</span></span></th>
       <th><span data-ttu-id="c5484-211">Kroky</span><span class="sxs-lookup"><span data-stu-id="c5484-211">Steps</span></span></th>
     </tr>
     <tr>
       <th rowspan="5"><span data-ttu-id="c5484-212">SetupSampleSplitMergeEnvironment.ps1</span><span class="sxs-lookup"><span data-stu-id="c5484-212">SetupSampleSplitMergeEnvironment.ps1</span></span></th>
       <td>1.    <span data-ttu-id="c5484-213">Vytvoří databázi správce horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="c5484-213">Creates a shard map manager database</span></span></td>
     </tr>
     <tr>
       <td>2.    <span data-ttu-id="c5484-214">Vytvoří 2 horizontálního oddílu databáze.</span><span class="sxs-lookup"><span data-stu-id="c5484-214">Creates 2 shard databases.</span></span>
     </tr>
     <tr>
       <td>3.    <span data-ttu-id="c5484-215">Vytvoří mapu horizontálního oddílu pro tyto databáze (odstraní všechny existující horizontálního oddílu mapování na tyto databáze).</span><span class="sxs-lookup"><span data-stu-id="c5484-215">Creates a shard map for those database (deletes any existing shard maps on those databases).</span></span> </td>
     </tr>
     <tr>
       <td>4.    <span data-ttu-id="c5484-216">Vytvoří tabulku malé ukázkové v obou horizontálních oddílů a naplní tabulky v jednom z horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="c5484-216">Creates a small sample table in both the shards, and populates the table in one of the shards.</span></span></td>
     </tr>
     <tr>
       <td>5.    <span data-ttu-id="c5484-217">Deklaruje SchemaInfo pro horizontálně dělenou tabulku.</span><span class="sxs-lookup"><span data-stu-id="c5484-217">Declares the SchemaInfo for the sharded table.</span></span></td>
     </tr>
   </table>
   <table style="width:100%">
     <tr>
       <th><span data-ttu-id="c5484-218">Soubor PowerShell</span><span class="sxs-lookup"><span data-stu-id="c5484-218">PowerShell file</span></span></th>
       <th><span data-ttu-id="c5484-219">Kroky</span><span class="sxs-lookup"><span data-stu-id="c5484-219">Steps</span></span></th>
     </tr>
   <tr>
       <th rowspan="4"><span data-ttu-id="c5484-220">ExecuteSampleSplitMerge.ps1</span><span class="sxs-lookup"><span data-stu-id="c5484-220">ExecuteSampleSplitMerge.ps1</span></span> </th>
       <td>1.    <span data-ttu-id="c5484-221">Odešle požadavek rozdělení front-end webové rozdělení sloučení služby, který rozdělí poloviční dat z první horizontálního oddílu, do druhé horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="c5484-221">Sends a split request to the Split-Merge Service web frontend, which splits half the data from the first shard to the second shard.</span></span></td>
     </tr>
     <tr>
       <td>2.    <span data-ttu-id="c5484-222">Dotazování front-endu webové pro stav žádosti o rozdělení a čeká na dokončení žádosti.</span><span class="sxs-lookup"><span data-stu-id="c5484-222">Polls the web frontend for the split request status and waits until the request completes.</span></span></td>
     </tr>
     <tr>
       <td>3.    <span data-ttu-id="c5484-223">Odešle požadavek sloučení pro front-end webové rozdělení sloučení služby, který přesouvá data z druhé horizontálního oddílu zpět na první horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="c5484-223">Sends a merge request to the Split-Merge Service web frontend, which moves the data from the second shard back to the first shard.</span></span></td>
     </tr>
     <tr>
       <td>4.    <span data-ttu-id="c5484-224">Dotazování front-endu webové pro stav žádosti o sloučení a čeká na dokončení žádosti.</span><span class="sxs-lookup"><span data-stu-id="c5484-224">Polls the web frontend for the merge request status and waits until the request completes.</span></span></td>
     </tr>
   </table>
   
## <a name="use-powershell-to-verify-your-deployment"></a><span data-ttu-id="c5484-225">Ověření nasazení pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c5484-225">Use PowerShell to verify your deployment</span></span>
1. <span data-ttu-id="c5484-226">Otevřete nové okno prostředí PowerShell a přejděte do adresáře, kam jste stáhli balíček rozdělení sloučení a pak přejděte do adresáře "powershell".</span><span class="sxs-lookup"><span data-stu-id="c5484-226">Open a new PowerShell window and navigate to the directory where you downloaded the Split-Merge package, and then navigate into the “powershell” directory.</span></span>
2. <span data-ttu-id="c5484-227">Vytvoření serveru Azure SQL database (nebo vyberte existující server) kde bude vytvořen správce mapy horizontálního oddílu a horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="c5484-227">Create an Azure SQL database server (or choose an existing server) where the shard map manager and shards will be created.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c5484-228">Ve výchozím nastavení do skriptu zjednodušení vytvoří skript SetupSampleSplitMergeEnvironment.ps1 těchto databází na stejném serveru.</span><span class="sxs-lookup"><span data-stu-id="c5484-228">The SetupSampleSplitMergeEnvironment.ps1 script creates all these databases on the same server by default to keep the script simple.</span></span> <span data-ttu-id="c5484-229">Toto není omezení služby rozdělení sloučení sám sebe.</span><span class="sxs-lookup"><span data-stu-id="c5484-229">This is not a restriction of the Split-Merge Service itself.</span></span>
   >
   
   <span data-ttu-id="c5484-230">Přihlašovací jméno ověřování SQL s přístupem pro čtení a zápis do Operations Manager bude potřeba pro službu rozdělení sloučení pro přesun dat a aktualizaci mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="c5484-230">A SQL authentication login with read/write access to the DBs will be needed for the Split-Merge service to move data and update the shard map.</span></span> <span data-ttu-id="c5484-231">Vzhledem k tomu, že služba rozdělení sloučení běží v cloudu, nepodporuje aktuálně integrované ověřování.</span><span class="sxs-lookup"><span data-stu-id="c5484-231">Since the Split-Merge Service runs in the cloud, it does not currently support Integrated Authentication.</span></span>
   
   <span data-ttu-id="c5484-232">Ujistěte se, že server Azure SQL je nakonfigurována pro povolení přístupu z IP adresy počítače spuštěného tyto skripty.</span><span class="sxs-lookup"><span data-stu-id="c5484-232">Make sure the Azure SQL server is configured to allow access from the IP address of the machine running these scripts.</span></span> <span data-ttu-id="c5484-233">Toto nastavení v rámci serveru Azure SQL můžete najít / configuration / povolené IP adresy.</span><span class="sxs-lookup"><span data-stu-id="c5484-233">You can find this setting under the Azure SQL server / configuration / allowed IP addresses.</span></span>
3. <span data-ttu-id="c5484-234">Spusťte skript SetupSampleSplitMergeEnvironment.ps1 k vytvoření ukázkové prostředí.</span><span class="sxs-lookup"><span data-stu-id="c5484-234">Execute the SetupSampleSplitMergeEnvironment.ps1 script to create the sample environment.</span></span>
   
   <span data-ttu-id="c5484-235">Spuštěním tohoto skriptu budou vymazat všechny existující data správy mapy horizontálního oddílu struktury na databázi horizontálního oddílu mapa správce a horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="c5484-235">Running this script will wipe out any existing shard map management data structures on the shard map manager database and the shards.</span></span> <span data-ttu-id="c5484-236">Může být užitečné skript znovu spustit, pokud chcete znovu inicializovat horizontálního oddílu mapy nebo horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="c5484-236">It may be useful to rerun the script if you wish to re-initialize the shard map or shards.</span></span>
   
   <span data-ttu-id="c5484-237">Ukázka příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="c5484-237">Sample command line:</span></span>

   ```   
     .\SetupSampleSplitMergeEnvironment.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'
   ```      
4. <span data-ttu-id="c5484-238">Spusťte skript Getmappings.ps1 zobrazení mapování, která momentálně existují v ukázkové prostředí.</span><span class="sxs-lookup"><span data-stu-id="c5484-238">Execute the Getmappings.ps1 script to view the mappings that currently exist in the sample environment.</span></span>
   
   ```
     .\GetMappings.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'

   ```         
5. <span data-ttu-id="c5484-239">Spusťte skript ExecuteSampleSplitMerge.ps1 provést operaci rozdělení (přesunutí poloviční dat na první horizontálního oddílu druhý horizontálního oddílu) a pak operace sloučení (přesunutí dat zpět na první horizontálního oddílu).</span><span class="sxs-lookup"><span data-stu-id="c5484-239">Execute the ExecuteSampleSplitMerge.ps1 script to execute a split operation (moving half the data on the first shard to the second shard) and then a merge operation (moving the data back onto the first shard).</span></span> <span data-ttu-id="c5484-240">Pokud jste nakonfigurovali SSL a zbývajících koncový bod http zakázaná, ujistěte se, že je použít koncový bod https://.</span><span class="sxs-lookup"><span data-stu-id="c5484-240">If you configured SSL and left the http endpoint disabled, ensure that you use the https:// endpoint instead.</span></span>
   
   <span data-ttu-id="c5484-241">Ukázka příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="c5484-241">Sample command line:</span></span>

   ```   
     .\ExecuteSampleSplitMerge.ps1
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net' 
         -SplitMergeServiceEndpoint 'https://mysplitmergeservice.cloudapp.net' 
         -CertificateThumbprint '0123456789abcdef0123456789abcdef01234567'
   ```      
   
   <span data-ttu-id="c5484-242">Pokud se zobrazí níže chyba, je pravděpodobně k potížím s certifikátem váš koncový bod webové.</span><span class="sxs-lookup"><span data-stu-id="c5484-242">If you receive the below error, it is most likely a problem with your Web endpoint’s certificate.</span></span> <span data-ttu-id="c5484-243">Pokuste se připojit ke koncovému bodu webové pomocí Oblíbené webový prohlížeč a zkontrolujte, jestli Chyba certifikátu.</span><span class="sxs-lookup"><span data-stu-id="c5484-243">Try connecting to the Web endpoint with your favorite Web browser and check if there is a certificate error.</span></span>
   
     ```
     Invoke-WebRequest : The underlying connection was closed: Could not establish trust relationship for the SSL/TLSsecure channel.
     ```
   
   <span data-ttu-id="c5484-244">Pokud byly úspěšné, výstup by měl vypadat jako níže:</span><span class="sxs-lookup"><span data-stu-id="c5484-244">If it succeeded, the output should look like the below:</span></span>
   
   ```
   > .\ExecuteSampleSplitMerge.ps1 -UserName 'mysqluser' -Password 'MySqlPassw0rd' -ShardMapManagerServerName 'abcdefghij.database.windows.net' -SplitMergeServiceEndpoint 'http://mysplitmergeservice.cloudapp.net' -CertificateThumbprint 0123456789abcdef0123456789abcdef01234567
   > Sending split request
   > Began split operation with id dc68dfa0-e22b-4823-886a-9bdc903c80f3
   > Polling split-merge request status. Press Ctrl-C to end
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source to target shard.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Waiting for reference tables copy     completion.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source to target shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing the request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > Sending merge request
   > Began merge operation with id 6ffc308f-d006-466b-b24e-857242ec5f66
   > Polling request status. Press Ctrl-C to end
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source to target shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing the request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > 
   ```
6. <span data-ttu-id="c5484-245">Experimentujte s jiné datové typy!</span><span class="sxs-lookup"><span data-stu-id="c5484-245">Experiment with other data types!</span></span> <span data-ttu-id="c5484-246">Všechny tyto skripty trvat volitelný parametr - ShardKeyType, který vám umožní určit typ klíče.</span><span class="sxs-lookup"><span data-stu-id="c5484-246">All of these scripts take an optional -ShardKeyType parameter that allows you to specify the key type.</span></span> <span data-ttu-id="c5484-247">Výchozí hodnota je Int32, ale můžete také určit Int64, identifikátor Guid nebo Binary.</span><span class="sxs-lookup"><span data-stu-id="c5484-247">The default is Int32, but you can also specify Int64, Guid, or Binary.</span></span>

## <a name="create-requests"></a><span data-ttu-id="c5484-248">Požadavky na vytvoření</span><span class="sxs-lookup"><span data-stu-id="c5484-248">Create requests</span></span>
<span data-ttu-id="c5484-249">Službu lze pomocí webového uživatelského rozhraní nebo importováním a pomocí modulu SplitMerge.psm1 PowerShell, které se budou odesílat své žádosti prostřednictvím webové role.</span><span class="sxs-lookup"><span data-stu-id="c5484-249">The service can be used either by using the web UI or by importing and using the SplitMerge.psm1 PowerShell module which will submit your requests through the web role.</span></span>

<span data-ttu-id="c5484-250">Službu můžete přesouvat data v horizontálně dělené tabulky a referenční tabulky.</span><span class="sxs-lookup"><span data-stu-id="c5484-250">The service can move data in both sharded tables and reference tables.</span></span> <span data-ttu-id="c5484-251">Horizontálně dělenou tabulku má klíčový sloupec horizontálního dělení a má jiný řádek dat v každém horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="c5484-251">A sharded table has a sharding key column and has different row data on each shard.</span></span> <span data-ttu-id="c5484-252">Referenční tabulka není horizontálně dělené tak, aby obsahovala stejná data řádků na každé horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="c5484-252">A reference table is not sharded so it contains the same row data on every shard.</span></span> <span data-ttu-id="c5484-253">Referenční tabulky jsou užitečné pro data, která se nemění často a slouží k připojení k s horizontálně dělené tabulky v dotazech.</span><span class="sxs-lookup"><span data-stu-id="c5484-253">Reference tables are useful for data that does not change often and is used to JOIN with sharded tables in queries.</span></span>

<span data-ttu-id="c5484-254">Aby bylo možné provést operaci merge rozdělení, je potřeba deklarovat horizontálně dělené tabulky a referenční tabulky, které chcete mít přesunout.</span><span class="sxs-lookup"><span data-stu-id="c5484-254">In order to perform a split-merge operation, you must declare the sharded tables and reference tables that you want to have moved.</span></span> <span data-ttu-id="c5484-255">To je provedeno pomocí **SchemaInfo** rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c5484-255">This is accomplished with the **SchemaInfo** API.</span></span> <span data-ttu-id="c5484-256">Toto rozhraní API je v **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="c5484-256">This API is in the **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** namespace.</span></span>

1. <span data-ttu-id="c5484-257">Pro každou horizontálně dělenou tabulku, vytvořte **ShardedTableInfo** objekt popisující název schématu nadřazené tabulky (volitelné, výchozí hodnota je "dbo"), název tabulky a název sloupce v této tabulce, který obsahuje klíč horizontálního dělení.</span><span class="sxs-lookup"><span data-stu-id="c5484-257">For each sharded table, create a **ShardedTableInfo** object describing the table’s parent schema name (optional, defaults to “dbo”), the table name, and the column name in that table that contains the sharding key.</span></span>
2. <span data-ttu-id="c5484-258">Pro každý odkaz na tabulku, vytvoření **ReferenceTableInfo** objekt popisující název schématu nadřazené tabulky (volitelné, výchozí hodnota je "dbo") a název tabulky.</span><span class="sxs-lookup"><span data-stu-id="c5484-258">For each reference table, create a **ReferenceTableInfo** object describing the table’s parent schema name (optional, defaults to “dbo”) and the table name.</span></span>
3. <span data-ttu-id="c5484-259">Výše uvedené objekty TableInfo přidávat do nové **SchemaInfo** objektu.</span><span class="sxs-lookup"><span data-stu-id="c5484-259">Add the above TableInfo objects to a new **SchemaInfo** object.</span></span>
4. <span data-ttu-id="c5484-260">Získat odkaz na **ShardMapManager** objekt a volání **GetSchemaInfoCollection**.</span><span class="sxs-lookup"><span data-stu-id="c5484-260">Get a reference to a **ShardMapManager** object, and call **GetSchemaInfoCollection**.</span></span>
5. <span data-ttu-id="c5484-261">Přidat **SchemaInfo** k **SchemaInfoCollection**, poskytuje název pro mapování horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="c5484-261">Add the **SchemaInfo** to the **SchemaInfoCollection**, providing the shard map name.</span></span>

<span data-ttu-id="c5484-262">Příklad toho si můžete prohlédnout ve skriptu SetupSampleSplitMergeEnvironment.ps1.</span><span class="sxs-lookup"><span data-stu-id="c5484-262">An example of this can be seen in the SetupSampleSplitMergeEnvironment.ps1 script.</span></span>

<span data-ttu-id="c5484-263">Službu rozdělení sloučení nevytvoří cílová databáze (nebo schéma pro všechny tabulky v databázi) pro vás.</span><span class="sxs-lookup"><span data-stu-id="c5484-263">The Split-Merge service does not create the target database (or schema for any tables in the database) for you.</span></span> <span data-ttu-id="c5484-264">Musí být předem vytvořené před odesláním požadavku službě.</span><span class="sxs-lookup"><span data-stu-id="c5484-264">They must be pre-created before sending a request to the service.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="c5484-265">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="c5484-265">Troubleshooting</span></span>
<span data-ttu-id="c5484-266">Může se zobrazit pod zpráv při spuštění ukázkové skripty prostředí powershell:</span><span class="sxs-lookup"><span data-stu-id="c5484-266">You may see the below message when running the sample powershell scripts:</span></span>

   ```
   Invoke-WebRequest : The underlying connection was closed: Could not establish trust relationship for the SSL/TLS secure channel.
   ```

<span data-ttu-id="c5484-267">Tato chyba znamená, že svůj certifikát SSL není správně nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="c5484-267">This error means that your SSL certificate is not configured correctly.</span></span> <span data-ttu-id="c5484-268">Postupujte podle pokynů v části 'Připojení s webovým prohlížečem'.</span><span class="sxs-lookup"><span data-stu-id="c5484-268">Please follow the instructions in section 'Connecting with a web browser'.</span></span>

<span data-ttu-id="c5484-269">Pokud nelze odeslat požadavky mohou se zobrazit tato:</span><span class="sxs-lookup"><span data-stu-id="c5484-269">If you cannot submit requests you may see this:</span></span>

```
[Exception] System.Data.SqlClient.SqlException (0x80131904): Could not find stored procedure 'dbo.InsertRequest'. 
```

<span data-ttu-id="c5484-270">Zkontrolujte v takovém případě konfiguračního souboru na konkrétní nastavení pro **WorkerRoleSynchronizationStorageAccountConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="c5484-270">In this case, check your configuration file, in particular the setting for **WorkerRoleSynchronizationStorageAccountConnectionString**.</span></span> <span data-ttu-id="c5484-271">Tato chyba obvykle značí, že role pracovního procesu nelze úspěšně spustit databázi metadat při prvním použití.</span><span class="sxs-lookup"><span data-stu-id="c5484-271">This error typically indicates that the worker role could not successfully initialize the metadata database on first use.</span></span> 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/allowed-services.png
[2]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/manage.png
[3]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/staging.png
[4]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/upload.png
[5]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/storage.png

