---
title: aaaLotus Domino konektor | Microsoft Docs
description: "Tento článek popisuje, jak tooconfigure Microsoft Lotus Domino Connector."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: e07fd469-d862-470f-a3c6-3ed2a8d745bf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: affef1fec91eb39f7e91ec274fdd1b3a9c4a32fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="lotus-domino-connector-technical-reference"></a>Technické reference Domino konektoru Lotus
Tento článek popisuje hello konektoru Lotus Domino. Hello článek se týká toohello následující produkty:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manageru 2010 R2 (FIM2010R2)
  * Musíte použít opravu hotfix 4.1.3671.0 nebo novější [KB3092178](https://support.microsoft.com/kb/3092178).

Pro MIM2016 a FIM2010R2, je k dispozici ke stažení z hello hello konektor [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-hello-lotus-domino-connector"></a>Přehled hello konektoru Lotus Domino
Hello konektoru Lotus Domino umožňuje toointegrate hello synchronizační služba společnosti IBM Lotus Domino serveru.

Z hlediska vysoké úrovně podporuje hello aktuální verzi konektoru hello hello následující funkce:

| Funkce | Podpora |
| --- | --- |
| Připojeného zdroje dat |Server: <li>Lotus Domino 8.5.x</li><li>Lotus Domino 9.x</li>Klienta:<li>Lotus Domino 8.5.x</li><li>Lotus Notes 9.x</li> |
| Scénáře |<li>Správa životního cyklu objektu</li><li>Správa skupin</li><li>Správa hesel</li> |
| Operace |<li>Úplné a rozdílový Import</li><li>Export</li><li>Nastavit a změnit heslo na heslo protokolu HTTP</li> |
| Schéma |<li>Uživatel (uživatel Roaming, obraťte se na (osoby s žádný certifikát))</li><li>Skupina</li><li>Prostředek (prostředků, místnosti, Online schůzky)</li><li>E-mailu v databázi</li><li>Dynamické zjišťování atributy pro podporované objekty</li> |

konektoru Lotus Domino Hello používá hello Lotus Notes klienta toocommunicate s Lotus Domino serveru. V důsledku této závislosti musí být nainstalován podporovaný klient Lotus Notes na serveru pro synchronizaci hello. Hello komunikace mezi hello klient a hello server se implementuje pomocí rozhraní hello Lotus Notes .NET, zprostředkovatel komunikace s objekty (Interop.domino.dll). Toto rozhraní umožňuje hello komunikaci mezi hello Microsoft.NET platformy a Lotus Notes klienta a podporuje přístup tooLotus Domino dokumentů a zobrazení. Pro Rozdílový import je také možné, že nativní rozhraní hello C++ slouží (v závislosti na metodě import delta hello vybrané).

### <a name="prerequisites"></a>Požadavky
Než použijete hello konektor, zkontrolujte, zda že máte následující požadavky na serveru pro synchronizaci hello hello:

* 4.5.2 rozhraní Microsoft .NET Framework nebo novější
* klient Lotus Notes Hello musí být nainstalován na serveru pro synchronizaci
* Hello konektoru Lotus Domino vyžaduje hello výchozí Lotus Domino LDAP schématu databáze (schema.nsf) toobe nacházejí na serveru Domino Directory hello. Pokud není přítomen, můžete ho nainstalovat spuštění nebo restartování hello služba využívající protokol LDAP na hello Domino server.

### <a name="connected-data-source-permissions"></a>Oprávnění připojených zdrojů dat
tooperform hello podporují se všechny úlohy v konektoru Lotus Domino, musíte být členem následujících skupin:

* Úplný přístup správce
* Správci
* Správce databáze

Hello následující tabulka uvádí hello oprávnění, které jsou požadovány pro každou operaci:

| Operace | Přístupová práva |
| --- | --- |
| Import |<li>Veřejné dokumenty pro čtení</li><li> Správce s úplnými oprávněními přístupu (Pokud jste členem skupiny administrators úplný přístup, je automaticky používat hello platný přístup tooin seznamu ACL.)</li> |
| Export a nastavit heslo. |Efektivní přístup: <li>Vytváření dokumentů</li><li>Odstranit dokumenty</li><li>Veřejné dokumenty pro čtení</li><li>Zápis veřejné dokumenty</li><li>Replikovat nebo kopírování dokumentů</li>Pro operace exportu musíte taky hello následující role: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li> |

### <a name="direct-operations-and-adminp"></a>Přímé operace a AdminP
Operace toohello Domino directory přejděte přímo nebo prostřednictvím hello AdminP zpracovat. Hello následujících tabulkách jsou uvedeny všechny podporované objekty, operace, a pokud jsou k dispozici, hello související metoda implementace:

**Primární adresáře**

| Objekt | Vytvořit | Aktualizace | Odstranění |
| --- | --- | --- | --- |
| Osoba |AdminP |Přímé |AdminP |
| Skupina |AdminP |Přímé |AdminP |
| MailInDB |Přímé |Přímé |Přímé |
| Prostředek |AdminP |Přímé |AdminP |

**Sekundární adresáře**

| Objekt | Vytvořit | Aktualizace | Odstranění |
| --- | --- | --- | --- |
| Osoba |Není k dispozici |Přímé |Přímé |
| Skupina |Přímé |Přímé |Přímé |
| MailInDB |Přímé |Přímé |Přímé |
| Prostředek |Není k dispozici |Není dostupné. |Není k dispozici |

Když je vytvořen prostředek, vytvoří se poznámky k dokumentu. Podobně při odstranění prostředku je odstranit hello poznámky k dokumentu.

### <a name="ports-and-protocols"></a>Porty a protokoly
IBM Lotus Notes klienta a servery Domino komunikovat pomocí poznámky vzdálené procedury volání (NRPC) kde NRPC musí používat protokol TCP/IP. Hello výchozí číslo portu je 1352, ale může změnit správce Domino hello.

### <a name="not-supported"></a>Nepodporuje se
hello aktuální verzi konektoru Lotus Domino hello nepodporuje Hello následující operace:

* Přesuňte poštovní schránky mezi servery.

## <a name="create-a-new-connector"></a>Vytvořit nový konektor
### <a name="client-software-installation-and-configuration"></a>Instalace klientského softwaru a konfigurace
Lotus Notes musí být nainstalován na hello server **před** hello konektor nainstalovaný.

Když nainstalujete, ujistěte se, můžete udělat **jeden uživatel nainstalovat**. Hello výchozí **nainstalovat více uživateli** nefunguje.  
![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

Na stránce funkce hello instalovat pouze hello požadované funkce Lotus Notes a **jeden pro přihlašování klientů**. Je požadováno pro možnost toolog toobe hello konektoru na serveru Domino toohello jednom přihlášení.  
![Notes2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**Poznámka:** počáteční Lotus Notes po s uživatelem, který je umístěný na hello stejný server jako hello účet použít jako účet služby konektoru hello. Na serveru hello způsobují, že tooclose hello Lotus Notes klienta. Nelze používat v hello stejný čas hello konektor pokusí tooconnect toohello Domino serveru.

### <a name="create-connector"></a>Vytvořit konektor
tooCreate konektoru Lotus Domino v **synchronizační služba** vyberte **agenta pro správu** a **vytvořit**. Vyberte hello **Lotus Domino (Microsoft)** konektor.  
![CreateConnector](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

Pokud vaše verze služby synchronizace nabízí možnost tooconfigure hello **architektura**, zkontrolujte hello konektor je nastavený tooits výchozí hodnotu toorun **proces**.

### <a name="connectivity"></a>Připojení
Na stránce hello připojení musíte zadat název serveru Lotus Domino hello a zadejte přihlašovací údaje hello.  
![Připojení](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

Hello vlastnost Domino serveru podporuje dva formáty pro název serveru hello:

* serverName
* ServerName/DirectoryName

Hello **ServerName/DirectoryName** formát je hello upřednostňované formát pro tento atribut, protože poskytuje rychlejší reakci při hello konektor kontakty hello Domino serveru.

Hello, pokud je uložený soubor ID uživatele v databázi konfigurace hello hello synchronizační služba.

Pro **rozdílový Import** máte tyto možnosti:

* **Žádný**. Hello konektor neprovádí žádné Rozdílový import.
* **Přidat nebo aktualizovat**. Hello konektor nemá Rozdílový import přidejte a operace aktualizace. Pro odstranění **úplný Import** operace je požadováno. Tato operace je pomocí zprostředkovatele komunikace .net hello.
* **Přidání, aktualizace nebo odstranění**. Hello konektor nemá Rozdílový import přidat, aktualizovat a odstranit operace. Tato operace je pomocí nativních rozhraní C++ hello.

V **možnosti schématu** máte hello následující možnosti:

* **Výchozí schéma**. Hello konektor zjistí hello schématu ze serveru Domino hello. Tato volba je výchozí možnost hello.
* **Schéma DSML**. Použít jen v případě, že hello Domino server nevystavuje hello schématu. Potom můžete vytvořit soubor DSML se schématem hello a importujte ho. Další informace o DSML najdete v tématu [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

Po kliknutí na tlačítko Další, je ověřeno hello ID uživatele a heslo parametry konfigurace.

### <a name="global-parameters"></a>Globální parametry
Na stránce hello globální parametry nakonfigurujte hello časové pásmo a hello import a export – možnost operaci.  
![Globální parametry](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

Hello **časové pásmo serveru Domino** parametr definuje umístění hello Domino serveru.

Tato možnost konfigurace je požadovaná toosupport **Rozdílový import** operations protože umožní hello synchronizační služba zjištění změn mezi posledních dvou importy hello.

>[!Note]
Počínaje hello března 2017 aktualizace hello globální parametry obrazovky zahrnuje hello možnost toodelete hello uživatele e-mailu databáze při odstranění hello uživatele.

![Odstranit poštovní schránky uživatele](./media/active-directory-aadconnectsync-connector-domino/AdminP.png)

#### <a name="import-settings-method"></a>Import nastavení, – metoda
Hello **provést úplné Import pomocí** tyto možnosti:

* Search
* Zobrazení (doporučeno)

**Hledání** je pomocí indexování v Domino, ale je běžné, že hello indexy se neaktualizují v reálném čase a hello daty vrácenými ze serveru hello vždy není správná. Tato možnost pro systém s mnoho změn, obvykle nebude fungovat správně a poskytuje false odstraní v některých situacích. Ale **vyhledávání** je rychlejší než **zobrazení**.

**Zobrazení** je hello doporučená možnost vzhledem k tomu, že poskytuje správný stav hello data. Je něco pomalejší než vyhledávání.

#### <a name="creation-of-virtual-contact-objects"></a>Vytvoření virtuální kontaktní objektů
Hello **povolení vytvoření \_objekt kontakt** tyto možnosti:

* Žádný
* Hodnoty – referenční dokumentace
* Referenční dokumentace a – referenční dokumentace hodnoty

V Domino může atributy typu odkaz obsahovat mnoho různých formátech tooreference jiné objekty. různé varianty možné toorepresent toobe hello konektor implementuje \_obraťte se na objekty, také známé jako **virtuální kontakty** (VC). Tyto objekty jsou vytvořené tak bylo možné připojit tooexisting MV objekty nebo projekci jako nové objekty. Tímto způsobem je možné zachovat atribut odkazy.

Povolením tohoto nastavení a v případě hello obsah atribut typu odkaz není rozlišující název formátu, \_je vytvořen objekt kontaktu. Atribut člen skupiny může například obsahovat adresy SMTP. Je také možné toohave shortName a další atributy v atributy typu odkaz. V tomto scénáři vyberte **Non-Reference hodnoty**. Tato konfigurace je hello nejběžnější nastavení pro implementace Domino.

Po Lotus Domino nakonfigurované toohave samostatné adresáře s různými názvy rozlišující představující hello stejný objekt, vytvořit možné tooalso \_obraťte se na objekty pro všechny referenční hodnoty, které se nacházejí v adresáři. V tomto scénáři vyberte hello **odkaz a hodnoty Non-Reference** možnost.

Pokud máte více hodnot v atributu hello **FullName** v Domino, pak budete také chtít tooenable hello vytvoření virtuální kontaktů, odkazy, lze vyřešit. Tento atribut například může mít více hodnot po manželství nebo rozvodu. Zaškrtněte políčko hello **povolit... Úplný název má více hodnot** pro tento scénář.

Sloučením správné atributy hello hello \_obraťte se na objekty by být připojené k toohello MV objekt.

Tyto objekty mají VC =\_tootheir DN přidat kontakt.

#### <a name="import-settings-conflict-object"></a>Importovat nastavení, konfliktu objektu
**Vyloučit konflikt objektu**

V velké Domino implementaci je možné, že mají více objektů hello stejné DN kvůli tooreplication problémy. V těchto případech by hello konektor zobrazí dva objekty s jinou UniversalIDs ale stejné rozlišující název. Tento konflikt by způsobilo objekt přechodný vytváří v prostoru konektoru hello. Hello konektoru můžete ignorovat hello objekty, které byly vybrány v Domino jako obětí replikace. Hello doporučuje tookeep toto políčko zaškrtnuto.

#### <a name="export-settings"></a>Export nastavení
Pokud hello možnost **použít AdminP aktualizace odkazů** nezaškrtnuté, pak export atributy typu odkaz, jako je například člena, je přímé volání a nepoužívá hello AdminP proces. Tuto možnost použijte pouze v případě AdminP nebyl nakonfigurovaný toomaintain referenční integrity.

#### <a name="routing-information"></a>Informace o směrování
V Domino je možné, že atribut typu odkaz obsahuje informace o směrování vložených jako přípona toohello rozlišující název. Například může obsahovat atribut hello členství ve skupině **CN =example/organization@ABC**. přípona Hello @ABC je hello informace o směrování. informace o směrování Hello je používán Domino toosend e-mailů toohello správné Domino systém, který může být systém v jiné organizaci. V poli hello informace o směrování můžete zadat hello směrování přípon používaných v rámci organizace hello v oboru hello konektor. Pokud jako příponu jednu z těchto hodnot naleznete v atribut typu odkaz, odeberou se z hello referenční informace o směrování hello. Pokud hello směrování příponu na hodnotu odkazu nemůže být odpovídající tooone tyto hodnoty zadané, \_je vytvořen objekt kontaktu. Tyto \_obraťte se na objekty jsou vytvořeny pomocí **RO = @<RoutingSuffix>**  vloženy do hello rozlišující název. Pro tyto \_obraťte se na objekty hello následující atributy jsou rovněž přidány tooallow připojení tooa skutečné objektu v případě potřeby: \_routingName, \_kontakt, \_displayName a UniversalID.

#### <a name="additional-address-books"></a>Další adresáře
Pokud nemáte **directory pomoc** nainstalovaná, který poskytuje název hello sekundární adresářů a pak tyto adresáře lze zadat ručně.

#### <a name="multivalued-transformation"></a>S více hodnotami transformace
Počet atributů v Lotus Domino jsou více hodnotami. Hello odpovídající úložiště metaverse atributy jsou obvykle jednu hodnot. Nakonfigurováním hello importu a možností operace exportu hello povolíte hello konektor toohelp s hello vyžaduje překlad hello vliv atributů.

**Export**  
možnost operace exportu Hello podporuje dva způsoby:

* Append – položky
* Nahraďte položku

**Nahraďte položku** – Pokud vyberte tuto možnost, vždy konektor hello odebrat hello aktuální hodnoty atributu hello v Domino a nahradíte je hello zadané hodnoty. Hello zadaný s hodnotou může být buď jednu nebo více hodnot.

Příklad: atribut pomocníka hello objektu osoba má hello následující hodnoty:

* CN = Gregu Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

Pokud nového Pomocníka s názvem **David Alexander** je přiřazen objekt toothis osoba, výsledkem hello je:

* CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf

**Připojit položky** – Pokud vyberete tuto možnost, konektor hello uchovává existující hodnoty hello na atribut hello v Domino a vložení nové hodnoty hello horní části seznamu dat hello.

Příklad: atribut pomocníka hello objektu osoba má hello následující hodnoty:

* CN = Gregu Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

Pokud nového Pomocníka s názvem **David Alexander** je přiřazen objekt toothis osoba, výsledkem hello je:

* CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Gregu Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

**Import**  
Hello možnost operace importu podporuje dva způsoby:

* Výchozí
* S více hodnotami tooSingle hodnota

**Výchozí** – Pokud vyberete hello výchozí možnost, všechny hodnoty všech hello atributy jsou importovány.

**S více hodnotami tooSingle hodnotu** – Pokud vyberete tuto možnost, více hodnot atributů je převeden do atribut jednou hodnotou. Pokud existuje více než jednu hodnotu, bude použita hodnota hello hello nahoře (Tato hodnota se obvykle taky hello nejnovější).

Příklad: atribut pomocníka hello objektu osoba má hello následující hodnoty:

* CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Gregu Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

Hello nejnovější aktualizace toothis atribut je **David Alexander**. Protože hello možnost operace importu je nastavena tooMultivalued tooSingle hodnotu, konektor importuje jenom **David Alexander** do prostoru konektoru hello.

více hodnot atributů tooconvert Hello logiku do jednohodnotové atributy nevztahuje atributu člen skupiny toohello a toohello osoba úplný název atributu.

Je také možné tooconfigure import a export pravidel transformace pro vícehodnotové atributy každý atribut, jako pravidlo výjimky toohello globální. tooconfigure, tato možnost, zadejte [objecttype]. [attributename] v hello **importovat seznam atributů vyloučení** a **exportovat seznam atributů vyloučení** textových polí. Například pokud zadáte Person.Assistant a globální příznak hello je nastaven tooimport všechny hodnoty, první hodnota pouze hello je importované pro pomocníka hello.

#### <a name="certifiers"></a>Udělení licence certifikátorům
Všechny organizace nebo organizační jednotky jsou uvedeny konektorem hello. toobe možné tooexport osoba objekty toohello primární, certifier s jeho heslo je nutný adresář.

Pokud mají všechny udělení licence certifikátorům hello stejné heslo, hello **heslo pro všechny Certifers** lze použít. Potom můžete zadat heslo hello a určovat pouze hello certifier souboru.

Pokud jenom importujete, pak jste toospecify žádné udělení licence certifikátorům.

### <a name="configure-provisioning-hierarchy"></a>Konfigurace hierarchie zřizování
Při konfiguraci konektoru Lotus Domino hello, přeskočte tuto stránku dialogové okno. konektoru Lotus Domino Hello nepodporuje hierarchie zřizování.  
![Zřizování hierarchie](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfigurace oddílů a hierarchií
Když konfigurujete oddílů a hierarchií, je nutné vybrat hello primární adresáře názvem NAB=names.nsf. Kromě toho primární adresáře toohello, můžete vybrat sekundární adresáře Pokud existují.  
![Oddíly](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### <a name="select-attributes"></a>Vyberte atributy
Když konfigurujete vaší atributy, je nutné vybrat všechny atributy, které mají předponu  **\_MMS\_**. Tyto atributy jsou požadovaná při zřizování nové objekty tooLotus Domino

![Atributy](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Správa životního cyklu objektu
Tato část obsahuje přehled různých objektů Domino hello.

### <a name="person-objects"></a>Objekty osoba
objekt osoba Hello představuje uživatele v organizaci a organizační jednotky. Kromě toho toohello výchozí atributy hello Domino správce můžete přidat vlastní atributy tooa osoba objektu. Minimálně musí obsahovat objekt osoba všechny povinné atributy. Úplný seznam povinné atributy, najdete v části [Lotus Notes vlastnosti](#lotus-notes-properties). musí být splněné tooregister objekt osoba, hello následující požadavky:

* Hello adresáře (names.nsf) musí být definována a měla by být hello primární adresáře.
* Musíte mít hello O/OU certifier Id a hello heslo tooregister určitého uživatele v hello organizace nebo organizační jednotka.
* Je nutné nastavit určitou sadu Lotus Notes vlastnosti pro objekt osoby. Tyto vlastnosti se používají pro zřizování hello osoba objektu. Další informace najdete v tématu hello části s názvem [Lotus Notes vlastnosti](#lotus-notes-properties) dále v tomto dokumentu.
* Hello počáteční HTTP heslo pro osoby je atribut a sadu během zřizování.
* Hello osoba objekt musí mít jednu z hello následující tři podporované typy:
  1. Normální uživatele, který má soubor e-mailu a soubor id uživatele
  2. Roaming uživatele (obvykle uživatel, který zahrnuje všechny roamingu soubory databáze)
  3. Kontakty (uživatele s žádný soubor id)

Osob (s výjimkou kontakty) je možné seskupit do US uživatelům a uživatelům mezinárodní další, podle definice hello hodnotu hello \_MMS\_IDRegType vlastnost. Tyto osoby použít hello poznámky klienta tooaccess Lotus Domino servery, mají Id poznámky a osoby dokumentu. Pokud používají poznámky k e-mailu, pak mají také soubor e-mailu. Hello uživatel musí být registrovaný toobecome aktivní. Další informace naleznete v tématu:

* [Nastavení uživatelů poznámky](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
* [Registrace uživatele](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
* [Správa uživatelů](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
* [Přejmenování uživatele](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

Všechny tyto operace jsou prováděny v Lotus Domino a pak importovat do hello synchronizační služba.

### <a name="resources-and-rooms"></a>Prostředky a místnosti
Prostředek je databáze v Lotus Domino jiného typu. Prostředky, může být konferenčních místností s různými typy zařízení, například projektory. Existují podtypů nepodporuje konektoru Lotus Domino prostředky, které jsou definovány hello atribut typ prostředku:

| Typ prostředku | Atribut typ prostředku |
| --- | --- |
| Místnosti |1 |
| Prostředek (Další) |2 |
| Online schůzky |3 |

U hello toowork typu prostředku do objektu se vyžaduje hello následující:

* Rezervace databáze prostředků by již měla existovat na serveru Domino hello připojení
* Hello lokality je již definována pro hello prostředků

databáze prostředků rezervace Hello obsahuje tři typy dokumentů:

* Profil společnosti
* Prostředek
* Rezervace

Další informace o nastavení databáze prostředků rezervace, najdete v části [nastavení databáze prostředků rezervace hello](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**Vytvářet, aktualizovat a odstraňovat prostředky**  
Hello Create, Update a Delete operace jsou prováděny konektoru Lotus Domino hello v databázi prostředků rezervace hello. Prostředky jsou vytvořené jako dokumenty v Names.nsf (tedy hello primární adresář). Další informace o úpravy a odstranění prostředků najdete v tématu [úpravy a odstraňování dokumentů prostředků](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**Import a Export operace pro prostředky**  
Hello prostředků může být importované tooand exportovaný z hello synchronizační služby, jako libovolný jiný typ objektu. Vyberte hello typ objektu jako prostředek během konfigurace. Pro úspěšné exportu měli byste mít podrobnosti pro typ prostředku, konferenční databáze a název lokality.

### <a name="mail-in-databases"></a>E-mailu v databázích
E-mailu v databázi je databáze, která je navrženou tooreceive e-mailů. Je Lotus poštovní schránky, není přidružen k žádné konkrétní uživatelský účet Lotus Domino (to znamená, nemá svůj vlastní soubor ID a heslo). E-mailu v databázi má jedinečný UserID (dále jen "krátkého názvu") přidružené k jeho a vlastní e-mailovou adresu.

Pokud je zapotřebí pro samostatné poštovní schránku s vlastní e-mailovou adresu, který může být sdílen mezi různými uživateli (například group@contoso.com), bude vytvořena databáze v e-mailu. Hello přístup toothis poštovní schránky se řídí prostřednictvím jeho řízení přístupu seznamu (ACL), který obsahuje názvy hello hello poznámky uživatelů, kteří jsou povoleny tooopen hello poštovní schránky.

Seznam atributů hello vyžaduje, najdete v části hello s názvem [povinné atributy](#mandatory-attributes) dále v tomto článku.

Pokud je databáze navrženou tooreceive e-mailu, vytvoří se v Lotus Domino dokumentu e-mailu v databázi. Tento dokument musí existovat v adresáři Domino všech serverů, které je uložená kopie databáze hello. Podrobný popis vytvoření dokumentu v e-mailu v databázi, naleznete v části [vytváření e-mailu v databázi dokumentů](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html).

Před vytvořením e-mailu v databázi, databázi hello by již měla existovat (měla být vytvořená Lotus správce) na serveru Domino hello.

### <a name="group-management"></a>Správa skupin
Podrobný přehled o hello Lotus Domino skupiny správy můžete získat z hello následující prostředky:

* [Použití skupin](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
* [Vytvoření skupiny](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
* [Vytvoření a úprava skupiny](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
* [Správa skupin](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
* [Přejmenování skupiny](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Správa hesel
Registrovaný uživatel Lotus Domino existují dva typy hesel:

1. Heslo uživatele (uložený v souboru User.id)
2. Internet nebo heslo protokolu HTTP

Hello Lotus Domino konektor podporuje pouze operace s heslem HTTP.

Správa hesel tooperform, měli byste povolit Správa hesel pro konektor hello v hello Návrhář správy agenta. Správa hesel tooenable, vyberte **povolit správu hesel** na hello **konfigurace rozšíření** dialogové okno stránky.  
![Konfigurace rozšíření](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

Podpora konektoru Lotus Domino Hello následující operace na Internetu heslo:

* Nastavit heslo: Nastavit heslo Nastaví nové heslo protokolu HTTP nebo Internet hello uživatele v Domino. Ve výchozím nastavení je také hello účet odemknout. Hello odemknutí příznak je vystaven na rozhraní WMI hello hello synchronizační modul.
* Změnit heslo: V tomto scénáři, uživatel může být vhodné toochange hello heslo nebo výzvami toochange heslo po určitou dobu. Pro operaci tootake místo (hello staré i nové heslo hello) jsou povinné. Po změně ve Lotus Domino aktualizaci hello nové heslo.

Další informace naleznete v tématu:

* [Pomocí funkce uzamčení Internet hello](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
* [Správa hesel Internetu](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Referenční informace
Tato část uvádí například popisy atribut a atribut požadavky pro konektoru Lotus Domino hello.

### <a name="lotus-notes-properties"></a>Vlastnosti aplikace Lotus Notes
Při zřizování osoba objekty tooyour Lotus Domino adresáře vaší objekty musí mít konkrétní sadu vlastností s konkrétními hodnotami naplněno. Tyto hodnoty jsou pouze potřebné pro vytvoření operací.

Hello následující tabulka uvádí tyto vlastnosti a obsahuje popis z nich.

| Vlastnost | Popis |
| --- | --- |
| \_MMS_AltFullName |Hello alternativní celé jméno uživatele. |
| \_MMS_AltFullNameLanguage |toobe jazyk Hello použit k určení alternativního úplný název hello uživatele. |
| \_MMS_CertDaysToExpire |Hello počet dní od hello aktuální datum před hello certifikátu vyprší. Pokud není zadáno, je výchozí datum hello dva roky od hello aktuální datum. |
| \_MMS_Certifier |Vlastnost, která obsahuje název organizační hierarchie hello hello certifier. Příklad: OU = OrganizationUnit, O = Org, C = země. |
| \_MMS_IDPath |Pokud je vlastnost hello prázdná, žádný soubor identifikace uživatele vytvoří místně na hello synchronizační Server. Pokud vlastnost hello obsahuje název souboru, soubor ID uživatele se vytvoří ve složce madata hello. Vlastnost Hello může také obsahovat úplnou cestu. |
| \_MMS_IDRegType |Osob můžou být klasifikované jako kontakty, nám uživatelů a mezinárodní uživatele. Hello následující tabulka uvádí možné hodnoty hello: <li>0 – kontakt</li><li>1 - US uživatele</li><li>2 – mezinárodní uživatele</li> |
| \_MMS_IDStoreType |Požadovaná vlastnost pro USA a mezinárodní uživatele. Vlastnost Hello obsahuje celočíselná hodnota, která určuje, zda text hello identifikace uživatele je uložena jako přílohu v hello poznámky k adresáři nebo v souboru e-mailu hello osoby. Pokud soubor ID uživatele hello přílohy v adresáři hello, volitelně vytvořením jako soubor s \_MMS_IDPath. <li>Prázdný - ID úložiště souborů v ID trezoru, žádný soubor identifikace (používá se pro kontakty).</li><li> 1 - přílohy v hello poznámky k adresáři. Hello \_MMS_Password vlastnost musí být nastavena pro uživatele identifikace soubory, které jsou přílohy</li><li>2 - ID úložiště v souboru e-mailu osoby. Hello \_MMS_UseAdminP musí být nastavena toofalse toolet hello e-mailu během hello osoba registrace nelze vytvořit soubor. Hello \_MMS_Password vlastnost musí být nastavena pro soubory identifikace uživatele.</li> |
| \_MMS_MailQuotaSizeLimit |Hello počet MB, které jsou povoleny pro databázový soubor hello e-mailu. |
| \_MMS_MailQuotaWarningThreshold |Hello počet MB, které jsou povoleny pro databázový soubor e-mailu hello před objeví se upozornění. |
| \_MMS_MailTemplateName |Hello e-mailové šablony soubor, který soubor e-mailu použité toocreate hello uživatele. Pokud je zadán šablonu, hello e-mailu soubor se vytvoří pomocí zadané šablony hello. Pokud je zadána žádná šablona, hello výchozí šablony soubor je použité toocreate hello. |
| \_MMS_OU |Volitelné vlastnosti, která je název organizační jednotky hello pod hello certifier. Tato vlastnost by měla být prázdná pro kontakty. |
| \_MMS_Password |Požadovaná vlastnost pro uživatele. Vlastnost Hello obsahuje hello heslo pro soubor identifikace hello hello objektu. |
| \_MMS_UseAdminP |Vlastnost musí být sada tootrue, pokud by měl vytvořit soubor e-mailu hello hello AdminP proces na serveru Domino hello (procesu exportu asynchronní toohello). Pokud je nastavena toofalse, hello e-mailu je vytvořen soubor s hello Domino uživatele (synchronní v procesu exportu hello). |

Pro uživatele se souborem přidružené identifikace, hello \_vlastnost MMS_Password musí obsahovat hodnotu. Pro přístup k e-mailu prostřednictvím hello Lotus Notes klienta musí obsahovat hodnotu hello MailServer a MailFile vlastnosti uživatele.

tooaccess e-mailu pomocí webového prohlížeče, hello následující vlastnosti musí obsahovat hodnoty:

* MailFile - povinnou vlastnost, která obsahuje hello cestu na serveru Lotus Domino hello, kde je uložen soubor hello e-mailu.
* MailServer - povinnou vlastnost, která obsahuje název hello serveru Lotus Domino hello. Tato hodnota je hello název toouse, když jste vytvořili hello Lotus e-mailu soubor na serveru Domino hello.
* HTTPPassword - volitelné vlastnosti, která obsahuje hello webové přístupové heslo pro objekt hello.

tooaccess hello Domino Server bez schopnosti e-mailu, hello vlastnost HTTPPassword musí obsahovat hodnotu. Hello vlastnost MailFile a hello MailServer vlastnost nesmí být prázdné.

S \_MMS_ IDStoreType = 2 (id úložiště v souboru e-mailu), hello MailSystem vlastnost NotesRegistrationclass nastavena tooREG_MAILSYSTEM_INOTES (3).

### <a name="mandatory-attributes"></a>Povinné atributy
konektoru Lotus Domino Hello především podporuje tyto typy objektů (typů dokumentů):

* Skupina
* E-mailu v databázi
* Osoba
* Obraťte se na (osoba s žádné certifier)
* Prostředek

Tato část uvádí hello atributy, které jsou povinné pro každý podporovaný objekt tooexport tooa Domino server.

| Typ objektu | Povinné atributy |
| --- | --- |
| Skupina |<li>ListName</li> |
| V hlavní databázi |<li>Úplný název</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li> |
| Osoba |<li>Příjmení</li><li>MailFile</li><li>ShortName</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li> |
| Obraťte se na (osoba s žádné certifier) |<li>\_MMS_IDRegType</li> |
| Prostředek |<li>Úplný název</li><li>ResourceType</li><li>ConfDB</li><li>ResourceCapacity</li><li>Lokality</li><li>displayName</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li> |

## <a name="common-issues-and-questions"></a>Běžné problémy a otázky
### <a name="schema-detection-does-not-work"></a>Schéma detekce nefunguje.
toobe možné toodetect hello schématu, je nutné, že soubor schema.nsf hello je na serveru Domino hello. Tento soubor se zobrazí, pouze pokud je na serveru hello nainstalovaná LDAP. Pokud není schéma hello rozpoznat, ověřte následující hello:

* schema.nsf Hello soubor se nachází v kořenové složce hello hello Domino serveru
* Hello uživatel má oprávnění toosee hello schema.nsf souboru.
* Vynuťte restartování serveru LDAP hello. Otevřete **Lotus Domino konzoly** a používat **říct ReloadSchema LDAP** příkaz tooreload hello schématu.

### <a name="not-all-secondary-address-books-are-visible"></a>Ne všechny sekundární adresáře jsou viditelné
Hello Domino konektor spoléhá na funkce hello **Directory pomoc** toobe možné toofind hello sekundární adresářů. Pokud chybí sekundární hello adresářů, ověřte, zda [Directory pomoc](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html) je povolená a nakonfigurovaná na hello Domino serveru.

### <a name="custom-attributes-in-domino"></a>Vlastní atributy v Domino
Existuje několik způsobů ve schématu hello tooextend Domino, takže se jeví jako vlastní atribut použití podle hello konektor.

**Způsob 1: Rozšíření schématu Lotus Domino**

1. Vytvořit kopii šablony Directory Domino {PUBNAMES. NTF} podle [tyto kroky](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (nesmí přizpůsobit hello výchozí IBM Lotus Domino adresář šablony):
2. Otevřete hello kopie Domino directory šablony {CONTOSO. Šablona NTF}, který byl vytvořen v Návrháři Domino a postupujte podle těchto kroků:
   * Klikněte na tlačítko sdílené elementy a rozbalte podformulářů
   * Klikněte dvakrát na ${ObjectName} InheritableSchema podformulář (kde {ObjectName} je název hello hello výchozí strukturální objekt třídy, například: osoba).
   * Název atributu hello chcete tooadd na schéma {MyPersonAtrribute} a odpovídající atributu toothat. Vytvořit pole vyberte hello **vytvořit** nabídce a potom vyberte **pole** z nabídky.
   * V poli přidané hello nastavte tak, že vyberete typ, styl, velikost, písma a dalších souvisejících parametrů na pole vlastnosti – okno jeho vlastnosti.
   * Atribut hello zachovat výchozí hodnota stejná jako hello název pro tento atribut (například pokud je název atributu MyPersonAttribute, ponechte hodnotu výchozí hello s hello stejný název).
   * Uložte hello ${ObjectName} InheritableSchema podformulář s aktualizovanými hodnotami.
3. Nahraďte hello Domino Directory šablony {PUBNAMES. NTF} s hello novou vlastní šablonu {CONTOSO. NTF} podle [tyto kroky](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Zavřete Správce Domino a otevřete konzolu Domino toorestart hello služba využívající protokol LDAP a tooReload hello schématu LDAP:
   * V konzole Domino, vložte příkaz hello pod **příkaz Domino** text selhala služba využívající protokol LDAP hello toorestart - [restartujte LDAP úloh](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html).
   * schéma LDAP tooreload použijte příkaz říct LDAP - říct ReloadSchema LDAP
5. Otevřete správce Domino a vyberte kartu toosee lidé a skupiny přidat atribut se odrazí v domino přidat osoby.
6. Otevřete Schema.nsf z **soubory** kartě a najdete v části Přidání atributu se projeví do třídy objektu dominoPerson LDAP.

**Způsob 2: Vytvoření auxClass pomocí vlastních atributů a přidružte k třída objektu hello**

1. Vytvořit kopii šablony Directory Domino {PUBNAMES. NTF} podle [tyto kroky](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (nikdy přizpůsobit hello výchozí IBM Lotus Domino adresář šablony):
2. Otevřete hello kopie Domino directory šablony {CONTOSO. Šablona NTF}, který byl vytvořen v Návrháři Domino.
3. V levém podokně hello vyberte sdíleného kódu a následně podformulářů.
4. Klikněte na položku nový
5. Následující vlastnosti hello toospecify pro nové odpovídající hello hello:
   * S novou podformulář hello otevřete, zvolte návrh - podformulář vlastnosti
   * Další toohello vlastnosti Name, zadejte název pro třídu pomocného objektu hello – například TestSubform.
   * Zachovat hello možnosti vlastnost "Zahrnout podformulář Insert... dialogové okno" vybrali
   * Zrušte výběr možnosti vlastnost hello "Vykreslení předávají HTML v poznámkách."
   * Ponechejte hello dalších hello stejné vlastnosti a zavřete okno Vlastnosti podformulář hello.
   * Uložte a zavřete nový podformulář hello.
6. Hello následující tooadd třídu pole toodefine hello pomocného objektu:
   * Otevřete hello podformulář, který jste vytvořili.
   * Zvolte vytvořit - pole.
   * Další tooName na kartě Základní informace o hello hello pole dialogové okno, zadejte jakýkoli název, například: {MyPersonTestAttribute}.
   * V poli přidané hello nastavte výběrem typu, stylu, velikost, písma a souvisejících vlastností jeho vlastnosti.
   * Atribut hello zachovat výchozí hodnota stejná jako hello název pro tento atribut (například pokud je název atributu MyPersonTestAttribute, ponechte hodnotu výchozí hello s hello stejný název).
   * Uložit hello podformulář s aktualizovanými hodnotami a hello následující:
     * V levém podokně hello vyberte sdíleného kódu a následně podformulářů
     * Vyberte nový podformulář hello a zvolte návrh - vlastnosti návrhu.
     * Klikněte na kartu třetí hello zleva hello a vyberte **rozšířit tento zákaz změnu návrhu**.
7. Otevřete podformulář ExtensibleSchema ${ObjectName}, (kde {ObjectName} je hello název třídy hello výchozí strukturální objektu, například – osoba).
8. Vložit prostředek a vyberte hello podformulář (který jste vytvořili, například – TestSubform) a uložte podformulář ExtensibleSchema hello ${ObjectName}.
9. Nahraďte hello Domino Directory šablony {PUBNAMES. NTF} s hello novou vlastní šablonu {CONTOSO. NTF} podle [tyto kroky](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Zavřete Správce Domino a otevřete konzolu Domino toorestart hello služba využívající protokol LDAP a tooReload hello schématu LDAP:
    * V konzole Domino, vložte příkaz hello pod **příkaz Domino** text selhala služba využívající protokol LDAP hello toorestart - [restartujte LDAP úloh](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html).
    * schéma LDAP tooreload použijte příkaz říct LDAP **říct ReloadSchema LDAP**.
11. Otevřete správce Domino a vyberte kartu toosee lidé a skupiny přidat atribut se odrazí v domino přidat osoba (v rámci jiné kartě).
12. Otevřete Schema.nsf z **soubory** kartě a najdete v části Přidání atributu se projeví ve TestSubform LDAP pomocnou třídu objektu.

**Způsob 3: Přidání hello vlastní atribut toohello ExtensibleObject třídy**

1. Otevření souboru {Schema.nsf} umístit do kořenového adresáře hello
2. Vyberte objekt třídy LDAP hello v levé nabídce **všechny dokumenty schématu** a klikněte na tlačítko **přidat objekt třídy** tlačítko:
3. Zadejte název protokolu LDAP hello tvar {zzzExtensibleSchema} (kde zzz je hello název třídy hello výchozí strukturální objektu, například osoba). Například tooextend hello schématu pro třídu objektu osoba, zadejte název protokolu LDAP {PersonExtensibleSchema}.
4. Zadejte název třídy pro objekt nadřízeného objektu, pro které chcete tooextend hello schématu. Například tooextend hello schématu pro třídu objektu osoba, zadejte název třídy pro objekt nadřízeného objektu {dominoPerson}:
5. Zadejte platnou OID odpovídající toohello objekt třídu.
6. Vyberte rozšířené nebo vlastních atributů v povinné nebo nepovinný atribut typy polí podle požadavek hello:
7. Po přidání požadovaných atributy toohello ExtensibleObjectClass, klikněte na tlačítko **uložit a zavřít**.
8. ExtensibleObjectClass se vytvoří pro příslušné výchozí třídu objektů s rozšířenými atributy.

## <a name="troubleshooting"></a>Řešení potíží
* Informace o tom, jak protokolování tooenable tootroubleshoot hello konektoru najdete v tématu hello [jak tooEnable trasování ETW pro konektory](http://go.microsoft.com/fwlink/?LinkId=335731).
