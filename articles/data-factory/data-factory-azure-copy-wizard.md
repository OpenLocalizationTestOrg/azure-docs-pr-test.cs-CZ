---
title: "aaaData Průvodce kopírováním Azure | Microsoft Docs"
description: "Informace o tom, jak toouse hello Průvodce kopírováním Azure Data Factory toocopy data z toosinks podporované datové zdroje."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 0974eb40-db98-4149-a50d-48db46817076
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: spelluru
ms.openlocfilehash: 188b3ae15f937b84a58aec1b979347ac8090abf9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory-copy-wizard"></a>Průvodce kopírováním služby Azure Data Factory
Průvodce kopírováním objekt pro vytváření dat Azure Hello usnadňuje proces hello příjem dat, který je obvykle prvním krokem v případě integrace pomocí dat začátku do konce. Při průchodu přes hello Průvodce kopírováním objekt pro vytváření dat Azure, není nutné toounderstand všechny definice JSON propojené služby, datové sady a kanály. Hello průvodce automaticky vytvoří datový kanál toocopy z hello vybraná data zdroj toohello vybrané cíl. Kromě toho hello Průvodce kopírováním pomáhá toovalidate hello dat probíhá požity v době vytváření hello. Tím ušetříte čas, zvláště když je jsou příjem dat pro hello poprvé ze zdroje dat hello. toostart hello Průvodce kopírováním, klikněte na tlačítko hello **kopírování dat** dlaždici na domovskou stránku hello svojí datové továrny.

![Průvodce kopírováním](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="designed-for-big-data"></a>Určená pro velké objemy dat
Tento průvodce vám umožní tooeasily přesun dat z široké škály zdrojů toodestinations v minutách. Po projít průvodce hello kanál s aktivitou kopírování automaticky vytvoří za vás, společně s závislé entity služby Data Factory (propojené služby a datové sady). Nejsou žádné další kroky požadované toocreate hello kanálu.   

![Vyberte zdroj dat](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> Podrobné pokyny toocreate ukázková data toocopy kanálu z tabulky Azure SQL Database objektu blob Azure tooan, najdete v části hello [Průvodce kopírováním kurzu](data-factory-copy-data-wizard-tutorial.md).
>
>

Průvodce Hello je určen s velkým objemem dat v paměti od začátku hello s podporou pro různé data a typy objektů. Můžete vytvořit kanály pro vytváření dat, které přesunout stovky složky, soubory nebo tabulky. Průvodce Hello podporuje automatické datový ve verzi preview, schéma zachycení a mapování a filtrování dat.

## <a name="automatic-data-preview"></a>Náhled automatické dat
Ať co chcete hello data můžete zobrazit náhled součástí hello data z hello vybraného zdroje dat v pořadí toovalidate toocopy. Kromě toho pokud hello zdrojová data do textového souboru, hello Průvodce kopírováním analyzuje hello text souboru toolearn hello řádků a sloupců oddělovače a schéma automaticky.

![Nastavení formátu souboru](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Schéma zachycení a mapování
schéma Hello vstupních dat nemusí odpovídat schématu hello výstupních dat, v některých případech. V tomto scénáři musíte toomap sloupců z hello zdroj schématu toocolumns od schématu cílové hello.

> [!TIP]
> Při kopírování dat z SQL serveru nebo databáze SQL Azure do Azure SQL Data Warehouse, pokud tabulka hello neexistuje v hello cílové úložiště, Data Factory podporují automatické vytvoření tabulky pomocí schématu zdroje. Další informace z [přesunout tooand dat z Azure SQL Data Warehouse pomocí Azure Data Factory](./data-factory-azure-sql-data-warehouse-connector.md).
>

Pomocí rozevíracího seznamu tooselect sloupce z hello zdrojový schématu toomap tooa sloupec ve schématu cílové hello. Průvodce kopírováním Hello pokusí toounderstand vaše vzor pro mapování sloupců. Platí hello stejný vzor toohello rest hello sloupců, takže není nutné tooselect hello sloupců jednotlivě toocomplete hello schéma mapování. Pokud dáváte přednost, můžete přepsat tato mapování pomocí hello rozevírací seznamy toomap hello sloupců po jednom. vzor Hello stane přesnější jako mapovat více sloupců. Hello Průvodce kopírováním neustále aktualizuje vzor hello a nakonec dosáhnou hello správné vzor můžete mapování sloupců hello má tooachieve.     

![Schéma mapování](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Filtrování dat
Data lze filtrovat zdroje dat tooselect pouze hello vyžadující toobe zkopírovat toohello jímku dat úložiště. Filtrování snižuje objem hello hello toobe zkopírovaný toohello jímku dat data ukládat a proto zvyšuje propustnost hello operace kopírování hello. Poskytuje flexibilní toofilter dat v relační databázi pomocí dotazovacího jazyka pro hello SQL, nebo soubory ve složce aplikace objektů blob v Azure pomocí [Data Factory funkce a proměnné](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Filtrování dat v databázi
Hello následující snímek obrazovky ukazuje dotaz SQL pomocí hello `Text.Format` funkce a `WindowStart` proměnné.

![Ověření výrazy](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Filtrování dat v Azure blob složce
Proměnné můžete použít v hello složky cesta toocopy data ze složky, která je určena za běhu na základě [systémové proměnné](data-factory-functions-variables.md#data-factory-system-variables). jsou podporovány Hello proměnné: **{year}**, **{month}**, **{day}**, **{hodinu}**, **{minutu}**, a **{vlastní}**. Příklad: inputfolder / {year} / {month} / {day}.

Předpokládejme, že máte vstupní složky ve formátu hello:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Klikněte na tlačítko hello **Procházet** tlačítko pro **souboru nebo složky**, procházet tooone tyto složky (například 2016 -> 03 -> 01 -> 02) a klikněte na tlačítko **zvolte**. Měli byste vidět `2016/03/01/02` hello textového pole. Nyní, nahraďte **2016** s **{year}**, **03** s **{month}**, **01** s **{day}** , a **02** s **{hodinu}**a stiskněte klávesu hello **kartě** klíč. Měli byste vidět formát hello tooselect rozevírací seznamy pro tyto čtyři proměnné:

![Pomocí systémové proměnné](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Jak ukazuje následující snímek obrazovky hello, můžete použít také **vlastní** proměnné a všechny [podporované řetězce formátu](https://msdn.microsoft.com/library/8kb3ddd4.aspx). tooselect složka s této struktury použití hello **Procházet** nejprve tlačítko. Potom můžete nahradit hodnotu s **{vlastní}**a stiskněte klávesu hello **kartě** klíče toosee hello textového pole můžete zadat řetězec formátu hello.     

![Použití vlastní proměnné](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="scheduling-options"></a>Možnosti plánování
Operace kopírování hello můžete spustit jednou nebo podle plánu (každou hodinu, denně, a tak dále). Obě tyto možnosti lze použít pro hello spektra hello konektory prostředích, včetně místní, cloud a místní kopie plochy.

Operace kopírování jednorázové umožňuje přesun dat z zdroj tooa cíl pouze jednou. Bude se vztahovat toodata jakékoli velikosti a jakýkoli podporovaný formát. kopírování Hello naplánované umožňuje toocopy data v předepsaných opakování. Můžete použít bohaté nastavení (např. Zkuste to znovu, vypršení časového limitu a upozornění) tooconfigure hello naplánované kopírování.

![Plánování vlastnosti](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a>Další kroky
Rychlé návod k používání hello Průvodce kopírováním služby Data Factory toocreate kanál s aktivitou kopírování, najdete v části [kurz: vytvoření kanálu pomocí Průvodce kopírováním hello](data-factory-copy-data-wizard-tutorial.md).
