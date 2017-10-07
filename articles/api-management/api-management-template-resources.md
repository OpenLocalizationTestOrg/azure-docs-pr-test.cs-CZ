---
title: "prostředky šablony aaaAzure API Management | Microsoft Docs"
description: "Další informace o hello typů prostředků, které jsou k dispozici pro použití v šablonách portálu vývojáře ve službě Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 51a1b4c6-a9fd-4524-9e0e-03a9800c3e94
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 2221e3852986d485d13817b483e473dfe451d3c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-template-resources"></a>Šablony prostředků Azure API Management
Azure API Management poskytuje následující typy prostředků pro použití v šablonách portálu vývojáře hello hello.  
  
-   [Řetězce prostředků](#strings)  
  
-   [Glyfy prostředky](#glyphs)  
  
##  <a name="strings"></a>Řetězce prostředků  
 API Management poskytuje komplexní sadu řetězcové prostředky pro použití v portálu pro vývojáře hello. Tyto prostředky jsou lokalizované do všech hello jazyky podporované službou API Management. Hello výchozí sadu šablony použije tyto prostředky pro záhlaví stránky, popisky a všechny konstantní řetězce, které jsou zobrazeny v portálu pro vývojáře hello. toouse řetězec prostředku šablony zadejte předpona řetězce prostředků hello následuje název hello řetězce, jak ukazuje následující příklad hello.  
  
```  
{% localized "Prefix|Name" %}  
  
```  
  
 Hello následující příklad je z hello produktu seznamu šablony a zobrazí **produkty** hello horní části stránky hello.  
  
```  
<h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>  
  
```  
  
 Naleznete toohello následující tabulky pro hello řetězcové prostředky k dispozici pro použití v vaše šablony portálu pro vývojáře. Použijte název tabulky hello jako hello předponu pro hello řetězcové prostředky v této tabulce.  
  
-   [ApisStrings](#ApisStrings)  
  
-   [ApplicationListStrings](#ApplicationListStrings)  
  
-   [AppDetailsStrings](#AppDetailsStrings)  
  
-   [AppStrings](#AppStrings)  
  
-   [CommonResources](#CommonResources)  
  
-   [CommonStrings](#CommonStrings)  
  
-   [Dokumentace](#Documentation)  
  
-   [ErrorPageStrings](#ErrorPageStrings)  
  
-   [IssuesStrings](#IssuesStrings)  
  
-   [NotFoundStrings](#NotFoundStrings)  
  
-   [ProductDetailsStrings](#ProductDetailsStrings)  
  
-   [ProductsStrings](#ProductsStrings)  
  
-   [ProviderInfoStrings](#ProviderInfoStrings)  
  
-   [SigninResources](#SigninResources)  
  
-   [SigninStrings](#SigninStrings)  
  
-   [SignupStrings](#SignupStrings)  
  
-   [SubscriptionListStrings](#SubscriptionListStrings)  
  
-   [SubscriptionStrings](#SubscriptionStrings)  
  
-   [UpdateProfileStrings](#UpdateProfileStrings)  
  
-   [UserProfile](#UserProfile)  
  
###  <a name="ApisStrings"></a>ApisStrings  
  
|Name (Název)|Text|  
|----------|----------|  
|PageTitleApis|Rozhraní API|  
  
###  <a name="AppDetailsStrings"></a>AppDetailsStrings  
  
|Name (Název)|Text|  
|----------|----------|  
|WebApplicationsDetailsTitle|Náhled aplikace|  
|WebApplicationsRequirementsHeader|Požadavky|  
|WebApplicationsScreenshotAlt|snímek obrazovky|  
|WebApplicationsScreenshotsHeader|Snímky obrazovky|  
  
###  <a name="ApplicationListStrings"></a>ApplicationListStrings  
  
|Name (Název)|Text|  
|----------|----------|  
|WebDevelopersAppDeleteConfirmation|Jste si jisti, že chcete tooremove aplikace?|  
|WebDevelopersAppNotPublished|Není publikována.|  
|WebDevelopersAppNotSubminted|Neodesláno|  
|WebDevelopersAppTableCategoryHeader|Kategorie|  
|WebDevelopersAppTableNameHeader|Name (Název)|  
|WebDevelopersAppTableStateHeader|Stav|  
|WebDevelopersEditLink|Upravit|  
|WebDevelopersRegisterAppLink|Registrace aplikace|  
|WebDevelopersRemoveLink|Odebrat|  
|WebDevelopersSubmitLink|Odeslat|  
|WebDevelopersYourApplicationsHeader|Vaše aplikace|  
  
###  <a name="AppStrings"></a>AppStrings  
  
|Name (Název)|Text|  
|----------|----------|  
|WebApplicationsHeader|Aplikace|  
  
###  <a name="CommonResources"></a>CommonResources  
  
|Name (Název)|Text|  
|----------|----------|  
|NoItemsToDisplay|Nenašli jsme žádné výsledky.|  
|GeneralExceptionMessage|Něco není pravé. To může být k dočasné chybě nebo chyby. Zkuste to prosím znovu.|  
|GeneralJsonExceptionMessage|Něco není pravé. To může být k dočasné chybě nebo chyby. Zkontrolujte znovu načíst stránku hello a zkuste to znovu.|  
|ConfirmationMessageUnsavedChanges|Existují některé neuložené změny. Opravdu chcete toocancel a hello změny zahodit?|  
|Azureactivedirectory selhala|Azure Active Directory|  
|HttpLargeRequestMessage|Http text žádosti příliš velký.|  
  
###  <a name="CommonStrings"></a>CommonStrings  
  
|Name (Název)|Text|  
|----------|----------|  
|ButtonLabelCancel|Zrušit|  
|ButtonLabelSave|Uložit|  
|GeneralExceptionMessage|Něco není pravé. To může být k dočasné chybě nebo chyby. Zkuste to prosím znovu.|  
|NoItemsToDisplay|Neexistují žádné položky toodisplay.|  
|PagerButtonLabelFirst|První|  
|PagerButtonLabelLast|poslední|  
|PagerButtonLabelNext|Další|  
|PagerButtonLabelPrevious|Prev|  
|PagerLabelPageNOfM|Stránku {0} {1}|  
|PasswordTooShort|Hello heslo je příliš krátké.|  
|EmailAsPassword|Nepoužívejte e-mailu jako heslo|  
|PasswordSameAsUserName|Heslo nesmí obsahovat vaše uživatelské jméno|  
|PasswordTwoCharacterClasses|Použití třídy různých znaků|  
|PasswordTooManyRepetitions|Příliš mnoho opakování|  
|PasswordSequenceFound|Heslo obsahuje pořadí|  
|PagerLabelPageSize|Velikost stránky|  
|CurtainLabelLoading|Načítání...|  
|TablePlaceholderNothingToDisplay|Neexistují žádná data pro vybrané hello období a oboru|  
|ButtonLabelClose|Zavřít|  
  
###  <a name="Documentation"></a>Dokumentace  
  
|Name (Název)|Text|  
|----------|----------|  
|WebDocumentationInvalidHeaderErrorMessage|Neplatná hlavička {0}.|  
|WebDocumentationInvalidRequestErrorMessage|Neplatný požadavek adresy URL|  
|TextboxLabelAccessToken|Přístupový token *|  
|DropdownOptionPrimaryKeyFormat|Primární – {0}|  
|DropdownOptionSecondaryKeyFormat|Sekundární-{0}|  
|WebDocumentationSubscriptionKeyText|Svůj klíč předplatného|  
|WebDocumentationTemplatesAddHeaders|Přidejte požadované hlavičky protokolu HTTP|  
|WebDocumentationTemplatesBasicAuthSample|Ukázka základní ověřování|  
|WebDocumentationTemplatesCurlForBasicAuth|pro základní ověřování použijte:--uživatele {username}: {heslo}|  
|WebDocumentationTemplatesCurlValuesForPath|Zadejte hodnoty pro parametry cesty (zobrazené jako {...}), vašeho předplatného klíče a hodnoty pro parametry dotazu|  
|WebDocumentationTemplatesDeveloperKey|Zadejte svůj klíč předplatného|  
|WebDocumentationTemplatesJavaApache|Tato ukázka používá hello Apache HTTP klienta z komponenty HTTP (http://hc.apache.org/httpcomponents-client-ga/)|  
|WebDocumentationTemplatesOptionalParams|Zadejte hodnoty pro volitelné parametry, podle potřeby|  
|WebDocumentationTemplatesPhpPackage|Tato ukázka používá hello HTTP_Request2 balíček. (Další informace: http://pear.php.net/package/HTTP_Request2)|  
|WebDocumentationTemplatesPythonValuesForPath|Zadejte hodnoty pro parametry cesty (zobrazené jako {...}) a text žádosti v případě potřeby|  
|WebDocumentationTemplatesRequestBody|Zadejte text žádosti|  
|WebDocumentationTemplatesRequiredParams|Zadejte hodnoty pro následující parametry hello|  
|WebDocumentationTemplatesValuesForPath|Zadejte hodnoty pro parametry cesty (zobrazené jako {...})|  
|OAuth2AuthorizationEndpointDescription|koncový bod autorizace Hello je použité toointeract s vlastníkem prostředku hello a získat udělení autorizace.|  
|OAuth2AuthorizationEndpointName|Koncový bod autorizace|  
|OAuth2TokenEndpointDescription|koncový bod tokenu Hello se používá ve hello klienta tooobtain přístupový token prezentací jeho autorizace udělit nebo aktualizovat token.|  
|OAuth2TokenEndpointName|Koncový bod tokenu|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_Description|< p\> hello klient zahájí hello toku přesměrováním vlastník prostředku hello koncový bod autorizace toohello uživatelského agenta.  Hello klienta zahrnuje jeho identifikátor klienta, požadovaném oboru, místní stavu a identifikátor URI přesměrování toowhich hello autorizace server odešle hello uživatelského agenta zpět, když je přístup povolen (nebo je odepřen).     < /p\> < p\> serveru ověřování hello ověřuje vlastník prostředku hello (prostřednictvím hello uživatelského agenta) a určuje, zda vlastník prostředku hello uděluje, nebo odmítne žádost o přístup hello klienta.     < /p\> < p\> za předpokladu, že vlastník prostředku hello uděluje přístup, server ověřování hello hello uživatelského agenta zpět toohello klienta pomocí přesměrování hello zadaný identifikátor URI přesměruje starší (hello požadavku nebo při klien registrace t).  identifikátor URI přesměrování Hello zahrnuje autorizační kód a žádný místní stav dříve poskytované hello klienta.     < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_ErrorDescription|< p\> Pokud hello uživatele odmítne požadavek hello přístup, pokud hello žádosti je neplatný, hello klienta budou informováni pomocí hello následující parametry přidat na přesměrování toohello: < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_Name|Žádost o ověření|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_RequestDescription|< p\> hello klientskou aplikaci, musí v pořadí tooinitiate hello OAuth proces odeslat hello koncový bod autorizace toohello uživatele.          Na koncový bod autorizace hello hello uživatel ověří a poté udělí nebo odmítne přístup toohello aplikace.     < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_ResponseDescription|< p\> za předpokladu, že vlastník prostředku hello uděluje přístup, server ověřování hello uživatelského agenta zpět toohello klienta pomocí přesměrování hello zadaný identifikátor URI přesměruje starší (v hello požadavku nebo během registrace klienta).  identifikátor URI přesměrování Hello zahrnuje autorizační kód a žádný místní stav dříve poskytované hello klienta. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_Description|< p\> hello klient požádá o token přístupu ze serveru ověřování hello'' koncový bod tokenu s zahrnutím hello autorizační kód obdrželi v předchozím kroku hello.  Při vytváření požadavku hello, hello klient se ověří pomocí serveru ověřování hello.  Hello client zahrnuje hello přesměrování identifikátor URI použitý tooobtain hello autorizační kód pro ověření. < /p\> < p\> serveru ověřování hello ověřuje hello klienta, ověří hello autorizační kód a zajistí, že hello identifikátor URI přesměrování odpovídá hello URI používané tooredirect hello klienta získaný v kroku (C).  Pokud je platná, hello autorizace server odpoví zpět token přístupu a volitelně token obnovení. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_ErrorDescription|< p\> Pokud ověření klienta hello žádosti se nezdařilo nebo je neplatný, serveru ověřování hello odpoví s kódem stavu HTTP 400 (Chybný požadavek) (Pokud není uvedeno jinak) a zahrnuje následující parametry s odpovědí hello hello. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_RequestDescription|< p\> hello klient podá tokenu koncový bod žádosti toohello odesláním hello následující parametry pomocí hello "application/x--www-form-urlencoded" Formát znaky kódování UTF-8 v entity textu hello HTTP žádosti. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_ResponseDescription|< p\> serveru ověřování hello problémy přístupový token a volitelné obnovovací token a konstrukce hello přidáním hello následující parametry toohello entity text odpovědi hello HTTP s 200 (OK) stavový kód odpovědi. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_Description|< p\> hello klienta ověří server hello ověřování a požadavků přístupový token hello koncový bod tokenu. < /p\> < p\> serveru ověřování hello ověřuje hello klienta a pokud je platná, vydá přístupový token. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_ErrorDescription|< p\> Pokud hello žádosti se nezdařilo ověření klienta nebo je neplatný hello autorizace server odpoví stavový kód HTTP 400 (Chybný požadavek) (Pokud není uvedeno jinak) a zahrnuje následující parametry s odpovědí hello hello. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_RequestDescription|< p\> hello klienta umožňuje tokenu koncový bod žádosti toohello přidáním hello následující parametry pomocí hello "application/x--www-form-urlencoded" Formát znaky kódování UTF-8 v entity textu hello HTTP žádosti. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_ResponseDescription|< p\> hello žádosti o token přístupu je platná a oprávnění, server ověřování hello problémy přístupový token a volitelné obnovovací token, a konstrukce hello přidáním hello následující parametry toohello prvku body hello HTTP odpovědi odpověď se 200 (OK) stavovým kódem. < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_Description|< p\> hello klient zahájí hello toku přesměrováním vlastník prostředku hello'' s koncový bod autorizace toohello uživatelského agenta.  Hello klienta zahrnuje jeho identifikátor klienta, požadovaném oboru, místní stavu a identifikátor URI přesměrování toowhich hello autorizace server odešle hello uživatelského agenta zpět, když je přístup povolen (nebo je odepřen). < /p\> < p\> serveru ověřování hello ověřuje vlastník prostředku hello (prostřednictvím hello uživatelského agenta) a určuje, zda vlastník prostředku hello povoluje nebo zakazuje hello klienta s s žádost o přístup. < /p\> < p\> za předpokladu, že vlastník prostředku hello uděluje přístup, server ověřování hello přesměruje hello uživatelského agenta zpět toohello klienta pomocí dříve zadaný identifikátor URI přesměrování hello.  identifikátor URI přesměrování Hello zahrnuje hello přístupový token v identifikátoru URI fragmentu hello. < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_ErrorDescription|< p\> Pokud vlastník prostředku hello odmítne žádost o přístup hello nebo pokud hello požadavek selže z jiných důvodů než přesměrování chybí nebo je neplatný identifikátor URI, serveru ověřování hello informuje klienta hello přidáním následující parametry toohello fragme hello součást NT pomocí identifikátoru URI přesměrování hello hello formátu "application/x--www-form-urlencoded". < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_RequestDescription|< p\> hello klientskou aplikaci, musí v pořadí tooinitiate hello OAuth proces odeslat hello koncový bod autorizace toohello uživatele.      Na koncový bod autorizace hello hello uživatel ověří a poté udělí nebo odmítne přístup toohello aplikace. < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_ResponseDescription|< p\> když vlastník prostředku hello uděluje hello žádost o přístup, server ověřování hello vydá přístupový token a doručí jej toohello klienta přidáním hello následující parametry toohello fragment součást URI pro přesměrování hello pomocí hello "a aplikačních/x--www-form-urlencoded"formát. < /p\>|  
|OAuth2Flow_ObtainAuthorization_AuthorizationCodeGrant_Description|Tok autorizačního kódu je optimalizovaná pro klienty nastavitelná hello důvěrnost své přihlašovací údaje (například aplikací webového serveru implementovaná pomocí PHP, Java, Python, Ruby, ASP.NET, atd.).|  
|OAuth2Flow_ObtainAuthorization_AuthorizationCodeGrant_Name|Udělení autorizačního kódu|  
|OAuth2Flow_ObtainAuthorization_ClientCredentialsGrant_Description|Tok přihlašovacích údajů klienta je vhodné v případech, kdy klient hello (aplikace) požaduje přístup k prostředkům toohello chráněné pod její kontrolou. Hello klienta bude považován za jako vlastníka prostředku, takže nebude vyžadována žádná interakce s koncovým uživatelem.|  
|OAuth2Flow_ObtainAuthorization_ClientCredentialsGrant_Name|Udělení pověření klienta|  
|OAuth2Flow_ObtainAuthorization_ImplicitGrant_Description|Implicitní tok je optimalizovaná pro klienti nepodporující zachování důvěrnosti hello známé toooperate jejich přihlašovací údaje konkrétní identifikátor URI přesměrování. Tito klienti jsou obvykle implementované v prohlížeči pomocí skriptovací jazyk, jako je JavaScript.|  
|OAuth2Flow_ObtainAuthorization_ImplicitGrant_Name|Implicitní grant|  
|OAuth2Flow_ObtainAuthorization_ResourceOwnerPasswordCredentialsGrant_Description|Tok přihlašovacích údajů heslo vlastníka prostředku je vhodné v případech, kdy vlastník prostředku hello obsahuje vztah důvěryhodnosti s klientem hello (aplikace), jako je například hello zařízení operačního systému nebo vysoce privilegované aplikace. Tento tok je vhodná pro klienty získat pověření vlastníka prostředku hello (uživatelské jméno a heslo, obvykle pomocí interaktivního formuláře).|  
|OAuth2Flow_ObtainAuthorization_ResourceOwnerPasswordCredentialsGrant_Name|Udělení oprávnění hesla vlastníka prostředku|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_Description|< p\> vlastník prostředku hello poskytuje hello klienta s jeho uživatelské jméno a heslo. < /p\> < p\> hello klient požádá o token přístupu ze serveru ověřování hello'' koncový bod tokenu s zahrnutím hello pověření přijata od vlastníka prostředku hello.  Při vytváření požadavku hello, hello klient se ověří pomocí serveru ověřování hello. < /p\> < p\> serveru ověřování hello ověřuje hello klienta a pověření vlastníka prostředku hello ověří a pokud je platná, vydá přístupový token. < /p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_ErrorDescription|< p\> Pokud hello žádosti se nezdařilo ověření klienta nebo je neplatný hello autorizace server odpoví stavový kód HTTP 400 (Chybný požadavek) (Pokud není uvedeno jinak) a zahrnuje následující parametry s odpovědí hello hello. < /p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_RequestDescription|< p\> hello klienta umožňuje tokenu koncový bod žádosti toohello přidáním hello následující parametry pomocí hello "application/x--www-form-urlencoded" Formát znaky kódování UTF-8 v entity textu hello HTTP žádosti. < /p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_ResponseDescription|< p\> hello žádosti o token přístupu je platná a oprávnění, server ověřování hello problémy přístupový token a volitelné obnovovací token, a konstrukce hello přidáním hello následující parametry toohello prvku body respo hello HTTP odpovědi nse se 200 (OK) stavovým kódem. < /p\>|  
|OAuth2Step_AccessTokenRequest_Name|Žádosti o token přístupu|  
|OAuth2Step_AuthorizationRequest_Name|Žádost o ověření|  
|OAuth2AccessToken_AuthorizationCodeGrant_TokenResponse|VYŽADUJE SE. přístupový token Hello vystavený hello serveru ověřování.|  
|OAuth2AccessToken_ClientCredentialsGrant_TokenResponse|VYŽADUJE SE. přístupový token Hello vystavený hello serveru ověřování.|  
|OAuth2AccessToken_ImplicitGrant_AuthorizationResponse|VYŽADUJE SE. přístupový token Hello vystavený hello serveru ověřování.|  
|OAuth2AccessToken_ResourceOwnerPasswordCredentialsGrant_TokenResponse|VYŽADUJE SE. přístupový token Hello vystavený hello serveru ověřování.|  
|OAuth2ClientId_AuthorizationCodeGrant_AuthorizationRequest|VYŽADUJE SE. Identifikátor klienta.|  
|OAuth2ClientId_AuthorizationCodeGrant_TokenRequest|VYŽADUJE, pokud není klient hello ověřování pomocí serveru ověřování hello.|  
|OAuth2ClientId_ImplicitGrant_AuthorizationRequest|VYŽADUJE SE. identifikátor klienta Hello.|  
|OAuth2Code_AuthorizationCodeGrant_AuthorizationResponse|VYŽADUJE SE. Hello autorizační kód generovaný serverem hello autorizace.|  
|OAuth2Code_AuthorizationCodeGrant_TokenRequest|VYŽADUJE SE. autorizační kód Hello přijal od serveru ověřování hello.|  
|OAuth2ErrorDescription_AuthorizationCodeGrant_AuthorizationErrorResponse|VOLITELNÝ PARAMETR. Čitelný text ASCII poskytuje další informace.|  
|OAuth2ErrorDescription_AuthorizationCodeGrant_TokenErrorResponse|VOLITELNÝ PARAMETR. Čitelný text ASCII poskytuje další informace.|  
|OAuth2ErrorDescription_ClientCredentialsGrant_TokenErrorResponse|VOLITELNÝ PARAMETR. Čitelný text ASCII poskytuje další informace.|  
|OAuth2ErrorDescription_ImplicitGrant_AuthorizationErrorResponse|VOLITELNÝ PARAMETR. Čitelný text ASCII poskytuje další informace.|  
|OAuth2ErrorDescription_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|VOLITELNÝ PARAMETR. Čitelný text ASCII poskytuje další informace.|  
|OAuth2ErrorUri_AuthorizationCodeGrant_AuthorizationErrorResponse|VOLITELNÝ PARAMETR. Identifikátor URI identifikující čitelná pro člověka webovou stránku s informacemi o chybě hello.|  
|OAuth2ErrorUri_AuthorizationCodeGrant_TokenErrorResponse|VOLITELNÝ PARAMETR. Identifikátor URI identifikující čitelná pro člověka webovou stránku s informacemi o chybě hello.|  
|OAuth2ErrorUri_ClientCredentialsGrant_TokenErrorResponse|VOLITELNÝ PARAMETR. Identifikátor URI identifikující čitelná pro člověka webovou stránku s informacemi o chybě hello.|  
|OAuth2ErrorUri_ImplicitGrant_AuthorizationErrorResponse|VOLITELNÝ PARAMETR. Identifikátor URI identifikující čitelná pro člověka webovou stránku s informacemi o chybě hello.|  
|OAuth2ErrorUri_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|VOLITELNÝ PARAMETR. Identifikátor URI identifikující čitelná pro člověka webovou stránku s informacemi o chybě hello.|  
|OAuth2Error_AuthorizationCodeGrant_AuthorizationErrorResponse|VYŽADUJE SE. Jeden kód chyby ASCII z následující hello: invalid_request, unauthorized_client, access_denied, unsupported_response_type, invalid_scope, server_error, temporarily_unavailable.|  
|OAuth2Error_AuthorizationCodeGrant_TokenErrorResponse|VYŽADUJE SE. Jeden kód chyby ASCII z následující hello: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2Error_ClientCredentialsGrant_TokenErrorResponse|VYŽADUJE SE. Jeden kód chyby ASCII z následující hello: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2Error_ImplicitGrant_AuthorizationErrorResponse|VYŽADUJE SE. Jeden kód chyby ASCII z následující hello: invalid_request, unauthorized_client, access_denied, unsupported_response_type, invalid_scope, server_error, temporarily_unavailable.|  
|OAuth2Error_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|VYŽADUJE SE. Jeden kód chyby ASCII z následující hello: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2ExpiresIn_AuthorizationCodeGrant_TokenResponse|NEDOPORUČUJE. Doba platnosti Hello v sekundách hello přístupový token.|  
|OAuth2ExpiresIn_ClientCredentialsGrant_TokenResponse|NEDOPORUČUJE. Doba platnosti Hello v sekundách hello přístupový token.|  
|OAuth2ExpiresIn_ImplicitGrant_AuthorizationResponse|NEDOPORUČUJE. Doba platnosti Hello v sekundách hello přístupový token.|  
|OAuth2ExpiresIn_ResourceOwnerPasswordCredentialsGrant_TokenResponse|NEDOPORUČUJE. Doba platnosti Hello v sekundách hello přístupový token.|  
|OAuth2GrantType_AuthorizationCodeGrant_TokenRequest|VYŽADUJE SE. Hodnota musí být nastavena příliš "authorization_code".|  
|OAuth2GrantType_ClientCredentialsGrant_TokenRequest|VYŽADUJE SE. Hodnota musí být nastavena příliš "client_credentials".|  
|OAuth2GrantType_ResourceOwnerPasswordCredentialsGrant_TokenRequest|VYŽADUJE SE. Hodnota musí být nastavena příliš "password".|  
|OAuth2Password_ResourceOwnerPasswordCredentialsGrant_TokenRequest|VYŽADUJE SE. heslo vlastníka prostředku Hello.|  
|OAuth2RedirectUri_AuthorizationCodeGrant_AuthorizationRequest|VOLITELNÝ PARAMETR. koncový bod přesměrování Hello identifikátor URI musí být absolutní identifikátor URI.|  
|OAuth2RedirectUri_AuthorizationCodeGrant_TokenRequest|POŽADOVANÉ Pokud parametr "redirect_uri" hello je zahrnutý v hello požadavek ověřování, a jejich hodnoty musí být identické.|  
|OAuth2RedirectUri_ImplicitGrant_AuthorizationRequest|VOLITELNÝ PARAMETR. koncový bod přesměrování Hello identifikátor URI musí být absolutní identifikátor URI.|  
|OAuth2RefreshToken_AuthorizationCodeGrant_TokenResponse|VOLITELNÝ PARAMETR. Hello obnovovací token, který lze použít tooobtain nové přístupové tokeny.|  
|OAuth2RefreshToken_ClientCredentialsGrant_TokenResponse|VOLITELNÝ PARAMETR. Hello obnovovací token, který lze použít tooobtain nové přístupové tokeny.|  
|OAuth2RefreshToken_ResourceOwnerPasswordCredentialsGrant_TokenResponse|VOLITELNÝ PARAMETR. Hello obnovovací token, který lze použít tooobtain nové přístupové tokeny.|  
|OAuth2ResponseType_AuthorizationCodeGrant_AuthorizationRequest|VYŽADUJE SE. Hodnota musí být nastavena příliš "kódu".|  
|OAuth2ResponseType_ImplicitGrant_AuthorizationRequest|VYŽADUJE SE. Hodnota musí být nastaven příliš "token".|  
|OAuth2Scope_AuthorizationCodeGrant_AuthorizationRequest|VOLITELNÝ PARAMETR. rozsah Hello hello žádost o přístup.|  
|OAuth2Scope_AuthorizationCodeGrant_TokenResponse|VOLITELNÉ, pokud obor identické toohello požadovaný klientem hello; VYŽADUJE, jinak hodnota.|  
|OAuth2Scope_ClientCredentialsGrant_TokenRequest|VOLITELNÝ PARAMETR. rozsah Hello hello žádost o přístup.|  
|OAuth2Scope_ClientCredentialsGrant_TokenResponse|VOLITELNÉ, pokud je to stejné oboru toohello požadovaný klientem hello; VYŽADUJE, jinak hodnota.|  
|OAuth2Scope_ImplicitGrant_AuthorizationRequest|VOLITELNÝ PARAMETR. rozsah Hello hello žádost o přístup.|  
|OAuth2Scope_ImplicitGrant_AuthorizationResponse|VOLITELNÉ, pokud obor identické toohello požadovaný klientem hello; VYŽADUJE, jinak hodnota.|  
|OAuth2Scope_ResourceOwnerPasswordCredentialsGrant_TokenRequest|VOLITELNÝ PARAMETR. rozsah Hello hello žádost o přístup.|  
|OAuth2Scope_ResourceOwnerPasswordCredentialsGrant_TokenResponse|VOLITELNÉ, pokud je to stejné oboru toohello požadovaný klientem hello; VYŽADUJE, jinak hodnota.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationErrorResponse|VYŽADUJE, pokud parametr "stavu" hello nacházel v hello žádost o ověření klienta.  Hello přijatých od klienta hello přesná hodnota.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationRequest|NEDOPORUČUJE. Neprůhledné hodnoty, která používá toomaintain stavu klienta hello mezi hello požadavku a zpětného volání.  server Hello ověřování zahrnuje tato hodnota při přesměrování hello uživatelského agenta zpět toohello klienta.  Parametr Hello je třeba použít brání padělání požadavku posílaného mezi weby.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationResponse|VYŽADUJE, pokud parametr "stavu" hello nacházel v hello žádost o ověření klienta.  Hello přijatých od klienta hello přesná hodnota.|  
|OAuth2State_ImplicitGrant_AuthorizationErrorResponse|VYŽADUJE, pokud parametr "stavu" hello nacházel v hello žádost o ověření klienta.  Hello přijatých od klienta hello přesná hodnota.|  
|OAuth2State_ImplicitGrant_AuthorizationRequest|NEDOPORUČUJE. Neprůhledné hodnoty, která používá toomaintain stavu klienta hello mezi hello požadavku a zpětného volání.  server Hello ověřování zahrnuje tato hodnota při přesměrování hello uživatelského agenta zpět toohello klienta.  Parametr Hello je třeba použít brání padělání požadavku posílaného mezi weby.|  
|OAuth2State_ImplicitGrant_AuthorizationResponse|VYŽADUJE, pokud parametr "stavu" hello nacházel v hello žádost o ověření klienta.  Hello přijatých od klienta hello přesná hodnota.|  
|OAuth2TokenType_AuthorizationCodeGrant_TokenResponse|VYŽADUJE SE. Hello typem vydávaných tokenů hello.|  
|OAuth2TokenType_ClientCredentialsGrant_TokenResponse|VYŽADUJE SE. Hello typem vydávaných tokenů hello.|  
|OAuth2TokenType_ImplicitGrant_AuthorizationResponse|VYŽADUJE SE. Hello typem vydávaných tokenů hello.|  
|OAuth2TokenType_ResourceOwnerPasswordCredentialsGrant_TokenResponse|VYŽADUJE SE. Hello typem vydávaných tokenů hello.|  
|OAuth2UserName_ResourceOwnerPasswordCredentialsGrant_TokenRequest|VYŽADUJE SE. uživatelské jméno vlastníka prostředku Hello.|  
|OAuth2UnsupportedTokenType|Typ tokenu {0}' není supporetd.|  
|OAuth2InvalidState|Neplatná odpověď ze serveru ověřování|  
|OAuth2GrantType_AuthorizationCode|Autorizační kód|  
|OAuth2GrantType_Implicit|Implicitní|  
|OAuth2GrantType_ClientCredentials|Pověření klienta|  
|OAuth2GrantType_ResourceOwnerPassword|Heslo vlastníka prostředku|  
|WebDocumentation302Code|302 Found|  
|WebDocumentation400Code|400 (Chybný požadavek)|  
|OAuth2SendingMethod_AuthHeader|Autorizační hlavičky|  
|OAuth2SendingMethod_QueryParam|Parametr dotazu|  
|OAuth2AuthorizationServerGeneralException|Došlo k chybě při autorizaci přístupu prostřednictvím {0}|  
|OAuth2AuthorizationServerCommunicationException|Nelze navázat server tooauthorization připojení HTTP nebo neočekávaně bylo ukončeno.|  
|WebDocumentationOAuth2GeneralErrorMessage|Došlo k neočekávané chybě.|  
|AuthorizationServerCommunicationException|Došlo k výjimce komunikace serveru autorizace. Obraťte se na správce.|  
|TextblockSubscriptionKeyHeaderDescription|Klíč předplatného, které poskytuje rozhraní API toothis přístup. Najít v vaší < href = "/ vývojáře '\>profil < /a\>.|  
|TextblockOAuthHeaderDescription|Přístupový token OAuth 2.0 získané z < i\>{0} < /i\>. Podporované typy udělení: < i\>{1} < /i\>.|  
|TextblockContentTypeHeaderDescription|Typ média textu hello odeslané toohello rozhraní API.|  
|ErrorMessageApiNotAccessible|Hello rozhraní API, které se pokoušíte toocall není momentálně přístupné. Obraťte se na vydavatele hello rozhraní API < href = "/ problémy"\>sem < /a\>.|  
|ErrorMessageApiTimedout|Hello rozhraní API, které se pokoušíte toocall trvá déle než normální tooget odpověď zpět. Obraťte se na vydavatele hello rozhraní API < href = "/ problémy"\>sem < /a\>.|  
|BadRequestParameterExpected|"očekává se parametr {0}."|  
|TooltipTextDoubleClickToSelectAll|Dvakrát klikněte na tlačítko tooselect všechny.|  
|TooltipTextHideRevealSecret|Zobrazit či skrýt|  
|ButtonLinkOpenConsole|Vyzkoušet|  
|SectionHeadingRequestBody|Text žádosti|  
|SectionHeadingRequestParameters|Parametry žádosti|  
|SectionHeadingRequestUrl|Adresa URL požadavku|  
|SectionHeadingResponse|Odpověď|  
|SectionHeadingRequestHeaders|Hlavičky požadavku|  
|FormLabelSubtextOptional|Volitelné|  
|SectionHeadingCodeSamples|Ukázky kódů|  
|TextblockOpenidConnectHeaderDescription|OpenID Connect id token získaný < i\>{0} < /i\>. Podporované typy udělení: < i\>{1} < /i\>.|  
  
###  <a name="ErrorPageStrings"></a>ErrorPageStrings  
  
|Name (Název)|Text|  
|----------|----------|  
|LinkLabelBack|zpět|  
|LinkLabelHomePage|Domovská stránka|  
|LinkLabelSendUsEmail|Pošlete nám e-mail|  
|PageTitleError|Je nám líto došlo požadované stránce problém slouží hello|  
|TextblockPotentialCauseIntermittentIssue|To může být problém přístup přerušované dat, který je již pryč.|  
|TextblockPotentialCauseOldLink|může být Hello odkazu, který jste klikli na původním a není bod toohello už opravte umístění.|  
|TextblockPotentialCauseTechnicalProblem|Na naší straně může být k technickým potížím.|  
|TextblockPotentialSolutionRefresh|Zkuste aktualizovat stránku hello.|  
|TextblockPotentialSolutionStartOver|Začněte znovu z našich {0}.|  
|TextblockPotentialSolutionTryAgain|Přejděte {0} a zkuste to znovu provedené akce hello.|  
|TextReportProblem|{0} popisující, co došlo k chybě a My bude vypadat na to, co nejrychleji můžeme.|  
|TitlePotentialCause|Možná příčina|  
|TitlePotentialSolution|Je pravděpodobně právě dočasný problém, několik věcí tootry|  
  
###  <a name="IssuesStrings"></a>IssuesStrings  
  
|Name (Název)|Text|  
|----------|----------|  
|WebIssuesIndexTitle|Problémy|  
|WebIssuesNoActiveSubscriptions|Nemáte žádné aktivní odběry. Potřebujete toosubscribe pro produkt tooreport problém.|  
|WebIssuesNotSignin|Nejste přihlášení. Prosím {0} tooreport problém nebo odeslat komentář.|  
|WebIssuesReportIssueButton|Sestava problém|  
|WebIssuesSignIn|Přihlášení|  
|WebIssuesStatusReportedBy|Stav: {0} &#124; Hlášené {1}|  
  
###  <a name="NotFoundStrings"></a>NotFoundStrings  
  
|Name (Název)|Text|  
|----------|----------|  
|LinkLabelHomePage|Domovská stránka|  
|LinkLabelSendUsEmail|Pošlete nám e-mail|  
|PageTitleNotFound|Je nám líto Nemůžeme najít hello stránku, kterou hledáte|  
|TextblockPotentialCauseMisspelledUrl|Možná je chybně hello URL Pokud jste zadali v.|  
|TextblockPotentialCauseOldLink|může být Hello odkazu, který jste klikli na původním a není bod toohello už opravte umístění.|  
|TextblockPotentialSolutionRetype|Zkuste znovu napsat hello adresy URL.|  
|TextblockPotentialSolutionStartOver|Začněte znovu z našich {0}.|  
|TextReportProblem|{0} popisující, co došlo k chybě a My bude vypadat na to, co nejrychleji můžeme.|  
|TitlePotentialCause|Možná příčina|  
|TitlePotentialSolution|Možné řešení|  
  
###  <a name="ProductDetailsStrings"></a>ProductDetailsStrings  
  
|Name (Název)|Text|  
|----------|----------|  
|WebProductsAgreement|Pomocí přihlášení k odběru příliš {0} produktu, souhlasím toohello `<a data-toggle='modal' href='#legal-terms'\>Terms of Use</a\>`.|  
|WebProductsLegalTermsLink|Podmínky použití|  
|WebProductsSubscribeButton|Přihlásit odběr|  
|WebProductsUsageLimitsHeader|Omezení využití|  
|WebProductsYouAreNotSubscribed|Jste odebírané toothis produktu.|  
|WebProductsYouRequestedSubscription|Požadovaná předplatné toothis produktu.|  
|ErrorYouNeedtoAgreeWithLegalTerms|Abyste mohli pokračovat, musíte souhlasit toohello podmínky použití.|  
|ButtonLabelAddSubscription|Přidat předplatné|  
|LinkLabelChangeSubscriptionName|změnit|  
|ButtonLabelConfirm|Potvrďte|  
|TextblockMultipleSubscriptionsCount|Máte {0} odběry toothis produktu:|  
|TextblockSingleSubscriptionsCount|Máte {0} předplatné toothis produktu:|  
|TextblockSingleApisCount|Tento produkt obsahuje {0} rozhraní API:|  
|TextblockMultipleApisCount|Tento produkt obsahuje {0} rozhraní API:|  
|TextblockHeaderSubscribe|Přihlášení k odběru tooproduct|  
|TextblockSubscriptionDescription|Nový odběr bude vytvořen následujícím způsobem:|  
|TextblockSubscriptionLimitReached|Dosažen limit odběry.|  
  
###  <a name="ProductsStrings"></a>ProductsStrings  
  
|Name (Název)|Text|  
|----------|----------|  
|PageTitleProducts|Produkty|  
  
###  <a name="ProviderInfoStrings"></a>ProviderInfoStrings  
  
|Name (Název)|Text|  
|----------|----------|  
|TextboxExternalIdentitiesDisabled|Přihlášení je zakázané hello správci momentálně hello.|  
|TextboxExternalIdentitiesSigninInvitation|Případně Přihlaste se pomocí|  
|TextboxExternalIdentitiesSigninInvitationPrimary|Přihlaste se pomocí:|  
  
###  <a name="SigninResources"></a>SigninResources  
  
|Name (Název)|Text|  
|----------|----------|  
|PrincipalNotFound|Objekt nebyl nalezen nebo pokud podpis je neplatný|  
|ErrorSsoAuthenticationFailed|Jednotné přihlašování ověřování se nezdařilo.|  
|ErrorSsoAuthenticationFailedDetailed|Neplatný token poskytnutý nebo podpis nelze ověřit.|  
|ErrorSsoTokenInvalid|Token jednotného přihlašování je neplatný|  
|ValidationErrorSpecificEmailAlreadyExists|E-mailu {0}' již zaregistrován.|  
|ValidationErrorSpecificEmailInvalid|E-mailu: {0} je neplatný|  
|ValidationErrorPasswordInvalid|Heslo je neplatné. Opravte chyby hello a zkuste to znovu.|  
|PropertyTooShort|{0} je příliš krátké.|  
|WebAuthenticationAddresserEmailInvalidErrorMessage|Neplatná e-mailová adresa.|  
|ValidationMessageNewPasswordConfirmationRequired|Potvrzení nového hesla|  
|ValidationErrorPasswordConfirmationRequired|Potvrďte, že heslo je prázdné.|  
|WebAuthenticationEmailChangeNotice|Změna potvrzení e-mailu je na hello příliš {0}. Postupujte podle pokynů v něm tooconfirm novou e-mailovou adresu. Pokud e-mailu hello nedorazil tooyour doručených zpráv v hello další několik minut, zkontrolujte složku vašich nevyžádanou poštou.|  
|WebAuthenticationEmailChangeNoticeHeader|Vaše žádost o změnu e-mailu byl úspěšně zpracován|  
|WebAuthenticationEmailChangeNoticeTitle|Požadovaná změna e-mailu|  
|WebAuthenticationEmailHasBeenRevertedNotice|E-mail je již existuje. Žádost o byly vráceny zpět.|  
|ValidationErrorEmailAlreadyExists|E-mailu již existuje.|  
|ValidationErrorEmailInvalid|Neplatná e-mailová adresa|  
|TextboxLabelEmail|E-mail|  
|ValidationErrorEmailRequired|Vyžaduje se e-mailu.|  
|WebAuthenticationErrorNoticeHeader|Chyba|  
|WebAuthenticationFieldLengthErrorMessage|{0} musí být o maximální délce {1}|  
|TextboxLabelEmailFirstName|Jméno|  
|ValidationErrorFirstNameRequired|Jméno je povinné.|  
|ValidationErrorFirstNameInvalid|Neplatný křestní jméno|  
|NoticeInvalidInvitationToken|Upozorňujeme, že potvrzení odkazy jsou platné pouze 48 hodin. Pokud jste stále v rámci této časový rámec, Zkontrolujte prosím, že odkaz na vaši správná. Pokud vypršela platnost odkaz na vaši, opakujte hello akce, které se snažíte tooconfirm.|  
|NoticeHeaderInvalidInvitationToken|Neplatný pozvánku tokenu|  
|NoticeTitleInvalidInvitationToken|Chyba potvrzení|  
|WebAuthenticationLastNameInvalidErrorMessage|Neplatný název poslední|  
|TextboxLabelEmailLastName|Příjmení|  
|ValidationErrorLastNameRequired|Příjmení je povinné.|  
|WebAuthenticationLinkExpiredNotice|Potvrzení odkaz Odeslat tooyou vypršela platnost. `<a href={0}?token={1}>Resend confirmation email.</a\>`|  
|NoticePasswordResetLinkInvalidOrExpired|Odkaz na vaši resetování hesla je neplatná nebo vypršela její platnost.|  
|WebAuthenticationLinkExpiredNoticeTitle|Odkaz Odeslat|  
|WebAuthenticationNewPasswordLabel|Nové heslo|  
|ValidationMessageNewPasswordRequired|Nového hesla je povinné.|  
|TextboxLabelNotificationsSenderEmail|Oznámení odesílatele e-mailu|  
|TextboxLabelOrganizationName|Název organizace|  
|WebAuthenticationOrganizationRequiredErrorMessage|Název organizace je prázdný.|  
|WebAuthenticationPasswordChangedNotice|Heslo se úspěšně aktualizoval.|  
|WebAuthenticationPasswordChangedNoticeTitle|Heslo aktualizovat|  
|WebAuthenticationPasswordCompareErrorMessage|Hesla se neshodují.|  
|WebAuthenticationPasswordConfirmLabel|Potvrzení hesla|  
|ValidationErrorPasswordInvalidDetailed|Heslo je příliš slabé.|  
|WebAuthenticationPasswordLabel|Heslo|  
|ValidationErrorPasswordRequired|Je vyžadováno heslo.|  
|WebAuthenticationPasswordResetSendNotice|Změna hesla potvrzení e-mailu je na hello příliš {0}. Postupujte podle pokynů hello v rámci e-mailu toocontinue hello váš proces změny hesla.|  
|WebAuthenticationPasswordResetSendNoticeHeader|Vaše žádost o resetování hesla byl úspěšně zpracován.|  
|WebAuthenticationPasswordResetSendNoticeTitle|Požádáno o obnovení hesla|  
|WebAuthenticationRequestNotFoundNotice|Nenašla se žádost|  
|WebAuthenticationSenderEmailRequiredErrorMessage|Oznámení odesílatele e-mailu je prázdná|  
|WebAuthenticationSigninPasswordLabel|Potvrďte změnu hello zadáním hesla|  
|WebAuthenticationSignupConfirmNotice|Registrace potvrzení e-mailu je na cestě příliš {0}. < br /\> prosím postupujte podle pokynů v rámci hello e-mailu tooactivate vašeho účtu. < br /\> Pokud e-mailu hello nedorazil ve vaší doručené poště v rámci hello další několik minut, zkontrolujte složku vašich nevyžádanou poštou.|  
|WebAuthenticationSignupConfirmNoticeHeader|Váš účet byl úspěšně vytvořen.|  
|WebAuthenticationSignupConfirmNoticeRepeatHeader|Znovu odeslal registrační potvrzení e-mailu|  
|WebAuthenticationSignupConfirmNoticeTitle|Vytvoření účtu|  
|WebAuthenticationTokenRequiredErrorMessage|Token je prázdná|  
|WebAuthenticationUserAlreadyRegisteredNotice|Zdá se, že uživatel s Tento e-mail je již zaregistrován v systému hello. Pokud jste heslo zapomněli, zkuste to prosím toorestore ji nebo kontaktujte náš tým podpory.|  
|WebAuthenticationUserAlreadyRegisteredNoticeHeader|Uživatel již zaregistrován.|  
|WebAuthenticationUserAlreadyRegisteredNoticeTitle|Zaregistrována.|  
|ButtonLabelChangePassword|Změnit heslo|  
|ButtonLabelChangeAccountInfo|Změnit informace o účtu|  
|ButtonLabelCloseAccount|Zavřít účet|  
|WebAuthenticationInvalidCaptchaErrorMessage|Zadaný text neodpovídá text na obrázek hello. Zkuste to prosím znovu.|  
|ValidationErrorCredentialsInvalid|E-mailu nebo heslo je neplatné. Opravte chyby hello a zkuste to znovu.|  
|WebAuthenticationRequestIsNotValid|Požadavek není platný|  
|WebAuthenticationUserIsNotConfirm|Před pokusem o toosign v potvrďte registrace.|  
|WebAuthenticationInvalidEmailFormated|E-mailu je neplatná: {0}|  
|WebAuthenticationUserNotFound|Uživatel se nenašel.|  
|WebAuthenticationTenantNotRegistered|Váš účet patří tooa klienta Azure Active Directory, která není autorizovaný tooaccess tento portál.|  
|WebAuthenticationAuthenticationFailed|Ověřování se nezdařilo.|  
|WebAuthenticationGooglePlusNotEnabled|Ověřování se nezdařilo. Pokud aplikace hello oprávnění a obraťte se na správce toomake hello opravdu, Google ověření správně nakonfigurován.|  
|ValidationErrorAllowedTenantIsRequired|Je vyžadován povolené klienta|  
|ValidationErrorTenantIsNotValid|Hello klienta Azure Active Directory: {0} není platný.|  
|WebAuthenticationActiveDirectoryTitle|Azure Active Directory|  
|WebAuthenticationLoginUsingYourProvider|Přihlaste se pomocí účtu {0}|  
|WebAuthenticationUserLimitNotice|Tato služba byl dosažen maximální počet povolených uživatelů hello. Prosím `<a href="mailto:{0}"\>contact hello administrator</a\>` tooupgrade jejich registrace uživatele služby a znovu povolte.|  
|WebAuthenticationUserLimitNoticeHeader|Registrace uživatele byla zakázána.|  
|WebAuthenticationUserLimitNoticeTitle|Registrace uživatele byla zakázána.|  
|WebAuthenticationUserRegistrationDisabledNotice|Registrace uživatelů byla zakázána správcem hello. Přihlaste se pomocí zprostředkovatele externí identity.|  
|WebAuthenticationUserRegistrationDisabledNoticeHeader|Registrace uživatele byla zakázána.|  
|WebAuthenticationUserRegistrationDisabledNoticeTitle|Registrace uživatele byla zakázána.|  
|WebAuthenticationSignupPendingConfirmationNotice|Předtím, než dokončíme hello vytvoření vašeho účtu potřebujeme tooverify e-mailovou adresu. Poslali jsme vám e-mailu příliš {0}. Prosím podle pokynů hello uvnitř hello e-mailu tooactivate váš účet. Pokud e-mailu hello není v rámci hello přicházejí další několik minut, Zkontrolujte prosím složce nevyžádanou poštou.|  
|WebAuthenticationSignupPendingConfirmationAccountFoundNotice|Zjistili jsme nepotvrzeného účet pro e-mailovou adresu hello {0}. vytváření hello toocomplete svého účtu, že potřebujeme tooverify e-mailovou adresu. Poslali jsme vám e-mailu příliš {0}. Prosím podle pokynů hello uvnitř hello e-mailu tooactivate váš účet. Pokud e-mailu hello není v rámci hello přicházejí další několik minut, Zkontrolujte prosím své nevyžádanou poštou e-mailové složky|  
|WebAuthenticationSignupConfirmationAlmostDone|Skoro Hotovo|  
|WebAuthenticationSignupConfirmationEmailSent|Poslali jsme vám e-mailu příliš {0}. Prosím podle pokynů hello uvnitř hello e-mailu tooactivate váš účet. Pokud e-mailu hello není v rámci hello přicházejí další několik minut, Zkontrolujte prosím složce nevyžádanou poštou.|  
|WebAuthenticationEmailSentNotificationMessage|Příliš úspěšně odeslat e-mailu {0}|  
|WebAuthenticationNoAadTenantConfigured|Žádné nakonfigurované pro službu hello klienta Azure Active Directory.|  
|CheckboxLabelUserRegistrationTermsConsentRequired|Souhlasím toohello `<a data-toggle="modal" href="#" data-target="#terms"\>Terms of Use</a\>`.|  
|TextblockUserRegistrationTermsProvided|Přečtěte si`<a data-toggle="modal" href="#" data-target="#terms"\>Terms of Use.</a\>`|  
|DialogHeadingTermsOfUse|Podmínky použití|  
|ValidationMessageConsentNotAccepted|Abyste mohli pokračovat, musíte souhlasit toohello podmínky použití.|  
  
###  <a name="SigninStrings"></a>SigninStrings  
  
|Name (Název)|Text|  
|----------|----------|  
|WebAuthenticationForgotPassword|Zapomněli jste heslo?|  
|WebAuthenticationIfAdministrator|Pokud jste správce musíte se přihlásit `<a href="{0}"\>here</a\>`.|  
|WebAuthenticationNotAMember|Není členem ještě? `<a href="/signup"\>Sign up now</a\>`|  
|WebAuthenticationRemember|Zapamatovat uživatele na tomto počítači|  
|WebAuthenticationSigininWithPassword|Přihlaste se pomocí uživatelského jména a hesla|  
|WebAuthenticationSigninTitle|Přihlášení|  
|WebAuthenticationSignUpNow|Zaregistrujte se|  
  
###  <a name="SignupStrings"></a>SignupStrings  
  
|Name (Název)|Text|  
|----------|----------|  
|PageTitleSignup|Registrace|  
|WebAuthenticationAlreadyAMember|Již členem?|  
|WebAuthenticationCreateNewAccount|Vytvořit nový účet API Management|  
|WebAuthenticationSigninNow|Přihlaste se nyní|  
|ButtonLabelSignup|Registrace|  
  
###  <a name="SubscriptionListStrings"></a>SubscriptionListStrings  
  
|Name (Název)|Text|  
|----------|----------|  
|SubscriptionCancelConfirmation|Jste si jisti, že chcete toocancel tento odběr?|  
|SubscriptionRenewConfirmation|Jste si jisti, že chcete toorenew tento odběr?|  
|WebDevelopersManageSubscriptions|Správa předplatných|  
|WebDevelopersPrimaryKey|Primární klíč|  
|WebDevelopersRegenerateLink|Znovu vytvořit|  
|WebDevelopersSecondaryKey|Sekundární klíč|  
|ButtonLabelShowKey|Zobrazit|  
|ButtonLabelRenewSubscription|Obnovit|  
|WebDevelopersSubscriptionReqested|Požadovaný {0}|  
|WebDevelopersSubscriptionRequestedState|Požadovaný|  
|WebDevelopersSubscriptionTableNameHeader|Name (Název)|  
|WebDevelopersSubscriptionTableStateHeader|Stav|  
|WebDevelopersUsageStatisticsLink|Sestavy analýzy|  
|WebDevelopersYourSubscriptions|Předplatné|  
|SubscriptionPropertyLabelRequestedDate|Požadovaný|  
|SubscriptionPropertyLabelStartedDate|Bylo zahájeno|  
|PageTitleRenameSubscription|Přejmenujte předplatného|  
|SubscriptionPropertyLabelName|Název odběru|  
  
###  <a name="SubscriptionStrings"></a>SubscriptionStrings  
  
|Name (Název)|Text|  
|----------|----------|  
|SectionHeadingCloseAccount|Vyhledávání tooclose váš účet?|  
|PageTitleDeveloperProfile|Profil|  
|ButtonLabelHideKey|Skrýt|  
|ButtonLabelRegenerateKey|Znovu vytvořit|  
|InformationMessageKeyWasRegenerated|Jste si jisti, že chcete tooregenerate tento klíč?|  
|ButtonLabelShowKey|Zobrazit|  
  
###  <a name="UpdateProfileStrings"></a>UpdateProfileStrings  
  
|Name (Název)|Text|  
|----------|----------|  
|ButtonLabelUpdateProfile|Aktualizovat profil|  
|PageTitleUpdateProfile|Aktualizovat informace o účtu|  
  
###  <a name="UserProfile"></a>UserProfile  
  
|Name (Název)|Text|  
|----------|----------|  
|ButtonLabelChangeAccountInfo|Změnit informace o účtu|  
|ButtonLabelChangePassword|Změnit heslo|  
|ButtonLabelCloseAccount|Zavřít účet|  
|TextboxLabelEmail|E-mail|  
|TextboxLabelEmailFirstName|Jméno|  
|TextboxLabelEmailLastName|Příjmení|  
|TextboxLabelNotificationsSenderEmail|Oznámení odesílatele e-mailu|  
|TextboxLabelOrganizationName|Název organizace|  
|SubscriptionStateActive|Aktivní|  
|SubscriptionStateCancelled|Zrušena|  
|SubscriptionStateExpired|Platnost|  
|SubscriptionStateRejected|Odmítnut|  
|SubscriptionStateRequested|Požadovaný|  
|SubscriptionStateSuspended|pozastaveno|  
|DefaultSubscriptionNameTemplate|{0} (výchozí)|  
|SubscriptionNameTemplate|Přístup vývojáře #{0}|  
|TextboxLabelSubscriptionName|Název odběru|  
|ValidationMessageSubscriptionNameRequired|Název odběru nemůže být prázdný.|  
|ApiManagementUserLimitReached|Tato služba byl dosažen maximální počet povolených uživatelů hello. Upgradujte tooa vyšší cenová úroveň.|  
  
##  <a name="glyphs"></a>Glyfy prostředky  
 Šablony portálu vývojáře API Management můžete použít hello glyfů z [Glyphicons z Bootstrap](http://getbootstrap.com/components/#glyphicons). Tato sada glyfů obsahuje více než 250 glyfů ve formátu písma z hello [Glyphicon](http://glyphicons.com/) Halflings nastavit. toouse glyf z tohoto nastavení, použijte následující syntaxi hello.  
  
```html  
<span class="glyphicon glyphicon-user">  
```  
  
 Úplný seznam hello glyfů najdete v tématu [Glyphicons z Bootstrap](http://getbootstrap.com/components/#glyphicons).

## <a name="next-steps"></a>Další kroky
Další informace o práci se šablonami najdete v tématu [jak toocustomize hello portál pro vývojáře API Management pomocí šablon](api-management-developer-portal-templates.md).
