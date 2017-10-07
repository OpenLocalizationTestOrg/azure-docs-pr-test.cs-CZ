---
title: "aaaDeploy rozdělení sloučení služby | Microsoft Docs"
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
ms.openlocfilehash: 6fef641cbc1e73ce34a851c53298a072dade8f9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-split-merge-service"></a><span data-ttu-id="ef0fc-103">Nasazení služby dělení a slučování</span><span class="sxs-lookup"><span data-stu-id="ef0fc-103">Deploy a split-merge service</span></span>
<span data-ttu-id="ef0fc-104">Hello rozdělení sloučení nástroj umožňuje přesun dat mezi horizontálně dělené databáze.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-104">hello split-merge tool lets you move data between sharded databases.</span></span> <span data-ttu-id="ef0fc-105">V tématu [přesouvání dat mezi instancemi cloudu databáze](sql-database-elastic-scale-overview-split-and-merge.md)</span><span class="sxs-lookup"><span data-stu-id="ef0fc-105">See [Moving data between scaled-out cloud databases](sql-database-elastic-scale-overview-split-and-merge.md)</span></span>

## <a name="download-hello-split-merge-packages"></a><span data-ttu-id="ef0fc-106">Stáhnout balíčky rozdělení sloučení hello</span><span class="sxs-lookup"><span data-stu-id="ef0fc-106">Download hello Split-Merge packages</span></span>
1. <span data-ttu-id="ef0fc-107">Stažení nejnovější verze NuGet hello z [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="ef0fc-107">Download hello latest NuGet version from [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>
2. <span data-ttu-id="ef0fc-108">Otevřete příkazový řádek a přejděte toohello adresáře, kam jste stáhli nuget.exe.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-108">Open a command prompt and navigate toohello directory where you downloaded nuget.exe.</span></span> <span data-ttu-id="ef0fc-109">zahrnuje Hello stažení commmands prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-109">hello download includes PowerShell commmands.</span></span>
3. <span data-ttu-id="ef0fc-110">Stáhněte si nejnovější balíček rozdělení sloučení hello do aktuální adresář hello s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ef0fc-110">Download hello latest Split-Merge package into hello current directory with hello below command:</span></span>
   ```
   nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge
   ```  

<span data-ttu-id="ef0fc-111">Hello soubory jsou umístěny v adresáři s názvem **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** kde *x.x.xxx.x* zobrazuje číslo verze hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-111">hello files are placed in a directory named **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** where *x.x.xxx.x* reflects hello version number.</span></span> <span data-ttu-id="ef0fc-112">Najde hello soubory služby rozdělení sloučení v hello **content\splitmerge\service** podadresář a hello skriptů prostředí PowerShell rozdělení sloučení (a knihoven DLL klienta) v hello **content\splitmerge\powershell** podadresář.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-112">Find hello split-merge Service files in hello **content\splitmerge\service** sub-directory, and hello Split-Merge PowerShell scripts (and required client .dlls) in hello **content\splitmerge\powershell** sub-directory.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef0fc-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ef0fc-113">Prerequisites</span></span>
1. <span data-ttu-id="ef0fc-114">Vytvořte databázi Azure SQL DB, který se použije jako hello rozdělení sloučení stavu databáze.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-114">Create an Azure SQL DB database that will be used as hello split-merge status database.</span></span> <span data-ttu-id="ef0fc-115">Přejděte toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ef0fc-115">Go toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="ef0fc-116">Vytvořte novou **SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-116">Create a new **SQL Database**.</span></span> <span data-ttu-id="ef0fc-117">Zadejte název databáze hello a vytvořte nový správce a heslo.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-117">Give hello database a name and create a new administrator and password.</span></span> <span data-ttu-id="ef0fc-118">Být, že toorecord hello jméno a heslo pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-118">Be sure toorecord hello name and password for later use.</span></span>
2. <span data-ttu-id="ef0fc-119">Zajistěte, aby váš server Azure SQL DB povolovalo tooit tooconnect služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-119">Ensure that your Azure SQL DB server allows Azure Services tooconnect tooit.</span></span> <span data-ttu-id="ef0fc-120">V portálu hello hello **nastavení brány Firewall**, ujistěte se, že hello **povolit přístup k službám tooAzure** nastavení je příliš**na**.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-120">In hello portal, in hello **Firewall Settings**, ensure that hello **Allow access tooAzure Services** setting is set too**On**.</span></span> <span data-ttu-id="ef0fc-121">Klikněte na "ikonu uložit" hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-121">Click hello "save" icon.</span></span>
   
   ![Povolené služby][1]
3. <span data-ttu-id="ef0fc-123">Vytvoření účtu úložiště Azure, který se použije pro výstup diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-123">Create an Azure Storage account that will be used for diagnostics output.</span></span> <span data-ttu-id="ef0fc-124">Přejděte toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-124">Go toohello Azure Portal.</span></span> <span data-ttu-id="ef0fc-125">Hello levém panelu klikněte na **nový**, klikněte na tlačítko **Data + úložiště**, pak **úložiště**.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-125">In hello left bar, click **New**, click **Data + Storage**, then **Storage**.</span></span>
4. <span data-ttu-id="ef0fc-126">Vytvoření cloudové služby Azure, která bude obsahovat služby rozdělení sloučení.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-126">Create an Azure Cloud Service that will contain your Split-Merge service.</span></span>  <span data-ttu-id="ef0fc-127">Přejděte toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-127">Go toohello Azure Portal.</span></span> <span data-ttu-id="ef0fc-128">Hello levém panelu klikněte na **nový**, pak **výpočetní**, **Cloudová služba**, a **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-128">In hello left bar, click **New**, then **Compute**, **Cloud Service**, and **Create**.</span></span> 

## <a name="configure-your-split-merge-service"></a><span data-ttu-id="ef0fc-129">Konfigurace služby rozdělení sloučení</span><span class="sxs-lookup"><span data-stu-id="ef0fc-129">Configure your Split-Merge service</span></span>
### <a name="split-merge-service-configuration"></a><span data-ttu-id="ef0fc-130">Konfigurace služby rozdělení sloučení</span><span class="sxs-lookup"><span data-stu-id="ef0fc-130">Split-Merge service configuration</span></span>
1. <span data-ttu-id="ef0fc-131">Hello složky, kam jste stáhli hello rozdělení sloučení sestavení, vytvořit kopii hello **ServiceConfiguration.Template.cscfg** soubor, který se dodává spolu s **SplitMergeService.cspkg** a přejmenujte ji **Souboru ServiceConfiguration.cscfg**.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-131">In hello folder where you downloaded hello Split-Merge assemblies, create a copy of hello **ServiceConfiguration.Template.cscfg** file that shipped alongside **SplitMergeService.cspkg** and rename it **ServiceConfiguration.cscfg**.</span></span>
2. <span data-ttu-id="ef0fc-132">Otevřete **souboru ServiceConfiguration.cscfg** v textovém editoru, jako je například Visual Studio, která ověřuje vstupy například hello formát kryptografické otisky certifikátu.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-132">Open **ServiceConfiguration.cscfg** in a text editor such as Visual Studio that validates inputs such as hello format of certificate thumbprints.</span></span>
3. <span data-ttu-id="ef0fc-133">Vytvořit novou databázi nebo vyberte existující databázi tooserve jako hello stav databáze pro operace sloučení rozdělení a načtení připojovacího řetězce hello této databáze.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-133">Create a new database or choose an existing database tooserve as hello status database for Split-Merge operations and retrieve hello connection string of that database.</span></span> 
   
   > [!IMPORTANT]
   > <span data-ttu-id="ef0fc-134">V tomto okamžiku hello stav databáze musí používat kolaci Latin hello (SQL\_Latin1\_Obecné\_CP1\_CI\_AS).</span><span class="sxs-lookup"><span data-stu-id="ef0fc-134">At this time, hello status database must use hello Latin  collation (SQL\_Latin1\_General\_CP1\_CI\_AS).</span></span> <span data-ttu-id="ef0fc-135">Další informace najdete v tématu [název kolace systému Windows (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx).</span><span class="sxs-lookup"><span data-stu-id="ef0fc-135">For more information, see [Windows Collation Name (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx).</span></span>
   >

   <span data-ttu-id="ef0fc-136">S Azure SQL DB hello připojovací řetězec je obvykle ve formátu hello:</span><span class="sxs-lookup"><span data-stu-id="ef0fc-136">With Azure SQL DB, hello connection string typically is of hello form:</span></span>
      ```
      Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
      ```

4. <span data-ttu-id="ef0fc-137">Zadejte tento připojovací řetězec v hello soubor .cscfg v obou hello **SplitMergeWeb** a **SplitMergeWorker** role části hello ElasticScaleMetadata nastavení.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-137">Enter this connection string in hello cscfg file in both hello **SplitMergeWeb** and **SplitMergeWorker** role sections in hello ElasticScaleMetadata setting.</span></span>
5. <span data-ttu-id="ef0fc-138">Pro hello **SplitMergeWorker** role, zadejte platný připojovací řetězec tooAzure úložiště pro hello **WorkerRoleSynchronizationStorageAccountConnectionString** nastavení.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-138">For hello **SplitMergeWorker** role, enter a valid connection string tooAzure storage for hello **WorkerRoleSynchronizationStorageAccountConnectionString** setting.</span></span>

### <a name="configure-security"></a><span data-ttu-id="ef0fc-139">Konfigurace zabezpečení</span><span class="sxs-lookup"><span data-stu-id="ef0fc-139">Configure security</span></span>
<span data-ttu-id="ef0fc-140">Podrobné pokyny tooconfigure hello zabezpečení hello služby, najdete v části toohello [konfigurace zabezpečení rozdělení sloučení](sql-database-elastic-scale-split-merge-security-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="ef0fc-140">For detailed instructions tooconfigure hello security of hello service, refer toohello [Split-Merge security configuration](sql-database-elastic-scale-split-merge-security-configuration.md).</span></span>

<span data-ttu-id="ef0fc-141">Pro účely hello jednoduchá testovací nasazení pro tento kurz provést nejzákladnější konfigurace, která bude kroky tooget hello služby fungovaly.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-141">For hello purposes of a simple test deployment for this tutorial, a minimal set of configuration steps will be performed tooget hello service up and running.</span></span> <span data-ttu-id="ef0fc-142">Tyto kroky aktivují jenom hello jeden počítač nebo účet jejich spuštění toocommunicate službou hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-142">These steps enable only hello one machine/account executing them toocommunicate with hello service.</span></span>

### <a name="create-a-self-signed-certificate"></a><span data-ttu-id="ef0fc-143">Vytvořit certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="ef0fc-143">Create a self-signed certificate</span></span>
<span data-ttu-id="ef0fc-144">Vytvořte nový adresář a z tohoto adresáře execute hello následující pomocí příkazu [příkazový řádek vývojáře pro sadu Visual Studio](http://msdn.microsoft.com/library/ms229859.aspx) okno:</span><span class="sxs-lookup"><span data-stu-id="ef0fc-144">Create a new directory and from this directory execute hello following command using a [Developer Command Prompt for Visual Studio](http://msdn.microsoft.com/library/ms229859.aspx) window:</span></span>

   ```
    makecert ^
    -n "CN=*.cloudapp.net" ^
    -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2" ^
    -a sha1 -len 2048 ^
    -sr currentuser -ss root ^
    -sv MyCert.pvk MyCert.cer
   ```

<span data-ttu-id="ef0fc-145">Zobrazí se výzva pro heslo tooprotect hello privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-145">You are asked for a password tooprotect hello private key.</span></span> <span data-ttu-id="ef0fc-146">Zadejte silné heslo a potvrďte ho.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-146">Enter a strong password and confirm it.</span></span> <span data-ttu-id="ef0fc-147">Potom budete vyzváni k toobe hello heslo použít jednou po který.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-147">You are then prompted for hello password toobe used once more after that.</span></span> <span data-ttu-id="ef0fc-148">Klikněte na tlačítko **Ano** v hello end tooimport ho toohello úložiště Důvěryhodné kořenové certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-148">Click **Yes** at hello end tooimport it toohello Trusted Certification Authorities Root store.</span></span>

### <a name="create-a-pfx-file"></a><span data-ttu-id="ef0fc-149">Vytvořte soubor PFX</span><span class="sxs-lookup"><span data-stu-id="ef0fc-149">Create a PFX file</span></span>
<span data-ttu-id="ef0fc-150">Spustit následující příkaz z hello hello stejné okno, kde byl proveden makecert; použití hello stejné heslo, můžete použít toocreate hello certifikátu:</span><span class="sxs-lookup"><span data-stu-id="ef0fc-150">Execute hello following command from hello same window where makecert was executed; use hello same password that you used toocreate hello certificate:</span></span>

    pvk2pfx -pvk MyCert.pvk -spc MyCert.cer -pfx MyCert.pfx -pi <password>

### <a name="import-hello-client-certificate-into-hello-personal-store"></a><span data-ttu-id="ef0fc-151">Importovat hello klientský certifikát do osobního úložiště hello</span><span class="sxs-lookup"><span data-stu-id="ef0fc-151">Import hello client certificate into hello personal store</span></span>
1. <span data-ttu-id="ef0fc-152">V Průzkumníku Windows, klikněte dvakrát na **MyCert.pfx**.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-152">In Windows Explorer, double-click **MyCert.pfx**.</span></span>
2. <span data-ttu-id="ef0fc-153">V hello **Průvodce importem certifikátu** vyberte **aktuální uživatel** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-153">In hello **Certificate Import Wizard** select **Current User** and click **Next**.</span></span>
3. <span data-ttu-id="ef0fc-154">Potvrďte hello cesta k souboru a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-154">Confirm hello file path and click **Next**.</span></span>
4. <span data-ttu-id="ef0fc-155">Zadejte hello heslo, nechte **obsahovat všechny rozšířené vlastnosti** zaškrtnutí a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-155">Type hello password, leave **Include all extended properties** checked and click **Next**.</span></span>
5. <span data-ttu-id="ef0fc-156">Nechte **úložiště certifikátů automaticky vybere hello [...]**  zaškrtnutí a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-156">Leave **Automatically select hello certificate store[…]** checked and click **Next**.</span></span>
6. <span data-ttu-id="ef0fc-157">Klikněte na tlačítko **Dokončit** a **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-157">Click **Finish** and **OK**.</span></span>

### <a name="upload-hello-pfx-file-toohello-cloud-service"></a><span data-ttu-id="ef0fc-158">Nahrát hello PFX souboru toohello cloudové služby</span><span class="sxs-lookup"><span data-stu-id="ef0fc-158">Upload hello PFX file toohello cloud service</span></span>
1. <span data-ttu-id="ef0fc-159">Přejděte toohello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ef0fc-159">Go toohello [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ef0fc-160">Vyberte **cloudových služeb**.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-160">Select **Cloud Services**.</span></span>
3. <span data-ttu-id="ef0fc-161">Vyberte hello cloudovou službu, kterou jste vytvořili výše pro službu rozdělení či sloučení hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-161">Select hello cloud service you created above for hello Split/Merge service.</span></span>
4. <span data-ttu-id="ef0fc-162">Klikněte na tlačítko **certifikáty** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-162">Click **Certificates** on hello top menu.</span></span>
5. <span data-ttu-id="ef0fc-163">Klikněte na tlačítko **nahrát** v dolním panelu hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-163">Click **Upload** in hello bottom bar.</span></span>
6. <span data-ttu-id="ef0fc-164">Vyberte soubor PFX hello a zadejte hello stejné heslo, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-164">Select hello PFX file and enter hello same password as above.</span></span>
7. <span data-ttu-id="ef0fc-165">Po dokončení kopírování hello kryptografický otisk certifikátu z hello novou položku v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-165">Once completed, copy hello certificate thumbprint from hello new entry in hello list.</span></span>

### <a name="update-hello-service-configuration-file"></a><span data-ttu-id="ef0fc-166">Aktualizovat konfigurační soubor služby hello</span><span class="sxs-lookup"><span data-stu-id="ef0fc-166">Update hello service configuration file</span></span>
<span data-ttu-id="ef0fc-167">Vložte kryptografický otisk certifikátu hello výše zkopírují do atribut hello kryptografický otisk nebo hodnotu z těchto nastavení.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-167">Paste hello certificate thumbprint copied above into hello thumbprint/value attribute of these settings.</span></span>
<span data-ttu-id="ef0fc-168">Pro roli pracovního procesu hello:</span><span class="sxs-lookup"><span data-stu-id="ef0fc-168">For hello worker role:</span></span>
   ```
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

<span data-ttu-id="ef0fc-169">Pro roli webové hello:</span><span class="sxs-lookup"><span data-stu-id="ef0fc-169">For hello web role:</span></span>

   ```
    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />
    <Setting name="AllowedClientCertificateThumbprints" value="" />
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

<span data-ttu-id="ef0fc-170">Upozorňujeme, že pro nasazení v produkčním prostředí samostatné certifikáty se mají použít pro hello certifikační Autority, k šifrování, hello certifikátu serveru a klientské certifikáty.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-170">Please note that for production deployments separate certificates should be used for hello CA, for encryption, hello Server certificate and client certificates.</span></span> <span data-ttu-id="ef0fc-171">Podrobné pokyny v tomto najdete v tématu [konfigurace zabezpečení](sql-database-elastic-scale-split-merge-security-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="ef0fc-171">For detailed instructions on this, see [Security Configuration](sql-database-elastic-scale-split-merge-security-configuration.md).</span></span>

## <a name="deploy-your-service"></a><span data-ttu-id="ef0fc-172">Nasazení služby</span><span class="sxs-lookup"><span data-stu-id="ef0fc-172">Deploy your service</span></span>
1. <span data-ttu-id="ef0fc-173">Přejděte toohello [portál Azure](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="ef0fc-173">Go toohello [Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="ef0fc-174">Klikněte na tlačítko hello **cloudové služby** na levé straně hello a vyberte hello Cloudová služba, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-174">Click hello **Cloud Services** tab on hello left, and select hello cloud service that you created earlier.</span></span>
3. <span data-ttu-id="ef0fc-175">Klikněte na tlačítko **řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-175">Click **Dashboard**.</span></span>
4. <span data-ttu-id="ef0fc-176">Zvolte hello pracovní prostředí, a klikněte na **nahrát nový pracovní nasazení**.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-176">Choose hello staging environment, then click **Upload a new staging deployment**.</span></span>
   
   ![Pracovní][3]
5. <span data-ttu-id="ef0fc-178">V dialogovém okně hello zadejte označení nasazení.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-178">In hello dialog box, enter a deployment label.</span></span> <span data-ttu-id="ef0fc-179">Pro 'Balíček' i 'konfiguraci, klikněte na tlačítko 'Z Local' a vyberte hello **SplitMergeService.cspkg** Souborová služba a který jste dříve nakonfigurovali souboru .cscfg.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-179">For both 'Package' and 'Configuration', click 'From Local' and choose hello **SplitMergeService.cspkg** file and your .cscfg file that you configured earlier.</span></span>
6. <span data-ttu-id="ef0fc-180">Ujistěte se, toto zaškrtávací políčko hello s názvem bez přípony **nasadit, i když jedna nebo víc rolí obsahuje jednu instanci** je zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-180">Ensure that hello checkbox labeled **Deploy even if one or more roles contain a single instance** is checked.</span></span>
7. <span data-ttu-id="ef0fc-181">Stiskněte tlačítko hello značek v hello dolní správné toobegin hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-181">Hit hello tick button in hello bottom right toobegin hello deployment.</span></span> <span data-ttu-id="ef0fc-182">Jeho očekávat tootake toocomplete několik minut.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-182">Expect it tootake a few minutes toocomplete.</span></span>

   ![Odeslat][4]

## <a name="troubleshoot-hello-deployment"></a><span data-ttu-id="ef0fc-184">Řešení potíží s hello nasazení</span><span class="sxs-lookup"><span data-stu-id="ef0fc-184">Troubleshoot hello deployment</span></span>
<span data-ttu-id="ef0fc-185">Pokud vaši webovou roli selže toocome online, je pravděpodobně problém s konfigurací zabezpečení hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-185">If your web role fails toocome online, it is likely a problem with hello security configuration.</span></span> <span data-ttu-id="ef0fc-186">Zkontrolujte, že hello, který je nakonfigurovaný protokol SSL, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-186">Check that hello SSL is configured as described above.</span></span>

<span data-ttu-id="ef0fc-187">Pokud své role pracovního procesu selže toocome online, ale vaši webovou roli úspěšné, je pravděpodobně problém s připojením databáze toohello stav, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-187">If your worker role fails toocome online, but your web role succeeds, it is most likely a problem connecting toohello status database that you created earlier.</span></span>

* <span data-ttu-id="ef0fc-188">Ujistěte se, že hello připojovací řetězec v vašeho souboru .cscfg je přesný.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-188">Make sure that hello connection string in your .cscfg is accurate.</span></span>
* <span data-ttu-id="ef0fc-189">Zkontrolujte, že hello server a databáze existují a správnost hello id uživatele a heslo.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-189">Check that hello server and database exist, and that hello user id and password are correct.</span></span>
* <span data-ttu-id="ef0fc-190">Pro databáze SQL Azure musí mít připojovací řetězec hello hello formuláře:</span><span class="sxs-lookup"><span data-stu-id="ef0fc-190">For Azure SQL DB, hello connection string should be of hello form:</span></span>

   ```  
   Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
   ```

* <span data-ttu-id="ef0fc-191">Ujistěte se, že název serveru hello nezačíná **https://**.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-191">Ensure that hello server name does not begin with **https://**.</span></span>
* <span data-ttu-id="ef0fc-192">Zajistěte, aby váš server Azure SQL DB povolovalo tooit tooconnect služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-192">Ensure that your Azure SQL DB server allows Azure Services tooconnect tooit.</span></span> <span data-ttu-id="ef0fc-193">toodo, otevřete https://manage.windowsazure.com, na tlačítko "Databází SQL" na hello vlevo, klikněte na tlačítko "Servery" hello horní a vyberte svůj server.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-193">toodo this, open https://manage.windowsazure.com, click “SQL Databases” on hello left, click “Servers” at hello top, and select your server.</span></span> <span data-ttu-id="ef0fc-194">Klikněte na tlačítko **konfigurace** v hello top a ujistěte se, že hello **služeb Azure** nastavení je příliš "Ano".</span><span class="sxs-lookup"><span data-stu-id="ef0fc-194">Click **Configure** at hello top and ensure that hello **Azure Services** setting is set too“Yes”.</span></span> <span data-ttu-id="ef0fc-195">(Viz požadavky hello na hello horní části tohoto článku v části).</span><span class="sxs-lookup"><span data-stu-id="ef0fc-195">(See hello Prerequisites section at hello top of this article).</span></span>

## <a name="test-hello-service-deployment"></a><span data-ttu-id="ef0fc-196">Testovací nasazení pro službu hello</span><span class="sxs-lookup"><span data-stu-id="ef0fc-196">Test hello service deployment</span></span>
### <a name="connect-with-a-web-browser"></a><span data-ttu-id="ef0fc-197">Připojení s webovým prohlížečem</span><span class="sxs-lookup"><span data-stu-id="ef0fc-197">Connect with a web browser</span></span>
<span data-ttu-id="ef0fc-198">Určení hello koncový bod webové služby rozdělení sloučení.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-198">Determine hello web endpoint of your Split-Merge service.</span></span> <span data-ttu-id="ef0fc-199">To můžete najít v hello portálu Azure Classic podle budete toohello **řídicí panel** cloudové služby a vyhledávání v části **adresa URL webu** na pravé straně hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-199">You can find this in hello Azure Classic Portal by going toohello **Dashboard** of your cloud service and looking under **Site URL** on hello right side.</span></span> <span data-ttu-id="ef0fc-200">Nahraďte **http://** s **https://** vzhledem k tomu, že výchozí nastavení zabezpečení hello zakázat hello HTTP koncový bod.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-200">Replace **http://** with **https://** since hello default security settings disable hello HTTP endpoint.</span></span> <span data-ttu-id="ef0fc-201">Načíst stránku hello pro tuto adresu URL do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-201">Load hello page for this URL into your browser.</span></span>

### <a name="test-with-powershell-scripts"></a><span data-ttu-id="ef0fc-202">Testování pomocí skriptů prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ef0fc-202">Test with PowerShell scripts</span></span>
<span data-ttu-id="ef0fc-203">Hello nasazení a prostředí můžete otestovat pomocí spouštění skriptů prostředí PowerShell ukázkový hello zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-203">hello deployment and your environment can be tested by running hello included sample PowerShell scripts.</span></span>

<span data-ttu-id="ef0fc-204">soubory skriptu Hello zahrnuté jsou:</span><span class="sxs-lookup"><span data-stu-id="ef0fc-204">hello script files included are:</span></span>

1. <span data-ttu-id="ef0fc-205">**SetupSampleSplitMergeEnvironment.ps1** -nastaví datovou vrstvu testu pro rozdělení/Merge (v tabulce níže najdete podrobný popis)</span><span class="sxs-lookup"><span data-stu-id="ef0fc-205">**SetupSampleSplitMergeEnvironment.ps1** - sets up a test data tier for Split/Merge (see table below for detailed description)</span></span>
2. <span data-ttu-id="ef0fc-206">**ExecuteSampleSplitMerge.ps1** -provádí operace test na hello testovací datové vrstvy (v tabulce níže najdete podrobný popis)</span><span class="sxs-lookup"><span data-stu-id="ef0fc-206">**ExecuteSampleSplitMerge.ps1** - executes test operations on hello test data tier (see table below for detailed description)</span></span>
3. <span data-ttu-id="ef0fc-207">**GetMappings.ps1** – nejvyšší úrovně ukázkový skript, který vytiskne hello aktuální stav mapování horizontálních hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-207">**GetMappings.ps1** - top-level sample script that prints out hello current state of hello shard mappings.</span></span>
4. <span data-ttu-id="ef0fc-208">**ShardManagement.psm1** – Pomocník skript, který zabalí hello ShardManagement rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ef0fc-208">**ShardManagement.psm1**  - helper script that wraps hello ShardManagement API</span></span>
5. <span data-ttu-id="ef0fc-209">**SqlDatabaseHelpers.psm1** – Pomocník skriptu pro vytváření a správu databází SQL</span><span class="sxs-lookup"><span data-stu-id="ef0fc-209">**SqlDatabaseHelpers.psm1** - helper script for creating and managing SQL databases</span></span>
   
   <table style="width:100%">
     <tr>
       <th><span data-ttu-id="ef0fc-210">Soubor PowerShell</span><span class="sxs-lookup"><span data-stu-id="ef0fc-210">PowerShell file</span></span></th>
       <th><span data-ttu-id="ef0fc-211">Kroky</span><span class="sxs-lookup"><span data-stu-id="ef0fc-211">Steps</span></span></th>
     </tr>
     <tr>
       <th rowspan="5"><span data-ttu-id="ef0fc-212">SetupSampleSplitMergeEnvironment.ps1</span><span class="sxs-lookup"><span data-stu-id="ef0fc-212">SetupSampleSplitMergeEnvironment.ps1</span></span></th>
       <td>1.    <span data-ttu-id="ef0fc-213">Vytvoří databázi správce horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="ef0fc-213">Creates a shard map manager database</span></span></td>
     </tr>
     <tr>
       <td>2.    <span data-ttu-id="ef0fc-214">Vytvoří 2 horizontálního oddílu databáze.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-214">Creates 2 shard databases.</span></span>
     </tr>
     <tr>
       <td>3.    <span data-ttu-id="ef0fc-215">Vytvoří mapu horizontálního oddílu pro tyto databáze (odstraní všechny existující horizontálního oddílu mapování na tyto databáze).</span><span class="sxs-lookup"><span data-stu-id="ef0fc-215">Creates a shard map for those database (deletes any existing shard maps on those databases).</span></span> </td>
     </tr>
     <tr>
       <td>4.    <span data-ttu-id="ef0fc-216">Vytvoří tabulku malé ukázkové v obou hello horizontálních oddílů a naplní hello tabulky v jednom z hello horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-216">Creates a small sample table in both hello shards, and populates hello table in one of hello shards.</span></span></td>
     </tr>
     <tr>
       <td>5.    <span data-ttu-id="ef0fc-217">Deklaruje hello SchemaInfo pro horizontálně dělenou tabulku hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-217">Declares hello SchemaInfo for hello sharded table.</span></span></td>
     </tr>
   </table>
   <table style="width:100%">
     <tr>
       <th><span data-ttu-id="ef0fc-218">Soubor PowerShell</span><span class="sxs-lookup"><span data-stu-id="ef0fc-218">PowerShell file</span></span></th>
       <th><span data-ttu-id="ef0fc-219">Kroky</span><span class="sxs-lookup"><span data-stu-id="ef0fc-219">Steps</span></span></th>
     </tr>
   <tr>
       <th rowspan="4"><span data-ttu-id="ef0fc-220">ExecuteSampleSplitMerge.ps1</span><span class="sxs-lookup"><span data-stu-id="ef0fc-220">ExecuteSampleSplitMerge.ps1</span></span> </th>
       <td>1.    <span data-ttu-id="ef0fc-221">Odešle rozdělení požadavek toohello rozdělení sloučení služby webové front-end, která rozdělí poloviční hello data z hello první horizontálního oddílu toohello druhý horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-221">Sends a split request toohello Split-Merge Service web frontend, which splits half hello data from hello first shard toohello second shard.</span></span></td>
     </tr>
     <tr>
       <td>2.    <span data-ttu-id="ef0fc-222">Hlasování hello front-endu webové pro hello rozdělení stavu požadavku a čeká, až do dokončení požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-222">Polls hello web frontend for hello split request status and waits until hello request completes.</span></span></td>
     </tr>
     <tr>
       <td>3.    <span data-ttu-id="ef0fc-223">Odešle sloučení požadavek toohello rozdělení sloučení služby webové front-end, který přesouvá hello data z hello druhý horizontálního oddílu back toohello první horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-223">Sends a merge request toohello Split-Merge Service web frontend, which moves hello data from hello second shard back toohello first shard.</span></span></td>
     </tr>
     <tr>
       <td>4.    <span data-ttu-id="ef0fc-224">Hlasování hello front-endu webové pro stav žádosti o sloučení hello a čeká, až do dokončení požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-224">Polls hello web frontend for hello merge request status and waits until hello request completes.</span></span></td>
     </tr>
   </table>
   
## <a name="use-powershell-tooverify-your-deployment"></a><span data-ttu-id="ef0fc-225">Pomocí prostředí PowerShell tooverify nasazení</span><span class="sxs-lookup"><span data-stu-id="ef0fc-225">Use PowerShell tooverify your deployment</span></span>
1. <span data-ttu-id="ef0fc-226">Otevřete nové okno prostředí PowerShell a přejděte toohello adresáře, kam jste stáhli balíček rozdělení sloučení hello a pak přejděte do adresáře "powershell" hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-226">Open a new PowerShell window and navigate toohello directory where you downloaded hello Split-Merge package, and then navigate into hello “powershell” directory.</span></span>
2. <span data-ttu-id="ef0fc-227">Vytvoření serveru Azure SQL database (nebo vyberte existující server) kde bude vytvořen hello horizontálního oddílu mapa správce a horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-227">Create an Azure SQL database server (or choose an existing server) where hello shard map manager and shards will be created.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="ef0fc-228">Hello SetupSampleSplitMergeEnvironment.ps1 skript vytvoří těchto databází na hello stejný server výchozí tookeep hello skript jednoduché.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-228">hello SetupSampleSplitMergeEnvironment.ps1 script creates all these databases on hello same server by default tookeep hello script simple.</span></span> <span data-ttu-id="ef0fc-229">Toto není omezení hello rozdělení sloučení služby sám sebe.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-229">This is not a restriction of hello Split-Merge Service itself.</span></span>
   >
   
   <span data-ttu-id="ef0fc-230">Přihlášení k ověřování SQL s toohello přístup pro čtení a zápis, Operations Manager bude nutná pro hello sloučení rozdělení dat toomove služby a mapovat horizontálního oddílu hello aktualizace.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-230">A SQL authentication login with read/write access toohello DBs will be needed for hello Split-Merge service toomove data and update hello shard map.</span></span> <span data-ttu-id="ef0fc-231">Vzhledem k tomu, že hello rozdělení sloučení služba běží v cloudu hello, nepodporuje aktuálně integrované ověřování.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-231">Since hello Split-Merge Service runs in hello cloud, it does not currently support Integrated Authentication.</span></span>
   
   <span data-ttu-id="ef0fc-232">Ověřte, zda je server Azure SQL hello nakonfigurována tooallow přístup z hello IP adresu počítače hello spuštění těchto skriptů.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-232">Make sure hello Azure SQL server is configured tooallow access from hello IP address of hello machine running these scripts.</span></span> <span data-ttu-id="ef0fc-233">Můžete najít toto nastavení v rámci serveru Azure SQL hello / configuration / povolené IP adresy.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-233">You can find this setting under hello Azure SQL server / configuration / allowed IP addresses.</span></span>
3. <span data-ttu-id="ef0fc-234">Spusťte hello SetupSampleSplitMergeEnvironment.ps1 skriptu toocreate hello ukázkové prostředí.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-234">Execute hello SetupSampleSplitMergeEnvironment.ps1 script toocreate hello sample environment.</span></span>
   
   <span data-ttu-id="ef0fc-235">Spuštěním tohoto skriptu budou vymazat všechny existující data správy mapy horizontálního oddílu struktury na hello horizontálního oddílu mapa správce databáze a hello horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-235">Running this script will wipe out any existing shard map management data structures on hello shard map manager database and hello shards.</span></span> <span data-ttu-id="ef0fc-236">To může být užitečné toorerun hello skriptu, pokud chcete inicializovat toore hello horizontálního oddílu mapy nebo horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-236">It may be useful toorerun hello script if you wish toore-initialize hello shard map or shards.</span></span>
   
   <span data-ttu-id="ef0fc-237">Ukázka příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="ef0fc-237">Sample command line:</span></span>

   ```   
     .\SetupSampleSplitMergeEnvironment.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'
   ```      
4. <span data-ttu-id="ef0fc-238">Spustíte hello Getmappings.ps1 skriptu tooview hello mapování, která momentálně existují v prostředí ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-238">Execute hello Getmappings.ps1 script tooview hello mappings that currently exist in hello sample environment.</span></span>
   
   ```
     .\GetMappings.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'

   ```         
5. <span data-ttu-id="ef0fc-239">Hello ExecuteSampleSplitMerge.ps1 skriptu tooexecute provést operaci rozdělení (přesouvání poloviční hello dat na hello první horizontálního oddílu toohello druhý horizontálních) a pak operace sloučení (přesouvání hello dat zpět na první horizontálního oddílu hello).</span><span class="sxs-lookup"><span data-stu-id="ef0fc-239">Execute hello ExecuteSampleSplitMerge.ps1 script tooexecute a split operation (moving half hello data on hello first shard toohello second shard) and then a merge operation (moving hello data back onto hello first shard).</span></span> <span data-ttu-id="ef0fc-240">Pokud jste nakonfigurovali, SSL a koncový bod http levém hello zakázáno, ujistěte se, použijte místo toho hello https:// koncový bod.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-240">If you configured SSL and left hello http endpoint disabled, ensure that you use hello https:// endpoint instead.</span></span>
   
   <span data-ttu-id="ef0fc-241">Ukázka příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="ef0fc-241">Sample command line:</span></span>

   ```   
     .\ExecuteSampleSplitMerge.ps1
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net' 
         -SplitMergeServiceEndpoint 'https://mysplitmergeservice.cloudapp.net' 
         -CertificateThumbprint '0123456789abcdef0123456789abcdef01234567'
   ```      
   
   <span data-ttu-id="ef0fc-242">Pokud se zobrazí hello níže chyba, je pravděpodobně k potížím s certifikátem váš koncový bod webové.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-242">If you receive hello below error, it is most likely a problem with your Web endpoint’s certificate.</span></span> <span data-ttu-id="ef0fc-243">Pokuste se připojit koncový bod webové toohello pomocí Oblíbené webový prohlížeč a zkontrolujte, jestli Chyba certifikátu.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-243">Try connecting toohello Web endpoint with your favorite Web browser and check if there is a certificate error.</span></span>
   
     ```
     Invoke-WebRequest : hello underlying connection was closed: Could not establish trust relationship for hello SSL/TLSsecure channel.
     ```
   
   <span data-ttu-id="ef0fc-244">Pokud byly úspěšné, hello výstup by měl vypadat jako hello níže:</span><span class="sxs-lookup"><span data-stu-id="ef0fc-244">If it succeeded, hello output should look like hello below:</span></span>
   
   ```
   > .\ExecuteSampleSplitMerge.ps1 -UserName 'mysqluser' -Password 'MySqlPassw0rd' -ShardMapManagerServerName 'abcdefghij.database.windows.net' -SplitMergeServiceEndpoint 'http://mysplitmergeservice.cloudapp.net' -CertificateThumbprint 0123456789abcdef0123456789abcdef01234567
   > Sending split request
   > Began split operation with id dc68dfa0-e22b-4823-886a-9bdc903c80f3
   > Polling split-merge request status. Press Ctrl-C tooend
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source tootarget shard.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Waiting for reference tables copy     completion.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source tootarget shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing hello request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > Sending merge request
   > Began merge operation with id 6ffc308f-d006-466b-b24e-857242ec5f66
   > Polling request status. Press Ctrl-C tooend
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source tootarget shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing hello request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > 
   ```
6. <span data-ttu-id="ef0fc-245">Experimentujte s jiné datové typy!</span><span class="sxs-lookup"><span data-stu-id="ef0fc-245">Experiment with other data types!</span></span> <span data-ttu-id="ef0fc-246">Všechny tyto skripty trvat volitelný parametr - ShardKeyType, který vám umožní typ klíče toospecify hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-246">All of these scripts take an optional -ShardKeyType parameter that allows you toospecify hello key type.</span></span> <span data-ttu-id="ef0fc-247">Výchozí hodnota Hello je Int32, ale můžete také určit Int64, identifikátor Guid nebo Binary.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-247">hello default is Int32, but you can also specify Int64, Guid, or Binary.</span></span>

## <a name="create-requests"></a><span data-ttu-id="ef0fc-248">Požadavky na vytvoření</span><span class="sxs-lookup"><span data-stu-id="ef0fc-248">Create requests</span></span>
<span data-ttu-id="ef0fc-249">Hello služby lze pomocí hello webového uživatelského rozhraní nebo importováním a modulu SplitMerge.psm1 PowerShell hello, které se budou odesílat své žádosti prostřednictvím hello webové role.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-249">hello service can be used either by using hello web UI or by importing and using hello SplitMerge.psm1 PowerShell module which will submit your requests through hello web role.</span></span>

<span data-ttu-id="ef0fc-250">Hello služby můžete přesouvat data v horizontálně dělené tabulky a referenční tabulky.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-250">hello service can move data in both sharded tables and reference tables.</span></span> <span data-ttu-id="ef0fc-251">Horizontálně dělenou tabulku má klíčový sloupec horizontálního dělení a má jiný řádek dat v každém horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-251">A sharded table has a sharding key column and has different row data on each shard.</span></span> <span data-ttu-id="ef0fc-252">Referenční tabulka není horizontálně dělené tak, aby obsahovala hello stejný řádek dat v každé horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-252">A reference table is not sharded so it contains hello same row data on every shard.</span></span> <span data-ttu-id="ef0fc-253">Referenční tabulky jsou užitečné pro data, která se nemění často a je použité tooJOIN s horizontálně dělené tabulky v dotazech.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-253">Reference tables are useful for data that does not change often and is used tooJOIN with sharded tables in queries.</span></span>

<span data-ttu-id="ef0fc-254">V pořadí tooperform operace sloučení rozdělení musí deklarovat hello horizontálně dělené tabulky a referenční tabulky, které chcete přesunout toohave.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-254">In order tooperform a split-merge operation, you must declare hello sharded tables and reference tables that you want toohave moved.</span></span> <span data-ttu-id="ef0fc-255">To je provedeno pomocí hello **SchemaInfo** rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-255">This is accomplished with hello **SchemaInfo** API.</span></span> <span data-ttu-id="ef0fc-256">Toto rozhraní API je v hello **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-256">This API is in hello **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** namespace.</span></span>

1. <span data-ttu-id="ef0fc-257">Pro každou horizontálně dělenou tabulku, vytvořte **ShardedTableInfo** objekt popisující název schématu nadřazená tabulka hello (volitelné, výchozí příliš "dbo"), hello název tabulky a název sloupce v této tabulce, který obsahuje klíč horizontálního dělení hello hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-257">For each sharded table, create a **ShardedTableInfo** object describing hello table’s parent schema name (optional, defaults too“dbo”), hello table name, and hello column name in that table that contains hello sharding key.</span></span>
2. <span data-ttu-id="ef0fc-258">Pro každý odkaz na tabulku vytvořit **ReferenceTableInfo** objekt popisující název schématu nadřazená tabulka hello (volitelné, použije se výchozí hodnota příliš "dbo") a název tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-258">For each reference table, create a **ReferenceTableInfo** object describing hello table’s parent schema name (optional, defaults too“dbo”) and hello table name.</span></span>
3. <span data-ttu-id="ef0fc-259">Přidat hello výše TableInfo objekty tooa nové **SchemaInfo** objektu.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-259">Add hello above TableInfo objects tooa new **SchemaInfo** object.</span></span>
4. <span data-ttu-id="ef0fc-260">Získat odkaz na tooa **ShardMapManager** objekt a volání **GetSchemaInfoCollection**.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-260">Get a reference tooa **ShardMapManager** object, and call **GetSchemaInfoCollection**.</span></span>
5. <span data-ttu-id="ef0fc-261">Přidat hello **SchemaInfo** toohello **SchemaInfoCollection**, poskytuje název pro mapování horizontálních hello.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-261">Add hello **SchemaInfo** toohello **SchemaInfoCollection**, providing hello shard map name.</span></span>

<span data-ttu-id="ef0fc-262">Příklad toho si můžete prohlédnout ve hello SetupSampleSplitMergeEnvironment.ps1 skriptu.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-262">An example of this can be seen in hello SetupSampleSplitMergeEnvironment.ps1 script.</span></span>

<span data-ttu-id="ef0fc-263">Hello rozdělení sloučení služby nevytvoří hello cílová databáze (nebo schéma pro všechny tabulky v databázi hello) za vás.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-263">hello Split-Merge service does not create hello target database (or schema for any tables in hello database) for you.</span></span> <span data-ttu-id="ef0fc-264">Musí být předem vytvořené před odesláním požadavku toohello služby.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-264">They must be pre-created before sending a request toohello service.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="ef0fc-265">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="ef0fc-265">Troubleshooting</span></span>
<span data-ttu-id="ef0fc-266">Při spouštění skriptů prostředí powershell ukázkový text hello, mohou se zobrazit hello následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="ef0fc-266">You may see hello below message when running hello sample powershell scripts:</span></span>

   ```
   Invoke-WebRequest : hello underlying connection was closed: Could not establish trust relationship for hello SSL/TLS secure channel.
   ```

<span data-ttu-id="ef0fc-267">Tato chyba znamená, že svůj certifikát SSL není správně nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-267">This error means that your SSL certificate is not configured correctly.</span></span> <span data-ttu-id="ef0fc-268">Postupujte podle pokynů hello v části "připojení s webovým prohlížečem.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-268">Please follow hello instructions in section 'Connecting with a web browser'.</span></span>

<span data-ttu-id="ef0fc-269">Pokud nelze odeslat požadavky mohou se zobrazit tato:</span><span class="sxs-lookup"><span data-stu-id="ef0fc-269">If you cannot submit requests you may see this:</span></span>

```
[Exception] System.Data.SqlClient.SqlException (0x80131904): Could not find stored procedure 'dbo.InsertRequest'. 
```

<span data-ttu-id="ef0fc-270">V takovém případě zkontrolujte konfigurační soubor, v nastavení konkrétní hello **WorkerRoleSynchronizationStorageAccountConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-270">In this case, check your configuration file, in particular hello setting for **WorkerRoleSynchronizationStorageAccountConnectionString**.</span></span> <span data-ttu-id="ef0fc-271">Tato chyba obvykle značí, že tato role pracovního procesu hello nelze úspěšně spustit hello metadata databáze při prvním použití.</span><span class="sxs-lookup"><span data-stu-id="ef0fc-271">This error typically indicates that hello worker role could not successfully initialize hello metadata database on first use.</span></span> 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/allowed-services.png
[2]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/manage.png
[3]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/staging.png
[4]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/upload.png
[5]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/storage.png

