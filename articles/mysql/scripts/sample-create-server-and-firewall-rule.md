---
title: "Rozhraní příkazového řádku Azure Script – vytvoření Azure databáze pro databázi MySQL | Microsoft Docs"
description: "Tento ukázkový skript rozhraní příkazového řádku vytvoří databázi Azure pro server databáze MySQL a nakonfiguruje pravidla brány firewall na úrovni serveru."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.custom: mvc
ms.topic: sample
ms.date: 05/31/2017
ms.openlocfilehash: 201db294ce362ef3e09cbe62f48bd51c8ea94dbb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-mysql-server-and-configure-a-firewall-rule-using-the-azure-cli"></a><span data-ttu-id="31518-103">Vytvoření databáze MySQL serveru a nakonfigurujte pravidlo brány firewall pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="31518-103">Create a MySQL server and configure a firewall rule using the Azure CLI</span></span>
<span data-ttu-id="31518-104">Tento ukázkový skript rozhraní příkazového řádku vytvoří databázi Azure pro server databáze MySQL a nakonfiguruje pravidla brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="31518-104">This sample CLI script creates an Azure Database for MySQL server and configures a server-level firewall rule.</span></span> <span data-ttu-id="31518-105">Jakmile se skript spustí úspěšně, je přístupné všech služeb Azure a nakonfigurovanou IP adresu serveru MySQL.</span><span class="sxs-lookup"><span data-stu-id="31518-105">Once the script runs successfully, the MySQL server is accessible by all Azure services and the configured IP address.</span></span>

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="31518-106">Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="31518-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="31518-107">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="31518-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="31518-108">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="31518-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="31518-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="31518-109">Sample script</span></span>
<span data-ttu-id="31518-110">V tento ukázkový skript upravte zvýrazněné řádky k přizpůsobení uživatelské jméno správce a heslo.</span><span class="sxs-lookup"><span data-stu-id="31518-110">In this sample script, edit the highlighted lines to customize the admin username and password.</span></span>
<span data-ttu-id="31518-111">[!code-azurecli-interactive[hlavní](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/create-mysql-server-and-firewall-rule.sh?highlight=15-16 "vytvoření Azure databáze MySQL a pravidlo brány firewall na úrovni serveru.")]</span><span class="sxs-lookup"><span data-stu-id="31518-111">[!code-azurecli-interactive[main](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/create-mysql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for MySQL, and server-level firewall rule.")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="31518-112">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="31518-112">Clean up deployment</span></span>
<span data-ttu-id="31518-113">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="31518-113">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>
<span data-ttu-id="31518-114">[!code-azurecli-interactive[hlavní](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/delete-mysql.sh "Odstraňte skupinu prostředků.")]</span><span class="sxs-lookup"><span data-stu-id="31518-114">[!code-azurecli-interactive[main](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/delete-mysql.sh "Delete the resource group.")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="31518-115">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="31518-115">Script explanation</span></span>
<span data-ttu-id="31518-116">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="31518-116">This script uses the following commands.</span></span> <span data-ttu-id="31518-117">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="31518-117">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="31518-118">**Příkaz**</span><span class="sxs-lookup"><span data-stu-id="31518-118">**Command**</span></span> | <span data-ttu-id="31518-119">**Poznámky k**</span><span class="sxs-lookup"><span data-stu-id="31518-119">**Notes**</span></span> |
|---|---|
| [<span data-ttu-id="31518-120">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="31518-120">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="31518-121">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="31518-121">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="31518-122">server databáze mysql az vytvořit</span><span class="sxs-lookup"><span data-stu-id="31518-122">az mysql server create</span></span>](/cli/azure/mysql/server#create) | <span data-ttu-id="31518-123">Vytvoří MySQL serveru, který je hostitelem databáze.</span><span class="sxs-lookup"><span data-stu-id="31518-123">Creates a MySQL server that hosts the databases.</span></span> |
| [<span data-ttu-id="31518-124">Vytvoření brány firewall serveru az mysql</span><span class="sxs-lookup"><span data-stu-id="31518-124">az mysql server firewall create</span></span>](/cli/azure/mysql/server/firewall-rule#create) | <span data-ttu-id="31518-125">Vytvoří pravidlo brány firewall pro povolení přístupu k serveru a databází v něm ze zadané rozsahu IP adres.</span><span class="sxs-lookup"><span data-stu-id="31518-125">Creates a firewall rule to allow access to the server and databases under it from the entered IP address range.</span></span> |
| [<span data-ttu-id="31518-126">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="31518-126">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="31518-127">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="31518-127">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="31518-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31518-128">Next steps</span></span>
- <span data-ttu-id="31518-129">Přečtěte si další informace o Azure CLI: [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="31518-129">Read more information on the Azure CLI: [Azure CLI documentation](/cli/azure/overview).</span></span>
- <span data-ttu-id="31518-130">Zkuste další skripty: [ukázky rozhraní příkazového řádku Azure pro databázi Azure pro databázi MySQL](../sample-scripts-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="31518-130">Try additional scripts: [Azure CLI samples for Azure Database for MySQL](../sample-scripts-azure-cli.md)</span></span>
