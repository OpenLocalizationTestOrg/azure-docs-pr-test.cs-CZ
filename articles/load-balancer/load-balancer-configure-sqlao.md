---
title: "aaaConfigure Vyrovnávání zatížení pro SQL always on | Microsoft Docs"
description: "Nakonfigurujte toowork nástroje pro vyrovnávání zatížení s SQL vždy na a jak tooleverage prostředí powershell toocreate službu Vyrovnávání zatížení pro hello SQL – implementace"
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
ms.openlocfilehash: ac6200b18f725dadee2555b80055327d379417d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-load-balancer-for-sql-always-on"></a><span data-ttu-id="28933-103">Konfigurace pro vyrovnávání zatížení pro SQL vždy na</span><span class="sxs-lookup"><span data-stu-id="28933-103">Configure load balancer for SQL always on</span></span>

<span data-ttu-id="28933-104">Skupiny dostupnosti AlwaysOn serveru SQL je teď možné spustit s ILB.</span><span class="sxs-lookup"><span data-stu-id="28933-104">SQL Server AlwaysOn Availability Groups can now be run with ILB.</span></span> <span data-ttu-id="28933-105">Skupina dostupnosti je řešení nejdůležitějšího SQL serveru pro vysokou dostupnost a zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="28933-105">Availability Group is SQL Server's flagship solution for high availability and disaster recovery.</span></span> <span data-ttu-id="28933-106">Hello naslouchací proces skupiny dostupnosti umožňuje získat klientským aplikacím tooseamlessly připojit primární repliky toohello, bez ohledu na hello počet replik hello v konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="28933-106">hello Availability Group Listener allows client applications tooseamlessly connect toohello primary replica, irrespective of hello number of hello replicas in hello configuration.</span></span>

<span data-ttu-id="28933-107">Název naslouchacího procesu (DNS) Hello je IP Adresa mapovaná tooa Vyrovnávání zatížení sítě a nástroje pro vyrovnávání zatížení Azure směruje hello příchozí provoz tooonly hello primární server hello sady replik.</span><span class="sxs-lookup"><span data-stu-id="28933-107">hello listener (DNS) name is mapped tooa load-balanced IP address and Azure's load balancer directs hello incoming traffic tooonly hello primary server in hello replica set.</span></span>

<span data-ttu-id="28933-108">Podpora ILB můžete použít pro koncové body SQL Server AlwaysOn (naslouchací proces).</span><span class="sxs-lookup"><span data-stu-id="28933-108">You can use ILB support for SQL Server AlwaysOn (listener) endpoints.</span></span> <span data-ttu-id="28933-109">Nyní máte kontrolu nad hello usnadnění hello naslouchacího procesu a můžete zvolit hello Vyrovnávání zatížení sítě IP adresu z konkrétní podsítě ve virtuální síti (VNet).</span><span class="sxs-lookup"><span data-stu-id="28933-109">You now have control over hello accessibility of hello listener and can choose hello load-balanced IP address from a specific subnet in your Virtual Network (VNet).</span></span>

<span data-ttu-id="28933-110">Pomocí ILB na naslouchací proces hello hello koncový bod SQL server (například Server = tcp:ListenerName, 1433; Database = DatabaseName) je přístupná jenom pro:</span><span class="sxs-lookup"><span data-stu-id="28933-110">By using ILB on hello listener, hello SQL server endpoint (e.g. Server=tcp:ListenerName,1433;Database=DatabaseName) is accessible only by:</span></span>

* <span data-ttu-id="28933-111">Služby a virtuální počítače ve stejné virtuální síti hello</span><span class="sxs-lookup"><span data-stu-id="28933-111">Services and VMs in hello same Virtual network</span></span>
* <span data-ttu-id="28933-112">Služeb a virtuálních počítačů z připojených do místní sítě</span><span class="sxs-lookup"><span data-stu-id="28933-112">Services and VMs from connected on-premises network</span></span>
* <span data-ttu-id="28933-113">Služeb a virtuálních počítačů z vzájemně propojena virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="28933-113">Services and VMs from interconnected VNets</span></span>

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

<span data-ttu-id="28933-115">Obrázek 1 – nakonfigurovaná pomocí nástroje pro vyrovnávání zatížení internetového technologie AlwaysOn serveru SQL</span><span class="sxs-lookup"><span data-stu-id="28933-115">Figure 1 - SQL AlwaysOn configured with Internet-facing load balancer</span></span>

## <a name="add-internal-load-balancer-toohello-service"></a><span data-ttu-id="28933-116">Přidat službu toohello interní nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="28933-116">Add Internal Load Balancer toohello service</span></span>

1. <span data-ttu-id="28933-117">V následujícím příkladu hello jsme se nakonfigurovat virtuální síť, která obsahuje podsíť s názvem "Subnet-1.:</span><span class="sxs-lookup"><span data-stu-id="28933-117">In hello following example, we will configure a Virtual network that contains a subnet  called 'Subnet-1':</span></span>

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. <span data-ttu-id="28933-118">Přidat skupinu s vyrovnáváním zatížení koncové body pro ILB na jednotlivé virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="28933-118">Add load balanced endpoints for ILB on each VM</span></span>

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    <span data-ttu-id="28933-119">V předchozím příkladu hello, máte názvem "sqlsvc1" a "sqlsvc2" spuštěna 2 Virtuálního počítače v cloudu hello služby "Sqlsvc".</span><span class="sxs-lookup"><span data-stu-id="28933-119">In hello example above, you have 2 VM's called "sqlsvc1" and "sqlsvc2" running in hello cloud service "Sqlsvc".</span></span> <span data-ttu-id="28933-120">Po vytvoření hello ILB s `DirectServerReturn` přepnout, přidáte načíst vyrovnáváním koncové body toohello ILB tooallow SQL tooconfigure hello naslouchací procesy pro skupiny dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="28933-120">After creating hello ILB with `DirectServerReturn` switch, you add load balanced endpoints toohello ILB tooallow SQL tooconfigure hello listeners for hello availability groups.</span></span>

<span data-ttu-id="28933-121">Další informace o SQL AlwaysOn najdete v tématu [konfigurace interní nástroj pro skupinu dostupnosti AlwaysOn v Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span><span class="sxs-lookup"><span data-stu-id="28933-121">For more information about SQL AlwaysOn, see [Configure an internal load balancer for an AlwaysOn availability group in Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="28933-122">Viz také</span><span class="sxs-lookup"><span data-stu-id="28933-122">See Also</span></span>
[<span data-ttu-id="28933-123">Začít konfigurovat internetové nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="28933-123">Get started configuring an Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="28933-124">Začněte konfiguraci Vyrovnávání zatížení interní</span><span class="sxs-lookup"><span data-stu-id="28933-124">Get started configuring an Internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="28933-125">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="28933-125">Configure a Load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="28933-126">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="28933-126">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
