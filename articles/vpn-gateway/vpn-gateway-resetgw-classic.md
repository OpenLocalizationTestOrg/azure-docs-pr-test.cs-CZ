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
# <a name="reset-a-vpn-gateway"></a><span data-ttu-id="b4a85-104">Resetování služby VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="b4a85-104">Reset a VPN Gateway</span></span>

<span data-ttu-id="b4a85-105">Resetování brány Azure VPN je užitečné v případě ztráty připojení VPN mezi lokalitami na jednom nebo více tunelech VPN typu Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="b4a85-105">Resetting an Azure VPN gateway is helpful if you lose cross-premises VPN connectivity on one or more Site-to-Site VPN tunnels.</span></span> <span data-ttu-id="b4a85-106">V této situaci vaše místní zařízení VPN jsou všechny funguje správně, ale nebylo možné tooestablish tunelových propojení IPsec pomocí bran Azure VPN hello.</span><span class="sxs-lookup"><span data-stu-id="b4a85-106">In this situation, your on-premises VPN devices are all working correctly, but are not able tooestablish IPsec tunnels with hello Azure VPN gateways.</span></span> <span data-ttu-id="b4a85-107">Tento článek vám umožňuje resetování služby VPN gateway.</span><span class="sxs-lookup"><span data-stu-id="b4a85-107">This article helps you reset your VPN gateway.</span></span>

### <a name="what-happens-during-a-reset"></a><span data-ttu-id="b4a85-108">Co se stane během vynulování?</span><span class="sxs-lookup"><span data-stu-id="b4a85-108">What happens during a reset?</span></span>

<span data-ttu-id="b4a85-109">Brána sítě VPN se skládá ze dvou instancí virtuálního počítače spuštěných v konfiguraci aktivní pohotovostní.</span><span class="sxs-lookup"><span data-stu-id="b4a85-109">A VPN gateway is composed of two VM instances running in an active-standby configuration.</span></span> <span data-ttu-id="b4a85-110">Když resetujete hello brány, dojde k restartování hello brány a potom znovu použije hello mezi různými místy tooit konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b4a85-110">When you reset hello gateway, it reboots hello gateway, and then reapplies hello cross-premises configurations tooit.</span></span> <span data-ttu-id="b4a85-111">Hello brány uchovává hello veřejnou IP adresu, které už je.</span><span class="sxs-lookup"><span data-stu-id="b4a85-111">hello gateway keeps hello public IP address it already has.</span></span> <span data-ttu-id="b4a85-112">To znamená, že nebudete potřebovat konfiguraci směrovače VPN hello tooupdate s novou veřejnou IP adresu brány Azure VPN.</span><span class="sxs-lookup"><span data-stu-id="b4a85-112">This means you won’t need tooupdate hello VPN router configuration with a new public IP address for Azure VPN gateway.</span></span>

<span data-ttu-id="b4a85-113">Pokud vydáte hello příkaz tooreset hello brány, je okamžitě resetuje právě aktivní instance brány Azure VPN hello hello.</span><span class="sxs-lookup"><span data-stu-id="b4a85-113">When you issue hello command tooreset hello gateway, hello current active instance of hello Azure VPN gateway is rebooted immediately.</span></span> <span data-ttu-id="b4a85-114">Během převzetí služeb při selhání hello hello aktivní instance (která se resetuje), toohello pohotovostní instancí bude ke krátké prodlevě.</span><span class="sxs-lookup"><span data-stu-id="b4a85-114">There will be a brief gap during hello failover from hello active instance (being rebooted), toohello standby instance.</span></span> <span data-ttu-id="b4a85-115">Hello prodleva by neměla být delší než jedna minuta.</span><span class="sxs-lookup"><span data-stu-id="b4a85-115">hello gap should be less than one minute.</span></span>

<span data-ttu-id="b4a85-116">Pokud nedojde k obnovení připojení hello po hello první restartování, problém hello stejný příkaz znovu tooreboot hello druhé instance virtuálního počítače (hello nové aktivní brány).</span><span class="sxs-lookup"><span data-stu-id="b4a85-116">If hello connection is not restored after hello first reboot, issue hello same command again tooreboot hello second VM instance (hello new active gateway).</span></span> <span data-ttu-id="b4a85-117">Pokud jsou dvě restartování hello požadovaný back tooback, budou existovat trochu delší dobu kde jsou obě instance virtuálního počítače (aktivní a pohotovostní) resetovány.</span><span class="sxs-lookup"><span data-stu-id="b4a85-117">If hello two reboots are requested back tooback, there will be a slightly longer period where both VM instances (active and standby) are being rebooted.</span></span> <span data-ttu-id="b4a85-118">To způsobí delší prodlevu na připojení k síti VPN hello, až minut too4 too2 pro virtuální počítače toocomplete hello restartování.</span><span class="sxs-lookup"><span data-stu-id="b4a85-118">This will cause a longer gap on hello VPN connectivity, up too2 too4 minutes for VMs toocomplete hello reboots.</span></span>

<span data-ttu-id="b4a85-119">Po dvou resetováních Pokud stále dochází k potížím připojení mezi různými místy, otevřete prosím žádost o podporu od hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b4a85-119">After two reboots, if you are still experiencing cross-premises connectivity problems, please open a support request from hello Azure portal.</span></span>

## <span data-ttu-id="b4a85-120"><a name="before"></a>Než začnete</span><span class="sxs-lookup"><span data-stu-id="b4a85-120"><a name="before"></a>Before you begin</span></span>

<span data-ttu-id="b4a85-121">Před bránu, ověřte hello klíčové položky uvedeny pro každý tunelu IPsec Site-to-Site (S2S) VPN.</span><span class="sxs-lookup"><span data-stu-id="b4a85-121">Before you reset your gateway, verify hello key items listed below for each IPsec Site-to-Site (S2S) VPN tunnel.</span></span> <span data-ttu-id="b4a85-122">Jakákoli Neshoda v položkách hello způsobí odpojení hello tunelů S2S VPN.</span><span class="sxs-lookup"><span data-stu-id="b4a85-122">Any mismatch in hello items will result in hello disconnect of S2S VPN tunnels.</span></span> <span data-ttu-id="b4a85-123">Ověřením a opravením konfigurace hello místní a Azure VPN Gateway uloží můžete z zbytečnému resetování a přerušení hello ostatních fungujících připojení na hello brány.</span><span class="sxs-lookup"><span data-stu-id="b4a85-123">Verifying and correcting hello configurations for your on-premises and Azure VPN gateways saves you from unnecessary reboots and disruptions for hello other working connections on hello gateways.</span></span>

<span data-ttu-id="b4a85-124">Ověřte hello před resetováním brány následující položky:</span><span class="sxs-lookup"><span data-stu-id="b4a85-124">Verify hello following items before resetting your gateway:</span></span>

* <span data-ttu-id="b4a85-125">Hello Internet IP adresy (VIP) pro obě brány Azure VPN hello a hello místní brány VPN jsou správně nakonfigurovány v obě zásady hello Azure a hello místní VPN.</span><span class="sxs-lookup"><span data-stu-id="b4a85-125">hello Internet IP addresses (VIPs) for both hello Azure VPN gateway and hello on-premises VPN gateway are configured correctly in both hello Azure and hello on-premises VPN policies.</span></span>
* <span data-ttu-id="b4a85-126">Hello předsdílený klíč musí být hello stejné na Azure a místní brány VPN.</span><span class="sxs-lookup"><span data-stu-id="b4a85-126">hello pre-shared key must be hello same on both Azure and on-premises VPN gateways.</span></span>
* <span data-ttu-id="b4a85-127">Pokud použijete určitou konfiguraci protokolu IPsec/IKE, jako je šifrování, hašování algoritmů a PFS (Perfect Forward Secrecy), ujistěte se, jak hello Azure a místní brány VPN mají hello stejné konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b4a85-127">If you apply specific IPsec/IKE configuration, such as encryption, hashing algorithms, and PFS (Perfect Forward Secrecy), ensure both hello Azure and on-premises VPN gateways have hello same configurations.</span></span>

## <span data-ttu-id="b4a85-128"><a name="portal"></a>Portál Azure</span><span class="sxs-lookup"><span data-stu-id="b4a85-128"><a name="portal"></a>Azure portal</span></span>

<span data-ttu-id="b4a85-129">Můžete resetovat bránu VPN Resource Manageru pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b4a85-129">You can reset a Resource Manager VPN gateway using hello Azure portal.</span></span> <span data-ttu-id="b4a85-130">Pokud chcete tooreset classic brány, najdete v části hello [prostředí PowerShell](#resetclassic) kroky.</span><span class="sxs-lookup"><span data-stu-id="b4a85-130">If you want tooreset a classic gateway, see hello [PowerShell](#resetclassic) steps.</span></span>

### <a name="resource-manager-deployment-model"></a><span data-ttu-id="b4a85-131">Model nasazení Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b4a85-131">Resource Manager deployment model</span></span>

1. <span data-ttu-id="b4a85-132">Otevřete hello [portál Azure](https://portal.azure.com) a přejděte brány virtuální sítě Resource Manager toohello, které chcete tooreset.</span><span class="sxs-lookup"><span data-stu-id="b4a85-132">Open hello [Azure portal](https://portal.azure.com) and navigate toohello Resource Manager virtual network gateway that you want tooreset.</span></span>
2. <span data-ttu-id="b4a85-133">V okně hello pro bránu virtuální sítě hello klikněte na tlačítko 'Resetovat'.</span><span class="sxs-lookup"><span data-stu-id="b4a85-133">On hello blade for hello virtual network gateway, click 'Reset'.</span></span>

  ![Okno resetování brány VPN](./media/vpn-gateway-howto-reset-gateway/reset-vpn-gateway-portal.png)
3. <span data-ttu-id="b4a85-135">V hello resetovat okno, klikněte na hello **resetovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b4a85-135">On hello Reset blade, click hello **Reset** button.</span></span>

## <span data-ttu-id="b4a85-136"><a name="ps"></a>Prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="b4a85-136"><a name="ps"></a>PowerShell</span></span>

### <a name="resource-manager-deployment-model"></a><span data-ttu-id="b4a85-137">Model nasazení Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b4a85-137">Resource Manager deployment model</span></span>

<span data-ttu-id="b4a85-138">Hello rutiny pro resetování brány je **resetování AzureRmVirtualNetworkGateway**.</span><span class="sxs-lookup"><span data-stu-id="b4a85-138">hello cmdlet for resetting a gateway is **Reset-AzureRmVirtualNetworkGateway**.</span></span> <span data-ttu-id="b4a85-139">Před provedením provést obnovení, ujistěte se, abyste měli nejnovější verzi hello hello [rutiny prostředí PowerShell Resource Manager](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).</span><span class="sxs-lookup"><span data-stu-id="b4a85-139">Before performing a reset, make sure you have hello latest version of hello [Resource Manager PowerShell cmdlets](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).</span></span> <span data-ttu-id="b4a85-140">Hello následující příklad resetuje bránu virtuální sítě s názvem VNet1GW ve skupině prostředků TestRG1 hello:</span><span class="sxs-lookup"><span data-stu-id="b4a85-140">hello following example resets a virtual network gateway named VNet1GW in hello TestRG1 resource group:</span></span>

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw
```

<span data-ttu-id="b4a85-141">Výsledek:</span><span class="sxs-lookup"><span data-stu-id="b4a85-141">Result:</span></span>

<span data-ttu-id="b4a85-142">Jakmile se zobrazí návratový výsledek, můžete předpokládat, resetování brány hello byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="b4a85-142">When you receive a return result, you can assume hello gateway reset was successful.</span></span> <span data-ttu-id="b4a85-143">Však není nic v hello návratový výsledek, který explicitně určuje, že resetování hello byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="b4a85-143">However, there is nothing in hello return result that indicates explicitly that hello reset was successful.</span></span> <span data-ttu-id="b4a85-144">Pokud chcete, aby toolook úzce v historii toosee hello přesně, kdy resetuje hello brány došlo k chybě, můžete zobrazit tyto informace v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b4a85-144">If you want toolook closely at hello history toosee exactly when hello gateway reset occurred, you can view that information in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="b4a85-145">Hello portálu, přejděte příliš**'GatewayName} -> Stav prostředku**.</span><span class="sxs-lookup"><span data-stu-id="b4a85-145">In hello portal, navigate too**'GatewayName' -> Resource Health**.</span></span>

### <span data-ttu-id="b4a85-146"><a name="resetclassic"></a>Model nasazení Classic</span><span class="sxs-lookup"><span data-stu-id="b4a85-146"><a name="resetclassic"></a>Classic deployment model</span></span>

<span data-ttu-id="b4a85-147">Hello rutiny pro resetování brány je **Reset-AzureVNetGateway**.</span><span class="sxs-lookup"><span data-stu-id="b4a85-147">hello cmdlet for resetting a gateway is **Reset-AzureVNetGateway**.</span></span> <span data-ttu-id="b4a85-148">Před provedením provést obnovení, ujistěte se, abyste měli nejnovější verzi hello hello [rutiny prostředí PowerShell služby správy (SM)](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0).</span><span class="sxs-lookup"><span data-stu-id="b4a85-148">Before performing a reset, make sure you have hello latest version of hello [Service Management (SM) PowerShell cmdlets](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0).</span></span> <span data-ttu-id="b4a85-149">Hello následující příklad resetuje hello brány pro virtuální síť s názvem "ContosoVNet":</span><span class="sxs-lookup"><span data-stu-id="b4a85-149">hello following example resets hello gateway for a virtual network named "ContosoVNet":</span></span>

```powershell
Reset-AzureVNetGateway –VnetName “ContosoVNet”
```

<span data-ttu-id="b4a85-150">Výsledek:</span><span class="sxs-lookup"><span data-stu-id="b4a85-150">Result:</span></span>

```powershell
Error          :
HttpStatusCode : OK
Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
Status         : Successful
RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
StatusCode     : OK
```

## <span data-ttu-id="b4a85-151"><a name="cli"></a>Rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="b4a85-151"><a name="cli"></a>Azure CLI</span></span>

<span data-ttu-id="b4a85-152">tooreset hello brány, použijte hello [resetovat az brány virtuální sítě-](https://docs.microsoft.com/cli/azure/network/vnet-gateway#reset) příkaz.</span><span class="sxs-lookup"><span data-stu-id="b4a85-152">tooreset hello gateway, use hello [az network vnet-gateway reset](https://docs.microsoft.com/cli/azure/network/vnet-gateway#reset) command.</span></span> <span data-ttu-id="b4a85-153">Hello následující příklad resetuje bránu virtuální sítě s názvem VNet5GW ve skupině prostředků TestRG5 hello:</span><span class="sxs-lookup"><span data-stu-id="b4a85-153">hello following example resets a virtual network gateway named VNet5GW in hello TestRG5 resource group:</span></span>

```azurecli
az network vnet-gateway reset -n VNet5GW -g TestRG5
```

<span data-ttu-id="b4a85-154">Výsledek:</span><span class="sxs-lookup"><span data-stu-id="b4a85-154">Result:</span></span>

<span data-ttu-id="b4a85-155">Jakmile se zobrazí návratový výsledek, můžete předpokládat, resetování brány hello byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="b4a85-155">When you receive a return result, you can assume hello gateway reset was successful.</span></span> <span data-ttu-id="b4a85-156">Však není nic v hello návratový výsledek, který explicitně určuje, že resetování hello byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="b4a85-156">However, there is nothing in hello return result that indicates explicitly that hello reset was successful.</span></span> <span data-ttu-id="b4a85-157">Pokud chcete, aby toolook úzce v historii toosee hello přesně, kdy resetuje hello brány došlo k chybě, můžete zobrazit tyto informace v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b4a85-157">If you want toolook closely at hello history toosee exactly when hello gateway reset occurred, you can view that information in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="b4a85-158">Hello portálu, přejděte příliš**'GatewayName} -> Stav prostředku**.</span><span class="sxs-lookup"><span data-stu-id="b4a85-158">In hello portal, navigate too**'GatewayName' -> Resource Health**.</span></span>
