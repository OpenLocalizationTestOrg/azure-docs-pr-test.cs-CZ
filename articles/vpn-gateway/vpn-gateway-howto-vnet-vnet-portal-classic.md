---
title: "Vytvořte připojení mezi virtuálními sítěmi: classic: portálu Azure | Microsoft Docs"
description: "Jak společně tooconnect Azure virtuální sítě pomocí prostředí PowerShell a hello portál Azure classic."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: f29c3c091d049ff96cf59f4c28ab86a100bcb5fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vnet-to-vnet-connection-classic"></a>Konfigurace připojení typu VNet-to-VNet (klasické)

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Tento článek ukazuje, jak toocreate připojení k bráně VPN mezi virtuálními sítěmi. Hello virtuální sítě může být v hello stejné nebo různých oblastí, a z hello stejné nebo různých předplatných. Hello kroky v tomto článku se vztahují toohello modelu nasazení classic a hello portálu Azure. Můžete také vytvořit této konfigurace pomocí nástroje pro jiné nasazení nebo model nasazení tak, že vyberete jinou možnost z hello následující seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure Portal (Classic)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Propojení různých modelů nasazení – Azure Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Propojení různých modelů nasazení – PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

![Virtuální síť tooVNet Diagram připojení](./media/vpn-gateway-howto-vnet-vnet-portal-classic/v2vclassic.png)

## <a name="about-vnet-to-vnet-connections"></a>Informace o propojeních VNet-to-VNet

Propojení virtuální sítě tooanother virtuální síť (VNet-to-VNet) v modelu nasazení classic hello použitím brány VPN je podobné tooconnecting umístění lokality tooan místní virtuální sítě. Oba typy připojení využívají bránu tooprovide sítě VPN přes zabezpečené tunelové propojení prostřednictvím protokolu IPsec/IKE.

Hello připojení virtuální sítě může být v různých předplatných a různých oblastech. Můžete kombinovat komunikaci VNet tooVNet s konfigurací s více servery. Díky tomu je možné vytvářet topologie sítí, ve kterých se používá propojování více míst i propojování virtuálních sítí.

![Virtuální síť tooVNet připojení](./media/vpn-gateway-howto-vnet-vnet-portal-classic/aboutconnections.png)

### <a name="why"></a>Proč propojovat virtuální sítě?

Tooconnect virtuální sítě může být vhodné pro hello následujících důvodů:

* **Geografická redundance napříč oblastmi a geografická přítomnost**

  * Můžete nastavit vlastní geografickou replikaci nebo synchronizaci se zabezpečeným připojením bez procházení koncovými body připojenými k internetu.
  * S nástroj pro vyrovnávání zatížení Azure a Microsoft nebo třetích stran clustering technologií můžete nastavit úlohy s vysokou dostupností s geografickou redundancí nad několika oblastmi Azure. Jedním z důležitých příkladů je tooset až SQL Always On se skupinami dostupnosti nad několika oblastmi Azure.
* **Regionální vícevrstvé aplikace s silnou izolaci hranic**

  * Hello uvnitř stejné oblasti, můžete nastavit vícevrstvé aplikace s více virtuálních sítí připojené společně s silnou izolaci a zabezpečenou komunikaci mezi vrstvy.
* **Mezi předplatného, komunikace mezi organizace v Azure**

  * Pokud máte víc předplatných Azure, můžete se připojit úlohy z různých předplatných společně bezpečně mezi virtuálními sítěmi.
  * Pro podniky a poskytovatelé služeb můžete povolit komunikaci mezi organizace s zabezpečené technologie VPN v rámci Azure.

Další informace o připojení VNet-to-VNet najdete v tématu [VNet-to-VNet aspekty](#faq) na konci hello tohoto článku.

### <a name="before-you-begin"></a>Než začnete

Před zahájením tohoto cvičení, stáhněte a nainstalujte nejnovější verzi rutin prostředí PowerShell Azure Service Management (SM) hello hello. Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview). Pro většinu hello kroků používáme hello portálu, ale je třeba pomocí prostředí PowerShell toocreate hello připojení mezi hello virtuální sítě. Nelze vytvořit připojení hello pomocí hello portálu Azure.

## <a name="plan"></a>Krok 1: Plánování rozsahů IP adres

Je důležité toodecide hello rozsahy, že použijete tooconfigure virtuálních sítí. Pro tuto konfiguraci musíte zkontrolovat, že žádný z vaší virtuální sítě rozsahů nepřekrývá mezi sebou, nebo s žádným z hello místní sítě, které se připojují k.

Hello následující tabulka uvádí příklad toodefine vaší virtuální sítě. Rozsahy hello použijte jako vodítko pouze. Poznamenejte si hello rozsahy virtuálních sítí. Je třeba tyto informace pro pozdější kroky.

**Příklad**

| Virtual Network | Adresní prostor | Oblast | Připojí toolocal síťovou lokalitu |
|:--- |:--- |:--- |:--- |
| Virtuální síť TestVNet1 |Virtuální síť TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |Východ USA |VNet4Local<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| Virtuální síť TestVNet4 |Virtuální síť TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |Západní USA |VNet1Local<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

## <a name="vnetvalues"></a>Krok 2 – Vytvoření hello virtuální sítě

Vytvoření dvou virtuálních sítí v hello [portál Azure](https://portal.azure.com). Hello kroky toocreate klasické virtuální sítě, najdete v části [vytvoření klasické virtuální sítě](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). 

Pokud používáte portál toocreate hello klasickou virtuální síť, je nutné přejít okno toohello virtuální sítě pomocí následujících kroků, jinak se nezobrazí možnost toocreate hello klasickou virtuální síť hello:

1. Klikněte na tlačítko hello '+' tooopen hello 'New' okno.
2. V poli "Marketplace vyhledávání hello" hello zadejte "Virtuální sítě". Pokud místo toho vyberte síť -> virtuální sítě, nebude získat hello možnost toocreate klasické virtuální sítě.
3. Vyhledejte "virtuální sítě, z hello vrátil seznam a klikněte na něj tooopen hello virtuální sítě okno. 
4. V okně hello virtuální sítě vyberte 'Classic' toocreate klasické virtuální sítě. 

Pokud používáte tento článek jako cvičení, můžete použít následující ukázkové hodnoty hello:

**Hodnoty pro virtuální síť TestVNet1**

Název: TestVNet1<br>
Adresní prostor: 10.11.0.0/16, 10.12.0.0/16 (volitelné)<br>
Název podsítě: výchozí<br>
Rozsah adres podsítě: 10.11.0.1/24<br>
Skupina prostředků: ClassicRG<br>
Umístění: Východní USA<br>
GatewaySubnet: 10.11.1.0/27

**Hodnoty pro virtuální síť TestVNet4**

Název: virtuální sítě TestVNet4<br>
Adresní prostor: 10.41.0.0/16, 10.42.0.0/16 (volitelné)<br>
Název podsítě: výchozí<br>
Rozsah adres podsítě: 10.41.0.1/24<br>
Skupina prostředků: ClassicRG<br>
Umístění: Západní USA<br>
GatewaySubnet: 10.41.1.0/27

**Při vytváření vaší virtuální sítě, mějte na paměti hello následující nastavení:**

* **Adresní prostory virtuální sítě** – na stránce hello adresní prostory virtuální sítě, zadejte rozsah adres hello chcete toouse pro vaši virtuální síť. Toto jsou hello dynamické IP adresy, které budou přiřazeny toohello virtuální počítače a další instance rolí, abyste nasadili toothis virtuální sítě.<br>hello Hello adresní prostory, které vyberete se nesmí překrývat s hello adresní prostory pro některý z jiných virtuálních sítí nebo na místní umístění, které této virtuální sítě se budou připojovat k.

* **Umístění** – když vytvoříte virtuální síť, přiřaďte ji k Azure umístěním (oblastí). Například pokud chcete, aby vaše virtuální počítače, které jsou nasazeny toobe virtuální sítě tooyour fyzicky umístěné ve západní USA, vyberte toto umístění. Nelze změnit umístění hello přidruženou k virtuální síti po jejím vytvoření.

**Po vytvoření vaší virtuální sítě, můžete přidat hello následující nastavení:**

* **Adresní prostor** – další adresní prostor se nevyžaduje pro tuto konfiguraci, ale můžete přidat další adresní prostor po vytvoření hello virtuální sítě.

* **Podsítě** – dodatečné podsítě nejsou vyžadovány pro tuto konfiguraci, ale můžete chtít toohave virtuální počítače v podsíti oddělené od dalších instancí rolí.

* **Servery DNS** – zadejte název serveru DNS hello a IP adresu. Toto nastavení neslouží k vytvoření serveru DNS. Umožňuje vám toospecify hello DNS serverů, které má toouse pro překlad názvů pro tuto virtuální síť.

V této části nakonfigurujete hello typ připojení, hello místní lokality a vytvoření brány hello.

## <a name="localsite"></a>Krok 3: Konfigurace místního webu hello

Azure používá hello nastavení zadané v každé lokalitě toodetermine místní sítě, jak tooroute provoz mezi hello virtuální sítě. Každý virtuální síť musí odkazovat toohello příslušné místní síť, kterou chcete tooroute provoz. Můžete určit název hello chcete toouse toorefer tooeach místní síťový Web. Je nejlepší toouse něco popisný.

Například virtuální sítě TestVNet1 připojuje tooa místní síť, kterou vytvoříte s názvem 'VNet4Local'. Hello nastavení pro VNet4Local obsahovat hello předpony adres pro virtuální síť TestVNet4.

Hello místní lokality pro každou virtuální síť je hello jiné virtuální sítě. Následující ukázkové hodnoty Hello se používají pro naše konfigurace:

| Virtual Network | Adresní prostor | Oblast | Připojí toolocal síťovou lokalitu |
|:--- |:--- |:--- |:--- |
| Virtuální síť TestVNet1 |Virtuální síť TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |Východ USA |VNet4Local<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| Virtuální síť TestVNet4 |Virtuální síť TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |Západní USA |VNet1Local<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

1. Najděte virtuální síť TestVNet1 v hello portálu Azure. V hello **připojení k síti VPN** části hello okna, klikněte na tlačítko **brány**.

    ![Žádná brána](./media/vpn-gateway-howto-vnet-vnet-portal-classic/nogateway.png)
2. Na hello **nové připojení VPN** vyberte **Site-to-Site**.
3. Klikněte na tlačítko **místní lokality** tooopen hello stránka místního webu a nakonfigurovat nastavení hello.
4. Na hello **místní lokality** stránky, název vaší místní lokalitě. V našem příkladu jsme název místního webu hello 'VNet4Local'.
5. Pro **IP adresa brány VPN**, každou IP adresu, která chcete, můžete použít, dokud je v platném formátu. Obvykle byste použili hello skutečné externí IP adresu pro zařízení VPN. Ale pro konfiguraci classic VNet-to-VNet, použijte hello veřejnou IP adresu, která je přiřazena toohello brány pro vaši virtuální síť. Vzhledem k tomu, že jste ještě vytvořili hello brány virtuální sítě, je třeba zadat všechny platné veřejnou IP adresu jako zástupný znak.<br>Nemáte nechte pole prázdné, – není pro tuto konfiguraci volitelné. Později přejděte zpět do těchto nastavení a konfigurace po vygeneruje Azure s hello odpovídající virtuální síť brány IP adresy.
6. Pro **klienta adresní prostor**, použijte hello adresním prostoru hello jiné virtuální sítě. Tooyour plánování příklad naleznete v nápovědě. Klikněte na tlačítko **OK** toosave vaše nastavení a vrácení zpět toohello **nové připojení VPN** okno.

    ![místní lokalita](./media/vpn-gateway-howto-vnet-vnet-portal-classic/localsite.png)

## <a name="gw"></a>Krok 4 – vytvoření brány virtuální sítě hello

Každá virtuální síť musí mít bránu virtuální sítě. Brána virtuální sítě Hello tras a šifruje provoz.

1. Na hello **nové připojení VPN** okně, vyberte hello políčko **vytvořit bránu hned**.
2. Klikněte na tlačítko **podsíť, velikost a typ směrování**. Na hello **konfigurace brány** okně klikněte na tlačítko **podsítě**.
3. název podsítě brány Hello je automaticky vyplněno s požadovaným názvem hello "GatewaySubnet". Hello **rozsahu adres** obsahuje hello IP adresy, které jsou přiděleny toohello služby brány VPN. Některé konfigurace povolit /29, ale doporučené toouse podsíť brány o velikosti/28 nebo /27 tooaccommodate budoucí konfigurace, které mohou vyžadovat další IP adresy pro služby brány hello. V našem příkladu nastavení použijeme 10.11.1.0/27. Upravit hello adresní prostor a potom klikněte na **OK**.
4. Konfigurace hello **velikost brány**. Toto nastavení znamená toohello [skladová položka brány](vpn-gateway-about-vpn-gateway-settings.md#gwsku).
5. Konfigurace hello **typ směrování**. Hello typ směrování pro tuto konfiguraci musí být **dynamické**. Typ směrování hello nelze později změnit, pokud přerušit hello brány a vytvořte novou.
6. Klikněte na **OK**.
7. Na hello **nové připojení VPN** okně klikněte na tlačítko **OK** toobegin vytvoření brány virtuální sítě hello. Vytvoření brány může trvat často 45 minut nebo déle, v závislosti na vybrané skladová položka brány hello.

## <a name="vnet4settings"></a>Krok 5: Konfigurace nastavení virtuální sítě TestVNet4

Opakujte kroky hello příliš[vytvořte místního webu](#localsite) a [vytvořit bránu virtuální sítě hello](#gw) tooconfigure virtuální sítě TestVNet4, nahraďte hodnoty hello, pokud je to nezbytné. Pokud to dělají jako cvičení, použijte hello [ukázkové hodnoty](#vnetvalues).

## <a name="updatelocal"></a>Krok 6: Místní lokality hello aktualizace

Po vytvoření vaší brány virtuální sítě pro obě virtuální sítě, je nutné upravit místní lokality hello **IP adresa brány VPN** hodnoty.

|Název virtuální sítě|Připojená lokalita|IP adresa brány|
|:--- |:--- |:--- |
|Virtuální síť TestVNet1|VNet4Local|IP adresa brány VPN pro virtuální síť TestVNet4|
|Virtuální síť TestVNet4|VNet1Local|IP adresa brány VPN pro virtuální síť TestVNet1|

### <a name="part-1---get-hello-virtual-network-gateway-public-ip-address"></a>Část 1 - Get hello virtuální sítě veřejná IP adresa brány

1. Najděte virtuální síť v hello portálu Azure.
2. Klikněte na tlačítko tooopen hello virtuální síť **přehled** okno. V okně hello v **připojení k síti VPN**, můžete zobrazit hello IP adresy pro bránu virtuální sítě.

  ![Veřejná IP adresa](./media/vpn-gateway-howto-vnet-vnet-portal-classic/publicIP.png)
3. Zkopírujte hello IP adresu. Budete ho používat v další části hello.
4. Opakujte tyto kroky pro virtuální síť TestVNet4

### <a name="part-2---modify-hello-local-sites"></a>Část 2 – upravit místní lokality hello

1. Najděte virtuální síť v hello portálu Azure.
2. Na hello virtuální síť **přehled** okně klikněte na místní lokality hello.

  ![Místní web vytvořený pouze](./media/vpn-gateway-howto-vnet-vnet-portal-classic/local.png)
3. Na hello **připojení Site-to-Site VPN** okně klikněte na název hello hello místní sítě, které chcete toomodify.

  ![Otevřete místní lokality](./media/vpn-gateway-howto-vnet-vnet-portal-classic/openlocal.png)
4. Klikněte na tlačítko hello **místní lokality** , které chcete toomodify.

  ![Upravit web](./media/vpn-gateway-howto-vnet-vnet-portal-classic/connections.png)
5. Aktualizace hello **IP adresa brány VPN** a klikněte na tlačítko **OK** toosave hello nastavení.

  ![IP brány](./media/vpn-gateway-howto-vnet-vnet-portal-classic/gwupdate.png)
6. Zavřete hello dalších oknech.
7. Opakujte tyto kroky pro virtuální síť TestVNet4.

## <a name="getvalues"></a>Krok 7: načtení hodnoty z konfiguračního souboru sítě hello

Při vytváření virtuální sítě classic v hello portálu Azure, hello název, který zobrazí není hello úplný název, který používáte pro prostředí PowerShell. Například virtuální sítě, který se zobrazí toobe s názvem **virtuální sítě TestVNet1** hello portálu, může mít mnoho delší název v souboru konfigurace sítě hello. Hello název může vypadat podobně jako: **skupiny ClassicRG virtuální sítě TestVNet1**. Při vytváření připojení, je důležité toouse hello hodnoty, které se zobrazí v souboru konfigurace sítě hello.

V hello následující kroky připojíte se tooyour účet Azure a stažení a zobrazení, hello sítě konfigurační soubor tooobtain hello hodnoty, které jsou vyžadovány pro připojení.

1. Stáhněte a nainstalujte nejnovější verzi rutin prostředí PowerShell Azure Service Management (SM) hello hello. Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

2. Otevřete konzolu prostředí PowerShell se zvýšenými oprávněními a připojte tooyour účtu. Použijte následující příklad toohelp, ke kterým se připojujete hello:

  ```powershell
  Login-AzureRmAccount
  ```

  Zkontrolujte předplatná hello pro účet hello.

  ```powershell
  Get-AzureRmSubscription
  ```

  Pokud máte více než jedno předplatné, vyberte hello předplatné, které chcete toouse.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```

  Pak pomocí následující rutiny tooadd hello tooPowerShell vaše předplatné Azure pro model nasazení classic hello.

  ```powershell
  Add-AzureAccount
  ```
3. Export a zobrazení hello sítě konfigurační soubor. Vytvoření adresáře v počítači a pak je exportovat hello sítě konfiguračního souboru toohello adresáře. V tomto příkladu hello sítě konfigurační soubor exportu příliš**C:\AzureNet**.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
4. Otevřete soubor hello s názvy textového editoru a zobrazení hello virtuální sítě a lokality. Budou to hello název, který použijete při vytváření připojení.<br>Názvy virtuální sítě jsou uvedeny jako **VirtualNetworkSite name =**<br>Názvy lokalit jsou uvedeny jako **LocalNetworkSiteRef name =**

## <a name="createconnections"></a>Krok 8 – vytvoření brány sítě VPN hello

Když byly dokončeny všechny předchozí kroky hello, můžete nastavit hello protokolu IPsec/IKE předsdílených klíčů a vytvoření připojení hello. Tato sada kroků používá prostředí PowerShell. Připojení VNet-to-VNet pro model nasazení classic hello nelze konfigurovat v hello portálu Azure.

V příkladech hello Všimněte si, že hello sdílený klíč je přesně hello stejné. Hello sdílený klíč musí vždy odpovídat. Být jisti tooreplace hello hodnoty v těchto příkladech s hello přesný názvy virtuálních sítí a místní síťové lokality.

1. Vytvořte připojení tooTestVNet4 hello virtuální sítě TestVNet1.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName 'Group ClassicRG TestVNet1' `
  -LocalNetworkSiteName '17BE5E2C_VNet4Local' -SharedKey A1b2C3D4
  ```
2. Vytvořte připojení tooTestVNet1 virtuální sítě TestVNet4 hello.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName 'Group ClassicRG TestVNet4' `
  -LocalNetworkSiteName 'F7F7BFC7_VNet1Local' -SharedKey A1b2C3D4
  ```
3. Počkejte tooinitialize hello připojení. Jakmile hello brány byla inicializována, hello stav je "Bylo úspěšné".

  ```
  Error          :
  HttpStatusCode : OK
  Id             :
  Status         : Successful
  RequestId      :
  StatusCode     : OK
  ```

## <a name="faq"></a>Aspekty VNet-to-VNet pro virtuální sítě classic
* Hello virtuální sítě může být v hello stejné nebo různých předplatných.
* Hello virtuální sítě může být v hello oblastí Azure stejný nebo jiný (umístění).
* Cloudová služba ani koncový bod vyrovnávání zatížení nemůžou pracovat nad více virtuálními sítěmi ani v případě, že jsou propojeny.
* Propojováním více virtuálních sítí nevyžaduje žádné zařízení VPN.
* VNet-to-VNet podporují propojování virtuálních sítí Azure. Nepodporuje propojování virtuálních počítačů nebo cloudových služeb, které nejsou nasazené tooa virtuální sítě.
* VNet-to-VNet vyžaduje brány s dynamickým směrováním. Azure brány se statickým směrováním nejsou podporovány.
* Možnost připojení k virtuální síti je možné využívat současně se sítěmi VPN s více servery. Je maximálně 10 tunelů VPN pro bránu virtuální sítě VPN propojení tooeither jiné virtuální sítě, a místní sítí.
* Hello adresní prostory virtuální sítě hello a na místě, které se nesmí překrývat místních síťových webů. Překrývající se adresní prostory způsobí, že hello vytvoření virtuální sítě nebo odesílání toofail soubory konfigurace netcfg.
* Redundantní tunelová propojení mezi dvěma virtuálními sítěmi nejsou podporována.
* Všechny tunelovými propojeními VPN pro hello virtuální sítě, včetně P2S sítě VPN, sdílet hello dostupnou šířku pásma pro bránu VPN hello a hello stejné SLA provozu v Azure VPN gateway.
* Provoz VNet-to-VNet se přenáší po hello páteřní síti Azure.

## <a name="next-steps"></a>Další kroky
Ověřte stav připojení. V tématu [ověření připojení VPN Gateway](vpn-gateway-verify-connection-resource-manager.md).
