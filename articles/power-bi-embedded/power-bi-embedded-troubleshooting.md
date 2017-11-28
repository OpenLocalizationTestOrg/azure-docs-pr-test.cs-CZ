---
title: "Řešení potíží s Microsoft Power BI Embedded Preview"
description: "Řešení potíží s Microsoft Power BI Embedded Preview"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: c8aee652-ed8b-4c66-9c63-f798b7a655b4
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/06/2017
ms.author: asaxton
ms.openlocfilehash: f406d23e578acc825514aa5bd9eabcbf160bf9ec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a><span data-ttu-id="3d3f2-103">Řešení potíží s Microsoft Power BI Embedded Preview</span><span class="sxs-lookup"><span data-stu-id="3d3f2-103">Microsoft Power BI Embedded Preview troubleshooting</span></span>
<span data-ttu-id="3d3f2-104">Tento článek obsahuje odpovědi pro řešení potíží s **Power BI Embedded**.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-104">This article provides answers for how  to troubleshoot **Power BI Embedded**.</span></span>

<a name="connection-string"/>

## <a name="setting-sql-server-connection-strings"></a><span data-ttu-id="3d3f2-105">Nastavení systému SQL Server připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="3d3f2-105">Setting SQL Server connection strings</span></span>
<span data-ttu-id="3d3f2-106">Pokud chcete nastavit připojení řetězec systému SQL Server, budete muset postupovat podle konkrétním formátu.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-106">To set a SQL Server connecting string, you need to follow a specific format.</span></span> <span data-ttu-id="3d3f2-107">Níže je příklad připojovací řetězce pro SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-107">Below is an example connection string for SQL Server.</span></span>

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

<span data-ttu-id="3d3f2-108">Další informace o řetězce připojení SQL Server, naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="3d3f2-108">To learn more about SQL Server connection strings, see the following articles:</span></span>

* [<span data-ttu-id="3d3f2-109">Připojovací řetězce SQL serveru</span><span class="sxs-lookup"><span data-stu-id="3d3f2-109">SQL Server Connection Strings</span></span>](https://msdn.microsoft.com/library/jj653752.aspx)
* [<span data-ttu-id="3d3f2-110">SqlConnection.ConnectionString</span><span class="sxs-lookup"><span data-stu-id="3d3f2-110">SqlConnection.ConnectionString</span></span>](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>

## <a name="setting-credentials"></a><span data-ttu-id="3d3f2-111">Nastavení přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="3d3f2-111">Setting credentials</span></span>
<span data-ttu-id="3d3f2-112">V případě, kdy máte přihlašovací údaje pro vývoj nebo pracovní prostředí, například uživatelské jméno a heslo možná budete muset aktualizovat přihlašovací údaje, které odpovídají produkční řešení.</span><span class="sxs-lookup"><span data-stu-id="3d3f2-112">In the case where you have credentials for a development or staging environment, such as user name and password, you might need to update credentials that match a production solution.</span></span>

## <a name="see-also"></a><span data-ttu-id="3d3f2-113">Viz také</span><span class="sxs-lookup"><span data-stu-id="3d3f2-113">See Also</span></span>
* [<span data-ttu-id="3d3f2-114">Začínáme s ukázkou</span><span class="sxs-lookup"><span data-stu-id="3d3f2-114">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)
* [<span data-ttu-id="3d3f2-115">Co je Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="3d3f2-115">What is Power BI Embedded</span></span>](power-bi-embedded-what-is-power-bi-embedded.md)

