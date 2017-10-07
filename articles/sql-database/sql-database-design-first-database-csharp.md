---
title: "aaaDesign první databázi SQL Azure - C# | Microsoft Docs"
description: "Další informace toodesign svoji první databázi Azure SQL a připojit tooit s programu v C# pomocí ADO.NET."
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg-msft
editor: CarlRabeler
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: develop databases
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 07/31/2017
ms.author: genemi;carlrab
ms.openlocfilehash: 8161de24bff1ec2fa307efa93adab2bd1b761fd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a><span data-ttu-id="43278-103">Návrh Azure SQL database a připojení s C & #x23; a ADO.NET</span><span class="sxs-lookup"><span data-stu-id="43278-103">Design an Azure SQL database and connect with C&#x23; and ADO.NET</span></span>

<span data-ttu-id="43278-104">Databáze SQL Azure je relační databáze jako a služba (DBaaS) v hello cloudu Microsoftu ("Azure").</span><span class="sxs-lookup"><span data-stu-id="43278-104">Azure SQL Database is a relational database-as-a service (DBaaS) in hello Microsoft Cloud ("Azure").</span></span> <span data-ttu-id="43278-105">V tomto kurzu zjistíte, jak toouse hello portál Azure a ADO.NET pomocí sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="43278-105">In this tutorial, you learn how toouse hello Azure portal and ADO.NET with Visual Studio to:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="43278-106">Vytvoření databáze v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="43278-106">Create a database in hello Azure portal</span></span>
> * <span data-ttu-id="43278-107">Nastavit pravidlo brány firewall na úrovni serveru v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="43278-107">Set up a server-level firewall rule in hello Azure portal</span></span>
> * <span data-ttu-id="43278-108">Připojit databáze toohello s ADO.NET a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43278-108">Connect toohello database with ADO.NET and Visual Studio</span></span>
> * <span data-ttu-id="43278-109">Vytváření tabulek s ADO.NET</span><span class="sxs-lookup"><span data-stu-id="43278-109">Create tables with ADO.NET</span></span>
> * <span data-ttu-id="43278-110">Vložit, aktualizovat a odstranit data s ADO.NET</span><span class="sxs-lookup"><span data-stu-id="43278-110">Insert, update, and delete data with ADO.NET</span></span> 
> * <span data-ttu-id="43278-111">Dotaz na data ADO.NET</span><span class="sxs-lookup"><span data-stu-id="43278-111">Query data ADO.NET</span></span>

<span data-ttu-id="43278-112">Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.</span><span class="sxs-lookup"><span data-stu-id="43278-112">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43278-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="43278-113">Prerequisites</span></span>

<span data-ttu-id="43278-114">Instalaci sady [Visual Studio Community 2017, Visual Studio Professional 2017 nebo Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="43278-114">An installation of [Visual Studio Community 2017, Visual Studio Professional 2017, or Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).</span></span>

<!-- hello following included .md, sql-database-tutorial-portal-create-firewall-connection-1.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-tutorial-portal-create-firewall-connection-1](../../includes/sql-database-tutorial-portal-create-firewall-connection-1.md)]


<!-- hello following included .md, sql-database-csharp-adonet-create-query-2.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]


## <a name="next-steps"></a><span data-ttu-id="43278-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="43278-115">Next steps</span></span>

<span data-ttu-id="43278-116">V tomto kurzu jste se dozvěděli, že databáze basic úlohy, jako například vytvořit databáze a tabulky, načítat a zadávat dotazy na data a obnovit hello databáze tooa předchozího bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="43278-116">In this tutorial, you learned basic database tasks such as create a database and tables, load and query data, and restore hello database tooa previous point in time.</span></span> <span data-ttu-id="43278-117">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="43278-117">You learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="43278-118">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="43278-118">Create a database</span></span>
> * <span data-ttu-id="43278-119">Nastavit pravidlo brány firewall</span><span class="sxs-lookup"><span data-stu-id="43278-119">Set up a firewall rule</span></span>
> * <span data-ttu-id="43278-120">Připojit databáze toohello s [Visual Studio a C#](sql-database-connect-query-dotnet-visual-studio.md)</span><span class="sxs-lookup"><span data-stu-id="43278-120">Connect toohello database with [Visual Studio and C#](sql-database-connect-query-dotnet-visual-studio.md)</span></span>
> * <span data-ttu-id="43278-121">Vytváření tabulek</span><span class="sxs-lookup"><span data-stu-id="43278-121">Create tables</span></span>
> * <span data-ttu-id="43278-122">Vložit, aktualizovat a odstranit data</span><span class="sxs-lookup"><span data-stu-id="43278-122">Insert, update, and delete data</span></span>
> * <span data-ttu-id="43278-123">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="43278-123">Query data</span></span>

<span data-ttu-id="43278-124">Další kurz toolearn toohello o migraci dat zálohy.</span><span class="sxs-lookup"><span data-stu-id="43278-124">Advance toohello next tutorial toolearn about migrating your data.</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="43278-125">Migrace vaší tooAzure databáze systému SQL Server databáze SQL</span><span class="sxs-lookup"><span data-stu-id="43278-125">Migrate your SQL Server database tooAzure SQL Database</span></span>](sql-database-migrate-your-sql-server-database.md)

