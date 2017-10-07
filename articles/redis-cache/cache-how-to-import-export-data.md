---
title: "aaaImport a exportu dat ve službě Azure Redis Cache | Microsoft Docs"
description: "Zjistěte, jak tooimport a export tooand dat z úložiště objektů blob s vaší instancí Azure Redis Cache premium"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 4a68ac38-87af-4075-adab-569d37d7cc9e
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: sdanie
ms.openlocfilehash: f17464b207f1c652952f4da63ca147473fee2759
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="import-and-export-data-in-azure-redis-cache"></a><span data-ttu-id="0714b-103">Import a Export dat ve službě Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="0714b-103">Import and Export data in Azure Redis Cache</span></span>
<span data-ttu-id="0714b-104">Import a Export je Azure Redis Cache operace správy dat, která vám umožní tooimport data do Azure Redis Cache nebo exportu dat z Azure Redis Cache pomocí import a Export snímku databáze Redis Cache (RDB) z mezipaměti premium tooa objektu blob v Azure Účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="0714b-104">Import/Export is an Azure Redis Cache data management operation, which allows you tooimport data into Azure Redis Cache or export data from Azure Redis Cache by importing and exporting a Redis Cache Database (RDB) snapshot from a premium cache tooa blob in an Azure Storage Account.</span></span> 

- <span data-ttu-id="0714b-105">**Export** -exportujete vaší Azure Redis Cache RDB snímky tooa objekt Blob stránky.</span><span class="sxs-lookup"><span data-stu-id="0714b-105">**Export** - you can export your Azure Redis Cache RDB snapshots tooa Page Blob.</span></span>
- <span data-ttu-id="0714b-106">**Import** -vaší RDB mezipaměti Redis snímky můžete importovat z objekt Blob stránky nebo objekt Blob bloku.</span><span class="sxs-lookup"><span data-stu-id="0714b-106">**Import** - you can import your Redis Cache RDB snapshots from either a Page Blob or a Block Blob.</span></span>

<span data-ttu-id="0714b-107">Import a Export vám umožní toomigrate mezi různými instancemi Azure Redis Cache nebo naplňte hello mezipaměť s daty před použitím.</span><span class="sxs-lookup"><span data-stu-id="0714b-107">Import/Export enables you toomigrate between different Azure Redis Cache instances or populate hello cache with data before use.</span></span>

<span data-ttu-id="0714b-108">Tento článek obsahuje Průvodce pro import a export dat pomocí Azure Redis Cache a poskytuje hello odpovědi toocommonly dotazy.</span><span class="sxs-lookup"><span data-stu-id="0714b-108">This article provides a guide for importing and exporting data with Azure Redis Cache and provides hello answers toocommonly asked questions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0714b-109">Import a Export je ve verzi preview a je dostupná jenom pro [úroveň premium](cache-premium-tier-intro.md) ukládá do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="0714b-109">Import/Export is in preview and is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span>
>
>

## <a name="import"></a><span data-ttu-id="0714b-110">Import</span><span class="sxs-lookup"><span data-stu-id="0714b-110">Import</span></span>
<span data-ttu-id="0714b-111">Import lze použít toobring Redis kompatibilní RDB soubory z jakéhokoli Redis serveru se systémem v libovolném cloudu nebo prostředí, včetně Redis systémem Linux, Windows nebo kteréhokoli poskytovatele cloudových služeb jako Amazon Web Services a dalších.</span><span class="sxs-lookup"><span data-stu-id="0714b-111">Import can be used toobring Redis compatible RDB files from any Redis server running in any cloud or environment, including Redis running on Linux, Windows, or any cloud provider such as Amazon Web Services and others.</span></span> <span data-ttu-id="0714b-112">Import dat se snadný způsob toocreate mezipaměti s předem vyplněná daty.</span><span class="sxs-lookup"><span data-stu-id="0714b-112">Importing data is an easy way toocreate a cache with pre-populated data.</span></span> <span data-ttu-id="0714b-113">Během procesu importu hello Azure Redis Cache načte hello RDB soubory z úložiště Azure do paměti a poté vloží hello klíčů do mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="0714b-113">During hello import process, Azure Redis Cache loads hello RDB files from Azure storage into memory and then inserts hello keys into hello cache.</span></span>

> [!NOTE]
> <span data-ttu-id="0714b-114">Před zahájením operace importu hello, zkontrolujte, že Redis databáze (RDB) soubor nebo soubory jsou odeslány do stránky, nebo blok objektů BLOB v úložišti Azure, v hello stejné oblasti a předplatné jako instance služby Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="0714b-114">Before beginning hello import operation, ensure that your Redis Database (RDB) file or files are uploaded into page or block blobs in Azure storage, in hello same region and subscription as your Azure Redis Cache instance.</span></span> <span data-ttu-id="0714b-115">Další informace najdete v tématu [Začínáme s Azure Blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="0714b-115">For more information, see [Get started with Azure Blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="0714b-116">Pokud jste exportovali souboru RDB pomocí hello [Azure Redis Cache exportovat](#export) funkce RDB soubor je již uložen do objektu BLOB stránky a je připravený pro import.</span><span class="sxs-lookup"><span data-stu-id="0714b-116">If you exported your RDB file using hello [Azure Redis Cache Export](#export) feature, your RDB file is already stored in a page blob and is ready for importing.</span></span>
>
>

1. <span data-ttu-id="0714b-117">tooimport jeden nebo více exportovat mezipaměti objektů BLOB, [procházet mezipaměti tooyour](cache-configure.md#configure-redis-cache-settings) v hello portál Azure a klikněte na tlačítko **importovat data** z hello **prostředků nabídky**.</span><span class="sxs-lookup"><span data-stu-id="0714b-117">tooimport one or more exported cache blobs, [browse tooyour cache](cache-configure.md#configure-redis-cache-settings) in hello Azure portal and click **Import data** from hello **Resource menu**.</span></span>

    ![Import dat][cache-import-data]
2. <span data-ttu-id="0714b-119">Klikněte na tlačítko **zvolte Blob(s)** a vyberte účet úložiště hello, který obsahuje hello data tooimport.</span><span class="sxs-lookup"><span data-stu-id="0714b-119">Click **Choose Blob(s)** and select hello storage account that contains hello data tooimport.</span></span>

    ![Zvolte účet úložiště][cache-import-choose-storage-account]
3. <span data-ttu-id="0714b-121">Klikněte na tlačítko hello kontejner, který obsahuje hello data tooimport.</span><span class="sxs-lookup"><span data-stu-id="0714b-121">Click hello container that contains hello data tooimport.</span></span>

    ![Vyberte kontejner][cache-import-choose-container]
4. <span data-ttu-id="0714b-123">Vyberte jeden nebo více objektů BLOB tooimport kliknutím hello oblasti toohello nalevo od hello název objektu blob a potom klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="0714b-123">Select one or more blobs tooimport by clicking hello area toohello left of hello blob name, and then click **Select**.</span></span>

    ![Vyberte objekty BLOB][cache-import-choose-blobs]
5. <span data-ttu-id="0714b-125">Klikněte na tlačítko **importovat** toobegin hello importu.</span><span class="sxs-lookup"><span data-stu-id="0714b-125">Click **Import** toobegin hello import process.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="0714b-126">mezipaměť Hello není během procesu importu hello přístup klienti mezipaměti, a všechna existující data v mezipaměti hello se odstraní.</span><span class="sxs-lookup"><span data-stu-id="0714b-126">hello cache is not accessible by cache clients during hello import process, and any existing data in hello cache is deleted.</span></span>
   >
   >

    ![Import][cache-import-blobs]

    <span data-ttu-id="0714b-128">Můžete sledovat průběh operace importu hello hello následující hello oznámení z hello portál Azure nebo zobrazením hello události v hello [protokol auditování](../azure-resource-manager/resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="0714b-128">You can monitor hello progress of hello import operation by following hello notifications from hello Azure portal, or by viewing hello events in hello [audit log](../azure-resource-manager/resource-group-audit.md).</span></span>

    ![Probíhá import][cache-import-data-import-complete]

## <a name="export"></a><span data-ttu-id="0714b-130">Export</span><span class="sxs-lookup"><span data-stu-id="0714b-130">Export</span></span>
<span data-ttu-id="0714b-131">Export umožňuje tooexport hello data uložená v Azure Redis Cache tooRedis kompatibilní RDB soubory.</span><span class="sxs-lookup"><span data-stu-id="0714b-131">Export allows you tooexport hello data stored in Azure Redis Cache tooRedis compatible RDB file(s).</span></span> <span data-ttu-id="0714b-132">Můžete použít tuto funkci toomove data tooanother instanci Azure Redis Cache či tooanother serveru Redis.</span><span class="sxs-lookup"><span data-stu-id="0714b-132">You can use this feature toomove data from one Azure Redis Cache instance tooanother or tooanother Redis server.</span></span> <span data-ttu-id="0714b-133">Během procesu exportu hello dočasný soubor vytvořen na hello virtuálních počítačů, hostitelů hello instanci serveru Azure Redis Cache a soubor hello je nahrané toohello určený účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="0714b-133">During hello export process, a temporary file is created on hello VM that hosts hello Azure Redis Cache server instance, and hello file is uploaded toohello designated storage account.</span></span> <span data-ttu-id="0714b-134">Po dokončení operace exportu hello se stavem úspěch nebo selhání hello dočasný soubor bude odstraněn.</span><span class="sxs-lookup"><span data-stu-id="0714b-134">When hello export operation completes with either a status of success or failure, hello temporary file is deleted.</span></span>

1. <span data-ttu-id="0714b-135">tooexport hello aktuální obsah hello mezipaměti toostorage, [procházet mezipaměti tooyour](cache-configure.md#configure-redis-cache-settings) v hello portál Azure a klikněte na tlačítko **Export dat** z hello **prostředků nabídky**.</span><span class="sxs-lookup"><span data-stu-id="0714b-135">tooexport hello current contents of hello cache toostorage, [browse tooyour cache](cache-configure.md#configure-redis-cache-settings) in hello Azure portal and click **Export data** from hello **Resource menu**.</span></span>

    ![Vyberte kontejner úložiště][cache-export-data-choose-storage-container]
2. <span data-ttu-id="0714b-137">Klikněte na tlačítko **vyberte kontejner úložiště** a vyberte účet úložiště hello potřeby.</span><span class="sxs-lookup"><span data-stu-id="0714b-137">Click **Choose Storage Container** and select hello desired storage account.</span></span> <span data-ttu-id="0714b-138">účet úložiště Hello musí být v hello stejném předplatném, oblasti jako vaše mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="0714b-138">hello storage account must be in hello same subscription and region as your cache.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="0714b-139">Export funguje s objekty BLOB stránky, které podporují classic i Resource Manager účty úložiště, ale nepodporuje [účty úložiště Blob](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts) v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="0714b-139">Export works with page blobs, which are supported by both classic and Resource Manager storage accounts, but are not supported by [Blob storage accounts](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts) at this time.</span></span>
   >
   >

    ![Účet úložiště][cache-export-data-choose-account]
3. <span data-ttu-id="0714b-141">Zvolte hello potřeby kontejner objektů blob a klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="0714b-141">Choose hello desired blob container and click **Select**.</span></span> <span data-ttu-id="0714b-142">toouse nový kontejner, klikněte na tlačítko **přidat kontejner** tooadd první a pak vyberte ze seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="0714b-142">toouse new a container, click **Add Container** tooadd it first and then select it from hello list.</span></span>

    ![Vyberte kontejner úložiště][cache-export-data-container]
4. <span data-ttu-id="0714b-144">Zadejte **předponu názvu objektu Blob** a klikněte na tlačítko **exportovat** procesu exportu toostart hello.</span><span class="sxs-lookup"><span data-stu-id="0714b-144">Type a **Blob name prefix** and click **Export** toostart hello export process.</span></span> <span data-ttu-id="0714b-145">Předpona názvu objektu blob Hello je použité tooprefix hello názvy soubory generované této operace exportu.</span><span class="sxs-lookup"><span data-stu-id="0714b-145">hello blob name prefix is used tooprefix hello names of files generated by this export operation.</span></span>

    ![Export][cache-export-data]

    <span data-ttu-id="0714b-147">Můžete sledovat průběh operace exportu hello hello následující hello oznámení z hello portál Azure nebo zobrazením hello události v hello [protokol auditování](../azure-resource-manager/resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="0714b-147">You can monitor hello progress of hello export operation by following hello notifications from hello Azure portal, or by viewing hello events in hello [audit log](../azure-resource-manager/resource-group-audit.md).</span></span>

    ![Exportovat data dokončení][cache-export-data-export-complete]

    <span data-ttu-id="0714b-149">Během procesu exportu hello zůstávají dostupné k použití mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="0714b-149">Caches remain available for use during hello export process.</span></span>

## <a name="importexport-faq"></a><span data-ttu-id="0714b-150">Nejčastější dotazy k importu a exportu</span><span class="sxs-lookup"><span data-stu-id="0714b-150">Import/Export FAQ</span></span>
<span data-ttu-id="0714b-151">Tento oddíl obsahuje nejčastější dotazy o funkci hello importu a exportu.</span><span class="sxs-lookup"><span data-stu-id="0714b-151">This section contains frequently asked questions about hello Import/Export feature.</span></span>

* [<span data-ttu-id="0714b-152">Jaké cenové úrovně můžete použít k importu a exportu?</span><span class="sxs-lookup"><span data-stu-id="0714b-152">What pricing tiers can use Import/Export?</span></span>](#what-pricing-tiers-can-use-importexport)
* [<span data-ttu-id="0714b-153">Můžete importovat data z jakéhokoli serveru Redis?</span><span class="sxs-lookup"><span data-stu-id="0714b-153">Can I import data from any Redis server?</span></span>](#can-i-import-data-from-any-redis-server)
* [<span data-ttu-id="0714b-154">Jaké verze RDB můžete importovat?</span><span class="sxs-lookup"><span data-stu-id="0714b-154">What RDB versions can I import?</span></span>](#what-rdb-versions-can-i-import)
* [<span data-ttu-id="0714b-155">Je k dispozici Moje mezipaměti během operace importu a exportu?</span><span class="sxs-lookup"><span data-stu-id="0714b-155">Is my cache available during an Import/Export operation?</span></span>](#is-my-cache-available-during-an-importexport-operation)
* [<span data-ttu-id="0714b-156">Můžete použít s clusteru Redis importu a exportu?</span><span class="sxs-lookup"><span data-stu-id="0714b-156">Can I use Import/Export with Redis cluster?</span></span>](#can-i-use-importexport-with-redis-cluster)
* [<span data-ttu-id="0714b-157">Jak funguje importu a exportu s vlastní databáze nastavení?</span><span class="sxs-lookup"><span data-stu-id="0714b-157">How does Import/Export work with a custom databases setting?</span></span>](#how-does-importexport-work-with-a-custom-databases-setting)
* [<span data-ttu-id="0714b-158">Jak se liší od trvalosti Redis importu a exportu?</span><span class="sxs-lookup"><span data-stu-id="0714b-158">How is Import/Export different from Redis persistence?</span></span>](#how-is-importexport-different-from-redis-persistence)
* [<span data-ttu-id="0714b-159">Můžete automatizovat Import a Export pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiné správy klientů?</span><span class="sxs-lookup"><span data-stu-id="0714b-159">Can I automate Import/Export using PowerShell, CLI, or other management clients?</span></span>](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
* [<span data-ttu-id="0714b-160">Zobrazila vypršení časového limitu během Moje operace importu a exportu. Co to znamená?</span><span class="sxs-lookup"><span data-stu-id="0714b-160">I received a timeout error during my Import/Export operation. What does it mean?</span></span>](#i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean)
* [<span data-ttu-id="0714b-161">Chyba se zobrazí chybové při exportu Moje data tooAzure úložiště objektů Blob. Co se přihodilo?</span><span class="sxs-lookup"><span data-stu-id="0714b-161">I got an error when exporting my data tooAzure Blob Storage. What happened?</span></span>](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened)

### <a name="what-pricing-tiers-can-use-importexport"></a><span data-ttu-id="0714b-162">Jaké cenové úrovně můžete použít k importu a exportu?</span><span class="sxs-lookup"><span data-stu-id="0714b-162">What pricing tiers can use Import/Export?</span></span>
<span data-ttu-id="0714b-163">Import a Export je k dispozici pouze v cenová úroveň premium hello.</span><span class="sxs-lookup"><span data-stu-id="0714b-163">Import/Export is available only in hello premium pricing tier.</span></span>

### <a name="can-i-import-data-from-any-redis-server"></a><span data-ttu-id="0714b-164">Můžete importovat data z jakéhokoli serveru Redis?</span><span class="sxs-lookup"><span data-stu-id="0714b-164">Can I import data from any Redis server?</span></span>
<span data-ttu-id="0714b-165">Ano, kromě tooimporting data exportovaná z instance služby Azure Redis Cache, můžete importovat soubory RDB z jakéhokoli Redis serveru se systémem v libovolném cloudu nebo prostředí, například Linux, Windows nebo poskytovatelů cloudových například Amazon Web Services.</span><span class="sxs-lookup"><span data-stu-id="0714b-165">Yes, in addition tooimporting data exported from Azure Redis Cache instances, you can import RDB files from any Redis server running in any cloud or environment, such as Linux, Windows, or cloud providers such as Amazon Web Services.</span></span> <span data-ttu-id="0714b-166">toodo to nahrávání hello RDB soubor ze serveru Redis hello potřeby do stránky nebo blok objektů blob do účtu úložiště Azure a pak importovat ho do instance služby Azure Redis Cache premium.</span><span class="sxs-lookup"><span data-stu-id="0714b-166">toodo this, upload hello RDB file from hello desired Redis server into a page or block blob in an Azure Storage Account, and then import it into your premium Azure Redis Cache instance.</span></span> <span data-ttu-id="0714b-167">Můžete například chcete tooexport hello data z mezipaměti produkční a importujte ho do mezipaměti, používá jako součást pracovní prostředí pro testování nebo pro migraci.</span><span class="sxs-lookup"><span data-stu-id="0714b-167">For example, you may want tooexport hello data from your production cache and import it into a cache used as part of a staging environment for testing or migration.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0714b-168">toosuccessfully importovat data exportovaná z Redis servery než Azure Redis Cache, při použití objektů blob stránky, velikost objektu blob stránky hello musí být zarovnána na hranici 512 bajtů.</span><span class="sxs-lookup"><span data-stu-id="0714b-168">toosuccessfully import data exported from Redis servers other than Azure Redis Cache when using a page blob, hello page blob size must be aligned on a 512 byte boundary.</span></span> <span data-ttu-id="0714b-169">Pro ukázkový kód tooperform potřebné odsazení bajtu najdete v tématu [ukázkové stránky blogu nahrávání](https://github.com/JimRoberts-MS/SamplePageBlobUpload).</span><span class="sxs-lookup"><span data-stu-id="0714b-169">For sample code tooperform any required byte padding, see [Sample page blog upload](https://github.com/JimRoberts-MS/SamplePageBlobUpload).</span></span>
> 
> 

### <a name="what-rdb-versions-can-i-import"></a><span data-ttu-id="0714b-170">Jaké verze RDB můžete importovat?</span><span class="sxs-lookup"><span data-stu-id="0714b-170">What RDB versions can I import?</span></span>

<span data-ttu-id="0714b-171">Azure Redis Cache podporuje RDB import až prostřednictvím RDB verze 7.</span><span class="sxs-lookup"><span data-stu-id="0714b-171">Azure Redis Cache supports RDB import up through RDB version 7.</span></span>

### <a name="is-my-cache-available-during-an-importexport-operation"></a><span data-ttu-id="0714b-172">Je k dispozici Moje mezipaměti během operace importu a exportu?</span><span class="sxs-lookup"><span data-stu-id="0714b-172">Is my cache available during an Import/Export operation?</span></span>
* <span data-ttu-id="0714b-173">**Export** – mezipamětí zůstanou k dispozici a můžete pokračovat toouse mezipaměti během operace exportu.</span><span class="sxs-lookup"><span data-stu-id="0714b-173">**Export** - Caches remain available and you can continue toouse your cache during an export operation.</span></span>
* <span data-ttu-id="0714b-174">**Import** – mezipaměti k dispozici, když se spustí operace importu a bude k dispozici pro použití při dokončení operace importu hello.</span><span class="sxs-lookup"><span data-stu-id="0714b-174">**Import** - Caches become unavailable when an import operation starts, and become available for use when hello import operation completes.</span></span>

### <a name="can-i-use-importexport-with-redis-cluster"></a><span data-ttu-id="0714b-175">Můžete použít s clusteru Redis importu a exportu?</span><span class="sxs-lookup"><span data-stu-id="0714b-175">Can I use Import/Export with Redis cluster?</span></span>
<span data-ttu-id="0714b-176">Ano, a můžete můžete importu a exportu mezi Clusterové mezipaměti a mezipaměti vypojené z clusteru.</span><span class="sxs-lookup"><span data-stu-id="0714b-176">Yes, and you can import/export between a clustered cache and a non-clustered cache.</span></span> <span data-ttu-id="0714b-177">Od clusteru Redis [jen podporuje databáze 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), není-li importovat všechna data v databázích než 0.</span><span class="sxs-lookup"><span data-stu-id="0714b-177">Since Redis cluster [only supports database 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), any data in databases other than 0 isn't imported.</span></span> <span data-ttu-id="0714b-178">Při importu dat Clusterové mezipaměti, hello klíče se předistribuují mezi horizontálních oddílů hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="0714b-178">When clustered cache data is imported, hello keys are redistributed among hello shards of hello cluster.</span></span>

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a><span data-ttu-id="0714b-179">Jak funguje importu a exportu s vlastní databáze nastavení?</span><span class="sxs-lookup"><span data-stu-id="0714b-179">How does Import/Export work with a custom databases setting?</span></span>
<span data-ttu-id="0714b-180">Některé cenové úrovně mají různé [databáze omezení](cache-configure.md#databases), takže existují některé aspekty při importu, pokud jste nakonfigurovali vlastní hodnotu hello `databases` nastavení během vytváření mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="0714b-180">Some pricing tiers have different [databases limits](cache-configure.md#databases), so there are some considerations when importing if you configured a custom value for hello `databases` setting during cache creation.</span></span>

* <span data-ttu-id="0714b-181">Při importu tooa cenová úroveň s nižší `databases` limit než hello vrstvy, ze kterého jste exportovali:</span><span class="sxs-lookup"><span data-stu-id="0714b-181">When importing tooa pricing tier with a lower `databases` limit than hello tier from which you exported:</span></span>
  * <span data-ttu-id="0714b-182">Pokud používáte výchozí číslo hello `databases`, což je 16 pro všechny cenové úrovně, dojde ke ztrátě žádná data.</span><span class="sxs-lookup"><span data-stu-id="0714b-182">If you are using hello default number of `databases`, which is 16 for all pricing tiers, no data is lost.</span></span>
  * <span data-ttu-id="0714b-183">Pokud používáte vlastní počet `databases` že patří do hello limity pro hello vrstvy toowhich importujete, všechna data budou zachována.</span><span class="sxs-lookup"><span data-stu-id="0714b-183">If you are using a custom number of `databases` that falls within hello limits for hello tier toowhich you are importing, no data is lost.</span></span>
  * <span data-ttu-id="0714b-184">Pokud exportovaná data obsažená data v databázi, která překračuje omezení hello hello nové vrstvy, není importován hello data z těchto vyšší databází.</span><span class="sxs-lookup"><span data-stu-id="0714b-184">If your exported data contained data in a database that exceeds hello limits of hello new tier, hello data from those higher databases is not imported.</span></span>

### <a name="how-is-importexport-different-from-redis-persistence"></a><span data-ttu-id="0714b-185">Jak se liší od trvalosti Redis importu a exportu?</span><span class="sxs-lookup"><span data-stu-id="0714b-185">How is Import/Export different from Redis persistence?</span></span>
<span data-ttu-id="0714b-186">Trvalost Azure Redis Cache vám umožní toopersist data uložená v Redis tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="0714b-186">Azure Redis Cache persistence allows you toopersist data stored in Redis tooAzure Storage.</span></span> <span data-ttu-id="0714b-187">Pokud je nakonfigurovaná trvalost, trvá Azure Redis Cache snímek hello mezipaměti Redis v toodisk Redis binární formát na základě konfigurovat četnosti zálohování.</span><span class="sxs-lookup"><span data-stu-id="0714b-187">When persistence is configured, Azure Redis Cache persists a snapshot of hello Redis cache in a Redis binary format toodisk based on a configurable backup frequency.</span></span> <span data-ttu-id="0714b-188">Pokud dojde k závažné události, zakazující hello primární a mezipaměti repliky, je obnoven data do mezipaměti hello automaticky pomocí hello poslední snímek.</span><span class="sxs-lookup"><span data-stu-id="0714b-188">If a catastrophic event occurs that disables both hello primary and replica cache, hello cache data is restored automatically using hello most recent snapshot.</span></span> <span data-ttu-id="0714b-189">Další informace najdete v tématu [jak tooconfigure trvalosti dat pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-persistence.md).</span><span class="sxs-lookup"><span data-stu-id="0714b-189">For more information, see [How tooconfigure data persistence for a Premium Azure Redis Cache](cache-how-to-premium-persistence.md).</span></span>

<span data-ttu-id="0714b-190">Import / Export můžete toobring data do nebo export z Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="0714b-190">Import/ Export allows you toobring data into or export from Azure Redis Cache.</span></span> <span data-ttu-id="0714b-191">Není konfigurace zálohování a obnovení pomocí trvalosti Redis.</span><span class="sxs-lookup"><span data-stu-id="0714b-191">It does not configure backup and restore using Redis persistence.</span></span>

### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a><span data-ttu-id="0714b-192">Můžete automatizovat Import a Export pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiné správy klientů?</span><span class="sxs-lookup"><span data-stu-id="0714b-192">Can I automate Import/Export using PowerShell, CLI, or other management clients?</span></span>
<span data-ttu-id="0714b-193">Ano, pro prostředí PowerShell pokyny naleznete v části [tooimport mezipaměti Redis](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) a [tooexport mezipaměti Redis](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).</span><span class="sxs-lookup"><span data-stu-id="0714b-193">Yes, for PowerShell instructions see [tooimport a Redis cache](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) and [tooexport a Redis cache](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).</span></span>

### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a><span data-ttu-id="0714b-194">Zobrazila vypršení časového limitu během Moje operace importu a exportu.</span><span class="sxs-lookup"><span data-stu-id="0714b-194">I received a timeout error during my Import/Export operation.</span></span> <span data-ttu-id="0714b-195">Co to znamená?</span><span class="sxs-lookup"><span data-stu-id="0714b-195">What does it mean?</span></span>
<span data-ttu-id="0714b-196">Pokud zůstanou na hello **importovat data** nebo **exportovat data** okno delší než 15 minut před zahájením operace hello, obdržíte chybu s chybové zprávy podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="0714b-196">If you remain on hello **Import data** or **Export data** blade for longer than 15 minutes before initiating hello operation, you receive an error with an error message similar toohello following example:</span></span>

    hello request tooimport data into cache 'contoso55' failed with status 'error' and error 'One of hello SAS URIs provided could not be used for hello following reason: hello SAS token end time (se) must be at least 1 hour from now and hello start time (st), if given, must be at least 15 minutes in hello past.

<span data-ttu-id="0714b-197">tooresolve, zahájení importu hello nebo operace exportu předtím, než dojde k uplynutí 15 minut.</span><span class="sxs-lookup"><span data-stu-id="0714b-197">tooresolve this, initiate hello import or export operation before 15 minutes has elapsed.</span></span>

### <a name="i-got-an-error-when-exporting-my-data-tooazure-blob-storage-what-happened"></a><span data-ttu-id="0714b-198">Chyba se zobrazí chybové při exportu Moje data tooAzure úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="0714b-198">I got an error when exporting my data tooAzure Blob Storage.</span></span> <span data-ttu-id="0714b-199">Co se přihodilo?</span><span class="sxs-lookup"><span data-stu-id="0714b-199">What happened?</span></span>
<span data-ttu-id="0714b-200">Export pracuje pouze s RDB soubory uložené jako objekty BLOB stránky.</span><span class="sxs-lookup"><span data-stu-id="0714b-200">Export works only with RDB files stored as page blobs.</span></span> <span data-ttu-id="0714b-201">Jiné typy objektů blob nejsou aktuálně podporovány, včetně účty úložiště blob s horká a studená vrstvami.</span><span class="sxs-lookup"><span data-stu-id="0714b-201">Other blob types are not currently supported, including blob storage accounts with hot and cool tiers.</span></span> <span data-ttu-id="0714b-202">Další informace najdete v tématu [účty úložiště Blob](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts).</span><span class="sxs-lookup"><span data-stu-id="0714b-202">For more information, see [Blob storage accounts](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0714b-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0714b-203">Next steps</span></span>
<span data-ttu-id="0714b-204">Zjistěte, jak toouse více premium mezipaměti funkce.</span><span class="sxs-lookup"><span data-stu-id="0714b-204">Learn how toouse more premium cache features.</span></span>

* [<span data-ttu-id="0714b-205">Úvod toohello Azure Redis Cache na úrovni Premium</span><span class="sxs-lookup"><span data-stu-id="0714b-205">Introduction toohello Azure Redis Cache Premium tier</span></span>](cache-premium-tier-intro.md)    

<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png
