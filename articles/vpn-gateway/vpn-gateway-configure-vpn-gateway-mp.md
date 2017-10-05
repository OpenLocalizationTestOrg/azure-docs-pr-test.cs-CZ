---
title: "Konfigurovat bránu VPN: portál classic: Azure | Microsoft Docs"
description: "Tento článek vás provede konfiguraci virtuální sítě VPN gateway a změna brány směrování typ sítě VPN. Tento postup platí pro model nasazení classic a portálu classic."
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
ms.openlocfilehash: 2ea4e6bb86b1ba6f7b501b193d0713d3901457af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-vpn-gateway-in-the-classic-portal"></a>Konfigurovat bránu VPN na portálu classic 
Pokud chcete vytvořit zabezpečený mezi různými místy připojení mezi Azure a vaše místní umístění, budete muset vytvořit bránu virtuální sítě. Brána sítě VPN je konkrétní typ brány virtuální sítě. V modelu nasazení classic, brána sítě VPN může být jeden ze dvou typů směrování sítě VPN: statické nebo dynamické. Typ sítě VPN, který zvolíte, závisí na váš plán návrhu sítě i místní zařízení VPN, které chcete použít. Další informace o zařízeních VPN najdete v tématu [o zařízeních VPN](vpn-gateway-about-vpn-devices.md).

**O modelech nasazení Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-overview"></a>Přehled konfigurace
Následující postup vás provede procesem konfigurace vaší brány sítě VPN na portálu classic. Tento postup platí pro brány pro virtuální sítě, které byly vytvořené pomocí modelu nasazení classic. Ne všechny nastavení konfigurace pro brány v současné době jsou k dispozici na portálu Azure. Když jsou, vytvoříme novou sadu pokynů, které se vztahují k portálu Azure.

### <a name="before-you-begin"></a>Než začnete
Než začnete konfigurovat bránu, musíte nejprve vytvořit virtuální síť. Pokyny k vytvoření virtuální sítě pro připojení mezi různými místy, najdete v části [konfigurace virtuální sítě pomocí připojení site-to-site VPN](vpn-gateway-site-to-site-create.md), nebo [konfigurace virtuální sítě pomocí připojení VPN point-to-site](vpn-gateway-point-to-site-create.md). Pak použijte následující kroky konfigurace brány sítě VPN a shromáždit informace, které je nutné nakonfigurovat zařízení VPN. 

Pokud již máte bránu VPN a chcete změnit typ směrování sítě VPN, najdete v článku [jak změnit typ směrování sítě VPN pro bránu](#how-to-change-the-vpn-routing-type-for-your-gateway).

## <a name="create-a-vpn-gateway"></a>Vytvořit bránu VPN
1. V [portál Azure classic](https://manage.windowsazure.com)na **sítě** ověřte, že je u virtuální sítě ve sloupci Stav **vytvořená**.
2. V **název** sloupce, klikněte na název virtuální sítě.
3. Na **řídicí panel** stránky, Všimněte si, že tato virtuální síť nemá brána ještě nakonfigurovaná. Tento stav se zobrazí, jak projít kroky konfigurace vaší brány.

![Brána nebyla vytvořena](./media/vpn-gateway-configure-vpn-gateway-mp/IC717025.png)

Potom v dolní části stránky klikněte na tlačítko **vytvořit bránu**. Můžete vybrat, zda *statické směrování* nebo *dynamické směrování*. Typ směrování sítě VPN, který jste vybrali, závisí na několika faktorech. Například co podporuje vaše zařízení VPN a jestli potřebujete pro podporu připojení point-to-site. Zkontrolujte [o zařízeních VPN pro připojení k virtuální síti](vpn-gateway-about-vpn-devices.md) ověření směrování typ sítě VPN, které potřebujete. Po vytvoření brány, nemůžete změnit mezi typy směrování brány VPN bez odstranit a znovu vytvořit bránu. Když v systému vás vyzve k potvrzení, že chcete bránu vytvořit, klikněte na tlačítko **Ano**.

![Typ směrování brány sítě VPN](./media/vpn-gateway-configure-vpn-gateway-mp/IC717026.png)

Při vytváření je bránu, Všimněte si obrázek brány na stránce se změní na žlutý a uvádí *vytváření brány*. To může trvat až 45 minut pro vytvoření brány. Počkejte, dokud brány dokončení, než budete pokračovat dál s jiným nastavením konfigurace.

![Vytvoření brány](./media/vpn-gateway-configure-vpn-gateway-mp/IC717027.png)

Když změny brány *připojení*, můžete shromáždit informace budete potřebovat pro vaše zařízení VPN.

![Připojení brány](./media/vpn-gateway-configure-vpn-gateway-mp/IC717028.png)

## <a name="site-to-site-connections"></a>Připojení typu Site-to-Site

### <a name="step-1-gather-information-for-your-vpn-device-configuration"></a>Krok 1. Shromážděte informace pro konfiguraci zařízení VPN
Pokud vytváříte připojení Site-to-Site, po vytvoření brány, shromážděte informace o konfiguraci zařízení VPN. Tyto informace se nachází na **řídicí panel** stránky pro vaši virtuální síť:

1. **IP adresa brány -** IP adresu můžete najít na **řídicí panel** stránky. Nebudete moci zobrazit jeho až po dokončení vytvoření brány.
2. **Sdílený klíč -** klikněte na tlačítko **Správa klíče** v dolní části obrazovky. Klikněte na ikonu vedle klíč zkopírujte jej do schránky a potom vložte a uložte klíč. Toto tlačítko je funkční pouze po jednom tunel S2S VPN. Pokud máte více tunelů S2S VPN, použijte *získat virtuální sítě sdílený klíč brány* rozhraní API nebo rutiny prostředí PowerShell.

![Spravovat klíč](./media/vpn-gateway-configure-vpn-gateway-mp/IC717029.png)

### <a name="step-2--configure-your-vpn-device"></a>Krok 2.  Konfigurace zařízení VPN
Připojení Site-to-Site k místní síti vyžadují zařízení VPN. Protože neposkytujeme kroky konfigurace pro všechna zařízení VPN, můžete najít užitečné informace v následujících odkazech:

- Další informace o kompatibilních zařízeních VPN najdete v tématu s popisem [zařízení VPN](vpn-gateway-about-vpn-devices.md). 
- Odkazy na nastavení konfigurace zařízení najdete v popisu [ověřených zařízení VPN](vpn-gateway-about-vpn-devices.md#devicetable). Při poskytování těchto odkazů se snažíme maximálně vyhovět. Vždycky je nejlepší obrátit se na výrobce zařízení a vyžádat si nejnovější informace o konfiguraci.
- Informace o úpravách ukázek konfigurace zařízení najdete v tématu popisujícím [úpravy ukázek](vpn-gateway-about-vpn-devices.md#editing).
- Parametry protokolu IPsec/IKE najdete v popisu [parametrů](vpn-gateway-about-vpn-devices.md#ipsec).
- Před konfigurací zařízení VPN zkontrolujte [známé problémy s kompatibilitou zařízení](vpn-gateway-about-vpn-devices.md#known) pro zařízení VPN, které chcete použít.

Při konfiguraci zařízení VPN budete potřebovat následující položky:

- Veřejnou IP adresu vaší brány virtuální sítě. Najdete ji tak, že přejdete do okna **Přehled** pro vaši virtuální síť.
- Sdílený klíč. Jedná se o stejný sdílený klíč, který zadáváte při vytváření připojení VPN Site-to-Site. V našich ukázkách používáme velmi základní sdílený klíč. Pro použití byste měli generovat složitější klíč.

Po konfiguraci zařízení VPN můžete zobrazit vaše aktualizované informace připojení na stránce řídicího panelu pro vaši virtuální síť.

### <a name="step-3-verify-your-local-network-ranges-and-vpn-gateway-ip-address"></a>Krok 3. Ověřte rozsahy adres místní sítě a IP adresu brány VPN
#### <a name="verify-your-vpn-gateway-ip-address"></a>Ověřte, IP adresa brány VPN
Pro bránu pro připojení správně musí být IP adresu pro vaše zařízení VPN správně nakonfigurován pro místní síť, kterou jste zadali pro konfiguraci mezi různými místy. To je obvykle konfigurovaná během procesu konfigurace site-to-site. Pokud jste dřív použili tento místní sítě s jiného zařízení nebo došlo ke změně IP adresu pro tuto místní síť, ale upravte nastavení a zadejte správnou adresu IP brány.

1. Chcete-li ověřit vaše IP adresa brány, klikněte na tlačítko **sítě** v levém podokně portálu a potom vyberte **místní sítě** v horní části stránky. Zobrazí adresu brány VPN pro každou místní síť, kterou jste vytvořili. Chcete-li upravit IP adresu, vyberte virtuální sítě a klikněte na **upravit** v dolní části stránky.
2. Na **zadejte podrobnosti o vaší místní síti** stránky, upravte IP adresu a klikněte na šipku další v dolní části stránky.
3. Na **zadejte adresní prostor** klikněte na značku zaškrtnutí vpravo dole uložte nastavení.

#### <a name="verify-the-address-ranges-for-your-local-networks"></a>Ověřte rozsahy adres pro místní sítě
Pro správný provoz procházet skrz bránu na vaše místní umístění budete muset ověřte, zda je zadán každý rozsah IP adres. Každý rozsah musí být uvedené v vaší Azure **místní sítě** konfigurace. V závislosti na konfiguraci sítě vaše místní umístění může to být poněkud velkých úloh. Provoz, který je vázaný IP adresu, která je obsažena v seznamu rozsahů odešle přes bránu virtuální sítě VPN. Rozsahy, které uvedete v seznamu nemusí být rozsahy privátních, i když se chcete ověřit, že místní konfigurací může přijímat příchozí provoz.

Můžete přidat nebo upravit nastavení pro místní síť, použijte následující postup:

1. Chcete-li upravit rozsahy IP adres pro místní síť, klikněte na tlačítko **sítě** v levém podokně portálu a potom vyberte **místní sítě** v horní části stránky. Na portálu, je nejjednodušší způsob, jak zobrazit rozsahy, které jste uvedli v **upravit** stránky. Informace o vaší rozsahy, vyberte virtuální sítě a klikněte na **upravit** v dolní části stránky.
2. Na **zadejte podrobnosti o vaší místní síti** stránky, nemusíte nic měnit. Klikněte na šipku další v dolní části stránky.
3. Na **zadejte adresní prostor** vyberte změny adresního prostoru vaší sítě. Klikněte na značku zaškrtnutí uložte konfiguraci.

## <a name="how-to-view-gateway-traffic"></a>Postup zobrazení provoz brány
Můžete zobrazit brány a brány provoz z vaší virtuální sítě **řídicí panel** stránky.

Na **řídicí panel** stránky můžete zobrazit následující:

* Množství dat, která je předávaných mezi bránu, data a data.
* Názvy serverů DNS, které jsou určené pro vaši virtuální síť.
* Připojení mezi bránou a vaše zařízení VPN.
* Sdílený klíč, který slouží ke konfiguraci brány připojení k zařízení VPN.

## <a name="how-to-change-the-vpn-routing-type-for-your-gateway"></a>Postup při změně typu směrování VPN pro bránu
Protože některé konfigurace připojení jsou dostupná jenom pro některé typy směrování brány, můžete zjistit, že budete muset změnit bránu VPN směrování typ existující bránu VPN. Například můžete přidat do již existující připojení site-to-site, který má statická brána připojení point-to-site. Připojení point-to-site vyžadují dynamické brány. To znamená, konfigurace připojení P2S, budete muset změnit bránu směrování typ sítě VPN ze statické na dynamický.

Pokud potřebujete změnit brány směrování typ sítě VPN, budete odstraňte existující bránu a poté vytvořit novou bránu s novým typem směrování. Nemusíte odstranit celý virtuální síť, chcete-li změnit typ směrování brány.

Před změnou bránu směrování typ sítě VPN, nezapomeňte ověřit, jestli vaše zařízení VPN podporuje směrování typ, který chcete použít. Stáhněte si nový směrování ukázky konfigurace a zkontrolujte požadavky na zařízení VPN najdete v tématu [o zařízeních VPN pro připojení k virtuální síti](vpn-gateway-about-vpn-devices.md).

> [!IMPORTANT]
> Pokud odstraníte bránu VPN virtuální sítě, virtuální IP adresy přiřazené k bráně vydání. Při opětovném vytvoření brány, je k němu přiřazen nový virtuální IP adresy.
> 
> 

1. **Odstraňte existující bránu VPN.**
   
    Na **řídicí panel** pro vaši virtuální síť, přejděte do dolní části stránky a klikněte na tlačítko **odstranit bránu**. Počkejte, než pro oznámení, že brány byla odstraněna. Jakmile se zobrazí oznámení na obrazovce odstraněný bránu, můžete vytvořit novou bránu.
2. **Vytvořte novou bránu VPN.**
   
    Vytvořit novou bránu pomocí postupu v horní části stránky: [vytvořit bránu VPN](#create-a-vpn-gateway).

## <a name="next-steps"></a>Další kroky
Můžete přidávat virtuální počítače do svojí virtuální sítě. Viz [Vytvoření vlastního virtuálního počítače](../virtual-machines/windows/classic/createportal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Pokud chcete provést konfiguraci připojení point-to-site VPN, přečtěte si téma [konfigurace připojení typu point-to-site VPN](vpn-gateway-point-to-site-create.md).

