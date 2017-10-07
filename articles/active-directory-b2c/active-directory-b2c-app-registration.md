---
title: 'Azure Active Directory B2C: Registrace aplikace | Dokumentace Microsoftu'
description: "Jak tooregister vaší aplikace pomocí Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: PatAltimore
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f69
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: parakhj
ms.openlocfilehash: bd58e123751db387d6c8f16bd010291ba698b1a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Registrace vaší aplikace

Tento rychlý start vám pomůže zaregistrovat aplikaci v tenantovi Microsoft Azure Active Directory (Azure AD) B2C během několika minut. Jakmile budete hotovi, vaše aplikace je zaregistrovaný pro použití v klientovi hello Azure B2C.

## <a name="prerequisites"></a>Požadavky

toobuild aplikaci, která podporuje registrace a přihlašování uživatelů, musíte nejdřív aplikace hello tooregister s klienta služby Azure Active Directory B2C. Vlastního klienta získáte pomocí hello kroků uvedených v [vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md).

Aplikace vytvořené v okně hello Azure AD B2C v hello portál Azure musí být spravován z hello stejné umístění. Pokud chcete upravit hello B2C aplikací pomocí prostředí PowerShell nebo jiný portál, stane nepodporovaný a nefungují s Azure AD B2C. Zobrazit podrobnosti v hello [došlo k chybě aplikace](#faulted-apps) části. 

## <a name="navigate-toob2c-settings"></a>Přejděte tooB2C nastavení

Přihlaste se toohello [portál Azure](https://portal.azure.com/) jako globální správce klienta hello B2C hello. 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](../../includes/active-directory-b2c-portal-navigate-b2c-service.md)]

Vyberte další kroky na základě typu aplikace hello, které chcete zaregistrovat:

* [Registrace webové aplikace](#register-a-web-app)
* [Registrace webového rozhraní API](#register-a-web-api)
* [Registrace mobilní nebo nativní aplikace](#register-a-mobile-or-native-app)
 
## <a name="register-a-web-app"></a>Registrace webové aplikace

[!INCLUDE [active-directory-b2c-register-web-app](../../includes/active-directory-b2c-register-web-app.md)]

Pokud vaše webová aplikace volá webové rozhraní API zabezpečené pomocí Azure AD B2C, proveďte tyto kroky:
   1. Vytvořte tajný klíč aplikace tak, že budete toohello **klíče** okno a kliknutím na hello **vygenerovat klíč** tlačítko. Poznamenejte si hello **klíč aplikace** hodnotu. Použijte hodnotu hello jako tajný klíč aplikace hello v kódu aplikace.
   2. Klikněte na **Přístup přes rozhraní API**, pak na **Přidat** a vyberte vaše webové rozhraní API a obory (oprávnění).

> [!NOTE]
> **Tajný klíč aplikace** je důležitý údaj zabezpečení a musí být řádně zabezpečen.
> 

[Přeskočit příliš**další kroky**](#next-steps)

## <a name="register-a-web-api"></a>Registrace webové rozhraní API

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

Klikněte na tlačítko **publikovaná obory** tooadd více rozsahy podle potřeby. Ve výchozím nastavení je definováno obor "user_impersonation" hello. obor user_impersonation Hello poskytuje další aplikace hello možnost tooaccess toto rozhraní api jménem hello přihlášeného uživatele. Pokud chcete, můžete odebrat hello user_impersonation oboru.

[Přeskočit příliš**další kroky**](#next-steps)

## <a name="register-a-mobile-or-native-app"></a>Registrace mobilní nebo nativní aplikace

[!INCLUDE [active-directory-b2c-register-mobile-native-app](../../includes/active-directory-b2c-register-mobile-native-app.md)]

[Přeskočit příliš**další kroky**](#next-steps)

## <a name="limitations"></a>Omezení

### <a name="choosing-a-web-app-or-api-reply-url"></a>Výběr adresy URL odpovědi webové aplikace nebo rozhraní API

Aplikace, které jsou registrovány Azure AD B2C v současné době se s omezeným přístupem tooa omezenou sadu hodnot adresa URL odpovědi. Hello odpověď adresu URL pro webové aplikace a služby musí začínat řetězcem schématu hello `https`, a všechny odpovědi hodnoty adresa URL musí sdílí jedinou doménu DNS. Například nemůžete zaregistrovat webovou aplikaci s některou z těchto adres URL odpovědi:

`https://login-east.contoso.com`

`https://login-west.contoso.com`

systém registrace Hello porovná hello celý název DNS hello existující odpovědi URL toohello název DNS hello adresa URL odpovědi, který chcete přidat. Hello požadavek tooadd hello název DNS selže, pokud platí některá z hello následující podmínky:

* Hello celý název DNS hello nové adresa URL odpovědi neodpovídá názvu DNS hello hello stávající adresa URL odpovědi.
* Hello celý název DNS hello nové adresa URL odpovědi není subdoména hello stávající adresa URL odpovědi.

Například pokud hello aplikace má tato adresa URL odpovědi:

`https://login.contoso.com`

Můžete přidat tooit takto:

`https://login.contoso.com/new`

V tomto případě název DNS hello přesně odpovídá. Nebo můžete provést toto:

`https://new.login.contoso.com`

V takovém případě odkazujete tooa DNS subdoménou login.contoso.com. Pokud chcete toohave aplikaci, která má přihlášení east.contoso.com a west.contoso.com přihlášení jako odpověď adresy URL, je nutné přidat tyto adresy URL odpovědí v tomto pořadí:

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Protože jsou subdomény hello první adresa URL odpovědi, můžete přidat hello dvou contoso.com.

### <a name="choosing-a-native-app-redirect-uri"></a>Výběr identifikátoru URI přesměrování nativní aplikace

Existují dva důležité aspekty při výběru identifikátoru URI přesměrování pro mobilní/nativní aplikace:

* **Jedinečné**: hello schéma hello identifikátor URI přesměrování by měl být jedinečný pro každou aplikaci. V našem příkladu (com.onmicrosoft.contoso.appname://redirect/path) používáme com.onmicrosoft.contoso.appname hello schéma. Doporučujeme používat tento vzor. Pokud dvě aplikace sdílet hello stejné scheme, hello uživateli se zobrazí dialogové okno "Vyberte aplikace". Pokud uživatel hello provede nesprávné volbě, hello přihlášení selže.
* **Úplnost:** Identifikátor URI přesměrování musí mít schéma a cestu. Hello cesta musí obsahovat alespoň jeden lomítkem po hello domény (například //contoso/ funguje a selže //contoso).

Ujistěte se, že neexistují žádné speciální znaky, jako identifikátor uri pro přesměrování podtržítka v hello.

### <a name="faulted-apps"></a>Chybné aplikace

Aplikace B2C se NESMÍ upravovat:

* Na jiných portálech pro správu aplikací, jako je [portál Azure Classic](https://manage.windowsazure.com/) a [Portál pro registraci aplikací](https://apps.dev.microsoft.com/).
* Pomocí rozhraní Graph API nebo PowerShellu.

Pokud jste upravit aplikaci hello B2C, jak je popsáno výše a opakujte tooedit ho znovu okno s funkcemi hello Azure AD B2C na portálu Azure hello, stane se aplikace pro práci s chybou a aplikace již není použitelné s Azure AD B2C. Aplikace hello toodelete a vytvořte ho znovu.

toodelete hello aplikace, přejděte toohello [portálu pro registraci aplikace](https://apps.dev.microsoft.com/) a odstranit hello aplikaci existuje. Aby toobe aplikace hello viditelné je třeba vlastník hello toobe aplikace hello (a ne jenom správce hello klienta).

## <a name="next-steps"></a>Další kroky

Teď, když máte aplikaci registrovanou v Azure AD B2C, můžete dokončit jeden z [naše kurzy úvodní](active-directory-b2c-overview.md#get-started) tooget fungovaly.

> [!div class="nextstepaction"]
> [Vytvoření webové aplikace ASP.NET s registrací, přihlášením a resetováním hesla](active-directory-b2c-devquickstarts-web-dotnet-susi.md)