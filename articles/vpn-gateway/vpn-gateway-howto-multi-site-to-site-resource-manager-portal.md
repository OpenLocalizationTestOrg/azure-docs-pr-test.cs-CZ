---
title: "Přidání více připojení brány Site-to-Site VPN do virtuální sítě: portálu Azure: Resource Manager | Microsoft Docs"
description: "Přidat připojení S2S více lokalit pro bránu VPN, který má existující připojení"
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
ms.openlocfilehash: 7ec57789ee76f4ec54e4f7b68ea75c19522f3d7c
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Přidat připojení Site-to-Site k virtuální síti pomocí existujícího připojení brány sítě VPN

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (Classic)](vpn-gateway-multi-site.md)
>
> 

Tento článek vás provede pomocí portálu Azure přidat připojení Site-to-Site (S2S) pro bránu VPN, který má existující připojení. Tento typ připojení se často označuje jako "s více servery" konfigurace. Můžete přidat připojení S2S k virtuální síti, který už má připojení S2S, připojení Point-to-Site nebo připojení VNet-to-VNet. Při přidávání připojení existují určitá omezení. Zkontrolujte [před zahájením](#before) v tomto článku k ověření před zahájením konfiguraci. 

Tento článek se týká virtuálních sítí vytvořených pomocí modelu nasazení Resource Manager, které mají brána sítě VPN RouteBased. Tyto kroky se nevztahují na konfigurace koexistujících připojení ExpressRoute nebo Site-to-Site. V tématu [koexistujících připojení ExpressRoute nebo S2S](../expressroute/expressroute-howto-coexist-resource-manager.md) informace o připojení, která.

### <a name="deployment-models-and-methods"></a>Modely a metody nasazení
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Tuto tabulku aktualizujeme jako nové články a další nástroje je k dispozici pro tuto konfiguraci. Když je článek k dispozici, jsme přímý odkaz na něj z této tabulky.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="before"></a>Než začnete
Zkontrolujte následující položky:

* Nejsou vytvoření koexistujících připojení ExpressRoute nebo S2S.
* Máte virtuální síť, která byla vytvořena pomocí modelu nasazení Resource Manager stávající připojení.
* Brána virtuální sítě pro virtuální síť je RouteBased. Pokud máte bránu PolicyBased VPN, musíte odstranit bránu virtuální sítě a vytvořit novou bránu VPN jako RouteBased.
* Překrývat žádné z rozsahů adres pro všechny virtuální sítě, který tuto virtuální síť se připojuje k.
* Máte kompatibilní zařízení VPN a někoho, kdo jej umí nakonfigurovat. Viz [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md). Pokud nevíte, jak nakonfigurovat zařízení VPN, nebo neznáte rozsahy IP adres v konfiguraci vaší místní sítě, budete se muset spojit s někým, kdo vám s tím pomůže.
* Máte zvenčí veřejnou IP adresu pro vaše zařízení VPN. Tato IP adresa nesmí být umístěná za překladem adres (NAT).

## <a name="part1"></a>Část 1: Konfigurace připojení
1. V prohlížeči přejděte na portál [Azure Portal](http://portal.azure.com) a v případě potřeby se přihlaste pomocí účtu Azure.
2. Klikněte na tlačítko **všechny prostředky** a vyhledejte vaše **brány virtuální sítě** ze seznamu prostředků a klikněte na něj.
3. Na **Brána virtuální sítě** okně klikněte na tlačítko **připojení**.
   
    ![Okno Připojení](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Okno Připojení")<br>
4. Na **připojení** okně klikněte na tlačítko **+ přidat**.
   
    ![Tlačítko Přidat připojení](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "tlačítko Přidat připojení")<br>
5. Na **přidat připojení** okno, výplň na následující pole:
   
   * **Název:** název, kterou chcete přiřadit k lokalitě při vytváření připojení k.
   * **Typ připojení:** vyberte **Site-to-site (IPsec)**.
     
     ![Přidat připojení okno](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "okno Přidat připojení")<br>

## <a name="part2"></a>Část 2 – přidání brány místní sítě
1. Klikněte na tlačítko **brány místní sítě** ***zvolit bránu místní sítě***. Otevře se **brány místní sítě vyberte** okno.
   
    ![Zvolte bránu místní sítě](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "vyberte bránu místní sítě")<br>
2. Klikněte na tlačítko **vytvořit nový** otevřete **vytvořit bránu místní sítě** okno.
   
    ![Okno brány místní sítě vytvořit](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "vytvořit bránu místní sítě")<br>
3. Na **vytvořit bránu místní sítě** okno, výplň na následující pole:
   
   * **Název:** název chcete poskytnout prostředku brány místní sítě.
   * **IP adresa:** veřejnou IP adresu v lokalitě, kterou chcete připojit k zařízení VPN.
   * **Adresní prostor:** adresní prostor, který chcete směrovat do nové lokality místní sítě.
4. Klikněte na tlačítko **OK** na **vytvořit bránu místní sítě** okno a uložte změny.

## <a name="part3"></a>Část 3 – přidat sdílený klíč a vytvoření připojení
1. Na **přidat připojení** okně Přidat sdílený klíč, který chcete použít k vytvoření připojení. Můžete získat sdílený klíč z vašeho zařízení VPN, nebo si ho vytvořit tady a pak nakonfigurovat zařízení VPN použít stejný sdílený klíč. Důležité je, že klíče se přesně shodují.
   
    ![Sdílený klíč](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Sdílený klíč")<br>
2. V dolní části okna klikněte na tlačítko **OK** k vytvoření připojení.

## <a name="part4"></a>Součástí 4 – ověření připojení k síti VPN


[!INCLUDE [vpn-gateway-verify-connection-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="next-steps"></a>Další kroky

Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače. Další informace najdete v [naučné stezce](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) pro virtuální počítače.
