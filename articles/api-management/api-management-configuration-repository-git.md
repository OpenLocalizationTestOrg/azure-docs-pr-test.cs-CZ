---
title: "aaaConfigure služby API Management pomocí Git - Azure | Microsoft Docs"
description: "Zjistěte, jak toosave a nakonfigurujte konfigurace služby API Management pomocí Git."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: mattfarm
ms.assetid: 364cd53e-88fb-4301-a093-f132fa1f88f5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: ef7d4c18f2ea3f5c9b86403349a83aef240f979b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosave-and-configure-your-api-management-service-configuration-using-git"></a>Jak toosave a nakonfigurujte konfigurace služby API Management pomocí Git
> 
> 

Každá instance služby API Management udržuje databázi konfigurace, která obsahuje informace o konfiguraci hello a metadata pro instance služby hello. Můžete provádět změny instance služby toohello Změna nastavení hello portálu vydavatele, pomocí rutiny prostředí PowerShell nebo volání rozhraní REST API. Kromě toho toothese metod, můžete také spravovat konfigurace instance služby pomocí Git, například povolení scénářů správy služby:

* Konfigurace správy verzí - stažení a uložení různých verzích konfigurace služby
* Hromadné změny konfigurace – proveďte změny toomultiple součástí konfigurace služby v místním úložišti a integrovat hello změny zpět toohello server s jedinou operací.
* Známých nástrojů Git a pracovní postup - používat hello Git nástrojů a pracovní postupy, které jste již obeznámeni s

Hello následující diagram ukazuje přehled hello různé způsoby tooconfigure instanci služby API Management.

![Konfigurace Git][api-management-git-configure]

Pokud provedete změny tooyour služby pomocí portálu vydavatele hello, rutiny prostředí PowerShell nebo hello REST API, kterou spravujete konfigurační databáze služby service pomocí hello `https://{name}.management.azure-api.net` koncový bod, jak je znázorněno na pravé straně hello hello diagramu. Hello levé straně hello diagram znázorňuje, jak můžete spravovat konfiguraci služby pomocí Git a úložiště Git pro vaši službu na `https://{name}.scm.azure-api.net`.

Hello následující kroky poskytují přehled správy pomocí Git instanci služby API Management.

1. Konfigurace přístupu Git ve službě
2. Uložit úložiště Git tooyour databáze služby konfigurace
3. Klonování hello Git úložišti tooyour místního počítače
4. Nejnovější úložišti hello dolů tooyour místního počítače a potvrzení a posílejte nabízená oznámení změny zpět tooyour úložišti pro vyžádání obsahu
5. Nasadit hello změny z vašeho úložiště do konfigurační databáze služby service

Tento článek popisuje, jak tooenable pomocí Git toomanage konfigurace služby a poskytuje odkaz pro hello soubory a složky v úložišti Git hello.

## <a name="access-git-configuration-in-your-service"></a>Konfigurace přístupu Git ve službě
Můžete rychle zobrazit stav hello konfiguraci Git zobrazením hello Git ikonu v pravém horním rohu hello portálu vydavatele hello. V tomto příkladu hello stavová zpráva znamená, že se úložiště toohello neuložené změny. Je to proto, že konfigurační databázi služby API Management hello ještě nebyl uložen toohello úložiště.

![Stav Git][api-management-git-icon-enable]

tooview a nakonfigurovat nastavení konfigurace Git, můžete kliknutím na ikonu hello Git, nebo klikněte na tlačítko hello **zabezpečení** nabídky a přejděte toohello **konfigurace úložiště** kartě.

![Povolit GIT][api-management-enable-git]

> [!IMPORTANT]
> Všechny tajné klíče, které nejsou definovány vlastnosti bude uložen v úložišti hello a zůstane v historii dokud zakázat a znovu povolte přístup Git. Vlastnosti poskytují bezpečné místo toomanage konstantní řetězcové hodnoty, včetně tajných klíčů, ve všech konfigurací rozhraní API a zásady, takže není nutné toostore je přímo v příkazech jazyka zásad. Další informace najdete v tématu [jak toouse vlastnosti v Azure API Management zásady](api-management-howto-properties.md).
> 
> 

Informace o povolení nebo zakázání Git přístup pomocí hello REST API najdete v tématu [povolit nebo zakázat Git přístup pomocí rozhraní REST API hello](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).

## <a name="toosave-hello-service-configuration-toohello-git-repository"></a>toosave hello služby konfigurace toohello úložiště Git
prvním krokem Hello před klonováním hello úložiště je toosave hello aktuální stav hello služby konfigurace toohello úložiště. Klikněte na tlačítko **uložit konfiguraci toorepository**.

![Uložte konfiguraci][api-management-save-configuration]

Všechny požadované změny provádějte na potvrzovací obrazovce a hello a klikněte na tlačítko **Ok** toosave.

![Uložte konfiguraci][api-management-save-configuration-confirm]

Po chvíli hello konfiguraci uložit, a se zobrazí stav konfigurace hello hello úložiště, včetně hello datum a čas poslední změny konfigurace hello a hello poslední synchronizace mezi hello konfigurace služby a hello úložiště.

![Stav konfigurace][api-management-configuration-status]

Po konfiguraci hello je uložit toohello úložiště, můžete klonovat.

Informace o provedení této operace pomocí hello REST API najdete v tématu [potvrzení konfigurace snímek pomocí hello REST API](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).

## <a name="tooclone-hello-repository-tooyour-local-machine"></a>tooclone hello úložiště tooyour místního počítače
tooclone úložiště, musíte úložiště tooyour hello adresu URL, uživatelské jméno a heslo. Hello uživatelské jméno a adresa URL se zobrazí v horní hello části hello **konfigurace úložiště** kartě.

![Klonu Git][api-management-configuration-git-clone]

heslo Hello je vygenerován v hello dolní části hello **konfigurace úložiště** kartě.

![Generovat heslo][api-management-generate-password]

toogenerate heslo, nejdříve se ujistěte, že hello **vypršení platnosti** je nastavit toohello potřeby datum a čas vypršení a pak klikněte na tlačítko **vygenerovat Token**.

![Heslo][api-management-password]

> [!IMPORTANT]
> Toto heslo si poznamenejte. Po opuštění této stránky hello heslo se znovu nezobrazí.
> 
> 

Následující příklady použití hello Git Bash Hello nástroje z [Git pro Windows](http://www.git-scm.com/downloads) ale můžete použít jakýkoli Git nástroj, který jste se seznámili s.

Otevřete svůj nástroj Git do požadované složky hello a spusťte následující příkaz tooclone hello git úložiště tooyour místního počítače, pomocí příkazu hello poskytované portál vydavatele hello hello.

```
git clone https://bugbashdev4.scm.azure-api.net/
```

Zadejte hello uživatelské jméno a heslo po zobrazení výzvy.

Pokud se zobrazí všechny chyby, zkuste upravit vaše `git clone` příkaz tooinclude hello uživatelské jméno a heslo, jak ukazuje následující příklad hello.

```
git clone https://username:password@bugbashdev4.scm.azure-api.net/
```

Pokud to poskytuje k chybě, zkuste kódování hello heslo část příkazu hello adres URL. Jeden toodo rychlý způsob, jak je to tooopen Visual Studio, a problém hello následující příkaz v hello **hodnot proměnných**. tooopen hello **hodnot proměnných**, otevřete jakékoli řešení nebo produktu project v sadě Visual Studio (nebo vytvořte novou prázdnou konzolovou aplikaci) a zvolte **Windows**, **Immediate** z Hello **ladění** nabídky.

```
?System.NetWebUtility.UrlEncode("password from publisher portal")
```

Použijte heslo hello kódovaný společně s uživatelské jméno a úložiště umístění tooconstruct hello git příkazu.

```
git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/
```

Jakmile je klonovat úložiště hello můžete zobrazit a pracovat v místním systému souborů. Další informace najdete v tématu [souborů a složek struktury odkaz místní úložiště Git](#file-and-folder-structure-reference-of-local-git-repository).

## <a name="tooupdate-your-local-repository-with-hello-most-current-service-instance-configuration"></a>tooupdate místní úložiště s hello nejaktuálnější konfigurace instance služby
Pokud provedete instance služby API Management tooyour změny v portálu vydavatele hello nebo pomocí hello REST API, musíte uložit tyto změny toohello úložiště, než budete moct aktualizovat místní úložiště s nejnovější změny hello. toodo tento, klikněte na tlačítko **uložit konfiguraci toorepository** na hello **konfigurace úložiště** v hello portálu vydavatele a potom vydat hello následující příkaz v místním úložišti.

```
git pull
```

Dřív, než spustíte `git pull` ujistit, že jste hello složku pro místní úložiště. Pokud jste právě dokončili hello `git clone` pak hello directory tooyour úložišti musíte změnit spuštěním příkazu jako hello následující příkaz.

```
cd bugbashdev4.scm.azure-api.net/
```

## <a name="toopush-changes-from-your-local-repo-toohello-server-repo"></a>toopush změn z vaší místní úložiště toohello serveru úložiště
toopush změní z úložiště Místní úložiště toohello serveru, je nutné potvrdit změny a vložit je toohello serveru úložiště. toocommit změny, otevřete Git příkaz nástroj, přepínač toohello adresáře vaší místní úložiště a problém hello následující příkazy.

```
git add --all
git commit -m "Description of your changes"
```

toopush všechny hello potvrdí toohello server, spusťte následující příkaz hello.

```
git push
```

## <a name="toodeploy-any-service-configuration-changes-toohello-api-management-service-instance"></a>toodeploy všechny služby konfigurace změny toohello instance služby API Management
Jakmile jsou tyto místní změny potvrzeny a stisknutí toohello server úložiště, můžete je nasadit tooyour instance služby API Management.

![Nasazení][api-management-configuration-deploy]

Informace o provedení této operace pomocí hello REST API najdete v tématu [nasazení Git změny tooconfiguration databázi pomocí hello REST API](https://docs.microsoft.com/en-us/rest/api/apimanagement/tenantconfiguration).

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a>Odkaz struktury souborů a složek místní úložiště Git
Hello soubory a složky v úložišti místní git hello obsahovat informace o konfiguraci hello o hello instance služby.

| Položka | Popis |
| --- | --- |
| Kořenová složka api management |Obsahuje nejvyšší úrovně konfiguraci pro instance služby hello |
| rozhraní API složky |Obsahuje hello konfiguraci pro rozhraní API hello v instanci služby hello |
| složka skupiny |Obsahuje konfiguraci hello hello skupin v instanci služby hello |
| Složka zásad |Obsahuje hello zásady v instanci služby hello |
| portalStyles složky |Obsahuje konfiguraci hello přizpůsobení portálu hello vývojáře v instanci služby hello |
| produkty složky |Obsahuje konfiguraci hello produktů hello v instanci služby hello |
| složka šablon |Obsahuje konfiguraci hello hello e-mailových šablon v instanci služby hello |

Každé složky může obsahovat jeden nebo více souborů a v některých případech jeden nebo více složek, například do složky pro každé rozhraní API, produkty nebo skupiny. Hello souborů v rámci každé složky jsou specifické pro typ entity hello popsaného hello název složky.

| Typ souboru | Účel |
| --- | --- |
| JSON |Informace o konfiguraci o hello odpovídající entity |
| HTML |Popisy o hello entity, často zobrazeny v portálu pro vývojáře hello |
| xml |Příkazy zásad |
| šablon stylů CSS |Šablony stylů pro přizpůsobení portálu pro vývojáře |

Tyto soubory lze vytvořit, odstranit, upravit a spravovat v místním systému souborů a změny hello nasazené back toohello instanci služby API Management.

> [!NOTE]
> Hello následující entity nejsou obsaženy v úložišti Git hello a nelze konfigurovat pomocí Git.
> 
> * Uživatelé
> * Předplatná
> * Vlastnosti
> * Vývojáře portálu entity než styly
> 
> 

### <a name="root-api-management-folder"></a>Kořenová složka api management
kořenové Hello `api-management` složka obsahuje `configuration.json` soubor, který obsahuje nejvyšší úrovně informace o instanci služby hello v hello formátu.

```json
{
  "settings": {
    "RegistrationEnabled": "True",
    "UserRegistrationTerms": null,
    "UserRegistrationTermsEnabled": "False",
    "UserRegistrationTermsConsentRequired": "False",
    "DelegationEnabled": "False",
    "DelegationUrl": "",
    "DelegatedSubscriptionEnabled": "False",
    "DelegationValidationKey": ""
  },
  "$ref-policy": "api-management/policies/global.xml"
}
```

první čtyři nastavení Hello (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, a `UserRegistrationTermsConsentRequired`) mapování toohello následující nastavení na hello **identity** ve hello **zabezpečení** části.

| Nastavení identity | Mapuje příliš|
| --- | --- |
| RegistrationEnabled |**Přesměrování toosign stránku anonymní uživatelé** zaškrtávací políčko |
| UserRegistrationTerms |**Podmínky použití na registraci uživatele** textbox |
| UserRegistrationTermsEnabled |**Zobrazit podmínky použití na přihlašovací stránce** zaškrtávací políčko |
| UserRegistrationTermsConsentRequired |**Vyžadovat souhlas** zaškrtávací políčko |

![Nastavení identity][api-management-identity-settings]

Hello další čtyři nastavení (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, a `DelegationValidationKey`) mapování toohello následující nastavení na hello **delegování** ve hello **zabezpečení** části.

| Nastavení delegování | Mapuje příliš|
| --- | --- |
| DelegationEnabled |**Delegát přihlášení a registrace** zaškrtávací políčko |
| DelegationUrl |**Adresa URL koncového bodu delegování** textbox |
| DelegatedSubscriptionEnabled |**Delegovat odběru produktů** zaškrtávací políčko |
| DelegationValidationKey |**Delegovat ověřovací klíč** textbox |

![Nastavení delegování][api-management-delegation-settings]

Hello poslední nastavení `$ref-policy`, mapuje toohello globální zásady příkazy soubor pro instance služby hello.

### <a name="apis-folder"></a>rozhraní API složky
Hello `apis` složka obsahuje složku pro každé rozhraní API v instanci služby hello, který obsahuje hello následující položky.

* `apis\<api name>\configuration.json`-Toto je konfigurace hello hello rozhraní API a obsahuje informace o hello operace a adresa URL služby back-end hello. Toto je hello stejné informace, které by byla vrácena, pokud byste byli toocall [získat konkrétní rozhraní API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) s `export=true` v `application/json` formátu.
* `apis\<api name>\api.description.html`-Toto je popis hello hello rozhraní API a odpovídá toohello `description` vlastnost hello [rozhraní API entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).
* `apis\<api name>\operations\`– Tato složka obsahuje `<operation name>.description.html` soubory, které mapují toohello operace v hello rozhraní API. Každý soubor obsahuje popis hello jedné operace v hello rozhraní API, která se mapuje toohello `description` vlastnost hello [operaci entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) v hello REST API.

### <a name="groups-folder"></a>složka skupiny
Hello `groups` složka obsahuje složku pro každou skupinu definované v instanci služby hello.

* `groups\<group name>\configuration.json`-Toto je hello konfigurace pro skupinu hello. Toto je hello stejné informace, které by byla vrácena, pokud byste byli toocall hello [získat určité skupiny](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) operaci.
* `groups\<group name>\description.html`-Toto je popis hello hello skupiny a odpovídá toohello `description` vlastnost hello [skupiny entity](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).

### <a name="policies-folder"></a>Složka zásad
Hello `policies` složka obsahuje příkazy hello zásad pro instanci služby.

* `policies\global.xml`-obsahuje zásady definované v globálním oboru pro instanci služby.
* `policies\apis\<api name>\`– Pokud máte jakékoli zásady definované v oboru rozhraní API, jsou obsažené v této složce.
* `policies\apis\<api name>\<operation name>\`Složka – Pokud máte jakékoli zásady definované v oboru operaci, jsou obsažené v této složce v `<operation name>.xml` soubory, které mapují toohello příkazy zásad pro každou operaci.
* `policies\products\`– Pokud máte jakékoli zásady definované v produktu oboru, jsou obsažené v této složce, která obsahuje `<product name>.xml` soubory, které mapují toohello příkazy zásad pro jednotlivé produkty.

### <a name="portalstyles-folder"></a>portalStyles složky
Hello `portalStyles` složka obsahuje konfiguraci a stylu stylů pro vývojáře přizpůsobení portálu pro instance služby hello.

* `portalStyles\configuration.json`-obsahuje názvy hello používá portál pro vývojáře hello hello šablony stylů
* `portalStyles\<style name>.css`-Každý `<style name>.css` soubor obsahuje styly pro portál pro vývojáře hello (`Preview.css` a `Production.css` ve výchozím nastavení).

### <a name="products-folder"></a>produkty složky
Hello `products` složka obsahuje složku pro každý produkt definované v instanci služby hello.

* `products\<product name>\configuration.json`-Toto je konfigurace hello hello produktu. Toto je hello stejné informace, které by byla vrácena, pokud byste byli toocall hello [získat určitý produkt](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) operaci.
* `products\<product name>\product.description.html`-Toto je popis hello hello produktu a odpovídá toohello `description` vlastnost hello [entity produktu](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) v hello REST API.

### <a name="templates"></a>šablony
Hello `templates` složka obsahuje konfiguraci pro hello [e-mailových šablon](api-management-howto-configure-notifications.md) hello instance služby.

* `<template name>\configuration.json`-Toto je konfigurace hello hello e-mailové šablony.
* `<template name>\body.html`-Toto je hello text šablony e-mailu hello.

## <a name="next-steps"></a>Další kroky
Informace o dalších způsobů toomanage instanci služby, najdete v části:

* Spravovat instanci služby pomocí následující rutiny prostředí PowerShell hello
  * [Referenční informace k rutinám PowerShellu pro nasazení služeb](https://msdn.microsoft.com/library/azure/mt619282.aspx)
  * [Správa služeb referenční informace o rutinách prostředí PowerShell](https://msdn.microsoft.com/library/azure/mt613507.aspx)
* Spravovat instanci služby v portálu vydavatele hello
  * [Správa vašeho prvního rozhraní API](api-management-get-started.md)
* Spravovat instanci služby pomocí rozhraní REST API hello
  * [Referenční dokumentace rozhraní API REST API Management](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a>Podívejte se na video s přehledem
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Configuration-over-Git/player]
> 
> 

[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png




