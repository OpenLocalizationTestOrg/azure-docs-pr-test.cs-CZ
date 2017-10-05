---
title: "Přehled vývoje aplikace databáze pro databázi Azure pro databázi MySQL | Microsoft Docs"
description: "Zavádí aspekty návrhu, které vývojář při psaní kódu pro aplikace pro připojení k databázi Azure pro databázi MySQL"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 350dd775e172120d806d1193877a34d94f4d3f6a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a><span data-ttu-id="7e3db-103">Přehled vývoje aplikací pro databázi Azure pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="7e3db-103">Application development overview for Azure Database for MySQL</span></span> 
<span data-ttu-id="7e3db-104">Tento článek popisuje aspekty návrhu, které vývojář při psaní kódu pro aplikace pro připojení k databázi Azure pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="7e3db-104">This article discusses design considerations that a developer should follow when writing application code to connect to Azure Database for MySQL</span></span> 

> [!TIP]
> <span data-ttu-id="7e3db-105">Kurz ukazuje, jak vytvořit server, vytvoření brány firewall na serveru, zobrazení vlastností serveru, vytvořit databázi, připojení a dotazování pomocí workbench a mysql.exe, najdete v části [navrhnout první databáze MySQL na Azure](tutorial-design-database-using-portal.md)</span><span class="sxs-lookup"><span data-stu-id="7e3db-105">For a tutorial showing you how to create a server, create a server-based firewall, view server properties, create database, connect and query using workbench and mysql.exe, see [Design your first Azure MySQL database](tutorial-design-database-using-portal.md)</span></span>

## <a name="language-and-platform"></a><span data-ttu-id="7e3db-106">Jazyk a platforma</span><span class="sxs-lookup"><span data-stu-id="7e3db-106">Language and platform</span></span>
<span data-ttu-id="7e3db-107">K dispozici jsou ukázky kódu pro různé programovací jazyky a platformy.</span><span class="sxs-lookup"><span data-stu-id="7e3db-107">There are code samples available for various programming languages and platforms.</span></span> <span data-ttu-id="7e3db-108">Můžete najít odkazy na ukázky kódu v: [připojení knihovny používané pro připojení k databázi Azure pro databázi MySQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="7e3db-108">You can find links to the code samples at: [Connectivity libraries used to connect to Azure Database for MySQL](concepts-connection-libraries.md)</span></span>

## <a name="tools"></a><span data-ttu-id="7e3db-109">Nástroje</span><span class="sxs-lookup"><span data-stu-id="7e3db-109">Tools</span></span>
<span data-ttu-id="7e3db-110">MySQL community verze, kompatibilní s MySQL běžné nástroje pro správu jako je například nástroje Workbench nebo MySQL, jako je například mysql.exe, používá Azure databáze pro databázi MySQL [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql)a další.</span><span class="sxs-lookup"><span data-stu-id="7e3db-110">Azure Database for MySQL uses the MySQL community version, compatible with MySQL common management tools such as Workbench or MySQL utilities such as mysql.exe, [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql), and others.</span></span> <span data-ttu-id="7e3db-111">Také můžete portál Azure, rozhraní příkazového řádku Azure a rozhraní REST API pro interakci s službu databáze.</span><span class="sxs-lookup"><span data-stu-id="7e3db-111">You can also use the Azure portal, Azure CLI, and REST APIs to interact with the database service.</span></span>

## <a name="resource-limitations"></a><span data-ttu-id="7e3db-112">Omezení prostředků</span><span class="sxs-lookup"><span data-stu-id="7e3db-112">Resource limitations</span></span>
<span data-ttu-id="7e3db-113">Databáze MySQL Azure spravuje prostředky k dispozici pro server pomocí dvou různých mechanismů:</span><span class="sxs-lookup"><span data-stu-id="7e3db-113">Azure MySQL Database manages the resources available to a server using two different mechanisms:</span></span> 
- <span data-ttu-id="7e3db-114">Zásady správného řízení prostředků</span><span class="sxs-lookup"><span data-stu-id="7e3db-114">Resources Governance</span></span> 
- <span data-ttu-id="7e3db-115">Vynucení omezení.</span><span class="sxs-lookup"><span data-stu-id="7e3db-115">Enforcement of Limits.</span></span>

## <a name="security"></a><span data-ttu-id="7e3db-116">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="7e3db-116">Security</span></span>
<span data-ttu-id="7e3db-117">Databáze MySQL Azure poskytuje prostředky pro omezení přístupu, ochranu dat, konfigurace uživatele a role a monitorování aktivit v databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="7e3db-117">Azure MySQL Database provides resources for limiting access, protecting data, configuring users and role, and monitoring activities on a MySQL Database.</span></span>

## <a name="authentication"></a><span data-ttu-id="7e3db-118">Authentication</span><span class="sxs-lookup"><span data-stu-id="7e3db-118">Authentication</span></span>
<span data-ttu-id="7e3db-119">Databáze MySQL Azure podporuje serveru ověřování uživatelů a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="7e3db-119">Azure MySQL Database supports server authentication of users and logins.</span></span>

## <a name="resiliency"></a><span data-ttu-id="7e3db-120">Odolnost</span><span class="sxs-lookup"><span data-stu-id="7e3db-120">Resiliency</span></span>
<span data-ttu-id="7e3db-121">Dojde-li k přechodné chybě při připojování k databázi MySQL, by měl váš kód opakujte volání.</span><span class="sxs-lookup"><span data-stu-id="7e3db-121">When a transient error occurs while connecting to MySQL Database, your code should retry the call.</span></span> <span data-ttu-id="7e3db-122">Doporučujeme použít logiku opakování regrese logiku, tak, aby ji není zahlcovat databázi SQL s více klienty najednou opakování.</span><span class="sxs-lookup"><span data-stu-id="7e3db-122">We recommend the retry logic use back off logic, so that it does not overwhelm the SQL Database with multiple clients retrying simultaneously.</span></span>

- <span data-ttu-id="7e3db-123">Ukázky kódu jsou: ukázky kódu, které ilustrují logika opakovaných pokusů, najdete v části Ukázky pro jazyk podle vašeho výběru v: [připojení knihovny používané pro připojení k databázi Azure pro databázi MySQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="7e3db-123">Code samples: For code samples that illustrate retry logic, see samples for the language of your choice at: [Connectivity libraries used to connect to Azure Database for MySQL](concepts-connection-libraries.md)</span></span>

## <a name="managing-connections"></a><span data-ttu-id="7e3db-124">Správa připojení</span><span class="sxs-lookup"><span data-stu-id="7e3db-124">Managing Connections</span></span>
<span data-ttu-id="7e3db-125">Databázová připojení jsou omezené prostředku, proto doporučujeme rozumný použití připojení při přístupu k databázi MySQL můžete dosáhnout lepšího výkonu.</span><span class="sxs-lookup"><span data-stu-id="7e3db-125">Database connections are a limited resource, so we recommend sensible use of connections when accessing your MySQL Database to achieve better performance.</span></span>
- <span data-ttu-id="7e3db-126">Přístup k databázi pomocí sdružování připojení nebo trvalé připojení.</span><span class="sxs-lookup"><span data-stu-id="7e3db-126">Access the database by using connection pooling or persistent connections.</span></span>
- <span data-ttu-id="7e3db-127">Přístup k databázi pomocí připojení krátkou životnost.</span><span class="sxs-lookup"><span data-stu-id="7e3db-127">Access the database by using short connection life span.</span></span> 
- <span data-ttu-id="7e3db-128">Použít logika opakovaných pokusů v aplikaci v místě pokus o připojení, k zachycení chyb z důvodu souběžných připojení bylo dosaženo maximální povolený počet.</span><span class="sxs-lookup"><span data-stu-id="7e3db-128">Use retry logic in your application at the point of the connection attempt, to catch failures due to concurrent connections have reached the maximum allowed.</span></span> <span data-ttu-id="7e3db-129">V logice opakování nastavit chvíli trvat a potom počkejte, než pro náhodné dobu před pokusy o další připojení.</span><span class="sxs-lookup"><span data-stu-id="7e3db-129">In the retry logic, set a short delay, and then wait for a random time before the additional connection attempts.</span></span>