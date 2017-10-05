---
title: "Vytvoření exportu úlohy pro Azure Import/Export | Microsoft Docs"
description: "Naučte se vytvářet úlohy exportu pro službu Microsoft Azure Import/Export."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 613d480b-a8ef-4b28-8f54-54174d59b3f4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: bdeac373aa8270bd9de8f135ec7166d744fd83ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="creating-an-export-job-for-the-azure-importexport-service"></a><span data-ttu-id="8117c-103">Vytvoření úlohy exportu pro službu Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="8117c-103">Creating an export job for the Azure Import/Export service</span></span>
<span data-ttu-id="8117c-104">Vytvoření úlohy exportu pro službu Microsoft Azure Import/Export pomocí rozhraní REST API zahrnuje následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8117c-104">Creating an export job for the Microsoft Azure Import/Export service using the REST API involves the following steps:</span></span>

-   <span data-ttu-id="8117c-105">Výběr objektů blob pro export.</span><span class="sxs-lookup"><span data-stu-id="8117c-105">Selecting the blobs to export.</span></span>

-   <span data-ttu-id="8117c-106">Získání přesouvání umístění.</span><span class="sxs-lookup"><span data-stu-id="8117c-106">Obtaining a shipping location.</span></span>

-   <span data-ttu-id="8117c-107">Vytvoření úlohy exportu.</span><span class="sxs-lookup"><span data-stu-id="8117c-107">Creating the export job.</span></span>

-   <span data-ttu-id="8117c-108">Přesouvání prázdný jednotky společnosti Microsoft prostřednictvím podporovaných poskytovatel služby.</span><span class="sxs-lookup"><span data-stu-id="8117c-108">Shipping your empty drives to Microsoft via a supported carrier service.</span></span>

-   <span data-ttu-id="8117c-109">Úloha exportu aktualizace s informací o balíčku.</span><span class="sxs-lookup"><span data-stu-id="8117c-109">Updating the export job with the package information.</span></span>

-   <span data-ttu-id="8117c-110">Získání jednotky zpět od společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8117c-110">Receiving the drives back from Microsoft.</span></span>

 <span data-ttu-id="8117c-111">V tématu [pomocí služby Windows Azure Import/Export přenos dat do úložiště objektů Blob](storage-import-export-service.md) přehled službu Import/Export a kurz, který ukazuje, jak používat [portál Azure](https://portal.azure.com/) pro vytváření a správu import a export úloh.</span><span class="sxs-lookup"><span data-stu-id="8117c-111">See [Using the Windows Azure Import/Export service to Transfer Data to Blob Storage](storage-import-export-service.md) for an overview of the Import/Export service and a tutorial that demonstrates how to use the [Azure portal](https://portal.azure.com/) to create and manage import and export jobs.</span></span>

## <a name="selecting-blobs-to-export"></a><span data-ttu-id="8117c-112">Výběr objektů blob pro export</span><span class="sxs-lookup"><span data-stu-id="8117c-112">Selecting blobs to export</span></span>
 <span data-ttu-id="8117c-113">Pokud chcete vytvořit úlohy exportu, musíte poskytnout seznam objektů BLOB, které chcete exportovat z vašeho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="8117c-113">To create an export job, you will need to provide a list of blobs that you want to export from your storage account.</span></span> <span data-ttu-id="8117c-114">Vyberte objekty BLOB export několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="8117c-114">There are a few ways to select blobs to be exported:</span></span>

-   <span data-ttu-id="8117c-115">Cesta relativní objektů blob můžete vybrat jediného objektu blob a všechny jeho snímků.</span><span class="sxs-lookup"><span data-stu-id="8117c-115">You can use a relative blob path to select a single blob and all of its snapshots.</span></span>

-   <span data-ttu-id="8117c-116">Cesta relativní objektů blob můžete vybrat jeden objekt blob, s výjimkou jeho snímky.</span><span class="sxs-lookup"><span data-stu-id="8117c-116">You can use a relative blob path to select a single blob excluding its snapshots.</span></span>

-   <span data-ttu-id="8117c-117">Vyberte jeden snímek můžete cesta relativní objektů blob a čas snímku.</span><span class="sxs-lookup"><span data-stu-id="8117c-117">You can use a relative blob path and a snapshot time to select a single snapshot.</span></span>

-   <span data-ttu-id="8117c-118">Předponu objektu blob můžete vybrat všechny objekty BLOB a snímky s danou předponu.</span><span class="sxs-lookup"><span data-stu-id="8117c-118">You can use a blob prefix to select all blobs and snapshots with the given prefix.</span></span>

-   <span data-ttu-id="8117c-119">Můžete exportovat všechny objekty BLOB a snímky v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="8117c-119">You can export all blobs and snapshots in the storage account.</span></span>

 <span data-ttu-id="8117c-120">Další informace o zadání objektů blob pro export, najdete v článku [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operaci.</span><span class="sxs-lookup"><span data-stu-id="8117c-120">For more information about specifying blobs to export, see the [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span>

## <a name="obtaining-your-shipping-location"></a><span data-ttu-id="8117c-121">Získání vaši polohu přesouvání</span><span class="sxs-lookup"><span data-stu-id="8117c-121">Obtaining your shipping location</span></span>
<span data-ttu-id="8117c-122">Před vytvořením úlohy exportu, je nutné získat přenosů umístění názvu a adresy voláním [získat umístění](https://portal.azure.com) nebo [umístění seznamu](/rest/api/storageimportexport/listlocations) operaci.</span><span class="sxs-lookup"><span data-stu-id="8117c-122">Before creating an export job, you need to obtain a shipping location name and address by calling the [Get Location](https://portal.azure.com) or [List Locations](/rest/api/storageimportexport/listlocations) operation.</span></span> <span data-ttu-id="8117c-123">`List Locations`Vrátí seznam umístění a jejich poštovní adresy.</span><span class="sxs-lookup"><span data-stu-id="8117c-123">`List Locations` will return a list of locations and their mailing addresses.</span></span> <span data-ttu-id="8117c-124">Můžete vybrat umístění ze seznamu vrácených a dodávat vaše pevné disky na tuto adresu.</span><span class="sxs-lookup"><span data-stu-id="8117c-124">You can select a location from the returned list and ship your hard drives to that address.</span></span> <span data-ttu-id="8117c-125">Můžete také `Get Location` operace přesouvání adresu pro konkrétní umístění získat přímo.</span><span class="sxs-lookup"><span data-stu-id="8117c-125">You can also use the `Get Location` operation to obtain the shipping address for a specific location directly.</span></span>

<span data-ttu-id="8117c-126">Použijte následující postup k získání přesouvání umístění:</span><span class="sxs-lookup"><span data-stu-id="8117c-126">Follow the steps below to obtain the shipping location:</span></span>

-   <span data-ttu-id="8117c-127">Určete název umístění účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="8117c-127">Identify the name of the location of your storage account.</span></span> <span data-ttu-id="8117c-128">Tuto hodnotu najdete v části **umístění** na účet úložiště **řídicí panel** v klasickém portálu nebo předmětem dotazu pro pomocí operace rozhraní API pro správu služby [získat vlastnosti účtu úložiště](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span><span class="sxs-lookup"><span data-stu-id="8117c-128">This value can be found under the **Location** field on the storage account's **Dashboard** in the classic portal or queried for by using the service management API operation [Get Storage Account Properties](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span></span>

-   <span data-ttu-id="8117c-129">Načíst umístění, které jsou k dispozici pro zpracování tento účet úložiště pomocí volání `Get Location` operaci.</span><span class="sxs-lookup"><span data-stu-id="8117c-129">Retrieve the location that are available to process this storage account by calling the `Get Location` operation.</span></span>

-   <span data-ttu-id="8117c-130">Pokud `AlternateLocations` vlastnost umístění obsahuje umístění, sám sebe a potom je to v pořádku pro toto umístění používat.</span><span class="sxs-lookup"><span data-stu-id="8117c-130">If the `AlternateLocations` property of the location contains the location itself, then it is okay to use this location.</span></span> <span data-ttu-id="8117c-131">Jinak volání `Get Location` operaci zopakovat s alternativní umístění.</span><span class="sxs-lookup"><span data-stu-id="8117c-131">Otherwise, call the `Get Location` operation again with one of the alternate locations.</span></span> <span data-ttu-id="8117c-132">Do původního umístění může být pro údržbu dočasně ukončeny.</span><span class="sxs-lookup"><span data-stu-id="8117c-132">The original location might be temporarily closed for maintenance.</span></span>

## <a name="creating-the-export-job"></a><span data-ttu-id="8117c-133">Vytvoření úlohy exportu</span><span class="sxs-lookup"><span data-stu-id="8117c-133">Creating the export job</span></span>
 <span data-ttu-id="8117c-134">Chcete-li vytvořit úlohu export, zavolejte [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operaci.</span><span class="sxs-lookup"><span data-stu-id="8117c-134">To create the export job, call the [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span> <span data-ttu-id="8117c-135">Budete muset zadat následující informace:</span><span class="sxs-lookup"><span data-stu-id="8117c-135">You will need to provide the following information:</span></span>

-   <span data-ttu-id="8117c-136">Název úlohy.</span><span class="sxs-lookup"><span data-stu-id="8117c-136">A name for the job.</span></span>

-   <span data-ttu-id="8117c-137">Název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="8117c-137">The storage account name.</span></span>

-   <span data-ttu-id="8117c-138">Přenosů název umístění, získaných v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="8117c-138">The shipping location name, obtained in the previous step.</span></span>

-   <span data-ttu-id="8117c-139">Typ úlohy (exportovat).</span><span class="sxs-lookup"><span data-stu-id="8117c-139">A job type (Export).</span></span>

-   <span data-ttu-id="8117c-140">Zpětná adresa, kde by měly být odeslány jednotky po dokončení úlohy exportu.</span><span class="sxs-lookup"><span data-stu-id="8117c-140">The return address where the drives should be sent after the export job has completed.</span></span>

-   <span data-ttu-id="8117c-141">Seznam objektů BLOB (nebo objekt blob předpony) pro export.</span><span class="sxs-lookup"><span data-stu-id="8117c-141">The list of blobs (or blob prefixes) to be exported.</span></span>

## <a name="shipping-your-drives"></a><span data-ttu-id="8117c-142">Přesouvání jednotky</span><span class="sxs-lookup"><span data-stu-id="8117c-142">Shipping your drives</span></span>
 <span data-ttu-id="8117c-143">V dalším kroku nástrojem Azure Import/Export můžete určit počet jednotek, které potřebujete k odeslání, na základě objektů BLOB, které jste vybrali pro export a velikost disku.</span><span class="sxs-lookup"><span data-stu-id="8117c-143">Next, use the Azure Import/Export Tool to determine the number of drives you need to send, based on the blobs you have selected to be exported and the drive size.</span></span> <span data-ttu-id="8117c-144">Najdete v článku [Azure Import/Export nástroj odkaz](storage-import-export-tool-how-to-v1.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="8117c-144">See the [Azure Import/Export Tool Reference](storage-import-export-tool-how-to-v1.md) for details.</span></span>

 <span data-ttu-id="8117c-145">Balíček jednotek v jednom balíčku a jejich odeslání na adresu získaných v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="8117c-145">Package the drives in a single package and ship them to the address obtained in the earlier step.</span></span> <span data-ttu-id="8117c-146">Poznamenejte si číslo sledování vašeho balíčku pro další krok.</span><span class="sxs-lookup"><span data-stu-id="8117c-146">Note the tracking number of your package for the next step.</span></span>

> [!NOTE]
>  <span data-ttu-id="8117c-147">Je nutné dodat jednotky prostřednictvím podporovaných poskytovatel služby, která bude poskytovat sledování Číslo pro svůj balíček.</span><span class="sxs-lookup"><span data-stu-id="8117c-147">You must ship your drives via a supported carrier service, which will provide a tracking number for your package.</span></span>

## <a name="updating-the-export-job-with-your-package-information"></a><span data-ttu-id="8117c-148">Aktualizace úlohy exportu s informací o balíčku</span><span class="sxs-lookup"><span data-stu-id="8117c-148">Updating the export job with your package information</span></span>
 <span data-ttu-id="8117c-149">Až budete mít vaše číslo sledování, volání [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operace aktualizovala poskytovatel název a číslo úlohy sledování.</span><span class="sxs-lookup"><span data-stu-id="8117c-149">After you have your tracking number, call the [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation to updated the carrier name and tracking number for the job.</span></span> <span data-ttu-id="8117c-150">Volitelně můžete zadat počet jednotek, zpáteční adresu a také přesouvání datum.</span><span class="sxs-lookup"><span data-stu-id="8117c-150">You can optionally specify the number of drives, the return address, and the shipping date as well.</span></span>

## <a name="receiving-the-package"></a><span data-ttu-id="8117c-151">Přijetí balíčku</span><span class="sxs-lookup"><span data-stu-id="8117c-151">Receiving the package</span></span>
 <span data-ttu-id="8117c-152">Po zpracování vaše úloha exportu, budou vráceny jednotky vám s šifrovaná data.</span><span class="sxs-lookup"><span data-stu-id="8117c-152">After your export job has been processed, your drives will be returned to you with your encrypted data.</span></span> <span data-ttu-id="8117c-153">Klíč nástroje BitLocker pro každou z jednotky můžete načíst pomocí volání [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operaci.</span><span class="sxs-lookup"><span data-stu-id="8117c-153">You can retrieve the BitLocker key for each of the drives by calling the [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operation.</span></span> <span data-ttu-id="8117c-154">Potom můžete odemknout jednotku pomocí klíče.</span><span class="sxs-lookup"><span data-stu-id="8117c-154">You can then unlock the drive using the key.</span></span> <span data-ttu-id="8117c-155">Soubor manifestu jednotku na každém disku obsahuje seznam souborů na jednotce, stejně jako původní adresu objektů blob pro každý soubor.</span><span class="sxs-lookup"><span data-stu-id="8117c-155">The drive manifest file on each drive contains the list of files on the drive, as well as the original blob address for each file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8117c-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8117c-156">Next steps</span></span>

* [<span data-ttu-id="8117c-157">Pomocí REST API služby importu a exportu</span><span class="sxs-lookup"><span data-stu-id="8117c-157">Using the Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
