---
title: "Konfigurace pro vyrovnávání zatížení pro SQL vždy na | Microsoft Docs"
description: "Konfigurace pro vyrovnávání zatížení tak pracovat s SQL vždy na a jak využít prostředí powershell vytvořit nástroj pro vyrovnávání zatížení pro provedení SQL"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: d7bc3790-47d3-4e95-887c-c533011e4afd
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 68aad6253f185d53fdd7f11c8660c7287ef12655
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-load-balancer-for-sql-always-on"></a><span data-ttu-id="17dfd-103">Konfigurace pro vyrovnávání zatížení pro SQL vždy na</span><span class="sxs-lookup"><span data-stu-id="17dfd-103">Configure load balancer for SQL always on</span></span>

<span data-ttu-id="17dfd-104">Skupiny dostupnosti AlwaysOn serveru SQL je teď možné spustit s ILB.</span><span class="sxs-lookup"><span data-stu-id="17dfd-104">SQL Server AlwaysOn Availability Groups can now be run with ILB.</span></span> <span data-ttu-id="17dfd-105">Skupina dostupnosti je řešení nejdůležitějšího SQL serveru pro vysokou dostupnost a zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="17dfd-105">Availability Group is SQL Server's flagship solution for high availability and disaster recovery.</span></span> <span data-ttu-id="17dfd-106">Naslouchací proces skupiny dostupnosti, umožňuje klientským aplikacím bezproblémově připojit k primární replice, bez ohledu na počet replik v konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="17dfd-106">The Availability Group Listener allows client applications to seamlessly connect to the primary replica, irrespective of the number of the replicas in the configuration.</span></span>

<span data-ttu-id="17dfd-107">Název naslouchacího procesu (DNS) je namapovaný na IP adresu s vyrovnáváním zatížení a nástroj pro vyrovnávání zatížení Azure směruje příchozí provoz jenom na primární server sady replik.</span><span class="sxs-lookup"><span data-stu-id="17dfd-107">The listener (DNS) name is mapped to a load-balanced IP address and Azure's load balancer directs the incoming traffic to only the primary server in the replica set.</span></span>

<span data-ttu-id="17dfd-108">Podpora ILB můžete použít pro koncové body SQL Server AlwaysOn (naslouchací proces).</span><span class="sxs-lookup"><span data-stu-id="17dfd-108">You can use ILB support for SQL Server AlwaysOn (listener) endpoints.</span></span> <span data-ttu-id="17dfd-109">Nyní máte kontrolu nad usnadnění naslouchacího procesu a vyberte IP adresu Vyrovnávání zatížení sítě z konkrétní podsítě ve virtuální síti (VNet).</span><span class="sxs-lookup"><span data-stu-id="17dfd-109">You now have control over the accessibility of the listener and can choose the load-balanced IP address from a specific subnet in your Virtual Network (VNet).</span></span>

<span data-ttu-id="17dfd-110">Pomocí ILB na naslouchací proces, koncový bod SQL server (například Server = tcp:ListenerName, 1433; Database = DatabaseName) je přístupná jenom pro:</span><span class="sxs-lookup"><span data-stu-id="17dfd-110">By using ILB on the listener, the SQL server endpoint (e.g. Server=tcp:ListenerName,1433;Database=DatabaseName) is accessible only by:</span></span>

* <span data-ttu-id="17dfd-111">Služby a virtuální počítače ve stejné virtuální síti</span><span class="sxs-lookup"><span data-stu-id="17dfd-111">Services and VMs in the same Virtual network</span></span>
* <span data-ttu-id="17dfd-112">Služeb a virtuálních počítačů z připojených do místní sítě</span><span class="sxs-lookup"><span data-stu-id="17dfd-112">Services and VMs from connected on-premises network</span></span>
* <span data-ttu-id="17dfd-113">Služeb a virtuálních počítačů z vzájemně propojena virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="17dfd-113">Services and VMs from interconnected VNets</span></span>

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

<span data-ttu-id="17dfd-115">Obrázek 1 – nakonfigurovaná pomocí nástroje pro vyrovnávání zatížení internetového technologie AlwaysOn serveru SQL</span><span class="sxs-lookup"><span data-stu-id="17dfd-115">Figure 1 - SQL AlwaysOn configured with Internet-facing load balancer</span></span>

## <a name="add-internal-load-balancer-to-the-service"></a><span data-ttu-id="17dfd-116">Přidejte interní nástroj pro vyrovnávání zatížení do služby</span><span class="sxs-lookup"><span data-stu-id="17dfd-116">Add Internal Load Balancer to the service</span></span>

1. <span data-ttu-id="17dfd-117">V následujícím příkladu jsme nakonfigurovat virtuální síť, která obsahuje podsíť s názvem "Subnet-1.:</span><span class="sxs-lookup"><span data-stu-id="17dfd-117">In the following example, we will configure a Virtual network that contains a subnet  called 'Subnet-1':</span></span>

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. <span data-ttu-id="17dfd-118">Přidat skupinu s vyrovnáváním zatížení koncové body pro ILB na jednotlivé virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="17dfd-118">Add load balanced endpoints for ILB on each VM</span></span>

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    <span data-ttu-id="17dfd-119">V příkladu nahoře, máte názvem "sqlsvc1" a "sqlsvc2" spuštěna 2 Virtuálního počítače v cloudu služby "Sqlsvc".</span><span class="sxs-lookup"><span data-stu-id="17dfd-119">In the example above, you have 2 VM's called "sqlsvc1" and "sqlsvc2" running in the cloud service "Sqlsvc".</span></span> <span data-ttu-id="17dfd-120">Po vytvoření ILB s `DirectServerReturn` přepínače, můžete přidat skupinu s vyrovnáváním zatížení koncové body k ILB umožňující nakonfigurovat naslouchací procesy pro skupiny dostupnosti SQL.</span><span class="sxs-lookup"><span data-stu-id="17dfd-120">After creating the ILB with `DirectServerReturn` switch, you add load balanced endpoints to the ILB to allow SQL to configure the listeners for the availability groups.</span></span>

<span data-ttu-id="17dfd-121">Další informace o SQL AlwaysOn najdete v tématu [konfigurace interní nástroj pro skupinu dostupnosti AlwaysOn v Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span><span class="sxs-lookup"><span data-stu-id="17dfd-121">For more information about SQL AlwaysOn, see [Configure an internal load balancer for an AlwaysOn availability group in Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="17dfd-122">Viz také</span><span class="sxs-lookup"><span data-stu-id="17dfd-122">See Also</span></span>
[<span data-ttu-id="17dfd-123">Začít konfigurovat internetové nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="17dfd-123">Get started configuring an Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="17dfd-124">Začněte konfiguraci Vyrovnávání zatížení interní</span><span class="sxs-lookup"><span data-stu-id="17dfd-124">Get started configuring an Internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="17dfd-125">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="17dfd-125">Configure a Load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="17dfd-126">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="17dfd-126">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
