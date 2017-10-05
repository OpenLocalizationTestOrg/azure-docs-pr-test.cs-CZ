---
title: "Rozhraní příkazového řádku Azure Script – vytvoření Azure databáze pro PostgreSQL | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - vytvoří databázi Azure pro PostgreSQL server a nakonfiguruje pravidla brány firewall na úrovni serveru."
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: azure-cli
ms.topic: sample
ms.date: 05/31/2017
ms.openlocfilehash: e545b568cd57fdcf28ab33a5ebfa34a495111c7f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-azure-database-for-postgresql-server-and-configure-a-firewall-rule-using-the-azure-cli"></a><span data-ttu-id="7faca-103">Vytvoření databáze Azure pro PostgreSQL server a nakonfigurujte pravidlo brány firewall pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="7faca-103">Create an Azure Database for PostgreSQL server and configure a firewall rule using the Azure CLI</span></span>
<span data-ttu-id="7faca-104">Tento ukázkový skript rozhraní příkazového řádku vytvoří databázi Azure pro PostgreSQL server a nakonfiguruje pravidla brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="7faca-104">This sample CLI script creates an Azure Database for PostgreSQL server and configures a server-level firewall rule.</span></span> <span data-ttu-id="7faca-105">Jakmile úspěšně spustit skript serveru PostgreSQL je přístupná ze všech služeb Azure a nakonfigurovanou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="7faca-105">Once the script has been successfully run, the PostgreSQL server can be accessed from all Azure services and the configured IP address.</span></span>

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="7faca-106">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="7faca-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="7faca-107">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="7faca-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="7faca-108">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7faca-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="7faca-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="7faca-109">Sample script</span></span>
<span data-ttu-id="7faca-110">V tento ukázkový skript upravte zvýrazněné řádky k přizpůsobení uživatelské jméno správce a heslo.</span><span class="sxs-lookup"><span data-stu-id="7faca-110">In this sample script, edit the highlighted lines to customize the admin username and password.</span></span>
<span data-ttu-id="7faca-111">[!code-azurecli-interactive[hlavní](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/create-postgresql-server-and-firewall-rule.sh?highlight=15-16 "vytvoření Azure databáze PostgreSQL a pravidlo brány firewall na úrovni serveru.")]</span><span class="sxs-lookup"><span data-stu-id="7faca-111">[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/create-postgresql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for PostgreSQL, and server-level firewall rule.")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="7faca-112">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="7faca-112">Clean up deployment</span></span>
<span data-ttu-id="7faca-113">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="7faca-113">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>
<span data-ttu-id="7faca-114">[!code-azurecli-interactive[hlavní](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/delete-postgresql.sh "Odstraňte skupinu prostředků.")]</span><span class="sxs-lookup"><span data-stu-id="7faca-114">[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/delete-postgresql.sh "Delete the resource group.")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="7faca-115">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="7faca-115">Script explanation</span></span>
<span data-ttu-id="7faca-116">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="7faca-116">This script uses the following commands.</span></span> <span data-ttu-id="7faca-117">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="7faca-117">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="7faca-118">**Příkaz**</span><span class="sxs-lookup"><span data-stu-id="7faca-118">**Command**</span></span> | <span data-ttu-id="7faca-119">**Poznámky k**</span><span class="sxs-lookup"><span data-stu-id="7faca-119">**Notes**</span></span> |
|---|---|
| [<span data-ttu-id="7faca-120">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="7faca-120">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="7faca-121">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="7faca-121">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="7faca-122">AZ postgres server vytvořit</span><span class="sxs-lookup"><span data-stu-id="7faca-122">az postgres server create</span></span>](/cli/azure/postgres/server#create) | <span data-ttu-id="7faca-123">Vytvoří PostgreSQL serveru, který je hostitelem databáze.</span><span class="sxs-lookup"><span data-stu-id="7faca-123">Creates a PostgreSQL server that hosts the databases.</span></span> |
| [<span data-ttu-id="7faca-124">Vytvoření brány firewall serveru postgres az</span><span class="sxs-lookup"><span data-stu-id="7faca-124">az postgres server firewall create</span></span>](/cli/azure/postgres/server/firewall-rule#create) | <span data-ttu-id="7faca-125">Vytvoří pravidlo brány firewall pro povolení přístupu k serveru a databází v něm ze zadané rozsahu IP adres.</span><span class="sxs-lookup"><span data-stu-id="7faca-125">Creates a firewall rule to allow access to the server and databases under it from the entered IP address range.</span></span> |
| [<span data-ttu-id="7faca-126">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="7faca-126">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="7faca-127">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="7faca-127">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7faca-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7faca-128">Next steps</span></span>
- <span data-ttu-id="7faca-129">Přečtěte si další informace o Azure CLI: [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="7faca-129">Read more information on the Azure CLI: [Azure CLI documentation](/cli/azure/overview)</span></span>
- <span data-ttu-id="7faca-130">Zkuste další skripty: [ukázky rozhraní příkazového řádku Azure pro databázi Azure pro PostgreSQL](../sample-scripts-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="7faca-130">Try additional scripts: [Azure CLI samples for Azure Database for PostgreSQL](../sample-scripts-azure-cli.md)</span></span>
