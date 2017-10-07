---
title: "aaaAzure ukázkový skript prostředí PowerShell - Bind vlastní tooa webové aplikace certifikát SSL | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - vazby vlastní tooa webové aplikace certifikát SSL"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 23e83b74-614a-49a0-bc08-7542120eeec5
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 6e249ceedb9e2b8872dd0bc8b0aea0d718b28dab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="bind-a-custom-ssl-certificate-tooa-web-app"></a><span data-ttu-id="55630-103">Vytvoření vazby vlastní tooa webové aplikace certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="55630-103">Bind a custom SSL certificate tooa web app</span></span>

<span data-ttu-id="55630-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a potom vytvoří vazbu certifikátu SSL hello tooit název vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="55630-104">This sample script creates a web app in App Service with its related resources, then binds hello SSL certificate of a custom domain name tooit.</span></span> 

<span data-ttu-id="55630-105">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="55630-105">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview).</span></span> <span data-ttu-id="55630-106">Také zajistěte, aby:</span><span class="sxs-lookup"><span data-stu-id="55630-106">Also, ensure that:</span></span>

- <span data-ttu-id="55630-107">Připojení s Azure byl vytvořen pomocí hello `az login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="55630-107">A connection with Azure has been created using hello `az login` command.</span></span>
- <span data-ttu-id="55630-108">Máte přístup tooyour doménového registrátora na stránku konfigurace služby DNS.</span><span class="sxs-lookup"><span data-stu-id="55630-108">You have access tooyour domain registrar's DNS configuration page.</span></span>
- <span data-ttu-id="55630-109">Máte platný. Soubor PFX a heslo pro hello SSL certifikát má tooupload a jejich vazby.</span><span class="sxs-lookup"><span data-stu-id="55630-109">You have a valid .PFX file and its password for hello SSL certificate you want tooupload and bind.</span></span>

## <a name="sample-script"></a><span data-ttu-id="55630-110">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="55630-110">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate tooa web app")]

## <a name="clean-up-deployment"></a><span data-ttu-id="55630-111">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="55630-111">Clean up deployment</span></span> 

<span data-ttu-id="55630-112">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="55630-112">After hello script sample has been run, hello following command can be used tooremove hello resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="55630-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="55630-113">Script explanation</span></span>

<span data-ttu-id="55630-114">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="55630-114">This script uses hello following commands.</span></span> <span data-ttu-id="55630-115">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="55630-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="55630-116">Příkaz</span><span class="sxs-lookup"><span data-stu-id="55630-116">Command</span></span> | <span data-ttu-id="55630-117">Poznámky</span><span class="sxs-lookup"><span data-stu-id="55630-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="55630-118">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="55630-118">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="55630-119">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="55630-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="55630-120">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="55630-120">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="55630-121">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="55630-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="55630-122">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="55630-122">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="55630-123">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="55630-123">Creates a web app.</span></span> |
| [<span data-ttu-id="55630-124">Set-AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="55630-124">Set-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/set-azurermappserviceplan) | <span data-ttu-id="55630-125">Upravuje toochange plán App Service jeho cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="55630-125">Modifies an App Service plan toochange its pricing tier.</span></span> |
| [<span data-ttu-id="55630-126">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="55630-126">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="55630-127">Upraví konfiguraci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="55630-127">Modifies a web app's configuration.</span></span> |
| [<span data-ttu-id="55630-128">Nové AzureRmWebAppSSLBinding</span><span class="sxs-lookup"><span data-stu-id="55630-128">New-AzureRmWebAppSSLBinding</span></span>](/powershell/module/azurerm.websites/new-azurermwebappsslbinding) | <span data-ttu-id="55630-129">Vytvoří vazbu certifikátu SSL pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="55630-129">Creates an SSL certificate binding for a web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="55630-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="55630-130">Next steps</span></span>

<span data-ttu-id="55630-131">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="55630-131">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="55630-132">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v hello [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="55630-132">Additional Azure Powershell samples for Azure App Service Web Apps can be found in hello [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
