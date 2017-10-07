---
title: "adresy pro správu aaaAzure App Service Environment"
description: "Seznamy hello správy adresy použity toocommand služby App Service Environment"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a7738a24-89ef-43d3-bff1-77f43d5a3952
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: b34b6266dc3a35915421b14bf34eddc07c2825c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="app-service-environment-management-addresses"></a>Adresy pro správu App Service Environment

Hello App Service Environment(ASE) je nasazení hello Azure App Service na podsíť ve virtuální síti Azure (VNet).  Hello App Service Environment musí být dostupný z hello Azure App Service, tak, aby bylo možné jej spravovat.  Tento provoz správy App Service Environment prochází hello pod kontrolou uživatele sítě.  Pochází ze služby Azure App Service management servery toohello veřejné VIP přidružený hello App Service Environment.  Podrobnosti o App Service Environment hello sítě závislosti číst [sítě aspekty a hello App Service Environment][networking].  Obecné informace o App Service Environment hello můžete spustit v [toohello Úvod App Service Environment][intro].

Tento dokument uvádí hello zdrojové IP adresy pro správu provoz toohello App Service Environment. Můžete použít tyto adresy toocreate skupin zabezpečení sítě toolock dolů příchozí provoz nebo je podle potřeby použijte v směrovací tabulky.  toouse tyto informace budete potřebovat toouse:

* Hello IP adresy, které jsou uvedeny pro všechny oblasti
* Hello IP adresy, že shoda toohello oblast, kterou vaše App Service Environment je nasazený do.

příchozí provoz správy Hello pochází z těchto IP adres tooports 454 a 455.

| Oblast | Adresy |
|--------|-----------|
| Všechny oblasti | 70.37.57.58, 157.55.208.185, 52.174.22.21,13.94.149.179,13.94.143.126,13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251, 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141 |
| Střed USA – jih & střed USA – sever | 23.102.188.65, 191.236.154.88 |
| Austrálie – jihovýchod & Austrálie – východ | 23.101.234.41, 104.210.90.65 |
| USA – západ & USA – východ | 104.45.227.37, 191.236.60.72 |
| Západní Evropa & Severní Evropa | 191.233.94.45, 191.237.222.191 |
| Střed USA – západ & západní USA 2 | 13.78.148.75, 13.66.225.188 |
| Střed USA & východní USA 2 | 104.43.165.73, 104.46.108.135 |
| Východní Asie & Asie a Tichomoří – jihovýchod | 23.99.115.5, 104.215.158.33 |
| Japonsko – východ & Japonsko – západ | 104.41.185.116, 191.239.104.48 |
| Střední Kanada & Východní Kanada | 40.85.230.101, 40.86.229.100 |
| Spojené království – západ a Spojené království – jih | 51.141.8.34, 51.140.185.75 |
| Korejská Jižní & Korejská – střed | 52.231.200.177, 52.231.32.117 |
| Brazílie – jih & střed USA – jih| 104.41.46.178, 23.102.188.65 |
| Střed & Indie – jih | 104.211.98.24, 104.211.225.66 |
| Indie – Západ, Indie & Indie – jih | 104.211.160.229, 104.211.225.66 |


<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md
