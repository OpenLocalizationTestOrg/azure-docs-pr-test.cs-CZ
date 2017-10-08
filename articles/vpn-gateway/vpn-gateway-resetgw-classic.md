---
title: "Resetování Azure VPN gateway tooreestablish tunelových propojení IPsec | Microsoft Docs"
description: "Tento článek vás provede procesem resetování vašeho tunelových propojení IPsec tooreestablish Azure VPN Gateway. Hello článek se týká bran tooVPN hello classic i hello modelech nasazení Resource Manager."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 79d77cb8-d175-4273-93ac-712d7d45b1fe
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: cherylmc
ms.openlocfilehash: 84dd741f0bebd6b18cb235216a68a88da5fe17b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reset-a-vpn-gateway"></a>Resetování služby VPN Gateway

Resetování brány Azure VPN je užitečné v případě ztráty připojení VPN mezi lokalitami na jednom nebo více tunelech VPN typu Site-to-Site. V této situaci vaše místní zařízení VPN jsou všechny funguje správně, ale nebylo možné tooestablish tunelových propojení IPsec pomocí bran Azure VPN hello. Tento článek vám umožňuje resetování služby VPN gateway.

### <a name="what-happens-during-a-reset"></a>Co se stane během vynulování?

Brána sítě VPN se skládá ze dvou instancí virtuálního počítače spuštěných v konfiguraci aktivní pohotovostní. Když resetujete hello brány, dojde k restartování hello brány a potom znovu použije hello mezi různými místy tooit konfigurace. Hello brány uchovává hello veřejnou IP adresu, které už je. To znamená, že nebudete potřebovat konfiguraci směrovače VPN hello tooupdate s novou veřejnou IP adresu brány Azure VPN.

Pokud vydáte hello příkaz tooreset hello brány, je okamžitě resetuje právě aktivní instance brány Azure VPN hello hello. Během převzetí služeb při selhání hello hello aktivní instance (která se resetuje), toohello pohotovostní instancí bude ke krátké prodlevě. Hello prodleva by neměla být delší než jedna minuta.

Pokud nedojde k obnovení připojení hello po hello první restartování, problém hello stejný příkaz znovu tooreboot hello druhé instance virtuálního počítače (hello nové aktivní brány). Pokud jsou dvě restartování hello požadovaný back tooback, budou existovat trochu delší dobu kde jsou obě instance virtuálního počítače (aktivní a pohotovostní) resetovány. To způsobí delší prodlevu na připojení k síti VPN hello, až minut too4 too2 pro virtuální počítače toocomplete hello restartování.

Po dvou resetováních Pokud stále dochází k potížím připojení mezi různými místy, otevřete prosím žádost o podporu od hello portálu Azure.

## <a name="before"></a>Než začnete

Před bránu, ověřte hello klíčové položky uvedeny pro každý tunelu IPsec Site-to-Site (S2S) VPN. Jakákoli Neshoda v položkách hello způsobí odpojení hello tunelů S2S VPN. Ověřením a opravením konfigurace hello místní a Azure VPN Gateway uloží můžete z zbytečnému resetování a přerušení hello ostatních fungujících připojení na hello brány.

Ověřte hello před resetováním brány následující položky:

* Hello Internet IP adresy (VIP) pro obě brány Azure VPN hello a hello místní brány VPN jsou správně nakonfigurovány v obě zásady hello Azure a hello místní VPN.
* Hello předsdílený klíč musí být hello stejné na Azure a místní brány VPN.
* Pokud použijete určitou konfiguraci protokolu IPsec/IKE, jako je šifrování, hašování algoritmů a PFS (Perfect Forward Secrecy), ujistěte se, jak hello Azure a místní brány VPN mají hello stejné konfigurace.

## <a name="portal"></a>Portál Azure

Můžete resetovat bránu VPN Resource Manageru pomocí hello portálu Azure. Pokud chcete tooreset classic brány, najdete v části hello [prostředí PowerShell](#resetclassic) kroky.

### <a name="resource-manager-deployment-model"></a>Model nasazení Resource Manager

1. Otevřete hello [portál Azure](https://portal.azure.com) a přejděte brány virtuální sítě Resource Manager toohello, které chcete tooreset.
2. V okně hello pro bránu virtuální sítě hello klikněte na tlačítko 'Resetovat'.

  ![Okno resetování brány VPN](./media/vpn-gateway-howto-reset-gateway/reset-vpn-gateway-portal.png)
3. V hello resetovat okno, klikněte na hello **resetovat** tlačítko.

## <a name="ps"></a>Prostředí PowerShell

### <a name="resource-manager-deployment-model"></a>Model nasazení Resource Manager

Hello rutiny pro resetování brány je **resetování AzureRmVirtualNetworkGateway**. Před provedením provést obnovení, ujistěte se, abyste měli nejnovější verzi hello hello [rutiny prostředí PowerShell Resource Manager](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0). Hello následující příklad resetuje bránu virtuální sítě s názvem VNet1GW ve skupině prostředků TestRG1 hello:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw
```

Výsledek:

Jakmile se zobrazí návratový výsledek, můžete předpokládat, resetování brány hello byla úspěšná. Však není nic v hello návratový výsledek, který explicitně určuje, že resetování hello byla úspěšná. Pokud chcete, aby toolook úzce v historii toosee hello přesně, kdy resetuje hello brány došlo k chybě, můžete zobrazit tyto informace v hello [portál Azure](https://portal.azure.com). Hello portálu, přejděte příliš**'GatewayName} -> Stav prostředku**.

### <a name="resetclassic"></a>Model nasazení Classic

Hello rutiny pro resetování brány je **Reset-AzureVNetGateway**. Před provedením provést obnovení, ujistěte se, abyste měli nejnovější verzi hello hello [rutiny prostředí PowerShell služby správy (SM)](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0). Hello následující příklad resetuje hello brány pro virtuální síť s názvem "ContosoVNet":

```powershell
Reset-AzureVNetGateway –VnetName “ContosoVNet”
```

Výsledek:

```powershell
Error          :
HttpStatusCode : OK
Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
Status         : Successful
RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
StatusCode     : OK
```

## <a name="cli"></a>Rozhraní příkazového řádku Azure

tooreset hello brány, použijte hello [resetovat az brány virtuální sítě-](https://docs.microsoft.com/cli/azure/network/vnet-gateway#reset) příkaz. Hello následující příklad resetuje bránu virtuální sítě s názvem VNet5GW ve skupině prostředků TestRG5 hello:

```azurecli
az network vnet-gateway reset -n VNet5GW -g TestRG5
```

Výsledek:

Jakmile se zobrazí návratový výsledek, můžete předpokládat, resetování brány hello byla úspěšná. Však není nic v hello návratový výsledek, který explicitně určuje, že resetování hello byla úspěšná. Pokud chcete, aby toolook úzce v historii toosee hello přesně, kdy resetuje hello brány došlo k chybě, můžete zobrazit tyto informace v hello [portál Azure](https://portal.azure.com). Hello portálu, přejděte příliš**'GatewayName} -> Stav prostředku**.
