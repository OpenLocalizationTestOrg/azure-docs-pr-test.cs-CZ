---
title: "aaaUpload vlastní image pro Azure Remoteappu | Microsoft Docs"
description: "Zjistěte, jak tooupload vlastní obrázek pro Azure RemoteApp"
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
ms.openlocfilehash: 6ad40fe58795ece37f4c7900be01bc713938da87
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-custom-image-for-azure-remoteapp"></a>Nahrát vlastní image pro Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Teď, když jste vytvořili vlastního image šablony, nebo byly aktualizovány s změny, musíte tooupload bitové kopie knihovny Azure Remoteappu tooyour této bitové kopie. Pomocí těchto kroků.

## <a name="before-you-start"></a>Než začnete
1. Ověřit vlastní image splňuje hello [požadavky na image](remoteapp-imagereqs.md) a [požadavky na aplikace](remoteapp-appreqs.md).
2. Nainstalujte hello [modul Azure PowerShell](/powershell/azure/overview).

## <a name="step-by-step-on-how-tooupload-custom-image"></a>Podrobný návod, jak tooupload vlastní image
1. Otevřete portál pro správu Azure a přejděte toohello vzdálené aplikace RemoteApp stránky.
2. Na hello **Image šablony** , klikněte na **nahrát** v hello dolní části stránky hello.
3. Zadejte popisný název bitové kopie a umístění účtu úložiště hello. Zkontrolujte umístění hello hello stejné umístění jako vaše kolekce vzdálené aplikace RemoteApp nebo do umístění, kam chcete toocreate jeden.
4. Po zobrazení výzvy stáhnout hello skriptu tooyour místní počítač.
5. Parametry příkazu hello zkopírujte do schránky tooyour pole textu hello.
6. Otevřete okno prostředí Windows PowerShell se zvýšenými oprávněními.
7. Z hello se zvýšenými oprávněními okno prostředí Windows PowerShell, přejděte toohello stejný adresář, kam jste stáhli hello skriptu.
8. Vložení hello zkopírovat příkaz a stiskněte klávesu **Enter**.
   
   zahájí proces odesílání Hello a doba trvání může záviset na mnoha faktorech včetně rychlosti sítě a velikost bitové kopie hello
9. Pokud vaše nahrávání nezdaří z důvodu přerušení sítě nebo věcí jako je například, můžete vždy obnovit hello nahrávání procesu, který jste začali. tooresume nahrávaný, spusťte skript hello znovu s použitím hello stejné příkazového řádku.

> [!WARNING]
> Nikdy změnit skriptu nahrávání hello. Kontrolách konkrétních byla implementovaná tooensure, který hello image splňuje hello image požadavky a požadavky na aplikace.
> 
> 

## <a name="common-problems"></a>Běžné problémy
* Ujistěte se, že používáte prostředí Windows PowerShell, ne Azure PowerShell. Je nutné modul Azure PowerShell hello tooinstall, protože některé moduly jsou potřeba během procesu nahrávání hello.
* Nikdy alter hello skriptu, ověření existují pro usnadnění vaší práce.
* Pokud soubor virtuálního pevného disku hello získá uzamčen během nahrávání, zkopírujte soubor hello nebo přesunout ho znovu tooa nové umístění a pokus o odeslání. Může být některé procesu systému Windows, který brání odeslání.  

