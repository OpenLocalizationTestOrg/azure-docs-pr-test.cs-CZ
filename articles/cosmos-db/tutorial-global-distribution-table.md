---
title: "kurz globální distribuční aaaAzure Cosmos DB pro rozhraní API tabulky | Microsoft Docs"
description: "Zjistěte, jak pomocí globální distribuční databázi Cosmos Azure toosetup hello tabulky rozhraní API."
services: cosmos-db
keywords: "globální distribuční tabulky"
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: d2a995e09c37f9449856aef2ab707e95eb8a540c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-table-api"></a>Jak toosetup Azure Cosmos DB globální distribuční pomocí rozhraní API tabulky hello

V tomto článku ukážeme, jak toouse hello globální distribuční databázi Cosmos Azure Azure portálu toosetup a potom se připojte pomocí hello tabulky rozhraní API (preview).

Tento článek se zabývá hello následující úlohy: 

> [!div class="checklist"]
> * Nakonfigurujte globální distribuci pomocí hello portálu Azure
> * Nakonfigurujte globální distribuci pomocí hello [tabulky rozhraní API](table-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-tooa-preferred-region-using-hello-table-api"></a>Připojení tooa upřednostňovaná oblast pomocí hello tabulky rozhraní API

V pořadí tootake výhody [globální distribuční](distribute-data-globally.md), klientské aplikace můžete zadat hello seřazené předvoleb seznam oblastí toobe použít tooperform dokumentu operace. To můžete provést nastavení hello `TablePreferredLocations` hodnota konfigurace v konfiguraci aplikace hello pro hello preview SDK úložiště Azure. V závislosti na konfiguraci účtu Azure Cosmos DB hello aktuální místní dostupnosti a hello předvoleb seznamu zadán, hello většina optimální koncového bodu, bude použita volba hello sada SDK úložiště Azure tooperform zápis a operace čtení.

Hello `TablePreferredLocations` by měla obsahovat čárkami oddělený seznam upřednostňovaných (více funkci) umístění pro čtení. Každá instance klienta můžete určit podmnožinu těchto oblastí v hello upřednostňované pořadí pro čtení s nízkou latencí. Hello oblasti musí mít název pomocí jejich [zobrazované názvy](https://msdn.microsoft.com/library/azure/gg441293.aspx), například `West US`.

Všechny operace čtení budou odeslány toohello první dostupné oblasti hello `TablePreferredLocations` seznamu. Pokud hello požadavek selže, hello klienta se nezdaří dolů hello seznamu toohello další oblasti a tak dále.

Hello SDK se pouze pokusí tooread ze zadané v oblasti hello `TablePreferredLocations`. Ano, například pokud hello databázového účtu je k dispozici v tři oblasti, ale hello klienta určuje pouze dva z hello oblasti bez zápisu pro `TablePreferredLocations`, pak žádný čtení se zpracuje mimo oblast hello zápisu, i v případě hello převzetí služeb při selhání.

Hello SDK automaticky odesílat všechny zápisy toohello aktuální zápisu oblasti.

Pokud hello `TablePreferredLocations` není nastavena vlastnost, z aktuální oblasti zápisu hello se zpracuje všechny požadavky.

```xml
    <appSettings>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>           
    </appSettings>
```

Je to, že dokončení tohoto kurzu. Další informace jak toomanage hello konzistence účtu globálně replikované načtením [úrovně konzistence v Azure Cosmos DB](consistency-levels.md). A další informace o tom, jak globální replikace databáze v Azure Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Další kroky

V tomto kurzu provedete krok hello následující:

> [!div class="checklist"]
> * Nakonfigurujte globální distribuci pomocí hello portálu Azure
> * Nakonfigurujte globální distribuci pomocí hello DocumentDB rozhraní API

Nyní můžete přejít toohello další kurz toolearn jak hello toodevelop místně pomocí emulátoru místního Azure Cosmos DB.

> [!div class="nextstepaction"]
> [Vývoj místně pomocí emulátoru hello](local-emulator.md)
