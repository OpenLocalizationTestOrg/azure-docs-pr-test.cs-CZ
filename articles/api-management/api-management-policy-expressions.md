---
title: "výrazy zásad aaaAzure API Management | Microsoft Docs"
description: "Další informace o výrazy zásad v Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: ea160028-fc04-4782-aa26-4b8329df3448
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 79da0d6ca3963307ec811a33aaac3d63a7abd97d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-policy-expressions"></a>Výrazy zásad API Management
Syntaxe výrazy zásad je C# 6.0. Každý výraz má přístup toohello implicitně poskytuje [kontextu](api-management-policy-expressions.md#ContextVariables) proměnné a povolení [podmnožina](api-management-policy-expressions.md#CLRTypes) typů rozhraní .NET Framework.  
  
> [!NOTE]
>  Další informace o výrazů zásad najdete v tématu hello [výrazy zásad](https://azure.microsoft.com/documentation/videos/policy-expressions-in-azure-api-management/) videa.  
>   
>  Ukázky Konfigurace zásady pomocí výrazů zásad najdete v části [cloudu zahrnují díl 177: rozhraní API funkce správy více s Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Toto video obsahuje hello následující ukázky výraz zásad.  
>   
>  -   10:30 - najdete v části Jak tooapply zásady v hello API úrovně toosupply kontextové informace toohello back-end službu pomocí hello [nastavit parametr řetězce dotazu](api-management-transformation-policies.md#SetQueryStringParameter) a [hlavičky protokolu HTTP nastaven](api-management-transformation-policies.md#SetHTTPheader) zásady. Na 12:10 je ukázku volání operace v hello portál pro vývojáře, kde se můžete podívat tyto zásady v práci.  
> -   Najdete v části 13:50 - jak toouse hello [ověření JWT](api-management-access-restriction-policies.md#ValidateJWT) zásad toopre-toooperations přístupu na základě tokenu deklarací autorizace. Rychlé převinutí vpřed too15:00 toosee hello zásady nakonfigurované v editoru zásad hello, a potom too18:50 pro předvedení volání operace z portálu pro vývojáře hello s i bez hello vyžaduje autorizační token.  
> -   21:00 - najdete v části Jak toouse [rozhraní API Inspector](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) trasování toosee jak zásad se vyhodnocují a hello výsledky hodnocení hello.  
> -   25:25 - najdete v tématu Jak výrazy zásad toouse s hello [získat z mezipaměti](api-management-caching-policies.md#GetFromCache) a [toocache úložiště](api-management-caching-policies.md#StoreToCache) odpovědi API Management tooconfigure zásady ukládání do mezipaměti Doba trvání, zda text hello odpovídá ukládání odpovědí do mezipaměti z hello back-end službu podle specifikace hello zálohovaný služby `Cache-Control` – direktiva.  
> -   34:30 - najdete v tématu Jak obsah tooperform filtrování podle odebírání elementů data z odpovědi hello přijal od služby back-end hello pomocí hello [řízení toku](api-management-advanced-policies.md#choose) a [tělo nastavit](api-management-transformation-policies.md#SetBody) zásady. Spuštění 31:50 toosee přehled [hello tmavý Sky prognózy API](https://developer.forecast.io/) použít v této ukázce.  
> -   příkazy zásad toodownload hello používá v tomto videu, najdete v části hello [rozhraní api správy – ukázky nebo zásad](https://github.com/Azure/api-management-samples/tree/master/policies) úložiště github.  
  
  
##  <a name="Syntax"></a>Syntaxe  
 Jediný příkaz výrazy jsou uzavřené v `@(expression)`, kde `expression` je ve správném formátu prohlášení výrazu jazyka C#.  
  
 Výrazy vícepříkazové jsou uzavřené v `@{expression}`. Všechny cesty kódu v rámci vícepříkazové výrazy musí končit `return` příkaz.  
  
##  <a name="PolicyExpressionsExamples"></a>Příklady  
  
```  
@(true)  
  
@((1+1).ToString())  
  
@("Hi There".Length)  
  
@(Regex.Match(context.Response.Headers.GetValueOrDefault("Cache-Control",""), @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value)  
  
@(context.Variables.ContainsKey("maxAge") ? int.Parse((string)context.Variables["maxAge"]) : 3600)  
  
@{   
  string value;   
  if (context.Request.Headers.TryGetValue("Authorization", out value))   
  {   
    return Encoding.UTF8.GetString(Convert.FromBase64String(value));  
  }   
  else   
  {   
    return null;  
  }  
}  
```  
  
##  <a name="PolicyExpressionsUsage"></a>Využití  
 Výrazy lze použít jako hodnoty atributů nebo textové hodnoty v libovolných hello API Management [zásady](api-management-policies.md), pokud odkazy na zásady hello neurčí jinak.  
  
> [!IMPORTANT]
>  Upozorňujeme, že při použití výrazy zásad se jenom omezené ověření výrazy zásad hello při definování zásad hello. Vzhledem k tomu, že hello výrazy jsou provést při spuštění v kanálu příchozí nebo odchozí hello bránou hello, všechny výjimky v době běhu generované výrazy zásad hello výsledkem bude chyba za běhu v volání hello rozhraní API.  
  
##  <a name="CLRTypes"></a>Povolené ve výrazech zásad typy rozhraní .NET framework  
 Hello následující tabulka uvádí typy hello rozhraní .NET Framework a jejich členové, které jsou povoleny ve výrazech zásad.  
  
|Typ CLR|Podporované metody|  
|--------------|-----------------------|  
|Newtonsoft.Json.Linq.Extensions|Jsou podporovány všechny metody|  
|Newtonsoft.Json.Linq.JArray|Jsou podporovány všechny metody|  
|Newtonsoft.Json.Linq.JConstructor|Jsou podporovány všechny metody|  
|Newtonsoft.Json.Linq.JContainer|Jsou podporovány všechny metody|  
|Newtonsoft.Json.Linq.JObject|Jsou podporovány všechny metody|  
|Newtonsoft.Json.Linq.JProperty|Jsou podporovány všechny metody|  
|Newtonsoft.Json.Linq.JRaw|Jsou podporovány všechny metody|  
|Newtonsoft.Json.Linq.JToken|Jsou podporovány všechny metody|  
|Newtonsoft.Json.Linq.JTokenType|Jsou podporovány všechny metody|  
|Newtonsoft.Json.Linq.JValue|Jsou podporovány všechny metody|  
|System.Collections.Generic.IReadOnlyCollection < T\>|Všechny|  
|System.Collections.Generic.IReadOnlyDictionary < TKey, TValue >|Všechny|  
|System.Collections.Generic.ISet < TKey, TValue >|Všechny|  
|System.Collections.Generic.KeyValuePair < TKey, TValue >|Klíč, hodnota|  
|System.Collections.Generic.List < TKey, TValue >|Všechny|  
|System.Collections.Generic.Queue < TKey, TValue >|Všechny|  
|System.Collections.Generic.Stack < TKey, TValue >|Všechny|  
|Metodu System.Convert|Všechny|  
|System.DateTime|Všechny|  
|System.DateTimeKind|Čas UTC|  
|System.DateTimeOffset|Všechny|  
|System.Decimal|Všechny|  
|System.Double|Všechny|  
|System.Guid|Všechny|  
|System.IEnumerable < T\>|Všechny|  
|System.IEnumerator < T\>|Všechny|  
|System.Int16|Všechny|  
|System.Int32|Všechny|  
|System.Int64|Všechny|  
|System.Linq.Enumerable < T\>|Jsou podporovány všechny metody|  
|System.Math|Všechny|  
|System.MidpointRounding|Všechny|  
|System.Nullable < T\>|Všechny|  
|System.Random|Všechny|  
|System.SByte|Všechny|  
|System.Security.Cryptography. HMACSHA384|Všechny|  
|System.Security.Cryptography. HMACSHA512|Všechny|  
|System.Security.Cryptography.HashAlgorithm|Všechny|  
|System.Security.Cryptography.HMAC|Všechny|  
|System.Security.Cryptography.HMACMD5|Všechny|  
|System.Security.Cryptography.HMACSHA1|Všechny|  
|System.Security.Cryptography.HMACSHA256|Všechny|  
|System.Security.Cryptography.KeyedHashAlgorithm|Všechny|  
|System.Security.Cryptography.MD5|Všechny|  
|System.Security.Cryptography.RNGCryptoServiceProvider|Všechny|  
|System.Security.Cryptography.SHA1|Všechny|  
|System.Security.Cryptography.SHA1Managed|Všechny|  
|System.Security.Cryptography.SHA256|Všechny|  
|System.Security.Cryptography.SHA256Managed|Všechny|  
|System.Security.Cryptography.SHA384|Všechny|  
|System.Security.Cryptography.SHA384Managed|Všechny|  
|System.Security.Cryptography.SHA512|Všechny|  
|System.Security.Cryptography.SHA512Managed|Všechny|  
|System.Single|Všechny|  
|System.String|Všechny|  
|System.StringSplitOptions|Všechny|  
|System.Text.Encoding|Všechny|  
|System.Text.RegularExpressions.Capture|Hodnotu indexu, délka,|  
|System.Text.RegularExpressions.CaptureCollection|Počet položek|  
|System.Text.RegularExpressions.Group|Zachycení úspěch|  
|System.Text.RegularExpressions.GroupCollection|Počet položek|  
|System.Text.RegularExpressions.Match|Prázdné, skupin, výsledek|  
|System.Text.RegularExpressions.Regex|Nahraďte shodu, aby se shodovaly .ctor, IsMatch –,|  
|System.Text.RegularExpressions.RegexOptions|Kompilovat, IgnoreCase, IgnorePatternWhitespace, víceřádkových, None, RightToLeft, Jednořákový|  
|System.TimeSpan|Všechny|  
|System.Tuple|Všechny|  
|System.UInt16|Všechny|  
|System.UInt32|Všechny|  
|System.UInt64|Všechny|  
|System.Uri|Všechny|  
|System.Xml.Linq.Extensions|Jsou podporovány všechny metody|  
|System.Xml.Linq.XAttribute|Jsou podporovány všechny metody|  
|System.Xml.Linq.XCData|Jsou podporovány všechny metody|  
|System.Xml.Linq.XComment|Jsou podporovány všechny metody|  
|System.Xml.Linq.XContainer|Jsou podporovány všechny metody|  
|System.Xml.Linq.XDeclaration|Jsou podporovány všechny metody|  
|System.Xml.Linq.XDocument|Jsou podporovány všechny metody|  
|System.Xml.Linq.XDocumentType|Jsou podporovány všechny metody|  
|System.Xml.Linq.XElement|Jsou podporovány všechny metody|  
|System.Xml.Linq.XName|Jsou podporovány všechny metody|  
|System.Xml.Linq.XNamespace|Jsou podporovány všechny metody|  
|System.Xml.Linq.XNode|Jsou podporovány všechny metody|  
|System.Xml.Linq.XNodeDocumentOrderComparer|Jsou podporovány všechny metody|  
|System.Xml.Linq.XNodeEqualityComparer|Jsou podporovány všechny metody|  
|System.Xml.Linq.XObject|Jsou podporovány všechny metody|  
|System.Xml.Linq.XProcessingInstruction|Jsou podporovány všechny metody|  
|System.Xml.Linq.XText|Jsou podporovány všechny metody|  
|System.Xml.XmlNodeType|Všechny|  
  
##  <a name="ContextVariables"></a>Kontextové proměnné  
 Proměnné s názvem `context` je implicitně k dispozici v každé zásadě [výraz](api-management-policy-expressions.md#Syntax). Její členy poskytují informace o příslušném toohello `\request`. Všechny hello `context` členové jsou jen pro čtení.  
  
|Kontextové proměnné|Povolené metody, vlastnosti a hodnoty parametru|  
|----------------------|-------------------------------------------------------|  
|Kontext|Rozhraní API: IApi<br /><br /> Nasazení<br /><br /> Poslední chyba<br /><br /> Operace<br /><br /> Produkt<br /><br /> Žádost<br /><br /> ID žádosti: řetězec<br /><br /> Odpověď<br /><br /> Předplatné<br /><br /> Trasování: bool<br /><br /> Uživatel<br /><br /> Proměnné: IReadOnlyDictionary < string, object ><br /><br /> void Trace(message: string)|  
|kontext. Rozhraní API|ID: řetězec<br /><br /> Název: řetězec<br /><br /> Cesta: řetězec<br /><br /> ServiceUrl: IUrl|  
|kontext. Nasazení|Oblast: řetězec<br /><br /> ServiceName: řetězec|  
|kontext. Poslední chyba|Zdroj: řetězec<br /><br /> Důvod: řetězec<br /><br /> Zpráva: řetězec<br /><br /> Obor: řetězec<br /><br /> Část: řetězec<br /><br /> Cesta: řetězec<br /><br /> PolicyId: řetězec<br /><br /> Další informace o kontextu. Poslední chyba, najdete v části [zpracování chyb](api-management-error-handling-policies.md).|  
|kontext. Operace|ID: řetězec<br /><br /> Metoda: řetězec<br /><br /> Název: řetězec<br /><br /> UrlTemplate: řetězec|  
|kontext. Produktu|Rozhraní API: IEnumerable < IApi\><br /><br /> ApprovalRequired: bool<br /><br /> Skupiny: Rozhraní IEnumerable < IGroup\><br /><br /> ID: řetězec<br /><br /> Název: řetězec<br /><br /> Stav: výčtu ProductState {NotPublished, publikováno}<br /><br /> SubscriptionLimit: int?<br /><br /> SubscriptionRequired: bool|  
|kontext. Požadavek|Text: IMessageBody<br /><br /> Certifikát: System.Security.Cryptography.X509Certificates.X509Certificate2<br /><br /> Hlavičky: IReadOnlyDictionary < řetězec, řetězec [] ><br /><br /> IP adresa: řetězec<br /><br /> MatchedParameters: IReadOnlyDictionary < řetězec, řetězec ><br /><br /> Metoda: řetězec<br /><br /> OriginalUrl:IUrl<br /><br /> Adresa URL: IUrl|  
|řetězec kontext. Request.Headers.GetValueOrDefault (headerName: string, výchozí hodnota: string)|headerName: řetězec<br /><br /> Výchozí hodnota: string<br /><br /> Vrátí oddělených hodnot hlavičky požadavku nebo `defaultValue` Pokud hello hlavička nebyla nalezena.|  
|kontext. Odpověď|Text: IMessageBody<br /><br /> Hlavičky: IReadOnlyDictionary < řetězec, řetězec [] ><br /><br /> StatusCode: int<br /><br /> StatusReason: řetězec|  
|řetězec kontext. Response.Headers.GetValueOrDefault (headerName: string, výchozí hodnota: string)|headerName: řetězec<br /><br /> Výchozí hodnota: string<br /><br /> Vrátí hodnoty hlavičky odpovědi oddělených čárkou nebo `defaultValue` Pokud hello hlavička nebyla nalezena.|  
|kontext. Předplatné|CreatedTime: data a času<br /><br /> Koncové datum: data a času?<br /><br /> ID: řetězec<br /><br /> Klíč: řetězec<br /><br /> Název: řetězec<br /><br /> PrimaryKey: řetězec<br /><br /> Sekundární klíč: řetězec<br /><br /> Počátečním: data a času?|  
|kontext. Uživatel|E-mailu: řetězec<br /><br /> FirstName: řetězec<br /><br /> Skupiny: Rozhraní IEnumerable < IGroup\><br /><br /> ID: řetězec<br /><br /> Identity: Rozhraní IEnumerable < IUserIdentity\><br /><br /> Příjmení: řetězec<br /><br /> Poznámka: řetězec<br /><br /> RegistrationDate: data a času|  
|IApi|ID: řetězec<br /><br /> Název: řetězec<br /><br /> Cesta: řetězec<br /><br /> Protokoly: Rozhraní IEnumerable < řetězec\><br /><br /> ServiceUrl: IUrl<br /><br /> SubscriptionKeyParameterNames: ISubscriptionKeyParameterNames|  
|IGroup|ID: řetězec<br /><br /> Název: řetězec|  
|IMessageBody|Jako < T\>(preserveContent: bool = false): kde T: řetězec, JObject, JToken, JArray, XNode, XElement, XDocument<br /><br /> Hello `context.Request.Body.As<T>` a `context.Response.Body.As<T>` metody jsou použité tooread zprávu žádosti a odpovědi subjekty v zadaného typu `T`. Ve výchozím nastavení hello metoda používá hello původní text datového proudu zpráv a reneders není k dispozici po ho vrátí. tooavoid, která tak, že metoda hello pracovat na kopii datového proudu text hello, sada hello `preserveContent` parametr příliš`true`. Přejděte [sem](api-management-transformation-policies.md#SetBody) toosee příklad.|  
|IUrl|Hostitele: řetězec<br /><br /> Cesta: řetězec<br /><br /> Port: int<br /><br /> Dotaz: IReadOnlyDictionary < řetězec, řetězec [] ><br /><br /> Řetězec dotazu: řetězec<br /><br /> Schéma: řetězec|  
|IUserIdentity|ID: řetězec<br /><br /> Zprostředkovatel: řetězec|  
|ISubscriptionKeyParameterNames|Záhlaví: řetězec<br /><br /> Dotaz: řetězec|  
|řetězec IUrl.Query.GetValueOrDefault (queryParameterName: string, výchozí hodnota: string)|queryParameterName: řetězec<br /><br /> Výchozí hodnota: string<br /><br /> Vrátí čárkami oddělený hodnoty parametru dotazu nebo `defaultValue` Pokud není nalezen parametr hello.|  
|Kontext T. Variables.GetValueOrDefault < T\>(NázevProměnné: string, výchozí hodnota: T)|NázevProměnné: řetězec<br /><br /> Výchozí hodnota: T<br /><br /> Vrátí hodnotu proměnné přetypování tootype `T` nebo `defaultValue` Pokud není nalezena proměnná hello.<br /><br /> Tato metoda vyvolá výjimku, pokud hello zadaný typ neodpovídá hello skutečný typ hello vrátil proměnné.|  
|BasicAuthCredentials AsBasic(input: this string)|vstupní: řetězec<br /><br /> Pokud vstupní parametr hello obsahuje platnou hodnotu záhlaví autorizace požadavku HTTP základní ověřování, vrátí metoda hello objekt typu `BasicAuthCredentials`; v opačném případě hello metoda vrátí hodnotu null.|  
|BOOL TryParseBasic (vstup: Tento řetězec, výsledek: out BasicAuthCredentials)|vstupní: řetězec<br /><br /> výsledek: out BasicAuthCredentials<br /><br /> Pokud vstupní parametr hello obsahuje platnou hodnotu záhlaví autorizace požadavku HTTP základní ověřování, vrátí metoda hello `true` a hello výsledek parametr obsahuje hodnotu typu `BasicAuthCredentials`; v opačném případě vrátí metoda hello `false`.|  
|BasicAuthCredentials|Heslo: řetězec<br /><br /> ID uživatele: řetězec|  
|Jwt AsJwt(input: this string)|vstupní: řetězec<br /><br /> Pokud vstupní parametr hello obsahuje platnou hodnotu tokenu JWT, vrátí metoda hello objekt typu `Jwt`; v opačném případě vrátí metoda hello `null`.|  
|BOOL TryParseJwt (vstup: Tento řetězec, výsledek: out Jwt)|vstupní: řetězec<br /><br /> výsledek: out Jwt<br /><br /> Pokud vstupní parametr hello obsahuje platnou hodnotu tokenu JWT, vrátí metoda hello `true` a hello výsledek parametr obsahuje hodnotu typu `Jwt`; v opačném případě vrátí metoda hello `false`.|  
|Token Jwt|Algoritmus: řetězec<br /><br /> Cílová skupina: IEnumerable < řetězec\><br /><br /> Deklarace identity: IReadOnlyDictionary < řetězec, řetězec [] ><br /><br /> ExpirationTime: data a času?<br /><br /> ID: řetězec<br /><br /> Vystavitel: řetězec<br /><br /> Neplatí před: data a času?<br /><br /> Předmět: řetězec<br /><br /> Typ: řetězec|  
|řetězec Jwt.Claims.GetValueOrDefault (claimName: string, výchozí hodnota: string)|claimName: řetězec<br /><br /> Výchozí hodnota: string<br /><br /> Vrátí deklarace identity hodnot oddělených čárkou nebo `defaultValue` Pokud hello hlavička nebyla nalezena.|

## <a name="next-steps"></a>Další kroky
Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).  
