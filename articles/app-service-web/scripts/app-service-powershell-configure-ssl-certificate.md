---
title: "Azure ukázkový skript prostředí PowerShell - vazby SSL vlastní certifikát do webové aplikace | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - vazby vlastní certifikát SSL pro webovou aplikaci"
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
ms.openlocfilehash: de8deccadcd9571be75447a117888bf2b3f571a0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="bind-a-custom-ssl-certificate-to-a-web-app"></a><span data-ttu-id="73059-103">Vlastní certifikát SSL vazbu do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="73059-103">Bind a custom SSL certificate to a web app</span></span>

<span data-ttu-id="73059-104">Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a potom vytvoří vazbu certifikátu SSL vlastního názvu domény.</span><span class="sxs-lookup"><span data-stu-id="73059-104">This sample script creates a web app in App Service with its related resources, then binds the SSL certificate of a custom domain name to it.</span></span> 

<span data-ttu-id="73059-105">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="73059-105">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview).</span></span> <span data-ttu-id="73059-106">Také zajistěte, aby:</span><span class="sxs-lookup"><span data-stu-id="73059-106">Also, ensure that:</span></span>

- <span data-ttu-id="73059-107">Připojení s Azure byla vytvořena pomocí `az login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="73059-107">A connection with Azure has been created using the `az login` command.</span></span>
- <span data-ttu-id="73059-108">Máte přístup na stránku konfigurace DNS doménového registrátora.</span><span class="sxs-lookup"><span data-stu-id="73059-108">You have access to your domain registrar's DNS configuration page.</span></span>
- <span data-ttu-id="73059-109">Máte platný. Soubor PFX a heslo pro certifikát SSL chcete nahrát a jejich vazby.</span><span class="sxs-lookup"><span data-stu-id="73059-109">You have a valid .PFX file and its password for the SSL certificate you want to upload and bind.</span></span>

## <a name="sample-script"></a><span data-ttu-id="73059-110">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="73059-110">Sample script</span></span>

<span data-ttu-id="73059-111">[!code-powershell[hlavní](../../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "vlastní certifikát SSL vazbu do webové aplikace")]</span><span class="sxs-lookup"><span data-stu-id="73059-111">[!code-powershell[main](../../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="73059-112">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="73059-112">Clean up deployment</span></span> 

<span data-ttu-id="73059-113">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků, webové aplikace a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="73059-113">After the script sample has been run, the following command can be used to remove the resource group, web app, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a><span data-ttu-id="73059-114">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="73059-114">Script explanation</span></span>

<span data-ttu-id="73059-115">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="73059-115">This script uses the following commands.</span></span> <span data-ttu-id="73059-116">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="73059-116">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="73059-117">Příkaz</span><span class="sxs-lookup"><span data-stu-id="73059-117">Command</span></span> | <span data-ttu-id="73059-118">Poznámky</span><span class="sxs-lookup"><span data-stu-id="73059-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="73059-119">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="73059-119">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="73059-120">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="73059-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="73059-121">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="73059-121">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="73059-122">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="73059-122">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="73059-123">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="73059-123">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="73059-124">Vytvoří webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="73059-124">Creates a web app.</span></span> |
| [<span data-ttu-id="73059-125">Set-AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="73059-125">Set-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/set-azurermappserviceplan) | <span data-ttu-id="73059-126">Upravuje plán služby App Service změnit jeho cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="73059-126">Modifies an App Service plan to change its pricing tier.</span></span> |
| [<span data-ttu-id="73059-127">Set-AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="73059-127">Set-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/set-azurermwebapp) | <span data-ttu-id="73059-128">Upraví konfiguraci webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="73059-128">Modifies a web app's configuration.</span></span> |
| [<span data-ttu-id="73059-129">Nové AzureRmWebAppSSLBinding</span><span class="sxs-lookup"><span data-stu-id="73059-129">New-AzureRmWebAppSSLBinding</span></span>](/powershell/module/azurerm.websites/new-azurermwebappsslbinding) | <span data-ttu-id="73059-130">Vytvoří vazbu certifikátu SSL pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="73059-130">Creates an SSL certificate binding for a web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="73059-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="73059-131">Next steps</span></span>

<span data-ttu-id="73059-132">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="73059-132">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="73059-133">Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="73059-133">Additional Azure Powershell samples for Azure App Service Web Apps can be found in the [Azure PowerShell samples](../app-service-powershell-samples.md).</span></span>
