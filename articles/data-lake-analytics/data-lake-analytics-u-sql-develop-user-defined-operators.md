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
# <a name="develop-u-sql-user-defined-operators-udos"></a><span data-ttu-id="60ec0-103">Vývoj uživatelem definované operátory U-SQL (UDO)</span><span class="sxs-lookup"><span data-stu-id="60ec0-103">Develop U-SQL user-defined operators (UDOs)</span></span>
<span data-ttu-id="60ec0-104">Další informace jak vyvíjet uživatelem definované operátory při zpracování dat v rámci úlohy U-SQL.</span><span class="sxs-lookup"><span data-stu-id="60ec0-104">Learn how to develop user-defined operators to process data in a U-SQL job.</span></span>

<span data-ttu-id="60ec0-105">Pokyny týkající se vývoje pro obecné účely sestavení pro U-SQL najdete v tématu [sestavení vyvíjet U-SQL pro úlohy Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-assemblies.md)</span><span class="sxs-lookup"><span data-stu-id="60ec0-105">For instructions on developing general-purpose assemblies for U-SQL, see [Develop U-SQL assemblies for Azure Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-assemblies.md)</span></span>

## <a name="define-and-use-a-user-defined-operator-in-u-sql"></a><span data-ttu-id="60ec0-106">Definice a používání uživatelem definovaný operátor v U-SQL</span><span class="sxs-lookup"><span data-stu-id="60ec0-106">Define and use a user-defined operator in U-SQL</span></span>
<span data-ttu-id="60ec0-107">**K vytvoření a odeslání úlohy U-SQL**</span><span class="sxs-lookup"><span data-stu-id="60ec0-107">**To create and submit a U-SQL job**</span></span>

1. <span data-ttu-id="60ec0-108">V sadě Visual Studio vyberte **soubor > Nový > Projekt > Projekt U-SQL**.</span><span class="sxs-lookup"><span data-stu-id="60ec0-108">From the Visual Studio select **File > New > Project > U-SQL Project**.</span></span>
2. <span data-ttu-id="60ec0-109">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="60ec0-109">Click **OK**.</span></span> <span data-ttu-id="60ec0-110">Visual Studio vytvoří řešení se souborem Script.usql.</span><span class="sxs-lookup"><span data-stu-id="60ec0-110">Visual Studio creates a solution with a Script.usql file.</span></span>
3. <span data-ttu-id="60ec0-111">Z **Průzkumníku řešení**rozbalte Script.usql a potom dvakrát klikněte na **Script.usql.cs**.</span><span class="sxs-lookup"><span data-stu-id="60ec0-111">From **Solution Explorer**, expand Script.usql, and then double-click **Script.usql.cs**.</span></span>
4. <span data-ttu-id="60ec0-112">Vložte následující kód do souboru:</span><span class="sxs-lookup"><span data-stu-id="60ec0-112">Paste the following code into the file:</span></span>

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
6. <span data-ttu-id="60ec0-113">Otevřete **Script.usql**a vložte následující skript U-SQL:</span><span class="sxs-lookup"><span data-stu-id="60ec0-113">Open **Script.usql**, and paste the following U-SQL script:</span></span>

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
7. <span data-ttu-id="60ec0-114">Zadejte účet Data Lake Analytics, Databázi a Schéma.</span><span class="sxs-lookup"><span data-stu-id="60ec0-114">Specify the Data Lake Analytics account, Database, and Schema.</span></span>
8. <span data-ttu-id="60ec0-115">V **Průzkumníku řešení** klikněte pravým tlačítkem na položku **Script.usql** a pak klikněte na možnost **Sestavit skript**.</span><span class="sxs-lookup"><span data-stu-id="60ec0-115">From **Solution Explorer**, right-click **Script.usql**, and then click **Build Script**.</span></span>
9. <span data-ttu-id="60ec0-116">V **Průzkumníku řešení** klikněte pravým tlačítkem na položku **Script.usql** a pak klikněte na položku **Odeslat skript**.</span><span class="sxs-lookup"><span data-stu-id="60ec0-116">From **Solution Explorer**, right-click **Script.usql**, and then click **Submit Script**.</span></span>
10. <span data-ttu-id="60ec0-117">Pokud jste se nepřipojili k předplatnému Azure, vyzve k zadání přihlašovacích údajů účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="60ec0-117">If you haven't connected to your Azure subscription, you will be prompted to enter your Azure account credentials.</span></span>
11. <span data-ttu-id="60ec0-118">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="60ec0-118">Click **Submit**.</span></span> <span data-ttu-id="60ec0-119">Výsledky odeslání a odkaz na úlohu jsou k dispozici v okně výsledků po dokončení odesílání.</span><span class="sxs-lookup"><span data-stu-id="60ec0-119">Submission results and job link are available in the Results window when the submission is completed.</span></span>
12. <span data-ttu-id="60ec0-120">Klikněte **aktualizovat** tlačítko zobrazíte nejnovější stav úlohy a aktualizovat obrazovku.</span><span class="sxs-lookup"><span data-stu-id="60ec0-120">Click the **Refresh** button to see the latest job status and refresh the screen.</span></span>

<span data-ttu-id="60ec0-121">**Chcete-li zobrazit výstup**</span><span class="sxs-lookup"><span data-stu-id="60ec0-121">**To see the output**</span></span>

1. <span data-ttu-id="60ec0-122">Z **Průzkumníka serveru**, rozbalte položku **Azure**, rozbalte položku **Data Lake Analytics**, rozbalte účet Data Lake Analytics, rozbalte položku **účty úložiště**, klikněte pravým tlačítkem na výchozí úložiště a pak klikněte na tlačítko **Explorer**.</span><span class="sxs-lookup"><span data-stu-id="60ec0-122">From **Server Explorer**, expand **Azure**, expand **Data Lake Analytics**, expand your Data Lake Analytics account, expand **Storage Accounts**, right-click the Default Storage, and then click **Explorer**.</span></span>
2. <span data-ttu-id="60ec0-123">Rozbalte ukázky, rozbalte výstupy a potom dvakrát klikněte na **ovladače.csv**.</span><span class="sxs-lookup"><span data-stu-id="60ec0-123">Expand Samples, expand Outputs, and then double-click **Drivers.csv**.</span></span>

## <a name="see-also"></a><span data-ttu-id="60ec0-124">Viz také</span><span class="sxs-lookup"><span data-stu-id="60ec0-124">See also</span></span>
* [<span data-ttu-id="60ec0-125">Začínáme s Data Lake Analytics pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="60ec0-125">Get started with Data Lake Analytics using PowerShell</span></span>](data-lake-analytics-get-started-powershell.md)
* [<span data-ttu-id="60ec0-126">Začínáme s Data Lake Analytics pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="60ec0-126">Get started with Data Lake Analytics using the Azure portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="60ec0-127">Pomocí nástrojů Data Lake pro Visual Studio pro vývoj aplikací U-SQL</span><span class="sxs-lookup"><span data-stu-id="60ec0-127">Use Data Lake Tools for Visual Studio for developing U-SQL applications</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
