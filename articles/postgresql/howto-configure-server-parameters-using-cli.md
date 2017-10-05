---
title: "Konfigurovat parametry služby ve službě Azure Database pro PostgreSQL | Microsoft Docs"
description: "Tento článek popisuje, jak nakonfigurovat parametry služby ve službě Azure Database pro PostgreSQL pomocí příkazového řádku Azure CLI."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: c8a3b5a0225c2cede180d8d57681f2e1a6c6cc3a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="customize-server-configuration-parameters-using-azure-cli"></a><span data-ttu-id="38d8f-103">Přizpůsobit parametry konfigurace serveru pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="38d8f-103">Customize server configuration parameters using Azure CLI</span></span>
<span data-ttu-id="38d8f-104">Můžete zobrazit seznam, zobrazit a aktualizovat parametry konfigurace pro server Azure PostgreSQL pomocí rozhraní příkazového řádku (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="38d8f-104">You can list, show and update configuration parameters for an Azure PostgreSQL server using the Command Line Interface (Azure CLI).</span></span> <span data-ttu-id="38d8f-105">Však pouze podmnožinu modul konfigurace jsou zveřejněné na úrovni serveru a je možné upravit.</span><span class="sxs-lookup"><span data-stu-id="38d8f-105">However, only a subset of engine configurations are exposed at server-level and can be modified.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="38d8f-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="38d8f-106">Prerequisites</span></span>
<span data-ttu-id="38d8f-107">Chcete-li krok tímto průvodcem postupy, je třeba:</span><span class="sxs-lookup"><span data-stu-id="38d8f-107">To step through this how-to guide, you need:</span></span>
- <span data-ttu-id="38d8f-108">Server a databáze [vytvoření databáze Azure pro PostgreSQL](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="38d8f-108">A server and database [Create an Azure Database for PostgreSQL](quickstart-create-server-database-azure-cli.md)</span></span>
- <span data-ttu-id="38d8f-109">Nainstalujte [Azure CLI 2.0](/cli/azure/install-azure-cli) příkaz nástroj řádku nebo pomocí prostředí cloudové služby Azure v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="38d8f-109">Install [Azure CLI 2.0](/cli/azure/install-azure-cli) command line utility or use the Azure Cloud Shell in the browser.</span></span>

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a><span data-ttu-id="38d8f-110">Seznam parametrů konfigurace serveru pro databázi Azure pro PostgreSQL serveru</span><span class="sxs-lookup"><span data-stu-id="38d8f-110">List server configuration parameters for Azure Database for PostgreSQL server</span></span>
<span data-ttu-id="38d8f-111">Chcete-li zobrazit seznam všech změn parametrů v serveru a jejich hodnoty, spusťte [seznamu konfigurací serveru postgres az](/cli/azure/postgres/server/configuration#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="38d8f-111">To list all modifiable parameters in a server and their values, run the [az postgres server configuration list](/cli/azure/postgres/server/configuration#list) command.</span></span>

<span data-ttu-id="38d8f-112">Můžete vytvořit seznam parametrů konfigurace serveru pro server **mypgserver 20170401.postgres.database.azure.com** ve skupině prostředků **myresourcegroup**.</span><span class="sxs-lookup"><span data-stu-id="38d8f-112">You can list the server configuration parameters for the server **mypgserver-20170401.postgres.database.azure.com** under resource group **myresourcegroup**.</span></span>
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="show-server-configuration-parameter-details"></a><span data-ttu-id="38d8f-113">Zobrazit podrobnosti parametr konfigurace serveru</span><span class="sxs-lookup"><span data-stu-id="38d8f-113">Show server configuration parameter details</span></span>
<span data-ttu-id="38d8f-114">Chcete-li zobrazit podrobnosti o konkrétní konfiguračního parametru pro server, spusťte [az postgres serveru konfigurace zobrazit](/cli/azure/postgres/server/configuration#show) příkaz.</span><span class="sxs-lookup"><span data-stu-id="38d8f-114">To show details about a particular configuration parameter for a server, run the [az postgres server configuration show](/cli/azure/postgres/server/configuration#show)  command.</span></span>

<span data-ttu-id="38d8f-115">Tento příklad zobrazuje podrobnosti o **protokolu\_min\_zprávy** parametr konfigurace serveru pro server **mypgserver 20170401.postgres.database.azure.com** ve skupině prostředků **myresourcegroup.**</span><span class="sxs-lookup"><span data-stu-id="38d8f-115">This example shows details of the **log\_min\_messages** server configuration parameter for server **mypgserver-20170401.postgres.database.azure.com** under resource group **myresourcegroup.**</span></span>
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="modify-server-configuration-parameter-value"></a><span data-ttu-id="38d8f-116">Změnit hodnotu parametru konfigurace serveru</span><span class="sxs-lookup"><span data-stu-id="38d8f-116">Modify server configuration parameter value</span></span>
<span data-ttu-id="38d8f-117">Můžete také upravit hodnotu určité parametr konfigurace serveru, a tím se aktualizuje základní hodnotu konfigurace pro modul serveru PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="38d8f-117">You can also modify the value of a certain server configuration parameter, and this updates the underlying configuration value for the PostgreSQL server engine.</span></span> <span data-ttu-id="38d8f-118">Aktualizace konfigurace pomocí [az postgres serveru konfigurační sady](/cli/azure/postgres/server/configuration#set) příkaz.</span><span class="sxs-lookup"><span data-stu-id="38d8f-118">To update the configuration use the [az postgres server configuration set](/cli/azure/postgres/server/configuration#set) command.</span></span> 

<span data-ttu-id="38d8f-119">Aktualizace **protokolu\_min\_zprávy** parametr konfigurace serveru serveru **mypgserver 20170401.postgres.database.azure.com** ve skupině prostředků **myresourcegroup.**</span><span class="sxs-lookup"><span data-stu-id="38d8f-119">To update the **log\_min\_messages** server configuration parameter of server **mypgserver-20170401.postgres.database.azure.com** under resource group **myresourcegroup.**</span></span>
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401 --value INFO
```
<span data-ttu-id="38d8f-120">Pokud chcete resetovat hodnota parametru konfigurace, rozhodnete jednoduše vynechte nepovinný `--value` parametr a službu použije se výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="38d8f-120">If you want to reset the value of a configuration parameter, you simply choose to leave out the optional `--value` parameter, and the service will apply the default value.</span></span> <span data-ttu-id="38d8f-121">Ve výše příkladu ho bude vypadat:</span><span class="sxs-lookup"><span data-stu-id="38d8f-121">In above example, it would look like:</span></span>
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
<span data-ttu-id="38d8f-122">Tím **protokolu\_min\_zprávy** konfigurace na výchozí hodnotu **upozornění**.</span><span class="sxs-lookup"><span data-stu-id="38d8f-122">This will reset the **log\_min\_messages** configuration to the default value **WARNING**.</span></span> <span data-ttu-id="38d8f-123">Další informace o konfiguraci serveru a povolenou hodnoty, naleznete v dokumentaci k PostgreSQL na [konfigurace serveru](https://www.postgresql.org/docs/9.6/static/runtime-config.html).</span><span class="sxs-lookup"><span data-stu-id="38d8f-123">For further details on server configuration and permissible values, see PostgreSQL documentation on [Server Configuration](https://www.postgresql.org/docs/9.6/static/runtime-config.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="38d8f-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="38d8f-124">Next steps</span></span>
- <span data-ttu-id="38d8f-125">Konfigurace a přístup k protokolům serveru najdete v tématu [protokoly serveru v Azure databáze PostgreSQL](concepts-server-logs.md)</span><span class="sxs-lookup"><span data-stu-id="38d8f-125">To configure and access server logs, see [Server Logs in Azure Database for PostgreSQL](concepts-server-logs.md)</span></span>
