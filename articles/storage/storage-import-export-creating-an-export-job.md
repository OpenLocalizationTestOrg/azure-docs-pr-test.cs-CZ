---
title: "aaaCreate exportu úlohy pro Azure Import/Export | Microsoft Docs"
description: "Zjistěte, jak toocreate exportu úlohy pro hello služby Microsoft Azure Import/Export."
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
ms.openlocfilehash: 4a10b42cc86dbf3bcea3a515bc065e2259228ef9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-export-job-for-hello-azure-importexport-service"></a><span data-ttu-id="714fa-103">Vytvoření úlohy exportu pro hello služba Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="714fa-103">Creating an export job for hello Azure Import/Export service</span></span>
<span data-ttu-id="714fa-104">Vytvoření úlohy exportu pro službu Microsoft Azure Import/Export hello pomocí hello REST API zahrnuje hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="714fa-104">Creating an export job for hello Microsoft Azure Import/Export service using hello REST API involves hello following steps:</span></span>

-   <span data-ttu-id="714fa-105">Výběr hello objekty BLOB tooexport.</span><span class="sxs-lookup"><span data-stu-id="714fa-105">Selecting hello blobs tooexport.</span></span>

-   <span data-ttu-id="714fa-106">Získání přesouvání umístění.</span><span class="sxs-lookup"><span data-stu-id="714fa-106">Obtaining a shipping location.</span></span>

-   <span data-ttu-id="714fa-107">Vytvoření úlohy exportu hello.</span><span class="sxs-lookup"><span data-stu-id="714fa-107">Creating hello export job.</span></span>

-   <span data-ttu-id="714fa-108">Přesouvání vaší prázdný jednotky tooMicrosoft prostřednictvím podporovaných poskytovatel služby.</span><span class="sxs-lookup"><span data-stu-id="714fa-108">Shipping your empty drives tooMicrosoft via a supported carrier service.</span></span>

-   <span data-ttu-id="714fa-109">Probíhá aktualizace informací o balíčku hello úloha exportu hello.</span><span class="sxs-lookup"><span data-stu-id="714fa-109">Updating hello export job with hello package information.</span></span>

-   <span data-ttu-id="714fa-110">Přijetí hello jednotky zpět od společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="714fa-110">Receiving hello drives back from Microsoft.</span></span>

 <span data-ttu-id="714fa-111">V tématu [pomocí hello Windows Azure Import/Export služby tooTransfer Data tooBlob úložiště](storage-import-export-service.md) přehled hello importu/exportu služby a kurz, který ukazuje, jak toouse hello [portál Azure](https://portal.azure.com/) toocreate a spravovat import a export úloh.</span><span class="sxs-lookup"><span data-stu-id="714fa-111">See [Using hello Windows Azure Import/Export service tooTransfer Data tooBlob Storage](storage-import-export-service.md) for an overview of hello Import/Export service and a tutorial that demonstrates how toouse hello [Azure portal](https://portal.azure.com/) toocreate and manage import and export jobs.</span></span>

## <a name="selecting-blobs-tooexport"></a><span data-ttu-id="714fa-112">Výběr tooexport objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="714fa-112">Selecting blobs tooexport</span></span>
 <span data-ttu-id="714fa-113">toocreate úlohy exportu, budete potřebovat tooprovide seznam objektů BLOB, který má tooexport z vašeho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="714fa-113">toocreate an export job, you will need tooprovide a list of blobs that you want tooexport from your storage account.</span></span> <span data-ttu-id="714fa-114">Existuje několik způsobů tooselect objekty BLOB toobe export:</span><span class="sxs-lookup"><span data-stu-id="714fa-114">There are a few ways tooselect blobs toobe exported:</span></span>

-   <span data-ttu-id="714fa-115">Můžete vytvořit tooselect cesta relativní objektů blob a jediného objektu blob a všechny jeho snímků.</span><span class="sxs-lookup"><span data-stu-id="714fa-115">You can use a relative blob path tooselect a single blob and all of its snapshots.</span></span>

-   <span data-ttu-id="714fa-116">Můžete vytvořit tooselect cesta relativní blob jediného objektu blob s výjimkou jeho snímků.</span><span class="sxs-lookup"><span data-stu-id="714fa-116">You can use a relative blob path tooselect a single blob excluding its snapshots.</span></span>

-   <span data-ttu-id="714fa-117">Můžete použít cestu k relativní objektů blob a tooselect čas snímku jeden snímek.</span><span class="sxs-lookup"><span data-stu-id="714fa-117">You can use a relative blob path and a snapshot time tooselect a single snapshot.</span></span>

-   <span data-ttu-id="714fa-118">Předpona tooselect objektů blob můžete použít všechny objekty BLOB a snímky s hello zadané předpony.</span><span class="sxs-lookup"><span data-stu-id="714fa-118">You can use a blob prefix tooselect all blobs and snapshots with hello given prefix.</span></span>

-   <span data-ttu-id="714fa-119">Můžete exportovat všechny objekty BLOB a snímky v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="714fa-119">You can export all blobs and snapshots in hello storage account.</span></span>

 <span data-ttu-id="714fa-120">Další informace o zadání objekty BLOB tooexport, najdete v části hello [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operaci.</span><span class="sxs-lookup"><span data-stu-id="714fa-120">For more information about specifying blobs tooexport, see hello [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span>

## <a name="obtaining-your-shipping-location"></a><span data-ttu-id="714fa-121">Získání vaši polohu přesouvání</span><span class="sxs-lookup"><span data-stu-id="714fa-121">Obtaining your shipping location</span></span>
<span data-ttu-id="714fa-122">Před vytvořením úlohy exportu, budete potřebovat tooobtain přenosů název umístění a adresy podle volání hello [získat umístění](https://portal.azure.com) nebo [umístění seznamu](/rest/api/storageimportexport/listlocations) operaci.</span><span class="sxs-lookup"><span data-stu-id="714fa-122">Before creating an export job, you need tooobtain a shipping location name and address by calling hello [Get Location](https://portal.azure.com) or [List Locations](/rest/api/storageimportexport/listlocations) operation.</span></span> <span data-ttu-id="714fa-123">`List Locations`Vrátí seznam umístění a jejich poštovní adresy.</span><span class="sxs-lookup"><span data-stu-id="714fa-123">`List Locations` will return a list of locations and their mailing addresses.</span></span> <span data-ttu-id="714fa-124">Můžete vybrat umístění z hello vrátil seznam a dodávat vaše adresa toothat pevné disky.</span><span class="sxs-lookup"><span data-stu-id="714fa-124">You can select a location from hello returned list and ship your hard drives toothat address.</span></span> <span data-ttu-id="714fa-125">Můžete taky hello `Get Location` operace tooobtain hello přímo přesouvání adresu konkrétního umístění.</span><span class="sxs-lookup"><span data-stu-id="714fa-125">You can also use hello `Get Location` operation tooobtain hello shipping address for a specific location directly.</span></span>

<span data-ttu-id="714fa-126">Postupujte podle kroků hello tooobtain hello přenosů umístění:</span><span class="sxs-lookup"><span data-stu-id="714fa-126">Follow hello steps below tooobtain hello shipping location:</span></span>

-   <span data-ttu-id="714fa-127">Určení názvu hello hello umístění účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="714fa-127">Identify hello name of hello location of your storage account.</span></span> <span data-ttu-id="714fa-128">Tuto hodnotu najdete v části hello **umístění** na účet úložiště hello **řídicí panel** v klasickém hello portálu nebo předmětem dotazu pro pomocí hello service management operace rozhraní API [získat Vlastnosti účtu úložiště](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span><span class="sxs-lookup"><span data-stu-id="714fa-128">This value can be found under hello **Location** field on hello storage account's **Dashboard** in hello classic portal or queried for by using hello service management API operation [Get Storage Account Properties](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span></span>

-   <span data-ttu-id="714fa-129">Načíst hello umístění, které jsou k dispozici tooprocess tento účet úložiště tak, že volání hello `Get Location` operaci.</span><span class="sxs-lookup"><span data-stu-id="714fa-129">Retrieve hello location that are available tooprocess this storage account by calling hello `Get Location` operation.</span></span>

-   <span data-ttu-id="714fa-130">Pokud hello `AlternateLocations` vlastnost hello umístění obsahuje hello umístění sám sebe a potom je v pořádku toouse toto umístění.</span><span class="sxs-lookup"><span data-stu-id="714fa-130">If hello `AlternateLocations` property of hello location contains hello location itself, then it is okay toouse this location.</span></span> <span data-ttu-id="714fa-131">Jinak volání hello `Get Location` operaci zopakovat s hello alternativního umístění.</span><span class="sxs-lookup"><span data-stu-id="714fa-131">Otherwise, call hello `Get Location` operation again with one of hello alternate locations.</span></span> <span data-ttu-id="714fa-132">Hello původního umístění může být dočasně ukončeny kvůli údržbě.</span><span class="sxs-lookup"><span data-stu-id="714fa-132">hello original location might be temporarily closed for maintenance.</span></span>

## <a name="creating-hello-export-job"></a><span data-ttu-id="714fa-133">Vytvoření úlohy exportu hello</span><span class="sxs-lookup"><span data-stu-id="714fa-133">Creating hello export job</span></span>
 <span data-ttu-id="714fa-134">Úloha exportu toocreate hello, volání hello [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operaci.</span><span class="sxs-lookup"><span data-stu-id="714fa-134">toocreate hello export job, call hello [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span> <span data-ttu-id="714fa-135">Budete potřebovat tooprovide hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="714fa-135">You will need tooprovide hello following information:</span></span>

-   <span data-ttu-id="714fa-136">Název úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="714fa-136">A name for hello job.</span></span>

-   <span data-ttu-id="714fa-137">název účtu úložiště Hello.</span><span class="sxs-lookup"><span data-stu-id="714fa-137">hello storage account name.</span></span>

-   <span data-ttu-id="714fa-138">Hello přesouvání název umístění, získaných v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="714fa-138">hello shipping location name, obtained in hello previous step.</span></span>

-   <span data-ttu-id="714fa-139">Typ úlohy (exportovat).</span><span class="sxs-lookup"><span data-stu-id="714fa-139">A job type (Export).</span></span>

-   <span data-ttu-id="714fa-140">zpětná adresa Hello kde hello jednotky by měly být odeslány po dokončení úlohy exportu hello.</span><span class="sxs-lookup"><span data-stu-id="714fa-140">hello return address where hello drives should be sent after hello export job has completed.</span></span>

-   <span data-ttu-id="714fa-141">seznam objektů BLOB (nebo objekt blob předpony) toobe Hello exportovat.</span><span class="sxs-lookup"><span data-stu-id="714fa-141">hello list of blobs (or blob prefixes) toobe exported.</span></span>

## <a name="shipping-your-drives"></a><span data-ttu-id="714fa-142">Přesouvání jednotky</span><span class="sxs-lookup"><span data-stu-id="714fa-142">Shipping your drives</span></span>
 <span data-ttu-id="714fa-143">V dalším kroku použít hello nástroj Azure Import/Export toodetermine hello počet jednotek, je nutné toosend, založené na objekty BLOB hello výběru toobe exportovali a hello velikost disku.</span><span class="sxs-lookup"><span data-stu-id="714fa-143">Next, use hello Azure Import/Export Tool toodetermine hello number of drives you need toosend, based on hello blobs you have selected toobe exported and hello drive size.</span></span> <span data-ttu-id="714fa-144">V tématu hello [Azure Import/Export nástroj odkaz](storage-import-export-tool-how-to-v1.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="714fa-144">See hello [Azure Import/Export Tool Reference](storage-import-export-tool-how-to-v1.md) for details.</span></span>

 <span data-ttu-id="714fa-145">Balíček hello jednotky v jednom balíčku a dodávat je toohello adres získaných v hello dříve krok.</span><span class="sxs-lookup"><span data-stu-id="714fa-145">Package hello drives in a single package and ship them toohello address obtained in hello earlier step.</span></span> <span data-ttu-id="714fa-146">Všimněte si hello sledování Číslo vašeho balíčku hello další krok.</span><span class="sxs-lookup"><span data-stu-id="714fa-146">Note hello tracking number of your package for hello next step.</span></span>

> [!NOTE]
>  <span data-ttu-id="714fa-147">Je nutné dodat jednotky prostřednictvím podporovaných poskytovatel služby, která bude poskytovat sledování Číslo pro svůj balíček.</span><span class="sxs-lookup"><span data-stu-id="714fa-147">You must ship your drives via a supported carrier service, which will provide a tracking number for your package.</span></span>

## <a name="updating-hello-export-job-with-your-package-information"></a><span data-ttu-id="714fa-148">Aktualizace úlohy exportu hello s informací o balíčku</span><span class="sxs-lookup"><span data-stu-id="714fa-148">Updating hello export job with your package information</span></span>
 <span data-ttu-id="714fa-149">Až budete mít vaše číslo sledování, volání hello [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operace tooupdated hello poskytovatel název a číslo pro úlohu hello sledování.</span><span class="sxs-lookup"><span data-stu-id="714fa-149">After you have your tracking number, call hello [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation tooupdated hello carrier name and tracking number for hello job.</span></span> <span data-ttu-id="714fa-150">Volitelně můžete zadat počet hello jednotky, hello zpáteční adresu a také hello přesouvání datum.</span><span class="sxs-lookup"><span data-stu-id="714fa-150">You can optionally specify hello number of drives, hello return address, and hello shipping date as well.</span></span>

## <a name="receiving-hello-package"></a><span data-ttu-id="714fa-151">Přijetí balíčku hello</span><span class="sxs-lookup"><span data-stu-id="714fa-151">Receiving hello package</span></span>
 <span data-ttu-id="714fa-152">Po zpracování vaše úloha exportu, jednotky, bude vrácen tooyou s šifrovaná data.</span><span class="sxs-lookup"><span data-stu-id="714fa-152">After your export job has been processed, your drives will be returned tooyou with your encrypted data.</span></span> <span data-ttu-id="714fa-153">Pro každou hello jednotek tím volání hello můžete načíst klíč nástroje BitLocker hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operaci.</span><span class="sxs-lookup"><span data-stu-id="714fa-153">You can retrieve hello BitLocker key for each of hello drives by calling hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operation.</span></span> <span data-ttu-id="714fa-154">Potom můžete odemknout jednotku hello pomocí klíče hello.</span><span class="sxs-lookup"><span data-stu-id="714fa-154">You can then unlock hello drive using hello key.</span></span> <span data-ttu-id="714fa-155">Soubor manifestu Hello jednotku na každém disku obsahuje hello seznam souborů na jednotce hello také hello původní objekt blob adresu pro každý soubor.</span><span class="sxs-lookup"><span data-stu-id="714fa-155">hello drive manifest file on each drive contains hello list of files on hello drive, as well as hello original blob address for each file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="714fa-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="714fa-156">Next steps</span></span>

* [<span data-ttu-id="714fa-157">Pomocí REST API služby importu a exportu hello</span><span class="sxs-lookup"><span data-stu-id="714fa-157">Using hello Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
