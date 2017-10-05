---
title: "Vývoj uživatelem definované operátory U-SQL (UDO) | Microsoft Docs"
description: "Další informace jak vyvíjet uživatelem definované operátory k použití a znovu použít v úloh Data Lake Analytics. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: e5189e4e-9438-46d1-8686-ed4836bf3356
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: fdee02fb60b633c26704fc1774dfc3a7825b5e0d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="develop-u-sql-user-defined-operators-udos"></a>Vývoj uživatelem definované operátory U-SQL (UDO)
Další informace jak vyvíjet uživatelem definované operátory při zpracování dat v rámci úlohy U-SQL.

Pokyny týkající se vývoje pro obecné účely sestavení pro U-SQL najdete v tématu [sestavení vyvíjet U-SQL pro úlohy Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-assemblies.md)

## <a name="define-and-use-a-user-defined-operator-in-u-sql"></a>Definice a používání uživatelem definovaný operátor v U-SQL
**K vytvoření a odeslání úlohy U-SQL**

1. V sadě Visual Studio vyberte **soubor > Nový > Projekt > Projekt U-SQL**.
2. Klikněte na **OK**. Visual Studio vytvoří řešení se souborem Script.usql.
3. Z **Průzkumníku řešení**rozbalte Script.usql a potom dvakrát klikněte na **Script.usql.cs**.
4. Vložte následující kód do souboru:

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;

        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Suisse", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };

                public override IRow Process(IRow input, IUpdatableRow output)
                {

                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");

                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);

                    return output.AsReadOnly();
                }
            }
        }
6. Otevřete **Script.usql**a vložte následující skript U-SQL:

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);

        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    

        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);
7. Zadejte účet Data Lake Analytics, Databázi a Schéma.
8. V **Průzkumníku řešení** klikněte pravým tlačítkem na položku **Script.usql** a pak klikněte na možnost **Sestavit skript**.
9. V **Průzkumníku řešení** klikněte pravým tlačítkem na položku **Script.usql** a pak klikněte na položku **Odeslat skript**.
10. Pokud jste se nepřipojili k předplatnému Azure, vyzve k zadání přihlašovacích údajů účtu Azure.
11. Klikněte na tlačítko **odeslání**. Výsledky odeslání a odkaz na úlohu jsou k dispozici v okně výsledků po dokončení odesílání.
12. Klikněte **aktualizovat** tlačítko zobrazíte nejnovější stav úlohy a aktualizovat obrazovku.

**Chcete-li zobrazit výstup**

1. Z **Průzkumníka serveru**, rozbalte položku **Azure**, rozbalte položku **Data Lake Analytics**, rozbalte účet Data Lake Analytics, rozbalte položku **účty úložiště**, klikněte pravým tlačítkem na výchozí úložiště a pak klikněte na tlačítko **Explorer**.
2. Rozbalte ukázky, rozbalte výstupy a potom dvakrát klikněte na **ovladače.csv**.

## <a name="see-also"></a>Viz také
* [Začínáme s Data Lake Analytics pomocí prostředí PowerShell](data-lake-analytics-get-started-powershell.md)
* [Začínáme s Data Lake Analytics pomocí portálu Azure](data-lake-analytics-get-started-portal.md)
* [Pomocí nástrojů Data Lake pro Visual Studio pro vývoj aplikací U-SQL](data-lake-analytics-data-lake-tools-get-started.md)
