---
title: "aaaAzure ukázka skriptu prostředí PowerShell - nahrávání souborů tooa webovou aplikaci pomocí FTP | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - nahrávání souborů tooa webové aplikace pomocí protokolu FTP"
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
ms.openlocfilehash: 32a0a529e94c1315cc6730faf23fca2693c50ffb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-tooa-web-app-using-ftp"></a><span data-ttu-id="79ce1-103">Nahrání souborů tooa webové aplikace pomocí protokolu FTP</span><span class="sxs-lookup"><span data-stu-id="79ce1-103">Upload files tooa web app using FTP</span></span>

<span data-ttu-id="79ce1-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a poté nasadí kódu webové aplikace pomocí protokolu FTP (prostřednictvím [WebClient.UploadFile()](https://msdn.microsoft.com/library/ms144229.aspx)).</span><span class="sxs-lookup"><span data-stu-id="79ce1-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code using FTP (via [WebClient.UploadFile()](https://msdn.microsoft.com/library/ms144229.aspx)).</span></span>

<span data-ttu-id="79ce1-105">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="79ce1-105">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

## <a name="sample-script"></a><span data-ttu-id="79ce1-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="79ce1-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-ftp/deploy-ftp.ps1?highlight=1 "Upload files tooa web app using FTP")]

## <a name="clean-up-deployment"></a><span data-ttu-id="79ce1-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="79ce1-107">Clean up deployment</span></span> 

<span data-ttu-id="79ce1-108">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="79ce1-108">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name $webappname -Force
```

## <a name="script-explanation"></a><span data-ttu-id="79ce1-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="79ce1-109">Script explanation</span></span>

<span data-ttu-id="79ce1-110">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="79ce1-110">This script uses hello following commands.</span></span> <span data-ttu-id="79ce1-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="79ce1-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="79ce1-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="79ce1-112">Command</span></span> | <span data-ttu-id="79ce1-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="79ce1-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="79ce1-114">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="79ce1-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="79ce1-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="79ce1-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="79ce1-116">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="79ce1-116">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="79ce1-117">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="79ce1-117">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="79ce1-118">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="79ce1-118">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="79ce1-119">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="79ce1-119">Creates a web app.</span></span> |
| [<span data-ttu-id="79ce1-120">Get-AzureRmWebAppPublishingProfile</span><span class="sxs-lookup"><span data-stu-id="79ce1-120">Get-AzureRmWebAppPublishingProfile</span></span>](/powershell/module/azurerm.websites/get-azurermwebapppublishingprofile) | <span data-ttu-id="79ce1-121">Získáte profil publikování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="79ce1-121">Get a web app's publishing profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="79ce1-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79ce1-122">Next steps</span></span>

<span data-ttu-id="79ce1-123">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="79ce1-123">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="79ce1-124">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v hello [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="79ce1-124">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
