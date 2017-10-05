---
title: "Ukázky rozhraní příkazového řádku Azure pro Azure Cosmos DB | Microsoft Docs"
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
ms.openlocfilehash: 709d2ccce0f4b9827a8076f683c7e0f74cbdd4ea
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a><span data-ttu-id="4d229-103">Ukázek Azure CLI pro Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="4d229-103">Azure CLI samples for Azure Cosmos DB</span></span>

<span data-ttu-id="4d229-104">Následující tabulka obsahuje odkazy na ukázkové skripty rozhraní příkazového řádku Azure pro Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4d229-104">The following table includes links to sample Azure CLI scripts for Azure Cosmos DB.</span></span> <span data-ttu-id="4d229-105">Jsou k dispozici v referenční stránky pro všechny Cosmos DB rozhraní příkazového řádku Azure [referenční informace o Azure CLI 2.0](https://docs.microsoft.com/cli/azure/cosmosdb).</span><span class="sxs-lookup"><span data-stu-id="4d229-105">Reference pages for all Azure Cosmos DB CLI commands are available in the [Azure CLI 2.0 Reference](https://docs.microsoft.com/cli/azure/cosmosdb).</span></span>

| |  |
|---|---|
|<span data-ttu-id="4d229-106">**Vytvoření účtu Azure Cosmos DB, databáze a kontejnery**</span><span class="sxs-lookup"><span data-stu-id="4d229-106">**Create Azure Cosmos DB account, database, and containers**</span></span>||
|[<span data-ttu-id="4d229-107">Vytvoření účtu DocumentDB rozhraní API, grafu nebo tabulky API</span><span class="sxs-lookup"><span data-stu-id="4d229-107">Create a DocumentDB API, Graph, or Table API account</span></span>](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| <span data-ttu-id="4d229-108">Vytvoří jeden účet Azure Cosmos DB rozhraní API, databáze a kontejner pro použití s DocumentDB, grafu nebo tabulky rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4d229-108">Creates a single Azure Cosmos DB API account, database, and container for use with the DocumentDB, Graph, or Table APIs.</span></span> |
| [<span data-ttu-id="4d229-109">Vytvoření účtu MongoDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4d229-109">Create a MongoDB API account</span></span>](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="4d229-110">Vytvoří jeden účet, databázi a kolekci rozhraní API služby Azure Cosmos DB MongoDB.</span><span class="sxs-lookup"><span data-stu-id="4d229-110">Creates a single Azure Cosmos DB MongoDB API account, database, and collection.</span></span> |
|<span data-ttu-id="4d229-111">**Škálování Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="4d229-111">**Scale Azure Cosmos DB**</span></span>||
| [<span data-ttu-id="4d229-112">Propustnost kontejneru měřítka</span><span class="sxs-lookup"><span data-stu-id="4d229-112">Scale container throughput</span></span>](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="4d229-113">Změní zřízené througput do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="4d229-113">Changes the provisioned througput on a container.</span></span>|
|[<span data-ttu-id="4d229-114">Replikaci databáze účtu Azure Cosmos DB do několika oblastí a konfigurace priorit převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="4d229-114">Replicate Azure Cosmos DB database account in multiple regions and configure failover priorities</span></span>](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="4d229-115">Globálně replikuje data účtu do několika oblastí s prioritou zadaný převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="4d229-115">Globally replicates account data into multiple regions with a specified failover priority.</span></span>|
|<span data-ttu-id="4d229-116">**Zabezpečení Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="4d229-116">**Secure Azure Cosmos DB**</span></span>||
| [<span data-ttu-id="4d229-117">Získání klíče účtu</span><span class="sxs-lookup"><span data-stu-id="4d229-117">Get account keys</span></span>](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="4d229-118">Získá hlavní zápisu primární a sekundární klíče a primární a sekundární klíče jen pro čtení pro účet.</span><span class="sxs-lookup"><span data-stu-id="4d229-118">Gets the primary and secondary master write keys and primary and secondary read-only keys for the account.</span></span>|
| [<span data-ttu-id="4d229-119">Získat připojovací řetězec MongoDB</span><span class="sxs-lookup"><span data-stu-id="4d229-119">Get MongoDB connection string</span></span>](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="4d229-120">Získá připojovací řetězec pro připojení k účtu Azure Cosmos DB aplikace MongoDB.</span><span class="sxs-lookup"><span data-stu-id="4d229-120">Gets the connection string to connect your MongoDB app to your Azure Cosmos DB account.</span></span>|
|[<span data-ttu-id="4d229-121">Obnovit klíče účtu</span><span class="sxs-lookup"><span data-stu-id="4d229-121">Regenerate account keys</span></span>](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="4d229-122">Znovu vygeneruje klíč hlavní nebo jen pro čtení pro účet.</span><span class="sxs-lookup"><span data-stu-id="4d229-122">Regenerates the master or read-only key for the account.</span></span>|
|[<span data-ttu-id="4d229-123">Vytvoření brány firewall</span><span class="sxs-lookup"><span data-stu-id="4d229-123">Create a firewall</span></span>](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| <span data-ttu-id="4d229-124">Vytvoří příchozí IP zásadu řízení přístupu omezit přístup k účtu z schválené sadu počítačů nebo cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="4d229-124">Creates an inbound IP access control policy to limit access to the account from an approved set of machines and/or cloud services.</span></span>|
|<span data-ttu-id="4d229-125">**Vysoká dostupnost, zotavení po havárii, zálohování a obnovení**</span><span class="sxs-lookup"><span data-stu-id="4d229-125">**High availability, disaster recovery, backup and restore**</span></span>||
|[<span data-ttu-id="4d229-126">Konfigurace zásad převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="4d229-126">Configure failover policy</span></span>](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="4d229-127">Nastaví prioritu převzetí služeb při selhání z každou oblast, ve kterém se replikuje účet.</span><span class="sxs-lookup"><span data-stu-id="4d229-127">Sets the failover priority of each region in which the account is replicated.</span></span>|
|<span data-ttu-id="4d229-128">**Připojení k prostředkům Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="4d229-128">**Connect Azure Cosmos DB to resources**</span></span>||
|[<span data-ttu-id="4d229-129">Webovou aplikaci připojit k databázi Azure Cosmos</span><span class="sxs-lookup"><span data-stu-id="4d229-129">Connect a web app to Azure Cosmos DB</span></span>](https://docs.microsoft.com/azure/app-service-web/scripts/app-service-cli-app-service-documentdb?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="4d229-130">Vytvořit a připojit databázi Azure Cosmos DB a webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="4d229-130">Create and connect an Azure Cosmos DB database and an Azure web app.</span></span>|
|||
