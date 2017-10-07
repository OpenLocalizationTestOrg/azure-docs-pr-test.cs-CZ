---
title: aaaGeneric konektor SQL | Microsoft Docs
description: "Tento článek popisuje, jak tooconfigure Microsoft obecné konektor SQL."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: fd8ccef3-6605-47ba-9219-e0c74ffc0ec9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 2eab8f0894e83ab4738b9f2deb05b03cdc9a9d43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="generic-sql-connector-technical-reference"></a>Technické informace o obecné konektor SQL
Tento článek popisuje hello obecné konektor SQL. Hello článek se týká toohello následující produkty:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manageru 2010 R2 (FIM2010R2)
  * Musíte použít opravu hotfix 4.1.3671.0 nebo novější [KB3092178](https://support.microsoft.com/kb/3092178).

Pro MIM2016 a FIM2010R2, je k dispozici ke stažení z hello hello konektor [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

toosee tento konektor v praxi, najdete v části hello [obecné konektor SQL podrobné](active-directory-aadconnectsync-connector-genericsql-step-by-step.md) článku.

## <a name="overview-of-hello-generic-sql-connector"></a>Přehled hello obecné konektor SQL
Hello obecné konektor SQL umožňuje toointegrate hello synchronizační služba s databáze systémem, který nabízí připojení ODBC.  

Z hlediska vysoké úrovně podporuje hello aktuální verzi konektoru hello hello následující funkce:

| Funkce | Podpora |
| --- | --- |
| Připojeného zdroje dat |Hello konektor podporuje všechny ovladače ODBC 64-bit. Byl testován s hello následující: <li>Microsoft SQL Server a SQL Azure</li><li>IBM DB2 10.x</li><li>IBM DB2 9.x</li><li>Oracle 10 a 11 g</li><li>MySQL 5.x</li> |
| Scénáře |<li>Správa životního cyklu objektu</li><li>Správa hesel</li> |
| Operace |<li>Úplný Import a rozdílový Import, Export</li><li>Pro Export: Přidání, aktualizaci, odstranit a nahradit</li><li>Nastavení hesla, změnit heslo</li> |
| Schéma |<li>Dynamické zjišťování objektů a atributů</li> |

### <a name="prerequisites"></a>Požadavky
Než použijete hello konektor, zkontrolujte, zda že máte na serveru pro synchronizaci hello hello následující:

* 4.5.2 rozhraní Microsoft .NET Framework nebo novější
* 64bitová verze ovladače klienta rozhraní ODBC

### <a name="permissions-in-connected-data-source"></a>Oprávnění v připojeného zdroje dat
toocreate nebo proveďte jednu z úloh hello podporované v obecné SQL konektor, musí mít:

* db_datareader
* db_datawriter

### <a name="ports-and-protocols"></a>Porty a protokoly
Hello porty vyžadované pro toowork ovladač ODBC hello v dokumentaci dodavatele databáze hello.

## <a name="create-a-new-connector"></a>Vytvořit nový konektor
tooCreate konektor obecné SQL v **synchronizační služba** vyberte **agenta pro správu** a **vytvořit**. Vyberte hello **obecné SQL (Microsoft)** konektor.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Připojení
Hello konektor používá soubor název DSN rozhraní ODBC pro připojení k síti. Vytvořte pomocí souboru DSN hello **zdroje dat ODBC** najít v nabídce start hello pod **nástroje pro správu**. Hello nástroje pro správu, vytvořit **souborové DSN** tak lze zadat toohello konektor.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/connectivity.png)

úvodní obrazovka připojení je hello nejprve, když vytvoříte nový konektor obecné SQL. Je nutné nejprve tooprovide hello následující informace:

* Cesta k souboru DSN
* Authentication
  * Uživatelské jméno
  * Heslo

Hello databáze by měly podporovat jednu z těchto metod ověřování:

* **Ověřování systému Windows**: hello ověřování databáze používá hello Windows pověření tooverify hello uživatele. Hello uživatelské jméno a heslo zadané je použité tooauthenticate s databází hello. Tento účet potřebuje oprávnění toohello databáze.
* **Ověřování SQL**: hello ověřování databáze používá hello uživatelské jméno a heslo definované jedna hello připojení obrazovky tooconnect toohello databáze. Pokud ukládáte hello uživatelské jméno nebo heslo v souboru hello názvu DSN, hello přihlašovací údaje na obrazovce připojení hello mají přednost před.
* **Azure SQL Database ověřování**: Další informace najdete v tématu [připojit tooSQL databáze pomocí pomocí Azure Active Directory Authentication](../../sql-database/sql-database-aad-authentication.md).

**Rozlišující název je kotva**: Pokud vyberete tuto možnost, hello rozlišující název se používá jako atribut kotvy hello. Lze použít pro jednoduché implementace, ale také obsahuje hello následující omezení:

* Konektor podporuje pouze jeden objekt typu. Proto všechny odkazy, které může odkazovat pouze atributy hello stejný typ objektu.

**Typ exportu: Objekt nahraďte**: během exportu, když jenom některých atributů změnily, hello celý objekt s všechny atributy se exportují a nahradí hello existující objekt.

### <a name="schema-1-detect-object-types"></a>Schéma 1 (zjistit typy objektů)
Na této stránce, kterou budete tooconfigure jak hello konektor je probíhající toofind hello různé typy objektů v databázi hello.

Každý typ objektu je uvedené jako oddíl a nakonfigurovat další na **konfigurace oddílů a hierarchií**.

![schema1a](./media/active-directory-aadconnectsync-connector-genericsql/schema1a.png)

**Objekt metody detekce typu**: hello konektor podporuje tyto metody detekce typu objektu.

* **Pevná hodnota**: Zadejte hello seznam typů objektů se seznam oddělený čárkami. Například: `User,Group,Department`.  
  ![schema1b](./media/active-directory-aadconnectsync-connector-genericsql/schema1b.png)
* **Tabulka/zobrazení nebo uložené procedury**: Zadejte název hello hello tabulky, zobrazení nebo uložené procedury a poté hello název sloupce, který poskytuje hello seznam typů objektu. Pokud používáte uložené procedury, zadejte také parametry pro něj ve formátu hello **[Name]: [směr]: [hodnota]**. Zadejte každý parametr na samostatném řádku (pomocí kláves Ctrl + Enter tooget nového řádku).  
  ![schema1c](./media/active-directory-aadconnectsync-connector-genericsql/schema1c.png)
* **Dotaz SQL**: Tato možnost vám umožňuje tooprovide dotaz SQL, který vrátí jeden sloupec s typy objektů, například `SELECT [Column Name] FROM TABLENAME`. Hello vrátit sloupec musí být typu řetězec (varchar).

### <a name="schema-2-detect-attribute-types"></a>Schéma 2 (rozpoznat atribut typy)
Na této stránce kterou budete tooconfigure jak hello atribut názvy a typy budou toobe zjištěna. pro každý typ objektu na předchozí stránku hello zjištěna jsou uvedeny možnosti konfigurace Hello.

![schema2a](./media/active-directory-aadconnectsync-connector-genericsql/schema2a.png)

**Atribut metody detekce typu**: hello konektor podporuje tyto metody detekce typu atributu každý zjištěný objekt typu ve schématu 1 obrazovky.

* **Tabulka/zobrazení nebo uložené procedury**: Zadejte název tabulky, zobrazení nebo uložené procedury hello, která by měla být názvy atributů použitých toofind hello hello. Pokud používáte uložené procedury, zadejte také parametry pro něj ve formátu hello **[Name]: [směr]: [hodnota]**. Zadejte každý parametr na samostatném řádku (pomocí kláves Ctrl + Enter tooget nového řádku). názvy atributů hello toodetect ve více hodnotami atributu, poskytovat seznam oddělený čárkami tabulek nebo zobrazení. Scénáře s více hodnotami nejsou podporované při nadřazenou a podřízenou tabulku mají stejné názvy sloupců.
* **Dotaz SQL**: Tato možnost vám umožňuje tooprovide dotaz SQL, který vrátí jeden sloupec s názvy atributů, například `SELECT [Column Name] FROM TABLENAME`. Hello vrátit sloupec musí být typu řetězec (varchar).

### <a name="schema-3-define-anchor-and-dn"></a>Schéma 3 (definovat ukotvení a rozlišující název)
Tato stránka umožňuje tooconfigure ukotvení a rozlišující název atributu pro každý zjištěný objekt typu. Můžete vybrat víc atributů toomake hello ukotvení jedinečný.

![schema3a](./media/active-directory-aadconnectsync-connector-genericsql/schema3a.png)

* Nejsou uvedené atributy s více hodnotami a logická hodnota.
* Stejný atribut nelze použít pro rozlišující název a anchor, pokud **rozlišující název je kotva** je vybrána na stránce připojení hello.
* Pokud **rozlišující název je kotva** je vybrána na stránce hello připojení, tato stránka vyžaduje pouze hello rozlišující název atributu. Tento atribut se taky použije jako atribut kotvy hello.

  ![schema3b](./media/active-directory-aadconnectsync-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Schéma 4 (definovat typ atributu, odkaz a směr)
Tato stránka vám umožní tooconfigure hello atribut typu, například celé číslo, binární, nebo logická hodnota a směr pro každý atribut. Všechny atributy ze stránky **schématu 2** jsou uvedeny včetně více hodnot atributů.

![schema4a](./media/active-directory-aadconnectsync-connector-genericsql/schema4a.png)

* **Datový typ**: používá toomap hello atribut typu toothose typy známého hello synchronizační modul. Hello výchozí hodnota je stejný typ jako zjistil ve schématu SQL hello hello toouse, ale data a času a referenční informace nejsou snadno rozpoznat. Pro ty, budete potřebovat toospecify **data a času** nebo **odkaz**.
* **Směr**: můžete nastavit hello atribut směr tooImport, exportu nebo ImportExport. ImportExport je výchozí.

![schema4b](./media/active-directory-aadconnectsync-connector-genericsql/schema4b.png)

Poznámky:

* Pokud není typ atributu zjistitelná pomocí hello konektor, použije hello String – datový typ:
* **Vnořené tabulky** lze považovat za jeden sloupec databázových tabulek. Oracle ukládá hello řádků vnořené tabulky seřazeny. Ale pokud načítáte hello vnořené tabulky do proměnné PL/SQL, hello řádky jsou uvedeny po sobě jdoucích dolní indexy začínajícím hodnotou 1. Který poskytuje přístup jako pole tooindividual řádků.
* **VARRYS** nejsou podporovány v konektoru hello.

### <a name="schema-5-define-partition-for-reference-attributes"></a>Schéma 5 (Definujte oddíly pro atributy typu odkaz)
Na této stránce můžete nakonfigurovat pro oddíl, který (typ objektu) atribut odkazuje na všechny atributy typu odkaz.

![schema5](./media/active-directory-aadconnectsync-connector-genericsql/schema5.png)

Pokud používáte **rozlišující název je kotva**, pak je nutné použít stejný typ objektu jako jeden se vychází z hello hello. Jiný typ objektu nelze odkazovat.

>[!NOTE]
Počínaje hello března 2017 aktualizace je nyní k dispozici možnost pro "*" Pokud tato možnost je zvolená pak všechny typy možné člen bude importována.

![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/any-option.png)

>[!IMPORTANT]
 Od verze může 2017 hello "*" neboli **jakákoliv možnost** byl změněn toosupport import a export toku. Pokud chcete, aby toouse tuto možnost měli více hodnot tabulky či zobrazení atribut, který obsahuje typ objektu hello.

![](./media/active-directory-aadconnectsync-connector-genericsql/any-02.png)

 </br> Pokud "*" je vybrána, a také je třeba zadat název hello hello sloupce s typem objektu hello.</br> ![](./media/active-directory-aadconnectsync-connector-genericsql/any-03.png)

Po importu zobrazí se něco podobného toohello obrázku níže:

  ![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/after-import.png)



### <a name="global-parameters"></a>Globální parametry
Hello globální parametry stránka je použité tooconfigure rozdílový Import, formát data a času a metoda heslo.

![globalparameters1](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters1.png)



Hello obecné SQL konektor podporuje následující metody pro rozdílový Import hello:

* **Aktivační událost**: najdete v části [generování zobrazení rozdílů pomocí aktivační události](https://technet.microsoft.com/library/cc708665.aspx).
* **Vodoznak**: Obecný přístup, který lze použít s libovolnou databázi. Hello vodoznak dotazu je předem vyplní podle dodavatele databáze hello. Sloupec vodoznaku musí být na každé tabulky či zobrazení použít. Tento sloupec musí sledovat operace INSERT a Update toohello tabulky jako a jeho závislých (s více hodnotami nebo podřízené) tabulky. hodiny Hello mezi synchronizační službou a hello databázový server musí být synchronizovány. Pokud ne, může být některé položky v hello Rozdílový import vynechán.  
  Omezení:
  * Strategie vodoznak nemá podporu odstranit objekty.
* **Snímek**: (funguje pouze v systému Microsoft SQL Server) [generování zobrazení rozdílů pomocí snímků](https://technet.microsoft.com/library/cc720640.aspx)
* **Sledování změn**: (funguje pouze v systému Microsoft SQL Server) [o sledování změn](https://msdn.microsoft.com/library/bb933875.aspx)  
  Omezení:
  * Ukotvení & rozlišující název atributu musí být součástí primární klíč pro vybraný objekt hello v tabulce hello.
  * Během importu a exportu s sledování změn není podporováno dotazu SQL.

**Další parametry**: Zadejte hello databáze serveru časové pásmo, která určuje, kde se nachází databáze serveru. Tato hodnota se používá toosupport hello různými formáty atributů datum a čas.

Hello konektor vždy ukládá datum a datum a čas ve formátu UTC. možnost toocorrectly toobe převést hello data a časy, musí být zadán hello časové pásmo hello databázový server a hello formátu, který používá. Formát Hello by měla být vyjádřena v rozhraní .net formátu.

Při exportu každý atribut datum čas je třeba zadat toohello konektor ve formátu času UTC.

![globalparameters2](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters2.png)

**Konfigurace hesla**: hello konektor poskytuje funkce Synchronizace hesla a podporuje nastavit a změnit heslo.

Hello konektor nabízí dvě metody synchronizace hesel toosupport:

* **Uložené procedury**: Tato metoda vyžaduje dva uložené procedury toosupport sadu & Změnit heslo. Zadejte všechny parametry pro přidat a změňte operace hello hesla v **nastavit heslo SP** a **změnit heslo SP** parametry v uvedeném pořadí jako v následujícím příkladu.
  ![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters3.png)
* **Heslo rozšíření**: Tato metoda vyžaduje heslo rozšíření knihovny DLL (třeba tooprovide hello název knihovny DLL rozšíření, který implementuje hello [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) rozhraní). Sestavení rozšíření heslo musí být umístěny ve složce rozšíření, aby hello konektor mohl načíst hello DLL za běhu.
  ![globalparameters4](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters4.png)

Máte také tooenable hello Správa hesel na hello **nakonfigurovat rozšíření** stránky.
![globalparameters5](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfigurace oddílů a hierarchií
Na stránce hello oddílů a hierarchií vyberte všechny typy objektů. Pro každý typ objektu je vlastní oddíl.

![partitions1](./media/active-directory-aadconnectsync-connector-genericsql/partitions1.png)

Můžete také přepsat hello hodnot definovaných v hello **připojení** nebo **globální parametry** stránky.

![partitions2](./media/active-directory-aadconnectsync-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Konfigurace ukotvení
Tato stránka je jen pro čtení, protože již byl definován hello ukotvení. Hello vybrané ukotvení atribut je připojen vždy s tooensure typ objektu hello zůstane jedinečný mezi typy objektů.

![ukotvení](./media/active-directory-aadconnectsync-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Konfigurace parametrů kroku spuštění
Tyto kroky jsou nakonfigurované na hello profily spuštění na hello konektor. Tyto konfigurace hello import a export dat samotnou práci.

### <a name="full-and-delta-import"></a>Úplné a rozdílový Import
Obecné konektor SQL podporu úplné a rozdílový Import pomocí těchto metod:

* Table
* Zobrazení
* Uložená procedura
* Dotaz SQL

![runstep1](./media/active-directory-aadconnectsync-connector-genericsql/runstep1.png)

**Tabulka/zobrazení**  
více hodnot atributů tooimport pro objekt, máte název tabulky či zobrazení textový soubor s oddělovači hello tooprovide **název vícehodnotových tabulky nebo zobrazení** a podmínky pro příslušné připojení v hello **podmínkaspojení**s nadřazenou tabulkou hello.

Příklad: Chcete tooimport hello zaměstnanec objekt a všechny jeho atributy s více hodnotami. Existují dvě tabulky, s názvem zaměstnanec (hlavní tabulka) a oddělení (více hodnot).
Hello následující:

* Typ **zaměstnanec** v **tabulky nebo zobrazení/SP**.
* Typ oddělení v **název vícehodnotových tabulky nebo zobrazení**.
* Zadejte podmínku hello připojení mezi zaměstnanci & oddělení v **podmínku připojení**, například `Employee.DEPTID=Department.DepartmentID`.
  ![runstep2](./media/active-directory-aadconnectsync-connector-genericsql/runstep2.png)

**Uložené procedury**  
![runstep3](./media/active-directory-aadconnectsync-connector-genericsql/runstep3.png)

* Pokud máte množství dat, se doporučuje tooimplement stránkování se vaše uložené procedury.
* Vaše uložená procedura toosupport stránkování musíte tooprovide Start Index a End Index. Přejděte na téma: [efektivně stránkování prostřednictvím velké objemy dat](https://msdn.microsoft.com/library/bb445504.aspx).
* @StartIndexa @EndIndex v době provedení nahradí se hodnota velikosti stránky příslušné nakonfigurované na **krok konfigurace** stránky. Například když konektor hello načte první stránka a hello velikost stránky nastavená 500, v takové situaci @StartIndex by bylo 1 a @EndIndex 500. Tyto hodnoty zvýšit, když konektor načte následující stránky a změňte hello @StartIndex & @EndIndex hodnotu.
* tooexecute parametry uloženou proceduru, zadejte hello parametry v `[Name]:[Direction]:[Value]` formátu. Zadejte každý parametr na samostatném řádku (pomocí kombinace kláves Ctrl + Enter tooget nového řádku).
* Obecné SQL konektor rovněž podporuje operace importu z odkazované servery v systému Microsoft SQL Server. Pokud mají být načtena informace z tabulky propojené serveru, by měl být tabulky zadané ve formátu hello:`[ServerName].[Database].[Schema].[TableName]`
* Obecné SQL konektor podporuje pouze ty objekty, které mají podobnou strukturou (alias název i datový typ) mezi, která spustit kroky zjišťování informace a schéma. Pokud vybraná hello objektu ze schématu a zadaných informací v kroku spuštění se liší, pak konektor SQL se nepodařilo toosupport tento typ scénáře.

**Dotaz SQL**  
![runstep4](./media/active-directory-aadconnectsync-connector-genericsql/runstep4.png)

![runstep5](./media/active-directory-aadconnectsync-connector-genericsql/runstep5.png)

* Sady více výsledků dotazů není podporována.
* Dotaz SQL podporuje hello stránkování a zadejte počáteční a koncové indexem jako proměnné toosupport stránkování.

### <a name="delta-import"></a>Rozdílový Import
![runstep6](./media/active-directory-aadconnectsync-connector-genericsql/runstep6.png)

Rozdílový Import konfigurace vyžaduje určitou další konfiguraci ve srovnání s úplný Import.

* Pokud si zvolíte hello aktivační události nebo snímek přístup tootrack rozdílové změny, zadejte Tabulka historie nebo snímek databáze v **název tabulky historie nebo snímek databáze** pole.
* Musíte taky tooprovide podmínku připojení mezi historie tabulka a tabulka nadřazené, například`Employee.ID=History.EmployeeID`
* transakce hello tootrack na hello nadřazenou tabulku z tabulky historie hello, je nutné zadat název hello sloupce, který obsahuje informace o operaci hello (přidat, aktualizace nebo odstranění).
* Pokud si zvolíte vodoznak tootrack rozdílové změny, zadejte název hello sloupce, který obsahuje informace o operaci hello v **název sloupce označit horních**.
* Hello **změnit atribut Type** je vyžadován pro typ změny hello sloupec. Změnu, ke kterému dochází v primární tabulce hello mapování tohoto sloupce nebo tabulky s více hodnotami tooa změnit typ v zobrazení rozdílu hello. Tento sloupec může obsahovat hello Modify_Attribute změnit typ pro úroveň atributu změnit nebo přidat, upravit, nebo odstraňte změnit typ pro typ změny na úrovni objektů. Pokud je něco jiného než hello výchozí hodnotu přidat, upravit, nebo odstraňte a potom můžete definovat tyto hodnoty pomocí této možnosti.

### <a name="export"></a>Export
![runstep7](./media/active-directory-aadconnectsync-connector-genericsql/runstep7.png)

Obecné konektor SQL podporují Export pomocí čtyř podporované metody, jako třeba:

* Table
* Zobrazení
* Uložená procedura
* Dotaz SQL

**Tabulka/zobrazení**  
Pokud si zvolíte hello možnost tabulky či zobrazení, konektor hello generuje hello příslušných dotazů toodo hello Export.

**Uložené procedury**  
![runstep8](./media/active-directory-aadconnectsync-connector-genericsql/runstep8.png)

Pokud zvolíte možnost hello uloženou proceduru, Export vyžaduje tři různé uložené procedury tooperform vložení, aktualizaci nebo odstranění operace.

* **Přidejte název SP**: Tento SP spustí v případě, že jakýkoli objekt dodává tooconnector pro vložení v příslušné tabulce hello.
* **Aktualizujte název SP**: Tento SP spustí v případě, že jakýkoli objekt dodává tooconnector pro aktualizaci v příslušné tabulce hello.
* **Odstraňte název SP**: Tento SP spustí v případě, že jakýkoli objekt dodává tooconnector pro odstranění v příslušné tabulce hello.
* Atribut vybrané ze schématu hello použít jako parametr hodnotu toohello uložené procedury. Například `EmployeeName: INPUT: @EmployeeName` (EmployeeName je vybrána ve schématu hello konektoru a hello konektoru nahradí hodnotu odpovídající hello přitom export)
* toorun parametry uložené procedury, zadat parametry v `[Name]:[Direction]:[Value]` formátu. Zadejte každý parametr na samostatném řádku (pomocí kombinace kláves Ctrl + Enter tooget nového řádku).

**Dotaz SQL**  
![runstep9](./media/active-directory-aadconnectsync-connector-genericsql/runstep9.png)

Pokud zvolíte možnost dotazu SQL hello, vyžaduje Export že tři různé dotazy tooperform operací vložení, aktualizaci nebo odstranění.

* **Vložte dotaz**: Pokud libovolného objektu pochází tooconnector pro vložení v příslušné tabulce hello spuštění tohoto dotazu.
* **Aktualizace dotazu**: Pokud libovolného objektu pochází tooconnector pro aktualizaci v příslušné tabulce hello spuštění tohoto dotazu.
* **Dotaz odstranit**: Pokud libovolného objektu pochází tooconnector pro odstranění v příslušné tabulce hello spuštění tohoto dotazu.
* Vybraný ze schématu hello použít jako parametr dotazu toohello hodnotu, například atribut`Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Řešení potíží
* Informace o tom, jak protokolování tooenable tootroubleshoot hello konektoru najdete v tématu hello [jak tooEnable trasování ETW pro konektory](http://go.microsoft.com/fwlink/?LinkId=335731).
