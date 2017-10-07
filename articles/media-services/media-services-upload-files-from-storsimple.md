---
title: "aaaUpload souborů do účtu Azure Media Services z Azure StorSimple | Microsoft Docs"
description: "Tento článek poskytuje stručný přehled Azure StorSimple Data Manageru. článek Hello taky obsahuje odkazy tootutorials, která ukazují, jak tooextract data ze zařízení StorSimple a nahrajte ho jako prostředky tooan účtu Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 1dd09328-262b-43ef-8099-73241b49a925
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/27/2017
ms.author: juliako
ms.openlocfilehash: 7e9712aa480106bbd5fcc63eaecf0418b24a8bef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-an-azure-media-services-account-from-azure-storsimple"></a><span data-ttu-id="87b97-104">Odesílání souborů do účtu Azure Media Services z Azure StorSimple | Dokumentace Microsoftu</span><span class="sxs-lookup"><span data-stu-id="87b97-104">Upload files into an Azure Media Services account from Azure StorSimple</span></span>

<span data-ttu-id="87b97-105">Tento článek poskytuje stručný přehled Azure StorSimple Data Manageru.</span><span class="sxs-lookup"><span data-stu-id="87b97-105">This article gives a brief overview of Azure StorSimple Data Manager.</span></span> <span data-ttu-id="87b97-106">článek Hello taky obsahuje odkazy tootutorials, která ukazují, jak tooextract data ze zařízení StorSimple a nahrát tato data jako prostředky tooan účtu Azure Media Services (AMS).</span><span class="sxs-lookup"><span data-stu-id="87b97-106">hello article also links tootutorials that show you how tooextract data from StorSimple and upload this data as assets tooan Azure Media Services (AMS) account.</span></span>

> 
> [!NOTE]
> <span data-ttu-id="87b97-107">Azure StorSimple Data Manager je nyní ve verzi Private Preview.</span><span class="sxs-lookup"><span data-stu-id="87b97-107">Azure StorSimple Data Manager is currently in private preview.</span></span> 
> 

## <a name="overview"></a><span data-ttu-id="87b97-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="87b97-108">Overview</span></span>

<span data-ttu-id="87b97-109">Ve službě Media Services můžete digitální soubory nahrát do assetu.</span><span class="sxs-lookup"><span data-stu-id="87b97-109">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="87b97-110">Hello prostředku může obsahovat video, zvuk, obrázky, kolekci miniatur, text sleduje a titulků soubory (a hello metadata o těchto souborech.) Jakmile hello soubory jsou odeslány, váš obsah bezpečně uložen v hello cloudu pro další zpracování a streamování.</span><span class="sxs-lookup"><span data-stu-id="87b97-110">hello Asset  can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and hello metadata about these files.) Once hello files are uploaded, your content is stored securely in hello cloud for further processing and streaming.</span></span>

<span data-ttu-id="87b97-111">[Azure StorSimple](https://docs.microsoft.com/azure/storsimple/) používá cloudového úložiště, jako rozšíření hello místní řešení a automaticky úrovně dat napříč hello místní úložiště a úložiště v cloudu.</span><span class="sxs-lookup"><span data-stu-id="87b97-111">[Azure StorSimple](https://docs.microsoft.com/azure/storsimple/) uses cloud storage as an extension of hello on-premises solution and automatically tiers data across hello on-premises storage and cloud storage.</span></span> <span data-ttu-id="87b97-112">zařízení StorSimple Hello dedupes a zkomprimuje data před odesláním toohello cloudu, což velmi efektivní odesílání cloudu toohello velkých souborů.</span><span class="sxs-lookup"><span data-stu-id="87b97-112">hello StorSimple device dedupes and compresses your data before sending it toohello cloud making it very efficient for sending large files toohello cloud.</span></span> <span data-ttu-id="87b97-113">Hello [StorSimple Manager dat](../storsimple/storsimple-data-manager-overview.md) služba poskytuje rozhraní API, která umožňují vám tooextract dat ze zařízení StorSimple a je k dispozici jako prostředky AMS.</span><span class="sxs-lookup"><span data-stu-id="87b97-113">hello [StorSimple Data Manager](../storsimple/storsimple-data-manager-overview.md) service provides APIs that enable you tooextract data from StorSimple and present it as AMS assets.</span></span>

## <a name="get-started"></a><span data-ttu-id="87b97-114">Začínáme</span><span class="sxs-lookup"><span data-stu-id="87b97-114">Get started</span></span>

1. <span data-ttu-id="87b97-115">[Vytvoření účtu Media Services](media-services-portal-create-account.md) do kterého chcete tootransfer hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="87b97-115">[Create a Media Services account](media-services-portal-create-account.md) into which you want tootransfer hello assets.</span></span>
2. <span data-ttu-id="87b97-116">Zaregistrujte si verzi preview Data Manager, jak je popsáno v hello [StorSimple Manager dat](../storsimple/storsimple-data-manager-overview.md) článku.</span><span class="sxs-lookup"><span data-stu-id="87b97-116">Sign up for Data Manager preview, as described in hello [StorSimple Data Manager](../storsimple/storsimple-data-manager-overview.md) article.</span></span>
3. <span data-ttu-id="87b97-117">Vytvořte účet StorSimple Data Manageru.</span><span class="sxs-lookup"><span data-stu-id="87b97-117">Create a StorSimple Data Manager account.</span></span>
4. <span data-ttu-id="87b97-118">Vytvořte úlohu transformace dat, která po spuštění extrahuje data ze zařízení StorSimple a přenese je do účtu AMS jako prostředky.</span><span class="sxs-lookup"><span data-stu-id="87b97-118">Create a data transformation job that when runs, extracts data from a StorSimple device and transfers it into an AMS account as assets.</span></span> 

    <span data-ttu-id="87b97-119">Při hello úlohy spuštění, se vytvoří frontu úložiště.</span><span class="sxs-lookup"><span data-stu-id="87b97-119">When hello job starts running, a storage queue is created.</span></span> <span data-ttu-id="87b97-120">Tato fronta je naplňována zprávami o transformovaných objektech blob, jakmile jsou připravené.</span><span class="sxs-lookup"><span data-stu-id="87b97-120">This queue is populated with messages about transformed blobs as they are ready.</span></span> <span data-ttu-id="87b97-121">Hello název této fronty je hello stejný jako název hello hello definice úlohy.</span><span class="sxs-lookup"><span data-stu-id="87b97-121">hello name of this queue is hello same as hello name of hello job definition.</span></span> <span data-ttu-id="87b97-122">Můžete použít tuto frontu toodetermine při jako prostředek je připravené a zavolat vaší požadovanou operaci toorun Media Services.</span><span class="sxs-lookup"><span data-stu-id="87b97-122">You can use this queue toodetermine when as asset is ready and call your desired Media Services operation toorun on it.</span></span> <span data-ttu-id="87b97-123">Například můžete použít tuto frontu tootrigger funkce Azure, který se nachází hello nezbytného kódu Media Services.</span><span class="sxs-lookup"><span data-stu-id="87b97-123">For example, you can use this queue tootrigger an Azure Function that has hello necessary Media Services code in it.</span></span>

## <a name="see-also"></a><span data-ttu-id="87b97-124">Viz také</span><span class="sxs-lookup"><span data-stu-id="87b97-124">See also</span></span>

[<span data-ttu-id="87b97-125">Použití hello .net SDK tootrigger úloh v hello Data Manager</span><span class="sxs-lookup"><span data-stu-id="87b97-125">Use hello .Net SDK tootrigger jobs in hello Data Manager</span></span>](../storsimple/storsimple-data-manager-dotnet-jobs.md)

## <a name="media-services-learning-paths"></a><span data-ttu-id="87b97-126">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="87b97-126">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="87b97-127">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="87b97-127">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="87b97-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87b97-128">Next steps</span></span>

<span data-ttu-id="87b97-129">Nyní můžete kódovat nahrané assety.</span><span class="sxs-lookup"><span data-stu-id="87b97-129">You can now encode your uploaded assets.</span></span> <span data-ttu-id="87b97-130">Další informace najdete v tématu [Kódování assetů](media-services-portal-encode.md).</span><span class="sxs-lookup"><span data-stu-id="87b97-130">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>
