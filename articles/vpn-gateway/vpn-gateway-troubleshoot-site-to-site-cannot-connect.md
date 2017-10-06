---
title: "aaaTroubleshoot Azure připojení VPN typu site-to-site, která se nemůže připojit | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot připojení site-to-site VPN, najednou, přestane modul fungovat a nelze znovu připojit."
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
ms.openlocfilehash: 632c75bfcfb93a532eeead2855d43e6614569a99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-an-azure-site-to-site-vpn-connection-cannot-connect-and-stops-working"></a><span data-ttu-id="561cb-103">Řešení potíží: Připojení k Azure site-to-site VPN se nemůže připojit a zastaví práce</span><span class="sxs-lookup"><span data-stu-id="561cb-103">Troubleshooting: An Azure site-to-site VPN connection cannot connect and stops working</span></span>

<span data-ttu-id="561cb-104">Po dokončení konfigurace připojení site-to-site VPN mezi místní sítí a virtuální síť Azure, hello připojení k síti VPN najednou přestane fungovat a nelze znovu připojit.</span><span class="sxs-lookup"><span data-stu-id="561cb-104">After you configure a site-to-site VPN connection between an on-premises network and an Azure virtual network, hello VPN connection suddenly stops working and cannot be reconnected.</span></span> <span data-ttu-id="561cb-105">Tento článek obsahuje řešení problémů s kroky toohelp vyřešíte tento problém.</span><span class="sxs-lookup"><span data-stu-id="561cb-105">This article provides troubleshooting steps toohelp you resolve this problem.</span></span> 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a><span data-ttu-id="561cb-106">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="561cb-106">Troubleshooting steps</span></span>

<span data-ttu-id="561cb-107">tooresolve hello problém, zkuste je napřed příliš[resetování brány Azure VPN hello](vpn-gateway-resetgw-classic.md) a resetovat hello tunel ze zařízení VPN místní hello.</span><span class="sxs-lookup"><span data-stu-id="561cb-107">tooresolve hello problem, first try too[reset hello Azure VPN gateway](vpn-gateway-resetgw-classic.md) and reset hello tunnel from hello on-premises VPN device.</span></span> <span data-ttu-id="561cb-108">Pokud hello problém přetrvává, postupujte podle těchto kroků tooidentify hello příčinu problému hello.</span><span class="sxs-lookup"><span data-stu-id="561cb-108">If hello problem persists, follow these steps tooidentify hello cause of hello problem.</span></span>

### <a name="prerequisite-step"></a><span data-ttu-id="561cb-109">Požadovaný krok</span><span class="sxs-lookup"><span data-stu-id="561cb-109">Prerequisite step</span></span>

<span data-ttu-id="561cb-110">Zkontrolujte hello typ brány Azure VPN hello.</span><span class="sxs-lookup"><span data-stu-id="561cb-110">Check hello type of hello Azure VPN gateway.</span></span>

1. <span data-ttu-id="561cb-111">Přejděte toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="561cb-111">Go toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="561cb-112">Zkontrolujte hello **přehled** stránku hello brány VPN pro informace o typu hello.</span><span class="sxs-lookup"><span data-stu-id="561cb-112">Check hello **Overview** page of hello VPN gateway for hello type information.</span></span>
    
    ![Přehled brány hello](media\vpn-gateway-troubleshoot-site-to-site-cannot-connect\gatewayoverview.png)

### <a name="step-1-check-whether-hello-on-premises-vpn-device-is-validated"></a><span data-ttu-id="561cb-114">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="561cb-114">Step 1.</span></span> <span data-ttu-id="561cb-115">Zkontrolujte, zda je ověřen zařízení VPN místní hello</span><span class="sxs-lookup"><span data-stu-id="561cb-115">Check whether hello on-premises VPN device is validated</span></span>

1. <span data-ttu-id="561cb-116">Zkontrolujte, zda používáte [ověřit zařízení VPN a verzi operačního systému](vpn-gateway-about-vpn-devices.md#devicetable).</span><span class="sxs-lookup"><span data-stu-id="561cb-116">Check whether you are using a [validated VPN device and operating system version](vpn-gateway-about-vpn-devices.md#devicetable).</span></span> <span data-ttu-id="561cb-117">Pokud zařízení hello není ověřená zařízení VPN, může být toocontact hello zařízení výrobce toosee, pokud nastane problém s kompatibilitou.</span><span class="sxs-lookup"><span data-stu-id="561cb-117">If hello device is not a validated VPN device, you might have toocontact hello device manufacturer toosee if there is a compatibility issue.</span></span>

2. <span data-ttu-id="561cb-118">Ujistěte se, že toto zařízení VPN hello je správně nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="561cb-118">Make sure that hello VPN device is correctly configured.</span></span> <span data-ttu-id="561cb-119">Další informace najdete v tématu [upravit ukázky konfigurace zařízení](/vpn-gateway-about-vpn-devices.md#editing).</span><span class="sxs-lookup"><span data-stu-id="561cb-119">For more information, see [Edit device configuration samples](/vpn-gateway-about-vpn-devices.md#editing).</span></span>

### <a name="step-2-verify-hello-shared-key"></a><span data-ttu-id="561cb-120">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="561cb-120">Step 2.</span></span> <span data-ttu-id="561cb-121">Ověřte sdílený klíč hello</span><span class="sxs-lookup"><span data-stu-id="561cb-121">Verify hello shared key</span></span>

<span data-ttu-id="561cb-122">Porovnejte hello sdílený klíč pro hello místní VPN zařízení toohello virtuální sítě VPN Azure toomake jistotu, že hello klíče shodují.</span><span class="sxs-lookup"><span data-stu-id="561cb-122">Compare hello shared key for hello on-premises VPN device toohello Azure Virtual Network VPN toomake sure that hello keys match.</span></span> 

<span data-ttu-id="561cb-123">tooview hello sdílený klíč pro hello připojení Azure VPN, použijte jednu z následujících metod hello:</span><span class="sxs-lookup"><span data-stu-id="561cb-123">tooview hello shared key for hello Azure VPN connection, use one of hello following methods:</span></span>

<span data-ttu-id="561cb-124">**Azure Portal**</span><span class="sxs-lookup"><span data-stu-id="561cb-124">**Azure portal**</span></span>

1. <span data-ttu-id="561cb-125">Přejděte toohello připojení site-to-site brány v sítě VPN, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="561cb-125">Go toohello VPN gateway site-to-site connection that you created.</span></span>

2. <span data-ttu-id="561cb-126">V hello **nastavení** klikněte na tlačítko **sdílený klíč**.</span><span class="sxs-lookup"><span data-stu-id="561cb-126">In hello **Settings** section, click **Shared key**.</span></span>
    
    ![Sdílený klíč](media/vpn-gateway-troubleshoot-site-to-site-cannot-connect/sharedkey.png)

<span data-ttu-id="561cb-128">**Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="561cb-128">**Azure PowerShell**</span></span>

<span data-ttu-id="561cb-129">Pro model nasazení Azure Resource Manager hello:</span><span class="sxs-lookup"><span data-stu-id="561cb-129">For hello Azure Resource Manager deployment model:</span></span>

    Get-AzureRmVirtualNetworkGatewayConnectionSharedKey -Name <Connection name> -ResourceGroupName <Resource group name>

<span data-ttu-id="561cb-130">Pro model nasazení classic hello:</span><span class="sxs-lookup"><span data-stu-id="561cb-130">For hello classic deployment model:</span></span>

    Get-AzureVNetGatewayKey -VNetName -LocalNetworkSiteName

### <a name="step-3-verify-hello-vpn-peer-ips"></a><span data-ttu-id="561cb-131">Krok 3.</span><span class="sxs-lookup"><span data-stu-id="561cb-131">Step 3.</span></span> <span data-ttu-id="561cb-132">Ověřte hello VPN sdílené IP adresy</span><span class="sxs-lookup"><span data-stu-id="561cb-132">Verify hello VPN peer IPs</span></span>

-   <span data-ttu-id="561cb-133">Hello IP definice v hello **bránu místní sítě** objektu v Azure by měl odpovídat hello místní zařízení IP.</span><span class="sxs-lookup"><span data-stu-id="561cb-133">hello IP definition in hello **Local Network Gateway** object in Azure should match hello on-premises device IP.</span></span>
-   <span data-ttu-id="561cb-134">Služba Azure gateway IP definice, které jsou nastavené v hello Hello místního zařízení by měl odpovídat hello služba Azure gateway IP.</span><span class="sxs-lookup"><span data-stu-id="561cb-134">hello Azure gateway IP definition that is set on hello on-premises device should match hello Azure gateway IP.</span></span>

### <a name="step-4-check-udr-and-nsgs-on-hello-gateway-subnet"></a><span data-ttu-id="561cb-135">Krok 4.</span><span class="sxs-lookup"><span data-stu-id="561cb-135">Step 4.</span></span> <span data-ttu-id="561cb-136">Zkontrolujte UDR a skupiny Nsg na podsítě brány hello</span><span class="sxs-lookup"><span data-stu-id="561cb-136">Check UDR and NSGs on hello gateway subnet</span></span>

<span data-ttu-id="561cb-137">Zkontrolujte a odeberte uživatelem definované směrování (UDR) nebo skupiny zabezpečení sítě (Nsg) pro podsíť brány hello a poté otestujte hello výsledek.</span><span class="sxs-lookup"><span data-stu-id="561cb-137">Check for and remove user-defined routing (UDR) or Network Security Groups (NSGs) on hello gateway subnet, and then test hello result.</span></span> <span data-ttu-id="561cb-138">Pokud hello problém vyřešit, ověřte nastavení hello, použité UDR nebo NSG.</span><span class="sxs-lookup"><span data-stu-id="561cb-138">If hello problem is resolved, validate hello settings that UDR or NSG applied.</span></span>

### <a name="step-5-check-hello-on-premises-vpn-device-external-interface-address"></a><span data-ttu-id="561cb-139">Krok 5.</span><span class="sxs-lookup"><span data-stu-id="561cb-139">Step 5.</span></span> <span data-ttu-id="561cb-140">Kontrola hello místní adresa externí rozhraní zařízení VPN</span><span class="sxs-lookup"><span data-stu-id="561cb-140">Check hello on-premises VPN device external interface address</span></span>

- <span data-ttu-id="561cb-141">Pokud hello internetového IP adresa zařízení VPN hello je součástí hello **místní sítě** definice v Azure, můžete zaznamenat ojediněle odpojení.</span><span class="sxs-lookup"><span data-stu-id="561cb-141">If hello Internet-facing IP address of hello VPN device is included in hello **Local network** definition in Azure, you might experience sporadic disconnections.</span></span>
- <span data-ttu-id="561cb-142">Hello externí rozhraní zařízení musí být přímo na hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="561cb-142">hello device's external interface must be directly on hello Internet.</span></span> <span data-ttu-id="561cb-143">Měla by existovat žádné překlad síťových adres nebo brány firewall mezi hello Internet a hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="561cb-143">There should be no network address translation or firewall between hello Internet and hello device.</span></span>
- <span data-ttu-id="561cb-144">tooconfigure brány firewall clustering toohave virtuální IP adresy, je nutné rozdělit hello clusteru a vystavit zařízení VPN hello přímo veřejné rozhraní tooa této brány hello můžete rozhraní s.</span><span class="sxs-lookup"><span data-stu-id="561cb-144">tooconfigure firewall clustering toohave a virtual IP, you must break hello cluster and expose hello VPN appliance directly tooa public interface that hello gateway can interface with.</span></span>

### <a name="step-6-verify-that-hello-subnets-match-exactly-azure-policy-based-gateways"></a><span data-ttu-id="561cb-145">Krok 6.</span><span class="sxs-lookup"><span data-stu-id="561cb-145">Step 6.</span></span> <span data-ttu-id="561cb-146">Ověřte, zda text hello podsítě odpovídají přesně (Azure na základě zásad brány)</span><span class="sxs-lookup"><span data-stu-id="561cb-146">Verify that hello subnets match exactly (Azure policy-based gateways)</span></span>

-   <span data-ttu-id="561cb-147">Ověřte, zda text hello podsítě odpovídají přesně mezi hello virtuální síť Azure a místními definice pro hello virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="561cb-147">Verify that hello subnets match exactly between hello Azure virtual network and on-premises definitions for hello Azure virtual network.</span></span>
-   <span data-ttu-id="561cb-148">Ověřte, zda text hello podsítě odpovídají přesně mezi hello **bránu místní sítě** a místní definice pro hello do místní sítě.</span><span class="sxs-lookup"><span data-stu-id="561cb-148">Verify that hello subnets match exactly between hello **Local Network Gateway** and on-premises definitions for hello on-premises network.</span></span>

### <a name="step-7-verify-hello-azure-gateway-health-probe"></a><span data-ttu-id="561cb-149">Krok 7.</span><span class="sxs-lookup"><span data-stu-id="561cb-149">Step 7.</span></span> <span data-ttu-id="561cb-150">Ověřte test stavu služba Azure gateway hello</span><span class="sxs-lookup"><span data-stu-id="561cb-150">Verify hello Azure gateway health probe</span></span>

1. <span data-ttu-id="561cb-151">Přejděte toohello [test stavu](https://&lt;YourVirtualNetworkGatewayIP&gt;:8081/healthprobe).</span><span class="sxs-lookup"><span data-stu-id="561cb-151">Go toohello [health probe](https://&lt;YourVirtualNetworkGatewayIP&gt;:8081/healthprobe).</span></span>

2. <span data-ttu-id="561cb-152">Proklikejte se prostřednictvím hello upozornění certifikátu.</span><span class="sxs-lookup"><span data-stu-id="561cb-152">Click through hello certificate warning.</span></span>
3. <span data-ttu-id="561cb-153">Pokud se zobrazí odpověď, považuje za hello brána sítě VPN v pořádku.</span><span class="sxs-lookup"><span data-stu-id="561cb-153">If you receive a response, hello VPN gateway is considered healthy.</span></span> <span data-ttu-id="561cb-154">Pokud jste neobdrželi odpověď, nemusí být hello brány v pořádku nebo skupinu NSG na podsítě brány hello způsobuje problém hello.</span><span class="sxs-lookup"><span data-stu-id="561cb-154">If you don't receive a response, hello gateway might not be healthy or an NSG on hello gateway subnet is causing hello problem.</span></span> <span data-ttu-id="561cb-155">Následující text Hello je ukázková odpověď:</span><span class="sxs-lookup"><span data-stu-id="561cb-155">hello following text is a sample response:</span></span>

    <span data-ttu-id="561cb-156">&lt;? xml verze = "1.0"? > <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">primární Instance: GatewayTenantWorker_IN_1 GatewayTenantVersion: 14.7.24.6 < / řetězec&gt;</span><span class="sxs-lookup"><span data-stu-id="561cb-156">&lt;?xml version="1.0"?>  <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Primary Instance: GatewayTenantWorker_IN_1 GatewayTenantVersion: 14.7.24.6</string&gt;</span></span>

### <a name="step-8-check-whether-hello-on-premises-vpn-device-has-hello-perfect-forward-secrecy-feature-enabled"></a><span data-ttu-id="561cb-157">Krok 8.</span><span class="sxs-lookup"><span data-stu-id="561cb-157">Step 8.</span></span> <span data-ttu-id="561cb-158">Zkontrolujte, zda text hello místního zařízení VPN má povolenou funkci metoda perfect forward secrecy hello</span><span class="sxs-lookup"><span data-stu-id="561cb-158">Check whether hello on-premises VPN device has hello perfect forward secrecy feature enabled</span></span>

<span data-ttu-id="561cb-159">Hello metoda perfect forward secrecy funkce může způsobit problémy odpojení.</span><span class="sxs-lookup"><span data-stu-id="561cb-159">hello perfect forward secrecy feature can cause disconnection problems.</span></span> <span data-ttu-id="561cb-160">Pokud zařízení VPN hello má metoda perfect forward secrecy povolit, zakážete funkci hello.</span><span class="sxs-lookup"><span data-stu-id="561cb-160">If hello VPN device has perfect forward secrecy enabled, disable hello feature.</span></span> <span data-ttu-id="561cb-161">Aktualizujte zásady IPsec brány sítě VPN hello.</span><span class="sxs-lookup"><span data-stu-id="561cb-161">Then update hello VPN gateway IPsec policy.</span></span>

## <a name="next-steps"></a><span data-ttu-id="561cb-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="561cb-162">Next steps</span></span>

-   [<span data-ttu-id="561cb-163">Konfigurace virtuální sítě tooa připojení site-to-site</span><span class="sxs-lookup"><span data-stu-id="561cb-163">Configure a site-to-site connection tooa virtual network</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
-   [<span data-ttu-id="561cb-164">Konfigurace zásady protokolu IPsec/IKE pro připojení VPN typu site-to-site</span><span class="sxs-lookup"><span data-stu-id="561cb-164">Configure an IPsec/IKE policy for site-to-site VPN connections</span></span>](vpn-gateway-ipsecikepolicy-rm-powershell.md)
