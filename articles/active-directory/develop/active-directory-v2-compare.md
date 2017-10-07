---
title: "aaaWhat se liší v koncového bodu v2.0 hello Azure AD? | Dokumentace Microsoftu"
description: "Porovnání hello původní Azure AD a hello v2.0 koncové body."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 5060da46-b091-4e25-9fa8-af4ae4359b6c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: e7ed196f9053fc21db799cd6bc513ba5c2b92885
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="whats-different-about-hello-v20-endpoint"></a>Co se liší o koncového bodu v2.0 hello?
Pokud jste se seznámili s Azure Active Directory nebo aplikací mít integrované s Azure AD v posledních hello, může být určité rozdíly v koncového bodu v2.0 hello, které by uživatel očekával.  Tento dokument se volá na těchto rozdílů za pochopení.

> [!NOTE]
> Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0 hello.  toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
>

## <a name="microsoft-accounts-and-azure-ad-accounts"></a>Účty Microsoft a účty Azure AD
koncový bod v2.0 Hello umožňují vývojářům toowrite aplikace, které přijímají přihlášení z účtů Microsoft Accounts i Azure AD, koncový bod jedné ověřování pomocí.  Tato poskytuje hello možnost toowrite aplikaci zcela účet bez ohledu na; může být hello typu účtu, který hello uživatel se přihlásí, které ignorují.  Samozřejmě můžete *můžete* zvýšit vaši aplikaci využívající hello typu účtu se používá v konkrétní relace, ale nemusíte.

Například pokud aplikace zavolá hello [Microsoft Graph](https://graph.microsoft.io), některé další funkce a dat budou k dispozici tooenterprise uživatele, například svých webech služby SharePoint nebo dat adresáře.  Ale pro mnoho akce například [čtení e-mailu uživatele](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), hello kódu je možné zapisovat přesně hello stejné pro účty Microsoft Accounts i Azure AD.  

Integrace aplikace s Accounts Microsoft a účty Azure AD je nyní jeden jednoduchý proces.  Můžete použít jednu sadu koncové body, jeden knihovny a jediné aplikaci registrace toogain přístup tooboth hello spotřebních a podnikových světů.  Další informace o hello koncového bodu v2.0, podívejte se na toolearn [hello přehled](active-directory-appmodel-v2-overview.md).

## <a name="new-app-registration-portal"></a>Nový portál pro registraci aplikace
tooregister aplikaci, která funguje s koncovým bodem v2.0 hello, musíte použít nové portálu pro registraci aplikace: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Toto je hello portál, kde lze získat ID aplikace přizpůsobit vzhled hello přihlašovací stránku vaší aplikace a další.  Stačí tooaccess hello portál je účet používá technologii Microsoft - osobní nebo pracovní nebo školní účet.

## <a name="one-app-id-for-all-platforms"></a>ID jednu aplikaci pro všechny platformy
Pokud jste použili Azure Active Directory, pravděpodobně jste registrováni několik různých aplikací pro jeden projekt.  Například pokud jste vytvořili web a aplikaci pro iOS, musíte tooregister je samostatně pomocí dvou různých ID aplikace. portál pro registraci aplikace Hello Azure AD vynutit toomake můžete tento rozdíl během registrace:

![Původní registrace aplikace uživatelského rozhraní](../../media/active-directory-v2-flows/old_app_registration.PNG)

Podobně pokud jste měli web a back-endové webové rozhraní api, může jste si zaregistrovali každý jako samostatnou aplikaci ve službě Azure AD.  Nebo pokud jste měli k aplikaci pro iOS a aplikace pro Android, můžete také může mít registrovaný dvě různé aplikace.  Registrace jednotlivých součástí aplikace vedla toosome neočekávané chování pro vývojáře a zákazníkům:

* Jednotlivé komponenty zobrazovaly jako samostatnou aplikaci v klientovi služby Azure Active Directory hello jednotlivých zákazníků.
* Správce klienta se pokus o tooapply zásad, spravovat přístup k, nebo odstranit aplikaci, byste mají toodo Ano pro jednotlivé součásti aplikace hello.
* Když zákazníci dá souhlas tooan aplikace, by jednotlivé komponenty zobrazí se v úvodní obrazovka souhlasu jako odlišné aplikace.

S koncovým bodem v2.0 hello teď můžete zaregistrovat všechny součásti projektu jako registrace jedna aplikace a použít jeden Id aplikace pro celý projekt.  Můžete přidat několik tooa "platformy" každý projekt a zadejte pro každou platformu, kterou přidáte hello příslušná data.  Samozřejmě můžete vytvořit libovolný počet aplikací, jako třeba v závislosti na požadavcích, ale pro hello většině případů musí být pouze jedno Id aplikace potřebné.

Naše cílem je, že to vést tooa další zjednodušené správy aplikací a vývojové prostředí a vytvořit více konsolidované zobrazení jeden projekt, který je možné, že pracujete na.

## <a name="scopes-not-resources"></a>Obory, ne prostředky
V Azure Active Directory, můžete aplikaci chovat jako **prostředků**, nebo příjemce tokenů.  Prostředek můžete definovat několika **obory** nebo **oAuth2Permissions** , že rozumí, povolení toorequest tokeny toothat prostředku pro určitou sadu obory klientské aplikace.  Vezměte v úvahu hello Azure AD Graph API jako příklad prostředku:

* Identifikátor prostředku nebo `AppID URI`:`https://graph.windows.net/`
* Obory, nebo `OAuth2Permissions`: `Directory.Read`, `Directory.Write`atd.  

Všechny tyto platí pro koncový bod v2.0 hello hello.  Aplikace může stále chovat jako prostředek, definovat obory a identifikovat podle identifikátoru URI.  Klientské aplikace může stále požádat o přístup toothose obory.  Ale hello způsob, ve kterém klient požaduje tato oprávnění se změnil.  V posledních hello autorizace OAuth 2.0 tooAzure žádosti, které může mít hledá AD jako:

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

kde hello **prostředků** parametr uvedené, které prostředků hello klientská aplikace požaduje autorizaci pro.  Azure AD vypočtená hello oprávněních hello aplikace na základě statické konfigurace v hello portálu Azure a vystavené tokeny odpovídajícím způsobem.  Nyní hello stejné OAuth 2.0 autorizace požadavku vypadá jako:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

kde hello **oboru** parametr určuje, které aplikace hello prostředků a oprávnění požaduje autorizaci pro. Hello požadovaných prostředků je stále velmi součástí požadavku hello – jednoduše je zahrnut v každém hello hodnot parametru oboru hello.  Pomocí parametru oboru hello tímto způsobem umožňuje toobe koncový bod v2.0 hello více kompatibilní se specifikací hello OAuth 2.0 a přesněji zarovnaná s obvyklé postupy odvětví.  Umožňuje také aplikace tooperform [přírůstkové souhlasu](#incremental-and-dynamic-consent), který je popsán v další části hello.

## <a name="incremental-and-dynamic-consent"></a>Přírůstkové a dynamické souhlasu
Aplikace registrované v Azure AD dříve potřeby toospecify jejich požadovaná oprávnění OAuth 2.0 v hello portálu Azure, v okamžiku vytvoření aplikace:

![Registrace oprávnění uživatelského rozhraní](../../media/active-directory-v2-flows/app_reg_permissions.PNG)

požadované aplikace byla nakonfigurována oprávnění Hello **staticky**.  Zatímco to povoleno konfigurace tooexist aplikace hello v hello portálu Azure a uchovávat hello kód dobrý a jednoduché, uvede několik problémů pro vývojáře:

* Aplikace měla tooknow všechna oprávnění hello někdy potřebovat při vytváření aplikace.  Přidání oprávnění v čase bylo obtížné procesu.
* Aplikace měla tooknow všechny prostředky hello by někdy přístup předem.  Bylo obtížné toocreate aplikace, které mají přístup libovolný počet prostředků.
* Aplikace měla toorequest všechna oprávnění hello, které někdy potřebovat při hello na prvním přihlášení uživatele.  V některých případech to vedlo tooa velmi dlouhý seznam oprávnění, která se nedoporučuje. koncoví uživatelé z schválení aplikace hello přístup na počáteční přihlášení.

S koncovým bodem v2.0 hello, můžete zadat oprávnění hello vašim potřebám aplikace **dynamicky**, v době běhu během regulární využití vaší aplikace.  toodo Ano, můžete zadat hello rozsahy vašim potřebám aplikace v libovolném bodě v čase zahrnutím v hello `scope` parametr požadavku ověřování:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

Hello výše uvedených požadavků oprávnění pro tooread aplikace hello uživatele Azure AD data adresáře a také adresáře tootheir dat zápisu.  Pokud uživatel hello souhlasí toothose oprávnění v hello posledních pro tuto konkrétní aplikaci, bude jednoduše zadat své přihlašovací údaje a být přihlášeni k aplikace hello.  Pokud uživatel hello nedala souhlas tooany těchto oprávnění, koncového bodu v2.0 hello požádá uživatele hello souhlasu toothose oprávnění.  Další toolearn, můžete si přečíst na [oprávnění, souhlasu a obory](active-directory-v2-scopes.md).

Umožňuje aplikaci oprávnění toorequest dynamicky prostřednictvím hello `scope` parametr získáte plnou kontrolu nad možnosti pro uživatele.  Pokud chcete, můžete toofrontload vašeho souhlasu prostředí a požádat o všechna oprávnění v jedné žádosti počáteční autorizace.  Nebo pokud vaše aplikace vyžaduje velký počet oprávnění, můžete zvolit toogather tato oprávnění od uživatele hello postupně, jak pokoušejí určité funkce vaší aplikace pro toouse v čase.

## <a name="well-known-scopes"></a>Známé oborů
#### <a name="offline-access"></a>Přístup v režimu offline
Aplikace pomocí koncového bodu v2.0 hello může vyžadovat použití hello nové dobře známé oprávnění pro aplikace - hello `offline_access` oboru.  Všechny aplikace bude nutné toorequest toto oprávnění, pokud potřebují tooaccess prostředkům jménem hello uživatele pro delší dobu, i když uživatel hello nemusí být aplikaci aktivně hello.  Hello `offline_access` obor se zobrazí uživateli toohello v souhlas v dialogových oknech jako "Přístup k datům v režimu offline", které hello uživatel musí vyjádřit souhlas s.  Vyžádání hello `offline_access` oprávnění umožní vaší webové aplikace tooreceive OAuth 2.0 refresh_tokens z koncového bodu v2.0 hello.  Refresh_tokens je dlouhodobé a může být vyměňují pro nové access_tokens OAuth 2.0 po delší dobu přístupu.  

Pokud aplikace nevyžaduje hello `offline_access` oboru, neobdrží refresh_tokens.  To znamená, že pokud uplatníte authorization_code v toku kódu autorizace hello OAuth 2.0, pouze obdržíte zpět access_token z hello `/token` koncový bod.  Tento access_token zůstane platná, a to krátkou dobu (obvykle jedna hodina), ale nakonec vyprší.  V tomto bodě v čase, vaše aplikace bude potřebovat tooredirect hello uživatele zpět toohello `/authorize` tooretrieve koncový bod nové authorization_code.  Během této přesměrování hello uživatel může nebo nemusí potřebovat tooenter jejich přihlašovací údaje znovu nebo znovu souhlas toopermissions, v závislosti na typu hello hello aplikace.

Další informace o protokolu OAuth 2.0, refresh_tokens a access_tokens, podívejte se na hello toolearn [referenci na protokol v2.0](active-directory-v2-protocols.md).

#### <a name="openid-profile-and-email"></a>OpenID, profil a e-mailu
V minulosti by hello nejzákladnější tok OpenID Connect přihlášení s Azure Active Directory poskytují množství informací o hello uživatele v požadavku id_token výsledné hello.  deklarace identity Hello v požadavku id_token může obsahovat uživatele hello název, upřednostňované uživatelské jméno, e-mailovou adresu, ID objektu a další.

Snažíme se teď omezení hello informace, že hello `openid` oboru dává vám přístup aplikace k.  obor, openid, Hello bude pouze povolit vaší aplikace toosign hello uživateli v a přijímat identifikátor specifický pro aplikace pro uživatele hello.  Pokud chcete tooobtain identifikovatelné osobní údaje (PII) o hello uživateli ve vaší aplikaci, vaše aplikace bude potřebovat další oprávnění toorequest od uživatele hello.  Představujeme dva nové obory – hello `email` a `profile` obory – které umožňují toodo tak.

Hello `email` obor je velmi jednoduchá – umožňuje vaší aplikace přístup toohello primární e-mailovou adresu uživatele prostřednictvím hello `email` deklarací identity v požadavku id_token hello.  Hello `profile` oboru poskytuje tooall přístup k vaší aplikaci další základní informace o uživateli hello – jejich název, upřednostňované uživatelského jména, ID objektu a tak dále.

To vám umožní toocode aplikace způsobem minimální zpřístupnění – můžete pouze požádat hello uživatele pro sadu hello informací, že vaše aplikace vyžaduje toodo úlohy.  Další informace o těchto oborů, najdete v části příliš[hello odkaz oboru v2.0](active-directory-v2-scopes.md).

## <a name="token-claims"></a>Token deklarací identity
Hello deklarace identity v tokenech vystavený koncového bodu v2.0 hello nebudou identické tootokens vydaný koncové body hello všeobecně dostupná Azure AD – aplikace migraci toohello nové služby by neměl Předpokládejme, že konkrétní deklaraci identity, budou v id_tokens nebo access_tokens existovat. toolearn o hello specifických deklarací vygenerované v v2.0 tokeny, najdete v části hello [odkaz tokenu v2.0](active-directory-v2-tokens.md).

## <a name="limitations"></a>Omezení
Existuje několik omezení toobe vědět, když používáte bod v2.0 hello.  Naleznete toohello [v2.0 omezení doc](active-directory-v2-limitations.md) toosee, pokud platí některé z těchto omezení tooyour určitého scénáře.
