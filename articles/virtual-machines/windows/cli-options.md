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
# <a name="using-hello-azure-cli-on-windows"></a><span data-ttu-id="4cb22-103">Použití v systému Windows hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="4cb22-103">Using hello Azure CLI on Windows</span></span>

<span data-ttu-id="4cb22-104">Hello rozhraní příkazového řádku Azure (CLI) poskytuje příkazového řádku a skriptovací prostředí pro vytváření a správě prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="4cb22-104">hello Azure Command Line Interface (CLI) provides a command line and scripting environment for creating and managing Azure resources.</span></span> <span data-ttu-id="4cb22-105">Hello rozhraní příkazového řádku Azure je k dispozici pro systému macOS, Linux a operačních systémů Windows.</span><span class="sxs-lookup"><span data-stu-id="4cb22-105">hello Azure CLI is available for macOS, Linux, and Windows operating systems.</span></span> <span data-ttu-id="4cb22-106">Mezi tyto operační systémy, hello rozhraní příkazového řádku jsou stejné, ale specifickou syntaxi skriptování operačního systému se může lišit.</span><span class="sxs-lookup"><span data-stu-id="4cb22-106">Across these operating systems, hello CLI commands are identical, however operating system specific scripting syntax can differ.</span></span>

<span data-ttu-id="4cb22-107">Tento dokument podrobnosti hello způsobů, jak hello rozhraní příkazového řádku Azure lze nainstalovat a spustit v systému Windows a podrobnosti syntaktické důležité informace pro každý.</span><span class="sxs-lookup"><span data-stu-id="4cb22-107">This document details hello ways that hello Azure CLI can be installed and run on Windows and details syntactical considerations for each.</span></span> <span data-ttu-id="4cb22-108">Pro podrobné naleznete v dokumentaci rozhraní příkazového řádku Azure [dokumentaci k rozhraní příkazového řádku Azure]( https://docs.microsoft.com/en-us/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4cb22-108">For in-depth Azure CLI documentation see, [Azure CLI documentation]( https://docs.microsoft.com/en-us/cli/azure/overview).</span></span>

## <a name="windows-subsystem-for-linux"></a><span data-ttu-id="4cb22-109">Subsystém Windows pro Linux</span><span class="sxs-lookup"><span data-stu-id="4cb22-109">Windows Subsystem for Linux</span></span>

<span data-ttu-id="4cb22-110">Hello subsystému Windows pro Linux (WSL) poskytuje prostředí Ubuntu Linux na Windows 10 Anniversary a novější verze.</span><span class="sxs-lookup"><span data-stu-id="4cb22-110">hello Windows Subsystem for Linux (WSL) provides an Ubuntu Linux environment on Windows 10 Anniversary and later editions.</span></span> <span data-ttu-id="4cb22-111">Když je povolené, WSL poskytuje nativní prostředí Bash, který můžete použít pro vytváření a spouštění skriptů rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="4cb22-111">When enabled, WSL provides a native Bash experience, which can be used for creating and running Azure CLI scripts.</span></span> <span data-ttu-id="4cb22-112">Protože WSL poskytuje nativní prostředí Bash, rozhraní příkazového řádku Azure skripty lze sdílet mezi systému macOS, Linux a Windows bez úprav.</span><span class="sxs-lookup"><span data-stu-id="4cb22-112">Because WSL provides a native Bash experience, Azure CLI scripts can be shared between macOS, Linux, and Windows without modification.</span></span>

<span data-ttu-id="4cb22-113">toouse hello rozhraní příkazového řádku Azure v WSL, dokončete následující hello.</span><span class="sxs-lookup"><span data-stu-id="4cb22-113">toouse hello Azure CLI in WSL, complete hello following.</span></span>

|<span data-ttu-id="4cb22-114">Úkol</span><span class="sxs-lookup"><span data-stu-id="4cb22-114">Task</span></span> | <span data-ttu-id="4cb22-115">Pokyny</span><span class="sxs-lookup"><span data-stu-id="4cb22-115">Instructions</span></span> |
|---|---|
| <span data-ttu-id="4cb22-116">Povolit WSL</span><span class="sxs-lookup"><span data-stu-id="4cb22-116">Enable WSL</span></span> | [<span data-ttu-id="4cb22-117">Nainstalujte WSL dokumentace</span><span class="sxs-lookup"><span data-stu-id="4cb22-117">Install WSL documentation </span></span>](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide) |
| <span data-ttu-id="4cb22-118">Nainstalujte hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="4cb22-118">Install hello Azure CLI</span></span> |[<span data-ttu-id="4cb22-119">Nainstalujte na WSL/Ubuntu 14.04 hello rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="4cb22-119">Install hello CLI on WSL/Ubuntu 14.04</span></span>](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#ubuntu)|

## <a name="powershell"></a><span data-ttu-id="4cb22-120">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4cb22-120">PowerShell</span></span>

<span data-ttu-id="4cb22-121">Hello rozhraní příkazového řádku Azure se může spouštět nativně ve Windows.</span><span class="sxs-lookup"><span data-stu-id="4cb22-121">hello Azure CLI can be run natively in Windows.</span></span> <span data-ttu-id="4cb22-122">V této konfiguraci hello balíček rozhraní příkazového řádku Azure je nainstalován na operační systém Windows hello a příkazy můžete spustit z prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4cb22-122">In this configuration, hello Azure CLI package is installed on hello Windows operating system, and commands can be run from PowerShell.</span></span> <span data-ttu-id="4cb22-123">V této konfiguraci Azure CLI příkazy a skripty lze spustit na všechny podporované verze systému Windows, ale je vyžadován specifickou syntaxi skriptovací platformu.</span><span class="sxs-lookup"><span data-stu-id="4cb22-123">In this configuration, Azure CLI commands and scripts can be run on any supported version of Windows, however platform specific scripting syntax is required.</span></span> <span data-ttu-id="4cb22-124">Z toho důvodu skripty nelze nutně sdílet mezi systému macOS, Linux a Windows bez úprav.</span><span class="sxs-lookup"><span data-stu-id="4cb22-124">Because of this, scripts cannot necessarily be shared between macOS, Linux, and Windows without modification.</span></span>

<span data-ttu-id="4cb22-125">toouse hello rozhraní příkazového řádku Azure v systému Windows, instalovat balíček hello podle těchto pokynů [hello instalace rozhraní příkazového řádku v systému Windows](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#windows).</span><span class="sxs-lookup"><span data-stu-id="4cb22-125">toouse hello Azure CLI on Windows, install hello package using these instructions, [Install hello CLI on Windows](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#windows).</span></span>

## <a name="docker-image"></a><span data-ttu-id="4cb22-126">Obrázek docker</span><span class="sxs-lookup"><span data-stu-id="4cb22-126">Docker Image</span></span>

<span data-ttu-id="4cb22-127">Při použití Docker pro systém Windows, bitovou kopii Docker lze spustit zahrnující hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="4cb22-127">When using Docker for Windows, a Docker image can be started that includes hello Azure CLI.</span></span> <span data-ttu-id="4cb22-128">Tento image je založen na systému Linux a zahrnuje nativní prostředí Bash.</span><span class="sxs-lookup"><span data-stu-id="4cb22-128">This image is based off of Linux, and includes a native Bash experience.</span></span>  <span data-ttu-id="4cb22-129">Při použití Docker pro systém Windows a hello image Azure CLI, toobe skripty, které jsou sdílené mezi systému macOS, Linux a Windows.</span><span class="sxs-lookup"><span data-stu-id="4cb22-129">When using Docker for Windows and hello Azure CLI image, scripts toobe shared between macOS, Linux, and Windows.</span></span> 

<span data-ttu-id="4cb22-130">toouse hello rozhraní příkazového řádku Azure na Docker pro systém Windows, zajistěte, aby Docker pro systém Windows běží a spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="4cb22-130">toouse hello Azure CLI on Docker for Windows, ensure that Docker for Windows is running and run hello following command.</span></span>

```bash
docker run -it azuresdk/azure-cli-python:latest bash
```

<span data-ttu-id="4cb22-131">Po dokončení Bash, tedy spuštění relace se předem načtou hello nástrojů příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="4cb22-131">Once completed, a Bash session will start that is preloaded with hello Azure CLI tools.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4cb22-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4cb22-132">Next Steps</span></span>

[<span data-ttu-id="4cb22-133">Ukázka rozhraní příkazového řádku pro virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="4cb22-133">CLI sample for Azure virtual machines</span></span>](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="4cb22-134">Ukázky rozhraní příkazového řádku pro webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="4cb22-134">CLI samples for Azure Web Apps</span></span>](../../app-service-web/app-service-cli-samples.md)

[<span data-ttu-id="4cb22-135">Ukázky rozhraní příkazového řádku pro Azure SQL</span><span class="sxs-lookup"><span data-stu-id="4cb22-135">CLI samples for Azure SQL</span></span>](../../sql-database/sql-database-cli-samples.md)
