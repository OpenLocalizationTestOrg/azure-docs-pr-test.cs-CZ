---
title: "Azure skript prostředí PowerShell ukázkový – přidání certifikátu aplikací do clusteru s podporou | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – přidání certifikát aplikací do clusteru Service Fabric."
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
ms.openlocfilehash: 9ccd6bb0458bc03e52103fa70cad26bd6bf98bd5
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a><span data-ttu-id="c2edc-103">Přidat certifikát aplikací do clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c2edc-103">Add an application certificate to a Service Fabric cluster</span></span>

<span data-ttu-id="c2edc-104">Tento ukázkový skript vytvoří certifikát podepsaný svým držitelem v zadané Azure key vault a nainstaluje na všech uzlech tohoto clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="c2edc-104">This sample script creates a self-signed certificate in the specified Azure key vault and installs it to all nodes of the Service Fabric cluster.</span></span> <span data-ttu-id="c2edc-105">Certifikát se také stáhne do místní složky.</span><span class="sxs-lookup"><span data-stu-id="c2edc-105">The certificate also downloads to a local folder.</span></span> <span data-ttu-id="c2edc-106">Název stažený certifikát je stejný jako název certifikátu v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="c2edc-106">The name of the downloaded certificate is the same as the name of the certificate in the key vault.</span></span> <span data-ttu-id="c2edc-107">Podle potřeby upravte požadované parametry.</span><span class="sxs-lookup"><span data-stu-id="c2edc-107">Customize the parameters as needed.</span></span>

<span data-ttu-id="c2edc-108">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview) a spusťte `Login-AzureRmAccount` vytvořit připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="c2edc-108">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview) and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span> 

## <a name="sample-script"></a><span data-ttu-id="c2edc-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="c2edc-109">Sample script</span></span>

<span data-ttu-id="c2edc-110">[!code-powershell[hlavní](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "přidat certifikát aplikací do clusteru")]</span><span class="sxs-lookup"><span data-stu-id="c2edc-110">[!code-powershell[main](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "Add an application certificate to a cluster")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="c2edc-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="c2edc-111">Script explanation</span></span>

<span data-ttu-id="c2edc-112">Tento skript používá následující příkazy: každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="c2edc-112">This script uses the following commands: Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="c2edc-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="c2edc-113">Command</span></span> | <span data-ttu-id="c2edc-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="c2edc-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c2edc-115">Přidat AzureRmServiceFabricApplicationCertificate</span><span class="sxs-lookup"><span data-stu-id="c2edc-115">Add-AzureRmServiceFabricApplicationCertificate</span></span>](/powershell/module/azurerm.servicefabric/Add-AzureRmServiceFabricApplicationCertificate) | <span data-ttu-id="c2edc-116">Přidáte nový certifikát aplikace do sady škálování virtuálního počítače, který vytváří cluster.</span><span class="sxs-lookup"><span data-stu-id="c2edc-116">Add a new application certificate to the virtual machine scale set that make up the cluster.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="c2edc-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c2edc-117">Next steps</span></span>

<span data-ttu-id="c2edc-118">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c2edc-118">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="c2edc-119">Další ukázky pro Azure Service Fabric Azure Powershell lze nalézt v [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c2edc-119">Additional Azure Powershell samples for Azure Service Fabric can be found in the [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
