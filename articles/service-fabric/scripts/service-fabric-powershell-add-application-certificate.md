---
title: "aaaAzure ukázkový skript prostředí PowerShell – přidat aplikaci cert tooa cluster | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – Přidání clusteru Service Fabric tooa certifikát služby aplikací."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: aba62598e2e4775012f89b5070bef5e61aec64f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-an-application-certificate-tooa-service-fabric-cluster"></a><span data-ttu-id="da931-103">Přidejte cluster Service Fabric tooa certifikát služby aplikace</span><span class="sxs-lookup"><span data-stu-id="da931-103">Add an application certificate tooa Service Fabric cluster</span></span>

<span data-ttu-id="da931-104">Tento ukázkový skript vytvoří certifikát podepsaný svým držitelem v hello zadaný Azure trezoru klíčů a ho nainstaluje tooall uzly clusteru Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="da931-104">This sample script creates a self-signed certificate in hello specified Azure key vault and installs it tooall nodes of hello Service Fabric cluster.</span></span> <span data-ttu-id="da931-105">certifikát Hello také stáhne tooa místní složky.</span><span class="sxs-lookup"><span data-stu-id="da931-105">hello certificate also downloads tooa local folder.</span></span> <span data-ttu-id="da931-106">Název Hello hello stáhnout certifikát je hello stejný jako název hello hello certifikátu v trezoru klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="da931-106">hello name of hello downloaded certificate is hello same as hello name of hello certificate in hello key vault.</span></span> <span data-ttu-id="da931-107">Parametry hello přizpůsobte podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="da931-107">Customize hello parameters as needed.</span></span>

<span data-ttu-id="da931-108">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](/powershell/azure/overview) a spusťte `Login-AzureRmAccount` toocreate připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="da931-108">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview) and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span> 

## <a name="sample-script"></a><span data-ttu-id="da931-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="da931-109">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "Add an application certificate tooa cluster")]

## <a name="script-explanation"></a><span data-ttu-id="da931-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="da931-110">Script explanation</span></span>

<span data-ttu-id="da931-111">Tento skript používá hello následující příkazy: každý příkaz v tabulce hello odkazy toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="da931-111">This script uses hello following commands: Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="da931-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="da931-112">Command</span></span> | <span data-ttu-id="da931-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="da931-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="da931-114">Přidat AzureRmServiceFabricApplicationCertificate</span><span class="sxs-lookup"><span data-stu-id="da931-114">Add-AzureRmServiceFabricApplicationCertificate</span></span>](/powershell/module/azurerm.servicefabric/Add-AzureRmServiceFabricApplicationCertificate) | <span data-ttu-id="da931-115">Přidáte že nové aplikace certifikát toohello škálovací sady virtuálních počítačů které tvoří hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="da931-115">Add a new application certificate toohello virtual machine scale set that make up hello cluster.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="da931-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="da931-116">Next steps</span></span>

<span data-ttu-id="da931-117">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="da931-117">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="da931-118">Další ukázky pro Azure Service Fabric Azure Powershell lze nalézt v hello [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="da931-118">Additional Azure Powershell samples for Azure Service Fabric can be found in hello [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
