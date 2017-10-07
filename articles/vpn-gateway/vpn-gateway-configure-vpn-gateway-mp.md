---
title: "Konfigurovat bránu VPN: portál classic: Azure | Microsoft Docs"
description: "Tento článek vás provede konfiguraci virtuální sítě VPN gateway a změna brány směrování typ sítě VPN. Tento postup platí toohello nasazení classic modelu a hello portálu classic."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: fbe59ba8-b11f-4d21-9bb1-225ec6c6d351
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/04/2017
ms.author: cherylmc
ms.openlocfilehash: 00d2fa18bab6eb24b33ddb18113f2a557db638d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vpn-gateway-in-hello-classic-portal"></a>Konfigurovat bránu VPN portálu classic hello 
Pokud chcete, aby toocreate zabezpečené mezi různými místy připojení mezi Azure a vaše místní umístění, je třeba toocreate bránu virtuální sítě. Brána sítě VPN je konkrétní typ brány virtuální sítě. V modelu nasazení classic hello brány VPN může být jeden ze dvou typů směrování sítě VPN: statické nebo dynamické. Hello typ sítě VPN, který zvolíte, závisí na vaší plánování návrhu sítě i zařízení VPN místní hello chcete toouse. Další informace o zařízeních VPN najdete v tématu [o zařízeních VPN](vpn-gateway-about-vpn-devices.md).

**O modelech nasazení Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-overview"></a>Přehled konfigurace
Hello následující postup vás provede procesem konfigurace vaší brány sítě VPN v portálu classic hello. Tento postup platí toogateways pro virtuální sítě, které byly vytvořené pomocí modelu nasazení classic hello. Ne všechny hello nastavení konfigurace pro brány v současné době jsou k dispozici v hello portálu Azure. Když jsou, vytvoříme novou sadu pokynů, které se vztahují toohello portálu Azure.

### <a name="before-you-begin"></a>Než začnete
Než začnete konfigurovat bránu, musíte nejprve toocreate vaší virtuální sítě. Kroky toocreate virtuální sítě pro připojení mezi různými místy, najdete v části [konfigurace virtuální sítě pomocí připojení site-to-site VPN](vpn-gateway-site-to-site-create.md), nebo [konfigurace virtuální sítě pomocí připojení VPN point-to-site](vpn-gateway-point-to-site-create.md). Pak použijte následující kroky tooconfigure hello VPN gateway hello a shromažďovat hello informace, které budete potřebovat tooconfigure zařízení VPN. 

Pokud již máte bránu VPN a chcete toochange hello typ směrování sítě VPN, přečtěte si téma [jak toochange hello směrování typ sítě VPN pro bránu](#how-to-change-the-vpn-routing-type-for-your-gateway).

## <a name="create-a-vpn-gateway"></a>Vytvořit bránu VPN
1. V hello [portál Azure classic](https://manage.windowsazure.com), na hello **sítě** ověřte, že ve sloupci Stav hello pro vaši virtuální síť je **vytvořená**.
2. V hello **název** sloupce, klikněte na název hello virtuální sítě.
3. Na hello **řídicí panel** stránky, Všimněte si, že tato virtuální síť nemá brána ještě nakonfigurovaná. Tento stav se zobrazí, jak projít kroky tooconfigure hello bránu.

![Brána nebyla vytvořena](./media/vpn-gateway-configure-vpn-gateway-mp/IC717025.png)

V dalším kroku v hello dolní části stránky hello, klikněte na položku **vytvořit bránu**. Můžete vybrat, zda *statické směrování* nebo *dynamické směrování*. Hello směrování typ sítě VPN, kterou vyberete, závisí na několika faktorech. Například co podporuje vaše zařízení VPN a jestli je potřeba toosupport připojení point-to-site. Zkontrolujte [o zařízeních VPN pro připojení k virtuální síti](vpn-gateway-about-vpn-devices.md) tooverify hello směrování typ sítě VPN, které potřebujete. Po vytvoření brány hello nemůžete změnit mezi typy směrování brány VPN bez odstranit a znovu vytvořit bránu hello. Když hello systém zobrazí výzvu tooconfirm, který chcete hello bránu vytvořit, klikněte na tlačítko **Ano**.

![Typ směrování brány sítě VPN](./media/vpn-gateway-configure-vpn-gateway-mp/IC717026.png)

Při vytváření je bránu, Všimněte si hello brány obrázku na stránce hello změny tooyellow a uvádí *vytváření brány*. To může trvat až minut too45 toocreate hello brány. Počkejte, dokud hello brány dokončení, než budete pokračovat dál s jiným nastavením konfigurace.

![Vytvoření brány](./media/vpn-gateway-configure-vpn-gateway-mp/IC717027.png)

Když příliš hello změny brány*připojení*, můžete shromáždit informace hello budete potřebovat pro vaše zařízení VPN.

![Připojení brány](./media/vpn-gateway-configure-vpn-gateway-mp/IC717028.png)

## <a name="site-to-site-connections"></a>Připojení typu Site-to-Site

### <a name="step-1-gather-information-for-your-vpn-device-configuration"></a>Krok 1. Shromážděte informace pro konfiguraci zařízení VPN
Po vytvoření brány hello při vytváření připojení Site-to-Site, shromážděte informace o konfiguraci zařízení VPN. Tyto informace se nachází na hello **řídicí panel** stránky pro vaši virtuální síť:

1. **IP adresa brány -** hello IP adresu můžete najít na hello **řídicí panel** stránky. Nebudete moct toosee se až po vaší brány dokončí vytváření.
2. **Sdílený klíč -** klikněte na tlačítko **Správa klíče** v hello dolní části obrazovky hello. Klikněte na tlačítko hello ikonu další toohello klíče toocopy ho tooyour schránky a vložte a uložit klíč hello. Toto tlačítko je funkční pouze po jednom tunel S2S VPN. Pokud máte více tunelů S2S VPN, použijte hello *získat virtuální sítě sdílený klíč brány* rozhraní API nebo rutiny prostředí PowerShell.

![Spravovat klíč](./media/vpn-gateway-configure-vpn-gateway-mp/IC717029.png)

### <a name="step-2--configure-your-vpn-device"></a>Krok 2.  Konfigurace zařízení VPN
Připojení Site-to-Site tooan do místní sítě vyžaduje zařízení VPN. Když jsme neposkytují kroky konfigurace pro všechna zařízení VPN, možná v hello následující odkazy, které jsou užitečné informace hello:

- Další informace o kompatibilních zařízeních VPN najdete v tématu s popisem [zařízení VPN](vpn-gateway-about-vpn-devices.md). 
- Nastavení konfigurace toodevice odkazy, najdete v části [ověřit zařízení VPN](vpn-gateway-about-vpn-devices.md#devicetable). Při poskytování těchto odkazů se snažíme maximálně vyhovět. Vždycky je nejlepší toocheck se na výrobce zařízení hello nejnovější informace o konfiguraci.
- Informace o úpravách ukázek konfigurace zařízení najdete v tématu popisujícím [úpravy ukázek](vpn-gateway-about-vpn-devices.md#editing).
- Parametry protokolu IPsec/IKE najdete v popisu [parametrů](vpn-gateway-about-vpn-devices.md#ipsec).
- Před konfigurací zařízení VPN, zkontrolujte pro žádné [známé problémy s kompatibilitou zařízení](vpn-gateway-about-vpn-devices.md#known) pro hello zařízení VPN, které chcete toouse.

Při konfiguraci zařízení VPN, budete potřebovat hello následující položky:

- Hello veřejnou IP adresu brány virtuální sítě. To můžete najít ve budete toohello **přehled** okno pro vaši virtuální síť.
- Sdílený klíč. To je hello stejný sdílený klíč, který zadáte při vytváření připojení Site-to-Site VPN. V našich ukázkách používáme velmi základní sdílený klíč. Složitější klíče toouse měl generovat.

Po hello zařízení VPN je nakonfigurovaný, můžete zobrazit vaše aktualizované informace připojení na stránce řídicího panelu hello sítě vnet.

### <a name="step-3-verify-your-local-network-ranges-and-vpn-gateway-ip-address"></a>Krok 3. Ověřte rozsahy adres místní sítě a IP adresu brány VPN
#### <a name="verify-your-vpn-gateway-ip-address"></a>Ověřte, IP adresa brány VPN
Pro tooconnect brány správně, hello IP adresu pro vaše zařízení VPN musí být správně nakonfigurován pro hello místní síť, kterou jste zadali pro konfiguraci mezi různými místy. To je obvykle konfigurovaná během procesu konfigurace site-to-site hello. Pokud jste dřív použili tento místní sítě s jiného zařízení nebo došlo ke změně hello IP adresu pro tuto místní síť, ale upravte hello nastavení toospecify hello správná IP adresa brány.

1. tooverify IP adresu brány, klikněte na tlačítko **sítě** hello levém podokně portálu a pak vyberte **místní sítě** hello horní části stránky hello. Uvidíte hello adresu brány sítě VPN pro každou místní síť, kterou jste vytvořili. tooedit hello IP adresu, vyberte hello virtuální sítě a klikněte na **upravit** v hello dolní části stránky hello.
2. Na hello **zadejte podrobnosti o vaší místní síti** stránky, upravit hello IP adresu a potom klikněte na šipku další hello v hello dolní části stránky hello.
3. Na hello **zadejte hello adresní prostor** klikněte na značku zaškrtnutí hello v hello nižší správné toosave nastavení.

#### <a name="verify-hello-address-ranges-for-your-local-networks"></a>Ověřte hello rozsahy adres pro místní sítě
Pro správný provoz tooflow hello prostřednictvím hello brány tooyour místní umístění budete potřebovat tooverify, zda je zadán každý rozsah IP adres. Každý rozsah musí být uvedené v vaší Azure **místní sítě** konfigurace. V závislosti na konfiguraci sítě hello vaše místní umístění může to být poněkud velkých úloh. Provoz, který je vázaný IP adresu, která je obsažena v hello uvedené rozsahy budou odeslány prostřednictvím brány VPN hello virtuální sítě. Hello rozsahy, uvedete v seznamu nemají rozsahy privátních toobe, i když budete chtít tooverify, který může přijímat konfiguraci místní hello příchozí přenosy.

hello rozsahy tooadd nebo upravit v místní síti, použijte hello následující kroky:

1. rozsahy tooedit hello IP adres pro místní síť, klikněte na tlačítko **sítě** hello levém podokně portálu a pak vyberte **místní sítě** hello horní části stránky hello. Hello portálu, je hello nejjednodušší způsob, jak tooview hello rozsahy, které jste uvedli v hello **upravit** stránky. toosee rozsahy, vyberte hello virtuální sítě a klikněte na tlačítko **upravit** v hello dolní části stránky hello.
2. Na hello **zadejte podrobnosti o vaší místní síti** stránky, nemusíte nic měnit. Klikněte na šipku další hello v hello dolní části stránky hello.
3. Na hello **zadejte hello adresní prostor** vyberte změny adresního prostoru vaší sítě. Klikněte na tlačítko zaškrtnutí toosave hello konfiguraci.

## <a name="how-tooview-gateway-traffic"></a>Jak přenosy tooview brány
Můžete zobrazit brány a brány provoz z vaší virtuální sítě **řídicí panel** stránky.

Na hello **řídicí panel** stránky můžete zobrazit následující hello:

* Hello množství dat, která je předávaných mezi bránu, data a data.
* názvy Hello hello serverů DNS, které jsou určené pro vaši virtuální síť.
* Hello připojení mezi bránou a vaše zařízení VPN.
* Hello sdílený klíč, který je použité tooconfigure zařízení brány tooyour připojení VPN.

## <a name="how-toochange-hello-vpn-routing-type-for-your-gateway"></a>Jak toochange hello směrování typ sítě VPN pro bránu
Protože některé konfigurace připojení jsou dostupná jenom pro některé typy směrování brány, můžete zjistit, je nutné, aby toochange hello brány VPN směrování typ existující bránu VPN. Například můžete tooadd připojení point-to-site tooan stávající site-to-site připojení, které má statické bránu. Připojení point-to-site vyžadují dynamické brány. To znamená tooconfigure připojení P2S, máte toochange bránu směrování typ sítě VPN ze statické toodynamic.

Pokud potřebujete toochange brány směrování typ sítě VPN, budete odstranit existující bránu hello a poté vytvořit novou bránu s hello nový typ směrování. Nepotřebujete toodelete hello celý toochange hello typ brány virtuální sítě směrování.

Před změnou bránu směrování typ sítě VPN, být jisti tooverify, že vaše zařízení VPN podporuje hello typ směrování, které chcete toouse. Ukázky nové konfigurace směrování toodownload a zkontrolujte požadavky na zařízení VPN, najdete v části [o zařízeních VPN pro připojení k virtuální síti](vpn-gateway-about-vpn-devices.md).

> [!IMPORTANT]
> Pokud odstraníte bránu virtuální sítě VPN, vydání hello VIP přiřazené toohello brány. Pokud je znovu vytvořit bránu hello, nový virtuální IP adresy přiřazené tooit.
> 
> 

1. **Odstraňte existující bránu VPN hello.**
   
    Na hello **řídicí panel** pro vaši virtuální síť, přejděte toohello dolní části stránky hello a klikněte na tlačítko **odstranit bránu**. Počkejte, než hello oznámení, že hello brány byla odstraněna. Jakmile se zobrazí oznámení hello na úvodní obrazovka odstraněný bránu, můžete vytvořit novou bránu.
2. **Vytvořte novou bránu VPN.**
   
    Pomocí postupu hello hello horní části stránky toocreate hello novou bránu: [vytvořit bránu VPN](#create-a-vpn-gateway).

## <a name="next-steps"></a>Další kroky
Můžete přidat virtuální počítače tooyour virtuální sítě. V tématu [jak toocreate vlastního virtuálního počítače](../virtual-machines/windows/classic/createportal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Pokud chcete připojení tooconfigure point-to-site VPN, najdete v části [konfigurace připojení typu point-to-site VPN](vpn-gateway-point-to-site-create.md).

