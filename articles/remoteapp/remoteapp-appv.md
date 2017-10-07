---
title: "aaaUsing App-V aplikací s Azure Remoteappem | Microsoft Docs"
description: "Zjistěte, jak aplikace toouse App-V v Azure Remoteappu."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: e2292cb2-5c89-4b2b-ab11-74dbacd07c31
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 9cf5c2eeee2a0ce2cf98e1cf6497dffbc6eff016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-app-v-apps-in-azure-remoteapp"></a>Pomocí aplikace App-V v Azure Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Aplikace App-V můžete použít v hybridní kolekce Azure Remoteappu, který vyžaduje připojení k doméně.

Než začnete, ujistěte se, že klient App-V 5.1 hello tooinstall s nejnovějšími aktualizacemi hello. Budete potřebovat toocreate [vlastní image](remoteapp-create-custom-image.md) , zahrnuje client hello App-V.  

Je snadno toouse stávající infrastruktury sady App-V s Azure Remoteappem. Vzhledem k tomu, že hybridní kolekce je nasazený do virtuální sítě Azure, který má přístup k řadiči domény tooyour a hello virtuální počítače jsou připojené k doméně, můžete využít stávající App-v infrastruktury a nasazení metody tooeasyily hostitele App-V aplikaci v Azure Remoteappu. Zde jsou některé aspekty, které byste měli vědět, na základě typu nasazení App-V, které máte aktuálně hello:

| Možnosti konfigurace |  | Kladné | Záporná |
| --- | --- | --- | --- |
| Metoda doručování |Streamování na vyžádání) |Aplikace je vždy hello nejnovější a připravený |První časovou náročnost |
| Připojit |Nejrychlejší; aplikace se již nachází na hello virtuálních počítačů |Tomu - trvá místo bitové kopie (limit 127 GB) | |
| Aplikace umístění úložiště |Sdílený obsah |Spuštění aplikace v paměti instance Azure RemoteApp |Eats paměť a správné připojení toostreaming (soubor) serveru, kde se nachází aplikace hello |
| Disk (v mezipaměti) |Rychlé spuštění. Aplikace není závislý na dostupnosti zdroje obsahu |Tomu - trvá místo bitové kopie (limit 127 GB) | |
| Cílení na |Uživatel |Vyžaduje infrastrukturu úplné samostatné sady App-V | |
| Globální (počítače) |Předem publikovat nebo cílení pomocí publikování serveru |Třeba tooupdate Azure image, pokud chcete aplikace hello tooupdate (velký). Trvá místo na bitovou kopii. | |

 Jakmile vytvoříte vlastní bitovou kopii a hybridní kolekci, publikování aplikace, přiřazení uživatelů a získejte vaší stávající aplikace App-V, které jsou hostované v Azure Remoteappu doručit tooany zařízení a odkudkoli.

