---
title: "řešení potíží SSH aaaDetailed pro virtuální počítač Azure | Microsoft Docs"
description: "Podrobnější SSH řešení potíží pro problémy s připojením tooan virtuální počítač Azure"
keywords: "SSH připojení odmítnuto, ssh chyby, azure ssh, připojení SSH se nezdařilo"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: b8e8be5f-e8a6-489d-9922-9df8de32e839
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: support-article
ms.date: 07/06/2017
ms.author: iainfou
ms.openlocfilehash: 3f711e53a8251f8c06dbb589a258222566a4ae1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="detailed-ssh-troubleshooting-steps-for-issues-connecting-tooa-linux-vm-in-azure"></a>Podrobné SSH řešení potíží pro problémy s připojením tooa virtuálního počítače s Linuxem v Azure
Existuje mnoho možných příčin, že klient SSH hello nemusí být možné tooreach hello SSH službu na hello virtuálních počítačů. Pokud jste postupovali podle prostřednictvím hello více [obecné SSH při řešení potíží](troubleshoot-ssh-connection.md), je nutné toofurther potíží hello připojení. Tento článek vás provede podrobné řešení problémů s kroky toodetermine tam, kde se nedaří hello připojení SSH a jak tooresolve ho.

## <a name="take-preliminary-steps"></a>Proveďte předběžnou kroky
Hello následující diagram znázorňuje hello součásti, které jsou ovlivněny.

![Diagram zobrazující komponenty služby SSH](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

Hello následující postup vám pomůže izolovat hello zdroj selhání hello a zjistěte řešení či alternativní řešení.

1. Zkontrolujte stav hello hello virtuálních počítačů hello portálu.
   V hello [portál Azure](https://portal.azure.com), vyberte **virtuální počítače** > *název virtuálního počítače*.

   Hello podokno stavu služby pro hello virtuální počítač by měl zobrazit **systémem**. Posuňte se dolů tooshow poslední aktivitu na účtu pro výpočty, úložiště a síťové prostředky.

2. Vyberte **nastavení** tooexamine koncových bodů, IP adres, skupiny zabezpečení sítě a další nastavení.

   Hello virtuální počítač by měl mít koncový bod definované pro SSH provoz, který si můžete prohlédnout v **koncové body** nebo  **[skupinu zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md)**. Koncové body ve virtuálních počítačích, které byly vytvořeny pomocí Resource Manageru se ukládají na skupinu zabezpečení sítě. Také ověřte, zda pravidla hello byly použité toohello skupinu zabezpečení sítě a že se odkazuje v podsíti hello.

připojení k síti tooverify, kontrolu hello nakonfigurované koncové body a zjistěte, pokud se lze připojit hello virtuálních počítačů pomocí jiného protokolu, jako je například HTTP nebo jiné služby.

Po provedení těchto kroků zkuste to znovu připojení SSH hello.

## <a name="find-hello-source-of-hello-issue"></a>Najít hello zdroj problému hello
Klient SSH Hello ve vašem počítači může selhat tooreach hello SSH službu na hello virtuálního počítače Azure kvůli tooissues nebo nesprávné konfigurace v hello následující oblasti:

* [Klientský počítač SSH](#source-1-ssh-client-computer)
* [Hraniční zařízení organizace](#source-2-organization-edge-device)
* [Koncový bod služby v cloudu a přístup k seznamu řízení (ACL)](#source-3-cloud-service-endpoint-and-acl)
* [Skupiny zabezpečení sítě](#source-4-network-security-groups)
* [Na základě Linux virtuálního počítače Azure](#source-5-linux-based-azure-virtual-machine)

## <a name="source-1-ssh-client-computer"></a>Zdroj 1: SSH klientského počítače
tooeliminate počítače jako zdroj hello hello nezdaří, ověřte, že mohl zajistit SSH připojení tooanother místní, počítač se systémem Linux.

![Diagram, který označuje SSH klientské počítače komponenty](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Pokud hello připojení nezdaří, zkontrolujte hello ve vašem počítači následující problémy:

* Nastavení místní brány firewall, která blokuje příchozí nebo odchozí SSH provoz (port TCP 22)
* Místně nainstalován klientský software proxy serveru, který brání připojení SSH.
* Místně nainstalovaný software, který brání připojení SSH monitorování sítě
* Jiné typy zabezpečení softwaru, které monitorování provozu nebo povolí nebo zakáže určité typy přenosů

Pokud platí jedna z těchto podmínek, dočasně zakázat hello softwaru a akci SSH připojení tooan místní počítač toofind out příčiny hello hello připojení je blokován ve vašem počítači. Potom spolupracovat s vaší sítě Správce toocorrect hello softwaru nastavení tooallow připojení SSH.

Pokud používáte ověřování pomocí certifikátu, ověřte, zda máte tyto složky .ssh toohello oprávnění v domovském adresáři:

* Chmod – 700 ~/.ssh
* Chmod – 644 ~/.ssh/\*.pub
* Chmod – 600 ~/.ssh/id_rsa (nebo všechny soubory, které mají vaši privátní klíče uložené v nich)
* Chmod – 644 ~/.ssh/known_hosts (obsahuje hostitele, že jste připojeni toovia SSH)

## <a name="source-2-organization-edge-device"></a>Zdroj 2: Hraniční zařízení organizace
tooeliminate vaší organizace hraniční zařízení jako zdroj hello hello nezdaří, ověřte, že počítač, který je přímo připojených toohello Internet provádět tooyour připojení SSH virtuálního počítače Azure. Pokud se připojujete hello virtuálních počítačů přes síť site-to-site VPN nebo připojení k Azure ExpressRoute, přeskočte příliš[zdroj 4: skupin zabezpečení sítě](#nsg).

![Diagram, který označuje organizace hraniční zařízení](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Pokud nemáte počítač, který je přímo připojený toohello Internet, vytvořte nový virtuální počítač Azure v jeho vlastní prostředek skupiny nebo cloudovou službu a použít ho. Další informace najdete v tématu [vytvoření virtuálního počítače s Linuxem v Azure](quick-create-cli.md). Odstraňte skupinu nebo virtuálního počítače a cloudové služby prostředků hello, když jste hotovi s testování.

Pokud vytvoříte připojení SSH s počítači, který je přímo připojených toohello Internetu Zkontrolujte vaše organizace hraniční zařízení pro:

* Vnitřní brána firewall, která blokuje provoz SSH s hello Internetu
* Proxy server, který brání připojení SSH.
* Zjišťování neoprávněných vniknutí nebo software spuštěný na zařízeních ve vaší hraniční síť, která brání připojení SSH monitorování sítě

Nastavení sítě Správce toocorrect hello vaší organizace hraniční zařízení tooallow SSH provozu s hello Internetu pracovat.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Zdroj 3: Koncový bod služby Cloud a seznamu ACL
> [!NOTE]
> Tento zdroj platí pouze tooVMs, které se vytvořily s použitím modelu nasazení classic hello. Pro virtuální počítače, které byly vytvořeny pomocí Resource Manageru, přeskočte příliš[zdroje 4: skupin zabezpečení sítě](#nsg).

koncový bod tooeliminate hello cloudové služby a seznamu ACL jako zdroj hello hello nezdaří, ověřte jiný virtuální počítač Azure v hello stejné virtuální sítě můžete provést tooyour připojení SSH virtuálních počítačů.

![Diagram, který označuje koncového bodu služby cloud a seznamu ACL](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Pokud nemáte jiným virtuálním Počítačem v hello stejné virtuální síti, můžete snadno vytvořit jednu. Další informace najdete v tématu [vytvoření virtuálního počítače s Linuxem v Azure pomocí rozhraní příkazového řádku hello](quick-create-cli.md). Odstranit hello navíc virtuálních počítačů, když jste hotovi s testování.

Pokud vytvoříte připojení SSH s virtuálním Počítačem v hello stejné virtuální síť, zkontrolujte hello následující oblasti:

* **konfigurace koncového bodu Hello komunikaci SSH na hello cílovém virtuálním počítači.** Hello privátní port TCP koncového bodu hello by měl odpovídat hello TCP port, na které hello SSH je přijímá služba na hello virtuálních počítačů. (hello výchozí port je 22). Zkontrolujte číslo portu SSH TCP hello hello portálu Azure tak, že vyberete **virtuální počítače** > *název virtuálního počítače* > **nastavení**  >  **Koncové body**.
* **Hello seznamu ACL pro koncový bod provoz SSH hello na hello cílového virtuálního počítače.** Seznam ACL umožňuje toospecify povolené nebo zakázané příchozí přenosy z hello Internetu, na základě jeho zdrojové IP adresy. Nesprávně nakonfigurované seznamy ACL můžete zabránit příchozí SSH provoz toohello koncový bod. Zkontrolujte vaší tooensure seznamy ACL, který je povolen příchozí provoz z hello veřejné IP adresy proxy nebo jiný server edge. Další informace najdete v tématu [o přístup k síti řízení seznamy ACL](../../virtual-network/virtual-networks-acl.md).

koncový bod hello tooeliminate jako zdroj problému hello, odeberte hello aktuální koncový bod, vytvořte jiný koncový bod a zadejte jméno SSH hello (port TCP 22 pro veřejné a privátní port číslo hello). Další informace najdete v tématu [nastavení koncových bodů na virtuálním počítači v Azure](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

<a id="nsg"></a>

## <a name="source-4-network-security-groups"></a>Zdroje 4: Skupiny zabezpečení sítě
Skupiny zabezpečení sítě umožňují toohave členitější řízení toho povolené příchozí a odchozí přenosy. Můžete vytvořit pravidla, která span podsítě a cloudových služeb v virtuální sítě Azure. Zkontrolujte že tooand SSH přenosy z Internetu je povoleno hello tooensure pravidel skupiny zabezpečení vaší sítě.
Další informace najdete v tématu [o skupinách zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md).

Můžete také použít ověření IP toovalidate hello NSG konfigurace. Další informace najdete v tématu [síť Azure Přehled monitorování](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-monitoring-overview). 

## <a name="source-5-linux-based-azure-virtual-machine"></a>Zdroj 5: Systémem Linux virtuálního počítače Azure
Poslední zdroj Hello možných problémů je hello Azure samotného virtuálního počítače.

![Diagram, který označuje systémem Linux virtuálního počítače Azure](./media/detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Pokud jste tak ještě neučinili, postupujte podle pokynů hello [tooreset heslo nebo SSH pro virtuální počítače se systémem Linux](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

Zkuste se znovu připojit z vašeho počítače. Pokud stále nedaří, hello Následují některé hello informace o možných problémech:

* Hello SSH service není spuštěna na hello cílového virtuálního počítače.
* Hello SSH služby není naslouchá na portu TCP 22. tootest, nainstalujte klient služby telnet v místním počítači a spusťte "telnet *cloudServiceName*. cloudapp.net 22". Tento krok určuje, pokud je virtuální počítač hello umožňuje koncový bod SSH toohello příchozí a odchozí komunikaci.
* místní brána firewall Hello na hello cílový virtuální počítač obsahuje pravidla, která brání příchozích a odchozích přenosů SSH.
* Zjišťování neoprávněných vniknutí nebo software, který běží na virtuálním počítači Azure hello monitorování sítě brání připojení SSH.

## <a name="additional-resources"></a>Další zdroje
Další informace o odstraňování potíží s přístup k aplikaci najdete v tématu [Poradce při potížích přístup tooan aplikace běžící na virtuálním počítači Azure](troubleshoot-app-connection.md)
