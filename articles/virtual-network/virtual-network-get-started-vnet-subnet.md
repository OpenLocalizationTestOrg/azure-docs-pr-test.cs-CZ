---
title: "aaaCreate vaše první Azure virtuální sítě | Microsoft Docs"
description: "Zjistěte, jak toocreate virtuální síť Azure (VNet), připojení dvě virtuální počítače (VM) toohello virtuální sítě a připojení toohello virtuálních počítačů."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2016
ms.author: jdial
ms.openlocfilehash: 1981524cf706d5ebc83b1ff77735617550ff058a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-virtual-network"></a>Vytvoření první virtuální sítě

Zjistěte, jak toocreate virtuální síť (VNet) se dvěma podsítěmi, vytvořte dva virtuální počítače (VM) a připojení každého virtuálního počítače tooone hello podsítí, jak je znázorněno v následujícím obrázku hello:

![Diagram virtuální sítě](./media/virtual-network-get-started-vnet-subnet/vnet-diagram.png)

Virtuální sítě Azure (VNet) je reprezentace vlastní sítě v cloudu hello. Můžete ovládat své sítě Azure a určovat bloky adres DHCP, nastavení DNS, zásady zabezpečení a směrování. Další informace o virtuální síť koncepty, přečtěte si hello toolearn [Přehled virtuálních sítí](virtual-networks-overview.md) článku. Proveďte následující kroky toocreate hello prostředky vidět na obrázku hello hello:

1. [Vytvoření virtuální sítě se dvěma podsítěmi](#create-vnet)
2. [Vytvořte dva virtuální počítače, každý s jedním síťovým rozhraním (NIC)](#create-vms)a přidružte tooeach (NSG) skupiny zabezpečení sítě síťový adaptér
3. [Připojit tooand z hello virtuální počítače](#connect-to-from-vms)
4. [Odstranění všech prostředků](#delete-resources). Pro některé prostředky hello vytvořené v tomto cvičení, při jejich jste zřízený platit poplatky. poplatky za hello toominimize po dokončení hello cvičení, ujistěte se, že toocomplete hello kroky v této části toodelete hello prostředky, které vytvoříte.

Budete mít základní znalosti o tom, jak můžete použít virtuální síť po dokončuje hello kroky v tomto článku. Další kroky jsou uvedeny tak další informace o toouse virtuální sítě na podrobnější úrovni.

## <a name="create-vnet"></a>Vytvoření virtuální sítě se dvěma podsítěmi

toocreate virtuální síť se dvěma podsítěmi, dokončení hello kroky, které provést. Jiné podsítě jsou obvykle používá toocontrol hello tok přenosů dat mezi podsítěmi.

1. Přihlaste se toohello [portál Azure](<https://portal.azure.com>). Pokud ještě účet nemáte, můžete si zaregistrovat [zkušební verzi na měsíc zdarma](https://azure.microsoft.com/free). 
2. V hello **Oblíbené** podokně hello portálu, klikněte na tlačítko **nový**.
3. V hello **nový** okně klikněte na tlačítko **sítě**. V hello **sítě** okně klikněte na tlačítko **virtuální síť**, jak ukazuje následující obrázek hello:

    ![Diagram virtuální sítě](./media/virtual-network-get-started-vnet-subnet/virtual-network.png)

4.  V hello **virtuální síť** okno, ponechejte *Resource Manager* vybrán jako model nasazení hello a klikněte na **vytvořit**.
5.  V hello **okno vytvořit virtuální síť** které se zobrazí, zadejte následující hodnoty hello a pak klikněte na tlačítko **vytvořit**:

    |**Nastavení**|**Hodnota**|**Podrobnosti**|
    |---|---|---|
    |**Název**|*MyVNet*|Název Hello musí být jedinečný v rámci skupiny prostředků hello.|
    |**Adresní prostor**|*10.0.0.0/16*|Můžete zadat libovolný adresní prostor v notaci CIDR.|
    |**Název podsítě**|*Front-end*|název podsítě Hello musí být jedinečný v rámci virtuální sítě hello.|
    |**Rozsah adres podsítě**|*10.0.0.0/24*| Hello oblast, kterou zadáte, musí existovat v rámci hello adresního prostoru definovaného pro virtuální síť hello.|
    |**Předplatné**|*[Vaše předplatné]*|Vyberte hello toocreate předplatné virtuální sítě v. Virtuální síť existuje v jednom předplatném.|
    |**Skupina prostředků**|**Vytvořit novou:***MyRG*|Vytvořte skupinu prostředků. Název skupiny prostředků Hello musí být jedinečný v rámci předplatného hello, který jste vybrali. Další informace o skupinách prostředků, přečtěte si hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) článek s přehledem.|
    |**Umístění**|*Západní USA*| Obvykle je vybrána hello umístění, které je nejblíže tooyour fyzické umístění.|

    Hello virtuální síť trvá několik sekund toocreate. Jakmile je vytvořen, zobrazí hello Azure řídicí panel portálu.

6. S hello virtuální sítě vytvořené v hello portál Azure **Oblíbené** podokně klikněte na tlačítko **všechny prostředky**. Klikněte na tlačítko hello **MyVNet** virtuální sítě v hello **všechny prostředky** okno. Pokud jste vybrali, již předplatné hello neobsahuje několik prostředků, můžete zadat *MyVNet* v hello **filtrovat podle názvu...** pole tooeasily přístup hello virtuální sítě.
7. Hello **MyVNet** okno otevře a zobrazí informace o hello sítě VNet, jak je znázorněno v následujícím obrázku hello:

    ![Diagram virtuální sítě](./media/virtual-network-get-started-vnet-subnet/myvnet.png)

8. Jak ukazuje předchozí obrázek hello, klikněte na tlačítko **podsítě** toodisplay seznam hello podsítí v rámci hello virtuální sítě. Hello pouze podsíť, existuje je **Front-end**, hello podsítě, které jste vytvořili v kroku 5.
9. V hello MyVNet - okno podsítě, klikněte na **+ podsíť** toocreate podsíť s hello následující informace a klikněte na tlačítko **OK** toocreate hello podsítě:

    |**Nastavení**|**Hodnota**|**Podrobnosti**|
    |---|---|---|
    |**Název**|*Back-end*|Název Hello musí být jedinečný v rámci virtuální sítě hello.|
    |**Rozsah adres**|*10.0.1.0/24*|Hello oblast, kterou zadáte, musí existovat v rámci hello adresního prostoru definovaného pro virtuální síť hello.|
    |**Skupina zabezpečení sítě** a **Tabulka směrování**|*Žádný* (výchozí)|Skupinám zabezpečení sítě se věnujeme dále v tomto článku. Další informace o trasy definované uživatelem, přečtěte si hello toolearn [trasy definované uživatelem](virtual-networks-udr-overview.md) článku.|

10. Po přidání novou podsíť hello toohello virtuální síť, můžete zavřít hello **MyVNet – podsítě** okna a potom zavřete hello **všechny prostředky** okno.

## <a name="create-vms"></a>Vytvoření virtuálních počítačů

Hello virtuální sítě a podsítě vytvořit můžete vytvořit virtuální počítače hello. Pro toto cvičení oba virtuální počítače spustit operační systém Windows Server hello, ale mohou spouštět žádný operační systém nepodporuje v Azure, včetně několik různých distribucí Linux.

### <a name="create-web-server-vm"></a>Vytvoření hello webový server virtuálního počítače

toocreate hello webový server virtuální počítač, dokončení hello následující kroky:

1. V podokně hello Azure portálu Oblíbené položky, klikněte na **nový**, **výpočetní**, pak **Windows Server 2016 Datacenter**.
2. V hello **Windows Server 2016 Datacenter** okně klikněte na tlačítko **vytvořit**.
3. V hello **Základy** okno, které se zobrazí, zadejte nebo vyberte hello následující hodnoty a klikněte na tlačítko **OK**:

    |**Nastavení**| **Hodnota**|**Podrobnosti**|
    |---|---|---|
    |**Název**|*MyWebServer*|Tento virtuální počítač slouží jako webový server, ke kterému se připojují internetové prostředky.|
    |**Typ disku virtuálního počítače**|*SSD*|
    |**Uživatelské jméno**|*Nějaké si zvolte*|
    |**Heslo a Potvrzení hesla**|*Nějaké si zvolte*|
    | **Předplatné**|*<Your subscription>*|Hello odběr, musí být stejné předplatné, které jste vybrali v kroku 5 hello hello [vytvořit virtuální síť se dvěma podsítěmi](#create-vnet) tohoto článku. Hello virtuální síť připojení virtuálních počítačů toomust existovat v hello stejné předplatné jako hello virtuálních počítačů.|
    |**Skupina prostředků**|**Použít existující:** Vyberte *MyRG*|V případě, že používáme hello stejnou skupinu prostředků, jako jsme to udělali pro hello sítě VNet, hello prostředky nemají tooexist v hello stejnou skupinu prostředků.|
    |**Umístění**|*Západní USA*|musí být umístění Hello hello stejné umístění, které jste zadali v kroku 5 hello [vytvořit virtuální síť se dvěma podsítěmi](#create-vnet) tohoto článku. Virtuální počítače a hello virtuální sítě se připojují toomust existovat v hello stejné umístění.|

4. V hello **zvolte velikost** okně klikněte na tlačítko *DS1_V2 standardní*, pak klikněte na tlačítko **vyberte**. Čtení hello [velikosti virtuálních počítačů Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku seznam všech velikostí virtuálního počítače s Windows nepodporuje v Azure.
5. V hello **nastavení** okno, zadejte nebo vyberte hello následující hodnoty a klikněte na tlačítko **OK**:

    |**Nastavení**|**Hodnota**|**Podrobnosti**|
    |---|---|---|
    |**Úložiště: Použít spravované disky**|*Ano*||
    |**Virtuální síť**| Vyberte *MyVNet*|Můžete vybrat všechny virtuální sítě, který již existuje v hello stejné umístění jako hello virtuálních počítačů, kterou vytváříte. Další informace o virtuální sítě a podsítě, přečtěte si hello toolearn [virtuální síť](virtual-networks-overview.md) článku.|
    |**Podsíť**|Vyberte *Front-end*|Můžete vybrat všechny podsítě, která existuje v rámci hello virtuální sítě.|
    |**Veřejná IP adresa**|Přijměte výchozí hello|Veřejná IP adresa umožňuje vám tooconnect toohello virtuálních počítačů z hello Internetu. Další informace o veřejné IP adresy, přečtěte si hello toolearn [IP adresy](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) článku.|
    |**Skupina zabezpečení sítě (brána firewall)**|Přijměte výchozí hello|Klikněte na tlačítko hello **(Nový) MyWebServer nsg** výchozí NSG hello portál vytvořit tooview jeho nastavení. V hello **vytvořit skupinu zabezpečení sítě** okno, které se otevře, Všimněte si, má jeden příchozí pravidlo, které umožňuje přenos TCP/3389 (RDP) z libovolná Zdrojová IP adresa.|
    |**Všechny ostatní hodnoty**|Přijměte výchozí hodnoty hello|Další informace o hello zbývající nastavení, přečtěte si hello toolearn [o virtuálních počítačích](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.|

    Skupiny zabezpečení sítě (NSG) umožňují toocreate příchozí nebo odchozí pravidla pro hello typ síťového provozu, který může obtékat tooand z hello virtuálních počítačů. Ve výchozím nastavení všechny příchozí přenosy toohello virtuálního počítače byl odepřen. Pro produkční webový server můžete přidat další příchozí pravidla pro port TCP/80 (HTTP) a TCP/443 (HTTPS). Pro odchozí provoz žádné pravidlo není, protože ve výchozím nastavení je veškerý odchozí provoz povolen. Je můžete přidat nebo odebrat pravidla toocontrol provoz na vaše zásady. Čtení hello [skupin zabezpečení sítě](virtual-networks-nsg.md) článku toolearn více informací o skupiny Nsg.

6.  V hello **Souhrn** okně zkontrolujte hello nastavení a klikněte na tlačítko **OK** toocreate hello virtuálních počítačů. Dlaždice stav se zobrazí na řídicí panel portálu hello hello, které vytvoří virtuální počítač. Může trvat několik minut toocreate. Toowait není nutné pro něj toocomplete. Můžete pokračovat dalším krokem toohello při hello, které vytvoří virtuální počítač.

### <a name="create-database-server-vm"></a>Vytvoření databáze serveru hello virtuálních počítačů

toocreate hello databázový server virtuální počítač, dokončení hello následující kroky:

1.  V podokně hello Oblíbené položky, klikněte na **nový**, **výpočetní**, pak **Windows Server 2016 Datacenter**.
2.  V hello **Windows Server 2016 Datacenter** okně klikněte na tlačítko **vytvořit**.
3.  V hello **okno Základy**, zadejte nebo vyberte hello následující hodnoty a potom klikněte na tlačítko **OK**:

    |**Nastavení**|**Hodnota**|**Podrobnosti**|
    |---|---|---|
    |**Název**|*MyDBServer*|Tento virtuální počítač slouží jako databázový server, který webový server hello připojuje ke službě, že tento hello Internet se nemůže připojit k.|
    |**Typ disku virtuálního počítače**|*SSD*||
    |**Uživatelské jméno**|Nějaké si zvolte||
    |**Heslo a Potvrzení hesla**|Nějaké si zvolte||
    |**Předplatné**|<Your subscription>|Hello odběr, musí být stejné předplatné, které jste vybrali v kroku 5 hello hello [vytvořit virtuální síť se dvěma podsítěmi](#create-vnet) tohoto článku.|
    |**Skupina prostředků**|**Použít existující:** Vyberte *MyRG*|V případě, že používáme hello stejnou skupinu prostředků, jako jsme to udělali pro hello sítě VNet, hello prostředky nemají tooexist v hello stejnou skupinu prostředků.|
    |**Umístění**|*Západní USA*|musí být umístění Hello hello stejné umístění, které jste zadali v kroku 5 hello [vytvořit virtuální síť se dvěma podsítěmi](#create-vnet) tohoto článku.|

4.  V hello **zvolte velikost** okně klikněte na tlačítko *DS1_V2 standardní*, pak klikněte na tlačítko **vyberte**.
5.  V hello **nastavení** okno, zadejte nebo vyberte hello následující hodnoty a klikněte na tlačítko **OK**:

    |**Nastavení**|**Hodnota**|**Podrobnosti**|
    |----|----|---|
    |**Úložiště: Použít spravované disky**|*Ano*||
    |**Virtuální síť**|Vyberte *MyVNet*|Můžete vybrat všechny virtuální sítě, který již existuje v hello stejné umístění jako hello virtuálních počítačů, kterou vytváříte.|
    |**Podsíť**|Vyberte *Back-end* kliknutím hello **podsíť** pole, pak výběrem **Back-end** z hello **zvolte podsíť** okno|Můžete vybrat všechny podsítě, která existuje v rámci hello virtuální sítě.|
    |**Veřejná IP adresa**|NONE – klikněte na tlačítko hello výchozí adresu a pak klikněte na **žádné** z hello **zvolte veřejnou IP adresu** okno|Bez veřejnou IP adresu, budete moct připojit jen toohello virtuálního počítače z jiného virtuálního počítače připojené toohello stejnou virtuální síť. Tooit nemůžete se připojit přímo z hello Internetu.|
    |**Skupina zabezpečení sítě (brána firewall)**|Přijměte výchozí hello| Jako výchozí hello, které skupina NSG vytvořili pro hello MyWebServer virtuální počítač, tato skupina NSG také má hello stejné výchozí příchozí pravidlo. Pro databázový server můžete přidat další příchozí pravidlo pro protokol TCP/1433 (MS SQL). Pro odchozí provoz žádné pravidlo není, protože ve výchozím nastavení je veškerý odchozí provoz povolen. Je můžete přidat nebo odebrat pravidla toocontrol provoz na vaše zásady.|
    |**Všechny ostatní hodnoty**|Přijměte výchozí hodnoty hello||

6.  V hello **Souhrn** okně zkontrolujte hello nastavení a klikněte na tlačítko **OK** toocreate hello virtuálních počítačů. Dlaždice stav se zobrazí na řídicí panel portálu hello hello, které vytvoří virtuální počítač. Může trvat několik minut toocreate. Toowait není nutné pro něj toocomplete. Můžete pokračovat dalším krokem toohello při hello, které vytvoří virtuální počítač.

## <a name="review"></a>Kontrola prostředků

I když jste vytvořili jednu virtuální síť a dva virtuální počítače, hello portál Azure pro můžete vytvořit několik dalších prostředků ve skupině prostředků MyRG hello. Zkontrolujte obsah hello skupiny prostředků MyRG hello provedením hello následující kroky:

1. V hello **Oblíbené** podokně klikněte na tlačítko **další služby**.
2. V hello **další služby** podokně, typ *skupiny prostředků* hello pole, který má aplikace word hello *filtru* v ní. Klikněte na tlačítko **skupiny prostředků** až se zobrazí v hello filtrovaný seznam.
3. V hello **skupiny prostředků** podokně klikněte na tlačítko hello *MyRG* skupinu prostředků. Pokud máte mnoho existujících skupin prostředků v rámci vašeho předplatného, můžete zadat *MyRG* hello pole, která obsahuje hello text *filtrovat podle názvu...* Skupina tooquickly přístup hello MyRG prostředků.
4.  V hello **MyRG** okně uvidíte příslušné skupině hello prostředků obsahuje prostředky, 12, jak je znázorněno v následujícím obrázku hello:

    ![Obsah skupiny prostředků](./media/virtual-network-get-started-vnet-subnet/resource-group-contents.png)

Další informace o virtuálních počítačů, disků a účty úložiště, přečtěte si hello toolearn [virtuálního počítače](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [disku](../virtual-machines/windows/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-network%2ftoc.json), a [účet úložiště](../storage/common/storage-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) přehled články. Můžete zobrazit hello dvě výchozí skupiny Nsg hello portál vám vytvoří. Můžete také zjistit tohoto portálu hello vytvořit dvě síťové prostředky rozhraní (NIC). Síťový adaptér umožňuje prostředky virtuálních počítačů tooconnect tooother přes hello virtuální sítě. Čtení hello [seskupování](virtual-network-network-interface.md) článku toolearn Další informace o síťových karet. portál Hello také vytvořit jeden prostředek veřejné IP adresy. Veřejné IP adresy jsou jedním z nastavení prostředku veřejné IP adresy. Další informace o veřejné IP adresy, přečtěte si hello toolearn [IP adresy](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) článku.

## <a name="connect-to-from-vms"></a>Připojit virtuální počítače toohello

Virtuální síť a vytvořit dva virtuální počítače můžete teď připojit toohello virtuální počítače pomocí kroků hello v následující části hello:

### <a name="connect-from-internet"></a>Připojit toohello webový server virtuálního počítače z hello Internetu

aplikace tooconnect toohello webový server virtuálního počítače z hello Internetu, dokončení hello následující kroky:

1. Hello portálu, skupinu prostředků MyRG otevřete hello provedením hello kroky hello [zkontrolujte prostředky](#review) tohoto článku.
2. V hello **MyRG** okně klikněte na tlačítko hello **MyWebServer** virtuálních počítačů.
3. V hello **MyWebServer** okně klikněte na tlačítko **Connect**, jak ukazuje následující obrázek hello:

    ![Připojení serveru tooweb virtuálních počítačů](./media/virtual-network-get-started-vnet-subnet/webserver.png)

4. Povolit váš prohlížeč toodownload hello *MyWebServer.rdp* souboru a potom ho otevřete.
5. Pokud se zobrazí, dialogové okno pole informování, můžete tento hello vydavatel hello vzdáleného připojení nelze ověřit, klikněte na tlačítko **Connect**.
6. Při zadávání přihlašovacích údajů, ujistěte se, přihlásit se hello uživatelské jméno a heslo, které jste zadali v kroku 3 hello [vytvořit hello webový server virtuálního počítače](#create-web-server-vm) tohoto článku. Pokud hello **zabezpečení systému Windows** není pole, které se zobrazí seznam hello správné přihlašovací údaje, bude pravděpodobně nutné tooclick **další možnosti**, pak **použít jiný účet**, tak, aby se Zadejte hello správné uživatelské jméno a heslo). Klikněte na tlačítko **OK** tooconnect toohello virtuálních počítačů.
7. Pokud se zobrazí **připojení ke vzdálené ploše** pole oznamující, že hello identity hello vzdáleného počítače nelze ověřit, klikněte na tlačítko **Ano**.
8. Nyní jste připojené toohello MyWebServer virtuálních počítačů z hello Internetu. Ponechte připojení ke vzdálené ploše hello otevřete toocomplete hello kroků v další části hello.

připojení ke vzdálené Hello je toohello veřejnou IP adresu přiřadit toohello veřejnou IP adresu prostředku hello portál vytvořili v kroku 5 hello [vytvořit virtuální síť se dvěma podsítěmi](#create-vnet) tohoto článku. Hello připojení povoleno, protože hello výchozí pravidlo vytvořeny v hello **MyWebServer nsg** NSG povolené TCP/3389 (RDP) příchozí toohello virtuálních počítačů z libovolná Zdrojová IP adresa. Pokud se pokusíte tooconnect toohello virtuálních počítačů přes jiný port, hello připojení selže, pokud přidáte další příchozích pravidel toohello NSG povolení hello další porty.

>[!NOTE]
>Pokud přidáte další příchozích pravidel toohello NSG, ujistěte se, že hello stejné porty jsou otevřené bránu firewall systému Windows hello nebo hello připojení selže.
>

### <a name="connect-to-internet"></a>Připojit toohello Internetu z webového serveru hello virtuálních počítačů

tooconnect odchozí toohello Internetu z webového serveru hello virtuálních počítačů, dokončení hello následující kroky:

1. Pokud ještě nemáte připojení ke vzdálené toohello, otevřete MyWebServerVM, ujistěte se, toohello vzdáleného připojení virtuálních počítačů pomocí kroků hello v hello [Connect toohello webový server virtuálního počítače z hello Internet](#connect-from-internet) tohoto článku.
2. Z plochy Windows hello otevřete Internet Explorer. V hello **instalační program aplikace Internet Explorer 11** dialogové okno, klikněte na tlačítko **nepoužívejte doporučená nastavení**, pak klikněte na tlačítko **OK**. Je doporučené tooaccept hello doporučené nastavení pro provozní server.
3. Do adresního řádku Internet Exploreru hello, zadejte [vyhledávače bing.com](http:www.bing.com). Pokud se zobrazí dialogové okno aplikace Internet Explorer, klikněte na tlačítko **přidat**, pak **přidat** v hello **Důvěryhodné servery** dialogové okno a klikněte na tlačítko **Zavřít**. Tento postup opakujte pro všechna další dialogová okna aplikace Internet Explorer.
4. V hello Bing stránka hledání, zadejte *whatsmyipaddress*, pak klikněte na tlačítko s ikonou lupy hello. Bing vrátí hello veřejnou IP adresu přiřazenou toohello prostředek veřejné IP adresy vytvoří hello portálu, když jste vytvořili hello virtuálních počítačů. Pokud si projdete hello nastavení pro hello **MyWebServer ip** prostředků, uvidíte hello stejnou IP adresu přiřadit toohello prostředek veřejné IP adresy, jak je znázorněno v hello obrázek, který následuje dále. Hello IP adresu přiřazenou tooyour virtuálních počítačů se liší ale.

    ![Připojení serveru tooweb virtuálních počítačů](./media/virtual-network-get-started-vnet-subnet/webserver-pip.png)

5.  Ponechte připojení ke vzdálené ploše hello otevřete toocomplete hello kroků v další části hello.

Vzhledem k tomu, že všechny odchozí připojení z hello virtuálních počítačů je povoleno ve výchozím nastavení se možné tooconnect toohello Internetu z hello virtuálních počítačů. Odchozí připojení můžete omezit přidáním přidání pravidla toohello skupina NSG použitá toohello síťového adaptéru, toohello podsíť hello síťový adaptér je připojený, nebo obojí.

Pokud hello virtuálního počítače je uvést do hello zastavena (deallocated) stavu pomocí hello portálu, můžete změnit hello veřejnou IP adresu. Pokud požadujete hello veřejných IP adres nikdy změn, můžete použít hello statickou metodou přidělení pro hello IP adresu, nikoli metody dynamického přidělení hello, (což je výchozí hello). Další informace o toolearn hello rozdíly mezi metody přidělení, přečtěte si hello [IP adres, typy a metody přidělení](virtual-network-ip-addresses-overview-arm.md) článku.

### <a name="webserver-to-dbserver"></a>Připojení serveru databáze toohello virtuálních počítačů z webového serveru hello virtuálních počítačů

aplikace tooconnect toohello databázového serveru virtuálního počítače z webového serveru hello virtuálních počítačů, dokončení hello následující kroky:

1. Pokud ještě nemáte připojení ke vzdálené toohello, otevřete MyWebServer virtuálních počítačů, ujistěte se, toohello vzdáleného připojení virtuálních počítačů pomocí kroků hello v hello [Connect toohello webový server virtuálního počítače z hello Internet](#connect-from-internet) tohoto článku.
2. Klikněte na tlačítko Start hello v levém dolním rohu hello plochy Windows hello a pak začněte psát *vzdálené plochy*. Když se zobrazí seznam nabídky Start hello **připojení ke vzdálené ploše**, klikněte na něj.
3. V hello **připojení ke vzdálené ploše** dialogovém okně zadejte *DBServer* hello názvu počítače a klikněte na tlačítko **Connect**.
4. Zadejte hello uživatelského jména a hesla, které jste zadali v kroku 3 hello [vytvořit hello databázový server virtuálního počítače](#create-database-server-vm) části tohoto článku, klikněte **OK**.
5. Pokud se zobrazí, dialogové okno pole informování, můžete tuto identitu hello hello vzdáleného počítače nelze ověřit, klikněte na tlačítko **Ano**.
6. Ponechte připojení ke vzdálené ploše hello tooboth servery, které otevřete toocomplete hello kroků v další části hello.

Budete mít toomake hello připojení toohello databázového serveru virtuálního počítače z hello webový server virtuálních počítačů pro hello následujících důvodů:

- Příchozí připojení TCP nebo 3389 jsou povolené pro všechny zdrojové IP adresy v NSG vytvořen v kroku 5 hello výchozí hello [vytvořit hello databázový server virtuálního počítače](#create-database-server-vm) tohoto článku.
- Inicializovali hello připojení z webového serveru hello virtuální počítač, který je připojený toohello stejnou virtuální síť jako databázového serveru hello virtuálních počítačů. tooconnect tooa virtuální počítač, který nemá veřejnou IP adresu přiřazenou tooit, je nutné připojit z jiného virtuálního počítače připojené toohello stejné virtuální síti, i když hello virtuální počítač je připojený tooa jiné podsíti.
- I když hello virtuální počítače jsou připojené toodifferent podsítě, Azure vytvoří výchozí trasy, které umožňují připojení mezi podsítěmi. Vytvořením vlastní ale můžete přepsat výchozí trasy hello. Čtení hello [trasy definované uživatelem](virtual-networks-udr-overview.md) článku toolearn více informací o směrování v Azure.

Pokud se pokusíte tooinitiate vzdáleného připojení toohello databázový server virtuálního počítače z hello Internetu, jako jste to udělali v hello [Connect toohello webový server virtuálního počítače z hello Internet](#connect-from-internet) části tohoto článku, uvidíte, že hello **připojit** možnost je zobrazena šedě. Připojení je šedě, protože neexistuje žádný veřejnou IP adresu přiřazenou toohello virtuálních počítačů, takže tooit příchozí připojení z Internetu hello nejsou možné.

### <a name="connect-toohello-internet-from-hello-database-server-vm"></a>Připojit toohello Internet ze serveru databáze hello virtuálních počítačů

Připojte odchozí toohello Internet ze serveru databáze hello virtuálního počítače provedením hello následující kroky:

1. Pokud ještě nemáte toohello vzdáleného připojení virtuálních počítačů DBServer otevřít z hello MyWebServer virtuálních počítačů, dokončení hello kroky v hello [Connect toohello databázového serveru virtuálního počítače z webového serveru hello virtuálních počítačů](#webserver-to-dbserver) tohoto článku.
2. Z plochy Windows hello na hello DBServer virtuální počítač, otevřete Internet Explorer a reagovat toohello dialogových oken, jako jste to udělali v kroky 2 a 3 hello [připojit toohello Internetu z webového serveru hello virtuálních počítačů](#connect-to-internet) tohoto článku.
3. V panelu Adresa hello, zadejte [vyhledávače bing.com](http:www.bing.com).
4. Klikněte na tlačítko **přidat** v hello Internet Explorer dialogu, který se zobrazí, **přidat**, pak **Zavřít** v hello **důvěryhodné** dialogové okno lokality. Stejné kroky proveďte ve všech dalších dialogových oknech, která se případně zobrazí.
5. V hello Bing stránka hledání, zadejte *whatsmyipaddress*, pak klikněte na tlačítko s ikonou lupy hello. Bing vrátí hello veřejné IP adresy, které jsou přiřazeny toohello virtuálních počítačů na základě hello infrastruktury Azure. 6. Zavřete hello vzdálené plochy toohello DBServer virtuálních počítačů z hello MyWebServer virtuálních počítačů a potom zavřete hello vzdáleného připojení toohello MyWebServer virtuálních počítačů.

toohello Hello odchozí připojení k Internetu je povolena, protože všechny odchozí přenosy je povoleno ve výchozím nastavení, i když prostředek veřejné IP adresy není přiřazen toohello DBServer virtuálních počítačů. Všechny virtuální počítače, ve výchozím nastavení, je možné tooconnect odchozí toohello Internetu, s nebo bez veřejné toohello prostředků přiřazené IP adresy virtuálních počítačů. Nejste možné tooconnect toohello veřejnou IP adresu z hello Internetu však jako měla mít toofor hello MyWebServer virtuální počítač, který má veřejnou IP adresu adres zdroj přiřazený.

## <a name="delete-resources"></a>Odstranění všech prostředků

toodelete vytvořit všechny prostředky v tomto článku, dokončení hello následující kroky:

1. tooview hello MyRG vytvoření skupiny prostředků v tomto článku, dokončení kroků 1 až 3 v hello [zkontrolujte prostředky](#review) tohoto článku. Znovu zkontrolujte hello prostředky ve skupině prostředků hello. Pokud jste vytvořili skupinu prostředků MyRG hello, podle předchozího postupu, uvidíte hello 12 prostředky znázorněno hello obrázku v kroku 4.
2. V okně MyRG hello, klikněte na tlačítko hello **odstranit** tlačítko.
3. Hello portál vyžaduje tootype hello název hello prostředků skupiny tooconfirm, které chcete toodelete ho. Pokud uvidíte prostředky jiné než hello prostředky uvedené v kroku 4 hello [zkontrolujte prostředky](#review) části tohoto článku, klikněte na tlačítko **zrušit**. Pokud se zobrazí pouze hello 12 prostředky, které jsou vytvořené jako součást tohoto článku, zadejte *MyRG* hello název skupiny prostředků, klikněte **odstranit**. Odstranění skupiny prostředků se odstraní všechny prostředky v rámci skupiny prostředků hello, takže vždy být zda tooconfirm hello obsah skupinu prostředků. před odstraněním. Odstraní všechny prostředky obsažené v rámci skupiny prostředků hello Hello portálu, a pak odstraní samotná skupina prostředků hello. Tento proces trvá několik minut.

## <a name="next-steps"></a>Další kroky

V tomto cvičení jste vytvořili virtuální síť a dva virtuální počítače. Během vytváření virtuálních počítačů jste zadali některá vlastní nastavení a přijali několik výchozích nastavení. Doporučujeme, abyste si přečetli hello následující články, před nasazením produkční virtuálních sítí a virtuálních počítačů, tooensure pochopit všech dostupných nastavení:

- [Virtuální sítě](virtual-networks-overview.md)
- [Veřejné IP adresy](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses)
- [Síťová rozhraní](virtual-network-network-interface.md)
- [Skupiny zabezpečení sítě](virtual-networks-nsg.md)
- [Virtual Machines](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
