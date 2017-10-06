---
title: "aaaAzure ukázky rozhraní příkazového řádku pro Azure Cosmos DB | Microsoft Docs"
description: "Ukázek Azure CLI - vytvářet a spravovat účty Azure Cosmos DB, databáze, kontejnery, oblasti a brány firewall."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 06/07/2017
ms.author: mimig
ms.openlocfilehash: d6eefc3274e0b66eec4e69166bb7d4ddd58a522e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a><span data-ttu-id="9d802-103">Ukázek Azure CLI pro Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9d802-103">Azure CLI samples for Azure Cosmos DB</span></span>

<span data-ttu-id="9d802-104">Hello následující tabulka obsahuje odkazy toosample rozhraní příkazového řádku Azure skripty pro Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9d802-104">hello following table includes links toosample Azure CLI scripts for Azure Cosmos DB.</span></span> <span data-ttu-id="9d802-105">Referenční stránky pro všechny Cosmos DB rozhraní příkazového řádku Azure jsou k dispozici v hello [referenční informace o Azure CLI 2.0](https://docs.microsoft.com/cli/azure/cosmosdb).</span><span class="sxs-lookup"><span data-stu-id="9d802-105">Reference pages for all Azure Cosmos DB CLI commands are available in hello [Azure CLI 2.0 Reference](https://docs.microsoft.com/cli/azure/cosmosdb).</span></span>

| |  |
|---|---|
|<span data-ttu-id="9d802-106">**Vytvoření účtu Azure Cosmos DB, databáze a kontejnery**</span><span class="sxs-lookup"><span data-stu-id="9d802-106">**Create Azure Cosmos DB account, database, and containers**</span></span>||
|[<span data-ttu-id="9d802-107">Vytvoření účtu DocumentDB rozhraní API, grafu nebo tabulky API</span><span class="sxs-lookup"><span data-stu-id="9d802-107">Create a DocumentDB API, Graph, or Table API account</span></span>](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| <span data-ttu-id="9d802-108">Vytvoří jeden účet Azure Cosmos DB rozhraní API, databáze a kontejner pro použití s hello DocumentDB, grafu nebo tabulky rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9d802-108">Creates a single Azure Cosmos DB API account, database, and container for use with hello DocumentDB, Graph, or Table APIs.</span></span> |
| [<span data-ttu-id="9d802-109">Vytvoření účtu MongoDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="9d802-109">Create a MongoDB API account</span></span>](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="9d802-110">Vytvoří jeden účet, databázi a kolekci rozhraní API služby Azure Cosmos DB MongoDB.</span><span class="sxs-lookup"><span data-stu-id="9d802-110">Creates a single Azure Cosmos DB MongoDB API account, database, and collection.</span></span> |
|<span data-ttu-id="9d802-111">**Škálování Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="9d802-111">**Scale Azure Cosmos DB**</span></span>||
| [<span data-ttu-id="9d802-112">Propustnost kontejneru měřítka</span><span class="sxs-lookup"><span data-stu-id="9d802-112">Scale container throughput</span></span>](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="9d802-113">Změny hello zřizuje througput do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="9d802-113">Changes hello provisioned througput on a container.</span></span>|
|[<span data-ttu-id="9d802-114">Replikaci databáze účtu Azure Cosmos DB do několika oblastí a konfigurace priorit převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="9d802-114">Replicate Azure Cosmos DB database account in multiple regions and configure failover priorities</span></span>](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="9d802-115">Globálně replikuje data účtu do několika oblastí s prioritou zadaný převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="9d802-115">Globally replicates account data into multiple regions with a specified failover priority.</span></span>|
|<span data-ttu-id="9d802-116">**Zabezpečení Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="9d802-116">**Secure Azure Cosmos DB**</span></span>||
| [<span data-ttu-id="9d802-117">Získání klíče účtu</span><span class="sxs-lookup"><span data-stu-id="9d802-117">Get account keys</span></span>](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="9d802-118">Získá hello hlavní zápisu primární a sekundární klíče a primárního a sekundárního klíče jen pro čtení pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="9d802-118">Gets hello primary and secondary master write keys and primary and secondary read-only keys for hello account.</span></span>|
| [<span data-ttu-id="9d802-119">Získat připojovací řetězec MongoDB</span><span class="sxs-lookup"><span data-stu-id="9d802-119">Get MongoDB connection string</span></span>](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="9d802-120">Získá hello připojovací řetězec tooconnect vaše MongoDB aplikace tooyour účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9d802-120">Gets hello connection string tooconnect your MongoDB app tooyour Azure Cosmos DB account.</span></span>|
|[<span data-ttu-id="9d802-121">Obnovit klíče účtu</span><span class="sxs-lookup"><span data-stu-id="9d802-121">Regenerate account keys</span></span>](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="9d802-122">Regeneruje hello hlavní nebo klíč jen pro čtení pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="9d802-122">Regenerates hello master or read-only key for hello account.</span></span>|
|[<span data-ttu-id="9d802-123">Vytvoření brány firewall</span><span class="sxs-lookup"><span data-stu-id="9d802-123">Create a firewall</span></span>](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| <span data-ttu-id="9d802-124">Vytvoří příchozí IP přístup k řízení zásad toolimit toohello účet pro přístup z schválené sadu počítačů nebo cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="9d802-124">Creates an inbound IP access control policy toolimit access toohello account from an approved set of machines and/or cloud services.</span></span>|
|<span data-ttu-id="9d802-125">**Vysoká dostupnost, zotavení po havárii, zálohování a obnovení**</span><span class="sxs-lookup"><span data-stu-id="9d802-125">**High availability, disaster recovery, backup and restore**</span></span>||
|[<span data-ttu-id="9d802-126">Konfigurace zásad převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="9d802-126">Configure failover policy</span></span>](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="9d802-127">Nastaví hello převzetí služeb při selhání prioritu každé oblasti, ve kterém se replikují hello účtu.</span><span class="sxs-lookup"><span data-stu-id="9d802-127">Sets hello failover priority of each region in which hello account is replicated.</span></span>|
|<span data-ttu-id="9d802-128">**Připojení k prostředkům Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="9d802-128">**Connect Azure Cosmos DB to resources**</span></span>||
|[<span data-ttu-id="9d802-129">Připojení webové aplikaci tooAzure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9d802-129">Connect a web app tooAzure Cosmos DB</span></span>](https://docs.microsoft.com/azure/app-service-web/scripts/app-service-cli-app-service-documentdb?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="9d802-130">Vytvořit a připojit databázi Azure Cosmos DB a webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="9d802-130">Create and connect an Azure Cosmos DB database and an Azure web app.</span></span>|
|||
