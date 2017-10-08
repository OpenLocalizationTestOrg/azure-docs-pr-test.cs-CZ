---
title: "Připojte počítač tooan virtuální síť Azure pomocí Point-to-Site a ověření certifikátem: prostředí PowerShell | Microsoft Docs"
description: "Bezpečně připojte virtuální síť tooyour počítače tak, že vytvoříte připojení k bráně VPN Point-to-Site ověřování pomocí certifikátů. Tento článek vztahuje toohello modelu nasazení Resource Manager a používá prostředí PowerShell."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3eddadf6-2e96-48c4-87c6-52a146faeec6
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/10/2017
ms.author: cherylmc
ms.openlocfilehash: b962e4b1946a4ae17d4eb2b920ed54437bc26b61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-point-to-site-connection-tooa-vnet-using-certificate-authentication-powershell"></a>Konfigurace tooa připojení Point-to-Site virtuální síť ověřování pomocí certifikátů: prostředí PowerShell

Tento článek ukazuje, jak toocreate virtuální síť s připojením Point-to-Site v nasazení Resource Manager hello modelu pomocí prostředí PowerShell. Tato konfigurace používá certifikáty tooauthenticate hello připojení klienta. Můžete také vytvořit této konfigurace pomocí nástroje pro jiné nasazení nebo model nasazení tak, že vyberete jinou možnost z hello následující seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure Portal (Classic)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Brána sítě VPN typu Point-to-Site (P2S) umožňuje vytvoření bezpečného připojení virtuální sítě tooyour z jednotlivých klientských počítačů. Připojení point-to-Site VPN jsou užitečné, když chcete, aby tooconnect tooyour virtuální síti ze vzdáleného umístění, například při jsou telefonicky z domova nebo z konference. Síť VPN P2S je také užitečné řešení toouse místo Site-to-Site VPN, pokud máte pouze několik klientů, kteří potřebují tooconnect tooa virtuální sítě.

Používá P2S hello Secure Socket SSTP (Tunneling Protocol), což je protokol VPN založené na protokolu SSL. Připojení P2S VPN je vytvořeno spuštěním z hello klientského počítače.

![Připojení počítače tooan virtuální síť Azure – diagram připojení Point-to-Site](./media/vpn-gateway-howto-point-to-site-rm-ps/point-to-site-diagram.png)

Připojení point-to-Site certifikát ověřování vyžadovat hello následující:

* Bránu VPN typu RouteBased.
* Hello veřejný klíč (soubor .cer) pro kořenový certifikát, který je nahraný tooAzure. Po nahrání certifikátu hello je považován za důvěryhodný certifikát a slouží k ověřování.
* Klientský certifikát, který je generována z hello kořenový certifikát a nainstalovat na každý klientský počítač, který se bude připojovat toohello virtuální sítě. Tento certifikát se používá k ověřování klienta.
* Konfigurační balíček klienta VPN. balíček konfigurace klienta VPN Hello obsahuje nezbytné informace hello hello klienta tooconnect toohello virtuální sítě. balíček Hello nakonfiguruje hello existující klienta VPN, který je nativní toohello operačního systému Windows. Každý klient, který se připojuje musí být nakonfigurovaný pomocí konfiguračního balíčku hello.

Připojení typu Point-to-Site nevyžadují zařízení VPN ani místní veřejnou IP adresu. Vytvoří se Hello připojení VPN prostřednictvím protokolu SSTP (Secure Socket Tunneling Protocol). Na straně serveru hello podporujeme SSTP verze 1.0, 1.1 a 1.2. Klient Hello, rozhodne se které toouse verze. Pro Windows 8.1 a novější se standardně používá SSTP verze 1.2. 

Další informace o připojení Point-to-Site najdete v tématu hello [Point-to-Site – nejčastější dotazy](#faq) na konci hello tohoto článku.

## <a name="before-beginning"></a>Před zahájením

* Ověřte, že máte předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial).
* Nainstalujte nejnovější verzi rutin Powershellu pro Azure Resource Manager hello hello. Další informace o instalaci rutin prostředí PowerShell najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

### <a name="example"></a>Příklady hodnot

Můžete použít hello Příklad hodnoty toocreate testovací prostředí nebo najdete hodnoty toothese toobetter pochopit hello příklady v tomto článku. Nastaví proměnné hello v části [1](#declare) hello článku. Můžete buď použít jako návod hello kroky a použít hello hodnoty bez jejich změny nebo změnit je tooreflect prostředí. 

* **Název: VNet1**
* **Adresní prostor: 192.168.0.0/16** a **10.254.0.0/16**<br>V tomto příkladu používáme více než jeden tooillustrate místo adres, který tato konfigurace funguje s více adresní prostory. Více adresních prostorů pro ni ale není potřeba.
* **Název podsítě: FrontEnd**
  * **Rozsah adres podsítě: 192.168.1.0/24**
* **Název podsítě: BackEnd**
  * **Rozsah adres podsítě: 10.254.1.0/24**
* **Název podsítě: GatewaySubnet**<br>název podsítě Hello *GatewaySubnet* je povinné pro toowork brány VPN hello.
  * **Rozsah adres podsítě brány: 192.168.200.0/24** 
* **Fond adres klienta VPN: 172.16.201.0/24**<br>Klienti VPN, které se připojují toohello sítě VNet pomocí tohoto připojení Point-to-Site získali IP adresu z hello fond adres klienta VPN.
* **Předplatné:** Pokud máte více než jedno předplatné, ověřte, že používáte hello správná.
* **Skupina prostředků: TestRG**
* **Umístění: Východní USA**
* **Serveru DNS: IP adresa** chcete toouse pro překlad názvu serveru DNS hello.
* **Název brány: Vnet1GW**
* **Název veřejné IP adresy: VNet1GWPIP**
* **Typ sítě VPN: RouteBased** 

## <a name="declare"></a>1. Přihlášení a nastavení proměnných

V této části se přihlaste a deklarovat hello hodnoty používané pro tuto konfiguraci. Hello deklarovat hodnoty se používají v hello ukázkové skripty. Změňte hodnoty tooreflect hello svého vlastního prostředí. Nebo můžete použít hello deklarovaný hodnoty a projít hello kroky jako cvičení.

1. Otevřete konzolu prostředí PowerShell se zvýšenými oprávněními a přihlaste se tooyour účet Azure. Tato rutina vás vyzve k zadání přihlašovacích údajů hello. Po přihlášení stahování nastavení svého účtu, aby byly k dispozici tooAzure prostředí PowerShell.

  ```powershell
  Login-AzureRmAccount
  ```
2. Načtěte seznam předplatných Azure.

  ```powershell
  Get-AzureRmSubscription
  ```
3. Zadejte hello předplatné, které chcete toouse.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
4. Deklarujte hello proměnné, které chcete toouse. Použijte hello následující ukázka, nahraďte hello hodnoty pro vlastní, pokud je to nezbytné.

  ```powershell
  $VNetName  = "VNet1"
  $FESubName = "FrontEnd"
  $BESubName = "Backend"
  $GWSubName = "GatewaySubnet"
  $VNetPrefix1 = "192.168.0.0/16"
  $VNetPrefix2 = "10.254.0.0/16"
  $FESubPrefix = "192.168.1.0/24"
  $BESubPrefix = "10.254.1.0/24"
  $GWSubPrefix = "192.168.200.0/26"
  $VPNClientAddressPool = "172.16.201.0/24"
  $RG = "TestRG"
  $Location = "East US"
  $DNS = "10.1.1.3"
  $GWName = "VNet1GW"
  $GWIPName = "VNet1GWPIP"
  $GWIPconfName = "gwipconf"
  ```

## <a name="ConfigureVNet"></a>2. Konfigurace virtuální sítě

1. Vytvořte skupinu prostředků.

  ```powershell
  New-AzureRmResourceGroup -Name $RG -Location $Location
  ```
2. Vytvoření konfigurací podsítě pro virtuální síť hello jejich názvů hello *front-endu*, *back-end*, a *GatewaySubnet*. Tyto předpony musí být součástí hello adresní prostor sítě VNet, která je deklarován.

  ```powershell
  $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
  $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
  $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
  ```
3. Vytvoření virtuální sítě hello.

  V tomto příkladu hello DNS server je volitelné. Zadání hodnoty nevytvoří nový server DNS. Hello IP adresa serveru DNS, který zadáte musí být server DNS, který může překládat názvy hello hello prostředků, ke kterému se připojujete. V tomto příkladu jsme použili privátní IP adresy, ale je pravděpodobné, že se nejedná hello IP adresu serveru DNS. Být jisti toouse vlastní hodnoty.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS
  ```
4. Zadejte proměnné hello hello virtuální sítě, kterou jste vytvořili.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  ```
5. Brána VPN musí mít veřejnou IP adresu. Nejprve požádali o prostředek hello IP adresy a pak odkazovat tooit při vytváření brány virtuální sítě. Hello je přiřazená IP adresa dynamicky toohello prostředků při vytváření brány VPN hello. Služba VPN Gateway aktuálně podporuje pouze *dynamické* přidělení veřejné IP adresy. Nemůžete si vyžádat statické přiřazení IP adresy. Však neznamená, že hello IP adresa změní po byl přiřazen tooyour brány VPN. Hello jenom jednou hello změny veřejné IP adresy je při hello brány je odstraní a znovu vytvoří. V případě změny velikosti, resetování nebo jiné operace údržby/upgradu vaší brány VPN se nezmění.

  Požádejte o dynamicky přidělovanou veřejnou IP adresu.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```

## <a name="creategateway"></a>3. Vytvoření brány VPN hello

Nakonfigurujte a vytvořte hello brány virtuální sítě pro virtuální síť.

* Hello *- GatewayType* musí být **Vpn** a hello *- VpnType* musí být **RouteBased**.
* Brána sítě VPN může trvat až minut toocomplete too45, v závislosti na hello [skladová položka brány](vpn-gateway-about-vpn-gateway-settings.md) vyberete.

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
-Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
-VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 `
```

## <a name="addresspool"></a>4. Přidejte fond adres klienta VPN hello

Po dokončení vytvoření brány VPN hello můžete přidat fond adres klienta VPN hello. Hello fond adres klienta VPN je hello rozsah, ze kterého klienti VPN hello přijímat IP adresu pro připojení. Použijte privátní rozsah IP adres, který se nepřekrývá hello místní umístění, které můžete připojit z nebo s hello virtuální sítě, který chcete tooconnect k. V tomto příkladu hello fond adres klienta VPN je deklarován jako [proměnná](#declare) v kroku 1.

```powershell
$Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
```

## <a name="Certificates"></a>5. Generování certifikátů

Certifikáty se používají klienti VPN Azure tooauthenticate pro sítě Point-to-Site VPN. Můžete nahrát informace veřejného klíče hello z hello kořenový certifikát tooAzure. Hello veřejný klíč je pak považován za 'důvěryhodné'. Klientské certifikáty musí být vygenerovat z hello důvěryhodného kořenového certifikátu a následně je nainstalován na každém klientském počítači v úložišti certifikátů certifikáty – aktuální uživatel nebo osobní hello. certifikát Hello je použité tooauthenticate hello klienta při zahájí toohello připojení virtuální sítě. 

Pokud používáte certifikáty podepsané svým držitelem, musí se vytvořit pomocí konkrétních parametrů. Můžete vytvořit certifikát podepsaný svým držitelem pomocí hello pokyny pro [prostředí PowerShell a Windows 10](vpn-gateway-certificates-point-to-site.md), nebo pokud nemáte Windows 10, můžete použít [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Je důležité, postupujte podle kroků hello v hello pokyny, při generování vlastních kořenových certifikátů a klientské certifikáty. Jinak hodnota hello certifikátů, který generovat nebudou kompatibilní s připojení P2S a taky bude docházet k chybě připojení.

### <a name="cer"></a>1. Získat soubor .cer hello pro hello kořenový certifikát

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]


### <a name="generate"></a>2. Vygenerování klientského certifikátu

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="upload"></a>6. Nahrát hello kořenový certifikát informací veřejného klíče

Ověřte, že se dokončilo vytváření brány VPN. Po jeho dokončení můžete nahrát hello souboru .cer (obsahující informace veřejného klíče hello) pro tooAzure důvěryhodný kořenový certifikát. Po nahrání souboru a.cer Azure můžete ji použít tooauthenticate klientů, které jste nainstalovali klientský certifikát generují z hello důvěryhodný kořenový certifikát. V případě potřeby můžete nahrávat další důvěryhodný kořenový certifikát soubory – až tooa celkem 20 – později.

1. Deklarujte hello proměnná pro název certifikátu, nahraďte hello hodnoty vlastními.

  ```powershell
  $P2SRootCertName = "P2SRootCert.cer"
  ```
2. Nahraďte cesta k souboru hello vlastní a pak spusťte rutiny hello.

  ```powershell
  $filePathForCert = "C:\cert\P2SRootCert.cer"
  $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
  $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
  $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
  ```
3. Nahrajte tooAzure hello informace o veřejném klíči. Po odeslání informací o certifikátu hello Azure zvažuje tento toobe důvěryhodný kořenový certifikát.

   ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64
  ```

## <a name="clientconfig"></a>7. Stáhněte si balíček konfigurace klienta VPN hello

tooconnect tooa sítě VNet pomocí sítě VPN Point-to-Site, každý klient musíte nainstalovat balíček konfigurace klienta, který konfiguruje hello Nativní klient VPN s nastavením hello a soubory, které jsou nezbytné tooconnect toohello virtuální sítě. balíček konfigurace klienta VPN Hello nakonfiguruje Nativní klient VPN ve Windows hello, neinstaluje klienta VPN nové nebo jiné. 

Hello stejné konfigurace klienta VPN pomocí balíčku v každém klientském počítači, můžete použít také hello verze odpovídá hello architektura pro klienta hello. Seznam hello klientské operační systémy, které jsou podporovány, naleznete v části hello [připojeníPoint-to-Site – nejčastější dotazy](#faq) na konci hello tohoto článku.

1. Po vytvoření brány hello můžete vygenerovat a stáhněte balíček konfigurace klienta hello. Tento příklad stáhne hello balíček pro 64bitové klienty. Pokud chcete toodownload hello 32bitová verze klienta, nahraďte 'Amd64' 'x86'. Můžete také stáhnout hello klienta VPN pomocí hello portálu Azure.

  ```powershell
  Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
  -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64
  ```
2. Zkopírujte a vložte hello odkaz, který je vrácen tooa webového prohlížeče toodownload hello balíčku, dbejte na to tooremove hello uvozovky, které obaluje hello odkaz. 
3. Stáhněte a nainstalujte balíček hello na klientském počítači hello. Pokud se zobrazí automaticky otevírané okno filtru SmartScreen, klikněte na **Další informace** a potom na **Přesto spustit**. Můžete také uložit balíček tooinstall hello na další klientské počítače.
4. V počítači klienta hello přejděte příliš**nastavení sítě** a klikněte na tlačítko **VPN**. Hello připojení VPN se zobrazuje název hello hello virtuální sítě, který se připojuje ke službě.

## <a name="clientcertificate"></a>8. Instalace exportovaného klientského certifikátu

Pokud chcete připojení toocreate P2S z klientského počítače, než hello, kterou používá toogenerate hello klientské certifikáty, musíte tooinstall klientský certifikát. Při instalaci klientského certifikátu, je nutné hello heslo, které byla vytvořena, když byl exportován hello klientský certifikát. Obvykle je právě řádu dvakrát kliknete na soubor certifikátu hello a nainstalujte ho.

Zkontrolujte, zda text hello klientský certifikát byl exportován jako .pfx společně s hello celý řetěz certifikátů (což je výchozí hello). Jinak hello kořenový certifikát informace není přítomna na klientském počítači hello a hello klient nebude moct tooauthenticate správně. Další informace najdete v tématu [Instalace exportovaného klientského certifikátu](vpn-gateway-certificates-point-to-site.md#install). 

## <a name="connect"></a>9. Připojit tooAzure

1. tooconnect tooyour virtuální síť, v klientském počítači hello přejděte tooVPN připojení a vyhledejte hello připojení VPN, který jste vytvořili. Je název hello stejný název jako vaše virtuální síť. Klikněte na **Připojit**. Zobrazí se zpráva se zobrazí s odkazuje toousing hello certifikátu. Klikněte na tlačítko **pokračovat** toouse se zvýšenými oprávněními. 
2. Na hello **připojení** stavové stránce klikněte na tlačítko **připojit** toostart hello připojení. Pokud se zobrazí **vybrat certifikát** obrazovky, ověřte, zda je zobrazení certifikátu klienta hello hello jeden, které chcete toouse tooconnect. Pokud není, použijte hello šipku tooselect hello správný certifikát a pak klikněte na tlačítko **OK**.

  ![Klient VPN připojí tooAzure](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png)
3. Vaše připojení bylo vytvořeno.

  ![Vytvořené připojení](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png)

#### <a name="troubleshooting-p2s-connections"></a>Řešení potíží s připojeními P2S

[!INCLUDE [client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

## <a name="verify"></a>10. Ověření stavu připojení

1. tooverify, že je připojení k síti VPN aktivní, otevřete příkazový řádek se zvýšenými oprávněními a spusťte *ipconfig/all*.
2. Zobrazení výsledků hello. Všimněte si, že hello IP adresu, kterou jste obdrželi je jednou z adres hello v rámci hello Point-to-Site fond adres klienta v síti VPN, který jste zadali v konfiguraci. výsledky Hello jsou podobné toothis příklad:

  ```
  PPP adapter VNet1:
      Connection-specific DNS Suffix .:
      Description.....................: VNet1
      Physical Address................:
      DHCP Enabled....................: No
      Autoconfiguration Enabled.......: Yes
      IPv4 Address....................: 172.16.201.3(Preferred)
      Subnet Mask.....................: 255.255.255.255
      Default Gateway.................:
      NetBIOS over Tcpip..............: Enabled
  ```

## <a name="connectVM"></a>Připojit tooa virtuálního počítače

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="addremovecert"></a>Přidání nebo odebrání kořenového certifikátu

Důvěryhodný kořenový certifikát můžete do Azure přidat nebo ho z Azure odebrat. Když odeberete kořenový certifikát, klienti, kteří mají certifikát generují z hello kořenový certifikát nemůže ověřit, nebude možné tooconnect. Pokud chcete klienta tooauthenticate a připojit, je nutné vygenerovat nový klientský certifikát z kořenového certifikátu, který je důvěryhodný (nahrané) tooAzure tooinstall.

### <a name="addtrustedroot"></a>tooadd důvěryhodného kořenového certifikátu

Můžete přidat až too20 kořenový certifikát .cer soubory tooAzure. Hello následující kroky nápovědy přidat kořenový certifikát:

#### <a name="certmethod1"></a>Metoda 1

Toto je hello nejúčinnější metoda tooupload kořenový certifikát.

1. Příprava tooupload soubor .cer hello:

  ```powershell
  $filePathForCert = "C:\cert\P2SRootCert3.cer"
  $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
  $CertBase64_3 = [system.convert]::ToBase64String($cert.RawData)
  $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64_3
  ```
2. Nahrajte soubor hello. Najednou můžete nahrát jenom jeden soubor.

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64_3
  ```

3. tooverify tento soubor certifikátu hello odeslat:

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

#### <a name="certmethod2"></a>Metoda 2

Tato metoda je nemá další kroky než metoda 1, ale má hello stejného výsledku. Je zahrnut v případě, že potřebujete data certifikátu tooview hello.

1. Vytvoření a přípravě hello nového kořenového certifikátu tooadd tooAzure. Export hello veřejného klíče, protože kódování Base-64 X.509 (. CER) a otevřete jej pomocí textového editoru. Zkopírujte hodnoty hello, jak je znázorněno v hello následující ukázka:

  ![certifikát](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png)

  > [!NOTE]
  > Při kopírování dat hello certifikátu, ujistěte se, že zkopírujete hello text jako jeden řádek průběžné bez návrat na začátek a odřádkování. Může být nutné toomodify zobrazení hello textového editoru too'Show Symbol/zobrazit všechny znaky toosee hello znaků CR vrátí a řádek informačních kanálů.
  >
  >

2. Zadejte název certifikátu hello a informace o klíči jako proměnnou. Informace o hello nahraďte vlastním, jak je uvedené v hello následující příklad:

  ```powershell
  $P2SRootCertName2 = "ARMP2SRootCert2.cer"
  $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
  ```
3. Přidáte nový certifikát kořenové hello. Nelze přidat více certifikátů současně.

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2
  ```
4. Můžete ověřit, že tento nový certifikát hello pomocí hello následující ukázka správně přidány:

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

### <a name="removerootcert"></a>tooremove kořenového certifikátu

1. Deklarujte proměnné hello.

  ```powershell
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  $P2SRootCertName2 = "ARMP2SRootCert2.cer"
  $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
  ```
2. Odeberte certifikát hello.

  ```powershell
  Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
  ```
3. Hello použijte následující příklad tooverify, který hello certifikátu byla úspěšně odebrána.

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

## <a name="revoke"></a>Odvolání klientského certifikátu

Certifikáty klientů lze odvolat. Hello certifikátu, seznamu odvolaných certifikátů můžete tooselectively Odepřít připojení Point-to-Site na základě jednotlivých klientských certifikátů. To se liší od odebrání důvěryhodného kořenového certifikátu. Pokud odeberete z Azure důvěryhodného kořenového certifikátu .cer, odvolá hello přístup pro všechny klientské certifikáty generované podepsané pomocí hello odvolané kořenový certifikát. Odvolání certifikátu klienta, nikoli hello kořenový certifikát, umožňuje hello další certifikáty, které byly generovány od hello kořenový certifikát toocontinue toobe používá k ověřování.

běžnou praxí Hello je toouse hello kořenový certifikát toomanage přístup na tým nebo organizace úrovních, při použití odvolané klientské certifikáty pro řízení přístupu podrobných na jednotlivé uživatele.

### <a name="revokeclientcert"></a>toorevoke certifikát klienta

1. Načtěte hello kryptografický otisk certifikátu klienta. Další informace najdete v tématu [jak tooretrieve hello kryptografického otisku certifikátu](https://msdn.microsoft.com/library/ms734695.aspx).
2. Zkopírujte hello informace tooa textovém editoru a odeberte všechny mezery tak, aby se souvislý řetězec. Tento řetězec je deklarován jako proměnné v dalším kroku hello.
3. Deklarujte proměnné hello. Ujistěte se, že kryptografický otisk toodeclare hello, který jste získali v předchozím kroku hello.

  ```powershell
  $RevokedClientCert1 = "NameofCertificate"
  $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  ```
4. Přidejte hello kryptografický otisk toohello seznam odvolaných certifikátů. Zobrazí "Succeeded", pokud byl přidán hello kryptografický otisk.

  ```powershell
  Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
  -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG `
  -Thumbprint $RevokedThumbprint1
  ```
5. Ověřte, že kryptografický otisk tohoto hello byl přidán toohello seznam odvolaných certifikátů.

  ```powershell
  Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
  ```
6. Po přidání hello kryptografický otisk certifikátu hello se už nedá použité tooconnect. Klienti, které se pokusí použít tento certifikát tooconnect zobrazí zpráva, že tento certifikát hello již není platný.

### <a name="reinstateclientcert"></a>tooreinstate certifikát klienta

Odebráním hello kryptografický otisk z hello seznam odvolané klientské certifikáty lze obnovit certifikát klienta.

1. Deklarujte proměnné hello. Ujistěte se, že deklarujete hello správné kryptografický otisk certifikátu hello, které chcete tooreinstate.

  ```powershell
  $RevokedClientCert1 = "NameofCertificate"
  $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  ```
2. Kryptografický otisk certifikátu hello odeberte ze seznamu odvolaných certifikátů hello.

  ```powershell
  Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
  -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
  ```
3. Zkontrolujte, pokud kryptografický otisk hello se odebral ze seznamu odvolaných hello.

  ```powershell
  Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
  ```

## <a name="faq"></a>Nejčastější dotazy týkající se připojení Point-to-Site

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Další kroky
Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě. Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). toounderstand Další informace o sítě a virtuálních počítačů najdete v části [přehled sítě Azure a virtuální počítač s Linuxem](../virtual-machines/linux/azure-vm-network-overview.md).
