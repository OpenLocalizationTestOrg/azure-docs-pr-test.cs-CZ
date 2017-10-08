---
title: "Přidání více VPN brána Site-to-Site připojení tooa virtuální sítě: portálu Azure: Resource Manager | Microsoft Docs"
description: "Přidání více lokalit S2S připojení tooa VPN gateway, který má existující připojení"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f3e8b165-f20a-42ab-afbb-bf60974bb4b1
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2017
ms.author: cherylmc
ms.openlocfilehash: b8c9ff454967f509dcef725f8bcec8564fad9b00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-site-to-site-connection-tooa-vnet-with-an-existing-vpn-gateway-connection"></a>Přidat tooa připojení Site-to-Site virtuální síť s existujícím připojením VPN gateway

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (Classic)](vpn-gateway-multi-site.md)
>
> 

Tento článek vás provede pomocí hello Azure tooadd portálu Site-to-Site (S2S) připojení tooa VPN bránu, která má existující připojení. Tento typ připojení je často označují tooas "s více servery" konfigurace. Můžete přidat tooa připojení S2S virtuální sítě, který už má připojení S2S, připojení Point-to-Site nebo připojení VNet-to-VNet. Při přidávání připojení existují určitá omezení. Zkontrolujte hello [před zahájením](#before) část v tooverify Tento článek před zahájením konfiguraci. 

Tento článek se týká tooVNets vytvořené pomocí modelu nasazení Resource Manager hello, které mají brána sítě VPN RouteBased. Tyto kroky se nevztahují konfigurace koexistujících připojení tooExpressRoute/Site-to-Site. V tématu [koexistujících připojení ExpressRoute nebo S2S](../expressroute/expressroute-howto-coexist-resource-manager.md) informace o připojení, která.

### <a name="deployment-models-and-methods"></a>Modely a metody nasazení
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Tuto tabulku aktualizujeme jako nové články a další nástroje je k dispozici pro tuto konfiguraci. Když je článek k dispozici, jsme odkaz přímo tooit z této tabulky.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="before"></a>Než začnete
Ověřte hello následující položky:

* Nejsou vytvoření koexistujících připojení ExpressRoute nebo S2S.
* Máte virtuální síť, která byla vytvořena pomocí modelu nasazení Resource Manager hello s existujícím připojením.
* Hello brány virtuální sítě pro virtuální síť je RouteBased. Pokud máte bránu PolicyBased VPN, musíte odstranit bránu virtuální sítě hello a vytvořit novou bránu VPN jako RouteBased.
* Žádný z rozsahů adres hello nepřekrývá pro všechny virtuální sítě, který tuto virtuální síť se připojuje k hello.
* Máte kompatibilní zařízení VPN a někoho, kdo je možné tooconfigure ho. Viz [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md). Pokud si nejste obeznámeni s konfiguraci zařízení VPN nebo nejste obeznámeni s rozsahy IP adres hello umístěné ve vaší konfiguraci místní sítě, musíte toocoordinate s někým, kdo může pro vás jsou tyto podrobnosti obsaženy.
* Máte zvenčí veřejnou IP adresu pro vaše zařízení VPN. Tato IP adresa nesmí být umístěná za překladem adres (NAT).

## <a name="part1"></a>Část 1: Konfigurace připojení
1. V prohlížeči přejděte toohello [portál Azure](http://portal.azure.com) a v případě potřeby, přihlaste se pomocí účtu Azure.
2. Klikněte na tlačítko **všechny prostředky** a vyhledejte vaše **brány virtuální sítě** hello seznamu prostředků a klikněte na něj.
3. Na hello **Brána virtuální sítě** okně klikněte na tlačítko **připojení**.
   
    ![Okno Připojení](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Okno Připojení")<br>
4. Na hello **připojení** okně klikněte na tlačítko **+ přidat**.
   
    ![Tlačítko Přidat připojení](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "tlačítko Přidat připojení")<br>
5. Na hello **přidat připojení** okně výplně out hello následující pole:
   
   * **Název:** toogive toohello webový server při vytváření připojení hello k název hello.
   * **Typ připojení:** vyberte **Site-to-site (IPsec)**.
     
     ![Přidat připojení okno](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "okno Přidat připojení")<br>

## <a name="part2"></a>Část 2 – přidání brány místní sítě
1. Klikněte na tlačítko **brány místní sítě** ***zvolit bránu místní sítě***. Otevře se hello **brány místní sítě vyberte** okno.
   
    ![Zvolte bránu místní sítě](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "vyberte bránu místní sítě")<br>
2. Klikněte na tlačítko **vytvořit nový** tooopen hello **vytvořit bránu místní sítě** okno.
   
    ![Okno brány místní sítě vytvořit](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "vytvořit bránu místní sítě")<br>
3. Na hello **vytvořit bránu místní sítě** okně výplně out hello následující pole:
   
   * **Název:** hello název prostředku brány místní sítě toohello toogive.
   * **IP adresa:** hello veřejná IP adresa zařízení VPN hello na hello lokalitě, která chcete tooconnect k.
   * **Adresní prostor:** hello adresního prostoru, které chcete toobe směrovány toohello nové místní síťový Web.
4. Klikněte na tlačítko **OK** na hello **vytvořit bránu místní sítě** okno toosave hello změny.

## <a name="part3"></a>Část 3 – přidat sdílený klíč hello a vytvoření připojení hello
1. Na hello **přidat připojení** okně Přidat hello sdílený klíč, které mají toouse toocreate připojení. Můžete buď získat sdílený klíč hello z vašeho zařízení VPN, nebo si ho vytvořit tady a pak nakonfigurujte vaší hello toouse zařízení VPN stejný sdílený klíč. Důležité je co přesně se klíče hello Hello hello stejné.
   
    ![Sdílený klíč](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Sdílený klíč")<br>
2. Hello dolní části okna hello, klikněte na **OK** toocreate hello připojení.

## <a name="part4"></a>Součástí 4 – ověření připojení VPN hello


[!INCLUDE [vpn-gateway-verify-connection-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="next-steps"></a>Další kroky

Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě. V tématu hello virtuální počítače [kurzů](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) Další informace.
