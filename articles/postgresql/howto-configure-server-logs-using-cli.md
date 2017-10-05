---
title: "Konfigurace a přístup k protokolům serveru pro PostgreSQL pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje postup konfigurace a přístup v protokolech serveru v Azure databázi PostgreSQL pomocí příkazového řádku Azure CLI."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 26f8e12c493904f722cad5191ee053feff20f7fc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a><span data-ttu-id="71183-103">Konfigurace a přístup k protokolům serveru pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="71183-103">Configure and access server logs using Azure CLI</span></span>
<span data-ttu-id="71183-104">Můžete si stáhnout PostgreSQL protokoly chyb serveru pomocí rozhraní příkazového řádku (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="71183-104">You can download the PostgreSQL server error logs using the Command Line Interface (Azure CLI).</span></span> <span data-ttu-id="71183-105">Přístup k protokoly transakcí však není podporován.</span><span class="sxs-lookup"><span data-stu-id="71183-105">However, access to transaction logs is not supported.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="71183-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="71183-106">Prerequisites</span></span>
<span data-ttu-id="71183-107">Chcete-li krok tímto průvodcem postupy, je třeba:</span><span class="sxs-lookup"><span data-stu-id="71183-107">To step through this how-to guide, you need:</span></span>
- <span data-ttu-id="71183-108">[Azure databázi PostgreSQL serveru.](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="71183-108">An [Azure Database for PostgreSQL server](quickstart-create-server-database-azure-cli.md)</span></span>
- <span data-ttu-id="71183-109">Nainstalujte [Azure CLI 2.0](/cli/azure/install-azure-cli) nástroj příkazového řádku nebo pomocí prostředí cloudové služby Azure v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="71183-109">Install [Azure CLI 2.0](/cli/azure/install-azure-cli) command-line utility or use the Azure Cloud Shell in the browser.</span></span>

## <a name="configure-logging-for-azure-database-for-postgresql"></a><span data-ttu-id="71183-110">Konfigurace protokolování pro databázi Azure pro PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="71183-110">Configure logging for Azure Database for PostgreSQL</span></span>
<span data-ttu-id="71183-111">Můžete nastavit server pro přístup k dotazu protokoly a protokoly chyb.</span><span class="sxs-lookup"><span data-stu-id="71183-111">You can configure the server to access query logs and error logs.</span></span> <span data-ttu-id="71183-112">Protokoly chyb může obsahovat informace o automatické vakuu, připojení a kontrolní body.</span><span class="sxs-lookup"><span data-stu-id="71183-112">Error logs can contain auto-vacuum, connection, and checkpoints information.</span></span>
1. <span data-ttu-id="71183-113">Povolení protokolování</span><span class="sxs-lookup"><span data-stu-id="71183-113">Turn on logging</span></span>
2. <span data-ttu-id="71183-114">Aktualizace protokolu\_prohlášení a protokolu\_min\_doba trvání\_příkaz Povolit protokolování dotazu</span><span class="sxs-lookup"><span data-stu-id="71183-114">Update log\_statement and log\_min\_duration\_statement to enable query logging</span></span>
3. <span data-ttu-id="71183-115">Doba uchování aktualizace</span><span class="sxs-lookup"><span data-stu-id="71183-115">Update retention period</span></span>

<span data-ttu-id="71183-116">Další informace najdete v tématu [přizpůsobení parametry konfigurace serveru](howto-configure-server-parameters-using-cli.md).</span><span class="sxs-lookup"><span data-stu-id="71183-116">For more information, see [customizing server configuration parameters](howto-configure-server-parameters-using-cli.md).</span></span>

## <a name="list-logs-for-azure-database-for-postgresql-server"></a><span data-ttu-id="71183-117">Seznam protokolů pro databázi Azure pro PostgreSQL server</span><span class="sxs-lookup"><span data-stu-id="71183-117">List logs for Azure Database for PostgreSQL server</span></span>
<span data-ttu-id="71183-118">Chcete-li zobrazit seznam souborů k dispozici protokolu pro váš server, spusťte [az postgres protokoly serveru seznamu](/cli/azure/postgres/server-logs#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="71183-118">To list the available log files for your server, run the [az postgres server-logs list](/cli/azure/postgres/server-logs#list) command.</span></span>

<span data-ttu-id="71183-119">Můžete seznam souborů protokolu pro server **mypgserver 20170401.postgres.database.azure.com** ve skupině prostředků **myresourcegroup**a přesměrování na textový soubor s názvem **protokolu\_soubory\_seznam.txt.**</span><span class="sxs-lookup"><span data-stu-id="71183-119">You can list the log files for server **mypgserver-20170401.postgres.database.azure.com** under Resource Group **myresourcegroup**, and direct it to a text file called **log\_files\_list.txt.**</span></span>
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mypgserver-20170401 > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a><span data-ttu-id="71183-120">Stažení protokolů místně ze serveru</span><span class="sxs-lookup"><span data-stu-id="71183-120">Download logs locally from the server</span></span>
<span data-ttu-id="71183-121">[Az postgres – protokoly serveru stáhnout](/cli/azure/postgres/server-logs#download) příkazu lze stáhnout jednotlivých protokolových souborů pro váš server.</span><span class="sxs-lookup"><span data-stu-id="71183-121">The [az postgres server-logs download](/cli/azure/postgres/server-logs#download) command allows you to download individual log files for your server.</span></span> 

<span data-ttu-id="71183-122">Tento příklad stáhne konkrétním souboru protokolu pro server **mypgserver 20170401.postgres.database.azure.com** ve skupině prostředků **myresourcegroup** na vašem místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="71183-122">This example downloads the specific log file for the server **mypgserver-20170401.postgres.database.azure.com** under Resource Group **myresourcegroup** to your local environment.</span></span>
```azurecli-interactive
az postgres server-logs download --name 20170414-mypgserver-20170401-postgresql.log --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="next-steps"></a><span data-ttu-id="71183-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="71183-123">Next steps</span></span>
- <span data-ttu-id="71183-124">Další informace o protokolech serveru najdete v tématu [protokoly serveru v Azure databáze PostgreSQL](concepts-server-logs.md)</span><span class="sxs-lookup"><span data-stu-id="71183-124">To learn more about server logs, see [Server Logs in Azure Database for PostgreSQL](concepts-server-logs.md)</span></span>
- <span data-ttu-id="71183-125">Další informace o parametry serveru, najdete v části [přizpůsobit parametry konfigurace serveru pomocí rozhraní příkazového řádku Azure](howto-configure-server-parameters-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="71183-125">For more information on server parameters, see [Customize server configuration parameters using Azure CLI](howto-configure-server-parameters-using-cli.md)</span></span>
