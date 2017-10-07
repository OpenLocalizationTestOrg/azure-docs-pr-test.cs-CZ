---
title: "aaaCreate pravidlo Vyrovnávání zatížení Azure pro cluster"
description: "Konfigurace vyrovnávání zatížení Azure tooopen portů pro váš cluster Azure Service Fabric."
services: service-fabric
documentationcenter: na
author: thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/22/2017
ms.author: adegeo
ms.openlocfilehash: 4a40f62422bd895d782be8cbaace5f4e1af81db3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="open-ports-for-a-service-fabric-cluster"></a><span data-ttu-id="75860-103">Otevřete porty pro cluster Service Fabric</span><span class="sxs-lookup"><span data-stu-id="75860-103">Open ports for a Service Fabric cluster</span></span>

<span data-ttu-id="75860-104">Nástroj pro vyrovnávání zatížení Hello nasazené se službou Azure Service Fabric cluster bude směrovat provoz tooyour aplikaci spuštěnou na uzlu.</span><span class="sxs-lookup"><span data-stu-id="75860-104">hello load balancer deployed with your Azure Service Fabric cluster directs traffic tooyour app running on a node.</span></span> <span data-ttu-id="75860-105">Pokud změníte vaší aplikace toouse jiný port, musí vystavit tento port (nebo směrovat jiný port) v hello nástroj pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="75860-105">If you change your app toouse a different port, you must expose that port (or route a different port) in hello Azure Load Balancer.</span></span>

<span data-ttu-id="75860-106">Při nasazení vaší tooAzure fabric clusteru služby Vyrovnávání zatížení byl automaticky vytvořen za vás.</span><span class="sxs-lookup"><span data-stu-id="75860-106">When you deployed your service fabric cluster tooAzure, a load balancer was automatically created for you.</span></span> <span data-ttu-id="75860-107">Pokud jste nástroj pro vyrovnávání zatížení, přečtěte si téma [konfigurace vyrovnávání zatížení internetového](..\load-balancer\load-balancer-get-started-internet-portal.md).</span><span class="sxs-lookup"><span data-stu-id="75860-107">If you do not have a load balancer, see [Configure an Internet-facing load balancer](..\load-balancer\load-balancer-get-started-internet-portal.md).</span></span>

## <a name="configure-service-fabric"></a><span data-ttu-id="75860-108">Konfigurace infrastruktury služby</span><span class="sxs-lookup"><span data-stu-id="75860-108">Configure service fabric</span></span>

<span data-ttu-id="75860-109">Aplikace Service Fabric **ServiceManifest.xml** definuje koncové body hello aplikace očekává toouse konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="75860-109">Your Service Fabric application **ServiceManifest.xml** config file defines hello endpoints your application expects toouse.</span></span> <span data-ttu-id="75860-110">Po hello konfigurační soubor byl aktualizovaný toodefine koncový bod, hello nástroj pro vyrovnávání zatížení musí být aktualizované tooexpose tento (nebo jiný) port.</span><span class="sxs-lookup"><span data-stu-id="75860-110">After hello config file has been updated toodefine an endpoint, hello load balancer must be updated tooexpose that (or a different) port.</span></span> <span data-ttu-id="75860-111">Další informace o tom, jak toocreate hello koncový bod prostředků infrastruktury služby najdete v tématu [nastavit koncový bod](service-fabric-service-manifest-resources.md).</span><span class="sxs-lookup"><span data-stu-id="75860-111">For more information on how toocreate hello service fabric endpoint, see [Setup an Endpoint](service-fabric-service-manifest-resources.md).</span></span>

## <a name="create-a-load-balancer-rule"></a><span data-ttu-id="75860-112">Vytvořit pravidlo Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="75860-112">Create a load balancer rule</span></span>

<span data-ttu-id="75860-113">Pravidlo Vyrovnávání zatížení otevře k portu internetového a předá provoz toohello interní uzlu portu, který používá vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="75860-113">A load balancer rule opens up an internet-facing port and forwards traffic toohello internal node's port used by your application.</span></span> <span data-ttu-id="75860-114">Pokud jste nástroj pro vyrovnávání zatížení, přečtěte si téma [konfigurace vyrovnávání zatížení internetového](..\load-balancer\load-balancer-get-started-internet-portal.md).</span><span class="sxs-lookup"><span data-stu-id="75860-114">If you do not have a load balancer, see [Configure an Internet-facing load balancer](..\load-balancer\load-balancer-get-started-internet-portal.md).</span></span>

<span data-ttu-id="75860-115">toocreate pravidlo Vyrovnávání zatížení, budete potřebovat toocollect hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="75860-115">toocreate a load balancer rule, you need toocollect hello following information:</span></span>

- <span data-ttu-id="75860-116">Název služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="75860-116">Load balancer name.</span></span>
- <span data-ttu-id="75860-117">Skupiny prostředků hello zatížení vyrovnávání a service fabric cluster.</span><span class="sxs-lookup"><span data-stu-id="75860-117">Resource group of hello load balancer and service fabric cluster.</span></span>
- <span data-ttu-id="75860-118">Externí port.</span><span class="sxs-lookup"><span data-stu-id="75860-118">External port.</span></span>
- <span data-ttu-id="75860-119">Interní port.</span><span class="sxs-lookup"><span data-stu-id="75860-119">Internal port.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="75860-120">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="75860-120">Azure CLI</span></span>
>[!NOTE]
><span data-ttu-id="75860-121">Pokud potřebujete název hello toodetermine hello nástroj pro vyrovnávání zatížení, použijte tento příkaz get tooquickly seznam všechny skupiny prostředků hello přidružené a nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="75860-121">If you need toodetermine hello name of hello load balancer, use this command tooquickly get a list of all load balancers and hello associated resource groups.</span></span>
>
>`az network lb list --query "[].{ResourceGroup: resourceGroup, Name: name}"`
>

<span data-ttu-id="75860-122">Potrvá to jenom jeden příkaz toocreate pravidlo Vyrovnávání zatížení s hello **rozhraní příkazového řádku Azure**.</span><span class="sxs-lookup"><span data-stu-id="75860-122">It only takes a single command toocreate a load balancer rule with hello **Azure CLI**.</span></span> <span data-ttu-id="75860-123">Stačí tooknow obou hello název hello zatížení vyrovnávání a prostředků skupiny toocreate nové pravidlo.</span><span class="sxs-lookup"><span data-stu-id="75860-123">You just need tooknow both hello name of hello load balancer and resource group toocreate a new rule.</span></span>

```azurecli
az network lb rule create --backend-port 40000 --frontend-port 39999 --protocol Tcp --lb-name LB-svcfab3 -g svcfab_cli -n my-app-rule
```

<span data-ttu-id="75860-124">Hello příkazu příkazového řádku Azure CLI má několik parametrů, které jsou popsány v následující tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="75860-124">hello Azure CLI command has a few parameters that are described in hello following table:</span></span>

| <span data-ttu-id="75860-125">Parametr</span><span class="sxs-lookup"><span data-stu-id="75860-125">Parameter</span></span> | <span data-ttu-id="75860-126">Popis</span><span class="sxs-lookup"><span data-stu-id="75860-126">Description</span></span> |
| --------- | ----------- |
| `--backend-port`  | <span data-ttu-id="75860-127">Hello port hello service fabric aplikace naslouchá.</span><span class="sxs-lookup"><span data-stu-id="75860-127">hello port hello service fabric application is listening to.</span></span> |
| `--frontend-port` | <span data-ttu-id="75860-128">Hello port hello načíst zpřístupňuje vyrovnávání pro externí připojení.</span><span class="sxs-lookup"><span data-stu-id="75860-128">hello port hello load balancer exposes for external connections.</span></span> |
| `-lb-name` | <span data-ttu-id="75860-129">Název Hello hello načíst toochange vyrovnávání.</span><span class="sxs-lookup"><span data-stu-id="75860-129">hello name of hello load balancer toochange.</span></span> |
| `-g`       | <span data-ttu-id="75860-130">Hello skupinu prostředků, který má nástroj pro vyrovnávání zatížení hello a service fabric cluster.</span><span class="sxs-lookup"><span data-stu-id="75860-130">hello resource group that has both hello load balancer and service fabric cluster.</span></span> |
| `-n`       | <span data-ttu-id="75860-131">Zvolený název pravidla hello Hello.</span><span class="sxs-lookup"><span data-stu-id="75860-131">hello chosen name of hello rule.</span></span> |


>[!NOTE]
><span data-ttu-id="75860-132">Další informace o tom, jak toocreate Vyrovnávání zatížení s hello Azure CLI, najdete v části [vytvořit nástroj pro vyrovnávání zatížení s hello rozhraní příkazového řádku Azure](..\load-balancer\load-balancer-get-started-internet-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="75860-132">For more information on how toocreate a load balancer with hello Azure CLI, see [Create a load balancer with hello Azure CLI](..\load-balancer\load-balancer-get-started-internet-arm-cli.md).</span></span>

## <a name="powershell"></a><span data-ttu-id="75860-133">PowerShell</span><span class="sxs-lookup"><span data-stu-id="75860-133">PowerShell</span></span>

>[!NOTE]
><span data-ttu-id="75860-134">Pokud potřebujete název hello toodetermine hello nástroj pro vyrovnávání zatížení, použijte tento příkaz get tooquickly seznam všechny skupiny přidružených prostředků a nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="75860-134">If you need toodetermine hello name of hello load balancer, use this command tooquickly get a list of all load balancers and associated resource groups.</span></span>
>
>`Get-AzureRmLoadBalancer | Select Name, ResourceGroupName`

<span data-ttu-id="75860-135">PowerShell je trochu složitější než hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="75860-135">PowerShell is a little more complicated than hello Azure CLI.</span></span> <span data-ttu-id="75860-136">Koncepčně proveďte následující kroky toocreate hello pravidlo.</span><span class="sxs-lookup"><span data-stu-id="75860-136">Conceptually, do hello following steps toocreate a rule.</span></span>

1. <span data-ttu-id="75860-137">Nástroj pro vyrovnávání zatížení hello získáte ze služby Azure.</span><span class="sxs-lookup"><span data-stu-id="75860-137">Get hello load balancer from Azure.</span></span>
2. <span data-ttu-id="75860-138">Vytvořte pravidlo.</span><span class="sxs-lookup"><span data-stu-id="75860-138">Create a rule.</span></span>
3. <span data-ttu-id="75860-139">Přidejte hello pravidlo toohello vyrovnávání zátěže.</span><span class="sxs-lookup"><span data-stu-id="75860-139">Add hello rule toohello load balancer.</span></span>
4. <span data-ttu-id="75860-140">Aktualizujte nástroj pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="75860-140">Update hello load balancer.</span></span>

```powershell
# Get hello load balancer
$lb = Get-AzureRmLoadBalancer -Name LB-svcfab3 -ResourceGroupName svcfab_cli

# Create hello rule based on information from hello load balancer.
$lbrule = New-AzureRmLoadBalancerRuleConfig -Name my-app-rule7 -Protocol Tcp -FrontendPort 39990 -BackendPort 40009 `
                                            -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
                                            -BackendAddressPool  $lb.BackendAddressPools[0] `
                                            -Probe $lb.Probes[0]

# Add hello rule toohello load balancer
$lb.LoadBalancingRules.Add($lbrule)

# Update hello load balancer on Azure
$lb | Set-AzureRmLoadBalancer
```

<span data-ttu-id="75860-141">O hello `New-AzureRmLoadBalancerRuleConfig` příkaz hello `-FrontendPort` představuje hello port hello nástroj pro vyrovnávání zatížení vystaví pro externí připojení a hello `-BackendPort` prostředků infrastruktury aplikace hello service představuje hello portu naslouchá.</span><span class="sxs-lookup"><span data-stu-id="75860-141">Regarding hello `New-AzureRmLoadBalancerRuleConfig` command, hello `-FrontendPort` represents hello port hello load balancer exposes for external connections, and hello `-BackendPort` represents hello port hello service fabric app is listening to.</span></span>

>[!NOTE]
><span data-ttu-id="75860-142">Další informace o tom, jak toocreate Vyrovnávání zatížení v prostředí PowerShell najdete v části [vytvořit nástroj pro vyrovnávání zatížení v prostředí PowerShell](..\load-balancer\load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="75860-142">For more information on how toocreate a load balancer with PowerShell, see [Create a load balancer with PowerShell](..\load-balancer\load-balancer-get-started-internet-arm-ps.md).</span></span>

