---
title: "aaaUsing hello rozhraní příkazového řádku Azure v systému Windows | Microsoft Docs"
description: "Použití v systému Windows hello rozhraní příkazového řádku Azure"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/14/2017
ms.author: nepeters
ms.openlocfilehash: edca4a2701a7dfc0fc54df5649e8a5e7afc95490
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cli-on-windows"></a>Použití v systému Windows hello rozhraní příkazového řádku Azure

Hello rozhraní příkazového řádku Azure (CLI) poskytuje příkazového řádku a skriptovací prostředí pro vytváření a správě prostředků Azure. Hello rozhraní příkazového řádku Azure je k dispozici pro systému macOS, Linux a operačních systémů Windows. Mezi tyto operační systémy, hello rozhraní příkazového řádku jsou stejné, ale specifickou syntaxi skriptování operačního systému se může lišit.

Tento dokument podrobnosti hello způsobů, jak hello rozhraní příkazového řádku Azure lze nainstalovat a spustit v systému Windows a podrobnosti syntaktické důležité informace pro každý. Pro podrobné naleznete v dokumentaci rozhraní příkazového řádku Azure [dokumentaci k rozhraní příkazového řádku Azure]( https://docs.microsoft.com/en-us/cli/azure/overview).

## <a name="windows-subsystem-for-linux"></a>Subsystém Windows pro Linux

Hello subsystému Windows pro Linux (WSL) poskytuje prostředí Ubuntu Linux na Windows 10 Anniversary a novější verze. Když je povolené, WSL poskytuje nativní prostředí Bash, který můžete použít pro vytváření a spouštění skriptů rozhraní příkazového řádku Azure. Protože WSL poskytuje nativní prostředí Bash, rozhraní příkazového řádku Azure skripty lze sdílet mezi systému macOS, Linux a Windows bez úprav.

toouse hello rozhraní příkazového řádku Azure v WSL, dokončete následující hello.

|Úkol | Pokyny |
|---|---|
| Povolit WSL | [Nainstalujte WSL dokumentace](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide) |
| Nainstalujte hello rozhraní příkazového řádku Azure |[Nainstalujte na WSL/Ubuntu 14.04 hello rozhraní příkazového řádku](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#ubuntu)|

## <a name="powershell"></a>PowerShell

Hello rozhraní příkazového řádku Azure se může spouštět nativně ve Windows. V této konfiguraci hello balíček rozhraní příkazového řádku Azure je nainstalován na operační systém Windows hello a příkazy můžete spustit z prostředí PowerShell. V této konfiguraci Azure CLI příkazy a skripty lze spustit na všechny podporované verze systému Windows, ale je vyžadován specifickou syntaxi skriptovací platformu. Z toho důvodu skripty nelze nutně sdílet mezi systému macOS, Linux a Windows bez úprav.

toouse hello rozhraní příkazového řádku Azure v systému Windows, instalovat balíček hello podle těchto pokynů [hello instalace rozhraní příkazového řádku v systému Windows](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#windows).

## <a name="docker-image"></a>Obrázek docker

Při použití Docker pro systém Windows, bitovou kopii Docker lze spustit zahrnující hello rozhraní příkazového řádku Azure. Tento image je založen na systému Linux a zahrnuje nativní prostředí Bash.  Při použití Docker pro systém Windows a hello image Azure CLI, toobe skripty, které jsou sdílené mezi systému macOS, Linux a Windows. 

toouse hello rozhraní příkazového řádku Azure na Docker pro systém Windows, zajistěte, aby Docker pro systém Windows běží a spusťte následující příkaz hello.

```bash
docker run -it azuresdk/azure-cli-python:latest bash
```

Po dokončení Bash, tedy spuštění relace se předem načtou hello nástrojů příkazového řádku Azure.

## <a name="next-steps"></a>Další kroky

[Ukázka rozhraní příkazového řádku pro virtuální počítače Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Ukázky rozhraní příkazového řádku pro webové aplikace Azure](../../app-service-web/app-service-cli-samples.md)

[Ukázky rozhraní příkazového řádku pro Azure SQL](../../sql-database/sql-database-cli-samples.md)
