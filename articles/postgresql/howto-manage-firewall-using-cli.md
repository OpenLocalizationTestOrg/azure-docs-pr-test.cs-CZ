---
title: "Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje postup vytvoření a správě Azure databáze pro pravidla brány firewall PostgreSQL pomocí příkazového řádku Azure CLI."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 6f081416dd7d78f0153b3fda21a340a8c1a70c5f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-azure-cli"></a><span data-ttu-id="b4c09-103">Vytvářet a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="b4c09-103">Create and manage Azure Database for PostgreSQL firewall rules using Azure CLI</span></span>
<span data-ttu-id="b4c09-104">Pravidla brány firewall na úrovni serveru umožňují správcům řídit přístup k databázi Azure pro PostgreSQL Server z konkrétní IP adresu nebo rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="b4c09-104">Server-level firewall rules enable administrators to manage access to an Azure Database for PostgreSQL Server from a specific IP address or range of IP addresses.</span></span> <span data-ttu-id="b4c09-105">Pomocí vhodného rozhraní příkazového řádku Azure, můžete vytvořit, aktualizovat, odstranit, seznamu a zobrazit pravidla brány firewall ke správě serveru.</span><span class="sxs-lookup"><span data-stu-id="b4c09-105">Using convenient Azure CLI commands, you can create, update, delete, list, and show firewall rules to manage your server.</span></span> <span data-ttu-id="b4c09-106">Přehled Azure databáze pro PostgreSQL brány firewall, najdete v tématu [databáze Azure pro pravidla brány firewall serveru PostgreSQL](concepts-firewall-rules.md)</span><span class="sxs-lookup"><span data-stu-id="b4c09-106">For an overview of Azure Database for PostgreSQL firewalls, see [Azure Database for PostgreSQL Server firewall rules](concepts-firewall-rules.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b4c09-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b4c09-107">Prerequisites</span></span>
<span data-ttu-id="b4c09-108">Chcete-li krok tímto průvodcem postupy, je třeba:</span><span class="sxs-lookup"><span data-stu-id="b4c09-108">To step through this how-to guide, you need:</span></span>
- <span data-ttu-id="b4c09-109">[Databáze Azure pro PostgreSQL server a databáze](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="b4c09-109">An [Azure Database for PostgreSQL server and database](quickstart-create-server-database-azure-cli.md)</span></span>
- <span data-ttu-id="b4c09-110">Nainstalujte [Azure CLI 2.0](/cli/azure/install-azure-cli) příkaz nástroj řádku nebo pomocí prostředí cloudové služby Azure v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="b4c09-110">Install [Azure CLI 2.0](/cli/azure/install-azure-cli) command line utility or use the Azure Cloud Shell in the browser.</span></span>

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a><span data-ttu-id="b4c09-111">Konfigurace pravidel brány firewall pro databázi Azure pro PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="b4c09-111">Configure firewall rules for Azure Database for PostgreSQL</span></span>
<span data-ttu-id="b4c09-112">[Az postgres pravidla brány firewall-](/cli/azure/postgres/server/firewall-rule) příkazy se používají ke konfiguraci pravidel brány firewall.</span><span class="sxs-lookup"><span data-stu-id="b4c09-112">The [az postgres server firewall-rule](/cli/azure/postgres/server/firewall-rule) commands are used to configure firewall rules.</span></span>

## <a name="list-firewall-rules"></a><span data-ttu-id="b4c09-113">Seznam pravidel brány firewall</span><span class="sxs-lookup"><span data-stu-id="b4c09-113">List firewall rules</span></span> 
<span data-ttu-id="b4c09-114">Pro zobrazení seznamu existující pravidla brány firewall serveru na serveru, spusťte [seznamu pravidlo brány firewall serveru postgres az](/cli/azure/postgres/server/firewall-rule#list) příkaz.</span><span class="sxs-lookup"><span data-stu-id="b4c09-114">To list the existing server firewall rules on the server, run the [az postgres server firewall-rule list](/cli/azure/postgres/server/firewall-rule#list) command.</span></span>
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401
```
<span data-ttu-id="b4c09-115">Výstup obsahuje pravidla, pokud existuje, ve výchozím nastavení ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="b4c09-115">The output lists the rules if any, by default in JSON format.</span></span> <span data-ttu-id="b4c09-116">Můžete použít přepínač `--output table` pro přehlednějším tvaru tabulky jako výstup.</span><span class="sxs-lookup"><span data-stu-id="b4c09-116">You may use the switch `--output table` for a more readable table format as the output.</span></span>
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401 --output table
```
## <a name="create-firewall-rule"></a><span data-ttu-id="b4c09-117">Vytvořte pravidlo brány firewall</span><span class="sxs-lookup"><span data-stu-id="b4c09-117">Create firewall rule</span></span>
<span data-ttu-id="b4c09-118">Chcete-li vytvořit nové pravidlo brány firewall na serveru, spusťte [az postgres pravidla brány firewall-vytvořit](/cli/azure/postgres/server/firewall-rule#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="b4c09-118">To create a new firewall rule on the server, run the [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) command.</span></span> 

<span data-ttu-id="b4c09-119">Tento příklad umožňuje řadu všechny IP adresy pro přístup k serveru **mypgserver 20170401.postgres.database.azure.com**</span><span class="sxs-lookup"><span data-stu-id="b4c09-119">This example allows a range of all IP addresses to access the server **mypgserver-20170401.postgres.database.azure.com**</span></span>
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup  --server mypgserver-20170401 --name "AllowIpRange" --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
<span data-ttu-id="b4c09-120">Povolit singulární IP adresa pro přístup, zadejte stejnou adresu jako počáteční IP a koncové IP adresy, jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="b4c09-120">To allow a singular IP address to access, provide the same address as the Start IP and End IP, as in this example.</span></span>
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup  
--server mypgserver-20170401 --name "AllowSingleIpAddress" --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
<span data-ttu-id="b4c09-121">Po úspěšné výstupu příkazu jsou uvedeny podrobnosti o pravidlo brány firewall, které jste vytvořili, ve výchozím nastavení ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="b4c09-121">Upon success, the command output lists the details of the firewall rule you have created, by default in JSON format.</span></span> <span data-ttu-id="b4c09-122">Pokud dojde k selhání, text zprávy showserror výstup.</span><span class="sxs-lookup"><span data-stu-id="b4c09-122">If there is a failure, the output showserror message text instead.</span></span>

## <a name="update-firewall-rule"></a><span data-ttu-id="b4c09-123">Aktualizovat pravidla brány firewall</span><span class="sxs-lookup"><span data-stu-id="b4c09-123">Update firewall rule</span></span> 
<span data-ttu-id="b4c09-124">Aktualizovat existující pravidlo brány firewall na serveru pomocí [aktualizace pravidlo brány firewall serveru postgres az](/cli/azure/postgres/server/firewall-rule#update) příkaz.</span><span class="sxs-lookup"><span data-stu-id="b4c09-124">Update an existing firewall rule on the server using [az postgres server firewall-rule update](/cli/azure/postgres/server/firewall-rule#update) command.</span></span> <span data-ttu-id="b4c09-125">Zadejte název existující pravidla brány firewall jako vstup a spusťte IP adresy a koncové IP atributů k aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="b4c09-125">Provide the name of the existing firewall rule as input, and the start IP and end IP attributes to update.</span></span>
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.255
```
<span data-ttu-id="b4c09-126">Po úspěšné výstupu příkazu jsou uvedeny podrobnosti o pravidlo brány firewall, které jste aktualizovali, ve výchozím nastavení ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="b4c09-126">Upon success, the command output lists the details of the firewall rule you have updated, by default in JSON format.</span></span> <span data-ttu-id="b4c09-127">Pokud dojde k selhání, text zprávy showserror výstup.</span><span class="sxs-lookup"><span data-stu-id="b4c09-127">If there is a failure, the output showserror message text instead.</span></span>
> [!NOTE]
> <span data-ttu-id="b4c09-128">Pokud pravidlo brány firewall neexistuje, získá vytvořené pomocí příkazu update.</span><span class="sxs-lookup"><span data-stu-id="b4c09-128">If the firewall rule does not exist, it gets created by the update command.</span></span>

## <a name="show-firewall-rule-details"></a><span data-ttu-id="b4c09-129">Zobrazit podrobnosti o pravidlech brány firewall</span><span class="sxs-lookup"><span data-stu-id="b4c09-129">Show firewall rule details</span></span>
<span data-ttu-id="b4c09-130">Můžete také zobrazit existující brány firewall pravidla Podrobnosti serveru spuštěním [zobrazit pravidlo brány firewall serveru postgres az](/cli/azure/postgres/server/firewall-rule#show) příkaz.</span><span class="sxs-lookup"><span data-stu-id="b4c09-130">You can also show the existing firewall rule details for a server by running [az postgres server firewall-rule show](/cli/azure/postgres/server/firewall-rule#show) command.</span></span>
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange"
```
<span data-ttu-id="b4c09-131">Po úspěšné výstupu příkazu jsou uvedeny podrobnosti o pravidlo brány firewall, které jste zadali, ve výchozím nastavení ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="b4c09-131">Upon success, the command output lists the details of the firewall rule you have specified, by default in JSON format.</span></span> <span data-ttu-id="b4c09-132">Pokud dojde k selhání, text zprávy showserror výstup.</span><span class="sxs-lookup"><span data-stu-id="b4c09-132">If there is a failure, the output showserror message text instead.</span></span>

## <a name="delete-firewall-rule"></a><span data-ttu-id="b4c09-133">Odstranit pravidlo brány firewall</span><span class="sxs-lookup"><span data-stu-id="b4c09-133">Delete firewall rule</span></span>
<span data-ttu-id="b4c09-134">K odvolání přístupu pro rozsah adres IP ze serveru, odstraňte existující pravidlo brány firewall tak, že spustíte [odstranit az postgres pravidla brány firewall-](/cli/azure/postgres/server/firewall-rule#delete) příkaz.</span><span class="sxs-lookup"><span data-stu-id="b4c09-134">To revoke access for an IP range from the server, delete an existing firewall rule by executing the [az postgres server firewall-rule delete](/cli/azure/postgres/server/firewall-rule#delete) command.</span></span> <span data-ttu-id="b4c09-135">Zadejte název existující pravidlo brány firewall.</span><span class="sxs-lookup"><span data-stu-id="b4c09-135">Provide the name of the existing firewall rule.</span></span>
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange"
```
<span data-ttu-id="b4c09-136">Po úspěšné neexistuje žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="b4c09-136">Upon success, there is no output.</span></span> <span data-ttu-id="b4c09-137">Při selhání je vrácen text chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="b4c09-137">Upon failure, the error message text is returned.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b4c09-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b4c09-138">Next steps</span></span>
- <span data-ttu-id="b4c09-139">Podobně můžete použít webový prohlížeč, aby [vytvořit a spravovat databáze Azure pro pravidla brány firewall PostgreSQL pomocí portálu Azure](howto-manage-firewall-using-portal.md)</span><span class="sxs-lookup"><span data-stu-id="b4c09-139">Similarly, you can use a web browser to [Create and manage Azure Database for PostgreSQL firewall rules using the Azure portal](howto-manage-firewall-using-portal.md)</span></span>
- <span data-ttu-id="b4c09-140">Lépe porozumět o [databáze Azure pro pravidla brány firewall serveru PostgreSQL](concepts-firewall-rules.md)</span><span class="sxs-lookup"><span data-stu-id="b4c09-140">Understand more about [Azure Database for PostgreSQL Server firewall rules](concepts-firewall-rules.md)</span></span>
- <span data-ttu-id="b4c09-141">Pomoc při připojování k databázi Azure pro PostgreSQL server najdete v tématu [knihovny připojení pro databázi Azure pro PostgreSQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="b4c09-141">For help in connecting to an Azure Database for PostgreSQL server, see [Connection libraries for Azure Database for PostgreSQL](concepts-connection-libraries.md)</span></span>
