---
title: "Azure Active Directory B2C: Přidat vlastní atributy toocustom zásady a použít v profilu upravit | Microsoft Docs"
description: "Návod na pomocí vlastnosti rozšíření, vlastní atributy a jejich včetně hello uživatelské rozhraní"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: joroja
ms.openlocfilehash: 8cc9c6a38d7652797ba54a3e02078ac2bf4a693b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-creating-and-using-custom-attributes-in-a-custom-profile-edit-policy"></a>Azure Active Directory B2C: Vytváření a používání vlastních atributů v vlastního profilu upravit zásady

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

V tomto článku můžete vytvořit vlastní atribut v adresáři Azure AD B2C a používat tento nový atribut jako vlastních deklarací identity v hello profil upravit uživatele cesty.

## <a name="prerequisites"></a>Požadavky

Dokončení hello kroků v článku hello [Začínáme se zásadami vlastní](active-directory-b2c-get-started-custom.md).

## <a name="use-custom-attributes-toocollect-information-about-your-customers-in-azure-active-directory-b2c-using-custom-policies"></a>Použít vlastní atributy toocollect informace o zákazníky v Azure Active Directory B2C pomocí vlastních zásad
Adresáře Azure Active Directory (Azure AD) B2C se dodává s integrovanou sadu atributů: zadané jméno, příjmení, Město, PSČ, userPrincipalName, atd.  Často potřebujete toocreate vlastní atributy.  Například:
* Zákazník směřujících aplikací musí toopersist atribut jako je například "LoyaltyNumber."
* Zprostředkovatele identity má jedinečný identifikátor uživatele, musí být uložena jako je například "uniqueUserGUID"."
* Vlastní uživatelské cesty musí toopersist hello stavu uživatele, jako je například "migrationStatus."

S Azure AD B2C můžete rozšířit hello sadu atributů uložené u každého uživatelského účtu. Můžete také číst a zapsat tyto atributy pomocí hello [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

Vlastnosti rozšíření rozšířit schéma hello hello uživatelských objektů v adresáři hello.  Vlastnost rozšíření Hello podmínky, vlastních atributů a vlastních deklarací identity najdete v toohello, které samé v kontextu hello tento název článku a hello se liší v závislosti na kontextu hello (aplikace, objekt, zásady).

Vlastnosti rozšíření se dají registrovat jenom na objekt aplikace i v případě, že mohou obsahovat data pro uživatele. Vlastnost Hello je připojený toohello aplikace. objekt aplikace Hello musí být tooregister udělí oprávnění k zápisu ve vlastnosti rozšíření. 100 – vlastnosti rozšíření (v rámci všech typů a všechny aplikace), může být napsán tooany jednoho objektu. Vlastnosti rozšíření přidají toohello cílový adresář typ a že bude okamžitě dostupný v klientovi directory hello Azure AD B2C.
Pokud aplikace hello je odstraněna, budou odebrány také tyto vlastnosti rozšíření spolu s daty v nich obsažené pro všechny uživatele. Pokud ve vlastnosti rozšíření je odstraní hello aplikace, bude odebrán na hello cílových objektů adresáře a hello hodnoty odstranit.

Vlastnosti rozšíření existují pouze v kontextu hello zaregistrovanou aplikaci v klientovi hello. id objektu Hello této aplikace musí být součástí hello TechnicalProfile, který ho použít.

>[!NOTE]
>Hello Azure AD B2C directory obvykle zahrnuje webovou aplikaci s názvem `b2c-extensions-app`.  Tato aplikace je používána především hello b2c předdefinované zásady pro vlastní deklarace hello vytvořené prostřednictvím hello portálu Azure.  Pomocí této aplikace tooregister rozšíření pro vlastní zásady b2c se doporučuje jenom pro zkušené uživatele.  Pokyny k tomuto jsou součástí hello části Další kroky v tomto článku.


## <a name="creating-a-new-application-toostore-hello-extension-properties"></a>Vytvoření nové aplikace toostore hello – vlastnosti rozšíření

1. Spustí relaci prohlížeče a přejděte toohello [Azure Portal](https://portal.azure.com) a přihlaste se přihlašovacími údaji správce hello chcete tooconfigure adresáři B2C.
1. Klikněte na tlačítko **Azure Active Directory** v levém navigačním nabídce hello. Může být nutné ji tak, že vyberete více služeb toofind >.
1. Vyberte **registrace aplikace** a klikněte na tlačítko **novou registraci aplikace**
1. Poskytnout doporučené hello následující položky:
  * Zadejte název webové aplikace hello: **WebApp. GraphAPI DirectoryExtensions**
  * Typ aplikace: webové aplikace nebo rozhraní API
  * URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions přihlašování
1. Vyberte ** vytvořit. Úspěšné dokončení se zobrazí v hello **oznámení**
1. Vyberte nově vytvořený hello webové aplikace: **WebApp. GraphAPI DirectoryExtensions**
1. Vyberte nastavení: **požadovaná oprávnění**
1. Vybrat rozhraní API **služby Windows Active Directory**
1. Oprávnění aplikací, zaškrtněte: **pro čtení a zápis dat adresáře**, a **uložit**
1. Zvolte **udělit oprávnění** a potvrďte **Ano**.
1. Tooyour schránky zkopírujte a uložte hello následujících identifikátory z webové aplikace. GraphAPI DirectoryExtensions > Nastavení > Vlastnosti >
*  **ID aplikace** . Příklad:`103ee0e6-f92d-4183-b576-8c3739027780`
* **ID objektu**. Příklad:`80d8296a-da0a-49ee-b6ab-fd232aa45201`



## <a name="modifying-your-custom-policy-tooadd-hello-applicationobjectid"></a>Úprava vaše vlastní zásady tooadd hello ApplicationObjectId

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item>
                <Item Key="ClientId">insert appId here</Item>
              </Metadata>
            <!-- End of changes -->
              <CryptographicKeys>
                <Key Id="issuer_secret" StorageReferenceId="TokenSigningKeyContainer" />
              </CryptographicKeys>
              <IncludeInSso>false</IncludeInSso>
              <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
            </TechnicalProfile>
        </ClaimsProvider>
    </ClaimsProviders>
```

>[!NOTE]
>Hello <TechnicalProfile Id="AAD-Common"> je odkazované tooas "běžné", protože jeho prvky jsou součástí a opakovaně používat ve všech hello Azure Active Directory TechnicalProfiles pomocí elementu hello:`<IncludeTechnicalProfile ReferenceId="AAD-Common" />`

>[!NOTE]
>Když hello TechnicalProfile zapíše pro vlastnost rozšíření toohello nově vytvořený první čas hello, může docházet k chybě jednorázové.  Vlastnost rozšíření Hello se vytvoří hello poprvé, co se používá.  

## <a name="using-hello-new-extension-property--custom-attribute-in-a-user-journey"></a>Pomocí nové vlastnosti rozšíření hello / vlastní atribut cesty uživatele


1. Otevřete hello předávající Party(RP) soubor, který popisuje vaše zásady upravit uživatele cesty.  Pokud jsou začínáte, může být vhodné toodownload vaší už nakonfigurované verzi hello RP PolicyEdit souboru přímo z hello části vlastní zásady pro Azure B2C v hello portálu Azure.  Alternativně můžete otevřete soubor XML ze složky úložiště.
2. Přidat vlastní deklarace identity `loyaltyId`.  Zahrnutím hello vlastní deklarace identity v hello `<RelyingParty>` elementu, je předaná jako parametr toohello UserJourney TechnicalProfiles a součástí hello token pro aplikace hello.
```xml
<RelyingParty>
   <DefaultUserJourney ReferenceId="ProfileEdit" />
   <TechnicalProfile Id="PolicyProfile">
     <DisplayName>PolicyProfile</DisplayName>
     <Protocol Name="OpenIdConnect" />
     <OutputClaims>
       <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
       <OutputClaim ClaimTypeReferenceId="city" />

       <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />

     </OutputClaims>
     <SubjectNamingInfo ClaimType="sub" />
   </TechnicalProfile>
 </RelyingParty>
 ```
3. Přidání souboru deklarace identity definice toohello rozšíření zásad `TrustFrameworkExtensions.xml` uvnitř hello `<ClaimsSchema>` element, jak je vidět.
```xml
<ClaimsSchema>
        <ClaimType Id="extension_loyaltyId">
            <DisplayName>Loyalty Identification Tag</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Your loyalty number from your membership card</UserHelpText>
            <UserInputType>TextBox</UserInputType>
        </ClaimType>
</ClaimsSchema>
```
4. Přidat hello stejný soubor základní zásad toohello definice deklarace `TrustFrameworkBase.xml`.  
>Přidání `ClaimType` definice v základní hello i hello rozšíření soubor není obvykle nutné, ale vzhledem k tomu, že další kroky hello přidá hello extension_loyaltyId tooTechnicalProfiles hello základního souboru, program pro ověření zásad hello odmítnou nahrávání hello hello základního souboru bez ní.
>To může být užitečné tootrace hello provádění hello cesty uživatele s názvem "ProfileEdit" v souboru TrustFrameworkBase.xml hello.  Vyhledejte cestu uživatele hello hello stejný název ve svém editoru a pozorovat, že krok 5 Orchestration vyvolá hello TechnicalProfileReferenceID = "SelfAsserted-ProfileUpdate".  Hledání a zkontrolovat tento TechnicalProfile toofamiliarize s tokem hello.
5. Přidat loyaltyId jako vstupní a výstupní deklarací identity v hello TechnicalProfile "SelfAsserted-ProfileUpdate"
```xml
<TechnicalProfile Id="SelfAsserted-ProfileUpdate">
          <DisplayName>User ID signup</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ContentDefinitionReferenceId">api.selfasserted.profileupdate</Item>
          </Metadata>
          <IncludeInSso>false</IncludeInSso>
          <InputClaims>

            <InputClaim ClaimTypeReferenceId="alternativeSecurityId" />
            <InputClaim ClaimTypeReferenceId="userPrincipalName" />

            <!-- Optional claims. These claims are collected from hello user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written toodirectory after being updateed by hello user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <InputClaim ClaimTypeReferenceId="givenName" />
            <InputClaim ClaimTypeReferenceId="surname" />
            <InputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </InputClaims>
          <OutputClaims>
            <!-- Required claims -->
            <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />

            <!-- Optional claims. These claims are collected from hello user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written toodirectory after being updateed by hello user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <OutputClaim ClaimTypeReferenceId="givenName" />
            <OutputClaim ClaimTypeReferenceId="surname" />
            <OutputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="AAD-UserWriteProfileUsingObjectId" />
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
```
6. Přidání deklarace identity v TechnicalProfile "AAD-UserWriteProfileUsingObjectId" toopersist hello hodnoty deklarace identity hello ve vlastnosti rozšíření hello, pro hello aktuálního uživatele v adresáři hello.
```xml
<TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
          <Metadata>
            <Item Key="Operation">Write</Item>
            <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">false</Item>
            <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
          </Metadata>
          <IncludeInSso>false</IncludeInSso>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
          </InputClaims>
          <PersistedClaims>
            <!-- Required claims -->
            <PersistedClaim ClaimTypeReferenceId="objectId" />

            <!-- Optional claims -->
            <PersistedClaim ClaimTypeReferenceId="givenName" />
            <PersistedClaim ClaimTypeReferenceId="surname" />
            <PersistedClaim ClaimTypeReferenceId="extension_loyaltyId" />

          </PersistedClaims>
          <IncludeTechnicalProfile ReferenceId="AAD-Common" />
        </TechnicalProfile>
```
7. Přidání deklarace identity v TechnicalProfile "AAD-UserReadUsingObjectId" tooread hello hodnotu atributu rozšíření hello pokaždé, když se uživatel přihlásí. Doposud hello TechnicalProfiles byly změněny v toku hello pouze místní účty.  Pokud nový atribut hello se požaduje v toku hello sociálního nebo federované účtu, musí jinou sadu TechnicalProfiles toobe změnit. Najdete v části Další kroky.

```xml
<!-- hello following technical profile is used tooread data after user authenticates. -->
     <TechnicalProfile Id="AAD-UserReadUsingObjectId">
       <Metadata>
         <Item Key="Operation">Read</Item>
         <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
       </Metadata>
       <IncludeInSso>false</IncludeInSso>
       <InputClaims>
         <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
       </InputClaims>
       <OutputClaims>
         <!-- Optional claims -->
         <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
         <OutputClaim ClaimTypeReferenceId="displayName" />
         <OutputClaim ClaimTypeReferenceId="otherMails" />
         <OutputClaim ClaimTypeReferenceId="givenName" />
         <OutputClaim ClaimTypeReferenceId="surname" />
         <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
       </OutputClaims>
       <IncludeTechnicalProfile ReferenceId="AAD-Common" />
     </TechnicalProfile>
```


>[!IMPORTANT]
>Hello IncludeTechnicalProfile element přidá všechny elementy hello AAD běžné toothis TechnicalProfile.

## <a name="test-hello-custom-policy-using-run-now"></a>Testování hello "Spustit nyní" pomocí vlastních zásad
1. Otevřete hello **okno Azure AD B2C** a přejděte příliš**Identity rozhraní Framework > vlastní zásady**.
1. Vyberte hello vlastní zásadu, kterou jste nahráli a klikněte na tlačítko hello **spustit nyní** tlačítko.
1. Musí být schopný toosign díky e-mailovou adresu.

Hello id token odeslána zpět tooyour aplikace obsahuje novou vlastnost rozšíření hello jako vlastní deklaraci extension_loyaltyId před sebou. Podívejte se na příklad.

```
{
  "exp": 1493585187,
  "nbf": 1493581587,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493581587,
  "auth_time": 1493581587,
  "extension_loyaltyId": "abc",
  "city": "Redmond"
}
```

## <a name="next-steps"></a>Další kroky

Přidejte hello novou deklaraci identity toohello toky pro sociálních účet přihlášení změnou hello TechnicalProfiles nezobrazí. Tyto dvě TechnicalProfiles jsou používané toowrite sociálního/federovaný účet přihlášení a čtení dat uživatele hello pomocí hello alternativeSecurityId jako hello Lokátor hello objekt uživatele.
```
  <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

  <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```

Pomocí hello stejné atributy rozšíření mezi předdefinované a vlastní zásady.
Když přidáte rozšíření atributy (neboli vlastní atributy) prostřednictvím portálu prostředí hello, jsou tyto atributy registrovat pomocí hello ** b2c-rozšíření aplikaci, která existuje v každé klienta b2c.  toouse tyto atributy rozšíření ve vlastních zásadách:
1. V rámci svého klienta b2c na stránce portal.azure.com přejděte příliš**Azure Active Directory** a vyberte **registrace aplikace**
2. Najít váš **aplikace b2c rozšíření** a vyberte ho
3. V části 'Essentials' záznam hello **ID aplikace** a hello **ID objektu**
4. Zahrnout do AAD běžné technické metadata profil jako následujícím způsobem:

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item> <!-- This is hello "Object ID" from hello "b2c-extensions-app"-->
                <Item Key="ClientId">insert appId here</Item> <!--This is hello "Application ID" from hello "b2c-extensions-app"-->
              </Metadata>
```

konzistence tookeep hello portálu prostředí, vytvořte tyto atributy pomocí portálu hello uživatelského rozhraní *před* je použijete v vlastní zásady.  Při vytváření atributu "ActivationStatus" hello portálu musí odkazovat tooit následujícím způsobem:

```
extension_ActivationStatus in hello custom policy
extension_<app-guid>_ActivationStatus via hello Graph API.
```


## <a name="reference"></a>Referenční informace

* A **technické profilu (TP)** je typ elementu, který lze považovat za *funkce* název koncového bodu, jeho metadata, používá protokol, který definuje a podrobnosti hello výměny deklarací identity, které hello Identity Rozhraní Framework má být provedena.  Pokud to *funkce* je volána v kroku orchestration nebo z jiného TechnicalProfile hello InputClaims a OutputClaims jsou poskytovány jako parametry volající hello.


* Úplné zpracování u rozšíření vlastností, najdete v článku hello [rozšíření schématu adresáře | KONCEPTY GRAPH API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)

>[!NOTE]
>Rozšíření atributů v rozhraní Graph API jsou pojmenované pomocí konvencí hello `extension_ApplicationObjectID_attributename`. Vlastní zásady odkazovat tooextensions atributy jako extension_attributename, proto vynechání hello ApplicationObjectId v hello XML
