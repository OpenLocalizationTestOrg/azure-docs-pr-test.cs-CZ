---
title: "Řešení potíží s připojením mezi virtuálními počítači Azure | Microsoft Docs"
description: "Naučte se řešení problémů s připojením mezi virtuálními počítači Azure."
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/25/2017
ms.author: genli
ms.openlocfilehash: fd0f25c07cb3f385d3e8480ce1e33dec1ae0a214
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshooting-connectivity-problems-between-azure-vms"></a>Řešení potíží s připojením mezi virtuálními počítači Azure

Může dojít k potížím s připojením mezi virtuálními počítači Azure. Tento článek obsahuje postup pro odstraňování potíží při řešení tohoto problému. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Příznaky

Virtuální počítač Azure se nemůže připojit k jiným virtuálním Počítačem Azure.

## <a name="troubleshooting-guidance"></a>Pokyny při řešení potíží 

1. [Zkontrolujte, pokud síťový adaptér je špatně nakonfigurovaný.](#step-1-check-if-nic-is-misconfigured)
2. [Zkontrolujte, pokud je síťový provoz blokována NSG nebo UDR](#step-2-check-if-network-traffic-is-blocked-by-nsg-or-udr)
3. [Zkontrolujte, pokud je síťový provoz blokována bránou firewall virtuálních počítačů](#step-3-check-if-network-traffic-is-blocked-by-vm-firewall)
4. [Zkontrolujte, zda virtuální počítač aplikace nebo služba naslouchá na portu](#step-4-check-whether-vm-app-or-service-is-listening-on-the-port)
5. [Zkontrolujte, zda je problém způsoben překládat pomocí SNAT](#step-5-check-whether-the-problem-is-caused-by-snat)
6. [Zkontrolujte, zda je přenos dat blokován nastavením seznamy ACL pro klasické virtuální počítač.](#step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm)
7. [Zkontrolujte, zda je pro klasické virtuální počítač vytvořen koncový bod](#step-7-check-whether-the-endpoint-is-created-for-the-classic-vm)
8. [Nelze se připojit k síťové sdílené složky virtuálních počítačů](#step-8-unable-to-connect-to-a-vm-network-share)
9. [Připojení mezi virtuálními](#step-9-inter-vnet-connectivity)

## <a name="troubleshooting-steps"></a>Řešení potíží

Postupujte podle těchto kroků k vyřešení tohoto problému. Zkontrolujte, zda byl problém vyřešen po dokončení každého kroku. 

### <a name="step-1-check-if-nic-is-misconfigured"></a>Krok 1: Kontrola, pokud síťový adaptér je špatně nakonfigurovaný.

Postupujte podle [jak resetovat síťové rozhraní pro virtuální počítač s Windows Azure](../virtual-machines/windows/reset-network-interface.md). 

Pokud k problému dochází po úpravě síťové rozhraní (NIC), postupujte podle těchto kroků:

**Mulit seskupování virtuálních počítačů**

1. Přidejte síťový adaptér.
2. Opravte problémy v chybný síťový adaptér nebo odebrat chybný síťový adaptér.  Potom readd síťovou kartu.

Další informace najdete v tématu [síťová rozhraní pro přidání nebo odebrání z virtuálních počítačů](virtual-network-network-interface-vm.md).

**Jedním seskupování virtuálních počítačů** 

- [Znovu nasaďte Windows virtuálního počítače.](../virtual-machines/windows/redeploy-to-new-node.md)
- [Opětovné nasazení virtuálního počítače s Linuxem](../virtual-machines/linux/redeploy-to-new-node.md)

### <a name="step-2-check-if-network-traffic-is-blocked-by-nsg-or-udr"></a>Krok 2: Kontrola, pokud je síťový provoz blokována NSG nebo UDR

Využívat [ověřte toku IP sledovací proces sítě](../network-watcher/network-watcher-ip-flow-verify-overview.md) a [protokolování toku NSG](../network-watcher/network-watcher-nsg-flow-logging-overview.md) a zjistit, zda existuje skupina zabezpečení sítě nebo trasy definované uživatelem, který je v konfliktu s přenosy.

### <a name="step-3-check-if-network-traffic-is-blocked-by-vm-firewall"></a>Krok 3: Kontrola, pokud síťový provoz je blokována bránou firewall virtuálních počítačů

Zakažte bránu firewall a poté otestujte výsledek. Pokud se problém nevyřeší, ověřte nastavení v bráně firewall a znovu povolte.

### <a name="step-4-check-whether-vm-app-or-service-is-listening-on-the-port"></a>Krok 4: Kontrola, zda virtuální počítač aplikace nebo služba naslouchá na portu

Zkontrolujte, zda virtuální počítač aplikace nebo služba naslouchá na portu můžete použít jednu z následujících metod

- Spusťte následující příkazy ke kontrole, jestli server naslouchá na tomto portu.

**Virtuální počítač s Windows**

    netstat –ano

**Virtuální počítač s Linuxem**

    netstat -l

- Spustit **Telnet** příkazu ve virtuálním počítači na sebe sama k otestování portu. Pokud se test nezdaří, aplikace nebo služba není nakonfigurován pro naslouchání na portu.

### <a name="step-5-check-whether-the-problem-is-caused-by-snat"></a>Krok 5: Zkontrolujte, zda je problém způsoben překládat pomocí SNAT

V některých případech je virtuální počítač umístěn za řešení vyrovnávání zatížení, který má závislost na prostředky mimo Azure. V těchto scénářích platí, pokud dojde k problémům nepřerušované připojení problém může být způsobeno [překládat pomocí SNAT port vyčerpání](../load-balancer/load-balancer-outbound-connections.md). K vyřešení problému, vytvořte pro každý virtuální počítač, který je za zařízením na Vyrovnávání zatížení a zabezpečení pomocí NSG nebo seznamu ACL VIP (nebo splnění pro classic). 

### <a name="step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm"></a>Krok 6: Zkontrolujte, zda je přenos dat blokován nastavením seznamy ACL pro klasické virtuální počítač.

Seznam ACL umožňuje selektivně povolit nebo zakázat přenosů pro koncový bod virtuálního počítače. Další informace najdete v tématu [spravovat seznam ACL u koncového bodu](../virtual-machines/windows/classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).

### <a name="step-7-check-whether-the-endpoint-is-created-for-the-classic-vm"></a>Krok 7: Zkontrolujte, zda je pro klasické virtuální počítač vytvořen koncový bod

Všechny virtuální počítače, které vytvoříte v Azure pomocí modelu nasazení classic může automaticky komunikovat přes kanál privátní síť s dalšími virtuálními počítači ve stejné cloudové služby nebo virtuální sítě. Počítače v jiných virtuálních sítích však vyžadují koncové body směrovat příchozí síťový provoz do virtuálního počítače. Další informace najdete v tématu [jak nastavit koncové body](../virtual-machines/windows/classic/setup-endpoints.md).

### <a name="step-8-unable-to-connect-to-a-vm-network-share"></a>Krok 8: Nelze se připojit k síťové sdílené složky virtuálních počítačů

Pokud není možné se připojit k síťové sdílené složky virtuálních počítačů, problém může být způsobeno není k dispozici síťové adaptéry ve virtuálním počítači. Pokud chcete odstranit není k dispozici síťové karty, najdete v části [jak odstranit není k dispozici síťové karty](../virtual-machines/windows/reset-network-interface.md#delete-the-unavailable-nics)

### <a name="step-9-inter-vnet-connectivity"></a>Krok 9: Připojení mezi virtuálními

Využívat [ověřte toku IP sledovací proces sítě](../network-watcher/network-watcher-ip-flow-verify-overview.md) a [protokolování toku NSG](../network-watcher/network-watcher-nsg-flow-logging-overview.md) a zjistit, zda existuje skupina zabezpečení sítě nebo trasy definované uživatelem, který je v konfliktu s přenosy. Můžete taky ověřit konfiguraci Inter-Vnet [zde](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).

### <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém.