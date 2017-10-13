---
title: "CDN Azure – Přehled | Dokumentace Microsoftu"
description: "Zjistěte, co je Síť pro doručování obsahu (CDN) Azure, a jak ji používat k doručování širokopásmového obsahu díky ukládání objektů blob a statického obsahu do mezipaměti."
services: cdn
documentationcenter: 
author: smcevoy
manager: akucer
editor: 
ms.assetid: 866e0c30-1f33-43a5-91f0-d22f033b16c6
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/08/2017
ms.author: v-semcev
ms.openlocfilehash: 909c4dc3feaeaedf56ecacc78f4b7e0e15d98875
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-the-azure-content-delivery-network-cdn"></a>Přehled služby Content Delivery Network (CDN) Azure
> [!NOTE]
> Tento dokument popisuje, co je služba Content Delivery Network (CDN) Azure, jak funguje, a funkce všech produktů CDN Azure.  Chcete-li tyto informace přeskočit a rovnou přejít ke kurzu jak vytvořit koncový bod CDN, přejděte na [Používání CDN Azure](cdn-create-new-endpoint.md).  Chcete-li zobrazit aktuální seznam umístění uzlů CDN, přejděte na [Umístění POP v síti CDN](cdn-pop-locations.md).
> 
> 

Služba Content Delivery Network (CDN) Azure ukládá statický webový obsah do mezipaměti na strategicky umístěných místech, a tak poskytuje maximální propustnost pro doručování obsahu uživatelům.  CDN nabízí vývojářům globální řešení pro doručování širokopásmového obsahu díky ukládání obsahu do mezipaměti na fyzických uzlech po celém světě. 

Mezi výhody používání CDN k ukládání webových prostředků do mezipaměti patří:

* Lepší výkon a uživatelské prostředí pro koncové uživatele, zvláště při používání aplikací, které k načtení obsahu vyžadují vícenásobný přenos.
* Velké škálování pro lepší zvládání náhlého vysokého zatížení, například v případě uvedení produktu na trh.
* Díky distribuci uživatelských požadavků a poskytování obsahu ze serverů Edge dochází k nižšímu přenosu ke zdroji.

## <a name="how-it-works"></a>Jak to funguje
![Přehled CDN](./media/cdn-overview/cdn-overview.png)

1. Uživatel (Alice) požaduje soubor (také označovaný jako prostředek) pomocí adresy URL se speciálním názvem domény, například `<endpointname>.azureedge.net`.  DNS přesměruje požadavek na nejvýkonnější umístění POP (Point of Presence).  To je obvykle POP, který je geograficky nejblíže uživatele.
2. Pokud servery Edge v POP nemají soubor v mezipaměti, server Edge si ho vyžádá ze zdroje.  Zdrojem může být webová aplikace Azure, cloudová služba Azure, účet Azure Storage nebo jakýkoli veřejně přístupný webový server.
3. Zdroj vrátí soubor serveru Edge včetně volitelných hlaviček protokolu HTTP, které popisují hodnotu TTL (Time to Live) souboru.
4. Server Edge soubor uloží do mezipaměti a vrátí ho původnímu žadateli (Alici).  Soubor zůstane v mezipaměti na serveru Edge, dokud nevyprší platnost hodnoty TTL.  Pokud zdroj nezadal hodnotu TTL, je výchozí hodnota TTL sedm dní.
5. Další uživatelé mohou následně požadovat stejný soubor pomocí stejné adresy URL a budou přesměrováni na stejný POP.
6. Pokud hodnota TTL souboru nevypršela, vrátí server Edge soubor z mezipaměti.  To má za následek rychlejší a rychleji reagující uživatelské prostředí.

## <a name="azure-cdn-features"></a>Funkce Azure CDN
Existují tři produkty Azure CDN: **Azure CDN Standard od společnosti Akamai**, **Azure CDN Standard od společnosti Verizon** a **Azure CDN Premium od společnosti Verizon**.  V následující tabulce jsou uvedené funkce, které jsou k dispozici u každého produktu.

|  | Akamai Standard | Verizon Standard | Verizon Premium |
| --- | --- | --- | --- |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  __Funkce a optimalizace výkonu__ |
| [Akcelerace dynamického webu](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration) | **&amp;#x2713;**  | **&amp;#x2713;** | **&amp;#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  [Akcelerace dynamického webu – adaptivní komprese obrázků](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-akamai-only) | **&amp;#x2713;**  |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  [Akcelerace dynamického webu – předběžné načtení objektů](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-akamai-only) | **&amp;#x2713;**  |  |  |
| [Optimalizace streamování videa](https://docs.microsoft.com/azure/cdn/cdn-media-streaming-optimization) | **&amp;#x2713;**  | \* |  \* |
| [Optimalizace velkých souborů](https://docs.microsoft.com/azure/cdn/cdn-large-file-optimization) | **&amp;#x2713;**  | \* |  \* |
| [Vyrovnávání zatížení globálního serveru (GSLB)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| [Rychlé vyprázdnění](cdn-purge-endpoint.md) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| [Předběžné načítání prostředku](cdn-preload-endpoint.md) | |**&amp;#x2713;** |**&amp;#x2713;** |
| [Ukládání řetězce dotazu do mezipaměti](cdn-query-string.md) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| Duální sada protokolů IPv4/IPv6 |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| [Podpora HTTP/2](cdn-http2.md) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  __Zabezpečení__ |
| Podpora HTTPS s koncovými body CDN |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| [HTTPS pro vlastní doménu](cdn-custom-ssl.md) | |**&amp;#x2713;** |**&amp;#x2713;** |
| [Podpora vlastních názvů domén](cdn-map-content-to-custom-domain.md) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| [Geografická filtrování](cdn-restrict-access-by-country.md) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| [Ověření tokenu](cdn-token-auth.md)|  |  |**&amp;#x2713;**| 
| [Ochrana proti útoku DDoS](https://www.us-cert.gov/ncas/tips/ST04-015) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  __Analýzy a generování sestav__ |
| [Základní analýza](cdn-analyze-usage-patterns.md) | **&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| [Rozšířené sestavy HTTP](cdn-advanced-http-reports.md) | | |**&amp;#x2713;** |
| [Statistiky v reálném čase](cdn-real-time-stats.md) | | |**&amp;#x2713;** |
| [Výstrahy v reálném čase](cdn-real-time-alerts.md) | | |**&amp;#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  __Snadné použití__ |
| Snadná integrace se službami Azure, jako jsou [Storage](cdn-create-a-storage-account-with-cdn.md), [Cloud Services](cdn-cloud-service-with-cdn.md), [Web Apps](../app-service/app-service-web-tutorial-content-delivery-network.md) a [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| Správa prostřednictvím [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md) nebo [prostředí PowerShellu](cdn-manage-powershell.md) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| [Přizpůsobitelný modul doručování obsahu založený na pravidlech](cdn-rules-engine.md) | | |**&amp;#x2713;** |
| Nastavení mezipaměti nebo hlaviček (pomocí [stroje pravidel](cdn-rules-engine.md)) | | |**&amp;#x2713;** |
| Přesměrování nebo přepsání adresy URL (pomocí [stroje pravidel](cdn-rules-engine.md)) | | |**&amp;#x2713;** |
| Pravidla mobilních zařízení (pomocí [stroje pravidel](cdn-rules-engine.md)) | | |**&amp;#x2713;** |

\* Verizon podporuje přímé doručování velkých souborů a médií prostřednictvím Obecného doručování webu.


> [!TIP]
> Víte o funkci, kterou byste uvítali v CDN Azure?  [Sdělte nám svůj názor](https://feedback.azure.com/forums/169397-cdn)! 
> 
> 

## <a name="next-steps"></a>Další kroky
Chcete-li začít s CDN, podívejte se na [Používání Azure CDN](cdn-create-new-endpoint.md).

Pokud jste stávající zákazník CDN, můžete nyní spravovat koncové body CDN prostřednictvím portálu [Microsoft Azure Portal](https://portal.azure.com) nebo [PowerShellu](cdn-manage-powershell.md).

Chcete-li vidět CDN v akci, podívejte se na [video z naší konference Build 2016](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/).

Přečtěte si informace o automatizaci Azure CDN s [.NET](cdn-app-dev-net.md) nebo [Node.js](cdn-app-dev-node.md).

Informace o cenách naleznete v tématu [Ceny CDN](https://azure.microsoft.com/pricing/details/cdn/).

