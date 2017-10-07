---
title: "sestavení aaaDevelop U-SQL pro úlohy Azure Data Lake Analytics | Microsoft Docs"
description: "Zjistěte, jak toodevelop sestavení toobe používá a znovu použít v Data Lake Analytics úlohy. "
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/30/2016
ms.author: jejiang
ms.openlocfilehash: 86dd17b25e0967306ed36bb5b7f3178d9409d53d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-u-sql-assemblies-for-azure-data-lake-analytics-jobs"></a>Vývoj sestavení U-SQL pro úlohy Azure Data Lake Analytics
Zjistěte, jak tooturn kódu do sestavení toobe používá a znovu použít v úloh Data Lake Analytics. 

U-SQL umožňuje snadno tooadd vlastní vlastní kód v jazyce .net, například C#, VB.Net nebo F #. Vlastní modul runtime toosupport můžete nasadit i další jazyky.

Hello nejjednodušší způsob, jak toouse vlastní kód je toouse hello nástrojů Data Lake pro Visual Studio kódu možnosti. Další informace najdete v tématu [kurz: vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md). Existuje několik nevýhody použití kódu:

- pro každý skript odeslání získá nahrát Hello zdrojového kódu.
- kódu nemohou být sdíleny s ostatními úlohami.

tooaddress tyto nevýhody můžete zapnout kódu do sestavení a zaregistrovat hello sestavení toohello Data Lake Analytics katalogu.

## <a name="prerequisites"></a>Požadavky
* Visual Studio 2017, Visual Studio 2015, Visual Studio 2013 update 4 nebo Visual Studio 2012 s nainstalovaným Visual C++
* Microsoft Azure SDK pro .NET verze 2.5 nebo vyšší.  Nainstalujte ji pomocí instalačního programu webové platformy hello nebo instalační program Visual Studio
* Účet Data Lake Analytics.  Viz [Začínáme s Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-get-started-portal.md).
* Projděte hello [Začínáme s Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md) kurzu.
* Připojte tooAzure.
* Nahrání hello zdroje dat naleznete v tématu [Začínáme s Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md). 

## <a name="develop-assemblies-for-u-sql"></a>Vývoj sestavení pro U-SQL

**toocreate a odeslání úlohy U-SQL**

1. Z hello **soubor** nabídky, klikněte na tlačítko **nový**a potom klikněte na **projektu**.
2. Rozbalte položku **nainstalovaná**, **šablony**, **Azure Data Lake**, **U-SQL(ADLA)**, vyberte hello **knihovny tříd (pro U-SQL Aplikace)** šablony a pak klikněte na tlačítko **OK**.
3. Zápis kódu v Class1.cs.  Hello Následuje ukázka kódu.

        using Microsoft.Analytics.Interfaces;

        namespace USQLApplication_codebehind
        {
            [SqlUserDefinedProcessor]
            public class MyProcessor : IProcessor
            {
                public override IRow Process(IRow input, IUpdatableRow output)
                {
                    output.Set(0, input.Get<string>(0));
                    output.Set(0, input.Get<string>(0));
                    return output.AsReadOnly();
                }
            }
        }
4. Klikněte na tlačítko hello **sestavení** nabídce a pak klikněte na tlačítko **sestavit řešení** toocreate hello dll.

## <a name="register-assemblies"></a>Registrace sestavení

V tématu [použití Data Lake Analytics(U-SQL) katalogu](data-lake-analytics-use-u-sql-catalog.md).


## <a name="use-hello-assemblies"></a>Použití sestavení hello

V tématu [pomocí nástrojů hello Azure Data Lake pro Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).

## <a name="see-also"></a>Viz také
* [Začínáme s Data Lake Analytics pomocí prostředí PowerShell](data-lake-analytics-get-started-powershell.md)
* [Začínáme s Data Lake Analytics pomocí portálu Azure hello](data-lake-analytics-get-started-portal.md)
* [Pomocí nástrojů Data Lake pro Visual Studio pro vývoj aplikací U-SQL](data-lake-analytics-data-lake-tools-get-started.md)
* [Použití Data Lake Analytics(U-SQL) katalogu](data-lake-analytics-use-u-sql-catalog.md)
* [Pomocí nástrojů hello Azure Data Lake pro Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md)
