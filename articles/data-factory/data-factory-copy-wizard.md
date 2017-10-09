---
title: "aaaCopy dat snadno pomocí Průvodce kopírováním - Azure | Microsoft Docs"
description: "Informace o tom, jak toouse hello Průvodce kopírováním služby Data Factory toocopy data z toosinks podporované datové zdroje."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: f904972f-cd33-48db-9755-2b3196ae4168
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 99437ec16facf3b94c8be18487ec89e9f13acc9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-or-move-data-easily-with-azure-data-factory-copy-wizard"></a>Zkopírovat nebo přesunout data snadno pomocí Průvodce kopírováním objekt pro vytváření dat Azure
Hello Průvodce kopírováním objekt pro vytváření dat Azure je proces hello tooease příjem dat, který je obvykle prvním krokem v případě integrace pomocí dat začátku do konce. Při průchodu přes hello Průvodce kopírováním objekt pro vytváření dat Azure, není nutné toounderstand všechny definice JSON propojené služby, datové sady a kanály. Ale po dokončení všech kroků hello v Průvodci hello hello průvodce automaticky vytvoří toocopy datový kanál z hello vybraná data zdroj toohello vybrané cíl. Kromě toho hello Průvodce kopírováním vám pomůže toovalidate hello dat probíhá požity hello během vytváření, které ukládá většinu doby, hlavně když jste jsou příjem dat pro hello poprvé ze zdroje dat hello. toostart hello Průvodce kopírováním, klikněte na tlačítko hello **kopírování dat** dlaždici na domovskou stránku hello svojí datové továrny.

![Průvodce kopírováním](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="an-intuitive-wizard-for-copying-data"></a>Intuitivní Průvodce pro kopírování dat
Tento průvodce vám umožní tooeasily přesun dat z široké škály zdrojů toodestinations v minutách. Poté, co projde hello průvodce, kanál s aktivitou kopírování se automaticky vytvoří za vás společně s závislé entity služby Data Factory (propojené služby a datové sady). Nejsou žádné další kroky požadované toocreate hello kanálu.   

![Vyberte zdroj dat](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> V tématu [Průvodce kopírováním kurzu](data-factory-copy-data-wizard-tutorial.md) článek pro podrobné pokyny toocreate ukázková data toocopy kanálu z tabulky Azure SQL Database tooan objektů blob v Azure. 
> 
> 

Průvodce Hello je určen s velkým objemem dat v paměti od začátku hello. Je jednoduchý a efektivní tooauthor Data Factory kanály, které přesunout stovky složky, soubory nebo tabulky pomocí Průvodce kopírování dat hello. Hello Průvodce podporuje následující tři funkce hello: náhled dat, schématu zachycení a mapování a filtrování dat. 

## <a name="automatic-data-preview"></a>Náhled automatické dat
Průvodce kopírováním Hello umožňuje vám tooreview hello data z hello vybrané části zdroj dat pro vás toovalidate zda hello dat je hello pravým data, která chcete toocopy. Kromě toho pokud hello zdrojová data do textového souboru, Průvodce kopírováním hello analyzuje hello text souboru toolearn řádků a sloupců oddělovače a schématu automaticky. 

![Nastavení formátu souboru](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Schéma zachycení a mapování
schéma Hello vstupních dat nemusí odpovídat schématu hello výstupních dat, v některých případech. V tomto scénáři musíte toomap sloupců z hello zdroj schématu toocolumns od schématu cílové hello. 

Průvodce kopírováním Hello automaticky mapuje sloupců v hello zdroj schématu toocolumns ve schématu cílové hello. Můžete přepsat hello mapování pomocí rozevíracích seznamů hello (nebo) určete, zda sloupec musí toobe při kopírování dat hello vynecháno.   

![Schéma mapování](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Filtrování dat
Průvodce Hello vám umožní toofilter zdroje dat tooselect pouze hello data, která potřebují úložiště dat cílové/jímka toohello toobe zkopírovali. Filtrování snižuje objem hello hello toobe zkopírovaný toohello jímku dat data ukládat a proto zvyšuje propustnost hello operace kopírování hello. Poskytuje flexibilní toofilter dat v relační databázi pomocí SQL dotazu jazyka (nebo) soubory ve složce aplikace objektů blob v Azure pomocí [Data Factory funkce a proměnné](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Filtrování dat v databázi
V příkladu hello dotazu SQL hello používá hello `Text.Format` funkce a `WindowStart` proměnné. 

![Ověření výrazy](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Filtrování dat v Azure blob složce
Proměnné můžete použít v hello složky cesta toocopy data ze složky, která je určena za běhu na základě [systémové proměnné](data-factory-functions-variables.md#data-factory-system-variables). jsou podporovány Hello proměnné: **{year}**, **{month}**, **{day}**, **{hodinu}**, **{minutu}**, a **{vlastní}**. Příklad: inputfolder / {year} / {month} / {day}.

Předpokládejme, že máte vstupní složky ve formátu hello:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Klikněte na tlačítko hello **Procházet** tlačítko pro **souboru nebo složky**, procházet tooone tyto složky (například 2016 -> 03 -> 01 -> 02) a klikněte na tlačítko **zvolte**. Měli byste vidět `2016/03/01/02` hello textového pole. Nyní, nahraďte **2016** s **{year}**, **03** s **{month}**, **01** s **{day}**, a **02** s **{hodinu}**, a stiskněte klávesu Tab. Měli byste vidět formát hello tooselect rozevírací seznamy pro tyto čtyři proměnné:

![Pomocí systémové proměnné](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Jak ukazuje následující snímek obrazovky hello, můžete použít také **vlastní** proměnné a všechny [podporované řetězce formátu](https://msdn.microsoft.com/library/8kb3ddd4.aspx). tooselect složka s této struktury použití hello **Procházet** nejprve tlačítko. Potom můžete nahradit hodnotu s **{vlastní}**a stiskněte klávesu Tab toosee hello textového pole můžete zadat řetězec formátu hello.     

![Použití vlastní proměnné](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="support-for-diverse-data-and-object-types"></a>Podpora různých dat a typy objektů
Pomocí Průvodce kopírováním hello můžete efektivně přesouvají stovky složky, soubory nebo tabulky.

![Vyberte tabulky, z které toocopy dat](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>Možnosti plánování
Operace kopírování hello můžete spustit jednou nebo podle plánu (každou hodinu, denně, a tak dále). Obě tyto možnosti lze použít pro hello spektra hello konektory napříč místními, cloud a místní kopie plochy.

Operace kopírování jednorázové umožňuje přesun dat z zdroj tooa cíl pouze jednou. Bude se vztahovat toodata jakékoli velikosti a jakýkoli podporovaný formát. kopírování Hello naplánované umožňuje toocopy data v předepsaných opakování. Můžete použít bohaté nastavení (např. Zkuste to znovu, vypršení časového limitu a upozornění) tooconfigure hello naplánované kopírování.

![Plánování vlastnosti](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a>Další kroky
Rychlé návod k používání hello Průvodce kopírováním služby Data Factory toocreate kanál s aktivitou kopírování, najdete v části [kurz: vytvoření kanálu pomocí Průvodce kopírováním hello](data-factory-copy-data-wizard-tutorial.md).

