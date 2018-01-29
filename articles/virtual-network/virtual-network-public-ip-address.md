---
title: "Vytvoření, změnit nebo odstranit Azure veřejnou IP adresu | Microsoft Docs"
description: "Naučte se vytvářet, měnit nebo odstranit veřejnou IP adresu."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: jdial
ms.openlocfilehash: e52dc76608a83d446ccc8503d17445a8d6a61ae4
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="create-change-or-delete-a-public-ip-address"></a>Vytvoření, změnit nebo odstranit veřejnou IP adresu

Další informace o veřejné IP adresy a jak vytvářet, měnit a odstraňovat jeden. Veřejná IP adresa je prostředek s vlastním konfigurovatelným nastavením. Přiřazení veřejnou IP adresu k dalším prostředkům služby Azure umožňuje:
- Příchozí připojení k Internetu k prostředkům, například virtuální počítače Azure, sady škálování virtuálního počítače Azure, Azure VPN Gateway, aplikačních bran a internetové nástroje pro vyrovnávání zatížení Azure. Prostředky Azure nemůže přijímat příchozí komunikaci z Internetu bez přiřazenou veřejnou IP adresu. Některé prostředky Azure jsou ze své podstaty přístupné prostřednictvím veřejných IP adres, musí mít jiné prostředky veřejné IP adresy, které jsou jim přiřazeny byla přístupná z Internetu.
- Odchozí připojení k Internetu pomocí předvídatelný IP adresy. Virtuální počítač může například komunikovat odchozí k Internetu bez veřejnou IP adresu přiřadit k němu, ale jeho adresa je adresa sítě přeložit v Azure nepředvídatelným veřejné adresy. Přiřazení veřejnou IP adresu prostředku umožňuje vědět, která IP adresa se používá pro odchozí připojení. Když předvídatelný, můžete změnit adresu, v závislosti na metodě přiřazení vybrané. Další informace najdete v tématu [vytvoření veřejné IP adresy](#create-a-public-ip-address). Další informace o odchozí připojení z prostředků Azure, najdete [pochopit odchozí připojení](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.

## <a name="before-you-begin"></a>Než začnete

Před dokončením všech kroků v žádné části tohoto článku dokončete následující úlohy:

- Zkontrolujte [Azure omezuje](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) článku se dozvíte o omezeních pro veřejné IP adresy.
- Přihlaste se k Azure [portál](https://portal.azure.com), rozhraní příkazového řádku Azure (CLI) nebo Azure PowerShell s účet Azure. Pokud nemáte účet Azure, si zaregistrovat [Bezplatný zkušební účet](https://azure.microsoft.com/free).
- Pokud pomocí příkazů prostředí PowerShell k dokončení úloh v tomto článku [instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi rutin prostředí Azure PowerShell nainstalovaný. Chcete-li získat nápovědu pro příkazy prostředí PowerShell s příklady, zadejte `get-help <command> -full`.
- Pokud používáte rozhraní příkazového řádku Azure (CLI) příkazy k dokončení úloh v tomto článku [instalace a konfigurace rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Zajistěte, že abyste měli nejnovější verzi rozhraní příkazového řádku Azure, který je nainstalovaný. Chcete-li získat nápovědu pro příkazy rozhraní příkazového řádku, zadejte `az <command> --help`. Místo instalace rozhraní příkazového řádku a jeho požadavky, můžete použít prostředí cloudové služby Azure. Služba Azure Cloud Shell je volně dostupné prostředí Bash, které můžete spustit přímo z portálu Azure Portal. Má předinstalované rozhraní Azure CLI, které je nakonfigurované pro použití s vaším účtem. Chcete-li použít cloudové prostředí, klikněte na tlačítko prostředí cloudu **> _** tlačítka v horní části [portál](https://portal.azure.com).

Veřejné IP adresy mají nominální poplatků. Zobrazit cenách, přečtěte si téma [IP adresu ceny](https://azure.microsoft.com/pricing/details/ip-addresses) stránky. 

## <a name="create-a-public-ip-address"></a>Vytvoření veřejné IP adresy

1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je přiřazený (minimálně) oprávnění pro roli Přispěvatel sítě pro vaše předplatné. Pro čtení [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) článku Další informace o přiřazování rolí a oprávnění k účtům.
2. Do pole, která obsahuje text *vyhledávání prostředků* v horní části portálu Azure, zadejte *veřejnou ip adresu*. Když **veřejné IP adresy** se zobrazí ve výsledcích hledání klikněte na něj.
3. Klikněte na tlačítko **+ přidat** v **veřejnou IP adresu** okno, které se zobrazí.
4. Zadejte nebo vyberte hodnoty pro následující nastavení v **vytvoření veřejné IP adresy** okno, které se zobrazí, pak klikněte na tlačítko **vytvořit**:

    |Nastavení|Povinné?|Podrobnosti|
    |---|---|---|
    |Skladová jednotka (SKU)|Ano|Všechny veřejné IP adresy, které vytvořili předtím, než jsou uvedení SKU **základní** SKU veřejné IP adresy.  Verze SKU nelze změnit po vytvoření veřejné IP adresy. Základní nebo standardní SKU, můžete použít samostatný virtuální počítač, virtuální počítače v rámci skupiny dostupnosti nebo sady škálování virtuálního počítače.  Kombinování SKU mezi virtuálními počítači v rámci skupiny dostupnosti nebo sady škálování není povoleno. **Základní** SKU: při vytváření veřejnou IP adresu v oblasti, která podporuje dostupnost zóny **dostupnost zóny** nastavení je *žádné* ve výchozím nastavení. Můžete vybrat zóny dostupnost zaručit konkrétní zónu pro svoji veřejnou IP adresu. **Standardní** SKU: veřejnou IP adresu standardní SKU A může být přidružen k virtuálnímu počítači nebo uživatelského rozhraní nástroje pro vyrovnávání zatížení. Pokud vytváříte veřejnou IP adresu v oblasti, která podporuje dostupnost zóny **dostupnost zóny** nastavení je *Zónově redundantní* ve výchozím nastavení. Další informace o dostupnosti zóny, najdete v článku **dostupnost zóny** nastavení. Pokud přidružíte adresu nástroji pro vyrovnávání zatížení standardní, je nutné standardní SKU. Další informace o vyrovnávání zatížení standardní, najdete v části [pro vyrovnávání zatížení Azure standardní SKU](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Standardní SKU je ve verzi preview. Před vytvořením standardní SKU veřejnou IP adresu, musíte nejdřív dokončit kroky v [zaregistrovat pro standardní SKU preview](#register-for-the-standard-sku-preview) a vytvoření veřejné IP adresy v podporovaných umístěním (oblastí). Seznam podporovaných umístění najdete v tématu [dostupnost v oblastech](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region-availability) a monitorovat [aktualizace virtuální sítě Azure](https://azure.microsoft.com/updates/?product=virtual-network) stránky pro podporu další oblasti. Při přiřazování veřejné IP adresy standardní SKU k síťovému rozhraní virtuálního počítače je potřeba explicitně povolit plánovaný provoz pomocí [skupiny zabezpečení sítě](security-overview.md#network-security-groups). Komunikace s prostředkem nebude možná, dokud nevytvoříte a nepřiřadíte skupinu zabezpečení sítě a explicitně nepovolíte požadovaný provoz.|
    |Name (Název)|Ano|Název musí být jedinečný v rámci skupiny prostředků, kterou vyberete.|
    |Verze protokolu IP|Ano| Vyberte IPv4 nebo IPv6. Při veřejné IPv4 adresy můžete přiřadit několik prostředky Azure, veřejnou IP adresu IPv6 můžete přiřadit pouze Vyrovnávání zatížení, internetové brány. Nástroje pro vyrovnávání zatížení můžete vyrovnávat přenosy protokolu IPv6 na virtuálních počítačích Azure zatížení. Další informace o [přenosy protokolu IPv6 k virtuálním počítačům Vyrovnávání zatížení](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Pokud jste vybrali **standardní SKU**, nemáte možnost vybrat *IPv6*. Adresu IPv4 lze vytvořit pouze při použití **standardní SKU**.|
    |Přiřazení IP adresy|Ano|**Dynamické:** dynamické adresy jsou přiřazeny pouze po veřejná IP adresa je přidružená k síťové rozhraní připojené k virtuálnímu počítači a virtuální počítač běží poprvé. Dynamické adresy se mohou změnit, pokud je virtuální počítač, který síťové rozhraní je připojen k zastavena (deallocated). Adresa zůstává stejná, pokud virtuální počítač restartovat nebo zastavena (ale není navrácena). **Statické:** při vytvoření veřejné IP adresy jsou přiřazené statické adresy. Statické adresy neměňte i v případě, že virtuální počítač uveden do stavu ukončeno (deallocated). Adresu pouze vydání při odstranění rozhraní sítě. Přiřazení metodu můžete změnit po vytvoření síťové rozhraní. Pokud vyberete *IPv6* pro **IP verze**, je metoda přiřazení *dynamické*. Pokud vyberete *standardní* pro **SKU**, je metoda přiřazení *statické*.|
    |Časový limit nečinnosti (minuty)|Ne|Kolik minut pro uchování připojení TCP nebo HTTP otevřené bez nutnosti spoléhat se na klientům posílat zprávy keep-alive. Pokud vyberete IPv6 pro **IP verze**, tato hodnota nemůže být změněna. |
    |Popisek názvu DNS|Ne|Musí být jedinečný v rámci umístění Azure, vytvořte název v (v rámci všech odběrů a všem zákazníkům). Azure automaticky registruje názvu a IP adresy v jeho DNS, aby se mohli připojit k prostředku s názvem. Azure připojí, jako výchozí podsíť *location.cloudapp.azure.com* (kde umístění je umístění vyberete) k názvu zadáte, chcete-li vytvořit plně kvalifikovaný název DNS. Pokud se rozhodnete vytvořit obě verze adresu, se stejným názvem DNS přiřazen adresy IPv4 a IPv6. Výchozí Azure DNS obsahuje záznamy názvového IPv4 A a IPv6 AAAA a odpovědí se oba záznamy, pokud se hledá název DNS. Klient zvolí které adresy (IPv4 nebo IPv6) ke komunikaci s. Místo nebo kromě Popisek názvu DNS pomocí výchozí příponu, můžete službu Azure DNS nakonfigurovat DNS název s příponou vlastní, který se přeloží na veřejnou IP adresu. Další informace najdete v tématu [použití Azure DNS s Azure veřejnou IP adresou](../dns/dns-custom-domain.md?toc=%2fazure%2fvirtual-network%2ftoc.json#public-ip-address).|
    |Vytvořit adresu IPv6 (nebo IPv4)|Ne| Jestli je zobrazena IPv6 a IPv4 je závislý na výběru pro **IP verze**. Například, pokud vyberete **IPv4** pro **IP verze**, **IPv6** se zobrazí tady. Pokud vyberete *standardní* pro **SKU**, nemáte možnost vytvořit adresu IPv6.
    |Název (viditelné v případě, že je zaškrtnuté **vytvořit adresu IPv6 (nebo IPv4)** políčko)|Ano, pokud jste vybrali **vytvořit IPv6** (nebo IPv4) zaškrtávací políčko.|Název musí být jiný než název zadejte pro první **název** v tomto seznamu. Pokud vyberete možnost vytvořit IPv4 i adresu IPv6, portálu vytvoří dvě samostatné prostředky veřejné IP adresy adresu, jeden s verze jednotlivých IP adresy přiřazené.|
    |Přiřazení IP adresy (viditelné v případě, že je zaškrtnuté **vytvořit adresu IPv6 (nebo IPv4)** políčko)|Ano, pokud jste vybrali **vytvořit IPv6** (nebo IPv4) zaškrtávací políčko.|Pokud políčko uvádí **vytvořit adresu IPv4**, můžete vybrat metodu přiřazení. Pokud políčko uvádí **vytvořit adresu IPv6**, nelze vybrat metodu přiřazení jako musí být **dynamické**.|
    |Předplatné|Ano|Musí existovat ve stejné [předplatné](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) jako prostředek, který chcete přidružit veřejnou IP adresu, která.|
    |Skupina prostředků|Ano|Může existovat ve stejné nebo jiné, [skupiny prostředků](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) jako prostředek, který chcete přidružit veřejnou IP adresu, která.|
    |Umístění|Ano|Musí existovat ve stejné [umístění](https://azure.microsoft.com/regions), také nazývaný jako oblasti jako prostředek, který chcete přidružit veřejnou IP adresu, které se.|
    |Dostupnost zóny| Ne | Toto nastavení se zobrazí, pouze pokud jste vybrali podporovaných umístění. Seznam podporovaných umístění najdete v tématu [dostupnost zóny přehled](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dostupnost zóny jsou aktuálně ve verzi preview. Před výběrem zóny nebo zónově redundantní možnost, musíte nejdřív dokončit kroky v [zaregistrovat verzi Preview zón dostupnosti](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#get-started-with-the-availability-zones-preview). Pokud jste vybrali **základní** SKU, *žádné* je automaticky vybrána pro vás. Pokud dáváte přednost zaručit konkrétní zónu, můžete vybrat konkrétní zónu. Jednu z těchto možností není zónově redundantní. Pokud jste vybrali **standardní** SKU: Zónově redundantní se vybere automaticky pro vás a provede cestu k datům odolné selhání zóny. Pokud chcete zaručit konkrétní zóně, což není odolný proti selhání zóny, můžete vybrat konkrétní zónu.
  

**Příkazy**

Když na portál poskytuje možnost vytvořit dva veřejné IP adresy prostředky (jedna adresa IPv4 a IPv6 jeden), následující příkazy rozhraní příkazového řádku a prostředí PowerShell vytvořit jeden prostředek s adresou pro jeden IP verze nebo na druhý. Pokud chcete, aby dva veřejné IP adresy prostředky, jeden pro každou verzi protokolu IP, musí spustit příkaz dvakrát, zadání odlišné názvy a verze pro prostředky veřejné IP adresy. 

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[Vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nové AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)|

## <a name="view-change-settings-for-or-delete-a-public-ip-address"></a>Zobrazení, změňte nastavení pro nebo odstranit veřejnou IP adresu

1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je přiřazený (minimálně) oprávnění pro roli Přispěvatel sítě pro vaše předplatné. Pro čtení [předdefinované role pro řízení přístupu Azure na základě rolí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) článku Další informace o přiřazování rolí a oprávnění k účtům.
2. Do pole, která obsahuje text *vyhledávání prostředků* v horní části portálu Azure, zadejte *veřejnou ip adresu*. Když **veřejné IP adresy** se zobrazí ve výsledcích hledání klikněte na něj.
3. V **veřejné IP adresy** okno, které se zobrazí, klikněte na název veřejné IP adresy, které chcete zobrazit, změnit nastavení nebo odstranit.
4. V zobrazeném okně pro veřejnou IP adresu dokončete jeden z následujících možností podle toho, jestli chcete zobrazit, odstranit nebo změnit veřejnou IP adresu.
    - **Zobrazení**: **přehled** části okna se zobrazí nastavení klíče pro veřejnou IP adresu, například síťové rozhraní je přidružená (Pokud je adresa přidružená k síťovému rozhraní). Na portálu nezobrazí verze adresy (IPv4 nebo IPv6). Chcete-li zobrazit informace o verzi, použijte příkaz prostředí PowerShell nebo rozhraní příkazového řádku zobrazíte veřejnou IP adresu. Pokud verze IP adresy IPv6, se nezobrazí přiřazenou adresu, na portálu, prostředí PowerShell nebo rozhraní příkazového řádku. 
    - **Odstranit**: Chcete-li odstranit veřejnou IP adresu, klikněte na tlačítko **odstranit** v **přehled** části okna. Pokud je aktuálně přidružené ke konfiguraci IP adresa, nelze jej odstranit. Pokud je adresa aktuálně přidružen ke konfiguraci, klikněte na tlačítko **zrušte aktuální přidružení** zrušení přidružení z konfigurace IP adresu.
    - **Změna**: klikněte na tlačítko **konfigurace**. Změňte nastavení pomocí informací v kroku 4 [vytvoření veřejné IP adresy](#create-a-public-ip-address) tohoto článku. Pokud chcete změnit přiřazení pro adresu IPv4 ze statické na dynamickou, musíte nejprve zrušit přidružení na veřejnou IPv4 adresu z konfigurace protokolu IP, které je přidružené k. Potom můžete změnit metodu přiřazení pro dynamické a klikněte na tlačítko **přidružit** přidružit IP adresu, na stejnou konfiguraci IP, jinou konfiguraci nebo můžete nechat ji zrušit její přidružení. Chcete-li oddělit veřejnou IP adresu, v **přehled** klikněte na tlačítko **zrušte aktuální přidružení**.

>[!WARNING]
>Pokud změníte přiřazení metoda ze statické na dynamickou, ztratíte IP adresu, která byla přiřazena veřejnou IP adresu. Zatímco Azure veřejné servery DNS Udržovat mapování mezi statické nebo dynamické adresy a všechny Popisek názvu DNS (Pokud jste definovali jeden), dynamickou IP adresu můžete změnit, když se virtuální počítač spustí po je v zastaveném stavu (deallocated). Chcete-li zabránit změna adresu, přiřadíte statickou IP adresu.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[AZ sítě seznamu veřejné ip](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#list) do seznamu veřejné IP adresy, [az sítě veřejné ip – zobrazení](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#show) zobrazíte nastavení; [aktualizace veřejné ip sítě az](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#update) aktualizovat; [odstranit veřejné sítě az-ip](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#delete) odstranit|
|PowerShell|[Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) načíst objekt veřejných IP adres a zobrazit její nastavení [Set-AzureRmPublicIpAddress](/powershell/resourcemanager/azurerm.network/set-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) se aktualizovat nastavení; [Odebrat AzureRmPublicIpAddress](/powershell/module/azurerm.network/remove-azurermpublicipaddress) odstranit|

## <a name="register-for-the-standard-sku-preview"></a>Registrace pro standardní SKU preview

> [!NOTE]
> Funkce ve verzi preview nemusí mít stejnou úroveň dostupnost a spolehlivost, podle verze funkce, které jsou obecné dostupnosti. Funkce Preview nejsou podporovány, může mít omezené možnosti a nemusí být k dispozici ve všech Azure umístění. 

Před vytvořením standardní SKU veřejné IP adresy, nejprve je nutné zaregistrovat verzi Preview. Pomocí následujících kroků k registraci ve verzi Preview:

1. Instalace a konfigurace Azure [prostředí PowerShell](/powershell/azure/install-azurerm-ps).
2. Spustit `Get-Module -ListAvailable AzureRM` příkazu zobrazte, jaká verze modulu AzureRM jste nainstalovali. Musí mít verzi 4.4.0 nebo vyšší. Pokud ho použít nechcete, můžete nainstalovat nejnovější verzi [Galerie prostředí PowerShell](https://www.powershellgallery.com/packages/AzureRM).
3. Přihlaste se k Azure pomocí `login-azurermaccount` příkaz.
4. Zadejte následující příkaz pro registraci ve verzi Preview:
   
    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

5. Potvrďte, že jste zaregistrováni ve verzi preview tak, že zadáte následující příkaz:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

## <a name="next-steps"></a>Další kroky
Při vytváření následující prostředky Azure, přiřaďte veřejné IP adresy:

- [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuální počítače
- [Nástroje pro vyrovnávání zatížení Azure internetového](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Application Gateway](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Připojení Site-to-site pomocí služby Azure VPN Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Škálovací sadu virtuálních počítačů Azure](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
