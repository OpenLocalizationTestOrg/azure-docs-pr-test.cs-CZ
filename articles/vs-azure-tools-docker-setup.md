---
title: aaaConfigure Docker hostitele s VirtualBox | Microsoft Docs
description: "Podrobné pokyny tooconfigure výchozí Docker instance pomocí Docker počítač a VirtualBox"
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
ms.openlocfilehash: 1df2da4482444a803d05e413e019edcc57269062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-docker-host-with-virtualbox"></a><span data-ttu-id="bc377-103">Konfigurace hostitele Docker s VirtualBox</span><span class="sxs-lookup"><span data-stu-id="bc377-103">Configure a Docker Host with VirtualBox</span></span>
## <a name="overview"></a><span data-ttu-id="bc377-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="bc377-104">Overview</span></span>
<span data-ttu-id="bc377-105">Tento článek vás provede konfiguraci výchozí instance Docker pomocí Docker počítač a VirtualBox.</span><span class="sxs-lookup"><span data-stu-id="bc377-105">This article guides you through configuring a default Docker instance using Docker Machine and VirtualBox.</span></span> <span data-ttu-id="bc377-106">Pokud používáte hello [Docker pro systém Windows beta](http://beta.docker.com/), tato konfigurace není nutné.</span><span class="sxs-lookup"><span data-stu-id="bc377-106">If you’re using hello [Docker for Windows beta](http://beta.docker.com/), this configuration is not necessary.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bc377-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bc377-107">Prerequisites</span></span>
<span data-ttu-id="bc377-108">Hello tyto nástroje musí toobe nainstalována.</span><span class="sxs-lookup"><span data-stu-id="bc377-108">hello following tools need toobe installed.</span></span>

* [<span data-ttu-id="bc377-109">Sada nástrojů docker</span><span class="sxs-lookup"><span data-stu-id="bc377-109">Docker Toolbox</span></span>](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-hello-docker-client-with-windows-powershell"></a><span data-ttu-id="bc377-110">Konfigurace klienta Docker hello pomocí prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="bc377-110">Configuring hello Docker client with Windows PowerShell</span></span>
<span data-ttu-id="bc377-111">tooconfigure Docker klienta, jednoduše otevřete prostředí Windows PowerShell a proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bc377-111">tooconfigure a Docker client, simply open Windows PowerShell, and perform hello following steps:</span></span>

1. <span data-ttu-id="bc377-112">Vytvořte výchozí instance docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="bc377-112">Create a default docker host instance.</span></span>
   
    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
2. <span data-ttu-id="bc377-113">Ověřte, zda text hello výchozí instance je nakonfigurovaná a spuštěná.</span><span class="sxs-lookup"><span data-stu-id="bc377-113">Verify hello default instance is configured and running.</span></span> <span data-ttu-id="bc377-114">(Byste měli vidět instanci s názvem výchozí systémem.</span><span class="sxs-lookup"><span data-stu-id="bc377-114">(You should see an instance named \`default' running.</span></span>
   
    ```PowerShell
    docker-machine ls 
    ```
   
    ![výstup ls počítač docker][0]
3. <span data-ttu-id="bc377-116">Nastavit výchozí jako aktuální hostitel hello a nakonfigurujte své prostředí.</span><span class="sxs-lookup"><span data-stu-id="bc377-116">Set default as hello current host, and configure your shell.</span></span>
   
    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```
4. <span data-ttu-id="bc377-117">Zobrazte kontejnery služby active Docker hello.</span><span class="sxs-lookup"><span data-stu-id="bc377-117">Display hello active Docker containers.</span></span> <span data-ttu-id="bc377-118">seznam Hello by měla být prázdná.</span><span class="sxs-lookup"><span data-stu-id="bc377-118">hello list should be empty.</span></span>
   
    ```PowerShell
    docker ps
    ```
   
    ![výstup ps docker][1]

> [!NOTE]
> <span data-ttu-id="bc377-120">Pokaždé, když restartujete vývojovém počítači, budete potřebovat toorestart vaše místní docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="bc377-120">Each time you reboot your development machine, you’ll need toorestart your local docker host.</span></span>
> <span data-ttu-id="bc377-121">toodo se problém hello následující příkaz na příkazovém řádku: `docker-machine start default`.</span><span class="sxs-lookup"><span data-stu-id="bc377-121">toodo this, issue hello following command at a command prompt: `docker-machine start default`.</span></span>
> 
> 

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
