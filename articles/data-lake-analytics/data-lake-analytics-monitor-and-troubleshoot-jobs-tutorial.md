---
title: "aaaTroubleshoot úlohy Azure Data Lake Analytics pomocí portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello úloh Data Lake Analytics tootroubleshoot portálu Azure. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: b7066d81-3142-474f-8a34-32b0b39656dc
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: e810d56bab8f1a8254721ec9906bb6a4508dc22a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-data-lake-analytics-jobs-using-azure-portal"></a>Řešení potíží s úlohy Azure Data Lake Analytics pomocí portálu Azure
Zjistěte, jak toouse hello úloh Data Lake Analytics tootroubleshoot portálu Azure.

V tomto kurzu se nastavit chybějící problém zdrojový soubor a používat hello portálu Azure tootroubleshoot hello problém.

## <a name="submit-a-data-lake-analytics-job"></a>Odeslání úlohy Data Lake Analytics

Odešlete hello následující úlohy U-SQL:

```
@searchlog =
   EXTRACT UserId          int,
           Start           DateTime,
           Region          string,
           Query           string,
           Duration        int?,
           Urls            string,
           ClickedUrls     string
   FROM "/Samples/Data/SearchLog.tsv1"
   USING Extractors.Tsv();

OUTPUT @searchlog   
   too"/output/SearchLog-from-adls.csv"
   USING Outputters.Csv();
```
    
Hello zdrojový soubor definované ve skriptu hello je **/Samples/Data/SearchLog.tsv1**, kde by měly být **/Samples/Data/SearchLog.tsv**.


## <a name="troubleshoot-hello-job"></a>Řešení potíží s hello úlohy

**toosee všechny hello úlohy**

1. Z hello portálu Azure, klikněte na tlačítko **Microsoft Azure** v levém horním rohu hello.
2. Klikněte na dlaždici hello s název účtu Data Lake Analytics.  Hello úlohy souhrnu se zobrazí na hello **úlohy správy** dlaždici.

    ![Správa úloh Azure Data Lake Analytics](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    Hello úlohy správy poskytuje přehled o stavu úlohy hello. Všimněte si, že je neúspěšnou úlohu.
3. Klikněte na tlačítko hello **úlohy správy** dlaždici toosee hello úlohy. Hello úlohy jsou rozdělené do **systémem**, **zařazeno ve frontě**, a **ukončeno**. Zobrazí se vaše neúspěšnou úlohu v hello **ukončeno** části. Použije se první z nich v seznamu hello. Pokud máte mnoho úloh, můžete kliknout na **filtru** toohelp jste toolocate úlohy.

    ![Filtrujte úlohy Azure Data Lake Analytics](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)
4. Klikněte na tlačítko hello neúspěšnou úlohu z podrobnosti úlohy hello seznamu tooopen hello v novém okně:

    ![Azure Data Lake Analytics se nezdařilo úlohy](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    Všimněte si hello **odešlete znovu** tlačítko. Po vyřešení problému hello znovu odešlete hello úlohy.
5. Klikněte na tlačítko zvýrazněná část z hello předchozí snímek obrazovky tooopen hello podrobnosti o chybě.  Zobrazí se něco podobného jako:

    ![Azure Data Lake Analytics se nezdařilo podrobnosti úlohy](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    Zde zjistíte, že nebyl nalezen hello zdrojové složky.
6. Klikněte na tlačítko **duplicitní skriptu**.
7. Aktualizace hello **FROM** toohello následující cesty:

    "/ Samples/Data/SearchLog.tsv"
8. Klikněte na **Odeslat úlohu**.

## <a name="see-also"></a>Viz také
* [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Začínáme s Azure Data Lake Analytics pomocí Azure PowerShell](data-lake-analytics-get-started-powershell.md)
* [Začínáme s Azure Data Lake Analytics a jazykem U-SQL pomocí sady Visual Studio](data-lake-analytics-u-sql-get-started.md)
* [Správa Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md)
