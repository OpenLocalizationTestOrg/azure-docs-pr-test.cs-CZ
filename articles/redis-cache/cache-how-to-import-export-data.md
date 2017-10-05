---
title: "Import a Export dat ve službě Azure Redis Cache | Microsoft Docs"
description: "Postup importu a exportu dat do a z úložiště objektů blob s vaší instancí Azure Redis Cache premium"
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
ms.openlocfilehash: 5e6d731f0a1cecc1a191c74a45e37a9b94fd98ee
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="import-and-export-data-in-azure-redis-cache"></a><span data-ttu-id="4a365-103">Import a Export dat ve službě Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="4a365-103">Import and Export data in Azure Redis Cache</span></span>
<span data-ttu-id="4a365-104">Import a Export je operace Azure Redis Cache dat správy, který umožňuje importovat data do Azure Redis Cache nebo exportovat data z Azure Redis Cache pomocí import a Export snímku databáze Redis Cache (RDB) z mezipaměti premium do objektu blob v Azure Účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="4a365-104">Import/Export is an Azure Redis Cache data management operation, which allows you to import data into Azure Redis Cache or export data from Azure Redis Cache by importing and exporting a Redis Cache Database (RDB) snapshot from a premium cache to a blob in an Azure Storage Account.</span></span> 

- <span data-ttu-id="4a365-105">**Export** -vaše Azure Redis Cache RDB snímky můžete exportovat do objektů Blob stránky.</span><span class="sxs-lookup"><span data-stu-id="4a365-105">**Export** - you can export your Azure Redis Cache RDB snapshots to a Page Blob.</span></span>
- <span data-ttu-id="4a365-106">**Import** -vaší RDB mezipaměti Redis snímky můžete importovat z objekt Blob stránky nebo objekt Blob bloku.</span><span class="sxs-lookup"><span data-stu-id="4a365-106">**Import** - you can import your Redis Cache RDB snapshots from either a Page Blob or a Block Blob.</span></span>

<span data-ttu-id="4a365-107">Import a Export umožňuje migraci mezi různými instancemi Azure Redis Cache nebo naplnění mezipaměti s daty před použitím.</span><span class="sxs-lookup"><span data-stu-id="4a365-107">Import/Export enables you to migrate between different Azure Redis Cache instances or populate the cache with data before use.</span></span>

<span data-ttu-id="4a365-108">Tento článek obsahuje Průvodce pro import a export dat pomocí Azure Redis Cache a odpovědi na nejčastější dotazy.</span><span class="sxs-lookup"><span data-stu-id="4a365-108">This article provides a guide for importing and exporting data with Azure Redis Cache and provides the answers to commonly asked questions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4a365-109">Import a Export je ve verzi preview a je dostupná jenom pro [úroveň premium](cache-premium-tier-intro.md) ukládá do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="4a365-109">Import/Export is in preview and is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span>
>
>

## <a name="import"></a><span data-ttu-id="4a365-110">Import</span><span class="sxs-lookup"><span data-stu-id="4a365-110">Import</span></span>
<span data-ttu-id="4a365-111">Import umožňuje přinést si Redis kompatibilní RDB soubory z jakéhokoli Redis serveru se systémem v libovolném cloudu nebo prostředí, včetně Redis systémem Linux, Windows nebo kteréhokoli poskytovatele cloudových služeb jako Amazon Web Services a dalších.</span><span class="sxs-lookup"><span data-stu-id="4a365-111">Import can be used to bring Redis compatible RDB files from any Redis server running in any cloud or environment, including Redis running on Linux, Windows, or any cloud provider such as Amazon Web Services and others.</span></span> <span data-ttu-id="4a365-112">Import dat je snadný způsob, jak vytvořit mezipaměti s předem vyplněná data.</span><span class="sxs-lookup"><span data-stu-id="4a365-112">Importing data is an easy way to create a cache with pre-populated data.</span></span> <span data-ttu-id="4a365-113">Během procesu importu Azure Redis Cache načte RDB soubory z úložiště Azure do paměti a vloží klíčů do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="4a365-113">During the import process, Azure Redis Cache loads the RDB files from Azure storage into memory and then inserts the keys into the cache.</span></span>

> [!NOTE]
> <span data-ttu-id="4a365-114">Před zahájením operace importu se ujistěte, že Redis databáze (RDB) soubor nebo soubory jsou odeslány do stránky nebo blok objektů BLOB v úložišti Azure, ve stejné oblasti a předplatné jako instance služby Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="4a365-114">Before beginning the import operation, ensure that your Redis Database (RDB) file or files are uploaded into page or block blobs in Azure storage, in the same region and subscription as your Azure Redis Cache instance.</span></span> <span data-ttu-id="4a365-115">Další informace najdete v tématu [Začínáme s Azure Blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="4a365-115">For more information, see [Get started with Azure Blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="4a365-116">Pokud jste exportovali pomocí souboru RDB [Azure Redis Cache exportovat](#export) funkce RDB soubor je již uložen do objektu BLOB stránky a je připravený pro import.</span><span class="sxs-lookup"><span data-stu-id="4a365-116">If you exported your RDB file using the [Azure Redis Cache Export](#export) feature, your RDB file is already stored in a page blob and is ready for importing.</span></span>
>
>

1. <span data-ttu-id="4a365-117">Chcete-li importovat jeden nebo více objektů BLOB exportovaný mezipaměti, [přejděte do mezipaměti](cache-configure.md#configure-redis-cache-settings) na portálu Azure a klikněte na **importovat data** z **prostředků nabídky**.</span><span class="sxs-lookup"><span data-stu-id="4a365-117">To import one or more exported cache blobs, [browse to your cache](cache-configure.md#configure-redis-cache-settings) in the Azure portal and click **Import data** from the **Resource menu**.</span></span>

    ![Import dat][cache-import-data]
2. <span data-ttu-id="4a365-119">Klikněte na tlačítko **zvolte Blob(s)** a vyberte účet úložiště, který obsahuje data určená k importu.</span><span class="sxs-lookup"><span data-stu-id="4a365-119">Click **Choose Blob(s)** and select the storage account that contains the data to import.</span></span>

    ![Zvolte účet úložiště][cache-import-choose-storage-account]
3. <span data-ttu-id="4a365-121">Klikněte na kontejner, který obsahuje data určená k importu.</span><span class="sxs-lookup"><span data-stu-id="4a365-121">Click the container that contains the data to import.</span></span>

    ![Vyberte kontejner][cache-import-choose-container]
4. <span data-ttu-id="4a365-123">Vyberte jeden nebo více objektů BLOB k importu klepnutím na plochu nalevo od názvu objektu blob a potom klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="4a365-123">Select one or more blobs to import by clicking the area to the left of the blob name, and then click **Select**.</span></span>

    ![Vyberte objekty BLOB][cache-import-choose-blobs]
5. <span data-ttu-id="4a365-125">Klikněte na tlačítko **importovat** zahájíte proces importu.</span><span class="sxs-lookup"><span data-stu-id="4a365-125">Click **Import** to begin the import process.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="4a365-126">Mezipaměť je přístupný pro klienty mezipaměti není během procesu importu a všechna existující data v mezipaměti se odstraní.</span><span class="sxs-lookup"><span data-stu-id="4a365-126">The cache is not accessible by cache clients during the import process, and any existing data in the cache is deleted.</span></span>
   >
   >

    ![Import][cache-import-blobs]

    <span data-ttu-id="4a365-128">Pomocí následujících oznámení z portálu Azure nebo zobrazením událostí v můžete sledovat průběh operace importu [protokol auditování](../azure-resource-manager/resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="4a365-128">You can monitor the progress of the import operation by following the notifications from the Azure portal, or by viewing the events in the [audit log](../azure-resource-manager/resource-group-audit.md).</span></span>

    ![Probíhá import][cache-import-data-import-complete]

## <a name="export"></a><span data-ttu-id="4a365-130">Export</span><span class="sxs-lookup"><span data-stu-id="4a365-130">Export</span></span>
<span data-ttu-id="4a365-131">Export umožňuje exportovat data uložená ve službě Azure Redis Cache k Redis kompatibilní soubory RDB.</span><span class="sxs-lookup"><span data-stu-id="4a365-131">Export allows you to export the data stored in Azure Redis Cache to Redis compatible RDB file(s).</span></span> <span data-ttu-id="4a365-132">Tato funkce slouží pro přesun dat z jedné instance Azure Redis Cache do jiné nebo jinému serveru Redis.</span><span class="sxs-lookup"><span data-stu-id="4a365-132">You can use this feature to move data from one Azure Redis Cache instance to another or to another Redis server.</span></span> <span data-ttu-id="4a365-133">Během procesu exportu ve virtuálním počítači, který je hostitelem instance serveru Azure Redis Cache je vytvořit dočasný soubor a soubor je odeslán k účtu úložiště určený.</span><span class="sxs-lookup"><span data-stu-id="4a365-133">During the export process, a temporary file is created on the VM that hosts the Azure Redis Cache server instance, and the file is uploaded to the designated storage account.</span></span> <span data-ttu-id="4a365-134">Po dokončení operace exportu se stavem úspěch nebo selhání dočasný soubor bude odstraněn.</span><span class="sxs-lookup"><span data-stu-id="4a365-134">When the export operation completes with either a status of success or failure, the temporary file is deleted.</span></span>

1. <span data-ttu-id="4a365-135">Exportovat aktuální obsah mezipaměti do úložiště, [přejděte do mezipaměti](cache-configure.md#configure-redis-cache-settings) na portálu Azure a klikněte na **Export dat** z **prostředků nabídky**.</span><span class="sxs-lookup"><span data-stu-id="4a365-135">To export the current contents of the cache to storage, [browse to your cache](cache-configure.md#configure-redis-cache-settings) in the Azure portal and click **Export data** from the **Resource menu**.</span></span>

    ![Vyberte kontejner úložiště][cache-export-data-choose-storage-container]
2. <span data-ttu-id="4a365-137">Klikněte na tlačítko **vyberte kontejner úložiště** a vyberte požadovaný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="4a365-137">Click **Choose Storage Container** and select the desired storage account.</span></span> <span data-ttu-id="4a365-138">Účet úložiště musí být ve stejném předplatném, oblasti jako vaše mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="4a365-138">The storage account must be in the same subscription and region as your cache.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="4a365-139">Export funguje s objekty BLOB stránky, které podporují classic i Resource Manager účty úložiště, ale nepodporuje [účty úložiště Blob](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts) v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="4a365-139">Export works with page blobs, which are supported by both classic and Resource Manager storage accounts, but are not supported by [Blob storage accounts](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts) at this time.</span></span>
   >
   >

    ![Účet úložiště][cache-export-data-choose-account]
3. <span data-ttu-id="4a365-141">Vyberte kontejner objektů blob požadované a klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="4a365-141">Choose the desired blob container and click **Select**.</span></span> <span data-ttu-id="4a365-142">Chcete-li použít nový kontejner, klikněte na tlačítko **přidat kontejner** nejprve přidat a vyberte ho ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="4a365-142">To use new a container, click **Add Container** to add it first and then select it from the list.</span></span>

    ![Vyberte kontejner úložiště][cache-export-data-container]
4. <span data-ttu-id="4a365-144">Zadejte **předponu názvu objektu Blob** a klikněte na tlačítko **exportovat** ke spuštění procesu exportu.</span><span class="sxs-lookup"><span data-stu-id="4a365-144">Type a **Blob name prefix** and click **Export** to start the export process.</span></span> <span data-ttu-id="4a365-145">Předpona názvu objektu blob se používá jako předpona názvy soubory generované této operace exportu.</span><span class="sxs-lookup"><span data-stu-id="4a365-145">The blob name prefix is used to prefix the names of files generated by this export operation.</span></span>

    ![Export][cache-export-data]

    <span data-ttu-id="4a365-147">Pomocí následujících oznámení z portálu Azure nebo zobrazením událostí v můžete sledovat průběh operace exportu [protokol auditování](../azure-resource-manager/resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="4a365-147">You can monitor the progress of the export operation by following the notifications from the Azure portal, or by viewing the events in the [audit log](../azure-resource-manager/resource-group-audit.md).</span></span>

    ![Exportovat data dokončení][cache-export-data-export-complete]

    <span data-ttu-id="4a365-149">Mezipamětí zůstávají dostupné k použití během procesu exportu.</span><span class="sxs-lookup"><span data-stu-id="4a365-149">Caches remain available for use during the export process.</span></span>

## <a name="importexport-faq"></a><span data-ttu-id="4a365-150">Nejčastější dotazy k importu a exportu</span><span class="sxs-lookup"><span data-stu-id="4a365-150">Import/Export FAQ</span></span>
<span data-ttu-id="4a365-151">Tento oddíl obsahuje nejčastější dotazy týkající se funkce importu a exportu.</span><span class="sxs-lookup"><span data-stu-id="4a365-151">This section contains frequently asked questions about the Import/Export feature.</span></span>

* [<span data-ttu-id="4a365-152">Jaké cenové úrovně můžete použít k importu a exportu?</span><span class="sxs-lookup"><span data-stu-id="4a365-152">What pricing tiers can use Import/Export?</span></span>](#what-pricing-tiers-can-use-importexport)
* [<span data-ttu-id="4a365-153">Můžete importovat data z jakéhokoli serveru Redis?</span><span class="sxs-lookup"><span data-stu-id="4a365-153">Can I import data from any Redis server?</span></span>](#can-i-import-data-from-any-redis-server)
* [<span data-ttu-id="4a365-154">Jaké verze RDB můžete importovat?</span><span class="sxs-lookup"><span data-stu-id="4a365-154">What RDB versions can I import?</span></span>](#what-rdb-versions-can-i-import)
* [<span data-ttu-id="4a365-155">Je k dispozici Moje mezipaměti během operace importu a exportu?</span><span class="sxs-lookup"><span data-stu-id="4a365-155">Is my cache available during an Import/Export operation?</span></span>](#is-my-cache-available-during-an-importexport-operation)
* [<span data-ttu-id="4a365-156">Můžete použít s clusteru Redis importu a exportu?</span><span class="sxs-lookup"><span data-stu-id="4a365-156">Can I use Import/Export with Redis cluster?</span></span>](#can-i-use-importexport-with-redis-cluster)
* [<span data-ttu-id="4a365-157">Jak funguje importu a exportu s vlastní databáze nastavení?</span><span class="sxs-lookup"><span data-stu-id="4a365-157">How does Import/Export work with a custom databases setting?</span></span>](#how-does-importexport-work-with-a-custom-databases-setting)
* [<span data-ttu-id="4a365-158">Jak se liší od trvalosti Redis importu a exportu?</span><span class="sxs-lookup"><span data-stu-id="4a365-158">How is Import/Export different from Redis persistence?</span></span>](#how-is-importexport-different-from-redis-persistence)
* [<span data-ttu-id="4a365-159">Můžete automatizovat Import a Export pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiné správy klientů?</span><span class="sxs-lookup"><span data-stu-id="4a365-159">Can I automate Import/Export using PowerShell, CLI, or other management clients?</span></span>](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
* [<span data-ttu-id="4a365-160">Zobrazila vypršení časového limitu během Moje operace importu a exportu. Co to znamená?</span><span class="sxs-lookup"><span data-stu-id="4a365-160">I received a timeout error during my Import/Export operation. What does it mean?</span></span>](#i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean)
* [<span data-ttu-id="4a365-161">Chyba se zobrazí chybové při exportu svá data do úložiště objektů Blob Azure. Co se přihodilo?</span><span class="sxs-lookup"><span data-stu-id="4a365-161">I got an error when exporting my data to Azure Blob Storage. What happened?</span></span>](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened)

### <a name="what-pricing-tiers-can-use-importexport"></a><span data-ttu-id="4a365-162">Jaké cenové úrovně můžete použít k importu a exportu?</span><span class="sxs-lookup"><span data-stu-id="4a365-162">What pricing tiers can use Import/Export?</span></span>
<span data-ttu-id="4a365-163">Import a Export je k dispozici pouze v cenová úroveň premium.</span><span class="sxs-lookup"><span data-stu-id="4a365-163">Import/Export is available only in the premium pricing tier.</span></span>

### <a name="can-i-import-data-from-any-redis-server"></a><span data-ttu-id="4a365-164">Můžete importovat data z jakéhokoli serveru Redis?</span><span class="sxs-lookup"><span data-stu-id="4a365-164">Can I import data from any Redis server?</span></span>
<span data-ttu-id="4a365-165">Ano, kromě import data exportovaná z instance služby Azure Redis Cache, můžete importovat soubory RDB z jakéhokoli serveru Redis v libovolném cloudu nebo prostředí, například Linux, systém Windows spuštěný, nebo cloudu poskytovatelé, například Amazon Web Services.</span><span class="sxs-lookup"><span data-stu-id="4a365-165">Yes, in addition to importing data exported from Azure Redis Cache instances, you can import RDB files from any Redis server running in any cloud or environment, such as Linux, Windows, or cloud providers such as Amazon Web Services.</span></span> <span data-ttu-id="4a365-166">K tomuto účelu nahrát soubor RDB z požadovaného serveru Redis do stránky nebo blok objektů blob v účtu úložiště Azure a následně ho naimportovat do instance služby Azure Redis Cache premium.</span><span class="sxs-lookup"><span data-stu-id="4a365-166">To do this, upload the RDB file from the desired Redis server into a page or block blob in an Azure Storage Account, and then import it into your premium Azure Redis Cache instance.</span></span> <span data-ttu-id="4a365-167">Například můžete chtít exportovat data z mezipaměti produkční a importujte ho do mezipaměti, používá jako součást pracovní prostředí pro testování nebo pro migraci.</span><span class="sxs-lookup"><span data-stu-id="4a365-167">For example, you may want to export the data from your production cache and import it into a cache used as part of a staging environment for testing or migration.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4a365-168">Chcete-li úspěšně importovat data exportovaná z Redis servery než Azure Redis Cache, při použití objektů blob stránky, musí být zarovnána velikost stránky objektu blob na hranici 512 bajtů.</span><span class="sxs-lookup"><span data-stu-id="4a365-168">To successfully import data exported from Redis servers other than Azure Redis Cache when using a page blob, the page blob size must be aligned on a 512 byte boundary.</span></span> <span data-ttu-id="4a365-169">Ukázkový kód k provedení všech požadovaných bajtů odsazení, najdete v části [ukázkové stránky blogu nahrávání](https://github.com/JimRoberts-MS/SamplePageBlobUpload).</span><span class="sxs-lookup"><span data-stu-id="4a365-169">For sample code to perform any required byte padding, see [Sample page blog upload](https://github.com/JimRoberts-MS/SamplePageBlobUpload).</span></span>
> 
> 

### <a name="what-rdb-versions-can-i-import"></a><span data-ttu-id="4a365-170">Jaké verze RDB můžete importovat?</span><span class="sxs-lookup"><span data-stu-id="4a365-170">What RDB versions can I import?</span></span>

<span data-ttu-id="4a365-171">Azure Redis Cache podporuje RDB import až prostřednictvím RDB verze 7.</span><span class="sxs-lookup"><span data-stu-id="4a365-171">Azure Redis Cache supports RDB import up through RDB version 7.</span></span>

### <a name="is-my-cache-available-during-an-importexport-operation"></a><span data-ttu-id="4a365-172">Je k dispozici Moje mezipaměti během operace importu a exportu?</span><span class="sxs-lookup"><span data-stu-id="4a365-172">Is my cache available during an Import/Export operation?</span></span>
* <span data-ttu-id="4a365-173">**Export** – mezipamětí zůstanou dostupné, a můžete nadále používat vaše mezipaměť během operace exportu.</span><span class="sxs-lookup"><span data-stu-id="4a365-173">**Export** - Caches remain available and you can continue to use your cache during an export operation.</span></span>
* <span data-ttu-id="4a365-174">**Import** – mezipaměti k dispozici, když se spustí operace importu a bude k dispozici pro použití při dokončení operace importu.</span><span class="sxs-lookup"><span data-stu-id="4a365-174">**Import** - Caches become unavailable when an import operation starts, and become available for use when the import operation completes.</span></span>

### <a name="can-i-use-importexport-with-redis-cluster"></a><span data-ttu-id="4a365-175">Můžete použít s clusteru Redis importu a exportu?</span><span class="sxs-lookup"><span data-stu-id="4a365-175">Can I use Import/Export with Redis cluster?</span></span>
<span data-ttu-id="4a365-176">Ano, a můžete můžete importu a exportu mezi Clusterové mezipaměti a mezipaměti vypojené z clusteru.</span><span class="sxs-lookup"><span data-stu-id="4a365-176">Yes, and you can import/export between a clustered cache and a non-clustered cache.</span></span> <span data-ttu-id="4a365-177">Od clusteru Redis [jen podporuje databáze 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), není-li importovat všechna data v databázích než 0.</span><span class="sxs-lookup"><span data-stu-id="4a365-177">Since Redis cluster [only supports database 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), any data in databases other than 0 isn't imported.</span></span> <span data-ttu-id="4a365-178">Při importu dat Clusterové mezipaměti, klíče se předistribuují mezi horizontálních oddílů clusteru.</span><span class="sxs-lookup"><span data-stu-id="4a365-178">When clustered cache data is imported, the keys are redistributed among the shards of the cluster.</span></span>

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a><span data-ttu-id="4a365-179">Jak funguje importu a exportu s vlastní databáze nastavení?</span><span class="sxs-lookup"><span data-stu-id="4a365-179">How does Import/Export work with a custom databases setting?</span></span>
<span data-ttu-id="4a365-180">Některé cenové úrovně mají různé [databáze omezení](cache-configure.md#databases), takže existují některé aspekty při importu, pokud jste nakonfigurovali vlastní hodnotu `databases` nastavení během vytváření mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="4a365-180">Some pricing tiers have different [databases limits](cache-configure.md#databases), so there are some considerations when importing if you configured a custom value for the `databases` setting during cache creation.</span></span>

* <span data-ttu-id="4a365-181">Při importu do cenovou úroveň s nižší `databases` limit než vrstvy, ze kterého jste exportovali:</span><span class="sxs-lookup"><span data-stu-id="4a365-181">When importing to a pricing tier with a lower `databases` limit than the tier from which you exported:</span></span>
  * <span data-ttu-id="4a365-182">Pokud používáte výchozí počet `databases`, což je 16 pro všechny cenové úrovně, dojde ke ztrátě žádná data.</span><span class="sxs-lookup"><span data-stu-id="4a365-182">If you are using the default number of `databases`, which is 16 for all pricing tiers, no data is lost.</span></span>
  * <span data-ttu-id="4a365-183">Pokud používáte vlastní počet `databases` že patří do limity pro vrstvu, do které importujete, dojde ke ztrátě žádná data.</span><span class="sxs-lookup"><span data-stu-id="4a365-183">If you are using a custom number of `databases` that falls within the limits for the tier to which you are importing, no data is lost.</span></span>
  * <span data-ttu-id="4a365-184">Exportovaná data obsažená data v databázi, která překračuje omezení nové vrstvy, není k importu dat z těchto vyšší databází.</span><span class="sxs-lookup"><span data-stu-id="4a365-184">If your exported data contained data in a database that exceeds the limits of the new tier, the data from those higher databases is not imported.</span></span>

### <a name="how-is-importexport-different-from-redis-persistence"></a><span data-ttu-id="4a365-185">Jak se liší od trvalosti Redis importu a exportu?</span><span class="sxs-lookup"><span data-stu-id="4a365-185">How is Import/Export different from Redis persistence?</span></span>
<span data-ttu-id="4a365-186">Trvalost Azure Redis Cache vám umožňuje zachovat data uložená v Redis do služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="4a365-186">Azure Redis Cache persistence allows you to persist data stored in Redis to Azure Storage.</span></span> <span data-ttu-id="4a365-187">Pokud je nakonfigurovaná trvalost, trvá Azure Redis Cache snímek mezipaměti Redis v binárním formátu Redis na disk, na základě konfigurovat četnosti zálohování.</span><span class="sxs-lookup"><span data-stu-id="4a365-187">When persistence is configured, Azure Redis Cache persists a snapshot of the Redis cache in a Redis binary format to disk based on a configurable backup frequency.</span></span> <span data-ttu-id="4a365-188">Pokud dojde k závažné události, zakazující primární server a repliky mezipaměti, je obnoven data v mezipaměti automaticky pomocí poslední snímek.</span><span class="sxs-lookup"><span data-stu-id="4a365-188">If a catastrophic event occurs that disables both the primary and replica cache, the cache data is restored automatically using the most recent snapshot.</span></span> <span data-ttu-id="4a365-189">Další informace najdete v tématu [postup konfigurace trvalosti dat pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-persistence.md).</span><span class="sxs-lookup"><span data-stu-id="4a365-189">For more information, see [How to configure data persistence for a Premium Azure Redis Cache](cache-how-to-premium-persistence.md).</span></span>

<span data-ttu-id="4a365-190">Import / Export můžete přenést data do nebo exportovat z Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="4a365-190">Import/ Export allows you to bring data into or export from Azure Redis Cache.</span></span> <span data-ttu-id="4a365-191">Není konfigurace zálohování a obnovení pomocí trvalosti Redis.</span><span class="sxs-lookup"><span data-stu-id="4a365-191">It does not configure backup and restore using Redis persistence.</span></span>

### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a><span data-ttu-id="4a365-192">Můžete automatizovat Import a Export pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiné správy klientů?</span><span class="sxs-lookup"><span data-stu-id="4a365-192">Can I automate Import/Export using PowerShell, CLI, or other management clients?</span></span>
<span data-ttu-id="4a365-193">Ano, pro prostředí PowerShell pokyny naleznete v části [import mezipaměti Redis](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) a [Export mezipaměti Redis](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).</span><span class="sxs-lookup"><span data-stu-id="4a365-193">Yes, for PowerShell instructions see [To import a Redis cache](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) and [To export a Redis cache](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).</span></span>

### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a><span data-ttu-id="4a365-194">Zobrazila vypršení časového limitu během Moje operace importu a exportu.</span><span class="sxs-lookup"><span data-stu-id="4a365-194">I received a timeout error during my Import/Export operation.</span></span> <span data-ttu-id="4a365-195">Co to znamená?</span><span class="sxs-lookup"><span data-stu-id="4a365-195">What does it mean?</span></span>
<span data-ttu-id="4a365-196">Pokud zůstanou na **importovat data** nebo **exportovat data** okno delší než 15 minut před zahájením operace, obdržíte chybu s chybovou zprávou podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="4a365-196">If you remain on the **Import data** or **Export data** blade for longer than 15 minutes before initiating the operation, you receive an error with an error message similar to the following example:</span></span>

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

<span data-ttu-id="4a365-197">Chcete-li tento problém vyřešili, spusťte import nebo export fungování předtím, než 15 minut uplynul.</span><span class="sxs-lookup"><span data-stu-id="4a365-197">To resolve this, initiate the import or export operation before 15 minutes has elapsed.</span></span>

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a><span data-ttu-id="4a365-198">Chyba se zobrazí chybové při exportu svá data do úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="4a365-198">I got an error when exporting my data to Azure Blob Storage.</span></span> <span data-ttu-id="4a365-199">Co se přihodilo?</span><span class="sxs-lookup"><span data-stu-id="4a365-199">What happened?</span></span>
<span data-ttu-id="4a365-200">Export pracuje pouze s RDB soubory uložené jako objekty BLOB stránky.</span><span class="sxs-lookup"><span data-stu-id="4a365-200">Export works only with RDB files stored as page blobs.</span></span> <span data-ttu-id="4a365-201">Jiné typy objektů blob nejsou aktuálně podporovány, včetně účty úložiště blob s horká a studená vrstvami.</span><span class="sxs-lookup"><span data-stu-id="4a365-201">Other blob types are not currently supported, including blob storage accounts with hot and cool tiers.</span></span> <span data-ttu-id="4a365-202">Další informace najdete v tématu [účty úložiště Blob](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts).</span><span class="sxs-lookup"><span data-stu-id="4a365-202">For more information, see [Blob storage accounts](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4a365-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4a365-203">Next steps</span></span>
<span data-ttu-id="4a365-204">Naučte se používat další funkce mezipaměti premium.</span><span class="sxs-lookup"><span data-stu-id="4a365-204">Learn how to use more premium cache features.</span></span>

* [<span data-ttu-id="4a365-205">Úvod do Azure Redis Cache na úrovni Premium</span><span class="sxs-lookup"><span data-stu-id="4a365-205">Introduction to the Azure Redis Cache Premium tier</span></span>](cache-premium-tier-intro.md)    

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
