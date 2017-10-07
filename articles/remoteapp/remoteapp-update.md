---
title: aaaUpdate kolekce Azure Remoteappu | Microsoft Docs
description: "Zjistěte, jak tooupdate kolekcí vzdálené aplikace Azure RemoteApp"
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
ms.openlocfilehash: 849d7abfdfad4dbe6a235d2a28c71f7943eb812c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="update-a-collection-in-azure-remoteapp"></a>Aktualizace kolekce v Azure Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Existuje bude pocházet určité doby nevyhnutelnou, když potřebujete aplikace hello tooupdate nebo bitové kopie v kolekci Azure RemoteApp. Pokud použijete jednu z imagí hello součástí vašeho předplatného Azure RemoteApp, v cloud nebo hybridní kolekci, všechny aktualizace jsou zpracovávány Azure RemoteApp sebe, tak můžete easy rest.

Ale pokud používáte vlastní image (jste vytvořili od začátku nebo jste vytvořili úpravou jeden z našich obrázků), odpovídáte za údržbu hello image a aplikací. Pokud potřebujete tooupdate bitové kopie nebo některé z aplikací hello je uvnitř, je třeba toocreate nové a aktualizované verze hello bitové kopie, a poté nahraďte hello stávající image v kolekci pomocí této nové aktualizovanou bitovou kopii.

Ano jak tedy můžete o aktualizaci vaší kolekce? Jedná se o přímočará:

1. Obrázek hello aktualizace, který jste použili v kolekci. Použít všechny opravy nebo potřebné aktualizace a pak ho uložte s novým názvem.
2. [Nahrát](remoteapp-uploadimage.md) nebo [importovat](remoteapp-image-on-azurevm.md) tooRemoteApp této bitové kopie.
3. Nyní na stránce hello kolekce, klikněte na **aktualizace**.
4. Vybrat novou bitovou kopii hello z hello **Image šablony** seznamu.
5. Zde je složité součástí hello – jak budete potřebovat toodecide toodeal s žádným uživatelem, které aktuálně používáte aplikaci v kolekci hello. Máte hello následující možnosti:
   
   * **Ponechat uživatelům po aktualizaci hello 60 minut**. Ihned po dokončení aktualizace hello, Azure RemoteApp se zobrazí uživatelům active tooany zpráva upozorněním toosave jejich pracovních a protokolu vypnout a znovu přihlásit. Po 60 minutách všech aktivních uživatelů, kteří nejsou odhlášením automaticky odhlášeni. Uživatelé mohou okamžitě přihlásit znovu.
   * **Odhlásit uživatele hned**. Ihned po dokončení hello aktualizace, odhlaste se od všichni uživatelé automaticky bez varování. Pokud zvolíte tuto možnost, uživatelé mohou ztratit data. Však se znovu připojit aplikace toohello okamžitě.
6. Kliknutím na tlačítko hello zaškrtnutí toostart hello aktualizovat.

