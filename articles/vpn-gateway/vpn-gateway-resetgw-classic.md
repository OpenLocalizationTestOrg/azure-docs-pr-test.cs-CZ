---
title: "Resetování služby Azure VPN gateway k opětovnému zavedení tunelových propojení IPsec | Microsoft Docs"
description: "Tento článek vás provede procesem resetování služby Azure VPN Gateway k opětovnému zavedení tunelových propojení IPsec. Článek se týká bran VPN jak classic a modelem nasazení Resource Manager."
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
ms.openlocfilehash: 7c5ba9310568571991708ab54a5275df6ea84a39
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="reset-a-vpn-gateway"></a><span data-ttu-id="ad972-104">Resetování služby VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="ad972-104">Reset a VPN Gateway</span></span>

<span data-ttu-id="ad972-105">Resetování brány Azure VPN je užitečné v případě ztráty připojení VPN mezi lokalitami na jednom nebo více tunelech VPN typu Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="ad972-105">Resetting an Azure VPN gateway is helpful if you lose cross-premises VPN connectivity on one or more Site-to-Site VPN tunnels.</span></span> <span data-ttu-id="ad972-106">V této situaci vaše místní zařízení VPN fungují správně, ale nejsou schopná vytvořit tunelová propojení prostřednictvím protokolu IPsec s branami Azure VPN.</span><span class="sxs-lookup"><span data-stu-id="ad972-106">In this situation, your on-premises VPN devices are all working correctly, but are not able to establish IPsec tunnels with the Azure VPN gateways.</span></span> <span data-ttu-id="ad972-107">Tento článek vám umožňuje resetování služby VPN gateway.</span><span class="sxs-lookup"><span data-stu-id="ad972-107">This article helps you reset your VPN gateway.</span></span>

### <a name="what-happens-during-a-reset"></a><span data-ttu-id="ad972-108">Co se stane během vynulování?</span><span class="sxs-lookup"><span data-stu-id="ad972-108">What happens during a reset?</span></span>

<span data-ttu-id="ad972-109">Brána sítě VPN se skládá ze dvou instancí virtuálního počítače spuštěných v konfiguraci aktivní pohotovostní.</span><span class="sxs-lookup"><span data-stu-id="ad972-109">A VPN gateway is composed of two VM instances running in an active-standby configuration.</span></span> <span data-ttu-id="ad972-110">Při resetování brány, brána se restartuje a pak znovu použije konfigurace mezi různými místy k němu.</span><span class="sxs-lookup"><span data-stu-id="ad972-110">When you reset the gateway, it reboots the gateway, and then reapplies the cross-premises configurations to it.</span></span> <span data-ttu-id="ad972-111">Brána si ponechá stejnou veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="ad972-111">The gateway keeps the public IP address it already has.</span></span> <span data-ttu-id="ad972-112">To znamená, že nebudete muset aktualizovat konfiguraci směrovače VPN na novou veřejnou IP adresu brány Azure VPN.</span><span class="sxs-lookup"><span data-stu-id="ad972-112">This means you won’t need to update the VPN router configuration with a new public IP address for Azure VPN gateway.</span></span>

<span data-ttu-id="ad972-113">Pokud vydáte příkaz k resetování brány, se okamžitě resetuje právě aktivní instance brány Azure VPN.</span><span class="sxs-lookup"><span data-stu-id="ad972-113">When you issue the command to reset the gateway, the current active instance of the Azure VPN gateway is rebooted immediately.</span></span> <span data-ttu-id="ad972-114">Při selhání pohotovostní instancí od aktivní instance (která se resetuje), bude ke krátké prodlevě.</span><span class="sxs-lookup"><span data-stu-id="ad972-114">There will be a brief gap during the failover from the active instance (being rebooted), to the standby instance.</span></span> <span data-ttu-id="ad972-115">Prodleva by neměla být delší než jedna minuta.</span><span class="sxs-lookup"><span data-stu-id="ad972-115">The gap should be less than one minute.</span></span>

<span data-ttu-id="ad972-116">Pokud po prvním resetování nedojde k obnovení připojení, znovu vydejte stejný příkaz pro resetování druhé instance virtuálního počítače (nové aktivní brány).</span><span class="sxs-lookup"><span data-stu-id="ad972-116">If the connection is not restored after the first reboot, issue the same command again to reboot the second VM instance (the new active gateway).</span></span> <span data-ttu-id="ad972-117">Pokud se vyžadují dvě restartování za sebou, bude doba, po kterou se obě instance virtuálního počítače (aktivní a pohotovostní) restartují, trochu delší.</span><span class="sxs-lookup"><span data-stu-id="ad972-117">If the two reboots are requested back to back, there will be a slightly longer period where both VM instances (active and standby) are being rebooted.</span></span> <span data-ttu-id="ad972-118">To způsobí delší prodlevu propojování VPN – 2 až 4 minuty, než se dokončí resetování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ad972-118">This will cause a longer gap on the VPN connectivity, up to 2 to 4 minutes for VMs to complete the reboots.</span></span>

<span data-ttu-id="ad972-119">Po dvou resetováních Pokud stále dochází k potížím připojení mezi různými místy, otevřete prosím žádost o podporu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ad972-119">After two reboots, if you are still experiencing cross-premises connectivity problems, please open a support request from the Azure portal.</span></span>

## <span data-ttu-id="ad972-120"><a name="before"></a>Než začnete</span><span class="sxs-lookup"><span data-stu-id="ad972-120"><a name="before"></a>Before you begin</span></span>

<span data-ttu-id="ad972-121">Před restartováním brány ověřte pro každé tunelové propojení VPN typu Site-to-Site (S2S) s protokolem IPsec klíčové body následujícího seznamu.</span><span class="sxs-lookup"><span data-stu-id="ad972-121">Before you reset your gateway, verify the key items listed below for each IPsec Site-to-Site (S2S) VPN tunnel.</span></span> <span data-ttu-id="ad972-122">Jakákoli neshoda v těchto bodech způsobí odpojení tunelových propojení VPN typu S2S.</span><span class="sxs-lookup"><span data-stu-id="ad972-122">Any mismatch in the items will result in the disconnect of S2S VPN tunnels.</span></span> <span data-ttu-id="ad972-123">Ověřením a opravením konfigurace pro místní a brány Azure VPN uloží můžete z zbytečnému resetování a přerušení ostatních fungujících připojení na brány.</span><span class="sxs-lookup"><span data-stu-id="ad972-123">Verifying and correcting the configurations for your on-premises and Azure VPN gateways saves you from unnecessary reboots and disruptions for the other working connections on the gateways.</span></span>

<span data-ttu-id="ad972-124">Před resetováním brány ověřte následující položky:</span><span class="sxs-lookup"><span data-stu-id="ad972-124">Verify the following items before resetting your gateway:</span></span>

* <span data-ttu-id="ad972-125">Internetové IP adresy (virtuální IP adresy) pro bránu Azure VPN a bránu místní VPN jsou správně nakonfigurovány v zásadách Azure VPN i v zásadách místní VPN.</span><span class="sxs-lookup"><span data-stu-id="ad972-125">The Internet IP addresses (VIPs) for both the Azure VPN gateway and the on-premises VPN gateway are configured correctly in both the Azure and the on-premises VPN policies.</span></span>
* <span data-ttu-id="ad972-126">Předsdílený klíč musí být stejný v bráně Azure VPN i v bráně místní VPN.</span><span class="sxs-lookup"><span data-stu-id="ad972-126">The pre-shared key must be the same on both Azure and on-premises VPN gateways.</span></span>
* <span data-ttu-id="ad972-127">Pokud použijete určitou konfiguraci protokolu IPsec/IKE, jako například šifrování, algoritmy hash nebo metodu Perfect Forward Secrecy (PFS), ujistěte se, že je stejně nakonfigurovaná brána Azure VPN i brána místní sítě.</span><span class="sxs-lookup"><span data-stu-id="ad972-127">If you apply specific IPsec/IKE configuration, such as encryption, hashing algorithms, and PFS (Perfect Forward Secrecy), ensure both the Azure and on-premises VPN gateways have the same configurations.</span></span>

## <span data-ttu-id="ad972-128"><a name="portal"></a>Portál Azure</span><span class="sxs-lookup"><span data-stu-id="ad972-128"><a name="portal"></a>Azure portal</span></span>

<span data-ttu-id="ad972-129">Můžete resetovat bránu VPN Resource Manageru pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ad972-129">You can reset a Resource Manager VPN gateway using the Azure portal.</span></span> <span data-ttu-id="ad972-130">Pokud chcete resetovat bránu classic, přečtěte si téma [prostředí PowerShell](#resetclassic) kroky.</span><span class="sxs-lookup"><span data-stu-id="ad972-130">If you want to reset a classic gateway, see the [PowerShell](#resetclassic) steps.</span></span>

### <a name="resource-manager-deployment-model"></a><span data-ttu-id="ad972-131">Model nasazení Resource Manager</span><span class="sxs-lookup"><span data-stu-id="ad972-131">Resource Manager deployment model</span></span>

1. <span data-ttu-id="ad972-132">Otevřete [portál Azure](https://portal.azure.com) a přejděte na bránu virtuální sítě Resource Manager, který chcete obnovit.</span><span class="sxs-lookup"><span data-stu-id="ad972-132">Open the [Azure portal](https://portal.azure.com) and navigate to the Resource Manager virtual network gateway that you want to reset.</span></span>
2. <span data-ttu-id="ad972-133">V okně pro bránu virtuální sítě klikněte na 'Resetovat'.</span><span class="sxs-lookup"><span data-stu-id="ad972-133">On the blade for the virtual network gateway, click 'Reset'.</span></span>

  ![Okno resetování brány VPN](./media/vpn-gateway-howto-reset-gateway/reset-vpn-gateway-portal.png)
3. <span data-ttu-id="ad972-135">V okně resetování klikněte na **resetovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ad972-135">On the Reset blade, click the **Reset** button.</span></span>

## <span data-ttu-id="ad972-136"><a name="ps"></a>Prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ad972-136"><a name="ps"></a>PowerShell</span></span>

### <a name="resource-manager-deployment-model"></a><span data-ttu-id="ad972-137">Model nasazení Resource Manager</span><span class="sxs-lookup"><span data-stu-id="ad972-137">Resource Manager deployment model</span></span>

<span data-ttu-id="ad972-138">Rutina pro resetování brány je **resetování AzureRmVirtualNetworkGateway**.</span><span class="sxs-lookup"><span data-stu-id="ad972-138">The cmdlet for resetting a gateway is **Reset-AzureRmVirtualNetworkGateway**.</span></span> <span data-ttu-id="ad972-139">Před provedením provést obnovení, ujistěte se, abyste měli nejnovější verzi [rutiny prostředí PowerShell Resource Manager](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).</span><span class="sxs-lookup"><span data-stu-id="ad972-139">Before performing a reset, make sure you have the latest version of the [Resource Manager PowerShell cmdlets](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).</span></span> <span data-ttu-id="ad972-140">Následující příklad resetuje bránu virtuální sítě s názvem VNet1GW ve skupině prostředků TestRG1:</span><span class="sxs-lookup"><span data-stu-id="ad972-140">The following example resets a virtual network gateway named VNet1GW in the TestRG1 resource group:</span></span>

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw
```

<span data-ttu-id="ad972-141">Výsledek:</span><span class="sxs-lookup"><span data-stu-id="ad972-141">Result:</span></span>

<span data-ttu-id="ad972-142">Jakmile se zobrazí návratový výsledek, můžete předpokládat resetování brány byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="ad972-142">When you receive a return result, you can assume the gateway reset was successful.</span></span> <span data-ttu-id="ad972-143">Však není nic v návratové výsledek, který explicitně označuje, že obnovení bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="ad972-143">However, there is nothing in the return result that indicates explicitly that the reset was successful.</span></span> <span data-ttu-id="ad972-144">Pokud chcete podrobněji prohlížet historii, přesně při resetování brány došlo k chybě, můžete zobrazit tyto informace v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ad972-144">If you want to look closely at the history to see exactly when the gateway reset occurred, you can view that information in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="ad972-145">Na portálu, přejděte na **'GatewayName} -> Stav prostředku**.</span><span class="sxs-lookup"><span data-stu-id="ad972-145">In the portal, navigate to **'GatewayName' -> Resource Health**.</span></span>

### <span data-ttu-id="ad972-146"><a name="resetclassic"></a>Model nasazení Classic</span><span class="sxs-lookup"><span data-stu-id="ad972-146"><a name="resetclassic"></a>Classic deployment model</span></span>

<span data-ttu-id="ad972-147">Rutina pro resetování brány je **Reset-AzureVNetGateway**.</span><span class="sxs-lookup"><span data-stu-id="ad972-147">The cmdlet for resetting a gateway is **Reset-AzureVNetGateway**.</span></span> <span data-ttu-id="ad972-148">Před provedením provést obnovení, ujistěte se, abyste měli nejnovější verzi [rutiny prostředí PowerShell služby správy (SM)](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0).</span><span class="sxs-lookup"><span data-stu-id="ad972-148">Before performing a reset, make sure you have the latest version of the [Service Management (SM) PowerShell cmdlets](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0).</span></span> <span data-ttu-id="ad972-149">Následující příklad resetuje bránu pro virtuální síť s názvem "ContosoVNet":</span><span class="sxs-lookup"><span data-stu-id="ad972-149">The following example resets the gateway for a virtual network named "ContosoVNet":</span></span>

```powershell
Reset-AzureVNetGateway –VnetName “ContosoVNet”
```

<span data-ttu-id="ad972-150">Výsledek:</span><span class="sxs-lookup"><span data-stu-id="ad972-150">Result:</span></span>

```powershell
Error          :
HttpStatusCode : OK
Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
Status         : Successful
RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
StatusCode     : OK
```

## <span data-ttu-id="ad972-151"><a name="cli"></a>Rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="ad972-151"><a name="cli"></a>Azure CLI</span></span>

<span data-ttu-id="ad972-152">Chcete-li obnovit bránu, použijte [resetovat az brány virtuální sítě-](https://docs.microsoft.com/cli/azure/network/vnet-gateway#reset) příkaz.</span><span class="sxs-lookup"><span data-stu-id="ad972-152">To reset the gateway, use the [az network vnet-gateway reset](https://docs.microsoft.com/cli/azure/network/vnet-gateway#reset) command.</span></span> <span data-ttu-id="ad972-153">Následující příklad resetuje bránu virtuální sítě s názvem VNet5GW ve skupině prostředků TestRG5:</span><span class="sxs-lookup"><span data-stu-id="ad972-153">The following example resets a virtual network gateway named VNet5GW in the TestRG5 resource group:</span></span>

```azurecli
az network vnet-gateway reset -n VNet5GW -g TestRG5
```

<span data-ttu-id="ad972-154">Výsledek:</span><span class="sxs-lookup"><span data-stu-id="ad972-154">Result:</span></span>

<span data-ttu-id="ad972-155">Jakmile se zobrazí návratový výsledek, můžete předpokládat resetování brány byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="ad972-155">When you receive a return result, you can assume the gateway reset was successful.</span></span> <span data-ttu-id="ad972-156">Však není nic v návratové výsledek, který explicitně označuje, že obnovení bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="ad972-156">However, there is nothing in the return result that indicates explicitly that the reset was successful.</span></span> <span data-ttu-id="ad972-157">Pokud chcete podrobněji prohlížet historii, přesně při resetování brány došlo k chybě, můžete zobrazit tyto informace v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ad972-157">If you want to look closely at the history to see exactly when the gateway reset occurred, you can view that information in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="ad972-158">Na portálu, přejděte na **'GatewayName} -> Stav prostředku**.</span><span class="sxs-lookup"><span data-stu-id="ad972-158">In the portal, navigate to **'GatewayName' -> Resource Health**.</span></span>