---
title: "okno prostředí cloudu Azure (Preview) hello aaaUsing | Microsoft Docs"
description: "Okno prostředí cloudu Azure hello návod."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: juluk
ms.openlocfilehash: 571db3c8e177799a9e05f38a7cf8d2a4d5f8c8de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cloud-shell-window"></a>Pomocí okna hello prostředí cloudu Azure

Tento dokument popisuje, jak toouse hello okno cloudové prostředí.

## <a name="concurrent-sessions"></a>Souběžných relací
Cloudové prostředí umožňuje víc souběžných relací mezi záložkách prohlížeče tím, že každý tooexist relace jako samostatný proces Bash.
Pokud ukončení relace, ujistěte se, tooexit z každé relaci okno každý proces běží nezávisle i když běží na hello stejný počítač.

## <a name="restart-cloud-shell"></a>Restartujte cloudové prostředí
![](media/recycle.png)
> [!WARNING]
> Restartování prostředí cloudu obnoví stav počítače a všechny soubory ve vašem souboru není trvalý sdílené složky budou ztraceny.

* Klikněte na ikonu restartování hello na panelu nástrojů tooreceive hello nové prostředí pro cloudové prostředí.

## <a name="minimize--maximize-cloud-shell-window"></a>Minimalizovat & maximalizace okna cloudové prostředí
![](media/minmax.png)
* Klikněte na tlačítko hello minimalizovat ikonu na hello top napravo od okna toohide hello ho. Klikněte na tlačítko hello cloudové prostředí ikonu znovu toounhide.
* Klikněte na tlačítko hello maximalizovat ikonu tooset okno toomax výšku. okno tooprevious toorestore velikost, kliknutím na tlačítko Obnovit.

## <a name="copy-and-paste"></a>Kopírování a vkládání
* Windows: `Ctrl-insert` toocopy a `Shift-insert` toopaste. Pravým tlačítkem klikněte na rozevírací seznam můžete také povolit kopírování a vkládání.
  * FireFox nebo IE nemusí podporovat schránky oprávnění správně.
* Mac OS: `Cmd-c` toocopy a `Cmd-v` toopaste. Pravým tlačítkem klikněte na rozevírací seznam můžete také povolit kopírování a vkládání.

## <a name="resize-cloud-shell-window"></a>Změnit velikost okna cloudové prostředí
* Klikněte na tlačítko a přetáhněte hello horní okraj hello nástrojů nahoru nebo dolů tooresize hello cloudové prostředí okno.

## <a name="scrolling-text-display"></a>Posouvání zobrazení textu
* Posuňte se text terminálu toomove myši nebo dotykové.

## <a name="exit-command"></a>Příkaz exit
Spuštění `exit` ukončí aktivní relace hello. K tomuto chování dochází ve výchozím nastavení po 20 minutách bez zásahu.

## <a name="next-steps"></a>Další kroky
[Rychlý start cloudové prostředí](quickstart.md)
