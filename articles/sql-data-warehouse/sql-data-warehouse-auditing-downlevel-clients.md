---
title: "Podpora klientů nižší úrovně aaaSQL datového skladu dat auditování | Microsoft Docs"
description: "Další informace o podpoře klientů nižší úrovně SQL Data Warehouse pro auditování dat"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: dfe29ff3-dfeb-4309-83c0-c1a300f4f44e
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 377488680eb297c3e9b1dc754c003c5b19b47996
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sql-data-warehouse----downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a><span data-ttu-id="35917-103">SQL Data Warehouse – podpora klientů nižší úrovně pro auditování a dynamické maskování dat</span><span class="sxs-lookup"><span data-stu-id="35917-103">SQL Data Warehouse -  Downlevel clients support for auditing and Dynamic Data Masking</span></span>
<span data-ttu-id="35917-104">[Auditování](sql-data-warehouse-auditing-overview.md) funguje s klienty SQL, které podporují přesměrování TDS.</span><span class="sxs-lookup"><span data-stu-id="35917-104">[Auditing](sql-data-warehouse-auditing-overview.md) works with SQL clients that support TDS redirection.</span></span>

<span data-ttu-id="35917-105">Jakýkoli klient, který implementuje TDS 7.4 také podporuje přesměrování.</span><span class="sxs-lookup"><span data-stu-id="35917-105">Any client which implements TDS 7.4 should also support redirection.</span></span> <span data-ttu-id="35917-106">Výjimky toothis zahrnuty JDBC 4.0 kterou hello funkci přesměrování není plně podporována a Tedious pro Node.JS, ve kterém nebyla implementována přesměrování.</span><span class="sxs-lookup"><span data-stu-id="35917-106">Exceptions toothis include JDBC 4.0 in which hello redirection feature is not fully supported and Tedious for Node.JS in which redirection was not implemented.</span></span>

<span data-ttu-id="35917-107">Pro "Klienty nižší úrovně", tj. který podporují TDS verze 7.3 a níže - hello plně kvalifikovaný název domény serveru v připojovacím řetězci hello by měl být upraven:</span><span class="sxs-lookup"><span data-stu-id="35917-107">For "Downlevel clients", i.e. which support TDS version 7.3 and below - hello server FQDN in hello connection string should be modified:</span></span>

<span data-ttu-id="35917-108">Původní server plně kvalifikovaný název domény v připojovacím řetězci hello: <*název serveru*>. database.windows.net</span><span class="sxs-lookup"><span data-stu-id="35917-108">Original server FQDN in hello connection string: <*server name*>.database.windows.net</span></span>

<span data-ttu-id="35917-109">Upravené plně kvalifikovaný název domény serveru v připojovacím řetězci hello: <*název serveru*> .database. **zabezpečené**. windows.net</span><span class="sxs-lookup"><span data-stu-id="35917-109">Modified server FQDN in hello connection string: <*server name*>.database.**secure**.windows.net</span></span>

<span data-ttu-id="35917-110">Částečný seznam "Klienty nižší úrovně" zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="35917-110">A partial list of "Downlevel clients" includes:</span></span>

* <span data-ttu-id="35917-111">Rozhraní .NET 4.0 a níže,</span><span class="sxs-lookup"><span data-stu-id="35917-111">.NET 4.0 and below,</span></span>
* <span data-ttu-id="35917-112">ODBC 10.0 a níže.</span><span class="sxs-lookup"><span data-stu-id="35917-112">ODBC 10.0 and below.</span></span>
* <span data-ttu-id="35917-113">JDBC (při JDBC podporuje TDS 7.4, hello TDS přesměrování funkce není plně podporována)</span><span class="sxs-lookup"><span data-stu-id="35917-113">JDBC (while JDBC does support TDS 7.4, hello TDS redirection feature is not fully supported)</span></span>
* <span data-ttu-id="35917-114">Zdlouhavé (pro platformu Node.JS)</span><span class="sxs-lookup"><span data-stu-id="35917-114">Tedious (for Node.JS)</span></span>

<span data-ttu-id="35917-115">**Poznámka:** hello výše serveru úpravy plně kvalifikovaného názvu domény může být užitečné také pro použití zásad auditování na úrovni SQL serveru bez potřebu konfiguraci krok v každé databázi (dočasné zmírnění).</span><span class="sxs-lookup"><span data-stu-id="35917-115">**Remark:** hello above server FDQN modification may be useful also for applying a SQL Server Level Auditing policy without a need for a configuration step in each database (Temporary mitigation).</span></span>     

