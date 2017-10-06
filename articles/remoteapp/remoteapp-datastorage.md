---
title: "aaaNever úložiště citlivá data v vlastních bitových kopií pro Azure Remoteappu | Microsoft Docs"
description: "Další informace o hello pokyny pro ukládání dat do vlastních bitových kopií v Azure Remoteappu"
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
ms.openlocfilehash: 86a0fea218f8826d6d25f50d3c4c36e368e26fb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="never-store-sensitive-data-on-custom-images"></a>Nikdy ukládat citlivá data na vlastních bitových kopií
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Pokud je hostitelem vaší vlastní aplikaci v Azure Remoteappu, hello prvním krokem je toocreate vlastní image. Jsme použít této instance virtuálních počítačů toocreate vlastní image, které slouží tooyour uživatelů vaší aplikace. vlastní image Hello by měly obsahovat jenom aplikace a nikdy citlivá data, která může dojít ke ztrátě, například SQL databáze, soubory pracovníky nebo speciální datových souborů, jako jsou soubory QuickBooks společnosti. Všechny citlivá data by měl být umístěn externí tooAzure vzdálené aplikace RemoteApp na souborovém serveru, jiným virtuálním Počítačem Azure, nebo v SQL Azure. Obrázek Hello má právě aplikace hello hostitele, která připojí toohello zdroj dat a uvede hello data. Zkontrolujte [požadavky pro Azure RemoteApp image](remoteapp-imagereqs.md) Další informace. 

toounderstand, proč by neměla ukládat citlivá data, je nutné toounderstand jak funguje Azure RemoteApp. Kolekce se vytvoří nebo aktualizuje, pozadí hello více klony nebo kopie Image hello vytvářejí. Všechny tyto instance virtuálních počítačů jsou přesný repliky hello vlastní image; Když uživatelé spustí aplikace jsou připojené tooone instancí těchto virtuálních počítačů. Ale hello stejnou instanci není zaručena a neměli důležité, protože jsou dočasnou. Hello instancí virtuálních počítačů v hostitelských aplikací hello trvalé a může být zničený, nebo odstranit na základě, například během aktualizace kolekce. 

Jakmile hello kolekce je zřízený a uživatelé začít připojování toohello virtuálních počítačů, uživatelská data je trvalé a chránit, protože je uložen na samostatné úložiště v rámci virtuální pevný disk, který nazýváme [disk profilu uživatele (UPD)](remoteapp-upd.md), což je hello uživatelský profil v c:\users\<userprofile >. Při spuštění aplikace, hello UPD je připojena a zpracováván jako místní uživatelský profil hello operačním systému. Další informace o [Azure RemoteApp uloží uživatelských dat a nastavení](remoteapp-upd.md).

Příklad data, která se nesmí nacházet v bitové kopii hello:

* Sdílených dat pro tooaccess uživatelů
* Databázi SQL nebo QuickBooks DB
* Všechna data v D:\

Příklad dat, která mohou být umístěny v hello výchozí profil toobe zkopírují do UPD každých uživatelů:

* Konfigurační soubory na uživatele
* Skripty, které by uživatelé potřebují zachovaná v jejich UPD

Klíčové body:

* Nikdy neukládají citlivá data, která může dojít ke ztrátě na bitovou kopii hello při vytváření vlastní image.
* Citlivá data by měla vždy nacházet v samostatném souborovém serveru, jednotlivé virtuální počítač Azure, na cloudu hello a instancí virtuálních počítačů vždy externí toohello hostování vaší aplikace v rámci Azure RemoteApp. 
* Uživatelská data budou uložena a přetrvává v hello disk profilu uživatele (UPD)

