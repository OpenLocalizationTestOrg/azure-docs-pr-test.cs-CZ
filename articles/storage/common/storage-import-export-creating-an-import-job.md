---
title: "Vytvořit úlohu importu pro Azure Import/Export | Microsoft Docs"
description: "Naučte se vytvářet importu pro službu Microsoft Azure Import/Export."
author: muralikk
manager: syadav
editor: syadav
services: storage
documentationcenter: 
ms.assetid: 8b886e83-6148-4149-9d0f-5d48ec822475
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: d373d2a0e601f2796719fc5efb8761f276ab24d9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="creating-an-import-job-for-the-azure-importexport-service"></a><span data-ttu-id="d4cb8-103">Vytvoření úlohy importu do služby Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="d4cb8-103">Creating an import job for the Azure Import/Export service</span></span>

<span data-ttu-id="d4cb8-104">Vytvoření úlohy importu do služby Microsoft Azure Import/Export pomocí rozhraní REST API zahrnuje následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d4cb8-104">Creating an import job for the Microsoft Azure Import/Export service using the REST API involves the following steps:</span></span>

-   <span data-ttu-id="d4cb8-105">Příprava jednotky pomocí nástroje Azure Import/Export.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-105">Preparing drives with the Azure Import/Export Tool.</span></span>

-   <span data-ttu-id="d4cb8-106">Získání umístění, do kterého pro odeslání disk.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-106">Obtaining the location to which to ship the drive.</span></span>

-   <span data-ttu-id="d4cb8-107">Vytvoření úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-107">Creating the import job.</span></span>

-   <span data-ttu-id="d4cb8-108">Přesouvání jednotky společnosti Microsoft prostřednictvím podporovaných poskytovatel služby.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-108">Shipping the drives to Microsoft via a supported carrier service.</span></span>

-   <span data-ttu-id="d4cb8-109">Aktualizace úlohy importu s podrobnostmi o přesouvání.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-109">Updating the import job with the shipping details.</span></span>

 <span data-ttu-id="d4cb8-110">V tématu [pomocí služby Microsoft Azure Import/Export přenos dat do úložiště objektů Blob](storage-import-export-service.md) přehled službu Import/Export a kurz, který ukazuje, jak používat [portál Azure](https://portal.azure.com/) pro vytváření a správu import a export úloh.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-110">See [Using the Microsoft Azure Import/Export service to Transfer Data to Blob Storage](storage-import-export-service.md) for an overview of the Import/Export service and a tutorial that demonstrates how to use the [Azure  portal](https://portal.azure.com/) to create and manage import and export jobs.</span></span>

## <a name="preparing-drives-with-the-azure-importexport-tool"></a><span data-ttu-id="d4cb8-111">Příprava jednotky pomocí nástroje Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="d4cb8-111">Preparing drives with the Azure Import/Export Tool</span></span>

<span data-ttu-id="d4cb8-112">Kroky při přípravě jednotky pro úlohy importu jsou stejné, vytvořte jobvia portálu nebo pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-112">The steps to prepare drives for an import job are the same whether you create the jobvia the portal or via the REST API.</span></span>

<span data-ttu-id="d4cb8-113">Níže je stručný přehled přípravy jednotky.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-113">Below is a brief overview of drive preparation.</span></span> <span data-ttu-id="d4cb8-114">Odkazovat [Azure Import ExportTool odkaz](storage-import-export-tool-how-to-v1.md) úplné pokyny.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-114">Refer to the [Azure Import-ExportTool Reference](storage-import-export-tool-how-to-v1.md) for complete instructions.</span></span> <span data-ttu-id="d4cb8-115">Si můžete stáhnout nástroj Azure Import/Export [zde](http://go.microsoft.com/fwlink/?LinkID=301900).</span><span class="sxs-lookup"><span data-stu-id="d4cb8-115">You can download the Azure Import/Export Tool [here](http://go.microsoft.com/fwlink/?LinkID=301900).</span></span>

<span data-ttu-id="d4cb8-116">Příprava vašeho disku zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="d4cb8-116">Preparing your drive involves:</span></span>

-   <span data-ttu-id="d4cb8-117">Určení dat, která bude importována.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-117">Identifying the data to be imported.</span></span>

-   <span data-ttu-id="d4cb8-118">Identifikace cílové objekty BLOB v systému Windows Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-118">Identifying the destination blobs in Windows Azure Storage.</span></span>

-   <span data-ttu-id="d4cb8-119">Pomocí nástroje Azure Import/Export ke kopírování dat na jeden nebo více pevných disků.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-119">Using the Azure Import/Export Tool to copy your data to one or more hard drives.</span></span>

 <span data-ttu-id="d4cb8-120">Nástroj Azure Import/Export bude také generovat soubor manifestu pro každou z jednotky jako je připraveno.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-120">The Azure Import/Export Tool will also generate a manifest file for each of the drives as it is prepared.</span></span> <span data-ttu-id="d4cb8-121">Soubor manifestu obsahuje:</span><span class="sxs-lookup"><span data-stu-id="d4cb8-121">A manifest file contains:</span></span>

-   <span data-ttu-id="d4cb8-122">Výčet všech souborů určených pro odeslání a mapování z těchto souborů na objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-122">An enumeration of all the files intended for upload and the mappings from these files to blobs.</span></span>

-   <span data-ttu-id="d4cb8-123">Kontrolní součty segmentů každého souboru.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-123">Checksums of the segments of each file.</span></span>

-   <span data-ttu-id="d4cb8-124">Informace o metadatech a vlastnosti, které chcete přidružit každý objekt blob.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-124">Information about the metadata and properties to associate with each blob.</span></span>

-   <span data-ttu-id="d4cb8-125">Seznam pro případ, pokud objekt blob, který se nahrává má stejný název jako existující objekt blob v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-125">A listing of the action to take if a blob that is being uploaded has the same name as an existing blob in the container.</span></span> <span data-ttu-id="d4cb8-126">Dostupné možnosti patří: a) se souborem přepsat objektu blob, b) zachovat existující objekt blob a přeskočit nahrání souboru, c) přidejte příponu k názvu tak, aby nedošlo ke konfliktu s ostatními soubory.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-126">Possible options are: a) overwrite the blob with the file, b) keep the existing blob and skip uploading the file, c) append a suffix to the name so that it does not conflict with other files.</span></span>

## <a name="obtaining-your-shipping-location"></a><span data-ttu-id="d4cb8-127">Získání vaši polohu přesouvání</span><span class="sxs-lookup"><span data-stu-id="d4cb8-127">Obtaining your shipping location</span></span>

<span data-ttu-id="d4cb8-128">Před vytvořením úlohy importu, je nutné získat přenosů umístění názvu a adresy voláním [umístění seznamu](/rest/api/storageimportexport/listlocations) operaci.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-128">Before creating an import job, you need to obtain a shipping location name and address by calling the [List Locations](/rest/api/storageimportexport/listlocations) operation.</span></span> <span data-ttu-id="d4cb8-129">`List Locations`Vrátí seznam umístění a jejich poštovní adresy.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-129">`List Locations` will return a list of locations and their mailing addresses.</span></span> <span data-ttu-id="d4cb8-130">Můžete vybrat umístění ze seznamu vrácených a dodávat vaše pevné disky na tuto adresu.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-130">You can select a location from the returned list and ship your hard drives to that address.</span></span> <span data-ttu-id="d4cb8-131">Můžete také `Get Location` operace přesouvání adresu pro konkrétní umístění získat přímo.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-131">You can also use the `Get Location` operation to obtain the shipping address for a specific location directly.</span></span>

 <span data-ttu-id="d4cb8-132">Použijte následující postup k získání přesouvání umístění:</span><span class="sxs-lookup"><span data-stu-id="d4cb8-132">Follow the steps below to obtain the shipping location:</span></span>

-   <span data-ttu-id="d4cb8-133">Určete název umístění účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-133">Identify the name of the location of your storage account.</span></span> <span data-ttu-id="d4cb8-134">Tuto hodnotu najdete v části **umístění** na účet úložiště **řídicí panel** ve službě Azure portálu nebo předmětem dotazu pro pomocí operace rozhraní API pro správu služby [získat vlastnosti účtu úložiště](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span><span class="sxs-lookup"><span data-stu-id="d4cb8-134">This value can be found under the **Location** field on the storage account's **Dashboard** in the Azure portal or queried for by using the service management API operation [Get Storage Account Properties](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span></span>

-   <span data-ttu-id="d4cb8-135">Načíst umístění, které je k dispozici pro zpracování tento účet úložiště pomocí volání `Get Location` operaci.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-135">Retrieve the location that is available to process this storage account by calling the `Get Location` operation.</span></span>

-   <span data-ttu-id="d4cb8-136">Pokud `AlternateLocations` vlastnost umístění obsahuje umístění, sám sebe a potom je to v pořádku pro toto umístění používat.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-136">If the `AlternateLocations` property of the location contains the location itself, then it is okay to use this location.</span></span> <span data-ttu-id="d4cb8-137">Jinak volání `Get Location` operaci zopakovat s alternativní umístění.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-137">Otherwise, call the `Get Location` operation again with one of the alternate locations.</span></span> <span data-ttu-id="d4cb8-138">Do původního umístění může být pro údržbu dočasně ukončeny.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-138">The original location might be temporarily closed for maintenance.</span></span>

## <a name="creating-the-import-job"></a><span data-ttu-id="d4cb8-139">Vytvoření úlohy importu</span><span class="sxs-lookup"><span data-stu-id="d4cb8-139">Creating the import job</span></span>
<span data-ttu-id="d4cb8-140">Chcete-li vytvořit úlohu importu, zavolejte [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operaci.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-140">To create the import job, call the [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span> <span data-ttu-id="d4cb8-141">Budete muset zadat následující informace:</span><span class="sxs-lookup"><span data-stu-id="d4cb8-141">You will need to provide the following information:</span></span>

-   <span data-ttu-id="d4cb8-142">Název úlohy.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-142">A name for the job.</span></span>

-   <span data-ttu-id="d4cb8-143">Název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-143">The storage account name.</span></span>

-   <span data-ttu-id="d4cb8-144">Přenosů název umístění, získané z předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-144">The shipping location name, obtained from the previous step.</span></span>

-   <span data-ttu-id="d4cb8-145">Typ úlohy (Importovat).</span><span class="sxs-lookup"><span data-stu-id="d4cb8-145">A job type (Import).</span></span>

-   <span data-ttu-id="d4cb8-146">Zpětná adresa, kde by měly být odeslány jednotky po dokončení úlohy importu.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-146">The return address where the drives should be sent after the import job has completed.</span></span>

-   <span data-ttu-id="d4cb8-147">Seznam jednotek v úloze.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-147">The list of drives in the job.</span></span> <span data-ttu-id="d4cb8-148">Pro každou jednotku musí zahrnovat následující informace, které se zjišťovala během přípravy krok jednotky:</span><span class="sxs-lookup"><span data-stu-id="d4cb8-148">For each drive, you must include the following information that was obtained during the drive preparation step:</span></span>

    -   <span data-ttu-id="d4cb8-149">Id jednotky</span><span class="sxs-lookup"><span data-stu-id="d4cb8-149">The drive Id</span></span>

    -   <span data-ttu-id="d4cb8-150">Klíč nástroje BitLocker</span><span class="sxs-lookup"><span data-stu-id="d4cb8-150">The BitLocker key</span></span>

    -   <span data-ttu-id="d4cb8-151">Relativní cesta souboru manifestu na pevný disk.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-151">The manifest file relative path on the hard drive</span></span>

    -   <span data-ttu-id="d4cb8-152">Hodnota hash souboru manifestu MD5 kódovaný Base16</span><span class="sxs-lookup"><span data-stu-id="d4cb8-152">The Base16 encoded manifest file MD5 hash</span></span>

## <a name="shipping-your-drives"></a><span data-ttu-id="d4cb8-153">Přesouvání jednotky</span><span class="sxs-lookup"><span data-stu-id="d4cb8-153">Shipping your drives</span></span>
<span data-ttu-id="d4cb8-154">Je nutné dodat jednotky na adresu, kterou jste získali v předchozím kroku, a musíte poskytnout službu Import/Export číslem sledování balíčku.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-154">You must ship your drives to the address that you obtained from the previous step, and you must provide the Import/Export service with the tracking number of the package.</span></span>

> [!NOTE]
>  <span data-ttu-id="d4cb8-155">Je nutné dodat jednotky prostřednictvím podporovaných poskytovatel služby, která bude poskytovat sledování Číslo pro svůj balíček.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-155">You must ship your drives via a supported carrier service, which will provide a tracking number for your package.</span></span>

## <a name="updating-the-import-job-with-your-shipping-information"></a><span data-ttu-id="d4cb8-156">Aktualizace úlohy importu se vaše přenosů informace</span><span class="sxs-lookup"><span data-stu-id="d4cb8-156">Updating the import job with your shipping information</span></span>
<span data-ttu-id="d4cb8-157">Až budete mít vaše číslo sledování, volání [aktualizovat vlastnosti úlohy](/api/storageimportexport/jobs#Jobs_Update) operace aktualizace přenosů poskytovatel název, číslo sledování pro úlohu a poskytovatel číslo účtu pro návratový přesouvání.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-157">After you have your tracking number, call the [Update Job Properties](/api/storageimportexport/jobs#Jobs_Update) operation to update the shipping carrier name, the tracking number for the job, and the carrier account number for return shipping.</span></span> <span data-ttu-id="d4cb8-158">Volitelně můžete zadat počet jednotek a i přesouvání datum.</span><span class="sxs-lookup"><span data-stu-id="d4cb8-158">You can optionally specify the number of drives and the shipping date as well.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d4cb8-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d4cb8-159">Next steps</span></span>

* [<span data-ttu-id="d4cb8-160">Pomocí REST API služby importu a exportu</span><span class="sxs-lookup"><span data-stu-id="d4cb8-160">Using the Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
