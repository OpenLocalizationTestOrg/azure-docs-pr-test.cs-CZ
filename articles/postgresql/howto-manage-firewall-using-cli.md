---
title: "aaaCreate a správě Azure databáze pro pravidla brány firewall PostgreSQL pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje, jak toocreate a správě Azure databáze pro pravidla brány firewall PostgreSQL pomocí příkazového řádku Azure CLI."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 06e34c9e3996c2ec3df63191d794bc34d0ca0cf2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-azure-cli"></a><span data-ttu-id="c65cb-103">Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="c65cb-103">Create and manage Azure Database for PostgreSQL firewall rules using Azure CLI</span></span>
<span data-ttu-id="c65cb-104">Pravidla brány firewall na úrovni serveru povolte správci toomanage přístup tooan Azure databáze PostgreSQL serveru z konkrétní IP adresu nebo rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="c65cb-104">Server-level firewall rules enable administrators toomanage access tooan Azure Database for PostgreSQL Server from a specific IP address or range of IP addresses.</span></span> <span data-ttu-id="c65cb-105">Pomocí vhodného rozhraní příkazového řádku Azure, můžete vytvořit, aktualizovat, odstranit, seznamu a zobrazit toomanage pravidla brány firewall serveru.</span><span class="sxs-lookup"><span data-stu-id="c65cb-105">Using convenient Azure CLI commands, you can create, update, delete, list, and show firewall rules toomanage your server.</span></span> <span data-ttu-id="c65cb-106">Přehled Azure databáze pro PostgreSQL brány firewall, najdete v tématu [databáze Azure pro pravidla brány firewall serveru PostgreSQL](concepts-firewall-rules.md)</span><span class="sxs-lookup"><span data-stu-id="c65cb-106">For an overview of Azure Database for PostgreSQL firewalls, see [Azure Database for PostgreSQL Server firewall rules](concepts-firewall-rules.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c65cb-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c65cb-107">Prerequisites</span></span>
<span data-ttu-id="c65cb-108">toostep prostřednictvím této jak tooguide, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="c65cb-108">toostep through this how-tooguide, you need:</span></span>
- <span data-ttu-id="c65cb-109">[Databáze Azure pro PostgreSQL server a databáze](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="c65cb-109">An [Azure Database for PostgreSQL server and database](quickstart-create-server-database-azure-cli.md)</span></span>
- <span data-ttu-id="c65cb-110">Nainstalujte [Azure CLI 2.0](/cli/azure/install-azure-cli) příkazového řádku nástroje nebo použití hello prostředí cloudu Azure v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="c65cb-110">Install [Azure CLI 2.0](/cli/azure/install-azure-cli) command line utility or use hello Azure Cloud Shell in hello browser.</span></span>

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a><span data-ttu-id="c65cb-111">Konfigurace pravidel brány firewall pro databázi Azure pro PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="c65cb-111">Configure firewall rules for Azure Database for PostgreSQL</span></span>
<span data-ttu-id="c65cb-112">Hello [az postgres pravidla brány firewall-](/cli/azure/postgres/server/firewall-rule) příkazy jsou použité tooconfigure pravidla brány firewall.</span><span class="sxs-lookup"><span data-stu-id="c65cb-112">hello [az postgres server firewall-rule](/cli/azure/postgres/server/firewall-rule) commands are used tooconfigure firewall rules.</span></span>

## <a name="list-firewall-rules"></a><span data-ttu-id="c65cb-113">Seznam pravidel brány firewall</span><span class="sxs-lookup"><span data-stu-id="c65cb-113">List firewall rules</span></span> 
<span data-ttu-id="c65cb-114">existující hello toolist pravidel brány firewall serveru na hello server spustit hello [seznamu pravidlo brány firewall serveru postgres az](/cli/azure/postgres/server/firewall-rule#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="c65cb-114">toolist hello existing server firewall rules on hello server, run hello [az postgres server firewall-rule list](/cli/azure/postgres/server/firewall-rule#list) command.</span></span>
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401
```
<span data-ttu-id="c65cb-115">výstup Hello obsahuje seznam pravidel hello, pokud existuje, ve výchozím nastavení ve formátu JSON formátu.</span><span class="sxs-lookup"><span data-stu-id="c65cb-115">hello output lists hello rules if any, by default in JSON format.</span></span> <span data-ttu-id="c65cb-116">Můžete použít přepínač hello `--output table` pro přehlednějším tvaru tabulky jako výstup hello.</span><span class="sxs-lookup"><span data-stu-id="c65cb-116">You may use hello switch `--output table` for a more readable table format as hello output.</span></span>
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401 --output table
```
## <a name="create-firewall-rule"></a><span data-ttu-id="c65cb-117">Vytvořte pravidlo brány firewall</span><span class="sxs-lookup"><span data-stu-id="c65cb-117">Create firewall rule</span></span>
<span data-ttu-id="c65cb-118">toocreate nové pravidlo brány firewall na serveru hello, spuštěním hello [az postgres pravidla brány firewall-vytvořit](/cli/azure/postgres/server/firewall-rule#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="c65cb-118">toocreate a new firewall rule on hello server, run hello [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) command.</span></span> 

<span data-ttu-id="c65cb-119">Tento příklad umožňuje řadu všechny IP adresy tooaccess hello server **mypgserver 20170401.postgres.database.azure.com**</span><span class="sxs-lookup"><span data-stu-id="c65cb-119">This example allows a range of all IP addresses tooaccess hello server **mypgserver-20170401.postgres.database.azure.com**</span></span>
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup  --server mypgserver-20170401 --name "AllowIpRange" --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
<span data-ttu-id="c65cb-120">tooallow singulární tooaccess IP adresu, zadejte text hello stejnou adresu, protože hello počáteční IP a koncové IP adresy, jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="c65cb-120">tooallow a singular IP address tooaccess, provide hello same address as hello Start IP and End IP, as in this example.</span></span>
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup  
--server mypgserver-20170401 --name "AllowSingleIpAddress" --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
<span data-ttu-id="c65cb-121">Po úspěšné výstupu příkazu hello jsou uvedeny podrobnosti hello hello pravidla brány firewall, které jste vytvořili, ve výchozím nastavení ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="c65cb-121">Upon success, hello command output lists hello details of hello firewall rule you have created, by default in JSON format.</span></span> <span data-ttu-id="c65cb-122">Pokud dojde k selhání, výstup hello showserror text zprávy.</span><span class="sxs-lookup"><span data-stu-id="c65cb-122">If there is a failure, hello output showserror message text instead.</span></span>

## <a name="update-firewall-rule"></a><span data-ttu-id="c65cb-123">Aktualizovat pravidla brány firewall</span><span class="sxs-lookup"><span data-stu-id="c65cb-123">Update firewall rule</span></span> 
<span data-ttu-id="c65cb-124">Aktualizovat existující pravidlo brány firewall na serveru pomocí hello [aktualizace pravidlo brány firewall serveru postgres az](/cli/azure/postgres/server/firewall-rule#update) příkaz.</span><span class="sxs-lookup"><span data-stu-id="c65cb-124">Update an existing firewall rule on hello server using [az postgres server firewall-rule update](/cli/azure/postgres/server/firewall-rule#update) command.</span></span> <span data-ttu-id="c65cb-125">Zadejte název hello hello existující pravidla brány firewall jako vstup a hello počáteční IP adresy a koncové IP atributy tooupdate.</span><span class="sxs-lookup"><span data-stu-id="c65cb-125">Provide hello name of hello existing firewall rule as input, and hello start IP and end IP attributes tooupdate.</span></span>
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.255
```
<span data-ttu-id="c65cb-126">Po úspěšné výstupu příkazu hello jsou uvedeny podrobnosti hello hello pravidla brány firewall, které jste aktualizovali, ve výchozím nastavení ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="c65cb-126">Upon success, hello command output lists hello details of hello firewall rule you have updated, by default in JSON format.</span></span> <span data-ttu-id="c65cb-127">Pokud dojde k selhání, výstup hello showserror text zprávy.</span><span class="sxs-lookup"><span data-stu-id="c65cb-127">If there is a failure, hello output showserror message text instead.</span></span>
> [!NOTE]
> <span data-ttu-id="c65cb-128">Pokud pravidlo brány firewall hello neexistuje, získá vytvořit pomocí příkazu update hello.</span><span class="sxs-lookup"><span data-stu-id="c65cb-128">If hello firewall rule does not exist, it gets created by hello update command.</span></span>

## <a name="show-firewall-rule-details"></a><span data-ttu-id="c65cb-129">Zobrazit podrobnosti o pravidlech brány firewall</span><span class="sxs-lookup"><span data-stu-id="c65cb-129">Show firewall rule details</span></span>
<span data-ttu-id="c65cb-130">Můžete také zobrazit existující brány firewall hello pravidlo podrobnosti serveru spuštěním [zobrazit pravidlo brány firewall serveru postgres az](/cli/azure/postgres/server/firewall-rule#show) příkaz.</span><span class="sxs-lookup"><span data-stu-id="c65cb-130">You can also show hello existing firewall rule details for a server by running [az postgres server firewall-rule show](/cli/azure/postgres/server/firewall-rule#show) command.</span></span>
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange"
```
<span data-ttu-id="c65cb-131">Po úspěšné výstupu příkazu hello jsou uvedeny podrobnosti hello hello pravidla brány firewall, která jste zadali, ve výchozím nastavení ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="c65cb-131">Upon success, hello command output lists hello details of hello firewall rule you have specified, by default in JSON format.</span></span> <span data-ttu-id="c65cb-132">Pokud dojde k selhání, výstup hello showserror text zprávy.</span><span class="sxs-lookup"><span data-stu-id="c65cb-132">If there is a failure, hello output showserror message text instead.</span></span>

## <a name="delete-firewall-rule"></a><span data-ttu-id="c65cb-133">Odstranit pravidlo brány firewall</span><span class="sxs-lookup"><span data-stu-id="c65cb-133">Delete firewall rule</span></span>
<span data-ttu-id="c65cb-134">toorevoke přístup pro rozsah IP ze serveru hello, odstraňte existující pravidlo brány firewall tak, že spustíte hello [odstranit az postgres pravidla brány firewall-](/cli/azure/postgres/server/firewall-rule#delete) příkaz.</span><span class="sxs-lookup"><span data-stu-id="c65cb-134">toorevoke access for an IP range from hello server, delete an existing firewall rule by executing hello [az postgres server firewall-rule delete](/cli/azure/postgres/server/firewall-rule#delete) command.</span></span> <span data-ttu-id="c65cb-135">Zadejte název hello hello existující pravidla brány firewall.</span><span class="sxs-lookup"><span data-stu-id="c65cb-135">Provide hello name of hello existing firewall rule.</span></span>
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange"
```
<span data-ttu-id="c65cb-136">Po úspěšné neexistuje žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="c65cb-136">Upon success, there is no output.</span></span> <span data-ttu-id="c65cb-137">Při selhání je vrácen text hello chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="c65cb-137">Upon failure, hello error message text is returned.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c65cb-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c65cb-138">Next steps</span></span>
- <span data-ttu-id="c65cb-139">Podobně můžete použít webový prohlížeč příliš[vytvořit a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí hello portálu Azure](howto-manage-firewall-using-portal.md)</span><span class="sxs-lookup"><span data-stu-id="c65cb-139">Similarly, you can use a web browser too[Create and manage Azure Database for PostgreSQL firewall rules using hello Azure portal](howto-manage-firewall-using-portal.md)</span></span>
- <span data-ttu-id="c65cb-140">Lépe porozumět o [databáze Azure pro pravidla brány firewall serveru PostgreSQL](concepts-firewall-rules.md)</span><span class="sxs-lookup"><span data-stu-id="c65cb-140">Understand more about [Azure Database for PostgreSQL Server firewall rules](concepts-firewall-rules.md)</span></span>
- <span data-ttu-id="c65cb-141">Pomoc při připojování tooan Azure databáze PostgreSQL serveru najdete v tématu [knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="c65cb-141">For help in connecting tooan Azure Database for PostgreSQL server, see [Connection libraries for Azure Database for PostgreSQL](concepts-connection-libraries.md)</span></span>
