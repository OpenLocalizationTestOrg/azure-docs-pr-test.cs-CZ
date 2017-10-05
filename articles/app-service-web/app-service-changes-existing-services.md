---
title: "Azure App Service a její vliv na stávající služby Azure"
description: "Vysvětluje, jak nové Azure App Service a její funkce mít vliv na stávající služby v Azure."
services: app-service
documentationcenter: 
author: yochay
manager: nirma
editor: 
ms.assetid: 86c6a292-3c33-49f4-890c-89cc0321b397
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2016
ms.author: yochaykk
ms.openlocfilehash: ed967fda7a216ed49532be54228ebe888cf16b6f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-and-existing-azure-services"></a>Azure App Service a stávající služby Azure
Tento článek popisuje změny stávající služby Azure jako součást změny jak seskupit několik služeb Azure do [Azure App Service](https://azure.microsoft.com/services/app-service/), integrované novou nabídku.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a>Přehled
[Aplikační služba Azure](https://azure.microsoft.com/services/app-service/) je nových a jedinečných Cloudová služba, která umožňuje vývojářům vytvářet webových a mobilních aplikací pro libovolnou platformu a z jakéhokoliv zařízení. App Service je navržený tak, aby zjednodušit opakovaných kódování funkcí, integrace s enterprise a SaaS systémy a automatizovat firemní procesy. zároveň pokrývat vašim požadavkům na zabezpečení, spolehlivosti a škálovatelnosti integrované řešení.

Služby App Service spojuje následující stávající služby Azure - [weby](https://azure.microsoft.com/services/websites/), [Mobile Services](https://azure.microsoft.com/services/mobile-services/), a [Biztalk Services](https://azure.microsoft.com/services/biztalk-services/) do jednoho kombinaci služby, při přidávání výkonné nové funkce.  Služby App Service umožňuje hostování následující typy aplikací:

* Web Apps
* Mobile Apps
* API Apps
* Logic Apps

Následující tabulka vysvětluje, jak existující mapu služby Azure App Service a typy aplikací, která je k dispozici v něm.

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%">Stávající služba Azure</th>
<th align="left", style="width:10%">Azure App Service</th>
<th align="left", style="width:80%">Co se změnilo</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Azure Websites</td>
<td align="left">Web Apps</td>
<td align="left"><li>Pro weby Azure App Service je výhradně omezená na změnit název webové stránky do webové aplikace.
<p><li>Všechny existující instance webů, které jsou nyní webové aplikace ve službě App Service.</p>
<p><li>Přistupujete prostřednictvím své existující weby <a href="http://go.microsoft.com/fwlink/?LinkId=529715">portálu Azure</a>, kde najdete všechny existující weby v rámci <em>webové aplikace</em>.</p>
<p><li><em>Web, který je hostitelem plánování</em> je nyní <em>plán služby App Service</em>. <em>Plán služby App Service</em> může být hostitelem libovolného typu aplikace služby App Service, jako jsou webové, mobilní, logiku nebo rozhraní API aplikace.</p>
<p><li>Azure App Service Web Apps je obecné dostupnosti.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/web/">Další informace o webových aplikacích</a>.</p></td>
</tr>
<tr class="even">
<td align="left">Azure Mobile Services</td>
<td align="left">Mobile Apps</td>
<td align="left"><p><li>Mobile Services nadále k dispozici jako samostatné služby a zůstanou plně podporované.</p>
<p><li>Mobilní aplikace je typ aplikace ve službě App Service, která integruje všechny funkce Mobile Services a další.</p>
<p><li>Je snadné <a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">migrovat z mobilní služby do mobilní aplikace</a>.</p>
<p><li>Jako součást služby App Service Mobile Apps získat nové funkce nad rámec Mobile Services, například integrace s místními a SaaS systémy, přípravné sloty, webové úlohy, možnosti lépe měřítka a další.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/mobile/">Další informace o Mobile Apps</a>.</p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">API Apps</td>
<td align="left">
<p><li>API Apps je nový typ aplikace ve službě App Service, která vám umožní snadno vytvářet a používat rozhraní API v cloudu.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/api/">Další informace o aplikacích API</a>.</p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left">Logic Apps</td>
<td align="left">
<p><li>Služba Logic Apps je nový typ aplikace ve službě App Service, která umožňuje snadnou automatizaci obchodních procesů.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/logic/">Další informace o aplikacích logiky</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">Azure BizTalk Services</td>
<td align="left">Aplikacích API BizTalk</td>
<td align="left">
<li><p>BizTalk Services nadále k dispozici jako samostatné služby a zůstanou plně podporované.</p>
<li><p>Všechny funkce služby BizTalk Services jsou integrované do služby App Service jako aplikace rozhraní API umožňuje uživatelům provést integraci podnikových aplikací a integrační scénáře B2B s žádným z typů aplikací ve službě App Service.</p>
<li><p>S Logic Apps, můžete nyní obchodní procesy můžete automatizovat pomocí prostředí visual návrhu vytváření pracovních postupů.</p></td>
</tr>
</tbody>
</table>

Chcete-li získat další informace, navštivte [dokumentace služby App Service](https://azure.microsoft.com/documentation/services/app-service/).

