---
title: "Funkce aaaAzure prostředí cloudu (Preview) | Microsoft Docs"
description: "Přehled funkcí Azure cloudové prostředí"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: juluk
ms.openlocfilehash: 65482ca6caeac01dda18a6b12eabe943e3d68a96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="features-and-tools-for-azure-cloud-shell"></a>Funkce a nástroje pro prostředí cloudu Azure
Prostředí Azure Cloud je toomanage prostředí prostředí založené na prohlížeči a vyvíjet prostředků Azure.

Cloudové prostředí nabízí přístupných na prohlížeč, předem nakonfigurované prostředí prostředí pro správu prostředků Azure bez režie hello instalaci, správu verzí, a musí jí spravovat počítač s sami.

Prostředí cloudu se zřizuje počítače na základě požadavků a proto nebude stav počítače zachová napříč relacemi. Vzhledem k tomu, že pro interaktivní relace je integrované cloudové prostředí, prostředí shell automaticky ukončit po 20 minutách nečinnosti prostředí.

## <a name="bash-in-cloud-shell"></a>Bash v prostředí cloudu
### <a name="tools"></a>Nástroje
|Kategorie   |Name (Název)   |
|---|---|
|Překladač prostředí Linux|Bash<br> Zo               |
|Nástroje Azure            |[Rozhraní příkazového řádku Azure 2.0](https://github.com/Azure/azure-cli) a [1.0](https://github.com/Azure/azure-xplat-cli)<br> [AzCopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy)<br> [Batch Shipyard](https://github.com/Azure/batch-shipyard)     |
|Textové editory           |VIM<br> nano<br> EMACS       |
|Správa zdrojového kódu         |Git                    |
|Nástroje sestavení            |Ujistěte se<br> maven<br> npm<br> PIP         |
|Kontejnery             |[Rozhraní příkazového řádku dockeru](https://github.com/docker/cli)/[počítač Docker](https://github.com/docker/machine)<br> [Kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/)<br> [Koncept](https://github.com/Azure/draft)<br> [ROZHRANÍ PŘÍKAZOVÉHO ŘÁDKU DC/OS](https://github.com/dcos/dcos-cli)         |
|Databáze              |MySQL klienta<br> PostgreSql klienta<br> [Nástroj SQLCMD](https://docs.microsoft.com/sql/tools/sqlcmd-utility)<br> [Tvůrce MSSQL skriptů](https://github.com/Microsoft/sql-xplat-cli) |
|Ostatní                  |iPython klienta<br> [Cloud Foundry rozhraní příkazového řádku](https://github.com/cloudfoundry/cli)<br> |

### <a name="language-support"></a>Podpora jazyků
|Jazyk   |Verze   |
|---|---|
|.NET       |1.01       |
|Přejít         |1.7        |
|Java       |1.8        |
|Node.js    |6.9.4      |
|Python     |2.7 a 3.5 (výchozí)|

## <a name="secure-automatic-authentication"></a>Zabezpečení automatické ověřování
Cloudové prostředí bezpečně a automaticky ověřuje přístup k účtu pro hello 2.0 rozhraní příkazového řádku Azure.

## <a name="azure-files-persistence"></a>Azure trvalost soubory
Vzhledem k tomu, že cloudové prostředí je přidělena na základě požadavků pomocí dočasného počítače, nejsou soubory mimo vašemu $Home a počítač stavu trvalé napříč relacemi.
soubory toopersist napříč relacemi, cloudové prostředí nevystavíte slabé stránky zabezpečení prostřednictvím připojení Azure file sdílíte při prvním spuštění.
Po dokončení cloudové prostředí automaticky připojí úložiště pro všechny budoucí relace.

[Další informace o připojení tooCloud sdílené složky Azure file prostředí.](persisting-shell-storage.md)

## <a name="next-steps"></a>Další kroky
[Rychlý start cloudové prostředí](quickstart.md) <br>
[Další informace o rozhraní příkazového řádku Azure 2.0](https://docs.microsoft.com/cli/azure/) <br>