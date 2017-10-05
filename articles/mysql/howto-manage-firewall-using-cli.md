---
title: "Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje postup vytvoření a správě Azure databáze MySQL pravidla brány firewall pomocí příkazového řádku Azure CLI."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 9a03722e9f71be307bdbf0b846a4cbf7b34cd7ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-azure-cli"></a><span data-ttu-id="44c83-103">Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="44c83-103">Create and manage Azure Database for MySQL firewall rules using Azure CLI</span></span>
<span data-ttu-id="44c83-104">Pravidla brány firewall na úrovni serveru umožňují správcům řídit přístup k databázi Azure pro Server databáze MySQL z konkrétní IP adresu nebo rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="44c83-104">Server-level firewall rules enable administrators to manage access to an Azure Database for MySQL Server from a specific IP address or range of IP addresses.</span></span> <span data-ttu-id="44c83-105">Pomocí vhodného rozhraní příkazového řádku Azure, můžete vytvořit, aktualizovat, odstranit, seznamu a zobrazit pravidla brány firewall ke správě serveru.</span><span class="sxs-lookup"><span data-stu-id="44c83-105">Using convenient Azure CLI commands, you can create, update, delete, list, and show firewall rules to manage your server.</span></span> <span data-ttu-id="44c83-106">Přehled informací o Azure databáze MySQL brány firewall, najdete v článku [databáze Azure pro pravidla brány firewall serveru MySQL](./concepts-firewall-rules.md)</span><span class="sxs-lookup"><span data-stu-id="44c83-106">For an overview of Azure Database for MySQL firewalls, see [Azure Database for MySQL server firewall rules](./concepts-firewall-rules.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="44c83-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="44c83-107">Prerequisites</span></span>
* [<span data-ttu-id="44c83-108">Instalace rozhraní příkazového řádku Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="44c83-108">Install Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/install-azure-cli)
* <span data-ttu-id="44c83-109">Nainstalovat Azure Python SDK pro PostgreSQL a MySQL služby</span><span class="sxs-lookup"><span data-stu-id="44c83-109">Install Azure Python SDK for PostgreSQL and MySQL Services</span></span>
* <span data-ttu-id="44c83-110">Nainstalovat součást příkazového řádku Azure CLI pro PostgreSQL a databáze MySQL služby</span><span class="sxs-lookup"><span data-stu-id="44c83-110">Install the Azure CLI component for PostgreSQL and MySQL services</span></span>
* <span data-ttu-id="44c83-111">Vytvoření serveru Azure Database for MySQL</span><span class="sxs-lookup"><span data-stu-id="44c83-111">Create an Azure Database for MySQL server</span></span>

## <a name="firewall-rule-commands"></a><span data-ttu-id="44c83-112">Pravidlo brány firewall příkazy:</span><span class="sxs-lookup"><span data-stu-id="44c83-112">Firewall-Rule Commands:</span></span>
<span data-ttu-id="44c83-113">**Az mysql pravidla brány firewall-** příkaz z příkazového řádku Azure slouží k vytvoření, odstranění, seznam, zobrazit a aktualizovat pravidla brány firewall.</span><span class="sxs-lookup"><span data-stu-id="44c83-113">The **az mysql server firewall-rule** command is used from Azure CLI to create, delete, list, show, and update firewall rules.</span></span>

<span data-ttu-id="44c83-114">Příkazy:</span><span class="sxs-lookup"><span data-stu-id="44c83-114">Commands:</span></span>
- <span data-ttu-id="44c83-115">**vytvoření**: vytvoření pravidla brány firewall serveru Azure MySQL.</span><span class="sxs-lookup"><span data-stu-id="44c83-115">**create**: Create an Azure MySQL server firewall rule.</span></span>
- <span data-ttu-id="44c83-116">**Odstranit**: odstranění pravidla brány firewall serveru Azure MySQL.</span><span class="sxs-lookup"><span data-stu-id="44c83-116">**delete**: Delete an Azure MySQL server firewall rule.</span></span>
- <span data-ttu-id="44c83-117">**seznam** : seznam pravidla brány firewall serveru Azure MySQL.</span><span class="sxs-lookup"><span data-stu-id="44c83-117">**list** : List the Azure MySQL server firewall rules.</span></span>
- <span data-ttu-id="44c83-118">**Zobrazit** : Zobrazit podrobnosti o serveru databáze MySQL Azure pravidlo brány firewall.</span><span class="sxs-lookup"><span data-stu-id="44c83-118">**show** : Show the details of an Azure MySQL server firewall rule.</span></span>
- <span data-ttu-id="44c83-119">**Aktualizovat**: aktualizovat pravidlo brány firewall serveru Azure MySQL.</span><span class="sxs-lookup"><span data-stu-id="44c83-119">**update**: Update an Azure MySQL server firewall rule.</span></span>

## <a name="login-to-azure-and-list-your-azure-database-for-mysql-servers"></a><span data-ttu-id="44c83-120">Přihlášení k Azure a seznam Azure databáze MySQL servery</span><span class="sxs-lookup"><span data-stu-id="44c83-120">Login to Azure and List your Azure Database for MySQL Servers</span></span>
<span data-ttu-id="44c83-121">Rozhraní příkazového řádku Azure bezpečně připojte s vaším účtem Azure.</span><span class="sxs-lookup"><span data-stu-id="44c83-121">Securely connect Azure CLI with your Azure account.</span></span> <span data-ttu-id="44c83-122">Použití **az přihlášení** příkaz k tomu.</span><span class="sxs-lookup"><span data-stu-id="44c83-122">Use the **az login** command to do this.</span></span>

1. <span data-ttu-id="44c83-123">V příkazovém řádku spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="44c83-123">Run the following command from the command line.</span></span>
```azurecli
az login
```
<span data-ttu-id="44c83-124">Tento příkaz na výstupu zobrazí kód, který použijete v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="44c83-124">This command will output a code to use in the next step.</span></span>

2. <span data-ttu-id="44c83-125">Ve webovém prohlížeči otevřete stránku [https://aka.ms/devicelogin](https://aka.ms/devicelogin) a zadejte kód.</span><span class="sxs-lookup"><span data-stu-id="44c83-125">Use a web browser to open the page [https://aka.ms/devicelogin](https://aka.ms/devicelogin) and enter the code.</span></span>

3. <span data-ttu-id="44c83-126">V příkazovém řádku se přihlaste pomocí vašich přihlašovacích údajů Azure.</span><span class="sxs-lookup"><span data-stu-id="44c83-126">At the prompt, log in using your Azure credentials.</span></span>

4. <span data-ttu-id="44c83-127">Jakmile vaše přihlášení oprávnění, vytiskne se v konzole seznam předplatných.</span><span class="sxs-lookup"><span data-stu-id="44c83-127">Once your login is authorized, a list of subscriptions will be printed in the console.</span></span> <span data-ttu-id="44c83-128">Zkopírujte id požadované předplatné nastavte aktuální předplatné, které má být použit přesunutí dál.</span><span class="sxs-lookup"><span data-stu-id="44c83-128">Copy the id of the desired subscription to set the current subscription to be used moving forward.</span></span>
   ```azurecli-interactive
   az account set --subscription {your subscription id}
   ```

5. <span data-ttu-id="44c83-129">Zobrazí seznam Azure databáze MySQL serverů pro vaše předplatné nebo skupině prostředků, pokud si nejste jistí názvy.</span><span class="sxs-lookup"><span data-stu-id="44c83-129">List the Azure Databases for MySQL servers for your subscription and resource group if you are unsure of the names.</span></span>

   ```azurecli-interactive
   az mysql server list --resource-group myResourceGroup
   ```

   <span data-ttu-id="44c83-130">Poznámka: v seznamu, který se použije k určení které server MySQL pro práci na atribut názvu.</span><span class="sxs-lookup"><span data-stu-id="44c83-130">Note the name attribute in the listing, which will be used to specify which MySQL server to work on.</span></span> <span data-ttu-id="44c83-131">V případě potřeby potvrďte podrobnosti pro tento server k použití a ověřte, zda je název správný atribut názvu:</span><span class="sxs-lookup"><span data-stu-id="44c83-131">If needed, confirm the details for that server to using the name attribute to confirm name is correct:</span></span>

   ```azurecli-interactive
   az mysql server show --resource-group myResourceGroup --name mysqlserver4demo
   ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a><span data-ttu-id="44c83-132">Seznam pravidel brány Firewall na Azure databáze MySQL Server pro</span><span class="sxs-lookup"><span data-stu-id="44c83-132">List Firewall Rules on Azure Database for MySQL Server</span></span> 
<span data-ttu-id="44c83-133">Pomocí názvu serveru a název skupiny prostředků, seznamu existující pravidla brány firewall serveru na serveru.</span><span class="sxs-lookup"><span data-stu-id="44c83-133">Using the server name and the resource group name, list the existing server firewall rules on the server.</span></span> <span data-ttu-id="44c83-134">Všimněte si, že je atribut název serveru zadaný v **– server** přepínače a ne **– název** přepínače.</span><span class="sxs-lookup"><span data-stu-id="44c83-134">Notice that the server name attribute is specified in the **--server** switch and not the **--name** switch.</span></span>
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo
```
<span data-ttu-id="44c83-135">Výstup se zobrazí seznam pravidel, pokud existuje, ve výchozím nastavení ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="44c83-135">The output will list the rules if any, by default in JSON format.</span></span> <span data-ttu-id="44c83-136">Můžete použít přepínač **– výstupní tabulku** pro přehlednějším tvaru tabulky jako výstup.</span><span class="sxs-lookup"><span data-stu-id="44c83-136">You may use the switch **--output table** for a more readable table format as the output.</span></span>
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo --output table
```
## <a name="create-firewall-rule-on-azure-database-for-mysql-server"></a><span data-ttu-id="44c83-137">Vytvořte pravidlo brány Firewall pro Server databáze MySQL na Azure databáze</span><span class="sxs-lookup"><span data-stu-id="44c83-137">Create Firewall Rule on Azure Database for MySQL Server</span></span>
<span data-ttu-id="44c83-138">Pomocí Azure MySQL název serveru a název skupiny prostředků, vytvořte nové pravidlo brány firewall na serveru.</span><span class="sxs-lookup"><span data-stu-id="44c83-138">Using the Azure MySQL server name and the resource group name, create a new firewall rule on the server.</span></span> <span data-ttu-id="44c83-139">Zadejte název pro toto pravidlo, počáteční IP adresa a koncové IP adresu pro pravidlo tak, aby pokrývalo rozsah IP adres pro povolení přístupu.</span><span class="sxs-lookup"><span data-stu-id="44c83-139">Provide a name for the rule, the start IP, and end IP for the rule to cover a range of IP addresses to allow access.</span></span>
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```
<span data-ttu-id="44c83-140">Pro singulární IP adresu, bude přístup povolen zadejte stejnou adresu jako počáteční IP a koncové IP adresy, jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="44c83-140">For a singular IP address to be allowed access, provide the same address as the Start IP and End IP, as in this example.</span></span>
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  
--server mysql --name "Firewall Rule with a Single Address" --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```
<span data-ttu-id="44c83-141">Po úspěšné zobrazí seznam výstupu příkazu podrobnosti o pravidlo brány firewall, které jste vytvořili, ve výchozím nastavení ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="44c83-141">Upon success, the command output will list the details of the firewall rule you have created, by default in JSON format.</span></span> <span data-ttu-id="44c83-142">Pokud dojde k selhání, výstup zobrazí text chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="44c83-142">If there is a failure, the output will show error message text instead.</span></span>

## <a name="update-firewall-rule-on-azure-database-for-mysql-server"></a><span data-ttu-id="44c83-143">Aktualizovat pravidla brány Firewall v databázi Azure pro server databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="44c83-143">Update Firewall Rule on Azure Database for MySQL server</span></span> 
<span data-ttu-id="44c83-144">Pomocí Azure MySQL název serveru a název skupiny prostředků, aktualizujte existující pravidlo brány firewall na serveru.</span><span class="sxs-lookup"><span data-stu-id="44c83-144">Using the Azure MySQL server name and the resource group name, update an existing firewall rule on the server.</span></span> <span data-ttu-id="44c83-145">Zadejte název existující pravidla brány firewall jako vstup a spusťte IP adresy a koncové IP atributů k aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="44c83-145">Provide the name of the existing firewall rule as input, and the start IP and end IP attributes to update.</span></span>
```azurecli-interactive
az mysql server firewall-rule update --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
<span data-ttu-id="44c83-146">Po úspěšné zobrazí seznam výstupu příkazu podrobnosti o pravidlo brány firewall, které jste aktualizovali, ve výchozím nastavení ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="44c83-146">Upon success, the command output will list the details of the firewall rule you have updated, by default in JSON format.</span></span> <span data-ttu-id="44c83-147">Pokud dojde k selhání, výstup zobrazí text chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="44c83-147">If there is a failure, the output will show error message text instead.</span></span>

> [!NOTE]
> <span data-ttu-id="44c83-148">Pokud pravidlo brány firewall neexistuje, vytvoří se pomocí příkazu update.</span><span class="sxs-lookup"><span data-stu-id="44c83-148">If the firewall rule does not exist, it will be created by the update command.</span></span>

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a><span data-ttu-id="44c83-149">Zobrazit podrobnosti pravidla brány Firewall pro Server databáze MySQL na Azure databáze</span><span class="sxs-lookup"><span data-stu-id="44c83-149">Show Firewall Rule Details on Azure Database for MySQL Server</span></span>
<span data-ttu-id="44c83-150">Pomocí Azure MySQL název serveru a název skupiny prostředků, zobrazit existující brány firewall pravidla podrobnosti ze serveru.</span><span class="sxs-lookup"><span data-stu-id="44c83-150">Using the Azure MySQL server name and the resource group name, show the existing firewall rule details from the server.</span></span> <span data-ttu-id="44c83-151">Zadejte název existující pravidla brány firewall jako vstup.</span><span class="sxs-lookup"><span data-stu-id="44c83-151">Provide the name of the existing firewall rule as input.</span></span>
```azurecli-interactive
az mysql server firewall-rule show --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
<span data-ttu-id="44c83-152">Po úspěšné zobrazí seznam výstupu příkazu podrobnosti o pravidlo brány firewall, které jste zadali, ve výchozím nastavení ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="44c83-152">Upon success, the command output will list the details of the firewall rule you have specified, by default in JSON format.</span></span> <span data-ttu-id="44c83-153">Pokud dojde k selhání, výstup zobrazí text chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="44c83-153">If there is a failure, the output will show error message text instead.</span></span>

## <a name="delete-firewall-rule-on-azure-database-for-mysql-server"></a><span data-ttu-id="44c83-154">Odstranit pravidlo brány Firewall databáze Azure pro Server databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="44c83-154">Delete Firewall Rule on Azure Database for MySQL Server</span></span>
<span data-ttu-id="44c83-155">Pomocí Azure MySQL název serveru a název skupiny prostředků, odeberte existující pravidlo brány firewall ze serveru.</span><span class="sxs-lookup"><span data-stu-id="44c83-155">Using the Azure MySQL server name and the resource group name, remove an existing firewall rule from the server.</span></span> <span data-ttu-id="44c83-156">Zadejte název existující pravidlo brány firewall.</span><span class="sxs-lookup"><span data-stu-id="44c83-156">Provide the name of the existing firewall rule.</span></span>
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
<span data-ttu-id="44c83-157">Po úspěšné neexistuje žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="44c83-157">Upon success, there is no output.</span></span> <span data-ttu-id="44c83-158">Při selhání bude vrácen text chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="44c83-158">Upon failure, the error message text will be returned.</span></span>

## <a name="next-steps"></a><span data-ttu-id="44c83-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="44c83-159">Next steps</span></span>
- <span data-ttu-id="44c83-160">Lépe porozumět o [databáze Azure pro pravidla brány firewall serveru MySQL](./concepts-firewall-rules.md)</span><span class="sxs-lookup"><span data-stu-id="44c83-160">Understand more about [Azure Database for MySQL Server firewall rules](./concepts-firewall-rules.md)</span></span>
- [<span data-ttu-id="44c83-161">Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="44c83-161">Create and manage Azure Database for MySQL firewall rules using the Azure portal</span></span>](./howto-manage-firewall-using-portal.md)
