---
title: "Postup zálohování a obnovení serveru v databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Zjistěte, jak zálohovat a obnovit server v databázi Azure pro PostgreSQL pomocí rozhraní příkazového řádku Azure."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 871887e67d686a965a0648d2c6f0c72b3008db05
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-postgresql-by-using-the-azure-cli"></a><span data-ttu-id="2eef6-103">Postup zálohování a obnovení serveru v databázi Azure pro PostgreSQL pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="2eef6-103">How to back up and restore a server in Azure Database for PostgreSQL by using the Azure CLI</span></span>

<span data-ttu-id="2eef6-104">Databáze Azure pro PostgreSQL použijte k obnovení databáze serveru do předchozího stavu, která zahrnuje ze 7 na 35 dnů.</span><span class="sxs-lookup"><span data-stu-id="2eef6-104">Use Azure Database for PostgreSQL to restore a server database to an earlier date that spans from 7 to 35 days.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2eef6-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2eef6-105">Prerequisites</span></span>
<span data-ttu-id="2eef6-106">Chcete-li provést tento postup průvodce, je třeba:</span><span class="sxs-lookup"><span data-stu-id="2eef6-106">To complete this how-to guide, you need:</span></span>
- <span data-ttu-id="2eef6-107">[Databáze Azure pro PostgreSQL server a databáze](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="2eef6-107">An [Azure Database for PostgreSQL server and database](quickstart-create-server-database-azure-cli.md)</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> <span data-ttu-id="2eef6-108">Je-li nainstalovat a používat rozhraní příkazového řádku Azure místně, tento postup Průvodce vyžaduje, že používáte Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="2eef6-108">If you install and use the Azure CLI locally, this how-to guide requires that you use Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="2eef6-109">Chcete-li ověřit verzi příkazového řádku Azure CLI zadejte `az --version`.</span><span class="sxs-lookup"><span data-stu-id="2eef6-109">To confirm the version, at the Azure CLI command prompt, enter `az --version`.</span></span> <span data-ttu-id="2eef6-110">K instalaci nebo upgradu, najdete v části [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="2eef6-110">To install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="back-up-happens-automatically"></a><span data-ttu-id="2eef6-111">Zálohování se stane automaticky</span><span class="sxs-lookup"><span data-stu-id="2eef6-111">Back up happens automatically</span></span>
<span data-ttu-id="2eef6-112">Pokud používáte Azure databáze pro PostgreSQL, služba databáze automaticky provede zálohování služby každých 5 minut.</span><span class="sxs-lookup"><span data-stu-id="2eef6-112">When you use Azure Database for PostgreSQL, the database service automatically makes a backup of the service every 5 minutes.</span></span> 

<span data-ttu-id="2eef6-113">Pro úroveň Basic zálohy jsou k dispozici po dobu 7 dnů.</span><span class="sxs-lookup"><span data-stu-id="2eef6-113">For Basic Tier, the backups are available for 7 days.</span></span> <span data-ttu-id="2eef6-114">Pro úroveň Standard zálohování jsou k dispozici pro 35 dní.</span><span class="sxs-lookup"><span data-stu-id="2eef6-114">For Standard Tier, the backups are available for 35 days.</span></span> <span data-ttu-id="2eef6-115">Další informace najdete v tématu [databáze Azure pro PostgreSQL cenové úrovně](concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="2eef6-115">For more information, see [Azure Database for PostgreSQL pricing tiers](concepts-service-tiers.md).</span></span>

<span data-ttu-id="2eef6-116">Toto automatické funkce zálohování můžete obnovit serveru a jeho databáze do stavu nebo bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="2eef6-116">With this automatic backup feature, you can restore the server and its databases to an earlier date, or point in time.</span></span>

## <a name="restore-a-database-to-a-previous-point-in-time-by-using-the-azure-cli"></a><span data-ttu-id="2eef6-117">Obnovení databáze do předchozího bodu v čase pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="2eef6-117">Restore a database to a previous point in time by using the Azure CLI</span></span>
<span data-ttu-id="2eef6-118">Použijte databázi Azure pro PostgreSQL a obnovte server do předchozího bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="2eef6-118">Use Azure Database for PostgreSQL to restore the server to a previous point in time.</span></span> <span data-ttu-id="2eef6-119">Obnovená data se zkopíruje na nový server a existující server je ponechán beze.</span><span class="sxs-lookup"><span data-stu-id="2eef6-119">The restored data is copied to a new server, and the existing server is left as is.</span></span> <span data-ttu-id="2eef6-120">Například pokud tabulka omylem, bude vynechána. v poledne dnes můžete obnovit na čas těsně před polednem.</span><span class="sxs-lookup"><span data-stu-id="2eef6-120">For example, if a table is accidentally dropped at noon today, you can restore to the time just before noon.</span></span> <span data-ttu-id="2eef6-121">Potom můžete načíst chybějící tabulku a dat z obnovené kopie serveru.</span><span class="sxs-lookup"><span data-stu-id="2eef6-121">Then, you can retrieve the missing table and data from the restored copy of the server.</span></span> 

<span data-ttu-id="2eef6-122">K obnovení serveru, použijte rozhraní příkazového řádku Azure [obnovení serveru postgres az](/cli/azure/postgres/server#restore) příkaz.</span><span class="sxs-lookup"><span data-stu-id="2eef6-122">To restore the server, use the Azure CLI [az postgres server restore](/cli/azure/postgres/server#restore) command.</span></span>

### <a name="run-the-restore-command"></a><span data-ttu-id="2eef6-123">Spusťte příkaz restore</span><span class="sxs-lookup"><span data-stu-id="2eef6-123">Run the restore command</span></span>

<span data-ttu-id="2eef6-124">K obnovení serveru, na příkazovém řádku Azure CLI, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2eef6-124">To restore the server, at the Azure CLI command prompt, enter the following command:</span></span>

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

<span data-ttu-id="2eef6-125">`az postgres server restore` Příkaz vyžaduje následující parametry:</span><span class="sxs-lookup"><span data-stu-id="2eef6-125">The `az postgres server restore` command requires the following parameters:</span></span>
| <span data-ttu-id="2eef6-126">Nastavení</span><span class="sxs-lookup"><span data-stu-id="2eef6-126">Setting</span></span> | <span data-ttu-id="2eef6-127">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="2eef6-127">Suggested value</span></span> | <span data-ttu-id="2eef6-128">Popis</span><span class="sxs-lookup"><span data-stu-id="2eef6-128">Description</span></span>  |
| --- | --- | --- |
| <span data-ttu-id="2eef6-129">skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="2eef6-129">resource-group</span></span> |  <span data-ttu-id="2eef6-130">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="2eef6-130">myResourceGroup</span></span> |  <span data-ttu-id="2eef6-131">Skupinu prostředků, kde existuje na zdrojovém serveru.</span><span class="sxs-lookup"><span data-stu-id="2eef6-131">The resource group where the source server exists.</span></span>  |
| <span data-ttu-id="2eef6-132">jméno</span><span class="sxs-lookup"><span data-stu-id="2eef6-132">name</span></span> | <span data-ttu-id="2eef6-133">Obnovit mypgserver</span><span class="sxs-lookup"><span data-stu-id="2eef6-133">mypgserver-restored</span></span> | <span data-ttu-id="2eef6-134">Název nového serveru, který je vytvořen pomocí příkazu restore.</span><span class="sxs-lookup"><span data-stu-id="2eef6-134">The name of the new server that is created by the restore command.</span></span> |
| <span data-ttu-id="2eef6-135">obnovení bodu v čase</span><span class="sxs-lookup"><span data-stu-id="2eef6-135">restore-point-in-time</span></span> | <span data-ttu-id="2eef6-136">2017-04-13T13:59:00Z</span><span class="sxs-lookup"><span data-stu-id="2eef6-136">2017-04-13T13:59:00Z</span></span> | <span data-ttu-id="2eef6-137">Vyberte bod v čase k obnovení.</span><span class="sxs-lookup"><span data-stu-id="2eef6-137">Select a point in time to restore to.</span></span> <span data-ttu-id="2eef6-138">Toto datum a čas musí být v rámci zdrojového serveru na zálohování dobu uchování.</span><span class="sxs-lookup"><span data-stu-id="2eef6-138">This date and time must be within the source server's back up retention period.</span></span> <span data-ttu-id="2eef6-139">Použijte formát ISO8601 data a času.</span><span class="sxs-lookup"><span data-stu-id="2eef6-139">Use the ISO8601 date and time format.</span></span> <span data-ttu-id="2eef6-140">Například můžete použít vlastní místní časové pásmo, jako například `2017-04-13T05:59:00-08:00`.</span><span class="sxs-lookup"><span data-stu-id="2eef6-140">For example, you can use your own local time zone, such as `2017-04-13T05:59:00-08:00`.</span></span> <span data-ttu-id="2eef6-141">Můžete také použít formát zulština UTC, například `2017-04-13T13:59:00Z`.</span><span class="sxs-lookup"><span data-stu-id="2eef6-141">You can also use the UTC Zulu format, for example, `2017-04-13T13:59:00Z`.</span></span> |
| <span data-ttu-id="2eef6-142">zdrojový server</span><span class="sxs-lookup"><span data-stu-id="2eef6-142">source-server</span></span> | <span data-ttu-id="2eef6-143">mypgserver 20170401</span><span class="sxs-lookup"><span data-stu-id="2eef6-143">mypgserver-20170401</span></span> | <span data-ttu-id="2eef6-144">Název nebo ID obnovení ze zdrojového serveru.</span><span class="sxs-lookup"><span data-stu-id="2eef6-144">The name or ID of the source server to restore from.</span></span> |

<span data-ttu-id="2eef6-145">Při obnovení serveru do dřívějšího bodu v čase, se vytvoří nový server.</span><span class="sxs-lookup"><span data-stu-id="2eef6-145">When you restore a server to an earlier point in time, a new server is created.</span></span> <span data-ttu-id="2eef6-146">Původní server a její databáze ze zadaného bodu v čase se zkopírují na nový server.</span><span class="sxs-lookup"><span data-stu-id="2eef6-146">The original server and its databases from the specified point in time are copied to the new server.</span></span>

<span data-ttu-id="2eef6-147">Umístění a cenovou úroveň hodnoty pro obnovené server zůstat stejné jako původní server.</span><span class="sxs-lookup"><span data-stu-id="2eef6-147">The location and pricing tier values for the restored server remain the same as the original server.</span></span> 

<span data-ttu-id="2eef6-148">`az postgres server restore` Příkaz je synchronní.</span><span class="sxs-lookup"><span data-stu-id="2eef6-148">The `az postgres server restore` command is synchronous.</span></span> <span data-ttu-id="2eef6-149">Po obnovení serveru, můžete ho znovu opakujte postup pro jiný bod v čase.</span><span class="sxs-lookup"><span data-stu-id="2eef6-149">After the server is restored, you can use it again to repeat the process for a different point in time.</span></span> 

<span data-ttu-id="2eef6-150">Po dokončení procesu obnovení, vyhledejte na nový server a ověřte, že budou data obnovena podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="2eef6-150">After the restore process finishes, locate the new server and verify that the data is restored as expected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2eef6-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2eef6-151">Next steps</span></span>
[<span data-ttu-id="2eef6-152">Knihovny připojení pro databázi Azure pro PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="2eef6-152">Connection libraries for Azure Database for PostgreSQL</span></span>](concepts-connection-libraries.md)
