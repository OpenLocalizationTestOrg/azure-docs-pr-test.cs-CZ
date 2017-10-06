---
title: "AAA \"Azure CLI Script – vytvoření Azure databáze pro PostgreSQL | Microsoft Docs\""
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
ms.openlocfilehash: bbe31f77283aa4a3bb36d1fd9171c280594a8267
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-database-for-postgresql-server-and-configure-a-firewall-rule-using-hello-azure-cli"></a><span data-ttu-id="0fe4b-103">Vytvoření databáze Azure pro PostgreSQL server a nakonfigurujte pravidlo brány firewall pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="0fe4b-103">Create an Azure Database for PostgreSQL server and configure a firewall rule using hello Azure CLI</span></span>
<span data-ttu-id="0fe4b-104">Tento ukázkový skript rozhraní příkazového řádku vytvoří databázi Azure pro PostgreSQL server a nakonfiguruje pravidla brány firewall na úrovni serveru.</span><span class="sxs-lookup"><span data-stu-id="0fe4b-104">This sample CLI script creates an Azure Database for PostgreSQL server and configures a server-level firewall rule.</span></span> <span data-ttu-id="0fe4b-105">Jakmile hello skript byl úspěšně spuštěn, hello PostgreSQL server je přístupná ze všech služeb Azure a hello nakonfigurovat IP adresu.</span><span class="sxs-lookup"><span data-stu-id="0fe4b-105">Once hello script has been successfully run, hello PostgreSQL server can be accessed from all Azure services and hello configured IP address.</span></span>

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="0fe4b-106">Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="0fe4b-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="0fe4b-107">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="0fe4b-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="0fe4b-108">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0fe4b-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="0fe4b-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="0fe4b-109">Sample script</span></span>
<span data-ttu-id="0fe4b-110">V tento ukázkový skript upravte hello zvýrazněná řádky toocustomize hello jméno a heslo správce.</span><span class="sxs-lookup"><span data-stu-id="0fe4b-110">In this sample script, edit hello highlighted lines toocustomize hello admin username and password.</span></span>
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/create-postgresql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for PostgreSQL, and server-level firewall rule.")]

## <a name="clean-up-deployment"></a><span data-ttu-id="0fe4b-111">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="0fe4b-111">Clean up deployment</span></span>
<span data-ttu-id="0fe4b-112">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello a všechny prostředky, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="0fe4b-112">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/delete-postgresql.sh "Delete hello resource group.")]

## <a name="script-explanation"></a><span data-ttu-id="0fe4b-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="0fe4b-113">Script explanation</span></span>
<span data-ttu-id="0fe4b-114">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="0fe4b-114">This script uses hello following commands.</span></span> <span data-ttu-id="0fe4b-115">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="0fe4b-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="0fe4b-116">**Příkaz**</span><span class="sxs-lookup"><span data-stu-id="0fe4b-116">**Command**</span></span> | <span data-ttu-id="0fe4b-117">**Poznámky k**</span><span class="sxs-lookup"><span data-stu-id="0fe4b-117">**Notes**</span></span> |
|---|---|
| [<span data-ttu-id="0fe4b-118">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="0fe4b-118">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="0fe4b-119">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="0fe4b-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="0fe4b-120">AZ postgres server vytvořit</span><span class="sxs-lookup"><span data-stu-id="0fe4b-120">az postgres server create</span></span>](/cli/azure/postgres/server#create) | <span data-ttu-id="0fe4b-121">Vytvoří PostgreSQL serveru, který je hostitelem databáze hello.</span><span class="sxs-lookup"><span data-stu-id="0fe4b-121">Creates a PostgreSQL server that hosts hello databases.</span></span> |
| [<span data-ttu-id="0fe4b-122">Vytvoření brány firewall serveru postgres az</span><span class="sxs-lookup"><span data-stu-id="0fe4b-122">az postgres server firewall create</span></span>](/cli/azure/postgres/server/firewall-rule#create) | <span data-ttu-id="0fe4b-123">Vytvoří server toohello přístup tooallow pravidlo brány firewall a databáze v něm z hello zadaný rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="0fe4b-123">Creates a firewall rule tooallow access toohello server and databases under it from hello entered IP address range.</span></span> |
| [<span data-ttu-id="0fe4b-124">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="0fe4b-124">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="0fe4b-125">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="0fe4b-125">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="0fe4b-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0fe4b-126">Next steps</span></span>
- <span data-ttu-id="0fe4b-127">Přečtěte si další informace o hello příkazového řádku Azure CLI: [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="0fe4b-127">Read more information on hello Azure CLI: [Azure CLI documentation](/cli/azure/overview)</span></span>
- <span data-ttu-id="0fe4b-128">Zkuste další skripty: [ukázky rozhraní příkazového řádku Azure pro databázi Azure pro PostgreSQL](../sample-scripts-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="0fe4b-128">Try additional scripts: [Azure CLI samples for Azure Database for PostgreSQL](../sample-scripts-azure-cli.md)</span></span>
