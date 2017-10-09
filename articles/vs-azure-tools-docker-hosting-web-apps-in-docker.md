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
# <a name="deploy-an-aspnet-container-tooa-remote-docker-host"></a>Nasazení ASP.NET kontejneru tooa vzdáleného Docker hostitele
## <a name="overview"></a>Přehled
Docker je modul lightweight kontejneru, podobně jako v některé způsoby tooa virtuální počítač, který můžete použít toohost aplikací a služeb.
Tento kurz vás provede pomocí hello [Visual Studio Tools for Docker](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker) toodeploy rozšíření ASP.NET Core aplikace tooa Docker hostitele v Azure pomocí prostředí PowerShell.

## <a name="prerequisites"></a>Požadavky
Následující Hello je požadovaná toocomplete v tomto kurzu:

* Vytvoření virtuálního počítače Azure Docker hostitele, jak je popsáno v [jak toouse docker počítače v Azure](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Nainstalujte nejnovější verzi k hello [Visual Studio](https://www.visualstudio.com/downloads/)
* Stáhnout hello [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)
* Nainstalujte [Docker pro Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Vytvoření webové aplikace ASP.NET Core
Hello následující kroky vás provedou vytvořit základní aplikaci ASP.NET Core, která se použije v tomto kurzu.

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Přidání podpory Docker
[!INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-hello-dockertaskps1-powershell-script"></a>3. Použití hello DockerTask.ps1 skript prostředí PowerShell
1. Otevřete prostředí PowerShell výzva toohello kořenového adresáře projektu. 
   
   ```
   PS C:\Src\WebApplication1>
   ```
2. Ověření hello vzdáleného hostitele běží. Měli byste vidět stav = spuštěná 
   
   ```
   docker-machine ls
   NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
   MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
   ```
   
3. Sestavení hello aplikace pomocí hello - parametr sestavení
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
   ```  

   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
   > ```  
   > 
   > 
4. Spuštění aplikace hello pomocí hello - parametr spustit
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
   ```
   
   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
   > ```
   > 
   > 
   
   Po dokončení docker, měli byste vidět podobné toohello následující výsledky:
   
   ![Zobrazit aplikaci][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
