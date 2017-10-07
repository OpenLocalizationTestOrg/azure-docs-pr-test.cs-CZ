---
title: "Koncepty pro vývojáře katalogu aaaData | Microsoft Docs"
description: "Úvod toohello klíčové koncepty v konceptuálním modelu Azure Data Catalog, jak je k dispozici prostřednictvím hello rozhraní API REST katalogu."
services: data-catalog
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
tags: 
ms.assetid: 89de9137-a0a4-40d1-9f8d-625acad31619
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/03/2017
ms.author: spelluru
ms.openlocfilehash: d0b1628ff6c31458cb650efef852244f0c139b1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-developer-concepts"></a>Koncepty pro vývojáře Azure Data Catalog
Microsoft **Azure Data Catalog** je plně spravovaná Cloudová služba, která poskytuje možnosti pro zjišťování zdroje dat a metadat zdroje dat crowdsourcingový. Vývojáři mohou použít hello service pomocí jeho rozhraní REST API. Principy hello koncepcemi hello služby je důležité pro vývojáře integrovat toosuccessfully **Azure Data Catalog**.

## <a name="key-concepts"></a>Klíčové koncepty
Hello **Azure Data Catalog** konceptuální model je založen na čtyři klíčové koncepty: hello **katalogu**, **uživatelé**, **prostředky**a **Poznámky**.

![Koncept][1]

*Obrázek 1 – Azure Data Catalog zjednodušené konceptuálního modelu*

### <a name="catalog"></a>Katalogu
A **katalogu** je hello kontejner nejvyšší úrovně pro všechny organizace ukládá metadata hello. Existuje **katalogu** povolených pro účet Azure. Katalogy jsou vázané tooan předplatné Azure, ale jenom jeden **katalogu** lze vytvořit pro jakékoli dané účet Azure, i když se účet může mít několik odběrů.

Obsahuje katalog **uživatelé** a **prostředky**.

### <a name="users"></a>Uživatelé
Uživatelé jsou objekty zabezpečení, které mají oprávnění tooperform akce (vyhledat hello katalogu, přidat, upravit nebo odebrat položky, atd.) v hello katalogu.

Existuje několik různých rolí, které uživatel může mít. Informace o rolích najdete v části hello role a autorizace.

Můžete přidat jednotlivé uživatele a skupiny zabezpečení.

Azure Data Catalog používá Azure Active Directory pro správu identit a přístupu. Každý uživatel katalogu musí být členem skupiny hello služby Active Directory pro účet hello.

### <a name="assets"></a>Prostředky
A **katalogu** obsahuje datové prostředky. **Prostředky** jsou hello jednotka členitost spravuje hello katalogu.

zdroj dat se liší Hello členitost prostředek. Pro SQL Server nebo databáze Oracle může být prostředek tabulku nebo zobrazení. Pro SQL Server Analysis Services může být prostředek míry, dimenze nebo klíčového ukazatele výkonu (KPI). Pro SQL Server Reporting Services je prostředek sestavy.

**Asset** hello jedinou přidat nebo odebrat z katalogu. Je jednotka hello výsledku můžete získat zpět z **vyhledávání**.

**Asset** se skládá z jeho název, umístění a typ a poznámky, které další popisují ho.

### <a name="annotations"></a>Poznámky
Poznámky jsou položky, které představují metadata o prostředcích.

Příklady poznámky: popis, značky, schéma, dokumentace, atd. Úplný seznam hello asset typy a typy poznámky jsou hello části Asset objektu modelu.

## <a name="crowdsourcing-annotations-and-user-perspective-multiplicity-of-opinion"></a>Crowdsourcingový poznámky a uživatel perspektivy (násobnost stanovisko)
Klíčovým prvkem Azure Data Catalog je, jak podporuje hello crowdsourcingový metadat systému hello. Jako názvem na rozdíl od tooa wiki přístup – kde existuje pouze jeden stanovisko a hello poslední zapisovače wins – model hello Azure Data Catalog umožňuje více názory toolive vedle sebe v systému hello.

Tento přístup odráží hello reálném světě podnikových dat. kde umožňuje uživatelům používat různých perspektiv na daný prostředek:

* Správce databáze může poskytovat informace o smlouvy o úrovni služeb nebo hello k dispozici zpracování okna pro hromadné operace ETL
* Data steward může poskytnout informace o hello obchodní procesy toowhich hello asset vztahují nebo hello klasifikace, které hello firmy použil tooit
* Analytik finanční poskytnout informace o použití hello dat během období koncového úlohám generování sestav

Tento příklad, každý uživatel – hello DBA, steward hello dat a analytických hello – toosupport můžete přidat jednu tabulku popis tooa, který byl zaregistrován v hello katalogu. Všechny popisy jsou zachována ve hello systému a na portálu Azure Data Catalog hello jsou zobrazeny všechny popisy.

Tento vzor je použité toomost hello položek v modelu objektu hello, takže typy objektů v datové části JSON hello jsou často pole vlastností kde může být typu singleton.

Například pod hello asset kořenová je pole objektů popis. Vlastnost pole Hello je s názvem "Popis". Popis objektu má jednu vlastnost – Popis. vzor Hello je, že každý uživatel, který získá popis typy objekt popis vytvořili pro hello hodnota zadaná uživatelem hello.

Hello UX pak můžete vybrat, jak toodisplay hello kombinaci. Existují tři různé vzorce pro zobrazení.

* Nejjednodušším způsobem Hello je "Zobrazit vše". V tomto vzoru v zobrazení seznamu jsou uvedeny všechny objekty hello. portál Azure Data Catalog Hello UX používá tento vzor pro popis.
* Jiné vzor je "Merge". V tomto vzoru se všechny hodnoty hello od různých uživatelů hello sloučit společně s duplicitní odebrat. Příkladem tohoto vzoru portálu Azure Data Catalog hello UX jsou vlastnosti značek a odborníky hello.
* Třetí vzor je "poslední zapisovače wins". V tomto vzoru se zobrazí pouze hello nejnovější hodnotu zadanou v. friendlyName je příklad tohoto vzoru.

## <a name="asset-object-model"></a>Asset objektový model
Zavedeném v části klíčové koncepty hello hello **Azure Data Catalog** objektový model obsahuje položky, které mohou být prostředky nebo poznámky. Položky mají vlastnosti, které můžou být požadované nebo volitelné. Některé vlastnosti použít tooall položky. Některé vlastnosti použít tooall prostředky. Některé vlastnosti použít pouze typy toospecific asset.

### <a name="system-properties"></a>Systémové vlastnosti
<table><tr><td><b>Název vlastnosti</b></td><td><b>Datový typ</b></td><td><b>Komentáře</b></td></tr><tr><td>časové razítko</td><td>Data a času</td><td>Hello poslední čas hello položka byla změněna. Toto pole je generovaný serverem hello při vložení položky a při každé aktualizaci položky. Hello hodnota této vlastnosti je ignorována, na vstup z operací publikování.</td></tr><tr><td>id</td><td>identifikátor URI</td><td>Absolutní adresa url položky hello (jen pro čtení). Je hello jedinečný adresovatelné identifikátor URI pro položku hello.  Hello hodnota této vlastnosti je ignorována, na vstup z operací publikování.</td></tr><tr><td>type</td><td>Řetězec</td><td>Typ Hello hello asset (jen pro čtení).</td></tr><tr><td>Značka Etag</td><td>Řetězec</td><td>Řetězec odpovídající toohello verzi hello položky, které lze použít pro optimistické řízení souběžného při provádění operace, které aktualizují položky v katalogu hello. "*" může být použité toomatch libovolná hodnota.</td></tr></table>

### <a name="common-properties"></a>Běžné vlastnosti
Tyto vlastnosti použít tooall kořenové asset typy a všechny typy poznámky.

<table>
<tr><td><b>Název vlastnosti</b></td><td><b>Datový typ</b></td><td><b>Komentáře</b></td></tr>
<tr><td>fromSourceSystem</td><td>Logická hodnota</td><td>Určuje, zda je dat položky odvozené ze zdrojového systému (například databáze serveru Sql, databáze Oracle) nebo vytvořené uživatelem.</td></tr>
</table>

### <a name="common-root-properties"></a>Běžné vlastnosti kořenové
<p>
Tyto vlastnosti použít tooall kořenové asset typy.

<table><tr><td><b>Název vlastnosti</b></td><td><b>Datový typ</b></td><td><b>Komentáře</b></td></tr><tr><td>jméno</td><td>Řetězec</td><td>Název odvozený od hello informace o umístění zdroje dat</td></tr><tr><td>DSL</td><td>DataSourceLocation</td><td>Jednoznačně popisuje hello zdroj dat a je jedním z hello identifikátory pro prostředek hello. (Viz část duální identity).  Struktura Hello hello dsl se liší podle hello protokolu a typ zdroje.</td></tr><tr><td>zdroj dat</td><td>DataSourceInfo</td><td>Další podrobnosti o hello typ prostředku.</td></tr><tr><td>lastRegisteredBy</td><td>SecurityPrincipal</td><td>Popisuje hello uživatel, který naposledy zaregistroval tento prostředek.  Obsahuje hello jedinečné id pro hello uživatele (hello upn) a zobrazovaný název (jméno a příjmení).</td></tr><tr><td>identifikátor containerId</td><td>Řetězec</td><td>ID prostředku hello kontejner pro zdroj dat hello. Tato vlastnost není podporována pro hello typ kontejneru.</td></tr></table>

### <a name="common-non-singleton-annotation-properties"></a>Běžné vlastnosti není typu singleton poznámky
Tyto vlastnosti použít tooall která není typu singleton poznámky typy (poznámky, které povolené toobe více za asset).

<table>
<tr><td><b>Název vlastnosti</b></td><td><b>Datový typ</b></td><td><b>Komentáře</b></td></tr>
<tr><td>key</td><td>Řetězec</td><td>Uživatel zadaný klíč, který jednoznačně identifikuje hello poznámky v aktuální kolekci hello. Délka klíče Hello nesmí překročit 256 znaků.</td></tr>
</table>

### <a name="root-asset-types"></a>Kořenové asset typy
Kořenové asset typy jsou tyto typy, které představují hello různé typy datových prostředků, které lze registrovat v katalogu hello. Pro každý typ kořenové je zobrazení, která popisuje asset a poznámky, které jsou součástí hello zobrazení. Název zobrazení by být používána v hello odpovídající {view_name} segment adresy url při publikování assetu pomocí rozhraní REST API.

<table><tr><td><b>Typ prostředku (název zobrazení)</b></td><td><b>Další vlastnosti</b></td><td><b>Datový typ</b></td><td><b>Povolené poznámky</b></td><td><b>Komentáře</b></td></tr><tr><td>Tabulky ("tabulky")</td><td></td><td></td><td>Popis<p>FriendlyName<p>Značka<p>Schéma<p>ColumnDescription<p>ColumnTag<p> odborné<p>Preview<p>AccessInstruction<p>TableDataProfile<p>ColumnDataProfile<p>ColumnDataClassification<p>Dokumentace<p></td><td>Tabulka představuje žádná tabulková data.  Příklad: tabulku SQL, zobrazení SQL, tabulkové tabulky Analysis Services, Analysis Services Multidimensional dimenze, tabulky Oracle atd.   </td></tr><tr><td>Míru ("opatření")</td><td></td><td></td><td>Popis<p>FriendlyName<p>Značka<p>odborné<p>AccessInstruction<p>Dokumentace<p></td><td>Tento typ reprezentuje měr Analysis Services.</td></tr><tr><td></td><td>Míra</td><td>Sloupec</td><td></td><td>Metadata popisující hello měr</td></tr><tr><td></td><td>isCalculated </td><td>Logická hodnota</td><td></td><td>Určuje, pokud se počítá hello měr, nebo ne.</td></tr><tr><td></td><td>Skupina měr</td><td>Řetězec</td><td></td><td>Fyzické kontejner pro míru</td></tr><td>Klíčový ukazatel výkonu "(KPI)</td><td></td><td></td><td>Popis<p>FriendlyName<p>Značka<p>odborné<p>AccessInstruction<p>Dokumentace</td><td></td></tr><tr><td></td><td>Skupina měr</td><td>Řetězec</td><td></td><td>Fyzické kontejner pro míru</td></tr><tr><td></td><td>goalExpression</td><td>Řetězec</td><td></td><td>Číselný výraz MDX nebo výpočet, který vrátí hello cílová hodnota ukazatele hello klíčového ukazatele výkonu.</td></tr><tr><td></td><td>valueExpression</td><td>Řetězec</td><td></td><td>MDX číselný výraz, který vrací aktuální hodnotu hello hello klíčového ukazatele výkonu.</td></tr><tr><td></td><td>statusExpression</td><td>Řetězec</td><td></td><td>Výraz MDX, který představuje stav hello hello klíčového ukazatele výkonu v určitém bodě v čase.</td></tr><tr><td></td><td>trendExpression</td><td>Řetězec</td><td></td><td>Výraz MDX, která vyhodnotí hodnotu hello hello klíčového ukazatele výkonu v čase. Hello trend může být jakékoli založené na čase kritérium, které jsou užitečné v konkrétní obchodní kontext.</td>
<tr><td>Sestavy ("sestavy")</td><td></td><td></td><td>Popis<p>FriendlyName<p>Značka<p>odborné<p>AccessInstruction<p>Dokumentace<p></td><td>Tento typ reprezentuje sestavy služby SQL Server Reporting Services </td></tr><tr><td></td><td>assetCreatedDate</td><td>Řetězec</td><td></td><td></td></tr><tr><td></td><td>assetCreatedBy</td><td>Řetězec</td><td></td><td></td></tr><tr><td></td><td>assetModifiedDate</td><td>Řetězec</td><td></td><td></td></tr><tr><td></td><td>assetModifiedBy</td><td>Řetězec</td><td></td><td></td></tr><tr><td>Kontejner ("kontejner")</td><td></td><td></td><td>Popis<p>FriendlyName<p>Značka<p>odborné<p>AccessInstruction<p>Dokumentace<p></td><td>Tento typ reprezentuje kontejner jiných prostředků, jako je například SQL database, kontejner objektů BLOB služby Azure nebo model služby Analysis Services.</td></tr></table>

### <a name="annotation-types"></a>Typy poznámky
Typy poznámky představují typy metadat, které lze přiřadit tooother typů v rámci katalogu hello.

<table>
<tr><td><b>Typ poznámky (název zobrazení vnořené)</b></td><td><b>Další vlastnosti</b></td><td><b>Datový typ</b></td><td><b>Komentáře</b></td></tr>

<tr><td>Popis ("Popis")</td><td></td><td></td><td>Tato vlastnost obsahuje popis pro určitý prostředek. Každý uživatel systému hello můžete přidat vlastní popis.  Objekt popis hello lze upravit pouze tento uživatel.  (Správci a Asset vlastníky můžete odstranění hello popis objektu, ale nikoli pro úpravy). Hello systém uchovává uživatelů popisy samostatně.  Proto je pole popisů na každý prostředek (jeden pro každého uživatele, který má podílí své znalosti o hello asset v přidání toopossibly ten, který obsahuje informace odvozené ze zdroje dat hello).</td></tr>
<tr><td></td><td>description</td><td>Řetězec</td><td>Krátký popis (řádky 2-3) hello asset</td></tr>

<tr><td>Značky ("značky")</td><td></td><td></td><td>Tato vlastnost definuje značky pro prostředek. Každý uživatel systému hello můžete přidat více značek pro určitý prostředek.  Pouze hello uživatele, který vytvořil objekty značku upravovat.  (Správci a Asset vlastníky můžete odstranit objekt hello značky, ale není upravit). Hello systém uchovává uživatelů značky samostatně.  Proto je pole objektů značky na každý prostředek.</td></tr>
<tr><td></td><td>Značka</td><td>Řetězec</td><td>Značky popisující hello asset.</td></tr>

<tr><td>FriendlyName ("friendlyName")</td><td></td><td></td><td>Tato vlastnost obsahuje popisný název pro určitý prostředek. FriendlyName je typu singleton poznámky – lze přidat pouze jeden FriendlyName tooan asset.  Pouze hello uživatele, který vytvořil objekt FriendlyName můžete upravit ho. (Správci a Asset vlastníky můžete odstranit objekt FriendlyName hello, ale nikoli pro úpravy). Hello systém uchovává popisné názvy uživatelů samostatně.</td></tr>
<tr><td></td><td>FriendlyName</td><td>Řetězec</td><td>Popisný název hello asset.</td></tr>

<tr><td>Schéma ("schéma")</td><td></td><td></td><td>Hello schématu popisuje strukturu hello hello data.  Je seznam názvů hello atribut (sloupec, atribut, pole atd.), typy a další metadata.  Tyto informace je odvozena ze zdroje dat hello.  Schéma je typu singleton poznámky – lze přidat pouze jedno schéma pro prostředek.</td></tr>
<tr><td></td><td>sloupce</td><td>Sloupce]</td><td>Pole objektů sloupce. Popisují hello sloupec s informacemi, které jsou odvozené ze zdroje dat hello.</td></tr>

<tr><td>ColumnDescription ("columnDescriptions")</td><td></td><td></td><td>Tato vlastnost obsahuje popis pro sloupec.  Každý uživatel systému hello můžete přidat vlastní popisy pro více sloupců (maximálně jeden každý sloupec). Pouze hello uživatele, který vytvořil objekty ColumnDescription upravovat.  (Správci a Asset vlastníky můžete hello ColumnDescription objekt odstranit, ale nikoli pro úpravy). Hello systém uchovává popisy sloupců tyto uživatele samostatně.  Proto je pole objektů ColumnDescription na každý prostředek (jeden na každý sloupec pro každý uživatel, který má podílí své znalosti o hello sloupce kromě toopossibly, která obsahuje informace odvozené ze zdroje dat hello).  Hello ColumnDescription je volně vázané toohello schématu, takže můžete získat synchronizován. Hello ColumnDescription může popisují sloupec, který již neexistuje ve schématu hello.  Je toohello zapisovače tookeep popis a schéma synchronizované.  zdroj dat Hello mohou mít i informace popisu sloupce a jsou další ColumnDescription objekty, které by se vytvoří při spuštění nástroje hello.</td></tr>
<tr><td></td><td>columnName</td><td>Řetězec</td><td>Název Hello hello sloupec, který odkazuje tento popis.</td></tr>
<tr><td></td><td>description</td><td>Řetězec</td><td>Stručný popis (řádky 2-3) hello sloupce.</td></tr>

<tr><td>ColumnTag ("columnTags")</td><td></td><td></td><td>Tato vlastnost obsahuje značku pro sloupec. Každý uživatel systému hello můžete přidat více značek pro daný sloupec a můžete přidat značky pro více sloupců. Pouze hello uživatele, který vytvořil objekty ColumnTag upravovat. (Správci a Asset vlastníky můžete hello ColumnTag objekt odstranit, ale nikoli pro úpravy). Hello systém uchovává tito uživatelé sloupce značky samostatně.  Proto je pole objektů ColumnTag na každý prostředek.  Hello ColumnTag je volně vázané toohello schématu, takže můžete získat synchronizován. Hello ColumnTag může popisují sloupec, který již neexistuje ve schématu hello.  Je toohello zapisovače tookeep sloupce značky a schématu synchronizace.</td></tr>
<tr><td></td><td>columnName</td><td>Řetězec</td><td>Název Hello hello sloupec, který odkazuje tato značka.</td></tr>
<tr><td></td><td>Značka</td><td>Řetězec</td><td>Značka popisující hello sloupec.</td></tr>

<tr><td>Expert ("odborníky")</td><td></td><td></td><td>Tato vlastnost obsahuje uživatele, který je považován za odborník v sadě dat hello. Hello odborníky opinions(descriptions) bublin toohello horní části hello UX při výpisu popisy. Každý uživatel může určit vlastní odborníky. Objekt odborníky hello lze upravit pouze tento uživatel. (Správci a Asset vlastníky můžete odstranit objekty odborné hello, ale nikoli pro úpravy).</td></tr>
<tr><td></td><td>odborné</td><td>SecurityPrincipal</td><td></td></tr>

<tr><td>Verze Preview ("náhledů")</td><td></td><td></td><td>Hello preview obsahuje snímek hello horních řádků 20 dat pro prostředek hello. Náhled smysl jenom pro některé typy prostředků (má smysl pro tabulku, ale ne pro měr).</td></tr>
<tr><td></td><td>verze Preview</td><td>[] – objekt</td><td>Pole objektů, které představují sloupec.  Každý objekt má vlastnost mapování tooa sloupec s hodnotou pro tento sloupec pro řádek hello.</td></tr>

<tr><td>AccessInstruction ("accessInstructions")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>mimeType</td><td>Řetězec</td><td>typ mime Hello hello obsahu.</td></tr>
<tr><td></td><td>Obsah</td><td>Řetězec</td><td>Hello pokyny, jak tooget přístup toothis datový prostředek. Hello obsah může být adresa URL, e-mailovou adresu nebo u sady pokynů.</td></tr>

<tr><td>TableDataProfile ("tableDataProfiles")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>numberOfRows</td></td><td>celá čísla</td><td>Hello počet řádků v sadě dat hello</td></tr>
<tr><td></td><td>Velikost</td><td>dlouhá</td><td>Hello velikost v bajtech hello datových sad.  </td></tr>
<tr><td></td><td>schemaModifiedTime</td><td>Řetězec</td><td>Hello poslední čas hello schématu byla změněna.</td></tr>
<tr><td></td><td>dataModifiedTime</td><td>Řetězec</td><td>byla změněna Hello poslední čas hello datové sady (data byla přidána ke změně, nebo odstranit)</td></tr>

<tr><td>ColumnsDataProfile ("columnsDataProfiles")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>sloupce</td></td><td>[ColumnDataProfile]</td><td>Pole sloupec dat profily.</td></tr>

<tr><td>ColumnDataClassification ("columnDataClassifications")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>columnName</td><td>Řetězec</td><td>Název Hello hello sloupce, které odkazuje tato klasifikace.</td></tr>
<tr><td></td><td>klasifikace</td><td>Řetězec</td><td>klasifikace Hello hello dat v tomto sloupci.</td></tr>

<tr><td>Dokumentace k ("dokumentace")</td><td></td><td></td><td>Daný prostředek může mít pouze jeden dokumentaci s ním spojená.</td></tr>
<tr><td></td><td>mimeType</td><td>Řetězec</td><td>typ mime Hello hello obsahu.</td></tr>
<tr><td></td><td>Obsah</td><td>Řetězec</td><td>obsah dokumentace Hello.</td></tr>

</table>

### <a name="common-types"></a>Běžné typy
Běžné typy lze použít jako hello typy vlastností, ale nejsou žádné položky.

<table>
<tr><td><b>Společný typ.</b></td><td><b>Vlastnosti</b></td><td><b>Datový typ</b></td><td><b>Komentáře</b></td></tr>
<tr><td>DataSourceInfo</td><td></td><td></td><td></td></tr>
<tr><td></td><td>SourceType</td><td>Řetězec</td><td>Popisuje typ zdroje dat hello.  Příklad: SQL Server, databáze Oracle, atd.  </td></tr>
<tr><td></td><td>objectType</td><td>Řetězec</td><td>Popisuje typ objektu ve zdroji dat hello hello. Příklad: tabulka, zobrazení pro SQL Server.</td></tr>

<tr><td>DataSourceLocation</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Protokol</td><td>Řetězec</td><td>Povinná hodnota. Popisuje protokol použitý toocommunicate se zdrojem dat hello. Například: "tds" pro SQl Server, "oracle" pro Oracle atd. Odkazovat příliš[zdroje dat odkaz Specifikace – struktura DSL](data-catalog-dsr.md) hello seznam aktuálně podporované protokoly.</td></tr>
<tr><td></td><td>Adresa</td><td>Slovník<string, object></td><td>Povinná hodnota. Adresa je sada dat konkrétní toohello protokol, který je zdroj dat použité tooidentify hello se na ně odkazovat. data adres Hello obor konkrétní protokol tooa, což znamená, že je smysl bez znalosti protokolu hello.</td></tr>
<tr><td></td><td>Ověřování</td><td>Řetězec</td><td>Volitelné. schéma ověřování Hello používá toocommunicate se zdrojem dat hello. Příklad: windows oauth, atd.</td></tr>
<tr><td></td><td>connectionProperties</td><td>Slovník<string, object></td><td>Volitelné. Další informace o tom, zdroj dat tooa tooconnect.</td></tr>

<tr><td>SecurityPrincipal</td><td></td><td></td><td>back-end Hello neprovede žádné ověření zadané vlastnosti proti AAD během publikování.</td></tr>
<tr><td></td><td>hlavní název uživatele</td><td>Řetězec</td><td>Jedinečnou e-mailovou adresu uživatele. Musí být zadané, pokud není k dispozici objectId nebo v kontextu hello "lastRegisteredBy" vlastnosti, jinak volitelné.</td></tr>
<tr><td></td><td>objectId</td><td>Identifikátor GUID</td><td>Identity uživatele nebo zabezpečení skupiny AAD. Volitelné. Musí být zadán, pokud hlavní název uživatele není k dispozici, jinak volitelné.</td></tr>
<tr><td></td><td>FirstName</td><td>Řetězec</td><td>Křestní jméno uživatele (pro účely zobrazení). Volitelné. Jediná platná v kontextu hello "lastRegisteredBy" vlastnosti. Nelze zadat, pokud objekt zabezpečení poskytuje pro "role", "oprávnění" a "odborníci".</td></tr>
<tr><td></td><td>Příjmení</td><td>Řetězec</td><td>Příjmení uživatele (pro účely zobrazení). Volitelné. Jediná platná v kontextu hello "lastRegisteredBy" vlastnosti. Nelze zadat, pokud objekt zabezpečení poskytuje pro "role", "oprávnění" a "odborníci".</td></tr>

<tr><td>Sloupec</td><td></td><td></td><td></td></tr>
<tr><td></td><td>jméno</td><td>Řetězec</td><td>Název sloupce hello nebo atributu.</td></tr>
<tr><td></td><td>type</td><td>Řetězec</td><td>datový typ sloupce hello nebo atributu. Povolené typy Hello závisí na data sourceType hello majetku.  Je podporován pouze podmnožinu typy.</td></tr>
<tr><td></td><td>Hodnota maxLength</td><td>celá čísla</td><td>Hello maximální povolenou délku hello sloupec nebo atributu. Odvozené ze zdroje dat. Pouze typy zdrojů použít toosome.</td></tr>
<tr><td></td><td>přesnost</td><td>Bajtů</td><td>Hello přesnost sloupce hello nebo atributu. Odvozené ze zdroje dat. Pouze typy zdrojů použít toosome.</td></tr>
<tr><td></td><td>Vlastnost isNullable</td><td>Logická hodnota</td><td>Tom, zda text hello sloupec je nebo není povolené toohave hodnotu null. Odvozené ze zdroje dat. Pouze typy zdrojů použít toosome.</td></tr>
<tr><td></td><td>výraz</td><td>Řetězec</td><td>Pokud hodnota hello počítaný sloupec, toto pole obsahuje řetězec hello, který vyjadřuje hello hodnotu. Odvozené ze zdroje dat. Pouze typy zdrojů použít toosome.</td></tr>

<tr><td>ColumnDataProfile</td><td></td><td></td><td></td></tr>
<tr><td></td><td>columnName </td><td>Řetězec</td><td>Hello název sloupce hello</td></tr>
<tr><td></td><td>type </td><td>Řetězec</td><td>Hello typ sloupce hello</td></tr>
<tr><td></td><td>min </td><td>Řetězec</td><td>minimální hodnota Hello v sadě dat hello</td></tr>
<tr><td></td><td>maximální počet </td><td>Řetězec</td><td>Maximální hodnota Hello v sadě dat hello</td></tr>
<tr><td></td><td>průměr </td><td>Double</td><td>Hello průměrnou hodnotu v sadě dat hello</td></tr>
<tr><td></td><td>StDev </td><td>Double</td><td>Hello směrodatné odchylky pro datovou sadu hello</td></tr>
<tr><td></td><td>nullCount </td><td>celá čísla</td><td>Hello počet hodnot null ve hello datové sady</td></tr>
<tr><td></td><td>distinctCount  </td><td>celá čísla</td><td>Hello počet jedinečných hodnot v sadě dat hello</td></tr>


</table>

## <a name="asset-identity"></a>Asset identity
Azure Data Catalog používá (protocol) a vlastnosti identity z hello "adres" balík vlastností hello DataSourceLocation "dsl" vlastnost toogenerate identitu hello asset, který je použité tooaddress hello asset uvnitř hello katalogu.
Například protokol "tds" hello má vlastnosti identity "server", "databáze", "schématu" a "objekt". kombinace Hello hello protokolu a vlastnosti identity hello jsou použité toogenerate hello identitu hello Asset tabulky serveru SQL.
Azure Data Catalog poskytuje několik předdefinovaných datového zdroje protokoly, které jsou uvedeny v [zdroje dat odkaz Specifikace – struktura DSL](data-catalog-dsr.md).
Hello sadu podporovaných protokolů se dá rozšířit prostřednictvím kódu programu (viz. tooData katalogu REST API – referenční informace). Správci hello katalogu lze zaregistrovat zdroj protokoly vlastní data. Hello následující tabulka popisuje vlastnosti nutné tooregister hello vlastního protokolu.

### <a name="custom-data-source-protocol-specification"></a>Specifikace protokolu zdroj vlastních dat
<table>
<tr><td><b>Typ</b></td><td><b>Vlastnosti</b></td><td><b>Datový typ</b></td><td><b>Komentáře</b></td></tr>

<tr><td>DataSourceProtocol</td><td></td><td></td><td></td></tr>
<tr><td></td><td>obor názvů</td><td>Řetězec</td><td>obor názvů Hello hello protokolu. Namespace musí být od 1 too255 znaků, obsahovat jeden nebo více neprázdný části oddělené tečkou (.). Každá část musí být od 1 too255 znaků, začínat písmenem a obsahovat pouze písmena a číslice.</td></tr>
<tr><td></td><td>jméno</td><td>Řetězec</td><td>Název Hello hello protokolu. Název musí být od 1 too255 znaků, začínat písmenem a obsahovat pouze písmena, číslice a pomlčky (-) znak hello.</td></tr>
<tr><td></td><td>identityProperties</td><td>[DataSourceProtocolIdentityProperty]</td><td>Seznam vlastností identity, musí obsahovat alespoň jeden, ale žádné víc než 20 vlastnosti. Například: "server", "databáze", "schéma", "objekt" jsou vlastnosti identity hello "tds" protokolu.</td></tr>
<tr><td></td><td>identitySets</td><td>[DataSourceProtocolIdentitySet]</td><td>Seznam sad identity. Definuje sadu vlastnosti identity, které reprezentuje identitu platný asset. Musí obsahovat alespoň jeden, ale žádné víc než 20 sad. Příklad: {"server", "databáze", "schématu" a "objektu"} je identita pro protokol "tds", který definuje identitu asset tabulky serveru Sql Server.</td></tr>

<tr><td>DataSourceProtocolIdentityProperty</td><td></td><td></td><td></td></tr>
<tr><td></td><td>jméno</td><td>Řetězec</td><td>Hello název vlastnosti hello. Název musí mít 1 too100 znaků, začněte s písmenem a může obsahovat pouze písmena a čísla.</td></tr>
<tr><td></td><td>type</td><td>Řetězec</td><td>Typ Hello hello vlastnosti. Podporované hodnoty: "bool", logická hodnota ","bajtů","guid","int","číslo","dlouho","string","url"</td></tr>
<tr><td></td><td>IgnoreCase –</td><td>BOOL</td><td>Určuje, zda by měl při použití vlastnosti na hodnotu ignoruje velikost písmen. Můžete zadat pouze pro vlastnosti typu "řetězec". Výchozí hodnota je false.</td></tr>
<tr><td></td><td>urlPathSegmentsIgnoreCase</td><td>[] – operátor BOOL</td><td>Určuje, zda by měl pro každou část cesty hello url ignoruje velikost písmen. Můžete zadat pouze pro vlastnosti typu "url". Výchozí hodnota je [false].</td></tr>

<tr><td>DataSourceProtocolIdentitySet</td><td></td><td></td><td></td></tr>
<tr><td></td><td>jméno</td><td>Řetězec</td><td>nastavit název Hello hello identity.</td></tr>
<tr><td></td><td>properties</td><td>řetězec]</td><td>nastavit vlastnosti identity zahrnut do tuto identitu Hello seznamu. Nesmí obsahovat duplikáty. Každou vlastnost odkazuje sadu identity musí být definována v seznamu hello "identityProperties" hello protokolu.</td></tr>

</table>

## <a name="roles-and-authorization"></a>Role a autorizace
Microsoft Azure Data Catalog poskytuje možnosti autorizace pro operace CRUD na prostředky a poznámky.

## <a name="key-concepts"></a>Klíčové koncepty
Hello Azure Data Catalog používá dva mechanismy ověřování:

* Ověřování na základě rolí
* Ověření na základě oprávnění

### <a name="roles"></a>Role
Existují tři role: **správce**, **vlastníka**, a **Přispěvatel**.  Každá role má svou oboru a oprávnění, které jsou shrnuty v následující tabulce hello.

<table><tr><td><b>Role</b></td><td><b>Rozsah</b></td><td><b>Práva</b></td></tr><tr><td>Správce</td><td>Katalog (všechny prostředky nebo poznámky v hello katalog)</td><td>Odstranění ViewRoles pro čtení

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Vlastník</td><td>Každý prostředek (kořenovou položku)</td><td>Odstranění ViewRoles pro čtení

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Přispěvatel</td><td>Každý jednotlivý prostředek a poznámky</td><td>Aktualizace pro čtení odstranit ViewRoles Poznámka: všechna práva hello byly odvolány, pokud hello přečíst přímo na položku hello je odvolává hello přispěvatele</td></tr></table>

> [!NOTE]
> **Čtení**, **aktualizace**, **odstranit**, **ViewRoles** práva jsou položky použít tooany (asset nebo poznámky) při **vlastnictví**, **ChangeOwnership**, **ChangeVisibility**, **ViewPermissions** jsou pouze použít toohello kořenové asset.
> 
> **Odstranit** pravé platí položek tooan a všech podřízených položek či jednu položku pod ním. Například odstraněním prostředek odstraníte také žádné poznámky pro tento prostředek.
> 
> 

### <a name="permissions"></a>Oprávnění
Oprávnění je jako seznam položek řízení přístupu. Každé položce řízení přístupu přiřadí sadu objekt zabezpečení tooa práva. Oprávnění lze zadat pouze na prostředek (tedy kořenovou položku) a použít toohello prostředek a všechny podřízené položky.

Během hello **Azure Data Catalog** náhled pouze **čtení** práva je podporována v hello oprávnění seznamu tooenable scénář toorestrict viditelnosti majetku.

Ve výchozím nastavení má každý ověřený uživatel **čtení** pravým pro libovolnou položku v katalogu hello, pokud je omezen viditelnost toohello sady objektů zabezpečení v hello oprávnění.

## <a name="rest-api"></a>REST API
**UVEĎTE** a **POST** položky zobrazení požadavky, je možné použít toocontrol rolí a oprávnění: v datové části tooitem přidání, můžete být zadány dvě vlastnosti systému **role** a  **oprávnění**.

> [!NOTE]
> **oprávnění** pouze použít tooa kořenovou položku.
> 
> **Vlastník** role platí jenom tooa kořenovou položku.
> 
> Ve výchozím nastavení při vytvoření nové položky v katalogu hello jeho **Přispěvatel** nastavena toohello aktuálně ověřeného uživatele. Pokud má být položka aktualizovat kdokoli, **Přispěvatel** by mělo být nastavené příliš&lt;Everyone&gt; objekt speciální zabezpečení v hello **role** vlastnost položky při prvním publikování ( Viz. Následující ukázka toohello). **Přispěvatel** nelze změnit, a zůstane stejný hello dobu životnosti položky (i **správce** nebo **vlastníka** nemá správné toochange hello hello  **Přispěvatel**). Hello pouze hodnota podporován pro explicitní nastavení hello hello **Přispěvatel** je &lt;Everyone&gt;: **Přispěvatel** lze pouze uživatele, který vytvořil položku nebo &lt; Všichni&gt;.
> 
> 

### <a name="examples"></a>Příklady
**Nastavit Přispěvatel příliš&lt;Everyone&gt; při publikování položky.**
Objekt zabezpečení speciální &lt;Everyone&gt; má objectId "00000000-0000-0000-0000-000000000201".
  **POST** https://api.azuredatacatalog.com/catalogs/default/views/tables/?api-version=2016-03-30

> [!NOTE]
> Některých implementacích klienta HTTP může automaticky znovu vystavit požadavků v tooa odpovědi 302 ze serveru hello, ale obvykle pruhu hlavičky ověření z požadavku hello. Vzhledem k tomu, že hello autorizační hlavičky je požadovaná toomake požadavky tooAzure katalogu Data Catalog, je nutné zajistit, že hello autorizační hlavičky stále získáte při opětovné vystavení žádost o tooa přesměrování umístění, které Azure Data Catalog. Hello následující vzorový kód ukazuje pomocí objekt .NET HttpWebRequest hello.
> 
> 

**Text**

    {
        "roles": [
            {
                "role": "Contributor",
                "members": [
                    {
                        "objectId": "00000000-0000-0000-0000-000000000201"
                    }
                ]
            }
        ]
    }

  **Přiřadit vlastníků a omezení viditelnosti pro existující kořenovou položku**: **PUT** https://api.azuredatacatalog.com/catalogs/default/views/tables/042297b0...1be45ecd462a?api-version=2016-03-30

    {
        "roles": [
            {
                "role": "Owner",
                "members": [
                    {
                        "objectId": "c4159539-846a-45af-bdfb-58efd3772b43",
                        "upn": "user1@contoso.com"
                    },
                    {
                        "objectId": "fdabd95b-7c56-47d6-a6ba-a7c5f264533f",
                        "upn": "user2@contoso.com"
                    }
                ]
            }
        ],
        "permissions": [
            {
                "principal": {
                    "objectId": "27b9a0eb-bb71-4297-9f1f-c462dab7192a",
                    "upn": "user3@contoso.com"
                },
                "rights": [
                    {
                        "right": "Read"
                    }
                ]
            },
            {
                "principal": {
                    "objectId": "4c8bc8ce-225c-4fcf-b09a-047030baab31",
                    "upn": "user4@contoso.com"
                },
                "rights": [
                    {
                        "right": "Read"
                    }
                ]
            }
        ]
    }

> [!NOTE]
> V PUT není nutné toospecify datovou položku v textu hello: PUT lze použít tooupdate jenom role nebo oprávnění.
> 
> 

<!--Image references-->
[1]: ./media/data-catalog-developer-concepts/concept2.png
