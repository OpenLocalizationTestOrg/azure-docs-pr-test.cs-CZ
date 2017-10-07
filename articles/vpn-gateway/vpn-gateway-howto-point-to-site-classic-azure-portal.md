---
title: "Připojit virtuální síť počítače tooa pomocí Point-to-Site a certifikát ověřování: portál Azure classic | Microsoft Docs"
description: "Zabezpečené připojení tooyour classic Azure Virtual Network vytvořením připojení k bráně VPN Point-to-Site pomocí hello portálu Azure."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 65e14579-86cf-4d29-a6ac-547ccbd743bd
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: cherylmc
ms.openlocfilehash: 9b53ba43ee4dfb61defeec458905fb1f1b18c3a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-point-to-site-connection-tooa-vnet-using-certificate-authentication-classic-azure-portal"></a>Konfigurace tooa připojení Point-to-Site virtuální sítě pomocí ověřování pomocí certifikátu (klasické): portál Azure

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Tento článek ukazuje, jak toocreate virtuální síť s připojením Point-to-Site pomocí modelu nasazení classic hello hello portálu Azure. Tato konfigurace používá certifikáty tooauthenticate hello připojení klienta. Můžete také vytvořit této konfigurace pomocí nástroje pro jiné nasazení nebo model nasazení tak, že vyberete jinou možnost z hello následující seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure Portal (Classic)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>

Brána sítě VPN typu Point-to-Site (P2S) umožňuje vytvoření bezpečného připojení virtuální sítě tooyour z jednotlivých klientských počítačů. Připojení point-to-Site VPN jsou užitečné, když chcete, aby tooconnect tooyour virtuální síti ze vzdáleného umístění, například při jsou telefonicky z domova nebo z konference. Síť VPN P2S je také užitečné řešení toouse místo Site-to-Site VPN, pokud máte pouze několik klientů, kteří potřebují tooconnect tooa virtuální sítě. 

Používá P2S hello Secure Socket SSTP (Tunneling Protocol), což je protokol VPN založené na protokolu SSL. Připojení P2S VPN je vytvořeno spuštěním z hello klientského počítače.


![Diagram Point-to-Site](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/point-to-site-connection-diagram.png)


Připojení point-to-Site certifikát ověřování vyžadovat hello následující:

* Dynamickou bránu VPN.
* Hello veřejný klíč (soubor .cer) pro kořenový certifikát, který je nahraný tooAzure. Ten se považuje za důvěryhodný certifikát a používá se k ověřování.
* Klientský certifikát generují z hello kořenový certifikát a nainstalovat na každý klientský počítač, který se připojí. Tento certifikát se používá k ověřování klienta.
* Na každém klientském počítači, který se bude připojovat, musí být vygenerován a nainstalován balíček pro konfiguraci klienta VPN. balíček konfigurace klienta Hello nakonfiguruje hello nativního klienta VPN, který již je na hello operační systém s hello nezbytné informace tooconnect toohello virtuální sítě.

Připojení typu Point-to-Site nevyžadují zařízení VPN ani místní veřejnou IP adresu. Vytvoří se Hello připojení VPN prostřednictvím protokolu SSTP (Secure Socket Tunneling Protocol). Na straně serveru hello podporujeme SSTP verze 1.0, 1.1 a 1.2. Klient Hello, rozhodne se které toouse verze. Pro Windows 8.1 a novější se standardně používá SSTP verze 1.2. 

Další informace o připojení Point-to-Site najdete v tématu hello [Point-to-Site – nejčastější dotazy](#faq) na konci hello tohoto článku.

### <a name="example-settings"></a>Příklad nastavení

Můžete použít následující hodnoty toocreate testovacím prostředí hello nebo najdete hodnoty toothese toobetter pochopit hello příklady v tomto článku:

* **Název: VNet1**
* **Adresní prostor: 192.168.0.0/16**<br>V tomto příkladu používáme pouze jeden adresní prostor. Pro svoji virtuální síť můžete mít více adresních prostorů.
* **Název podsítě: FrontEnd**
* **Rozsah adres podsítě: 192.168.1.0/24**
* **Předplatné:** Pokud máte více než jedno předplatné, ověřte, že používáte hello správná.
* **Skupina prostředků: TestRG**
* **Umístění: Východní USA**
* **Typ připojení: Point-to-Site**
* **Klientský adresní prostor: 172.16.201.0/24**. Klienti VPN, které se připojují toohello sítě VNet pomocí tohoto připojení Point-to-Site přijímat IP adresu z hello zadaný fond.
* **Podsíť brány: 192.168.200.0/24**. podsíť brány Hello musíte použít název hello "GatewaySubnet".
* **Velikost:** vyberte hello brány, které chcete toouse SKU.
* **Typ směrování: Dynamické**

## <a name="vnetvpn"></a>1. Vytvoření virtuální sítě a brány VPN

Než začnete, ověřte, že máte předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial).

### <a name="createvnet"></a>Část 1: Vytvoření virtuální sítě

Pokud ještě nemáte virtuální síť, vytvořte si ji. Snímky obrazovek slouží jen jako příklady. Být jisti tooreplace hello hodnoty vlastními. hello toocreate virtuální sítě pomocí portálu Azure, hello použijte následující kroky:

1. V prohlížeči přejděte toohello [portál Azure](http://portal.azure.com) a v případě potřeby, přihlaste se pomocí účtu Azure.
2. Klikněte na možnost **Nové**. V hello **vyhledávání hello marketplace** pole, zadejte "Virtuální sítě". Vyhledejte **virtuální sítě** z hello vrátil seznamu a klikněte na tlačítko tooopen hello **virtuální sítě** stránky.

  ![Stránka pro vyhledání virtuální sítě](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png)
3. Téměř hello dolní části stránky hello virtuální sítě z hello **vybrat model nasazení** vyberte **Classic**a potom klikněte na **vytvořit**.

  ![Výběr modelu nasazení](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png)
4. Na hello **vytvořit virtuální síť** nakonfigurujte nastavení sítě VNet hello. Na této stránce přidáte první adresní prostor a jeden rozsah adres podsítě. Po dokončení vytváření hello virtuální síť, můžete přejít zpět a přidat další podsítě a adresní prostory.

  ![Stránka pro vytvoření virtuální sítě](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png)
5. Ověřte, že hello **předplatné** hello je správná. Odběry můžete změnit pomocí rozevíracího seznamu hello.
6. Klikněte na **Skupina prostředků** a vyberte existující skupinu nebo zadáním názvu nové skupiny prostředků vytvořte novou skupinu prostředků. Pokud vytváříte novou skupinu prostředků, skupinu prostředků hello název podle tooyour plánované hodnoty konfigurace. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Potom vyberte hello **umístění** nastavení pro virtuální síť. umístění Hello Určuje, kde se bude nacházet hello prostředky, abyste nasadili toothis virtuální sítě.
8. Vyberte **Pin toodashboard** Jestliže chcete mít toofind toobe virtuální síť snadno na řídicím panelu hello a pak klikněte na tlačítko **vytvořit**.

  ![Toodashboard PIN kód](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png)
9. Po kliknutí na vytvořit, se zobrazí na dlaždici na řídicím panelu se projeví hello průběh vaší virtuální sítě. změny dlaždice Hello jako hello se vytváří virtuální sítě.

  ![dlaždice Vytváří se virtuální síť](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png)
10. Po vytvoření virtuální sítě uvidíte **vytvořen** uvedené v části **stav** na stránce sítí hello hello portál Azure classic.
11. Přidejte server DNS (volitelné). Po vytvoření virtuální sítě, můžete přidat hello IP adresu serveru DNS pro rozlišení názvu. Hello IP adresa serveru DNS, který zadáte, by měla být hello adresu serveru DNS, která může vyřešit hello názvy pro hello prostředky ve vaší virtuální síti.<br>tooadd DNS server, otevřete hello nastavení pro vaši virtuální síť, klikněte na servery DNS a přidejte hello IP adresa serveru DNS hello, které chcete toouse.

### <a name="gateway"></a>Část 2: Vytvoření podsítě brány a brány dynamického směrování

V tomto kroku vytvoříte podsíť brány a bránu dynamického směrování. V hello portál Azure pro model nasazení classic hello, vytváření hello podsíť brány a brány hello lze provést prostřednictvím hello stejný konfigurační stránky.

1. Hello portálu přejděte toohello virtuální sítě, pro které chcete toocreate bránu.
2. Na stránce hello vaší virtuální sítě na hello **přehled** klikněte na stránce, v části připojení VPN hello **brány**.

  ![Klikněte na tlačítko toocreate brány](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png)
3. Na hello **nové připojení VPN** vyberte **Point-to-site**.

  ![Připojení typu Point-to-Site](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png)
4. Pro **klienta adresní prostor**, přidat rozsah IP adres hello. Toto je hello rozsah, ze kterého klienti VPN hello přijímat IP adresu pro připojení. Použijte privátní rozsah IP adres, který se nepřekrývá hello místní umístění, ke kterému se připojíte z nebo s hello virtuální sítě, který chcete tooconnect k. Můžete odstranit hello automaticky vyplněné rozsah a pak přidejte hello rozsah privátních IP adres, které chcete toouse.

  ![Klientský adresní prostor](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png)
5. Vyberte hello **vytvořit bránu hned** zaškrtávací políčko.

  ![Zaškrtávací políčko Vytvořit bránu hned](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png)
6. Klikněte na tlačítko **konfigurace volitelné brány** tooopen hello **konfigurace brány** stránky.

  ![Kliknutí na tlačítko Volitelná konfigurace brány](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png)
7. Klikněte na tlačítko **podsíť nakonfigurujte požadované nastavení** tooadd hello **podsíť brány**. I když je možné toocreate podsíť brány jako malé/29, doporučujeme vytvořit větší podsíť, která zahrnuje víc adres výběrem minimálně/28 nebo /27. To vám umožní dostatek adresy tooaccommodate možné další konfigurace, které můžete ve hello budoucí. Při práci s podsítí brány, vyhněte se přidružení podsíť brány toohello (NSG) skupiny zabezpečení sítě. Přidružení podsíť toothis skupiny zabezpečení sítě může způsobit vaše toostop brány VPN fungovat podle očekávání.

  ![Přidání podsítě brány](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png)
8. Vyberte hello brány **velikost**. velikost Hello je skladová položka brány hello pro bránu virtuální sítě. V portálu hello hello výchozí SKU je **základní**. Další informace o SKU brány najdete v tématu [Informace o nastavení VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

  ![Velikost brány](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)
9. Vyberte hello **typ směrování** pro bránu. Konfigurace P2S vyžadují **dynamický** typ směrování. Až dokončíte konfiguraci této stránky, klikněte na **OK**.

  ![Konfigurace typu směrování](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png)
10. Na hello **nové připojení VPN** klikněte na tlačítko **OK** dole hello toobegin stránku hello vytvoření brány virtuální sítě. Brána sítě VPN může trvat až minut toocomplete too45, v závislosti na hello skladová položka brány, kterou jste vybrali.

## <a name="generatecerts"></a>2. Vytvoření certifikátů

Certifikáty se používají klienti VPN Azure tooauthenticate pro sítě Point-to-Site VPN. Můžete nahrát informace veřejného klíče hello z hello kořenový certifikát tooAzure. Hello veřejný klíč je pak považován za 'důvěryhodné'. Klientské certifikáty musí být vygenerovat z hello důvěryhodného kořenového certifikátu a následně je nainstalován na každém klientském počítači v úložišti certifikátů certifikáty – aktuální uživatel nebo osobní hello. certifikát Hello je použité tooauthenticate hello klienta při zahájí toohello připojení virtuální sítě. 

Pokud používáte certifikáty podepsané svým držitelem, musí se vytvořit pomocí konkrétních parametrů. Můžete vytvořit certifikát podepsaný svým držitelem pomocí hello pokyny pro [prostředí PowerShell a Windows 10](vpn-gateway-certificates-point-to-site.md), nebo [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Je důležité, postupujte podle kroků hello v těchto pokynech, při práci s certifikáty podepsané svým držitelem kořenové a generovat klientské certifikáty z hello certifikát podepsaný svým držitelem kořenové. Jinak hello certifikáty, které vytvoříte, nebudou kompatibilní s připojení P2S a zobrazí se chyba připojení.

### <a name="cer"></a>Část 1: Získání hello veřejného klíče (.cer) pro hello kořenový certifikát

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="genclientcert"></a>Část 2: Vygenerování klientského certifikátu

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="upload"></a>3. Nahrát soubor .cer kořenového certifikátu hello

Po vytvoření brány hello můžete nahrát hello souboru .cer (obsahující informace veřejného klíče hello) pro tooAzure důvěryhodný kořenový certifikát. Hello privátní klíč pro hello kořenový certifikát tooAzure nenahrávejte. Po nahrání souboru a.cer Azure můžete ji použít tooauthenticate klientů, které jste nainstalovali klientský certifikát generují z hello důvěryhodný kořenový certifikát. V případě potřeby můžete nahrávat další důvěryhodný kořenový certifikát soubory – až tooa celkem 20 – později.  

1. Na hello **připojení k síti VPN** části hello stránky pro vaši virtuální síť, klikněte na tlačítko hello **klienti** hello grafické tooopen **Point-to-site VPN připojení** stránky.

  ![Klienti](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. Na hello **připojeníPoint-to-site** klikněte na tlačítko **spravovat certifikáty** tooopen hello **certifikáty** stránky.<br>

  ![Stránka certifikátů](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. Na hello **certifikáty** klikněte na tlačítko **nahrát** tooopen hello **nahrání certifikátu** stránky.<br>

    ![Stránka pro nahrání certifikátů](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png)<br>
4. Klikněte na tlačítko hello grafické toobrowse složky pro soubor .cer hello. Vyberte soubor hello a pak klikněte na **OK**. Aktualizace hello stránky toosee hello nahrát certifikát na hello **certifikáty** stránky.

  ![Nahrání certifikátu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png)<br>

## <a name="vpnclientconfig"></a>4. Konfigurace klienta hello

tooconnect tooa sítě VNet pomocí sítě VPN Point-to-Site, každý klient musíte nainstalovat balíček tooconfigure hello nativního klienta VPN ve Windows. Hello konfigurační balíček nakonfiguruje Nativní klient VPN ve Windows hello s hello nastavení potřebné tooconnect toohello virtuální sítě.

Hello stejné konfigurace klienta VPN pomocí balíčku v každém klientském počítači, můžete použít také hello verze odpovídá hello architektura pro klienta hello. Seznam hello klientské operační systémy, které jsou podporovány, naleznete v části hello [připojeníPoint-to-Site – nejčastější dotazy](#faq) na konci hello tohoto článku.

### <a name="generateconfigpackage"></a>Část 1: Generování a nainstalujte balíček konfigurace klienta VPN hello

1. V portálu Azure, v hello hello **přehled** stránky pro vaši virtuální síť v **připojení k síti VPN**, klikněte na tlačítko hello klienta grafické tooopen hello **Point-to-site VPN připojení** stránky.
2. Hello horní části hello **Point-to-site VPN připojení** klikněte na tlačítko hello balíček ke stažení, která odpovídá toohello klientský operační systém, ve kterém bude nainstalován:

  * U 64bitových klientů vyberte **Klient VPN (64bitový)**.
  * U 32bitových klientů vyberte **Klient VPN (32bitový)**.

  ![Stažení balíčku pro konfiguraci klienta VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png)<br>
3. Jakmile hello zabalené generuje, stáhněte a nainstalujte ji na klientském počítači. Pokud se zobrazí automaticky otevírané okno filtru SmartScreen, klikněte na **Další informace** a potom na **Přesto spustit**. Můžete také uložit balíček tooinstall hello na další klientské počítače.

### <a name="installclientcert"></a>Část 2: Instalace hello klientského certifikátu

Pokud chcete připojení toocreate P2S z klientského počítače, než hello, kterou používá toogenerate hello klientské certifikáty, musíte tooinstall klientský certifikát. Při instalaci klientského certifikátu, je nutné hello heslo, které byla vytvořena, když byl exportován hello klientský certifikát. Obvykle se jedná pouze stačit dvakrát kliknete na soubor certifikátu hello a nainstalujte ho. Další informace najdete v tématu [Instalace exportovaného klientského certifikátu](vpn-gateway-certificates-point-to-site.md#install).

## <a name="connect"></a>5. Připojit tooAzure

### <a name="connect-tooyour-vnet"></a>Připojit tooyour virtuální sítě

1. tooconnect tooyour virtuální síť, v klientském počítači hello přejděte tooVPN připojení a vyhledejte hello připojení VPN, který jste vytvořili. Je název hello stejný název jako vaše virtuální síť. Klikněte na **Připojit**. Zobrazí se zpráva se zobrazí s odkazuje toousing hello certifikátu. Pokud k tomu dojde, klikněte na tlačítko **pokračovat** toouse se zvýšenými oprávněními.
2. Na hello **připojení** stavové stránce klikněte na tlačítko **připojit** toostart hello připojení. Pokud se zobrazí **vybrat certifikát** obrazovky, ověřte, zda je zobrazení certifikátu klienta hello hello jeden, které chcete toouse tooconnect. Pokud není, použijte hello šipku tooselect hello správný certifikát a pak klikněte na tlačítko **OK**.

  ![Připojení klienta VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png)
3. Vaše připojení bylo vytvořeno.

  ![Vytvořené připojení](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png)

#### <a name="troubleshooting-p2s-connections"></a>Řešení potíží s připojeními P2S

[!INCLUDE [verify-client-certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

### <a name="verifyvpnconnect"></a>Ověření připojení VPN hello

1. tooverify, že je připojení k síti VPN aktivní, otevřete příkazový řádek se zvýšenými oprávněními a spusťte *ipconfig/all*.
2. Zobrazení výsledků hello. Všimněte si, že hello IP adresu, kterou jste obdrželi je jednou z hello adresy v rozsahu adres připojení hello Point-to-Site, kterou jste zadali při vytváření virtuální sítě. výsledky Hello by měl vypadat podobně jako příklad toothis:

  ```
    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled
  ```

## <a name="connectVM"></a>Připojit tooa virtuálního počítače

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-p2s-classic-include.md)]

## <a name="add"></a>Přidání a odebrání důvěryhodných kořenových certifikátů

Důvěryhodný kořenový certifikát můžete do Azure přidat nebo ho z Azure odebrat. Když odeberete kořenový certifikát, klienti, kteří mají vygenerovat z tento kořenový certifikát nebude možné tooauthenticate a proto nebude možné tooconnect. Pokud chcete klienta tooauthenticate a připojit, je nutné vygenerovat nový klientský certifikát z kořenového certifikátu, který je důvěryhodný (nahrané) tooAzure tooinstall.

### <a name="addtrustedroot"></a>tooadd důvěryhodného kořenového certifikátu

Můžete přidat až too20 důvěryhodný kořenový certifikát .cer soubory tooAzure. Pokyny najdete v tématu [oddíl 3 – soubor .cer kořenového certifikátu v odesílání hello](#upload).

### <a name="removetrustedroot"></a>tooremove důvěryhodného kořenového certifikátu

1. Na hello **připojení k síti VPN** části hello stránky pro vaši virtuální síť, klikněte na tlačítko hello **klienti** hello grafické tooopen **Point-to-site VPN připojení** stránky.

  ![Klienti](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. Na hello **připojeníPoint-to-site** klikněte na tlačítko **spravovat certifikáty** tooopen hello **certifikáty** stránky.<br>

  ![Stránka certifikátů](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. Na hello **certifikáty** klikněte na tlačítko hello třemi tečkami další toohello certifikátů, které tooremove, a potom klikněte na tlačítko **odstranit**.

  ![Odstranění kořenového certifikátu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deleteroot.png)<br>

## <a name="revokeclient"></a>Odvolání klientského certifikátu

Certifikáty klientů lze odvolat. Hello certifikátu, seznamu odvolaných certifikátů můžete tooselectively Odepřít připojení Point-to-Site na základě jednotlivých klientských certifikátů. To se liší od odebrání důvěryhodného kořenového certifikátu. Pokud odeberete z Azure důvěryhodného kořenového certifikátu .cer, odvolá hello přístup pro všechny klientské certifikáty generované podepsané pomocí hello odvolané kořenový certifikát. Odvolání certifikátu klienta, nikoli hello kořenový certifikát, umožňuje hello další certifikáty, které byly generovány od hello kořenový certifikát toocontinue toobe používá k ověřování pro připojení hello Point-to-Site.

běžnou praxí Hello je toouse hello kořenový certifikát toomanage přístup na tým nebo organizace úrovních, při použití odvolané klientské certifikáty pro řízení přístupu podrobných na jednotlivé uživatele.

### <a name="revokeclientcert"></a>toorevoke certifikát klienta

Přidáním seznamu odvolaných certifikátů toohello kryptografický otisk hello můžete odvolat certifikát klienta.

1. Načtěte hello kryptografický otisk certifikátu klienta. Další informace najdete v tématu [postupy: načtení hello kryptografického otisku certifikátu](https://msdn.microsoft.com/library/ms734695.aspx).
2. Zkopírujte hello informace tooa textovém editoru a odeberte všechny mezery tak, aby se souvislý řetězec.
3. Přejděte toohello **"název klasickou virtuální síť" > připojení Point-to-site VPN > Certifikáty** a pak klikněte na tlačítko **seznamu odvolaných certifikátů** stránku seznamu odvolaných certifikátů tooopen hello. 
4. Na hello **seznamu odvolaných certifikátů** klikněte na tlačítko **+ přidat certifikát** tooopen hello **přidat certifikát toorevocation seznamu** stránky.
5. Na hello **přidat certifikát toorevocation seznamu** stránky, vložte kryptografický otisk certifikátu hello jako průběžné řádků textu, bez mezer. Klikněte na tlačítko **OK** v hello dolní části stránky hello.
6. Po dokončení aktualizace hello certifikát už se nedá použít tooconnect. Klienti, které se pokusí použít tento certifikát tooconnect zobrazí zpráva, že tento certifikát hello již není platný.

## <a name="faq"></a>Nejčastější dotazy týkající se připojení Point-to-Site

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Další kroky
Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě. Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). toounderstand Další informace o sítě a virtuálních počítačů najdete v části [přehled sítě Azure a virtuální počítač s Linuxem](../virtual-machines/linux/azure-vm-network-overview.md).
