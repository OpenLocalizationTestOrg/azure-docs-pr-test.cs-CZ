---
title: "Přehled vývoje aplikace aaaDatabase pro databázi Azure pro databázi MySQL | Microsoft Docs"
description: "Zavádí aspekty návrhu, které vývojář při psaní aplikace kódu tooconnect tooAzure databáze pro databázi MySQL"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: f08df605eba21b4ba4b43565c0a7ded95779a171
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a><span data-ttu-id="46705-103">Přehled vývoje aplikací pro databázi Azure pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="46705-103">Application development overview for Azure Database for MySQL</span></span> 
<span data-ttu-id="46705-104">Tento článek popisuje aspekty návrhu, které vývojář při psaní aplikace kódu tooconnect tooAzure databáze pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="46705-104">This article discusses design considerations that a developer should follow when writing application code tooconnect tooAzure Database for MySQL</span></span> 

> [!TIP]
> <span data-ttu-id="46705-105">Kurz znázorňující, jak vytvořit toocreate serveru, brána firewall na serveru, zobrazení vlastností serveru, vytvoříte databázi, připojení a dotazování pomocí workbench a mysql.exe, najdete v části [navrhnout první databáze MySQL na Azure](tutorial-design-database-using-portal.md)</span><span class="sxs-lookup"><span data-stu-id="46705-105">For a tutorial showing you how toocreate a server, create a server-based firewall, view server properties, create database, connect and query using workbench and mysql.exe, see [Design your first Azure MySQL database](tutorial-design-database-using-portal.md)</span></span>

## <a name="language-and-platform"></a><span data-ttu-id="46705-106">Jazyk a platforma</span><span class="sxs-lookup"><span data-stu-id="46705-106">Language and platform</span></span>
<span data-ttu-id="46705-107">K dispozici jsou ukázky kódu pro různé programovací jazyky a platformy.</span><span class="sxs-lookup"><span data-stu-id="46705-107">There are code samples available for various programming languages and platforms.</span></span> <span data-ttu-id="46705-108">Odkazy můžete najít ukázky kódu toohello v: [používat knihovny připojení tooconnect tooAzure databáze pro databázi MySQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="46705-108">You can find links toohello code samples at: [Connectivity libraries used tooconnect tooAzure Database for MySQL](concepts-connection-libraries.md)</span></span>

## <a name="tools"></a><span data-ttu-id="46705-109">Nástroje</span><span class="sxs-lookup"><span data-stu-id="46705-109">Tools</span></span>
<span data-ttu-id="46705-110">MySQL community hello, verze kompatibilní s MySQL běžné nástroje pro správu jako je například nástroje Workbench nebo MySQL, jako je například mysql.exe, používá Azure databáze pro databázi MySQL [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql)a další.</span><span class="sxs-lookup"><span data-stu-id="46705-110">Azure Database for MySQL uses hello MySQL community version, compatible with MySQL common management tools such as Workbench or MySQL utilities such as mysql.exe, [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql), and others.</span></span> <span data-ttu-id="46705-111">Můžete taky hello portálu Azure, rozhraní příkazového řádku Azure a rozhraní REST API toointeract službou hello databáze.</span><span class="sxs-lookup"><span data-stu-id="46705-111">You can also use hello Azure portal, Azure CLI, and REST APIs toointeract with hello database service.</span></span>

## <a name="resource-limitations"></a><span data-ttu-id="46705-112">Omezení prostředků</span><span class="sxs-lookup"><span data-stu-id="46705-112">Resource limitations</span></span>
<span data-ttu-id="46705-113">Databáze MySQL Azure spravuje hello prostředky k dispozici tooa serveru pomocí dvou různých mechanismů:</span><span class="sxs-lookup"><span data-stu-id="46705-113">Azure MySQL Database manages hello resources available tooa server using two different mechanisms:</span></span> 
- <span data-ttu-id="46705-114">Zásady správného řízení prostředků</span><span class="sxs-lookup"><span data-stu-id="46705-114">Resources Governance</span></span> 
- <span data-ttu-id="46705-115">Vynucení omezení.</span><span class="sxs-lookup"><span data-stu-id="46705-115">Enforcement of Limits.</span></span>

## <a name="security"></a><span data-ttu-id="46705-116">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="46705-116">Security</span></span>
<span data-ttu-id="46705-117">Databáze MySQL Azure poskytuje prostředky pro omezení přístupu, ochranu dat, konfigurace uživatele a role a monitorování aktivit v databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="46705-117">Azure MySQL Database provides resources for limiting access, protecting data, configuring users and role, and monitoring activities on a MySQL Database.</span></span>

## <a name="authentication"></a><span data-ttu-id="46705-118">Authentication</span><span class="sxs-lookup"><span data-stu-id="46705-118">Authentication</span></span>
<span data-ttu-id="46705-119">Databáze MySQL Azure podporuje serveru ověřování uživatelů a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="46705-119">Azure MySQL Database supports server authentication of users and logins.</span></span>

## <a name="resiliency"></a><span data-ttu-id="46705-120">Odolnost</span><span class="sxs-lookup"><span data-stu-id="46705-120">Resiliency</span></span>
<span data-ttu-id="46705-121">V případě přechodná chyba při připojování tooMySQL databáze by měl váš kód opakujte volání hello.</span><span class="sxs-lookup"><span data-stu-id="46705-121">When a transient error occurs while connecting tooMySQL Database, your code should retry hello call.</span></span> <span data-ttu-id="46705-122">Doporučujeme použít logiku opakování hello stáhnul logiku, tak, aby ji není zahlcovat hello databázi SQL s více klienty najednou opakování.</span><span class="sxs-lookup"><span data-stu-id="46705-122">We recommend hello retry logic use back off logic, so that it does not overwhelm hello SQL Database with multiple clients retrying simultaneously.</span></span>

- <span data-ttu-id="46705-123">Ukázky kódu jsou: ukázky kódu, které ilustrují logika opakovaných pokusů, najdete v části Ukázky pro jazyk hello zvoleného v: [používat knihovny připojení tooconnect tooAzure databáze pro databázi MySQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="46705-123">Code samples: For code samples that illustrate retry logic, see samples for hello language of your choice at: [Connectivity libraries used tooconnect tooAzure Database for MySQL](concepts-connection-libraries.md)</span></span>

## <a name="managing-connections"></a><span data-ttu-id="46705-124">Správa připojení</span><span class="sxs-lookup"><span data-stu-id="46705-124">Managing Connections</span></span>
<span data-ttu-id="46705-125">Databázová připojení jsou omezené prostředku, proto doporučujeme rozumný použití připojení při přístupu k databázi MySQL tooachieve lepší výkon.</span><span class="sxs-lookup"><span data-stu-id="46705-125">Database connections are a limited resource, so we recommend sensible use of connections when accessing your MySQL Database tooachieve better performance.</span></span>
- <span data-ttu-id="46705-126">Databáze Access hello pomocí sdružování připojení nebo trvalé připojení.</span><span class="sxs-lookup"><span data-stu-id="46705-126">Access hello database by using connection pooling or persistent connections.</span></span>
- <span data-ttu-id="46705-127">Databáze Access hello pomocí připojení krátkou životnost.</span><span class="sxs-lookup"><span data-stu-id="46705-127">Access hello database by using short connection life span.</span></span> 
- <span data-ttu-id="46705-128">Logika opakovaných pokusů pro použití ve vaší aplikaci v okamžiku hello hello pokusu o připojení, toocatch selhání kvůli tooconcurrent připojení bylo dosaženo maximální povolený počet hello.</span><span class="sxs-lookup"><span data-stu-id="46705-128">Use retry logic in your application at hello point of hello connection attempt, toocatch failures due tooconcurrent connections have reached hello maximum allowed.</span></span> <span data-ttu-id="46705-129">V hello logika opakovaných pokusů, nastavte chvíli trvat a potom počkejte, než pro náhodné dobu před pokusy o hello další připojení.</span><span class="sxs-lookup"><span data-stu-id="46705-129">In hello retry logic, set a short delay, and then wait for a random time before hello additional connection attempts.</span></span>
