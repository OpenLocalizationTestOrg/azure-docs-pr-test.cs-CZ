---
title: "Správa Azure CDN ukládání do mezipaměti zásady ve službě Azure Media Services | Microsoft Docs"
description: "Naučte se spravovat Azure CDN ukládání do mezipaměti zásady ve službě Azure Media Services."
services: media-services,cdn
documentationcenter: .NET
author: juliako
manager: erikre
editor: 
ms.assetid: be33aecc-6dbe-43d7-a056-10ba911e0e94
ms.service: media-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/04/2017
ms.author: juliako
ms.openlocfilehash: bcb5a6e118c779e7e13ba8472d5bba510cddeb12
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/08/2018
---
# <a name="manage-azure-cdn-caching-policy-in-azure-media-services"></a>Správa Azure CDN ukládání do mezipaměti zásady ve službě Azure Media Services
Azure Media Services poskytuje adaptivní streamování a progresivní stahování založený na HTTP. Streamování na základě protokolu HTTP je vysoce škálovatelná výhod ukládání do mezipaměti v CDN vrstvy a proxy server, stejně jako ukládání do mezipaměti na straně klienta. Koncové body streamování poskytuje obecné možnosti streamování a také konfigurace pro mezipaměť hlavičky protokolu HTTP. Koncové body streamování nastaví HTTP Cache-Control: maximální doba a hlavičky Expires. Můžete získat další informace o mezipaměti hlavičky protokolu HTTP z [adrese W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

## <a name="default-caching-headers"></a>Výchozí záhlaví ukládání do mezipaměti
Ve výchozím nastavení použít – koncové body streamování hlavičky mezipaměť 3 dne pro streamování na vyžádání dat (fragmenty samotný mediální bloků dat) a manifest(playlist). Pro živé streamování koncových bodů streamování použít záhlaví mezipaměť 3 dne pro data (fragmenty samotný mediální bloků dat) a 2 sekund mezipaměti hlavičku pro žádosti o manifest(playlist). Když program za provozu se změní na vyžádání (za provozu archiv), potom použijte mezipaměti hlavičky streamování na vyžádání.

## <a name="azure-cdn-integration"></a>Integrace se službou Azure CDN
Služba Azure Media Services poskytuje [integrované CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) pro streamování koncové body. Hlavičky cache-control platí stejným způsobem jako koncových bodů streamování na CDN povolené koncových bodů streamování. Azure CDN používá koncový bod streamování nakonfigurované hodnot mezipaměti můžete definovat dobu životnosti interně v mezipaměti objektů a také používá tato hodnota k nastavení doručování hlaviček mezipaměti. Při používání CDN povoleno koncových bodů streamování se nedoporučuje pro nastavení hodnot malé mezipaměti. Malé hodnoty nastavení snížit výkon a snížit výhodou CDN. Nastavení mezipaměti hlavičky menší než 600 sekund pro CDN povoleno koncových bodů streamování není povoleno.

> [!IMPORTANT]
>Azure Media Services je kompletní integrovaná s Azure CDN. S jedním kliknutím můžete integrovat všechny dostupné Azure CDN zprostředkovatele (Akamai a Verizon) na váš koncový bod streamování, včetně produktů CDN Standard a Premium. Další informace najdete v tématu to [oznámení](https://azure.microsoft.com/blog/standardstreamingendpoint/).
> 
> Poplatků za data z datových proudů koncového bodu CDN získá pouze zakázáno, pokud je povoleno CDN prostřednictvím rozhraní API pro koncový bod streamování nebo pomocí oddíl koncového bodu streamování portálu Azure. Ruční integrace nebo přímo vytváření koncový bod CDN pomocí rozhraní API CDN nebo portálu části není zakázat poplatků za data.

## <a name="configuring-cache-headers-with-azure-media-services"></a>Konfigurace mezipaměti hlavičky službou Azure Media Services
Portál Azure nebo rozhraní API služby Azure Media Services můžete použít ke konfiguraci hodnoty hlavičky mezipaměti.

1. Konfigurace mezipaměti hlaviček pomocí portálu Azure, najdete v tématu [jak spravovat koncové body streamování](../media-services/media-services-portal-manage-streaming-endpoints.md) část konfigurace koncového bodu streamování.
2. Rozhraní REST API služby Azure Media Services [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Azure Media Services .NET SDK [StreamingEndpointCacheControl vlastnosti](http://go.microsoft.com/fwlink/?LinkId=615302).

## <a name="cache-configuration-precedence-order"></a>Pořadí priorit konfigurace mezipaměti
1. Azure Media Services nakonfigurovat mezipaměť hodnota přepíše výchozí hodnotu.
2. Pokud není žádná ruční konfigurace, budou použity výchozí hodnoty.
3. 2 sekundy mezipamětí výchozí hlavičky platí pro živé streamování manifest(playlist) bez ohledu na média Azure nebo Azure Storage konfigurací a přepsání této hodnoty není k dispozici.

