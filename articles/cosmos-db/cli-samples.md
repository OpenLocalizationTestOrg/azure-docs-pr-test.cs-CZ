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
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Ukázek Azure CLI pro Azure Cosmos DB

Hello následující tabulka obsahuje odkazy toosample rozhraní příkazového řádku Azure skripty pro Azure Cosmos DB. Referenční stránky pro všechny Cosmos DB rozhraní příkazového řádku Azure jsou k dispozici v hello [referenční informace o Azure CLI 2.0](https://docs.microsoft.com/cli/azure/cosmosdb).

| |  |
|---|---|
|**Vytvoření účtu Azure Cosmos DB, databáze a kontejnery**||
|[Vytvoření účtu DocumentDB rozhraní API, grafu nebo tabulky API](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Vytvoří jeden účet Azure Cosmos DB rozhraní API, databáze a kontejner pro použití s hello DocumentDB, grafu nebo tabulky rozhraní API. |
| [Vytvoření účtu MongoDB rozhraní API](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Vytvoří jeden účet, databázi a kolekci rozhraní API služby Azure Cosmos DB MongoDB. |
|**Škálování Azure Cosmos DB**||
| [Propustnost kontejneru měřítka](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Změny hello zřizuje througput do kontejneru.|
|[Replikaci databáze účtu Azure Cosmos DB do několika oblastí a konfigurace priorit převzetí služeb při selhání](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Globálně replikuje data účtu do několika oblastí s prioritou zadaný převzetí služeb při selhání.|
|**Zabezpečení Azure Cosmos DB**||
| [Získání klíče účtu](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Získá hello hlavní zápisu primární a sekundární klíče a primárního a sekundárního klíče jen pro čtení pro účet hello.|
| [Získat připojovací řetězec MongoDB](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Získá hello připojovací řetězec tooconnect vaše MongoDB aplikace tooyour účet Azure Cosmos DB.|
|[Obnovit klíče účtu](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Regeneruje hello hlavní nebo klíč jen pro čtení pro účet hello.|
|[Vytvoření brány firewall](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Vytvoří příchozí IP přístup k řízení zásad toolimit toohello účet pro přístup z schválené sadu počítačů nebo cloudové služby.|
|**Vysoká dostupnost, zotavení po havárii, zálohování a obnovení**||
|[Konfigurace zásad převzetí služeb při selhání](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Nastaví hello převzetí služeb při selhání prioritu každé oblasti, ve kterém se replikují hello účtu.|
|**Připojení k prostředkům Azure Cosmos DB**||
|[Připojení webové aplikaci tooAzure Cosmos DB](https://docs.microsoft.com/azure/app-service-web/scripts/app-service-cli-app-service-documentdb?toc=%2fcli%2fazure%2ftoc.json)|Vytvořit a připojit databázi Azure Cosmos DB a webové aplikace Azure.|
|||
