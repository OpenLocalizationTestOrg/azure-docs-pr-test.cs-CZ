---
title: "aaaAzure přehled CDN | Microsoft Docs"
description: "Zjistěte, jaké hello je Azure sítě pro doručování obsahu (CDN) a jak toouse ho toodeliver širokopásmového obsahu pomocí ukládání do mezipaměti objektů BLOB a statického obsahu."
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
ms.openlocfilehash: e0230a6e107969b845985f2f4d357bf93cd40d42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-hello-azure-content-delivery-network-cdn"></a>Přehled hello Azure Content Delivery Network (CDN)
> [!NOTE]
> Tento dokument popisuje, jaké hello je služba Content Delivery Network (CDN) Azure, jak to funguje a hello funkce všech produktů Azure CDN.  Pokud chcete tyto informace tooskip a přejděte rovnou tooa kurz o tom, najdete v části toocreate koncový bod CDN [používání CDN Azure](cdn-create-new-endpoint.md).  Pokud chcete toosee seznam aktuálních umístění uzlů CDN, najdete v části [umístění POP CDN Azure](cdn-pop-locations.md).
> 
> 

Hello Azure sítě pro doručování obsahu (CDN) ukládá do mezipaměti na strategicky umístěných místech tooprovide maximální propustnost pro doručování obsahu toousers statický webový obsah.  Hello CDN nabízí vývojářům globální řešení pro doručování širokopásmového obsahu pomocí ukládání do mezipaměti obsah hello na fyzických uzlech napříč hello, world. 

Příklady výhod Hello pomocí hello CDN toocache webových prostředků:

* Lepší výkon a uživatelské prostředí pro koncové uživatele, zejména v případě, že pomocí aplikací, kde jsou vícenásobný potřeby tooload obsah.
* Velké škálování toobetter zvládání náhlého vysokého zatížení, jako při spuštění hello produktu spusťte událost.
* Distribuci uživatelských požadavků a poskytování obsahu ze serverů edge, méně přenosy se odesílají toohello původu.

## <a name="how-it-works"></a>Jak to funguje
![Přehled CDN](./media/cdn-overview/cdn-overview.png)

1. Uživatel (Alice) požaduje soubor (také označovaný jako prostředek) pomocí adresy URL se speciálním názvem domény, například `<endpointname>.azureedge.net`.  DNS přesměruje požadavek hello toohello nejlépe provádění Point of Presence (POP) umístění.  To je obvykle POP, který je geograficky nejblíže uživatele toohello hello.
2. Pokud servery edge hello v hello POP nemají hello soubor v mezipaměti, hello edge server požádá o soubor hello z počátku hello.  původ Hello může být webová aplikace Azure, Cloudová služba Azure, účet úložiště Azure nebo jakékoli veřejně přístupný webový server.
3. Zdroj Hello vrátí hello souboru toohello serveru edge včetně volitelných hlaviček protokolu HTTP, které popisují hello souboru Time-to-Live (TTL).
4. Hello edge server ukládá do mezipaměti hello souboru a vrátí hello souboru toohello původnímu žadateli (Alici).  soubor Hello zůstává v mezipaměti na serveru edge hello do vypršení platnosti hello TTL.  Pokud hello zdroj nezadal hodnotu TTL, hello výchozí hodnota TTL je sedm dní.
5. Další uživatelé, kteří mohou žádost hello stejný soubor pomocí stejné adresy URL a také může být směrovanou toothat stejný POP.
6. Pokud hello TTL hello souboru nevypršela, vrátí hello edge server hello souboru z mezipaměti hello.  To má za následek rychlejší a rychleji reagující uživatelské prostředí.

## <a name="azure-cdn-features"></a>Funkce Azure CDN
Existují tři produkty Azure CDN: **Azure CDN Standard od společnosti Akamai**, **Azure CDN Standard od společnosti Verizon** a **Azure CDN Premium od společnosti Verizon**.  Hello následující tabulka uvádí funkce hello k dispozici u každého produktu.

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
| Snadná integrace se službami Azure, jako jsou [Storage](cdn-create-a-storage-account-with-cdn.md), [Cloud Services](cdn-cloud-service-with-cdn.md), [Web Apps](../app-service-web/app-service-web-tutorial-content-delivery-network.md) a [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| Správa prostřednictvím [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md) nebo [prostředí PowerShellu](cdn-manage-powershell.md) |**&amp;#x2713;** |**&amp;#x2713;** |**&amp;#x2713;** |
| [Přizpůsobitelný modul doručování obsahu založený na pravidlech](cdn-rules-engine.md) | | |**&amp;#x2713;** |
| Nastavení mezipaměti nebo hlaviček (pomocí [stroje pravidel](cdn-rules-engine.md)) | | |**&amp;#x2713;** |
| Přesměrování nebo přepsání adresy URL (pomocí [stroje pravidel](cdn-rules-engine.md)) | | |**&amp;#x2713;** |
| Pravidla mobilních zařízení (pomocí [stroje pravidel](cdn-rules-engine.md)) | | |**&amp;#x2713;** |

\* Verizon podporuje přímé doručování velkých souborů a médií prostřednictvím Obecného doručování webu.


> [!TIP]
> Je k dispozici funkci chcete toosee v Azure CDN?  [Sdělte nám svůj názor](https://feedback.azure.com/forums/169397-cdn)! 
> 
> 

## <a name="next-steps"></a>Další kroky
tooget začít s CDN, najdete v části [používání CDN Azure](cdn-create-new-endpoint.md).

Pokud jste stávající zákazník CDN, můžete nyní spravovat koncové body CDN prostřednictvím hello [portálu Microsoft Azure](https://portal.azure.com) nebo s [prostředí PowerShell](cdn-manage-powershell.md).

toosee hello CDN v akci, podívejte se na hello [video z naší konference Build 2016](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/).

Zjistěte, jak tooautomate Azure CDN s [.NET](cdn-app-dev-net.md) nebo [Node.js](cdn-app-dev-node.md).

Informace o cenách naleznete v tématu [Ceny CDN](https://azure.microsoft.com/pricing/details/cdn/).

