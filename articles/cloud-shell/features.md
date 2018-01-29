---
title: "Bash v prostředí cloudu Azure funkce | Microsoft Docs"
description: "Přehled funkcí Bash v prostředí cloudu Azure"
services: Azure
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
ms.date: 11/15/2017
ms.author: juluk
ms.openlocfilehash: a6627ab6febc763ae3f1cd464f26ad641f7c717d
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/23/2018
---
# <a name="features--tools-for-bash-in-azure-cloud-shell"></a>Funkce a nástroje pro Bash v prostředí cloudu Azure

[!INCLUDE [features-introblock](../../includes/cloud-shell-features-introblock.md)]

> [!TIP]
> Funkce a nástrojů v [prostředí PowerShell](features-powershell.md) je také k dispozici.

V prostředí cloudu běží na bash `Ubuntu 16.04 LTS`.

## <a name="features"></a>Funkce

### <a name="secure-automatic-authentication"></a>Zabezpečení automatické ověřování

Bash v prostředí cloudu bezpečně a automaticky ověřuje přístup k účtu pro Azure CLI 2.0.

### <a name="ssh-into-azure-linux-virtual-machines"></a>SSH na Azure Linux virtuální počítače

Vytváření virtuálního počítače s Linuxem z Azure CLI 2.0 můžete vytvořit výchozí klíč SSH a umístěte jej do vaší `$Home` adresáře. Umístění SSH klíče do `$Home` umožňuje přímé připojení SSH pro virtuální počítače Linux Azure přímo z prostředí cloudu. Klíče jsou uložené v acc_<user>img do sdílené složky, použijte osvědčené postupy při práci nebo sdílení přístup ke sdílené složce nebo klíče.

### <a name="home-persistence-across-sessions"></a>Trvalost $Home napříč relacemi

Pro soubory zachová napříč relacemi, cloudové prostředí vás provede procesem připojení sdílenou složku Azure při prvním spuštění.
Po dokončení cloudové prostředí automaticky připojí úložiště (připojit jako `$Home\clouddrive`) pro všechny budoucí relace.
Kromě toho v Bash v prostředí cloudu vaší `$Home` adresář je uchován jako img v Azure sdílené složky.
Soubory mimo `$Home` a stav počítače nejsou trvalé napříč relacemi.

[Další informace o zachování souborů v Bash v prostředí cloudu.](persisting-shell-storage.md)

## <a name="tools"></a>Nástroje

|Kategorie   |Název   |
|---|---|
|Nástroje pro Linux            |Bash<br> Zo<br> tmux<br> dig<br>               |
|Nástroje Azure            |[Rozhraní příkazového řádku Azure 2.0](https://github.com/Azure/azure-cli) a [1.0](https://github.com/Azure/azure-xplat-cli)<br> [AzCopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy)<br> [Batch Shipyard](https://github.com/Azure/batch-shipyard) <br> [Service Fabric CLI](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) <br> [blobxfer](https://github.com/Azure/blobxfer#blobxfer) |
|Textové editory           |VIM<br> nano<br> EMACS       |
|Správa zdrojového kódu         |Git                    |
|Nástroje sestavení            |make<br> maven<br> npm<br> pip         |
|Kontejnery             |[Rozhraní příkazového řádku dockeru](https://github.com/docker/cli)/[počítač Docker](https://github.com/docker/machine)<br> [Kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/)<br> [Helm](https://github.com/kubernetes/helm)<br> [ROZHRANÍ PŘÍKAZOVÉHO ŘÁDKU DC/OS](https://github.com/dcos/dcos-cli)         |
|Databáze              |MySQL klienta<br> PostgreSql klienta<br> [Nástroj SQLCMD](https://docs.microsoft.com/sql/tools/sqlcmd-utility)<br> [mssql-scripter](https://github.com/Microsoft/sql-xplat-cli) |
|Ostatní                  |iPython klienta<br> [Cloud Foundry rozhraní příkazového řádku](https://github.com/cloudfoundry/cli)<br> [Terraform](https://www.terraform.io/docs/providers/azurerm/) |

## <a name="language-support"></a>Podpora jazyků

|Jazyk   |Verze   |
|---|---|
|.NET       |2.0.0       |
|Přejít         |1.7        |
|Java       |1.8        |
|Node.js    |6.9.4      |
|PowerShell |[6.0 (beta)](https://github.com/PowerShell/powershell/releases)       |
|Python     |2.7 a 3.5 (výchozí)|

## <a name="next-steps"></a>Další postup
[Bash v prostředí cloudu rychlý start](quickstart.md) <br>
[Další informace o rozhraní příkazového řádku Azure 2.0](https://docs.microsoft.com/cli/azure/)
