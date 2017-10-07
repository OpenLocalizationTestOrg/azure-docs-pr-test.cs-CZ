---
title: "modul shodu podmínky pravidla aaaAzure CDN | Microsoft Docs"
description: "Referenční dokumentace pro Azure CDN pravidla shody stav motoru a funkce."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: 5e79f8f0c75a646e13bf315c492b9f2a9defc396
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cdn-rules-engine-match-conditions"></a>Stroj pravidel Azure CDN splňují podmínky
Toto téma seznamy podrobný popis tohoto hello k dispozici podmínky shody pro Azure Content Delivery Network (CDN) [stroj pravidel](cdn-rules-engine.md).

Druhá část Hello pravidla je podmínka shodu hello. Stav shody identifikuje konkrétní typy žádostí, pro které se provede sadu funkcí.

Například může být použité toofilter požadavky na obsah v konkrétních místech, generovány z konkrétní IP adresu nebo země, nebo informace hlavičky žádosti.

## <a name="always"></a>Vždy

Hello vždy shodu podmínka je navrženou tooapply výchozí sada funkcí tooall požadavky.

## <a name="device"></a>Zařízení

Hello zařízení shodu podmínka identifikuje požadavky z mobilního zařízení podle jeho vlastnosti.  Mobilní zařízení jsou detekovány prostřednictvím [WURFL](http://wurfl.sourceforge.net/).  Možnosti WURFL a jejich proměnné stroj pravidel CDN jsou uvedeny níže.

> [!NOTE] 
> proměnné Hello níže jsou podporovány v hello **změnit hlavičky žádosti klienta** a **upravit hlavičku odpovědi klienta** funkce.

Schopnost | Proměnná | Popis | Ukázkové hodnoty
-----------|----------|-------------|----------------
Název značky | % {wurfl_cap_brand_name} | Řetězec, který označuje název značky hello hello zařízení. | Samsung
Operačního systému zařízení | % {wurfl_cap_device_os} | Řetězec, který označuje hello operační systém nainstalovaný na zařízení hello. | IOS
Verze operačního systému zařízení | % {wurfl_cap_device_os_version} | Řetězec, který označuje číslo verze hello hello operační systém nainstalovaný na zařízení hello. | 1.0.1
Duální orientace | % {wurfl_cap_dual_orientation} | Logická hodnota, která určuje, zda text hello zařízení podporuje dva orientace. | Hodnota TRUE
Upřednostňovaný souboru DTD protokolu HTML | % {wurfl_cap_html_preferred_dtd} | Řetězec, který označuje definice typu upřednostňované dokumentu (DTD) hello mobilních zařízení pro obsah HTML. | None<br/>xhtml_basic<br/>HTML5
Vložené bitové kopie | % {wurfl_cap_image_inlining} | Logická hodnota, která určuje, zda text hello zařízení podporuje Base64 kódovaný bitové kopie. | False
Se systémem Android | % {wurfl_vcap_is_android} | Logická hodnota, která označuje, zda text hello zařízení používá hello operační systém Android. | Hodnota TRUE
IOS | % {wurfl_vcap_is_ios} | Logická hodnota, která označuje, zda text hello zařízení používá iOS. | False
Je inteligentní TV | % {wurfl_cap_is_smarttv} | Logická hodnota, která určuje, zda je zařízení hello inteligentní televize. | False
Je Smartphone | % {wurfl_vcap_is_smartphone} | Logická hodnota, která určuje, zda text hello zařízení smartphone. | Hodnota TRUE
Je Tablet | % {wurfl_cap_is_tablet} | Logická hodnota, která určuje, zda text hello zařízení tablet. Toto je popis nezávislé operačního systému. | Hodnota TRUE
Je bezdrátových zařízení | % {wurfl_cap_is_wireless_device} | Logická hodnota, která určuje, zda text hello zařízení je považováno za bezdrátových zařízení. | Hodnota TRUE
Název marketing | % {wurfl_cap_marketing_name} | Řetězec, který označuje marketing název hello zařízení. | BlackBerry 8100 Pearl
Prohlížeč pro mobilní zařízení | % {wurfl_cap_mobile_browser} | Řetězec, který označuje prohlížeče hello používá toorequest obsahu ze zařízení hello. | Chrome
Verze mobilní prohlížeče | % {wurfl_cap_mobile_browser_version} | Řetězec, který označuje hello verzi prohlížeče hello používá toorequest obsahu ze zařízení hello. | 31
Název modelu | % {wurfl_cap_model_name} | Řetězec určující název modelu hello zařízení. | S3
Progresivní stahování | % {wurfl_cap_progressive_download} | Logická hodnota, která určuje, zda zařízení hello podporuje hello přehrávání zvuku a videa, zatímco stále probíhá stahování. | Hodnota TRUE
Datum vydání | % {wurfl_cap_release_date} | Řetězec, který označuje hello rok a měsíc, na které hello zařízení byl přidán toohello WURFL databáze.<br/><br/>Formát:`yyyy_mm` | 2013_december
Výška řešení | % {wurfl_cap_resolution_height} | Celé číslo, které určuje výšku hello zařízení v pixelech. | 768
Šířka řešení | % {wurfl_cap_resolution_width} | Celé číslo, které označuje hello zařízení šířku v pixelech. | 1024


## <a name="location"></a>Umístění

Toto nastavení odpovídat podmínky jsou navrženou tooidentify požadavky na základě umístění hello žadatele.

Name (Název) | Účel
-----|--------
JAKO počet | Určuje požadavky, které pocházejí z konkrétní sítě.
Země | Určuje požadavky, které pocházejí z hello zadaný zemích.


## <a name="origin"></a>Zdroj

Toto nastavení odpovídat podmínky jsou navrženou tooidentify požadavky tohoto bodu tooCDN úložiště nebo původním serveru zákazníka.

Name (Název) | Účel
-----|--------
Původu CDN | Identifikuje požadavky na obsah se uloží do úložiště CDN.
Původ zákazníka | Identifikuje požadavky na obsah uložený na původním serveru konkrétního zákazníka.


## <a name="request"></a>Žádost

Toto nastavení odpovídat podmínky jsou navrženou tooidentify požadavky na základě jejich vlastností.

Name (Název) | Účel
-----|--------
IP adresa klienta | Určuje požadavky, které pocházejí z konkrétní IP adresu.
Parametr souboru cookie | Kontroly hello soubory cookie související s každou žádostí pro hello zadaná hodnota.
Soubor cookie parametr Regex | Ověří, zda text hello soubory cookie související s každou žádostí pro hello zadaný regulární výraz.
Hraniční Cname | Určuje požadavky, které bod tooa konkrétní edge CNAME.
Odkazující domény | Určuje požadavky, které byly uvedené z hello zadaný hostname(s).
Literál hlavičky požadavku | Určuje požadavky, které obsahují zadaný hello hlavičky set tooa zadané hodnoty.
Regex hlavičky požadavku | Určuje požadavky, které obsahují zadaný hello zadat hodnotu hlavičky set tooa, který odpovídá hello regulární výraz.
Zástupný znak hlavičky požadavku | Určuje požadavky, které obsahují zadaný hello záhlaví nastavit tooa hodnotu, která odpovídá zadanému vzoru hello.
Request – metoda | Identifikuje požadavky jejich metodou HTTP.
Schéma požadavku | Identifikuje požadavků podle jejich protokolu HTTP.

## <a name="url"></a>ADRESA URL

Toto nastavení odpovídat podmínky jsou navrženou tooidentify požadavky, které jsou založené na jejich adresy URL.

Name (Název) | Účel
-----|--------
Adresa URL cesta adresáře | Identifikuje požadavků podle jejich relativní cestu.
Rozšíření cesty adresy URL | Identifikuje požadavků podle jejich příponu názvu souboru.
Název souboru cestu adresy URL | Identifikuje požadavků podle jejich název souboru.
Literál cestu adresy URL | Porovná žádost je relativní cesta toohello zadané hodnotě.
Regulární výraz cesty adresy URL | Porovná žádost je relativní cesta toohello zadaný regulární výraz.
Cesta URL zástupný znak | Porovná požadavek na relativní cestu toohello zadanému vzoru.
Adresa URL dotazu literál | Porovná žádost je toohello řetězec dotazu zadané hodnotě.
Parametr URL dotazu | Určuje požadavky, které obsahují zadaný dotaz hello, že parametr řetězce nastaven tooa hodnotu, která odpovídá zadanému vzoru.
Adresa URL dotazu Regex | Určuje požadavky, které obsahují zadaný dotaz hello, že parametr řetězce nastaven tooa hodnotu, která odpovídá zadanému regulárnímu výrazu.
Adresa URL dotazu zástupný znak | Porovná hello zadat hodnoty pro řetězec dotazu požadavku hello.


## <a name="next-steps"></a>Další kroky
* [Přehled Azure CDN](cdn-overview.md)
* [Referenční dokumentace pravidel modulu](cdn-rules-engine-reference.md)
* [Podmíněné výrazy stroj pravidel](cdn-rules-engine-reference-conditional-expressions.md)
* [Funkce modulu pravidla](cdn-rules-engine-reference-features.md)
* [Přepsání výchozího nastavení HTTP používá stroj pravidel hello](cdn-rules-engine.md)

