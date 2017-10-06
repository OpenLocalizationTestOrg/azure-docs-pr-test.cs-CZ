---
title: "zásady služby API Management aaaAzure | Microsoft Docs"
description: "Další informace o hello zásady, které jsou k dispozici pro použití v Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 1cc460cb-8e67-41aa-bc76-bbafc1892798
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 1c468ff37d73359f1dd694b91e20c2ca04f8934e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-policies"></a>Zásady služby API Management
Tato část obsahuje odkaz pro hello následující zásady služby API Management. Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).  
  
 Zásady jsou vynikající funkcí hello systému, který povolí hello vydavatele toochange hello chování hello rozhraní API prostřednictvím konfigurace. Zásady představují kolekci příkazů, které se postupně provádí na hello požadavku nebo odezvy z rozhraní API. Oblíbené příkazy patří převod formátu XML tooJSON a četnosti omezení toorestrict hello množství příchozích volání od vývojáře. Mnoho další zásady jsou k dispozici předinstalované hello.  
  
 Výrazy zásad můžete použít jako hodnoty atributů nebo textové hodnoty v libovolných hello zásad služby API Management, pokud hello zásady neurčí jinak. Některé zásady, jako je například hello [řízení toku](api-management-advanced-policies.md#choose) a [nastavená proměnná](api-management-advanced-policies.md#set-variable) jsou založené na výrazech zásad. Další informace najdete v tématu [pokročilé zásady](api-management-advanced-policies.md#AdvancedPolicies) a [výrazy zásad](api-management-policy-expressions.md).  
  
##  <a name="ProxyPolicies"></a>Zásady  
  
-   [Zásady omezení přístupu](api-management-access-restriction-policies.md#AccessRestrictionPolicies)  
  
    -   [Kontrola HTTP záhlaví](api-management-access-restriction-policies.md#CheckHTTPHeader) -vynucuje existence nebo hodnoty hlavičky protokolu HTTP.  
  
    -   [Omezení četnosti volání podle předplatného](api-management-access-restriction-policies.md#LimitCallRate) -špičky využití brání rozhraní API omezením četnosti volání, na základě za předplatné.  
  
    -   [Omezení četnosti volání podle klíče](api-management-access-restriction-policies.md#LimitCallRateByKey) -špičky využití brání rozhraní API omezením četnosti volání, na základě na klíč.  
  
    -   [Omezit volající IP adresy](api-management-access-restriction-policies.md#RestrictCallerIPs) -filtry (umožňuje nebo zakazuje) volání z konkrétní IP adresy a rozsahy adres.  
  
    -   [Nastavení kvóty využití podle předplatného](api-management-access-restriction-policies.md#SetUsageQuota) -umožňuje tooenforce obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě za předplatné.  
  
    -   [Nastavení kvóty využití podle klíče](api-management-access-restriction-policies.md#SetUsageQuotaByKey) -umožňuje tooenforce obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě na klíč.  
  
    -   [Ověření JWT](api-management-access-restriction-policies.md#ValidateJWT) -vynucuje existence a platnosti token JWT extrahovány ze zadaného záhlaví HTTP nebo parametr zadaný dotaz.  
  
-   [Pokročilé zásady](api-management-advanced-policies.md#AdvancedPolicies)  
  
    -   [Řízení toku](api-management-advanced-policies.md#choose) – podmíněně platí příkazy zásad podle hello vyhodnocení výrazy logických hodnot.  
  
    -   [Předat dál žádost](api-management-advanced-policies.md#ForwardRequest) -předává hello požadavek toohello back-end službu.  
  
    -   [Protokolu tooEvent rozbočovače](api-management-advanced-policies.md#log-to-eventhub) -odešle zprávy v hello zadaný formát tooa cíl zprávy definované entity protokolovacího nástroje.  
  
    -   [Opakujte](api-management-advanced-policies.md#Retry) -provádění opakování hello uzavřené příkazy zásad, pokud a dokud je splněna podmínka hello. Spuštění se bude opakovat v hello zadaným časovým intervalům a až toohello zadaný počet opakování.  
  
    -   [Vrátí odpověď](api-management-advanced-policies.md#ReturnResponse) -přerušení způsobených kanálu provádění a vrátí hello zadané odpovědi přímo toohello volajícího.  
  
    -   [Odeslání žádosti o jednorázové jednoduché](api-management-advanced-policies.md#SendOneWayRequest) -odešle žádost toohello zadané adresy URL bez čekání na odpověď.  
  
    -   [Odeslání požadavku](api-management-advanced-policies.md#SendRequest) -odešle žádost toohello zadaná adresa URL.  
  
    -   [Nastavená proměnná](api-management-advanced-policies.md#set-variable) -zachovat hodnotu do proměnné s názvem kontext pro pozdější přístup.  
  
    -   [Nastaví metodu požadavku](api-management-advanced-policies.md#SetRequestMethod) -vám umožní toochange hello HTTP metoda pro žádost.  
  
    -   [Nastavit stavový kód](api-management-advanced-policies.md#SetStatus) -toohello kód stavu hello HTTP změny zadané hodnotě.  
  
    -   [Trasování](api-management-advanced-policies.md#Trace) -přidá řetězec do hello [rozhraní API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) výstup.  
  
    -   [Počkejte](api-management-advanced-policies.md#Wait) -čeká pro uzavřené [odeslán požadavek na](api-management-advanced-policies.md#SendRequest), [získat hodnotu z mezipaměti](api-management-caching-policies.md#GetFromCacheByKey), nebo [řízení toku](api-management-advanced-policies.md#choose) zásady toocomplete než budete pokračovat.  
  
-   [Zásady ověřování](api-management-authentication-policies.md#AuthenticationPolicies)  
  
    -   [Ověřování Basic](api-management-authentication-policies.md#Basic) -ověření pomocí back-end službu pomocí základního ověřování.  
  
    -   [Ověřování pomocí certifikátu klienta](api-management-authentication-policies.md#ClientCertificate) -ověření pomocí back-end službu pomocí klientských certifikátů.  
  
-   [Zásady ukládání do mezipaměti](api-management-caching-policies.md#CachingPolicies)  
  
    -   [Získat z mezipaměti](api-management-caching-policies.md#GetFromCache) -provést mezipaměti vyhledat a vrátit platnou odpověď uložená v mezipaměti pokud je k dispozici.  
  
    -   [Uložení toocache](api-management-caching-policies.md#StoreToCache) -mezipamětí odpověď podle toohello uvedená konfigurace mezipaměti ovládacího prvku.  
  
    -   [Získat hodnotu z mezipaměti](api-management-caching-policies.md#GetFromCacheByKey) -načíst položky v mezipaměti podle klíče.  
  
    -   [Uložení v mezipaměti hodnoty](api-management-caching-policies.md#StoreToCacheByKey) -uložit položku hello mezipaměti podle klíče.  
  
    -   [Odebrat hodnotu z mezipaměti](api-management-caching-policies.md#RemoveCacheByKey) -odebrání položky v mezipaměti hello podle klíče.  
  
-   [Mezidoménové zásady](api-management-cross-domain-policies.md#CrossDomainPolicies)  
  
    -   [Povolit volání mezi doménami](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -zpřístupní hello rozhraní API z Adobe Flash a Microsoft Silverlight založené na prohlížeči klientů.  
  
    -   [CORS](api-management-cross-domain-policies.md#CORS) -přidá sdílení prostředků různých původů (CORS) podporoval tooan operaci nebo volání rozhraní API tooallow napříč doménami založené na prohlížeči klientů.  
  
    -   [JSONP](api-management-cross-domain-policies.md#JSONP) – přidá JSON s odsazení (JSONP) operaci tooan podpory nebo rozhraní API tooallow napříč doménami volá z klientů JavaScript založené na prohlížeči.  
  
-   [Zásady transformace](api-management-transformation-policies.md#TransformationPolicies)  
  
    -   [Převést JSON tooXML](api-management-transformation-policies.md#ConvertJSONtoXML) – převede požadavku nebo odpovědi textu z JSON tooXML.  
  
    -   [Převod XML tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) – převede požadavku nebo odpovědi textu z XML tooJSON.  
  
    -   [Najít a nahradit řetězec v textu](api-management-transformation-policies.md#Findandreplacestringinbody) – najde dílčí řetězec požadavku nebo odpovědi a nahradí je jiný dílčí řetězec.  
  
    -   [Maskování adresy URL v obsahu](api-management-transformation-policies.md#MaskURLSContent) -znovu zapíše (masky) odkazy v odpovědi hello body tak, aby ukazovaly toohello ekvivalentní propojení prostřednictvím brány hello.  
  
    -   [Nastavení back-end služby](api-management-transformation-policies.md#SetBackendService) -změny hello back-end službu pro příchozí žádosti.  
  
    -   [Tělo nastavit](api-management-transformation-policies.md#SetBody) -nastaví hello text zprávy pro příchozí a odchozí požadavky.  
  
    -   [Set – hlavička protokolu HTTP](api-management-transformation-policies.md#SetHTTPheader) – přiřadí hodnota tooan existující odpovědi nebo hlavička požadavku nebo přidá hlavičku odpovědi nebo žádost o nový.  
  
    -   [Nastavte parametr řetězce dotazu](api-management-transformation-policies.md#SetQueryStringParameter) – přidá, nahradí hodnotu nebo odstraní parametr řetězce dotazu požadavku.  
  
    -   [Přepsání adresy URL](api-management-transformation-policies.md#RewriteURL) – převede adresu URL požadavku z formuláře toohello jeho veřejné formuláře očekávanou hello webové služby.  
  
    -   [Transformace XML pomocí transformaci XSLT](api-management-transformation-policies.md#XSLTransform) -vztahuje k transformaci tooXML XSL v textu požadavku nebo odpovědi hello.  
  
## <a name="next-steps"></a>Další kroky
Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).  
