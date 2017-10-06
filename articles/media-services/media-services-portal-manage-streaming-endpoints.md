---
title: "aaaManage streamování koncové body pomocí hello portálu Azure | Microsoft Docs"
description: "Toto téma ukazuje, jak hello koncových bodů streamování toomanage pomocí portálu Azure."
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
ms.openlocfilehash: dfa9352894d37edb317a6334d7f109419deb362b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-streaming-endpoints-with-hello-azure-portal"></a>Správa koncových bodů streamování s hello portálu Azure

Toto téma ukazuje, jak toouse hello Azure portálu toomanage koncových bodů streamování. 

>[!NOTE]
>Ujistěte se, zda text hello tooreview [přehled](media-services-streaming-endpoints-overview.md) tématu. 

Informace o tom, jak tooscale hello koncový bod streamování najdete v tématu [to](media-services-portal-scale-streaming-endpoints.md) tématu.

## <a name="start-managing-streaming-endpoints"></a>Zahájení správy koncových bodů streamování 

Správa koncových bodů streamování pro váš účet toostart hello následující.

1. V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Azure Media Services.
2. V hello **nastavení** vyberte **koncové body streamování**.
   
    ![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

> [!NOTE]
> Fakturuje se jenom, když je v běžícím stavu je koncový bod streamování.

## <a name="adddelete-a-streaming-endpoint"></a>Přidání nebo odstranění koncového bodu streamování

>[!NOTE]
>nelze odstranit, Hello výchozího koncového bodu streamování.

tooadd nebo odstranění pomocí koncového bodu streamování hello portálu Azure, hello následující:

1. tooadd koncový bod streamování, klikněte na tlačítko hello **+ koncový bod** hello horní části stránky hello. 

    Pokud máte v plánu toohave můžete několik koncových bodů streamování různých sítím CDN nebo CDN a přímý přístup.

2. stiskněte klávesu toodelete koncový bod streamování, **odstranit** tlačítko.      
3. Klikněte na tlačítko hello **spustit** hello toostart tlačítko koncový bod streamování.
   
    ![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)


## <a id="configure_streaming_endpoints"></a>Konfigurace hello Streaming koncový bod
Koncový bod streamování umožňuje tooconfigure hello následující vlastnosti:

* Řízení přístupu
* Ovládací prvek mezipaměti
* Různé zásady přístupu lokality

Podrobné informace o těchto vlastnostech najdete v tématu [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).

Koncový bod streamování můžete nakonfigurovat pomocí tohoto postupu hello následující:

1. Vyberte hello chcete tooconfigure koncový bod streamování.
2. Klikněte na tlačítko **nastavení**.

Následuje stručný popis hello pole.

![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)

1. Maximální mezipaměť zásad: používané tooconfigure Životnost mezipaměti pro prostředky obsluhovat prostřednictvím tento koncový bod streamování. Pokud není nastavena žádná hodnota, použije se výchozí hello. Hello výchozí hodnoty můžete definovat také přímo v úložišti Azure. Pokud Azure CDN je povolená pro hello koncový bod streamování, neměla by nastavená hello mezipaměť zásad hodnota tooless než 600 sekund.  
2. Povolené IP adresy: používá toospecify IP adresy, které bude mít možnost tooconnect toohello publikovaná koncového bodu streamování. Pokud žádné IP adresy zadané jakékoli IP adresy by mít tooconnect. IP adresy lze zadat jako jednu IP adresu (například (10.0.0.1), rozsah IP pomocí IP adresy a masky podsítě CIDR (například (10.0.0.1/22) nebo rozsah IP adres pomocí IP adresy a masky podsítě s tečkou (například 10.0.0.1 ' () 255.255.255.0) ").
3. Konfigurace pro ověřování záhlaví Akamai podpisu: používá toospecify konfiguraci žádosti o ověření podpisu hlavičky ze serverů Akamai. Vypršení platnosti je ve formátu UTC.

## <a name="scale-your-premium-streaming-endpoint"></a>Škálovat vaše Premium koncový bod streamování

Další informace najdete v [tomto](media-services-portal-scale-streaming-endpoints.md) tématu.

## <a id="enable_cdn"></a>Povolit integraci Azure CDN

Když vytvoříte nový účet, je výchozí streamování koncového bodu Azure CDN integrace povolena ve výchozím nastavení.

Pokud chcete později toodisable/povolit hello CDN, koncový bod streamování, musí být v hello **zastavena** stavu. Může trvat až too2 hodin pro tooget integrace Azure CDN hello povolená a toobe změny hello active mezi všechny hello CDN POP. Však můžete spustit váš koncový bod streamování a datový proud bez přerušení z hello koncový bod streamování a po dokončení integrace hello hello datový proud doručen z hello CDN. Při zřizování období hello koncový bod streamování bude v **od** stavu a můžete všimnout degredad výkonu.

Integrace CDN je povolený v všechny hello dat Azure centrech execpt Číně a Federal Goverment oblastí.

Když je tato funkce povolená, hello **řízení přístupu**, **vlastní název hostitele** a **ověřování podpisu Akamai** konfigurace získá zakázána.
 
> [!IMPORTANT]
> Azure Media Services integraci s Azure CDN se implementuje na **Azure CDN společnosti Verizon** pro standardní koncové body streamování. Premium koncové body streamování pomocí se dají konfigurovat všechny **Azure CDN cenové úrovně a poskytovatelé**. Další informace o funkcích Azure CDN najdete v tématu hello [přehled CDN](../cdn/cdn-overview.md).
 
### <a name="additional-considerations"></a>Další aspekty

* Pokud CDN je povoleno pro koncový bod streamování, nemůže klient vyžádá obsah přímo z počátku hello. Pokud potřebujete hello možnost tootest obsah s nebo bez CDN, můžete vytvořit další streamování koncový bod, který není povolen CDN.
* Vaše streamování zůstane názvu hostitele koncového bodu hello stejné po povolení CDN. Nepotřebujete toomake pracovního postupu všechny změny tooyour media services po povolení CDN. Například pokud vaše streamování hostitele koncového bodu strasbourg.streaming.mediaservices.windows.net, po povolení CDN, se používá hello přesně stejný název hostitele.
* Pro nové koncových bodů streamování můžete povolit CDN jednoduše tak, že vytvoříte nový koncový bod; pro existující koncových bodů streamování budete potřebovat toofirst zastavení hello koncový bod a poté povolí nebo zakáže hello CDN.
* Koncový bod streamování standard se dá nakonfigurovat jenom pomocí **poskytovatele CDN úrovně Standard Verizon** pomocí portálu pro správu Azure. Můžete ale povolit jiných poskytovatelů Azure CDN pomocí rozhraní REST API.

## <a name="configure-cdn-profile"></a>Konfigurace profilu CDN

Profil CDN hello můžete nakonfigurovat tak, že vyberete hello **spravovat CDN** tlačítko shora hello.

![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints6.png)

## <a name="next-steps"></a>Další kroky
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

