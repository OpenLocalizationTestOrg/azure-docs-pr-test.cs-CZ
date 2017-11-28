---
title: "uživatelem definované operátory aaaDevelop U-SQL (UDO) | Microsoft Docs"
description: "Zjistěte, jak toodevelop uživatelem definované operátory toobe používá a znovu použít v Data Lake Analytics úlohy. "
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
ms.openlocfilehash: 6b86618efd3751cd9a5e91875879d7dd6d6a7b02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-u-sql-user-defined-operators-udos"></a><span data-ttu-id="24178-103">Vývoj uživatelem definované operátory U-SQL (UDO)</span><span class="sxs-lookup"><span data-stu-id="24178-103">Develop U-SQL user-defined operators (UDOs)</span></span>
<span data-ttu-id="24178-104">Zjistěte, jak toodevelop uživatelem definované operátory tooprocess data v rámci úlohy U-SQL.</span><span class="sxs-lookup"><span data-stu-id="24178-104">Learn how toodevelop user-defined operators tooprocess data in a U-SQL job.</span></span>

<span data-ttu-id="24178-105">Pokyny týkající se vývoje pro obecné účely sestavení pro U-SQL najdete v tématu [sestavení vyvíjet U-SQL pro úlohy Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-assemblies.md)</span><span class="sxs-lookup"><span data-stu-id="24178-105">For instructions on developing general-purpose assemblies for U-SQL, see [Develop U-SQL assemblies for Azure Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-assemblies.md)</span></span>

## <a name="define-and-use-a-user-defined-operator-in-u-sql"></a><span data-ttu-id="24178-106">Definice a používání uživatelem definovaný operátor v U-SQL</span><span class="sxs-lookup"><span data-stu-id="24178-106">Define and use a user-defined operator in U-SQL</span></span>
<span data-ttu-id="24178-107">**toocreate a odeslání úlohy U-SQL**</span><span class="sxs-lookup"><span data-stu-id="24178-107">**toocreate and submit a U-SQL job**</span></span>

1. <span data-ttu-id="24178-108">Hello Visual Studio vyberte **soubor > Nový > Projekt > Projekt U-SQL**.</span><span class="sxs-lookup"><span data-stu-id="24178-108">From hello Visual Studio select **File > New > Project > U-SQL Project**.</span></span>
2. <span data-ttu-id="24178-109">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="24178-109">Click **OK**.</span></span> <span data-ttu-id="24178-110">Visual Studio vytvoří řešení se souborem Script.usql.</span><span class="sxs-lookup"><span data-stu-id="24178-110">Visual Studio creates a solution with a Script.usql file.</span></span>
3. <span data-ttu-id="24178-111">Z **Průzkumníku řešení**rozbalte Script.usql a potom dvakrát klikněte na **Script.usql.cs**.</span><span class="sxs-lookup"><span data-stu-id="24178-111">From **Solution Explorer**, expand Script.usql, and then double-click **Script.usql.cs**.</span></span>
4. <span data-ttu-id="24178-112">Vložte následující kód do souboru hello hello:</span><span class="sxs-lookup"><span data-stu-id="24178-112">Paste hello following code into hello file:</span></span>

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
6. <span data-ttu-id="24178-113">Otevřete **Script.usql**, a hello vložte následující skript U-SQL:</span><span class="sxs-lookup"><span data-stu-id="24178-113">Open **Script.usql**, and paste hello following U-SQL script:</span></span>

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
            too"/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);
7. <span data-ttu-id="24178-114">Zadejte účet Data Lake Analytics hello a schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="24178-114">Specify hello Data Lake Analytics account, Database, and Schema.</span></span>
8. <span data-ttu-id="24178-115">V **Průzkumníku řešení** klikněte pravým tlačítkem na položku **Script.usql** a pak klikněte na možnost **Sestavit skript**.</span><span class="sxs-lookup"><span data-stu-id="24178-115">From **Solution Explorer**, right-click **Script.usql**, and then click **Build Script**.</span></span>
9. <span data-ttu-id="24178-116">V **Průzkumníku řešení** klikněte pravým tlačítkem na položku **Script.usql** a pak klikněte na položku **Odeslat skript**.</span><span class="sxs-lookup"><span data-stu-id="24178-116">From **Solution Explorer**, right-click **Script.usql**, and then click **Submit Script**.</span></span>
10. <span data-ttu-id="24178-117">Pokud jste se nepřipojili tooyour předplatné Azure, budete mít výzvami tooenter přihlašovací údaje účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="24178-117">If you haven't connected tooyour Azure subscription, you will be prompted tooenter your Azure account credentials.</span></span>
11. <span data-ttu-id="24178-118">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="24178-118">Click **Submit**.</span></span> <span data-ttu-id="24178-119">Výsledky odeslání a odkaz na úlohu jsou k dispozici v okně Výsledky hello po dokončení odesílání hello.</span><span class="sxs-lookup"><span data-stu-id="24178-119">Submission results and job link are available in hello Results window when hello submission is completed.</span></span>
12. <span data-ttu-id="24178-120">Klikněte na tlačítko hello **aktualizovat** tlačítko toosee hello nejnovější úlohy stavu a aktualizace úvodní obrazovka.</span><span class="sxs-lookup"><span data-stu-id="24178-120">Click hello **Refresh** button toosee hello latest job status and refresh hello screen.</span></span>

<span data-ttu-id="24178-121">**výstup hello toosee**</span><span class="sxs-lookup"><span data-stu-id="24178-121">**toosee hello output**</span></span>

1. <span data-ttu-id="24178-122">Z **Průzkumníka serveru**, rozbalte položku **Azure**, rozbalte položku **Data Lake Analytics**, rozbalte účet Data Lake Analytics, rozbalte položku **účty úložiště**, klikněte pravým tlačítkem na hello výchozí úložiště a pak klikněte na tlačítko **Explorer**.</span><span class="sxs-lookup"><span data-stu-id="24178-122">From **Server Explorer**, expand **Azure**, expand **Data Lake Analytics**, expand your Data Lake Analytics account, expand **Storage Accounts**, right-click hello Default Storage, and then click **Explorer**.</span></span>
2. <span data-ttu-id="24178-123">Rozbalte ukázky, rozbalte výstupy a potom dvakrát klikněte na **ovladače.csv**.</span><span class="sxs-lookup"><span data-stu-id="24178-123">Expand Samples, expand Outputs, and then double-click **Drivers.csv**.</span></span>

## <a name="see-also"></a><span data-ttu-id="24178-124">Viz také</span><span class="sxs-lookup"><span data-stu-id="24178-124">See also</span></span>
* [<span data-ttu-id="24178-125">Začínáme s Data Lake Analytics pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="24178-125">Get started with Data Lake Analytics using PowerShell</span></span>](data-lake-analytics-get-started-powershell.md)
* [<span data-ttu-id="24178-126">Začínáme s Data Lake Analytics pomocí portálu Azure hello</span><span class="sxs-lookup"><span data-stu-id="24178-126">Get started with Data Lake Analytics using hello Azure portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="24178-127">Pomocí nástrojů Data Lake pro Visual Studio pro vývoj aplikací U-SQL</span><span class="sxs-lookup"><span data-stu-id="24178-127">Use Data Lake Tools for Visual Studio for developing U-SQL applications</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
