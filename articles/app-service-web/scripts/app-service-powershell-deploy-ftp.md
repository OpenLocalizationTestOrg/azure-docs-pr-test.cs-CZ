---
title: "Azure ukázkový skript prostředí PowerShell – nahrání souborů do webové aplikace pomocí protokolu FTP | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell – nahrání souborů do webové aplikace pomocí protokolu FTP"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: b7d46d6f-44fd-454c-8008-87dab6eefbc1
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 96b99110b63b037746fcc40eb15db5d718eb71a1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="upload-files-to-a-web-app-using-ftp"></a><span data-ttu-id="48fd2-103">Nahrání souborů do webové aplikace pomocí protokolu FTP</span><span class="sxs-lookup"><span data-stu-id="48fd2-103">Upload files to a web app using FTP</span></span>

<span data-ttu-id="48fd2-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a poté nasadí kódu webové aplikace pomocí protokolu FTP (prostřednictvím [WebClient.UploadFile()](https://msdn.microsoft.com/library/ms144229.aspx)).</span><span class="sxs-lookup"><span data-stu-id="48fd2-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code using FTP (via [WebClient.UploadFile()](https://msdn.microsoft.com/library/ms144229.aspx)).</span></span>

<span data-ttu-id="48fd2-105">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` vytvořit připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="48fd2-105">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="48fd2-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="48fd2-106">Sample script</span></span>

<span data-ttu-id="48fd2-107">[!code-powershell[hlavní](../../../powershell_scripts/app-service/deploy-ftp/deploy-ftp.ps1?highlight=1 "nahrání souborů do webové aplikace pomocí protokolu FTP")]</span><span class="sxs-lookup"><span data-stu-id="48fd2-107">[!code-powershell[main](../../../powershell_scripts/app-service/deploy-ftp/deploy-ftp.ps1?highlight=1 "Upload files to a web app using FTP")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="48fd2-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="48fd2-108">Clean up deployment</span></span> 

<span data-ttu-id="48fd2-109">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="48fd2-109">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name $webappname -Force
```

## <a name="script-explanation"></a><span data-ttu-id="48fd2-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="48fd2-110">Script explanation</span></span>

<span data-ttu-id="48fd2-111">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="48fd2-111">This script uses the following commands.</span></span> <span data-ttu-id="48fd2-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="48fd2-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="48fd2-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="48fd2-113">Command</span></span> | <span data-ttu-id="48fd2-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="48fd2-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="48fd2-115">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="48fd2-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="48fd2-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="48fd2-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="48fd2-117">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="48fd2-117">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="48fd2-118">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="48fd2-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="48fd2-119">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="48fd2-119">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="48fd2-120">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="48fd2-120">Creates a web app.</span></span> |
| [<span data-ttu-id="48fd2-121">Get-AzureRmWebAppPublishingProfile</span><span class="sxs-lookup"><span data-stu-id="48fd2-121">Get-AzureRmWebAppPublishingProfile</span></span>](/powershell/module/azurerm.websites/get-azurermwebapppublishingprofile) | <span data-ttu-id="48fd2-122">Získáte profil publikování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="48fd2-122">Get a web app's publishing profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="48fd2-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48fd2-123">Next steps</span></span>

<span data-ttu-id="48fd2-124">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="48fd2-124">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="48fd2-125">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="48fd2-125">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
