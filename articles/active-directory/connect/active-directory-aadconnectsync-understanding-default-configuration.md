---
title: "Synchronizace Azure AD Connect: Principy hello výchozí konfigurace | Microsoft Docs"
description: "Tento článek popisuje hello výchozí konfigurace v synchronizaci Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: ed876f22-6892-4b9d-acbe-6a2d112f1cd1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 1cf95759af913cad8a1393839d3a836434e7c9bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understanding-hello-default-configuration"></a>Synchronizace Azure AD Connect: Principy hello výchozí konfigurace
Tento článek vysvětluje hello out-of-box konfigurační pravidla. Ho dokumenty hello pravidla a jak tato pravidla ovlivnit hello konfigurace. Je také vás provede procesem hello výchozí konfigurace synchronizace služby Azure AD Connect. Hello cílem je, že hello čtečky plně chápe, jak funguje hello konfigurační model s názvem deklarativní zřizování v příkladu reálného. Tento článek předpokládá, že jste již nainstalovali a konfigurace synchronizace služby Azure AD Connect pomocí Průvodce instalací hello.

Podrobnosti hello toounderstand hello konfigurační model, přečtěte si [Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

## <a name="out-of-box-rules-from-on-premises-tooazure-ad"></a>Out-of-box pravidla z tooAzure místní AD
Následující výrazy Hello možné najít v konfiguraci out-of-box hello.

### <a name="user-out-of-box-rules"></a>Pravidla out-of-box uživatele
Typ objektu iNetOrgPerson toohello také platí následující pravidla.

Objekt uživatele, musí splňovat následující toobe synchronizované hello:

* Musí mít sourceAnchor.
* Po hello objekt byl vytvořen ve službě Azure AD a potom sourceAnchor nelze změnit. Pokud je hodnota hello změněné místní, zastaví hello objekt synchronizace dokud nezměníte předchozí hodnotu back tooits hello sourceAnchor.
* Musí mít atribut accountEnabled (userAccountControl) hello naplněno. S místní Active Directory tento atribut je stále přítomen a vyplněná.

Hello následující uživatelské objekty jsou **není** synchronizované tooAzure AD:

* `IsPresent([isCriticalSystemObject])`. Ujistěte se, mnoho out-of-box objektů ve službě Active Directory, jako je například hello předdefinovaný účet správce, nejsou synchronizovány.
* `IsPresent([sAMAccountName]) = False`. Zkontrolujte, zda nejsou synchronizované objekty uživatele se žádné atributy sAMAccountName. Tento případ by mohlo dojít pouze prakticky v doméně upgradu ze systému Windows NT 4.
* `Left([sAMAccountName], 4) = "AAD_"`, `Left([sAMAccountName], 5) = "MSOL_"`. Nesynchronizovat hello účet využívaný synchronizace Azure AD Connect a její starší verze.
* Nesynchronizovat účty serveru Exchange, které nebude fungovat v systému Exchange Online.
  * `[sAMAccountName] = "SUPPORT_388945a0"`
  * `Left([mailNickname], 14) = "SystemMailbox{"`
  * `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`
  * `(Left([sAMAccountName], 4) = "CAS_" && (InStr([sAMAccountName], "}")> 0))`
* Nesynchronizovat objekty, které nebude fungovat v systému Exchange Online.
  `CBool(IIF(IsPresent([msExchRecipientTypeDetails]),BitAnd([msExchRecipientTypeDetails],&H21C07000) > 0,NULL))`  
  Tato bitová maska (& H21C07000) by filtrovat hello následující objekty:
  * Poštovní veřejné složky
  * Odpovídající poštovní schránky systému
  * Poštovní schránky databáze poštovní schránky (poštovní schránka systému)
  * Univerzální skupina zabezpečení (nemohli použít pro uživatele, ale je k dispozici z důvodu starší verze)
  * Non-univerzální skupiny (nemohli použít pro uživatele, ale je k dispozici z důvodu starší verze)
  * Plán poštovní schránky
  * Zjišťování poštovní schránky
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Nesynchronizovat všechny objekty postižené replikace.

platí následující pravidla atribut Hello:

* `sourceAnchor <- IIF([msExchRecipientTypeDetails]=2,NULL,..)`. atribut sourceAnchor Hello není podílí z propojená poštovní schránky. Předpokládá se, že pokud byl nalezen propojená poštovní schránka, je skutečný účet hello později připojený.
* Exchange související s atributy se synchronizují pouze pokud hello atribut **mailNickName** má hodnotu.
* Po několika doménovými strukturami se potom atributy používán v hello následující pořadí:
  1. Atributy související toosign v (pro příklad userPrincipalName) jsou podílí z doménové struktury hello s aktivovaný účet.
  2. Atributy, které lze nalézt v Exchange GAL (seznamu pro globální adresa) jsou podílí z doménové struktury hello s poštovní schránky systému Exchange.
  3. Pokud je možné najít žádné poštovní schránky, tyto atributy mohou pocházet z jakékoli doménové struktury.
  4. Exchange související s atributy (technické atributy nejsou viditelné v hello GAL) jsou podílí z doménové struktury hello kde `mailNickname ISNOTNULL`.
  5. Pokud máte více doménových struktur, které by splňovat jedno z těchto pravidel a pak hello pořadí vytváření (datum a čas) konektorů hello (doménové struktury) je použité toodetermine, která doménová struktura přispívá hello atributy.

### <a name="contact-out-of-box-rules"></a>Obraťte se na out-of-box pravidla
Objekt kontaktu musí splňovat následující toobe synchronizované hello:

* Obraťte se na Hello musí být povoleným e-mailem. Ověření se hello následující pravidla:
  * `IsPresent([proxyAddresses]) = True)`. musí být naplněn Hello proxyAddresses atribut.
  * Primární e-mailovou adresu naleznete v atributu proxyAddresses hello nebo atribut mail hello. Hello přítomnost @ je použité tooverify, že je hello obsah e-mailovou adresu. Mezi tyto dvě pravidla musí být vyhodnotí tooTrue.
    * `(Contains([proxyAddresses], "SMTP:") > 0) && (InStr(Item([proxyAddresses], Contains([proxyAddresses], "SMTP:")), "@") > 0))`. Je k dispozici položku s "SMTP:" a pokud neexistuje, můžete @ být nalezen v řetězci hello?
    * `(IsPresent([mail]) = True && (InStr([mail], "@") > 0)`. Je hello naplní atribut mail a je-li, můžete @ být nalezen v řetězci hello?

Hello následující kontaktní objekty jsou **není** synchronizované tooAzure AD:

* `IsPresent([isCriticalSystemObject])`. Ujistěte se, jsou synchronizovány žádné kontaktní objekty označení kritický. Nesmí být žádné s výchozí konfigurací.
* `((InStr([displayName], "(MSOL)") > 0) && (CBool([msExchHideFromAddressLists])))`.
* `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`. Tyto objekty nebude fungovat v systému Exchange Online.
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Nesynchronizovat všechny objekty postižené replikace.

### <a name="group-out-of-box-rules"></a>Skupina out-of-box pravidla
Objekt skupiny musí splňovat následující toobe synchronizované hello:

* Musí mít méně než 50 000 členy. Je to počet hello počet členů v místní skupině hello.
  * Pokud má víc členů než synchronizace začne hello poprvé, hello skupiny není synchronizován.
  * Pokud z růst hello počet členů, když byl původně vytvořen, pak když dorazí do 50 000 členy zastaví synchronizace, dokud hello členství počet je menší než 50 000 znovu.
  * Poznámka: počet 50 000 členství hello je také vynucené Azure AD. Můžete nejsou možné toosynchronize skupiny s více členů, i když můžete upravit nebo odebrat toto pravidlo.
* Pokud skupina hello **distribuční skupiny**, a také musí být povoleno e-mailu. V tématu [obraťte se na pravidla out-of-box](#contact-out-of-box-rules) pro toto pravidlo se nevynutí.

Následující objekty skupiny Hello jsou **není** synchronizované tooAzure AD:

* `IsPresent([isCriticalSystemObject])`. Ujistěte se, mnoho out-of-box objektů ve službě Active Directory, jako je například hello předdefinované skupiny administrators, nejsou synchronizovány.
* `[sAMAccountName] = "MSOL_AD_Sync_RichCoexistence"`. Starší verze skupina, která nástroj DirSync používá.
* `BitAnd([msExchRecipientTypeDetails],&amp;H40000000)`. Skupina role.
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Nesynchronizovat všechny objekty postižené replikace.

### <a name="foreignsecurityprincipal-out-of-box-rules"></a>ForeignSecurityPrincipal out-of-box pravidla
FSP připojeni příliš "žádné" (\*) objektu v úložišti metaverse hello. Ve skutečnosti tohoto spojení se dělá jenom pro uživatele a skupiny zabezpečení. Tato konfigurace zajistí, že jsou ve skupinách mezi doménovými strukturami přeložit a správně reprezentováni v Azure AD.

### <a name="computer-out-of-box-rules"></a>Pravidla out-of-box počítače
Objekt počítače se musí splňovat následující toobe synchronizované hello:

* `userCertificate ISNOTNULL`. Pouze počítače s Windows 10 naplnit tento atribut. Všechny objekty počítače s hodnotou v tomto atributu jsou synchronizovány.

## <a name="understanding-hello-out-of-box-rules-scenario"></a>Principy hello out-of-box pravidla scénář
V tomto příkladu používáme nasazení jedné doménové struktuře účtu (A), jednu doménovou strukturu prostředků (R) a jeden adresář Azure AD.

![Obrázek s popis scénáře](./media/active-directory-aadconnectsync-understanding-default-configuration/scenario.png)

V této konfiguraci se předpokládá, že aktivovaný účet v doménové struktuře účtu hello a deaktivovaného účtu v doménové struktuře prostředku hello s propojená poštovní schránka.

Naším cílem s hello výchozí konfigurace je:

* Toosign v související s atributy se synchronizují z doménové struktury hello účtem hello povolena.
* Atributy, které lze nalézt v hello GAL (seznamu pro globální adresa) jsou synchronizovány z doménové struktury hello s hello poštovní schránky. Pokud lze nalézt žádné poštovní schránky, je použít jiné doménové struktury.
* Pokud je nalezen propojená poštovní schránka, hello propojené aktivovaný účet je nutné nalézt pro hello objekt toobe exportovat tooAzure AD.

### <a name="synchronization-rule-editor"></a>Synchronization Rule Editor
Konfigurace Hello se může zobrazit a změnit nástrojem hello synchronizace pravidla Editor (SRE) a tooit zástupce lze nalézt v nabídce start hello.

![Ikona editoru pravidel synchronizace](./media/active-directory-aadconnectsync-understanding-default-configuration/sre.png)

instalaci synchronizace Azure AD Connect Hello SRE je nástroj resource kit. možnost toostart toobe, musíte být členem skupiny ADSyncAdmins hello. Po jeho spuštění, se zobrazí přibližně takto:

![Příchozí pravidla synchronizace](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

V tomto podokně zobrazí všechny synchronizační pravidla, které jsou vytvořeny pro vaši konfiguraci. Každý řádek v tabulce hello je jedno pravidlo synchronizace. toohello zbývajících v části typy pravidel, jsou uvedeny dva různé typy hello: příchozí a odchozí. Příchozí a odchozí je ze zobrazení hello hello metaverse. Jsou především probíhající toofocus na hello příchozí pravidla v tomto přehledu. Hello skutečný seznam pravidel synchronizace závisí na schéma hello zjištěna ve službě AD. Na obrázku hello výše byly vytvořeny hello účet doménové struktury (fabrikamonline.com) nemá žádné služby, jako je Exchange a Lync a žádná pravidla synchronizace pro tyto služby. Ale v doménové struktuře prostředku hello (res.fabrikamonline.com) zjistíte synchronizační pravidla pro tyto služby. obsah Hello pravidel hello se liší v závislosti na verzi hello zjištěna. Například v nasazení s Exchange 2013 existují další toky atributů, které jsou nakonfigurované než v systému Exchange 2010 nebo 2007.

### <a name="synchronization-rule"></a>Pravidlo synchronizace
Pravidlo synchronizace je objekt konfigurace sadu atributů toku, pokud je podmínka. Je také použít toodescribe, jak je objekt v prostoru konektoru tooan související objekt v metaverse hello, označuje jako **spojení** nebo **odpovídat**. Hello synchronizační pravidla mají přednost před hodnotu, která určuje, jak se vztahují tooeach jiné. Pravidlo synchronizace s nižší číselná hodnota má vyšší prioritu a v konfliktu tok atributů, vyšší prioritu wins hello řešení konfliktů.

Jako příklad, podívejte se na hello synchronizační pravidlo **v ze služby Active Directory – uživatel AccountEnabled**. Označit tento řádek ve hello SRE a vyberte **upravit**.

Vzhledem k tomu, že toto pravidlo je pravidlo out-of-box, zobrazí se při otevření hello pravidlo upozornění. Neprovádějte žádné [změny pravidel tooout-of-box](active-directory-aadconnectsync-best-practices-changing-default-configuration.md), takže se zobrazí dotaz, jaké jsou vaše záměry. V takovém případě chcete pouze tooview hello pravidlo. Vyberte **ne**.

![Synchronizační pravidla upozornění](./media/active-directory-aadconnectsync-understanding-default-configuration/warningeditrule.png)

Synchronizační pravidlo obsahuje čtyři části konfigurace: popis, rozsahu filtru, spojení pravidel a transformace.

#### <a name="description"></a>Popis
Hello první část obsahuje základní informace, jako je název a popis.

![Popis kartě editor synchronizované pravidla ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruledescription.png)

Můžete také najít informace o tom, které připojený systém, který se vztahuje na připojený systém se toto pravidlo vztahuje, který typ v hello objektu a hello typ objekt úložiště metaverse. Typ objektu úložiště metaverse Hello je vždy osoba bez ohledu na to, po hello typ zdrojového objektu uživatele, iNetOrgPerson nebo požádejte. Typ objektu úložiště metaverse Hello nikdy změnit tak, že je vytvořen jako obecného typu. Hello typu propojení můžete nastavit, tooJoin, StickyJoin nebo zřídit. Toto nastavení spolupracuje s hello připojení pravidla části a věnuje se později.

Zobrazí se také, že toto synchronizační pravidlo se používá pro synchronizaci hesel. Pokud je uživatel v oboru pro toto synchronizační pravidlo, hello heslo synchronizovaných z místní toocloud (za předpokladu, že jste povolili funkce Synchronizace hesla hello).

#### <a name="scoping-filter"></a>Filtr vytváření oboru
Hello části filtru pro vytváření oboru je použité tooconfigure, pokud by se měly používat synchronizační pravidlo. Vzhledem k tomu, že název hello hello synchronizační pravidlo, které se díváte uvádí, že by měl použít pouze pro povolené uživatele, hello obor se nakonfiguruje tak atribut hello AD **userAccountControl** nesmí mít hello 2 sadu bitů. Je-li hello synchronizační modul Vyhledá uživatele ve službě AD, platí toto synchronizační pravidlo, když **userAccountControl** nastavena toohello desítkovou hodnotu 512 (povoleného normální uživatele). Pokud má uživatel hello nevztahuje pravidlo hello **userAccountControl** nastavit too514 (zakázaný normální uživatel).

![Karta oboru v editoru pravidel synchronizace ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilter.png)

Hello oboru filtru má skupin a věty, které mohou být použity. Všechny klauzule uvnitř skupiny musí být splněny pro tooapply synchronizační pravidlo. Když jsou definovány více skupin, musí být alespoň jednu skupinu splněny pro pravidlo tooapply hello. To znamená logické nebo vyhodnocena mezi skupinami a logickou a vyhodnotí uvnitř skupiny. Příklad této konfigurace můžete nalézt v hello odchozí pravidlo synchronizace **Out tooAAD – připojení skupiny**. Existuje několik skupin synchronizace filtru, například jeden pro skupiny zabezpečení (`securityEnabled EQUAL True`) a jeden pro distribuční skupiny (`securityEnabled EQUAL False`).

![Karta oboru v editoru pravidel synchronizace ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilterout.png)

Toto pravidlo je použité toodefine, které skupiny by se měly zřízené tooAzure AD. Distribuční skupiny musí být povolena poštovní toobe synchronizovat se službou Azure AD, ale pro skupiny zabezpečení e-mailu se nevyžaduje.

#### <a name="join-rules"></a>Připojení k pravidla
je třetí oddíl Hello použité tooconfigure vztahy mezi objekty v prostoru konektoru hello tooobjects v úložišti metaverse hello. Hello pravidlo, které jste dříve prohlédli nemá žádnou konfiguraci pro připojení k pravidla, tak místo toho jsou probíhající toolook v **v ze služby Active Directory – připojení uživatele k**.

![Karta pravidla připojení v editoru pravidel synchronizace ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulejoinrules.png)

obsah Hello hello spojení pravidla závisí na hello odpovídající možnosti vybrané v hello Průvodce instalací. Pro příchozí pravidlo vyhodnocení hello začíná na objekt v prostoru konektoru hello zdroje a každou skupinu v pravidlech spojení hello je vyhodnoceny v pořadí. Pokud ke zdrojovému objektu je vyhodnotí toomatch právě jeden objekt v hello metaverse pomocí jednoho pravidla hello spojení, objekty hello připojeni. Pokud byly vyhodnoceny všechna pravidla a není nalezena žádná shoda, se používá hello typu propojení na stránce popis hello. Pokud tuto konfiguraci je nastaven příliš**zřídit**, pak se vytvoří nový objekt v hello cíl, hello metaverse. tooprovision nové metaverse toohello objektu je také označován jako příliš**projektu** metaverse toohello k objektu.

Hello spojení pravidla se vyhodnocují jenom jednou. Když jsou připojena objekt prostor konektoru a objektu úložiště metaverse, zůstanou připojené k, tak dlouho, dokud hello rozsah hello synchronizační pravidlo je stále splněna.

Při vyhodnocování pravidel synchronizace, musí být pouze jeden synchronizační pravidlo s spojení pravidla definovaná v oboru. Pokud se více pravidel synchronizace s pravidly spojení pro jeden objekt, je vržena chyba. Z tohoto důvodu hello osvědčeným postupem je toohave při více pravidel synchronizace jsou v oboru pro objekt definovaný pouze jeden synchronizační pravidlo s spojení. V části Konfigurace out-of-box hello synchronizace Azure AD Connect, můžete tato pravidla nalezen prohlížením hello název a najít ty slovem hello **připojení** hello konec názvu hello. Pravidlo synchronizace bez definováno žádné připojení k pravidel platí toky atributů hello při dalším synchronizačním pravidle propojeny hello objekty nebo zřídit nový objekt v cílové hello.

Pokud si prohlédnete hello obrázku výše vidíte daného pravidla hello se pokouší toojoin **objectSID** s **msExchMasterAccountSid** (Exchange) a **msRTCSIP-OriginatorSid** () Lync), který je co Očekáváme, že v účtu prostředků topologie doménové struktury. Najde hello stejné pravidlo na všechny doménové struktury. Hello předpokladem je, že každá doménová struktura může být účtu nebo prostředek doménové struktury. Tato konfigurace funguje taky Pokud máte účty, které za provozu v jedné doménové struktury a nemají toobe připojený.

#### <a name="transformations"></a>Transformace
Hello transformačním oddílu definuje všechny toky atributů, které použít toohello cílový objekt, když jsou připojeny hello objekty a hello obor filtru je splněné. Návratem toohello **v ze služby Active Directory – uživatel AccountEnabled** synchronizační pravidlo, najít hello transformace následující:

![Transformace kartě editor synchronizované pravidla ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruletransformations.png)

tooput v kontextu, v nasazení doménové struktury prostředků účtu, tato konfigurace je očekávané toofind aktivovaný účet v doménové struktuře účtu hello a deaktivovaného účtu v prostředku hello doménové struktury s nastavením Exchange a Lync. Hello se díváte synchronizační pravidlo obsahuje atributy hello požadované pro přihlášení a tyto atributy musí procházet z doménové struktury hello je aktivovaný účet. Všechny tyto toky atributů jsou připravili v jedné synchronizační pravidlo.

Transformace může mít různé typy: Konstanta, přímo a výraz.

* Konstantní toku vždy toky pevně zakódované hodnotu. V případě hello výše vždy nastaví hodnotu hello **True** do atribut úložiště metaverse hello s názvem **accountEnabled**.
* Přímé toku vždy toků hello hodnotu atributu hello v hello zdroj toohello atribut target jako-je.
* Typ toku třetí Hello je výraz a umožňuje pro pokročilejší konfigurace.

výraz jazyka Hello je VBA (Visual Basic pro aplikace), takže uživatelé s prostředí systému Microsoft Office nebo VBScript rozpozná hello formátu. Atributy jsou uzavřené v hranatých závorkách [attributeName]. Názvy atributů a funkce jsou malá a velká písmena, ale hello editoru pravidel synchronizace vyhodnotí výrazy hello a zobrazit upozornění, pokud hello výraz není platný. Všechny výrazy jsou vyjádřeny na jeden řádek s vnořené funkce. napájení hello tooshow hello konfigurace jazyka, zde je hello toku pwdLastSet, ale s další komentáři vložit:

```
// If-then-else
IIF(
// (hello evaluation for IIF) Is hello attribute pwdLastSet present in AD?
IsPresent([pwdLastSet]),
// (hello True part of IIF) If it is, then from right tooleft, convert hello AD time format tooa .Net datetime, change it toohello time format used by Azure AD, and finally convert it tooa string.
CStr(FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")),
// (hello False part of IIF) Nothing toocontribute
NULL
)
```

V tématu [Principy deklarativní zřizování výrazy](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) Další informace o jazyce hello výraz pro toky atributů.

### <a name="precedence"></a>Priorita
Jste nyní prohlédli některé jednotlivé synchronizační pravidla, ale pravidla hello spolupracují v konfiguraci hello. V některých případech je hodnota atributu podílí z více toohello pravidla synchronizace stejný atribut target. V takovém případě přednost atributu je použité toodetermine wins, který atribut. Jako příklad podívejte se na hello atribut sourceAnchor. Tento atribut je důležité atribut toobe možné toosign v tooAzure AD. Můžete najít tok atributů pro tento atribut v dvě různá pravidla synchronizace **v ze služby Active Directory – uživatel AccountEnabled** a **v ze služby Active Directory – běžné uživatele**. Z důvodu tooSynchronization prioritu pravidel hello atribut sourceAnchor je podílí z doménové struktury hello s aktivovaný účet nejprve po několik objektu úložiště metaverse připojený k toohello objekty. Pokud nejsou povolené účty, pak hello synchronizační modul používá hello catch-all synchronizační pravidlo **v ze služby Active Directory – běžné uživatele**. Tato konfigurace zajistí, že i pro účty, kteří jsou zakázáni, stále existuje sourceAnchor.

![Příchozí pravidla synchronizace](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

Hello prioritu pro synchronizační pravidla se nastavuje v skupiny pomocí Průvodce instalací hello. U všech pravidel ve skupině mít hello stejný název, ale jsou připojené toodifferent připojení adresáře. Průvodce instalací Hello dává hello pravidlo **v ze služby Active Directory – připojení uživatele k** nejvyšší prioritu a iterace, přes všechny připojené adresáře AD. Poté pokračuje v hello další skupiny pravidel v předdefinovaném pořadí. Uvnitř skupiny se přidají hello pravidla v pořadí hello hello konektory byly přidány v Průvodci hello. Pokud jiný konektor přidána prostřednictvím Průvodce hello, přeuspořádají hello synchronizační pravidla a pravidla hello nový konektor jsou vloženy poslední v každé skupině.

### <a name="putting-it-all-together"></a>Třeba umisťovat všechny společně
Nyní víme, dostatek o synchronizační pravidla toobe možné toounderstand fungování hello konfigurace s hello různá pravidla synchronizace. Pokud se podíváte na uživatele a hello atributy, které jsou podílí toohello metaverse, hello pravidla se používají v hello následující pořadí:

| Name (Název) | Komentář |
|:--- |:--- |
| V ze služby Active Directory – připojení uživatele |Pravidlo pro připojení objekty konektoru místa s úložiště metaverse. |
| V ze služby Active Directory – povolené uživatelské účty |Atributy potřebné pro přihlášení tooAzure AD a Office 365. Chceme, aby tyto atributy z účtu hello povolena. |
| V ze služby Active Directory – běžné uživatele ze systému Exchange |V seznamu pro globální adresa hello byly nalezeny atributy. Předpokládáme, že v doménové struktuře hello kde našli jsme poštovní schránky hello uživatele je nejvhodnější kvality dat. hello. |
| V ze služby Active Directory – běžné uživatele |V seznamu pro globální adresa hello byly nalezeny atributy. V případě, že jsme nenašli poštovní schránku, můžete přispívat libovolného připojený k objektu hodnota atributu hello. |
| V ze služby Active Directory – Exchange uživatele |Existuje pouze pokud byl zjištěn Exchange. Ho toky všechny atributy Exchange infrastruktury. |
| V ze služby Active Directory – uživatele aplikace Lync |Existuje pouze pokud byla zjištěna aplikace Lync. Ho toky všechny atributy Lync infrastruktury. |

## <a name="next-steps"></a>Další kroky
* Další informace o hello konfigurační model v [Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Další informace o hello výrazu jazyka v [Principy deklarativní zřizování výrazy](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
* Pokračujte ve čtení fungování hello out-of-box konfigurace [Principy uživatelů a kontaktů](active-directory-aadconnectsync-understanding-users-and-contacts.md)
* V tématu Jak toomake praktická změnit pomocí deklarativní zřizování v [jak toomake toohello změnu výchozí konfigurace](active-directory-aadconnectsync-change-the-configuration.md).

**Témata s přehledem**

* [Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)

