---
title: 'Synchronizace Azure AD Connect: Principy hello architektura | Microsoft Docs'
description: "Toto téma popisuje architekturu hello synchronizace Azure AD Connect a vysvětluje hello termínů používaných."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 465bcbe9-3bdd-4769-a8ca-f8905abf426d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 9fb979fcf8feb7b4d406789102239480b0b4bc94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understanding-hello-architecture"></a>Synchronizace Azure AD Connect: Principy architektura hello
Toto téma popisuje základní architekturu hello pro synchronizaci Azure AD Connect. V mnoha aspektům je podobné tooits Předchůdci serveru MIIS 2003 ILM 2007 a FIM 2010. Synchronizace Azure AD Connect je hello vývoj těchto technologií. Pokud jste obeznámeni s žádným z těchto starších technologií, hello obsah tohoto tématu bude také známé tooyou. Pokud jste nový toosynchronization, toto téma je pro vás. Není ale hello podrobnosti tooknow požadavek na toto téma toobe úspěšné při vytvoření vlastního nastavení synchronizace tooAzure AD Connect (volané synchronizační modul v tomto tématu).

## <a name="architecture"></a>Architektura
synchronizační modul Hello ucelený přehled objekty, které jsou uložené v několika připojených zdrojů dat vytváří a spravuje informace o identitě v těchto zdrojů dat. Toto integrované zobrazení je určen podle informací o identitě hello načíst z připojených zdrojů dat a sada pravidel, které určují, jak tooprocess tyto informace.

### <a name="connected-data-sources-and-connectors"></a>Konektory a připojených zdrojů dat
synchronizační modul Hello zpracovává informace o identitě z různých datových úložišť, jako je například služby Active Directory nebo databázi serveru SQL Server. Každý dat úložiště, který slouží k uspořádání jeho data ve formátu databázového typu, který poskytuje metody standardní přístup k datům je potenciální kandidátem zdroje dat pro synchronizační modul hello. Hello úložiště dat, které jsou synchronizovány pomocí synchronizační modul se nazývají **připojení zdroje dat** nebo **připojení adresáře** (CD).

synchronizační modul Hello zapouzdří interakci s připojeného zdroje dat v rámci modul s názvem **konektor**. Každý typ připojeného zdroje dat má konkrétní konektor. Hello konektor překládá požadované operace do hello formátu, který hello připojené datového zdroje rozumí.

Konektory provádět volání rozhraní API informací o identitě tooexchange (čtení a zápisu) s připojeného zdroje dat. Je také možné tooadd vlastní konektor pomocí hello extensible connectivity framework. Hello následující obrázek ukazuje, jak konektor připojí modul připojených dat toohello zdroje synchronizace.

![Arch1](./media/active-directory-aadconnectsync-understanding-architecture/arch1.png)

V obou směrech můžete toku dat, ale jeho nelze současně toku v obou směrech. Jinými slovy konektor může být nakonfigurované tooallow data tooflow z hello připojené datový zdroj toosync modul nebo synchronizační modul toohello připojeného zdroje dat, ale jenom některé z těchto operací může nastat v jednom okamžiku pro jeden objekt a atribut. Směr Hello se může lišit pro různé objekty a pro různé atributy.

tooconfigure konektor, určíte, že objekt hello typy, které chcete toosynchronize. Určení typů objektů hello definuje hello rozsah objektů, které jsou součástí procesu synchronizace hello. dalším krokem Hello je tooselect hello atributy toosynchronize, což se označuje jako seznam povolených atribut. Tato nastavení můžete změnit kdykoli v odpovědi toochanges tooyour obchodní pravidla. Pokud použijete Průvodce instalací hello Azure AD Connect, tato nastavení jsou konfigurována pro vás.

tooexport objekty tooa připojeného zdroje dat, seznam zahrnutí atributů hello musí obsahovat alespoň minimální hello atributy požadované toocreate do připojeného zdroje dat zadejte určitý objekt. Například hello **sAMAccountName** atributu musí být zahrnut v hello atribut zahrnutí seznamu tooexport objekt uživatele tooActive adresáře, protože musí mít všechny uživatelské objekty ve službě Active Directory **sAMAccountName**  definován atribut. Průvodce instalací hello znovu, nemá tato konfigurace za vás.

Pokud hello připojeného zdroje dat používá strukturální komponenty, například objekty tooorganize oddíly nebo kontejnerů, můžete omezit hello oblastí v hello připojeného zdroje dat, které se používají pro danou řešení.

### <a name="internal-structure-of-hello-sync-engine-namespace"></a>Interní struktury hello synchronizační modul oboru názvů
Hello celý synchronizační modul obor názvů se skládá ze dvou obory názvů, které obsahují informace o identitě hello. jsou dva obory názvů Hello:

* prostoru konektoru Hello (CS)
* úložiště metaverse Hello (MV)

Hello **prostoru konektoru** je pracovní oblasti, která obsahuje reprezentace hello určené objekty z připojených dat zdroje a hello atributy určená v seznamu povolených atribut hello. synchronizační modul Hello používá toodetermine místa konektoru hello co se změnilo v hello připojené datového zdroje a toostage příchozí změny. synchronizační modul Hello používá také toostage místa konektoru hello odchozí změny pro export toohello připojeného zdroje dat. synchronizační modul Hello udržuje prostoru odlišné konektoru jako pracovní oblast pro každý konektor.

Pomocí pracovní oblasti hello synchronizační modul zůstává nezávislé hello připojené zdroje dat a nemá vliv jejich dostupnost a usnadnění přístupu. V důsledku toho může zpracovat informace o identitě kdykoli pomocí hello data v pracovní oblasti hello. synchronizační modul Hello může požádat o pouze hello změny provedené v hello připojeného zdroje dat, protože hello poslední komunikace relace ukončena nebo nabízené pouze hello změny tooidentity informace, které hello připojeného zdroje dat ještě neobdržel, což snižuje Hello síťový provoz mezi hello synchronizační modul a hello připojeného zdroje dat.

Kromě toho synchronizační modul ukládá informace o všech objektů, zpracuje v prostoru konektoru hello stavu. Odeslaná nová data, synchronizační modul vždy vyhodnotí, zda text hello dat již byly synchronizovány.

Hello **úložiště metaverse** je úložiště obsahující informace o identitě hello agregovat z více připojených zdrojů dat, poskytuje jednotný pohled globální, integrované součet všech objektů. Podle informací o identitě hello, které se načítají ze zdroje dat hello připojené a sada pravidel, které vám umožňují proces synchronizace hello toocustomize jsou vytvořeny objekty úložiště Metaverse.

Hello následující obrázek znázorňuje hello konektor obor názvů a oboru názvů metaverse hello v rámci hello synchronizační modul.

![Arch2](./media/active-directory-aadconnectsync-understanding-architecture/arch2.png)

## <a name="sync-engine-identity-objects"></a>Objekty identity modul synchronizace
reprezentace buď objektů v hello připojeného zdroje dat jsou objekty Hello v hello synchronizační modul nebo hello integrované zobrazení, které synchronizovat modul má těchto objektů. Každý objekt synchronizační modul musí mít globálně jedinečný identifikátor (GUID). Identifikátory GUID zajištění integrity dat a express relace mezi objekty.

### <a name="connector-space-objects"></a>Objekty prostoru konektoru
Pokud synchronizační modul komunikuje s připojeného zdroje dat, čte informace o identitě hello v hello připojeného zdroje dat a používá tento informace toocreate znázornění hello identity objektu v prostoru konektoru hello. Nelze vytvořit nebo odstranit tyto objekty jednotlivě. Můžete však ručně odstranit všechny objekty v prostoru konektoru.

Všechny objekty v prostoru konektoru hello mají dva atributy:

* Globálně jedinečný identifikátor (GUID)
* Rozlišující název (DN)

Pokud připojení hello přiřadí objekt toohello jedinečný atribut zdroje dat a potom objekty v prostoru konektoru hello může mít také atributem ukotvení. atribut kotvy Hello jednoznačně identifikuje objektů v hello připojeného zdroje dat. synchronizační modul Hello používá hello připojeného zdroje dat hello ukotvení toolocate hello odpovídající reprezentace tohoto objektu. Synchronizační modul se předpokládá, že hello ukotvení objektu nikdy změny přes hello životnosti objektu hello.

Mnoho hello konektorů známé jedinečný identifikátor toogenerate anchor automaticky používat pro každý objekt při jeho importu. Například hello konektor služby Active Directory používá hello **objectGUID** atribut pro element anchor. Pro připojené datových zdrojů, které neposkytují jasně definované jedinečný identifikátor můžete určit generaci ukotvení jako součást konfigurace konektoru hello.

V tom případě hello ukotvení vychází z jednoho nebo více jedinečné atributy objektu typ, ani změny, které a který jednoznačně identifikuje objekt hello v prostoru konektoru hello (například číslo zaměstnance nebo ID uživatele).

Objekt místa konektoru může být jedna z následujících hello:

* Pracovní objekt
* Zástupný text

### <a name="staging-objects"></a>Pracovní objekty
Objekt pracovní představuje instanci hello určené typy objektů z hello připojeného zdroje dat. Kromě toho toohello GUID a rozlišující název hello pracovní objekt má vždy hodnotu, která určuje typ objektu hello.

Pracovní objekty, které byly naimportovány vždy obsahovat hodnotu pro atribut kotvy hello. Pracovní objekty, které byly nově zajištěny synchronizační modul a jsou v hello proces vytváření v hello připojeného zdroje dat nemají hodnotu pro atribut kotvy hello.

Pracovní objekty také provádění aktuálních hodnot atributů obchodní a provozní informace potřebné pro proces synchronizace synchronizační modul tooperform hello. Provozní informace zahrnují příznaky, které indikují hello typ aktualizací, které jsou připravené na hello pracovní objekt. Pokud objekt pracovní obdržel nové informace o identitě z hello připojeného zdroje dat, ještě nebyla zpracována, je objekt hello označení **čekajícího na import**. Pokud pracovní objekt má nové informace o identitě, která dosud nebyla exportovaný toohello připojeného zdroje dat, je označení **čekající na vyřízení export**.

Pracovní objekt může být objekt import nebo export objektu. synchronizační modul Hello vytvoří objekt import pomocí objektu informací od hello připojeného zdroje dat. Informace o hello existence nový objekt, který odpovídá jednomu z vybraných v hello konektor typy objektů hello přijetí synchronizační modul vytvoří objekt importu v prostoru konektoru hello jako reprezentace objektu hello v hello připojeného zdroje dat.

Hello následující obrázek znázorňuje, import objekt, který reprezentuje objekt v hello připojeného zdroje dat.

![Arch3](./media/active-directory-aadconnectsync-understanding-architecture/arch3.png)

synchronizační modul Hello vytvoří objekt export pomocí informací o objektu v úložišti metaverse hello. Export objekty jsou exportovaný toohello připojeného zdroje dat během hello další komunikace relace. Z hlediska pohledu hello hello synchronizační modul se export objekty neexistují v hello připojeného zdroje dat ještě. Atribut kotvy hello export objektu proto není k dispozici. Po přijetí hello objekt z synchronizační modul, hello připojeného zdroje dat vytvoří jedinečnou hodnotu pro atribut kotvy hello hello objektu.

Hello následující obrázek ukazuje, jak je vytvořen objekt export pomocí informací o identitě v úložišti metaverse hello.

![Arch4](./media/active-directory-aadconnectsync-understanding-architecture/arch4.png)

synchronizační modul Hello potvrdí hello export objektu hello podle opakovaném importování hello objekt z hello připojeného zdroje dat. Export objektů po synchronizační modul je obdrží během importu další hello z tohoto připojeného zdroje dat, se import objektů.

### <a name="placeholders"></a>Zástupné symboly
Hello synchronizační modul používá objekty toostore plochý obor názvů. Ale některé připojených zdrojů dat jako je Active Directory pomocí hierarchického oboru názvů. tootransform informace z hierarchického oboru názvů do plochý obor názvů, synchronizační modul používá zástupné symboly toopreserve hello hierarchie.

Každý zástupný symbol představuje komponentu hierarchické název objektu, který nebyl importován do synchronizační modul, ale je hierarchická název požadované tooconstruct hello (například organizační jednotku). Jejich vyplnění mezer vytvořené odkazy v tooobjects zdroje dat hello připojené, který nejsou pracovní objekty v prostoru konektoru hello.

synchronizační modul Hello používá také zástupné symboly toostore odkazuje objekty, které dosud nebyl importován. Například, pokud sync je nakonfigurovaná tooinclude hello manager atribut pro hello *Abbie Spencer* objektu a hello přijatá hodnota je objekt, který ještě nebyl dosud, například importované *CN = Lee Sperry, CN = Users, DC = fabrikam, DC = com*, hello manager informace jsou uloženy jako zástupné symboly v prostoru konektoru hello. Pokud je objekt správce hello později importovat, hello zástupný objekt přepsány hello pracovní objekt, který představuje hello správce.

### <a name="metaverse-objects"></a>Objekty úložiště Metaverse
Objekt úložiště metaverse obsahuje zobrazení hello agregovat tento synchronizační modul je dobrý den pracovní objekty v prostoru konektoru hello. Synchronizační modul vytváří úložiště metaverse objektů pomocí hello informace v import objektů. Několik objekty místa konektoru může být propojené tooa metaverse jeden objekt, ale objekt místa konektoru nemůže být propojené toomore než jednoho objektu metaverse.

Úložiště Metaverse objekty nelze ručně vytvořit nebo odstranit. synchronizační modul Hello automaticky odstraní úložiště metaverse objekty, které nemají objekt spojení konektoru tooany místa v prostoru konektoru hello.

toomap objektů v rámci připojené tooa odpovídající typ objektu zdroje dat v úložišti metaverse hello, synchronizační modul poskytuje rozšiřitelné schéma s předdefinovanou sadu typy objektů a přidružených atributů. Můžete vytvořit nové typy objektů a atributy pro objekty úložiště metaverse. Atributy mohou být jednoho nebo více hodnot a typy hello atributů může být řetězce, odkazy, čísla a logické hodnoty.

### <a name="relationships-between-staging-objects-and-metaverse-objects"></a>Vztahy mezi pracovní objekty a objekty úložiště metaverse
V rámci oboru názvů hello synchronizační modul je ve hello odkaz vztah mezi pracovní objekty a úložiště metaverse objekty povolené hello datový tok. Pracovní objekt tedy objektu úložiště metaverse propojené tooa nazývá **připojený k objektu** (nebo **objekt konektoru**). Je volána pracovní objekt, který není propojené tooa objektu úložiště metaverse **odpojování objekt** (nebo **odpojovače objekt**). podmínky Hello připojen a odpojování jsou upřednostňované toonot matou s hello konektory zodpovědná za importu a exportu dat z připojeného adresáře.

Zástupné znaky jsou nikdy propojené tooa objektu úložiště metaverse

Připojený k objektu obsahuje pracovní objekt a jeho objekt jednoho úložiště metaverse tooa propojené relace. Připojených objektů jsou hodnoty atributu použité toosynchronize mezi objektem prostor konektoru a objektu úložiště metaverse.

Když pracovní objekt během synchronizace nebude připojený k objektu, můžete mezi hello pracovní objekt a objektu úložiště metaverse hello toku atributů. Tok atributů obousměrný a se konfiguruje pomocí pravidel atribut import a export atribut pravidel.

Objekt místo jeden konektor může být propojené tooonly metaverse jeden objekt. Každý objekt úložiště metaverse však může být propojené toomultiple objekty konektoru místo v hello stejný nebo jiný konektor mezery, jak ukazuje následující obrázek hello.

![Arch5](./media/active-directory-aadconnectsync-understanding-architecture/arch5.png)

vztah mezi pracovní objekt hello propojené Hello a objektu úložiště metaverse je trvalé a lze odebrat pouze pomocí pravidel, která jste zadali.

Objekt rozděleným je pracovní objekt, který není propojené tooany objektu úložiště metaverse. Hello atribut, který hodnoty objekt rozděleným nejsou zpracovány žádné další v úložišti metaverse hello. Hello atribut, který hodnoty hello odpovídající objektu v hello připojeného zdroje dat nejsou aktualizovat synchronizační modul.

Pomocí rozděleným objektů můžete ukládat informace o identitě v synchronizační modul a později ji zpracovat. Zachovat pracovní objekt jako objekt rozděleným v prostoru konektoru hello má mnoho výhod. Protože hello systému je již dvoufázové instalace hello požadované informace o tomto objektu, není nutné toocreate reprezentace tohoto objektu během hello znovu importovat další z hello připojeného zdroje dat. Tímto způsobem, synchronizační modul má kompletní snímek hello připojeného zdroje dat, vždy i v případě, že není žádná aktuální připojení toohello připojeného zdroje dat. Rozděleným objekty mohou být převedeny do připojených objektů a naopak a v závislosti na hello pravidla, které zadáte.

Import objektu je vytvořit jako objekt odpojené. Export objektu musí být připojený k objektu. Hello systému logiku vynucuje toto pravidlo a odstraní každý export objekt, který není připojený k objektu.

## <a name="sync-engine-identity-management-process"></a>Procesu synchronizace modul identity management
proces správy identity Hello řídí, jak je aktualizovat informace o identitě mezi různé připojených zdrojů dat. Správa identit dojde v tři procesy:

* Import
* Synchronizace
* Export

Během procesu importu hello vyhodnotí synchronizační modul hello příchozí identity informace z připojeného zdroje dat. Při zjištění změny, je buď vytvoří nové pracovní objekty, nebo existující pracovní objekty v prostoru konektoru hello pro synchronizaci aktualizací.

Během procesu synchronizace hello synchronizační modul aktualizuje hello metaverse tooreflect změny, ke kterým došlo v prostoru konektoru hello a aktualizuje hello konektoru místo tooreflect změny, ke kterým došlo v úložišti metaverse hello.

Během procesu exportu hello se synchronizační modul přenáší změny, které jsou připravené na pracovní objekty a které jsou označeny jako čekající na export.

Hello následující obrázek ukazuje, kde každý z procesů hello dojde k jako toky identity informace z tooanother jeden připojený datový zdroj.

![Arch6](./media/active-directory-aadconnectsync-understanding-architecture/arch6.png)

### <a name="import-process"></a>Proces importu
Během procesu importu hello vyhodnotí synchronizační modul aktualizace tooidentity informace. Synchronizační modul porovná informace o identitě hello přijal od hello připojeného zdroje dat s informací o identitě hello o pracovní objektu a určuje, zda text hello pracovní objekt vyžaduje aktualizace. Pokud je nutné tooupdate hello pracovní objekt se nová data, hello pracovní objekt je označený jako čekajícího na import.

Pomocí pracovní objekty v prostoru konektoru hello před synchronizací, synchronizační modul může zpracovat pouze informace identity hello, který byl změněn. Tento proces zajišťuje hello následující výhody:

* **Efektivní synchronizace**. je minimalizován Hello objem dat zpracovaných během synchronizace.
* **Efektivní Opakovaná synchronizace**. Jak synchronizační modul zpracovává informace o identitě bez opětovně se připojujícího hello synchronizační modul toohello datového zdroje, můžete změnit.
* **Možnost toopreview synchronizace**. Můžete zobrazit náhled synchronizace tooverify správnost předpokladů o procesu správy identit hello.

Pro každý objekt zadaný v hello konektor hello synchronizační modul poprvé pokusí toolocate reprezentace objektu hello v prostoru konektoru hello hello konektor. Synchronizační modul prozkoumá všechny pracovní objekty v prostoru konektoru hello a pokusí toofind odpovídající pracovní objekt, který má odpovídající atribut ukotvení. Pokud žádné existující pracovní objekt má odpovídající ukotvení atribut, synchronizaci toofind pokusů modul odpovídající pracovní objekt s hello stejné rozlišující název.

Když synchronizační modul vyhledá pracovní objekt, který odpovídá rozlišující název, ale není anchor, hello speciální očekávejte toto chování:

* Pokud má objekt hello v prostoru konektoru hello žádná kotva pak synchronizační modul odebere prostoru konektoru hello tento objekt a značky hello objektu úložiště metaverse je propojené tooas **opakujte zřizování při další synchronizaci spustit**. Pak vytvoří nový objekt import hello.
* Pokud objekt hello v prostoru konektoru hello anchor, synchronizační modul se předpokládá, že tento objekt buď přejmenován nebo odstraněn v hello připojený adresář. Přiřadí dočasné, nové rozlišující název objektu prostoru konektoru hello tak, aby ho můžete Příprava hello příchozí objekt. Hello původního objektu se pak stane **přechodný**, čekání hello konektor tooimport hello přejmenování nebo odstranění tooresolve hello situaci.

Pokud synchronizační modul vyhledá pracovní objekt, který odpovídá toohello objekt zadaný v hello konektor, určuje, jaký druh tooapply změny. Synchronizační modul může přejmenovat nebo odstranit objekt hello v hello připojeného zdroje dat nebo může aktualizovat pouze hodnoty atributů objektu hello.

Pracovní objekty s aktualizovaná data jsou označené jako čekajícího na import. Různé typy čekající na vyřízení importy jsou k dispozici. V závislosti na hello výsledek procesu importu hello pracovní objekt v prostoru konektoru hello má jeden hello následující typy čekajícího importu:

* **Žádný**. Žádné změny tooany hello atributů hello pracovní objekt nejsou k dispozici. Synchronizační modul není příznak tento typ jako čekajícího na import.
* **Přidat**. Hello pracovní objekt je nový objekt importu v prostoru konektoru hello. Synchronizační modul příznaky tento typ jako čekajícího na import pro další zpracování v úložišti metaverse hello.
* **Aktualizace**. Synchronizační modul najde odpovídající pracovní objekt v prostoru konektoru hello a označí tento typ jako čekajícího na import, aby atributy toohello aktualizace lze zpracovat v úložišti metaverse hello. Aktualizace zahrnují přejmenování objektu.
* **Odstranit**. Synchronizační modul najde odpovídající pracovní objekt v prostoru konektoru hello a označí tento typ jako čekajícího na import, aby hello připojený k objektu může být odstraněny.
* **Odstranit nebo přidat**. Synchronizační modul najde odpovídající pracovní objekt v hello prostoru konektoru, ale typy objektů hello se neshodují. V takovém případě odstranění přidat úpravy je připravený. A delete přidat úpravy označuje toohello synchronizační modul, který dokončení opakované synchronizace tohoto objektu musí dojít, protože různé sady pravidel použít toothis objektu, když se změní typ objektu hello.

Nastavení hello čekající na vyřízení stav importu pracovní objektu, se dá tooreduce výrazně hello objem dat zpracovaných během synchronizace, protože tak učiníte, umožníte hello systému tooprocess pouze ty objekty, které byly aktualizovány data.

### <a name="synchronization-process"></a>Procesu synchronizace
Synchronizace se skládá ze dvou souvisejících procesů:

* Příchozí synchronizace, když obsah hello hello metaverse se aktualizuje pomocí hello data v prostoru konektoru hello.
* Odchozí synchronizace, při aktualizaci obsahu hello místa konektoru hello pomocí dat v úložišti metaverse hello.

Pomocí hello informace dvoufázové instalace v prostoru konektoru hello hello proces synchronizace příchozích dat vytvoří v zobrazení hello integrované úložiště metaverse hello hello data, která je uložená v hello připojené zdroje dat. Všechny pracovní objekty nebo pouze ty informace čekajícího importu nejsou agregovány, v závislosti na tom, jak jsou nakonfigurované hello pravidla.

Hello odchozí synchronizace proces aktualizace exportovat objekty při změně objektů v metaverse.

Příchozí synchronizace vytvoří zobrazení hello integrované v úložišti metaverse hello hello identity informací, které při obdržení ze zdroje dat hello připojen. Synchronizační modul může zpracovat informace o identitě kdykoli pomocí hello nejnovější informace o identitě, byl ze hello připojený zdroj dat.

**Synchronizace příchozích dat**

Synchronizace příchozích dat zahrnuje hello následující procesy:

* **Zřízení** (také nazývané **projekce** Pokud je důležité toodistinguish tento proces z odchozí synchronizace zřizování). Synchronizační modul Hello vytvoří nový objekt úložiště metaverse založeného na pracovní objektu a jejich spojení. Zřizování je operace úrovni objektů.
* **Připojení k**. Synchronizační modul Hello odkazů pracovní objekt tooan existující objektu úložiště metaverse. Spojení je operace úrovni objektů.
* **Import toku atributů**. Synchronizační modul aktualizuje hello hodnoty atributu, názvem toku atributů hello objektu v úložišti metaverse hello. Import toku atributů je úroveň atributu operaci, která vyžaduje odkaz mezi objektem pracovní a objektu úložiště metaverse.

Zřizování je hello pouze proces, který vytvoří objektů v hello metaverse. Zřízení ovlivňuje pouze importovat objekty, které jsou odpojené objekty. Synchronizační modul během zřizování, vytvoří objekt úložiště metaverse, který odpovídá typu objektu toohello hello import objektu a vytvoří vazbu mezi oba objekty, tedy vytvoření připojený k objektu.

Proces připojení Hello také vytvoří propojení mezi import objektů a objektu úložiště metaverse. Hello rozdíl mezi spojení a zřízení je, že proces připojení hello vyžaduje tento objekt import hello jsou propojené tooan existující objektu úložiště metaverse, kde proces zřízení hello vytvoří nový objekt úložiště metaverse.

Synchronizační modul se pokusí toojoin objekt import tooa objektu úložiště metaverse pomocí kritéria, která je zadaná v konfiguraci hello synchronizační pravidlo.

Synchronizační modul během zřizování hello a procesy spojení, odkazy na samostatném objektu tooa úložiště metaverse objekt přitom připojené k. Po dokončení těchto operací úrovni objektů můžete aktualizovat synchronizační modul hello hodnot atributů objektu úložiště metaverse přidružené hello. Tento proces se nazývá toku atributů importu.

Tok atributů importu dojde na všechny importovat objekty, které bude přenášet nová data a jsou propojené tooa objektu úložiště metaverse.

**Odchozí synchronizace**

Při změně objektu úložiště metaverse, ale neodstraní, odchozí synchronizace aktualizací exportovat objekty. cílem Hello odchozí synchronizace je tooevaluate, zda změny toometaverse objekty vyžadují aktualizace toostaging objekty v prostor konektoru hello. V některých případech hello změny může vyžadovat, aby pracovní objekty ve všech prostor konektoru aktualizovat. Pracovní objekty, které se změnily jsou označeny jako čekající na vyřízení exportu, přitom exportovat objekty. Tyto exportu objekty jsou později instalováni toohello připojeného zdroje dat během procesu exportu hello.

Odchozí synchronizace má tři procesy:

* **Zřizování**
* **Zrušení zřízení**
* **Export toku atributů**

Zajišťování a rušení zajištění jsou operace úrovni objektů. Zrušení zřízení závisí na zřizování, protože pouze zřizování můžete spustit ho. Zrušení zřízení se aktivuje při zřizování odebere hello propojení mezi objektu úložiště metaverse a export objektu.

Zřizování se vždy aktivuje, když jsou použité tooobjects v úložišti metaverse hello změny. Při změně toometaverse objekty, synchronizační modul můžete provádět následující úlohy jako součást procesu zřizování hello hello:

* Vytvoření připojených objektů, kde je objekt úložiště metaverse propojené tooa nově vytvořený objekt export.
* Přejmenování připojený k objektu.
* Odpojte propojení mezi objektu úložiště metaverse a pracovní objekty, vytvoření rozděleným objektu.

Pokud zřizování vyžaduje synchronizační modul toocreate nový objekt konektoru, hello pracovní objektu úložiště metaverse hello toowhich objektu je propojené je vždy export objektu, protože objekt hello v hello připojeného zdroje dat ještě neexistuje.

Pokud zřizování vyžaduje synchronizační modul toodisjoin připojený k objektu, vytváření objektu rozděleným zrušení zřízení se aktivuje. Hello proces zrušení zřízení odstraní objekt hello.

Během zrušení zřízení, odstraněním objekt export nedojde k odstranění fyzicky hello objektu. Hello objektu je označení **odstranit**, což znamená, že operace odstranění hello se připraví se na objekt hello.

Export toku atributů také dojde během procesu hello odchozí synchronizace, podobně jako toohello tak, aby importovat toku atributů spadá příchozí synchronizace. Tok atributů export nastávají jenom mezi úložiště metaverse a export objektů, které jsou připojené.

### <a name="export-process"></a>Proces exportu
Během procesu exportu hello se synchronizační modul hledá všechny objekty export, které jsou označeny jako čekající na export v prostoru konektoru hello a pak odešle aktualizace toohello připojení zdroje dat.

synchronizační modul Hello můžete určit úspěšný hello exportu, ale nemůže dostatečně zjistit, že byl dokončen proces správy identity hello. Objekty v hello připojeného zdroje dat můžete kdykoli změnit jinými procesy. Vzhledem k tomu, že synchronizační modul nemá trvalé připojení toohello připojeného zdroje dat, není dostatek toomake předpoklady o hello vlastnosti objektu v hello připojeného zdroje dat podle pouze oznámení o úspěšném exportu.

Například procesu v hello připojené zdroje dat, může atributů objektu hello změnu zpět tootheir původní hodnoty (tj, hello připojeného zdroje dat může přepsat hodnoty hello ihned po hello data odesílána odhlašování synchronizační modul a úspěšně použití v hello připojeného zdroje dat).

Hello synchronizační modul úložiště exportovat a importovat informace o jednotlivých pracovních objektů stavu. Pokud se hodnoty hello atributů, které jsou určené v seznamu povolených atribut hello změnily od poslední export hello, hello úložiště import a export stav umožňuje synchronizační modul tooreact správně. Synchronizační modul používá hello import proces tooconfirm hodnoty atributů, které byly exportovaný toohello připojeného zdroje dat. Porovnání mezi hello importovat a exportované informace, jak ukazuje následující obrázek, hello umožňuje synchronizační modul toodetermine zda hello exportu byla úspěšná, nebo pokud vyžaduje toobe opakuje.

![Arch7](./media/active-directory-aadconnectsync-understanding-architecture/arch7.png)

Například pokud synchronizační modul exportuje atribut C, která má hodnotu 5, tooa připojení zdroje dat, C = 5 je uložený v jeho export stav paměť. Každý další export na tento objekt dochází pokusu o tooexport C = 5 toohello připojeného zdroje dat znovu protože synchronizační modul se předpokládá, tato hodnota nebyl trvale použité toohello objektu (Pokud se v poslední době importovali jinou hodnotu z hello připojení zdroje dat). Hello export paměti není zaškrtnuto, když je obdržena C = 5 během operace importu v objektu hello.

## <a name="next-steps"></a>Další kroky
Další informace o hello [synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfigurace.

Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).

