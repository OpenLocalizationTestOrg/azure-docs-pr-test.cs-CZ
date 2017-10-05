---
title: "Ukázky pro Azure Cosmos DB Azure PowerShell | Microsoft Docs"
description: "Ukázek Azure PowerShell - skripty, které vám pomůžou vytvářet a spravovat účty pro Azure Cosmos DB."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 7698e03c0dc8d1c6d1e926f45e903a909bfd0c93
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a><span data-ttu-id="15220-103">Ukázek Azure PowerShell pro Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="15220-103">Azure PowerShell samples for Azure Cosmos DB</span></span>

<span data-ttu-id="15220-104">Následující tabulka obsahuje odkazy na ukázkové skripty prostředí Azure PowerShell pro Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="15220-104">The following table includes links to sample Azure PowerShell scripts for Azure Cosmos DB.</span></span>

| |  |
|---|---|
|<span data-ttu-id="15220-105">**Vytvoření účtu Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="15220-105">**Create an Azure Cosmos DB account**</span></span>||
|[<span data-ttu-id="15220-106">Vytvoření účtu DocumentDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="15220-106">Create a DocumentDB API account</span></span>](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="15220-107">Vytvoří jeden účet Azure Cosmos DB používat s rozhraním API pro DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="15220-107">Creates a single Azure Cosmos DB account to use with the DocumentDB API.</span></span> |
|<span data-ttu-id="15220-108">**Škálování Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="15220-108">**Scale Azure Cosmos DB**</span></span>||
|[<span data-ttu-id="15220-109">Replikaci účtu Azure Cosmos DB do několika oblastí a konfigurace priorit převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="15220-109">Replicate Azure Cosmos DB account in multiple regions and configure failover priorities</span></span>](scripts/scale-multiregion-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|<span data-ttu-id="15220-110">Globálně replikuje data účtu do několika oblastí s prioritou zadaný převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="15220-110">Globally replicates account data into multiple regions with a specified failover priority.</span></span>|
|<span data-ttu-id="15220-111">**Zabezpečení Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="15220-111">**Secure Azure Cosmos DB**</span></span>||
| [<span data-ttu-id="15220-112">Získání klíče účtu</span><span class="sxs-lookup"><span data-stu-id="15220-112">Get account keys</span></span>](scripts/secure-get-account-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="15220-113">Získá hlavní zápisu primární a sekundární klíče a primární a sekundární klíče jen pro čtení pro účet.</span><span class="sxs-lookup"><span data-stu-id="15220-113">Gets the primary and secondary master write keys and primary and secondary read-only keys for the account.</span></span>|
| [<span data-ttu-id="15220-114">Získat připojovací řetězec MongoDB</span><span class="sxs-lookup"><span data-stu-id="15220-114">Get MongoDB connection string</span></span>](scripts/secure-mongo-connection-string-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="15220-115">Získá připojovací řetězec pro připojení k účtu Azure Cosmos DB aplikace MongoDB.</span><span class="sxs-lookup"><span data-stu-id="15220-115">Gets the connection string to connect your MongoDB app to your Azure Cosmos DB account.</span></span>|
|[<span data-ttu-id="15220-116">Obnovit klíče účtu</span><span class="sxs-lookup"><span data-stu-id="15220-116">Regenerate account keys</span></span>](scripts/secure-regenerate-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|<span data-ttu-id="15220-117">Znovu vygeneruje klíč hlavní nebo jen pro čtení pro účet.</span><span class="sxs-lookup"><span data-stu-id="15220-117">Regenerates the master or read-only key for the account.</span></span>|
|[<span data-ttu-id="15220-118">Vytvoření brány firewall</span><span class="sxs-lookup"><span data-stu-id="15220-118">Create a firewall</span></span>](scripts/create-firewall-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="15220-119">Vytvoří příchozí IP zásadu řízení přístupu omezit přístup k účtu z schválené sadu počítačů nebo cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="15220-119">Creates an inbound IP access control policy to limit access to the account from an approved set of machines and/or cloud services.</span></span>|
|<span data-ttu-id="15220-120">**Vysoká dostupnost, zotavení po havárii, zálohování a obnovení**</span><span class="sxs-lookup"><span data-stu-id="15220-120">**High availability, disaster recovery, backup and restore**</span></span>||
|[<span data-ttu-id="15220-121">Konfigurace zásad převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="15220-121">Configure failover policy</span></span>](scripts/ha-failover-policy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|<span data-ttu-id="15220-122">Nastaví prioritu převzetí služeb při selhání z každou oblast, ve kterém se replikuje účet.</span><span class="sxs-lookup"><span data-stu-id="15220-122">Sets the failover priority of each region in which the account is replicated.</span></span>|
|||