---
title: "omezení aaaAzure prostředí cloudu (Preview) | Microsoft Docs"
description: "Přehled omezení prostředí cloudu Azure"
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
ms.date: 08/21/2017
ms.author: juluk
ms.openlocfilehash: 8462b0b9850fcde790a386433009439bbab52c0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="limitations-of-azure-cloud-shell"></a>Omezení prostředí cloudu Azure
Azure Cloud prostředí má následující hello známé omezení:

## <a name="system-state-and-persistence"></a>Stav systému a trvalost
Hello počítač, který obsahuje vaše cloudové prostředí relace je dočasný a bude recyklována po vaše relace je neaktivní po dobu 20 minut. Cloudové prostředí vyžaduje toobe sdílenou složku soubor připojené. Vaše předplatné v důsledku toho musí být schopný tooset až tooaccess prostředky úložiště cloudové prostředí. Mezi další aspekty patří:
* S použitím připojené úložiště, pouze změny v rámci vaší `$Home` adresáře nebo `clouddrive` adresáře jsou nastavené jako trvalé.
* Sdílené složky může být připojen pouze z uvnitř vaší [přiřazené oblast](persisting-shell-storage.md#mount-a-new-clouddrive).
* Soubory Azure podporuje pouze místně redundantního úložiště a účty geograficky redundantní úložiště.

## <a name="user-permissions"></a>Uživatelská oprávnění
Máte nastavená oprávnění jako běžní uživatelé bez přístupu sudo. Všechny instalace mimo vaší `$Home` directory neuchovává.
I když některé příkazy v rámci hello `clouddrive` adresář, jako třeba `git clone`, nemáte správná oprávnění, vaše `$Home` directory nemá oprávnění.

## <a name="browser-support"></a>Podpora prohlížeče
Cloudové prostředí podporuje hello nejnovější verze Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox a Apple Safari. Safari v privátním režimu není podporována.

## <a name="copy-and-paste"></a>Kopírování a vkládání
CTRL + C a Ctrl + V nefungují jako zkopírujte a vložte zkratky v prostředí cloudu na počítače s Windows, použijte kombinaci kláves Ctrl + Insert a Shift + Insert toocopy a vložení v uvedeném pořadí.

Klikněte pravým tlačítkem kopírování a vkládání možnosti jsou dostupné i, ale funkce klikněte pravým tlačítkem na je subjektu toobrowser specifické schránky přístup.

## <a name="editing-bashrc"></a>Úpravy .bashrc
Proveďte opatrní při úpravě .bashrc, tak může způsobit neočekávané chyby v prostředí cloudu.

## <a name="bashhistory"></a>.bash_history
Historii bash příkazy může být nekonzistentní z důvodu narušení relace prostředí cloudu nebo souběžných relací.

## <a name="usage-limits"></a>Omezení využití
Cloudové prostředí je určen pro případy použití interaktivní. V důsledku toho ukončení relací neinteraktivní všechny dlouhodobé bez upozornění.

## <a name="network-connectivity"></a>Připojení k síti
Všechny latence v prostředí cloudu je připojení k Internetu toolocal předmětu, pokračuje v prostředí cloudu tooattempt toocarry se žádné pokyny odeslána.

## <a name="next-steps"></a>Další kroky
[Rychlý start cloudové prostředí](quickstart.md)
