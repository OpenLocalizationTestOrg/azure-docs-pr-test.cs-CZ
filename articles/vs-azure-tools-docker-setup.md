---
title: Konfigurace hostitele Docker s VirtualBox | Microsoft Docs
description: "Podrobné pokyny ke konfiguraci výchozí instance Docker pomocí Docker počítač a VirtualBox"
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 0b1335a2-7720-42a8-8260-4e06fc00c9f6
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: e9465afb560a73d74f853c19094b3ee75b8c470c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-docker-host-with-virtualbox"></a><span data-ttu-id="3ed0f-103">Konfigurace hostitele Docker s VirtualBox</span><span class="sxs-lookup"><span data-stu-id="3ed0f-103">Configure a Docker Host with VirtualBox</span></span>
## <a name="overview"></a><span data-ttu-id="3ed0f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="3ed0f-104">Overview</span></span>
<span data-ttu-id="3ed0f-105">Tento článek vás provede konfiguraci výchozí instance Docker pomocí Docker počítač a VirtualBox.</span><span class="sxs-lookup"><span data-stu-id="3ed0f-105">This article guides you through configuring a default Docker instance using Docker Machine and VirtualBox.</span></span> <span data-ttu-id="3ed0f-106">Pokud používáte [Docker pro systém Windows beta](http://beta.docker.com/), tato konfigurace není nutné.</span><span class="sxs-lookup"><span data-stu-id="3ed0f-106">If you’re using the [Docker for Windows beta](http://beta.docker.com/), this configuration is not necessary.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3ed0f-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3ed0f-107">Prerequisites</span></span>
<span data-ttu-id="3ed0f-108">Je potřeba nainstalovat následující nástroje.</span><span class="sxs-lookup"><span data-stu-id="3ed0f-108">The following tools need to be installed.</span></span>

* [<span data-ttu-id="3ed0f-109">Sada nástrojů docker</span><span class="sxs-lookup"><span data-stu-id="3ed0f-109">Docker Toolbox</span></span>](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-the-docker-client-with-windows-powershell"></a><span data-ttu-id="3ed0f-110">Konfigurace klienta Docker v prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="3ed0f-110">Configuring the Docker client with Windows PowerShell</span></span>
<span data-ttu-id="3ed0f-111">Konfigurace klienta Docker, jednoduše otevřete prostředí Windows PowerShell a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3ed0f-111">To configure a Docker client, simply open Windows PowerShell, and perform the following steps:</span></span>

1. <span data-ttu-id="3ed0f-112">Vytvořte výchozí instance docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="3ed0f-112">Create a default docker host instance.</span></span>
   
    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
2. <span data-ttu-id="3ed0f-113">Ověřte, zda že je výchozí instanci nakonfigurovaná a spuštěná.</span><span class="sxs-lookup"><span data-stu-id="3ed0f-113">Verify the default instance is configured and running.</span></span> <span data-ttu-id="3ed0f-114">(Byste měli vidět instanci s názvem výchozí systémem.</span><span class="sxs-lookup"><span data-stu-id="3ed0f-114">(You should see an instance named \`default' running.</span></span>
   
    ```PowerShell
    docker-machine ls 
    ```
   
    ![výstup ls počítač docker][0]
3. <span data-ttu-id="3ed0f-116">Nastavit výchozí jako aktuální hostitel a nakonfigurujte své prostředí.</span><span class="sxs-lookup"><span data-stu-id="3ed0f-116">Set default as the current host, and configure your shell.</span></span>
   
    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```
4. <span data-ttu-id="3ed0f-117">Zobrazte aktivní Docker kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="3ed0f-117">Display the active Docker containers.</span></span> <span data-ttu-id="3ed0f-118">V seznamu by měla být prázdná.</span><span class="sxs-lookup"><span data-stu-id="3ed0f-118">The list should be empty.</span></span>
   
    ```PowerShell
    docker ps
    ```
   
    ![výstup ps docker][1]

> [!NOTE]
> <span data-ttu-id="3ed0f-120">Pokaždé, když restartujete vývojovém počítači, budete muset restartovat místní docker hostiteli.</span><span class="sxs-lookup"><span data-stu-id="3ed0f-120">Each time you reboot your development machine, you’ll need to restart your local docker host.</span></span>
> <span data-ttu-id="3ed0f-121">K tomuto účelu vydejte následující příkaz na příkazovém řádku: `docker-machine start default`.</span><span class="sxs-lookup"><span data-stu-id="3ed0f-121">To do this, issue the following command at a command prompt: `docker-machine start default`.</span></span>
> 
> 

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
