---
title: "Připojit virtuální síťové weby toomultiple pomocí brány sítě VPN a prostředí PowerShell: Classic | Microsoft Docs"
description: "Tento článek vás provede připojením více místní lokality tooa virtuální sítě pomocí brány VPN pro model nasazení classic hello."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-service-management
ms.assetid: b043df6e-f1e8-4a4d-8467-c06079e2c093
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2017
ms.author: yushwang
ms.openlocfilehash: 5404b1c55ed3453b4dbc94dfd93e47c0812025f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-site-to-site-connection-tooa-vnet-with-an-existing-vpn-gateway-connection-classic"></a>Přidat tooa připojení Site-to-Site virtuální sítě se existující připojení brány sítě VPN (klasické)

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (Classic)](vpn-gateway-multi-site.md)
>
>

Tento článek vás provede pomocí prostředí PowerShell tooadd Site-to-Site (S2S) připojení tooa VPN bránu, která má existující připojení. Tento typ připojení je často označují tooas "s více servery" konfigurace. Hello kroky v tomto článku použít toovirtual sítě vytvořené pomocí modelu nasazení classic hello (také označované jako Service Management). Tyto kroky se nevztahují konfigurace koexistujících připojení tooExpressRoute/Site-to-Site.

### <a name="deployment-models-and-methods"></a>Modely a metody nasazení

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Tuto tabulku aktualizujeme jako nové články a další nástroje je k dispozici pro tuto konfiguraci. Když je článek k dispozici, jsme odkaz přímo tooit z této tabulky.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="about-connecting"></a>O připojení

Více místními lokalitami tooa jedné virtuální sítě se můžete připojit. To je zvláště atraktivní pro vytváření hybridní cloudové řešení. Vytvoření brány virtuální sítě Azure tooyour připojení typu Multi-Site je podobné toocreating jiná připojení Site-to-Site. Ve skutečnosti můžete použít existující bránu Azure VPN, tak dlouho, dokud hello brány je dynamický (trasové).

Pokud již máte statická Brána virtuální sítě připojený tooyour, můžete změnit toodynamic typ brány hello bez nutnosti toorebuild hello virtuální sítě v pořadí tooaccommodate více lokalit. Před změnou typu hello směrování, ujistěte se, že vaše místní brána podporuje konfigurace sítě VPN založené na trasách.

![diagram Multi-Site](./media/vpn-gateway-multi-site/multisite.png "Multi-Site")

## <a name="points-tooconsider"></a>Body tooconsider

**Nebudete moct toouse hello portálu toomake změny toothis virtuální sítě.** Budete potřebovat soubor konfigurace toomake změny toohello sítě místo pomocí portálu hello. Pokud provedete změny v portálu hello, že budete přepsat nastavení odkaz na více lokalit pro tuto virtuální síť.

Musí mít možnost hello sítě konfiguračního souboru pomocí hello čas jste dokončili postup hello více lokalit. Pokud máte více lidí pracujících na konfiguraci sítě, ale budete potřebovat toomake, že všichni ví o toto omezení. To neznamená, nelze použít hello portál vůbec. Můžete ji všem ostatním, s výjimkou provedení konfigurace změny toothis konkrétní virtuální sítě.

## <a name="before-you-begin"></a>Než začnete

Před zahájením konfigurace, ověřte, zda máte hello následující:

* Kompatibilní hardware sítě VPN pro jednotlivé místní umístění. Zkontrolujte [o zařízeních VPN pro připojení k virtuální síti](vpn-gateway-about-vpn-devices.md) tooverify Pokud hello zařízení, které chcete toouse něco, který se označuje toobe kompatibilní.
* Zvenčí veřejnou IPv4 adresu IP pro každé zařízení VPN. Hello IP adresa nesmí být umístěné za adres (NAT) Toto je požadavek.
* Budete potřebovat tooinstall hello nejnovější verzi rutin prostředí Azure PowerShell hello. Ujistěte se, že instalujete hello služby správy (SM) verze v přidání toohello Resource Manager verzi. V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace.
* Někoho, kdo je znalosti v konfiguraci hardwaru sítě VPN. Budete mít toohave silné pochopení toho, jak tooconfigure zařízení VPN nebo práci s uživatelem, který nemá.
* Hello rozsahy IP adres má toouse pro vaši virtuální síť (Pokud jste již žádný nevytvořili).
* pro každou hello místních síťových webů, které budete připojovat k rozsahy Hello IP adres. Budete potřebovat toomake jistotu, že rozsahy hello IP adres pro každou hello místních síťových webů, které chcete tooconnect toodo není překrývají. V opačném hello portálu nebo hello REST API odmítnou hello Konfigurace odesílání.<br>Například pokud máte dvě místní sítě, že oba obsahují hello IP adresa rozsahu 10.2.3.0/24 a balíčků s cílovou adresou 10.2.3.3, Azure nebude vědět, který server chcete toosend hello balíček toobecause hello rozsahy adres jsou překrývající se. tooprevent směrování problémy, Azure vám neumožňuje tooupload konfiguračního souboru, který má překrývající se rozsahy.

## <a name="1-create-a-site-to-site-vpn"></a>1. Vytvoření S2S (Site-to-site) VPN
Pokud už máte sítě Site-to-Site VPN s brány dynamického směrování, skvěle! Abyste mohli pokračovat příliš[exportovat nastavení konfigurace virtuální sítě hello](#export). Pokud ne, hello následující:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Pokud již máte virtuální síť Site-to-Site, ale má statické směrování brány (zásadové):
1. Změňte vaší brány typ toodynamic směrování. Síť VPN více lokalit vyžaduje (také označované jako založené na směrování) brány dynamického směrování. Zadejte toochange bránu, budete potřebovat toofirst odstranění hello existující bránu a pak vytvořte novou. Pokyny najdete v tématu [jak toochange hello směrování typ sítě VPN pro bránu](vpn-gateway-configure-vpn-gateway-mp.md).  
2. Konfigurace nové brány a vytvořte vaše tunelového připojení sítě VPN. Pokyny najdete v tématu [konfigurovat bránu VPN v hello portálu Azure Classic](vpn-gateway-configure-vpn-gateway-mp.md). Nejprve změňte vaší brány typ toodynamic směrování.

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Pokud nemáte virtuální síť Site-to-Site:
1. Vytvoření svojí virtuální sítě Site-to-Site pomocí těchto pokynů: [vytvoření virtuální sítě pomocí připojení Site-to-Site VPN v hello portálu Azure Classic](vpn-gateway-site-to-site-create.md).  
2. Konfigurace brány dynamického směrování podle těchto pokynů: [konfigurovat bránu VPN](vpn-gateway-configure-vpn-gateway-mp.md). Být jisti tooselect **dynamické směrování** pro váš typ brány.

## <a name="export"></a>2. Konfigurační soubor exportu hello sítě
Exportujte konfiguračního souboru sítě Azure tak, že spustíte následující příkaz hello. Hello umístění hello tooexport tooa jiné umístění souboru v případě potřeby můžete změnit.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

## <a name="3-open-hello-network-configuration-file"></a>3. Otevřete hello sítě konfiguračního souboru
Otevřete hello sítě konfigurační soubor, který jste stáhli v posledním kroku hello. Pomocí editoru xml, který chcete. Hello soubor by měl vypadat podobně jako toohello následující:

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a>4. Přidání více odkazů na web
Když přidáváte nebo odebíráte lokality referenční informace, budete provedete změny konfigurace toohello ConnectionsToLocalNetwork/LocalNetworkSiteRef. Přidání odkazu na novou místní lokality aktivuje Azure toocreate nové tunelové propojení. V příkladu hello níže hello síťové konfigurace je pro připojení k jedné lokalitě. Jakmile dokončíte provádění změny, uložte soubor hello.

```
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

odkazy na další lokality tooadd (vytvořit konfiguraci s více servery), jednoduše přidejte další "LocalNetworkSiteRef" řádky, jak je znázorněno v následujícím příkladu hello:

```
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
      <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

## <a name="5-import-hello-network-configuration-file"></a>5. Soubor importu hello síťové konfigurace
Import hello sítě konfigurační soubor. Při importování tohoto souboru se změnami hello, se přidají nové tunely hello. Hello tunely použije hello dynamickou bránu, kterou jste vytvořili dříve. Můžete použít buď portálu classic hello nebo soubor hello tooimport prostředí PowerShell.

## <a name="6-download-keys"></a>6. Stáhněte si klíče
Po přidání vaší nové tunely, použijte hello prostředí PowerShell rutinu 'Get-AzureVNetGatewayKey' tooget hello protokolu IPsec/IKE předsdílených klíčů pro každé tunelové propojení.

Například:

```powershell
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"
```

Pokud dáváte přednost, můžete také použít hello *získat virtuální sítě sdílený klíč brány* REST API tooget hello předsdílených klíčů.

## <a name="7-verify-your-connections"></a>7. Zkontrolujte svá připojení
Zkontrolujte stav hello tunelového propojení více lokalit. Po stažení hello klíče pro každé tunelové propojení, budete muset tooverify připojení. Použijte 'Get-AzureVnetConnection' tooget tunelových propojení seznam virtuální sítě, jak je znázorněno v následujícím příkladu hello. VNet1 je název hello hello virtuální sítě.

```powershell
Get-AzureVnetConnection -VNetName VNET1
```

Příklad návratový:

```
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : hello connectivity state for hello local network site 'Site1' changed from Not Connected tooConnected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : hello connectivity state for hello local network site 'Site2' changed from Not Connected tooConnected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
```

## <a name="next-steps"></a>Další kroky

toolearn Další informace o branách VPN najdete v části [informace o branách VPN](vpn-gateway-about-vpngateways.md).
