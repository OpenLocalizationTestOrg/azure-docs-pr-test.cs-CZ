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
# <a name="configure-load-balancer-for-sql-always-on"></a>Konfigurace pro vyrovnávání zatížení pro SQL vždy na

Skupiny dostupnosti AlwaysOn serveru SQL je teď možné spustit s ILB. Skupina dostupnosti je řešení nejdůležitějšího SQL serveru pro vysokou dostupnost a zotavení po havárii. Hello naslouchací proces skupiny dostupnosti umožňuje získat klientským aplikacím tooseamlessly připojit primární repliky toohello, bez ohledu na hello počet replik hello v konfiguraci hello.

Název naslouchacího procesu (DNS) Hello je IP Adresa mapovaná tooa Vyrovnávání zatížení sítě a nástroje pro vyrovnávání zatížení Azure směruje hello příchozí provoz tooonly hello primární server hello sady replik.

Podpora ILB můžete použít pro koncové body SQL Server AlwaysOn (naslouchací proces). Nyní máte kontrolu nad hello usnadnění hello naslouchacího procesu a můžete zvolit hello Vyrovnávání zatížení sítě IP adresu z konkrétní podsítě ve virtuální síti (VNet).

Pomocí ILB na naslouchací proces hello hello koncový bod SQL server (například Server = tcp:ListenerName, 1433; Database = DatabaseName) je přístupná jenom pro:

* Služby a virtuální počítače ve stejné virtuální síti hello
* Služeb a virtuálních počítačů z připojených do místní sítě
* Služeb a virtuálních počítačů z vzájemně propojena virtuální sítě

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

Obrázek 1 – nakonfigurovaná pomocí nástroje pro vyrovnávání zatížení internetového technologie AlwaysOn serveru SQL

## <a name="add-internal-load-balancer-toohello-service"></a>Přidat službu toohello interní nástroj pro vyrovnávání zatížení

1. V následujícím příkladu hello jsme se nakonfigurovat virtuální síť, která obsahuje podsíť s názvem "Subnet-1.:

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. Přidat skupinu s vyrovnáváním zatížení koncové body pro ILB na jednotlivé virtuální počítače

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    V předchozím příkladu hello, máte názvem "sqlsvc1" a "sqlsvc2" spuštěna 2 Virtuálního počítače v cloudu hello služby "Sqlsvc". Po vytvoření hello ILB s `DirectServerReturn` přepnout, přidáte načíst vyrovnáváním koncové body toohello ILB tooallow SQL tooconfigure hello naslouchací procesy pro skupiny dostupnosti hello.

Další informace o SQL AlwaysOn najdete v tématu [konfigurace interní nástroj pro skupinu dostupnosti AlwaysOn v Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Viz také
[Začít konfigurovat internetové nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md)

[Začněte konfiguraci Vyrovnávání zatížení interní](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
