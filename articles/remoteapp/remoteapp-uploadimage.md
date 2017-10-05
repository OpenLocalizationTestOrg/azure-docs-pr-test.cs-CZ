---
title: "Nahrát vlastní image pro Azure Remoteappu | Microsoft Docs"
description: "Naučte se nahrát vlastní image pro Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: 299e0510-1a6b-4fdf-914a-3631b061a360
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: ericor
ms.openlocfilehash: 5a235fac88d6e95ea294bda197641108acb4a09f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="upload-a-custom-image-for-azure-remoteapp"></a>Nahrát vlastní image pro Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Teď, když jste vytvořili vlastního image šablony, nebo byly aktualizovány s změny, budete muset nahrát této bitové kopie do bitové kopie knihovny Azure Remoteappu. Pomocí těchto kroků.

## <a name="before-you-start"></a>Než začnete
1. Ověřit vlastní image splňuje [požadavky na image](remoteapp-imagereqs.md) a [požadavky na aplikace](remoteapp-appreqs.md).
2. Nainstalujte [modul Azure PowerShell](/powershell/azure/overview).

## <a name="step-by-step-on-how-to-upload-custom-image"></a>Podrobný návod, jak nahrát vlastní image
1. Otevřete portál pro správu Azure a přejděte na stránku vzdálené aplikace RemoteApp.
2. Na **Image šablony** , klikněte na **nahrát** v dolní části stránky.
3. Zadejte popisný název bitové kopie a umístění účtu úložiště. Zkontrolujte, zda umístění stejné umístění jako vaše kolekce vzdálené aplikace RemoteApp nebo umístění, kde chcete vytvořit.
4. Po zobrazení výzvy stáhněte skript na vašem místním počítači.
5. Parametry příkazu v textovém poli zkopírujte do schránky.
6. Otevřete okno prostředí Windows PowerShell se zvýšenými oprávněními.
7. Z okna zvýšenými oprávněními prostředí Windows PowerShell přejděte do stejného adresáře, kam jste stáhli skript.
8. Vložte zkopírovaný příkaz a stiskněte klávesu **Enter**.
   
   Zahájí proces odesílání a doba trvání může záviset na mnoha faktorech včetně rychlosti sítě a velikost bitové kopie
9. Pokud vaše nahrávání nezdaří z důvodu přerušení sítě nebo věcí jako je například, můžete vždy obnovit proces odesílání, které jste začali. Obnovit odeslání, spusťte skript znovu s použitím stejném příkazovém řádku.

> [!WARNING]
> Nikdy upravit nahrávání skript. Chcete-li zajistit, aby splňoval požadavky na image a požadavky na aplikace bitovou kopii je implementovaná specifických kontrolách.
> 
> 

## <a name="common-problems"></a>Běžné problémy
* Ujistěte se, že používáte prostředí Windows PowerShell, ne Azure PowerShell. Potřebujete nainstalovat modul Azure PowerShell, protože některé moduly jsou potřeba během procesu nahrávání.
* Nikdy alter skript, ověření existují pro usnadnění vaší práce.
* Pokud soubor vhd získá uzamčen během nahrávání, zkopírujte soubor nebo ho přesunout do nového umístění a pokus o odeslání znovu. Může být některé procesu systému Windows, který brání odeslání.  

