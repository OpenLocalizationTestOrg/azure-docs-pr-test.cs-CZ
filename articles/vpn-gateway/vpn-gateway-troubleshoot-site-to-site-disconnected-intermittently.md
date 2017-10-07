---
title: "Azure Site-to-Site VPN odpojí občas aaaTroubleshoot | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot hello problém v které připojení Site-to-Site VPN hello pravidelně odpojen."
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
ms.openlocfilehash: 1ce3c4ff9d8f650312e45f33b760ebcc6597fc13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-site-to-site-vpn-disconnects-intermittently"></a><span data-ttu-id="be16d-103">Řešení potíží: Azure Site-to-Site VPN odpojí občas</span><span class="sxs-lookup"><span data-stu-id="be16d-103">Troubleshooting: Azure Site-to-Site VPN disconnects intermittently</span></span>

<span data-ttu-id="be16d-104">Může dojít k hello problém, že nové nebo existující připojení VPN v Microsoft Azure Point-to-Site není stabilní nebo odpojí pravidelně.</span><span class="sxs-lookup"><span data-stu-id="be16d-104">You might experience hello problem that a new or existing Microsoft Azure Point-to-Site VPN connection is not stable or disconnects regularly.</span></span> <span data-ttu-id="be16d-105">Tento článek obsahuje kroky toohelp identifikovat a řešit hello příčinu hello problém vyřešit.</span><span class="sxs-lookup"><span data-stu-id="be16d-105">This article provides troubleshoot steps toohelp you identify and resolve hello cause of hello problem.</span></span> 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a><span data-ttu-id="be16d-106">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="be16d-106">Troubleshooting steps</span></span>

### <a name="prerequisite-step"></a><span data-ttu-id="be16d-107">Požadovaný krok</span><span class="sxs-lookup"><span data-stu-id="be16d-107">Prerequisite step</span></span>

<span data-ttu-id="be16d-108">Zkontrolujte hello typ brány virtuální sítě Azure:</span><span class="sxs-lookup"><span data-stu-id="be16d-108">Check hello type of Azure  virtual network gateway:</span></span>

1. <span data-ttu-id="be16d-109">Přejděte příliš[portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="be16d-109">Go too[Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="be16d-110">Zkontrolujte hello **přehled** stránku hello brány virtuální sítě pro informace o typu hello.</span><span class="sxs-lookup"><span data-stu-id="be16d-110">Check hello **Overview** page of hello virtual network gateway for hello type information.</span></span>
    
    ![Přehled Hello hello brány](media\vpn-gateway-troubleshoot-site-to-site-disconnected-intermittently\gatewayoverview.png)

### <a name="step-1-check-whether-hello-on-premises-vpn-device-is-validated"></a><span data-ttu-id="be16d-112">Krok 1 Zkontrolujte, zda text hello místního zařízení VPN se ověří</span><span class="sxs-lookup"><span data-stu-id="be16d-112">Step 1 Check whether hello on-premises VPN device is validated</span></span>

1. <span data-ttu-id="be16d-113">Zkontrolujte, zda používáte [ověřit zařízení VPN a verzi operačního systému](vpn-gateway-about-vpn-devices.md#devicetable).</span><span class="sxs-lookup"><span data-stu-id="be16d-113">Check whether you are using a [validated VPN device and operating system version](vpn-gateway-about-vpn-devices.md#devicetable).</span></span> <span data-ttu-id="be16d-114">Pokud není ověřená zařízení VPN hello, pokud neexistuje žádné potíže s kompatibilitou mohou mít toocontact hello zařízení výrobce toosee.</span><span class="sxs-lookup"><span data-stu-id="be16d-114">If hello VPN device is not validated, you may have toocontact hello device manufacturer toosee if there is any compatibility issue.</span></span>
2. <span data-ttu-id="be16d-115">Ujistěte se, že toto zařízení VPN hello je správně nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="be16d-115">Make sure that hello VPN device is correctly configured.</span></span> <span data-ttu-id="be16d-116">Další informace najdete v tématu [ukázky úpravy konfigurace zařízení](vpn-gateway-about-vpn-devices.md#editing).</span><span class="sxs-lookup"><span data-stu-id="be16d-116">For more information, see [Editing device configuration samples](vpn-gateway-about-vpn-devices.md#editing).</span></span>

### <a name="step-2-check-hello-security-association-settingsfor-policy-based-azure-virtual-network-gateways"></a><span data-ttu-id="be16d-117">Krok 2 nastavení kontroly hello přidružení zabezpečení (pro brány na základě zásad Azure virtuální sítě)</span><span class="sxs-lookup"><span data-stu-id="be16d-117">Step 2 Check hello Security Association settings(for policy-based Azure virtual network gateways)</span></span>

1. <span data-ttu-id="be16d-118">Ujistěte se, že hello virtuální sítě, podsítě a rozsahy v hello **brány místní sítě** definice v Microsoft Azure jsou stejné jako hello konfigurace v zařízení VPN místní hello.</span><span class="sxs-lookup"><span data-stu-id="be16d-118">Make sure that hello virtual network, subnets and, ranges in hello **Local network gateway** definition in Microsoft Azure are same as hello configuration on hello on-premises VPN device.</span></span>
2. <span data-ttu-id="be16d-119">Ověřte, které odpovídají nastavení hello přidružení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="be16d-119">Verify that hello Security Association settings match.</span></span>

### <a name="step-3-check-for-user-defined-routes-or-network-security-groups-on-gateway-subnet"></a><span data-ttu-id="be16d-120">Krok 3 kontroly pro trasy definované uživatelem nebo skupiny zabezpečení sítě na podsíť brány</span><span class="sxs-lookup"><span data-stu-id="be16d-120">Step 3 Check for User-Defined Routes or Network Security Groups on Gateway Subnet</span></span>

<span data-ttu-id="be16d-121">Uživatelem definované trasy na podsíť brány hello mohou některé provozy, omezení a umožňuje ostatní provoz.</span><span class="sxs-lookup"><span data-stu-id="be16d-121">A user-defined route on hello gateway subnet may be restricting some traffic and allowing other traffic.</span></span> <span data-ttu-id="be16d-122">Díky tomu se zobrazí, zda je připojení k síti VPN hello nespolehlivé pro některé provozy a vhodný pro ostatní.</span><span class="sxs-lookup"><span data-stu-id="be16d-122">This makes it appear that hello VPN connection is unreliable for some traffic and good for others.</span></span> 

### <a name="step-4-check-hello-one-vpn-tunnel-per-subnet-pair-setting-for-policy-based-virtual-network-gateways"></a><span data-ttu-id="be16d-123">Krok 4 kontrola hello "jeden tunelového připojení sítě VPN za pár podsítě" nastavení (pro brány virtuální sítě na základě zásad)</span><span class="sxs-lookup"><span data-stu-id="be16d-123">Step 4 Check hello "one VPN Tunnel per Subnet Pair" setting (for policy-based virtual network gateways)</span></span>

<span data-ttu-id="be16d-124">Ujistěte se, že hello místní zařízení VPN je nastaven toohave **jeden tunelového připojení sítě VPN za pár podsítě** pro brány virtuální sítě na základě zásad.</span><span class="sxs-lookup"><span data-stu-id="be16d-124">Make sure that hello on-premises VPN device is set toohave **one VPN tunnel per subnet pair** for policy-based virtual network gateways.</span></span>

### <a name="step-5-check-for-security-association-limitation-for-policy-based-virtual-network-gateways"></a><span data-ttu-id="be16d-125">Krok 5 Kontrola omezení přidružení zabezpečení (pro brány virtuální sítě na základě zásad)</span><span class="sxs-lookup"><span data-stu-id="be16d-125">Step 5 Check for Security Association Limitation (for policy-based virtual network gateways)</span></span>

<span data-ttu-id="be16d-126">brány založené na zásadách virtuální sítě Hello má limit 200 párů podsítě přidružení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="be16d-126">hello Policy-based virtual network gateway has limit of 200 subnet Security Association pairs.</span></span> <span data-ttu-id="be16d-127">Pokud hello počet podsítí virtuální sítě Azure násobí časy podle hello počet místní podsítě je větší než 200, najdete v části ojediněle podsítě odpojení.</span><span class="sxs-lookup"><span data-stu-id="be16d-127">If hello number of Azure virtual network subnets multiplied times by hello number of local subnets is greater than 200, you see sporadic subnets disconnecting.</span></span>

### <a name="step-6-check-on-premises-vpn-device-external-interface-address"></a><span data-ttu-id="be16d-128">Krok 6 zkontrolujte místní adresa externí rozhraní zařízení VPN</span><span class="sxs-lookup"><span data-stu-id="be16d-128">Step 6 Check on-premises VPN device external interface address</span></span>

- <span data-ttu-id="be16d-129">Pokud hello internetové IP adresa zařízení VPN hello je součástí hello **brány místní sítě** definice v Azure, se může vyskytnout občasné odpojení.</span><span class="sxs-lookup"><span data-stu-id="be16d-129">If hello Internet facing IP address of hello VPN device is included in hello **Local network gateway** definition in Azure, you may experience sporadic disconnections.</span></span>
- <span data-ttu-id="be16d-130">Hello externí rozhraní zařízení musí být přímo na hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="be16d-130">hello device's external interface must be directly on hello Internet.</span></span> <span data-ttu-id="be16d-131">Měla by existovat žádné překlad adres (NAT) nebo brány firewall mezi hello Internet a hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="be16d-131">There should be no Network Address Translation (NAT) or firewall between hello Internet and hello device.</span></span>
-  <span data-ttu-id="be16d-132">Pokud nakonfigurujete Clustering brány Firewall toohave virtuální IP adresy, musí přerušení hello clusteru a vystavit zařízení VPN hello přímo tooa veřejné rozhraní, která hello brány můžete rozhraní s.</span><span class="sxs-lookup"><span data-stu-id="be16d-132">If you configure Firewall Clustering toohave a virtual IP, you must break hello cluster and expose hello VPN appliance directly tooa public interface that hello gateway can interface with.</span></span>

### <a name="step-7-check-whether-hello-on-premises-vpn-device-has-perfect-forward-secrecy-enabled"></a><span data-ttu-id="be16d-133">Krok 7 zkontrolujte, zda hello místního zařízení VPN má metoda Perfect Forward Secrecy povoleno</span><span class="sxs-lookup"><span data-stu-id="be16d-133">Step 7 Check whether hello on-premises VPN device has Perfect Forward Secrecy enabled</span></span>

<span data-ttu-id="be16d-134">Hello **metoda Perfect Forward Secrecy** funkce může způsobit problémy odpojení hello.</span><span class="sxs-lookup"><span data-stu-id="be16d-134">hello **Perfect Forward Secrecy** feature can cause hello disconnection problems.</span></span> <span data-ttu-id="be16d-135">Pokud má zařízení VPN hello **ideální přeposílání** hello funkci povolit, zakázat.</span><span class="sxs-lookup"><span data-stu-id="be16d-135">If hello VPN device has **Perfect forward Secrecy** enabled, disable hello feature.</span></span> <span data-ttu-id="be16d-136">Potom [aktualizovat zásady IPsec brány virtuální sítě hello](vpn-gateway-ipsecikepolicy-rm-powershell.md#managepolicy).</span><span class="sxs-lookup"><span data-stu-id="be16d-136">Then [update hello virtual network gateway IPsec policy](vpn-gateway-ipsecikepolicy-rm-powershell.md#managepolicy).</span></span>

## <a name="next-steps"></a><span data-ttu-id="be16d-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="be16d-137">Next steps</span></span>

- [<span data-ttu-id="be16d-138">Konfigurace virtuální sítě tooa připojení Site-to-Site</span><span class="sxs-lookup"><span data-stu-id="be16d-138">Configure a Site-to-Site connection tooa virtual network</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [<span data-ttu-id="be16d-139">Konfigurace zásad protokolu IPsec/IKE pro připojení VPN typu Site-to-Site</span><span class="sxs-lookup"><span data-stu-id="be16d-139">Configure IPsec/IKE policy for Site-to-Site VPN connections</span></span>](vpn-gateway-ipsecikepolicy-rm-powershell.md)

