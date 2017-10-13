---
title: "Synchronizace Azure AD Connect: provedení změn v synchronizaci Azure AD Connect v konfiguraci | Microsoft Docs"
description: "Vás provede procesem jak provést změnu konfigurace v synchronizaci Azure AD Connect."
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
ms.openlocfilehash: 63a7ae9d39e1a74294637172efd607ee41b2d69b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-how-to-make-a-change-to-the-default-configuration"></a>Synchronizace Azure AD Connect: jak provést změnu výchozí konfigurace
Účelem tohoto tématu je vám ukážeme, jak změnit výchozí konfigurace v synchronizaci Azure AD Connect. Popisuje kroky pro některé běžné scénáře. Replikace byste měli možnost provádět některé jednoduché změny do vlastní konfigurace založené na vlastní obchodní pravidla.

## <a name="synchronization-rules-editor"></a>Editor pravidla synchronizace
Editor pravidla synchronizace umožňuje zobrazit a změnit výchozí konfiguraci. Najdete ho v nabídce Start pod **Azure AD Connect** skupiny.  
![Nabídka Start s Editor pravidla synchronizace](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Po otevření, zobrazí se výchozí pravidla out-of-box.

![Editor pravidla synchronizace](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Navigace v editoru
Rozevírací seznamy v horní části editoru umožňují rychle vyhledat konkrétní pravidlo. Předpokládejme například pokud chcete zobrazit pravidla, kde je zahrnuté atribut proxyAddresses, by změnit rozevírací seznamy takto:  
![Filtrování SRE](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
Obnovit filtrování a načíst novou konfiguraci, stiskněte klávesu **F5** na klávesnici.

Vpravo nahoře máte tlačítko **přidat nové pravidlo**. Toto tlačítko slouží k vytvoření vlastního pravidla.

V dolní části máte tlačítka funguje na vybranou synchronizační pravidlo. **Upravit** a **odstranit** udělat, co očekáváte. **Export** vytvoří skript prostředí PowerShell k opětovnému vytvoření synchronizační pravidlo. Tento postup umožňuje přesunout synchronizační pravidlo z jednoho serveru na jiný.

## <a name="create-your-first-custom-rule"></a>Vytvoření vaší první vlastní pravidlo
Nejběžnější změna je změny toky atributů. Data ve vašem adresáři zdroj nemusí být v Azure AD. V příkladu v této části můžete chtít zajistit, křestní jméno uživatele je vždy v **správné případ**.

### <a name="disable-the-scheduler"></a>Zakázat plánovače
[Plánovač](active-directory-aadconnectsync-feature-scheduler.md) ve výchozím nastavení spouští každých 30 minut. Chcete Ujistěte se, že se nespouští při provádění změn a řešení potíží s nové pravidel. K dočasnému zakázání Plánovač, spusťte prostředí PowerShell a spusťte`Set-ADSyncScheduler -SyncCycleEnabled $false`

![Zakázat plánovače](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>Vytvoření pravidla
1. Klikněte na tlačítko **přidat nové pravidlo**.
2. Na **popis** stránky zadejte následující:  
   ![Příchozí pravidlo filtrování](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
   * Název: Zadejte popisný název pravidla.
   * Popis: Některé vyjasnění, někdo pochopit, co pravidlo je.
   * Připojený systém: objekt lze nalézt v systému. V takovém případě vyberte konektor služby Active Directory.
   * Typ objektu připojené systému/Metaverse: Vyberte **uživatele** a **osoba** v uvedeném pořadí.
   * Typ propojení: Změňte tuto hodnotu s **připojení**.
   * Priorita: Zadejte hodnotu, která je v systému jedinečný. Nižší číselná hodnota znamená vyšší prioritu.
   * Značky: Ponechte prázdné. Toto pole zadá hodnotu by měl mít jenom out-of-box pravidla od společnosti Microsoft.
3. Na **Scoping filtru** zadejte **givenName ISNOTNULL**.  
   ![Příchozí pravidlo filtru pro vytváření oboru](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
   V této části se používá k definování, které objekty, že pravidlo by se měly používat k. Pokud zůstane prázdné, pravidlo by platit pro všechny uživatelské objekty. Ale které by mělo zahrnovat konferenční místnosti, účty služeb a jiných jiné osoby uživatelských objektů.
4. Na **připojení pravidla**, ponechte prázdné.
5. Na **transformace** změňte typ toku k **výraz**. Vyberte cílový atribut **givenName**a ve zdroji zadejte `PCase([givenName])`.
   ![Příchozí pravidlo transformace](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
   Synchronizační modul je malá a velká písmena název funkce a název atributu. Pokud zadáte něco v pořádku, když přidáte pravidlo zobrazit upozornění. Editor umožňuje uložit a pokračovat, tak budete muset znovu otevřete pravidlo a opravte pravidlo.
6. Klikněte na tlačítko **přidat** uložíte pravidlo.

Nové vlastní pravidlo by měly jít vidět s jinými pravidly synchronizace v systému.

### <a name="verify-the-change"></a>Ověření změny
Pomocí této nové změny budete chtít zajistěte, aby ho pracuje dle očekávání a není aktivována případné chyby. V závislosti na počet objektů, které máte se dvěma různými způsoby, tento krok.

1. Spustit úplnou synchronizaci pro všechny objekty
2. Spustit úplné synchronizace a preview na jednoho objektu.

Spustit **synchronizační služba** z nabídky start. Postup v této části jsou v tento nástroj.

1. **Úplná synchronizace pro všechny objekty**  
   Vyberte **konektory** v horní části. Identifikovat konektor jste provedli změnu v předchozí části, v tomto případě Active Directory Domain Services, a vyberte jej. Vyberte **spustit** akce a výběrem **úplná synchronizace** a **OK**.
   ![Úplná synchronizace](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
   Objekty jsou nyní aktualizovat v úložišti metaverse. Chcete teď podívejte se na objekt v úložišti metaverse.
2. **Verze Preview a úplnou synchronizaci na jednoho objektu.**  
   Vyberte **konektory** v horní části. Identifikovat konektor jste provedli změnu v předchozí části, v tomto případě Active Directory Domain Services, a vyberte jej. Vyberte **hledání prostoru konektoru**. Vyhledání objektu, který chcete použít k testování změn pomocí oboru. Vyberte objekt a klikněte na tlačítko **Preview**. Na nové obrazovce vyberte **potvrdit Preview**.  
   ![Potvrzení verze preview](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
   Tato změna se potvrdí teď do úložiště metaverse.

**Podívejte se na objekt v úložišti metaverse**  
Chcete nyní vybrat několik ukázkových objekty a ujistěte se, že se očekává hodnotu a že pravidlo použije. Vyberte **hledání úložiště Metaverse** shora. Přidejte libovolný filtr, budete muset najít relevantní objekty. Výsledek hledání otevřete objekt. Podívejte se na hodnoty atributu a také ověřit v **pravidla synchronizace** sloupec, který pravidlo použije podle očekávání.  
![Vyhledávání Metaverse](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  

### <a name="enable-the-scheduler"></a>Povolit plánovače
Pokud všechno podle očekávání, můžete znovu povolit plánovače. Z prostředí PowerShell, spusťte `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Další běžné změny toku atributů
V předchozí části je popsané, jak provádět změny tok atributů. V této části najdete některé další příklady. Kroky pro vytvoření pravidla synchronizace je zkratka, ale úplné postup najdete v předchozí části.

### <a name="use-another-attribute-than-the-default"></a>Použití jiného atributu než výchozí
Ve firmě Fabrikam je doménové struktuře, kde se používá místní abecedy pro křestní jméno, příjmení a zobrazovaný název. Reprezentace znaků latinky těchto atributů najdete v rozšiřující atributy. Při sestavování v globálním adresáři v Azure AD a Office 365, organizace chce těchto atributů, který se má použít místo.

S konfigurací výchozí objekt z místní doménové struktuře vypadat třeba takto:  
![Tok atributů 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

K vytvoření pravidla s jinými toky atributů, postupujte takto:

* Spustit **Synchronization Rule Editor** z nabídky start.
* S **příchozí** vybraný na levé straně a klikněte na tlačítko **přidat nové pravidlo**.
* Toto pravidlo zadejte název a popis. Vyberte místní služby Active Directory a typy objektů relevantní. V **typu propojení**, vyberte **připojení**. Prioritu vyberte číslo, které nepoužívají jiné pravidlo. Pravidla out-of-box začínat 100, takže hodnota 50 lze použít v tomto příkladu.
  ![Tok atributů 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
* Ponechte prázdné oboru (to znamená, že by se měly používat pro všechny objekty uživatele v doménové struktuře).
* Nechte spojení pravidla prázdná (tj, umožní pravidlo out-of-box zpracovat žádné spojení).
* V transformace vytvořte následující toky:  
  ![Tok atributů 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
* Klikněte na tlačítko **přidat** uložíte pravidlo.
* Přejděte na **Synchronization Service Manager**. Na **konektory**, vyberte konektor, které jsme přidali pravidlo. Vyberte **spustit**, a **úplné synchronizace**. Úplná synchronizace přepočítá všechny objekty, které používá aktuální pravidla.

Toto je výsledek pro stejný objekt s tohoto vlastního pravidla:  
![Tok atributů 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Délka atributů
Řetězec atributy jsou ve výchozím nastavení nastaven jako indexovanou a maximální délka je 448 znaků. Pokud pracujete s atributy řetězce, které může obsahovat více, proveďte se tok atributů obsahuje následující:  
`attributeName` <- `Left([attributeName],448)`

### <a name="changing-the-userprincipalsuffix"></a>Změna userPrincipalSuffix
Atribut userPrincipalName ve službě Active Directory není vždy znám uživatelé a nemusí být vhodný jako přihlášení. Azure AD Connect, Průvodce instalací synchronizace umožňuje výběr jiný atribut, například poštovní. Ale v některých případech musí být atribut vypočítat. Například společnost Contoso má dva adresáře Azure AD, jeden pro produkční prostředí a jeden pro testování. Mají uživatelé v jejich testovacím klientem používat jinou příponu v přihlášení.  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

V tomto výrazu trvat všechno levém prvního @-sign (Word) a zřetězení s pevnou řetězec.

### <a name="convert-a-multi-value-to-a-single-value"></a>Převést na hodnotu single s více hodnotami
Některé atributy ve službě Active Directory jsou více hodnot ve schématu, i když se zobrazí jeden obsah s hodnotou v Active Directory Users and Computers. Příkladem je popis atributů.  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

V tomto výrazu v případě, že atribut má hodnotu, proveďte první položku (položky) v atributu, odebrat úvodní i koncové mezery (Trim) a pak udržovat (vlevo) nejprve 448 znaků v řetězci.

### <a name="do-not-flow-an-attribute"></a>Není toku atributu
Pozadí na scénáři pro tento oddíl, najdete v části [řídit proces toku atributů](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Existují dva způsoby, jak není toku atributu. První je k dispozici v Průvodci instalací a umožňuje [odebrat vybrané atributy](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Tato možnost funguje, pokud byly nikdy synchronizované atribut před. Ale pokud jste spustili synchronizaci tento atribut a později ho odebrat pomocí této funkce, pak zastaví synchronizační modul, Správa atribut a existující hodnoty jsou ponechána ve službě Azure AD.

Pokud chcete odebrat hodnotu atributu a ujistěte se, že není v budoucnu toku, je třeba vytvořit vlastní pravidlo.

Ve firmě Fabrikam uvědomili jsme mít si, že některé atributy, které jsme synchronizace do cloudu by neměl být existuje. Chceme mít jistotu, že tyto atributy se odeberou z Azure AD.  
![Chybný rozšíření atributy](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

* Vytvořit nové pravidlo příchozí synchronizace a naplnit popis ![popisy](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
* Vytvořit toky atributů typu **výraz** a se zdrojem **AuthoritativeNull**. Literálové **AuthoritativeNull** označuje, že hodnota by měla být prázdná v MV i v případě, že nižší prioritu synchronizační pravidlo se pokusí zadání hodnoty.
  ![Transformace pro atributy rozšíření](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
* Uložte pravidlo synchronizace. Spustit **synchronizační služba**, najít konektor, vyberte **spustit**, a **úplná synchronizace**. Tento krok přepočítá všechny toky atributů.
* Ověřte, zda určený změny být exportovány podle hledání prostoru konektoru.
  ![Odstranění dvoufázové instalace](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="create-rules-with-powershell"></a>Vytvoření pravidla pomocí prostředí PowerShell
Pomocí editoru pravidlo synchronizace funguje správně, pokud máte pouze několik změn, aby. Pokud potřebujete provést mnoho změn, může být prostředí PowerShell lepší volbou. Některé pokročilé funkce jsou dostupné v prostředí PowerShell jenom.

### <a name="get-the-powershell-script-for-an-out-of-box-rule"></a>Získat skript prostředí PowerShell pro pravidlo out-of-box
Chcete-li zobrazit PowerShell skriptu vytvořené pravidlo out-of-box, v editoru pravidel synchronizace vyberte pravidlo a klikněte na **exportovat**. Tato akce vám dává skript prostředí PowerShell, který vytvořili pravidlo.

### <a name="advanced-precedence"></a>Pokročilé priorit
Pravidla synchronizace out-of-box začínat hodnotou priority 100. Pokud máte mnoho doménových struktur a budete muset udělat mnoho vlastní změn, nemusí být 99 synchronizační pravidla dostatek.

Můžete určit, aby synchronizační modul, které chcete vložit dříve, než pravidla out-of-box dalších pravidlech. Chcete-li toto chování, postupujte takto:

1. Označit první pravidlo synchronizace out-of-box (toto pravidlo je **v z připojení uživatele AD**) v editoru synchronizační pravidlo a vyberte **exportovat**. Zkopírujte hodnotu SR identifikátor.  
![Prostředí PowerShell před změnou](./media/active-directory-aadconnectsync-change-the-configuration/powershell1.png)  
2. Vytvořte nové pravidlo synchronizace. Editor pravidla synchronizace můžete použít k jeho vytvoření. Pravidlo exportujte do skriptu prostředí PowerShell.
3. Ve vlastnosti **PrecedenceBefore**, vložte identifikátor hodnotu z pravidla out-of-box. Nastavte **přednost** k **0**. Zajistěte, aby atribut identifikátor je jedinečný a nejsou opakovaného použití identifikátoru GUID z jiné pravidlo. Také zkontrolujte, zda **ImmutableTag** není nastavena vlastnost; tato vlastnost by měla být nastavena pouze pro pravidlo out-of-box. Uložení skriptu prostředí PowerShell a spusťte ji. Výsledkem je, že vlastní pravidla je přiřazena přednost před hodnotu 100 a všemi ostatními pravidly out-of-box se zvýší.  
![Prostředí PowerShell po změně](./media/active-directory-aadconnectsync-change-the-configuration/powershell2.png)  

Může mít mnoho vlastní synchronizační pravidla pomocí stejné **PrecedenceBefore** hodnota v případě potřeby.


## <a name="enable-synchronization-of-preferreddatalocation"></a>Povolit synchronizaci PreferredDataLocation
Azure AD Connect podporuje synchronizaci **PreferredDataLocation** atribut pro **uživatele** objekty ve verzi 1.1.524.0 a po. Přesněji řečeno byly zavedeny následující změny:

* Schéma typu objektu **uživatele** v konektor služby Azure AD je rozšířen o PreferredDataLocation atribut, který je typu řetězec a je jedinou hodnotu.

* Schéma typu objektu **osoba** v úložišti Metaverse je rozšířen o PreferredDataLocation atribut, který je typu řetězec a je jedinou hodnotu.

Ve výchozím nastavení není atribut PreferredDataLocation povolit pro synchronizaci, protože neexistuje žádný odpovídající atribut PreferredDataLocation v místní službě Active Directory. Je nutné ručně povolit synchronizaci.

> [!IMPORTANT]
> V současné době Azure AD umožňuje že atribut PreferredDataLocation na synchronizované objekty uživatele a cloud uživatele objekty být přímo konfigurovat pomocí Azure AD PowerShell. Jakmile povolíte synchronizaci PreferredDataLocation atributu, je nutné zastavit pomocí Azure AD PowerShell ke konfiguraci atribut na **synchronizované uživatelské objekty** jako Azure AD Connect se přepíše je na základě Zdroj hodnot atributů v místní službě Active Directory.

> [!IMPORTANT]
> V 1 září 2017, Azure AD nebude povolovat atribut PreferredDataLocation na **synchronizované uživatelské objekty** přímo nakonfigurovat pomocí Azure AD PowerShell. Ke konfiguraci PreferredLocation atribut na synchronizované objekty uživatele, musíte použít jenom Azure AD Connect.

Než povolíte synchronizaci atribut PreferredDataLocation, musíte:

 * Nejdřív se rozhodněte, které místní služby Active Directory atribut má být použit jako zdrojový atribut. Musí být typu **řetězec** a je **jednohodnotové**.

 * Pokud jste dříve nakonfigurovali atribut PreferredDataLocation na existující synchronizované uživatelské objekty ve službě Azure AD pomocí Azure AD PowerShell, je nutné **backport** hodnoty atributů, které mají odpovídající uživatelské objekty v místní služby Active Directory.
 
    > [!IMPORTANT]
    > V takovém případě není backport hodnoty atributů, které mají odpovídající uživatelské objekty ve službě Active Directory v místě Azure AD Connect Odebere existující hodnoty atributu ve službě Azure AD, pokud je povolená synchronizace pro atribut PreferredDataLocation.

 * Je doporučeno, nakonfigurujete zdrojový atribut na alespoň několik místní uživatele AD objekty nyní, který může být použit pro ověření později.
 
Postup povolení synchronizace atributu PreferredDataLocation jde vyhodnotit jako:

1. Zakažte plánovače synchronizace a ověřte, že neexistuje žádná synchronizace v průběhu

2. Přidejte zdrojový atribut pro místní schéma AD Connector.

3. Přidejte PreferredDataLocation do schématu Azure AD Connector.

4. Vytvoření pravidla synchronizace příchozích dat tok hodnota atributu z místní služby Active Directory

5. Vytvořit pravidlo odchozí synchronizace pro tok hodnota atributu do služby Azure AD

6. Spustit úplnou synchronizaci cyklus

7. Povolit synchronizaci plánovače

> [!NOTE]
> Zbývající část tohoto oddílu popisuje tyto kroky v podrobnostech. Jsou popsány v rámci nasazení služby Azure AD s jednou doménovou strukturou topologií a bez vlastní synchronizační pravidla. Pokud máte topologie s více doménovými strukturami, vlastní synchronizační pravidla nakonfigurován nebo mít na testovacím serveru, bude nutné upravit kroky odpovídajícím způsobem.

### <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Krok 1: Zakázat plánovače synchronizace a ověřte, že neexistuje žádná synchronizace v průběhu
Zajistěte, aby že žádná synchronizace bude probíhat, když jste právě aktualizují se pravidla synchronizace, aby se zabránilo neúmyslnému změny se exportují do služby Azure AD. Postup při zakázání plánovače předdefinované synchronizace:

 1. Spusťte relaci prostředí PowerShell na server Azure AD Connect.

 2. Zakážete plánované synchronizace spuštěním rutiny:`Set-ADSyncScheduler -SyncCycleEnabled $false`
 
 3. Spuštění **Synchronization Service Manager** přechodem na spuštění → synchronizační služba.
 
 4. Přejděte na **operace** kartě a potvrďte neprobíhá žádná operace, jejichž stav je *"v průběhu."*

![Zkontrolujte Synchronization Service Manager - žádné operace v průběhu](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step1.png)

### <a name="step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema"></a>Krok 2: Přidejte do místní zdrojový atribut schéma AD Connector.
Všechny atributy AD importují do místní AD prostoru konektoru. Zdrojový atribut přidat do seznamu importované atributy:

 1. Přejděte na **konektory** kartě Synchronization Service Manager.
 
 2. Klikněte pravým tlačítkem na **místní AD Connector** a vyberte **vlastnosti**.
 
 3. V dialogovém okně automaticky otevírané okno, přejděte na **vybrat atributy** kartě.
 
 4. Ujistěte se, že zdrojový atribut je zaškrtnutí v seznamu atributů.
 
 5. Klikněte na tlačítko **OK** uložit.

![Přidejte zdrojový atribut místně schéma AD Connector.](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step2.png)

### <a name="step-3-add-preferreddatalocation-to-the-azure-ad-connector-schema"></a>Krok 3: Přidání PreferredDataLocation schématu Azure AD Connector.
Ve výchozím nastavení není atribut PreferredDataLocation importovány do Azure AD Connect prostoru. Přidat atribut PreferredDataLocation do seznamu importované atributy:

 1. Přejděte na **konektory** kartě Synchronization Service Manager.

 2. Klikněte pravým tlačítkem na **konektoru služby Azure AD** a vyberte **vlastnosti**.

 3. V dialogovém okně automaticky otevírané okno, přejděte na **vybrat atributy** kartě.

 4. Ujistěte se, že je zaškrtnuté atribut PreferredDataLocation v seznamu atributů.

 5. Klikněte na tlačítko **OK** uložit.

![Přidejte zdrojový atribut do schématu Azure AD Connector.](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step3.png)

### <a name="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory"></a>Krok 4: Vytvoření pravidla synchronizace příchozích dat tok hodnota atributu z místní služby Active Directory
Pravidla synchronizace příchozích dat umožňuje hodnota atributu, které jsou předávány z atributu zdroje z místní služby Active Directory do úložiště Metaverse:

1. Spuštění **editoru pravidel synchronizace** tak, že přejdete do editoru pravidla synchronizace. počáteční →.

2. Nastavit filtr hledání **směr** být **příchozí**.

3. Klikněte na tlačítko **přidat nové pravidlo** tlačítko pro vytvoření nového příchozího pravidla.

4. V části **popis** kartě, zadejte následující konfiguraci:
 
    | Atribut | Hodnota | Podrobnosti |
    | --- | --- | --- |
    | Name (Název) | *Zadejte název* | Například *"v ze služby Active Directory – uživatel PreferredDataLocation"* |
    | Popis | *Zadejte popis* |  |
    | Připojený systém | *Vyberte místní AD connector.* |  |
    | Typ objektu systému připojené | **Uživatel** |  |
    | Typ objektu úložiště Metaverse | **Osoba** |  |
    | Typ propojení | **Spojení** |  |
    | Priorita | *Vyberte číslo mezi 1 – 99* | 1 – 99 je vyhrazený pro vlastní synchronizačního pravidla. Není vyberte hodnotu, která je používána jinou synchronizační pravidlo. |

5. Přejděte na **Scoping filtru** kartě a přidejte **jednu skupinu oboru filtru se následující klauzule**:
 
    | Atribut | Operátor | Hodnota |
    | --- | --- | --- |
    | adminDescription | NOTSTARTWITH | Uživatel\_ | 
 
    Oboru filtru určuje, které místní AD objekty že do je použito toto pravidlo příchozí synchronizace. V tomto příkladu používáme stejné oboru filtru použít jako *"v ze služby Active Directory – běžné uživatele"* OOB synchronizační pravidlo, které brání použití pro uživatelské objekty vytvořené pomocí uživatele Azure AD zpětný zápis funkce synchronizačního pravidla. Budete muset upravit oboru filtru, podle vašeho nasazení Azure AD Connect.

6. Přejděte na **transformace karta** a implementovat následující pravidla transformace:
 
    | Typ toku | Atribut target | Zdroj | Použít jednou | Merge – typ |
    | --- | --- | --- | --- | --- |
    | Přímé | PreferredDataLocation | Vyberte zdrojový atribut | Nezaškrtnuto | Aktualizace |

7. Klikněte na tlačítko **přidat** k vytvoření příchozího pravidla.

![Vytvoření pravidla synchronizace příchozích dat](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step4.png)

### <a name="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad"></a>Krok 5: Vytvoření pravidlo odchozí synchronizace pro tok hodnota atributu do služby Azure AD
Pravidlo odchozí synchronizace umožňuje hodnota atributu, které jsou předávány z úložiště Metaverse do atribut PreferredDataLocation ve službě Azure AD:

1. Přejděte na **synchronizační pravidla** Editor.

2. Nastavit filtr hledání **směr** být **odchozí**.

3. Klikněte na tlačítko **přidat nové pravidlo** tlačítko.

4. V části **popis** kartě, zadejte následující konfiguraci:

    | Atribut | Hodnota | Podrobnosti |
    | --- | --- | --- |
    | Name (Název) | *Zadejte název* | Například "se do AAD – uživatel PreferredDataLocation" |
    | Popis | *Zadejte popis* |
    | Připojený systém | *Vyberte konektor AAD* |
    | Typ objektu systému připojené | Uživatel ||
    | Typ objektu úložiště Metaverse | **Osoba** ||
    | Typ propojení | **Spojení** ||
    | Priorita | *Vyberte číslo mezi 1 – 99* | 1 – 99 je vyhrazený pro vlastní synchronizačního pravidla. YDo není vyberte hodnotu, která je používána jinou synchronizační pravidlo. |

5. Přejděte na **Scoping filtru** kartě a přidejte **jednu skupinu oboru filtru se dvěma klauzule**:
 
    | Atribut | Operátor | Hodnota |
    | --- | --- | --- |
    | sourceObjectType | ROVNÁ | Uživatel |
    | cloudMastered | NOTEQUAL | True |

    Určuje oboru filtru, které objekty Azure AD, že je toto pravidlo odchozí synchronizace použito pro. V tomto příkladu používáme stejné oboru filtru z "Na více systémů do AD – identitu uživatele" OOB synchronizační pravidlo. Pravidlo synchronizace zabrání bylo použito pro uživatelské objekty, které nejsou synchronizovány z místní služby Active Directory. Budete muset upravit oboru filtru, podle vašeho nasazení Azure AD Connect.
    
6. Přejděte na **transformace** kartě a implementovat následující pravidla transformace:

    | Typ toku | Atribut target | Zdroj | Použít jednou | Merge – typ |
    | --- | --- | --- | --- | --- |
    | Přímé | PreferredDataLocation | PreferredDataLocation | Nezaškrtnuto | Aktualizace |

7. Zavřít **přidat** vytvoření odchozí pravidla.

![Vytvořit pravidlo odchozí synchronizace](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step5.png)

### <a name="step-6-run-full-synchronization-cycle"></a>Krok 6: Spusťte úplnou synchronizaci cyklu
Obecně je úplná synchronizace cyklus požadované vzhledem k tomu, že jsme přidali nové atributy pro obě služby AD a Azure AD Connector schématu a přináší vlastní synchronizační pravidla. Doporučujeme ověřit změny před exportováním je do Azure AD. Chcete-li ověřit změny při spuštění ručně kroky, které tvoří cyklus úplné synchronizace můžete použít následující kroky. 

1. Spustit **úplný import** krok na **místní AD Connector**:

   1. Přejděte na **Operations** kartě Synchronization Service Manager.

   2. Klikněte pravým tlačítkem na **místní AD Connector** a vyberte **spustit...**

   3. V dialogovém okně automaticky otevírané okno Vyberte **úplný Import** a klikněte na tlačítko **OK**.
    
   4. Počkejte na dokončení operace.

    > [!NOTE]
    > Úplný Import, můžete přeskočit na místní AD Connector. Pokud zdrojový atribut je již zahrnut v seznamu importu atributy. Jinými slovy, můžete neměl k provedení jakékoli změny během [krok 2: přidání atributu zdroje do místní AD Connector. schéma](#step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema).

2. Spustit **úplný import** krok na **konektoru služby Azure AD**:

   1. Klikněte pravým tlačítkem na **konektoru služby Azure AD** a vyberte **spustit...**

   2. V dialogovém okně automaticky otevírané okno Vyberte **úplný Import** a klikněte na tlačítko **OK**.
   
   3. Počkejte na dokončení operace.

3. Ověření změny pravidel synchronizace na existující objekt uživatele:

Zdrojový atribut z místního služby Active Directory a PreferredDataLocation z Azure AD byly naimportovány do příslušných prostoru konektor. Než budete pokračovat v kroku úplnou synchronizaci, je doporučeno, abyste provedli **Preview** na stávajícího uživatele objekt v místní AD prostoru konektoru. Objekt, který jste vybrali měli zdrojový atribut naplněno. Úspěšně **Preview** s PreferredDataLocation vložené do úložiště Metaverse je dobré indikátoru, které jste nakonfigurovali synchronizaci pravidla správně. Informace o tom, jak provést **Preview**, informace naleznete v sekci [změnu ověřit](#verify-the-change).

4. Spustit **úplná synchronizace** krok na **místní AD Connector**:

   1. Klikněte pravým tlačítkem na **místní AD Connector** a vyberte **spustit...**
  
   2. V dialogovém okně automaticky otevírané okno Vyberte **úplná synchronizace** a klikněte na tlačítko **OK**.
   
   3. Počkejte na dokončení operace.

5. Ověřte **čekajících exportů** do služby Azure AD:

   1. Klikněte pravým tlačítkem na **konektoru služby Azure AD** a vyberte **hledání prostoru konektoru**.

   2. V dialogovém okně hledání prostoru konektoru automaticky otevírané okno:

      1. Nastavit **oboru** k **čekající na vyřízení Export**.
      
      2. Zkontrolujte všechny tři políčka, včetně **přidat, upravit a odstranit**.
      
      3. Klikněte **vyhledávání** tlačítko získat seznam objektů se změnami pro export. Chcete-li zkontrolovat změny pro daný objekt, dvakrát klikněte na objekt.
      
      4. Ověřte, že se očekává změny.

6. Spustit **exportovat** krok na **Azure AD Connector.**
      
   1. Klikněte pravým tlačítkem myši **konektoru služby Azure AD** a vyberte **spustit...**
   
   2. V dialogovém okně Spustit konektor automaticky otevírané okno, vyberte **exportovat** a klikněte na tlačítko **OK**.
   
   3. Počkejte, než pro Export do Azure AD pro dokončení.

> [!NOTE]
> Můžete si všimnout, že kroky nezahrnují krok úplná synchronizace a Export kroku na konektor služby Azure AD. Vzhledem k tomu, že jsou hodnoty atributu odesílaných z místní služby Active Directory do Azure AD pouze, nejsou nutné kroky.

### <a name="step-7-re-enable-sync-scheduler"></a>Krok 7: Opětovné povolení plánovače synchronizace
Znovu povolte plánovače předdefinované synchronizace:

1. Spusťte relaci prostředí PowerShell.

2. Plánované synchronizace znovu povolte spuštěním rutiny:`Set-ADSyncScheduler -SyncCycleEnabled $true`



## <a name="next-steps"></a>Další kroky
* Další informace o konfiguraci modelu v [Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Další informace o jazyk výrazů v [Principy deklarativní zřizování výrazy](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Témata s přehledem**

* [Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
