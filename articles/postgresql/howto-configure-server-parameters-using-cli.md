---
title: "parametry služby hello aaaConfigure v databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Tento článek popisuje, jak tooconfigure parametry hello služby v Azure databázi PostgreSQL použití hello příkazového řádku Azure CLI."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 84a11de24ba87fc0eb6744aaa4b53f65a183903d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-server-configuration-parameters-using-azure-cli"></a><span data-ttu-id="67578-103">Přizpůsobit parametry konfigurace serveru pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="67578-103">Customize server configuration parameters using Azure CLI</span></span>
<span data-ttu-id="67578-104">Můžete zobrazit seznam, zobrazit a aktualizovat parametry konfigurace pro server Azure PostgreSQL pomocí hello rozhraní příkazového řádku (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="67578-104">You can list, show and update configuration parameters for an Azure PostgreSQL server using hello Command Line Interface (Azure CLI).</span></span> <span data-ttu-id="67578-105">Však pouze podmnožinu modul konfigurace jsou zveřejněné na úrovni serveru a je možné upravit.</span><span class="sxs-lookup"><span data-stu-id="67578-105">However, only a subset of engine configurations are exposed at server-level and can be modified.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="67578-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="67578-106">Prerequisites</span></span>
<span data-ttu-id="67578-107">toostep prostřednictvím této jak tooguide, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="67578-107">toostep through this how-tooguide, you need:</span></span>
- <span data-ttu-id="67578-108">Server a databáze [vytvoření databáze Azure pro PostgreSQL](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="67578-108">A server and database [Create an Azure Database for PostgreSQL](quickstart-create-server-database-azure-cli.md)</span></span>
- <span data-ttu-id="67578-109">Nainstalujte [Azure CLI 2.0](/cli/azure/install-azure-cli) příkazového řádku nástroje nebo použití hello prostředí cloudu Azure v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="67578-109">Install [Azure CLI 2.0](/cli/azure/install-azure-cli) command line utility or use hello Azure Cloud Shell in hello browser.</span></span>

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a><span data-ttu-id="67578-110">Seznam parametrů konfigurace serveru pro databázi Azure pro PostgreSQL serveru</span><span class="sxs-lookup"><span data-stu-id="67578-110">List server configuration parameters for Azure Database for PostgreSQL server</span></span>
<span data-ttu-id="67578-111">spuštění všech změn parametrů v serveru a jejich hodnoty toolist hello [seznamu konfigurací serveru postgres az](/cli/azure/postgres/server/configuration#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="67578-111">toolist all modifiable parameters in a server and their values, run hello [az postgres server configuration list](/cli/azure/postgres/server/configuration#list) command.</span></span>

<span data-ttu-id="67578-112">Můžete vytvořit seznam hello parametry konfigurace serveru pro hello server **mypgserver 20170401.postgres.database.azure.com** ve skupině prostředků **myresourcegroup**.</span><span class="sxs-lookup"><span data-stu-id="67578-112">You can list hello server configuration parameters for hello server **mypgserver-20170401.postgres.database.azure.com** under resource group **myresourcegroup**.</span></span>
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="show-server-configuration-parameter-details"></a><span data-ttu-id="67578-113">Zobrazit podrobnosti parametr konfigurace serveru</span><span class="sxs-lookup"><span data-stu-id="67578-113">Show server configuration parameter details</span></span>
<span data-ttu-id="67578-114">tooshow podrobnosti o konkrétní konfiguračního parametru pro server, spusťte hello [az postgres serveru konfigurace zobrazit](/cli/azure/postgres/server/configuration#show) příkaz.</span><span class="sxs-lookup"><span data-stu-id="67578-114">tooshow details about a particular configuration parameter for a server, run hello [az postgres server configuration show](/cli/azure/postgres/server/configuration#show)  command.</span></span>

<span data-ttu-id="67578-115">Tento příklad zobrazuje podrobnosti o hello **protokolu\_min\_zprávy** parametr konfigurace serveru pro server **mypgserver 20170401.postgres.database.azure.com** v části Skupina prostředků **myresourcegroup.**</span><span class="sxs-lookup"><span data-stu-id="67578-115">This example shows details of hello **log\_min\_messages** server configuration parameter for server **mypgserver-20170401.postgres.database.azure.com** under resource group **myresourcegroup.**</span></span>
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="modify-server-configuration-parameter-value"></a><span data-ttu-id="67578-116">Změnit hodnotu parametru konfigurace serveru</span><span class="sxs-lookup"><span data-stu-id="67578-116">Modify server configuration parameter value</span></span>
<span data-ttu-id="67578-117">Můžete také upravit hello hodnotu určité parametr konfigurace serveru, a tím se aktualizuje hello základní hodnota konfigurace pro modul server hello PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="67578-117">You can also modify hello value of a certain server configuration parameter, and this updates hello underlying configuration value for hello PostgreSQL server engine.</span></span> <span data-ttu-id="67578-118">tooupdate hello konfigurace použití hello [az postgres serveru konfigurační sady](/cli/azure/postgres/server/configuration#set) příkaz.</span><span class="sxs-lookup"><span data-stu-id="67578-118">tooupdate hello configuration use hello [az postgres server configuration set](/cli/azure/postgres/server/configuration#set) command.</span></span> 

<span data-ttu-id="67578-119">tooupdate hello **protokolu\_min\_zprávy** parametr konfigurace serveru serveru **mypgserver 20170401.postgres.database.azure.com** ve skupině prostředků **myresourcegroup.**</span><span class="sxs-lookup"><span data-stu-id="67578-119">tooupdate hello **log\_min\_messages** server configuration parameter of server **mypgserver-20170401.postgres.database.azure.com** under resource group **myresourcegroup.**</span></span>
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401 --value INFO
```
<span data-ttu-id="67578-120">Pokud chcete tooreset hello hodnoty parametru konfigurace, jednoduše vyberte tooleave out hello volitelné `--value` parametr a hello služby budou platit hello výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="67578-120">If you want tooreset hello value of a configuration parameter, you simply choose tooleave out hello optional `--value` parameter, and hello service will apply hello default value.</span></span> <span data-ttu-id="67578-121">Ve výše příkladu ho bude vypadat:</span><span class="sxs-lookup"><span data-stu-id="67578-121">In above example, it would look like:</span></span>
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
<span data-ttu-id="67578-122">Tím obnovíte hello **protokolu\_min\_zprávy** konfigurace toohello výchozí hodnota **upozornění**.</span><span class="sxs-lookup"><span data-stu-id="67578-122">This will reset hello **log\_min\_messages** configuration toohello default value **WARNING**.</span></span> <span data-ttu-id="67578-123">Další informace o konfiguraci serveru a povolenou hodnoty, naleznete v dokumentaci k PostgreSQL na [konfigurace serveru](https://www.postgresql.org/docs/9.6/static/runtime-config.html).</span><span class="sxs-lookup"><span data-stu-id="67578-123">For further details on server configuration and permissible values, see PostgreSQL documentation on [Server Configuration](https://www.postgresql.org/docs/9.6/static/runtime-config.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="67578-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="67578-124">Next steps</span></span>
- <span data-ttu-id="67578-125">protokoly serveru tooconfigure a přístup, najdete v části [protokoly serveru v Azure databáze PostgreSQL](concepts-server-logs.md)</span><span class="sxs-lookup"><span data-stu-id="67578-125">tooconfigure and access server logs, see [Server Logs in Azure Database for PostgreSQL](concepts-server-logs.md)</span></span>
