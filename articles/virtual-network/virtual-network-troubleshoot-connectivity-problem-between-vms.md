---
title: "potíže s připojením k aaaTroubleshooting mezi virtuálními počítači Azure | Microsoft Docs"
description: "Zjistěte, jak tooTroubleshoot hello potíže s připojením k mezi virtuálními počítači Azure."
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
ms.openlocfilehash: ee0819178dcbee2bf529a495758e6c33f49e085e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-connectivity-problems-between-azure-vms"></a>Řešení potíží s připojením mezi virtuálními počítači Azure

Může dojít k potížím s připojením mezi virtuálními počítači Azure. Tento článek obsahuje řešení problémů s kroky toohelp vyřešíte tento problém. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Příznaky

Virtuální počítač Azure se nemůže připojit tooanother virtuálního počítače Azure.

## <a name="troubleshooting-guidance"></a>Pokyny při řešení potíží 

1. [Zkontrolujte, pokud síťový adaptér je špatně nakonfigurovaný.](#step-1-check-if-nic-is-misconfigured)
2. [Zkontrolujte, pokud je síťový provoz blokována NSG nebo UDR](#step-2-check-if-network-traffic-is-blocked-by-nsg-or-udr)
3. [Zkontrolujte, pokud je síťový provoz blokována bránou firewall virtuálních počítačů](#step-3-check-if-network-traffic-is-blocked-by-vm-firewall)
4. [Zkontrolujte, zda virtuální počítač aplikace nebo služba naslouchá na portu hello](#step-4-check-whether-vm-app-or-service-is-listening-on-the-port)
5. [Zkontrolujte, zda text hello problém je způsoben překládat pomocí SNAT](#step-5-check-whether-the-problem-is-caused-by-snat)
6. [Zkontrolujte zda je přenos dat blokován nastavením seznamy ACL pro hello classic virtuálních počítačů](#step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm)
7. [Zkontrolujte zda hello koncový bod vytvořen pro hello classic virtuálních počítačů](#step-7-check-whether-the-endpoint-is-created-for-the-classic-vm)
8. [Nelze tooconnect tooa virtuální počítač síťové sdílené složky](#step-8-unable-to-connect-to-a-vm-network-share)
9. [Připojení mezi virtuálními](#step-9-inter-vnet-connectivity)

## <a name="troubleshooting-steps"></a>Řešení potíží

Postupujte podle těchto kroků tootroubleshoot hello problém. Zkontrolujte, zda text hello problém vyřešen po dokončení každého kroku. 

### <a name="step-1-check-if-nic-is-misconfigured"></a>Krok 1: Kontrola, pokud síťový adaptér je špatně nakonfigurovaný.

Postupujte podle [jak tooreset síťové rozhraní pro virtuální počítač s Windows Azure](../virtual-machines/windows/reset-network-interface.md). 

Pokud vznikne problém hello po úpravě hello síťové rozhraní (NIC), postupujte takto:

**Mulit seskupování virtuálních počítačů**

1. Přidejte síťový adaptér.
2. Opravte problémy hello v hello chybný síťový adaptér nebo odeberte chybné síťový adaptér.  Potom readd hello síťový adaptér.

Další informace najdete v tématu [tooor rozhraní sítě přidat odebrat z virtuálních počítačů](virtual-network-network-interface-vm.md).

**Jedním seskupování virtuálních počítačů** 

- [Znovu nasaďte Windows virtuálního počítače.](../virtual-machines/windows/redeploy-to-new-node.md)
- [Opětovné nasazení virtuálního počítače s Linuxem](../virtual-machines/linux/redeploy-to-new-node.md)

### <a name="step-2-check-if-network-traffic-is-blocked-by-nsg-or-udr"></a>Krok 2: Kontrola, pokud je síťový provoz blokována NSG nebo UDR

Využívat [ověřte toku IP sledovací proces sítě](../network-watcher/network-watcher-ip-flow-verify-overview.md) a [protokolování toku NSG](../network-watcher/network-watcher-nsg-flow-logging-overview.md) tooidentify, pokud je skupina zabezpečení sítě nebo trasy definované uživatelem, který je v konfliktu s přenosy.

### <a name="step-3-check-if-network-traffic-is-blocked-by-vm-firewall"></a>Krok 3: Kontrola, pokud síťový provoz je blokována bránou firewall virtuálních počítačů

Zakažte hello brány firewall a poté otestujte hello výsledek. Pokud je hello problém vyřešen, ověřte nastavení hello v bráně firewall hello a znovu povolte.

### <a name="step-4-check-whether-vm-app-or-service-is-listening-on-hello-port"></a>Krok 4: Kontrola, zda virtuální počítač aplikace nebo služba naslouchá na portu hello

Můžete použít jednu z následujících metod toocheck, zda virtuální počítač aplikace nebo služba naslouchá na portu hello hello

- Spusťte následující příkazy toocheck, zda text hello server naslouchá na tomto portu hello.

**Virtuální počítač s Windows**

    netstat –ano

**Virtuální počítač s Linuxem**

    netstat -l

- Spustit hello **Telnet** na hello port hello tootest tooitself virtuálních počítačů. Pokud hello test se nezdaří, aplikace nebo služba není nakonfigurované toolisten na portu hello.

### <a name="step-5-check-whether-hello-problem-is-caused-by-snat"></a>Krok 5: Zkontrolujte, zda text hello problém je způsoben překládat pomocí SNAT

V některých scénářích hello virtuální počítač je umístěn za řešení vyrovnávání zatížení, který má závislost na prostředky mimo Azure. V těchto scénářích platí, pokud dojde k problémům nepřerušované připojení hello problém může být způsobeno [překládat pomocí SNAT port vyčerpání](../load-balancer/load-balancer-outbound-connections.md). tooresolve hello problém, vytvoření virtuální adresy IP (nebo splnění pro klasické) pro každý virtuální počítač, který je za zařízením na Vyrovnávání zatížení hello a zabezpečení pomocí NSG nebo seznamu ACL. 

### <a name="step-6-check-whether-traffic-is-blocked-by-acls-for-hello-classic-vm"></a>Krok 6: Zkontrolujte zda je přenos dat blokován nastavením seznamy ACL pro hello classic virtuálních počítačů

Seznam ACL poskytuje hello možnost tooselectively povolení nebo zakážou provoz pro koncový bod virtuálního počítače. Další informace najdete v tématu [spravovat hello seznamu ACL v koncovém bodě](../virtual-machines/windows/classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).

### <a name="step-7-check-whether-hello-endpoint-is-created-for-hello-classic-vm"></a>Krok 7: Zkontrolujte zda hello koncový bod vytvořen pro hello classic virtuálních počítačů

Všechny virtuální počítače, které vytvoříte v Azure pomocí modelu nasazení classic hello můžete automaticky komunikovat přes kanál privátní síť s dalšími virtuálními počítači v hello že stejné cloudové služby nebo virtuální sítě. Počítače v jiných virtuálních sítích však vyžadují koncové body toodirect hello příchozích síťových přenosů tooa virtuálního počítače. Další informace najdete v tématu [jak tooset až koncové body](../virtual-machines/windows/classic/setup-endpoints.md).

### <a name="step-8-unable-tooconnect-tooa-vm-network-share"></a>Krok 8: Nelze tooconnect tooa virtuální počítač síťové sdílené složky

Pokud jste nelze tooconnect tooa virtuální počítač síťové sdílené složky, hello problém může být způsobeno není k dispozici síťové adaptéry v hello virtuálních počítačů. toodelete hello síťové adaptéry k dispozici, najdete v části [jak toodelete hello není k dispozici síťové karty](../virtual-machines/windows/reset-network-interface.md#delete-the-unavailable-nics)

### <a name="step-9-inter-vnet-connectivity"></a>Krok 9: Připojení mezi virtuálními

Využívat [ověřte toku IP sledovací proces sítě](../network-watcher/network-watcher-ip-flow-verify-overview.md) a [protokolování toku NSG](../network-watcher/network-watcher-nsg-flow-logging-overview.md) tooidentify, pokud je skupina zabezpečení sítě nebo trasy definované uživatelem, který je v konfliktu s přenosy. Můžete taky ověřit konfiguraci Inter-Vnet [zde](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).

### <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit problém.
