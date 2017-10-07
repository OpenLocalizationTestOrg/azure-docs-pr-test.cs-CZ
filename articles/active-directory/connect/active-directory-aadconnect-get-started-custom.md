---
title: "Azure AD Connect: Vlastní instalace | Dokumentace Microsoftu"
description: "Tento dokument podrobně popisuje možnosti vlastní instalace Azure AD Connect hello. Použijte tyto pokyny tooinstall služby Active Directory přes Azure AD Connect."
services: active-directory
keywords: "co je Azure AD Connect, instalace služby Active Directory, požadované součásti služby Azure AD"
documentationcenter: 
author: billmath
manager: femila
ms.assetid: 6d42fb79-d9cf-48da-8445-f482c4c536af
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/02/2017
ms.author: billmath
ms.openlocfilehash: c49724fab78c5a1688662043f69506fff6f82104
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="custom-installation-of-azure-ad-connect"></a>Vlastní instalace služby Azure AD Connect
Azure AD Connect **vlastní nastavení** se používá, pokud chcete využít další možnosti pro instalaci hello. Pokud máte více doménových struktur, nebo pokud chcete tooconfigure volitelné funkce nejsou zahrnuté v hello rychlé instalace se používá. Se používá ve všech případech, kde hello [ **Expresní instalace** ](active-directory-aadconnect-get-started-express.md) nevyhovuje nasazení nebo topologii.

Před zahájením instalace Azure AD Connect, ujistěte se, příliš[stáhnout Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) a dokončení hello nezbytné kroky [Azure AD Connect: Hardware a nezbytné předpoklady](active-directory-aadconnect-prerequisites.md). Taky se ujistěte, jestli máte požadované účty, které jsou popsané v tématu [Účty a oprávnění Azure AD Connect](active-directory-aadconnect-accounts-permissions.md).

Pokud vlastní nastavení neodpovídá vaší topologii, třeba tooupgrade DirSync, přečtěte si téma [související dokumentaci](#related-documentation) s dalšími scénáři.

## <a name="custom-settings-installation-of-azure-ad-connect"></a>Instalace Azure AD Connect s vlastním nastavením
### <a name="express-settings"></a>Expresní nastavení
Na této stránce, klikněte na tlačítko **přizpůsobit** toostart instalaci s vlastním nastavením.

### <a name="install-required-components"></a>Instalace požadovaných součástí
Při instalaci služeb synchronizace hello hello volitelné konfigurační oddíl můžete nechat nezaškrtnuté a Azure AD Connect nastaví všechno automaticky. Nastavuje instance SQL Server 2012 Express LocalDB, vytvořte hello příslušných skupin a přiřaďte oprávnění. Pokud chcete výchozí hello toochange, můžete použít následující tabulky toounderstand hello volitelné konfiguraci možnosti, které jsou k dispozici hello.

![Požadované součásti](./media/active-directory-aadconnect-get-started-custom/requiredcomponents.png)

| Volitelná konfigurace | Popis |
| --- | --- |
| Použít existující server SQL Server |Můžete se název serveru SQL Server hello toospecify a název instance hello. Tuto možnost zvolte, pokud už máte databázový server, které chcete toouse. Zadejte název instance hello následuje čárka a port číslo v **název Instance** Pokud SQL Server nemá povoleno procházení. |
| Použít existující účet služby |Ve výchozím nastavení používá Azure AD Connect účet virtuální služby pro toouse služby synchronizace hello. Pokud používáte vzdálený SQL server nebo použít proxy server, který vyžaduje ověření, je třeba toouse **účet spravované služby** nebo použijte účet služby v doméně hello a znát heslo hello. V těchto případech zadejte účet toouse hello. Zajistěte, aby hello uživatel, který spouští instalaci hello je SA v SQL, takže je možné vytvořit přihlašovací údaje pro účet služby hello. Viz téma [Účty a oprávnění Azure AD Connect](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account). |
| Zadat vlastní skupiny pro synchronizaci |Ve výchozím nastavení vytvoří Azure AD Connect při instalaci služeb synchronizace hello čtyři skupiny toohello místní server. Tyto skupiny jsou: skupiny Administrators, skupina Operators, skupina Browse a skupina Password Reset hello. Tady můžete zadat vlastní skupiny. Hello skupiny musí být místní na serveru hello a nemůže nacházet v doméně hello. |

### <a name="user-sign-in"></a>Přihlášení uživatele
Po instalaci hello požadované součásti, se zobrazí výzva tooselect vaši uživatelé jediná metoda přihlašování. Hello následující tabulka obsahuje stručný popis dostupných možností hello. Úplný popis metod hello přihlášení, naleznete v části [přihlášení uživatele](active-directory-aadconnect-user-signin.md).

![Přihlášení uživatele](./media/active-directory-aadconnect-get-started-custom/usersignin2.png)

| Možnost jednotného přihlašování | Popis |
| --- | --- |
| Synchronizace hesla |Uživatelé jsou možné toosign v tooMicrosoft cloudové služby, např. Office 365 pomocí hello stejné heslo, které se používají ve svojí místní síti. hesla uživatelů Hello jsou synchronizované tooAzure AD jako hodnoty hash hesla a v cloudu hello dochází k ověřování. Další informace najdete v tématu [Synchronizace hesel](active-directory-aadconnectsync-implement-password-synchronization.md). |
|Předávací ověřování (Preview)|Uživatelé jsou možné toosign v tooMicrosoft cloudové služby, např. Office 365 pomocí hello stejné heslo, které se používají ve svojí místní síti.  heslo uživatele Hello předána toohello místní služby Active Directory řadiče toobe ověřit.
| Federace se službou AD FS |Uživatelé jsou možné toosign v tooMicrosoft cloudové služby, např. Office 365 pomocí hello stejné heslo, které se používají ve svojí místní síti.  Hello uživatelé přesměrovaní tootheir místní toosign instance služby AD FS v a ověření probíhá místně. |
| Nekonfigurovat |Ani jedna z funkcí není nainstalovaná a nakonfigurovaná. Tuto možnost zvolte, pokud už využíváte federační server třetí strany nebo jiné existující řešení. |
|Povolit jednotné přihlašování|Tahle možnost je dostupná se synchronizací hesla i předávací ověřování a poskytuje jednotné přihlašování v prostředí plochy uživatelům v podnikové síti hello.  Další informace najdete v tématu [Jednotné přihlašování](active-directory-aadconnect-sso.md). </br>Poznámka pro zákazníky služby AD FS tato možnost není k dispozici vzhledem k tomu, že služba AD FS už nabízí hello stejnou úroveň jednotné přihlašování.</br>(Pokud není k dispozici PTA ve hello současně)
|Možnost přihlašování|Tahle možnost je dostupné pro zákazníky, synchronizaci hesel a poskytuje jednotné přihlašování v prostředí plochy uživatelům v podnikové síti hello.  </br>Další informace najdete v tématu [Jednotné přihlašování](active-directory-aadconnect-sso.md). </br>Poznámka pro zákazníky služby AD FS tato možnost není k dispozici vzhledem k tomu, že služba AD FS už nabízí hello stejnou úroveň jednotné přihlašování.


### <a name="connect-tooazure-ad"></a>Připojit tooAzure AD
Na úvodní obrazovka tooAzure AD Connect zadejte účet globálního správce a heslo. Pokud jste vybrali **federační službou AD FS** na předchozí stránku hello, nepřihlašujte se pomocí účtu v doméně plánujete tooenable pro federaci. Doporučuje toouse účet ve výchozí hello **onmicrosoft.com** doménu, která se dodává s adresáře Azure AD.

Tento účet je pouze použité toocreate účet služby ve službě Azure AD a nepoužívá po dokončení Průvodce hello.  
![Přihlášení uživatele](./media/active-directory-aadconnect-get-started-custom/connectaad.png)

Pokud má váš účet globálního správce povolené ověřování MFA, musíte heslo hello tooprovide do hello přihlašovací v automaticky otevřeném okně a dokončení hello ověřovací test MFA. Hello výzvy může být zadání ověřovacího kódu nebo telefonní hovor.  
![Přihlášení uživatele s MFA](./media/active-directory-aadconnect-get-started-custom/connectaadmfa.png)

Účet globálního správce Hello může mít [Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md) povolena.

Pokud se zobrazí chyba a máte problémy s připojením, přečtěte si téma [Řešení problémů s připojením](active-directory-aadconnect-troubleshoot-connectivity.md).

## <a name="pages-under-hello-section-sync"></a>Stránky části hello synchronizace

### <a name="connect-your-directories"></a>Připojení adresářů
tooconnect tooyour Active Directory Domain Services, Azure AD Connect je potřeba hello název doménové struktury a přihlašovací údaje účtu s dostatečnými oprávněními.

![Připojení adresáře](./media/active-directory-aadconnect-get-started-custom/connectdir01.png)

Po zadání názvu doménové struktury hello a kliknutím na **přidat adresář**, zobrazí se dialogové okno automaticky otevírané okno se zobrazí a zobrazí výzvu s hello následující možnosti:

| Možnost | Popis |
| --- | --- |
| Použít existující účet | Tuto možnost vyberte, pokud chcete, aby účet tooprovide existující služby AD DS, toobe použité pro připojování doménové struktury toohello AD během synchronizace adresářů Azure AD Connect. Můžete zadat část hello domény ve formátu NetBios nebo plně kvalifikovaný název domény, který je FABRIKAM\syncuser nebo fabrikam.com\syncuser. Tento účet může být běžného uživatelského účtu, protože je nutné pouze oprávnění ke čtení na výchozí hello. Je ale možné, že v závislosti na scénáři budete potřebovat větší oprávnění. Další informace najdete v tématu [Účty a oprávnění v Azure AD Connect](active-directory-aadconnect-accounts-permissions.md#create-the-ad-ds-account). |
| Vytvořit nový účet | Tuto možnost vyberte, pokud chcete, aby Azure AD Connect Průvodce toocreate hello služby AD DS účet vyžaduje Azure AD Connect pro připojení doménové struktury toohello AD během synchronizace adresářů. Pokud je vybraná tato možnost, zadejte hello uživatelské jméno a heslo pro účet správce podnikové sítě. Hello zadaný účet správce podnikové použije přes Azure AD Connect Průvodce toocreate hello vyžaduje účet služby AD DS. Můžete zadat část hello domény ve formátu NetBios nebo plně kvalifikovaný název domény, tj. FABRIKAM\administrator nebo fabrikam.com\administrator. |

![Připojení adresáře](./media/active-directory-aadconnect-get-started-custom/connectdir02.png)


### <a name="azure-ad-sign-in-configuration"></a>Konfigurace přihlášení k Azure AD
Tato stránka vám umožní tooreview hello hlavní název uživatele domény v místní službě AD DS a které byly ověřeny ve službě Azure AD. Tato stránka vám také umožní tooconfigure hello atribut toouse pro hello userPrincipalName.

![Neověřené domény](./media/active-directory-aadconnect-get-started-custom/aadsigninconfig.png)  
Zkontrolujte všechny domény označené jako **Nepřidáno** a **Neověřeno**. Ujistěte se, že domény, které používáte, byly ověřeny v Azure AD. Po ověření domény, klikněte na symbol obnovení hello. Další informace najdete v tématu [přidání a ověření domény hello](../active-directory-add-domain.md)

**UserPrincipalName** -hello atribut userPrincipalName je hello atribut, který uživatelé používají při přihlášení tooAzure AD a Office 365. Hello domény použít také označovaný jako hello přípona UPN, je třeba ověřit ve službě Azure AD před synchronizací uživatelů hello. Společnost Microsoft doporučuje tookeep hello výchozí atribut userPrincipalName. Pokud tento atribut není směrovatelný a nedá se ověřit, pak je možné tooselect jiný atribut. Můžete například vybrat e-mail jako hello atribut, který uchovává hello přihlášení. Použití jiného atributu než userPrincipalName se nazývá **Alternativní ID**. Hodnota atributu alternativní ID Hello musí následovat hello RFC822 standardní. Alternativní ID se dá použít se synchronizací hesla i federací.

>[!NOTE]
> Když povolíte předávací ověřování musí mít alespoň jedna ověřená doména, v pořadí toocontinue prostřednictvím Průvodce hello.

> [!WARNING]
> Použití atributu Alternativní ID není kompatibilní se všemi úlohami Office 365. Další informace najdete v části příliš[konfigurace alternativního přihlašovacího ID](https://technet.microsoft.com/library/dn659436.aspx).
>
>

### <a name="domain-and-ou-filtering"></a>Filtrování domén a organizačních jednotek
Ve výchozím nastavení se synchronizují všechny domény a organizační jednotky. Pokud jsou některé domény nebo organizační jednotky nechcete toosynchronize tooAzure AD, můžete zrušit výběr těchto domén a organizačních jednotek.  
![Filtrování organizačních jednotek domén](./media/active-directory-aadconnect-get-started-custom/domainoufiltering.png)  
Tuto stránku hello Průvodce konfiguruje filtrování založené na doméně a na základě organizační jednotky. Pokud máte v plánu toomake změny, potom v tématu [filtrování podle domén](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) a [filtrování založené na organizační jednotku](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) před provedením těchto změn. Některé organizační jednotky jsou zásadní pro fungování hello a nesmí nezaškrtnuté.

Pokud použijete filtrování podle organizačních jednotek v Azure AD Connect verze starší než 1.1.524.0, nové organizační jednotky přidané později se budou synchronizovat automaticky. Pokud chcete hello chování to nový by neměl být synchronizovány organizační jednotky, pak můžete nakonfigurovat, po dokončení Průvodce hello s [založené na organizační jednotku filtrování](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering). Azure AD Connect verze 1.1.524.0 nebo po můžete určit, zda chcete, aby nové organizační jednotky toobe synchronizovány nebo ne.

Pokud máte v plánu toouse [filtrování podle skupiny](#sync-filtering-based-on-groups), pak zajistěte, aby hello organizační jednotky se skupinou hello je zahrnutý a není filtrovaný pomocí filtrování organizační jednotky. Filtrování podle organizačních jednotek se vyhodnocuje dřív než filtrování podle skupin.

Je také možné, že některé domény nejsou dostupné kvůli omezení toofirewall. Tyto domény nejsou ve výchozím nastavení vybrané a je u nich zobrazené upozornění.  
![Nedostupné domény](./media/active-directory-aadconnect-get-started-custom/unreachable.png)  
Pokud se zobrazí toto upozornění, ujistěte se, že jsou tyto domény skutečně nedostupné a hello upozornění očekávané.

### <a name="uniquely-identifying-your-users"></a>Jednoznačná identifikace uživatelů

#### <a name="select-how-users-should-be-identified-in-your-on-premises-directories"></a>Vyberte způsob, jakým se mají uživatelé identifikovat v místních adresářích
Hello párování napříč doménovými strukturami funkce vám umožní toodefine, jak jsou uživatelé z vaší doménové struktury služby AD DS reprezentováni v Azure AD. Uživatel buď může být reprezentován jenom jednou v rámci všech doménových struktur, nebo může mít kombinaci povolených a zakázaných účtů. Hello uživatele může být také reprezentován jako kontakt v některých doménových strukturách.

![Jedinečná](./media/active-directory-aadconnect-get-started-custom/unique.png)

| Nastavení | Popis |
| --- | --- |
| [Uživatelé jsou reprezentováni jen jednou v rámci všech doménových struktur](active-directory-aadconnect-topologies.md#multiple-forests-single-azure-ad-tenant) |Všichni uživatelé jsou vytvořeni jako jednotlivé objekty v Azure AD. Hello objekty nejsou připojené v úložišti metaverse hello. |
| [Atribut Mail](active-directory-aadconnect-topologies.md#multiple-forests-single-azure-ad-tenant) |Tato možnost spojí uživatele a kontakty, pokud má atribut mail hello hello stejnou hodnotu v různých doménových strukturách. Tuto možnost použijte, pokud byly kontakty vytvořeny pomocí GALSync. Pokud zvolíte tuto možnost, nebudou objekty uživatele, jehož atribut e-mailu nejsou naplněny synchronizované tooAzure AD. |
| Atributy [ObjectSID a msExchangeMasterAccountSID/ msRTCSIP-OriginatorSid](active-directory-aadconnect-topologies.md#multiple-forests-single-azure-ad-tenant) |Tato možnost spojí povoleného uživatele v doménové struktuře účtu se zakázaným uživatelem v doménové struktuře prostředku. V systému Exchange se tato konfigurace označuje jako propojená poštovní schránka. Tuto možnost můžete také použijí, pokud používáte pouze Lync a Exchange není k dispozici v doménové struktuře prostředku hello. |
| Atributy sAMAccountName a MailNickName |Tato možnost spojí u atributů tam, kde je očekávané hello přihlašovací ID pro uživatele hello lze nalézt. |
| Konkrétní atribut |Tato možnost vám umožňuje tooselect vlastní atribut. Pokud zvolíte tuto možnost, nebudou objekty uživatele, jehož atribut (vybrané) nejsou naplněny synchronizované tooAzure AD. **Omezení:** Ujistěte se, že toopick atribut, který se již nachází v úložišti metaverse hello. Pokud vyberete vlastní atribut (není v úložišti metaverse hello), Průvodce hello se nedá dokončit. |

#### <a name="select-how-users-should-be-identified-with-azure-ad---source-anchor"></a>Vyberte, jak se mají uživatelé identifikovat s Azure AD – zdrojové ukotvení
Hello atribut sourceAnchor je atribut, který se nedá změnit během hello životnosti objektu uživatele. Je hello primární klíč propojující místního uživatele hello hello uživatele ve službě Azure AD.

| Nastavení | Popis |
| --- | --- |
| Aby Azure spravovat hello zdrojové ukotvení | Tuto možnost vyberte, pokud chcete, aby pro vás Azure AD toopick hello atribut. Pokud vyberete tuto možnost, použije průvodce Azure AD Connect hello sourceAnchor atribut výběr logiku popsané v části článku [Azure AD Connect: Principy – pomocí msDS-ConsistencyGuid jako sourceAnchor návrh](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor). Hello Průvodce vás upozorní, které atribut bylo vydáno jako atribut zdrojové ukotvení hello po dokončení instalace vlastní. |
| Konkrétní atribut | Tuto možnost vyberte, pokud chcete toospecify existující atribut AD jako atribut sourceAnchor hello. |

Vzhledem k tomu, že atribut hello nelze změnit, je nutné naplánovat toouse dobrý atribut. Jednou z vhodných možností je objectGUID. Tento atribut se změní, pokud hello uživatelský účet přesune mezi doménovými strukturami nebo doménami. V prostředí s více doménovými strukturami, kde přesouváte účty mezi doménovými strukturami musí být jiný atribut použít, například atribut s hello employeeID. Vyhněte se atributům, které se mění, když uživatel uzavře manželství nebo se změní jeho přiřazení. Nelze použít atributy se symbolem @-sign, takže se nedá použít e-mail ani atribut userPrincipalName. atribut Hello je také malá a velká písmena proto při přesunutí objektu mezi doménovými strukturami, ujistěte se, že toopreserve hello velká a malá písmena. Binární atributy se zakódují do formátu Base64, ale ostatní typy atributů zůstávají v nekódovaném stavu. Při federacích a v některých rozhraních Azure AD se tento atribut taky nazývá immutableID. Další informace o hello zdrojové ukotvení najdete v hello [konceptech návrhu](active-directory-aadconnect-design-concepts.md#sourceanchor).

### <a name="sync-filtering-based-on-groups"></a>Filtrování synchronizace podle skupin
Hello funkce filtrování podle skupin umožňuje toosync pouze malou podmnožinu objektů pro pilotní nasazení. toouse tuto funkci, vytvořte skupinu pro tento účel ve vaší místní službě Active Directory. Přidejte uživatele a skupiny, které by měly být synchronizovaná tooAzure AD jako přímé členy. Později můžete přidávat a odebírat uživatele toothis skupiny toomaintain hello seznamu objektů, které by měly být dostupné v Azure AD. Všechny objekty, které chcete toosynchronize musí být přímým členem skupiny hello. Přímými členy musí být uživatelé, skupiny, kontakty a počítače/zařízení. Členství ve vnořené skupině se nepřeloží. Když přidáte skupinu, která je členem jenom samotná skupina hello přidání a ne její členové.

![Filtrování synchronizace](./media/active-directory-aadconnect-get-started-custom/filter2.png)

> [!WARNING]
> Tato funkce je jenom zamýšlený toosupport pilotního nasazení. Nepoužívejte ji v plnohodnotném produkčním nasazení.
>
>

V plnohodnotném produkčním nasazení bude toobe pevný toomaintain jednu skupinu se toosynchronize všechny objekty. Místo toho by měla používat jednu z metod hello v [konfiguraci filtrování](active-directory-aadconnectsync-configure-filtering.md).

### <a name="optional-features"></a>Volitelné funkce
Tato obrazovka umožňuje tooselect hello volitelných funkcí pro konkrétní scénáře.

![Volitelné funkce](./media/active-directory-aadconnect-get-started-custom/optional.png)

> [!WARNING]
> Pokud aktuálně máte DirSync nebo Azure AD Sync aktivní, Neaktivujte žádnou z funkcí zpětného zápisu hello v Azure AD Connect.
>
>

| Volitelné funkce | Popis |
| --- | --- |
| Hybridní nasazení systému Exchange |Hello funkce hybridního nasazení Exchange umožňuje hello poštovní schránky Exchange existovaly jak místně a v Office 365. Azure AD Connect synchronizuje konkrétní sadu [atributů](active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) z Azure AD zpátky do místního adresáře. |
| Veřejné složky e-mailu Exchange | Funkce Hello veřejných složek e-mailu Exchange umožňuje toosynchronize poštovní veřejné složky objekty z vaší místní služby Active Directory tooAzure AD. |
| Filtrování aplikací a atributů Azure AD |Když zapnete filtrování aplikací Azure AD a atributů, hello sadu synchronizovaných atributů můžete přizpůsobit. Tato možnost přidá dva další Průvodce toohello stránky konfigurace. Další informace najdete v tématu [Filtrování aplikací a atributů Azure AD](#azure-ad-app-and-attribute-filtering). |
| Synchronizace hesel |Pokud jste vybrali federační jako řešení přihlašování hello, můžete tuto možnost povolte. Synchronizaci hesel je pak možné použít jako záložní možnost. Další informace najdete v tématu [Synchronizace hesel](active-directory-aadconnectsync-implement-password-synchronization.md). </br></br>Tato možnost je povolená ve výchozím nastavení tooensure podporu pro starší verze klientů a jako záložní možnost, pokud jste vybrali předávací ověřování. Další informace najdete v tématu [Synchronizace hesel](active-directory-aadconnectsync-implement-password-synchronization.md).|
| Zpětný zápis hesla |Když zapnete zpětný zápis hesla, změny hesel vzniklé ve službě Azure AD se nezapisují zpět tooyour místního adresáře. Další informace najdete v tématu [Začínáme se správou hesel](../active-directory-passwords-getting-started.md). |
| Zpětný zápis skupin |Pokud používáte hello **skupiny Office 365** funkci, pak máte tyto skupiny reprezentované ve vaší místní službě Active Directory. Tato možnost je dostupná jenom v případě, že se v místní službě Active Directory nachází Exchange. Další informace najdete v tématu [Zpětný zápis skupin](active-directory-aadconnect-feature-preview.md#group-writeback). |
| Zpětný zápis zařízení |Umožňuje toowriteback objekty zařízení v Azure AD tooyour místní služby Active Directory pro situace podmíněného přístupu. Další informace najdete v tématu [Povolení zpětného zápisu zařízení v Azure AD Connect](active-directory-aadconnect-feature-device-writeback.md). |
| Synchronizace atributů rozšíření adresáře |Když zapnete synchronizaci atributů rozšíření adresáře, určené atributy jsou synchronizovaná tooAzure AD. Další informace najdete v tématu [Rozšíření adresáře](active-directory-aadconnectsync-feature-directory-extensions.md). |

### <a name="azure-ad-app-and-attribute-filtering"></a>Filtrování aplikací a atributů Azure AD
Pokud chcete, aby toolimit, které atributy toosynchronize tooAzure AD, začněte výběrem služeb, kterou používáte. Pokud provedete změny konfigurace na této stránce, má novou službu toobe explicitně vybrat opětovným spuštěním Průvodce instalací hello.

![Volitelné funkce – aplikace](./media/active-directory-aadconnect-get-started-custom/azureadapps2.png)

Podle hello služeb vybraných v předchozím kroku hello, tato stránka zobrazuje všechny atributy, které jsou synchronizované. Tento seznam je kombinací všech synchronizovaných typů objektů. Pokud existují konkrétní atributy potřebujete toonot synchronizovat, můžete zrušit výběr těchto atributů.

![Volitelné funkce – atributy](./media/active-directory-aadconnect-get-started-custom/azureadattributes2.png)

> [!WARNING]
> Odebrání atributů může mít vliv na funkčnost. Osvědčené postupy a doporučení najdete v tématu [synchronizované atributy](active-directory-aadconnectsync-attributes-synchronized.md#attributes-to-synchronize).
>
>

### <a name="directory-extension-attribute-sync"></a>Synchronizace atributů rozšíření adresáře
Schéma hello ve službě Azure AD můžete rozšířit vlastními atributy přidané aplikací vaší organizace, nebo dalšími atributy ve službě Active Directory. toouse tuto funkci, vyberte **synchronizace atributů rozšíření adresáře** na hello **volitelné funkce** stránky. Můžete vybrat další atributy toosync na této stránce.

![Rozšíření adresáře](./media/active-directory-aadconnect-get-started-custom/extension2.png)

Další informace najdete v tématu [Rozšíření adresáře](active-directory-aadconnectsync-feature-directory-extensions.md).

### <a name="enabling-single-sign-on-sso"></a>Povolení jednotného přihlašování (SSO)
Konfigurace jednotného přihlašování pro použití s ověřováním synchronizace hesel nebo průchozí je jednoduchý proces, stačí toocomplete jednou pro každou doménovou strukturu, která se synchronizované tooAzure AD. Konfigurace zahrnuje tyto dva kroky:

1.  Vytvořte účet hello nezbytné počítače ve vaší místní službě Active Directory.
2.  Konfigurace zóny intranetu hello počítačů klienta hello toosupport jeden přihlásit.

#### <a name="create-hello-computer-account-in-active-directory"></a>Vytvoření účtu počítače hello ve službě Active Directory
Pro každou doménovou strukturu, která byla přidána do Azure AD Connect budete potřebovat oprávnění správce domény toosupply tak, aby účet počítače hello lze vytvořit v každé doménové struktuře. přihlašovací údaje Hello se jenom využité toocreate hello účet a nejsou uložené nebo použít pro jiná operace. Jednoduše přidat přihlašovací údaje hello na hello **povolit jeden přihlásit** stránky průvodce hello Azure AD Connect, jak je znázorněno:

![Povolit jednotné přihlašování](./media/active-directory-aadconnect-get-started-custom/enablesso.png)

>[!NOTE]
>Pokud nechcete, aby toouse jednotné přihlašování s této doménové struktuře, můžete přeskočit určité doménové struktuře.

#### <a name="configure-hello-intranet-zone-for-client-machines"></a>Konfigurace zóny intranetu hello pro klientské počítače
tooensure hello klienta přihlášení automaticky v intranetu hello zónu budete potřebovat tooensure že dvou adres URL jsou součástí zóny intranetu hello. Tím se zajistí, že hello počítači připojeném k doméně automaticky odesílá tooAzure lístek protokolu Kerberos AD, pokud je připojený toohello podnikové síti.
Na počítači, který má hello nástroje pro správu zásad skupiny.

1.  Nástroje pro správu zásad skupiny otevřete hello
2.  Upravte hello zásad skupiny, které budou použité tooall uživatele. Například hello výchozí zásada domény.
3.  Přejděte příliš**uživatele Konfigurace počítače\Šablony pro správu\Součásti systému Windows\Internet Internetu řízení Panel\Security stránky** a vyberte **lokality tooZone přiřazení seznamu** za hello image níže.
4.  Povolit zásady hello a zadejte následující dvě položky v dialogovém okně hello hello.

        Value: `https://autologon.microsoftazuread-sso.com`  
        Data: 1  
        Value: `https://aadg.windows.net.nsatc.net`  
        Data: 1

5.  Měl by vypadat podobně jako toohello následující:  
![Zóny intranetu](./media/active-directory-aadconnect-get-started-custom/sitezone.png)

6.  Dvakrát klikněte na **OK**.

## <a name="configuring-federation-with-ad-fs"></a>Konfigurace federace se službou AD FS
Konfigurace služby AD FS se službou Azure AD Connect je jednoduchá a dá se provést několika kliknutími. Následující Hello je vyžadována před hello konfigurace.

* Server Windows Server 2012 R2 pro federační server hello s povolenou vzdálenou správou
* Server Windows Server 2012 R2 pro server služby Proxy webové aplikace hello s povolenou vzdálenou správou
* Certifikát protokolu SSL pro hello federační služby název, který chcete toouse (např. sts.contoso.com)

### <a name="ad-fs-configuration-pre-requisites"></a>Předpoklady konfigurace služby AD FS
tooconfigure službě AD FS farmy pomocí služby Azure AD Connect, zkontrolujte na vzdálených serverech hello je povolená služba WinRM. Kromě toho projít požadavek hello porty uvedené v [tabulka 3 – Azure AD Connect a federační servery/WAP](active-directory-aadconnect-ports.md#table-3---azure-ad-connect-and-ad-fs-federation-serverswap).

### <a name="create-a-new-ad-fs-farm-or-use-an-existing-ad-fs-farm"></a>Vytvoření nové farmy služby AD FS nebo použití existující farmy služby AD FS
Můžete použít existující farmu služby AD FS nebo můžete toocreate novou farmu služby AD FS. Pokud si zvolíte toocreate nový, jste certifikát SSL požadovaný tooprovide hello. Pokud certifikát protokolu SSL hello je chráněný heslem, zobrazí se výzva k hello hesla.

![Farma služby AD FS](./media/active-directory-aadconnect-get-started-custom/adfs1.png)

Pokud si zvolíte toouse existující farmu služby AD FS, přejdete přímo toohello konfigurace hello vztah důvěryhodnosti mezi AD FS a Azure obrazovky.

### <a name="specify-hello-ad-fs-servers"></a>Zadejte servery hello služby AD FS
Zadejte hello servery, které chcete tooinstall služby AD FS na. Podle potřeb plánování kapacity můžete přidat jeden nebo víc serverů. Připojte všechny servery tooActive adresář před provedením této konfigurace. Společnost Microsoft doporučuje instalaci jednoho serveru služby AD FS pro zkušební a pilotní nasazení. Pak přidejte a nasaďte další servery toomeet potřeb škálování tak, že po počáteční konfiguraci znovu spusťte Azure AD Connect.

> [!NOTE]
> Zajistěte, aby všechny servery připojené k tooan AD domény před provedením této konfigurace.
>
>

![Servery služby AD FS](./media/active-directory-aadconnect-get-started-custom/adfs2.png)

### <a name="specify-hello-web-application-proxy-servers"></a>Zadejte hello Proxy serverech webových aplikací
Zadejte hello servery, které chcete používat jako proxy servery vaší webové aplikace. proxy server webových aplikací Hello je nasazený v podsíti (extranetového) DMZ a podporuje požadavky na ověření z extranetu hello. Podle potřeb plánování kapacity můžete přidat jeden nebo víc serverů. Společnost Microsoft doporučuje instalaci jednoho proxy serveru webové aplikace pro zkušební a pilotní nasazení. Pak přidejte a nasaďte další servery toomeet potřeb škálování tak, že po počáteční konfiguraci znovu spusťte Azure AD Connect. Doporučujeme mít stejný počet proxy serverů toosatisfy ověřování z intranetu hello.

> [!NOTE]
> <li> Pokud hello účet, který používáte, není místní správce na serverech hello služby AD FS, pak budete vyzváni k zadání pověření správce.</li>
> <li> Zkontrolujte, zda je připojení HTTP/HTTPS mezi serverem Azure AD Connect hello a serveru Proxy webových aplikací hello před provedením tohoto kroku.</li>
> <li> Zkontrolujte, zda je připojení HTTP/HTTPS mezi hello serveru webové aplikace a hello AD FS server tooallow ověřování požadavků tooflow prostřednictvím.</li>
>

![Webová aplikace](./media/active-directory-aadconnect-get-started-custom/adfs3.png)

Jste výzvami tooenter přihlašovací údaje, aby hello serveru webové aplikace může vytvořit zabezpečeného připojení serveru toohello AD FS. Tyto přihlašovací údaje musí toobe místní správce na serveru služby AD FS hello.

![Proxy server](./media/active-directory-aadconnect-get-started-custom/adfs4.png)

### <a name="specify-hello-service-account-for-hello-ad-fs-service"></a>Zadejte účet služby hello hello služba AD FS
Služba Hello AD FS vyžaduje doménu služby účet tooauthenticate uživatele a hledat informace o uživatelích ve službě Active Directory. Podporuje dva typy účtů služeb:

* **Skupinový účet spravované služby** – Zaveden ve službě Active Directory Domain Services se systémem Windows Server 2012. Tento typ účtu poskytuje služby, jako je služba AD FS, jeden účet bez nutnosti heslo účtu hello tooupdate pravidelně. Tuto možnost použijte, pokud již máte řadiče domény systému Windows Server 2012 v doméně hello, které patří server služby AD FS.
* **Uživatelský účet domény** – tento typ účtu vyžaduje, abyste tooprovide heslo a pravidelně aktualizaci hello hesla při hello změní nebo vyprší platnost. Tuto možnost použijte pouze v případě, že nemáte řadiče domény systému Windows Server 2012 v doméně hello, které patří server služby AD FS.

Pokud jste vybrali skupinový účet spravované služby a tato funkce se ve službě Active Directory ještě nikdy nepoužila, zobrazí se výzva k zadání pověření správce podniku. Tyto přihlašovací údaje jsou úložiště klíčů používaných tooinitiate hello a povolit funkci hello ve službě Active Directory.

> [!NOTE]
> Azure AD Connect provede toodetect kontrolu, pokud služba hello AD FS je již zaregistrován jako hlavního názvu služby v doméně hello.  Služba AD DS neumožní toobe duplicitní SPN zaregistrovat najednou.  Pokud najde duplicitní hlavní název služby, nebudete moct tooproceed další dokud neodebere hello SPN.

![Účet služby AD FS](./media/active-directory-aadconnect-get-started-custom/adfs5.png)

### <a name="select-hello-azure-ad-domain-that-you-wish-toofederate"></a>Vyberte doménu hello Azure AD, kterou chcete toofederate
Tato konfigurace je použité toosetup hello federačního vztahu mezi AD FS a Azure. Nakonfiguruje službu AD FS tooissue zabezpečení tokeny tooAzure AD, přičemž konfigurace Azure AD tootrust hello tokeny z této konkrétní instance služby AD FS. Tato stránka jenom umožňuje tooconfigure jediné doméně v hello počáteční instalaci. Později můžete znovu spustit Azure AD Connect a nakonfigurovat další domény.

![Doména služby Azure AD](./media/active-directory-aadconnect-get-started-custom/adfs6.png)

### <a name="verify-hello-azure-ad-domain-selected-for-federation"></a>Zkontrolujte vybrané pro federaci domény hello Azure AD
Když vyberete toobe domény hello federovaný, Azure AD Connect poskytuje nezbytné informace tooverify neověřené domény. V tématu [přidat a ověření domény hello](../active-directory-add-domain.md) jak toouse tyto informace.

![Doména služby Azure AD](./media/active-directory-aadconnect-get-started-custom/verifyfeddomain.png)

> [!NOTE]
> AD Connect pokusů tooverify hello domény během hello nakonfigurovat fáze. Pokud budete pokračovat tooconfigure bez přidání hello nezbytné záznamy DNS, není možné toocomplete hello konfigurace Průvodce hello.
>
>

## <a name="configure-and-verify-pages"></a>Konfigurace a ověření stránek
na této stránce Probíhá konfigurace Hello.

> [!NOTE]
> Než budete pokračovat v instalaci a pokud jste nakonfigurovali federaci, ujistěte se, že jste nakonfigurovali [Překlad IP adres pro federační servery](active-directory-aadconnect-prerequisites.md#name-resolution-for-federation-servers).
>
>

![Připraveno tooconfigure](./media/active-directory-aadconnect-get-started-custom/readytoconfigure2.png)

### <a name="staging-mode"></a>Pracovní režim
Je možné toosetup nový synchronizační server souběžně s pracovním režimu. Je pouze podporované toohave jeden synchronizační server exportující tooone adresáře v cloudu hello. Ale pokud chcete, aby toomove z jiného serveru, například spuštěným nástrojem DirSync, pak je možné povolit Azure AD Connect v pracovním režimu. Pokud je povolená, hello synchronizační modul importu a synchronizuje data běžným způsobem, ale neexportuje nic tooAzure AD ani AD. v pracovním režimu jsou zakázány Hello funkce synchronizace a heslo zpětný zápis hesla.

![Pracovní režim](./media/active-directory-aadconnect-get-started-custom/stagingmode.png)

V pracovním režimu, je možné toomake požadované změny toohello synchronizační modul a zkontrolujte, co je o toobe exportovali. Když hello konfigurací spokojeni, znovu spusťte Průvodce instalací hello a vypněte pracovní režim. Data jsou nyní exportovaný tooAzure AD z tohoto serveru. Ujistěte se, že toodisable hello jiný server v hello stejný čas tak pouze jeden server prováděl aktivní export.

Další informace najdete v tématu [Pracovní režim](active-directory-aadconnectsync-operations.md#staging-mode).

### <a name="verify-your-federation-configuration"></a>Ověření konfigurace federace
Azure AD Connect ověřuje hello nastavení DNS pro vás, když kliknete na tlačítko Ověřit hello.

**Kontrola připojení k intranetu**

* Vyřešte federační plně kvalifikovaný název domény: Azure AD Connect kontroluje, zda mohou být vyřešeny hello federační plně kvalifikovaný název domény DNS tooensure připojení. Pokud Azure AD Connect nelze vyřešit hello plně kvalifikovaný název domény, se nezdaří ověření hello. Zajistěte, aby byl záznam DNS k dispozici pro hello federation service plně kvalifikovaný název domény v pořadí toosuccessfully hello dokončení ověření.
* Záznam DNS A: Azure AD Connect zkontroluje, jestli pro vaši federační službu existuje záznam A. V hello chybí záznam A se nezdaří ověření hello. Vytvoření A záznam a není záznam CNAME pro federační plně kvalifikovaný název domény v pořadí toosuccessfully dokončení ověření hello.

**Kontrola připojení k extranetu**

* Vyřešte federační plně kvalifikovaný název domény: Azure AD Connect kontroluje, zda mohou být vyřešeny hello federační plně kvalifikovaný název domény DNS tooensure připojení.

![Dokončit](./media/active-directory-aadconnect-get-started-custom/completed.png)

![Ověřit](./media/active-directory-aadconnect-get-started-custom/adfs7.png)

Kromě toho proveďte následující postup ověření hello:

* Ověřte, že můžete přihlásit z prohlížeče počítači připojeném k doméně v síti intranet hello: Připojte toohttps://myapps.microsoft.com a ověřte hello Přihlaste se pomocí přihlášeného účtu. účet správce Hello předdefinované AD DS není synchronizován a nelze použít pro ověření.
* Ověřte, že můžete přihlásit ze zařízení z extranetu hello. Domácí počítač nebo mobilní zařízení připojte toohttps://myapps.microsoft.com a zadáte své přihlašovací údaje.
* Ověřte přihlášení plně funkčního klienta. Připojit toohttps://testconnectivity.microsoft.com, zvolte hello **Office 365** a vyberte hello **Office 365 jeden přihlašování testovací**.

## <a name="next-steps"></a>Další kroky
Po dokončení instalace hello Odhlásit se a přihlaste znovu tooWindows. teprve pak použijte Synchronization Service Manager nebo Synchronization Rule Editor.

Teď, když máte nainstalovanou službu Azure AD Connect můžete [ověřit hello instalaci a přiřadit licence](active-directory-aadconnect-whats-next.md).

Další informace o těchto funkcích, které byly povoleny v rámci instalace hello: [prevence náhodného odstranění](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) a [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-sync.md).

Další informace o těchto běžných tématech: [Plánovač a jak synchronizaci tootrigger](active-directory-aadconnectsync-feature-scheduler.md).

Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
