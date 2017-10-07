---
title: "Seznam B2B aplikace logiky chyby a řešení: Azure App Service | Microsoft Docs"
description: "Seznam B2B aplikace logiky chyby a řešení"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 0340e2979f1972ba631354e206c93969e55946e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="logic-apps-b2b-list-of-errors-and-solutions"></a>Seznam B2B aplikace logiky chyby a řešení  
Tento článek vám pomůže vyřešit chyby, které by se mohlo stát v scénáře B2B aplikace logiky a navrhne příslušné akce při opravách tyto chyby.


## <a name="agreement-resolution"></a>Smlouva řešení

### <a name="no-agreement-found"></a>* Byla nalezena žádná smlouva. 

|   |   |  
|---|---|
| Popis chyby | Žádné smlouvy nalezen s parametry řešení smlouvy|    
| Akce uživatele | Hello smlouvy musí být přidaní toohello účet integrace s identitami odsouhlaseného firmy.</br> Hello firmy identity shodovat s identifikátory toohello vstupní zpráv|  
|   |   |

### <a name="-no-agreement-found-with-identities"></a>* Žádná smlouva se identity nalezena.

|   |   | 
|---|---|
| Popis chyby | Žádná nalezena s identitami smlouva: 'AS2Identity':: 'Partner1' a 'AS2Identity':: 'Partner3.| 
| Akce uživatele | Neplatný AS2-z nebo AS2 tooconfigured pro smlouvu. </br> Správné AS2 zpráva AS2-z nebo AS2 tooheaders nebo smlouvu toomatch AS2 ID v AS2 zprávy záhlaví s konfigurací smlouvy |
|   |   |     

## <a name="as2"></a>AS2

### <a name="-missing-as2-message-headers"></a>* Chybí záhlaví AS2 zpráv  

|   |   |  
|---|---|
| Popis chyby| Neplatný AS2 hlavičky. Jeden z ' AS2-k ' nebo ' AS2-z, hlavičky jsou prázdné| 
| Akce uživatele | Byla přijata zpráva AS2, který neobsahoval hello AS2-z nebo AS2 tooor obou hlavičky. </br> Zkontrolujte zprávy a AS2 AS2-z a AS2 tooheaders a opravte je na základě smlouvy konfigurace |
|  |  | 


### <a name="-missing-as2-message-body-and-headers"></a>* Chybí tělo zprávy AS2 a hlavičky    

|   |   |  
|---|---|
| Popis chyby| obsah žádosti Hello je null nebo prázdná | 
| Akce uživatele | Byla přijata zpráva AS2, který neobsahoval tělo zprávy hello |
|  |  | 

### <a name="-as2-message-decryption-failure"></a>* Chyba při dešifrování zprávy AS2

|   |   | 
|---|---|
| Popis chyby |  [zpracování chybou: Neúspěšné dešifrování] | 
| Akce uživatele | Přidat @base64ToBinary tooAS2Message před odesláním toopartner 
```java
            "HTTP": {
                "inputs": {
                    "body": "@base64ToBinary(body('Encode_to_AS2_message')?['AS2Message']?['Content'])",
                    "headers": "@body('Encode_to_AS2_message')?['AS2Message']?['OutboundHeaders']",
                    "method": "POST",
                    "uri": "xxxxx.xxx"
                },
                
``` 

### <a name="-mdn-decryption-failure"></a>* Chyba při dešifrování MDN

|   |   | 
|---|---|
| Popis chyby |  [zpracování chybou: Neúspěšné dešifrování] | 
| Akce uživatele | Přidat @base64ToBinary tooMDN před odesláním toopartner 
```java
            "Response": {
                "inputs": {
                    "body": "@base64ToBinary(body('Decode_AS2_message')?['OutgoingMDN']?['Content'])",
                    "headers": "@body('Decode_AS2_message')?['OutgoingMDN']?['OutboundHeaders']",
                    "statusCode": 200
                },
                
``` 

### <a name="-missing-signing-certificate"></a>* Chybí podpisový certifikát

|   |   |  
|---|---|
| Popis chyby| Certifikát pro podpis Hello nebyla nakonfigurována pro stranu AS2. </br> AS2-z: partner1 AS2-k: partner2 | 
| Akce uživatele | Konfigurace nastavení smlouvy AS2 s správný certifikát pro podpis |
|  |  | 

## <a name="x12-and-edifact"></a>X12 a EDIFACT

### <a name="-leading-or-trailing-space-found"></a>* Počáteční nebo koncové mezery nalezené    
    
|   |   | 
|---|---|
| Popis chyby | Během analýzy došlo k chybě. Dobrý den Edifact transakce s id ' 123456 'obsažená v výměnu (bez skupina) s id ' 987654', s id odesílatele 'Partner1', id příjemce 'Partner2' bylo pozastaveno s následujícími chybami: úvodní koncové oddělovače nalezen |
| Akce uživatele | Hello smlouvy nastavení toobe nakonfigurovat tooallow počátečních a koncových znaků. </br> Upravit nastavení tooallow smlouvy počátečních a koncových znaků |
|   |   |

![Povolit místa](./media/logic-apps-enterprise-integration-b2b-list-errors-solutions/leadingandtrailing.png)

### <a name="-duplicate-check-has-enabled-in-hello-agreement"></a>* Duplicitní kontrola povolil hello smlouvy

|   |   | 
|---|---| 
| Popis chyby | Duplicitní číslo ovládacího prvku |
| Akce uživatele | Tato chyba znamená, že hello přijata zpráva má duplicitní řízení čísla. </br> Opravte hello řízení číslo a opakujte odeslání uvítací zprávu |
|   |   |

### <a name="-missing-schema-in-hello-agreement"></a>* Chybějící schématu hello smlouvy

|   |   | 
|---|---| 
| Popis chyby | Během analýzy došlo k chybě. Sada Hello X12 transakce s id '564220001' obsažená v funkční skupiny s id '56422', v výměnu s id "000056422" s id odesílatele ' 12345678', id příjemce ' 87654321' bylo pozastaveno s následujícími chybami "hello zpráva má ty neznámé dokumentu PE a nebylo možné přeložit tooany hello existující schémata nakonfigurované smlouvy hello " |
| Akce uživatele | Nakonfigurujte schématu smlouvy hello  |
|   |   |

### <a name="-incorrect-schema-in-hello-agreement"></a>* Nesprávné schéma hello smlouvy

|   |   | 
|---|---| 
| Popis chyby | uvítací zprávu má typ Neznámý dokumentu a nebylo možné přeložit tooany hello existující schémata nakonfigurované hello smlouvy. |
| Akce uživatele | Nakonfigurujte správné schéma smlouvy hello  |
|   |   |

## <a name="flat-file"></a>Plochý soubor

### <a name="-input-message-with-no-body"></a>* Vstupní zprávu s žádný text

|   |   | 
|---|---|
| Popis chyby | InvalidTemplate. Výrazy jazyka šablony nelze tooprocess v vstupy 'Flat_File_Decoding' akce v řádku '1' a ve sloupci '1902': ' požadované vlastnosti 'content' očekává hodnotu ale byla null. Cesta k ".". |
| Akce uživatele | Tato chyba označuje, že vstupní uvítací zprávu neobsahuje text |
|   |   | 

## <a name="learn-more"></a>Další informace
[Další informace o hello Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md)
