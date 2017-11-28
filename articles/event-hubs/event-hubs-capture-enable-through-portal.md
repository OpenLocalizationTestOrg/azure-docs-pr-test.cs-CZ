---
title: "Povolit aaaAzure zaznamenat centra událostí prostřednictvím portálu | Microsoft Docs"
description: "Povolte funkci hello zaznamenat centra událostí pomocí hello portálu Azure."
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/28/2017
ms.author: sethm
ms.openlocfilehash: 27c7528552c497a4d98873a22bd56a991c66247c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-event-hubs-capture-using-hello-azure-portal"></a><span data-ttu-id="54a9c-103">Povolit pomocí portálu Azure hello Capture centra událostí</span><span class="sxs-lookup"><span data-stu-id="54a9c-103">Enable Event Hubs Capture using hello Azure portal</span></span>

<span data-ttu-id="54a9c-104">Zaznamenat lze nakonfigurovat v hello událostí okamžiku vytvoření centra pomocí hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="54a9c-104">You can configure Capture at hello event hub creation time using hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="54a9c-105">Můžete buď zachycení hello data tooan Azure [úložiště objektů Blob](https://azure.microsoft.com/services/storage/blobs/) kontejner nebo tooan [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) účtu.</span><span class="sxs-lookup"><span data-stu-id="54a9c-105">You can either capture hello data tooan Azure [Blob storage](https://azure.microsoft.com/services/storage/blobs/) container, or tooan [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) account.</span></span>

## <a name="capture-data-tooan-azure-storage-account"></a><span data-ttu-id="54a9c-106">Zaznamenat účet Azure Storage tooan dat</span><span class="sxs-lookup"><span data-stu-id="54a9c-106">Capture data tooan Azure Storage account</span></span>  

<span data-ttu-id="54a9c-107">Při vytváření centra událostí můžete povolit zachycení kliknutím hello **na** tlačítka na hello **vytvoření centra událostí** okno portálu.</span><span class="sxs-lookup"><span data-stu-id="54a9c-107">When you create an event hub, you can enable Capture by clicking hello **On** button in hello **Create Event Hub** portal blade.</span></span> <span data-ttu-id="54a9c-108">Potom zadejte účet úložiště a kontejneru kliknutím **Azure Storage** v hello **zaznamenání zprostředkovatele** pole.</span><span class="sxs-lookup"><span data-stu-id="54a9c-108">You then specify a Storage Account and container by clicking **Azure Storage** in hello **Capture Provider** box.</span></span> <span data-ttu-id="54a9c-109">Vzhledem k zachycení událostí centra využívá service-to-service ověřování s úložištěm, není nutné toospecify připojovací řetězec úložiště.</span><span class="sxs-lookup"><span data-stu-id="54a9c-109">Because Event Hubs Capture uses service-to-service authentication with storage, you do not need toospecify a storage connection string.</span></span> <span data-ttu-id="54a9c-110">Výběr prostředku Hello automaticky vybere hello URI pro váš účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="54a9c-110">hello resource picker selects hello resource URI for your storage account automatically.</span></span> <span data-ttu-id="54a9c-111">Pokud používáte Azure Resource Manager, musíte tento identifikátor URI explicitně zadat jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="54a9c-111">If you use Azure Resource Manager, you must supply this URI explicitly as a string.</span></span>

<span data-ttu-id="54a9c-112">Hello výchozí časový interval je 5 minut.</span><span class="sxs-lookup"><span data-stu-id="54a9c-112">hello default time window is 5 minutes.</span></span> <span data-ttu-id="54a9c-113">Hello minimální hodnota je 1, hello maximální 15.</span><span class="sxs-lookup"><span data-stu-id="54a9c-113">hello minimum value is 1, hello maximum 15.</span></span> <span data-ttu-id="54a9c-114">Hello **velikost** okno má rozsah 10 500 MB.</span><span class="sxs-lookup"><span data-stu-id="54a9c-114">hello **Size** window has a range of 10-500 MB.</span></span>

![][1]

## <a name="capture-data-tooan-azure-data-lake-store-account"></a><span data-ttu-id="54a9c-115">Zaznamenat účet Azure Data Lake Store tooan dat</span><span class="sxs-lookup"><span data-stu-id="54a9c-115">Capture data tooan Azure Data Lake Store account</span></span>

<span data-ttu-id="54a9c-116">toocapture data tooAzure Data Lake Store, vytvoření účtu Data Lake Store a centra událostí:</span><span class="sxs-lookup"><span data-stu-id="54a9c-116">toocapture data tooAzure Data Lake Store, you create a Data Lake Store account, and an event hub:</span></span>

### <a name="create-an-azure-data-lake-store-account-and-folders"></a><span data-ttu-id="54a9c-117">Vytvoření účtu Azure Data Lake Store a složek</span><span class="sxs-lookup"><span data-stu-id="54a9c-117">Create an Azure Data Lake Store account and folders</span></span>

1. <span data-ttu-id="54a9c-118">Vytvoření účtu Data Lake Store, pokynů hello v [Začínáme s Azure Data Lake Store pomocí portálu Azure hello](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="54a9c-118">Create a Data Lake Store account, following hello instructions in [Get started with Azure Data Lake Store using hello Azure portal](../data-lake-store/data-lake-store-get-started-portal.md).</span></span> 
2. <span data-ttu-id="54a9c-119">Vytvořte složku pod tímto účtem, pokynů hello v hello [vytváření složek v účtu Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md#createfolder) části.</span><span class="sxs-lookup"><span data-stu-id="54a9c-119">Create a folder under this account, following hello instructions in hello [Create folders in Azure Data Lake Store account](../data-lake-store/data-lake-store-get-started-portal.md#createfolder) section.</span></span>
3. <span data-ttu-id="54a9c-120">V okně účtu Data Lake Store hello, klikněte na tlačítko **Průzkumníku dat**.</span><span class="sxs-lookup"><span data-stu-id="54a9c-120">In hello Data Lake Store account blade, click **Data Explorer**.</span></span>
4. <span data-ttu-id="54a9c-121">Klikněte na **Přístup**.</span><span class="sxs-lookup"><span data-stu-id="54a9c-121">Click **Access**.</span></span>
5. <span data-ttu-id="54a9c-122">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="54a9c-122">Click **Add**.</span></span>
6. <span data-ttu-id="54a9c-123">V hello **vyhledávání podle názvu nebo e-mailu** zadejte **Microsoft.EventHubs** a pak vyberte tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="54a9c-123">In hello **Search by name or email** box type **Microsoft.EventHubs** and then select this option.</span></span> 
7. <span data-ttu-id="54a9c-124">Hello **oprávnění** se zobrazí karta.</span><span class="sxs-lookup"><span data-stu-id="54a9c-124">hello **Permissions** tab appears.</span></span> <span data-ttu-id="54a9c-125">Nastavte oprávnění hello, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="54a9c-125">Set hello permissions as shown in hello following figure:</span></span>

    ![][6]

8. <span data-ttu-id="54a9c-126">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="54a9c-126">Click **OK**.</span></span>
9. <span data-ttu-id="54a9c-127">Nyní vytvořte složku v kořenové složce hello toohello cílové složce procházení a kliknutím na název složky hello.</span><span class="sxs-lookup"><span data-stu-id="54a9c-127">Now, create a folder in hello root folder by browsing toohello target folder and clicking on hello folder name.</span></span>
10. <span data-ttu-id="54a9c-128">Klikněte na **Přístup**.</span><span class="sxs-lookup"><span data-stu-id="54a9c-128">Click **Access**.</span></span>
11. <span data-ttu-id="54a9c-129">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="54a9c-129">Click **Add**.</span></span>
12. <span data-ttu-id="54a9c-130">V hello **vyhledávání podle názvu nebo e-mailu** zadejte **Microsoft.EventHubs** a pak vyberte tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="54a9c-130">In hello **Search by name or email** box type **Microsoft.EventHubs** and then select this option.</span></span>
13. <span data-ttu-id="54a9c-131">Hello **oprávnění** karta se znovu zobrazí.</span><span class="sxs-lookup"><span data-stu-id="54a9c-131">hello **Permissions** tab appears again.</span></span> <span data-ttu-id="54a9c-132">Nastavte oprávnění hello, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="54a9c-132">Set hello permissions as shown in hello following figure:</span></span>

    ![][5]

### <a name="create-an-event-hub"></a><span data-ttu-id="54a9c-133">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="54a9c-133">Create an event hub</span></span>

1. <span data-ttu-id="54a9c-134">Všimněte si, že Centrum událostí hello musí být v hello stejné předplatné jako hello Azure Data Lake Store, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="54a9c-134">Note that hello event hub must be in hello same Azure subscription as hello Azure Data Lake Store you just created.</span></span> <span data-ttu-id="54a9c-135">Centra událostí vytvořit hello kliknutím hello **na** tlačítko pod **zaznamenat** v hello **vytvoření centra událostí** okno portálu.</span><span class="sxs-lookup"><span data-stu-id="54a9c-135">Create hello event hub, clicking hello **On** button under **Capture** in hello **Create Event Hub** portal blade.</span></span> 
2. <span data-ttu-id="54a9c-136">V hello **vytvoření centra událostí** okno portálu, vyberte **Azure Data Lake Store** z hello **zaznamenání zprostředkovatele** pole.</span><span class="sxs-lookup"><span data-stu-id="54a9c-136">In hello **Create Event Hub** portal blade, select **Azure Data Lake Store** from hello **Capture Provider** box.</span></span>
3. <span data-ttu-id="54a9c-137">V **vyberte Data Lake Store**, zadejte hello účtu Data Lake Store, které jste vytvořili dříve a v hello **Data Lake cesta** zadejte hello cesta toohello data složku jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="54a9c-137">In **Select Data Lake Store**, specify hello Data Lake Store account you created previously, and in hello **Data Lake Path** field, enter hello path toohello data folder you created.</span></span>

    ![][3]

## <a name="add-or-configure-capture-on-an-existing-event-hub"></a><span data-ttu-id="54a9c-138">Přidání nebo konfigurace funkce Capture v existujícím centru událostí</span><span class="sxs-lookup"><span data-stu-id="54a9c-138">Add or configure Capture on an existing event hub</span></span>

<span data-ttu-id="54a9c-139">Funkci Capture můžete nakonfigurovat v existujících centrech událostí, která jsou v oborech názvů služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="54a9c-139">You can configure Capture on existing event hubs that are in Event Hubs namespaces.</span></span> <span data-ttu-id="54a9c-140">tooenable zaznamenat na existující centra událostí nebo toochange zachycení nastavení, klikněte na tlačítko hello obor názvů tooload hello **Essentials** okno, pak klikněte na tlačítko hello centra událostí, pro kterou chcete tooenable nebo změnit nastavení zachycení hello.</span><span class="sxs-lookup"><span data-stu-id="54a9c-140">tooenable Capture on an existing event hub, or toochange your Capture settings, click hello namespace tooload hello **Essentials** blade, then click hello event hub for which you want tooenable or change hello Capture setting.</span></span> <span data-ttu-id="54a9c-141">Nakonec klikněte na hello **vlastnosti** části hello otevřete okno a potom upravte nastavení zachycení hello, jak je znázorněno v hello následující údaje:</span><span class="sxs-lookup"><span data-stu-id="54a9c-141">Finally, click hello **Properties** section of hello open blade and then edit hello Capture settings, as shown in hello following figures:</span></span>

### <a name="azure-blob-storage"></a><span data-ttu-id="54a9c-142">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="54a9c-142">Azure Blob Storage</span></span>

![][2]

### <a name="azure-data-lake-store"></a><span data-ttu-id="54a9c-143">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="54a9c-143">Azure Data Lake Store</span></span>

![][4]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png
[3]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture3.png
[4]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture4.png
[5]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture5.png
[6]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture6.png

## <a name="next-steps"></a><span data-ttu-id="54a9c-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="54a9c-144">Next steps</span></span>

<span data-ttu-id="54a9c-145">Ke konfiguraci funkce Event Hubs Capture můžete také použít šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="54a9c-145">You can also configure Event Hubs Capture using Azure Resource Manager templates.</span></span> <span data-ttu-id="54a9c-146">Další informace najdete v tématu věnovaném [povolení funkce Capture pomocí šablony Azure Resource Manageru](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).</span><span class="sxs-lookup"><span data-stu-id="54a9c-146">For more information, see [Enable Capture using an Azure Resource Manager template](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).</span></span>
