---
title: "Jak tooback a obnovení serveru v databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Zjistěte, jak hello tooback zálohu a obnovení serveru v databázi Azure pro PostgreSQL pomocí rozhraní příkazového řádku Azure."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 0b9ed25e3e3a88dd5c7ffe2ae7c27f8eef9be710
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooback-up-and-restore-a-server-in-azure-database-for-postgresql-by-using-hello-azure-cli"></a><span data-ttu-id="43452-103">Jak hello tooback zálohu a obnovení serveru v databázi Azure pro PostgreSQL pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="43452-103">How tooback up and restore a server in Azure Database for PostgreSQL by using hello Azure CLI</span></span>

<span data-ttu-id="43452-104">Použijte databázi Azure pro PostgreSQL toorestore tooan databáze serveru starší data, která zahrnuje ze 7 dní too35.</span><span class="sxs-lookup"><span data-stu-id="43452-104">Use Azure Database for PostgreSQL toorestore a server database tooan earlier date that spans from 7 too35 days.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43452-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="43452-105">Prerequisites</span></span>
<span data-ttu-id="43452-106">toocomplete tento tooguide postupy, musíte:</span><span class="sxs-lookup"><span data-stu-id="43452-106">toocomplete this how-tooguide, you need:</span></span>
- <span data-ttu-id="43452-107">[Databáze Azure pro PostgreSQL server a databáze](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="43452-107">An [Azure Database for PostgreSQL server and database](quickstart-create-server-database-azure-cli.md)</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> <span data-ttu-id="43452-108">Je-li nainstalovat a používat rozhraní příkazového řádku Azure hello místně, tento jak tooguide vyžaduje, že používáte Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="43452-108">If you install and use hello Azure CLI locally, this how-tooguide requires that you use Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="43452-109">Zadejte verzi hello tooconfirm, příkazového řádku Azure CLI hello `az --version`.</span><span class="sxs-lookup"><span data-stu-id="43452-109">tooconfirm hello version, at hello Azure CLI command prompt, enter `az --version`.</span></span> <span data-ttu-id="43452-110">tooinstall nebo aktualizace, najdete v části [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="43452-110">tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="back-up-happens-automatically"></a><span data-ttu-id="43452-111">Zálohování se stane automaticky</span><span class="sxs-lookup"><span data-stu-id="43452-111">Back up happens automatically</span></span>
<span data-ttu-id="43452-112">Použijete-li databáze Azure pro PostgreSQL, služba hello databáze automaticky provede zálohování služby hello každých 5 minut.</span><span class="sxs-lookup"><span data-stu-id="43452-112">When you use Azure Database for PostgreSQL, hello database service automatically makes a backup of hello service every 5 minutes.</span></span> 

<span data-ttu-id="43452-113">Pro úroveň Basic hello zálohy jsou k dispozici po dobu 7 dnů.</span><span class="sxs-lookup"><span data-stu-id="43452-113">For Basic Tier, hello backups are available for 7 days.</span></span> <span data-ttu-id="43452-114">Pro úroveň Standard hello zálohy jsou k dispozici pro 35 dní.</span><span class="sxs-lookup"><span data-stu-id="43452-114">For Standard Tier, hello backups are available for 35 days.</span></span> <span data-ttu-id="43452-115">Další informace najdete v tématu [databáze Azure pro PostgreSQL cenové úrovně](concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="43452-115">For more information, see [Azure Database for PostgreSQL pricing tiers](concepts-service-tiers.md).</span></span>

<span data-ttu-id="43452-116">S touto automatické zálohování funkcí můžete obnovit hello server a její databází tooan starší datum nebo bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="43452-116">With this automatic backup feature, you can restore hello server and its databases tooan earlier date, or point in time.</span></span>

## <a name="restore-a-database-tooa-previous-point-in-time-by-using-hello-azure-cli"></a><span data-ttu-id="43452-117">Obnovit do databáze tooa předchozího bodu v čase pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="43452-117">Restore a database tooa previous point in time by using hello Azure CLI</span></span>
<span data-ttu-id="43452-118">Použijte databázi Azure pro PostgreSQL toorestore hello server tooa předchozí bod v čase.</span><span class="sxs-lookup"><span data-stu-id="43452-118">Use Azure Database for PostgreSQL toorestore hello server tooa previous point in time.</span></span> <span data-ttu-id="43452-119">Hello obnovit data jsou zkopírované tooa nový server a existující server hello je ponechán beze.</span><span class="sxs-lookup"><span data-stu-id="43452-119">hello restored data is copied tooa new server, and hello existing server is left as is.</span></span> <span data-ttu-id="43452-120">Například pokud tabulku omylem, bude vynechána. v poledne dnes můžete obnovit toohello čas těsně před polednem.</span><span class="sxs-lookup"><span data-stu-id="43452-120">For example, if a table is accidentally dropped at noon today, you can restore toohello time just before noon.</span></span> <span data-ttu-id="43452-121">Potom můžete načíst hello chybějící tabulky a data z hello obnovit kopii hello serveru.</span><span class="sxs-lookup"><span data-stu-id="43452-121">Then, you can retrieve hello missing table and data from hello restored copy of hello server.</span></span> 

<span data-ttu-id="43452-122">server hello toorestore, hello pomocí rozhraní příkazového řádku Azure [obnovení serveru postgres az](/cli/azure/postgres/server#restore) příkaz.</span><span class="sxs-lookup"><span data-stu-id="43452-122">toorestore hello server, use hello Azure CLI [az postgres server restore](/cli/azure/postgres/server#restore) command.</span></span>

### <a name="run-hello-restore-command"></a><span data-ttu-id="43452-123">Spusťte příkaz restore hello</span><span class="sxs-lookup"><span data-stu-id="43452-123">Run hello restore command</span></span>

<span data-ttu-id="43452-124">server hello toorestore, hello příkazového řádku Azure CLI, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="43452-124">toorestore hello server, at hello Azure CLI command prompt, enter hello following command:</span></span>

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

<span data-ttu-id="43452-125">Hello `az postgres server restore` příkaz vyžaduje hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="43452-125">hello `az postgres server restore` command requires hello following parameters:</span></span>
| <span data-ttu-id="43452-126">Nastavení</span><span class="sxs-lookup"><span data-stu-id="43452-126">Setting</span></span> | <span data-ttu-id="43452-127">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="43452-127">Suggested value</span></span> | <span data-ttu-id="43452-128">Popis</span><span class="sxs-lookup"><span data-stu-id="43452-128">Description</span></span>  |
| --- | --- | --- |
| <span data-ttu-id="43452-129">skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="43452-129">resource-group</span></span> |  <span data-ttu-id="43452-130">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="43452-130">myResourceGroup</span></span> |  <span data-ttu-id="43452-131">Skupinu prostředků, kde existuje hello zdrojového serveru.</span><span class="sxs-lookup"><span data-stu-id="43452-131">The resource group where hello source server exists.</span></span>  |
| <span data-ttu-id="43452-132">jméno</span><span class="sxs-lookup"><span data-stu-id="43452-132">name</span></span> | <span data-ttu-id="43452-133">Obnovit mypgserver</span><span class="sxs-lookup"><span data-stu-id="43452-133">mypgserver-restored</span></span> | <span data-ttu-id="43452-134">Název Hello hello nový server, který je vytvořen pomocí příkazu restore hello.</span><span class="sxs-lookup"><span data-stu-id="43452-134">hello name of hello new server that is created by hello restore command.</span></span> |
| <span data-ttu-id="43452-135">obnovení bodu v čase</span><span class="sxs-lookup"><span data-stu-id="43452-135">restore-point-in-time</span></span> | <span data-ttu-id="43452-136">2017-04-13T13:59:00Z</span><span class="sxs-lookup"><span data-stu-id="43452-136">2017-04-13T13:59:00Z</span></span> | <span data-ttu-id="43452-137">Vyberte bod v toorestore čas k.</span><span class="sxs-lookup"><span data-stu-id="43452-137">Select a point in time toorestore to.</span></span> <span data-ttu-id="43452-138">Toto datum a čas musí být v rámci hello zdrojového serveru zálohovat dobu uchování.</span><span class="sxs-lookup"><span data-stu-id="43452-138">This date and time must be within hello source server's back up retention period.</span></span> <span data-ttu-id="43452-139">Používejte formát hello ISO8601 data a času.</span><span class="sxs-lookup"><span data-stu-id="43452-139">Use hello ISO8601 date and time format.</span></span> <span data-ttu-id="43452-140">Například můžete použít vlastní místní časové pásmo, jako například `2017-04-13T05:59:00-08:00`.</span><span class="sxs-lookup"><span data-stu-id="43452-140">For example, you can use your own local time zone, such as `2017-04-13T05:59:00-08:00`.</span></span> <span data-ttu-id="43452-141">Můžete taky hello UTC zulština formátu, například `2017-04-13T13:59:00Z`.</span><span class="sxs-lookup"><span data-stu-id="43452-141">You can also use hello UTC Zulu format, for example, `2017-04-13T13:59:00Z`.</span></span> |
| <span data-ttu-id="43452-142">zdrojový server</span><span class="sxs-lookup"><span data-stu-id="43452-142">source-server</span></span> | <span data-ttu-id="43452-143">mypgserver 20170401</span><span class="sxs-lookup"><span data-stu-id="43452-143">mypgserver-20170401</span></span> | <span data-ttu-id="43452-144">Hello název nebo ID hello zdrojový server toorestore z.</span><span class="sxs-lookup"><span data-stu-id="43452-144">hello name or ID of hello source server toorestore from.</span></span> |

<span data-ttu-id="43452-145">Při obnovení serveru tooan dřívějšího bodu v čase se vytvoří nový server.</span><span class="sxs-lookup"><span data-stu-id="43452-145">When you restore a server tooan earlier point in time, a new server is created.</span></span> <span data-ttu-id="43452-146">původní server Hello a její databáze z hello zadané bodu v čase zkopírovaný toohello nový server.</span><span class="sxs-lookup"><span data-stu-id="43452-146">hello original server and its databases from hello specified point in time are copied toohello new server.</span></span>

<span data-ttu-id="43452-147">hodnoty Hello umístění a cenovou úroveň pro zůstanou hello obnovit server hello stejné jako původní server hello.</span><span class="sxs-lookup"><span data-stu-id="43452-147">hello location and pricing tier values for hello restored server remain hello same as hello original server.</span></span> 

<span data-ttu-id="43452-148">Hello `az postgres server restore` příkaz je synchronní.</span><span class="sxs-lookup"><span data-stu-id="43452-148">hello `az postgres server restore` command is synchronous.</span></span> <span data-ttu-id="43452-149">Po obnovení serveru hello, můžete ho znovu toorepeat hello proces pro jiný bod v čase.</span><span class="sxs-lookup"><span data-stu-id="43452-149">After hello server is restored, you can use it again toorepeat hello process for a different point in time.</span></span> 

<span data-ttu-id="43452-150">Po hello dokončení procesu obnovení, vyhledejte nový server hello a ověřte, že je hello data obnovit podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="43452-150">After hello restore process finishes, locate hello new server and verify that hello data is restored as expected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="43452-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="43452-151">Next steps</span></span>
[<span data-ttu-id="43452-152">Knihovny připojení pro databázi Azure pro PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="43452-152">Connection libraries for Azure Database for PostgreSQL</span></span>](concepts-connection-libraries.md)
