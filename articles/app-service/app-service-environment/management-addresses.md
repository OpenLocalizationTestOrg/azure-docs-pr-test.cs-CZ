---
title: "Adresy pro správu Azure App Service Environment"
description: "Zobrazí seznam adres správy používá k příkazu služby App Service Environment"
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
ms.openlocfilehash: e97a084772fd16252d925b62498d2e696629a25d
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="app-service-environment-management-addresses"></a>Adresy pro správu App Service Environment

Environment(ASE) služby aplikace je nasazení služby Azure App Service na podsíť ve virtuální síti Azure (VNet).  App Service Environment musí být přístupné ze služby Azure App Service, tak, aby bylo možné jej spravovat.  Tento provoz správy App Service Environment prochází sítí pod kontrolou uživatele.  Pochází ze serverů pro správu služby Azure App Service na veřejné VIP, který je přidružen App Service Environment.  Podrobnosti o App Service Environment sítě závislosti číst [sítě aspekty a App Service Environment][networking].  Obecné informace o App Service Environment můžete spustit v [Úvod do služby App Service Environment][intro].

Tento dokument uvádí zdrojové IP adresy pro přenos pro správu na App Service Environment. Tyto adresy můžete použít k vytvoření skupin zabezpečení sítě pro zamknout příchozí provoz nebo je používat v směrovací tabulky podle potřeby.  Tyto informace používat, budete muset použít:

* IP adresy, které jsou uvedené pro všechny oblasti
* IP adresy, které odpovídají oblast, která vaše App Service Environment je nasazena do.

Příchozí přenosy dat správy pochází z těchto IP adres na portech 454 a 455.

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
