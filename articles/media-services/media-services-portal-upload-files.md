---
title: " Odeslání souborů do účtu Azure Media Services pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "Tento kurz vás provede kroky pro odeslání souborů do účtu služby Azure Media Services pomocí webu Azure Portal."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 3ad3dcea-95be-4711-9aae-a455a32434f6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 3a1dd7470f940da839687478b636464d930d8ab7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="upload-files-into-a-media-services-account-using-the-azure-portal"></a><span data-ttu-id="7a151-103">Odeslání souborů do účtu Azure Media Services pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7a151-103">Upload files into a Media Services account using the Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7a151-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7a151-104">Portal</span></span>](media-services-portal-upload-files.md)
> * [<span data-ttu-id="7a151-105">.NET</span><span class="sxs-lookup"><span data-stu-id="7a151-105">.NET</span></span>](media-services-dotnet-upload-files.md)
> * [<span data-ttu-id="7a151-106">REST</span><span class="sxs-lookup"><span data-stu-id="7a151-106">REST</span></span>](media-services-rest-upload-files.md)
> 
> [!NOTE]
> <span data-ttu-id="7a151-107">K dokončení tohoto kurzu potřebujete mít účet Azure.</span><span class="sxs-lookup"><span data-stu-id="7a151-107">To complete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="7a151-108">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7a151-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 


<span data-ttu-id="7a151-109">Ve službě Media Services můžete digitální soubory nahrát do assetu.</span><span class="sxs-lookup"><span data-stu-id="7a151-109">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="7a151-110">Asset může obsahovat video, zvuk, obrázky, kolekci miniatur, textové stopy a soubory titulků (a metadata o těchto souborech.) Jakmile soubory odešlete, bude váš obsah bezpečně uložen v cloudu pro další zpracování a streamování.</span><span class="sxs-lookup"><span data-stu-id="7a151-110">The Asset  can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and the metadata about these files.) Once the files are uploaded, your content is stored securely in the cloud for further processing and streaming.</span></span>


## <a name="upload-files"></a><span data-ttu-id="7a151-111">Nahrání souborů</span><span class="sxs-lookup"><span data-stu-id="7a151-111">Upload files</span></span>

>[!NOTE]
><span data-ttu-id="7a151-112">Maximální velikost souboru podporovaná při zpracování ve službě Media Services je omezená.</span><span class="sxs-lookup"><span data-stu-id="7a151-112">There is a limit to the maximum file size supported for processing in Media Services.</span></span> <span data-ttu-id="7a151-113">Podrobnosti o omezení velikosti souboru najdete [tady](media-services-quotas-and-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="7a151-113">Please see [this](media-services-quotas-and-limitations.md) topic for details about the file size limitation.</span></span>
>

1. <span data-ttu-id="7a151-114">Na webu [Azure Portal](https://portal.azure.com/) zvolte účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="7a151-114">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="7a151-115">V okně **Nastavení** klikněte na **Assety**.</span><span class="sxs-lookup"><span data-stu-id="7a151-115">On the **Settings** blade, click **Assets**.</span></span>
   
    ![Nahrání souborů](./media/media-services-portal-vod-get-started/media-services-upload.png)
3. <span data-ttu-id="7a151-117">Klikněte na tlačítko **Nahrát**.</span><span class="sxs-lookup"><span data-stu-id="7a151-117">Click the **Upload** button.</span></span>
   
    <span data-ttu-id="7a151-118">Zobrazí se okno **Nahrát asset videa**.</span><span class="sxs-lookup"><span data-stu-id="7a151-118">The **Upload a video asset** window appears.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="7a151-119">Velikost souboru není nijak omezená.</span><span class="sxs-lookup"><span data-stu-id="7a151-119">There is no file size limitation.</span></span>
   > 
   > 
4. <span data-ttu-id="7a151-120">Přejděte v počítači na požadovaného video, vyberte ho a klikněte na OK.</span><span class="sxs-lookup"><span data-stu-id="7a151-120">Browse to the desired video on your computer, select it, and hit OK.</span></span>  
   
    <span data-ttu-id="7a151-121">Spustí se nahrávání. Jeho průběh můžete sledovat pod názvem souboru.</span><span class="sxs-lookup"><span data-stu-id="7a151-121">The upload starts and you can see the progress under the file name.</span></span>  

<span data-ttu-id="7a151-122">Po dokončení nahrávání se nový prostředek zobrazí v okně **Assety**.</span><span class="sxs-lookup"><span data-stu-id="7a151-122">Once the upload completes, you will see the new asset listed in the **Assets** window.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="7a151-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7a151-123">Next steps</span></span>
<span data-ttu-id="7a151-124">Nyní můžete kódovat nahrané assety.</span><span class="sxs-lookup"><span data-stu-id="7a151-124">You can now encode your uploaded assets.</span></span> <span data-ttu-id="7a151-125">Další informace najdete v tématu [Kódování assetů](media-services-portal-encode.md).</span><span class="sxs-lookup"><span data-stu-id="7a151-125">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>

<span data-ttu-id="7a151-126">Můžete také použít službu Azure Functions k aktivaci úlohy kódování při příchodu souboru do nakonfigurovaného kontejneru.</span><span class="sxs-lookup"><span data-stu-id="7a151-126">You can also use Azure Functions to trigger an encoding job based on a file arriving in the configured container.</span></span> <span data-ttu-id="7a151-127">Další informace najdete v [této ukázce](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span><span class="sxs-lookup"><span data-stu-id="7a151-127">For more information, see [this sample](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="7a151-128">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="7a151-128">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7a151-129">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="7a151-129">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

