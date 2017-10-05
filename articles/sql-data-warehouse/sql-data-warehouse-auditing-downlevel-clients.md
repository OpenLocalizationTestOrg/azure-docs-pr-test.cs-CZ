---
title: "Podpora klientů nižší úrovně SQL Data Warehouse data auditování | Microsoft Docs"
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
ms.openlocfilehash: a7ea6141285a0098339f1e071af2592dd4535c12
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sql-data-warehouse----downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a><span data-ttu-id="df063-103">SQL Data Warehouse – podpora klientů nižší úrovně pro auditování a dynamické maskování dat</span><span class="sxs-lookup"><span data-stu-id="df063-103">SQL Data Warehouse -  Downlevel clients support for auditing and Dynamic Data Masking</span></span>
<span data-ttu-id="df063-104">[Auditování](sql-data-warehouse-auditing-overview.md) funguje s klienty SQL, které podporují přesměrování TDS.</span><span class="sxs-lookup"><span data-stu-id="df063-104">[Auditing](sql-data-warehouse-auditing-overview.md) works with SQL clients that support TDS redirection.</span></span>

<span data-ttu-id="df063-105">Jakýkoli klient, který implementuje TDS 7.4 také podporuje přesměrování.</span><span class="sxs-lookup"><span data-stu-id="df063-105">Any client which implements TDS 7.4 should also support redirection.</span></span> <span data-ttu-id="df063-106">Výjimky zahrnují JDBC 4.0, ve kterém funkci přesměrování není plně podporována a Tedious pro Node.JS, ve které přesměrování nebyla implementována.</span><span class="sxs-lookup"><span data-stu-id="df063-106">Exceptions to this include JDBC 4.0 in which the redirection feature is not fully supported and Tedious for Node.JS in which redirection was not implemented.</span></span>

<span data-ttu-id="df063-107">Pro "Klienty nižší úrovně" tj. by měl být které podporu TDS 7.3 a pod - server plně kvalifikovaný název domény pro připojení řetězec verze upraven:</span><span class="sxs-lookup"><span data-stu-id="df063-107">For "Downlevel clients", i.e. which support TDS version 7.3 and below - the server FQDN in the connection string should be modified:</span></span>

<span data-ttu-id="df063-108">Původní server plně kvalifikovaný název domény v připojovacím řetězci: <*název serveru*>. database.windows.net</span><span class="sxs-lookup"><span data-stu-id="df063-108">Original server FQDN in the connection string: <*server name*>.database.windows.net</span></span>

<span data-ttu-id="df063-109">Upravené plně kvalifikovaný název domény serveru v připojovacím řetězci: <*název serveru*> .database. **zabezpečené**. windows.net</span><span class="sxs-lookup"><span data-stu-id="df063-109">Modified server FQDN in the connection string: <*server name*>.database.**secure**.windows.net</span></span>

<span data-ttu-id="df063-110">Částečný seznam "Klienty nižší úrovně" zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="df063-110">A partial list of "Downlevel clients" includes:</span></span>

* <span data-ttu-id="df063-111">Rozhraní .NET 4.0 a níže,</span><span class="sxs-lookup"><span data-stu-id="df063-111">.NET 4.0 and below,</span></span>
* <span data-ttu-id="df063-112">ODBC 10.0 a níže.</span><span class="sxs-lookup"><span data-stu-id="df063-112">ODBC 10.0 and below.</span></span>
* <span data-ttu-id="df063-113">JDBC (při JDBC podporuje TDS 7.4, TDS přesměrování funkce není podporována plně)</span><span class="sxs-lookup"><span data-stu-id="df063-113">JDBC (while JDBC does support TDS 7.4, the TDS redirection feature is not fully supported)</span></span>
* <span data-ttu-id="df063-114">Zdlouhavé (pro platformu Node.JS)</span><span class="sxs-lookup"><span data-stu-id="df063-114">Tedious (for Node.JS)</span></span>

<span data-ttu-id="df063-115">**Poznámka:** výše uvedeném serveru úpravy plně kvalifikovaného názvu domény může být užitečné také pro použití zásad auditování na úrovni SQL serveru bez potřebu konfiguraci krok v každé databázi (dočasné zmírnění).</span><span class="sxs-lookup"><span data-stu-id="df063-115">**Remark:** The above server FDQN modification may be useful also for applying a SQL Server Level Auditing policy without a need for a configuration step in each database (Temporary mitigation).</span></span>     

