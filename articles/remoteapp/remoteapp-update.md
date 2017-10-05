---
title: Aktualizace kolekce Azure Remoteappu | Microsoft Docs
description: Postup aktualizace kolekce Azure Remoteappu
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: e553d432-e581-48fe-b996-c432357eb64a
ms.service: remoteapp
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 454d78445d6092aec9eaa383e4c50cf15195848c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="update-a-collection-in-azure-remoteapp"></a>Aktualizace kolekce v Azure Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Existuje bude pocházet určité doby nevyhnutelnou, když je potřeba aktualizovat aplikací nebo bitové kopie v kolekci Azure RemoteApp. Pokud použijete jednu z imagí, které jsou součástí vašeho předplatného Azure RemoteApp, v cloud nebo hybridní kolekci, všechny aktualizace jsou zpracovávány Azure RemoteApp sebe, tak můžete easy rest.

Ale pokud používáte vlastní image (jste vytvořili od začátku nebo jste vytvořili úpravou jeden z našich obrázků), odpovídáte za údržbu image a aplikací. Pokud je potřeba aktualizovat image nebo některé z aplikací je uvnitř, budete muset vytvořit nové a aktualizované verze bitové kopie a poté nahraďte tento nový aktualizovanou bitovou kopii existující bitová kopie v kolekci.

Ano jak tedy můžete o aktualizaci vaší kolekce? Jedná se o přímočará:

1. Aktualizujte bitovou kopii, která jste použili v kolekci. Použít všechny opravy nebo potřebné aktualizace a pak ho uložte s novým názvem.
2. [Nahrát](remoteapp-uploadimage.md) nebo [importovat](remoteapp-image-on-azurevm.md) této bitové kopie do vzdálené aplikace RemoteApp.
3. Nyní na stránce kolekce, klikněte na **aktualizace**.
4. Vyberte novou bitovou kopii z **Image šablony** seznamu.
5. Zde je složité část – je potřeba rozhodnout, jak nakládat s žádným uživatelem, které aktuálně používáte aplikaci v kolekci. K dispozici jsou následující možnosti:
   
   * **Ponechat uživatelům po aktualizaci 60 minut**. Ihned po dokončení aktualizace, Azure RemoteApp se zobrazí zpráva pro všechny aktivní uživatele upozorněním uložit práci a odhlásit a znovu přihlásit. Po 60 minutách všech aktivních uživatelů, kteří nejsou odhlášením automaticky odhlášeni. Uživatelé mohou okamžitě přihlásit znovu.
   * **Odhlásit uživatele hned**. Ihned po dokončení aktualizace, odhlaste se od všech uživatelů automaticky bez varování. Pokud zvolíte tuto možnost, uživatelé mohou ztratit data. Však se znovu připojit k aplikaci okamžitě.
6. Kliknutím na značku zaškrtnutí zahájíte aktualizace.

