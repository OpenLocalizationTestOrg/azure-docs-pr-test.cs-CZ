---
title: "aaaCreate interní Azure pro vyrovnávání zátěže - PowerShell classic | Microsoft Docs"
description: "Zjistěte, jak toocreate na interní nástroj pro vyrovnávání pomocí prostředí PowerShell v modelu nasazení classic hello zatížení"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 3be93168-3787-45a5-a194-9124fe386493
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 382db80c42ffab09905513019b72e85a4f9dfeff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a>Začínáme vytvářet interní nástroj pro vyrovnávání zatížení (Classic) pomocí prostředí PowerShell

> [!div class="op_single_selector"]
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [Cloudové služby](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).  Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Zjistěte, jak příliš[proveďte tyto kroky, pomocí modelu Resource Manager hello](load-balancer-get-started-ilb-arm-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a>Vytvoření sady interního nástroje pro vyrovnávání zatížení pro virtuální počítače

toocreate interní nástroj nastavit a hello servery, které se odesílají tooit jejich provoz, máte následující toodo hello:

1. Vytvoření instance interní Vyrovnávání zatížení, bude koncový bod hello příchozí provoz toobe vyrovnáváno zatížení napříč servery hello sady Vyrovnávání zatížení sítě.
2. Přidáte koncové body odpovídající toohello virtuálních počítačů, které bude moci přijmout příchozí provoz hello.
3. Konfiguraci hello serverů, které se budou odesílat, že hello provoz toobe s vyrovnáváním zatížení se toosend jejich provoz toohello virtuální adresa IP (VIP) instance hello interní Vyrovnávání zatížení.

### <a name="step-1-create-an-internal-load-balancing-instance"></a>Krok 1: Vytvoření instance interního vyrovnávání zatížení

Pro stávající cloudovou službu nebo cloudové služby nasadit v rámci regionální virtuální síť můžete vytvořit instanci interní Vyrovnávání zatížení s hello následující příkazy prostředí Windows PowerShell:

```powershell
$svc="<Cloud Service Name>"
$ilb="<Name of your ILB instance>"
$subnet="<Name of hello subnet within your virtual network>"
$IP="<hello IPv4 address toouse on hello subnet-optional>"

Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP
```

Všimněte si, že toto využití hello [přidat AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) rutiny prostředí Windows PowerShell používá sada parametrů DefaultProbe hello. Více informací o dalších sadách parametrů najdete v dokumentaci k rutině [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).

### <a name="step-2-add-endpoints-toohello-internal-load-balancing-instance"></a>Krok 2: Přidáte instanci interní Vyrovnávání zatížení toohello koncové body

Zde naleznete příklad:

```powershell
$svc="mytestcloud"
$vmname="DB1"
$epname="TCP-1433-1433"
$lbsetname="lbset"
$prot="tcp"
$locport=1433
$pubport=1433
$ilb="ilbset"
Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -Lbset $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM
```

### <a name="step-3-configure-your-servers-toosend-their-traffic-toohello-new-internal-load-balancing-endpoint"></a>Krok 3: Konfigurace vaše servery toosend jejich provoz toohello nový interní Vyrovnávání zatížení koncového bodu

Nakonfigurujete mít příliš hello servery, jejichž provoz je probíhající toobe skupinu s vyrovnáváním zatížení toouse hello novou IP adresu (hello VIP) hello instanci interní Vyrovnávání zatížení. Toto je adresa hello, na které hello interní Vyrovnávání zatížení naslouchá instance. Ve většině případů potřebujete toojust přidat nebo upravit záznam DNS pro hello VIP instance hello interní Vyrovnávání zatížení.

Pokud jste zadali IP adresu hello během vytváření hello instance hello interní Vyrovnávání zatížení, už máte hello VIP. Jinak uvidíte hello VIP z hello následující příkazy:

```powershell
$svc="<Cloud Service Name>"
Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer
```

toouse tyto příkazy, vyplňte hodnoty hello a odebrat hello < a >. Zde naleznete příklad:

```powershell
$svc="mytestcloud"
Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer
```

Z hello zobrazení hello příkaz Get-AzureInternalLoadBalancer poznamenejte si hello IP adresu a zajistěte, aby hello potřebné změny tooyour servery nebo tooensure záznamy DNS, který získá data odeslaná toohello VIP.

> [!NOTE]
> Platforma Microsoft Azure Hello používá statické veřejně směrovatelné IPv4 adresu pro různé scénáře pro správu. Hello IP adresa je 168.63.129.16. Tuto IP adresu by neměla blokovat žádná brána firewall, protože by to mohlo způsobit neočekávané chování.
> S ohledem tooAzure interní Vyrovnávání zatížení tato IP adresa je používán monitorování sondy z hello zatížení vyrovnávání toodetermine hello stav pro virtuální počítače v skupinu s vyrovnáváním zatížení. Pokud skupina zabezpečení sítě je použité toorestrict provoz tooAzure virtuálních počítačů v sadu interně Vyrovnávání zatížení sítě nebo je použité tooa podsíť virtuální sítě, ujistěte se, zda pravidla zabezpečení sítě je přidána tooallow provoz z 168.63.129.16.

## <a name="example-of-internal-load-balancing"></a>Příklad interního vyrovnávání zatížení

toostep prostřednictvím hello koncoví tooend proces vytváření sadu Vyrovnávání zatížení sítě pro dvě konfigurace příklad zobrazí hello následující části.

### <a name="an-internet-facing-multi-tier-application"></a>Internetová, vícevrstvá aplikace

Chcete tooprovide služby Vyrovnávání zatížení databáze pro sadu internetové webové servery. Hostitelem obou sad serverů je jedna cloudová služba Azure. Webový server provoz tooTCP port 1433 musí být distribuována mezi dvěma virtuálními počítači v hello databázové vrstvy. Obrázek 1 zobrazuje konfiguraci hello.

![Interní sada Vyrovnávání zatížení sítě pro hello databázové vrstvy](./media/load-balancer-internal-getstarted/IC736321.png)

Konfigurace Hello se skládá z následujících hello:

* Hello stávající cloudovou službu hostování hello virtuálních počítačů je s názvem mytestcloud.
* Hello dvě existující databázové servery jsou pojmenované DB1 DB2.
* Webové servery v hello webová vrstva připojit servery databáze toohello hello databázové vrstvy pomocí hello privátní IP adresu. Další možností je toouse vlastní DNS pro virtuální síť hello a ruční registraci záznamu A pro sadu Nástroje pro vyrovnávání zatížení interní hello.

Hello následující příkazy nakonfigurujte novou instanci interní Vyrovnávání zatížení s názvem **ILBset** a přidat koncové body toohello virtuálních počítačů odpovídající toohello dva databázové servery:

```powershell
$svc="mytestcloud"
$ilb="ilbset"
Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb
$prot="tcp"
$locport=1433
$pubport=1433
$epname="TCP-1433-1433"
$lbsetname="lbset"
$vmname="DB1"
Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

$epname="TCP-1433-1433-2"
$vmname="DB2"
Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM
```

## <a name="remove-an-internal-load-balancing-configuration"></a>Odebrání konfigurace interního vyrovnávání zatížení

tooremove virtuálního počítače jako koncový bod z instance nástroje pro vyrovnávání zatížení interní hello použijte následující příkazy:

```powershell
$svc="<Cloud service name>"
$vmname="<Name of hello VM>"
$epname="<Name of hello endpoint>"
Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM
```

toouse tyto příkazy vyplnit hello hodnoty, odstranění hello < a >.

Zde naleznete příklad:

```powershell
$svc="mytestcloud"
$vmname="DB1"
$epname="TCP-1433-1433"
Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM
```

tooremove instance nástroje pro vyrovnávání zatížení interní z cloudové služby, hello použijte následující příkazy:

```powershell
$svc="<Cloud service name>"
Remove-AzureInternalLoadBalancer -ServiceName $svc
```

Zadejte hodnotu hello toouse tyto příkazy, a odeberte hello < a >.

Zde naleznete příklad:

```powershell
$svc="mytestcloud"
Remove-AzureInternalLoadBalancer -ServiceName $svc
```

## <a name="additional-information-about-internal-load-balancer-cmdlets"></a>Další informace o rutinách interního nástroje pro vyrovnávání zatížení

tooobtain Další informace o rutinách interní Vyrovnávání zatížení, spusťte následující příkazy příkazového řádku Windows Powershellu hello:

```powershell
Get-Help New-AzureInternalLoadBalancerConfig -full
Get-Help Add-AzureInternalLoadBalancer -full
Get-Help Get-AzureInternalLoadbalancer -full
Get-Help Remove-AzureInternalLoadBalancer -full
```

## <a name="next-steps"></a>Další kroky

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení pomocí spřažení se zdrojovou IP adresou](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)

