---
title: "aaaGet Začínáme s Azure Data Lake Analytics pomocí portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello Azure toocreate portálu účtu Data Lake Analytics, vytvoření úlohy Data Lake Analytics pomocí U-SQL a odeslání úlohy hello. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: edmaca
ms.openlocfilehash: 6bb54404fa42cfed25b18bc2bfb7c72e6c361149
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-portal"></a>Začínáme s Azure Data Lake Analytics s využitím webu Azure Portal
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Zjistěte, jak toouse hello Azure toocreate portálu účtů Azure Data Lake Analytics, definování úloh v [U-SQL](data-lake-analytics-u-sql-get-started.md)a odeslání úlohy toohello Data Lake Analytics služby. Další informace o Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).

## <a name="prerequisites"></a>Požadavky

Než začnete tento kurz, musíte mít **předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-data-lake-analytics-account"></a>Vytvoření účtu Data Lake Analytics

Nyní vytvoříte Data Lake Analytics a účet Data Lake Store v hello stejný čas.  Tento krok je jednoduchý a trvá jenom o toofinish 60 sekund.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na **Nový** >  **Data a analýzy** > **Data Lake Analytics**.
3. Vyberte hodnoty pro hello následující položky:
   * **Název:** Pojmenujte svůj účet Data Lake Analytics (povolena jsou pouze malá písmena a číslice).
   * **Předplatné**: Zvolte předplatné Azure použité pro účet Analytics hello hello.
   * **Skupina prostředků**. Vyberte některou z existujících skupin prostředků Azure nebo vytvořte novou.
   * **Umístění**. Vyberte datové centrum Azure pro účet Data Lake Analytics hello.
   * **Data Lake Store**: postupujte podle instrukcí toocreate hello nový účet Data Lake Store, nebo vyberte nějaký existující. 
4. Volitelně vyberte cenovou úroveň pro svůj účet Data Lake Analytics.
5. Klikněte na možnost **Vytvořit**. 


## <a name="your-first-u-sql-script"></a>Váš první skript U-SQL

Následující text Hello je velmi jednoduchý skript U-SQL. Všechny dělá je definovat na malou datovou sadu v rámci skriptu hello a zapište si tuto datovou sadu se toohello výchozí Data Lake Store jako soubor s názvem `/data.csv`.

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
```

## <a name="submit-a-u-sql-job"></a>Odeslání úlohy U-SQL

1. Z hello účtu Data Lake Analytics, klikněte na tlačítko **nová úloha**.
2. Vložte hello textu hello skript U-SQL, které jsou uvedené výše. 
3. Klikněte na **Odeslat úlohu**.   
4. Počkejte na změny stavu úlohy hello příliš**úspěšné**.
5. Pokud hello úloha se nezdařila, přečtěte si téma [sledování úloh Data Lake Analytics a odstraňování potíží](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).
6. Klikněte na tlačítko hello **výstup** a pak klikněte `data.csv`. 

## <a name="see-also"></a>Viz také

* tooget práce s vývojem aplikací U-SQL najdete v části [skriptů vyvíjet U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* toolearn U-SQL, najdete v části [Začínáme s jazykem Azure Data Lake Analytics U-SQL](data-lake-analytics-u-sql-get-started.md).
* Informace týkající se úloh správy najdete v tématu [Správa služby Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md).
