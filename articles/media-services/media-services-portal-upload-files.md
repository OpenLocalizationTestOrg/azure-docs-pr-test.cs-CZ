---
title: "AAA\"nahrávání souborů do účtu Media Services pomocí hello portálu Azure | Microsoft Docs\""
description: "Tento kurz vás provede kroky hello odesílání souborů do účtu Media Services pomocí hello portálu Azure"
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
ms.openlocfilehash: 4ce1e133c72854532735ba7c72a43c92a75bc240
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-a-media-services-account-using-hello-azure-portal"></a><span data-ttu-id="fbe06-103">Nahrání souborů do účtu Media Services pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fbe06-103">Upload files into a Media Services account using hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fbe06-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fbe06-104">Portal</span></span>](media-services-portal-upload-files.md)
> * [<span data-ttu-id="fbe06-105">.NET</span><span class="sxs-lookup"><span data-stu-id="fbe06-105">.NET</span></span>](media-services-dotnet-upload-files.md)
> * [<span data-ttu-id="fbe06-106">REST</span><span class="sxs-lookup"><span data-stu-id="fbe06-106">REST</span></span>](media-services-rest-upload-files.md)
> 
> [!NOTE]
> <span data-ttu-id="fbe06-107">toocomplete tohoto kurzu potřebujete účet Azure.</span><span class="sxs-lookup"><span data-stu-id="fbe06-107">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="fbe06-108">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fbe06-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 


<span data-ttu-id="fbe06-109">Ve službě Media Services můžete digitální soubory nahrát do assetu.</span><span class="sxs-lookup"><span data-stu-id="fbe06-109">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="fbe06-110">Hello prostředku může obsahovat video, zvuk, obrázky, kolekci miniatur, text sleduje a titulků soubory (a hello metadata o těchto souborech.) Jakmile hello soubory jsou odeslány, váš obsah bezpečně uložen v hello cloudu pro další zpracování a streamování.</span><span class="sxs-lookup"><span data-stu-id="fbe06-110">hello Asset  can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and hello metadata about these files.) Once hello files are uploaded, your content is stored securely in hello cloud for further processing and streaming.</span></span>


## <a name="upload-files"></a><span data-ttu-id="fbe06-111">Nahrání souborů</span><span class="sxs-lookup"><span data-stu-id="fbe06-111">Upload files</span></span>

>[!NOTE]
><span data-ttu-id="fbe06-112">Existuje limit toohello maximální velikost souboru pro zpracování ve službě Media Services podporována.</span><span class="sxs-lookup"><span data-stu-id="fbe06-112">There is a limit toohello maximum file size supported for processing in Media Services.</span></span> <span data-ttu-id="fbe06-113">Najdete v tématu [to](media-services-quotas-and-limitations.md) téma podrobné informace o omezení velikosti souborů hello.</span><span class="sxs-lookup"><span data-stu-id="fbe06-113">Please see [this](media-services-quotas-and-limitations.md) topic for details about hello file size limitation.</span></span>
>

1. <span data-ttu-id="fbe06-114">V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="fbe06-114">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="fbe06-115">Na hello **nastavení** okně klikněte na tlačítko **prostředky**.</span><span class="sxs-lookup"><span data-stu-id="fbe06-115">On hello **Settings** blade, click **Assets**.</span></span>
   
    ![Nahrání souborů](./media/media-services-portal-vod-get-started/media-services-upload.png)
3. <span data-ttu-id="fbe06-117">Klikněte na tlačítko hello **nahrát** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fbe06-117">Click hello **Upload** button.</span></span>
   
    <span data-ttu-id="fbe06-118">Hello **nahrát asset videa** se zobrazí v okně.</span><span class="sxs-lookup"><span data-stu-id="fbe06-118">hello **Upload a video asset** window appears.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="fbe06-119">Velikost souboru není nijak omezená.</span><span class="sxs-lookup"><span data-stu-id="fbe06-119">There is no file size limitation.</span></span>
   > 
   > 
4. <span data-ttu-id="fbe06-120">Procházet toohello požadovaného video ve vašem počítači, vyberte ho a klikněte na OK.</span><span class="sxs-lookup"><span data-stu-id="fbe06-120">Browse toohello desired video on your computer, select it, and hit OK.</span></span>  
   
    <span data-ttu-id="fbe06-121">Spustí nahrávání Hello a zobrazí se průběh hello pod názvem souboru hello.</span><span class="sxs-lookup"><span data-stu-id="fbe06-121">hello upload starts and you can see hello progress under hello file name.</span></span>  

<span data-ttu-id="fbe06-122">Po dokončení nahrávání hello uvidíte hello nový prostředek zobrazí v hello **prostředky** okno.</span><span class="sxs-lookup"><span data-stu-id="fbe06-122">Once hello upload completes, you will see hello new asset listed in hello **Assets** window.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="fbe06-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fbe06-123">Next steps</span></span>
<span data-ttu-id="fbe06-124">Nyní můžete kódovat nahrané assety.</span><span class="sxs-lookup"><span data-stu-id="fbe06-124">You can now encode your uploaded assets.</span></span> <span data-ttu-id="fbe06-125">Další informace najdete v tématu [Kódování assetů](media-services-portal-encode.md).</span><span class="sxs-lookup"><span data-stu-id="fbe06-125">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>

<span data-ttu-id="fbe06-126">Můžete také použít Azure Functions tootrigger úlohu kódování na základě souboru přicházejících do kontejneru hello nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="fbe06-126">You can also use Azure Functions tootrigger an encoding job based on a file arriving in hello configured container.</span></span> <span data-ttu-id="fbe06-127">Další informace najdete v [této ukázce](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span><span class="sxs-lookup"><span data-stu-id="fbe06-127">For more information, see [this sample](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="fbe06-128">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="fbe06-128">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="fbe06-129">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="fbe06-129">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

