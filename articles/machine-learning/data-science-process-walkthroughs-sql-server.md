---
title: "Návody vědecké účely dat systému SQL Server pomocí R, Python a T-SQL | Microsoft Docs"
description: "Příklady, které provede použití R, Python a T-SQL v systému SQL Server pro prediktivní analýzy."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: bradsev
ms.openlocfilehash: 333fd4ee6dcdcbfd347d6b5e8cb900474f6ec8cc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="sql-server-data-science-walkthroughs-using-r-python-and-t-sql"></a><span data-ttu-id="ade81-103">Návody vědecké účely dat systému SQL Server pomocí R, Python a T-SQL</span><span class="sxs-lookup"><span data-stu-id="ade81-103">SQL Server data science walkthroughs using R, Python and T-SQL</span></span>

<span data-ttu-id="ade81-104">Tyto postupy použijte pro prediktivní analýzy systému SQL Server, služby SQL Server R a Python služby serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ade81-104">These walkthroughs use SQL Server, SQL Server R Services, and SQL Server Python Services to do predictive analytics.</span></span> <span data-ttu-id="ade81-105">Kód R nebo Python je nasazena v uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="ade81-105">R and Python code is deployed in stored procedures.</span></span> <span data-ttu-id="ade81-106">Jejich postupujte podle kroků uvedených v procesu Team dat vědecké účely.</span><span class="sxs-lookup"><span data-stu-id="ade81-106">They follow the steps outlined in the Team Data Science Process.</span></span> <span data-ttu-id="ade81-107">Přehled procesu Team dat vědecké účely, najdete v tématu [proces vědecké účely dat](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ade81-107">For an overview of the Team Data Science Process, see [Data Science Process](data-science-process-overview.md).</span></span> 

<span data-ttu-id="ade81-108">Návody vědecké účely další data, které se spouští proces vědecké účely Team dat jsou seskupené podle **platformy** , které používají:</span><span class="sxs-lookup"><span data-stu-id="ade81-108">Additional data science walkthroughs that execute the Team Data Science Process are grouped by the **platform** that they use:</span></span> 

[!INCLUDE [tdsp-walkthroughs-by-platform](../../includes/tdsp-walkthroughs-by-platform.md)]


## <a name="predict-taxi-tips-using-python-and-sql-queries-with-sql-server"></a><span data-ttu-id="ade81-109">Předpověď taxíkem tipy pomocí dotazů Python a SQL s SQL serverem</span><span class="sxs-lookup"><span data-stu-id="ade81-109">Predict taxi tips using Python and SQL queries with SQL Server</span></span> 

<span data-ttu-id="ade81-110">[Použít SQL Server](machine-learning-data-science-process-sql-walkthrough.md) návod ukazuje, jak vytvořit a nasadit machine learning klasifikace a modely regrese pomocí systému SQL Server a veřejně dostupné NYC taxi služební cestě a tarif datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="ade81-110">The [Use SQL Server](machine-learning-data-science-process-sql-walkthrough.md) walkthrough shows how you build and deploy machine learning classification and regression models using SQL Server and a publicly available NYC taxi trip and fare dataset.</span></span>


## <a name="predict-taxi-tips-using-microsoft-r-with-sql-server"></a><span data-ttu-id="ade81-111">Předpověď taxíkem tipy pomocí Microsoft R SQL Server</span><span class="sxs-lookup"><span data-stu-id="ade81-111">Predict taxi tips using Microsoft R with SQL Server</span></span> 

<span data-ttu-id="ade81-112">[Použití SQL serveru R Services](https://msdn.microsoft.com/library/mt612857.aspx) názorný postup obsahuje datových vědců s kombinací kódu jazyka R, dat systému SQL Server, a vlastní funkce SQL k sestavení a nasazení R model k systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ade81-112">The [Use SQL Server R Services](https://msdn.microsoft.com/library/mt612857.aspx) walkthrough provides data scientists with a combination of R code, SQL Server data, and custom SQL functions to build and deploy an R model to SQL Server.</span></span> <span data-ttu-id="ade81-113">Průvodce je určena pro vývojáře R představit R služby (v databázi).</span><span class="sxs-lookup"><span data-stu-id="ade81-113">The walkthrough is designed to introduce R developers to R Services (In-Database).</span></span>


## <a name="predict-taxi-tips-using-r-from-t-sql-or-stored-procedures-with-sql-server"></a><span data-ttu-id="ade81-114">Předpověď taxíkem tipy R z T-SQL nebo uložené procedury pomocí systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="ade81-114">Predict taxi tips using R from T-SQL or stored procedures with SQL Server</span></span>

<span data-ttu-id="ade81-115">[Datové vědy návod pro R a SQL Server](https://docs.microsoft.com/en-us/sql/advanced-analytics/tutorials/walkthrough-data-science-end-to-end-walkthrough) poskytuje programátory v jazyce SQL s prostředím sestavování řešení s pokročilou analýzu pomocí jazyka Transact-SQL pomocí služby SQL Server R Services pro zprovoznění řešení s R.</span><span class="sxs-lookup"><span data-stu-id="ade81-115">The [Data science walkthrough for R and SQL Server](https://docs.microsoft.com/en-us/sql/advanced-analytics/tutorials/walkthrough-data-science-end-to-end-walkthrough) provides SQL programmers with experience building an advanced analytics solution with Transact-SQL using SQL Server R Services to operationalize an R solution.</span></span> 


## <a name="predict-taxi-tips-using-python-in-sql-server-stored-procedures"></a><span data-ttu-id="ade81-116">Předpověď taxíkem tipy v uložených procedur SQL Server používá Python</span><span class="sxs-lookup"><span data-stu-id="ade81-116">Predict taxi tips using Python in SQL Server stored procedures</span></span>

<span data-ttu-id="ade81-117">[Použití T-SQL s SQL Server Python Services](https://docs.microsoft.com/en-us/sql/advanced-analytics/tutorials/sqldev-in-database-python-for-sql-developers) názorný postup obsahuje programátory v jazyce SQL s prostředím vytváření machine learning řešení v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ade81-117">The [Use T-SQL with SQL Server Python Services](https://docs.microsoft.com/en-us/sql/advanced-analytics/tutorials/sqldev-in-database-python-for-sql-developers) walkthrough provides SQL programmers with experience building a machine learning solution in SQL Server.</span></span> <span data-ttu-id="ade81-118">Ukazuje, jak začlenit Python do aplikace tak, že přidáte kód Python k uloženým procedurám.</span><span class="sxs-lookup"><span data-stu-id="ade81-118">It demonstrates how to incorporate Python into an application by adding Python code to stored procedures.</span></span>


## <a name="next-steps"></a><span data-ttu-id="ade81-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ade81-119">Next steps</span></span>

<span data-ttu-id="ade81-120">Informace součástí klíče, které tvoří proces Team dat. vědecké účely, naleznete v [přehled tým datové vědy procesu](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ade81-120">For a discussion of the key components that comprise the Team Data Science Process, see [Team Data Science Process overview](data-science-process-overview.md).</span></span>

<span data-ttu-id="ade81-121">Informace životního cyklu Team datové vědy procesu, můžete použít pro struktury projekty vědecké účely dat, naleznete v [procesu vědecké účely Team datového cyklu](data-science-process-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="ade81-121">For a discussion of the Team Data Science Process lifecycle that you can use to structure your data science projects, see [Team Data Science Process lifecycle](data-science-process-lifecycle.md).</span></span> <span data-ttu-id="ade81-122">Životní cyklus popisuje kroky, od začátku do konce, projekty obvykle postupujte při jejich spuštění.</span><span class="sxs-lookup"><span data-stu-id="ade81-122">The lifecycle outlines the steps, from start to finish, that projects usually follow when they are executed.</span></span> 