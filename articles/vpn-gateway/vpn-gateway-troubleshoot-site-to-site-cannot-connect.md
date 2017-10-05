---
title: "Řešení potíží s Azure připojení VPN typu site-to-site, která se nemůže připojit | Microsoft Docs"
description: "Zjistěte, jak řešení připojení site-to-site VPN, který najednou, přestane modul fungovat a nelze znovu připojit."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/21/2017
ms.author: genli
ms.openlocfilehash: e7a3da64895f0307e5d6c3563672205a2f93a7d2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-an-azure-site-to-site-vpn-connection-cannot-connect-and-stops-working"></a><span data-ttu-id="c23d1-103">Řešení potíží: Připojení k Azure site-to-site VPN se nemůže připojit a zastaví práce</span><span class="sxs-lookup"><span data-stu-id="c23d1-103">Troubleshooting: An Azure site-to-site VPN connection cannot connect and stops working</span></span>

<span data-ttu-id="c23d1-104">Po dokončení konfigurace připojení site-to-site VPN mezi místní sítí a virtuální síť Azure, připojení k síti VPN najednou přestane fungovat a nelze znovu připojit.</span><span class="sxs-lookup"><span data-stu-id="c23d1-104">After you configure a site-to-site VPN connection between an on-premises network and an Azure virtual network, the VPN connection suddenly stops working and cannot be reconnected.</span></span> <span data-ttu-id="c23d1-105">Tento článek obsahuje postup pro odstraňování potíží při řešení tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="c23d1-105">This article provides troubleshooting steps to help you resolve this problem.</span></span> 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a><span data-ttu-id="c23d1-106">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="c23d1-106">Troubleshooting steps</span></span>

<span data-ttu-id="c23d1-107">Chcete-li problém vyřešit, zkuste je napřed [resetování brány Azure VPN](vpn-gateway-resetgw-classic.md) a resetovat tunelové propojení z místního zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="c23d1-107">To resolve the problem, first try to [reset the Azure VPN gateway](vpn-gateway-resetgw-classic.md) and reset the tunnel from the on-premises VPN device.</span></span> <span data-ttu-id="c23d1-108">Pokud potíže potrvají, postupujte podle těchto kroků zjistit příčinu problému.</span><span class="sxs-lookup"><span data-stu-id="c23d1-108">If the problem persists, follow these steps to identify the cause of the problem.</span></span>

### <a name="prerequisite-step"></a><span data-ttu-id="c23d1-109">Požadovaný krok</span><span class="sxs-lookup"><span data-stu-id="c23d1-109">Prerequisite step</span></span>

<span data-ttu-id="c23d1-110">Zkontrolujte typ brány Azure VPN.</span><span class="sxs-lookup"><span data-stu-id="c23d1-110">Check the type of the Azure VPN gateway.</span></span>

1. <span data-ttu-id="c23d1-111">Přejděte na [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c23d1-111">Go to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="c23d1-112">Zkontrolujte **přehled** stránka služby VPN gateway pro informace o typu.</span><span class="sxs-lookup"><span data-stu-id="c23d1-112">Check the **Overview** page of the VPN gateway for the type information.</span></span>
    
    ![Přehled brány](media\vpn-gateway-troubleshoot-site-to-site-cannot-connect\gatewayoverview.png)

### <a name="step-1-check-whether-the-on-premises-vpn-device-is-validated"></a><span data-ttu-id="c23d1-114">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="c23d1-114">Step 1.</span></span> <span data-ttu-id="c23d1-115">Zkontrolujte, zda je ověřen místního zařízení VPN</span><span class="sxs-lookup"><span data-stu-id="c23d1-115">Check whether the on-premises VPN device is validated</span></span>

1. <span data-ttu-id="c23d1-116">Zkontrolujte, zda používáte [ověřit zařízení VPN a verzi operačního systému](vpn-gateway-about-vpn-devices.md#devicetable).</span><span class="sxs-lookup"><span data-stu-id="c23d1-116">Check whether you are using a [validated VPN device and operating system version](vpn-gateway-about-vpn-devices.md#devicetable).</span></span> <span data-ttu-id="c23d1-117">Pokud zařízení není ověřená zařízení VPN, budete možná muset obraťte se na výrobce zařízení a jestli jsou potíže s kompatibilitou.</span><span class="sxs-lookup"><span data-stu-id="c23d1-117">If the device is not a validated VPN device, you might have to contact the device manufacturer to see if there is a compatibility issue.</span></span>

2. <span data-ttu-id="c23d1-118">Ujistěte se, zda je správně nakonfigurováno zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="c23d1-118">Make sure that the VPN device is correctly configured.</span></span> <span data-ttu-id="c23d1-119">Další informace najdete v tématu [upravit ukázky konfigurace zařízení](/vpn-gateway-about-vpn-devices.md#editing).</span><span class="sxs-lookup"><span data-stu-id="c23d1-119">For more information, see [Edit device configuration samples](/vpn-gateway-about-vpn-devices.md#editing).</span></span>

### <a name="step-2-verify-the-shared-key"></a><span data-ttu-id="c23d1-120">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="c23d1-120">Step 2.</span></span> <span data-ttu-id="c23d1-121">Ověřte sdílený klíč</span><span class="sxs-lookup"><span data-stu-id="c23d1-121">Verify the shared key</span></span>

<span data-ttu-id="c23d1-122">Porovnání se sdílený klíč pro místní zařízení VPN Azure virtuální sítě VPN a ujistěte se, jestli se klíče shodují.</span><span class="sxs-lookup"><span data-stu-id="c23d1-122">Compare the shared key for the on-premises VPN device to the Azure Virtual Network VPN to make sure that the keys match.</span></span> 

<span data-ttu-id="c23d1-123">Chcete-li zobrazit sdílený klíč pro připojení k síti VPN Azure, použijte jednu z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="c23d1-123">To view the shared key for the Azure VPN connection, use one of the following methods:</span></span>

<span data-ttu-id="c23d1-124">**Azure Portal**</span><span class="sxs-lookup"><span data-stu-id="c23d1-124">**Azure portal**</span></span>

1. <span data-ttu-id="c23d1-125">Přejděte na připojení site-to-site VPN brány, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="c23d1-125">Go to the VPN gateway site-to-site connection that you created.</span></span>

2. <span data-ttu-id="c23d1-126">V **nastavení** klikněte na tlačítko **sdílený klíč**.</span><span class="sxs-lookup"><span data-stu-id="c23d1-126">In the **Settings** section, click **Shared key**.</span></span>
    
    ![Sdílený klíč](media/vpn-gateway-troubleshoot-site-to-site-cannot-connect/sharedkey.png)

<span data-ttu-id="c23d1-128">**Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="c23d1-128">**Azure PowerShell**</span></span>

<span data-ttu-id="c23d1-129">Pro model nasazení Azure Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="c23d1-129">For the Azure Resource Manager deployment model:</span></span>

    Get-AzureRmVirtualNetworkGatewayConnectionSharedKey -Name <Connection name> -ResourceGroupName <Resource group name>

<span data-ttu-id="c23d1-130">Pro model nasazení classic:</span><span class="sxs-lookup"><span data-stu-id="c23d1-130">For the classic deployment model:</span></span>

    Get-AzureVNetGatewayKey -VNetName -LocalNetworkSiteName

### <a name="step-3-verify-the-vpn-peer-ips"></a><span data-ttu-id="c23d1-131">Krok 3.</span><span class="sxs-lookup"><span data-stu-id="c23d1-131">Step 3.</span></span> <span data-ttu-id="c23d1-132">Ověřte sdílené sítě VPN IP adresy</span><span class="sxs-lookup"><span data-stu-id="c23d1-132">Verify the VPN peer IPs</span></span>

-   <span data-ttu-id="c23d1-133">V definici IP **bránu místní sítě** objektu v Azure by měl odpovídat IP místní zařízení.</span><span class="sxs-lookup"><span data-stu-id="c23d1-133">The IP definition in the **Local Network Gateway** object in Azure should match the on-premises device IP.</span></span>
-   <span data-ttu-id="c23d1-134">Služba Azure gateway IP by měl odpovídat definici IP brány Azure, který je nastavený na místní zařízení.</span><span class="sxs-lookup"><span data-stu-id="c23d1-134">The Azure gateway IP definition that is set on the on-premises device should match the Azure gateway IP.</span></span>

### <a name="step-4-check-udr-and-nsgs-on-the-gateway-subnet"></a><span data-ttu-id="c23d1-135">Krok 4.</span><span class="sxs-lookup"><span data-stu-id="c23d1-135">Step 4.</span></span> <span data-ttu-id="c23d1-136">Zkontrolujte UDR a skupiny Nsg na podsítě brány</span><span class="sxs-lookup"><span data-stu-id="c23d1-136">Check UDR and NSGs on the gateway subnet</span></span>

<span data-ttu-id="c23d1-137">Kontrolovat a odeberte uživatelem definované směrování (UDR) nebo skupiny zabezpečení sítě (Nsg) pro podsíť brány a poté otestujte výsledek.</span><span class="sxs-lookup"><span data-stu-id="c23d1-137">Check for and remove user-defined routing (UDR) or Network Security Groups (NSGs) on the gateway subnet, and then test the result.</span></span> <span data-ttu-id="c23d1-138">Pokud byl problém vyřešen, ověřte nastavení použité UDR nebo NSG.</span><span class="sxs-lookup"><span data-stu-id="c23d1-138">If the problem is resolved, validate the settings that UDR or NSG applied.</span></span>

### <a name="step-5-check-the-on-premises-vpn-device-external-interface-address"></a><span data-ttu-id="c23d1-139">Krok 5.</span><span class="sxs-lookup"><span data-stu-id="c23d1-139">Step 5.</span></span> <span data-ttu-id="c23d1-140">Zkontrolujte adresu místního zařízení VPN externí rozhraní</span><span class="sxs-lookup"><span data-stu-id="c23d1-140">Check the on-premises VPN device external interface address</span></span>

- <span data-ttu-id="c23d1-141">Pokud je součástí internetového IP adresa zařízení VPN **místní sítě** definice v Azure, můžete zaznamenat ojediněle odpojení.</span><span class="sxs-lookup"><span data-stu-id="c23d1-141">If the Internet-facing IP address of the VPN device is included in the **Local network** definition in Azure, you might experience sporadic disconnections.</span></span>
- <span data-ttu-id="c23d1-142">Externí rozhraní zařízení musí být přímo na Internetu.</span><span class="sxs-lookup"><span data-stu-id="c23d1-142">The device's external interface must be directly on the Internet.</span></span> <span data-ttu-id="c23d1-143">Měla by existovat žádné překlad síťových adres nebo brány firewall mezi Internetu a zařízení.</span><span class="sxs-lookup"><span data-stu-id="c23d1-143">There should be no network address translation or firewall between the Internet and the device.</span></span>
- <span data-ttu-id="c23d1-144">Konfigurace brány firewall clustering tak, aby měl virtuální IP adresy, musí přerušení clusteru a vystavit zařízení VPN přímo k veřejné rozhraní, které můžete bránu rozhraní s.</span><span class="sxs-lookup"><span data-stu-id="c23d1-144">To configure firewall clustering to have a virtual IP, you must break the cluster and expose the VPN appliance directly to a public interface that the gateway can interface with.</span></span>

### <a name="step-6-verify-that-the-subnets-match-exactly-azure-policy-based-gateways"></a><span data-ttu-id="c23d1-145">Krok 6.</span><span class="sxs-lookup"><span data-stu-id="c23d1-145">Step 6.</span></span> <span data-ttu-id="c23d1-146">Ověřte, že podsítě přesně odpovídají (Azure na základě zásad brány)</span><span class="sxs-lookup"><span data-stu-id="c23d1-146">Verify that the subnets match exactly (Azure policy-based gateways)</span></span>

-   <span data-ttu-id="c23d1-147">Ověřte, že podsítě odpovídají přesně mezi virtuální sítí Azure a místními definice pro virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="c23d1-147">Verify that the subnets match exactly between the Azure virtual network and on-premises definitions for the Azure virtual network.</span></span>
-   <span data-ttu-id="c23d1-148">Ověřte, že podsítí mezi přesně odpovídají **bránu místní sítě** a místní definice pro místní sítě.</span><span class="sxs-lookup"><span data-stu-id="c23d1-148">Verify that the subnets match exactly between the **Local Network Gateway** and on-premises definitions for the on-premises network.</span></span>

### <a name="step-7-verify-the-azure-gateway-health-probe"></a><span data-ttu-id="c23d1-149">Krok 7.</span><span class="sxs-lookup"><span data-stu-id="c23d1-149">Step 7.</span></span> <span data-ttu-id="c23d1-150">Ověřte test stavu služba Azure gateway</span><span class="sxs-lookup"><span data-stu-id="c23d1-150">Verify the Azure gateway health probe</span></span>

1. <span data-ttu-id="c23d1-151">Přejděte na [test stavu](https://&lt;YourVirtualNetworkGatewayIP&gt;:8081/healthprobe).</span><span class="sxs-lookup"><span data-stu-id="c23d1-151">Go to the [health probe](https://&lt;YourVirtualNetworkGatewayIP&gt;:8081/healthprobe).</span></span>

2. <span data-ttu-id="c23d1-152">Proklikejte se prostřednictvím upozornění certifikátu.</span><span class="sxs-lookup"><span data-stu-id="c23d1-152">Click through the certificate warning.</span></span>
3. <span data-ttu-id="c23d1-153">Pokud se zobrazí odpověď, považuje za bránu sítě VPN v pořádku.</span><span class="sxs-lookup"><span data-stu-id="c23d1-153">If you receive a response, the VPN gateway is considered healthy.</span></span> <span data-ttu-id="c23d1-154">Pokud jste neobdrželi odpověď, nemusí být brána v pořádku nebo skupinu NSG na podsítě brány je příčinou problému.</span><span class="sxs-lookup"><span data-stu-id="c23d1-154">If you don't receive a response, the gateway might not be healthy or an NSG on the gateway subnet is causing the problem.</span></span> <span data-ttu-id="c23d1-155">Tento text je ukázková odpověď:</span><span class="sxs-lookup"><span data-stu-id="c23d1-155">The following text is a sample response:</span></span>

    <span data-ttu-id="c23d1-156">&lt;? xml verze = "1.0"? > <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">primární Instance: GatewayTenantWorker_IN_1 GatewayTenantVersion: 14.7.24.6 < / řetězec&gt;</span><span class="sxs-lookup"><span data-stu-id="c23d1-156">&lt;?xml version="1.0"?>  <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Primary Instance: GatewayTenantWorker_IN_1 GatewayTenantVersion: 14.7.24.6</string&gt;</span></span>

### <a name="step-8-check-whether-the-on-premises-vpn-device-has-the-perfect-forward-secrecy-feature-enabled"></a><span data-ttu-id="c23d1-157">Krok 8.</span><span class="sxs-lookup"><span data-stu-id="c23d1-157">Step 8.</span></span> <span data-ttu-id="c23d1-158">Zkontrolujte, zda má místního zařízení VPN povolena funkce metoda perfect forward secrecy</span><span class="sxs-lookup"><span data-stu-id="c23d1-158">Check whether the on-premises VPN device has the perfect forward secrecy feature enabled</span></span>

<span data-ttu-id="c23d1-159">Metoda perfect forward secrecy funkce může způsobit problémy odpojení.</span><span class="sxs-lookup"><span data-stu-id="c23d1-159">The perfect forward secrecy feature can cause disconnection problems.</span></span> <span data-ttu-id="c23d1-160">Pokud zařízení VPN má metoda perfect forward secrecy povolit, zakážete funkci.</span><span class="sxs-lookup"><span data-stu-id="c23d1-160">If the VPN device has perfect forward secrecy enabled, disable the feature.</span></span> <span data-ttu-id="c23d1-161">Aktualizujte bránu VPN typu zásad protokolu IPsec.</span><span class="sxs-lookup"><span data-stu-id="c23d1-161">Then update the VPN gateway IPsec policy.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c23d1-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c23d1-162">Next steps</span></span>

-   [<span data-ttu-id="c23d1-163">Konfigurace připojení typu site-to-site k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="c23d1-163">Configure a site-to-site connection to a virtual network</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
-   [<span data-ttu-id="c23d1-164">Konfigurace zásady protokolu IPsec/IKE pro připojení VPN typu site-to-site</span><span class="sxs-lookup"><span data-stu-id="c23d1-164">Configure an IPsec/IKE policy for site-to-site VPN connections</span></span>](vpn-gateway-ipsecikepolicy-rm-powershell.md)
