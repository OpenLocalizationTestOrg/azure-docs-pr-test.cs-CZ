---
title: "Deklarace identity mapování v Azure Active Directory (verze public preview) | Microsoft Docs"
description: "Tato stránka popisuje mapování deklarace Azure Active Directory."
services: active-directory
author: billmath
manager: femila
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: billmath
ms.openlocfilehash: ff07b9954d5c2ce71ab0ffd0db49fde15f323586
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="claims-mapping-in-azure-active-directory-public-preview"></a>Deklarace identity mapování v Azure Active Directory (verze public preview)

>[!NOTE]
>Tato funkce nahrazuje a nahrazuje hello [deklarací přizpůsobení](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization) nabízeným přes portál hello ještě dnes. Pokud upravíte deklarace identity pomocí portálu hello kromě toohello metoda grafu nebo PowerShell zmíněné v tomto dokumentu na hello stejné aplikaci, tokeny vydané pro tuto aplikaci bude ignorovat hello konfigurací na portálu hello.
Konfigurace provedená prostřednictvím metody hello zmíněné v tomto dokumentu se neprojeví v portálu hello.

Tato funkce používá klienta admins toocustomize hello deklarací vygenerované v tokeny pro konkrétní aplikaci daného klienta. Deklarace identity mapování zásady, které můžete použít:

- Vyberte deklarace, které jsou součástí tokeny.
- Vytvoření typů deklarací identity, které již neexistují.
- Zvolte nebo změnit hello zdroj dat vygenerované v konkrétní deklarací identity.

>[!NOTE]
>Tato funkce je aktuálně ve verzi public preview. Připravit toorevert nebo odeberte všechny změny. Hello funkce je dostupná v libovolné předplatné služby Azure Active Directory (Azure AD) verzi public Preview. Když funkce hello je obecně dostupná, může vyžadovat některé aspekty funkcí hello však předplatné služby Azure Active Directory premium.

## <a name="claims-mapping-policy-type"></a>Mapování typu zásady deklarace identity
Ve službě Azure AD **zásad** objekt představuje sadu pravidel vynucené u jednotlivých aplikací, nebo na všechny aplikace v organizaci. Jednotlivé typy zásad obsahuje strukturu jedinečnou sadu vlastností, které jsou pak použita toowhich tooobjects, které jsou přiřazeni.

Deklarace A mapování zásad je typ **zásad** objekt, který upravuje hello deklarací vygenerované v tokeny vydané pro konkrétní aplikace.

## <a name="claim-sets"></a>Sady deklarací identity
Existují určité sady deklarací identity, které definují, jak a kdy se používají v tokenech.

### <a name="core-claim-set"></a>Základní sady deklarací.
Deklarace identity v základní hello deklarace sady jsou k dispozici v každé tokenu, bez ohledu na zásady. Tyto deklarace identity jsou také považovány za omezený a nemůže být upraven.

### <a name="basic-claim-set"></a>Sada základní deklarací identity
Hello základní deklarace sada obsahuje deklarace identity hello vydávané ve výchozím nastavení pro tokeny (v sadě deklarací základní přidání toohello). Tyto deklarace můžete tento parametr vynechán nebo upravit pomocí deklarace identity hello mapování zásad.

### <a name="restricted-claim-set"></a>Sada deklarací identity s omezeným přístupem
Pomocí zásad nemůže být upraven deklarace identity s omezeným přístupem. zdroj dat Hello nelze změnit, a při generování tyto deklarace identity, použije se žádná transformace.

#### <a name="table-1-json-web-token-jwt-restricted-claim-set"></a>Tabulka 1: JSON Web Token (JWT) omezené sady deklarací.
|Typ deklarace identity (název)|
| ----- |
|_claim_names|
|_claim_sources|
|access_token|
|account_type|
|acr|
|Objektu actor|
|actortoken|
|AIO|
|altsecid|
|AMR|
|app_chain|
|app_displayname|
|app_res|
|appctx|
|appctxsender|
|AppID|
|appidacr|
|Kontrolní výraz|
|at_hash|
|oblast|
|auth_data|
|auth_time|
|authorization_code|
|azp|
|azpacr|
|c_hash|
|ca_enf|
|Kopie|
|cert_token_use|
|client_id|
|cloud_graph_host_name|
|cloud_instance_name|
|možností cnf|
|Kód|
|ovládací prvky|
|credential_keys|
|oddělení služeb zákazníkům|
|csr_type|
|ID zařízení|
|dns_names|
|domain_dns_name|
|domain_netbios_name|
|e_exp|
|E-mailu|
|koncový bod|
|enfpolids|
|Exp|
|expires_on|
|grant_type|
|Graf|
|group_sids|
|skupiny|
|hasgroups|
|hash_alg|
|home_oid|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/AuthenticationInstant|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/AuthenticationMethod|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/Expiration|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/Expired|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/EmailAddress|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/Name|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/NameIdentifier|
|IAT|
|identityprovider|
|deklarací identity|
|in_corp|
|Instance|
|IPADDR|
|isbrowserhostedapp|
|iss|
|jwk|
|key_id|
|key_type –|
|mam_compliance_url|
|mam_enrollment_url|
|mam_terms_of_use_url|
|mdm_compliance_url|
|mdm_enrollment_url|
|mdm_terms_of_use_url|
|nameid|
|NBF|
|netbios_name|
|hodnotu Nonce|
|OID|
|on_prem_id|
|onprem_sam_account_name|
|onprem_sid|
|openid2_id|
|heslo|
|platf|
|polids|
|pop_jwk|
|preferred_username|
|previous_refresh_token|
|primary_sid|
|identifikátor PUID|
|pwd_exp|
|pwd_url|
|redirect_uri|
|refresh_token|
|refreshtoken|
|request_nonce|
|Prostředek|
|Role|
|role|
|Obor|
|spojovací bod služby|
|identifikátor SID|
|Podpis|
|signin_state|
|src1|
|src2|
|Sub –|
|tbid|
|tenant_display_name|
|tenant_region_scope|
|thumbnail_photo|
|TID|
|tokenAutologonEnabled|
|trustedfordelegation|
|unique_name|
|hlavní název uživatele|
|user_setting_sync_url|
|uživatelské jméno|
|uti|
|ver|
|verified_primary_email|
|verified_secondary_email|
|wids|
|win_ver|

#### <a name="table-2-security-assertion-markup-language-saml-restricted-claim-set"></a>Tabulka 2: Security (Assertion Markup Language SAML) omezené sady deklarací.
|Typ deklarace identity (URI)|
| ----- |
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/Expiration|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/Expired|
|http://schemas.microsoft.com/identity/Claims/accesstoken|
|http://schemas.microsoft.com/identity/Claims/openid2_id|
|http://schemas.microsoft.com/identity/Claims/identityprovider|
|http://schemas.microsoft.com/identity/Claims/objectidentifier|
|http://schemas.microsoft.com/identity/Claims/PUID|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/NameIdentifier [MR1] |
|http://schemas.microsoft.com/identity/Claims/tenantid|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/AuthenticationInstant|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/AuthenticationMethod|
|http://schemas.microsoft.com/accesscontrolservice/2010/07/Claims/identityprovider|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/groups|
|http://schemas.microsoft.com/Claims/groups.Link|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/role|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/wids|
|http://schemas.microsoft.com/2014/09/devicecontext/Claims/iscompliant|
|http://schemas.microsoft.com/2014/02/devicecontext/Claims/isknown|
|http://schemas.microsoft.com/2012/01/devicecontext/Claims/ismanaged|
|http://schemas.microsoft.com/2014/03/psso|
|http://schemas.microsoft.com/Claims/authnmethodsreferences|
|http://schemas.xmlsoap.org/ws/2009/09/identity/Claims/actor|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/samlissuername|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/confirmationkey|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsaccountname|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/primarygroupsid|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/primarysid|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/authorizationdecision|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/Authentication|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/SID|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/denyonlyprimarygroupsid|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/denyonlyprimarysid|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/denyonlysid|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/denyonlywindowsdevicegroup|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsdeviceclaim|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsdevicegroup|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsfqbnversion|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowssubauthority|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsuserclaim|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/x500distinguishedname|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/UPN|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/GroupSID|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/SPN|
|http://schemas.microsoft.com/ws/2008/06/identity/Claims/ispersistent|
|http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/privatepersonalidentifier|
|http://schemas.microsoft.com/identity/Claims/scope|

## <a name="claims-mapping-policy-properties"></a>Deklarace identity mapování vlastnosti zásad
Pomocí vlastností hello mapování zásad toocontrol deklarace, které jsou vygenerované a kde je hello data získaná z deklarací. Pokud je nastavené žádné zásady, hello systému vydává tokeny obsahující sadu deklarací hello jádra, hello základní sady deklarací a volitelné deklarace identity, které aplikace hello vybrána tooreceive.

### <a name="include-basic-claim-set"></a>Patří nastavení základní deklarace identity

**Řetězec:** IncludeBasicClaimSet

**Datový typ:** logická hodnota (True nebo False)

**Souhrn:** tato vlastnost určuje, zda sada hello základní deklarací identity je součástí tokeny vliv na tuto zásadu. 

- Pokud sada tooTrue, všechny deklarace identity v sadě hello základní deklarace identity jsou vygenerované v tokeny vliv hello zásad. 
- Pokud sada tooFalse, deklarace identity v sadě deklarací základní hello nejsou ve tokeny hello, pokud se jednotlivě nepřidají ve vlastnosti schématu deklarace identity hello hello stejné zásady.

>[!NOTE] 
>Deklarace identity v základní hello deklarace identity, jsou k dispozici v každé tokenu, bez ohledu na to, co je tato vlastnost nastavená na sadu. 

### <a name="claims-schema"></a>Schéma deklarace identity

**Řetězec:** ClaimsSchema

**Datový typ:** JSON blob pomocí jedné nebo víc položek schématu deklarace identity

**Souhrn:** definuje tato vlastnost deklarace, které se nacházejí v tokenech hello vliv hello zásad, kromě toohello základní sady deklarací a hello základní sady deklarací.
U jednotlivých deklarací identity položek schéma definované v této vlastnosti určité informace se vyžaduje. Je nutné zadat, kde hello data pocházejí (**hodnotu** nebo **ID zdrojového nebo pár**), a který hello data deklarace identity jsou vydávány jako (**deklarace identity typu**).

### <a name="claim-schema-entry-elements"></a>Prvky schématu vstupní deklarace identity

**Hodnota:** hello hodnota elementu definuje statickou hodnotu jako toobe data hello vygenerované v hello deklarace identity.

**ID zdrojového nebo pár:** hello zdroje a elementy ID definovat, kde jsou data hello v hello deklarace identity pochází z. 

Hello Source element musí být nastavena tooone hello následující: 


- "user": hello data v hello deklarací identity je vlastnost v objektu User hello. 
- "aplikace": hello data v hello deklarací identity je vlastnost v objektu služby hello aplikace (klient). 
- "prostředek": hello data v hello deklarací identity je vlastnost na hello prostředků instanční objekt.
- "cílová skupina": hello data v deklaraci hello je vlastnost na hello instanční objekt, který je cílová skupina hello hello tokenu (buď hello klienta nebo prostředek instanční objekt).
- "společnost": hello data v hello deklarací identity je vlastnost v objektu společnosti hello prostředků klienta.
- "transformace": hello data v hello deklarací identity je z transformace deklarace identity (viz část hello "transformace deklarace identity" později v tomto článku). 

Pokud je zdroj hello transformace, hello **TransformationID** element musí být součástí této deklarace identity definici.

ID elementu Hello identifikuje, kterého je vlastnost na zdroji hello poskytuje hello hodnoty pro deklarace identity hello. Hello následující tabulka uvádí hello hodnoty ID, které jsou platné pro každou hodnotu zdroje.

#### <a name="table-3-valid-id-values-per-source"></a>Tabulka 3: Hodnoty platné ID na zdroj
|Zdroj|ID|Popis|
|-----|-----|-----|
|Uživatel|Příjmení|Příjmení|
|Uživatel|givenName|Křestní jméno|
|Uživatel|DisplayName|Zobrazovaný název|
|Uživatel|objectid|ObjectID|
|Uživatel|E-mailu|E-mailová adresa|
|Uživatel|userPrincipalName|Hlavní název uživatele|
|Uživatel|Oddělení|Oddělení|
|Uživatel|onpremisessamaccountname|Na místní název účtu Sam|
|Uživatel|název pro rozhraní NetBIOS|Název pro rozhraní NetBios|
|Uživatel|název_domény_DNS|Název domény DNS|
|Uživatel|onpremisesecurityidentifier|Identifikátor zabezpečení na místě|
|Uživatel|NázevSpolečnosti|Název organizace|
|Uživatel|streetAddress|Ulice|
|Uživatel|PSČ|PSČ|
|Uživatel|preferredlanguange|Upřednostňovaný jazyk|
|Uživatel|onpremisesuserprincipalname|místní hlavní název uživatele|
|Uživatel|mailnickname|Přezdívka e-mailu|
|Uživatel|extensionattribute1|Atribut rozšíření 1|
|Uživatel|extensionattribute2|Atribut rozšíření 2|
|Uživatel|extensionattribute3|Rozšíření atribut 3|
|Uživatel|extensionattribute4|Atribut rozšíření 4|
|Uživatel|extensionattribute5|Rozšíření atribut 5|
|Uživatel|extensionattribute6|Atribut rozšíření 6|
|Uživatel|extensionattribute7|Atribut rozšíření 7|
|Uživatel|extensionattribute8|Atribut rozšíření 8|
|Uživatel|extensionattribute9|Rozšíření atribut 9|
|Uživatel|extensionattribute10|Atribut rozšíření 10|
|Uživatel|extensionattribute11|Atribut rozšíření 11|
|Uživatel|extensionattribute12|Atribut rozšíření 12|
|Uživatel|extensionattribute13|Atribut rozšíření 13|
|Uživatel|extensionattribute14|Atribut rozšíření 14|
|Uživatel|extensionattribute15|Atribut rozšíření 15|
|Uživatel|othermail|Další e-mailu|
|Uživatel|Země|Země|
|Uživatel|city|Město|
|Uživatel|state|Stav|
|Uživatel|pracovní funkce|Funkce|
|Uživatel|Číslo zaměstnance|ID zaměstnance|
|Uživatel|facsimiletelephonenumber|Faxem telefonní číslo|
|aplikace, prostředků, cílová skupina|DisplayName|Zobrazovaný název|
|aplikace, prostředků, cílová skupina|proti|ObjectID|
|aplikace, prostředků, cílová skupina|tags|Značka instančního objektu|
|Společnost|tenantcountry|Země klienta|

**TransformationID:** hello TransformationID element musí být zadané jenom v případě hello Source element nastavena příliš "transformace".

- Tento element musí odpovídat ID elementu hello hello transformace položky v hello **ClaimsTransformation** vlastnost, která definuje, jak se vygeneruje hello data pro tuto deklaraci.

**Typ deklarace identity:** hello **JwtClaimType** a **SamlClaimType** elementy definovat, které deklarace identity odkazuje tato položka schématu deklarace identity.

- Hello JwtClaimType musí obsahovat název hello toobe deklarace identity hello vygenerované v Jwt.
- Hello SamlClaimType musí obsahovat hello URI hello deklarace toobe vygenerované v tokeny SAML.

>[!NOTE]
>Názvy a identifikátory URI deklarací identity v hello omezený deklarací identity, sadu nelze použít pro elementy typu deklarace identity hello. Další informace najdete v tématu část "Výjimky a omezení" hello později v tomto článku.

### <a name="claims-transformation"></a>Transformace deklarace identity

**Řetězec:** ClaimsTransformation

**Datový typ:** objekt blob JSON, s jedné nebo víc položek transformace 

**Souhrn:** používat tato vlastnost tooapply běžné transformace toosource data, toogenerate hello výstupní data pro deklarace identity zadané v hello schématu deklarací identity.

**ID:** použití hello ID elementu tooreference tato položka transformaci v hello položka schématu TransformationID deklarací identity. Tato hodnota musí být jedinečný pro každý záznam transformaci v rámci této zásady.

**TransformationMethod:** hello TransformationMethod element identifikuje, kterou operaci se provádí toogenerate hello data hello deklarace identity.

Založené na metodě hello vybrané, se očekává sadu vstupy a výstupy. Tyto jsou definované za použití hello **InputClaims**, **vstupní parametry** a **OutputClaims** elementy.

#### <a name="table-4-transformation-methods-and-expected-inputs-and-outputs"></a>Tabulka 4: Metody transformaci a očekávané vstupy a výstupy
|TransformationMethod|Očekávaný vstup|Očekávaný výstup|Popis|
|-----|-----|-----|-----|
|Spojit|řetězec1, řetězec2, oddělovače|outputClaim|Spojení vstup řetězce s použitím oddělovače v rozmezí. Příklad: řetězec1: "foo@bar.com", řetězec2: "izolovaného prostoru", oddělovač: "." výsledkem outputClaim: "foo@bar.com.sandbox"|
|ExtractMailPrefix|E-mailu|outputClaim|Extrahuje hello místní část e-mailovou adresu. Příklad: e-mailu: "foo@bar.com" výsledkem outputClaim: "foo". Když není @ přihlašovací nachází, hello orignal vstupní řetězec je vrácena jako je.|

**InputClaims:** InputClaims element toopass hello data z tooa transformace deklarace identity schématu položky použít. Má dva atributy: **ClaimTypeReferenceId** a **TransformationClaimType**.

- **ClaimTypeReferenceId** je spojen s ID elementu hello deklarace identity schématu položka toofind hello odpovídající vstupní deklaraci identity. 
- **TransformationClaimType** je použité toogive vstup toothis jedinečný název. Tento název se musí shodovat s jedním hello očekává vstupy pro metodu transformace hello.

**Vstupní parametry:** použít vstupní parametry element toopass transformace tooa konstantní hodnotu. Má dva atributy: **hodnotu** a **ID**.

- **Hodnota** je předán toobe skutečná konstantní hodnota hello.
- **ID** je použité toogive vstup toothis jedinečný název. Tento název se musí shodovat s jedním hello očekává vstupy pro metodu transformace hello.

**OutputClaims:** použijte OutputClaims element toohold hello data generována transformaci a svázání se položka schématu tooa deklarace identity. Má dva atributy: **ClaimTypeReferenceId** a **TransformationClaimType**.

- **ClaimTypeReferenceId** je spojen s ID hello hello deklarace identity schématu položka toofind hello deklarace identity odpovídající výstup.
- **TransformationClaimType** je použité toogive výstup toothis jedinečný název. Tento název se musí shodovat s jedním výstupy hello očekávání pro metodu transformace hello.

### <a name="exceptions-and-restrictions"></a>Výjimky a omezení

**SAML NameID a UPN:** hello atributů, ze kterých zdroje hello NameID a UPN hodnoty a hello deklarací transformace, které jsou povoleny, jsou omezené.

#### <a name="table-5-attributes-allowed-as-a-data-source-for-saml-nameid"></a>Tabulka 5: Atributy povolena jako zdroj dat pro SAML NameID
|Zdroj|ID|Popis|
|-----|-----|-----|
|Uživatel|E-mailu|E-mailová adresa|
|Uživatel|userPrincipalName|Hlavní název uživatele|
|Uživatel|onpremisessamaccountname|Na místní název účtu Sam|
|Uživatel|Číslo zaměstnance|ID zaměstnance|
|Uživatel|extensionattribute1|Atribut rozšíření 1|
|Uživatel|extensionattribute2|Atribut rozšíření 2|
|Uživatel|extensionattribute3|Rozšíření atribut 3|
|Uživatel|extensionattribute4|Atribut rozšíření 4|
|Uživatel|extensionattribute5|Rozšíření atribut 5|
|Uživatel|extensionattribute6|Atribut rozšíření 6|
|Uživatel|extensionattribute7|Atribut rozšíření 7|
|Uživatel|extensionattribute8|Atribut rozšíření 8|
|Uživatel|extensionattribute9|Rozšíření atribut 9|
|Uživatel|extensionattribute10|Atribut rozšíření 10|
|Uživatel|extensionattribute11|Atribut rozšíření 11|
|Uživatel|extensionattribute12|Atribut rozšíření 12|
|Uživatel|extensionattribute13|Atribut rozšíření 13|
|Uživatel|extensionattribute14|Atribut rozšíření 14|
|Uživatel|extensionattribute15|Atribut rozšíření 15|

#### <a name="table-6-transformation-methods-allowed-for-saml-nameid"></a>Tabulka 6: Transformace metody povolené pro SAML NameID
|TransformationMethod|Omezení|
| ----- | ----- |
|ExtractMailPrefix|Žádný|
|Spojit|přípona Hello spojených musí být ověřené domény klienta prostředků hello.|

### <a name="custom-signing-key"></a>Vlastní podpisového klíče
Vlastní podpisový klíč musí být přiřazen toohello objekt zabezpečení služby deklarací identity mapování zásad tootake vliv. Tento klíč jsou podepsané všechny tokeny vydané, které má vliv hello zásad. Aplikace musí být nakonfigurované tooaccept tokeny podepsaný pomocí tohoto klíče. Tím se zajistí, že potvrzení, že byl změněn tokeny pomocí Tvůrce hello hello deklarací mapování zásad. To chrání aplikace z deklarací identity mapování zásady vytvořené pomocí nebezpečného actors.

### <a name="cross-tenant-scenarios"></a>Scénáře napříč klienta
Mapování zásad deklarace identity se nevztahují tooguest uživatele. Pokud uživatel guest pokusů o tooaccess aplikace s deklaracemi identity mapování zásad přiřazené tooits služby objekt zabezpečení, se objeví hello výchozí token (hello zásada nemá žádný vliv).

## <a name="claims-mapping-policy-assignment"></a>Mapování přiřazení zásady deklarace identity
Deklarace identity, že zásady mapování lze přiřadit pouze hlavní objekty tooservice.

### <a name="example-claims-mapping-policies"></a>Příklad deklarací mapování zásad

Ve službě Azure AD je možné mnoho scénářů, pokud můžete přizpůsobit pro konkrétní službu objektů vygenerované v tokeny deklarací identity. V této části jsme provede několik běžných scénářů, které vám může pomoct pochopit, jak toouse hello deklarací typ mapování zásad.

#### <a name="prerequisites"></a>Požadavky
V následující příklady hello vytvářet, aktualizovat, propojení a odstranit zásady pro objekty služby. Pokud jste nový tooAzure AD, doporučujeme vám, další informace o tom, jak tooget na Azure AD klientů předtím, než budete pokračovat v těchto příkladech. 

tooget spuštění hello následující kroky:


1. Stáhněte si nejnovější hello [verze public preview modul Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.127).
2.  Spusťte hello Connect příkaz toosign v tooyour účet správce Azure AD. Spusťte tento příkaz pokaždé, když zahájit novou relaci.
    
     ``` powershell
    Connect-AzureAD -Confirm
    
    ```
3.  toosee všechny zásady, které byly vytvořeny ve vaší organizaci, spusťte hello následující příkaz. Doporučujeme vám spustit tento příkaz po většinu operací v hello následující scénáře, očekává toocheck, který jako během vytváření zásad.
   
    ``` powershell
        Get-AzureADPolicy
    
    ```
#### <a name="example-create-and-assign-a-policy-tooomit-hello-basic-claims-from-tokens-issued-tooa-service-principal"></a>Příklad: Vytvořte a přiřaďte zásad tooomit hello základní deklarace identity z tokeny vydané tooa instanční objekt.
V tomto příkladu vytvoříte zásadu, která odebere sady základní deklarací hello tokeny vydané toolinked objekty služby.


1. Vytvořte deklarace identity mapování zásad. Tato zásada, propojené toospecific objekty služby, odebere sady hello základní deklarací z tokenů.
    1. toocreate hello zásady, spusťte tento příkaz: 
    
     ``` powershell
    New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"false"}}') -DisplayName "OmitBasicClaims” -Type "ClaimsMappingPolicy"
    ```
    2. toosee, které vaše nové zásady a zásady hello tooget ObjectId, spusťte hello následující příkaz:
    
     ``` powershell
    Get-AzureADPolicy
    ```
2.  Přiřaďte hello zásad tooyour instanční objekt. Budete také potřebovat tooget hello ObjectId instanční objekt. 
    1.  toosee objekty všechna firemní služby, se můžete dotazovat Microsoft Graph. Nebo v Azure AD Graph Explorer přihlaste tooyour účet Azure AD.
    2.  Pokud máte hello ObjectId vaší služby hlavní, spusťte hello následující příkaz:  
     
     ``` powershell
    Add-AzureADServicePrincipalPolicy -Id <ObjectId of hello ServicePrincipal> -RefObjectId <ObjectId of hello Policy>
    ```
#### <a name="example-create-and-assign-a-policy-tooinclude-hello-employeeid-and-tenantcountry-as-claims-in-tokens-issued-tooa-service-principal"></a>Příklad: Vytvoření a přiřazení zásad tooinclude hello EmployeeID a TenantCountry jako deklarace identity v tokenech vystavený tooa instanční objekt.
V tomto příkladu vytvoříte zásadu, která přidá hello EmployeeID a TenantCountry tootokens vydané toolinked objekty služby. Hello EmployeeID jsou vydávány jako typ deklarace identity názvu hello tokeny SAML a tokeny Jwt. Hello TenantCountry jsou vydávány jako typ tokeny SAML a tokeny Jwt deklarace země hello. V tomto příkladu abychom mohli pokračovat tooinclude hello základní deklarací nastavit hello tokenů.

1. Vytvořte deklarace identity mapování zásad. Tato zásada, propojené toospecific objekty služby, přidá tootokens hello EmployeeID a TenantCountry deklarací identity.
    1. toocreate hello zásady, spusťte tento příkaz:  
     
     ``` powershell
    New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema": [{"Source":"user","ID":"employeeid","SamlClaimType":"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name","JwtClaimType":"name"},{"Source":"company","ID":" tenantcountry ","SamlClaimType":" http://schemas.xmlsoap.org/ws/2005/05/identity/claims/country ","JwtClaimType":"country"}]}}') -DisplayName "ExtraClaimsExample” -Type "ClaimsMappingPolicy"
    ```
    
    2. toosee, které vaše nové zásady a zásady hello tooget ObjectId, spusťte hello následující příkaz:
     
     ``` powershell  
    Get-AzureADPolicy
    ```
2.  Přiřaďte hello zásad tooyour instanční objekt. Budete také potřebovat tooget hello ObjectId instanční objekt. 
    1.  toosee objekty všechna firemní služby, se můžete dotazovat Microsoft Graph. Nebo v Azure AD Graph Explorer přihlaste tooyour účet Azure AD.
    2.  Pokud máte hello ObjectId vaší služby hlavní, spusťte hello následující příkaz:  
     
     ``` powershell
    Add-AzureADServicePrincipalPolicy -Id <ObjectId of hello ServicePrincipal> -RefObjectId <ObjectId of hello Policy>
    ```
#### <a name="example-create-and-assign-a-policy-that-uses-a-claims-transformation-in-tokens-issued-tooa-service-principal"></a>Příklad: Vytvořte a přiřaďte zásadu, která používá transformaci deklarací identity v tokeny vydané tooa instanční objekt.
V tomto příkladu vytvoříte zásadu, která vydá vlastní deklarace "JoinedData", že tooJWTs vydané toolinked objekty služby. Toto tvrzení obsahuje hodnotu vytvořený sloučením hello data uložená v atributu extensionattribute1 hello u objektu uživatele hello s ".sandbox". V tomto příkladu jsme vyloučit hello základní deklarace identity, nastavte v tokenech hello.


1. Vytvořte deklarace identity mapování zásad. Tato zásada, propojené toospecific objekty služby, přidá tootokens hello EmployeeID a TenantCountry deklarací identity.
    1. toocreate hello zásady, spusťte tento příkaz: 
     
     ``` powershell
    New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema":[{"Source":"user","ID":"extensionattribute1"},{"Source":"transformation","ID":"DataJoin","TransformationId":"JoinTheData","JwtClaimType":"JoinedData"}],"ClaimsTransformation":[{"ID":"JoinTheData","TransformationMethod":"Join","InputClaims":[{"ClaimTypeReferenceId":"extensionattribute1","TransformationClaimType":"string1"}], "InputParameters": [{"Id":"string2","Value":"sandbox"},{"Id":"separator","Value":"."}],"OutputClaims":[{"ClaimTypeReferenceId":"DataJoin","TransformationClaimType":"outputClaim"}]}]}}') -DisplayName "TransformClaimsExample” -Type "ClaimsMappingPolicy"
    ```
    
    2. toosee, které vaše nové zásady a zásady hello tooget ObjectId, spusťte hello následující příkaz: 
     
     ``` powershell
    Get-AzureADPolicy
    ```
2.  Přiřaďte hello zásad tooyour instanční objekt. Budete také potřebovat tooget hello ObjectId instanční objekt. 
    1.  toosee objekty všechna firemní služby, se můžete dotazovat Microsoft Graph. Nebo v Azure AD Graph Explorer přihlaste tooyour účet Azure AD.
    2.  Pokud máte hello ObjectId vaší služby hlavní, spusťte hello následující příkaz: 
     
     ``` powershell
    Add-AzureADServicePrincipalPolicy -Id <ObjectId of hello ServicePrincipal> -RefObjectId <ObjectId of hello Policy>
    ```
