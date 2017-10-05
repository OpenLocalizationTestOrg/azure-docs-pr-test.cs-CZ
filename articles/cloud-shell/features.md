---
title: "Funkce Azure Cloud prostředí (Preview) | Microsoft Docs"
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
ms.openlocfilehash: 67f03d5857e37b253ac57536e289b5468d69e9b5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="features-and-tools-for-azure-cloud-shell"></a>Funkce a nástroje pro prostředí cloudu Azure
Prostředí Azure Cloud je prostředí založené na prohlížeči prostředí pro správu a vývoj prostředků Azure.

Cloudové prostředí nabízí prostředí přístupných prohlížeče, předem nakonfigurovaná prostředí pro správu prostředků Azure bez nutnosti instalace, Správa verzí a údržba počítače s sami.

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
Cloudové prostředí bezpečně a automaticky ověřuje přístup k účtu pro Azure CLI 2.0.

## <a name="azure-files-persistence"></a>Azure trvalost soubory
Vzhledem k tomu, že cloudové prostředí je přidělena na základě požadavků pomocí dočasného počítače, nejsou soubory mimo vašemu $Home a počítač stavu trvalé napříč relacemi.
Pro soubory zachová napříč relacemi, cloudové prostředí vás provede procesem připojení sdílenou složku Azure při prvním spuštění.
Po dokončení cloudové prostředí automaticky připojí úložiště pro všechny budoucí relace.

[Další informace o připojení sdílené složky Azure pro cloudové prostředí.](persisting-shell-storage.md)

## <a name="next-steps"></a>Další kroky
[Rychlý start cloudové prostředí](quickstart.md) <br>
[Další informace o rozhraní příkazového řádku Azure 2.0](https://docs.microsoft.com/cli/azure/) <br>