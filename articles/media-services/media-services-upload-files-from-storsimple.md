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
# <a name="upload-files-into-an-azure-media-services-account-from-azure-storsimple"></a>Odesílání souborů do účtu Azure Media Services z Azure StorSimple | Dokumentace Microsoftu

Tento článek poskytuje stručný přehled Azure StorSimple Data Manageru. článek Hello taky obsahuje odkazy tootutorials, která ukazují, jak tooextract data ze zařízení StorSimple a nahrát tato data jako prostředky tooan účtu Azure Media Services (AMS).

> 
> [!NOTE]
> Azure StorSimple Data Manager je nyní ve verzi Private Preview. 
> 

## <a name="overview"></a>Přehled

Ve službě Media Services můžete digitální soubory nahrát do assetu. Hello prostředku může obsahovat video, zvuk, obrázky, kolekci miniatur, text sleduje a titulků soubory (a hello metadata o těchto souborech.) Jakmile hello soubory jsou odeslány, váš obsah bezpečně uložen v hello cloudu pro další zpracování a streamování.

[Azure StorSimple](https://docs.microsoft.com/azure/storsimple/) používá cloudového úložiště, jako rozšíření hello místní řešení a automaticky úrovně dat napříč hello místní úložiště a úložiště v cloudu. zařízení StorSimple Hello dedupes a zkomprimuje data před odesláním toohello cloudu, což velmi efektivní odesílání cloudu toohello velkých souborů. Hello [StorSimple Manager dat](../storsimple/storsimple-data-manager-overview.md) služba poskytuje rozhraní API, která umožňují vám tooextract dat ze zařízení StorSimple a je k dispozici jako prostředky AMS.

## <a name="get-started"></a>Začínáme

1. [Vytvoření účtu Media Services](media-services-portal-create-account.md) do kterého chcete tootransfer hello prostředky.
2. Zaregistrujte si verzi preview Data Manager, jak je popsáno v hello [StorSimple Manager dat](../storsimple/storsimple-data-manager-overview.md) článku.
3. Vytvořte účet StorSimple Data Manageru.
4. Vytvořte úlohu transformace dat, která po spuštění extrahuje data ze zařízení StorSimple a přenese je do účtu AMS jako prostředky. 

    Při hello úlohy spuštění, se vytvoří frontu úložiště. Tato fronta je naplňována zprávami o transformovaných objektech blob, jakmile jsou připravené. Hello název této fronty je hello stejný jako název hello hello definice úlohy. Můžete použít tuto frontu toodetermine při jako prostředek je připravené a zavolat vaší požadovanou operaci toorun Media Services. Například můžete použít tuto frontu tootrigger funkce Azure, který se nachází hello nezbytného kódu Media Services.

## <a name="see-also"></a>Viz také

[Použití hello .net SDK tootrigger úloh v hello Data Manager](../storsimple/storsimple-data-manager-dotnet-jobs.md)

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Další kroky

Nyní můžete kódovat nahrané assety. Další informace najdete v tématu [Kódování assetů](media-services-portal-encode.md).
