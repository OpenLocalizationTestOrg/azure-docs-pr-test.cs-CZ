---
title: "Konfigurace aaaSample – zařízení Cisco ASA připojení brány VPN tooAzure | Microsoft Docs"
description: "Tento článek obsahuje ukázková konfigurace pro zařízení Cisco ASA připojení brány VPN tooAzure."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: yushwang
ms.openlocfilehash: dad13e02afe8dad2379db750eb09602e08e8ea99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sample-configuration-cisco-asa-device-ikev2no-bgp"></a><span data-ttu-id="e134c-103">Ukázková konfigurace: Cisco ASA zařízení (BGP IKEv2/ne)</span><span class="sxs-lookup"><span data-stu-id="e134c-103">Sample configuration: Cisco ASA device (IKEv2/no BGP)</span></span>
<span data-ttu-id="e134c-104">Tento článek obsahuje ukázkové konfigurace pro zařízení Cisco ASA připojení brány VPN tooAzure.</span><span class="sxs-lookup"><span data-stu-id="e134c-104">This article provides sample configurations for Cisco ASA devices connecting tooAzure VPN gateways.</span></span>

## <a name="device-at-a-glance"></a><span data-ttu-id="e134c-105">Zařízení na první pohled</span><span class="sxs-lookup"><span data-stu-id="e134c-105">Device at a glance</span></span>

|                        |                                   |
| ---                    | ---                               |
| <span data-ttu-id="e134c-106">Výrobce zařízení</span><span class="sxs-lookup"><span data-stu-id="e134c-106">Device vendor</span></span>          | <span data-ttu-id="e134c-107">Cisco</span><span class="sxs-lookup"><span data-stu-id="e134c-107">Cisco</span></span>                             |
| <span data-ttu-id="e134c-108">Model zařízení</span><span class="sxs-lookup"><span data-stu-id="e134c-108">Device model</span></span>           | <span data-ttu-id="e134c-109">ASA (adaptivní zabezpečení zařízení)</span><span class="sxs-lookup"><span data-stu-id="e134c-109">ASA (Adaptive Security Appliance)</span></span> |
| <span data-ttu-id="e134c-110">Cílová verze</span><span class="sxs-lookup"><span data-stu-id="e134c-110">Target version</span></span>         | <span data-ttu-id="e134c-111">8.4+</span><span class="sxs-lookup"><span data-stu-id="e134c-111">8.4+</span></span>                              |
| <span data-ttu-id="e134c-112">Otestované modelu</span><span class="sxs-lookup"><span data-stu-id="e134c-112">Tested model</span></span>           | <span data-ttu-id="e134c-113">ASA 5505</span><span class="sxs-lookup"><span data-stu-id="e134c-113">ASA 5505</span></span>                          |
| <span data-ttu-id="e134c-114">Otestované verze</span><span class="sxs-lookup"><span data-stu-id="e134c-114">Tested version</span></span>         | <span data-ttu-id="e134c-115">9.2</span><span class="sxs-lookup"><span data-stu-id="e134c-115">9.2</span></span>                               |
| <span data-ttu-id="e134c-116">Verze IKE</span><span class="sxs-lookup"><span data-stu-id="e134c-116">IKE version</span></span>            | <span data-ttu-id="e134c-117">**IKEv2**</span><span class="sxs-lookup"><span data-stu-id="e134c-117">**IKEv2**</span></span>                         |
| <span data-ttu-id="e134c-118">Protokol BGP</span><span class="sxs-lookup"><span data-stu-id="e134c-118">BGP</span></span>                    | <span data-ttu-id="e134c-119">**Ne**</span><span class="sxs-lookup"><span data-stu-id="e134c-119">**No**</span></span>                            |
| <span data-ttu-id="e134c-120">Typ brány Azure VPN</span><span class="sxs-lookup"><span data-stu-id="e134c-120">Azure VPN gateway type</span></span> | <span data-ttu-id="e134c-121">**Založené na trasách** brána sítě VPN</span><span class="sxs-lookup"><span data-stu-id="e134c-121">**Route-based** VPN gateway</span></span>       |
|                        |                                   |

> [!NOTE]
> 1. <span data-ttu-id="e134c-122">Hello konfigurace níže se připojuje tooan zařízení Cisco ASA Azure **založené na trasách** VPN gateway pomocí vlastních zásad protokolu IPsec/IKE s možností "UserPolicyBasedTrafficSelectors", jak je popsáno v [v tomto článku](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="e134c-122">hello configuration below connects a Cisco ASA device tooan Azure **route-based** VPN gateway using custom IPsec/IKE policy with "UserPolicyBasedTrafficSelectors" option, as described in [this article](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span></span>
> 2. <span data-ttu-id="e134c-123">Vyžaduje ASA zařízení toouse **IKEv2** s konfigurací na základě přístup k seznamu není na základě VTI.</span><span class="sxs-lookup"><span data-stu-id="e134c-123">It requires ASA devices toouse **IKEv2** with access-list-based configurations, not VTI-based.</span></span>
> 3. <span data-ttu-id="e134c-124">Přečtěte si vaše specifikace dodavatele zařízení VPN, které zásady tooensure hello je podporované na vaše místní zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="e134c-124">Please consult with your VPN device vendor specifications tooensure hello policy is supported on your on-premises VPN devices.</span></span>

## <a name="vpn-device-requirements"></a><span data-ttu-id="e134c-125">Požadavky na zařízení VPN</span><span class="sxs-lookup"><span data-stu-id="e134c-125">VPN device requirements</span></span>
<span data-ttu-id="e134c-126">Azure VPN Gateway pomocí standardního protokolu IPsec/IKE protokol sady tooestablish S2S VPN tunely.</span><span class="sxs-lookup"><span data-stu-id="e134c-126">Azure VPN gateways use standard IPsec/IKE protocol suites tooestablish S2S VPN tunnels.</span></span> <span data-ttu-id="e134c-127">Odkazovat příliš[informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md) hello podrobné parametry protokolu IPsec/IKE a výchozí kryptografické algoritmy pro Azure VPN Gateway.</span><span class="sxs-lookup"><span data-stu-id="e134c-127">Refer too[About VPN devices](vpn-gateway-about-vpn-devices.md) for hello detailed IPsec/IKE protocol parameters and default cryptographic algorithms for Azure VPN gateways.</span></span> <span data-ttu-id="e134c-128">Volitelně můžete zadat přesný kombinace hello kryptografické algoritmy a klíče síly pro konkrétní připojení jak je popsáno v [o požadavcích na kryptografických](vpn-gateway-about-compliance-crypto.md).</span><span class="sxs-lookup"><span data-stu-id="e134c-128">You can optionally specify hello exact combination of cryptographic algorithms and key strengths for a specific connection as described in [About cryptographic requirements](vpn-gateway-about-compliance-crypto.md).</span></span> <span data-ttu-id="e134c-129">Pokud vyberete konkrétní kombinaci kryptografické algoritmy a klíče síly, Zkontrolujte prosím, že používáte odpovídající specifikace hello na vaše zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="e134c-129">If you select a specific combination of cryptographic algorithms and key strengths, please make sure you use hello corresponding specifications on your VPN devices.</span></span>

## <a name="single-vpn-tunnel"></a><span data-ttu-id="e134c-130">Jedno tunelové propojení sítě VPN</span><span class="sxs-lookup"><span data-stu-id="e134c-130">Single VPN tunnel</span></span>
<span data-ttu-id="e134c-131">Tato topologie se skládá z jednoho tunelu S2S VPN mezi bránu VPN Azure VPN a vaše místní zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="e134c-131">This topology consists of a single S2S VPN tunnel between an Azure VPN gateway and your on-premises VPN device.</span></span> <span data-ttu-id="e134c-132">Volitelně můžete nakonfigurovat protokol BGP mezi hello tunelového připojení sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="e134c-132">You can optionally configure BGP across hello VPN tunnel.</span></span>

![jedno tunelové propojení](./media/vpn-gateway-3rdparty-device-config-cisco-asa/singletunnel.png)

<span data-ttu-id="e134c-134">Odkazovat příliš[jedním tunelem instalace](vpn-gateway-3rdparty-device-config-overview.md#singletunnel) podrobné naleznete podrobné pokyny toobuild hello konfigurace Azure.</span><span class="sxs-lookup"><span data-stu-id="e134c-134">Refer too[Single tunnel setup](vpn-gateway-3rdparty-device-config-overview.md#singletunnel) for detailed, step-by-step instructions toobuild hello Azure configurations.</span></span>

### <a name="network-and-vpn-gateway-information"></a><span data-ttu-id="e134c-135">Informace o bráně sítě a sítě VPN</span><span class="sxs-lookup"><span data-stu-id="e134c-135">Network and VPN gateway information</span></span>
<span data-ttu-id="e134c-136">V této části seznamu hello parametry pro hello této ukázce.</span><span class="sxs-lookup"><span data-stu-id="e134c-136">This section list hello parameters for hello this sample.</span></span>

| <span data-ttu-id="e134c-137">**Parametr**</span><span class="sxs-lookup"><span data-stu-id="e134c-137">**Parameter**</span></span>                | <span data-ttu-id="e134c-138">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="e134c-138">**Value**</span></span>                    |
| ---                          | ---                          |
| <span data-ttu-id="e134c-139">Předpony adres sítí VNet</span><span class="sxs-lookup"><span data-stu-id="e134c-139">VNet address prefixes</span></span>        | <span data-ttu-id="e134c-140">10.11.0.0/16</span><span class="sxs-lookup"><span data-stu-id="e134c-140">10.11.0.0/16</span></span><br><span data-ttu-id="e134c-141">10.12.0.0/16</span><span class="sxs-lookup"><span data-stu-id="e134c-141">10.12.0.0/16</span></span> |
| <span data-ttu-id="e134c-142">Azure IP adresa brány sítě VPN</span><span class="sxs-lookup"><span data-stu-id="e134c-142">Azure VPN gateway IP</span></span>         | <span data-ttu-id="e134c-143">Azure_Gateway_Public_IP</span><span class="sxs-lookup"><span data-stu-id="e134c-143">Azure_Gateway_Public_IP</span></span>      |
| <span data-ttu-id="e134c-144">Předpony adres místní</span><span class="sxs-lookup"><span data-stu-id="e134c-144">On-premises address prefixes</span></span> | <span data-ttu-id="e134c-145">10.51.0.0/16</span><span class="sxs-lookup"><span data-stu-id="e134c-145">10.51.0.0/16</span></span><br><span data-ttu-id="e134c-146">10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="e134c-146">10.52.0.0/16</span></span> |
| <span data-ttu-id="e134c-147">IP adresa zařízení místní sítě VPN</span><span class="sxs-lookup"><span data-stu-id="e134c-147">On-premises VPN device IP</span></span>    | <span data-ttu-id="e134c-148">OnPrem_Device_Public_IP</span><span class="sxs-lookup"><span data-stu-id="e134c-148">OnPrem_Device_Public_IP</span></span>     |
| <span data-ttu-id="e134c-149">* Protokol BGP ASN virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="e134c-149">*VNet BGP ASN</span></span>                | <span data-ttu-id="e134c-150">65010</span><span class="sxs-lookup"><span data-stu-id="e134c-150">65010</span></span>                        |
| <span data-ttu-id="e134c-151">* Azure IP adresa partnera BGP</span><span class="sxs-lookup"><span data-stu-id="e134c-151">*Azure BGP peer IP</span></span>           | <span data-ttu-id="e134c-152">10.12.255.30</span><span class="sxs-lookup"><span data-stu-id="e134c-152">10.12.255.30</span></span>                 |
| <span data-ttu-id="e134c-153">* Místní ASN protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="e134c-153">*On-premises BGP ASN</span></span>         | <span data-ttu-id="e134c-154">65050</span><span class="sxs-lookup"><span data-stu-id="e134c-154">65050</span></span>                        |
| <span data-ttu-id="e134c-155">* IP adresa partnera BGP na místě</span><span class="sxs-lookup"><span data-stu-id="e134c-155">*On-premises BGP peer IP</span></span>     | <span data-ttu-id="e134c-156">10.52.255.254</span><span class="sxs-lookup"><span data-stu-id="e134c-156">10.52.255.254</span></span>                |
|                              |                              |

* <span data-ttu-id="e134c-157">(*) Volitelné parametry pro protokol BGP jenom.</span><span class="sxs-lookup"><span data-stu-id="e134c-157">(*) Optional parameters for BGP only.</span></span>

### <a name="ipsecike-policy--parameters"></a><span data-ttu-id="e134c-158">Zásady protokolu IPsec/IKE a parametry</span><span class="sxs-lookup"><span data-stu-id="e134c-158">IPsec/IKE policy & parameters</span></span>

<span data-ttu-id="e134c-159">Následující tabulka Hello uvádí algoritmy hello protokolu IPsec/IKE a parametry použité v ukázce hello.</span><span class="sxs-lookup"><span data-stu-id="e134c-159">hello table below lists hello IPsec/IKE algorithms and parameters used in hello sample.</span></span> <span data-ttu-id="e134c-160">Přečtěte si vaše se, že všechny algoritmy, které jsou uvedené výše jsou podporována modely zařízení VPN a verzí firmwaru VPN toomake specifikace zařízení.</span><span class="sxs-lookup"><span data-stu-id="e134c-160">Please consult your VPN device specifications toomake sure all algorithms listed above are supported by your VPN device models and firmware versions.</span></span>

| <span data-ttu-id="e134c-161">**IPsec/IKEv2**</span><span class="sxs-lookup"><span data-stu-id="e134c-161">**IPsec/IKEv2**</span></span>  | <span data-ttu-id="e134c-162">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="e134c-162">**Value**</span></span>                            |
| ---              | ---                                  |
| <span data-ttu-id="e134c-163">Šifrování protokolem IKEv2</span><span class="sxs-lookup"><span data-stu-id="e134c-163">IKEv2 Encryption</span></span> | <span data-ttu-id="e134c-164">AES256</span><span class="sxs-lookup"><span data-stu-id="e134c-164">AES256</span></span>                               |
| <span data-ttu-id="e134c-165">Integrita protokolu IKEv2</span><span class="sxs-lookup"><span data-stu-id="e134c-165">IKEv2 Integrity</span></span>  | <span data-ttu-id="e134c-166">SHA384</span><span class="sxs-lookup"><span data-stu-id="e134c-166">SHA384</span></span>                               |
| <span data-ttu-id="e134c-167">Skupina DH</span><span class="sxs-lookup"><span data-stu-id="e134c-167">DH Group</span></span>         | <span data-ttu-id="e134c-168">DHGroup24</span><span class="sxs-lookup"><span data-stu-id="e134c-168">DHGroup24</span></span>                            |
| <span data-ttu-id="e134c-169">Šifrování protokolem IPsec</span><span class="sxs-lookup"><span data-stu-id="e134c-169">IPsec Encryption</span></span> | <span data-ttu-id="e134c-170">AES256</span><span class="sxs-lookup"><span data-stu-id="e134c-170">AES256</span></span>                               |
| <span data-ttu-id="e134c-171">Integrita protokolu IPsec</span><span class="sxs-lookup"><span data-stu-id="e134c-171">IPsec Integrity</span></span>  | <span data-ttu-id="e134c-172">SHA1</span><span class="sxs-lookup"><span data-stu-id="e134c-172">SHA1</span></span>                                 |
| <span data-ttu-id="e134c-173">Skupina PFS</span><span class="sxs-lookup"><span data-stu-id="e134c-173">PFS Group</span></span>        | <span data-ttu-id="e134c-174">PFS24</span><span class="sxs-lookup"><span data-stu-id="e134c-174">PFS24</span></span>                                |
| <span data-ttu-id="e134c-175">Doba života přidružení zabezpečení v rychlém režimu</span><span class="sxs-lookup"><span data-stu-id="e134c-175">QM SA Lifetime</span></span>   | <span data-ttu-id="e134c-176">7200 sekund</span><span class="sxs-lookup"><span data-stu-id="e134c-176">7200 seconds</span></span>                         |
| <span data-ttu-id="e134c-177">Selektor provozu</span><span class="sxs-lookup"><span data-stu-id="e134c-177">Traffic Selector</span></span> | <span data-ttu-id="e134c-178">UsePolicyBasedTrafficSelectors $True</span><span class="sxs-lookup"><span data-stu-id="e134c-178">UsePolicyBasedTrafficSelectors $True</span></span> |
| <span data-ttu-id="e134c-179">Předsdílený klíč</span><span class="sxs-lookup"><span data-stu-id="e134c-179">Pre-Shared Key</span></span>   | <span data-ttu-id="e134c-180">PreSharedKey</span><span class="sxs-lookup"><span data-stu-id="e134c-180">PreSharedKey</span></span>                         |
|                  |                                      |

- <span data-ttu-id="e134c-181">(*) Na některých zařízeních musí být integrity protokolu IPsec "null", pokud GCM AES se používá jako šifrovací algoritmus protokolu IPsec.</span><span class="sxs-lookup"><span data-stu-id="e134c-181">(*) On some device, IPsec integrity must be "null" if GCM-AES is used as IPsec encryption algorithm.</span></span>

### <a name="device-notes"></a><span data-ttu-id="e134c-182">Poznámky k zařízení</span><span class="sxs-lookup"><span data-stu-id="e134c-182">Device notes</span></span>

>[!NOTE]
>
> 1. <span data-ttu-id="e134c-183">Má podporu protokolu IKEv2 vyžaduje ASA verze 8.4 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="e134c-183">IKEv2 support requires ASA version 8.4 and above.</span></span>
> 2. <span data-ttu-id="e134c-184">Podpora skupiny DH vyšší a PFS (kromě skupiny 5) vyžaduje verzi ASA 9.x.</span><span class="sxs-lookup"><span data-stu-id="e134c-184">Higher DH and PFS group support (beyond Group 5) requires ASA version 9.x.</span></span>
> 3. <span data-ttu-id="e134c-185">Šifrování pomocí protokolu IPsec s AES-GCM a protokolu IPsec integrita s SHA-256, SHA-384 a SHA-512 podporu vyžaduje verzi ASA 9.x na novější ASA hardwaru; 5505 ASA, 5510, 5520, 5540, 5550, jsou 5580 **není** podporována.</span><span class="sxs-lookup"><span data-stu-id="e134c-185">IPsec encryption with AES-GCM and IPsec integrity with SHA-256, SHA-384, SHA-512 support requires ASA version 9.x on newer ASA hardware; ASA 5505, 5510, 5520, 5540, 5550, 5580 are **not** supported.</span></span> <span data-ttu-id="e134c-186">(Zkontrolujte hello dodavatele specifikace tooconfirm.)</span><span class="sxs-lookup"><span data-stu-id="e134c-186">(Please check hello vendor specifications tooconfirm.)</span></span>
>


### <a name="sample-device-configurations"></a><span data-ttu-id="e134c-187">Ukázka konfigurace zařízení</span><span class="sxs-lookup"><span data-stu-id="e134c-187">Sample device configurations</span></span>
<span data-ttu-id="e134c-188">níže uvedený skript Hello poskytuje ukázkové konfiguraci podle hello topologie a parametry uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="e134c-188">hello script below provides a sample configuration based on hello topology and parameters listed above.</span></span> <span data-ttu-id="e134c-189">Konfigurace tunel S2S VPN Hello se skládá z hello následující části:</span><span class="sxs-lookup"><span data-stu-id="e134c-189">hello S2S VPN tunnel configuration consists of hello following parts:</span></span>

1. <span data-ttu-id="e134c-190">Rozhraní & trasy</span><span class="sxs-lookup"><span data-stu-id="e134c-190">Interfaces & routes</span></span>
2. <span data-ttu-id="e134c-191">Seznamy přístupu</span><span class="sxs-lookup"><span data-stu-id="e134c-191">Access lists</span></span>
3. <span data-ttu-id="e134c-192">Zásady IKE a parametry (fáze 1 nebo hlavního režimu)</span><span class="sxs-lookup"><span data-stu-id="e134c-192">IKE policy and parameters (Phase 1 or Main Mode)</span></span>
4. <span data-ttu-id="e134c-193">Parametry (fáze 2 nebo rychlého režimu) a zásad protokolu IPsec</span><span class="sxs-lookup"><span data-stu-id="e134c-193">IPsec policy and parameters (Phase 2 or Quick Mode)</span></span>
5. <span data-ttu-id="e134c-194">Další parametry (upínací MSS protokolu TCP, atd.)</span><span class="sxs-lookup"><span data-stu-id="e134c-194">Other parameters (TCP MSS clamping, etc.)</span></span>

>[!IMPORTANT] 
><span data-ttu-id="e134c-195">Zkontrolujte prosím, že dokončení hello níže uvedené další konfigurace a nahraďte zástupné symboly hello hello skutečnými hodnotami:</span><span class="sxs-lookup"><span data-stu-id="e134c-195">Please make sure you complete hello additional configuration listed below and replace hello placeholders with hello actual values:</span></span>
> 
> - <span data-ttu-id="e134c-196">Konfigurace rozhraní pro i mimo ně rozhraní</span><span class="sxs-lookup"><span data-stu-id="e134c-196">Interface configuration for both inside and outside interfaces</span></span>
> - <span data-ttu-id="e134c-197">Trasy pro vaše uvnitř a privátního a mimo nebo veřejné sítě</span><span class="sxs-lookup"><span data-stu-id="e134c-197">Routes for your inside/private and outside/public networks</span></span>
> - <span data-ttu-id="e134c-198">Zajistěte, všechny názvy a zásad čísla jedinečná na zařízení hello</span><span class="sxs-lookup"><span data-stu-id="e134c-198">Ensure all names and policy numbers are unique on hello device</span></span>
> - <span data-ttu-id="e134c-199">Ujistěte se, že hello kryptografické algoritmy jsou podporovány ve vašem zařízení</span><span class="sxs-lookup"><span data-stu-id="e134c-199">Ensure hello cryptographic algorithms are supported on your device</span></span>
> - <span data-ttu-id="e134c-200">Nahraďte hello následující zástupného skutečnými hodnotami hello</span><span class="sxs-lookup"><span data-stu-id="e134c-200">Replace hello following place holders with hello actual values</span></span>
>   - <span data-ttu-id="e134c-201">Mimo název rozhraní: "mimo"</span><span class="sxs-lookup"><span data-stu-id="e134c-201">Outside interface name: "outside"</span></span>
>   - <span data-ttu-id="e134c-202">Azure_Gateway_Public_IP</span><span class="sxs-lookup"><span data-stu-id="e134c-202">Azure_Gateway_Public_IP</span></span>
>   - <span data-ttu-id="e134c-203">OnPrem_Device_Public_IP</span><span class="sxs-lookup"><span data-stu-id="e134c-203">OnPrem_Device_Public_IP</span></span>
>   - <span data-ttu-id="e134c-204">IKE Pre_Shared_Key</span><span class="sxs-lookup"><span data-stu-id="e134c-204">IKE Pre_Shared_Key</span></span>
>   - <span data-ttu-id="e134c-205">Virtuální síť a názvy brány místní sítě (VNetName, LNGName)</span><span class="sxs-lookup"><span data-stu-id="e134c-205">VNet and local network gateway names (VNetName, LNGName)</span></span>
>   - <span data-ttu-id="e134c-206">Virtuální síť a místní sítě předpony adres</span><span class="sxs-lookup"><span data-stu-id="e134c-206">VNet and on-premises network address prefixes</span></span>
>   - <span data-ttu-id="e134c-207">Správné masky sítě</span><span class="sxs-lookup"><span data-stu-id="e134c-207">Proper netmasks</span></span>

#### <a name="sample-configuration"></a><span data-ttu-id="e134c-208">Ukázková konfigurace</span><span class="sxs-lookup"><span data-stu-id="e134c-208">Sample configuration</span></span>

```
! Sample ASA configuration for connecting tooAzure VPN gateway
!
! Tested hardware: ASA 5505
! Tested version:  ASA version 9.2(4)
!
! Replace hello following place holders with your actual values:
!   - Interface names - default are "outside" and "inside"
!   - <Azure_Gateway_Public_IP>
!   - <OnPrem_Device_Public_IP>
!   - <Pre_Shared_Key>
!   - <VNetName>*
!   - <LNGName>* ==> LocalNetworkGateway - hello Azure resource that represents the
!     on-premises network, specifies network prefixes, device public IP, BGP info, etc.
!   - <PrivateIPAddress> ==> Replace it with a private IP address if applicable
!   - <Netmask> ==> Replace it with appropriate netmasks
!   - <Nexthop> ==> Replace it with hello actual nexthop IP address
!
! (*) Must be unique names in hello device configuration
!
! ==> Interface & route configurations
!
!     > <OnPrem_Device_Public_IP> address on hello outside interface or vlan
!     > <PrivateIPAddress> on hello inside interface or vlan; e.g., 10.51.0.1/24
!     > Route tooconnect too<Azure_Gateway_Public_IP> address
!
!     > Example:
!
!       interface Ethernet0/0
!        switchport access vlan 2
!       exit
!
!       interface vlan 1
!        nameif inside
!        security-level 100
!        ip address <PrivateIPAddress> <Netmask>
!       exit
!
!       interface vlan 2
!        nameif outside
!        security-level 0
!        ip address <OnPrem_Device_Public_IP> <Netmask>
!       exit
!
!       route outside 0.0.0.0 0.0.0.0 <NextHop IP> 1
!
! ==> Access lists
!
!     > Most firewall devices deny all traffic by default. Create access lists to
!       (1) Allow S2S VPN tunnels between hello ASA and hello Azure gateway public IP address
!       (2) Construct traffic selectors as part of IPsec policy or proposal
!
access-list outside_access_in extended permit ip host <Azure_Gateway_Public_IP> host <OnPrem_Device_Public_IP>
!
!     > Object group that consists of all VNet prefixes (e.g., 10.11.0.0/16 &
!       10.12.0.0/16)
!
object-group network Azure-<VNetName>
 description Azure virtual network <VNetName> prefixes
 network-object 10.11.0.0 255.255.0.0
 network-object 10.12.0.0 255.255.0.0
exit
!
!     > Object group that corresponding toohello <LNGName> prefixes.
!       E.g., 10.51.0.0/16 and 10.52.0.0/16. Note that LNG = "local network gateway".
!       In Azure network resource, a local network gateway defines hello on-premises
!       network properties (address prefixes, VPN device IP, BGP ASN, etc.)
!
object-group network <LNGName>
 description On-Premises network <LNGName> prefixes
 network-object 10.51.0.0 255.255.0.0
 network-object 10.52.0.0 255.255.0.0
exit
!
!     > Specify hello access-list between hello Azure VNet and your on-premises network.
!       This access list defines hello IPsec SA traffic selectors.
!
access-list Azure-<VNetName>-acl extended permit ip object-group <LNGName> object-group Azure-<VNetName>
!
!     > No NAT required between hello on-premises network and Azure VNet
!
nat (inside,outside) source static <LNGName> <LNGName> destination static Azure-<VNetName> Azure-<VNetName>
!
! ==> IKEv2 configuration
!
!     > General IKEv2 configuration - enable IKEv2 for VPN
!
group-policy DfltGrpPolicy attributes
 vpn-tunnel-protocol ikev1 ikev2
exit
!
crypto isakmp identity address
crypto ikev2 enable outside
!
!     > Define IKEv2 Phase 1/Main Mode policy
!       - Make sure hello policy number is not used
!       - integrity and prf must be hello same
!       - DH group 14 and above require ASA version 9.x.
!
crypto ikev2 policy 1
 encryption       aes-256
 integrity        sha384
 prf              sha384
 group            24
 lifetime seconds 86400
exit
!
!     > Set connection type and pre-shared key
!
tunnel-group <Azure_Gateway_Public_IP> type ipsec-l2l
tunnel-group <Azure_Gateway_Public_IP> ipsec-attributes
 ikev2 remote-authentication pre-shared-key <Pre_Shared_Key> 
 ikev2 local-authentication  pre-shared-key <Pre_Shared_Key> 
exit
!
! ==> IPsec configuration
!
!     > IKEv2 Phase 2/Quick Mode proposal
!       - AES-GCM and SHA-2 requires ASA version 9.x on newer ASA models. ASA
!         5505, 5510, 5520, 5540, 5550, 5580 are not supported.
!       - ESP integrity must be null if AES-GCM is configured as ESP encryption
!
crypto ipsec ikev2 ipsec-proposal AES-256
 protocol esp encryption aes-256
 protocol esp integrity  sha-1
exit
!
!     > Set access list & traffic selectors, PFS, IPsec protposal, SA lifetime
!       - This sample uses "Azure-<VNetName>-map" as hello crypto map name
!       - ASA supports only one crypto map per interface, if you already have
!         an existing crypto map assigned tooyour outside interface, you must use
!         hello same crypto map name, but with a different sequence number for
!         this policy
!       - "match address" policy uses hello access-list "Azure-<VNetName>-acl" defined 
!         previously
!       - "ipsec-proposal" uses hello proposal "AES-256" defined previously 
!       - PFS groups 14 and beyond requires ASA version 9.x.
!
crypto map Azure-<VNetName>-map 1 match address Azure-<VNetName>-acl
crypto map Azure-<VNetName>-map 1 set pfs group24
crypto map Azure-<VNetName>-map 1 set peer <Azure_Gateway_Public_IP>
crypto map Azure-<VNetName>-map 1 set ikev2 ipsec-proposal AES-256
crypto map Azure-<VNetName>-map 1 set security-association lifetime seconds 7200
crypto map Azure-<VNetName>-map interface outside
!
! ==> Set TCP MSS too1350
!
sysopt connection tcpmss 1350
!
```

## <a name="simple-debugging-commands"></a><span data-ttu-id="e134c-209">Jednoduché ladění příkazy</span><span class="sxs-lookup"><span data-stu-id="e134c-209">Simple debugging commands</span></span>

<span data-ttu-id="e134c-210">Zde jsou některé příkazy ASA pro účely ladění:</span><span class="sxs-lookup"><span data-stu-id="e134c-210">Here are some ASA commands for debugging purposes:</span></span>

1. <span data-ttu-id="e134c-211">Zobrazit hello IPsec a přidružení zabezpečení protokolu IKE</span><span class="sxs-lookup"><span data-stu-id="e134c-211">Show hello IPsec and IKE SA's</span></span>
    - <span data-ttu-id="e134c-212">"Zobrazit kryptografických ipsec sa"</span><span class="sxs-lookup"><span data-stu-id="e134c-212">"show crypto ipsec sa"</span></span>
    - <span data-ttu-id="e134c-213">"Zobrazit šifrování sa protokolu ikev2"</span><span class="sxs-lookup"><span data-stu-id="e134c-213">"show crypto ikev2 sa"</span></span>
2. <span data-ttu-id="e134c-214">Vstup režim ladění – to můžete získat v konzole hello velmi aktivní</span><span class="sxs-lookup"><span data-stu-id="e134c-214">Entering debug mode - this can get very noisy on hello console</span></span>
    - <span data-ttu-id="e134c-215">"ladění kryptografických ikev2 platformy <level>"</span><span class="sxs-lookup"><span data-stu-id="e134c-215">"debug crypto ikev2 platform <level>"</span></span>
    - <span data-ttu-id="e134c-216">"ladění protokolu ikev2 kryptografických <level>"</span><span class="sxs-lookup"><span data-stu-id="e134c-216">"debug crypto ikev2 protocol <level>"</span></span>
3. <span data-ttu-id="e134c-217">Aktuální konfigurace seznamu</span><span class="sxs-lookup"><span data-stu-id="e134c-217">List current configurations</span></span>
    - <span data-ttu-id="e134c-218">"spuštění zobrazit" – ukazuje hello aktuální konfigurace na hello zařízení; můžete použít různé dílčí příkazy toolist konkrétní části hello konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="e134c-218">"show run" - shows hello current configurations on hello device; you can use hello various sub-commands toolist specific parts of hello configuration.</span></span> <span data-ttu-id="e134c-219">Například "Zobrazit spustit kryptografických", "spustit zobrazit seznam přístupu", "Zobrazit spustit tunel skupina", atd.</span><span class="sxs-lookup"><span data-stu-id="e134c-219">E.g., "show run crypto", "show run access-list", "show run tunnel-group", etc.</span></span>


## <a name="next-steps"></a><span data-ttu-id="e134c-220">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e134c-220">Next steps</span></span>
<span data-ttu-id="e134c-221">V tématu [konfiguraci brány VPN aktivní-aktivní pro mezi různými místy a připojení VNet-to-VNet](vpn-gateway-activeactive-rm-powershell.md) kroky tooconfigure aktivní aktivní mezi různými místy a připojení VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="e134c-221">See [Configuring Active-Active VPN Gateways for Cross-Premises and VNet-to-VNet Connections](vpn-gateway-activeactive-rm-powershell.md) for steps tooconfigure active-active cross-premises and VNet-to-VNet connections.</span></span>

