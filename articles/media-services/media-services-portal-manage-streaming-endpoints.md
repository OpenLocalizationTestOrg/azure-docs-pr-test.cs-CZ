---
title: "Správa koncových bodů streamování pomocí portálu Azure | Microsoft Docs"
description: "Toto téma ukazuje, jak spravovat koncových bodů streamování pomocí portálu Azure."
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: cfowler
editor: 
ms.assetid: bb1aca25-d23a-4520-8c45-44ef3ecd5371
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 797dced6c3e2525730afa29987259cb9b435ba66
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-streaming-endpoints-with-the-azure-portal"></a>Správa koncových bodů streamování pomocí portálu Azure

Toto téma ukazuje, jak pomocí portálu Azure ke správě koncových bodů streamování. 

>[!NOTE]
>Projděte si [přehled](media-services-streaming-endpoints-overview.md) tématu. 

Informace o tom, jak škálování koncový bod streamování najdete v tématu [to](media-services-portal-scale-streaming-endpoints.md) tématu.

## <a name="start-managing-streaming-endpoints"></a>Zahájení správy koncových bodů streamování 

Pokud chcete spustit, Správa koncových bodů streamování pro váš účet, postupujte takto.

1. Na webu [Azure Portal](https://portal.azure.com/) zvolte účet Azure Media Services.
2. V **nastavení** vyberte **koncové body streamování**.
   
    ![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

> [!NOTE]
> Fakturuje se jenom, když je v běžícím stavu je koncový bod streamování.

## <a name="adddelete-a-streaming-endpoint"></a>Přidání nebo odstranění koncového bodu streamování

>[!NOTE]
>Nelze odstranit výchozí koncový bod streamování.

K přidání nebo odstranění koncového bodu streamování pomocí portálu Azure, postupujte takto:

1. Chcete-li přidat koncový bod streamování, klikněte na tlačítko **+ koncový bod** v horní části stránky. 

    Pokud chcete mít jiný sítím CDN nebo CDN a přímý přístup můžete několik koncových bodů streamování.

2. Pokud chcete odstranit koncový bod streamování, stiskněte **odstranit** tlačítko.      
3. Klikněte **spustit** tlačítko Spustit koncový bod streamování.
   
    ![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)


## <a id="configure_streaming_endpoints"></a>Konfigurace koncového bodu streamování
Koncový bod streamování, můžete nakonfigurovat následující vlastnosti:

* Řízení přístupu
* Ovládací prvek mezipaměti
* Různé zásady přístupu lokality

Podrobné informace o těchto vlastnostech najdete v tématu [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).

Koncový bod streamování můžete nakonfigurovat následujícím způsobem:

1. Vyberte koncový bod streamování, kterou chcete konfigurovat.
2. Klikněte na tlačítko **nastavení**.

Následuje stručný popis polí.

![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)

1. Maximální mezipaměť zásad: slouží ke konfiguraci Životnost mezipaměti pro prostředky obsluhovat prostřednictvím tento koncový bod streamování. Pokud není nastavena žádná hodnota, použije se výchozí hodnota. Výchozí hodnoty můžete definovat také přímo v úložišti Azure. Pokud Azure CDN je povolená pro koncový bod streamování, nastavíte nesmí hodnota zásady mezipaměti na menší než 600 sekund.  
2. Povolené IP adresy: slouží k zadání IP adresy, které bude mít možnost připojení k publikované koncový bod streamování. Pokud žádné IP adresy zadané, bude moct připojit jakékoli IP adresy. IP adresy lze zadat jako jednu IP adresu (například (10.0.0.1), rozsah IP pomocí IP adresy a masky podsítě CIDR (například (10.0.0.1/22) nebo rozsah IP adres pomocí IP adresy a masky podsítě s tečkou (například 10.0.0.1 ' () 255.255.255.0) ").
3. Konfigurace pro ověřování záhlaví Akamai podpisu: umožňuje určit, jak konfigurovat žádosti o ověření podpisu hlavičky ze serverů Akamai. Vypršení platnosti je ve formátu UTC.

## <a name="scale-your-premium-streaming-endpoint"></a>Škálovat vaše Premium koncový bod streamování

Další informace najdete v [tomto](media-services-portal-scale-streaming-endpoints.md) tématu.

## <a id="enable_cdn"></a>Povolit integraci Azure CDN

Když vytvoříte nový účet, je výchozí streamování koncového bodu Azure CDN integrace povolena ve výchozím nastavení.

Pokud budete později chtít zapnout/vypnout CDN, koncový bod streamování, musí být ve **zastavena** stavu. Může to trvat až 2 hodiny pro integraci produktů Azure CDN tak, aby získat povolená a změny se být aktivní v rámci všech CDN POP. Však můžete spustit z koncového bodu streamování váš koncový bod streamování a datový proud bez přerušení a po dokončení integrace datový proud doručen od CDN. Zřizování období koncový bod streamování bude v **od** stavu a můžete všimnout degredad výkonu.

Integrace CDN je povolena ve všech centrech execpt dat Azure Číně a Federal Goverment oblasti.

Když je tato funkce povolená, **řízení přístupu**, **vlastní název hostitele** a **ověřování podpisu Akamai** konfigurace získá zakázána.
 
> [!IMPORTANT]
> Azure Media Services integraci s Azure CDN se implementuje na **Azure CDN společnosti Verizon** pro standardní koncové body streamování. Premium koncové body streamování pomocí se dají konfigurovat všechny **Azure CDN cenové úrovně a poskytovatelé**. Další informace o funkcích Azure CDN najdete v tématu [přehled CDN](../cdn/cdn-overview.md).
 
### <a name="additional-considerations"></a>Další aspekty

* Pokud CDN je povoleno pro koncový bod streamování, nemůže klient vyžádá obsah přímo z tohoto počátku. Pokud chcete mít možnost otestovat váš obsah s nebo bez CDN, můžete vytvořit další streamování koncový bod, který není povolen CDN.
* Vaše streamování hostitele koncového bodu zůstává stejná i poté, co povolíte CDN. Nepotřebujete žádné změny do pracovního postupu media services po povolení CDN. Například pokud vaše streamování hostitele koncového bodu strasbourg.streaming.mediaservices.windows.net, po povolení CDN, je použít přesný stejný název hostitele.
* Pro nové koncových bodů streamování můžete povolit CDN jednoduše tak, že vytvoříte nový koncový bod; pro existující koncových bodů streamování musíte nejprve zastavit koncový bod a potom povolit nebo zakázat CDN.
* Koncový bod streamování standard se dá nakonfigurovat jenom pomocí **poskytovatele CDN úrovně Standard Verizon** pomocí portálu pro správu Azure. Můžete ale povolit jiných poskytovatelů Azure CDN pomocí rozhraní REST API.

## <a name="configure-cdn-profile"></a>Konfigurace profilu CDN

Profil CDN můžete nakonfigurovat tak, že vyberete **spravovat CDN** tlačítko shora.

![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints6.png)

## <a name="next-steps"></a>Další kroky
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

