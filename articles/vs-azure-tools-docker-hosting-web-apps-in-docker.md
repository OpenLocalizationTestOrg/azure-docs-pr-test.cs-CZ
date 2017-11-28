---
title: "Nasaďte kontejner ASP.NET Core Linux Docker vzdáleného hostitele Docker | Microsoft Docs"
description: "Další informace o použití nástroje sady Visual Studio pro Docker nasazení webové aplikace ASP.NET Core na kontejner Docker spuštěna na virtuálním počítači Azure Docker hostiteli systému Linux"
services: azure-container-service
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: e5e81c5e-dd18-4d5a-a24d-a932036e78b9
ms.service: azure-container-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 4a87ee69f23779bf4f6f5db40bc05edbcfc7668d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a><span data-ttu-id="85275-103">Nasaďte kontejner ASP.NET vzdáleného hostitele Docker</span><span class="sxs-lookup"><span data-stu-id="85275-103">Deploy an ASP.NET container to a remote Docker host</span></span>
## <a name="overview"></a><span data-ttu-id="85275-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="85275-104">Overview</span></span>
<span data-ttu-id="85275-105">Docker je modul lightweight kontejneru, podobně jako v některých způsobech virtuální počítač, který můžete použít k hostování aplikací a služeb.</span><span class="sxs-lookup"><span data-stu-id="85275-105">Docker is a lightweight container engine, similar in some ways to a virtual machine, which you can use to host applications and services.</span></span>
<span data-ttu-id="85275-106">Tento kurz vás provede pomocí [Visual Studio Tools for Docker](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker) rozšíření pro nasazení aplikace ASP.NET Core do hostitelů Docker v Azure pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="85275-106">This tutorial walks you through using the [Visual Studio Tools for Docker](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker) extension to deploy an ASP.NET Core app to a Docker host on Azure using PowerShell.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="85275-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="85275-107">Prerequisites</span></span>
<span data-ttu-id="85275-108">K dokončení tohoto kurzu se vyžaduje následující text:</span><span class="sxs-lookup"><span data-stu-id="85275-108">The following is required to complete this tutorial:</span></span>

* <span data-ttu-id="85275-109">Vytvoření virtuálního počítače Azure Docker hostitele, jak je popsáno v [použití docker počítače s Azure](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="85275-109">Create an Azure Docker Host VM as described in [How to use docker-machine with Azure](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* <span data-ttu-id="85275-110">Nainstalujte nejnovější verzi [Visual Studio](https://www.visualstudio.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="85275-110">Install the latest version of [Visual Studio](https://www.visualstudio.com/downloads/)</span></span>
* <span data-ttu-id="85275-111">Stažení [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)</span><span class="sxs-lookup"><span data-stu-id="85275-111">Download the [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)</span></span>
* <span data-ttu-id="85275-112">Nainstalujte [Docker pro Windows](https://docs.docker.com/docker-for-windows/install/)</span><span class="sxs-lookup"><span data-stu-id="85275-112">Install [Docker for Windows](https://docs.docker.com/docker-for-windows/install/)</span></span>

## <a name="1-create-an-aspnet-core-web-app"></a><span data-ttu-id="85275-113">1. Vytvoření webové aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="85275-113">1. Create an ASP.NET Core web app</span></span>
<span data-ttu-id="85275-114">Následující postup vás provede vytvořením základní aplikace ASP.NET Core, který se použije v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="85275-114">The following steps guide you through creating a basic ASP.NET Core app that will be used in this tutorial.</span></span>

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a><span data-ttu-id="85275-115">2. Přidání podpory Docker</span><span class="sxs-lookup"><span data-stu-id="85275-115">2. Add Docker support</span></span>
[!INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a><span data-ttu-id="85275-116">3. Pomocí skriptu prostředí PowerShell DockerTask.ps1</span><span class="sxs-lookup"><span data-stu-id="85275-116">3. Use the DockerTask.ps1 PowerShell Script</span></span>
1. <span data-ttu-id="85275-117">Otevřete příkazovém řádku prostředí PowerShell do kořenového adresáře projektu.</span><span class="sxs-lookup"><span data-stu-id="85275-117">Open a PowerShell prompt to the root directory of your project.</span></span> 
   
   ```
   PS C:\Src\WebApplication1>
   ```
2. <span data-ttu-id="85275-118">Ověření vzdáleném hostiteli spuštěn.</span><span class="sxs-lookup"><span data-stu-id="85275-118">Validate the remote host is running.</span></span> <span data-ttu-id="85275-119">Měli byste vidět stav = spuštěná</span><span class="sxs-lookup"><span data-stu-id="85275-119">You should see state = Running</span></span> 
   
   ```
   docker-machine ls
   NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
   MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
   ```
   
3. <span data-ttu-id="85275-120">Sestavení aplikace pomocí parametru - sestavení</span><span class="sxs-lookup"><span data-stu-id="85275-120">Build the app using the -Build parameter</span></span>
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
   ```  

   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
   > ```  
   > 
   > 
4. <span data-ttu-id="85275-121">Spuštění aplikace, pomocí parametru - spustit</span><span class="sxs-lookup"><span data-stu-id="85275-121">Run the app, using the -Run parameter</span></span>
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
   ```
   
   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
   > ```
   > 
   > 
   
   <span data-ttu-id="85275-122">Po dokončení docker, měli byste vidět výsledky podobné následujícím:</span><span class="sxs-lookup"><span data-stu-id="85275-122">Once docker completes, you should see results similar to the following:</span></span>
   
   ![Zobrazit aplikaci][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
