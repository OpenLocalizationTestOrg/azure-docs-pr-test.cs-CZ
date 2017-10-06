---
title: "aaaManage Azure CDN zásady ukládání do mezipaměti ve službě Azure Media Services | Microsoft Docs"
description: "Zjistěte, jak toomanage Azure CDN zásady ukládání do mezipaměti ve službě Azure Media Services."
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
ms.openlocfilehash: 4c7ee2922fcbb5727e10fbe44fc6e61b79f6917c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-cdn-caching-policy-in-azure-media-services"></a>Správa Azure CDN ukládání do mezipaměti zásady ve službě Azure Media Services
Azure Media Services poskytuje adaptivní streamování a progresivní stahování založený na HTTP. Streamování na základě protokolu HTTP je vysoce škálovatelná výhod ukládání do mezipaměti v proxy a CDN vrstvách a také použití mezipaměti klienta. Koncové body streamování poskytuje obecné možnosti streamování a také konfigurace pro mezipaměť hlavičky protokolu HTTP. Koncové body streamování nastaví HTTP Cache-Control: maximální doba a hlavičky Expires. Můžete získat další informace o mezipaměti hlavičky protokolu HTTP z [adrese W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

## <a name="default-caching-headers"></a>Výchozí záhlaví ukládání do mezipaměti
Ve výchozím nastavení použít – koncové body streamování hlavičky mezipaměť 3 dne pro streamování na vyžádání dat (fragmenty samotný mediální bloků dat) a manifest(playlist). Pro živé streamování koncových bodů streamování použít záhlaví mezipaměť 3 dne pro data (fragmenty samotný mediální bloků dat) a 2 sekund mezipaměti hlavičku pro žádosti o manifest(playlist). Je-li živou program změní na vyžádání tooon (archiv za provozu) mezipaměti hlavičky streamování na vyžádání se použije.

## <a name="azure-cdn-integration"></a>Integrace se službou Azure CDN
Služba Azure Media Services poskytuje [integrované CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) pro streamování koncové body. Hlavičky cache-control provede v hello stejný způsobem jako streamování tooCDN koncových bodů povoleno koncových bodů streamování. Azure CDN používá interně streamování koncovým bodem nakonfigurovaným mezipaměti hodnoty toodefine hello životnosti čas hello do mezipaměti objektů a také používá tuto hodnotu tooset hello doručení mezipaměti hlavičky. Při používání CDN povoleno koncových bodů streamování není doporučeno tooset malé mezipaměti hodnoty. Nastavení malé hodnot snížit hello výkon a snížit hello výhodou CDN. Není povoleno tooset mezipaměti hlavičky menší než 600 sekund pro CDN s povoleným koncových bodů streamování.

> [!IMPORTANT]
>Azure Media Services je kompletní integrovaná s Azure CDN. Jediným kliknutím můžete integrovat všechny hello k dispozici Azure CDN zprostředkovatelé (Akamai a Verizon) tooyour včetně produktů CDN Standard a Premium koncový bod streamování. Další informace najdete v tématu to [oznámení](https://azure.microsoft.com/blog/standardstreamingendpoint/).
> 
> Poplatků za data z datových proudů tooCDN koncový bod získá pouze zakázáno, je-li hello CDN povoleno přes rozhraní API pro koncový bod streamování nebo pomocí portálu Azure management portal streamování oddíl koncového bodu. Ruční integrace nebo přímo vytváření koncový bod CDN pomocí rozhraní API CDN nebo portálu části zakázat, nebude poplatků za data hello.

## <a name="configuring-cache-headers-with-azure-media-services"></a>Konfigurace mezipaměti hlavičky službou Azure Media Services
Můžete použít portál pro správu Azure nebo hodnoty hlavičky mezipaměti tooconfigure rozhraní API služby Azure Media Services.

1. hlavičky mezipaměti tooconfigure pomocí správy portálu naleznete příliš[jak koncové body streamování tooManage](../media-services/media-services-portal-manage-streaming-endpoints.md) část konfigurace hello Streaming koncový bod.
2. Rozhraní REST API služby Azure Media Services [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Azure Media Services .NET SDK [StreamingEndpointCacheControl vlastnosti](http://go.microsoft.com/fwlink/?LinkId=615302).

## <a name="cache-configuration-precedence-order"></a>Pořadí priorit konfigurace mezipaměti
1. Azure Media Services nakonfigurovat mezipaměť hodnota přepíše výchozí hodnotu.
2. Pokud není žádná ruční konfigurace, použije se výchozí hodnoty.
3. Ve výchozím nastavení je 2 sekund mezipaměti hlavičky platí toolive streamování manifest(playlist) bez ohledu na média Azure nebo Azure Storage konfigurací a přepsáním této hodnoty nedostupný.

