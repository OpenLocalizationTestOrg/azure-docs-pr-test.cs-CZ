---
title: "Připojení klasické virtuální sítě tooAzure virtuální sítě Resource Manager: portál | Microsoft Docs"
description: "Zjistěte, jak toocreate připojení VPN mezi klasické virtuální sítě a virtuální sítě Resource Manager pomocí brány sítě VPN a hello portálu"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 5a90498c-4520-4bd3-a833-ad85924ecaf9
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: cherylmc
ms.openlocfilehash: bef63b4e829335b2e1a9434a35ebfe33b4fd7373
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-hello-portal"></a>Připojit virtuální sítě z různé modely nasazení pomocí portálu hello

Tento článek ukazuje, jak tooconnect klasické virtuální sítě tooResource Správce virtuálních sítí tooallow hello prostředků umístěných v hello samostatné nasazení modely toocommunicate mezi sebou. Hello kroky v tomto článku používají primárně hello portálu Azure, ale můžete také vytvořit této konfigurace pomocí prostředí PowerShell hello výběrem hello článek z tohoto seznamu.

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

Připojení klasické virtuální sítě tooa virtuální sítě Resource Manageru je podobné tooconnecting umístění lokality tooan místní virtuální síť. Oba typy připojení využívají bránu tooprovide sítě VPN přes zabezpečené tunelové propojení prostřednictvím protokolu IPsec/IKE. Můžete vytvořit připojení mezi virtuální sítě, které jsou v různých předplatných a v různých oblastech. Virtuální sítě, které už máte připojení tooon místní sítě, můžete také připojit, pokud je hello brány, které byly nakonfigurovány k dynamické nebo založené na trasách. Další informace o připojení VNet-to-VNet, najdete v části hello [nejčastější dotazy týkající se propojení VNet-to-VNet](#faq) na konci hello tohoto článku. 

Pokud vaše virtuální sítě jsou v hello stejné oblasti, může být vhodné tooinstead zvažte připojení pomocí virtuální sítě partnerský vztah. Partnerské vztahy virtuálních sítí nepoužívají bránu VPN. Další informace najdete v tématu [Partnerské vztahy virtuálních sítí](../virtual-network/virtual-network-peering-overview.md). 

### <a name="prerequisites"></a>Požadavky

* Tento postup předpokládá, že jste již vytvořili obě virtuální sítě. Pokud používáte tento článek jako cvičení a nemáte virtuálních sítí, existují odkazy v hello kroky toohelp jejich vytvoření.
* Ověřte, zda pro virtuální sítě se nepřekrývají mezi sebou hello rozsahy adres hello nebo překrývají s žádným z hello rozsahy pro další připojení této brány může být připojen k hello.
* Nainstalujte hello nejnovější rutiny prostředí PowerShell pro Resource Manager a Service Management (klasické). V tomto článku používáme hello portál Azure a prostředí PowerShell. PowerShell je požadovaná toocreate hello připojení z hello klasické virtuální sítě toohello virtuální sítě Resource Manageru. Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview). 

### <a name="values"></a>Příklady nastavení

Můžete použít tyto hodnoty toocreate testovací prostředí nebo odkazovat toothem toobetter pochopit hello příklady v tomto článku.

**Klasické virtuální sítě**

Název virtuální sítě = ClassicVNet <br>
Adresní prostor = 10.0.0.0/24 <br>
Podsíť 1 = 10.0.0.0/27 <br>
Skupina prostředků = ClassicRG <br>
Umístění = západní USA <br>
GatewaySubnet = 10.0.0.32/28 <br>
Místní lokalita = RMVNetLocal <br>

**Virtuální sítě Resource Manageru**

Název virtuální sítě = RMVNet <br>
Adresní prostor = 192.168.0.0/16 <br>
Podsíť 1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Skupina prostředků = RG1 <br>
Umístění = východní USA <br>
Název brány virtuální sítě = RMGateway <br>
Typ brány sítě VPN = <br>
Typ sítě VPN = trasové <br>
Název veřejné IP adresy brány = rmgwpip <br>
Brána místní sítě = ClassicVNetLocal <br>
Název připojení = RMtoClassic

### <a name="connection-overview"></a>Připojení – přehled

Pro tuto konfiguraci vytvořit připojení k bráně VPN přes tunelové propojení protokolu IPsec/IKE VPN mezi virtuálními sítěmi hello. Ujistěte se, že žádný z vaší virtuální sítě rozsahů nepřekrývá mezi sebou, nebo s žádným z hello místní sítě, které se připojují k.

Hello následující tabulka uvádí příklad, jak jsou definovány hello příklad virtuálních sítí a místních webů:

| Virtual Network | Adresní prostor | Oblast | Připojí toolocal síťovou lokalitu |
|:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |Západní USA | RMVNetLocal (192.168.0.0/16) |
| RMVNet | (192.168.0.0/16) |Východ USA |ClassicVNetLocal (10.0.0.0/24) |

## <a name="classicvnet"></a>1. Nakonfigurujte nastavení virtuální sítě classic hello

V této části vytvoříte hello místní síť (místní lokality) a hello brány virtuální sítě pro vaše klasické virtuální sítě. Pokud nemáte klasické virtuální síti a jsou spuštěné tyto kroky jako cvičení, můžete vytvořit virtuální síť pomocí [v tomto článku](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) a hello [příklad](#values) hodnoty nastavení z výše.

Pokud používáte portál toocreate hello klasickou virtuální síť, je nutné přejít okno toohello virtuální sítě pomocí následujících kroků, jinak se nezobrazí možnost toocreate hello klasickou virtuální síť hello:

1. Klikněte na tlačítko hello '+' tooopen hello 'New' okno.
2. V poli "Marketplace vyhledávání hello" hello zadejte "Virtuální sítě". Pokud místo toho vyberte síť -> virtuální sítě, nebude získat hello možnost toocreate klasické virtuální sítě.
3. Vyhledejte "virtuální sítě, z hello vrátil seznam a klikněte na něj tooopen hello virtuální sítě okno. 
4. V okně hello virtuální sítě vyberte 'Classic' toocreate klasické virtuální sítě. 

Pokud již máte virtuální síť, brána sítě VPN, ověřte, zda že tento hello brány je dynamický. Pokud je statická, musíte nejprve odstranit bránu VPN hello a pokračovat.

Snímky obrazovek slouží jen jako příklady. Být jisti tooreplace hello hodnoty vlastními, nebo použijte hello [příklad](#values) hodnoty.

### <a name="part-1---configure-hello-local-site"></a>Část 1: Konfigurace místního webu hello

Otevřete hello [portál Azure](https://ms.portal.azure.com) a přihlaste se pomocí účtu Azure.

1. Přejděte příliš**všechny prostředky** a vyhledejte hello **ClassicVNet** v seznamu hello.
2. Na hello **přehled** okno, v hello **připojení k síti VPN** klikněte na tlačítko hello **brány** grafické toocreate bránu.

    ![Konfigurovat bránu VPN](./media/vpn-gateway-connect-different-deployment-models-portal/gatewaygraphic.png "konfigurovat bránu VPN")
3. Na hello **nové připojení VPN** okně pro **typ připojení**, vyberte **Site-to-site**.
4. Pro **místní lokality**, klikněte na tlačítko **konfigurovat požadované nastavení**. Tím se otevře hello **místní lokality** okno.
5. Na hello **místní lokality** okně Vytvořit toohello toorefer název virtuální sítě Resource Manageru. Například 'RMVNetLocal'.
6. Pokud hello brány VPN pro hello virtuální sítě Resource Manageru již má veřejnou IP adresu, použijte hodnotu hello hello **IP adresa brány VPN** pole. Pokud jsou to tyto kroky jako cvičení, nebo nemáte ještě bránu virtuální sítě pro virtuální sítě Resource Manager, můžete nastavit až na zástupný symbol IP adresu. Ujistěte se, že IP adresa zástupný symbol hello používá platný formát. Hello veřejnou IP adresu brány virtuální sítě Resource Manager hello později, je nahradit hello zástupný symbol IP adresu.
7. Pro **klienta adresní prostor**, použijte hello hodnoty pro hello virtuální síť adresní prostory IP adres pro hello virtuální sítě Resource Manageru. Toto nastavení je použité toospecify hello adresu prostory tooroute toohello Resource Manager virtuální sítě.
8. Klikněte na tlačítko **OK** toosave hello hodnoty a vrátit toohello **nové připojení VPN** okno.

### <a name="part-2---create-hello-virtual-network-gateway"></a>Část 2 – Vytvoření brány virtuální sítě hello

1. Na hello **nové připojení VPN** okně, vyberte hello **vytvořit bránu hned** zaškrtávací políčko a klikněte na tlačítko **konfigurace volitelné brány** tooopen hello  **Konfigurace brány** okno. 

    ![Otevřete bránu konfigurace okna](./media/vpn-gateway-connect-different-deployment-models-portal/optionalgatewayconfiguration.png "okno Konfigurace otevřete brány")
2. Klikněte na tlačítko **podsíť – nakonfigurujte požadovaná nastavení** tooopen hello **přidat podsíť** okno. Hello **název** je již nakonfigurován s hodnotou hello požadované **GatewaySubnet**.
3. Hello **rozsahu adres** odkazuje toohello rozsah pro podsíť brány hello. I když můžete vytvořit podsíť brány s/29 adres rozsah (3 adresy), doporučujeme vytvořit podsíť brány, který obsahuje víc IP adres. To je dostatečná pro budoucí konfigurace, které mohou vyžadovat další dostupné IP adresy. Pokud je to možné použijte/27 nebo velikosti/28. Pokud používáte tyto kroky jako cvičení, můžete se podívat toohello [příklad](#values) hodnoty. Klikněte na tlačítko **OK** podsíť brány toocreate hello.
4. Na hello **konfigurace brány** okně **velikost** odkazuje toohello skladová položka brány. Vyberte skladová položka brány hello pro bránu sítě VPN.
5. Ověřte hello **typ směrování** je **dynamické**, pak klikněte na tlačítko **OK** tooreturn toohello **nové připojení VPN** okno.
6. Na hello **nové připojení VPN** okně klikněte na tlačítko **OK** toobegin vytváření brány VPN. Vytvoření brány VPN může trvat až toocomplete too45 minut.

### <a name="ip"></a>Část 3 - brány virtuální sítě hello kopie veřejnou IP adresu

Po vytvoření brány virtuální sítě hello, můžete zobrazit IP adresu brány hello. 

1. Přejděte tooyour klasické virtuální sítě a klikněte na **přehled**.
2. Klikněte na tlačítko **připojení k síti VPN** tooopen hello VPN připojení okno. V okně připojení VPN hello můžete zobrazit hello veřejnou IP adresu. Toto je hello veřejnou IP adresu přiřadit tooyour brány virtuální sítě. 
3. Zapište nebo zkopírujte hello IP adresu. Použijete jej v dalších krocích při práci s nastavení konfigurace brány místní sítě Resource Manager. Můžete také zobrazit stav hello připojení brány. Všimněte si hello místního síťového webu, kterou jste vytvořili je uveden jako "Připojení". Hello stav se změní po vytvoření připojení.
4. Zavřete okno hello po kopírování IP adresu brány hello.

## <a name="rmvnet"></a>2. Nakonfigurujte nastavení virtuální sítě Resource Manager hello

V této části vytvoříte hello brány virtuální sítě a hello brány místní sítě pro virtuální sítě Resource Manager. Pokud nemáte virtuální sítě Resource Manageru a běží tyto kroky jako cvičení, můžete vytvořit virtuální síť pomocí [v tomto článku](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) a hello [příklad](#values) hodnoty nastavení z výše.

Snímky obrazovek slouží jen jako příklady. Být jisti tooreplace hello hodnoty vlastními, nebo použijte hello [příklad](#values) hodnoty.

### <a name="part-1---create-a-gateway-subnet"></a>Část 1 – vytvořit podsíť brány

Před vytvořením brány virtuální sítě, je nutné nejprve podsíť brány toocreate hello. Vytvořte podsíť brány s počtem CIDR/28 nebo větší. (/ 27, / 26, atd.)

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="part-2---create-a-virtual-network-gateway"></a>Část 2 – Vytvoření brány virtuální sítě

[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

### <a name="createlng"></a>Část 3 – vytvoření brány místní sítě

Brána místní sítě Hello určuje rozsah adres hello a hello veřejné IP adresy přidružené k vaší klasické virtuální sítě a jeho brány virtuální sítě.

Pokud byste tyto kroky jako cvičení, podívejte se na toothese nastavení:

| Virtual Network | Adresní prostor | Oblast | Připojí toolocal síťovou lokalitu |Veřejná IP adresa brány|
|:--- |:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |Západní USA | RMVNetLocal (192.168.0.0/16) |Hello veřejnou IP adresu, která je přiřazena toohello ClassicVNet brány|
| RMVNet | (192.168.0.0/16) |Východ USA |ClassicVNetLocal (10.0.0.0/24) |Hello veřejnou IP adresu, která je přiřazena toohello RMVNet brány.|

[!INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="modifylng"></a>3. Upravit nastavení místní lokality virtuální sítě classic hello

V této části nahradíte hello zástupný symbol IP adresu, která jste použili při zadávání nastavení hello místní lokality, s hello IP adresu brány VPN Resource Manager. Tato část používá hello classic rutiny prostředí PowerShell (SM).

1. V hello portálu Azure přejděte toohello klasickou virtuální síť.
2. V okně hello pro vaši virtuální síť, klikněte na tlačítko **přehled**.
3. V hello **připojení k síti VPN** klikněte na název hello místní lokality v hello obrázek.

    ![Připojení VPN](./media/vpn-gateway-connect-different-deployment-models-portal/vpnconnections.png "připojení k síti VPN")
4. Na hello **připojení Site-to-site VPN** okně klikněte na název hello hello lokality.

    ![Název lokality](./media/vpn-gateway-connect-different-deployment-models-portal/sitetosite3.png "název místního webu")
5. V okně připojení hello pro místní lokality, klikněte na název hello hello místní lokality tooopen hello **místní lokality** okno.

    ![Otevřete místní site](./media/vpn-gateway-connect-different-deployment-models-portal/openlocal.png "otevřete místní lokality")
6. Na hello **místní lokality** okno, nahraďte hello **IP adresa brány VPN** hello IP adresu brány hello Resource Manager.

    ![Adresa ip brány](./media/vpn-gateway-connect-different-deployment-models-portal/gwipaddress.png "IP adresa brány")
7. Klikněte na tlačítko **OK** tooupdate hello IP adresu.

## <a name="RMtoclassic"></a>4. Vytvoření připojení tooclassic Resource Manager

V těchto kroků nakonfigurujete hello připojení z virtuální sítě Resource Manageru hello toohello klasické virtuální sítě pomocí hello portálu Azure.

1. V **všechny prostředky**, vyhledejte bránu místní sítě hello. V našem příkladu se brána místní sítě hello **ClassicVNetLocal**.
2. Klikněte na tlačítko **konfigurace** a ověřte, zda je hello IP adresu hodnotu hello brány VPN pro hello klasické virtuální sítě. Aktualizovat, v případě potřeby a pak klikněte na **Uložit**. Okno zavřít hello.
3. V **všechny prostředky**, klikněte na tlačítko brána místní sítě hello.
4. Klikněte na tlačítko **připojení** tooopen hello připojení okno.
5. Na hello **připojení** okně klikněte na tlačítko  **+**  tooadd připojení.
6. Na hello **přidat připojení** okno, název hello připojení. Například 'RMtoClassic'.
7. **Site-to-Site** je již vybrána v tomto okně.
8. Vyberte hello brány virtuální sítě, které chcete tooassociate s touto lokalitou.
9. Vytvoření **sdílený klíč**. Tento klíč se také používá v hello připojení, které vytvoříte z hello klasické virtuální sítě toohello virtuální sítě Resource Manageru. Můžete vygenerovat klíč hello nebo si ho vymyslet. V našem příkladu používáme 'abc123', ale můžete (a měli) používáte něco složitější.
10. Klikněte na tlačítko **OK** toocreate hello připojení.

##<a name="classictoRM"></a>5. Vytvoření klasického tooResource Správce připojení

V těchto kroků můžete nakonfigurovat hello připojení z hello klasické virtuální sítě toohello virtuální sítě Resource Manageru. Tyto kroky vyžadují prostředí PowerShell. Toto připojení nelze vytvořit hello portálu. Zajistěte, aby byly staženy a nainstalovány hello classic (SM) a rutiny prostředí PowerShell Resource Manager (RM).

### <a name="1-connect-tooyour-azure-account"></a>1. Připojit tooyour účet Azure

Otevřete konzolu PowerShell hello se zvýšenými oprávněními a přihlaste se tooyour účet Azure. Hello následující rutiny vás vyzve k zadání hello přihlašovací údaje pro účet Azure. Po přihlášení, se stáhnou nastavení svého účtu, aby byly k dispozici tooAzure prostředí PowerShell.

```powershell
Login-AzureRmAccount
```
   
Získání seznamu předplatné Azure, pokud máte více než jedno předplatné.

```powershell
Get-AzureRmSubscription
```

Zadejte hello předplatné, které chcete toouse. 

```powershell
Select-AzureRmSubscription -SubscriptionName "Name of subscription"
```

Přidáte váš účet Azure toouse hello classic rutiny prostředí PowerShell (SM). toodo Ano, můžete použít hello následující příkaz:

```powershell
Add-AzureAccount
```

### <a name="2-view-hello-network-configuration-file-values"></a>2. Zobrazit hodnoty souboru konfigurace sítě hello

Když vytvoříte virtuální síť v hello portálu Azure, hello úplný název, který používá Azure není zobrazená v hello portálu Azure. Například virtuální sítě, který se zobrazí toobe s názvem 'ClassicVNet' v hello portálu Azure může mít mnoho delší název v souboru konfigurace sítě hello. Hello název může vypadat podobně jako: 'Skupiny ClassicRG ClassicVNet'. V následujícím postupu si stáhnout hello sítě konfigurační soubor a zobrazení hello hodnoty.

Vytvoření adresáře v počítači a pak je exportovat hello sítě konfiguračního souboru toohello adresáře. V tomto příkladu hello sítě konfigurační soubor je exportovaný tooC:\AzureNet.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

Otevřete soubor hello s názvem textového editoru a zobrazení hello pro klasické virtuální síti. Použije názvy hello v konfiguračním souboru na hello sítě při spuštění vaše rutiny prostředí PowerShell.

- Názvy virtuální sítě jsou uvedeny jako **VirtualNetworkSite name =**
- Názvy lokalit jsou uvedeny jako **LocalNetworkSite name =**

### <a name="3-create-hello-connection"></a>3. Vytvoření připojení hello

Nastavit sdílený klíč hello a vytvoření připojení hello z hello klasické virtuální sítě toohello virtuální sítě Resource Manageru. Nelze nastavit sdílený klíč hello pomocí portálu hello. Ujistěte se, že spustíte tyto kroky při přihlašování pomocí hello classic verze hello rutiny prostředí PowerShell. Ano, použít toodo **Add-AzureAccount**. Jinak nebude možné tooset hello '-AzureVNetGatewayKey'.

- V tomto příkladu **- VNetName** je název hello hello klasické virtuální síti jako nalezena v konfiguračním souboru sítě. 
- Hello **- LocalNetworkSiteName** hello název zadaný pro hello místní lokality, jako je nalezen v konfiguračním souboru sítě.
- Hello **- SharedKey** je hodnota, která můžete vygenerovat a zadat. V tomto příkladu jsme použili *abc123*, ale můžete vygenerovat něco složitější. Důležité: co je tuto hodnotu hello, který zde určíte Hello musí být hello stejnou hodnotu, kterou jste zadali při vytváření připojení tooclassic Resource Manager.

```powershell
Set-AzureVNetGatewayKey -VNetName "Group ClassicRG ClassicVNet" `
-LocalNetworkSiteName "172B9E16_RMVNetLocal" -SharedKey abc123
```

##<a name="verify"></a>6. Zkontrolujte svá připojení

Můžete ověřit, že hello připojení pomocí portálu Azure nebo prostředí PowerShell. Při ověřování, může být nutné toowait minutu nebo dvě jako vytváření hello připojení. Pokud je připojení úspěšné, stav připojení hello změní z too'Connected "Připojení" '.

### <a name="tooverify-hello-connection-from-your-classic-vnet-tooyour-resource-manager-vnet"></a>tooverify hello připojení z klasické virtuální sítě tooyour virtuální sítě Resource Manageru

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

###<a name="tooverify-hello-connection-from-your-resource-manager-vnet-tooyour-classic-vnet"></a>tooverify hello připojení z vaší virtuální sítě Resource Manageru tooyour klasické virtuální sítě

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="faq"></a>Nejčastější dotazy týkající se propojení VNet-to-VNet

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]
