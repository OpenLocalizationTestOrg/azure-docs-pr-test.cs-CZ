---
title: "Nikdy ukládat citlivá data na vlastních bitových kopií pro Azure Remoteappu | Microsoft Docs"
description: "Další informace o pokyny pro ukládání dat do vlastních bitových kopií v Azure Remoteappu"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 5a19903b-15f9-49d9-9bc1-ae80f2671aa1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 75d5415d33324d957617426e75909a6c6c58b1f9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="never-store-sensitive-data-on-custom-images"></a>Nikdy ukládat citlivá data na vlastních bitových kopií
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Pokud je hostitelem vaší vlastní aplikaci v Azure Remoteappu, prvním krokem je vytvoření vlastní image. Pro vytvoření instancí virtuálních počítačů, které slouží aplikace uživatelům používáme této vlastní bitové kopie. Vlastní image by měly obsahovat jenom aplikace a nikdy citlivá data, která může dojít ke ztrátě, například SQL databáze, soubory pracovníky nebo speciální datových souborů, jako jsou soubory QuickBooks společnosti. Všechny citlivá data by měl být umístěn mimo Azure RemoteApp na souborovém serveru, jiným virtuálním Počítačem Azure, nebo v SQL Azure. Bitovou kopii by měl hostitel jenom aplikace, která se připojuje ke zdroji dat a představuje data. Zkontrolujte [požadavky pro Azure RemoteApp image](remoteapp-imagereqs.md) Další informace. 

Chcete-li pochopit, proč by neměla ukládat citlivá data, je potřeba pochopit, jak funguje Azure RemoteApp. Při vytvoření nebo aktualizaci na pozadí více klonech nebo se obrázek kolekce se vytvoří. Všechny tyto instance virtuálních počítačů jsou přesný replik vlastní Image; Když uživatelé spustí aplikace jsou připojeny k jednu z těchto instancí virtuálních počítačů. Ale na stejnou instanci není zaručena a neměli důležité, protože jsou dočasnou. Instance virtuálních počítačů, který je hostitelem aplikace jsou dočasnou a lze zničen nebo odstranit na základě, například během aktualizace kolekce. 

Jakmile kolekce je zřízený a uživatelé začít připojení k virtuálním počítačům, data uživatele je trvalé a chránit, protože je uložen na samostatné úložiště v rámci virtuální pevný disk, který nazýváme [disk profilu uživatele (UPD)](remoteapp-upd.md), což je profilu uživatele v c:\users\<userprofile >. Při spuštění aplikace, UPD je připojena a zpracováván jako místní uživatelský profil operačního systému. Další informace o [Azure RemoteApp uloží uživatelských dat a nastavení](remoteapp-upd.md).

Příklad data, která se nesmí nacházet v bitové kopii:

* Sdílená data uživatelům přístup k
* Databázi SQL nebo QuickBooks DB
* Všechna data v D:\

Příklad data, která mohou být umístěny v výchozí profil zkopírovat do UPD každých uživatelů:

* Konfigurační soubory na uživatele
* Skripty, které by uživatelé potřebují zachovaná v jejich UPD

Klíčové body:

* Nikdy úložiště citlivá data, která může dojít ke ztrátě na bitovou kopii při vytváření vlastní image.
* Citlivá data vždy by měl být umístěn na samostatném souborovém serveru, samostatný virtuální počítač Azure, v cloudu a externí vždy k hostování vaší aplikace v rámci Azure RemoteApp instance virtuálních počítačů. 
* Uživatelská data budou uložena a přetrvává v disk profilu uživatele (UPD)

