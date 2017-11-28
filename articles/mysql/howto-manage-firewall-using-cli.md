---
title: "aaaCreate a správě Azure databáze MySQL pravidla brány firewall pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje, jak toocreate a správě Azure databáze MySQL pravidla brány firewall pomocí příkazového řádku Azure CLI."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: b2f0b4ccf34ce88e3a5e72a64d3f8c78a5bc2a54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-azure-cli"></a><span data-ttu-id="b5289-103">Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="b5289-103">Create and manage Azure Database for MySQL firewall rules using Azure CLI</span></span>
<span data-ttu-id="b5289-104">Pravidla brány firewall na úrovni serveru povolte správci toomanage přístup tooan Azure databáze MySQL serveru z konkrétní IP adresu nebo rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="b5289-104">Server-level firewall rules enable administrators toomanage access tooan Azure Database for MySQL Server from a specific IP address or range of IP addresses.</span></span> <span data-ttu-id="b5289-105">Pomocí vhodného rozhraní příkazového řádku Azure, můžete vytvořit, aktualizovat, odstranit, seznamu a zobrazit toomanage pravidla brány firewall serveru.</span><span class="sxs-lookup"><span data-stu-id="b5289-105">Using convenient Azure CLI commands, you can create, update, delete, list, and show firewall rules toomanage your server.</span></span> <span data-ttu-id="b5289-106">Přehled informací o Azure databáze MySQL brány firewall, najdete v článku [databáze Azure pro pravidla brány firewall serveru MySQL](./concepts-firewall-rules.md)</span><span class="sxs-lookup"><span data-stu-id="b5289-106">For an overview of Azure Database for MySQL firewalls, see [Azure Database for MySQL server firewall rules](./concepts-firewall-rules.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b5289-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b5289-107">Prerequisites</span></span>
* [<span data-ttu-id="b5289-108">Instalace rozhraní příkazového řádku Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="b5289-108">Install Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/install-azure-cli)
* <span data-ttu-id="b5289-109">Nainstalovat Azure Python SDK pro PostgreSQL a MySQL služby</span><span class="sxs-lookup"><span data-stu-id="b5289-109">Install Azure Python SDK for PostgreSQL and MySQL Services</span></span>
* <span data-ttu-id="b5289-110">Nainstalovat rozhraní příkazového řádku Azure součást hello služeb PostgreSQL a MySQL</span><span class="sxs-lookup"><span data-stu-id="b5289-110">Install hello Azure CLI component for PostgreSQL and MySQL services</span></span>
* <span data-ttu-id="b5289-111">Vytvoření serveru Azure Database for MySQL</span><span class="sxs-lookup"><span data-stu-id="b5289-111">Create an Azure Database for MySQL server</span></span>

## <a name="firewall-rule-commands"></a><span data-ttu-id="b5289-112">Pravidlo brány firewall příkazy:</span><span class="sxs-lookup"><span data-stu-id="b5289-112">Firewall-Rule Commands:</span></span>
<span data-ttu-id="b5289-113">Hello **az mysql pravidla brány firewall-** se používá příkaz z příkazového řádku Azure CLI toocreate, odstranit, seznam, zobrazit a aktualizovat pravidla brány firewall.</span><span class="sxs-lookup"><span data-stu-id="b5289-113">hello **az mysql server firewall-rule** command is used from Azure CLI toocreate, delete, list, show, and update firewall rules.</span></span>

<span data-ttu-id="b5289-114">Příkazy:</span><span class="sxs-lookup"><span data-stu-id="b5289-114">Commands:</span></span>
- <span data-ttu-id="b5289-115">**vytvoření**: vytvoření pravidla brány firewall serveru Azure MySQL.</span><span class="sxs-lookup"><span data-stu-id="b5289-115">**create**: Create an Azure MySQL server firewall rule.</span></span>
- <span data-ttu-id="b5289-116">**Odstranit**: odstranění pravidla brány firewall serveru Azure MySQL.</span><span class="sxs-lookup"><span data-stu-id="b5289-116">**delete**: Delete an Azure MySQL server firewall rule.</span></span>
- <span data-ttu-id="b5289-117">**seznam** : seznam pravidla brány firewall serveru Azure MySQL hello.</span><span class="sxs-lookup"><span data-stu-id="b5289-117">**list** : List hello Azure MySQL server firewall rules.</span></span>
- <span data-ttu-id="b5289-118">**Zobrazit** : Zobrazit podrobnosti hello serveru Azure MySQL pravidlo brány firewall.</span><span class="sxs-lookup"><span data-stu-id="b5289-118">**show** : Show hello details of an Azure MySQL server firewall rule.</span></span>
- <span data-ttu-id="b5289-119">**Aktualizovat**: aktualizovat pravidlo brány firewall serveru Azure MySQL.</span><span class="sxs-lookup"><span data-stu-id="b5289-119">**update**: Update an Azure MySQL server firewall rule.</span></span>

## <a name="login-tooazure-and-list-your-azure-database-for-mysql-servers"></a><span data-ttu-id="b5289-120">Seznam vaší databázi Azure pro servery, MySQL a tooAzure přihlášení</span><span class="sxs-lookup"><span data-stu-id="b5289-120">Login tooAzure and List your Azure Database for MySQL Servers</span></span>
<span data-ttu-id="b5289-121">Rozhraní příkazového řádku Azure bezpečně připojte s vaším účtem Azure.</span><span class="sxs-lookup"><span data-stu-id="b5289-121">Securely connect Azure CLI with your Azure account.</span></span> <span data-ttu-id="b5289-122">Použití hello **az přihlášení** příkaz toodo to.</span><span class="sxs-lookup"><span data-stu-id="b5289-122">Use hello **az login** command toodo this.</span></span>

1. <span data-ttu-id="b5289-123">Spusťte následující příkaz z příkazového řádku hello hello.</span><span class="sxs-lookup"><span data-stu-id="b5289-123">Run hello following command from hello command line.</span></span>
```azurecli
az login
```
<span data-ttu-id="b5289-124">Tento příkaz výstup toouse kódu v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="b5289-124">This command will output a code toouse in hello next step.</span></span>

2. <span data-ttu-id="b5289-125">Použijte stránku hello webového prohlížeče tooopen [https://aka.ms/devicelogin](https://aka.ms/devicelogin) a zadejte kód hello.</span><span class="sxs-lookup"><span data-stu-id="b5289-125">Use a web browser tooopen hello page [https://aka.ms/devicelogin](https://aka.ms/devicelogin) and enter hello code.</span></span>

3. <span data-ttu-id="b5289-126">Na příkazovém řádku hello Přihlaste se pomocí přihlašovacích údajů Azure.</span><span class="sxs-lookup"><span data-stu-id="b5289-126">At hello prompt, log in using your Azure credentials.</span></span>

4. <span data-ttu-id="b5289-127">Jakmile vaše přihlášení oprávnění, vytiskne se v konzole hello seznam předplatných.</span><span class="sxs-lookup"><span data-stu-id="b5289-127">Once your login is authorized, a list of subscriptions will be printed in hello console.</span></span> <span data-ttu-id="b5289-128">Zkopírujte id hello hello potřeby předplatné tooset hello aktuální předplatné toobe používá postoupíte.</span><span class="sxs-lookup"><span data-stu-id="b5289-128">Copy hello id of hello desired subscription tooset hello current subscription toobe used moving forward.</span></span>
   ```azurecli-interactive
   az account set --subscription {your subscription id}
   ```

5. <span data-ttu-id="b5289-129">Zobrazí seznam hello Azure databáze MySQL serverů pro vaše předplatné nebo skupině prostředků, pokud si nejste jistí názvů hello.</span><span class="sxs-lookup"><span data-stu-id="b5289-129">List hello Azure Databases for MySQL servers for your subscription and resource group if you are unsure of hello names.</span></span>

   ```azurecli-interactive
   az mysql server list --resource-group myResourceGroup
   ```

   <span data-ttu-id="b5289-130">Všimněte si hello name – atribut v hello výpis, který bude použité toospecify, které server toowork MySQL na.</span><span class="sxs-lookup"><span data-stu-id="b5289-130">Note hello name attribute in hello listing, which will be used toospecify which MySQL server toowork on.</span></span> <span data-ttu-id="b5289-131">V případě potřeby potvrďte hello podrobnosti pro tento server toousing hello atribut tooconfirm název není správný:</span><span class="sxs-lookup"><span data-stu-id="b5289-131">If needed, confirm hello details for that server toousing hello name attribute tooconfirm name is correct:</span></span>

   ```azurecli-interactive
   az mysql server show --resource-group myResourceGroup --name mysqlserver4demo
   ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a><span data-ttu-id="b5289-132">Seznam pravidel brány Firewall na Azure databáze MySQL Server pro</span><span class="sxs-lookup"><span data-stu-id="b5289-132">List Firewall Rules on Azure Database for MySQL Server</span></span> 
<span data-ttu-id="b5289-133">Pomocí hello název serveru a název skupiny prostředků hello, seznamu hello existující pravidla brány firewall serveru na hello server.</span><span class="sxs-lookup"><span data-stu-id="b5289-133">Using hello server name and hello resource group name, list hello existing server firewall rules on hello server.</span></span> <span data-ttu-id="b5289-134">Všimněte si, že atribut name server hello je uveden v hello **– server** přepínače a není hello **– název** přepínače.</span><span class="sxs-lookup"><span data-stu-id="b5289-134">Notice that hello server name attribute is specified in hello **--server** switch and not hello **--name** switch.</span></span>
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo
```
<span data-ttu-id="b5289-135">výstup Hello zobrazí seznam pravidel hello, pokud existuje, ve výchozím nastavení ve formátu JSON formátu.</span><span class="sxs-lookup"><span data-stu-id="b5289-135">hello output will list hello rules if any, by default in JSON format.</span></span> <span data-ttu-id="b5289-136">Můžete použít přepínač hello **– výstupní tabulku** pro přehlednějším tvaru tabulky jako výstup hello.</span><span class="sxs-lookup"><span data-stu-id="b5289-136">You may use hello switch **--output table** for a more readable table format as hello output.</span></span>
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo --output table
```
## <a name="create-firewall-rule-on-azure-database-for-mysql-server"></a><span data-ttu-id="b5289-137">Vytvořte pravidlo brány Firewall pro Server databáze MySQL na Azure databáze</span><span class="sxs-lookup"><span data-stu-id="b5289-137">Create Firewall Rule on Azure Database for MySQL Server</span></span>
<span data-ttu-id="b5289-138">Pomocí hello Azure MySQL název serveru a název skupiny prostředků hello, vytvořte nové pravidlo brány firewall na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="b5289-138">Using hello Azure MySQL server name and hello resource group name, create a new firewall rule on hello server.</span></span> <span data-ttu-id="b5289-139">Zadejte název pravidla hello hello počáteční IP adresa a koncová IP adresa pro pravidlo toocover hello tooallow přístup k rozsahu IP adres.</span><span class="sxs-lookup"><span data-stu-id="b5289-139">Provide a name for hello rule, hello start IP, and end IP for hello rule toocover a range of IP addresses tooallow access.</span></span>
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```
<span data-ttu-id="b5289-140">Pro singulární IP adres toobe povolený přístup, zadejte text hello stejnou adresu, protože hello počáteční IP a koncové IP adresy, jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="b5289-140">For a singular IP address toobe allowed access, provide hello same address as hello Start IP and End IP, as in this example.</span></span>
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  
--server mysql --name "Firewall Rule with a Single Address" --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```
<span data-ttu-id="b5289-141">Po úspěšné zobrazí výstup příkazu hello seznam hello podrobnosti o hello pravidlo brány firewall, které jste vytvořili, ve výchozím nastavení ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="b5289-141">Upon success, hello command output will list hello details of hello firewall rule you have created, by default in JSON format.</span></span> <span data-ttu-id="b5289-142">Pokud dojde k selhání, výstup hello zobrazí text chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="b5289-142">If there is a failure, hello output will show error message text instead.</span></span>

## <a name="update-firewall-rule-on-azure-database-for-mysql-server"></a><span data-ttu-id="b5289-143">Aktualizovat pravidla brány Firewall v databázi Azure pro server databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="b5289-143">Update Firewall Rule on Azure Database for MySQL server</span></span> 
<span data-ttu-id="b5289-144">Pomocí hello Azure MySQL název serveru a název skupiny prostředků hello, aktualizovat existující pravidlo brány firewall na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="b5289-144">Using hello Azure MySQL server name and hello resource group name, update an existing firewall rule on hello server.</span></span> <span data-ttu-id="b5289-145">Zadejte název hello hello existující pravidla brány firewall jako vstup a hello počáteční IP adresy a koncové IP atributy tooupdate.</span><span class="sxs-lookup"><span data-stu-id="b5289-145">Provide hello name of hello existing firewall rule as input, and hello start IP and end IP attributes tooupdate.</span></span>
```azurecli-interactive
az mysql server firewall-rule update --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
<span data-ttu-id="b5289-146">Po úspěšné zobrazí výstup příkazu hello seznam hello podrobnosti hello pravidla brány firewall, které jste aktualizovali, ve výchozím nastavení ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="b5289-146">Upon success, hello command output will list hello details of hello firewall rule you have updated, by default in JSON format.</span></span> <span data-ttu-id="b5289-147">Pokud dojde k selhání, výstup hello zobrazí text chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="b5289-147">If there is a failure, hello output will show error message text instead.</span></span>

> [!NOTE]
> <span data-ttu-id="b5289-148">Pokud pravidlo brány firewall hello neexistuje, vytvoří se pomocí příkazu update hello.</span><span class="sxs-lookup"><span data-stu-id="b5289-148">If hello firewall rule does not exist, it will be created by hello update command.</span></span>

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a><span data-ttu-id="b5289-149">Zobrazit podrobnosti pravidla brány Firewall pro Server databáze MySQL na Azure databáze</span><span class="sxs-lookup"><span data-stu-id="b5289-149">Show Firewall Rule Details on Azure Database for MySQL Server</span></span>
<span data-ttu-id="b5289-150">Pomocí názvu serveru Azure MySQL hello a název skupiny prostředků hello, zobrazit podrobnosti pro pravidlo existující brány firewall ze hello ze serveru hello.</span><span class="sxs-lookup"><span data-stu-id="b5289-150">Using hello Azure MySQL server name and hello resource group name, show hello existing firewall rule details from hello server.</span></span> <span data-ttu-id="b5289-151">Zadejte název existujícího pravidla firewallu hello hello jako vstup.</span><span class="sxs-lookup"><span data-stu-id="b5289-151">Provide hello name of hello existing firewall rule as input.</span></span>
```azurecli-interactive
az mysql server firewall-rule show --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
<span data-ttu-id="b5289-152">Po úspěšné zobrazí výstup příkazu hello seznam hello podrobnosti o hello pravidlo brány firewall, která jste zadali, ve výchozím nastavení ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="b5289-152">Upon success, hello command output will list hello details of hello firewall rule you have specified, by default in JSON format.</span></span> <span data-ttu-id="b5289-153">Pokud dojde k selhání, výstup hello zobrazí text chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="b5289-153">If there is a failure, hello output will show error message text instead.</span></span>

## <a name="delete-firewall-rule-on-azure-database-for-mysql-server"></a><span data-ttu-id="b5289-154">Odstranit pravidlo brány Firewall databáze Azure pro Server databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="b5289-154">Delete Firewall Rule on Azure Database for MySQL Server</span></span>
<span data-ttu-id="b5289-155">Pomocí hello Azure MySQL název serveru a název skupiny prostředků hello, odeberte existující pravidlo brány firewall ze serveru hello.</span><span class="sxs-lookup"><span data-stu-id="b5289-155">Using hello Azure MySQL server name and hello resource group name, remove an existing firewall rule from hello server.</span></span> <span data-ttu-id="b5289-156">Zadejte název hello hello existující pravidla brány firewall.</span><span class="sxs-lookup"><span data-stu-id="b5289-156">Provide hello name of hello existing firewall rule.</span></span>
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
<span data-ttu-id="b5289-157">Po úspěšné neexistuje žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="b5289-157">Upon success, there is no output.</span></span> <span data-ttu-id="b5289-158">Při selhání bude vrácen text hello chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="b5289-158">Upon failure, hello error message text will be returned.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b5289-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b5289-159">Next steps</span></span>
- <span data-ttu-id="b5289-160">Lépe porozumět o [databáze Azure pro pravidla brány firewall serveru MySQL](./concepts-firewall-rules.md)</span><span class="sxs-lookup"><span data-stu-id="b5289-160">Understand more about [Azure Database for MySQL Server firewall rules](./concepts-firewall-rules.md)</span></span>
- [<span data-ttu-id="b5289-161">Vytvářet a spravovat databáze Azure pro pravidla brány firewall MySQL pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b5289-161">Create and manage Azure Database for MySQL firewall rules using hello Azure portal</span></span>](./howto-manage-firewall-using-portal.md)
