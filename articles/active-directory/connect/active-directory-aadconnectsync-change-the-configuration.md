---
title: "Synchronizace Azure AD Connect: provedení změn v synchronizaci Azure AD Connect v konfiguraci | Microsoft Docs"
description: "Vás provede procesem jak toomake změna toohello konfigurace ve službě Azure AD Connect synchronizovat."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 7b9df836-e8a5-4228-97da-2faec9238b31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 78e96d9166831a668439c2b8aa6a0022bc472da4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-how-toomake-a-change-toohello-default-configuration"></a>Synchronizace Azure AD Connect: jak toomake toohello změnu výchozí konfigurace
účelem Hello tohoto tématu je toowalk vás jak toomake změny toohello výchozí konfigurace v synchronizaci Azure AD Connect. Popisuje kroky pro některé běžné scénáře. Replikace musí být schopný toomake některé jednoduchými změnami tooyour vlastní konfigurace založené na vlastní obchodní pravidla.

## <a name="synchronization-rules-editor"></a>Editor pravidla synchronizace
Hello editoru pravidel synchronizace je použité toosee a změňte hello výchozí konfigurace. Najdete ho v nabídce Start hello pod hello **Azure AD Connect** skupiny.  
![Nabídka Start s Editor pravidla synchronizace](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Když otevřete, uvidíte hello výchozí out-of-box pravidla.

![Editor pravidla synchronizace](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-hello-editor"></a>Navigace v editoru hello
rozevírací seznamy Hello hello horní části editoru hello povolit tooquickly najít konkrétní pravidlo. Předpokládejme například pokud chcete pravidla hello toosee, kde je zahrnuté hello atribut proxyAddresses, by změnit hello rozevírací seznamy toohello následující:  
![Filtrování SRE](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
filtrování tooreset a zatížení novou konfiguraci, stiskněte klávesu **F5** na hello klávesnice.

vpravo nahoře toohello, máte tlačítko **přidat nové pravidlo**. Toto tlačítko je použité toocreate vlastní pravidla.

V dolní části hello máte tlačítka funguje na vybranou synchronizační pravidlo. **Upravit** a **odstranit** udělat, co očekáváte. **Export** vytvoří skript prostředí PowerShell k opětovnému vytvoření hello synchronizační pravidlo. Tento postup umožňuje toomove synchronizační pravidlo z jednoho serveru tooanother.

## <a name="create-your-first-custom-rule"></a>Vytvoření vaší první vlastní pravidlo
nejběžnější změna Hello je toky atributů toohello změny. Hello data ve vašem adresáři zdroj nemusí být v Azure AD. V příkladu hello v této části, chcete toomake se hello křestní jméno uživatele je vždy v **správné případ**.

### <a name="disable-hello-scheduler"></a>Zakázat hello plánovače
Hello [Plánovač](active-directory-aadconnectsync-feature-scheduler.md) ve výchozím nastavení spouští každých 30 minut. Chcete toomake se, že se nespouští při provádění změn a řešení potíží s nové pravidel. tootemporarily zakázat hello scheduler, spusťte prostředí PowerShell a spusťte`Set-ADSyncScheduler -SyncCycleEnabled $false`

![Zakázat hello plánovače](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-hello-rule"></a>Vytvoření pravidla hello
1. Klikněte na tlačítko **přidat nové pravidlo**.
2. Na hello **popis** stránky zadejte hello následující:  
   ![Příchozí pravidlo filtrování](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
   * Název: Zadejte pro pravidlo hello popisný název.
   * Popis: Některé vyjasnění, někdo pochopit, jaké hello pravidlo je pro.
   * Připojený systém: objekt hello hello systému naleznete v. V takovém případě vyberte hello konektor služby Active Directory.
   * Typ objektu připojené systému/Metaverse: Vyberte **uživatele** a **osoba** v uvedeném pořadí.
   * Typ propojení: Tuto hodnotu změnit příliš**připojení**.
   * Priorita: Zadejte hodnotu, která je v systému hello jedinečný. Nižší číselná hodnota znamená vyšší prioritu.
   * Značky: Ponechte prázdné. Toto pole zadá hodnotu by měl mít jenom out-of-box pravidla od společnosti Microsoft.
3. Na hello **Scoping filtru** zadejte **givenName ISNOTNULL**.  
   ![Příchozí pravidlo filtru pro vytváření oboru](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
   Tato část je použité toodefine pravidlo hello objekty, které by se měly používat k. Pokud je ponecháno prázdné, hello pravidlo vztahuje tooall uživatelské objekty. Ale které by mělo zahrnovat konferenční místnosti, účty služeb a jiných jiné osoby uživatelských objektů.
4. Na hello **připojení pravidla**, ponechte prázdné.
5. Na hello **transformace** změňte hello typ toku příliš**výraz**. Vyberte hello atribut Target **givenName**a ve zdroji zadejte `PCase([givenName])`.
   ![Příchozí pravidlo transformace](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
   Hello synchronizační modul je malá a velká písmena název funkce hello a hello název atributu hello. Pokud zadáte něco v pořádku, když přidáte pravidlo hello zobrazit upozornění. Hello editor vám umožní toosave a pokračovat, takže byste měli tooreopen hello pravidla a pravidla správné hello.
6. Klikněte na tlačítko **přidat** toosave hello pravidlo.

Nové vlastní pravidlo by měly jít vidět s hello další synchronizaci pravidla v systému hello.

### <a name="verify-hello-change"></a>Ověřte změnu hello
Díky této změně nové chcete toomake se, že funguje podle očekávání a není aktivována případné chyby. V závislosti na hello počet objektů, které máte existují dva různé způsoby toodo tento krok.

1. Spustit úplnou synchronizaci pro všechny objekty
2. Spustit úplné synchronizace a preview na jednoho objektu.

Spustit **synchronizační služba** z nabídky start hello. Hello kroky v této části jsou v tento nástroj.

1. **Úplná synchronizace pro všechny objekty**  
   Vyberte **konektory** v horní části hello. Identifikovat hello konektoru, které jste provedli změnu tooin hello předchozí části, v takovém případě hello Active Directory Domain Services a vyberte ho. Vyberte **spustit** akce a výběrem **úplná synchronizace** a **OK**.
   ![Úplná synchronizace](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
   Hello objekty jsou nyní aktualizovat v úložišti metaverse hello. Chcete nyní toolook na hello objektu v úložišti metaverse hello.
2. **Verze Preview a úplnou synchronizaci na jednoho objektu.**  
   Vyberte **konektory** v horní části hello. Identifikovat hello konektoru, které jste provedli změnu tooin hello předchozí části, v takovém případě hello Active Directory Domain Services a vyberte ho. Vyberte **hledání prostoru konektoru**. Použijte obor toofind objekt, který chcete změnit hello tootest toouse. Vyberte objekt hello a klikněte na tlačítko **Preview**. Na nové obrazovce hello vyberte **potvrdit Preview**.  
   ![Potvrzení verze preview](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
   Změna Hello je nyní potvrdit toohello metaverse.

**Podívejte se na hello objektu v úložišti metaverse hello**  
Chcete nyní toopick očekávána několik ukázkových objektů toomake zda hello je hodnota a že hello pravidlo. Vyberte **hledání úložiště Metaverse** shora hello. Přidejte libovolný filtr potřebujete toofind hello relevantní objekty. Výsledek hledání hello otevřete objekt. Podívejte se na hodnoty atributu hello a také ověřit v hello **pravidla synchronizace** sloupec, který hello pravidlo podle očekávání.  
![Vyhledávání Metaverse](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  

### <a name="enable-hello-scheduler"></a>Povolit hello plánovače
Pokud všechno podle očekávání, můžete znovu povolit hello plánovače. Z prostředí PowerShell, spusťte `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Další běžné změny toku atributů
předchozí části Hello popsané, jak toomake změny tooan toku atributů. V této části najdete některé další příklady. Hello kroky pro jak toocreate hello synchronizační pravidlo se používá zkratka, ale můžete najít hello úplné kroky v předchozí části hello.

### <a name="use-another-attribute-than-hello-default"></a>Použití jiného atributu než výchozí hello
Ve firmě Fabrikam je doménové struktuře, kde se používá místní abecedy hello křestní jméno, příjmení a zobrazovaný název. Hello znaků latinky reprezentace těchto atributů najdete v atributech rozšíření hello. Při sestavování hello globálním seznamu ve službě Azure AD a Office 365, hello organizace přeje, aby tyto atributy toobe místo toho použít.

S konfigurací výchozí objekt z místní doménové struktuře hello vypadat třeba takto:  
![Tok atributů 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

toocreate pravidlo s jinými toky atributů hello následující:

* Spustit **Synchronization Rule Editor** z nabídky start hello.
* S **příchozí** stále vybrané toohello vlevo, klikněte na tlačítko hello **přidat nové pravidlo**.
* Pravidlo hello zadejte název a popis. Vyberte hello místní služby Active Directory a hello relevantní typy objektů. V **typu propojení**, vyberte **připojení**. Prioritu vyberte číslo, které nepoužívají jiné pravidlo. pravidla out-of-box Hello začínat 100, takže hello hodnotu 50 lze použít v tomto příkladu.
  ![Tok atributů 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
* Ponechte prázdné oboru (to znamená, že by se měly používat tooall uživatelských objektů v doménové struktuře hello).
* Nechte spojení pravidel prázdný (která je, umožní hello out-of-box pravidlo popisovač žádné spojení).
* V transformace vytvořte následující toky hello:  
  ![Tok atributů 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
* Klikněte na tlačítko **přidat** toosave hello pravidlo.
* Přejděte příliš**Synchronization Service Manager**. Na **konektory**, vyberte hello konektoru, které jsme přidali hello pravidlo. Vyberte **spustit**, a **úplné synchronizace**. Úplná synchronizace přepočítá všechny objekty pomocí hello aktuální pravidla.

Toto je hello výsledek hello stejný objekt s tohoto vlastního pravidla:  
![Tok atributů 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Délka atributů
Atributy řetězce jsou výchozí sadu toobe indexovanou a hello maximální délka je 448 znaků. Při práci s atributy řetězce, které může obsahovat více, proveďte následující hello zda tooinclude v toku atributů hello:  
`attributeName` <- `Left([attributeName],448)`

### <a name="changing-hello-userprincipalsuffix"></a>Změna hello userPrincipalSuffix
atribut userPrincipalName Hello ve službě Active Directory není vždy znám hello uživatelé a nemusí být vhodný jako hello přihlášení. Hello Azure AD Connect sync Průvodce instalací umožňuje výběr jiný atribut, například poštovní. Ale v některých případech hello musí být atribut vypočítat. Například hello společnost Contoso má dva adresáře Azure AD, jeden pro produkční prostředí a jeden pro testování. Chtějí, že uživatelé hello v jejich testu klienta toouse jinou příponu v hello přihlášení.  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

V tomto výrazu, proveďte všechno levé hello nejprve @-sign (Word) a zřetězení s pevnou řetězec.

### <a name="convert-a-multi-value-tooa-single-value"></a>Převést jednou hodnotou tooa s více hodnotami
Některé atributy ve službě Active Directory jsou více hodnot ve schématu hello, i když se zobrazí jeden obsah s hodnotou v Active Directory Users and Computers. Příkladem je popis atributů hello.  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

V tomto výrazu v případě, že hello atributu obsahuje hodnotu, trvat hello první položku (položky) v atributu hello odebrat úvodní i koncové mezery (Trim) a pak zachovat hello nejprve 448 znaků (vlevo) v řetězci hello.

### <a name="do-not-flow-an-attribute"></a>Není toku atributu
Pozadí na hello scénáře v této části najdete v části [řízení procesu toku atributů hello](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Existují dva způsoby toonot toku atributu. Hello nejprve je k dispozici v Průvodci instalací hello a umožňuje vám příliš[odebrat vybrané atributy](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Tato možnost funguje, pokud byly nikdy synchronizované hello atribut před. Ale pokud jste spustili toosynchronize tento atribut a později ho odebrat pomocí této funkce, pak hello synchronizační modul zastaví Správa hello atribut a hello existující hodnoty jsou ponechána ve službě Azure AD.

Pokud chcete tooremove hello hodnotu atributu a ujistěte se, že že není toku v budoucnu hello, je třeba vytvořit vlastní pravidlo.

Ve firmě Fabrikam uvědomili jsme mít si, že některé hello atributy jsme synchronizovat toohello cloudu by neměl být existuje. Chceme toomake se, že tyto atributy se odeberou z Azure AD.  
![Chybný rozšíření atributy](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

* Vytvořit nové pravidlo příchozí synchronizace a naplnit hello popis ![popisy](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
* Vytvořit toky atributů typu **výraz** a se zdrojem hello **AuthoritativeNull**. Hello literálu **AuthoritativeNull** označuje, že hello hodnota by měla být v hello MV prázdné, i v případě, že hodnota hello toopopulate pokusí nižší prioritu synchronizační pravidlo.
  ![Transformace pro atributy rozšíření](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
* Uložte hello synchronizační pravidlo. Spustit **synchronizační služba**, najde hello konektor, vyberte **spustit**, a **úplná synchronizace**. Tento krok přepočítá všechny toky atributů.
* Ověřte, že tento hello zamýšlená změny jsou o toobe exportované sadou hledání prostoru konektoru hello.
  ![Odstranění dvoufázové instalace](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="create-rules-with-powershell"></a>Vytvoření pravidla pomocí prostředí PowerShell
Pomocí editoru pravidlo synchronizace hello funguje správně, pokud máte pouze několik toomake změny. Pokud potřebujete toomake mnoho změn, může být prostředí PowerShell lepší volbou. Některé pokročilé funkce jsou dostupné v prostředí PowerShell jenom.

### <a name="get-hello-powershell-script-for-an-out-of-box-rule"></a>Získat hello skript prostředí PowerShell pro pravidlo out-of-box
hello toosee Powershellový skript, který vytvořili out-of-box pravidla, vyberte hello pravidlo v synchronizaci hello pravidla editoru a klikněte na tlačítko **exportovat**. Tato akce poskytuje hello prostředí PowerShell skript daného pravidla vytvořený hello.

### <a name="advanced-precedence"></a>Pokročilé priorit
pravidla synchronizace out-of-box Hello začínat hodnotou priority 100. Pokud máte mnoho doménových struktur a potřebujete toomake mnoho vlastní změny a pak 99 synchronizační pravidla nemusí být dost.

Můžete určit, aby hello synchronizační modul, který chcete vložit dříve, než pravidla out-of-box hello dalších pravidlech. tooget toto chování, postupujte takto:

1. První pravidlo synchronizace out-of-box označit hello (toto pravidlo je hello **v z připojení uživatele AD**) v editoru pravidlo synchronizace hello a vyberte **exportovat**. Zkopírujte hodnotu identifikátoru SR hello.  
![Prostředí PowerShell před změnou](./media/active-directory-aadconnectsync-change-the-configuration/powershell1.png)  
2. Vytvořte nové pravidlo synchronizace hello. Můžete použít hello synchronizační pravidlo editor toocreate ho. Skript prostředí PowerShell tooa pravidlo hello exportujte.
3. Ve vlastnosti hello **PrecedenceBefore**, vložte hodnotu identifikátoru hello z hello out-of-box pravidla. Sada hello **přednost** příliš**0**. Zajistěte, aby atribut identifikátoru hello je jedinečná a nejsou opakovaného použití identifikátoru GUID z jiné pravidlo. Také zkontrolujte, že hello **ImmutableTag** není nastavena vlastnost; tato vlastnost by měla být nastavena pouze pro pravidlo out-of-box. Uložení skriptu prostředí PowerShell hello a spusťte ji. Hello výsledkem je, že vlastní pravidla je přiřazena hello přednost před hodnotu 100 a všemi ostatními pravidly out-of-box se zvýší.  
![Prostředí PowerShell po změně](./media/active-directory-aadconnectsync-change-the-configuration/powershell2.png)  

Může mít mnoho vlastní synchronizační pravidla pomocí stejné hello **PrecedenceBefore** hodnota v případě potřeby.


## <a name="enable-synchronization-of-preferreddatalocation"></a>Povolit synchronizaci PreferredDataLocation
Azure AD Connect podporuje synchronizace hello **PreferredDataLocation** atribut pro **uživatele** objekty ve verzi 1.1.524.0 a po. Přesněji řečeno byly zavedeny následující změny:

* schéma Hello typu objektu hello **uživatele** v hello konektoru služby Azure AD je rozšířeno tooinclude PreferredDataLocation atribut, který je typu řetězec a je jedinou hodnotu.

* schéma Hello typu objektu hello **osoba** v hello je rozšířené úložiště Metaverse tooinclude PreferredDataLocation atribut, který je typu řetězec a je jedinou hodnotu.

Ve výchozím nastavení není povoleno hello PreferredDataLocation atribut pro synchronizaci, protože neexistuje žádný odpovídající atribut PreferredDataLocation v místní službě Active Directory. Je nutné ručně povolit synchronizaci.

> [!IMPORTANT]
> V současné době Azure AD umožňuje hello PreferredDataLocation atribut na synchronizované objekty uživatele a cloud toobe objekty uživatele přímo konfigurovat pomocí Azure AD PowerShell. Jakmile povolíte synchronizaci hello PreferredDataLocation atributu, je nutné zastavit pomocí Azure AD PowerShell tooconfigure hello atribut na **synchronizované uživatelské objekty** jako Azure AD Connect se přepíše je na základě Hello zdroj hodnot atributů v místní službě Active Directory.

> [!IMPORTANT]
> V 1 září 2017, Azure AD nebude povolovat hello PreferredDataLocation atribut na **synchronizované uživatelské objekty** toobe přímo konfigurovat pomocí Azure AD PowerShell. atribut PreferredLocation tooconfigure synchronizované objekty uživatele, je nutné použít jenom Azure AD Connect.

Než povolíte synchronizaci hello PreferredDataLocation atributu, musíte:

 * Nejdřív se rozhodněte, které místní služby Active Directory atribut toobe použít jako zdrojový atribut hello. Musí být typu **řetězec** a je **jednohodnotové**.

 * Pokud jste dříve nakonfigurovali hello PreferredDataLocation atribut na existující synchronizované uživatelské objekty ve službě Azure AD pomocí Azure AD PowerShell, je nutné **backport** hello atribut hodnoty toohello odpovídající uživatelské objekty v místní službě Active Directory.
 
    > [!IMPORTANT]
    > Pokud uděláte není backport hello atribut hodnoty toohello odpovídající uživatelské objekty ve službě Active Directory v místě, Azure AD Connect odebere hello existující hodnoty atributu ve službě Azure AD, když je synchronizace pro atribut PreferredDataLocation hello Povolit.

 * Doporučujeme nakonfigurovat hello zdrojový atribut na alespoň několik místní uživatele AD objekty nyní, který může být použit pro ověření později.
 
synchronizace tooenable kroky Hello hello PreferredDataLocation atributu můžete shrnuto jako:

1. Zakažte plánovače synchronizace a ověřte, že neexistuje žádná synchronizace v průběhu

2. Přidat hello zdrojový atribut toohello místní AD Connector schématu

3. Přidat PreferredDataLocation toohello konektoru služby Azure AD schématu

4. Vytvořit hodnotu atributu hello tooflow pravidla synchronizace příchozích dat z místní služby Active Directory

5. Vytvoření tooAzure atribut hodnota odchozí synchronizace pravidlo tooflow hello AD

6. Spustit úplnou synchronizaci cyklus

7. Povolit synchronizaci plánovače

> [!NOTE]
> Hello zbývající část tohoto oddílu popisuje tyto kroky v podrobnostech. Jsou popsány v kontextu hello nasazení služby Azure AD s jednou doménovou strukturou topologií a bez vlastní synchronizační pravidla. Pokud máte topologie s více doménovými strukturami, vlastní synchronizační pravidla nakonfigurovaná nebo mít na testovacím serveru, musíte tooadjust hello kroky odpovídajícím způsobem.

### <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Krok 1: Zakázat plánovače synchronizace a ověřte, že neexistuje žádná synchronizace v průběhu
Ujistěte se, žádná synchronizace bude probíhat, když jste uprostřed hello aktualizace synchronizace pravidla tooavoid nezamýšleným změní vrácení exportovat tooAzure AD. plánovače předdefinované synchronizace toodisable hello:

 1. Spusťte relaci prostředí PowerShell na server Azure AD Connect hello.

 2. Zakážete plánované synchronizace spuštěním rutiny:`Set-ADSyncScheduler -SyncCycleEnabled $false`
 
 3. Spustit hello **Synchronization Service Manager** podle přejdete → tooSTART synchronizační služba.
 
 4. Přejděte toohello **operace** kartě a potvrďte neprobíhá žádná operace, jejichž stav je *"v průběhu."*

![Zkontrolujte Synchronization Service Manager - žádné operace v průběhu](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step1.png)

### <a name="step-2-add-hello-source-attribute-toohello-on-premises-ad-connector-schema"></a>Krok 2: Přidání hello zdrojový atribut toohello místní AD Connector schématu
Všechny atributy AD jsou importovány do hello místní AD prostoru konektoru. tooadd hello zdrojový atribut toohello seznam hello importovány atributy:

 1. Přejděte toohello **konektory** ve hello Synchronization Service Manager.
 
 2. Klikněte pravým tlačítkem na hello **místní AD Connector** a vyberte **vlastnosti**.
 
 3. V místním dialogovém okně hello, přejděte toohello **vybrat atributy** kartě.
 
 4. Zkontrolujte, zda je v seznamu atributů hello zaškrtnuté hello zdrojový atribut.
 
 5. Klikněte na tlačítko **OK** toosave.

![Přidejte zdrojový atribut tooon místní schéma AD Connector.](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step2.png)

### <a name="step-3-add-preferreddatalocation-toohello-azure-ad-connector-schema"></a>Krok 3: Přidání PreferredDataLocation toohello konektoru služby Azure AD schématu
Ve výchozím nastavení není atribut PreferredDataLocation hello naimportován do hello Azure AD Connect místa. seznam toohello tooadd hello PreferredDataLocation atributů importované atributů:

 1. Přejděte toohello **konektory** ve hello Synchronization Service Manager.

 2. Klikněte pravým tlačítkem na hello **konektoru služby Azure AD** a vyberte **vlastnosti**.

 3. V místním dialogovém okně hello, přejděte toohello **vybrat atributy** kartě.

 4. Zajistěte, aby atribut PreferredDataLocation hello se změnami hello seznam atributů.

 5. Klikněte na tlačítko **OK** toosave.

![Přidejte zdrojový atribut tooAzure schéma AD Connector.](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step3.png)

### <a name="step-4-create-an-inbound-synchronization-rule-tooflow-hello-attribute-value-from-on-premises-active-directory"></a>Krok 4: Vytvoření hodnota atributu hello tooflow pravidla synchronizace příchozích dat z místní služby Active Directory
pravidla synchronizace příchozích dat Hello umožňuje tooflow hodnota atributu hello z hello zdrojový atribut z místní služby Active Directory toohello úložiště Metaverse:

1. Spustit hello **editoru pravidel synchronizace** podle přejdete → tooSTART pravidla synchronizace Editor.

2. Sada hello vyhledávací filtr **směr** toobe **příchozí**.

3. Klikněte na tlačítko **přidat nové pravidlo** toocreate tlačítko Nový příchozí pravidlo.

4. V části hello **popis** kartě, zadejte hello následující konfigurace:
 
    | Atribut | Hodnota | Podrobnosti |
    | --- | --- | --- |
    | Name (Název) | *Zadejte název* | Například *"v ze služby Active Directory – uživatel PreferredDataLocation"* |
    | Popis | *Zadejte popis* |  |
    | Připojený systém | *Vyberte hello místní AD connector.* |  |
    | Typ objektu systému připojené | **Uživatel** |  |
    | Typ objektu úložiště Metaverse | **Osoba** |  |
    | Typ propojení | **Spojení** |  |
    | Priorita | *Vyberte číslo mezi 1 – 99* | 1 – 99 je vyhrazený pro vlastní synchronizačního pravidla. Není vyberte hodnotu, která je používána jinou synchronizační pravidlo. |

5. Přejděte toohello **Scoping filtru** kartě a přidejte **jednu skupinu oboru filtru se hello následující klauzule**:
 
    | Atribut | Operátor | Hodnota |
    | --- | --- | --- |
    | adminDescription | NOTSTARTWITH | Uživatel\_ | 
 
    Oboru filtru určuje, které místní AD objekty že do je použito toto pravidlo příchozí synchronizace. V tomto příkladu používáme hello stejné oboru filtru použít jako *"v ze služby Active Directory – běžné uživatele"* OOB synchronizační pravidlo, které brání v dodržení hello synchronizační pravidlo použije tooUser objekty vytvořené pomocí zpětný zápis uživatelů Azure AD funkce. Může být nutné tootweak hello oboru filtru, podle tooyour nasazení Azure AD Connect.

6. Přejděte toohello **transformace karta** a implementovat následující pravidla transformace hello:
 
    | Typ toku | Atribut target | Zdroj | Použít jednou | Merge – typ |
    | --- | --- | --- | --- | --- |
    | Přímé | PreferredDataLocation | Vyberte zdrojový atribut hello | Nezaškrtnuto | Aktualizace |

7. Klikněte na tlačítko **přidat** toocreate hello příchozí pravidlo.

![Vytvoření pravidla synchronizace příchozích dat](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step4.png)

### <a name="step-5-create-an-outbound-synchronization-rule-tooflow-hello-attribute-value-tooazure-ad"></a>Krok 5: Vytvoření tooAzure atribut hodnota odchozí synchronizace pravidlo tooflow hello AD
Pravidlo odchozí synchronizace Hello umožňuje tooflow hodnota atributu hello z hello Metaverse toohello PreferredDataLocation atributu ve službě Azure AD:

1. Přejděte toohello **synchronizační pravidla** Editor.

2. Sada hello vyhledávací filtr **směr** toobe **odchozí**.

3. Klikněte na tlačítko **přidat nové pravidlo** tlačítko.

4. V části hello **popis** kartě, zadejte hello následující konfigurace:

    | Atribut | Hodnota | Podrobnosti |
    | --- | --- | --- |
    | Name (Název) | *Zadejte název* | Například "Out tooAAD – PreferredDataLocation uživatele" |
    | Popis | *Zadejte popis* |
    | Připojený systém | *Vyberte konektor AAD hello* |
    | Typ objektu systému připojené | Uživatel ||
    | Typ objektu úložiště Metaverse | **Osoba** ||
    | Typ propojení | **Spojení** ||
    | Priorita | *Vyberte číslo mezi 1 – 99* | 1 – 99 je vyhrazený pro vlastní synchronizačního pravidla. YDo není vyberte hodnotu, která je používána jinou synchronizační pravidlo. |

5. Přejděte toohello **Scoping filtru** kartě a přidejte **jednu skupinu oboru filtru se dvěma klauzule**:
 
    | Atribut | Operátor | Hodnota |
    | --- | --- | --- |
    | sourceObjectType | ROVNÁ | Uživatel |
    | cloudMastered | NOTEQUAL | True |

    Určuje oboru filtru, které objekty Azure AD, že je toto pravidlo odchozí synchronizace použito pro. V tomto příkladu používáme hello stejné oboru filtru z "Out tooAD – identitu uživatele" OOB synchronizační pravidlo. Pravidlo synchronizace hello zabrání se použité tooUser objekty, které nejsou synchronizovány z místní služby Active Directory. Může být nutné tootweak hello oboru filtru, podle tooyour nasazení Azure AD Connect.
    
6. Přejděte toohello **transformace** kartě a implementovat následující pravidla transformace hello:

    | Typ toku | Atribut target | Zdroj | Použít jednou | Merge – typ |
    | --- | --- | --- | --- | --- |
    | Přímé | PreferredDataLocation | PreferredDataLocation | Nezaškrtnuto | Aktualizace |

7. Zavřít **přidat** toocreate hello odchozí pravidlo.

![Vytvořit pravidlo odchozí synchronizace](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step5.png)

### <a name="step-6-run-full-synchronization-cycle"></a>Krok 6: Spusťte úplnou synchronizaci cyklu
Obecně platí je vyžaduje úplnou synchronizaci cyklus, protože jsme přidali nové atributy tooboth hello AD a Azure AD Connector schéma a zavedená vlastní synchronizační pravidla. Doporučujeme, abyste ověřili hello změny před exportováním je tooAzure AD. Můžete použít následující kroky tooverify hello změny při spuštění ručně hello kroky, které tvoří cyklus úplné synchronizace hello. 

1. Spustit **úplný import** kroku na hello **místní AD Connector**:

   1. Přejděte toohello **Operations** ve hello Synchronization Service Manager.

   2. Klikněte pravým tlačítkem na hello **místní AD Connector** a vyberte **spustit...**

   3. V místním dialogovém okně hello, vyberte **úplný Import** a klikněte na tlačítko **OK**.
    
   4. Počkejte toocomplete operaci.

    > [!NOTE]
    > Úplný Import můžete přeskočit na hello místní AD Connector. Pokud hello zdrojový atribut je již obsažena v seznamu hello importované atributů. Jinými slovy, můžete neměl toomake všechny změny během [krok 2: Přidat hello zdrojový atribut toohello místní AD Connector schématu](#step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema).

2. Spustit **úplný import** kroku na hello **konektoru služby Azure AD**:

   1. Klikněte pravým tlačítkem na hello **konektoru služby Azure AD** a vyberte **spustit...**

   2. V místním dialogovém okně hello, vyberte **úplný Import** a klikněte na tlačítko **OK**.
   
   3. Počkejte toocomplete operaci.

3. Ověřte hello změny pravidel synchronizace na existující objekt uživatele:

zdrojový atribut Hello z místní služby Active Directory a PreferredDataLocation z Azure AD byly naimportovány na portál hello příslušných konektor místa. Než budete pokračovat v kroku úplnou synchronizaci, je doporučeno, abyste provedli **Preview** stávajícího uživatele objektu v hello na místní AD prostoru konektoru. Hello objekt, který jste vybrali měli hello zdrojový atribut naplněno. Úspěšně **Preview** s hello PreferredDataLocation vložené do úložiště Metaverse hello je dobré indikátoru, že jste správně nakonfigurovali hello synchronizační pravidla. Informace o toodo **Preview**, odkazovat toosection [ověřit změnu hello](#verify-the-change).

4. Spustit **úplná synchronizace** kroku na hello **místní AD Connector**:

   1. Klikněte pravým tlačítkem na hello **místní AD Connector** a vyberte **spustit...**
  
   2. V místním dialogovém okně hello, vyberte **úplná synchronizace** a klikněte na tlačítko **OK**.
   
   3. Počkejte toocomplete operaci.

5. Ověřte **čekající exporty** tooAzure AD:

   1. Klikněte pravým tlačítkem na hello **konektoru služby Azure AD** a vyberte **hledání prostoru konektoru**.

   2. V dialogu automaticky otevírané okno hledání prostoru konektoru hello:

      1. Nastavit **oboru** příliš**čekající exportovat**.
      
      2. Zkontrolujte všechny tři políčka, včetně **přidat, upravit a odstranit**.
      
      3. Klikněte na tlačítko hello **vyhledávání** tooget hello seznam objektů se změny toobe exportovali. změny hello tooexamine pro daný objekt, dvakrát klikněte na objekt hello.
      
      4. Ověřte, že se očekává hello změny.

6. Spustit **exportovat** kroku na hello **Azure AD Connector.**
      
   1. Klikněte pravým tlačítkem na hello **konektoru služby Azure AD** a vyberte **spustit...**
   
   2. V automaticky otevíraný dialog hello spustit konektor, vyberte **exportovat** a klikněte na tlačítko **OK**.
   
   3. Počkejte, než pro Export toocomplete tooAzure AD.

> [!NOTE]
> Můžete si všimnout, že kroky hello nezahrnují hello úplná synchronizace kroky a Export na hello konektoru služby Azure AD. Hello kroky nejsou potřeba, protože jsou hodnoty atributu hello odesílaných z místní tooAzure služby Active Directory AD pouze.

### <a name="step-7-re-enable-sync-scheduler"></a>Krok 7: Opětovné povolení plánovače synchronizace
Znovu povolte plánovače předdefinované synchronizace hello:

1. Spusťte relaci prostředí PowerShell.

2. Plánované synchronizace znovu povolte spuštěním rutiny:`Set-ADSyncScheduler -SyncCycleEnabled $true`



## <a name="next-steps"></a>Další kroky
* Další informace o hello konfigurační model v [Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Další informace o hello výrazu jazyka v [Principy deklarativní zřizování výrazy](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Témata s přehledem**

* [Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
