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
# <a name="upload-files-into-a-media-services-account-using-hello-azure-portal"></a>Nahrání souborů do účtu Media Services pomocí hello portálu Azure
> [!div class="op_single_selector"]
> * [Azure Portal](media-services-portal-upload-files.md)
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> 
> [!NOTE]
> toocomplete tohoto kurzu potřebujete účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 


Ve službě Media Services můžete digitální soubory nahrát do assetu. Hello prostředku může obsahovat video, zvuk, obrázky, kolekci miniatur, text sleduje a titulků soubory (a hello metadata o těchto souborech.) Jakmile hello soubory jsou odeslány, váš obsah bezpečně uložen v hello cloudu pro další zpracování a streamování.


## <a name="upload-files"></a>Nahrání souborů

>[!NOTE]
>Existuje limit toohello maximální velikost souboru pro zpracování ve službě Media Services podporována. Najdete v tématu [to](media-services-quotas-and-limitations.md) téma podrobné informace o omezení velikosti souborů hello.
>

1. V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Azure Media Services.
2. Na hello **nastavení** okně klikněte na tlačítko **prostředky**.
   
    ![Nahrání souborů](./media/media-services-portal-vod-get-started/media-services-upload.png)
3. Klikněte na tlačítko hello **nahrát** tlačítko.
   
    Hello **nahrát asset videa** se zobrazí v okně.
   
   > [!NOTE]
   > Velikost souboru není nijak omezená.
   > 
   > 
4. Procházet toohello požadovaného video ve vašem počítači, vyberte ho a klikněte na OK.  
   
    Spustí nahrávání Hello a zobrazí se průběh hello pod názvem souboru hello.  

Po dokončení nahrávání hello uvidíte hello nový prostředek zobrazí v hello **prostředky** okno. 

## <a name="next-steps"></a>Další kroky
Nyní můžete kódovat nahrané assety. Další informace najdete v tématu [Kódování assetů](media-services-portal-encode.md).

Můžete také použít Azure Functions tootrigger úlohu kódování na základě souboru přicházejících do kontejneru hello nakonfigurované. Další informace najdete v [této ukázce](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

