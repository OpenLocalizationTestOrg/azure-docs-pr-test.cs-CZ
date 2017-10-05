---
title: "Navrhnout první databázi SQL Azure - C# | Microsoft Docs"
description: "Naučte se navrhnout první databáze Azure SQL a k nim připojit pomocí programu v C# pomocí ADO.NET."
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
ms.openlocfilehash: d9731cf5399cce6f103129ccda521f2867bd8da6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a><span data-ttu-id="0ec57-103">Návrh Azure SQL database a připojení s C & #x23; a ADO.NET</span><span class="sxs-lookup"><span data-stu-id="0ec57-103">Design an Azure SQL database and connect with C&#x23; and ADO.NET</span></span>

<span data-ttu-id="0ec57-104">Databáze SQL Azure je relační databáze jako a služba (DBaaS) v cloudu Microsoftu ("Azure").</span><span class="sxs-lookup"><span data-stu-id="0ec57-104">Azure SQL Database is a relational database-as-a service (DBaaS) in the Microsoft Cloud ("Azure").</span></span> <span data-ttu-id="0ec57-105">V tomto kurzu můžete další informace o použití portálu Azure a ADO.NET pomocí sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="0ec57-105">In this tutorial, you learn how to use the Azure portal and ADO.NET with Visual Studio to:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="0ec57-106">Vytvoření databáze na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0ec57-106">Create a database in the Azure portal</span></span>
> * <span data-ttu-id="0ec57-107">Nastavit pravidlo brány firewall na úrovni serveru, na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0ec57-107">Set up a server-level firewall rule in the Azure portal</span></span>
> * <span data-ttu-id="0ec57-108">Připojení k databázi s ADO.NET a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ec57-108">Connect to the database with ADO.NET and Visual Studio</span></span>
> * <span data-ttu-id="0ec57-109">Vytváření tabulek s ADO.NET</span><span class="sxs-lookup"><span data-stu-id="0ec57-109">Create tables with ADO.NET</span></span>
> * <span data-ttu-id="0ec57-110">Vložit, aktualizovat a odstranit data s ADO.NET</span><span class="sxs-lookup"><span data-stu-id="0ec57-110">Insert, update, and delete data with ADO.NET</span></span> 
> * <span data-ttu-id="0ec57-111">Dotaz na data ADO.NET</span><span class="sxs-lookup"><span data-stu-id="0ec57-111">Query data ADO.NET</span></span>

<span data-ttu-id="0ec57-112">Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.</span><span class="sxs-lookup"><span data-stu-id="0ec57-112">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0ec57-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0ec57-113">Prerequisites</span></span>

<span data-ttu-id="0ec57-114">Instalaci sady [Visual Studio Community 2017, Visual Studio Professional 2017 nebo Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="0ec57-114">An installation of [Visual Studio Community 2017, Visual Studio Professional 2017, or Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).</span></span>

<!-- The following included .md, sql-database-tutorial-portal-create-firewall-connection-1.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-tutorial-portal-create-firewall-connection-1](../../includes/sql-database-tutorial-portal-create-firewall-connection-1.md)]


<!-- The following included .md, sql-database-csharp-adonet-create-query-2.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]


## <a name="next-steps"></a><span data-ttu-id="0ec57-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0ec57-115">Next steps</span></span>

<span data-ttu-id="0ec57-116">V tomto kurzu jste se dozvěděli, že databáze basic úlohy, jako například vytvořit databáze a tabulky, načítat a zadávat dotazy na data a obnovit databázi do předchozího bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="0ec57-116">In this tutorial, you learned basic database tasks such as create a database and tables, load and query data, and restore the database to a previous point in time.</span></span> <span data-ttu-id="0ec57-117">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="0ec57-117">You learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="0ec57-118">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="0ec57-118">Create a database</span></span>
> * <span data-ttu-id="0ec57-119">Nastavit pravidlo brány firewall</span><span class="sxs-lookup"><span data-stu-id="0ec57-119">Set up a firewall rule</span></span>
> * <span data-ttu-id="0ec57-120">Připojení k databázi s [Visual Studio a C#](sql-database-connect-query-dotnet-visual-studio.md)</span><span class="sxs-lookup"><span data-stu-id="0ec57-120">Connect to the database with [Visual Studio and C#](sql-database-connect-query-dotnet-visual-studio.md)</span></span>
> * <span data-ttu-id="0ec57-121">Vytváření tabulek</span><span class="sxs-lookup"><span data-stu-id="0ec57-121">Create tables</span></span>
> * <span data-ttu-id="0ec57-122">Vložit, aktualizovat a odstranit data</span><span class="sxs-lookup"><span data-stu-id="0ec57-122">Insert, update, and delete data</span></span>
> * <span data-ttu-id="0ec57-123">Dotazování dat</span><span class="sxs-lookup"><span data-stu-id="0ec57-123">Query data</span></span>

<span data-ttu-id="0ec57-124">Přechodu na další informace o migraci dat v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="0ec57-124">Advance to the next tutorial to learn about migrating your data.</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="0ec57-125">Migrovat databázi SQL serveru do Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="0ec57-125">Migrate your SQL Server database to Azure SQL Database</span></span>](sql-database-migrate-your-sql-server-database.md)

