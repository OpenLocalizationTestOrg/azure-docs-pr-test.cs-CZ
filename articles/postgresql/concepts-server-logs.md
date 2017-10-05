---
title: "Server v protokolech v Azure databázi PostgreSQL | Microsoft Docs"
description: "Generuje protokoly dotazu a chyby v databázi Azure pro PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 10f6c490a4fdb4c70cb80b035eaebb9d2ad2e699
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="server-logs-in-azure-database-for-postgresql"></a><span data-ttu-id="05404-103">Server v protokolech v Azure databázi PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="05404-103">Server Logs in Azure Database for PostgreSQL</span></span> 
<span data-ttu-id="05404-104">Generuje Azure databázi PostgreSQL dotazu a chybové protokoly.</span><span class="sxs-lookup"><span data-stu-id="05404-104">Azure Database for PostgreSQL generates query and error logs.</span></span> <span data-ttu-id="05404-105">Přístup k protokoly transakcí však není podporován.</span><span class="sxs-lookup"><span data-stu-id="05404-105">However, access to transaction logs is not supported.</span></span> <span data-ttu-id="05404-106">Tyto protokoly můžete použít k identifikaci, řešení potíží a opravte chyby konfigurace a optimální výkon.</span><span class="sxs-lookup"><span data-stu-id="05404-106">These logs can be used to identify, troubleshoot, and repair configuration errors and sub-optimal performance.</span></span> <span data-ttu-id="05404-107">Další informace najdete v tématu [Chyba protokolování a generování sestav](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html).</span><span class="sxs-lookup"><span data-stu-id="05404-107">For more information, see [Error Reporting and Logging](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html).</span></span>

## <a name="access-server-logs"></a><span data-ttu-id="05404-108">Přístup k serveru protokolům</span><span class="sxs-lookup"><span data-stu-id="05404-108">Access server logs</span></span>
<span data-ttu-id="05404-109">Můžete zobrazit seznam a stáhnout protokoly chyb serveru Azure PostgreSQL pomocí portálu Azure [rozhraní příkazového řádku Azure](howto-configure-server-logs-using-cli.md) a rozhraní API REST Azure.</span><span class="sxs-lookup"><span data-stu-id="05404-109">You can list and download Azure PostgreSQL server error logs using the Azure Portal, [Azure CLI](howto-configure-server-logs-using-cli.md) and Azure REST APIs.</span></span>

## <a name="log-retention"></a><span data-ttu-id="05404-110">Uchování protokolu</span><span class="sxs-lookup"><span data-stu-id="05404-110">Log retention</span></span>
<span data-ttu-id="05404-111">Můžete nastavit dobu uchování pro protokolů systému pomocí **protokolu\_uchování\_období** parametr přidružené k serveru.</span><span class="sxs-lookup"><span data-stu-id="05404-111">You can set the retention period for system logs using the **log\_retention\_period** parameter associated with your server.</span></span> <span data-ttu-id="05404-112">Jednotka pro tento parametr je dní (třeba k potvrzení).</span><span class="sxs-lookup"><span data-stu-id="05404-112">The unit for this parameter is days (need to confirm).</span></span> <span data-ttu-id="05404-113">Výchozí hodnota je tři dny.</span><span class="sxs-lookup"><span data-stu-id="05404-113">The default value is three days.</span></span> <span data-ttu-id="05404-114">Maximální hodnota je 7 dní.</span><span class="sxs-lookup"><span data-stu-id="05404-114">The maximum value is 7 days.</span></span> <span data-ttu-id="05404-115">Všimněte si, že váš server musí mít dostatek přidělené úložiště tak, aby obsahovala zachované protokolové soubory.</span><span class="sxs-lookup"><span data-stu-id="05404-115">Note that your server must have enough allocated storage to contain the retained log files.</span></span>
<span data-ttu-id="05404-116">Soubory protokolu se otočit každých 1 hodinu nebo velikost 100MB, nastane dříve.</span><span class="sxs-lookup"><span data-stu-id="05404-116">The log files will rotate every 1 hour or 100MB size, whichever comes first.</span></span>

## <a name="configure-logging-for-azure-postgresql-server"></a><span data-ttu-id="05404-117">Konfigurace protokolování pro server Azure PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="05404-117">Configure logging for Azure PostgreSQL server</span></span>
<span data-ttu-id="05404-118">Pro váš server můžete povolit protokolování dotazu a protokolování chyb.</span><span class="sxs-lookup"><span data-stu-id="05404-118">You can enable query logging and error logging for your server.</span></span> <span data-ttu-id="05404-119">Protokoly chyb může obsahovat automaticky vakuu, připojení a informace o kontrolní body.</span><span class="sxs-lookup"><span data-stu-id="05404-119">Error logs can contain auto-vacuum, connection and checkpoints information.</span></span>

<span data-ttu-id="05404-120">Můžete povolit protokolování dotazu pro instanci databáze PostgreSQL tak dva parametry serveru: protokolu\_prohlášení a protokolu\_min\_doba trvání\_příkaz.</span><span class="sxs-lookup"><span data-stu-id="05404-120">You can enable query logging for your PostgreSQL DB instance by setting two server parameters: log\_statement and log\_min\_duration\_statement.</span></span>

<span data-ttu-id="05404-121">**Protokolu\_příkaz** parametr řídí, jaké příkazy SQL se zaznamenávají.</span><span class="sxs-lookup"><span data-stu-id="05404-121">The **log\_statement** parameter controls which SQL statements are logged.</span></span> <span data-ttu-id="05404-122">Doporučujeme, abyste nastavení tohoto parametru na ***všechny*** do protokolu všechny příkazy; výchozí hodnota je žádný (třeba k potvrzení).</span><span class="sxs-lookup"><span data-stu-id="05404-122">We recommend setting this parameter to ***all*** to log all statements; the default value is none (need to confirm).</span></span>

<span data-ttu-id="05404-123">**Protokolu\_min\_doba trvání\_příkaz** parametr nastaví limit v milisekundách příkazu zaznamenávat do protokolu.</span><span class="sxs-lookup"><span data-stu-id="05404-123">The **log\_min\_duration\_statement** parameter sets the limit in milliseconds of a statement to be logged.</span></span> <span data-ttu-id="05404-124">Všechny příkazy SQL, které se spouštět déle než nastavení pro parametr přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="05404-124">All SQL statements that run longer than the parameter setting are logged.</span></span> <span data-ttu-id="05404-125">Tento parametr je zakázána a nastavena na minus 1 (-1) ve výchozím nastavení (třeba k potvrzení).</span><span class="sxs-lookup"><span data-stu-id="05404-125">This parameter is disabled and set to minus 1 (-1) by default (need to confirm).</span></span> <span data-ttu-id="05404-126">Povolení tento parametr může být užitečné v sledování dolů neoptimalizované dotazů ve svých aplikacích.</span><span class="sxs-lookup"><span data-stu-id="05404-126">Enabling this parameter can be helpful in tracking down unoptimized queries in your applications.</span></span>

<span data-ttu-id="05404-127">**Protokolu\_min\_zprávy** umožňuje můžete řídit, která zprávy úrovně se zapisují do protokolu serveru.</span><span class="sxs-lookup"><span data-stu-id="05404-127">The **log\_min\_messages** allows you to control which message levels are written to the server log.</span></span> <span data-ttu-id="05404-128">Výchozí hodnota je upozornění.</span><span class="sxs-lookup"><span data-stu-id="05404-128">The default is WARNING.</span></span> <span data-ttu-id="05404-129">(musí potvrdit)</span><span class="sxs-lookup"><span data-stu-id="05404-129">(need to confirm)</span></span>

<span data-ttu-id="05404-130">Další informace o těchto nastaveních najdete v tématu [Chyba protokolování a generování sestav](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="05404-130">For more information on these settings, see [Error Reporting and Logging](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html) documentation.</span></span> <span data-ttu-id="05404-131">Zejména konfigurace databáze Azure pro PostgreSQL parametry serveru, najdete v části [protokoly serveru v Azure databáze PostgreSQL](concepts-server-logs.md).</span><span class="sxs-lookup"><span data-stu-id="05404-131">For particularly configuring Azure Database for PostgreSQL server parameters, see [Server Logs in Azure Database for PostgreSQL](concepts-server-logs.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="05404-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="05404-132">Next steps</span></span>
- <span data-ttu-id="05404-133">Přístup k protokolům pomocí rozhraní příkazového řádku Azure CLI, najdete v části [konfigurace a přístup protokolů serveru pomocí rozhraní příkazového řádku Azure](howto-configure-server-logs-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="05404-133">To access logs using Azure CLI command line interface, see [Configure and access server logs using Azure CLI](howto-configure-server-logs-using-cli.md)</span></span>
- <span data-ttu-id="05404-134">Další informace o parametry serveru, najdete v části [přizpůsobit parametry konfigurace serveru pomocí rozhraní příkazového řádku Azure](howto-configure-server-parameters-using-cli.md).</span><span class="sxs-lookup"><span data-stu-id="05404-134">For more information on server parameters, see [Customize server configuration parameters using Azure CLI](howto-configure-server-parameters-using-cli.md).</span></span>