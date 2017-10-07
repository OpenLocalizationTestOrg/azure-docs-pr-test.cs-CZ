---
title: "úlohy aaaDebug U-SQL | Microsoft Docs"
description: "Zjistěte, jak toodebug U-SQL se nezdařilo vrchol pomocí sady Visual Studio."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: bcd0b01e-1755-4112-8e8a-a5cabdca4df2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/02/2016
ms.author: saveenr
ms.openlocfilehash: 092bffa1a59ed91c5837402d0276447480b923fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a>Ladění uživatelem definované C# – kód pro selhání úloh U-SQL

U-SQL poskytuje model rozšiřitelnosti pomocí jazyka C#, takže můžete napsat váš kód tooadd funkce například vlastní Extraktor nebo reduktorem. Další, najdete v části toolearn [U-SQL programovatelnosti průvodce](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf). V praxi může potřebovat žádný kód, ladění a systémy velkých objemů dat může poskytnout jenom omezené runtime ladění informace, jako jsou soubory protokolu.

Azure nástrojů Data Lake pro Visual Studio poskytuje funkci **se nezdařilo ladění vrchol**, která vám umožňuje klonovat neúspěšnou úlohu z místního počítače tooyour hello cloudu pro ladění. Vytvořit místní kopii Hello zaznamená hello celého cloudového prostředí, včetně všech vstupních dat a uživatelského kódu.

Hello toto video ukazuje se nezdařilo vrchol ladění ve nástrojů Azure Data Lake pro Visual Studio.

> [!VIDEO https://e0d1.wpc.azureedge.net/80E0D1/OfficeMixProdMediaBlobStorage/asset-d3aeab42-6149-4ecc-b044-aa624901ab32/b0fc0373c8f94f1bb8cd39da1310adb8.mp4?sv=2012-02-12&sr=c&si=a91fad76-cfdd-4513-9668-483de39e739c&sig=K%2FR%2FdnIi9S6P%2FBlB3iLAEV5pYu6OJFBDlQy%2FQtZ7E7M%3D&se=2116-07-19T09:27:30Z&rscd=attachment%3B%20filename%3DDebugyourcustomcodeinUSQLADLA.mp4]
>

> [!NOTE]
> Visual Studio vyžaduje hello následující dvě aktualizace, pokud již nejsou nainstalovány: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) a [Universal C Runtime pro systém Windows](https://www.microsoft.com/download/details.aspx?id=50410).

## <a name="download-failed-vertex-toolocal-machine"></a>Počítač toolocal vrchol stažení se nezdařilo.

Při otevření neúspěšnou úlohu v nástrojů Azure Data Lake pro Visual Studio zobrazí žlutý výstrahy pruh s podrobné chybové zprávy v kartě chyby hello.

1. Klikněte na tlačítko **Stáhnout** toodownload hello všechny potřebné prostředky a vstupní datové proudy. Pokud stahování hello nedokončí, klikněte na tlačítko **opakujte**.

2. Klikněte na tlačítko **otevřete** po dokončení stahování hello toogenerate prostředí místní ladění. Nová instance Visual Studio s ladění řešení je automaticky vytvořen a otevřít.

![Azure Data Lake Analytics U-SQL vrchol stažení sady visual studio pro ladění](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

Úlohy mohou zahrnovat soubory zdrojového kódu nebo registrovaný sestavení a tyto dva typy mají různé scénáře ladění.

- [Ladění úlohy se nezdařilo s kódem v pozadí](#debug-job-failed-with-code-behind)
- [Ladění neúspěšnou úlohu se sestavení](#debug-job-failed-with-assemblies)


## <a name="debug-job-failed-with-code-behind"></a>Ladění úlohy se nezdařilo s kódem v pozadí

Pokud se nezdaří úlohy U-SQL a hello úlohy zahrnuje uživatelského kódu (obvykle s názvem `Script.usql.cs` v projektu U-SQL), že zdrojový kód je importovat do hello ladění řešení.  Odtud můžete hello Visual Studio ladění nástrojů (sledovat, proměnné atd.) tootroubleshoot hello problém.

> [!NOTE]
> Před ladění, se že toocheck **výjimky modulu CLR** v okně Nastavení výjimky hello (**Ctrl + Alt + E**).

![Azure Data Lake Analytics U-SQL ladění sady visual studio nastavení](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

1. Stiskněte klávesu **F5** toorun hello kódu kódu. Bude fungovat, dokud nebude zastaven výjimkou.

2. Otevřete hello `ADLTool_Codebehind.usql.cs` souboru a nastavit zarážky, stiskněte **F5** toodebug hello kód krok za krokem.

    ![Azure Data Lake Analytics U-SQL ladění výjimek](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

## <a name="debug-job-failed-with-assemblies"></a>Ladění úlohy se nezdařilo s sestavení

Pokud používáte registrované sestavení ve vašem skriptu U-SQL, hello systému nelze získat hello zdrojového kódu. V takovém případě ručně přidejte hello sestavení zdrojového kódu soubory toohello řešení.

### <a name="configure-hello-solution"></a>Konfigurace řešení hello

1. Klikněte pravým tlačítkem na **řešení 'VertexDebug' > Přidat > existující projekt...**  toofind hello se sestavení zdrojového kódu a přidejte toohello projektu hello ladění řešení.

    ![Přidání projektu Azure Data Lake Analytics U-SQL ladění](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. Klikněte pravým tlačítkem na **LocalVertexHost > vlastnosti** v řešení a zkopírujte hello hello **pracovní adresář** cesta.

3. Klikněte pravým tlačítkem na **sestavení projektu zdrojového kódu > vlastnosti**, vyberte hello **sestavení** kartě na levé straně a vložte hello zkopírovat cestu jako **výstup > Výstupní cesta**.

    ![Azure Data Lake Analytics U-SQL ladění, nastavte cestu pdb](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

4. Stiskněte klávesu **Ctrl + Alt + E**, zkontrolujte **výjimky modulu CLR** v okně Nastavení výjimky.

### <a name="start-debug"></a>Spuštění ladění

1. Klikněte pravým tlačítkem na **sestavení projektu zdrojového kódu > sestavit** toooutput PDB soubory toohello `LocalVertexHost` pracovní adresář.

2. Stiskněte klávesu **F5** a hello projektu se spustí, dokud nebude zastaven výjimkou. Může se zobrazit hello následující upozornění, které můžete bezpečně ignorovat. To může trvat až tooa minut tooget toohello ladění obrazovky.

    ![Azure Data Lake Analytics U-SQL ladění sady visual studio upozornění](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

3. Otevřete vašeho zdrojového kódu a nastavit zarážky, stiskněte **F5** toodebug hello kód krok za krokem.

Můžete také použít hello Visual Studio ladění nástrojů (sledovat, proměnné atd.) tootroubleshoot hello problém.

> [!NOTE]
> Znovu sestavte projekt hello sestavení zdrojového kódu pokaždé, když po úpravě soubory PDB toogenerate aktualizovat kód hello.

Po ladění, po úspěšném dokončení projektu hello okno výstup hello ukazuje hello následující zprávou:

```
hello Program 'LocalVertexHost.exe' has exited with code 0 (0x0).
```

![Azure Data Lake Analytics U-SQL ladění úspěšné](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

## <a name="resubmit-hello-job"></a>Odešlete znovu úlohu hello

Po dokončení ladění odešlete znovu hello neúspěšnou úlohu.

1. Pro úlohy s kódem v pozadí řešení, zkopírujte kódu C# do souboru zdrojového kódu hello (obvykle `Script.usql.cs`).
2. Pro úlohy se sestaveními registraci sestavení .dll hello aktualizovat do vaší databáze ADLA:
    1. Z Průzkumníka serveru nebo v Průzkumníku cloudu, rozbalte položku hello **ADLA účet > databáze** uzlu.
    2. Klikněte pravým tlačítkem na **sestavení** a registraci vaší nové sestavení .dll s databází ADLA hello: ![Azure Data Lake Analytics U-SQL ladění registrace sestavení](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)
3. Odešlete znovu úlohu.

## <a name="next-steps"></a>Další kroky

- [Průvodce programovatelnosti U-SQL](data-lake-analytics-u-sql-programmability-guide.md)
- [Vývoj U-SQL uživatelem definované operátory pro úlohy Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [Kurz: vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
