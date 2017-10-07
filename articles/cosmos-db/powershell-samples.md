---
title: "aaaAzure ukázky prostředí PowerShell pro Azure Cosmos DB | Microsoft Docs"
description: "Ukázek Azure PowerShell - toohelp skripty můžete vytvořit a spravovat účty pro Azure Cosmos DB."
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
ms.openlocfilehash: 8815e775f720226e98cc8dcf7743e481c213272b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a><span data-ttu-id="9b65b-103">Ukázek Azure PowerShell pro Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9b65b-103">Azure PowerShell samples for Azure Cosmos DB</span></span>

<span data-ttu-id="9b65b-104">Hello následující tabulka obsahuje odkazy toosample prostředí Azure PowerShell skripty pro Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9b65b-104">hello following table includes links toosample Azure PowerShell scripts for Azure Cosmos DB.</span></span>

| |  |
|---|---|
|<span data-ttu-id="9b65b-105">**Vytvoření účtu Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="9b65b-105">**Create an Azure Cosmos DB account**</span></span>||
|[<span data-ttu-id="9b65b-106">Vytvoření účtu DocumentDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="9b65b-106">Create a DocumentDB API account</span></span>](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="9b65b-107">Vytvoří jeden toouse účet Azure Cosmos DB hello DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9b65b-107">Creates a single Azure Cosmos DB account toouse with hello DocumentDB API.</span></span> |
|<span data-ttu-id="9b65b-108">**Škálování Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="9b65b-108">**Scale Azure Cosmos DB**</span></span>||
|[<span data-ttu-id="9b65b-109">Replikaci účtu Azure Cosmos DB do několika oblastí a konfigurace priorit převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="9b65b-109">Replicate Azure Cosmos DB account in multiple regions and configure failover priorities</span></span>](scripts/scale-multiregion-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|<span data-ttu-id="9b65b-110">Globálně replikuje data účtu do několika oblastí s prioritou zadaný převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="9b65b-110">Globally replicates account data into multiple regions with a specified failover priority.</span></span>|
|<span data-ttu-id="9b65b-111">**Zabezpečení Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="9b65b-111">**Secure Azure Cosmos DB**</span></span>||
| [<span data-ttu-id="9b65b-112">Získání klíče účtu</span><span class="sxs-lookup"><span data-stu-id="9b65b-112">Get account keys</span></span>](scripts/secure-get-account-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="9b65b-113">Získá hello hlavní zápisu primární a sekundární klíče a primárního a sekundárního klíče jen pro čtení pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="9b65b-113">Gets hello primary and secondary master write keys and primary and secondary read-only keys for hello account.</span></span>|
| [<span data-ttu-id="9b65b-114">Získat připojovací řetězec MongoDB</span><span class="sxs-lookup"><span data-stu-id="9b65b-114">Get MongoDB connection string</span></span>](scripts/secure-mongo-connection-string-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="9b65b-115">Získá hello připojovací řetězec tooconnect vaše MongoDB aplikace tooyour účet Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9b65b-115">Gets hello connection string tooconnect your MongoDB app tooyour Azure Cosmos DB account.</span></span>|
|[<span data-ttu-id="9b65b-116">Obnovit klíče účtu</span><span class="sxs-lookup"><span data-stu-id="9b65b-116">Regenerate account keys</span></span>](scripts/secure-regenerate-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|<span data-ttu-id="9b65b-117">Regeneruje hello hlavní nebo klíč jen pro čtení pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="9b65b-117">Regenerates hello master or read-only key for hello account.</span></span>|
|[<span data-ttu-id="9b65b-118">Vytvoření brány firewall</span><span class="sxs-lookup"><span data-stu-id="9b65b-118">Create a firewall</span></span>](scripts/create-firewall-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="9b65b-119">Vytvoří příchozí IP přístup k řízení zásad toolimit toohello účet pro přístup z schválené sadu počítačů nebo cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="9b65b-119">Creates an inbound IP access control policy toolimit access toohello account from an approved set of machines and/or cloud services.</span></span>|
|<span data-ttu-id="9b65b-120">**Vysoká dostupnost, zotavení po havárii, zálohování a obnovení**</span><span class="sxs-lookup"><span data-stu-id="9b65b-120">**High availability, disaster recovery, backup and restore**</span></span>||
|[<span data-ttu-id="9b65b-121">Konfigurace zásad převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="9b65b-121">Configure failover policy</span></span>](scripts/ha-failover-policy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|<span data-ttu-id="9b65b-122">Nastaví hello převzetí služeb při selhání prioritu každé oblasti, ve kterém se replikují hello účtu.</span><span class="sxs-lookup"><span data-stu-id="9b65b-122">Sets hello failover priority of each region in which hello account is replicated.</span></span>|
|||