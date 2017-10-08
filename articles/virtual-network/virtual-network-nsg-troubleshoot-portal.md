---
title: "aaaTroubleshoot skupin zabezpečení sítě - portál | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot skupin zabezpečení sítě v nasazení Azure Resource Manager hello modelu pomocí hello portálu Azure."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: a54feccf-0123-4e49-a743-eb8d0bdd1ebc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 0d3d2110fe1507f36e3b933de924a0876db2747a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-network-security-groups-using-hello-azure-portal"></a>Řešení potíží s hello portálu Azure pomocí skupin zabezpečení sítě
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-nsg-troubleshoot-portal.md)
> * [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
> 
> 

Pokud jste nakonfigurovali skupiny zabezpečení sítě (Nsg) ve virtuálním počítači (VM) a dochází problémy s připojením k virtuální počítač, tento článek obsahuje přehled možností diagnostiky pro řešení potíží s toohelp další skupiny Nsg.

Skupiny Nsg umožňují toocontrol hello typy přenosů s tokem do aplikace a z virtuálních počítačů (VM). Skupiny Nsg můžou být použité toosubnets ve virtuální síti Azure (VNet), síťová rozhraní (NIC) nebo obojí. Hello efektivní pravidla použít tooa síťový adaptér jsou hello pravidla, které existují v hello skupiny Nsg použije tooa síťových Adaptérů a hello podsíť, ve které je připojený k agregaci. Pravidla v rámci těchto skupin Nsg můžete někdy vzájemném konfliktu a mít vliv na připojení k síti Virtuálního počítače.  

Můžete zobrazit všechna pravidla efektivní zabezpečení hello od vaší skupiny Nsg použije na síťové adaptéry Virtuálního počítače. Tento článek ukazuje, jak hello problémy s připojením k tootroubleshoot virtuálních počítačů pomocí těchto pravidel v modelu nasazení Azure Resource Manager. Pokud si nejste obeznámeni s virtuální sítí a NSG koncepty, přečtěte si hello [virtuální síť](virtual-networks-overview.md) a [skupin zabezpečení sítě](virtual-networks-nsg.md) přehled články.

## <a name="using-effective-security-rules-tootroubleshoot-vm-traffic-flow"></a>Pomocí tok přenosů dat virtuálního počítače tootroubleshoot efektivní pravidla zabezpečení
Hello scénář, který následuje je příkladem častých problémů připojení:

Virtuální počítač s názvem *VM1* je součástí podsíť s názvem *Subnet1* v rámci virtuální sítě s názvem *WestUS VNet1*. Pokusu o tooconnect toohello virtuálního počítače pomocí protokolu RDP přes TCP port 3389 selže. Skupiny Nsg se použijí v obou hello seskupování *VM1 NIC1* a hello podsíť *Subnet1*. V hello NSG přidružená hello síťové rozhraní je povolen provoz tooTCP portu 3389 *VM1 NIC1*, ale TCP příkazem ping otestovat tooVM1 na portu 3389 selže.

Při tomto příkladu používá TCP port 3389, hello následující postup se dá použít toodetermine selhání příchozí a odchozí připojení prostřednictvím libovolný port.

### <a name="vm"></a>Zobrazení pravidla efektivní zabezpečení pro virtuální počítač
Proveďte následující kroky tootroubleshoot skupiny Nsg pro virtuální počítač hello:

Úplný seznam pravidel hello efektivní zabezpečení můžete zobrazit na síťovém adaptéru, z hello virtuální počítač. Můžete také přidat, upravit a odstranit pravidla NSG síťových Adaptérů a podsítě v okně efektivní pravidla hello, pokud máte oprávnění tooperform těchto operací.

1. Přihlášení toohello portál Azure na https://portal.azure.com.
2. Klikněte na tlačítko **další služby**, pak klikněte na tlačítko **virtuální počítače** v zobrazeném seznamu hello.
3. Vyberte virtuální počítač tootroubleshoot hello seznamu, které se zobrazí a zobrazí se okno virtuálních počítačů s možnostmi.
4. Klikněte na tlačítko **Diagnostikujte & řešení problémů** a pak vyberte častých problémů. V tomto příkladu **nelze se připojit toomy virtuální počítač s Windows** je vybrána. 
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image1.png)
5. Kroky se zobrazí pod hello problému, jak je znázorněno v následujícím obrázku hello: 
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image2.png)
   
    Klikněte na tlačítko *pravidel skupiny zabezpečení efektivní* hello seznamu doporučené kroky.
6. Hello **získat efektivní zabezpečení pravidla** okno se zobrazí, jak je znázorněno v následujícím obrázku hello:
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image3.png)
   
    Všimněte si následujících sekcí hello obrázek hello:
   
   * **Obor:** nastavit příliš*VM1*, hello virtuální počítač vybrali v kroku 3.
   * **Síťové rozhraní:** *VM1 NIC1* je vybrána. Virtuální počítač může mít více síťových rozhraní (NIC). Každý síťový adaptér může mít jedinečnou efektivní pravidla. Při řešení potíží, budete potřebovat tooview hello efektivní zabezpečení pravidla pro každý síťový adaptér.
   * **Přidružené skupiny Nsg:** skupiny Nsg můžou být použité tooboth hello síťových Adaptérů a hello podsíť hello síťový adaptér je připojen k. Hello obrázku byl skupina NSG použitá tooboth hello síťových Adaptérů a hello podsíť, ve které je připojený k. Kliknutím na názvy NSG hello toodirectly upravit pravidla v hello skupiny Nsg.
   * **Karta VM1 nsg:** hello seznam pravidel zobrazí hello obrázku je hello NSG použije toohello síťový adaptér. Několik výchozích pravidel vytvářejí Azure vždy, když je vytvořena skupina NSG. Nelze odebrat hello výchozí pravidla, ale je možné přepsat pravidla s vyšší prioritou. Další informace o výchozích pravidel, přečtěte si hello toolearn [NSG přehled](virtual-networks-nsg.md#default-rules) článku.
   * **CÍLOVÝ sloupec:** některé hello pravidla mají text ve sloupci hello jiné mají předpony adres. Hello text je hello název výchozí pravidlo zabezpečení toohello značky použít při vytváření. Hello značky jsou identifikátory poskytované systémem, které představují více předpony. Vybrat pravidlo s značkou, jako například *AllowInternetOutBound*, uvádí hello předpony v hello **předpony adres** okno.
   * **Stáhnout:** hello seznam pravidel, může trvat dlouho. Soubor .csv hello pravidel pro offline analýzu můžete stáhnout kliknutím **Stáhnout** a uložení souboru hello.
   * **AllowRDP** příchozí pravidlo: Toto pravidlo umožňuje toohello připojení RDP virtuálních počítačů.
7. Klikněte na tlačítko hello **Subnet1 NSG** kartě tooview hello efektivní pravidla z hello NSG používají toohello podsíť, jak je znázorněno v následujícím obrázku hello: 
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image4.png)
   
    Všimněte si hello *denyRDP* **příchozí** pravidlo. Použije na podsíť hello příchozí pravidla se vyhodnocují před pravidla používaná v hello síťové rozhraní. Vzhledem k tomu, že hello odepřít, že pravidlo se použije na podsíť hello, hello požadavek tooconnect tooTCP 3389 nezdaří, protože hello pravidlo povolit v hello síťový adaptér je nikdy vyhodnocen. 
   
    Hello *denyRDP* pravidlo je hello důvod, proč se nedaří hello připojení RDP. Jeho odebráním by měla vyřešit problém hello.
   
   > [!NOTE]
   > Pokud hello virtuálních počítačů spojené s hello síťový adaptér není ve spuštěném stavu, nebo skupin Nsg nebyly použité toohello síťového adaptéru nebo podsíť, zobrazí se žádná pravidla.
   > 
   > 
8. Klikněte na tlačítko pravidla NSG tooedit, *Subnet1 NSG* v hello **přidružené skupiny Nsg** části.
   Tím se otevře hello **Subnet1 NSG** okno. Pravidla hello lze přímo upravit kliknutím na **příchozí pravidla zabezpečení**.
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image7.png)
9. Po odebrání hello *denyRDP* příchozí pravidlo v hello **Subnet1 NSG** a přidání *allowRDP* pravidlo seznamu efektivní pravidel hello vypadá hello následujícím obrázku:
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image8.png)
   
    Zkontrolujte, zda TCP port 3389 otevřete otevřením toohello připojení RDP virtuálního počítače nebo pomocí nástroje Pspingu hello. Další informace o Pspingu ve čtení hello [stránky pro stažení Pspingu](https://technet.microsoft.com/sysinternals/psping.aspx).

### <a name="nic"></a>Zobrazení pravidla efektivní zabezpečení pro rozhraní sítě
Pokud vaše tok přenosů dat virtuálního počítače je ovlivněn pro konkrétní síťové karty, můžete zobrazit úplný seznam hello efektivní pravidla pro hello síťový adaptér z kontextu rozhraní sítě hello provedením hello následující kroky:

1. Přihlášení toohello portál Azure na https://portal.azure.com.
2. Klikněte na tlačítko **další služby**, pak klikněte na tlačítko **síťových rozhraní** v zobrazeném seznamu hello.
3. Vyberte síťové rozhraní. V následujícím obrázku hello, síťový adaptér s názvem *VM1 NIC1* je vybrána.
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image5.png)
   
    Všimněte si, že hello **oboru** nastavena vybrané toohello síťové rozhraní. Další informace o toolearn hello Další informace, které ukazuje, přečtěte si krok 6 hello **odstraňování skupin Nsg pro virtuální počítač** tohoto článku.
   
   > [!NOTE]
   > Pokud je skupina NSG odebrán ze síťového rozhraní, hello podsíť NSG je stále platná na hello Zadaný síťový adaptér. V takovém případě by výstup hello zobrazit pouze pravidla z podsítě hello NSG. Pravidla se zobrazí, pouze pokud hello síťový adaptér je připojený tooa virtuálních počítačů.
   > 
   > 
4. Můžete přímo upravit pravidla pro skupiny Nsg přidružená síťový adaptér a podsítě. toolearn jak, číst, krok 8 hello **zobrazit pravidla efektivní zabezpečení pro virtuální počítač** tohoto článku.

## <a name="nsg"></a>Zobrazení pravidla efektivní zabezpečení pro skupinu zabezpečení sítě (NSG)
Při úpravě pravidla NSG, může být vhodné tooreview hello dopad pravidla hello přidávané na konkrétním virtuálním počítači. Můžete zobrazit úplný seznam pravidel hello efektivní zabezpečení pro všechny hello síťovými adaptéry, které daný NSG se použije, bez nutnosti tooswitch kontext z hello zadané okno NSG. tootroubleshoot efektivní pravidel ve skupině NGS, dokončení hello následující kroky:

1. Přihlášení toohello portál Azure na https://portal.azure.com.
2. Klikněte na tlačítko **další služby**, pak klikněte na tlačítko **skupin zabezpečení sítě** v zobrazeném seznamu hello.
3. Vyberte skupinu NSG. V následujícím obrázku hello nebyla vybrána skupina NSG s názvem VM1 nsg.
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image6.png)
   
    Všimněte si následujících sekcí předchozí obrázek hello hello:
   
   * **Obor:** nastavit toohello vybrána skupina NSG.
   * **Virtuální počítač:** při NSG podsíti použité tooa, je použité tooall síťové rozhraní připojené tooall virtuální počítače připojené toohello podsítě. Tento seznam obsahuje tato skupina NSG se použije pro všechny virtuální počítače. Žádné virtuální počítače můžete vybrat ze seznamu hello.
     
     > [!NOTE]
     > Pokud skupina NSG použitá tooonly podsíť prázdný, virtuální počítače nebude v seznamu uveden. Pokud skupina NSG použitá tooa síťový adaptér, který není přidružený virtuální počítač, nebude uvedené také tyto síťové adaptéry. 
     > 
     > 
   * **Síťové rozhraní:** virtuální počítač může mít více síťových rozhraní. Můžete vybrat síťové rozhraní připojené toohello vybraný virtuální počítač.
   * **AssociatedNSGs:** kdykoli, síťový adaptér může mít až tootwo efektivní skupin Nsg, jeden použít toohello síťový adaptér a hello jiných toohello podsítě. I když hello obor je vybrán jako VM1-nsg, pokud má podsíť efektivní NSG hello síťový adaptér, se zobrazí výstup hello obě skupiny Nsg.
4. Můžete přímo upravit pravidla pro skupiny zabezpečení sítě spojená s síťový adaptér nebo podsíť. toolearn jak, číst, krok 8 hello **zobrazit pravidla efektivní zabezpečení pro virtuální počítač** tohoto článku.

Další informace o toolearn hello Další informace, které ukazuje, přečtěte si krok 6 hello **zobrazit pravidla efektivní zabezpečení pro virtuální počítač** tohoto článku.

> [!NOTE]
> Když podsítě a síťový adaptér mohou mít pouze jeden toothem skupina NSG se použije, může být skupina NSG přidružená toomultiple síťových adaptérů a více podsítí.
> 
> 

## <a name="considerations"></a>Požadavky
Mějte na paměti následující body při řešení potíží s potíže s připojením k hello:

* Výchozí pravidla NSG se blokují příchozí přístup z hello Internetu a povolení jenom virtuální síť příchozí přenosy. Pravidla musí být explicitně přidán tooallow příchozí přístup z Internetu, podle potřeby.
* Pokud neexistují žádná pravidla zabezpečení NSG způsobuje toofail připojení k síti Virtuálního počítače, může být problém hello příčiny:
  * Software brány firewall běžící v rámci operačního systému hello Virtuálního počítače
  * Trasy nakonfigurované pro virtuální zařízení nebo místní provoz. Přenosy z Internetu může být přesměrovaného tooon místní prostřednictvím vynucené tunelování. Připojení k protokolu RDP/SSH z Internetu tooyour hello virtuálních počítačů nemusí fungovat s tímto nastavením, v závislosti na tom, jak hello místní síťový hardware zpracovává tento provoz. Čtení hello [řešení potíží s trasy](virtual-network-routes-troubleshoot-powershell.md) toolearn článku jak toodiagnose trasy problémy, které může být zpomalovat hello tok přenosů dat do a z hello virtuálních počítačů. 
* Pokud máte peered virtuálních sítí, ve výchozím nastavení, rozbalte hello VIRTUAL_NETWORK značky automaticky tooinclude předpon pro peered virtuální sítě. Tyto předpony můžete zobrazit v hello **ExpandedAddressPrefix** seznamu, tootroubleshoot všechny problémy související tooVNet partnerský vztah připojení. 
* Efektivní zabezpečení pravidla se zobrazují pouze pokud je, že skupina NSG přidružená hello Virtuálního počítače síťový adaptér a nebo podsíť. 
* Pokud neexistují žádné skupiny Nsg přidružená hello síťový adaptér nebo podsíť a jestli máte veřejnou IP adresu přiřadit tooyour virtuálních počítačů, budou všechny porty otevřené pro příchozí a odchozí přístup. Pokud hello virtuálních počítačů má veřejnou IP adresu, použití skupin Nsg toohello síťový adaptér nebo podsíť, důrazně doporučujeme.

