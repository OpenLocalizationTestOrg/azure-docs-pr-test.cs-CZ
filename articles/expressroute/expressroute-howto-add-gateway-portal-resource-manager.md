---
title: "Přidání tooa brány virtuální sítě VNet pro ExpressRoute: portál: Azure | Microsoft Docs"
description: "Tento článek vás provede přidáním tooan brány virtuální sítě už vytvořený virtuální sítě Resource Manageru pro ExpressRoute."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2017
ms.author: cherylmc
ms.openlocfilehash: 9e922af1f3676eeebc569b57c3ae3a22d4e0b395
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-hello-azure-portal"></a>Konfigurace brány virtuální sítě pro ExpressRoute pomocí hello portálu Azure
> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager – PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Classic – prostředí PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video – portál Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Tento článek vás provede kroky tooadd hello bránu virtuální sítě pro už existující virtuální síť. Tento článek vás provede kroky tooadd hello, přizpůsobit a odebrat bránu virtuální sítě (VNet) pro už existující virtuální síť. Hello kroky pro tuto konfiguraci jsou speciálně určené pro virtuální sítě vytvořené pomocí modelu nasazení Resource Manager hello, který se použije v konfiguraci ExpressRoute. Další informace o brány virtuální sítě a nastavení konfigurace brány ExpressRoute najdete v tématu [o brány virtuální sítě pro ExpressRoute](expressroute-about-virtual-network-gateways.md). 


## <a name="before-beginning"></a>Před zahájením

Hello kroky pro tuto úlohu použijte virtuální sítě na základě hello hodnot v hello následující referenční seznam konfigurace. V našem příkladu kroky jsme pomocí tohoto seznamu. Toouse hello seznamu můžete kopírovat jako odkaz, nahraďte hello hodnoty vlastními.

**Seznam odkazů konfigurace**

* Název virtuální sítě = "TestVNet"
* Virtuální adresní prostor sítě = 192.168.0.0/16
* Název podsítě = "FrontEnd" 
    * Adresního prostoru podsítě = "192.168.1.0/24"
* Skupina prostředků = "TestRG"
* Umístění = "East US"
* Název podsítě brány: "GatewaySubnet" název musí být vždy podsíť brány *GatewaySubnet*.
    * Adresního prostoru podsítě brány = "192.168.200.0/26"
* Název brány = "ERGW"
* Název brány IP = "MyERGWVIP"
* Typ brány = "ExpressRoute" Tento typ je požadovaná konfigurace ExpressRoute.
* Název veřejné IP adresy brány = "MyERGWVIP"

Můžete zobrazit [Video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network) z těchto kroků před zahájením konfigurace.

## <a name="create-hello-gateway-subnet"></a>Vytvořit podsíť brány hello

1. V hello [portál](http://portal.azure.com), přejděte toohello Resource Manager virtuální sítě, pro které chcete toocreate bránu virtuální sítě.
2. V hello **nastavení** části okně vaší virtuální sítě, klikněte na tlačítko **podsítě** okno podsítě tooexpand hello.
3. Na hello **podsítě** okně klikněte na tlačítko **+ podsíť brány** tooopen hello **přidat podsíť** okno. 
   
    ![Přidání podsítě brány hello](./media/expressroute-howto-add-gateway-portal-resource-manager/addgwsubnet.png "přidat podsíť brány hello")


4. Hello **název** pro podsíť je automaticky vyplněno hello hodnotu "GatewaySubnet". Tato hodnota je nutná pro podsíť hello Azure toorecognize jako hello podsíť brány. Upravit hello automaticky vyplněné **rozsahu adres** hodnoty toomatch vaše požadavky na konfiguraci. Doporučujeme vytvořit podsíť brány/27 nebo větší (/ 26, / 25 atd.). Potom klikněte na **OK** toosave hello hodnoty a vytvořit podsíť brány hello.

    ![Přidání podsítě hello](./media/expressroute-howto-add-gateway-portal-resource-manager/addsubnetgw.png "přidání podsítě hello")

## <a name="create-hello-virtual-network-gateway"></a>Vytvoření brány virtuální sítě hello

1. Hello portálu na levé straně hello, klikněte na tlačítko  **+**  a zadejte brány virtuální sítě v hledání. Vyhledejte **Brána virtuální sítě** v hello vyhledávání vrátí a klikněte na položku hello. Na hello **Brána virtuální sítě** okně klikněte na tlačítko **vytvořit** v hello dolní části okna hello. Tím se otevře hello **vytvořit bránu virtuální sítě** okno.
2. Na hello **vytvořit bránu virtuální sítě** okno vyplňte hello hodnoty pro bránu virtuální sítě.

    ![Pole v okně Vytvořit bránu virtuální sítě](./media/expressroute-howto-add-gateway-portal-resource-manager/gw.png "Pole v okně Vytvořit bránu virtuální sítě")
3. **Název**: Zadejte pro bránu název. Toto není hello stejné jako podsítě brány. To je hello název objektu hello brány, kterou vytváříte.
4. **Typ brány**: vyberte **ExpressRoute**.
5. **Skladová položka**: skladová položka brány hello vyberte z rozevíracího seznamu hello.
6. **Umístění**: Upravit hello **umístění** pole toopoint toohello umístění, kde se nachází virtuální sítě. Umístění hello neukazuje toohello oblasti, kde je umístěn virtuální sítě, virtuální sítě hello nezobrazí v rozevíracím seznamu "Zvolte virtuální síť" hello.
7. Vyberte virtuální síť toowhich hello tooadd chcete tuto bránu. Klikněte na tlačítko **virtuální síť** tooopen hello **vyberte virtuální síť** okno. Vyberte hello virtuální sítě. Pokud nevidíte virtuální síť, ujistěte se, zda text hello **umístění** pole ukazovat toohello oblast, ve kterém se nachází virtuální sítě.
9. Zvolte veřejnou IP adresu. Klikněte na tlačítko **veřejnou IP adresu** tooopen hello **zvolte veřejnou IP adresu** okno. Klikněte na tlačítko **+ vytvořit nový** tooopen hello **vytvořit veřejnou IP adresu okno**. Zadejte název veřejné IP adresy. Toto okno vytvoří veřejné toowhich IP adresu objektu, bude dynamicky přiřadit veřejnou IP adresu. Klikněte na tlačítko **OK** toosave okno toothis vaše změny.
10. **Předplatné**: Ověřte, že hello správný, Vybrat předplatné.
11. **Skupina prostředků**: Toto nastavení je dáno hello virtuální síť, kterou vyberete.
12. Neupravovat hello **umístění** po zadání hello předchozí nastavení.
13. Ověřte nastavení hello. Pokud chcete, aby vaše tooappear brány na řídicím panelu hello, můžete si vybrat **Pin toodashboard** v hello dolní části okna hello.
14. Klikněte na tlačítko **vytvořit** toobegin vytváření hello brány. ověření nastavení Hello a nasadí hello brány. Vytvoření brány virtuální sítě může trvat až toocomplete too45 minut.

## <a name="next-steps"></a>Další kroky
Po vytvoření brány virtuální sítě hello, můžete se propojit vaší virtuální sítě tooan okruh ExpressRoute. V tématu [propojení virtuální sítě tooan okruh ExpressRoute](expressroute-howto-linkvnet-portal-resource-manager.md).
