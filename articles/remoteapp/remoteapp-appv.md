---
title: "Pomocí aplikace App-V s Azure Remoteappem | Microsoft Docs"
description: "Další informace o použití aplikace App-V v Azure Remoteappu."
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
ms.openlocfilehash: e55bb8db83c04025c46b383a9ebbef4399178116
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-app-v-apps-in-azure-remoteapp"></a>Pomocí aplikace App-V v Azure Remoteappu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Aplikace App-V můžete použít v hybridní kolekce Azure Remoteappu, který vyžaduje připojení k doméně.

Než začnete, ujistěte se, zda je instalace klienta App-V 5.1 pomocí nejnovější aktualizace. Budete muset vytvořit [vlastní image](remoteapp-create-custom-image.md) , který obsahuje klienta sady App-V.  

Je snadné použití existující infrastruktury sady App-V s Azure RemoteApp. Vzhledem k tomu, že hybridní kolekce je nasazený do virtuální sítě Azure, který má přístup k řadiči domény a virtuální počítače jsou připojené k doméně, můžete využít vaší existující sady App-v infrastruktury a nasazení metody easyily hostitele aplikace App-V v Azure Remoteappu. Zde jsou některé aspekty, které byste měli vědět, na základě typu nasazení App-V, které aktuálně máte:

| Možnosti konfigurace |  | Kladné | Záporná |
| --- | --- | --- | --- |
| Metoda doručování |Streamování na vyžádání) |Aplikace je vždy nejnovější a čerstvou |První časovou náročnost |
| Připojit |Nejrychlejší; aplikace se již nachází ve virtuálním počítači |Tomu - trvá místo bitové kopie (limit 127 GB) | |
| Aplikace umístění úložiště |Sdílený obsah |Spuštění aplikace v paměti instance Azure RemoteApp |Eats paměti a dobré připojení k vysílání datového proudu (soubor) serveru, kde se nachází aplikace |
| Disk (v mezipaměti) |Rychlé spuštění. Aplikace není závislý na dostupnosti zdroje obsahu |Tomu - trvá místo bitové kopie (limit 127 GB) | |
| Cílení na |Uživatel |Vyžaduje infrastrukturu úplné samostatné sady App-V | |
| Globální (počítače) |Předem publikovat nebo cílení pomocí publikování serveru |Potřeba aktualizovat Azure image, pokud chcete aktualizovat aplikaci (velký). Trvá místo na bitovou kopii. | |

 Jakmile vytvoříte vlastní bitovou kopii a hybridní kolekci, publikování aplikace, přiřazení uživatelů a získejte vaší stávající aplikace App-V, které jsou hostované v Azure Remoteappu doručí na jakémkoliv zařízení a odkudkoli.

