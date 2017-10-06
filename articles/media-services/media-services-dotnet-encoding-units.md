---
title: "média aaaScale zpracování přidáním kódování jednotky – Azure |  Microsoft Docs"
description: "Zjistěte, jak toohow tooadd kódování jednotky s rozhraním .NET"
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 33f7625a-966a-4f06-bc09-bccd6e2a42b5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;milangada;
ms.openlocfilehash: b9f71a6487c5d136319a38a1598d60edfaa81b9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooscale-encoding-with-net-sdk"></a>Jak tooscale kódování pomocí .NET SDK
> [!div class="op_single_selector"]
> * [Azure Portal](media-services-portal-scale-media-processing.md)
> * [.NET](media-services-dotnet-encoding-units.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>Přehled
> [!IMPORTANT]
> Ujistěte se, zda text hello tooreview [přehled](media-services-scale-media-processing-overview.md) tématu tooget Další informace o škálování média zpracování tématu.
> 
> 

toochange hello vyhrazené jednotky typu a hello číslo jednotky rezervované pro kódování pomocí sady .NET SDK hello následující:

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds tooS1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);

    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();

    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

## <a name="opening-a-support-ticket"></a>Otevření lístku podpory
Ve výchozím nastavení každých účtu Media Services můžete škálovat tooup too25 kódování a 5 na vyžádání jednotek rezervovaných pro streamování. Vyšší limit můžete požádat tak, že otevřete lístek podpory.

### <a name="open-a-support-ticket"></a>Otevřete lístek podpory
tooopen lístek podpory hello následující:

1. Klikněte na tlačítko [získat podporu](https://manage.windowsazure.com/?getsupport=true). Pokud nejste přihlášeni, budete mít výzvami tooenter přihlašovacích údajů.
2. Vyberte své předplatné.
3. V rámci podpory typu vyberte "Technical".
4. Kliknutím na tlačítko "Vytvoření lístku".
5. Vyberte "Azure Media Services" v seznamu produktů hello uvedené na další stránku hello.
6. Vyberte typ problému"", je vhodné pro váš problém.
7. Klikněte na tlačítko Pokračovat.
8. Postupujte podle pokynů na další stránce a potom zadejte podrobnosti o problému.
9. Klikněte na tlačítko pro odeslání tooopen hello lístku.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

