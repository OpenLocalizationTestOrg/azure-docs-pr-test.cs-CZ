---
title: "Používání okna prostředí cloudu Azure (Preview) | Microsoft Docs"
description: "Návod okno prostředí cloudu Azure."
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
ms.openlocfilehash: a47961dfdaf178a6b793bd68105d9792a9275bb3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="using-the-azure-cloud-shell-window"></a>Používání okna prostředí cloudu Azure

Tento dokument vysvětluje, jak použít okno cloudové prostředí.

## <a name="concurrent-sessions"></a>Souběžných relací
Cloudové prostředí umožňuje víc souběžných relací mezi záložkách prohlížeče tím, že každou relaci existovat jako samostatný proces Bash.
Pokud ukončení relace, je nutné ukončit každé relaci okno každý proces běží nezávisle i když běží na stejném počítači.

## <a name="restart-cloud-shell"></a>Restartujte cloudové prostředí
![](media/recycle.png)
> [!WARNING]
> Restartování prostředí cloudu obnoví stav počítače a všechny soubory ve vašem souboru není trvalý sdílené složky budou ztraceny.

* Klikněte na ikonu restartování na panelu nástrojů pro příjem nové prostředí pro cloudové prostředí.

## <a name="minimize--maximize-cloud-shell-window"></a>Minimalizovat & maximalizace okna cloudové prostředí
![](media/minmax.png)
* Klikněte na ikonu Minimalizovat nahoře napravo od okna ho chcete skrýt. Klikněte na ikonu cloudové prostředí znovu na Zobrazit skryté.
* Klikněte na ikonu Maximalizovat nastavit okno na maximální výšku. Pokud chcete obnovit původní velikost okna, kliknutím na tlačítko Obnovit.

## <a name="copy-and-paste"></a>Kopírování a vkládání
* Windows: `Ctrl-insert` zkopírovat a `Shift-insert` vložit. Pravým tlačítkem klikněte na rozevírací seznam můžete také povolit kopírování a vkládání.
  * FireFox nebo IE nemusí podporovat schránky oprávnění správně.
* Mac OS: `Cmd-c` zkopírovat a `Cmd-v` vložit. Pravým tlačítkem klikněte na rozevírací seznam můžete také povolit kopírování a vkládání.

## <a name="resize-cloud-shell-window"></a>Změnit velikost okna cloudové prostředí
* Klikněte na tlačítko a přetáhněte ji na horní okraj panelu nástrojů nahoru nebo dolů změňte velikost okna cloudové prostředí.

## <a name="scrolling-text-display"></a>Posouvání zobrazení textu
* Posouvání pomocí myši nebo dotykové přesunout terminálu text.

## <a name="exit-command"></a>Příkaz exit
Spuštění `exit` ukončí aktivní relace. K tomuto chování dochází ve výchozím nastavení po 20 minutách bez zásahu.

## <a name="next-steps"></a>Další kroky
[Rychlý start cloudové prostředí](quickstart.md)
