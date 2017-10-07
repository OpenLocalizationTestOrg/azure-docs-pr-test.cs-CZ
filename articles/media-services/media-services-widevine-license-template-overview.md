---
title: "Přehled šablonu licence aaaWidevine | Microsoft Docs"
description: "Toto téma poskytuje přehled o šablonu licence Widevine, který používá licence na Widevine tooconfigure."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0e6f1f05-7ed6-4ed6-82a0-0cc2182b075a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 67a6ae38cf3d3c21e1b7282aef15f79b21776414
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="widevine-license-template-overview"></a>Přehled šablonu licence Widevine
## <a name="overview"></a>Přehled
Azure Media Services nyní umožňuje licence na Widevine tooconfigure a požadavek. Když player hello koncový uživatel pokusí tooplay Widevine chráněný obsah, žádost je odeslané toohello licence doručení služby tooobtain licenci. Pokud hello licenční služby schválí požadavek hello, vystavuje hello licenci, která je klient odeslané toohello a může být použité toodecrypt a play hello zadaný obsah.

Požadavek na licenční Widevine je naformátován jako zprávu JSON.  

>[!NOTE]
> Můžete vybrat toocreate zprávu prázdný s žádné hodnoty jenom "{}" a vytvoří se šablona licence s všechny výchozí hodnoty. Výchozí Hello funguje pro většině případů. Například pro MS na základě scénáře doručení licence, které by měl vždycky být výchozí. Pokud potřebujete tooset hello "zprostředkovatele" a "content_id" hodnoty, se musí shodovat zprostředkovatele Google Widevine pověření.

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

## <a name="json-message"></a>Zpráva JSON
| Name (Název) | Hodnota | Popis |
| --- | --- | --- |
| datová část |Řetězec s kódováním base64 |požadavek na licenční Hello odeslány klientem. |
| content_id |Řetězec s kódováním base64 |Identifikátor používá tooderive KeyId(s) a obsahu klíče pro každý content_key_specs.track_type. |
| Zprostředkovatel |Řetězec |Použít toolook klíčů k obsahu a zásady. Pokud MS doručení klíče se používá pro doručování licence Widevine, tento parametr je ignorován. |
| název_zásad |Řetězec |Název zásady dříve zaregistrovaný. Nepovinné |
| allowed_track_types |výčet |SD_ONLY nebo SD_HD. Ovládací prvky, které obsahu klíče by měl být součástí licenci |
| content_key_specs |pole JSON struktur, najdete v části **obsahu specifikace klíče** níže |Přesnější možnosti řízení na který obsah klíče tooreturn. Podrobnosti najdete v obsahu specifikace klíče níže.  Lze zadat pouze jeden z allowed_track_types a content_key_specs. |
| use_policy_overrides_exclusively |Logická hodnota. hodnotu true nebo false |Pomocí určeného policy_overrides atributy zásad a vynechejte všechny dříve uložené zásad. |
| policy_overrides |Struktura JSON, najdete v části **zásada přepíše** níže |Nastavení zásad pro tuto licenci.  V případě hello tento prostředek má předem definované zásady, budou použity tyto zadané hodnoty. |
| session_init |Struktura JSON, najdete v části **inicializace relace** níže |Volitelná data předána toolicense. |
| parse_only |Logická hodnota. hodnotu true nebo false |požadavek na licenční Hello je analyzována, ale je vydán žádná licence. Požadavek na licenční hello hodnot formuláře jsou však vráceny v odpovědi hello. |

## <a name="content-key-specs"></a>Obsahu specifikace klíče
Pokud existují předem existující zásady, neexistuje žádný toospecify nutné žádné hello hodnoty v hello obsahu specifikace klíče.  Hello existující zásady přidružené k tomuto obsahu bude ochrany výstup hello použité toodetermine například HDCP a CGMS.  Pokud už existující zásady není zaregistrována hello Widevine licenční Server, můžete poskytovateli obsahu hello vložit hello hodnoty do požadavek na licenční hello.   

Každý content_key_specs je třeba zadat pro všechny sleduje, bez ohledu na to možnost use_policy_overrides_exclusively hello. 

| Name (Název) | Hodnota | Popis |
| --- | --- | --- |
| content_key_specs. track_type |Řetězec |Název typu sledování. Pokud content_key_specs je určený v požadavku hello licence, ujistěte se, že toospecify, které sledují všechny typy explicitně. Selhání toodo proto způsobí selhání tooplayback posledních 10 sekund. |
| content_key_specs  <br/> security_level |UInt32 |Definuje požadavky na klienta odolnosti pro přehrávání. <br/> 1 - softwarový whitebox kryptografických je povinný. <br/> 2 – software kryptografických a zakódovaná dekodér je vyžadován. <br/> 3 - hello materiálech a kryptografické operace s klíčem, je nutné provést v prostředí zálohovány důvěryhodné spouštěcí hardwaru. <br/> 4 - hello kryptografických a dekódování obsahu, je nutné provést v prostředí zálohovány důvěryhodné spouštěcí hardwaru.  <br/> 5 - hello kryptografických, dekódování a všechny zpracování hello média (komprimovaným a nekomprimovaným) musí být zpracován zálohovány důvěryhodné spouštěcí prostředí hardwaru. |
| content_key_specs <br/> required_output_protection.hDC |String – jeden z: HDCP_V2 HDCP_NONE, HDCP_V1, |Označuje, zda je vyžadují HDCP |
| content_key_specs <br/>key |formátu Base64. <br/>řetězec s kódováním |Obsahu toouse klíče pro toto sledování. -Li zadána, hello track_type nebo key_id je vyžadován.  Tato možnost umožňuje poskytovateli obsahu hello tooinject klíč obsahu hello tento stopy místo Widevine licenční server generovat nebo vyhledat klíč. |
| content_key_specs.key_id |Kódováním base64, pomocí řetězce binární, 16 bajtů |Jedinečný identifikátor pro klíč hello. |

## <a name="policy-overrides"></a>Přepsání zásad
| Name (Název) | Hodnota | Popis |
| --- | --- | --- |
| policy_overrides. can_play |Logická hodnota. hodnotu true nebo false |Určuje, že přehrávání obsahu hello je povolený. Výchozí hodnota je false. |
| policy_overrides. can_persist |Logická hodnota. hodnotu true nebo false |Označuje, že licence hello může být trvalý toonon volatile úložiště pro použití v offline režimu. Výchozí hodnota je false. |
| policy_overrides. can_renew |Logická hodnota PRAVDA nebo NEPRAVDA |Označuje, zda je povoleno obnovení tuto licenci. V případě hodnoty true hello období platnosti licence hello můžete rozšířit pomocí prezenčního signálu. Výchozí hodnota je false. |
| policy_overrides. license_duration_seconds |Int64 |Označuje hello časový interval pro tuto konkrétní licenci. Hodnota 0 značí, že neexistuje žádné omezení toohello trvání. Výchozí hodnota je 0. |
| policy_overrides. rental_duration_seconds |Int64 |Označuje hello časové okno při přehrávání je povoleno. Hodnota 0 značí, že neexistuje žádné omezení toohello trvání. Výchozí hodnota je 0. |
| policy_overrides. playback_duration_seconds |Int64 |zobrazení okna času po zahájení v rámci licence trvání hello Hello. Hodnota 0 značí, že neexistuje žádné omezení toohello trvání. Výchozí hodnota je 0. |
| policy_overrides. renewal_server_url |Řetězec |Všechny požadavky prezenčního signálu (obnovení) pro tuto licenci musí směřovat toohello zadaná adresa URL. Toto pole je použít jen v případě can_renew má hodnotu true. |
| policy_overrides. renewal_delay_seconds |Int64 |Kolik sekund po license_start_time, než je první pokus o obnovení. Toto pole je použít jen v případě can_renew má hodnotu true. Výchozí hodnota je 0. |
| policy_overrides. renewal_retry_interval_seconds |Int64 |Určuje hello zpoždění v sekundách mezi požadavky na obnovení certifikátu další licence, v případě selhání. Toto pole je použít jen v případě can_renew má hodnotu true. |
| policy_overrides. renewal_recovery_duration_seconds |Int64 |okno Hello dobu, ve kterém je povolen přehrávání toocontinue při obnovení je pokus o, ještě neúspěšné kvůli toobackend problémy s hello licenční server. Hodnota 0 značí, že neexistuje žádné omezení toohello trvání. Toto pole je použít jen v případě can_renew má hodnotu true. |
| policy_overrides. renew_with_usage |Logická hodnota PRAVDA nebo NEPRAVDA |Označuje, že se zasílá hello licencích pro obnovení, při spuštění využití. Toto pole je použít jen v případě can_renew má hodnotu true. |

## <a name="session-initialization"></a>Inicializace relace
| Name (Název) | Hodnota | Popis |
| --- | --- | --- |
| provider_session_token |Řetězec s kódováním base64 |Tento token relace je předán zpět v hello licenci a bude existovat v dalších prodloužení.  Hello relace tokenu neuchovává kromě relací. |
| provider_client_token |Řetězec s kódováním base64 |Klient tokenu toosend zpět v odpovědi licence hello.  Pokud požadavek na licenční hello obsahuje token klienta, je tato hodnota ignorována. token klienta Hello se uchová nad rámec relace licence. |
| override_provider_client_token |Logická hodnota. hodnotu true nebo false |Pokud požadavek na licenční hodnotu false a hello obsahuje token klienta, použijte hello tokenu z požadavku hello i v případě, že token klienta byla zadána v této struktuře.  V případě hodnoty true vždy používejte token hello zadané v této struktuře. |

## <a name="configure-your-widevine-licenses-using-net-types"></a>Nakonfigurujte své licence Widevine pomocí typy .NET
Služba Media Services poskytuje rozhraní API .NET, která umožňují nakonfigurujte své licence Widevine. 

### <a name="classes-as-defined-in-hello-media-services-net-sdk"></a>Třídy definované v hello sady Media Services .NET SDK
Hello následují definice hello z těchto typů.

    public class WidevineMessage
    {
        public WidevineMessage();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

### <a name="example"></a>Příklad
Následující příklad ukazuje, jak Hello toouse rozhraní API technologie .NET tooconfigure jednoduché licenci Widevine.

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Viz také
[Použití běžného šifrování PlayReady nebo Widevine dynamický](media-services-protect-with-drm.md)

