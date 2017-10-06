---
title: "protokoly serveru aaaConfigure a přístup pro PostgreSQL pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje, jak server hello tooconfigure a přístup protokolech v Azure databázi PostgreSQL pomocí příkazového řádku Azure CLI."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 924d4e7634cc48405bcb0f813cf61d8b72ad8aa0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a><span data-ttu-id="30d8a-103">Konfigurace a přístup k protokolům serveru pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="30d8a-103">Configure and access server logs using Azure CLI</span></span>
<span data-ttu-id="30d8a-104">Můžete si stáhnout hello PostgreSQL serveru protokoly chyb pomocí hello rozhraní příkazového řádku (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="30d8a-104">You can download hello PostgreSQL server error logs using hello Command Line Interface (Azure CLI).</span></span> <span data-ttu-id="30d8a-105">Přístup k protokolům tootransaction však není podporován.</span><span class="sxs-lookup"><span data-stu-id="30d8a-105">However, access tootransaction logs is not supported.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="30d8a-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="30d8a-106">Prerequisites</span></span>
<span data-ttu-id="30d8a-107">toostep prostřednictvím této jak tooguide, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="30d8a-107">toostep through this how-tooguide, you need:</span></span>
- <span data-ttu-id="30d8a-108">[Azure databázi PostgreSQL serveru.](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="30d8a-108">An [Azure Database for PostgreSQL server](quickstart-create-server-database-azure-cli.md)</span></span>
- <span data-ttu-id="30d8a-109">Nainstalujte [Azure CLI 2.0](/cli/azure/install-azure-cli) příkazového řádku nástroje nebo použití hello prostředí cloudu Azure v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="30d8a-109">Install [Azure CLI 2.0](/cli/azure/install-azure-cli) command-line utility or use hello Azure Cloud Shell in hello browser.</span></span>

## <a name="configure-logging-for-azure-database-for-postgresql"></a><span data-ttu-id="30d8a-110">Konfigurace protokolování pro databázi Azure pro PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="30d8a-110">Configure logging for Azure Database for PostgreSQL</span></span>
<span data-ttu-id="30d8a-111">Můžete nakonfigurovat hello serveru tooaccess dotazu protokoly a protokoly chyb.</span><span class="sxs-lookup"><span data-stu-id="30d8a-111">You can configure hello server tooaccess query logs and error logs.</span></span> <span data-ttu-id="30d8a-112">Protokoly chyb může obsahovat informace o automatické vakuu, připojení a kontrolní body.</span><span class="sxs-lookup"><span data-stu-id="30d8a-112">Error logs can contain auto-vacuum, connection, and checkpoints information.</span></span>
1. <span data-ttu-id="30d8a-113">Povolení protokolování</span><span class="sxs-lookup"><span data-stu-id="30d8a-113">Turn on logging</span></span>
2. <span data-ttu-id="30d8a-114">Aktualizace protokolu\_prohlášení a protokolu\_min\_doba trvání\_protokolování dotazu tooenable – příkaz</span><span class="sxs-lookup"><span data-stu-id="30d8a-114">Update log\_statement and log\_min\_duration\_statement tooenable query logging</span></span>
3. <span data-ttu-id="30d8a-115">Doba uchování aktualizace</span><span class="sxs-lookup"><span data-stu-id="30d8a-115">Update retention period</span></span>

<span data-ttu-id="30d8a-116">Další informace najdete v tématu [přizpůsobení parametry konfigurace serveru](howto-configure-server-parameters-using-cli.md).</span><span class="sxs-lookup"><span data-stu-id="30d8a-116">For more information, see [customizing server configuration parameters](howto-configure-server-parameters-using-cli.md).</span></span>

## <a name="list-logs-for-azure-database-for-postgresql-server"></a><span data-ttu-id="30d8a-117">Seznam protokolů pro databázi Azure pro PostgreSQL server</span><span class="sxs-lookup"><span data-stu-id="30d8a-117">List logs for Azure Database for PostgreSQL server</span></span>
<span data-ttu-id="30d8a-118">soubory k dispozici protokolu hello toolist pro váš server, spusťte hello [az postgres protokoly serveru seznamu](/cli/azure/postgres/server-logs#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="30d8a-118">toolist hello available log files for your server, run hello [az postgres server-logs list](/cli/azure/postgres/server-logs#list) command.</span></span>

<span data-ttu-id="30d8a-119">Můžete vytvořit seznam hello soubory protokolu pro server **mypgserver 20170401.postgres.database.azure.com** ve skupině prostředků **myresourcegroup**a nasměrovat ho tooa textového souboru s názvem **protokolu\_soubory\_seznam.txt.**</span><span class="sxs-lookup"><span data-stu-id="30d8a-119">You can list hello log files for server **mypgserver-20170401.postgres.database.azure.com** under Resource Group **myresourcegroup**, and direct it tooa text file called **log\_files\_list.txt.**</span></span>
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mypgserver-20170401 > log_files_list.txt
```
## <a name="download-logs-locally-from-hello-server"></a><span data-ttu-id="30d8a-120">Stažení protokolů místně z hello server</span><span class="sxs-lookup"><span data-stu-id="30d8a-120">Download logs locally from hello server</span></span>
<span data-ttu-id="30d8a-121">Hello [az postgres – protokoly serveru stáhnout](/cli/azure/postgres/server-logs#download) příkaz vám umožní toodownload jednotlivých protokolových souborů pro váš server.</span><span class="sxs-lookup"><span data-stu-id="30d8a-121">hello [az postgres server-logs download](/cli/azure/postgres/server-logs#download) command allows you toodownload individual log files for your server.</span></span> 

<span data-ttu-id="30d8a-122">Tento příklad stahování hello konkrétní protokol pro hello server **mypgserver 20170401.postgres.database.azure.com** ve skupině prostředků **myresourcegroup** tooyour místní prostředí.</span><span class="sxs-lookup"><span data-stu-id="30d8a-122">This example downloads hello specific log file for hello server **mypgserver-20170401.postgres.database.azure.com** under Resource Group **myresourcegroup** tooyour local environment.</span></span>
```azurecli-interactive
az postgres server-logs download --name 20170414-mypgserver-20170401-postgresql.log --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="next-steps"></a><span data-ttu-id="30d8a-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="30d8a-123">Next steps</span></span>
- <span data-ttu-id="30d8a-124">toolearn Další informace o protokoly serveru, najdete v části [protokoly serveru v Azure databáze PostgreSQL](concepts-server-logs.md)</span><span class="sxs-lookup"><span data-stu-id="30d8a-124">toolearn more about server logs, see [Server Logs in Azure Database for PostgreSQL](concepts-server-logs.md)</span></span>
- <span data-ttu-id="30d8a-125">Další informace o parametry serveru, najdete v části [přizpůsobit parametry konfigurace serveru pomocí rozhraní příkazového řádku Azure](howto-configure-server-parameters-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="30d8a-125">For more information on server parameters, see [Customize server configuration parameters using Azure CLI](howto-configure-server-parameters-using-cli.md)</span></span>
