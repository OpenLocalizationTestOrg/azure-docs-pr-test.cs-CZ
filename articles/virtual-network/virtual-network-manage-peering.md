---
title: "aaaCreate, změnit nebo odstranit partnerský vztah virtuální síti Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate, měnit a odstraňovat virtuální sítě partnerský vztah."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: jdial
ms.openlocfilehash: 70f85364ab23e23533ad276aeec0156cebe89be5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-change-or-delete-a-virtual-network-peering"></a>Vytvoření, jejich změny nebo odstranění virtuální sítě partnerského vztahu

Zjistěte, jak toocreate, měnit a odstraňovat virtuální sítě partnerský vztah. Virtuální sítě můžete tooconnect dvou virtuálních sítí v hello stejného umístění Azure prostřednictvím partnerského vztahu umožňuje hello Azure páteřní síti. Jakmile peered, hello dvě virtuální sítě se pořád spravují jako samostatné prostředky. Prostředky ve buď virtuální síti komunikovat s hello stejné latence a šířku pásma, jako kdyby hello prostředky v hello stejné virtuální síti. Pokud si nejste obeznámeni s partnerský vztah virtuální sítě, doporučujeme čtení hello [partnerského vztahu Přehled virtuálních sítí](virtual-network-peering-overview.md) a dokončení hello [vytvoření partnerského vztahu kurzu virtuální sítě](virtual-network-create-peering.md), před dokončením Hello úlohy v tomto článku.

## <a name="before-you-begin"></a>Než začnete

Proveďte následující úkoly před dokončením kroků v žádné části tohoto článku hello:

- Zkontrolujte hello [Azure omezuje](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) toolearn článek o omezeních pro partnerský vztah.
- Přihlaste se toohello portálu Azure, rozhraní příkazového řádku Azure (CLI) nebo Azure PowerShell s účet Azure. Pokud nemáte účet Azure, si zaregistrovat [Bezplatný zkušební účet](https://azure.microsoft.com/free).
- Pokud pomocí prostředí PowerShell příkazy toocomplete úlohy v tomto článku [instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Zajistěte, že abyste měli nejnovější verzi rutin prostředí Azure PowerShell hello nainstalován hello. Zadejte tooget nápovědy pro příkazy prostředí PowerShell s příklady, `get-help <command> -full`.
- Pokud pomocí rozhraní příkazového řádku Azure (CLI) příkazy toocomplete úlohy v tomto článku [instalace a konfigurace rozhraní příkazového řádku Azure hello](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi hello hello nainstalované rozhraní příkazového řádku Azure. tooget nápovědy pro příkazy rozhraní příkazového řádku, zadejte `az <command> --help`. Namísto instalaci hello rozhraní příkazového řádku a jeho požadavky můžete použít hello prostředí cloudu Azure. Hello cloudové prostředí Azure je bezplatná prostředí Bash, který můžete spustit přímo v rámci hello portálu Azure. Hello cloudové prostředí má hello rozhraní příkazového řádku Azure předinstalována a nakonfigurované toouse s vaším účtem. toouse hello cloudové prostředí, klikněte na tlačítko hello cloudové prostředí **> _** tlačítko hello horní části hello [portál](https://portal.azure.com). 

## <a name="create-a-peering"></a>Vytvoření partnerského vztahu

>[!NOTE]
>Existuje několik požadavků, omezení a toosuccessfully důležité informace o vytvoření partnerského vztahu virtuální sítě. Před vytvořením partnerský vztah, ujistěte se, jste seznámili s hello [požadavky a omezení](#requirements-and-constraints) a [požadovaná oprávnění](#permissions).
>

1. Přihlaste se toohello [portál](https://portal.azure.com) pomocí účtu, který je přiřazen hello potřeby [role nebo oprávnění](#permissions).
2. V poli hello, který obsahuje hello text *vyhledávání prostředků* hello horní části hello portálu Azure, zadejte *virtuální sítě*. Když **virtuální sítě** se zobrazí ve výsledcích hledání hello, klikněte na něj. Nevybírejte **virtuální sítě (klasické)** Pokud se zobrazí v hello seznamu, protože nelze vytvořit z virtuální sítě nasazené prostřednictvím modelu nasazení classic hello partnerský vztah.
3. V hello **virtuální sítě** okno, které se zobrazí, klikněte na tlačítko hello chcete toocreate partnerský vztah pro virtuální sítě.
4. V podokně hello, který se zobrazí pro vybrané virtuální síť hello, klikněte na **partnerských vztahů** v hello **nastavení** části.
5. Klikněte na tlačítko **+ přidat**. 
6. <a name="add-peering"></a>V hello **přidat partnerský vztah** okno, zadejte nebo vyberte hodnoty pro hello následující nastavení:
    - **Název:** hello název pro partnerský vztah hello musí být jedinečný v rámci virtuální sítě hello.
    - **Virtuální síť modelu nasazení:** vyberte, které hello model nasazení virtuální sítě, kterou chcete toopeer s nasazená prostřednictvím.
    - **Vím Moje ID prostředku:** Pokud máte přístup pro čtení toohello virtuální sítě chcete toopeer s, nechte toto políčko nezaškrtnuté. Pokud nemáte přístup pro čtení toohello virtuální sítě nebo předplatné, které chcete toopeer s, zaškrtněte toto políčko. Zadejte hello úplné ID prostředku hello virtuální sítě chcete toopeer s v hello **ID prostředku** pole, které se zobrazily, pokud je zaškrtnuto políčko hello. Hello prostředků zadáte ID musí být pro virtuální síť, která existuje v hello stejné Azure [umístění](https://azure.microsoft.com/regions) jako této virtuální síti. Hello úplné ID prostředku vypadá podobně jakopříliš/odběry/<Id>/providers/Microsoft.Network/virtualNetworks/ /resourceGroups/ <-název skupiny prostředků-> <-název virtuální sítě->. ID prostředku hello virtuální sítě můžete získat zobrazením hello vlastnosti pro virtuální síť. jak zjistit, tooview hello vlastnosti pro virtuální síť, toolearn [spravovat virtuální sítě](virtual-network-manage-network.md#view-vnet).
    - **Předplatné:** vyberte hello [předplatné](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) hello virtuální sítě chcete toopeer s. Jeden nebo více odběrů nejsou uvedeny, v závislosti na tom, kolik odběry má váš účet přístup pro čtení. Pokud zaškrtnutí hello **ID prostředku** zaškrtávací políčko, toto nastavení není k dispozici. Virtuální sítě v různých předplatných mohou párově tak dlouho, dokud obě virtuální sítě byly vytvořeny prostřednictvím Správce prostředků. možnost toopeer Hello napříč vytvořena prostřednictvím různé modely nasazení odběrů je ve verzi preview. Registrace pro náhled hello před vytvořením partnerský vztah mezi virtuálními sítěmi nasadit v rámci různých nasazení modely, které existují v různých předplatných. Další informace o tooregister pro hello preview a [peer virtuální sítě vytvořené pomocí různé modely nasazení v různých předplatných](create-peering-different-deployment-models-subscriptions.md).
    - **Virtuální sítě:** vyberte hello chcete toopeer s virtuální sítě. Můžete vybrat virtuální síti vytvořené prostřednictvím buď modelu nasazení Azure, ale hello virtuální síť musí být ve stejné umístění jako hello virtuální síť, kterou jste inicializaci hello partnerského vztahu z hello. Potřebujete čtení přístup toohello virtuální sítě pro něj toobe zobrazená v seznamu hello. Pokud je uveden virtuální sítě, ale šedě, může být protože hello adresní prostor pro virtuální síť hello se překrývá s adresním prostorem hello pro tuto virtuální síť. Pokud adresu virtuální sítě prostory překrývají, nemůže být peered. Pokud zaškrtnutí hello **ID prostředku** zaškrtávací políčko, toto nastavení není k dispozici.
    - **Povolit přístup k virtuální síti:** vyberte **povoleno** (výchozí), pokud chcete, aby tooenable komunikace mezi hello dvě virtuální sítě. Povolení komunikace mezi virtuálními sítěmi umožňuje prostředky připojenými tooeither toocommunicate virtuální sítě s jinými s hello stejné šířky pásma a latence, jako kdyby byly připojené toohello stejné virtuální síti. Veškerá komunikace mezi prostředky v hello dvě virtuální sítě je nad hello Azure privátní sítě. Hello **virtuální síť** výchozí značka pro skupiny zabezpečení sítě zahrnuje hello virtuální sítě a peered virtuální sítě. Další informace o toolearn sítě značky výchozí skupiny zabezpečení, přečtěte si hello [přehled skupin zabezpečení sítě](virtual-networks-nsg.md#default-tags) článku.  Vyberte **zakázané** Pokud nechcete, aby provoz tooflow toohello peered virtuální sítě. Můžete vybrat **zakázané** Pokud jste peered virtuální síť s jinou virtuální sítí, ale někdy má toodisable tok přenosů mezi virtuálními sítěmi hello dva. Můžete zjistit, že povolení nebo zákaz je vhodnější než odstranit a znovu vytvořit partnerských vztahů. Pokud je toto nastavení zakázáno, nebude přenos mezi hello peered virtuální sítě.
    - **Povolit přesměrovaných přenosů:** zaškrtněte toto políčko tooallow přenos předávaný toohello peered virtuální síti (provoz není pocházející z hello peered virtuální sítě) tooflow toothis virtuální sítě. Předávání dál provoz je běžné, když nasadíte virtuální zařízení sítě ve virtuální síti hello jste partnerského vztahu a vytvořena trasy definované uživatelem tooforward provoz prostřednictvím hello síťové virtuální zařízení. Pokud toto není zaškrtnuto políčko (výchozí), předávány z hello peered virtuální sítě nemůže přenos toothis virtuální sítě. Při povolení této funkce umožňuje hello předávaných provoz prostřednictvím hello partnerského vztahu, nebudou vytvořit všechny trasy definované uživatelem a síťových virtuálních zařízení. Trasy definované uživatelem a virtuální zařízení sítě se vytvářejí zvlášť. Další informace o [trasy definované uživatelem](virtual-networks-udr-overview.md).
    - **Povolit bránu přenosu:** toto políčko zaškrtněte, pokud máte virtuální sítě připojený toothis brány virtuální sítě a chcete tooallow provoz z hello peered tooflow virtuální sítě prostřednictvím brány hello. Tato virtuální síť může být například připojené tooan do místní sítě prostřednictvím brány virtuální sítě. Pokud zaškrtnete že toto políčko umožňuje provoz z hello peered tooflow virtuální sítě prostřednictvím hello brány toothis připojené virtuální sítě. Pokud zaškrtnete toto políčko, hello peered virtuální síť nemůže mít brána nakonfigurovaná. Hello peered virtuální síť musí mít hello **používání služby Brána vzdálené** zaškrtávací políčko zaškrtnuto, když nastavení z partnerského vztahu hello hello jiné virtuální síti toothis virtuální sítě. Pokud toto není zaškrtnuto políčko (výchozí), provoz z hello peered virtuální sítě stále toků toothis virtuální sítě, ale se nelze procházet skrz virtuální sítě připojený toothis brány virtuální sítě. Další informace o [brány virtuální sítě](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti). 
    
    Tuto možnost nelze povolit, pokud jste partnerský vztah virtuální síť (Resource Manager) s virtuální sítě (klasické). I když hello přenosy dat mezi dvěma virtuálními sítěmi hello, nelze hello virtuální sítě (klasické) provoz procházet skrz síťové brány toohello připojené virtuální sítě (Resource Manager).
    - **Používat vzdálený brány:** zaškrtněte toto políčko tooallow provoz z tooflow této virtuální síti prostřednictvím virtuální sítě brány toohello připojené virtuální sítě se partnerský vztah s. Například hello virtuální síť, kterou jste partnerský vztah s nemá bránu VPN, který je připojen umožňující komunikaci tooan do místní sítě.  Zaškrtnutím tohoto políčka umožňuje provoz z této virtuální sítě tooflow prostřednictvím hello brány připojené toohello peered virtuální síť VPN. Pokud zaškrtnete toto políčko, hello peered virtuální síť musí mít tooit brány připojené virtuální sítě a musí mít hello **povolit brány přenosu** políčko zaškrtnuto. Pokud toto není zaškrtnuto políčko (výchozí), provoz z hello peered virtuální sítě můžete stále toku toothis virtuální sítě, ale nelze procházet skrz virtuální sítě připojený toothis brány virtuální sítě. 
    
    Tuto možnost nelze povolit, pokud jste partnerský vztah virtuální síť (Resource Manager) s virtuální sítě (klasické). I když hello přenosy dat mezi dvěma virtuálními sítěmi hello, hello virtuální sítě (Resource Manager) nelze přenos přes síť brány toohello připojené virtuální sítě (klasické).
7. Klikněte na tlačítko hello **OK** tlačítko tooadd hello podsíť toohello virtuální sítě jste vybrali.

### <a name="commands"></a>Příkazy

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[virtuální síť az sítě partnerský vztah vytvořit.](/cli/azure/network/vnet/peering#create?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Přidat AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json)|


### <a name="scenarios"></a>Scénáře

Partnerský vztah virtuální sítě se vytvoří mezi virtuální sítě vytvořené pomocí hello stejné nebo jiné nasazení modely, které existují v hello stejné nebo různých předplatných. Proveďte podrobný kurz pro jednu hello následující scénáře:
 
|Model nasazení Azure  | Předplatné  |
|---------|---------|
|Obě Resource Manager |[Stejné](virtual-network-create-peering.md)|
| |[Různé](create-peering-different-subscriptions.md)|
|Jedna Resource Manager, druhá Classic     |[Stejné](create-peering-different-deployment-models.md)|
| |[Různé](create-peering-different-deployment-models-subscriptions.md)|

## <a name="view-or-change-peering-settings"></a>Zobrazení nebo změna nastavení partnerského vztahu

1. Přihlaste se toohello [portál](https://portal.azure.com) pomocí účtu, který je přiřazen hello potřeby [role nebo oprávnění](#permissions).
2. V poli hello, který obsahuje hello text *vyhledávání prostředků* hello horní části hello portálu Azure, zadejte *virtuální sítě*. Když **virtuální sítě** se zobrazí ve výsledcích hledání hello, klikněte na něj.
3. V hello **virtuální sítě** okno, které se zobrazí, klikněte na tlačítko hello chcete toocreate partnerský vztah pro virtuální sítě.
4. V podokně hello, který se zobrazí pro vybrané virtuální síť hello, klikněte na **partnerských vztahů** v hello **nastavení** části.
5. Klikněte na partnerský vztah hello mají tooview nebo změnit nastavení pro.
6. Hello příslušné nastavení změnit. Přečtěte si informace o hello možnosti pro každé nastavení v [krok 6](#add-peering) z hello vytvoření partnerského vztahu části tohoto článku. 

    >[!NOTE]
    >Existuje několik požadavků, omezení a toosuccessfully důležité informace o vytvoření partnerského vztahu virtuální sítě. Před vytvořením partnerský vztah, ujistěte se, jste seznámili s hello [požadavky a omezení](#requirements-and-constraints) a [požadovaná oprávnění](#permissions).
    >

7. Klikněte na **Uložit**.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[partnerského vztahu seznam az sítě vnet](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#list) toolist partnerských vztahů pro virtuální síť, [az sítě vnet partnerského vztahu zobrazit](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#show) tooshow nastavení pro konkrétní partnerský vztah, a [az sítě vnet partnerského vztahu aktualizace](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#update) nastavení toochange partnerského vztahu.|
|PowerShell|[Get-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json) tooretrieve zobrazit nastavení partnerského vztahu a [Set-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/set-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json) toochange nastavení.|

## <a name="delete-a-peering"></a>Odstranit partnerský vztah
Při odstranění partnerský vztah provoz z virtuální sítě už toky toohello peered virtuální sítě. Při nasazení prostřednictvím Resource Manager virtuální sítě se kterými mají partnerský, každý virtuální síť nemá partnerského vztahu toohello jiné virtuální sítě. Když odstraníte hello partnerského vztahu z jedné virtuální sítě zakáže hello komunikace mezi virtuálními sítěmi hello, ale neodstraníte hello partnerského vztahu z hello jiné virtuální sítě. Hello partnerského vztahu stav pro hello partnerský vztah, který existuje v hello jiné virtuální síť se **odpojeno**. Nelze znovu vytvořit hello partnerský vztah, dokud je znovu vytvořit hello partnerský vztah v hello první virtuální síť a hello partnerského vztahu stav pro virtuální sítě změny příliš*připojeno*. 

Pokud chcete někdy toocommunicate virtuální sítě, ale ne vždy místo odstraněním partnerský vztah, můžete nastavit hello **povolit přístup k virtuální síti** nastavení příliš**zakázané** místo. toolearn jak, číst, krok 6 hello [partnerský vztah vytvořit](#create-peering) tohoto článku. Můžete zjistit, zakázání a povolení přístupu k síti jednodušší než odstranit a znovu vytvořit partnerských vztahů.

1. Přihlaste se toohello [portál](https://portal.azure.com) pomocí účtu, který je přiřazen hello potřeby [role nebo oprávnění](#permissions).
2. V poli hello, který obsahuje hello text *vyhledávání prostředků* hello horní části hello portálu Azure, zadejte *virtuální sítě*. Když **virtuální sítě** se zobrazí ve výsledcích hledání hello, klikněte na něj.
3. V hello **virtuální sítě** okno, které se zobrazí, klikněte na tlačítko hello chcete toodelete partnerský vztah z virtuální sítě.
4. V hello zobrazeném okně pro virtuální síť hello jste vybrali, klikněte na **partnerských vztahů** pod **nastavení**.
5. V seznamu hello partnerských vztahů, které se zobrazí v okně hello partnerských vztahů, klikněte pravým tlačítkem na hello partnerského vztahu můžete chcete toodelete, klikněte na **odstranit**, pak **Ano** toodelete hello partnerského vztahu z první virtuální síť hello.
6. Dokončení hello předchozí kroky toodelete hello partnerského vztahu z hello jiné virtuální sítě v partnerském vztahu hello.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[AZ síť vnet partnerského vztahu odstranit](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Odebrat AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/remove-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="requirements-and-constraints"></a>Požadavky a omezení 

- Hello virtuální sítě, které jste partnerský uzel musí mít-překrývající se adresní prostory IP adres.
- Nelze přidejte adresní prostory k, nebo odstraňte adresní prostory z virtuální sítě, jakmile je peered virtuální sítě s jinou virtuální sítí. tooadd nebo odeberte adresní prostory, hello odstranit partnerský vztah, přidejte nebo odeberte hello adresní prostory a pak znovu vytvoření partnerského vztahu hello. tooadd adresní prostory k nebo odebrání adresní prostory virtuálních sítí, přečtěte si hello [Create, změn nebo odstranění virtuální sítě](virtual-network-manage-network.md#add-address-spaces) článku. 
- Mohou párově dvě virtuální sítě nasazené prostřednictvím Resource Manager nebo virtuální síť s virtuální sítí nasazené prostřednictvím modelu nasazení classic hello nasazení prostřednictvím Resource Manager. Nelze peer dvě virtuální sítě vytvořené pomocí modelu nasazení classic hello. Pokud si nejste obeznámeni s modelech nasazení Azure, přečtěte si hello [modelech nasazení Azure pochopit](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku. Můžete použít [brány VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) tooconnect dvě virtuální sítě vytvořené pomocí modelu nasazení classic hello.
- Pokud partnerský vztah dvě virtuální sítě vytvořené pomocí Resource Manager, musí pro každou virtuální síť v partnerském vztahu hello nakonfigurovat partnerský vztah. Vytváříte-li z první virtuální síť hello hello partnerského vztahu toohello druhá virtuální síť, můžete stav partnerského vztahu hello je *získaných*.  Když vytvoříte hello partnerského vztahu z hello druhý virtuální sítě toohello první virtuální síť, jeho stav partnerského vztahu je *připojeno*. Pokud můžete zobrazit stav partnerského vztahu hello hello první virtuální sítě, zobrazí se jeho stav se změnil z *získaných* příliš*připojeno*. Hello partnerského vztahu není úspěšně vytvořeno, dokud je stav partnerského vztahu hello pro partnerské vztahy obě virtuální sítě *připojeno*. 
- Pokud partnerský vztah virtuální síti vytvořené pomocí Správce prostředků s virtuální síti vytvořené pomocí modelu nasazení classic hello, můžete pouze nakonfigurovat partnerský vztah pro virtuální síť hello nasazení prostřednictvím Resource Manager. Nelze nakonfigurovat partnerský vztah pro virtuální sítě (klasické), nebo mezi dvěma virtuálními sítěmi nasazené prostřednictvím modelu nasazení classic hello. Když vytvoříte hello partnerského vztahu z hello virtuální sítě (Resource Manager) toohello virtuální sítě (klasické), je stav partnerského vztahu hello *aktualizace*, poté krátce změní příliš*připojeno*.   
- Mezi dvěma virtuálními sítěmi je navázat partnerský vztah. Partnerských vztahů nejsou přechodné. Pokud vytvoříte partnerských vztahů mezi:
    - VirtualNetwork1 & VirtualNetwork2
    - VirtualNetwork2 & VirtualNetwork3

  Není žádný partnerský vztah mezi VirtualNetwork1 a VirtualNetwork3 prostřednictvím VirtualNetwork2. Pokud chcete toocreate partnerský vztah mezi VirtualNetwork1 a VirtualNetwork3 virtuální síť, máte toocreate partnerský vztah mezi VirtualNetwork1 a VirtualNetwork3.
- Nelze přeložit názvy v peered virtuální sítě pomocí výchozí Azure překlad. názvy tooresolve v jiných virtuálních sítí, musíte použít vlastního serveru DNS. jak tooset vlastní server DNS číst toolearn hello [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) článku.
- Prostředky v obě virtuální sítě v partnerském vztahu hello můžete vzájemně komunikovat s hello stejné šířky pásma a latence jako kdyby byly v hello stejné virtuální síti. Velikost pro všechny virtuální počítače ale má vlastní maximální šířku pásma sítě. Další informace o maximální šířku pásma sítě pro jiný virtuální počítač velikostí, přečtěte si hello toolearn [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) články velikostí virtuálního počítače.
- Mohou párově virtuální sítě nasazení prostřednictvím Resource Manager, které jsou v hello stejné nebo různých předplatných.
- Mohou párově virtuálních sítí nasadit v rámci různých nasazení modelů, které jsou v hello stejné, nebo různých předplatných (preview). 
- Hello odběry, které jsou obě virtuální sítě v musí být přidružené toohello stejné klienta Azure Active Directory. Pokud ještě nemáte klient služby AD, můžete rychle [vytvořit](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#start-from-scratch). Můžete použít [brány VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) tooconnect dvě virtuální sítě, které existují v různých předplatných přidružené toodifferent klientech služby Active Directory.
- Virtuální síť může být peered tooanother virtuální sítě a také být připojené tooanother virtuální síť s bránu virtuální sítě Azure. Pokud virtuální sítě jsou připojené prostřednictvím partnerského vztahu a bránu, provoz mezi virtuálními sítěmi hello toky prostřednictvím hello konfiguraci partnerského vztahu, nikoli brány hello.
- Za příchozí a výchozí přenos dat využívající partnerský vztah virtuálních sítí se účtuje nominální poplatek. Další informace najdete v tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/virtual-network).


## <a name="permissions"></a>Oprávnění

Hello účty, že používáte toocreate partnerský vztah virtuální síť musí mít hello potřebné role nebo oprávnění. Například pokud dvě virtuální sítě s názvem myVnetA a myVnetB byly partnerský vztah, musí váš účet se přiřazenou hello následující minimální role nebo oprávnění pro každou virtuální síť:
    
|Virtuální síť|Model nasazení|Role|Oprávnění|
|---|---|---|---|
|myVnetA|Resource Manager|[Přispěvatel sítě](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Classic|[Přispěvatel klasických sítí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Není k dispozici|
|myVnetB|Resource Manager|[Přispěvatel sítě](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Classic|[Přispěvatel klasických sítí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Další informace o [předdefinované role](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) a přiřazení konkrétní oprávnění příliš[vlastní role](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (pouze Resource Manager).

## <a name="next-steps"></a>Další kroky

Zjistěte, jak toocreate [hvězdicové topologie sítě](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) 
