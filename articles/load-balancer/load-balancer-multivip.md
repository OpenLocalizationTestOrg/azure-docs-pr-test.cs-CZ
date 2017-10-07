---
title: "aaaMutiple virtuální IP adresy pro cloudové služby"
description: "Přehled o víc virtuálními IP adresami a jak tooset několika virtuálními IP adresami v cloudové službě"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: 85f6d26a-3df5-4b8e-96a1-92b2793b5284
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: b3e0f2b24968cb75a7064484a09ffe94505bb70b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-multiple-vips-for-a-cloud-service"></a>Konfigurace více virtuální IP adresy pro cloudové služby

Cloudové služby Azure můžete přistupovat přes hello veřejného Internetu pomocí IP adresy zadané v Azure. Tato veřejná IP adresa je odkazované tooas VIP (virtuální IP adresy) vzhledem k tomu, že je propojena toohello Azure nástroj pro vyrovnávání zatížení a ne hello instancí virtuálního počítače (VM) v rámci hello cloudové služby. Všechny instance virtuálního počítače v rámci cloudové služby můžete přistupovat pomocí jedné virtuální IP adresy.

Ale existují scénáře, ve kterých budete potřebovat víc virtuálních IP adres jako vstupní bod toohello stejné cloudové služby. Cloudové služby může například hostování více webů, který vyžaduje připojení SSL pomocí hello výchozí port 443, jako je každá lokalita hostované pro různých zákazníků, nebo klienta. V tomto scénáři musíte toohave jinou veřejnou přístupných IP adresu pro každý web. Hello následující diagram ukazuje typické víceklientské hostování webů s potřeba více SSL certifikáty na hello stejný veřejný port.

![Scénář více virtuálních IP adres SSL](./media/load-balancer-multivip/Figure1.png)

V příkladu hello výše, všechny virtuální IP adresy použijte hello stejný veřejný port (443) a provoz je přesměrovaného tooone nebo další zatížení vyrovnáváním virtuální počítače na jedinečný privátní port pro hello interní IP adresu hello cloudové služby hostování všechny weby hello.

> [!NOTE]
> Jiné situaci vyžadují hello použití hello několika virtuálními IP adresami je hostitelem více posluchače skupiny dostupnosti SQL AlwaysOn na hello stejnou sadu virtuálních počítačů.

Virtuální IP adresy jsou dynamické ve výchozím nastavení, což znamená, že v průběhu času mění hello skutečné přiřazené IP adresy toohello cloudové služby. tooprevent nedocházelo, můžete vyhradit VIP pro vaši službu. toolearn Další informace o vyhrazenou virtuální IP adresy, najdete v části [vyhrazené veřejné IP adresy](../virtual-network/virtual-networks-reserved-public-ip.md).

> [!NOTE]
> Najdete v tématu [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses/) informace o cenách na virtuální IP adresy a vyhrazené IP adresy.

Můžete pomocí prostředí PowerShell tooverify hello virtuální IP adresy používané vaší cloudové služby, a také přidat a odebrat virtuální IP adresy, přidružit tooan koncový bod VIP a konfigurovat konkrétní VIP pro vyrovnávání zatížení.

## <a name="limitations"></a>Omezení

V tomto okamžiku funkce více virtuálních IP adres je omezený toohello následující scénáře:

* **Pouze IaaS**. Více virtuálních IP adres můžete povolit jenom pro cloudové služby, které obsahují virtuální počítače. Více virtuálních IP adres nelze použít ve scénářích PaaS s instancí rolí.
* **Prostředí PowerShell pouze**. Více virtuálních IP adres může spravovat jenom pomocí prostředí PowerShell.

Tato omezení jsou dočasné a může kdykoli změnit. Ujistěte se, že toorevisit tuto stránku tooverify budoucí změny.

## <a name="how-tooadd-a-vip-tooa-cloud-service"></a>Jak tooadd VIP tooa cloudové služby
Služba tooyour tooadd VIP, spusťte následující příkaz prostředí PowerShell hello:

```powershell
Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

Tento příkaz zobrazí výsledek podobné toohello následující ukázka:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-tooremove-a-vip-from-a-cloud-service"></a>Jak tooremove virtuální IP adresu z cloudové služby
tooremove hello VIP přidána tooyour služby v příkladu hello nad spuštění hello následující příkaz prostředí PowerShell:

```powershell
Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

> [!IMPORTANT]
> Virtuální IP adresu lze odebrat pouze pokud má žádné tooit přiřazeny koncové body.


## <a name="how-tooretrieve-vip-information-from-a-cloud-service"></a>Jak tooretrieve VIP informace z cloudové služby
tooretrieve hello VIP spojené s cloudovou službou, spusťte následující skript prostředí PowerShell hello:

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

skript Hello zobrazí výsledek podobné toohello následující ukázka:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

V tomto příkladu hello cloudové služby má 3 virtuální IP adresy:

* **Vip1** je hello výchozí VIP, víte, že vzhledem k tomu, že hodnota hello IsDnsProgrammedName nastavena tootrue.
* **Vip2** a **Vip3** nejsou použity jako nemají žádné IP adresy. Pouze používají Pokud přidružíte koncový bod toohello VIP.

> [!NOTE]
> Vaše předplatné bude pouze vám účtována navíc VIP po jsou spojeny s koncový bod. Další informace o cenách najdete v tématu [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="how-tooassociate-a-vip-tooan-endpoint"></a>Jak tooassociate VIP tooan koncový bod

tooassociate virtuální IP adresu na cloudu tooan koncového bodu služby, spusťte následující příkaz prostředí PowerShell hello:

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 |
    Update-AzureVM
```

Hello příkaz vytvoří koncový bod názvem VIP propojené toohello *Vip2* na portu *80*a propojí ji toohello virtuálního počítače s názvem *myVM1* v rámci cloudové služby s názvem  *Moje_služba* pomocí *TCP* na portu *8080*.

tooverify hello konfigurace, spusťte následující příkaz prostředí PowerShell hello:

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

výstup Hello vypadá podobně jako toohello následující ukázka:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-tooenable-load-balancing-on-a-specific-vip"></a>Jak načíst tooenable konkrétní VIP pro vyrovnávání

Více virtuálních počítačů pro účely Vyrovnávání zatížení je možné přidružit jednu VIP. Například máte cloudové služby s názvem *Moje_služba*a dva virtuální počítače s názvem *myVM1* a *Můjvp2*. A cloudové služby má několika virtuálními IP adresami, jeden z nich s názvem *Vip2*. Pokud chcete, aby všechny přenosy dat tooport tooensure *81* na *Vip2* je rovnoměrně rozdělen mezi *myVM1* a *Můjvp2* na portu *8181* spusťte hello následující skript prostředí PowerShell:

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2 -DefaultProbe |
    Update-AzureVM

Get-AzureVM -ServiceName myService -Name myVM2 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe |
    Update-AzureVM
```

Můžete také aktualizovat vaše toouse nástroje pro vyrovnávání zatížení různé VIP. Například pokud spustíte následující příkaz prostředí PowerShell text hello, změní toouse sadu s názvem Vip1 VIP pro vyrovnávání zatížení hello:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1
```

## <a name="next-steps"></a>Další kroky

[Analýzy protokolů pro vyrovnávání zatížení Azure](load-balancer-monitor-log.md)

[Přehled nástroje pro vyrovnávání zatížení přístupných Internetu](load-balancer-internet-overview.md)

[Začněte na internetové nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md)

[Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md)

[Vyhrazená IP adresa REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx)
