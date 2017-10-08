---
title: "Připojit virtuální síť počítače tooa pomocí Point-to-Site a certifikát ověřování: portálu Azure | Microsoft Docs"
description: "Bezpečně připojte počítač tooyour Azure Virtual Network vytvořením připojení k bráně VPN Point-to-Site ověřování pomocí certifikátů. V tomto článku se vztahuje toohello modelu nasazení Resource Manager a používá hello portálu Azure."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a15ad327-e236-461f-a18e-6dbedbf74943
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/10/2017
ms.author: cherylmc
ms.openlocfilehash: 1419d6b4c160140b62d656b25bd02f6af7fd6655
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-point-to-site-connection-tooa-vnet-using-certificate-authentication-azure-portal"></a>Konfigurace tooa připojení Point-to-Site virtuální síť ověřování pomocí certifikátů: portál Azure

Tento článek ukazuje, jak toocreate virtuální síť s připojením Point-to-Site pomocí modelu nasazení Resource Manager hello hello portálu Azure. Tato konfigurace používá certifikáty tooauthenticate hello připojení klienta. Můžete také vytvořit této konfigurace pomocí nástroje pro jiné nasazení nebo model nasazení tak, že vyberete jinou možnost z hello následující seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure Portal (Classic)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Brána sítě VPN typu Point-to-Site (P2S) umožňuje vytvoření bezpečného připojení virtuální sítě tooyour z jednotlivých klientských počítačů. Připojení point-to-Site VPN jsou užitečné, když chcete, aby tooconnect tooyour virtuální síti ze vzdáleného umístění, například při jsou telefonicky z domova nebo z konference. Síť VPN P2S je také užitečné řešení toouse místo Site-to-Site VPN, pokud máte pouze několik klientů, kteří potřebují tooconnect tooa virtuální sítě. 

Používá P2S hello Secure Socket SSTP (Tunneling Protocol), což je protokol VPN založené na protokolu SSL. Připojení P2S VPN je vytvořeno spuštěním z hello klientského počítače.

![Diagram Point-to-Site](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/point-to-site-connection-diagram.png)

Připojení point-to-Site certifikát ověřování vyžadovat hello následující:

* Bránu VPN typu RouteBased.
* Hello veřejný klíč (soubor .cer) pro kořenový certifikát, který je nahraný tooAzure. Po nahrání certifikátu hello je považován za důvěryhodný certifikát a slouží k ověřování.
* Klientský certifikát, který je generována z hello kořenový certifikát a nainstalovat na každý klientský počítač, který se bude připojovat toohello virtuální sítě. Tento certifikát se používá k ověřování klienta.
* Konfigurační balíček klienta VPN. balíček konfigurace klienta VPN Hello obsahuje nezbytné informace hello hello klienta tooconnect toohello virtuální sítě. balíček Hello nakonfiguruje hello existující klienta VPN, který je nativní toohello operačního systému Windows. Každý klient, který se připojuje musí být nakonfigurovaný pomocí konfiguračního balíčku hello.

Připojení typu Point-to-Site nevyžadují zařízení VPN ani místní veřejnou IP adresu. Vytvoří se Hello připojení VPN prostřednictvím protokolu SSTP (Secure Socket Tunneling Protocol). Na straně serveru hello podporujeme SSTP verze 1.0, 1.1 a 1.2. Klient Hello, rozhodne se které toouse verze. Pro Windows 8.1 a novější se standardně používá SSTP verze 1.2.

Další informace o připojení Point-to-Site najdete v tématu hello [Point-to-Site – nejčastější dotazy](#faq) na konci hello tohoto článku.

#### <a name="example"></a>Příklady hodnot

Můžete použít následující hodnoty toocreate testovacím prostředí hello nebo najdete hodnoty toothese toobetter pochopit hello příklady v tomto článku:

* **Název virtuální sítě:** VNet1
* **Adresní prostor:** 192.168.0.0/16<br>V tomto příkladu používáme pouze jeden adresní prostor. Pro svoji virtuální síť můžete mít více adresních prostorů.
* **Název podsítě:** FrontEnd
* **Rozsah adres podsítě:** 192.168.1.0/24
* **Předplatné:** Pokud máte více než jedno předplatné, ověřte, že používáte hello správná.
* **Skupina prostředků:** TestRG
* **Umístění:** Východní USA
* **Podsíť brány:** 192.168.200.0/24<br>
* **DNS Server:** (volitelné) IP adresa serveru DNS hello chcete toouse překladu IP adres.
* **Název brány virtuální sítě:** VNet1GW
* **Typ brány:** Síť VPN
* **Typ sítě VPN:** Založená na trasách
* **Název veřejné IP adresy:** VNet1GWpip
* **Typ připojení:** Point-to-Site
* **Fond adres klienta:** 172.16.201.0/24<br>Klienti VPN, které se připojují toohello sítě VNet pomocí tohoto připojení Point-to-Site získali IP adresu z fondu adres klienta hello.

## <a name="createvnet"></a>1. Vytvoření virtuální sítě

Než začnete, ověřte, že máte předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial).

[!INCLUDE [Basic Point-to-Site VNet](../../includes/vpn-gateway-basic-p2s-vnet-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>2. Přidání podsítě brány

Před připojením tooa brány virtuální sítě, musíte nejdřív podsíť brány hello toocreate toowhich hello virtuální sítě se má tooconnect. služby brány Hello použijte hello IP adres zadaných v hello podsíť brány. Pokud je to možné, vytvořit podsíť brány pomocí blok CIDR/28 nebo /27 tooprovide dost IP adres tooaccommodate další budoucí konfigurační požadavky.

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-p2s-rm-portal-include.md)]

## <a name="dns"></a>3. Určení serveru DNS (volitelné)

Po vytvoření virtuální sítě, můžete přidat IP adresu překlad názvu DNS serveru toohandle hello. Hello DNS server pro tuto konfiguraci je volitelný, ale vyžaduje, pokud chcete, aby překlad názvů. Zadání hodnoty nevytvoří nový server DNS. Hello IP adresa serveru DNS, který zadáte musí být server DNS, který může překládat názvy hello hello prostředků, ke kterému se připojujete. V tomto příkladu jsme použili privátní IP adresy, ale je pravděpodobné, že se nejedná hello IP adresu serveru DNS. Být jisti toouse vlastní hodnoty.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>4. Vytvoření brány virtuální sítě

[!INCLUDE [create-gateway](../../includes/vpn-gateway-add-gw-p2s-rm-portal-include.md)]

## <a name="generatecert"></a>5. Generování certifikátů

Certifikáty se používají klienti Azure tooauthenticate připojení tooa virtuální sítě přes připojení VPN typu Point-to-Site. Po získání kořenového certifikátu, můžete [nahrát](#uploadfile) hello tooAzure informace o veřejném klíči. Hello kořenový certifikát se považovat za "důvěryhodné' Azure pro připojení P2S toohello virtuální síti. Můžete taky generovat klientské certifikáty z hello důvěryhodný kořenový certifikát a pak je nainstalujte na každém klientském počítači. Hello klientský certifikát je použité tooauthenticate hello klient při zahájí toohello připojení virtuální sítě. 

### <a name="getcer"></a>1. Získat soubor .cer hello pro hello kořenový certifikát

[!INCLUDE [root-certificate](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="generateclientcert"></a>2. Vygenerování klientského certifikátu

[!INCLUDE [generate-client-cert](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="addresspool"></a>6. Přidejte fond adres klienta hello

fond adres klienta Hello je rozsah privátních IP adres, které zadáte. Hello klientů, které se připojují přes síť VPN Point-to-Site získali IP adresu z tohoto rozsahu. Použijte privátní rozsah IP adres, který se nepřekrývá s hello místní umístění, které můžete připojit z nebo hello virtuální sítě, který chcete tooconnect k.

1. Po vytvoření brány virtuální sítě hello přejděte toohello **nastavení** části hello stránka brány virtuální sítě. V hello **nastavení** klikněte na tlačítko **KonfiguracePoint-to-site** tooopen hello **bodu na--konfigurace lokality** stránky.

  ![Stránka Point-to-Site](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/gatewayblade.png)
2. Na hello **bodu na--konfigurace lokality** stránky, můžete odstranit hello automaticky vyplněné rozsah a pak přidejte hello rozsah privátních IP adres, které chcete toouse. Klikněte na tlačítko **Uložit** toovalidate a uložte nastavení hello.

  ![Fond adres klienta](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="uploadfile"></a>7. Nahrání hello kořenového certifikátu veřejné certifikační dat

Po vytvoření brány hello nahrajete hello informace o veřejném klíči pro hello kořenový certifikát tooAzure. Po nahrání dat veřejný certifikát hello Azure můžete ji použít tooauthenticate klientů, které jste nainstalovali klientský certifikát generují z hello důvěryhodný kořenový certifikát. Můžete nahrát další důvěryhodné kořenové certifikáty up tooa celkem 20.

1. Certifikáty jsou přidány na hello **KonfiguracePoint-to-site** stránku hello **kořenový certifikát** části.  
2. Zkontrolujte, zda jste exportovali hello kořenový certifikát, protože kódování Base-64 souboru X.509 (.cer). Budete potřebovat tooexport hello certifikát v tomto formátu, takže hello certifikát můžete otevřít pomocí textového editoru.
3. Pomocí textového editoru, například Poznámkový blok otevřete hello certifikát. Při kopírování dat hello certifikátu, ujistěte se, že zkopírujete hello text jako jeden řádek průběžné bez návrat na začátek a odřádkování. Může být nutné toomodify zobrazení hello textového editoru too'Show Symbol/zobrazit všechny znaky toosee hello znaků CR vrátí a řádek informačních kanálů. Zkopírujte následující části jako jeden řádek průběžné pouze hello:

  ![Data certifikátu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png)
4. Vložte data certifikátu hello do hello **veřejná Data certifikátu** pole. **Název** hello certifikát a potom klikněte na **Uložit**. Můžete přidat až too20 důvěryhodné kořenové certifikáty.

  ![Nahrání certifikátu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcertupload.png)

## <a name="clientconfig"></a>8. Generování a nainstalujte balíček konfigurace klienta VPN hello

tooconnect tooa sítě VNet pomocí sítě VPN Point-to-Site, každý klient musíte nainstalovat balíček konfigurace klienta, který konfiguruje hello Nativní klient VPN s nastavením hello a soubory, které jsou nezbytné tooconnect toohello virtuální sítě. balíček konfigurace klienta VPN Hello nakonfiguruje Nativní klient VPN ve Windows hello, neinstaluje klienta VPN nové nebo jiné.

Hello stejné konfigurace klienta VPN pomocí balíčku v každém klientském počítači, můžete použít také hello verze odpovídá hello architektura pro klienta hello. Seznam hello klientské operační systémy, které jsou podporovány, naleznete v části hello [připojeníPoint-to-Site – nejčastější dotazy](#faq) na konci hello tohoto článku.

### <a name="step-1---generate-and-download-hello-client-configuration-package"></a>Krok 1 – generování a stáhněte balíček konfigurace klienta hello

1. Na hello **KonfiguracePoint-to-site** klikněte na tlačítko **klienta VPN Stáhnout** tooopen hello **klienta VPN Stáhnout** stránky. Bude trvat minutu nebo dvě pro balíček toogenerate hello.

  ![Stažení klienta VPN 1](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadvpnclient1.png)
2. Vyberte správný balíček hello pro vašeho klienta a pak klikněte na tlačítko **Stáhnout**. Uložte soubor balíčku konfigurace hello. Balíček konfigurace klienta VPN hello nainstalujete na každý klientský počítač, který se připojuje toohello virtuální sítě.

  ![Stažení klienta VPN 2](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpnclient.png)

### <a name="step-2---install-hello-client-configuration-package"></a>Krok 2 – balíček konfigurace klienta hello instalace

1. Kopírování hello konfigurační soubor místně toohello počítače, které chcete tooconnect tooyour virtuální sítě. 
2. Dvakrát klikněte na balíček hello hello .exe souboru tooinstall na klientském počítači hello. Vzhledem k tomu, že vytvoření hello konfigurační balíček není podepsaný a mohou se zobrazit upozornění. Pokud se zobrazí místní okno Windows SmartScreen, klikněte na tlačítko **více informací o** (na levé straně hello), pak **spustit i přesto** tooinstall hello balíčku.
3. Nainstalujte balíček hello hello klientského počítače. Pokud se zobrazí místní okno Windows SmartScreen, klikněte na tlačítko **více informací o** (na levé straně hello), pak **spustit i přesto** tooinstall hello balíčku.
4. V počítači klienta hello přejděte příliš**nastavení sítě** a klikněte na tlačítko **VPN**. Hello připojení VPN se zobrazuje název hello hello virtuální sítě, který se připojuje ke službě.

## <a name="installclientcert"></a>9. Instalace exportovaného klientského certifikátu

Pokud chcete připojení toocreate P2S z klientského počítače, než hello, kterou používá toogenerate hello klientské certifikáty, musíte tooinstall klientský certifikát. Při instalaci klientského certifikátu, je nutné hello heslo, které byla vytvořena, když byl exportován hello klientský certifikát. Obvykle je právě řádu dvakrát kliknete na soubor certifikátu hello a nainstalujte ho.

Zkontrolujte, zda text hello klientský certifikát byl exportován jako .pfx společně s hello celý řetěz certifikátů (což je výchozí hello). Jinak hello kořenový certifikát informace není přítomna na klientském počítači hello a hello klient nebude moct tooauthenticate správně. Další informace najdete v tématu [Instalace exportovaného klientského certifikátu](vpn-gateway-certificates-point-to-site.md#install).

## <a name="connect"></a>10. Připojit tooAzure

1. tooconnect tooyour virtuální síť, v klientském počítači hello přejděte tooVPN připojení a vyhledejte hello připojení VPN, který jste vytvořili. Je název hello stejný název jako vaše virtuální síť. Klikněte na **Připojit**. Zobrazí se zpráva se zobrazí s odkazuje toousing hello certifikátu. Klikněte na tlačítko **pokračovat** toouse se zvýšenými oprávněními.

2. Na hello **připojení** stavové stránce klikněte na tlačítko **připojit** toostart hello připojení. Pokud se zobrazí **vybrat certifikát** obrazovky, ověřte, zda je zobrazení certifikátu klienta hello hello jeden, které chcete toouse tooconnect. Pokud není, použijte hello šipku tooselect hello správný certifikát a pak klikněte na tlačítko **OK**.

  ![Klient VPN připojí tooAzure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)
3. Vaše připojení bylo vytvořeno.

  ![Vytvořené připojení](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)

#### <a name="troubleshooting-p2s-connections"></a>Řešení potíží s připojeními P2S

[!INCLUDE [verifies client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

## <a name="verify"></a>11. Ověření stavu připojení

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

## <a name="add"></a>Přidání a odebrání důvěryhodných kořenových certifikátů

Důvěryhodný kořenový certifikát můžete do Azure přidat nebo ho z Azure odebrat. Když odeberete kořenový certifikát, klienti, kteří mají vygenerovat z tento kořenový certifikát nebude možné tooauthenticate a proto nebude možné tooconnect. Pokud chcete klienta tooauthenticate a připojit, je nutné vygenerovat nový klientský certifikát z kořenového certifikátu, který je důvěryhodný (nahrané) tooAzure tooinstall.

### <a name="tooadd-a-trusted-root-certificate"></a>tooadd důvěryhodného kořenového certifikátu

Můžete přidat až too20 důvěryhodný kořenový certifikát .cer soubory tooAzure. Pokyny najdete v části hello [nahrát důvěryhodný kořenový certifikát](#uploadfile) v tomto článku.

### <a name="tooremove-a-trusted-root-certificate"></a>tooremove důvěryhodného kořenového certifikátu

1. tooremove důvěryhodný kořenový certifikát, přejděte toohello **KonfiguracePoint-to-site** stránky pro bránu virtuální sítě.
2. V hello **kořenový certifikát** části hello stránky, vyhledejte hello certifikát, který má tooremove.
3. Klikněte hello třemi tečkami další toohello certifikát a potom klikněte na "Odebrat".

## <a name="revokeclient"></a>Odvolání klientského certifikátu

Certifikáty klientů lze odvolat. Hello certifikátu, seznamu odvolaných certifikátů můžete tooselectively Odepřít připojení Point-to-Site na základě jednotlivých klientských certifikátů. To se liší od odebrání důvěryhodného kořenového certifikátu. Pokud odeberete z Azure důvěryhodného kořenového certifikátu .cer, odvolá hello přístup pro všechny klientské certifikáty generované podepsané pomocí hello odvolané kořenový certifikát. Odvolání certifikátu klienta, nikoli hello kořenový certifikát, umožňuje hello další certifikáty, které byly generovány od hello kořenový certifikát toocontinue toobe používá k ověřování.

běžnou praxí Hello je toouse hello kořenový certifikát toomanage přístup na tým nebo organizace úrovních, při použití odvolané klientské certifikáty pro řízení přístupu podrobných na jednotlivé uživatele.

### <a name="toorevoke-a-client-certificate"></a>toorevoke certifikát klienta

Přidáním seznamu odvolaných certifikátů toohello kryptografický otisk hello můžete odvolat certifikát klienta.

1. Načtěte hello kryptografický otisk certifikátu klienta. Další informace najdete v tématu [jak tooretrieve hello kryptografického otisku certifikátu](https://msdn.microsoft.com/library/ms734695.aspx).
2. Zkopírujte hello informace tooa textovém editoru a odeberte všechny mezery tak, aby se souvislý řetězec.
3. Přejděte brány virtuální sítě toohello **bodu na--konfigurace lokality** stránky. Toto je hello stejné stránce, který jste použili příliš[nahrát důvěryhodný kořenový certifikát](#uploadfile).
4. V hello **odvolané certifikáty** části, zadejte popisný název certifikátu hello (nemá certifikát hello toobe CN).
5. Zkopírujte a vložte hello kryptografický otisk řetězec toohello **kryptografický otisk** pole.
6. Kryptografický otisk Hello ověří a se automaticky přidá toohello seznamu odvolaných certifikátů. Zpráva se zobrazí na úvodní obrazovka této hello aktualizuje seznam. 
7. Po dokončení aktualizace hello certifikát už se nedá použít tooconnect. Klienti, které se pokusí použít tento certifikát tooconnect zobrazí zpráva, že tento certifikát hello již není platný.

## <a name="faq"></a>Nejčastější dotazy týkající se připojení Point-to-Site

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Další kroky
Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě. Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). toounderstand Další informace o sítě a virtuálních počítačů najdete v části [přehled sítě Azure a virtuální počítač s Linuxem](../virtual-machines/linux/azure-vm-network-overview.md).
