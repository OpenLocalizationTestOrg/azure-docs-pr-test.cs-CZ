---
title: "aaaCreate směřujících Internetu pro vyrovnávání zátěže – prostředí PowerShell Azure classic | Microsoft Docs"
description: "Zjistěte, jak toocreate přístupem Internetu pro vyrovnávání zátěže v klasickém režimu pomocí prostředí PowerShell"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: 73e8bfa4-8086-4ef0-9e35-9e00b24be319
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 76d9a712a0acda223fc86b80be9c35c0ed9f3a50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a><span data-ttu-id="d3362-103">Začínáme vytvářet internetový nástroj pro vyrovnávání zatížení (Classic) v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d3362-103">Get started creating an Internet facing load balancer (classic) in PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="d3362-104">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="d3362-104">Azure classic portal</span></span>](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [<span data-ttu-id="d3362-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d3362-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [<span data-ttu-id="d3362-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d3362-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [<span data-ttu-id="d3362-107">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="d3362-107">Azure Cloud Services</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="d3362-108">Než začnete pracovat s prostředky Azure, je důležité toounderstand Azure aktuálně má dva modely nasazení: Azure Resource Manager a Klasický model.</span><span class="sxs-lookup"><span data-stu-id="d3362-108">Before you work with Azure resources, it's important toounderstand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="d3362-109">Před zahájením práce s jakýmikoli prostředky Azure se ujistěte, že rozumíte [modelům nasazení a příslušným nástrojům](../azure-classic-rm.md).</span><span class="sxs-lookup"><span data-stu-id="d3362-109">Make sure you understand [deployment models and tools](../azure-classic-rm.md) before you work with any Azure resource.</span></span> <span data-ttu-id="d3362-110">Hello dokumentaci k různým nástrojům můžete zobrazit kliknutím na karty hello hello horní části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="d3362-110">You can view hello documentation for different tools by clicking hello tabs at hello top of this article.</span></span> <span data-ttu-id="d3362-111">Tento článek se týká modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="d3362-111">This article covers hello classic deployment model.</span></span> <span data-ttu-id="d3362-112">Můžete také [zjistěte, jak toocreate přístupem Internetu pro vyrovnávání zátěže pomocí Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="d3362-112">You can also [Learn how toocreate an Internet facing load balancer using Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-load-balancer-using-powershell"></a><span data-ttu-id="d3362-113">Nastavení nástroje pro vyrovnávání zatížení pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d3362-113">Set up load balancer using PowerShell</span></span>

<span data-ttu-id="d3362-114">tooset si nástroj pro vyrovnávání zatížení pomocí prostředí powershell, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="d3362-114">tooset up a load balancer using powershell, follow hello steps below:</span></span>

1. <span data-ttu-id="d3362-115">Pokud jste prostředí Azure PowerShell nikdy nepoužívali, projděte si téma [jak tooInstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) a postupujte podle pokynů hello všechny toohello hello způsob ukončení toosign do Azure a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="d3362-115">If you have never used Azure PowerShell, see [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) and follow hello instructions all hello way toohello end toosign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="d3362-116">Po vytvoření virtuálního počítače, můžete použít tooadd rutiny prostředí PowerShell nástroje pro vyrovnávání zatížení tooa virtuálního počítače v rámci hello stejné cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="d3362-116">After creating a virtual machine, you can use PowerShell cmdlets tooadd a load balancer tooa virtual machine within hello same cloud service.</span></span>

<span data-ttu-id="d3362-117">V hello následující příklad, že přidáte skupinu nástroje pro vyrovnávání zatížení názvem toocloud "webová farma" služby "mytestcloud" (nebo myctestcloud.cloudapp.net), přidání toovirtual nástroje pro vyrovnávání zátěže hello koncové body pro hello počítačů s názvem "web1" a "web2".</span><span class="sxs-lookup"><span data-stu-id="d3362-117">In hello following example you will add a load balancer set called "webfarm" toocloud service "mytestcloud" (or myctestcloud.cloudapp.net) , adding hello endpoints for hello load balancer toovirtual machines named "web1" and "web2".</span></span> <span data-ttu-id="d3362-118">Hello nástroj pro vyrovnávání zatížení přijímá síťový provoz na portu 80 a vyrovnává zatížení mezi virtuálními počítači hello definované hello místní koncový bod (v této případu port 80) pomocí protokolu TCP.</span><span class="sxs-lookup"><span data-stu-id="d3362-118">hello load balancer receives network traffic on port 80 and load balances between hello virtual machines defined by hello local endpoint (in this case port 80) using TCP.</span></span>

### <a name="step-1"></a><span data-ttu-id="d3362-119">Krok 1</span><span class="sxs-lookup"><span data-stu-id="d3362-119">Step 1</span></span>

<span data-ttu-id="d3362-120">Vytvořte skupinu s vyrovnáváním zatížení koncový bod pro server web1"hello první virtuální počítač"</span><span class="sxs-lookup"><span data-stu-id="d3362-120">Create a load balanced endpoint for hello first VM "web1"</span></span>

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

### <a name="step-2"></a><span data-ttu-id="d3362-121">Krok 2</span><span class="sxs-lookup"><span data-stu-id="d3362-121">Step 2</span></span>

<span data-ttu-id="d3362-122">Vytvořte jiný koncový bod pro hello druhý virtuální počítač "webu 2" hello pomocí stejné načíst název sady vyrovnávání</span><span class="sxs-lookup"><span data-stu-id="d3362-122">Create another endpoint for hello second VM  "web2" using hello same load balancer set name</span></span>

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a><span data-ttu-id="d3362-123">Odebrání virtuálního počítače z nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="d3362-123">Remove a virtual machine from a load balancer</span></span>

<span data-ttu-id="d3362-124">Můžete odebrat AzureEndpoint tooremove koncový bod virtuálního počítače z nástroje pro vyrovnávání zatížení hello</span><span class="sxs-lookup"><span data-stu-id="d3362-124">You can use Remove-AzureEndpoint tooremove a virtual machine endpoint from hello load balancer</span></span>

```powershell
Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin | Update-AzureVM
```

## <a name="next-steps"></a><span data-ttu-id="d3362-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d3362-125">Next steps</span></span>

<span data-ttu-id="d3362-126">Můžete také [začít vytvářet interní nástroj pro vyrovnávání zatížení](load-balancer-get-started-ilb-classic-ps.md) a nakonfigurovat, jaký typ [distribučního režimu](load-balancer-distribution-mode.md) se má použít pro konkrétní chování nástroje pro vyrovnávání zatížení síťového provozu.</span><span class="sxs-lookup"><span data-stu-id="d3362-126">You can also [get started creating an internal load balancer](load-balancer-get-started-ilb-classic-ps.md) and configure what type of [distribution mode](load-balancer-distribution-mode.md) for a specific load balancer network traffic behavior.</span></span>

<span data-ttu-id="d3362-127">Pokud aplikace potřebuje tookeep připojení zachování připojení pro servery za službou Vyrovnávání zatížení, můžete lépe porozumět o [nečinnosti nastavení časového limitu TCP pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md).</span><span class="sxs-lookup"><span data-stu-id="d3362-127">If your application needs tookeep connections alive for servers behind a load balancer, you can understand more about [idle TCP timeout settings for a load balancer](load-balancer-tcp-idle-timeout.md).</span></span> <span data-ttu-id="d3362-128">Pokud používáte nástroj pro vyrovnávání zatížení Azure ho pomůže toolearn o chování nečinné připojení.</span><span class="sxs-lookup"><span data-stu-id="d3362-128">It will help toolearn about idle connection behavior when you are using Azure Load Balancer.</span></span>
