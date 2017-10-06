---
title: "aaaAzure služby Active Directory na základě skupin licencí další scénáře | Microsoft Docs"
description: "Další scénáře pro Azure Active Directory na základě skupiny licencí"
services: active-directory
keywords: "Licencování Azure AD"
documentationcenter: 
author: curtand
manager: femila
editor: piotrci
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/02/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 782b7c9aa1c062a55c1241d69af673466f7a849c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scenarios-limitations-and-known-issues-using-groups-toomanage-licensing-in-azure-active-directory"></a>Scénáře, omezení a známé problémy pomocí skupin toomanage licencování v Azure Active Directory

Použijte následující informace a příklady toogain rozsáhlejšími znalostmi Azure Active Directory (Azure AD) na základě skupiny licencování hello.

## <a name="usage-location"></a>Umístění využití

Některé služby společnosti Microsoft nejsou k dispozici ve všech umístěních. Před tooa uživatele lze přiřadit licenci, má správce hello toospecify hello **umístění využití** vlastnost hello uživatele. V [hello portál Azure](https://portal.azure.com), můžete zadat v **uživatele** &gt; **profil** &gt; **nastavení**.

Pro přiřazení skupiny licencí zdědí všechny uživatele bez zadané umístění využití hello umístění adresáře hello. Pokud máte uživatele na více místech, ujistěte se, že tooreflect, správně v uživatelské objekty před přidáním toogroups uživatelé s licencí.

> [!NOTE]
> Přiřazení licencí skupiny nikdy upraví stávající hodnotu umístění využití určitého uživatele. Doporučujeme vždy nastavit umístění využití v rámci vaší toku vytvoření uživatele ve službě Azure AD (např. přes AAD Connect konfigurace) –, který zajistí hello výsledek přiřazení licence je vždy správný, a uživatelé neobdrží v umístění, které nejsou služby povolené.

## <a name="use-group-based-licensing-with-dynamic-groups"></a>Správa na základě skupiny licencí s dynamickými skupinami

Na základě skupiny licencování s žádnou skupinu zabezpečení, což znamená, že ho mohou být kombinovány s dynamických skupin Azure AD, můžete použít. Dynamické skupiny pravidel spouštění tooautomatically atributy objektu uživatele přidávat a odebírat uživatele ze skupin.

Například můžete vytvořit dynamické skupiny pro některé sadu produktů chcete tooassign toousers. Každý skupina se zaplní pravidlem přidávání uživatelů podle jejich atributů a každá skupina je hello přiřazené licence, zda je chcete tooreceive. Můžete přiřadit hello atribut místně a synchronizovat s Azure AD, nebo je můžete spravovat hello atribut přímo v cloudu hello.

Licence jsou přiřazeny toohello uživatele krátce po jejich přidání toohello skupiny. Při změně atributu hello hello uživatel opustí hello skupin a jsou odebrány licence hello.

### <a name="example"></a>Příklad

Podívejte se na příklad hello řešení správy identity místně, rozhodne, které uživatelé by měli mít přístup tooMicrosoft webové služby. Používá **extensionAttribute1** toostore řetězec hodnotu představující hello licence by měl mít uživatel hello. Azure AD Connect synchronizuje se službou Azure AD.

Uživatelé mohou potřebovat licence ale není jiný, nebo může být nutné obě. Tady je příklad, ve kterém jsou distribuci Office 365 Enterprise E5 a Enterprise Mobility + Security (EMS) licence toousers ve skupinách:

#### <a name="office-365-enterprise-e5-base-services"></a>Office 365 Enterprise E5: základní služby

![Snímek obrazovky z Office 365 Enterprise E5 základní služby](media/active-directory-licensing-group-advanced/o365-e5-base-services.png)

#### <a name="enterprise-mobility--security-licensed-users"></a>Enterprise Mobility + Security: licencovaní uživatelé

![Snímek obrazovky Enterprise Mobility + Security licencované uživatele](media/active-directory-licensing-group-advanced/o365-e5-licensed-users.png)

V tomto příkladu úprava jednoho uživatele a nastavte jejich hodnotu toohello extensionAttribute1 `EMS;E5_baseservices;` Pokud chcete hello uživatele toohave obou licence. Tato úprava můžete provést místní. Po hello změnit synchronizacemi hello cloudu, hello je automaticky přidán tooboth skupin a přiřazené licence.

![Snímek obrazovky ukazující, jak tooset hello extensionAttribute1 uživatele](media/active-directory-licensing-group-advanced/user-set-extensionAttribute1.png)

> [!WARNING]
> Buďte opatrní při úpravě pravidla členství ve stávající skupině. Pokud pravidlo mění, hello členství ve skupině hello bude znovu zhodnotí a uživatelé, kteří už neodpovídá hello nové pravidel se odeberou (uživatelů, kteří stále odpovídat hello nové pravidlo nebude mít vliv během tohoto procesu). Tito uživatelé budou mít jejich licencí během hello proces, který může mít za následek ztrátu služby, nebo v některých případech ke ztrátě dat odebrány.

> Pokud máte velké dynamická skupina, které závisí na pro přiřazení licence, vezměte v úvahu ověřování žádnými většími změnami v testovací skupině, menší než je použijete toohello hlavní skupiny.

## <a name="multiple-groups-and-multiple-licenses"></a>Více skupin a více licencí

Uživatel může být členem více skupin s licencí. Zde jsou některé tooconsider věcí:

- Více licencí pro hello stejné produktu může dojít k překrytí a mít za následek povoleny všechny služeb, které jsou použité toohello uživatele. Hello následující příklad ukazuje dva licencování skupin: *základní služby E3* obsahuje hello foundation services toodeploy nejdřív tooall uživatele. A *E3 rozšířené služby* obsahuje další služby (boční výkyvy a Planner) toodeploy pouze toosome uživatele. V tomto příkladu byl přidán uživatel hello tooboth skupiny:

  ![Snímek obrazovky povolené služby](media/active-directory-licensing-group-advanced/view-enabled-services.png)

  V důsledku toho uživatel hello má 7 hello 12 služeb v produktu hello povolené, při použití pouze jednu licenci tohoto produktu.

- Výběr hello *E3* licence ukazuje další podrobnosti, včetně informací o tom, které skupiny způsobila jaké služby toobe pro hello uživatele povoleno.

  ![Snímek obrazovky povolené služby podle skupiny](media/active-directory-licensing-group-advanced/view-enabled-service-by-group.png)

## <a name="direct-licenses-coexist-with-group-licenses"></a>Přímé licence souběžnou existenci s skupiny licencí

Když uživatel dědí licenci ze skupiny, nedá přímo odebrat ani změnit tohoto přiřazení licence ve vlastnostech hello uživatele. Změny ve skupině hello musí být provedeny a pak se rozšíří tooall uživatele.

Je možné, ale tooassign hello stejné licenci produktu přímo toohello uživatele v přidání toohello zděděná licence. Další služby z produktu hello jen pro jednoho uživatele, můžete povolit bez ovlivnění jiných uživatelů.

Přímo přiřazené licence lze odebrat a nemají vliv zděděná licence. Vezměte v úvahu hello uživatele, který dědí ze skupiny licenci pro Office 365 Enterprise E3.

1. Původně hello uživatele dědí hello licencí pouze z hello *základní služby E3* skupinu, která umožňuje čtyři plány služby, jak je znázorněno:

  ![Snímek obrazovky E3 skupiny povolené služby](media/active-directory-licensing-group-advanced/e3-group-enabled-services.png)

2. Můžete vybrat **přiřadit** toodirectly přiřadit uživatele toohello licence E3. V takovém případě budete toodisable plány všechny služby s výjimkou Yammer Enterprise:

  ![Snímek obrazovky jak tooassign licenci přímo tooa uživatele](media/active-directory-licensing-group-advanced/assign-license-to-user.png)

3. V důsledku toho uživatel hello pořád používají pouze jednu licenci produktu hello E3. Ale umožňuje přímé přiřazení hello hello Služba Yammer Enterprise pouze pro tohoto uživatele. Můžete zjistit, které služby jsou povolené ve členství ve skupině hello versus hello přímé přiřazení:

  ![Snímek obrazovky zděděná versus přímé přiřazení](media/active-directory-licensing-group-advanced/direct-vs-inherited-assignment.png)

4. Pokud používáte přímé přiřazení, jsou povoleny hello následující operace:

  - Yammer Enterprise můžete vypnout v objektu user hello přímo. Hello **zapnutí nebo vypnutí** přepnutí hello obrázku byla povolená pro tuto službu, jako toohello názvem na rozdíl od jiných služeb přepínačů. Protože je povolena služba hello přímo na hello uživatele, můžete změnit.
  - Jako součást hello přímo přiřazena licence můžete taky povolit další služby.
  - Hello **odebrat** tlačítko lze použít tooremove hello přímé licenci hello uživatele. Uvidíte, že hello uživatel nyní má pouze hello zděděné skupinu licencí a pouze původní služby hello zůstat zapnuté:

    ![Snímek obrazovky ukazující, jak tooremove přímé přiřazení](media/active-directory-licensing-group-advanced/remove-direct-license.png)

## <a name="managing-new-services-added-tooproducts"></a>Správa nové služby přidat tooproducts
Když Microsoft přidá nového produktu tooa služby, bude povoleno ve výchozím nastavení všechny skupiny toowhich přiřadili jste licence produktu hello. Uživatelé ve vašem klientovi, kteří jsou odebírané toonotifications o změnách produktu bude dostávat e-maily s předstihem jim informace o přidání nadcházející služby hello.

Jako správce můžete revidovat všechny skupiny hello změnou ovlivněná a provést akci, například zakázání hello novou službu v každé skupině. Například pokud jste vytvořili skupiny cílení pouze konkrétní služby pro nasazení, můžete pokroku těchto skupin a ujistěte se, že všechny nově přidaní služby jsou zakázány.

Tady je příklad, jak může vypadat tento proces:

1. Původně, jste přiřadili hello *Office 365 Enterprise E5* skupiny tooseveral produktu. Jeden z těchto skupin nazývaných *O365 E5 - Exchange pouze* byla pouze hello navrženou tooenable *Exchange Online (plán 2)* služby pro její členy.

2. Obdržela oznámení o společnosti Microsoft, která hello E5 produktu bude rozšířeno o novou službu - *Microsoft Stream*. Jakmile hello služby k dispozici ve vašem klientovi, můžete provést následující hello:

3. Přejděte toohello [ **Azure Active Directory > licence > všechny produkty** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products) a vyberte *Office 365 Enterprise E5*, pak vyberte **licenci skupiny**  tooview seznam všechny skupiny s tohoto produktu.

4. Klikněte na skupinu hello chcete tooreview (v tomto případě *O365 E5 - Exchange pouze*). Otevře se hello **licence** kartě. Kliknutím na licenční E5 hello se otevře okno výpis všech služeb povoleno.
> [!NOTE]
> Hello *Microsoft Stream* služby je automaticky přidán a povoleno v této skupině v přidání toohello *Exchange Online* služby:

  ![Snímek obrazovky nové služby přidat tooa skupiny licencí](media/active-directory-licensing-group-advanced/manage-new-services.png)

5. Pokud chcete toodisable hello nové služby v této skupině, klikněte na tlačítko hello **zapnutí nebo vypnutí** přepnutí další toohello služby a klikněte na tlačítko hello **Uložit** tlačítko tooconfirm hello změnu. Azure AD bude nyní zpracovat všichni uživatelé v hello skupiny tooapply hello změnu; všechny nové uživatele přidaných toohello skupiny nebude mít hello *Microsoft Stream* služby povolena.

  > [!NOTE]
  > Uživatelé mohou mít i nadále hello služby pomocí některé jiné přiřazení licence (jiné skupiny jsou členy nebo přiřazení přímé licence) povolena.

6. V případě potřeby proveďte hello přiřazen stejný postup pro jiné skupiny s tohoto produktu.

## <a name="use-powershell-toosee-who-has-inherited-and-direct-licenses"></a>Použijte toosee prostředí PowerShell, který dědí a přímé licencí
Toocheck skript prostředí PowerShell můžete použít, pokud uživatelé používají licence přiřadit přímo nebo zděděno od skupiny.

1. Spustit hello `connect-msolservice` tooauthenticate rutiny a připojte tooyour klienta.

2. `Get-MsolAccountSku`lze použít toodiscover všechny licence zřízené produktu v klientovi hello.

  ![Snímek obrazovky rutiny Get-Msolaccountsku hello](media/active-directory-licensing-group-advanced/get-msolaccountsku-cmdlet.png)

3. Použití hello *AccountSkuId* hodnotu pro vás zajímá s licencí hello [tento skript prostředí PowerShell](./active-directory-licensing-ps-examples.md#check-if-user-license-is-assigned-directly-or-inherited-from-a-group). Vznikne tak seznam uživatelů, kteří mají tuto licenci s hello informace o tom, jak hello licence se nepřiřadí.

## <a name="use-audit-logs-toomonitor-group-based-licensing-activity"></a>Protokoly auditu použití toomonitor na základě skupiny licencování aktivity

Můžete použít [protokoly auditu Azure AD](./active-directory-reporting-activity-audit-logs.md#audit-logs) toosee veškeré aktivity související na základě toogroup licencování, včetně:
- kdo změnil licencí na skupiny
- Při spuštění systému hello zpracování změnu skupinu licencí a po jeho dokončení
- jaké změny licencí byly provedeny tooa uživatele v důsledku přiřazení licencí skupiny.

>[!NOTE]
> Protokoly auditu jsou k dispozici na většině oken v Azure Active Directory části portálu hello hello. V závislosti na tom, odkud je může být filtry předem použité tooonly zobrazit aktivitu relevantní toohello kontextu okna hello. Pokud nevidíte hello výsledky očekáváte, prozkoumejte [hello možnosti filtrování](./active-directory-reporting-activity-audit-logs.md#filtering-audit-logs) nebo přístup protokoly auditu hello nefiltrovaná pod [ **Azure Active Directory > Aktivity > protokoly auditu** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Audit).

### <a name="find-out-who-modified-a-group-license"></a>Zjistěte, kdo upravit skupinu licencí

1. Sada hello **aktivity** filtrovat příliš*sady skupiny licencí* a klikněte na tlačítko **použít**.
2. Hello výsledky obsahovat všech případech se nastavit nebo změnit na skupiny licencí.
>[!TIP]
> Můžete také zadat název hello hello skupiny v hello *cíl* filtrování výsledků tooscope hello.

3. Klikněte na položky v seznamu zobrazit hello toosee-podrobnosti o hello co se změnilo. V části *upravit vlastnosti* staré a nové hodnoty pro přiřazení licence hello jsou uvedené.

Tady je příklad nedávné změny skupiny licencí, s podrobnostmi:

![Změny licencí skupiny – snímek obrazovky](media/active-directory-licensing-group-advanced/audit-group-license-change.png)

### <a name="find-out-when-group-changes-started-and-finished-processing"></a>Zjistit, kdy změny skupiny spuštění a dokončení zpracování

Pokud licence se změní na skupinu, Azure AD se spustí, použití hello změny tooall uživatele.

1. toosee při spuštění skupiny zpracování, nastavte hello **aktivity** filtrovat příliš*spustit použití skupinu na základě licencí toousers*. Všimněte si, že hello objektu actor pro operaci hello je *Microsoft Azure AD na základě skupiny Licensing* -systému účtu, který je použité tooexecute všechny změny skupiny licencí.
>[!TIP]
> Klikněte na položku v hello seznamu toosee hello *upravit vlastnosti* pole - zobrazuje změny hello licencí, které byly zachyceny pro zpracování. To je užitečné, pokud jste provedli několik změny tooa skupiny a si nejste jisti, která byla zpracována.

2. Podobně toosee po dokončení skupiny zpracování, hodnota filtru hello použití *dokončit použití skupinu na základě licencí toousers*.
>[!TIP]
> V takovém případě hello *upravit vlastnosti* pole obsahuje souhrn výsledků hello – to je užitečné tooquickly kontrolu v případě zpracování výsledkem případné chyby. Ukázkový výstup:
> ```
Modified Properties
...
Name : Result
Old Value : []
New Value : [Users successfully assigned licenses: 6, Users for whom license assignment failed: 0.];
> ```

3. dokončení protokolu toosee hello jak byla zpracována skupiny, včetně změny všech uživatelů, nastavit hello následující filtry:
  - **Zahájit (objektu Actor)**: "Microsoft Azure AD na základě skupin licencí."
  - **Rozsah** (volitelné): Pokud znáte konkrétní skupinu vlastní rozsah spuštění a dokončení zpracování

Tento ukázkový výstup zobrazuje hello začátek zpracování, všechny výsledné změny uživatelů a hello dokončit zpracování.

![Změny licencí skupiny – snímek obrazovky](media/active-directory-licensing-group-advanced/audit-group-processing-log.png)

>[!TIP]
> Kliknutím na položky související s příliš*uživatelské licence pro změnu* zobrazí podrobnosti pro licenční změny tooeach jednotlivé uživatele.

## <a name="limitations-and-known-issues"></a>Omezení a známé problémy

Pokud chcete použít, na základě skupiny licencí, je vhodné toofamiliarize sami s hello následující seznam omezení a známé problémy.

- Na základě skupiny licencování aktuálně nepodporuje skupiny, které obsahují další skupiny (vnořené skupiny). Pokud použijete tooa vnořené skupiny licencí, mají pouze členové okamžitou první úrovně uživatele hello hello skupiny licencí hello použít.

- Funkce Hello lze použít pouze se skupinami zabezpečení. Aktuálně nejsou podporované skupiny Office a nebude ji již možné toouse je v procesu přiřazení licence hello.

- Hello [portálu pro správu Office 365](https://portal.office.com ) současné době nepodporuje na základě skupin licencí. Pokud uživatel dědí licenci ze skupiny, zobrazí tuto licenci v portálu pro správu Office hello jako běžných uživatelských licencí. Pokud se pokusíte toomodify, který licence nebo zkuste tooremove hello licence, portál hello vrátí chybovou zprávu. Licence zděděné skupiny nemůže být upraven přímo na uživatele.

- Pokud uživatele se odebere ze skupiny a ztratí hello licence, plány služby hello z tohoto licence (například služby SharePoint Online) se nastaví tooa **pozastaveno** stavu. Hello plány služeb nejsou nastaveny tooa konečné, zakázané stavu. Toto opatření se můžete vyhnout náhodného odstranění dat uživatele, pokud správce zadá chybu ve správě členství skupiny.

- Když jsou licence přiřadit nebo změnit pro velké skupinu (například 100 000 uživatelů), může ovlivnit výkon. Konkrétně hello objemu změn generované automatizace Azure AD může mít negativní vliv hello výkon vaší synchronizace adresářů mezi službami Azure AD a místních systémech.

- Automatizace správy licencí tooall typů změn v prostředí hello automaticky nereaguje. Například můžete pravděpodobně vyčerpala volné licence, způsobuje toobe někteří uživatelé v chybovém stavu. toofree až počet hello k dispozici stanici, můžete odebrat některé přímo přiřazené licence od jiných uživatelů. Hello systému však automaticky reagovat toothis změn a opravte uživatelé v tomto stavu chyby.

  Jako alternativní řešení toothese, druhy omezení můžete přejít toohello **skupiny** okno ve službě Azure AD a klikněte na tlačítko **znovu zpracovat**. Tento příkaz zpracovává všechny uživatele v dané skupině a přeloží hello chybové stavy, pokud je to možné.

- Na základě skupiny licencování nezaznamenává chyby, pokud nebylo možné přiřadit licenci tooa uživatele kvůli konfiguraci adresy tooa duplicitní proxy serveru v systému Exchange Online; tyto uživatele jsou přeskočeny při přiřazení licence. Další informace o tom, tooidentify a tento problém vyřešit, najdete v části [v této části](./active-directory-licensing-group-problem-resolution-azure-portal.md#license-assignment-fails-silently-for-a-user-due-to-duplicate-proxy-addresses-in-exchange-online).

## <a name="next-steps"></a>Další kroky

toolearn Další informace o scénáře pro správu licencí prostřednictvím na základě skupiny licencí, najdete v části:

* [Co je na základě skupin licencování v Azure Active Directory?](active-directory-licensing-whatis-azure-portal.md)
* [Přiřazení licencí tooa skupiny ve službě Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Identifikace a řešení potíží s licencí pro skupinu v Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Jak toomigrate jednotlivé licencované uživatele toogroup základě licencování v Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
