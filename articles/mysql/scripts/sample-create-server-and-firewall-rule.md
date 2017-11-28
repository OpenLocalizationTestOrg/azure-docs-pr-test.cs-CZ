---
title: "AAA \"Azure CLI Script – vytvoření Azure databáze pro databázi MySQL | Microsoft Docs\""
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
ms.openlocfilehash: 1d619ee0547efd8275eaf7c1347b6c3427025c3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-mysql-server-and-configure-a-firewall-rule-using-hello-azure-cli"></a><span data-ttu-id="bce4b-103">Vytvoření databáze MySQL serveru a nakonfigurujte pravidlo brány firewall pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="bce4b-103">Create a MySQL server and configure a firewall rule using hello Azure CLI</span></span>
<span data-ttu-id="bce4b-104">Tento ukázkový skript rozhraní příkazového řádku vytvoří databázi Azure pro server databáze MySQL a nakonfiguruje pravidla brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="bce4b-104">This sample CLI script creates an Azure Database for MySQL server and configures a server-level firewall rule.</span></span> <span data-ttu-id="bce4b-105">Po úspěšném spuštění skriptu hello hello MySQL server je přístupný pro všechny služby Azure a hello nakonfigurovat IP adresu.</span><span class="sxs-lookup"><span data-stu-id="bce4b-105">Once hello script runs successfully, hello MySQL server is accessible by all Azure services and hello configured IP address.</span></span>

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="bce4b-106">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="bce4b-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="bce4b-107">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="bce4b-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="bce4b-108">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="bce4b-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="bce4b-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="bce4b-109">Sample script</span></span>
<span data-ttu-id="bce4b-110">V tento ukázkový skript upravte hello zvýrazněná řádky toocustomize hello jméno a heslo správce.</span><span class="sxs-lookup"><span data-stu-id="bce4b-110">In this sample script, edit hello highlighted lines toocustomize hello admin username and password.</span></span>
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/create-mysql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for MySQL, and server-level firewall rule.")]

## <a name="clean-up-deployment"></a><span data-ttu-id="bce4b-111">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="bce4b-111">Clean up deployment</span></span>
<span data-ttu-id="bce4b-112">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="bce4b-112">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/delete-mysql.sh "Delete hello resource group.")]

## <a name="script-explanation"></a><span data-ttu-id="bce4b-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="bce4b-113">Script explanation</span></span>
<span data-ttu-id="bce4b-114">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="bce4b-114">This script uses hello following commands.</span></span> <span data-ttu-id="bce4b-115">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="bce4b-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="bce4b-116">**Příkaz**</span><span class="sxs-lookup"><span data-stu-id="bce4b-116">**Command**</span></span> | <span data-ttu-id="bce4b-117">**Poznámky k**</span><span class="sxs-lookup"><span data-stu-id="bce4b-117">**Notes**</span></span> |
|---|---|
| [<span data-ttu-id="bce4b-118">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="bce4b-118">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="bce4b-119">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="bce4b-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="bce4b-120">server databáze mysql az vytvořit</span><span class="sxs-lookup"><span data-stu-id="bce4b-120">az mysql server create</span></span>](/cli/azure/mysql/server#create) | <span data-ttu-id="bce4b-121">Vytvoří MySQL serveru, který je hostitelem databáze hello.</span><span class="sxs-lookup"><span data-stu-id="bce4b-121">Creates a MySQL server that hosts hello databases.</span></span> |
| [<span data-ttu-id="bce4b-122">Vytvoření brány firewall serveru az mysql</span><span class="sxs-lookup"><span data-stu-id="bce4b-122">az mysql server firewall create</span></span>](/cli/azure/mysql/server/firewall-rule#create) | <span data-ttu-id="bce4b-123">Vytvoří server toohello přístup tooallow pravidlo brány firewall a databáze v něm z hello zadaný rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="bce4b-123">Creates a firewall rule tooallow access toohello server and databases under it from hello entered IP address range.</span></span> |
| [<span data-ttu-id="bce4b-124">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="bce4b-124">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="bce4b-125">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="bce4b-125">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="bce4b-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bce4b-126">Next steps</span></span>
- <span data-ttu-id="bce4b-127">Přečtěte si další informace o hello příkazového řádku Azure CLI: [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bce4b-127">Read more information on hello Azure CLI: [Azure CLI documentation](/cli/azure/overview).</span></span>
- <span data-ttu-id="bce4b-128">Zkuste další skripty: [ukázky rozhraní příkazového řádku Azure pro databázi Azure pro databázi MySQL](../sample-scripts-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="bce4b-128">Try additional scripts: [Azure CLI samples for Azure Database for MySQL](../sample-scripts-azure-cli.md)</span></span>
