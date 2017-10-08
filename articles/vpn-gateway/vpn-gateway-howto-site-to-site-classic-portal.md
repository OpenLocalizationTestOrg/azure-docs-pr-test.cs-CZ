---
title: "Připojení vaší místní síti tooan virtuální síť Azure: Site-to-Site VPN (klasické): portál | Microsoft Docs"
description: "Kroky toocreate připojení IPsec z místní sítě tooan virtuální síť Azure přes hello veřejného Internetu. Tyto kroky můžete vytvořit připojení mezi různými místy Site-to-Site VPN Gateway pomocí portálu hello."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/010/2017
ms.author: cherylmc
ms.openlocfilehash: b260bdf610b264458660b278bd32bf0fc5b519ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-site-to-site-connection-using-hello-azure-portal-classic"></a>Umožňuje vytvořit připojení Site-to-Site pomocí portálu Azure (klasický) hello

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Tento článek ukazuje, jak toouse hello Azure portálu toocreate připojení k bráně VPN Site-to-Site z vaší místní síti toohello virtuální sítě. Hello kroky v tomto článku použít toohello modelu nasazení classic. Můžete také vytvořit této konfigurace pomocí nástroje pro jiné nasazení nebo model nasazení tak, že vyberete jinou možnost z hello následující seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Rozhraní příkazového řádku](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Azure Portal (Classic)](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>

Připojení k bráně VPN Site-to-Site je použité tooconnect místní sítě tooan virtuální síť Azure přes tunelové propojení VPN protokolu IPsec/IKE (IKEv1 nebo IKEv2). Tento typ připojení vyžaduje sítě VPN zařízení nachází v místě které má zvenčí veřejnou IP adresu přiřazenou tooit. Další informace o bránách VPN najdete v tématu [Informace o službě VPN Gateway](vpn-gateway-about-vpngateways.md).

![Diagram připojení VPN Gateway typu Site-to-Site mezi různými místy](./media/vpn-gateway-howto-site-to-site-classic-portal/site-to-site-diagram.png)

## <a name="before-you-begin"></a>Než začnete

Ověřte, že jste splnili hello před zahájením konfigurace následující kritéria:

* Ověřte, že chcete toowork v modelu nasazení classic hello. Pokud chcete toowork v modelu nasazení Resource Manager hello, najdete v části [vytvořit připojení Site-to-Site (Resource Manager)](vpn-gateway-howto-site-to-site-resource-manager-portal.md). Pokud je to možné, doporučujeme vám, že používáte hello modelu nasazení Resource Manager.
* Zajistěte, aby byla kompatibilní zařízení VPN a někoho, kdo je možné tooconfigure ho. Další informace o kompatibilních zařízeních VPN a konfiguraci zařízení najdete v tématu [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md).
* Ověřte, že máte veřejnou IPv4 adresu pro vaše zařízení VPN. Tato IP adresa nesmí být umístěná za překladem adres (NAT).
* Pokud jste obeznámeni s rozsahy IP adres hello umístěný v konfiguraci místní sítě, musíte toocoordinate s někým, kdo může pro vás jsou tyto podrobnosti obsaženy. Když vytváříte tuto konfiguraci, musíte zadat hello IP adresa rozsahu předpony, Azure bude směrovat tooyour místní umístění. Žádná z podsítí hello vaší místní sítě můžete prostřednictvím okruhu s hello podsítě virtuální sítě, které chcete tooconnect k.
* V současné době prostředí PowerShell je požadovaná toospecify hello sdílený klíč a vytvoření připojení k bráně VPN hello. Nainstalujte nejnovější verzi rutin prostředí PowerShell Azure Service Management (SM) hello hello. Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview). Při práci s prostředím PowerShell pro tuto konfiguraci se ujistěte, že jej spouštíte jako správce. 

### <a name="values"></a>Ukázkové hodnoty konfigurace pro toto cvičení

Hello příklady v tomto článku používají hello následující hodnoty. Můžete použít tyto hodnoty toocreate testovací prostředí nebo odkazovat toothem toobetter pochopit hello příklady v tomto článku.

* **Název virtuální sítě:** TestVNet1
* **Adresní prostor:** 
  * 10.11.0.0/16
  * 10.12.0.0/16 (volitelné pro toto cvičení)
* **Podsítě:**
  * FrontEnd: 10.11.0.0/24
  * BackEnd: 10.12.0.0/24 (volitelné pro toto cvičení)
* **Podsíť brány:** 10.11.255.0/27
* **Skupina prostředků:** TestRG1
* **Umístění:** Východní USA
* **Server DNS:** 10.11.0.3 (volitelné pro toto cvičení)
* **Název místní lokality:** Site2
* **Klient adresní prostor:** hello adresní prostor, který se nachází na místním serverem.

## <a name="CreatVNet"></a>1. Vytvoření virtuální sítě

Když vytvoříte virtuální síť toouse pro připojení S2S, je nutné toomake jistotu, že hello adresní prostory, které zadáte nepřekrývají s žádným z hello klienta adresních prostorů pro hello místní weby, které chcete tooconnect k. Pokud se podsítě překrývají, připojení nebude fungovat správně.

* Pokud již máte virtuální síť, ověřte, zda text hello nastavení jsou kompatibilní s návrhem vaší brány VPN. Věnujte zvláštní pozornost tooany podsítě, které se mohou překrývat s jinými sítěmi. 

* Pokud ještě nemáte virtuální síť, vytvořte si ji. Snímky obrazovek slouží jen jako příklady. Být jisti tooreplace hello hodnoty vlastními.

### <a name="toocreate-a-virtual-network"></a>toocreate virtuální sítě

1. V prohlížeči přejděte toohello [portál Azure](http://portal.azure.com) a v případě potřeby, přihlaste se pomocí účtu Azure.
2. Klikněte na **+**. V hello **vyhledávání hello marketplace** pole, zadejte "Virtuální sítě". Vyhledejte **virtuální sítě** z hello vrátil seznamu a klikněte na tlačítko tooopen hello **virtuální sítě** stránky.

  ![Stránka pro vyhledání virtuální sítě](./media/vpn-gateway-howto-site-to-site-classic-portal/newvnetportal700.png)
3. Téměř hello dolní části stránky hello virtuální sítě z hello **vybrat model nasazení** rozevíracího seznamu vyberte **Classic**a potom klikněte na **vytvořit**.

  ![Výběr modelu nasazení](./media/vpn-gateway-howto-site-to-site-classic-portal/selectmodel.png)
4. Na hello **vytvořit virtuální network(classic)** nakonfigurujte nastavení sítě VNet hello. Na této stránce přidáte první adresní prostor a jeden rozsah adres podsítě. Po dokončení vytváření hello virtuální síť, můžete přejít zpět a přidat další podsítě a adresní prostory.

  ![Stránka pro vytvoření virtuální sítě](./media/vpn-gateway-howto-site-to-site-classic-portal/createvnet.png "Stránka pro vytvoření virtuální sítě")
5. Ověřte, že hello **předplatné** hello je správná. Odběry můžete změnit pomocí rozevíracího seznamu hello.
6. Klikněte na **Skupina prostředků** a vyberte existující skupinu prostředků nebo zadáním názvu vytvořte novou. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Potom vyberte hello **umístění** nastavení pro virtuální síť. umístění Hello Určuje, kde se bude nacházet hello prostředky, abyste nasadili toothis virtuální sítě.
8. Pokud chcete mít toofind toobe virtuální síť snadno na řídicím panelu hello, vyberte **Pin toodashboard**. Klikněte na tlačítko **vytvořit** toocreate virtuální síť.

  ![PIN kód toodashboard](./media/vpn-gateway-howto-site-to-site-classic-portal/pintodashboard150.png "toodashboard PIN kód")
9. Po kliknutí na tlačítko 'Vytvořit', zobrazí se na dlaždici na hello řídicí panel, který zobrazuje průběh hello virtuální síť. změny dlaždice Hello jako hello se vytváří virtuální sítě.

  ![Dlaždice Vytváří se virtuální síť](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Vytváří se virtuální síť")

Po vytvoření virtuální sítě uvidíte **vytvořen** uvedené v části **stav** na stránce sítí hello hello portál Azure classic.

## <a name="additionaladdress"></a>2. Přidání dalšího adresního prostoru

Po vytvoření virtuální sítě můžete přidat další adresní prostor. Přidání další adresní prostor není požadovaná součást S2S konfigurace, ale pokud budete potřebovat více adresní prostory, použijte hello následující kroky:

1. Vyhledejte hello virtuálních sítí v portálu hello.
2. Na stránce hello vaší virtuální sítě, v části hello **nastavení** klikněte na tlačítko **adresní prostor**.
3. Na stránce místo hello adresu, klikněte na **+ přidat** a zadat další adresní prostor.

## <a name="dns"></a>3. Určení serveru DNS

Nastavení DNS se nevyžaduje jako součást konfigurace Site-to-Site, ale pokud chcete překlad IP adres, server DNS je nezbytný. Zadání hodnoty nevytvoří nový server DNS. Hello IP adresa serveru DNS, který zadáte musí být server DNS, který může překládat názvy hello hello prostředků, ke kterému se připojujete. Pro příklad nastavení hello jsme použili privátní IP adresy. Hello IP adresu, kterou používáme je pravděpodobně není hello IP adresu serveru DNS. Být jisti toouse vlastní hodnoty.

Po vytvoření virtuální sítě, můžete přidat IP adresu překlad názvu DNS serveru toohandle hello. Otevřete hello nastavení pro vaši virtuální síť, klikněte na servery DNS a přidejte hello IP adresa serveru DNS hello chcete toouse překladu IP adres.

1. Vyhledejte hello virtuálních sítí v portálu hello.
2. Na stránce hello vaší virtuální sítě, v části hello **nastavení** klikněte na tlačítko **servery DNS**.
3. Přidejte server DNS.
4. Klikněte na nastavení, toosave **Uložit** hello horní části stránky hello.

## <a name="localsite"></a>4. Nakonfigurujte místní lokality hello

Hello místní sítě obvykle odkazuje tooyour místní umístění. Obsahuje IP adresu hello toowhich zařízení VPN hello vytvoříte připojení a hello rozsahů IP adres, které budou směrovány prostřednictvím zařízení VPN toohello brány sítě VPN hello.

1. Hello portálu přejděte toohello virtuální sítě, pro které chcete toocreate bránu.
2. Na stránce hello vaší virtuální sítě na hello **přehled** klikněte na stránce, v části připojení VPN hello **brány** tooopen hello **nové připojení VPN** stránky.

  ![Klikněte na tlačítko nastavení brány tooconfigure](./media/vpn-gateway-howto-site-to-site-classic-portal/beforegw125.png "klikněte na tlačítko nastavení brány tooconfigure")
3. Na hello **nové připojení VPN** vyberte **Site-to-site**.
4. Klikněte na tlačítko **místní lokality – nakonfigurujte požadovaná nastavení** tooopen hello **místní lokality** stránky. Nakonfigurujte nastavení hello a pak klikněte na tlačítko **OK** toosave hello nastavení.
  - **Název:** vytvořit název pro vaše místní lokality toomake snadno můžete tooidentify.
  - **IP adresa brány sítě VPN:** jde hello veřejná IP adresa zařízení VPN hello ve vaší místní síti. zařízení VPN Hello vyžaduje veřejnou IP adresu IPv4. Zadejte platnou veřejnou IP adresu pro hello toowhich zařízení VPN chcete tooconnect. Ho nemůže být za serverem NAT a má toobe dostupný v Azure. Pokud si nejste jisti hello IP adresa vašeho zařízení VPN, můžete vždy uvést do hodnotu zástupného symbolu (předpokladu, že je ve formátu hello platné veřejné IP adresy) a později ji změnit.
  - **Klient adresní prostor:** seznam rozsahů adres IP v hello, které chcete toohello místní sítě prostřednictvím této brány se směrováním. Můžete přidat více různých rozsahů adres. Zajistěte, aby hello rozsahy, který zde určíte se nepřekrývají rozsahy jiných sítí, ke kterým vaše virtuální síť se připojuje k nebo s rozsahy adres hello hello virtuální sítě, sám sebe.

  ![Místní lokalita](./media/vpn-gateway-howto-site-to-site-classic-portal/localnetworksite.png "Konfigurace místní lokality")

## <a name="gatewaysubnet"></a>5. Nakonfigurujte podsítě brány hello

Pro bránu VPN je nutné vytvořit podsíť brány. podsíť brány Hello obsahuje hello IP adresy, které používají služby brány VPN hello.

1. Na hello **nové připojení VPN** stránky, vyberte hello políčko **vytvořit bránu hned**. Zobrazí se stránka Hello "Konfigurace volitelné brány'. Pokud nevyberete hello políčko, neuvidíte podsíť brány hello tooconfigure stránku hello.

  ![Konfigurace brány – Podsíť, velikost, typ směrování](./media/vpn-gateway-howto-site-to-site-classic-portal/optional.png "Konfigurace brány – Podsíť, velikost, typ směrování")
2. tooopen hello **konfigurace brány** klikněte na tlačítko **konfigurace volitelné brány - podsíť, velikost a typ směrování**.
3. Na hello **konfigurace brány** klikněte na tlačítko **podsíť – nakonfigurujte požadovaná nastavení** tooopen hello **přidat podsíť** stránky.

  ![Konfigurace brány – podsíť brány](./media/vpn-gateway-howto-site-to-site-classic-portal/subnetrequired.png "Konfigurace brány – podsíť brány")
4. Na hello **přidat podsíť** přidejte podsíť brány hello. velikost Hello hello podsíť brány, který zadáte závisí na konfiguraci brány VPN hello, které chcete toocreate. I když je možné toocreate podsíť brány jako malé/29, doporučujeme použít/27 nebo velikosti/28. Vytvoří se tak vetší podsíť zahrnující více adres. Pomocí větší podsíť brány umožňuje dost IP adres tooaccommodate možné budoucí konfigurace.

  ![Přidání podsítě brány](./media/vpn-gateway-howto-site-to-site-classic-portal/addgwsubnet.png "Přidání podsítě brány")

## <a name="sku"></a>6. Zadejte hello SKU a typ sítě VPN

1. Vyberte hello brány **velikost**. Toto je hello brány SKU použít toocreate brány virtuální sítě. V portálu hello hello 'výchozí SKU = **základní**. Classic brány sítě VPN pomocí hello původní (starší) SKU brány. Další informace o starší verze SKU brány hello najdete v tématu [práce s SKU (starý SKU) brány virtuální sítě](vpn-gateway-about-skus-legacy.md).

  ![Výběr SKU a typu sítě VPN](./media/vpn-gateway-howto-site-to-site-classic-portal/sku.png "Výběr SKU a typu sítě VPN")
2. Vyberte hello **typ směrování** pro bránu. Tím se taky říká hello typ sítě VPN. Je důležité tooselect hello brány správný typ protože tooanother jeden typ nelze převést hello brány. Vaše zařízení VPN musí být kompatibilní s typem směrování hello, kterou vyberete. Další informace o typu sítě VPN najdete v tématu [Informace o nastavení služby VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md#vpntype). Může se zobrazit články odkazující too'RouteBased' a 'PolicyBased' VPN typy. ' Dynamické odpovídá too'RouteBased, a "Statická" odpovídá 'PolicyBased'.
3. Klikněte na tlačítko **OK** toosave hello nastavení.
4. Na hello **nové připojení VPN** klikněte na tlačítko **OK** dole hello toobegin stránku hello vytvoření brány virtuální sítě. V závislosti na hello SKU vyberete může to trvat až minut toocreate too45 bránu virtuální sítě.

## <a name="vpndevice"></a>7. Konfigurace zařízení VPN

Připojení Site-to-Site tooan do místní sítě vyžaduje zařízení VPN. V tomto kroku nakonfigurujete zařízení VPN. Při konfiguraci zařízení VPN, je třeba hello následující:

- Sdílený klíč. To je hello stejný sdílený klíč, který zadáte při vytváření připojení Site-to-Site VPN. V našich ukázkách používáme základní sdílený klíč. Doporučujeme, abyste vygenerování složitější klíče toouse.
- Hello veřejnou IP adresu brány virtuální sítě. Hello veřejnou IP adresu můžete zobrazit pomocí hello portálu Azure, PowerShell nebo rozhraní příkazového řádku.

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>8. Vytvoření připojení hello
V tomto kroku nastavit sdílený klíč hello a vytvořit hello připojení. klíč Hello nastavíte musí být hello stejný klíč, který se používal v konfiguraci zařízení VPN.

> [!NOTE]
> Tento krok je v současné době není k dispozici v hello portálu Azure. Musíte použít verzi služby správy (SM) hello hello rutin prostředí Azure PowerShell.
>

### <a name="step-1-connect-tooyour-azure-account"></a>Krok 1. Připojit tooyour účet Azure

1. Otevřete konzolu prostředí PowerShell se zvýšenými oprávněními a připojte tooyour účtu. Použijte následující příklad toohelp, ke kterým se připojujete hello:

  ```powershell
  Add-AzureAccount
  ```
2. Zkontrolujte předplatná hello pro účet hello.

  ```powershell
  Get-AzureSubscription
  ```
3. Pokud máte více než jedno předplatné, vyberte hello předplatné, které chcete toouse.

  ```powershell
  Select-AzureSubscription -SubscriptionId "Replace_with_your_subscription_ID"
  ```

### <a name="step-2-set-hello-shared-key-and-create-hello-connection"></a>Krok 2. Nastavit sdílený klíč hello a vytvořit připojení hello

Při práci s modelem nasazení classic PowerShell a hello, někdy hello názvy zdrojů v portálu hello nejsou hello názvy hello Azure očekává toosee při použití prostředí PowerShell. Hello následující postup vám pomůže exportovat hello síťové konfigurace souboru tooobtain hello přesné hodnoty pro názvy hello.

1. Vytvoření adresáře v počítači a pak je exportovat hello sítě konfiguračního souboru toohello adresáře. V tomto příkladu hello sítě konfigurační soubor je exportovaný tooC:\AzureNet.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
2. Otevřete konfigurační soubor hello sítě pomocí editoru xml a zkontrolujte hello hodnoty "LocalNetworkSite název" a "VirtualNetworkSite název". Hello příklad tooreflect hello hodnoty, které je třeba upravte. Pokud zadáte název, který obsahuje mezery, použijte jednoduchých uvozovek hello hodnotu.

3. Nastavit sdílený klíč hello a vytvořit hello připojení. Hello '-SharedKey, je hodnota, která můžete vygenerovat a zadat. V příkladu hello jsme použili 'abc123', ale můžete vygenerovat (a měli) použít něco složitější. Důležité: co je tuto hodnotu hello, který zde určíte Hello musí být hello stejnou hodnotu, kterou jste zadali při konfiguraci zařízení VPN.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName 'Group TestRG1 TestVNet1' `
  -LocalNetworkSiteName 'D1BFC9CB_Site2' -SharedKey abc123
  ```
Při vytváření připojení hello je výsledek hello: **stav: úspěšné**.

## <a name="verify"></a>9. Ověření stavu připojení

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

Pokud máte potíže s připojením, přečtěte si téma hello **Poradce při potížích** části tabulky hello obsahu v levém podokně hello.

## <a name="reset"></a>Jak tooreset brána sítě VPN

Resetování brány Azure VPN je užitečné v případě ztráty připojení VPN mezi lokalitami na jednom nebo více tunelech VPN typu Site-to-Site. V této situaci vaše místní zařízení VPN jsou všechny funguje správně, ale nebylo možné tooestablish tunelových propojení IPsec pomocí bran Azure VPN hello. Pokyny najdete v tématu [Resetování brány VPN](vpn-gateway-resetgw-classic.md).

## <a name="changesku"></a>Jak toochange skladová položka brány

Hello kroky toochange skladová položka brány, najdete v části [změnit velikost skladová položka brány](vpn-gateway-about-SKUS-legacy.md).

## <a name="next-steps"></a>Další kroky

* Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě. Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
* Informace o vynuceném tunelování najdete v tématu [Informace o vynuceném tunelování](vpn-gateway-about-forced-tunneling.md).