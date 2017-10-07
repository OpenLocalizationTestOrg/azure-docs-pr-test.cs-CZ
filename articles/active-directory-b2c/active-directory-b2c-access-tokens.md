---
title: "Azure Active Directory B2C: Požaduje přístupové tokeny | Microsoft Docs"
description: "Tento článek vám ukáže, jak toosetup klientskou aplikaci a získat přístupový token."
services: active-directory-b2c
documentationcenter: android
author: parakhj
manager: krassk
editor: 
ms.assetid: 1c75f17f-5ec5-493a-b906-f543b3b1ea66
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: parakhj
ms.openlocfilehash: 410639424fd4e5119a5d58751dfdb6e8957fd100
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-requesting-access-tokens"></a>Azure AD B2C: Požaduje přístupové tokeny

Přístupový token (označené jako **přístup\_tokenu** v hello odpovědí z Azure AD B2C) je formulář tokenu zabezpečení, který může klient používat tooaccess prostředky, které jsou zabezpečeny [serveru ověřování](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-protocols#the-basics), jako jsou webové rozhraní API. Přístupové tokeny, jsou reprezentovány jako [Jwt](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-tokens#types-of-tokens) a obsahují informace o hello určený prostředek a hello udělená oprávnění toohello serveru. Při volání metody server hello prostředků, musí být přítomen v požadavku HTTP hello hello přístupový token.

Tento článek popisuje, jak tooconfigure klientské aplikace a webové rozhraní API v pořadí tooobtain **přístup\_tokenu**.

> [!NOTE]
> **Webové rozhraní API řetězy (On-Behalf-Of) nepodporuje Azure AD B2C.**
>
> Mnoho architektur zahrnuje webové rozhraní API, které potřebuje toocall jiné podřízené webové rozhraní API, i zabezpečené pomocí Azure AD B2C. Tento scénář je častý u nativních klientů, které mají webové rozhraní API back-end, které zavolá online službu Microsoftu, jako je například hello Azure AD Graph API.
>
> Tento scénář zřetězených webových rozhraní API může být podporován pomocí hello udělení přihlašovacích údajů nosiče OAuth 2.0 JWT, jinak hodnota označuje jako hello tok On-Behalf-Of. Hello tok On-Behalf-Of však není aktuálně implementována v Azure AD B2C.

## <a name="register-a-web-api-and-publish-permissions"></a>Webové rozhraní API pro registraci a publikování oprávnění

Před vyžádáním přístupový token, nejprve nutné tooregister webového rozhraní API a publikujte oprávnění (oborům), která může toohello klientské aplikace.

### <a name="register-a-web-api"></a>Registrace webové rozhraní API

1. Na okno s funkcemi hello B2C na hello portálu Azure, klikněte na tlačítko **aplikace**.
1. Klikněte na tlačítko **+ přidat** hello horní části okna hello.
1. Zadejte **název** hello aplikace, který popíše tooconsumers vaší aplikace. Můžete například zadat "API společnosti Contoso".
1. Přepnutí hello **zahrnout webovou aplikaci / webové rozhraní API** přepínač příliš**Ano**.
1. Zadejte libovolnou hodnotu hello **adresy URL odpovědí**. Zadejte například `https://localhost:44316/`. Hodnota Hello nezáleží, protože rozhraní API by neměl být přijetí hello token přímo z Azure AD B2C.
1. Zadejte **Identifikátor URI ID aplikace**. Toto je identifikátor hello používá pro webového rozhraní API. Zadejte například "Poznámky" hello pole. Hello **identifikátor ID URI aplikace** by pak bylo `https://{tenantName}.onmicrosoft.com/notes`.
1. Klikněte na tlačítko **vytvořit** tooregister vaší aplikace.
1. Klikněte na tlačítko hello aplikace, kterou jste právě vytvořili a poznamenejte hello globálně jedinečný **ID klienta aplikace** , které budete používat později ve svém kódu.

### <a name="publishing-permissions"></a>Publikování oprávnění

Obory, které se podobá toopermissions, jsou nezbytné, pokud vaše aplikace je volání rozhraní API. Některé příklady oborů jsou "číst" nebo "zápis". Předpokládejme, že chcete web nebo nativní aplikaci příliš "číst" z rozhraní API. Aplikace by volání Azure AD B2C a žádosti o token přístupu, který poskytuje obor přístupu toohello "číst". Aby Azure AD B2C tooemit takový token přístupu musí aplikace hello toobe udělena oprávnění příliš "číst" z konkrétní API hello. toodo tohoto oboru "číst" hello toopublish nejprve potřebuje vaše rozhraní API.

1. V rámci hello Azure AD B2C **aplikace** okně Otevřít hello webové aplikace rozhraní API ("API společnosti Contoso").
1. Klikněte na **Publikované obory**. Toto je, kde můžete definovat hello oprávnění (oborům), která může tooother aplikace.
1. Přidat **hodnoty oboru** podle potřeby (například "číst"). Ve výchozím nastavení bude hello "user_impersonation" oboru definován. To můžete ignorovat, pokud chcete. Zadejte popis hello oboru v hello **název oboru** sloupce.
1. Klikněte na **Uložit**.

> [!IMPORTANT]
> Hello **název oboru** hello popis hello **hodnota oboru**. Při použití hello oboru, ujistěte se, zda text hello toouse **hodnota oboru**.

## <a name="grant-a-native-or-web-app-permissions-tooa-web-api"></a>Udělit nativní nebo webové aplikaci oprávnění tooa webového rozhraní API

Nakonfigurované toopublish obory po rozhraní API klienta aplikace hello musí toobe udělena tyto obory prostřednictvím hello portálu Azure.

1. Přejděte toohello **aplikace** nabídky v okno s funkcemi hello B2C.
1. Registrace aplikace klienta ([webové aplikace](active-directory-b2c-app-registration.md#register-a-web-app) nebo [nativního klienta](active-directory-b2c-app-registration.md#register-a-mobile-or-native-app)) Pokud již nemáte. Použijete-li tato příručka jako počáteční bod, budete potřebovat tooregister klientské aplikace.
1. Klikněte na **přístup pomocí rozhraní API**.
1. Klikněte na **Přidat**.
1. Vyberte webové rozhraní API a hello obory (oprávnění), které byste chtěli toogrant.
1. Klikněte na **OK**.

> [!NOTE]
> Azure AD B2C nezeptá vašeho klienta aplikace uživatelům na jejich souhlasu. Místo toho jsou poskytovány všechny souhlasu Dobrý den, správce, na základě oprávnění hello nakonfigurované mezi aplikacemi hello popsané výše. Pokud odvolán udělit oprávnění pro aplikaci, všichni uživatelé, kteří byly dříve možné tooacquire tato oprávnění přestane být schopný toodo tak.

## <a name="requesting-a-token"></a>Požadování tokenu

Žádosti o token přístupu, klientská aplikace hello potřebuje toospecify hello požadovaných oprávnění v hello **oboru** parametr hello požadavku. Například toospecify hello **hodnota oboru** "číst" pro hello rozhraní API, která má hello **identifikátor ID URI aplikace** z `https://contoso.onmicrosoft.com/notes`, bude hello oboru `https://contoso.onmicrosoft.com/notes/read`. Dole je příklad autorizační kód požadavku toohello `/authorize` koncový bod.

> [!NOTE]
> Vlastní domény nejsou aktuálně podporované společně s přístupových tokenů. Je nutné použít doménu tenantName.onmicrosoft.com v adrese URL žádosti hello.

```
https://login.microsoftonline.com/<tenantName>.onmicrosoft.com/oauth2/v2.0/authorize?p=<yourPolicyId>&client_id=<appID_of_your_client_application>&nonce=anyRandomValue&redirect_uri=<redirect_uri_of_your_client_application>&scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread&response_type=code 
```

tooacquire více oprávnění v hello stejné žádosti, můžete přidat více položek v jednom hello **oboru** parametr, oddělené mezerami. Například:

Adresa URL dekódovat:

```
scope=https://contoso.onmicrosoft.com/notes/read openid offline_access
```

Kódovaná adresou URL:

```
scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread%20openid%20offline_access
```

Může požádat o další obory (oprávnění) pro prostředek, než co je povolen pro klientské aplikace. Pokud se jedná o případ hello, hello volání bude úspěšné, pokud alespoň jeden je povoleno. Výsledná Hello **přístup\_tokenu** bude mít svůj požadavek "spojovací bod služby" zahrnovat pouze hello oprávnění, které byly úspěšně udělen.

> [!NOTE] 
> Nepodporujeme vyžadování oprávnění pro dva různé webové prostředky hello stejném požadavku. Tento druh požadavek selže.

### <a name="special-cases"></a>Zvláštní případy

Hello OpenID Connect standardní určuje několik hodnot speciální "obor". Hello následující oprávnění hello představují zvláštní obory příliš "přístup k profilu uživatele hello":

* **openid**: to požadavky tokenu ID
* **offline\_přístup**: to požadavky obnovovací token (pomocí [tok ověřování kódu](active-directory-b2c-reference-oauth-code.md)).

Pokud hello `response_type` parametr `/authorize` žádost obsahuje `token`, hello `scope` parametr musí obsahovat alespoň jeden prostředek oboru (jiné než `openid` a `offline_access`), bude udělen. V opačném hello `/authorize` požadavku se ukončí se selháním.

## <a name="hello-returned-token"></a>Vrátí token Hello

V úspěšně minted **přístup\_tokenu** (z buď hello `/authorize` nebo `/token` koncového bodu), hello následující deklarace identity, bude k dispozici:

| Name (Název) | Deklarovat | Popis |
| --- | --- | --- |
|Cílová skupina |`aud` |Hello **ID aplikace** jednoho prostředku hello token tuto hello uděluje přístup k. |
|Rozsah |`scp` |udělení oprávnění Hello toohello prostředků. Více udělená oprávnění budou oddělte mezerou. |
|Autorizovaný strany |`azp` |Hello **ID aplikace** hello klienta aplikace, která hello žádost iniciovala. |

Pokud vaše rozhraní API obdrží hello **přístup\_tokenu**, musí [ověřit hello token](active-directory-b2c-reference-tokens.md) tooprove, který hello tokenu je platná a má správný deklarace identity hello.

Snažíme se vždy otevřete toofeedback a návrhy! Pokud máte jakékoli problémy s tímto tématem, prosím zveřejnit na Stack Overflow pomocí značky hello [, azure ad b2c,](https://stackoverflow.com/questions/tagged/azure-ad-b2c). Pro žádosti o funkce, přidejte je příliš[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

## <a name="next-steps"></a>Další kroky

* Sestavení webového rozhraní API pomocí [.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi)
* Sestavení webového rozhraní API pomocí [Node.JS](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi)
