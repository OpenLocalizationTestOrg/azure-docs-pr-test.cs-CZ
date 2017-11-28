---
title: "aaaCreate úlohy importu pro Azure Import/Export | Microsoft Docs"
description: "Zjistěte, jak toocreate importu pro hello služby Microsoft Azure Import/Export."
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
ms.openlocfilehash: da974c33a3688bb5e2412c8bfcbeca704096c2fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-import-job-for-hello-azure-importexport-service"></a><span data-ttu-id="4cc8e-103">Vytvoření úlohy importu pro hello služba Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="4cc8e-103">Creating an import job for hello Azure Import/Export service</span></span>

<span data-ttu-id="4cc8e-104">Vytvoření úlohy importu pro službu Microsoft Azure Import/Export hello pomocí hello REST API zahrnuje hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4cc8e-104">Creating an import job for hello Microsoft Azure Import/Export service using hello REST API involves hello following steps:</span></span>

-   <span data-ttu-id="4cc8e-105">Příprava jednotky se hello nástroj Azure Import/Export.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-105">Preparing drives with hello Azure Import/Export Tool.</span></span>

-   <span data-ttu-id="4cc8e-106">Získání hello umístění toowhich tooship hello jednotky.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-106">Obtaining hello location toowhich tooship hello drive.</span></span>

-   <span data-ttu-id="4cc8e-107">Vytvoření úlohy importu hello.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-107">Creating hello import job.</span></span>

-   <span data-ttu-id="4cc8e-108">Přesouvání hello jednotky tooMicrosoft prostřednictvím podporovaných poskytovatel služby.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-108">Shipping hello drives tooMicrosoft via a supported carrier service.</span></span>

-   <span data-ttu-id="4cc8e-109">Probíhá aktualizace úlohy importu hello hello přesouvání podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-109">Updating hello import job with hello shipping details.</span></span>

 <span data-ttu-id="4cc8e-110">V tématu [pomocí hello Microsoft Azure Import/Export služby tooTransfer Data tooBlob úložiště](storage-import-export-service.md) přehled hello importu/exportu služby a kurz, který ukazuje, jak toouse hello [portál Azure](https://portal.azure.com/) toocreate a spravovat import a export úloh.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-110">See [Using hello Microsoft Azure Import/Export service tooTransfer Data tooBlob Storage](storage-import-export-service.md) for an overview of hello Import/Export service and a tutorial that demonstrates how toouse hello [Azure  portal](https://portal.azure.com/) toocreate and manage import and export jobs.</span></span>

## <a name="preparing-drives-with-hello-azure-importexport-tool"></a><span data-ttu-id="4cc8e-111">Příprava jednotky se hello nástroj Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="4cc8e-111">Preparing drives with hello Azure Import/Export Tool</span></span>

<span data-ttu-id="4cc8e-112">pro úlohy importu Hello jednotek tooprepare kroky jsou hello stejné jestli vytvoříte hello jobvia hello portálu nebo pomocí hello REST API.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-112">hello steps tooprepare drives for an import job are hello same whether you create hello jobvia hello portal or via hello REST API.</span></span>

<span data-ttu-id="4cc8e-113">Níže je stručný přehled přípravy jednotky.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-113">Below is a brief overview of drive preparation.</span></span> <span data-ttu-id="4cc8e-114">Odkazovat toohello [Azure Import ExportTool odkaz](storage-import-export-tool-how-to-v1.md) úplné pokyny.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-114">Refer toohello [Azure Import-ExportTool Reference](storage-import-export-tool-how-to-v1.md) for complete instructions.</span></span> <span data-ttu-id="4cc8e-115">Můžete si stáhnout hello nástroj Azure Import/Export [zde](http://go.microsoft.com/fwlink/?LinkID=301900).</span><span class="sxs-lookup"><span data-stu-id="4cc8e-115">You can download hello Azure Import/Export Tool [here](http://go.microsoft.com/fwlink/?LinkID=301900).</span></span>

<span data-ttu-id="4cc8e-116">Příprava vašeho disku zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="4cc8e-116">Preparing your drive involves:</span></span>

-   <span data-ttu-id="4cc8e-117">Identifikace toobe hello data importovat.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-117">Identifying hello data toobe imported.</span></span>

-   <span data-ttu-id="4cc8e-118">Identifikace hello cílové objekty BLOB v systému Windows Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-118">Identifying hello destination blobs in Windows Azure Storage.</span></span>

-   <span data-ttu-id="4cc8e-119">Pomocí nástroje Azure Import/Export toocopy hello vaše data tooone nebo více pevných disků.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-119">Using hello Azure Import/Export Tool toocopy your data tooone or more hard drives.</span></span>

 <span data-ttu-id="4cc8e-120">Hello nástroj Azure Import/Export také vygeneruje soubor manifestu pro každou hello jednotek, jako je připraveno.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-120">hello Azure Import/Export Tool will also generate a manifest file for each of hello drives as it is prepared.</span></span> <span data-ttu-id="4cc8e-121">Soubor manifestu obsahuje:</span><span class="sxs-lookup"><span data-stu-id="4cc8e-121">A manifest file contains:</span></span>

-   <span data-ttu-id="4cc8e-122">Výčet všech hello soubory určené pro odesílání a hello mapování z tooblobs tyto soubory.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-122">An enumeration of all hello files intended for upload and hello mappings from these files tooblobs.</span></span>

-   <span data-ttu-id="4cc8e-123">Kontrolní součty segmentů hello každého souboru.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-123">Checksums of hello segments of each file.</span></span>

-   <span data-ttu-id="4cc8e-124">Informace o hello metadata a vlastnosti tooassociate s jednotlivých objektů blob.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-124">Information about hello metadata and properties tooassociate with each blob.</span></span>

-   <span data-ttu-id="4cc8e-125">Výpis tootake akce hello, pokud má objekt blob, který se nahrává hello stejný název jako existující objekt blob v kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-125">A listing of hello action tootake if a blob that is being uploaded has hello same name as an existing blob in hello container.</span></span> <span data-ttu-id="4cc8e-126">Dostupné možnosti patří: a) souborem hello přepsat hello blob, b) zachovat existující objekt blob hello a skip nahrávání souboru hello, c) připojit příponu toohello tak, aby nedošlo ke konfliktu s ostatními soubory.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-126">Possible options are: a) overwrite hello blob with hello file, b) keep hello existing blob and skip uploading hello file, c) append a suffix toohello name so that it does not conflict with other files.</span></span>

## <a name="obtaining-your-shipping-location"></a><span data-ttu-id="4cc8e-127">Získání vaši polohu přesouvání</span><span class="sxs-lookup"><span data-stu-id="4cc8e-127">Obtaining your shipping location</span></span>

<span data-ttu-id="4cc8e-128">Před vytvořením úlohy importu, budete potřebovat tooobtain přenosů název umístění a adresy podle volání hello [umístění seznamu](/rest/api/storageimportexport/listlocations) operaci.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-128">Before creating an import job, you need tooobtain a shipping location name and address by calling hello [List Locations](/rest/api/storageimportexport/listlocations) operation.</span></span> <span data-ttu-id="4cc8e-129">`List Locations`Vrátí seznam umístění a jejich poštovní adresy.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-129">`List Locations` will return a list of locations and their mailing addresses.</span></span> <span data-ttu-id="4cc8e-130">Můžete vybrat umístění z hello vrátil seznam a dodávat vaše adresa toothat pevné disky.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-130">You can select a location from hello returned list and ship your hard drives toothat address.</span></span> <span data-ttu-id="4cc8e-131">Můžete taky hello `Get Location` operace tooobtain hello přímo přesouvání adresu konkrétního umístění.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-131">You can also use hello `Get Location` operation tooobtain hello shipping address for a specific location directly.</span></span>

 <span data-ttu-id="4cc8e-132">Postupujte podle kroků hello tooobtain hello přenosů umístění:</span><span class="sxs-lookup"><span data-stu-id="4cc8e-132">Follow hello steps below tooobtain hello shipping location:</span></span>

-   <span data-ttu-id="4cc8e-133">Určení názvu hello hello umístění účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-133">Identify hello name of hello location of your storage account.</span></span> <span data-ttu-id="4cc8e-134">Tuto hodnotu najdete v části hello **umístění** na účet úložiště hello **řídicí panel** v hello Azure portálu nebo předmětem dotazu pro pomocí hello service management operace rozhraní API [získat úložiště Účet vlastnosti](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span><span class="sxs-lookup"><span data-stu-id="4cc8e-134">This value can be found under hello **Location** field on hello storage account's **Dashboard** in hello Azure portal or queried for by using hello service management API operation [Get Storage Account Properties](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span></span>

-   <span data-ttu-id="4cc8e-135">Načíst hello umístění, které je k dispozici tooprocess tento účet úložiště tak, že volání hello `Get Location` operaci.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-135">Retrieve hello location that is available tooprocess this storage account by calling hello `Get Location` operation.</span></span>

-   <span data-ttu-id="4cc8e-136">Pokud hello `AlternateLocations` vlastnost hello umístění obsahuje hello umístění sám sebe a potom je v pořádku toouse toto umístění.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-136">If hello `AlternateLocations` property of hello location contains hello location itself, then it is okay toouse this location.</span></span> <span data-ttu-id="4cc8e-137">Jinak volání hello `Get Location` operaci zopakovat s hello alternativního umístění.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-137">Otherwise, call hello `Get Location` operation again with one of hello alternate locations.</span></span> <span data-ttu-id="4cc8e-138">Hello původního umístění může být dočasně ukončeny kvůli údržbě.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-138">hello original location might be temporarily closed for maintenance.</span></span>

## <a name="creating-hello-import-job"></a><span data-ttu-id="4cc8e-139">Vytvoření úlohy importu hello</span><span class="sxs-lookup"><span data-stu-id="4cc8e-139">Creating hello import job</span></span>
<span data-ttu-id="4cc8e-140">Úloha importu toocreate hello, volání hello [Put úlohy](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operaci.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-140">toocreate hello import job, call hello [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span> <span data-ttu-id="4cc8e-141">Budete potřebovat tooprovide hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="4cc8e-141">You will need tooprovide hello following information:</span></span>

-   <span data-ttu-id="4cc8e-142">Název úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-142">A name for hello job.</span></span>

-   <span data-ttu-id="4cc8e-143">název účtu úložiště Hello.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-143">hello storage account name.</span></span>

-   <span data-ttu-id="4cc8e-144">Hello přesouvání název umístění, získané z předchozího kroku hello.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-144">hello shipping location name, obtained from hello previous step.</span></span>

-   <span data-ttu-id="4cc8e-145">Typ úlohy (Importovat).</span><span class="sxs-lookup"><span data-stu-id="4cc8e-145">A job type (Import).</span></span>

-   <span data-ttu-id="4cc8e-146">zpětná adresa Hello kde hello jednotky by měly být odeslány po dokončení úlohy importu hello.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-146">hello return address where hello drives should be sent after hello import job has completed.</span></span>

-   <span data-ttu-id="4cc8e-147">Hello seznam jednotek v úloze hello.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-147">hello list of drives in hello job.</span></span> <span data-ttu-id="4cc8e-148">Pro každou jednotku musí zahrnovat následující informace, který byl získán během kroku přípravy jednotky hello hello:</span><span class="sxs-lookup"><span data-stu-id="4cc8e-148">For each drive, you must include hello following information that was obtained during hello drive preparation step:</span></span>

    -   <span data-ttu-id="4cc8e-149">jednotka Hello Id</span><span class="sxs-lookup"><span data-stu-id="4cc8e-149">hello drive Id</span></span>

    -   <span data-ttu-id="4cc8e-150">klíč nástroje BitLocker Hello</span><span class="sxs-lookup"><span data-stu-id="4cc8e-150">hello BitLocker key</span></span>

    -   <span data-ttu-id="4cc8e-151">relativní cesta souboru manifestu Hello na pevném disku hello</span><span class="sxs-lookup"><span data-stu-id="4cc8e-151">hello manifest file relative path on hello hard drive</span></span>

    -   <span data-ttu-id="4cc8e-152">Hello Base16 kódovaný hodnota hash MD5 souboru manifestu</span><span class="sxs-lookup"><span data-stu-id="4cc8e-152">hello Base16 encoded manifest file MD5 hash</span></span>

## <a name="shipping-your-drives"></a><span data-ttu-id="4cc8e-153">Přesouvání jednotky</span><span class="sxs-lookup"><span data-stu-id="4cc8e-153">Shipping your drives</span></span>
<span data-ttu-id="4cc8e-154">Je nutné dodat vaši adresu toohello jednotky, který jste získali v předchozím kroku hello a je nutné zadat hello importu/exportu služby s hello sledování počtu hello balíčku.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-154">You must ship your drives toohello address that you obtained from hello previous step, and you must provide hello Import/Export service with hello tracking number of hello package.</span></span>

> [!NOTE]
>  <span data-ttu-id="4cc8e-155">Je nutné dodat jednotky prostřednictvím podporovaných poskytovatel služby, která bude poskytovat sledování Číslo pro svůj balíček.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-155">You must ship your drives via a supported carrier service, which will provide a tracking number for your package.</span></span>

## <a name="updating-hello-import-job-with-your-shipping-information"></a><span data-ttu-id="4cc8e-156">Aktualizace úlohy importu hello s informace o způsobu dodání</span><span class="sxs-lookup"><span data-stu-id="4cc8e-156">Updating hello import job with your shipping information</span></span>
<span data-ttu-id="4cc8e-157">Až budete mít vaše číslo sledování, volání hello [vlastnosti úlohy aktualizace](/api/storageimportexport/jobs#Jobs_Update) hello tooupdate operace přesouvání poskytovatel název, číslo sledování hello hello úlohy a hello poskytovatel účet číslo návratový přesouvání.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-157">After you have your tracking number, call hello [Update Job Properties](/api/storageimportexport/jobs#Jobs_Update) operation tooupdate hello shipping carrier name, hello tracking number for hello job, and hello carrier account number for return shipping.</span></span> <span data-ttu-id="4cc8e-158">Volitelně můžete zadat počet hello jednotky a hello přesouvání i datum.</span><span class="sxs-lookup"><span data-stu-id="4cc8e-158">You can optionally specify hello number of drives and hello shipping date as well.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4cc8e-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4cc8e-159">Next steps</span></span>

* [<span data-ttu-id="4cc8e-160">Pomocí REST API služby importu a exportu hello</span><span class="sxs-lookup"><span data-stu-id="4cc8e-160">Using hello Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
