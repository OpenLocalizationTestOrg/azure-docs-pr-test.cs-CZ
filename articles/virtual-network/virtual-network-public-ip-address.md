---
title: "aaaCreate, měnit a odstraňovat Azure veřejnou IP adresu | Microsoft Docs"
description: "Zjistěte, jak toocreate, změnit nebo odstranit veřejnou IP adresu."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: jdial
ms.openlocfilehash: 0f2578e8661c8f33419b896debcfa0ba1b41fc5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-change-or-delete-a-public-ip-address"></a>Vytvoření, změnit nebo odstranit veřejnou IP adresu

Další informace o veřejných IP adres a jak toocreate, změnit a odstraňte jednu. Veřejná IP adresa je prostředek s vlastním konfigurovatelným nastavením. Přiřazení veřejnou IP adresu tooother prostředky Azure umožňuje:
- Příchozí tooresources připojení k Internetu například Azure virtuální počítače (VM), sady škálování virtuálního počítače Azure, Azure VPN Gateway, aplikačních bran a internetové nástroje pro vyrovnávání zatížení Azure. Prostředky Azure nemůže přijímat příchozí komunikaci z hello Internetu bez přiřazenou veřejnou IP adresu. Některé prostředky Azure jsou ze své podstaty přístupné prostřednictvím veřejných IP adres, musí mít jiné prostředky veřejné IP adresy přiřazené toothem toobe přístupné z Internetu hello.
- Toohello odchozí připojení k Internetu pomocí předvídatelný IP adresu. Například virtuální počítač může komunikovat se odchozí toohello Internetu bez veřejnou IP adresu přiřazenou tooit, ale jeho adresa je adresa sítě přeložen Azure tooan nepředvídatelným veřejnou adresu. Přiřazení prostředek tooa veřejné IP adres umožňuje tooknow IP adresu, která se používá pro odchozí připojení hello. Když předvídatelný, můžete změnit adresu hello, v závislosti na metodě přiřazení hello vybrali. Další informace najdete v tématu [vytvoření veřejné IP adresy](#create-a-public-ip-address). Další informace o odchozí připojení z prostředky Azure, přečtěte si hello toolearn [pochopit odchozí připojení](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.

## <a name="before-you-begin"></a>Než začnete

Dokončení hello následující úkoly před dokončením všechny kroky v žádné části tohoto článku:

- Zkontrolujte hello [Azure omezuje](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) toolearn článek o omezeních pro veřejné IP adresy.
- Přihlaste se toohello Azure [portál](https://portal.azure.com), rozhraní příkazového řádku Azure (CLI) nebo Azure PowerShell s účet Azure. Pokud nemáte účet Azure, si zaregistrovat [Bezplatný zkušební účet](https://azure.microsoft.com/free).
- Pokud pomocí prostředí PowerShell příkazy toocomplete úlohy v tomto článku [instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Zajistěte, že abyste měli nejnovější verzi rutin prostředí Azure PowerShell hello nainstalován hello. Zadejte tooget nápovědy pro příkazy prostředí PowerShell s příklady, `get-help <command> -full`.
- Pokud pomocí rozhraní příkazového řádku Azure (CLI) příkazy toocomplete úlohy v tomto článku [instalace a konfigurace rozhraní příkazového řádku Azure hello](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi hello hello nainstalované rozhraní příkazového řádku Azure. tooget nápovědy pro příkazy rozhraní příkazového řádku, zadejte `az <command> --help`. Namísto instalaci hello rozhraní příkazového řádku a jeho požadavky můžete použít hello prostředí cloudu Azure. Hello cloudové prostředí Azure je bezplatná prostředí Bash, který můžete spustit přímo v rámci hello portálu Azure. Má hello rozhraní příkazového řádku Azure předinstalována a nakonfigurované toouse s vaším účtem. toouse hello cloudové prostředí, klikněte na tlačítko hello cloudové prostředí **> _** tlačítko hello horní části hello [portál](https://portal.azure.com).

Veřejné IP adresy mají nominální poplatků. tooview hello ceny, přečtěte si hello [IP adresu ceny](https://azure.microsoft.com/pricing/details/ip-addresses) stránky. 

## <a name="create-a-public-ip-address"></a>Vytvoření veřejné IP adresy

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je přiřazený (minimálně) oprávnění pro roli Přispěvatel sítě hello pro vaše předplatné. Čtení hello [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) článku toolearn informace o přiřazení role a oprávnění tooaccounts.
2. V poli hello, který obsahuje hello text *vyhledávání prostředků* hello horní části hello portálu Azure, zadejte *veřejnou ip adresu*. Když **veřejné IP adresy** se zobrazí ve výsledcích hledání hello, klikněte na něj.
3. Klikněte na tlačítko **+ přidat** v hello **veřejnou IP adresu** okno, které se zobrazí.
4. Zadejte nebo vyberte hodnoty pro následující nastavení v hello hello **vytvoření veřejné IP adresy** okno, které se zobrazí, pak klikněte na tlačítko **vytvořit**:

    |Nastavení|Povinné?|Podrobnosti|
    |---|---|---|
    |Name (Název)|Ano|Název Hello musí být jedinečný v rámci skupiny prostředků hello, kterou vyberete.|
    |Verze protokolu IP|Ano| Vyberte IPv4 nebo IPv6. Při veřejné IPv4 adresy mohou být přiřazeny tooseveral prostředky Azure, veřejnou IP adresu IPv6 lze přiřadit pouze tooan internetového nástroj pro vyrovnávání zatížení. Hello nástroj pro vyrovnávání zatížení můžete vyrovnávat zatížení IPv6 provoz tooAzure virtuálních počítačů. Další informace o [počítače toovirtual přenosy IPv6 Vyrovnávání zatížení](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    |Přiřazení IP adresy|Ano|**Dynamické:** dynamické adresy přiřazené jenom po hello veřejná IP adresa je přidružená tooa síťový adaptér připojený tooa virtuálních počítačů a hello spuštění virtuálního počítače je pro hello poprvé. Dynamické adresy se mohou změnit, pokud hello hello virtuálního počítače síťový adaptér je připojený toois zastavena (nepřiřazeném). Hello adresa zůstane stejný hello, pokud hello virtuálních počítačů je restartovat nebo zastavena (ale ne navrácena). **Statické:** jsou přiřazené statické adresy, když je vytvořen hello veřejnou IP adresu. Statické adresy udělat, není to i v že případě hello virtuálního počítače je vložit do hello zastavena změny (nepřiřazeném) stavu. Adresa Hello pouze vydání při odstranění hello síťový adaptér. Hello přiřazení metodu můžete změnit po vytvoření hello síťový adaptér. Pokud vyberete IPv6 pro hello **IP verze**, je k dispozici pouze přiřazení metoda hello **dynamické**.|
    |Časový limit nečinnosti (minuty)|Ne|Kolik minut tookeep připojení TCP nebo HTTP otevřete bez spoléhání na zprávy keep-alive toosend klientů. Pokud vyberete IPv6 pro **IP verze**, tato hodnota nemůže být změněna. |
    |Popisek názvu DNS|Ne|Musí být jedinečný v rámci hello umístění Azure vytvořit název hello v (v rámci všech odběrů a všem zákazníkům). Hello Azure veřejné služby DNS automaticky registruje hello názvu a IP adresy, které můžete připojit tooa prostředků s názvem hello. Azure připojí *location.cloudapp.azure.com* (kde umístění je umístění hello vyberete) název toohello toocreate hello zadáte plně kvalifikovaný název DNS. Pokud si zvolíte toocreate obě verze adresu, hello stejný název DNS je přiřazen tooboth hello IPv4 a IPv6 adresy. Hello služba Azure DNS obsahuje záznamy, název IPv4 A i IPv6 AAAA a odpovědí se oba záznamy, pokud se hledá název DNS hello. Klient Hello zvolí které toocommunicate adresy (IPv4 nebo IPv6) s.|
    |Vytvořit adresu IPv6 (nebo IPv4)|Ne| Jestli je zobrazena IPv6 a IPv4 je závislý na výběru pro **IP verze**. Například, pokud vyberete **IPv4** pro **IP verze**, **IPv6** se zobrazí tady.
    |Název (viditelné v případě zaškrtnuté hello **vytvořit adresu IPv6 (nebo IPv4)** políčko)|Ano, pokud vyberete hello **vytvořit IPv6** (nebo IPv4) zaškrtávací políčko.|Hello název musí být jiný než název hello nejprve zadejte pro hello **název** v tomto seznamu. Pokud si zvolíte toocreate IPv4 i adresu IPv6, hello portál vytvoří dvě samostatné prostředky veřejné IP adresy adresu, jeden s verze jednotlivých IP adresy přiřazené tooit.|
    |Přiřazení IP adresy (viditelné v případě zaškrtnuté hello **vytvořit adresu IPv6 (nebo IPv4)** políčko)|Ano, pokud vyberete hello **vytvořit IPv6** (nebo IPv4) zaškrtávací políčko.|Pokud políčko hello uvádí **vytvořit adresu IPv4**, můžete vybrat metodu přiřazení. Pokud políčko hello uvádí **vytvořit adresu IPv6**, nelze vybrat metodu přiřazení jako musí být **dynamické**.|
    |Předplatné|Ano|Musí existovat v hello stejné [předplatné](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) jako prostředek hello tooassociate hello veřejnou IP adresu, která chcete.|
    |Skupina prostředků|Ano|Může existovat v hello stejný nebo jiný, [skupiny prostředků](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) jako prostředek hello tooassociate hello veřejnou IP adresu, která chcete.|
    |Umístění|Ano|Musí existovat v hello stejné [umístění](https://azure.microsoft.com/regions), označuje také jako prostředek hello tooassociate hello veřejnou IP adresu, která chcete tooas oblast.|

**Příkazy**

I když hello portál nabízí možnost toocreate hello dva veřejné IP adresy prostředky (jedna adresa IPv4 a IPv6 jeden), hello následující příkazy rozhraní příkazového řádku a prostředí PowerShell vytvořit jeden prostředek s adresou pro jeden IP verze nebo hello jiné. Pokud chcete, aby dva veřejné IP adresy prostředky, jeden pro každou verzi protokolu IP, musíte spustit příkaz hello dvakrát zadání odlišné názvy a verze pro prostředky hello veřejnou IP adresu. 

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[Vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nové AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)|

## <a name="view-change-settings-for-or-delete-a-public-ip-address"></a>Zobrazení, změňte nastavení pro nebo odstranit veřejnou IP adresu

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je přiřazený (minimálně) oprávnění pro roli Přispěvatel sítě hello pro vaše předplatné. Čtení hello [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) článku toolearn informace o přiřazení role a oprávnění tooaccounts.
2. V poli hello, který obsahuje hello text *vyhledávání prostředků* hello horní části hello portálu Azure, zadejte *veřejnou ip adresu*. Když **veřejné IP adresy** se zobrazí ve výsledcích hledání hello, klikněte na něj.
3. V hello **veřejné IP adresy** okno, které se zobrazí, klikněte na název hello hello veřejné IP adresy mají tooview, změnit nastavení nebo odstranit.
4. V okně hello, který se zobrazí hello veřejných IP adres, dokončení jedné z následujících možností podle toho, jestli chcete tooview, hello odstranit nebo změnit hello veřejnou IP adresu.
    - **Zobrazení**: hello **přehled** části hello okno se zobrazí nastavení klíče hello veřejných IP adres, jako je například hello síťové rozhraní je příliš přiřazen (Pokud hello adresa je přidružená tooa síťové rozhraní). Hello portál nezobrazuje hello verze hello adresy (IPv4 nebo IPv6). tooview hello informace o verzi, použijte hello prostředí PowerShell nebo rozhraní příkazového řádku příkaz tooview hello veřejnou IP adresu. Pokud verze hello IP adresy IPv6, se nezobrazí hello přiřazená adresa hello portál, prostředí PowerShell nebo hello rozhraní příkazového řádku. 
    - **Odstranit**: toodelete hello veřejnou IP adresu, klikněte na tlačítko **odstranit** v hello **přehled** části okna hello. Pokud adresa hello je aktuálně přidružené tooan konfigurace protokolu IP, nelze jej odstranit. Pokud adresa hello je aktuálně přidružen ke konfiguraci, klikněte na tlačítko **zrušte aktuální přidružení** toodissociate hello adresu z hello konfigurace protokolu IP.
    - **Změna**: klikněte na tlačítko **konfigurace**. Změňte nastavení pomocí hello informací v kroku 4 hello [vytvoření veřejné IP adresy](#create-a-public-ip-address) tohoto článku. přiřazení hello toochange pro adresu IPv4 ze statické toodynamic, musíte nejprve oddělit hello veřejnou IPv4 adresu z hello konfigurace protokolu IP, které je přidružené k. Potom můžete změnit hello přiřazení metoda toodynamic a klikněte na tlačítko **přidružit** tooassociate hello IP adresu toohello stejnou IP adresu konfigurace, jinou konfiguraci nebo je můžete nechat ji zrušit její přidružení. toodissociate veřejnou IP adresu, v hello **přehled** klikněte na tlačítko **zrušte aktuální přidružení**.

>[!WARNING]
>Když změníte přiřazení metoda hello statické toodynamic, přijdete o hello IP adresu, kterému byla přiřazena toohello veřejnou IP adresu. Při hello Azure veřejné servery DNS Udržovat mapování mezi statické nebo dynamické adresy a všechny Popisek názvu DNS (Pokud jste definovali jeden), dynamickou IP adresu lze změnit, pokud hello virtuální počítač spustí po v hello zastavenou (deallocated) stavu. Adresa hello tooprevent změnu, přiřadit statickou IP adresu.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[AZ sítě seznamu veřejné ip](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#list) toolist veřejné IP adresy, [az sítě veřejné ip – zobrazení](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#show) tooshow nastavení; [aktualizace veřejné ip sítě az](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#update) tooupdate; [odstranit veřejné sítě az-ip](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#delete) toodelete|
|PowerShell|[Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) tooretrieve veřejných IP adres objekt a zobrazit jeho nastavení [Set-AzureRmPublicIpAddress](/powershell/resourcemanager/azurerm.network/set-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) tooupdate nastavení; [Odebrat AzureRmPublicIpAddress](/powershell/module/azurerm.network/remove-azurermpublicipaddress) toodelete|

## <a name="next-steps"></a>Další kroky
Při vytváření hello následující prostředky Azure, přiřaďte veřejné IP adresy:

- [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuální počítače
- [Nástroje pro vyrovnávání zatížení Azure internetového](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Application Gateway](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Připojení Site-to-site pomocí služby Azure VPN Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Škálovací sadu virtuálních počítačů Azure](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
