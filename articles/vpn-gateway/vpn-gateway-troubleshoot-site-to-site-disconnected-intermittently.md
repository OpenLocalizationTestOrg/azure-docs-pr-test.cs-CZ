---
title: "Řešení potíží s Azure Site-to-Site VPN odpojí občas | Microsoft Docs"
description: "Zjistěte, jak k vyřešení tohoto problému, v němž odpojena pravidelně připojení Site-to-Site VPN."
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
ms.openlocfilehash: 99a790617baa65116bfba976cd9279627e8775f3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-azure-site-to-site-vpn-disconnects-intermittently"></a><span data-ttu-id="9a8d2-103">Řešení potíží: Azure Site-to-Site VPN odpojí občas</span><span class="sxs-lookup"><span data-stu-id="9a8d2-103">Troubleshooting: Azure Site-to-Site VPN disconnects intermittently</span></span>

<span data-ttu-id="9a8d2-104">Může dojít k potížím, že nové nebo existující připojení VPN v Microsoft Azure Point-to-Site není stabilní nebo odpojí pravidelně.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-104">You might experience the problem that a new or existing Microsoft Azure Point-to-Site VPN connection is not stable or disconnects regularly.</span></span> <span data-ttu-id="9a8d2-105">Tento článek obsahuje řešení potíží s kroky, které vám pomohou identifikovat a vyřešit příčinu problému.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-105">This article provides troubleshoot steps to help you identify and resolve the cause of the problem.</span></span> 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a><span data-ttu-id="9a8d2-106">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="9a8d2-106">Troubleshooting steps</span></span>

### <a name="prerequisite-step"></a><span data-ttu-id="9a8d2-107">Požadovaný krok</span><span class="sxs-lookup"><span data-stu-id="9a8d2-107">Prerequisite step</span></span>

<span data-ttu-id="9a8d2-108">Zkontrolujte typ brány virtuální sítě Azure:</span><span class="sxs-lookup"><span data-stu-id="9a8d2-108">Check the type of Azure  virtual network gateway:</span></span>

1. <span data-ttu-id="9a8d2-109">Přejděte na [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9a8d2-109">Go to [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="9a8d2-110">Zkontrolujte **přehled** stránky bránu virtuální sítě pro informace o typu.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-110">Check the **Overview** page of the virtual network gateway for the type information.</span></span>
    
    ![Přehled brány](media\vpn-gateway-troubleshoot-site-to-site-disconnected-intermittently\gatewayoverview.png)

### <a name="step-1-check-whether-the-on-premises-vpn-device-is-validated"></a><span data-ttu-id="9a8d2-112">Krok 1 Zkontrolujte, zda byl ověřen místního zařízení VPN</span><span class="sxs-lookup"><span data-stu-id="9a8d2-112">Step 1 Check whether the on-premises VPN device is validated</span></span>

1. <span data-ttu-id="9a8d2-113">Zkontrolujte, zda používáte [ověřit zařízení VPN a verzi operačního systému](vpn-gateway-about-vpn-devices.md#devicetable).</span><span class="sxs-lookup"><span data-stu-id="9a8d2-113">Check whether you are using a [validated VPN device and operating system version](vpn-gateway-about-vpn-devices.md#devicetable).</span></span> <span data-ttu-id="9a8d2-114">Pokud není ověřená zařízení VPN, budete muset obraťte se na výrobce zařízení a jestli jsou všechny potíže s kompatibilitou.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-114">If the VPN device is not validated, you may have to contact the device manufacturer to see if there is any compatibility issue.</span></span>
2. <span data-ttu-id="9a8d2-115">Ujistěte se, zda je správně nakonfigurováno zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-115">Make sure that the VPN device is correctly configured.</span></span> <span data-ttu-id="9a8d2-116">Další informace najdete v tématu [ukázky úpravy konfigurace zařízení](vpn-gateway-about-vpn-devices.md#editing).</span><span class="sxs-lookup"><span data-stu-id="9a8d2-116">For more information, see [Editing device configuration samples](vpn-gateway-about-vpn-devices.md#editing).</span></span>

### <a name="step-2-check-the-security-association-settingsfor-policy-based-azure-virtual-network-gateways"></a><span data-ttu-id="9a8d2-117">Krok 2 Zkontrolujte nastavení přidružení zabezpečení (pro brány na základě zásad Azure virtuální sítě)</span><span class="sxs-lookup"><span data-stu-id="9a8d2-117">Step 2 Check the Security Association settings(for policy-based Azure virtual network gateways)</span></span>

1. <span data-ttu-id="9a8d2-118">Ujistěte se, že virtuální sítě, podsítě a rozsahy v **brány místní sítě** definice v Microsoft Azure jsou stejné jako konfiguraci místního zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-118">Make sure that the virtual network, subnets and, ranges in the **Local network gateway** definition in Microsoft Azure are same as the configuration on the on-premises VPN device.</span></span>
2. <span data-ttu-id="9a8d2-119">Ověřte, že odpovídají nastavení přidružení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-119">Verify that the Security Association settings match.</span></span>

### <a name="step-3-check-for-user-defined-routes-or-network-security-groups-on-gateway-subnet"></a><span data-ttu-id="9a8d2-120">Krok 3 kontroly pro trasy definované uživatelem nebo skupiny zabezpečení sítě na podsíť brány</span><span class="sxs-lookup"><span data-stu-id="9a8d2-120">Step 3 Check for User-Defined Routes or Network Security Groups on Gateway Subnet</span></span>

<span data-ttu-id="9a8d2-121">Uživatelem definované trasy na podsíť brány mohou některé provozy, omezení a umožňuje ostatní provoz.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-121">A user-defined route on the gateway subnet may be restricting some traffic and allowing other traffic.</span></span> <span data-ttu-id="9a8d2-122">Díky tomu se zobrazí, že je připojení k síti VPN pro některé provozy, nespolehlivé a vhodný pro ostatní.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-122">This makes it appear that the VPN connection is unreliable for some traffic and good for others.</span></span> 

### <a name="step-4-check-the-one-vpn-tunnel-per-subnet-pair-setting-for-policy-based-virtual-network-gateways"></a><span data-ttu-id="9a8d2-123">Krok 4 kontrola "jeden tunelového připojení sítě VPN za pár podsítě" nastavení (pro brány virtuální sítě na základě zásad)</span><span class="sxs-lookup"><span data-stu-id="9a8d2-123">Step 4 Check the "one VPN Tunnel per Subnet Pair" setting (for policy-based virtual network gateways)</span></span>

<span data-ttu-id="9a8d2-124">Ujistěte se, že místního zařízení VPN je nastaven tak, aby měl **jeden tunelového připojení sítě VPN za pár podsítě** pro brány virtuální sítě na základě zásad.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-124">Make sure that the on-premises VPN device is set to have **one VPN tunnel per subnet pair** for policy-based virtual network gateways.</span></span>

### <a name="step-5-check-for-security-association-limitation-for-policy-based-virtual-network-gateways"></a><span data-ttu-id="9a8d2-125">Krok 5 Kontrola omezení přidružení zabezpečení (pro brány virtuální sítě na základě zásad)</span><span class="sxs-lookup"><span data-stu-id="9a8d2-125">Step 5 Check for Security Association Limitation (for policy-based virtual network gateways)</span></span>

<span data-ttu-id="9a8d2-126">Brána virtuální sítě založené na zásadách má limit 200 párů podsítě přidružení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-126">The Policy-based virtual network gateway has limit of 200 subnet Security Association pairs.</span></span> <span data-ttu-id="9a8d2-127">Pokud počet podsítí virtuální sítě Azure násobí časy podle počtu místní podsítě je větší než 200, najdete v části ojediněle podsítě odpojení.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-127">If the number of Azure virtual network subnets multiplied times by the number of local subnets is greater than 200, you see sporadic subnets disconnecting.</span></span>

### <a name="step-6-check-on-premises-vpn-device-external-interface-address"></a><span data-ttu-id="9a8d2-128">Krok 6 zkontrolujte místní adresa externí rozhraní zařízení VPN</span><span class="sxs-lookup"><span data-stu-id="9a8d2-128">Step 6 Check on-premises VPN device external interface address</span></span>

- <span data-ttu-id="9a8d2-129">Pokud je součástí Internetové IP adresa zařízení VPN **brány místní sítě** definice v Azure, se může vyskytnout občasné odpojení.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-129">If the Internet facing IP address of the VPN device is included in the **Local network gateway** definition in Azure, you may experience sporadic disconnections.</span></span>
- <span data-ttu-id="9a8d2-130">Externí rozhraní zařízení musí být přímo na Internetu.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-130">The device's external interface must be directly on the Internet.</span></span> <span data-ttu-id="9a8d2-131">Měla by existovat žádné překlad adres (NAT) nebo brány firewall mezi Internetu a zařízení.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-131">There should be no Network Address Translation (NAT) or firewall between the Internet and the device.</span></span>
-  <span data-ttu-id="9a8d2-132">Pokud nakonfigurujete Clustering brány Firewall tak, aby měl virtuální IP adresy, musí přerušení clusteru a vystavit zařízení VPN přímo k veřejné rozhraní, které můžete bránu rozhraní s.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-132">If you configure Firewall Clustering to have a virtual IP, you must break the cluster and expose the VPN appliance directly to a public interface that the gateway can interface with.</span></span>

### <a name="step-7-check-whether-the-on-premises-vpn-device-has-perfect-forward-secrecy-enabled"></a><span data-ttu-id="9a8d2-133">Krok 7 zkontrolujte, jestli má metoda Perfect Forward Secrecy povoleno místního zařízení VPN</span><span class="sxs-lookup"><span data-stu-id="9a8d2-133">Step 7 Check whether the on-premises VPN device has Perfect Forward Secrecy enabled</span></span>

<span data-ttu-id="9a8d2-134">**Metoda Perfect Forward Secrecy** funkce může způsobit problémy odpojení.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-134">The **Perfect Forward Secrecy** feature can cause the disconnection problems.</span></span> <span data-ttu-id="9a8d2-135">Pokud má zařízení VPN **ideální přeposílání** povolit, zakázat funkci.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-135">If the VPN device has **Perfect forward Secrecy** enabled, disable the feature.</span></span> <span data-ttu-id="9a8d2-136">Potom [aktualizovat bránu virtuální sítě zásad protokolu IPsec](vpn-gateway-ipsecikepolicy-rm-powershell.md#managepolicy).</span><span class="sxs-lookup"><span data-stu-id="9a8d2-136">Then [update the virtual network gateway IPsec policy](vpn-gateway-ipsecikepolicy-rm-powershell.md#managepolicy).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a8d2-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9a8d2-137">Next steps</span></span>

- [<span data-ttu-id="9a8d2-138">Konfigurace připojení typu Site-to-Site k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="9a8d2-138">Configure a Site-to-Site connection to a virtual network</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [<span data-ttu-id="9a8d2-139">Konfigurace zásad protokolu IPsec/IKE pro připojení VPN typu Site-to-Site</span><span class="sxs-lookup"><span data-stu-id="9a8d2-139">Configure IPsec/IKE policy for Site-to-Site VPN connections</span></span>](vpn-gateway-ipsecikepolicy-rm-powershell.md)

