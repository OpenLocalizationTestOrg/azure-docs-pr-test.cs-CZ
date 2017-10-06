---
title: "Synchronizace Azure AD Connect: Konfigurace filtrování | Microsoft Docs"
description: "Vysvětluje, jak tooconfigure filtrování v synchronizaci Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 880facf6-1192-40e9-8181-544c0759d506
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 97979b508c560a6de6cb091b1b621bc1d51b25c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-configure-filtering"></a>Synchronizace Azure AD Connect: Konfigurace filtrování
Pomocí filtrování můžete řídit objektů, které se zobrazí v Azure Active Directory (Azure AD) z vašeho místního adresáře. Výchozí konfigurace Hello trvá všechny objekty ve všech doménách v doménových strukturách hello nakonfigurované. Obecně platí toto je doporučená konfigurace hello. Uživatele, kteří používají úlohami Office 365, jako je Exchange Online a Skype pro firmy, těžit z úplný seznam globální adresy tak, aby jejich odeslání e-mailu a volání everyone. S výchozí konfigurací hello že by měla mít hello stejné prostředí, aby měly by se implementace místní Exchange nebo Lync.

V některých případech ale jste vyžaduje některé změny toohello výchozí konfiguraci nastavit. Zde je několik příkladů:

* Máte v plánu toouse hello [více Azure directory topologie AD](active-directory-aadconnect-topologies.md#each-object-only-once-in-an-azure-ad-tenant). Pak musíte tooapply filtru toocontrol, jaké objekty jsou directory synchronizované tooa konkrétní Azure AD.
* Spustit pilotní nasazení pro Azure nebo Office 365 a chcete jen podmnožinu uživatelů ve službě Azure AD. V hello malé pilotní nasazení není důležité toohave je kompletní funkce hello toodemonstrate globálním adresáři.
* Máte velký počet účtů služby a jiné neosobní účty, které nechcete použít ve službě Azure AD.
* Kvůli dodržování předpisů neodstraníte všechny uživatelské účty na místě. Zakážete jenom je. Ale ve službě Azure AD, chcete pouze aktivní účty toobe přítomen.

Tento článek popisuje, jak tooconfigure hello metod filtrování.

> [!IMPORTANT]
> Microsoft nepodporuje úpravy nebo operační synchronizace Azure AD Connect mimo hello akce, které jsou popsané dříve. Některé z těchto akcí, může dojít v nekonzistentní nebo v nepodporovaném stavu synchronizace Azure AD Connect. Výsledkem je Microsoft nemůže poskytnout se na technickou podporu takovýchto nasazeních.

## <a name="basics-and-important-notes"></a>Základní informace a důležité poznámky
Synchronizace Azure AD Connect můžete povolit filtrování kdykoli. Pokud spustíte s výchozí konfigurací synchronizace adresářů a pak nakonfigurujte filtrování, hello objekty, které jsou odfiltrována již nejsou synchronizované tooAzure AD. Z důvodu této změny se odstraní všechny objekty ve službě Azure AD, které byly dříve synchronizovaných položek, ale pak byly filtrovány ve službě Azure AD.

Před provedením změny toofiltering, ujistěte se, že jste [zakázat naplánované úlohy hello](#disable-scheduled-task) tak nejsou omylem exportovat změny nebyly dosud ověřit toobe správné.

Protože filtrování můžete odebrat mnoho objektů v hello stejný čas, chcete toomake se, že nové filtry jsou správné, před zahájením export žádné změny tooAzure AD. Po dokončení kroků konfigurace hello, důrazně doporučujeme postupujte podle hello [postup ověření](#apply-and-verify-changes) před exportovat a proveďte změny tooAzure AD.

Funkce tooprotect můžete z mnoha odstraňování objektů omylem odstraněný, hello "[prevence náhodného odstranění](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)" ve výchozím nastavení zapnutý. Pokud odstraníte mnoho objektů z důvodu toofiltering (500 ve výchozím nastavení), je třeba toofollow hello kroků v této hello tooallow článku odstraní toogo prostřednictvím tooAzure AD.

Pokud používáte sestavení před listopad 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)), proveďte konfiguraci změnu tooa filtru a použít synchronizace hesel, pak je nutné tootrigger úplnou synchronizaci všechna hesla po dokončení konfigurace hello. Kroky v tom, jak tootrigger heslo úplné synchronizace najdete v tématu [spustit úplnou synchronizaci hesel všech](active-directory-aadconnectsync-troubleshoot-password-synchronization.md#trigger-a-full-sync-of-all-passwords). Pokud je sestavení 1.0.9125 nebo novější, pak hello regular **úplné synchronizace** akce také vypočítá, jestli by měl synchronizovat hesla a pokud tento další krok se už nevyžaduje.

Pokud **uživatele** měla z důvodu chyby filtrování nechtěně odstranit objekty ve službě Azure AD, můžete znovu vytvořit hello uživatelských objektů ve službě Azure AD odebráním vaše konfigurace filtrování. Pak můžete znovu synchronizujte adresáře. Tato akce obnoví hello uživatelé z koše hello ve službě Azure AD. Nelze však zrušení odstranění jiné typy objektů. Například Pokud omylem odstraníte skupinu zabezpečení a bylo použité tooACL prostředku, skupiny hello a jeho seznamy ACL nelze obnovit.

Azure AD Connect odstraní pouze objekty, jednou považuje toobe v oboru. Pokud nejsou objekty ve službě Azure AD, které byly vytvořeny jiný modul synchronizace a tyto objekty nejsou v oboru, přidání filtrování neodstraní je. Například pokud spustíte s serveru nástroje DirSync, která vytvoří kompletní kopie celý adresář ve službě Azure AD a nainstalovat nový server Azure AD Connect sync paralelně s povoleným od začátku hello filtrováním, Azure AD Connect neodstraní hello navíc objekty, které jsou vytvořené pomocí nástroje DirSync.

Konfigurace filtrování Hello se uchovávají při instalaci nebo upgradu tooa novější verze služby Azure AD Connect. Jeho vždy nejlepší tooverify postupem, který hello konfigurace nebyl po upgradu tooa novější verze před spuštěním hello první synchronizační cyklus nechtěně změnit.

Pokud máte více než jedné doménové struktuře, pak musíte použít hello filtrování konfigurace, které jsou popsané v této doménové struktuře tooevery tématu (za předpokladu, že chcete hello stejné konfigurace pro všechny z nich).

### <a name="disable-hello-scheduled-task"></a>Zakázat hello naplánované úlohy
hello toodisable integrované se plánovače, který aktivuje synchronizační cyklus každých 30 minut, postupujte takto:

1. Přejděte tooa příkazovém řádku prostředí PowerShell.
2. Spustit `Set-ADSyncScheduler -SyncCycleEnabled $False` toodisable hello plánovače.
3. Proveďte hello změny, které jsou popsané v tomto článku.
4. Spustit `Set-ADSyncScheduler -SyncCycleEnabled $True` tooenable hello znovu plánovače.

**Pokud použijete sestavení Azure AD Connect před 1.1.105.0**  
hello toodisable naplánované se úloha, která aktivuje synchronizační cyklus každé tři hodiny, postupujte takto:

1. Spustit **Plánovač úloh** z hello **spustit** nabídky.
2. Přímo pod **Knihovna plánovače úloh**, najít hello úloha s názvem **Azure AD Sync Scheduler**, klikněte pravým tlačítkem a vyberte **zakázat**.  
   ![Plánovač úloh](./media/active-directory-aadconnectsync-configure-filtering/taskscheduler.png)  
3. Teď můžete provádět změny konfigurace a ruční spuštění hello synchronizační modul z hello **Synchronization Service Manager** konzoly.

Po dokončení všech filtrování změny, nezapomeňte toocome zpět a **povolit** hello úlohu opakujte.

## <a name="filtering-options"></a>Možnosti filtrování
Můžete použít následující filtrování konfigurace typy toohello nástroj pro synchronizaci adresáře hello:

* [**Na základě skupiny**](#group-based-filtering): filtrování založené na jednu skupinu se dá nakonfigurovat jenom na počáteční instalaci pomocí Průvodce instalací hello.
* [**Založené na doméně**](#domain-based-filtering): pomocí této možnosti můžete vybrat, které domény synchronizovat tooAzure AD. Můžete také přidat a odebrat domény z konfigurace modulu hello sync, pokud provedete změny tooyour na místní infrastrukturu po instalaci synchronizace Azure AD Connect.
* [**Organizační jednotka (OU) – na základě**](#organizational-unitbased-filtering): pomocí této možnosti můžete vybrat, které organizační jednotky synchronizovat tooAzure AD. Tato možnost je pro všechny typy objektů ve vybrané organizační jednotky.
* [**Na základě atributů**](#attribute-based-filtering): pomocí této možnosti můžete filtrovat podle hodnot atributů na objekty hello objekty. Můžete taky nechat různých filtrů pro různé typy objektů.

Více možností filtrování se dá použít na hello stejnou dobu. Například můžete použít k filtrování založené na organizační jednotku tooonly zahrnout objekty v jedné organizační jednotce. Na hello stejný čas, můžete na základě atributů filtrování toofilter hello objekty Další. Pokud používáte více metod filtrování, filtry hello mezi hello filtry použijte logické "a".

## <a name="domain-based-filtering"></a>Filtrování podle domén
Tato část vám poskytne hello kroky tooconfigure filtru domény. Je-li přidat nebo odebrat domény v doménové struktuře, po instalaci Azure AD Connect, máte také tooupdate hello konfigurací filtrování.

Hello upřednostňovaný způsob filtrování podle domén toochange je spuštěním Průvodce instalací hello a změna [domény a filtrování organizační jednotky](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). Průvodce instalací Hello automatizuje všechny hello úlohy, které jsou popsané v tomto tématu.

Pouze postupujte podle těchto kroků Pokud z nějakého důvodu jste Průvodce instalací nemůže toorun hello.

Konfigurace filtrování založené na doméně se skládá z těchto kroků:

1. [Vyberte domény hello](#select-domains-to-be-synchronized) , které chcete tooinclude v synchronizaci hello.
2. Pro každý přidat nebo odebrat domény, upravte hello [profily spuštění](#update-run-profiles).
3. [Použít a ověřte změny](#apply-and-verify-changes).

### <a name="select-hello-domains-toobe-synchronized"></a>Vyberte hello toobe domény synchronizovat
tooset hello domény filtrovat, hello následující kroky:

1. Přihlaste se toohello serveru, který je spuštěna synchronizace Azure AD Connect pomocí účtu, který je členem skupiny hello **ADSyncAdmins** skupiny zabezpečení.
2. Spustit **synchronizační služba** z hello **spustit** nabídky.
3. Vyberte **konektory**a v hello **konektory** vyberte hello konektor s typem hello **Active Directory Domain Services**. V **akce**, vyberte **vlastnosti**.  
   ![Vlastnosti konektoru](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Klikněte na tlačítko **konfigurace oddílů adresáře**.
5. V hello **Vybrat oddíly adresářů** vyberte a zrušte výběr domén podle potřeby. Ověřte, jestli jsou vybrané jenom hello oddíly, které chcete toosynchronize.  
   ![Oddíly](./media/active-directory-aadconnectsync-configure-filtering/connectorpartitions.png)  
   Pokud jste změnili na místní infrastruktuře služby Active Directory a přidat nebo odebrat domény z doménové struktury hello, pak klikněte na hello **aktualizovat** tlačítko tooget aktualizovaný seznam. Když aktualizujete, budete vyzváni k zadání pověření. Všechny přihlašovací údaje poskytnout přístup pro čtení tooWindows Server Active Directory. Nemá toobe hello uživatele, který je předem v dialogovém okně hello.  
   ![Aktualizace potřeba](./media/active-directory-aadconnectsync-configure-filtering/refreshneeded.png)  
6. Když jste hotovi, zavřete hello **vlastnosti** dialogové okno kliknutím **OK**. Pokud jste odstranili domény z doménové struktury hello, zprávu automaticky otevírané okno říká, že byla odebrána domény a tato konfigurace se vyčistí.
7. Pokračovat tooadjust hello [profily spuštění](#update-run-profiles).

### <a name="update-hello-run-profiles"></a>Aktualizovat profily hello spustit
Pokud jste aktualizovali domény filtru, musíte také profily tooupdate hello spustit.

1. V hello **konektory** seznamu, ujistěte se, že hello je vybrán konektor, který jste změnili v předchozím kroku hello. V **akce**, vyberte **konfigurovat profily spuštění**.  
   ![Konektor profily 1 spuštění](./media/active-directory-aadconnectsync-configure-filtering/connectorrunprofiles1.png)  
2. Vyhledat a identifikovat hello následující profily:
    * Úplný Import
    * Úplná synchronizace
    * Rozdílový Import
    * Rozdílová synchronizace
    * Export
3. Pro každý profil upravit hello **přidat** a **odebrat** domén.
    1. Pro každou hello pět profilů hello následující kroky pro každý **přidat** domény:
        1. Vyberte profil spuštění hello a klikněte na tlačítko **nový krok**.
        2. Na hello **krok konfigurace** stránku hello **typ** rozevírací nabídky vyberte hello odpovídající typ s hello stejný název jako hello profilu, které jste konfigurace. Pak klikněte na tlačítko **Další**.  
        ![Konektor profily 2 spuštění](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep1.png)  
        3. Na hello **konfigurace konektoru** stránku hello **oddílu** rozevírací nabídky, vyberte hello název hello domény, zda jste přidali tooyour domény filtru.  
        ![Konektor profily 3 spuštění](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep2.png)  
        4. tooclose hello **konfigurací profilu spuštění** dialogové okno, klikněte na tlačítko **Dokončit**.
    2. Pro každou hello pět profilů hello následující kroky pro každý **odebrat** domény:
        1. Vyberte profil spuštění hello.
        2. Pokud hello **hodnotu** z hello **oddílu** atribut je identifikátor GUID, vyberte hello spustit krok a klikněte na tlačítko **odstranit krok**.  
        ![Konektor profily 4 spuštění](./media/active-directory-aadconnectsync-configure-filtering/runprofilesdeletestep.png)  
    3. Zkontrolujte změny. Každá doména, které chcete toosynchronize by měl být uvedený jako krok v každý profil spuštění.
4. tooclose hello **konfigurovat profily spuštění** dialogové okno, klikněte na tlačítko **OK**.
5.  Konfigurace hello toocomplete, je nutné toorun **úplný import** a **rozdílová synchronizace**. Pokračujte ve čtení hello části [použít a ověřte změny](#apply-and-verify-changes).

## <a name="organizational-unitbased-filtering"></a>Filtrování organizační jednotka se systémem
Hello upřednostňovaný způsob toochange založené na organizační jednotku filtrování je spuštěním Průvodce instalací hello a změna [domény a filtrování organizační jednotky](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). Průvodce instalací Hello automatizuje všechny hello úlohy, které jsou popsané v tomto tématu.

Pouze postupujte podle těchto kroků Pokud z nějakého důvodu jste Průvodce instalací nemůže toorun hello.

tooconfigure organizační jednotky – filtrování podle, hello následující kroky:

1. Přihlaste se toohello serveru, který je spuštěna synchronizace Azure AD Connect pomocí účtu, který je členem skupiny hello **ADSyncAdmins** skupiny zabezpečení.
2. Spustit **synchronizační služba** z hello **spustit** nabídky.
3. Vyberte **konektory**a v hello **konektory** vyberte hello konektor s typem hello **Active Directory Domain Services**. V **akce**, vyberte **vlastnosti**.  
   ![Vlastnosti konektoru](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Klikněte na tlačítko **konfigurace oddílů adresářů**, vyberte hello domény má tooconfigure a pak klikněte na tlačítko **kontejnery**.
5. Když se zobrazí výzva, zadejte všechny přihlašovací údaje, s přístupem pro čtení tooyour místní služby Active Directory. Nemá toobe hello uživatele, který je předem v dialogovém okně hello.
6. V hello **vybrat kontejnery** dialogové okno, zrušte hello organizační jednotky, nemáte má toosynchronize s adresářem v cloudu hello a pak klikněte na tlačítko **OK**.  
   ![Organizační jednotky v dialogové okno Vybrat kontejnery hello](./media/active-directory-aadconnectsync-configure-filtering/ou.png)  
   * Hello **počítače** kontejneru, měla by být vybrána pro vaše počítače toobe Windows 10 bylo úspěšně synchronizováno tooAzure AD. Pokud počítače připojené k doméně se nachází v jiné organizační jednotky, ujistěte se, že ty, které jsou vybrány.
   * Hello **ForeignSecurityPrincipals** kontejneru, měla by být vybrána, pokud máte více doménových struktur s vztahy důvěryhodnosti. Tento kontejner umožňuje toobe členství ve skupině zabezpečení mezi doménovými strukturami přeložit.
   * Hello **RegisteredDevices** organizační jednotky, měla by být vybrána, pokud jste povolili funkce zpětný zápis zařízení hello. Pokud používáte další funkcí zpětného zápisu, jako je například zpětný zápis skupin, zkontrolujte, zda že jsou vybrané těchto umístění.
   * Vyberte jiné OU, kde jsou umístěny uživatelů, objektů InetOrgPerson, skupiny, kontakty a počítače. Hello obrázku jsou všechny tyto organizační jednotky umístěné v hello ManagedObjects organizační jednotky.
   * Pokud použijete filtrování podle skupiny, musí být součástí hello organizační jednotky, kde je umístěna skupina hello.
   * Všimněte si, že můžete nakonfigurovat, jestli se nové organizační jednotky, které jsou přidány po dokončení konfigurace filtrování hello synchronizována, nebo nejsou synchronizovány. Viz další část hello podrobnosti.
7. Když jste hotovi, zavřete hello **vlastnosti** dialogové okno kliknutím **OK**.
8. Konfigurace hello toocomplete, je nutné toorun **úplný import** a **rozdílová synchronizace**. Pokračujte ve čtení hello části [použít a ověřte změny](#apply-and-verify-changes).

### <a name="synchronize-new-ous"></a>Synchronizovat nové organizační jednotky
Ve výchozím nastavení jsou synchronizovány nové organizační jednotky, které jsou vytvořeny po filtrování byl nakonfigurován. Tento stav je indikován zaškrtnuté políčko. Také můžete zrušit zaškrtnutí některé dílčí organizační jednotky. tooget toto chování, dokud nebude bílé se modré zaškrtnutím (výchozího stavu), klikněte na pole hello. Poté zrušte výběr žádné dílčí-organizační jednotky nechcete toosynchronize.

Pokud se synchronizují všechny dílčí-organizační jednotky, je políčko hello bílé s modrá značka zaškrtnutí.  
![Organizační jednotku s všechna políčka vybrané](./media/active-directory-aadconnectsync-configure-filtering/ousyncnewall.png)

Pokud některé dílčí organizační jednotky nezaškrtnuté, je políčko hello šedá s bílým zaškrtnutí.  
![Organizační jednotku s některé dílčí ou zrušit](./media/active-directory-aadconnectsync-configure-filtering/ousyncnew.png)

Pomocí této konfigurace se synchronizují nové organizační jednotky, která byla vytvořena v rámci ManagedObjects.

Průvodce instalací Hello Azure AD Connect vždycky vytvoří tuto konfiguraci.

### <a name="dont-synchronize-new-ous"></a>Nesynchronizovat nové organizační jednotky
Můžete nakonfigurovat synchronizaci hello modul toonot synchronizovat nové organizačními jednotkami po dokončení hello konfigurací filtrování. Tento stav je uvedené v hello uživatelského rozhraní pomocí zobrazovaných plnou šedou bez zaškrtnutí políčka hello. tooget toto chování, dokud nebude bílé bez zaškrtnutí, klikněte na pole hello. Pak vyberte hello sub-organizační jednotky, které chcete toosynchronize.

![Organizační jednotky s kořenem hello zrušit](./media/active-directory-aadconnectsync-configure-filtering/oudonotsyncnew.png)

Pomocí této konfigurace nové organizační jednotky, která byla vytvořena v rámci ManagedObjects není synchronizován.

## <a name="attribute-based-filtering"></a>Filtrování podle atributů
Ujistěte se, že používáte hello listopad 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)) nebo novější build pro tyto kroky toowork.

Filtrování podle atributů je hello nejpružnější způsob toofilter objekty. Můžete použít hello výkon [deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md) toocontrol skoro každý aspekt po objekt synchronizovat tooAzure AD.

Můžete použít [příchozí](#inbound-filtering) filtrování z úložiště metaverse toohello služby Active Directory, a [odchozí](#outbound-filtering) filtrování z hello metaverse tooAzure AD. Doporučujeme použít příchozí filtrování, protože se jedná o nejjednodušší toomaintain hello. Používejte pouze odchozí filtrování, pokud je to požadováno toojoin objekty z více než jedné doménové struktuře než hello vyhodnocení může proběhnout.

### <a name="inbound-filtering"></a>Příchozí filtrování
Příchozí filtrování používá hello výchozí konfigurace, kde budete tooAzure AD objekty musí mít cloudFiltered atribut úložiště metaverse hello není nastavena toobe hodnotu tooa synchronizovány. Pokud hodnota tohoto atributu je nastaven příliš**True**, pak hello objekt není synchronizován. Neměla by být nastavená příliš**False**, návrh. toomake ostatní pravidla mít možnost toocontribute hello hodnotu, tento atribut je pouze by měl toohave hello hodnoty **True** nebo **NULL** (chybí).

V příchozí filtrování použijete hello výkon **oboru** toodetermine, která objekty toosynchronize nebo nebude synchronizovat. Toto je, kde můžete provést úpravy toofit požadavky vaší vlastní organizaci. modul oboru Hello má **skupiny** a **klauzule** toodetermine při synchronizační pravidlo je v oboru. Skupina obsahuje jednu nebo více klauzulích. Mezi více klauzulí a logickým operátorem "nebo" mezi více skupin je logické "a".

Dejte nám podívejte se na příklad:  
![Rozsah](./media/active-directory-aadconnectsync-configure-filtering/scope.png)  
To byste si měli přečíst jako **(oddělení = IT) nebo (oddělení = prodeje a c = US)**.

V hello následující ukázky a kroky, použijte objekt uživatele hello jako příklad, ale můžete použít pro všechny typy objektů.

V následující ukázky hello začíná hello přednost před hodnotu 50. To může být jakékoli číslo, které nejsou používány, ale musí být menší než 100.

#### <a name="negative-filtering-do-not-sync-these"></a>Záporné filtrování: "není synchronizována tyto"
Následující ukázka hello, můžete vyfiltrovat (nesynchronizovat) všech uživatelů kde **extensionAttribute15** má hodnotu hello **NoSync**.

1. Přihlaste se toohello serveru, který je spuštěna synchronizace Azure AD Connect pomocí účtu, který je členem skupiny hello **ADSyncAdmins** skupiny zabezpečení.
2. Spustit **editoru pravidel synchronizace** z hello **spustit** nabídky.
3. Zajistěte, aby **příchozí** je vybrána a klikněte na tlačítko **přidat nové pravidlo**.
4. Zadejte pro pravidlo hello popisný název, například "*v ze služby Active Directory – uživatel DoNotSyncFilter*". Vyberte hello správné doménové struktury, vyberte **uživatele** jako hello **typ objektu CS**a vyberte **osoba** jako hello **typ objektu MV**. V **typu propojení**, vyberte **připojení**. V **přednost**, zadejte hodnotu, která není aktuálně používán jiným pravidlem synchronizace (například 50) a pak klikněte na tlačítko **Další**.  
   ![Příchozí 1 popis](./media/active-directory-aadconnectsync-configure-filtering/inbound1.png)  
5. V **Scoping filtru**, klikněte na tlačítko **přidat skupinu**a klikněte na tlačítko **přidat klauzuli**. V **atribut**, vyberte **ExtensionAttribute15**. Ujistěte se, že **operátor** je nastaven příliš**ROVNA**a zadejte hodnotu hello **NoSync** v hello **hodnotu** pole. Klikněte na **Další**.  
   ![Příchozí 2 oboru](./media/active-directory-aadconnectsync-configure-filtering/inbound2.png)  
6. Nechte hello **připojení** pravidla prázdný a potom klikněte na **Další**.
7. Klikněte na tlačítko **přidat transformace**, vyberte hello **typ toku** jako **konstantní**a vyberte **cloudFiltered** jako hello  **Cíle atributů**. V hello **zdroj** textového pole, typ **True**. Klikněte na tlačítko **přidat** toosave hello pravidlo.  
   ![Příchozí 3 transformace](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)
8. Konfigurace hello toocomplete, je nutné toorun **úplné synchronizace**. Pokračujte ve čtení hello části [použít a ověřte změny](#apply-and-verify-changes).

#### <a name="positive-filtering-only-sync-these"></a>Kladné filtrování: "synchronizovat pouze ty"
Vyjádření kladné filtrování může být více náročné, protože máte také tooconsider objekty, které nejsou zřejmé toobe synchronizován, jako je například konferenční místnosti. Můžete se také probíhající toooverride hello výchozí filtr v pravidle out-of-box hello **v ze služby Active Directory - připojení uživatele k**. Když vytvoříte vlastního filtru, zajistěte, aby toonot zahrnují nezbytně nutné systémové objekty, objekty konflikt replikace, speciální poštovních schránek a hello účty služby Azure AD Connect.

kladné filtrování možnost Hello vyžaduje dvě pravidla synchronizace. Musíte jedno pravidlo (nebo několik) s hello správný obor toosynchronize objekty. Budete také potřebovat druhé pravidlo synchronizace catch-all, který odfiltruje všechny objekty, které dosud nebyly určeny jako objekt, který se má synchronizovat.

V následujícím příkladu hello, kde hello oddělení atribut má hodnotu hello uživatelské objekty pouze synchronizovat **prodej**.

1. Přihlaste se toohello serveru, který je spuštěna synchronizace Azure AD Connect pomocí účtu, který je členem skupiny hello **ADSyncAdmins** skupiny zabezpečení.
2. Spustit **editoru pravidel synchronizace** z hello **spustit** nabídky.
3. Zajistěte, aby **příchozí** je vybrána a klikněte na tlačítko **přidat nové pravidlo**.
4. Zadejte pro pravidlo hello popisný název, například "*v ze služby Active Directory – prodej uživatele synchronizovat*". Vyberte hello správné doménové struktury, vyberte **uživatele** jako hello **typ objektu CS**a vyberte **osoba** jako hello **typ objektu MV**. V **typu propojení**, vyberte **připojení**. V **přednost**, zadejte hodnotu, která není aktuálně používán jiným pravidlem synchronizace (například 51) a pak klikněte na tlačítko **Další**.  
   ![Příchozí 4 popis](./media/active-directory-aadconnectsync-configure-filtering/inbound4.png)  
5. V **Scoping filtru**, klikněte na tlačítko **přidat skupinu**a klikněte na tlačítko **přidat klauzuli**. V **atribut**, vyberte **oddělení**. Ujistěte se, že operátor je nastaven příliš**ROVNA**a zadejte hodnotu hello **prodej** v hello **hodnotu** pole. Klikněte na **Další**.  
   ![Příchozí 5 oboru](./media/active-directory-aadconnectsync-configure-filtering/inbound5.png)  
6. Nechte hello **připojení** pravidla prázdný a potom klikněte na **Další**.
7. Klikněte na tlačítko **přidat transformace**, vyberte **konstantní** jako hello **typ toku**a vyberte hello **cloudFiltered** jako hello  **Cíle atributů**. V hello **zdroj** zadejte **False**. Klikněte na tlačítko **přidat** toosave hello pravidlo.  
   ![Příchozí 6 transformace](./media/active-directory-aadconnectsync-configure-filtering/inbound6.png)  
   Toto je zvláštní případ, kdy je explicitně nastavit cloudFiltered příliš**False**.
8. Nyní je k dispozici toocreate hello catch-all synchronizační pravidlo. Zadejte pro pravidlo hello popisný název, například "*v ze služby Active Directory – všechny uživatele Catch filtru*". Vyberte hello správné doménové struktury, vyberte **uživatele** jako hello **typ objektu CS**a vyberte **osoba** jako hello **typ objektu MV**. V **typu propojení**, vyberte **připojení**. V **přednost před**, zadejte hodnotu, která není aktuálně používán jiným pravidlem synchronizace (například 99). Vybrali jste přednost před hodnotu, která je vyšší (nižší prioritu) než hello předchozí synchronizační pravidlo. Ale některé místnosti jste zbývajících také tak, aby můžete přidat více pravidel filtrování synchronizace později když chcete, aby toostart synchronizaci další oddělení. Klikněte na **Další**.  
   ![Příchozí 7 popis](./media/active-directory-aadconnectsync-configure-filtering/inbound7.png)  
9. Nechte **Scoping filtru** prázdná a klikněte na tlačítko **Další**. Filtr prázdný znamená, že daného pravidla hello je toobe použít tooall objekty.
10. Nechte hello **připojení** pravidla prázdný a potom klikněte na **Další**.
11. Klikněte na tlačítko **přidat transformace**, vyberte **konstantní** jako hello **typ toku**a vyberte **cloudFiltered** jako hello  **Cíle atributů**. V hello **zdroj** zadejte **True**. Klikněte na tlačítko **přidat** toosave hello pravidlo.  
    ![Příchozí 3 transformace](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)  
12. Konfigurace hello toocomplete, je nutné toorun **úplné synchronizace**. Pokračujte ve čtení hello části [použít a ověřte změny](#apply-and-verify-changes).

Pokud potřebujete, můžete vytvořit další pravidla první typu hello, kde můžete zahrnout další objekty hello synchronizace.

### <a name="outbound-filtering"></a>Odchozí filtrování
V některých případech je nutné toodo hello filtrování až po připojení hello objektů v hello metaverse. Například je může být nutné toolook na atribut mail hello z doménové struktury prostředku hello a atribut userPrincipalName hello z doménové struktury účtu hello, pokud objekt by měl synchronizovat toodetermine. V těchto případech můžete vytvořit hello hello odchozí pravidlo filtrování.

V tomto příkladu změníte hello filtrování tak, aby pouze uživatelé, které mají své e-mailu a userPrincipalName končící na @contoso.com jsou synchronizovány:

1. Přihlaste se toohello serveru, který je spuštěna synchronizace Azure AD Connect pomocí účtu, který je členem skupiny hello **ADSyncAdmins** skupiny zabezpečení.
2. Spustit **editoru pravidel synchronizace** z hello **spustit** nabídky.
3. V části **typ pravidel**, klikněte na tlačítko **odchozí**.
4. Najít hello pravidlo s názvem **Out tooAAD – připojení uživatele k**a klikněte na tlačítko **upravit**.
5. V místní nabídce hello zodpovědět **Ano** toocreate kopii hello pravidlo.
6. Na hello **popis** změňte **přednost** tooan nepoužívané hodnotu, jako je 50.
7. Klikněte na tlačítko **Scoping filtru** hello levé navigační a pak klikněte na tlačítko **přidat klauzuli**. V **atribut**, vyberte **e-mailu**. V **operátor**, vyberte **ENDSWITH**. V **hodnotu**, typ  **@contoso.com** a potom klikněte na **přidat klauzuli**. V **atribut**, vyberte **userPrincipalName**. V **operátor**, vyberte **ENDSWITH**. V **hodnotu**, typ  **@contoso.com** .
8. Klikněte na **Uložit**.
9. Konfigurace hello toocomplete, je nutné toorun **úplné synchronizace**. Pokračujte ve čtení hello části [použít a ověřte změny](#apply-and-verify-changes).

## <a name="apply-and-verify-changes"></a>Použít a ověřte změny
Po provedení změny konfigurace, je nutné je použít toohello objekty, které jsou již přítomny v systému hello. Je také možné, že má být zpracován hello objekty, které nejsou aktuálně v hello synchronizační modul (a hello synchronizační modul musí tooread hello zdrojovém systému znovu tooverify jeho obsah).

Pokud jste změnili konfiguraci hello pomocí **domény** nebo **organizační jednotky** filtrování, pak je nutné toodo **úplný import**, za nímž následují **rozdílů synchronizace**.

Pokud jste změnili konfiguraci hello pomocí **atribut** filtrování, pak je nutné toodo **úplné synchronizace**.

Hello následující kroky:

1. Spustit **synchronizační služba** z hello **spustit** nabídky.
2. Vyberte **konektory**. V hello **konektory** vyberte hello konektoru, které jste provedli dříve změnu konfigurace. V **akce**, vyberte **spustit**.  
   ![Konektor spustit](./media/active-directory-aadconnectsync-configure-filtering/connectorrun.png)  
3. V **profily spuštění**, vyberte hello operace, která byla uvedená v předchozí části hello. Pokud potřebujete toorun dvě akce, spusťte hello druhý po hello první z nich dokončila. (hello **stavu** sloupec je **nečinnosti** pro konektor hello vybrána.)

Po synchronizaci hello jsou všechny změny dvoufázové instalace toobe exportovali. Dříve, než ve skutečnosti hello změny ve službě Azure AD, budete chtít tooverify správnost tyto změny.

1. Spusťte příkazový řádek a přejděte příliš`%Program Files%\Microsoft Azure AD Sync\bin`.
2. Spusťte `csexport "Name of Connector" %temp%\export.xml /f:x`.  
   Název Hello hello konektoru se synchronizační služba. Obsahuje název podobné too"contoso.com – AAD" pro Azure AD.
3. Spusťte `CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`.
4. Nyní máte soubor v adresáři % temp % s názvem export.csv, který může být prověřen v aplikaci Microsoft Excel. Tento soubor obsahuje všechny hello změny, které jsou o toobe exportovali.
5. Proveďte potřebné změny hello toohello data nebo konfigurace a spusťte tyto kroky opakujte (Import, synchronizovat a ověřte, zda) až hello změny, které jsou o toobe exportovat se toho, co očekáváte.

Až budete spokojeni, exportujte hello změny tooAzure AD.

1. Vyberte **konektory**. V hello **konektory** vyberte hello konektoru služby Azure AD. V **akce**, vyberte **spustit**.
2. V **profily spuštění**, vyberte **exportovat**.
3. Pokud vaše změny konfigurace odstraňují mnoho objektů, pak zobrazí chybu v souboru exportu hello při hello počet je větší než prahová hodnota hello nakonfigurované (ve výchozím nastavení 500). Pokud se zobrazí tato chyba, pak je nutné zakázat hello tootemporarily "[prevence náhodného odstranění](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)" funkce.

Nyní je čas tooenable hello scheduler znovu.

1. Spustit **Plánovač úloh** z hello **spustit** nabídky.
2. Přímo pod **Knihovna plánovače úloh**, najít hello úloha s názvem **Azure AD Sync Scheduler**, klikněte pravým tlačítkem a vyberte **povolit**.

## <a name="group-based-filtering"></a>Filtrování podle skupiny
Můžete nakonfigurovat filtrování hello na základě skupiny poprvé, nainstalujte Azure AD Connect s použitím [vlastní instalaci](active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups). Jeho určený pro pilotní nasazení místo pouze malou sadu objektů toobe synchronizovány. Pokud zakážete filtrování podle skupiny, nelze znovu povolena. Má *nepodporuje* toouse skupiny založené na filtrování na vlastní konfiguraci. Tooconfigure ji obsahoval podporované tato funkce pouze pomocí Průvodce instalací hello. Po dokončení vašeho pilotního nasazení, potom použijte jednu z hello jiné možnosti filtrování v tomto tématu. Pokud používáte založené na organizační jednotku filtrování ve spojení s filtrování podle skupiny, musí být zahrnut hello OU(s), kde se nachází hello skupiny a její členy.

Při synchronizaci více doménových struktur služby AD, můžete nakonfigurovat filtrování podle skupiny zadáním jiné skupiny pro každý konektor AD. Pokud chcete toosynchronize uživatel v jedné doménové struktuře AD a hello stejného uživatele obsahuje jednu nebo více odpovídající FSP (cizí objekt zabezpečení) objekty v jiných doménových strukturách AD, ujistěte se, tento objekt uživatele hello a všechny odpovídající FSP objekty jsou v rámci na základě skupiny filtrování oboru. Pokud jeden nebo více objektů FSP hello jsou vyloučená, a to na základě skupiny filtrování, objekt uživatele hello nebudou synchronizovaná tooAzure AD.

## <a name="next-steps"></a>Další kroky
- Další informace o [synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfigurace.
- Další informace o [integrace místních identit s Azure AD](active-directory-aadconnect.md).
