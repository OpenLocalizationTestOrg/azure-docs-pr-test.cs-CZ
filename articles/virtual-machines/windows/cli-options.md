---
title: "Použití Azure CLI v systému Windows | Microsoft Docs"
description: "Použití Azure CLI v systému Windows"
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
ms.openlocfilehash: be02ad0d7752cb08f092deeb5a86dcd126403237
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-the-azure-cli-on-windows"></a><span data-ttu-id="bdfa8-103">Použití Azure CLI v systému Windows</span><span class="sxs-lookup"><span data-stu-id="bdfa8-103">Using the Azure CLI on Windows</span></span>

<span data-ttu-id="bdfa8-104">Rozhraní příkazového řádku Azure (CLI) poskytuje příkazového řádku a skriptovací prostředí pro vytváření a správě prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="bdfa8-104">The Azure Command Line Interface (CLI) provides a command line and scripting environment for creating and managing Azure resources.</span></span> <span data-ttu-id="bdfa8-105">Rozhraní příkazového řádku Azure je k dispozici pro systému macOS, Linux a operačních systémů Windows.</span><span class="sxs-lookup"><span data-stu-id="bdfa8-105">The Azure CLI is available for macOS, Linux, and Windows operating systems.</span></span> <span data-ttu-id="bdfa8-106">Mezi tyto operační systémy, rozhraní příkazového řádku jsou stejné, ale specifickou syntaxi skriptování operačního systému se může lišit.</span><span class="sxs-lookup"><span data-stu-id="bdfa8-106">Across these operating systems, the CLI commands are identical, however operating system specific scripting syntax can differ.</span></span>

<span data-ttu-id="bdfa8-107">Tento dokument podrobně popisuje způsoby, rozhraní příkazového řádku Azure můžete nainstalovat a spustit v systému Windows a podrobnosti syntaktické důležité informace pro každý.</span><span class="sxs-lookup"><span data-stu-id="bdfa8-107">This document details the ways that the Azure CLI can be installed and run on Windows and details syntactical considerations for each.</span></span> <span data-ttu-id="bdfa8-108">Pro podrobné naleznete v dokumentaci rozhraní příkazového řádku Azure [dokumentaci k rozhraní příkazového řádku Azure]( https://docs.microsoft.com/en-us/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bdfa8-108">For in-depth Azure CLI documentation see, [Azure CLI documentation]( https://docs.microsoft.com/en-us/cli/azure/overview).</span></span>

## <a name="windows-subsystem-for-linux"></a><span data-ttu-id="bdfa8-109">Subsystém Windows pro Linux</span><span class="sxs-lookup"><span data-stu-id="bdfa8-109">Windows Subsystem for Linux</span></span>

<span data-ttu-id="bdfa8-110">Subsystém Windows pro Linux (WSL) poskytuje prostředí Ubuntu Linux na Windows 10 Anniversary a novější verze.</span><span class="sxs-lookup"><span data-stu-id="bdfa8-110">The Windows Subsystem for Linux (WSL) provides an Ubuntu Linux environment on Windows 10 Anniversary and later editions.</span></span> <span data-ttu-id="bdfa8-111">Když je povolené, WSL poskytuje nativní prostředí Bash, který můžete použít pro vytváření a spouštění skriptů rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="bdfa8-111">When enabled, WSL provides a native Bash experience, which can be used for creating and running Azure CLI scripts.</span></span> <span data-ttu-id="bdfa8-112">Protože WSL poskytuje nativní prostředí Bash, rozhraní příkazového řádku Azure skripty lze sdílet mezi systému macOS, Linux a Windows bez úprav.</span><span class="sxs-lookup"><span data-stu-id="bdfa8-112">Because WSL provides a native Bash experience, Azure CLI scripts can be shared between macOS, Linux, and Windows without modification.</span></span>

<span data-ttu-id="bdfa8-113">Pokud chcete používat rozhraní příkazového řádku Azure v WSL, proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="bdfa8-113">To use the Azure CLI in WSL, complete the following.</span></span>

|<span data-ttu-id="bdfa8-114">Úkol</span><span class="sxs-lookup"><span data-stu-id="bdfa8-114">Task</span></span> | <span data-ttu-id="bdfa8-115">Pokyny</span><span class="sxs-lookup"><span data-stu-id="bdfa8-115">Instructions</span></span> |
|---|---|
| <span data-ttu-id="bdfa8-116">Povolit WSL</span><span class="sxs-lookup"><span data-stu-id="bdfa8-116">Enable WSL</span></span> | [<span data-ttu-id="bdfa8-117">Nainstalujte WSL dokumentace</span><span class="sxs-lookup"><span data-stu-id="bdfa8-117">Install WSL documentation </span></span>](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide) |
| <span data-ttu-id="bdfa8-118">Instalace rozhraní příkazového řádku Azure CLI</span><span class="sxs-lookup"><span data-stu-id="bdfa8-118">Install the Azure CLI</span></span> |[<span data-ttu-id="bdfa8-119">Nainstalovat rozhraní příkazového řádku na WSL/Ubuntu 14.04</span><span class="sxs-lookup"><span data-stu-id="bdfa8-119">Install the CLI on WSL/Ubuntu 14.04</span></span>](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#ubuntu)|

## <a name="powershell"></a><span data-ttu-id="bdfa8-120">PowerShell</span><span class="sxs-lookup"><span data-stu-id="bdfa8-120">PowerShell</span></span>

<span data-ttu-id="bdfa8-121">Rozhraní příkazového řádku Azure se může spouštět nativně ve Windows.</span><span class="sxs-lookup"><span data-stu-id="bdfa8-121">The Azure CLI can be run natively in Windows.</span></span> <span data-ttu-id="bdfa8-122">V této konfiguraci balíček rozhraní příkazového řádku Azure je nainstalovaný na operačním systému Windows a příkazy můžete spustit z prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bdfa8-122">In this configuration, the Azure CLI package is installed on the Windows operating system, and commands can be run from PowerShell.</span></span> <span data-ttu-id="bdfa8-123">V této konfiguraci Azure CLI příkazy a skripty lze spustit na všechny podporované verze systému Windows, ale je vyžadován specifickou syntaxi skriptovací platformu.</span><span class="sxs-lookup"><span data-stu-id="bdfa8-123">In this configuration, Azure CLI commands and scripts can be run on any supported version of Windows, however platform specific scripting syntax is required.</span></span> <span data-ttu-id="bdfa8-124">Z toho důvodu skripty nelze nutně sdílet mezi systému macOS, Linux a Windows bez úprav.</span><span class="sxs-lookup"><span data-stu-id="bdfa8-124">Because of this, scripts cannot necessarily be shared between macOS, Linux, and Windows without modification.</span></span>

<span data-ttu-id="bdfa8-125">Pokud chcete používat rozhraní příkazového řádku Azure v systému Windows, nainstalujte balíček podle těchto pokynů [nainstalovat rozhraní příkazového řádku v systému Windows](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#windows).</span><span class="sxs-lookup"><span data-stu-id="bdfa8-125">To use the Azure CLI on Windows, install the package using these instructions, [Install the CLI on Windows](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#windows).</span></span>

## <a name="docker-image"></a><span data-ttu-id="bdfa8-126">Obrázek docker</span><span class="sxs-lookup"><span data-stu-id="bdfa8-126">Docker Image</span></span>

<span data-ttu-id="bdfa8-127">Při použití Docker pro systém Windows, bitovou kopii Docker lze spustit zahrnující Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="bdfa8-127">When using Docker for Windows, a Docker image can be started that includes the Azure CLI.</span></span> <span data-ttu-id="bdfa8-128">Tento image je založen na systému Linux a zahrnuje nativní prostředí Bash.</span><span class="sxs-lookup"><span data-stu-id="bdfa8-128">This image is based off of Linux, and includes a native Bash experience.</span></span>  <span data-ttu-id="bdfa8-129">Při použití Docker pro systém Windows a bitovou kopii rozhraní příkazového řádku Azure, skripty, které sdílet mezi systému macOS, Linux a Windows.</span><span class="sxs-lookup"><span data-stu-id="bdfa8-129">When using Docker for Windows and the Azure CLI image, scripts to be shared between macOS, Linux, and Windows.</span></span> 

<span data-ttu-id="bdfa8-130">Pokud chcete používat rozhraní příkazového řádku Azure na Docker pro systém Windows, zkontrolujte, zda je spuštěn Docker pro systém Windows a spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="bdfa8-130">To use the Azure CLI on Docker for Windows, ensure that Docker for Windows is running and run the following command.</span></span>

```bash
docker run -it azuresdk/azure-cli-python:latest bash
```

<span data-ttu-id="bdfa8-131">Po dokončení Bash, tedy spuštění relace se předem načtou nástrojů příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="bdfa8-131">Once completed, a Bash session will start that is preloaded with the Azure CLI tools.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bdfa8-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bdfa8-132">Next Steps</span></span>

[<span data-ttu-id="bdfa8-133">Ukázka rozhraní příkazového řádku pro virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="bdfa8-133">CLI sample for Azure virtual machines</span></span>](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="bdfa8-134">Ukázky rozhraní příkazového řádku pro webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="bdfa8-134">CLI samples for Azure Web Apps</span></span>](../../app-service-web/app-service-cli-samples.md)

[<span data-ttu-id="bdfa8-135">Ukázky rozhraní příkazového řádku pro Azure SQL</span><span class="sxs-lookup"><span data-stu-id="bdfa8-135">CLI samples for Azure SQL</span></span>](../../sql-database/sql-database-cli-samples.md)
