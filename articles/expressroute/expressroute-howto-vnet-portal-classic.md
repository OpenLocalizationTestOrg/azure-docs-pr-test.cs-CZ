---
title: "aaaConfigure virtuální sítě a brány pro ExpressRoute v hello klasického portálu | Microsoft Docs"
description: "Tento článek vás provede procesem nastavení virtuální sítě pro ExpressRoute pomocí modelu nasazení classic hello a portálu classic hello."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 688817d9-51c8-4372-9af8-33fa098c7c5a
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/20/2016
ms.author: cherylmc
ms.openlocfilehash: dd1f6c5e85dbb3ad0a53ecd81c13b4d3f5c06e66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-for-expressroute-in-hello-classic-portal"></a>Vytvoření virtuální sítě pro ExpressRoute portálu classic hello
Hello kroky v tomto článku vás provede konfiguraci virtuální sítě a brány virtuální sítě pro použití s ExpressRoute pomocí modelu nasazení classic hello a portálu classic hello.

Pokud hledáte pokyny pro hello modelu nasazení Resource Manager, můžete použít následující články hello: [vytvoření virtuální sítě pomocí prostředí PowerShell](../virtual-network/virtual-networks-create-vnet-arm-ps.md) a [přidat tooa bránu VPN virtuální sítě Resource Manageru pro ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**O modelech nasazení Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="create-a-classic-vnet-and-gateway"></a>Vytvoření klasické virtuální sítě a brány
Hello následujících kroků vytvořte klasické virtuální sítě a brány virtuální sítě. Pokud již máte klasické virtuální síti, najdete v části hello [nakonfigurovat existující klasické virtuální síti](#config) v tomto článku.

1. Přihlaste se toohello [portál Azure classic](http://manage.windowsazure.com).
2. V hello levém dolním rohu úvodní obrazovka, klikněte na **nový**. V navigačním podokně hello, klikněte na tlačítko **síťové služby**a potom klikněte na **virtuální sítě**. Klikněte na tlačítko **vytvořit vlastní** Průvodce konfigurací toobegin hello.
3. Na hello **podrobnosti virtuální sítě** zadejte hello následující:
   
   * **Název** – název virtuální sítě. Tento název virtuální sítě budete používat při nasazení vaší virtuálních počítačů a instancí PaaS, takže se nemusí má název hello toomake nenastavujte příliš komplikovaný.
   * **Umístění** – umístění hello je přímo související toohello fyzickým umístěním (oblastí) místo tooreside vaše prostředky (virtuální počítače). Například pokud chcete hello virtuálních počítačů, které nasazujete toobe virtuální sítě toothis fyzicky umístěné ve východní USA, vyberte toto umístění. Nelze změnit hello oblast přidruženou k virtuální síti po jejím vytvoření.
4. Na hello **servery DNS a připojení VPN** stránky, zadejte hello následující informace a potom klikněte na šipku další hello v pravé dolní hello. 
   
   * **Servery DNS** – zadejte název serveru DNS hello a IP adresu, nebo vyberte dříve zaregistrovaný server DNS z místní nabídky hello. Toto nastavení neslouží k vytvoření serveru DNS. Umožňuje vám toospecify hello DNS serverů, které má toouse pro překlad názvů pro tuto virtuální síť.
   * **Připojení Site-to-Site** – vyberte hello zaškrtávací políčko pro **konfigurace VPN typu site-to-site**.
   * **ExpressRoute** – vyberte hello políčko **pomocí ExpressRoute**. Tato možnost se zobrazí, pouze pokud jste vybrali **konfigurace VPN typu Site-to-Site**.
   * **Místní sítě** -jsou požadované toohave místní síťový web pro ExpressRoute. Ale v případě hello připojení ExpressRoute, předpony adres hello určená pro hello místní síť budou ignorovány. Místo toho hello předpony inzerované tooMicrosoft prostřednictvím hello okruh ExpressRoute se bude používat pro účely směrování.<BR>Pokud již máte místní síť vytvořenou pro připojení ExpressRoute, můžete ji vyberte z rozevíracího seznamu hello. Pokud ne, vyberte **zadejte novou místní síť**.
5. Hello **připojení Site-to-Site** stránka se zobrazí, pokud jste vybrali v předchozím kroku hello toospecify novou místní síť. tooconfigure vaší místní síti, zadejte hello následující informace a potom klikněte na šipku další hello. 
   
   * **Název** -hello název, který má toocall vaše místní síť (místní).
   * **Adresní prostor** – včetně počáteční IP adresu a CIDR (počet adres). Můžete zadat libovolný rozsah adres, tak dlouho, dokud se nepřekrývá s hello rozsah adres pro virtuální síť. Obvykle to by zadejte hello rozsahy adres pro místní sítě, ale v případě hello expressroute se toto nastavení nepoužívají. Toto nastavení je však vyžadováno v pořadí toocreate hello místní sítě při použití portálu classic hello.
   * **Přidání adresního prostoru** – toto nastavení není relevantní pro ExpressRoute.
6. Na hello **adresní prostory virtuální sítě** zadejte hello následující informace a potom klikněte na značku zaškrtnutí hello v hello nižší správné tooconfigure vaší sítě. 
   
   * **Adresní prostor** – včetně počáteční IP a počet adres. Ověřte, že zadáte hello adresní prostory nepřekrývají s hello adresní prostory, které máte v místní síti.
   * **Přidání podsítě** – včetně počáteční IP adresu a počet adres. Dodatečné podsítě nejsou vyžadovány.
   * **Přidání podsítě brány** – klikněte na podsíť brány tooadd hello. podsíť brány Hello se používá pouze pro bránu virtuální sítě hello a požadované pro tuto konfiguraci.<BR>Hello podsíť brány CIDR (počet adres) pro ExpressRoute musí být velikosti/28 nebo větší (/ 27, / 26 atd.). To umožňuje dost IP adres v této podsíti tooallow hello konfigurace toowork. Na portálu classic hello Pokud jste vybrali hello políčko toouse ExpressRoute, portál hello určuje podsíť brány s velikosti/28.  Nelze upravit počet adres CIDR hello portálu classic hello. podsíť brány Hello se zobrazí jako **brány** hello klasickém portálu, i když hello skutečné jméno hello podsíť brány, který se vytvoří ve skutečnosti je **GatewaySubnet**. Tento název můžete zobrazit pomocí prostředí PowerShell nebo v hello portálu Azure.
7. Klikněte na značku zaškrtnutí v hello dolní části stránky hello hello a virtuální sítě se začnou toocreate. Po dokončení se zobrazí **vytvořen** uvedené v části **stav** na hello **sítě** stránky portálu classic hello.

## <a name="gw"></a>Vytvoření brány hello
1. Na hello **sítě** stránky, klikněte na tlačítko hello virtuální síť, kterou jste právě vytvořili a pak klikněte na tlačítko **řídicí panel** hello horní části stránky hello.
2. Na konci hello hello **řídicí panel** klikněte na tlačítko **vytvořit bránu** a vyberte **dynamické směrování**. Klikněte na tlačítko **Ano** tooconfirm, které chcete toocreate bránu.
3. Při spuštění služby hello gateway vytváření, se zobrazí zpráva informací, víte, že byla spuštěna této brány hello. To může trvat až minut too45 toocreate hello brány.
4. Propojení okruhu tooa vaší sítě. Postupujte podle pokynů hello v článku hello [jak toolink virtuálních sítí tooExpressRoute okruhy](expressroute-howto-linkvnet-classic.md).

## <a name="config"></a>Konfigurace existující klasické virtuální sítě pro ExpressRoute
Pokud již máte klasické virtuální sítě, můžete ho nakonfigurovat tooconnect tooExpressRoute portálu classic hello. nastavení Hello jsou hello stejné jako hello části výše, tak pro čtení prostřednictvím těchto oddílů toobecome obeznámeni s hello požadované nastavení. Pokud chcete toocreate koexistujících připojení k ExpressRoute nebo Site-to-Site, najdete v části [v tomto článku](expressroute-howto-coexist-classic.md) hello kroky. Jsou jiné než hello kroky v tomto článku.

1. Je nutné toocreate hello místní sítě, než aktualizujete hello zbytek nastavení virtuální sítě. Klikněte na novou místní síť, která se vyžaduje při konfiguraci ExpressRoute prostřednictvím portálu classic hello, toocreate **nový**  **>**  **síťové služby**  **>**  **Virtuální sítě**  **>**  **místní sítě přidat**. Postupujte podle hello Průvodce kroky toocreate hello místní sítě.
2. Použití **konfigurace** stránka tooupdate hello zbytek hello nastavení pro virtuální síť a tooassociate hello virtuální síť toohello místní síti.
3. Po dokončení konfigurace nastavení hello, přejděte toohello [vytvořit bránu hello](#gw) části tohoto článku toocreate hello brány.

## <a name="next-steps"></a>Další kroky
* Pokud chcete, aby virtuální síť tooyour tooadd virtuální počítače, přečtěte si téma [virtuální počítače kurzů](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).
* Pokud chcete, aby toolearn Další informace o ExpressRoute, přečtěte si téma hello [přehled ExpressRoute](expressroute-introduction.md).

