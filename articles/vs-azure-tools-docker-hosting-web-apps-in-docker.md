---
title: "aaaDeploy ASP.NET Core Linux Docker kontejneru tooa Docker hostitele vzdálené | Microsoft Docs"
description: "Zjistěte, jak toouse Visual Studio Tools for Docker toodeploy ASP.NET Core webový kontejner Docker tooa aplikace spuštěna na virtuálním počítači Azure Docker hostiteli systému Linux"
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
ms.openlocfilehash: 27b0c6420628c73220200bc071b47a4cd89fff58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-aspnet-container-tooa-remote-docker-host"></a><span data-ttu-id="458fc-103">Nasazení ASP.NET kontejneru tooa vzdáleného Docker hostitele</span><span class="sxs-lookup"><span data-stu-id="458fc-103">Deploy an ASP.NET container tooa remote Docker host</span></span>
## <a name="overview"></a><span data-ttu-id="458fc-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="458fc-104">Overview</span></span>
<span data-ttu-id="458fc-105">Docker je modul lightweight kontejneru, podobně jako v některé způsoby tooa virtuální počítač, který můžete použít toohost aplikací a služeb.</span><span class="sxs-lookup"><span data-stu-id="458fc-105">Docker is a lightweight container engine, similar in some ways tooa virtual machine, which you can use toohost applications and services.</span></span>
<span data-ttu-id="458fc-106">Tento kurz vás provede pomocí hello [Visual Studio Tools for Docker](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker) toodeploy rozšíření ASP.NET Core aplikace tooa Docker hostitele v Azure pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="458fc-106">This tutorial walks you through using hello [Visual Studio Tools for Docker](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker) extension toodeploy an ASP.NET Core app tooa Docker host on Azure using PowerShell.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="458fc-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="458fc-107">Prerequisites</span></span>
<span data-ttu-id="458fc-108">Následující Hello je požadovaná toocomplete v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="458fc-108">hello following is required toocomplete this tutorial:</span></span>

* <span data-ttu-id="458fc-109">Vytvoření virtuálního počítače Azure Docker hostitele, jak je popsáno v [jak toouse docker počítače v Azure](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="458fc-109">Create an Azure Docker Host VM as described in [How toouse docker-machine with Azure](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* <span data-ttu-id="458fc-110">Nainstalujte nejnovější verzi k hello [Visual Studio](https://www.visualstudio.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="458fc-110">Install hello latest version of [Visual Studio](https://www.visualstudio.com/downloads/)</span></span>
* <span data-ttu-id="458fc-111">Stáhnout hello [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)</span><span class="sxs-lookup"><span data-stu-id="458fc-111">Download hello [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)</span></span>
* <span data-ttu-id="458fc-112">Nainstalujte [Docker pro Windows](https://docs.docker.com/docker-for-windows/install/)</span><span class="sxs-lookup"><span data-stu-id="458fc-112">Install [Docker for Windows](https://docs.docker.com/docker-for-windows/install/)</span></span>

## <a name="1-create-an-aspnet-core-web-app"></a><span data-ttu-id="458fc-113">1. Vytvoření webové aplikace ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="458fc-113">1. Create an ASP.NET Core web app</span></span>
<span data-ttu-id="458fc-114">Hello následující kroky vás provedou vytvořit základní aplikaci ASP.NET Core, která se použije v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="458fc-114">hello following steps guide you through creating a basic ASP.NET Core app that will be used in this tutorial.</span></span>

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a><span data-ttu-id="458fc-115">2. Přidání podpory Docker</span><span class="sxs-lookup"><span data-stu-id="458fc-115">2. Add Docker support</span></span>
[!INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-hello-dockertaskps1-powershell-script"></a><span data-ttu-id="458fc-116">3. Použití hello DockerTask.ps1 skript prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="458fc-116">3. Use hello DockerTask.ps1 PowerShell Script</span></span>
1. <span data-ttu-id="458fc-117">Otevřete prostředí PowerShell výzva toohello kořenového adresáře projektu.</span><span class="sxs-lookup"><span data-stu-id="458fc-117">Open a PowerShell prompt toohello root directory of your project.</span></span> 
   
   ```
   PS C:\Src\WebApplication1>
   ```
2. <span data-ttu-id="458fc-118">Ověření hello vzdáleného hostitele běží.</span><span class="sxs-lookup"><span data-stu-id="458fc-118">Validate hello remote host is running.</span></span> <span data-ttu-id="458fc-119">Měli byste vidět stav = spuštěná</span><span class="sxs-lookup"><span data-stu-id="458fc-119">You should see state = Running</span></span> 
   
   ```
   docker-machine ls
   NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
   MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
   ```
   
3. <span data-ttu-id="458fc-120">Sestavení hello aplikace pomocí hello - parametr sestavení</span><span class="sxs-lookup"><span data-stu-id="458fc-120">Build hello app using hello -Build parameter</span></span>
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
   ```  

   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
   > ```  
   > 
   > 
4. <span data-ttu-id="458fc-121">Spuštění aplikace hello pomocí hello - parametr spustit</span><span class="sxs-lookup"><span data-stu-id="458fc-121">Run hello app, using hello -Run parameter</span></span>
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
   ```
   
   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
   > ```
   > 
   > 
   
   <span data-ttu-id="458fc-122">Po dokončení docker, měli byste vidět podobné toohello následující výsledky:</span><span class="sxs-lookup"><span data-stu-id="458fc-122">Once docker completes, you should see results similar toohello following:</span></span>
   
   ![Zobrazit aplikaci][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
