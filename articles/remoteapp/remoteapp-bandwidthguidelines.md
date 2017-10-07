---
title: "Šířka pásma sítě vzdálené aplikace RemoteApp aaaAzure - obecné pokyny | Microsoft Docs"
description: "Pochopte některé základní síťové šířky pásma pokyny pro vaše kolekce vzdálené aplikace RemoteApp Azure a aplikace."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 529bf318-ae37-4c14-a11c-43fa703d68a3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d3407eea71e2e8ac524787ee680cfd870c800864
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-remoteapp-network-bandwidth---general-guidelines-if-you-cant-test-your-own"></a>Azure RemoteApp šířka pásma sítě – obecné pokyny (Pokud nelze testovat vlastní)
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Pokud nemáte hello hello čas nebo schopností toorun [testy šířky pásma sítě](remoteapp-bandwidthtests.md) pro Azure RemoteApp, tady jsou některé poměrně obecné pokyny, které vám může pomoct odhad šířky pásma sítě na uživatele.

Pokud máte kombinaci těchto scénářů, nedoporučujeme nic menší než (nebo rovno) 10 MB/s jako hello minimální šířku pásma sítě pro moderní aplikace připojené k Internetu ve vzdáleném prostředí. (I když popsané, to nebude zajistit vyšší než průměrná uživatelské prostředí.)

## <a name="complex-powerpoint-simple-powerpoint"></a>Komplexní PowerPoint jednoduché PowerPoint
Azure RemoteApp nepodporuje nejlépe v místní síti 100 MB. V hello 10 MB/s síťový profil, kolísání výše 120 ms po více než 5 %, hello uživatele se zobrazí průměrnou prostředí. Na hello 1 MB/s jinou je výrazná – například v prezentaci, nemusí hello uživateli zobrazit animovaný přechody vůbec, protože se přeskočí rámce.

## <a name="internet-explorer-mixed-pdf-pdf-text"></a>Internet Explorer ve smíšeném, PDF, PDF, Text
Profil síťového 10 MB/s je zavřít tooLAN v většinu aspektů. 1 MB/s poskytne OK prostředí, i když existují některé zpoždění, co uživatel posune Přestože jsou bitové kopie na úvodní obrazovka.

## <a name="flash-video-youtube"></a>Flash video (YouTube)
100 MB/s LAN poskytuje hello co nejlepších výsledků, zatímco 10 MB/s je přijatelné (tj. jsme udržovat tempo s hello obnovovací frekvence, ale zmenší se zvyšuje). Zpoždění v 1 MB/s, je velmi vysoké a znatelné.

## <a name="word-typing-word-remote-input"></a>Aplikace Word zadáním (Word vzdálené vstup)
Toto je scénáři použití malou šířkou pásma. Na 256 KB/s poskytujeme stejně dobře z prostředí jako LAN.

## <a name="learn-more"></a>Další informace
* [Odhadnout využití šířky pásma sítě Azure RemoteApp](remoteapp-bandwidth.md)
* [Azure RemoteApp – jak šířku pásma sítě a kvalitu prostředí pracovní společně?](remoteapp-bandwidthexperience.md)
* [Azure RemoteApp - tseting využití šířky pásma sítě s některé běžné scénáře](remoteapp-bandwidthtests.md)

