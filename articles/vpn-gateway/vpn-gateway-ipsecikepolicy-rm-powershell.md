---
title: "Konfigurace zásad protokolu IPsec/IKE pro připojení S2S VPN nebo VNet-to-VNet: Azure Resource Manager: prostředí PowerShell | Microsoft Docs"
description: "Tento článek vás provede procesem konfigurace zásad protokolu IPsec/IKE pro připojení S2S nebo VNet-to-VNet s Azure VPN Gateway pomocí Azure Resource Manageru a prostředí PowerShell."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: yushwang
ms.openlocfilehash: 798014b6e8d4495db99ef2e2d2ea487ae7d02fd0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-ipsecike-policy-for-s2s-vpn-or-vnet-to-vnet-connections"></a><span data-ttu-id="e23cd-103">Konfigurace zásad protokolu IPsec/IKE pro připojení S2S VPN nebo VNet-to-VNet</span><span class="sxs-lookup"><span data-stu-id="e23cd-103">Configure IPsec/IKE policy for S2S VPN or VNet-to-VNet connections</span></span>

<span data-ttu-id="e23cd-104">Tento článek vás provede kroky pro konfiguraci zásad protokolu IPsec/IKE pro připojení Site-to-Site VPN nebo VNet-to-VNet pomocí modelu nasazení Resource Manager a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e23cd-104">This article walks you through the steps to configure IPsec/IKE policy for Site-to-Site VPN or VNet-to-VNet connections using the Resource Manager deployment model and PowerShell.</span></span>

## <span data-ttu-id="e23cd-105"><a name="about"></a>O parametry zásad protokolu IPsec a IKE pro Azure VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="e23cd-105"><a name="about"></a>About IPsec and IKE policy parameters for Azure VPN gateways</span></span>
<span data-ttu-id="e23cd-106">Protokol IPsec a IKE standardní podporuje širokou škálu kryptografických algoritmů v různých kombinacích.</span><span class="sxs-lookup"><span data-stu-id="e23cd-106">IPsec and IKE protocol standard supports a wide range of cryptographic algorithms in various combinations.</span></span> <span data-ttu-id="e23cd-107">Odkazovat na [o kryptografických požadavky a brány Azure VPN](vpn-gateway-about-compliance-crypto.md) zobrazíte, jak to může pomoct zajistit mezi různými místy a připojení VNet-to-VNet splňují vaše požadavky na dodržování předpisů nebo zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="e23cd-107">Refer to [About cryptographic requirements and Azure VPN gateways](vpn-gateway-about-compliance-crypto.md) to see how this can help ensuring cross-premises and VNet-to-VNet connectivity satisfy your compliance or security requirements.</span></span>

<span data-ttu-id="e23cd-108">Tento článek obsahuje pokyny k vytvoření a konfiguraci zásad protokolu IPsec/IKE a použití na nový nebo existující připojení:</span><span class="sxs-lookup"><span data-stu-id="e23cd-108">This article provides instructions to create and configure an IPsec/IKE policy and apply to a new or existing connection:</span></span>

* [<span data-ttu-id="e23cd-109">Část 1 – pracovní postup pro vytvoření a nastavení zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="e23cd-109">Part 1 - Workflow to create and set IPsec/IKE policy</span></span>](#workflow)
* [<span data-ttu-id="e23cd-110">Část 2 - podporované kryptografické algoritmy a klíče síly</span><span class="sxs-lookup"><span data-stu-id="e23cd-110">Part 2 - Supported cryptographic algorithms and key strengths</span></span>](#params)
* [<span data-ttu-id="e23cd-111">Část 3 – vytvoření nového připojení S2S VPN pomocí zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="e23cd-111">Part 3 - Create a new S2S VPN connection with IPsec/IKE policy</span></span>](#crossprem)
* [<span data-ttu-id="e23cd-112">Součástí 4 – vytvoření nového připojení VNet-to-VNet s zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="e23cd-112">Part 4 - Create a new VNet-to-VNet connection with IPsec/IKE policy</span></span>](#vnet2vnet)
* [<span data-ttu-id="e23cd-113">Část 5 – spravovat (vytvořit, přidat, odebrat) zásady protokolu IPsec/IKE pro připojení</span><span class="sxs-lookup"><span data-stu-id="e23cd-113">Part 5 - Manage (create, add, remove) IPsec/IKE policy for a connection</span></span>](#managepolicy)

> [!IMPORTANT]
> 1. <span data-ttu-id="e23cd-114">Všimněte si, že zásady protokolu IPsec/IKE funguje pouze na následující SKU brány:</span><span class="sxs-lookup"><span data-stu-id="e23cd-114">Note that IPsec/IKE policy only works on the following gateway SKUs:</span></span>
>    * <span data-ttu-id="e23cd-115">***VpnGw1, VpnGw2, VpnGw3*** (trasové)</span><span class="sxs-lookup"><span data-stu-id="e23cd-115">***VpnGw1, VpnGw2, VpnGw3*** (route-based)</span></span>
>    * <span data-ttu-id="e23cd-116">***Standardní*** a ***HighPerformance*** (trasové)</span><span class="sxs-lookup"><span data-stu-id="e23cd-116">***Standard*** and ***HighPerformance*** (route-based)</span></span>
> 2. <span data-ttu-id="e23cd-117">Pro jedno připojení můžete zadat pouze ***jednu*** kombinaci zásad.</span><span class="sxs-lookup"><span data-stu-id="e23cd-117">You can only specify ***one*** policy combination for a given connection.</span></span>
> 3. <span data-ttu-id="e23cd-118">Je nutné zadat všechny algoritmy a parametry pro IKE (hlavní režim) a protokolu IPsec (rychlý režim).</span><span class="sxs-lookup"><span data-stu-id="e23cd-118">You must specify all algorithms and parameters for both IKE (Main Mode) and IPsec (Quick Mode).</span></span> <span data-ttu-id="e23cd-119">Zadání částečných zásad není povoleno.</span><span class="sxs-lookup"><span data-stu-id="e23cd-119">Partial policy specification is not allowed.</span></span>
> 4. <span data-ttu-id="e23cd-120">Poraďte se s dokumentaci VPN dodavatele k a ujistěte se, že zásady se podporuje na vaše místní zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="e23cd-120">Consult with your VPN device vendor specifications to ensure the policy is supported on your on-premises VPN devices.</span></span> <span data-ttu-id="e23cd-121">S2S nebo připojení VNet-to-VNet nejde vytvořit, pokud zásady nejsou kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="e23cd-121">S2S or VNet-to-VNet connections cannot establish if the policies are incompatible.</span></span>

## <span data-ttu-id="e23cd-122"><a name ="workflow"></a>Část 1 – pracovní postup pro vytvoření a nastavení zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="e23cd-122"><a name ="workflow"></a>Part 1 - Workflow to create and set IPsec/IKE policy</span></span>
<span data-ttu-id="e23cd-123">Tato část popisuje postup vytvoření a aktualizace zásady protokolu IPsec/IKE na připojení S2S VPN nebo VNet-to-VNet:</span><span class="sxs-lookup"><span data-stu-id="e23cd-123">This section outlines the workflow to create and update IPsec/IKE policy on a S2S VPN or VNet-to-VNet connection:</span></span>
1. <span data-ttu-id="e23cd-124">Vytvoření virtuální sítě a brány VPN</span><span class="sxs-lookup"><span data-stu-id="e23cd-124">Create a virtual network and a VPN gateway</span></span>
2. <span data-ttu-id="e23cd-125">Vytvoření brány místní sítě pro mezi místní připojení nebo jinou virtuální sítí a Brána pro propojení VNet-to-vnet</span><span class="sxs-lookup"><span data-stu-id="e23cd-125">Create a local network gateway for cross premises connection, or another virtual network and gateway for VNet-to-VNet connection</span></span>
3. <span data-ttu-id="e23cd-126">Vytvoření zásady protokolu IPsec/IKE s vybranou algoritmů hash a parametry</span><span class="sxs-lookup"><span data-stu-id="e23cd-126">Create an IPsec/IKE policy with selected algorithms and parameters</span></span>
4. <span data-ttu-id="e23cd-127">Vytvoření připojení (protokol IPsec nebo VNet2VNet) pomocí zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="e23cd-127">Create a connection (IPsec or VNet2VNet) with the IPsec/IKE policy</span></span>
5. <span data-ttu-id="e23cd-128">Přidání nebo aktualizace nebo odebrání zásad protokolu IPsec/IKE pro existující připojení</span><span class="sxs-lookup"><span data-stu-id="e23cd-128">Add/update/remove an IPsec/IKE policy for an existing connection</span></span>

<span data-ttu-id="e23cd-129">Podle pokynů v tomto článku vám pomůže nastavit a konfigurovat zásady protokolu IPsec/IKE, jak je vidět v diagramu:</span><span class="sxs-lookup"><span data-stu-id="e23cd-129">The instructions in this article helps you set up and configure IPsec/IKE policies as shown in the diagram:</span></span>

![zásady protokolu IPSec ike](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)

## <span data-ttu-id="e23cd-131"><a name ="params"></a>Část 2 - podporované kryptografické algoritmy & klíče síly</span><span class="sxs-lookup"><span data-stu-id="e23cd-131"><a name ="params"></a>Part 2 - Supported cryptographic algorithms & key strengths</span></span>

<span data-ttu-id="e23cd-132">Následující tabulka uvádí podporované kryptografické algoritmy a klíče síly konfigurovat zákazníkům:</span><span class="sxs-lookup"><span data-stu-id="e23cd-132">The following table lists the supported cryptographic algorithms and key strengths configurable by the customers:</span></span>

| <span data-ttu-id="e23cd-133">**IPsec/IKEv2**</span><span class="sxs-lookup"><span data-stu-id="e23cd-133">**IPsec/IKEv2**</span></span>  | <span data-ttu-id="e23cd-134">**Možnosti**</span><span class="sxs-lookup"><span data-stu-id="e23cd-134">**Options**</span></span>    |
| ---  | --- 
| <span data-ttu-id="e23cd-135">Šifrování protokolem IKEv2</span><span class="sxs-lookup"><span data-stu-id="e23cd-135">IKEv2 Encryption</span></span> | <span data-ttu-id="e23cd-136">AES256, AES192, AES128, DES3, DES</span><span class="sxs-lookup"><span data-stu-id="e23cd-136">AES256, AES192, AES128, DES3, DES</span></span>  
| <span data-ttu-id="e23cd-137">Integrita protokolu IKEv2</span><span class="sxs-lookup"><span data-stu-id="e23cd-137">IKEv2 Integrity</span></span>  | <span data-ttu-id="e23cd-138">SHA384, SHA256, SHA1, MD5</span><span class="sxs-lookup"><span data-stu-id="e23cd-138">SHA384, SHA256, SHA1, MD5</span></span>  |
| <span data-ttu-id="e23cd-139">Skupina DH</span><span class="sxs-lookup"><span data-stu-id="e23cd-139">DH Group</span></span>         | <span data-ttu-id="e23cd-140">DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, None</span><span class="sxs-lookup"><span data-stu-id="e23cd-140">DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, None</span></span> |
| <span data-ttu-id="e23cd-141">Šifrování protokolem IPsec</span><span class="sxs-lookup"><span data-stu-id="e23cd-141">IPsec Encryption</span></span> | <span data-ttu-id="e23cd-142">GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, Žádné</span><span class="sxs-lookup"><span data-stu-id="e23cd-142">GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, None</span></span>    |
| <span data-ttu-id="e23cd-143">Integrita protokolu IPsec</span><span class="sxs-lookup"><span data-stu-id="e23cd-143">IPsec Integrity</span></span>  | <span data-ttu-id="e23cd-144">GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5</span><span class="sxs-lookup"><span data-stu-id="e23cd-144">GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5</span></span> |
| <span data-ttu-id="e23cd-145">Skupina PFS</span><span class="sxs-lookup"><span data-stu-id="e23cd-145">PFS Group</span></span>        | <span data-ttu-id="e23cd-146">PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, Žádná</span><span class="sxs-lookup"><span data-stu-id="e23cd-146">PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, None</span></span> 
| <span data-ttu-id="e23cd-147">Doba života přidružení zabezpečení v rychlém režimu</span><span class="sxs-lookup"><span data-stu-id="e23cd-147">QM SA Lifetime</span></span>   | <span data-ttu-id="e23cd-148">(**Volitelné**: výchozí hodnoty jsou použít, pokud není zadána)</span><span class="sxs-lookup"><span data-stu-id="e23cd-148">(**Optional**: default values are used if not specified)</span></span><br><span data-ttu-id="e23cd-149">Sekundy (integer; **min. 300** / výchozí hodnota 27 000 sekund)</span><span class="sxs-lookup"><span data-stu-id="e23cd-149">Seconds (integer; **min. 300**/default 27000 seconds)</span></span><br><span data-ttu-id="e23cd-150">Kilobajty (integer; **min. 1024** / výchozí hodnota 102 400 000 kilobajtů)</span><span class="sxs-lookup"><span data-stu-id="e23cd-150">KBytes (integer; **min. 1024**/default 102400000 KBytes)</span></span>   |
| <span data-ttu-id="e23cd-151">Selektor provozu</span><span class="sxs-lookup"><span data-stu-id="e23cd-151">Traffic Selector</span></span> | <span data-ttu-id="e23cd-152">UsePolicyBasedTrafficSelectors ** ($True nebo $False; **Volitelné**, výchozí $False není-li zadána)</span><span class="sxs-lookup"><span data-stu-id="e23cd-152">UsePolicyBasedTrafficSelectors** ($True/$False; **Optional**, default $False if not specified)</span></span>    |
|  |  |

> [!IMPORTANT]
> 1. <span data-ttu-id="e23cd-153">**Pokud GCMAES slouží jako algoritmus šifrování pomocí protokolu IPsec, musíte vybrat stejný GCMAES algoritmus a délku klíče protokolu IPsec integritu; například pro obě pomocí GCMAES128**</span><span class="sxs-lookup"><span data-stu-id="e23cd-153">**If GCMAES is used as for IPsec Encryption algorithm, you must select the same GCMAES algorithm and key length for IPsec Integrity; for example, using GCMAES128 for both**</span></span>
> 2. <span data-ttu-id="e23cd-154">V branách Azure VPN Gateway je doba života přidružení zabezpečení protokolu IKEv2 v hlavním režimu pevně nastavena na 28 800 sekund.</span><span class="sxs-lookup"><span data-stu-id="e23cd-154">IKEv2 Main Mode SA lifetime is fixed at 28,800 seconds on the Azure VPN gateways</span></span>
> 3. <span data-ttu-id="e23cd-155">Nastavení "UsePolicyBasedTrafficSelectors" na $True pro připojení nakonfigurovat bránu Azure VPN pro připojení založená na zásadách brány firewall VPN místně.</span><span class="sxs-lookup"><span data-stu-id="e23cd-155">Setting "UsePolicyBasedTrafficSelectors" to $True on a connection will configure the Azure VPN gateway to connect to policy-based VPN firewall on premises.</span></span> <span data-ttu-id="e23cd-156">Pokud povolíte PolicyBasedTrafficSelectors, musíte mít jistotu, že vaše zařízení VPN má odpovídající provoz selektory definovaný s všechny kombinace předponami vaší místní sítě (brány místní sítě) z předpony virtuální síť Azure, místo any-to-any.</span><span class="sxs-lookup"><span data-stu-id="e23cd-156">If you enable PolicyBasedTrafficSelectors, you need to ensure your VPN device has the matching traffic selectors defined with all combinations of your on-premises network (local network gateway) prefixes to/from the Azure virtual network prefixes, instead of any-to-any.</span></span> <span data-ttu-id="e23cd-157">Například pokud jsou předpony vaší místní sítě 10.1.0.0/16 a 10.2.0.0/16 a předpony vaší virtuální sítě jsou 192.168.0.0/16 a 172.16.0.0/16, je potřeba zadat následující selektory provozu:</span><span class="sxs-lookup"><span data-stu-id="e23cd-157">For example, if your on-premises network prefixes are 10.1.0.0/16 and 10.2.0.0/16, and your virtual network prefixes are 192.168.0.0/16 and 172.16.0.0/16, you need to specify the following traffic selectors:</span></span>
>    * <span data-ttu-id="e23cd-158">10.1.0.0/16 <====> 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="e23cd-158">10.1.0.0/16 <====> 192.168.0.0/16</span></span>
>    * <span data-ttu-id="e23cd-159">10.1.0.0/16 <====> 172.16.0.0/16</span><span class="sxs-lookup"><span data-stu-id="e23cd-159">10.1.0.0/16 <====> 172.16.0.0/16</span></span>
>    * <span data-ttu-id="e23cd-160">10.2.0.0/16 <====> 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="e23cd-160">10.2.0.0/16 <====> 192.168.0.0/16</span></span>
>    * <span data-ttu-id="e23cd-161">10.2.0.0/16 <====> 172.16.0.0/16</span><span class="sxs-lookup"><span data-stu-id="e23cd-161">10.2.0.0/16 <====> 172.16.0.0/16</span></span>

<span data-ttu-id="e23cd-162">Další informace týkající se provozu na základě zásad selektory najdete v tématu [připojení více místně na základě zásad zařízení VPN](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="e23cd-162">For more information regarding policy-based traffic selectors, see [Connect multiple on-premises policy-based VPN devices](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span></span>

<span data-ttu-id="e23cd-163">Následující tabulka uvádí odpovídající skupiny Diffie-Hellman nepodporuje vlastní zásady:</span><span class="sxs-lookup"><span data-stu-id="e23cd-163">The following table lists the corresponding Diffie-Hellman Groups supported by the custom policy:</span></span>

| <span data-ttu-id="e23cd-164">**Skupina Diffie-Hellman**</span><span class="sxs-lookup"><span data-stu-id="e23cd-164">**Diffie-Hellman Group**</span></span>  | <span data-ttu-id="e23cd-165">**DHGroup**</span><span class="sxs-lookup"><span data-stu-id="e23cd-165">**DHGroup**</span></span>              | <span data-ttu-id="e23cd-166">**PFSGroup**</span><span class="sxs-lookup"><span data-stu-id="e23cd-166">**PFSGroup**</span></span> | <span data-ttu-id="e23cd-167">**Délka klíče**</span><span class="sxs-lookup"><span data-stu-id="e23cd-167">**Key length**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e23cd-168">1</span><span class="sxs-lookup"><span data-stu-id="e23cd-168">1</span></span>                         | <span data-ttu-id="e23cd-169">DHGroup1</span><span class="sxs-lookup"><span data-stu-id="e23cd-169">DHGroup1</span></span>                 | <span data-ttu-id="e23cd-170">PFS1</span><span class="sxs-lookup"><span data-stu-id="e23cd-170">PFS1</span></span>         | <span data-ttu-id="e23cd-171">768bitová skupina MODP</span><span class="sxs-lookup"><span data-stu-id="e23cd-171">768-bit MODP</span></span>   |
| <span data-ttu-id="e23cd-172">2</span><span class="sxs-lookup"><span data-stu-id="e23cd-172">2</span></span>                         | <span data-ttu-id="e23cd-173">DHGroup2</span><span class="sxs-lookup"><span data-stu-id="e23cd-173">DHGroup2</span></span>                 | <span data-ttu-id="e23cd-174">PFS2</span><span class="sxs-lookup"><span data-stu-id="e23cd-174">PFS2</span></span>         | <span data-ttu-id="e23cd-175">1024bitová skupina MODP</span><span class="sxs-lookup"><span data-stu-id="e23cd-175">1024-bit MODP</span></span>  |
| <span data-ttu-id="e23cd-176">14</span><span class="sxs-lookup"><span data-stu-id="e23cd-176">14</span></span>                        | <span data-ttu-id="e23cd-177">DHGroup14</span><span class="sxs-lookup"><span data-stu-id="e23cd-177">DHGroup14</span></span><br><span data-ttu-id="e23cd-178">DHGroup2048</span><span class="sxs-lookup"><span data-stu-id="e23cd-178">DHGroup2048</span></span> | <span data-ttu-id="e23cd-179">PFS2048</span><span class="sxs-lookup"><span data-stu-id="e23cd-179">PFS2048</span></span>      | <span data-ttu-id="e23cd-180">2048bitová skupina MODP</span><span class="sxs-lookup"><span data-stu-id="e23cd-180">2048-bit MODP</span></span>  |
| <span data-ttu-id="e23cd-181">19</span><span class="sxs-lookup"><span data-stu-id="e23cd-181">19</span></span>                        | <span data-ttu-id="e23cd-182">ECP256</span><span class="sxs-lookup"><span data-stu-id="e23cd-182">ECP256</span></span>                   | <span data-ttu-id="e23cd-183">ECP256</span><span class="sxs-lookup"><span data-stu-id="e23cd-183">ECP256</span></span>       | <span data-ttu-id="e23cd-184">256bitová skupina ECP</span><span class="sxs-lookup"><span data-stu-id="e23cd-184">256-bit ECP</span></span>    |
| <span data-ttu-id="e23cd-185">20</span><span class="sxs-lookup"><span data-stu-id="e23cd-185">20</span></span>                        | <span data-ttu-id="e23cd-186">ECP384</span><span class="sxs-lookup"><span data-stu-id="e23cd-186">ECP384</span></span>                   | <span data-ttu-id="e23cd-187">ECP284</span><span class="sxs-lookup"><span data-stu-id="e23cd-187">ECP284</span></span>       | <span data-ttu-id="e23cd-188">384bitová skupina ECP</span><span class="sxs-lookup"><span data-stu-id="e23cd-188">384-bit ECP</span></span>    |
| <span data-ttu-id="e23cd-189">24</span><span class="sxs-lookup"><span data-stu-id="e23cd-189">24</span></span>                        | <span data-ttu-id="e23cd-190">DHGroup24</span><span class="sxs-lookup"><span data-stu-id="e23cd-190">DHGroup24</span></span>                | <span data-ttu-id="e23cd-191">PFS24</span><span class="sxs-lookup"><span data-stu-id="e23cd-191">PFS24</span></span>        | <span data-ttu-id="e23cd-192">2048bitová skupina MODP</span><span class="sxs-lookup"><span data-stu-id="e23cd-192">2048-bit MODP</span></span>  |

<span data-ttu-id="e23cd-193">Další podrobnosti najdete v článcích týkajících se [RFC3526](https://tools.ietf.org/html/rfc3526) a [RFC5114](https://tools.ietf.org/html/rfc5114).</span><span class="sxs-lookup"><span data-stu-id="e23cd-193">Refer to [RFC3526](https://tools.ietf.org/html/rfc3526) and [RFC5114](https://tools.ietf.org/html/rfc5114) for more details.</span></span>

## <span data-ttu-id="e23cd-194"><a name ="crossprem"></a>Část 3 – vytvoření nového připojení S2S VPN pomocí zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="e23cd-194"><a name ="crossprem"></a>Part 3 - Create a new S2S VPN connection with IPsec/IKE policy</span></span>

<span data-ttu-id="e23cd-195">Tato část vás provede kroky k vytvoření připojení S2S VPN pomocí zásad protokolu IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="e23cd-195">This section walks you through the steps of creating a S2S VPN connection with an IPsec/IKE policy.</span></span> <span data-ttu-id="e23cd-196">Následující postup vytvoření připojení, jak je vidět v diagramu:</span><span class="sxs-lookup"><span data-stu-id="e23cd-196">The following steps create the connection as shown in the diagram:</span></span>

![zásady s2s](./media/vpn-gateway-ipsecikepolicy-rm-powershell/s2spolicy.png)

<span data-ttu-id="e23cd-198">V tématu [vytvoření připojení S2S VPN](vpn-gateway-create-site-to-site-rm-powershell.md) podrobnější podrobné pokyny pro vytvoření připojení S2S VPN.</span><span class="sxs-lookup"><span data-stu-id="e23cd-198">See [Create a S2S VPN connection](vpn-gateway-create-site-to-site-rm-powershell.md) for more detailed step-by-step instructions for creating a S2S VPN connection.</span></span>

### <span data-ttu-id="e23cd-199"><a name="before"></a>Než začnete</span><span class="sxs-lookup"><span data-stu-id="e23cd-199"><a name="before"></a>Before you begin</span></span>

* <span data-ttu-id="e23cd-200">Ověřte, že máte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="e23cd-200">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="e23cd-201">Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e23cd-201">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="e23cd-202">Nainstalujte rutiny Powershellu pro Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="e23cd-202">Install the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="e23cd-203">V tématu [Přehled prostředí Azure PowerShell](/powershell/azure/overview) pro další informace o instalaci rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e23cd-203">See [Overview of Azure PowerShell](/powershell/azure/overview) for more information about installing the PowerShell cmdlets.</span></span>

### <span data-ttu-id="e23cd-204"><a name="createvnet1"></a>Krok 1 – vytvoření virtuální sítě, brána sítě VPN a bránu místní sítě</span><span class="sxs-lookup"><span data-stu-id="e23cd-204"><a name="createvnet1"></a>Step 1 - Create the virtual network, VPN gateway, and local network gateway</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="e23cd-205">1. Deklarace proměnných</span><span class="sxs-lookup"><span data-stu-id="e23cd-205">1. Declare your variables</span></span>

<span data-ttu-id="e23cd-206">Pro toto cvičení začneme deklarací proměnných.</span><span class="sxs-lookup"><span data-stu-id="e23cd-206">For this exercise, we start by declaring our variables.</span></span> <span data-ttu-id="e23cd-207">Při konfiguraci pro ostrý provoz nezapomeňte nahradit hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="e23cd-207">Be sure to replace the values with your own when configuring for production.</span></span>

```powershell
$Sub1          = "<YourSubscriptionName>"
$RG1           = "TestPolicyRG1"
$Location1     = "East US 2"
$VNetName1     = "TestVNet1"
$FESubName1    = "FrontEnd"
$BESubName1    = "Backend"
$GWSubName1    = "GatewaySubnet"
$VNetPrefix11  = "10.11.0.0/16"
$VNetPrefix12  = "10.12.0.0/16"
$FESubPrefix1  = "10.11.0.0/24"
$BESubPrefix1  = "10.12.0.0/24"
$GWSubPrefix1  = "10.12.255.0/27"
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GW1IPName1    = "VNet1GWIP1"
$GW1IPconf1    = "gw1ipconf1"
$Connection16  = "VNet1toSite6"

$LNGName6      = "Site6"
$LNGPrefix61   = "10.61.0.0/16"
$LNGPrefix62   = "10.62.0.0/16"
$LNGIP6        = "131.107.72.22"
```

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a><span data-ttu-id="e23cd-208">2. Připojení k vašemu předplatnému a vytvořte novou skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="e23cd-208">2. Connect to your subscription and create a new resource group</span></span>

<span data-ttu-id="e23cd-209">Ujistěte se, že jste přešli do režimu prostředí PowerShell, aby bylo možné používat rutiny Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="e23cd-209">Make sure you switch to PowerShell mode to use the Resource Manager cmdlets.</span></span> <span data-ttu-id="e23cd-210">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="e23cd-210">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="e23cd-211">Otevřete konzolu prostředí PowerShell a připojte se ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="e23cd-211">Open your PowerShell console and connect to your account.</span></span> <span data-ttu-id="e23cd-212">Připojení vám usnadní následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="e23cd-212">Use the following sample to help you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a><span data-ttu-id="e23cd-213">3. Vytvoření virtuální sítě, brána sítě VPN a bránu místní sítě</span><span class="sxs-lookup"><span data-stu-id="e23cd-213">3. Create the virtual network, VPN gateway, and local network gateway</span></span>

<span data-ttu-id="e23cd-214">Následující příklad vytvoří virtuální síť, virtuální sítě TestVNet1 s tři podsítě a bránu VPN.</span><span class="sxs-lookup"><span data-stu-id="e23cd-214">The following sample creates the virtual network, TestVNet1, with three subnets, and the VPN gateway.</span></span> <span data-ttu-id="e23cd-215">Při nahrazování hodnot je důležité vždy přiřadit podsíti brány konkrétní název GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="e23cd-215">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="e23cd-216">Pokud použijete jiný název, vytvoření brány se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="e23cd-216">If you name it something else, your gateway creation fails.</span></span>

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance

New-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
```

### <span data-ttu-id="e23cd-217"><a name="s2sconnection"></a>Krok 2 – Vytvoření připojení S2S VPN pomocí zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="e23cd-217"><a name="s2sconnection"></a>Step 2 - Create a S2S VPN connection with an IPsec/IKE policy</span></span>

#### <a name="1-create-an-ipsecike-policy"></a><span data-ttu-id="e23cd-218">1. Vytvoření zásady protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="e23cd-218">1. Create an IPsec/IKE policy</span></span>

<span data-ttu-id="e23cd-219">Následující ukázkový skript vytvoří zásadu protokolu IPsec/IKE se tyto algoritmy a parametry:</span><span class="sxs-lookup"><span data-stu-id="e23cd-219">The following sample script creates an IPsec/IKE policy with the following algorithms and parameters:</span></span>

* <span data-ttu-id="e23cd-220">IKEv2: DHGroup24 AES256, SHA384</span><span class="sxs-lookup"><span data-stu-id="e23cd-220">IKEv2: AES256, SHA384, DHGroup24</span></span>
* <span data-ttu-id="e23cd-221">Protokol IPsec: AES256, SHA256, PFS24 7200 životnost přidružení zabezpečení sekund & 2048KB</span><span class="sxs-lookup"><span data-stu-id="e23cd-221">IPsec: AES256, SHA256, PFS24, SA Lifetime 7200 seconds & 2048KB</span></span>

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 2048
```

<span data-ttu-id="e23cd-222">Pokud používáte GCMAES pro IPsec, musíte použít stejné GCMAES algoritmus a délku klíče pro šifrování pomocí protokolu IPsec a integritu, například:</span><span class="sxs-lookup"><span data-stu-id="e23cd-222">If you use GCMAES for IPsec, you must use the same GCMAES algorithm and key length for both IPsec encryption and integrity, for example:</span></span>

* <span data-ttu-id="e23cd-223">IKEv2: DHGroup24 AES256, SHA384</span><span class="sxs-lookup"><span data-stu-id="e23cd-223">IKEv2: AES256, SHA384, DHGroup24</span></span>
* <span data-ttu-id="e23cd-224">Protokol IPsec: **GCMAES256, GCMAES256**, PFS24 7200 životnost přidružení zabezpečení sekund & 2048 KB</span><span class="sxs-lookup"><span data-stu-id="e23cd-224">IPsec: **GCMAES256, GCMAES256**, PFS24, SA Lifetime 7200 seconds & 2048KB</span></span>

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption GCMAES256 -IpsecIntegrity GCMAES256 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 2048
```

#### <a name="2-create-the-s2s-vpn-connection-with-the-ipsecike-policy"></a><span data-ttu-id="e23cd-225">2. Vytvoření připojení k síti VPN S2S s zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="e23cd-225">2. Create the S2S VPN connection with the IPsec/IKE policy</span></span>

<span data-ttu-id="e23cd-226">Vytvoření připojení k síti VPN S2S a použití zásad protokolu IPsec/IKE vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="e23cd-226">Create an S2S VPN connection and apply the IPsec/IKE policy created earlier.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

<span data-ttu-id="e23cd-227">Volitelně můžete přidat "-UsePolicyBasedTrafficSelectors $True" do rutiny vytvořit připojení k povolení brány Azure VPN pro připojení k zařízení VPN založené na zásadách místně, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="e23cd-227">You can optionally add "-UsePolicyBasedTrafficSelectors $True" to the create connection cmdlet to enable Azure VPN gateway to connect to policy-based VPN devices on premises, as described above.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e23cd-228">Po připojení je zadán pro zásady protokolu IPsec/IKE, brána Azure VPN pouze odeslat nebo přijmout protokolu IPsec/IKE návrh s zadané kryptografické algoritmy a klíče síly na konkrétní připojení.</span><span class="sxs-lookup"><span data-stu-id="e23cd-228">Once an IPsec/IKE policy is specified on a connection, the Azure VPN gateway will only send or accept the IPsec/IKE proposal with specified cryptographic algorithms and key strengths on that particular connection.</span></span> <span data-ttu-id="e23cd-229">Zajistěte, aby vaše místní zařízení VPN pro připojení používá nebo přijímá kombinaci přesný zásad, jinak nebude vytvořit tunel S2S VPN.</span><span class="sxs-lookup"><span data-stu-id="e23cd-229">Make sure your on-premises VPN device for the connection uses or accepts the exact policy combination, otherwise the S2S VPN tunnel will not establish.</span></span>


## <span data-ttu-id="e23cd-230"><a name ="vnet2vnet"></a>Součástí 4 – vytvoření nového připojení VNet-to-VNet s zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="e23cd-230"><a name ="vnet2vnet"></a>Part 4 - Create a new VNet-to-VNet connection with IPsec/IKE policy</span></span>

<span data-ttu-id="e23cd-231">Kroky k vytvoření připojení VNet-to-VNet pomocí zásad protokolu IPsec/IKE jsou podobné jako připojení S2S VPN.</span><span class="sxs-lookup"><span data-stu-id="e23cd-231">The steps of creating a VNet-to-VNet connection with an IPsec/IKE policy are similar to that of a S2S VPN connection.</span></span> <span data-ttu-id="e23cd-232">Následující ukázkové skripty vytvoření připojení, jak je vidět v diagramu:</span><span class="sxs-lookup"><span data-stu-id="e23cd-232">The following sample scripts create the connection as shown in the diagram:</span></span>

![zásady v2v](./media/vpn-gateway-ipsecikepolicy-rm-powershell/v2vpolicy.png)

<span data-ttu-id="e23cd-234">V tématu [vytvořit připojení VNet-to-VNet](vpn-gateway-vnet-vnet-rm-ps.md) podrobné kroky pro vytvoření připojení VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="e23cd-234">See [Create a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md) for more detailed steps for creating a VNet-to-VNet connection.</span></span> <span data-ttu-id="e23cd-235">Je třeba provést [část 3](#crossprem) vytvořit a nakonfigurovat virtuální síť TestVNet1 a bránu VPN.</span><span class="sxs-lookup"><span data-stu-id="e23cd-235">You must complete [Part 3](#crossprem) to create and configure TestVNet1 and the VPN Gateway.</span></span>

### <span data-ttu-id="e23cd-236"><a name="createvnet2"></a>Krok 1 – Vytvoření druhý virtuální sítě a brány VPN</span><span class="sxs-lookup"><span data-stu-id="e23cd-236"><a name="createvnet2"></a>Step 1 - Create the second virtual network and VPN gateway</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="e23cd-237">1. Deklarace proměnných</span><span class="sxs-lookup"><span data-stu-id="e23cd-237">1. Declare your variables</span></span>

<span data-ttu-id="e23cd-238">Nezapomeňte nahradit hodnoty těmi, které chcete použít pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="e23cd-238">Be sure to replace the values with the ones that you want to use for your configuration.</span></span>

```powershell
$RG2          = "TestPolicyRG2"
$Location2    = "East US 2"
$VNetName2    = "TestVNet2"
$FESubName2   = "FrontEnd"
$BESubName2   = "Backend"
$GWSubName2   = "GatewaySubnet"
$VNetPrefix21 = "10.21.0.0/16"
$VNetPrefix22 = "10.22.0.0/16"
$FESubPrefix2 = "10.21.0.0/24"
$BESubPrefix2 = "10.22.0.0/24"
$GWSubPrefix2 = "10.22.255.0/27"
$DNS2         = "8.8.8.8"
$GWName2      = "VNet2GW"
$GW2IPName1   = "VNet2GWIP1"
$GW2IPconf1   = "gw2ipconf1"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-the-second-virtual-network-and-vpn-gateway-in-the-new-resource-group"></a><span data-ttu-id="e23cd-239">2. Vytvořit druhý virtuální sítě a brány VPN do nové skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="e23cd-239">2. Create the second virtual network and VPN gateway in the new resource group</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

$gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1

New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance
```

### <a name="step-2---create-a-vnet-tovnet-connection-with-the-ipsecike-policy"></a><span data-ttu-id="e23cd-240">Krok 2 – Vytvoření připojení virtuální sítě toVNet pomocí zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="e23cd-240">Step 2 - Create a VNet-toVNet connection with the IPsec/IKE policy</span></span>

<span data-ttu-id="e23cd-241">Podobá se připojení S2S VPN, vytvoření zásady protokolu IPsec/IKE pak použít pro zásady nové připojení.</span><span class="sxs-lookup"><span data-stu-id="e23cd-241">Similar to the S2S VPN connection, create an IPsec/IKE policy then apply to policy to the new connection.</span></span>

#### <a name="1-create-an-ipsecike-policy"></a><span data-ttu-id="e23cd-242">1. Vytvoření zásady protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="e23cd-242">1. Create an IPsec/IKE policy</span></span>

<span data-ttu-id="e23cd-243">Následující ukázkový skript vytvoří jiné zásady protokolu IPsec/IKE s parametry data a tyto algoritmy:</span><span class="sxs-lookup"><span data-stu-id="e23cd-243">The following sample script creates a different IPsec/IKE policy with the following algorithms and parameters:</span></span>
* <span data-ttu-id="e23cd-244">IKEv2: DHGroup14 AES128, SHA1,</span><span class="sxs-lookup"><span data-stu-id="e23cd-244">IKEv2: AES128, SHA1, DHGroup14</span></span>
* <span data-ttu-id="e23cd-245">Protokol IPsec: GCMAES128, GCMAES128, PFS14, životnost přidružení zabezpečení 7200 sekund a 4096KB</span><span class="sxs-lookup"><span data-stu-id="e23cd-245">IPsec: GCMAES128, GCMAES128, PFS14, SA Lifetime 7200 seconds & 4096KB</span></span>

```powershell
$ipsecpolicy2 = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup PFS14 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 4096
```

#### <a name="2-create-vnet-to-vnet-connections-with-the-ipsecike-policy"></a><span data-ttu-id="e23cd-246">2. Vytvoření připojení VNet-to-VNet s zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="e23cd-246">2. Create VNet-to-VNet connections with the IPsec/IKE policy</span></span>

<span data-ttu-id="e23cd-247">Umožňuje vytvořit připojení VNet-to-VNet a použití zásady protokolu IPsec/IKE, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="e23cd-247">Create a VNet-to-VNet connection and apply the IPsec/IKE policy you created.</span></span> <span data-ttu-id="e23cd-248">V tomto příkladu jsou obě brány ve stejném předplatném.</span><span class="sxs-lookup"><span data-stu-id="e23cd-248">In this example, both gateways are in the same subscription.</span></span> <span data-ttu-id="e23cd-249">Proto je možné vytvořit a nakonfigurovat stejné zásady protokolu IPsec/IKE obě připojení ve stejné relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e23cd-249">So it is possible to create and configure both connections with the same IPsec/IKE policy in the same PowerShell session.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2  -ResourceGroupName $RG2

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'
```

> [!IMPORTANT]
> <span data-ttu-id="e23cd-250">Po připojení je zadán pro zásady protokolu IPsec/IKE, brána Azure VPN pouze odeslat nebo přijmout protokolu IPsec/IKE návrh s zadané kryptografické algoritmy a klíče síly na konkrétní připojení.</span><span class="sxs-lookup"><span data-stu-id="e23cd-250">Once an IPsec/IKE policy is specified on a connection, the Azure VPN gateway will only send or accept the IPsec/IKE proposal with specified cryptographic algorithms and key strengths on that particular connection.</span></span> <span data-ttu-id="e23cd-251">Ujistěte se, že zásady protokolu IPsec pro obě připojení jsou stejné, jinak nebude navázání připojení VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="e23cd-251">Make sure the IPsec policies for both connections are the same, otherwise the VNet-to-VNet connection will not establish.</span></span>

<span data-ttu-id="e23cd-252">Po dokončení těchto kroků, připojení za pár minut a bude mít následující topologie sítě, jak je znázorněno v začátku:</span><span class="sxs-lookup"><span data-stu-id="e23cd-252">After completing these steps, the connection is established in a few minutes, and you will have the following network topology as shown in the beginning:</span></span>

![zásady protokolu IPSec ike](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)


## <span data-ttu-id="e23cd-254"><a name ="managepolicy"></a>Část 5 – zásady aktualizace protokolu IPsec/IKE pro připojení</span><span class="sxs-lookup"><span data-stu-id="e23cd-254"><a name ="managepolicy"></a>Part 5 - Update IPsec/IKE policy for a connection</span></span>

<span data-ttu-id="e23cd-255">V poslední části se dozvíte, jak ke správě zásad protokolu IPsec/IKE pro existující připojení S2S nebo VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="e23cd-255">The last section shows you how to manage IPsec/IKE policy for an existing S2S or VNet-to-VNet connection.</span></span> <span data-ttu-id="e23cd-256">Cvičení dole vás provede následující operace na připojení:</span><span class="sxs-lookup"><span data-stu-id="e23cd-256">The exercise below walks you through the following operations on a connection:</span></span>

1. <span data-ttu-id="e23cd-257">Zobrazit zásady protokolu IPsec/IKE připojení</span><span class="sxs-lookup"><span data-stu-id="e23cd-257">Show the IPsec/IKE policy of a connection</span></span>
2. <span data-ttu-id="e23cd-258">Přidáte nebo aktualizujete zásady protokolu IPsec/IKE pro připojení</span><span class="sxs-lookup"><span data-stu-id="e23cd-258">Add or update the IPsec/IKE policy to a connection</span></span>
3. <span data-ttu-id="e23cd-259">Odebrání připojení zásady protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="e23cd-259">Remove the IPsec/IKE policy from a connection</span></span>

<span data-ttu-id="e23cd-260">Stejný postup platí pro připojení S2S a VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="e23cd-260">The same steps apply to both S2S and VNet-to-VNet connections.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e23cd-261">Zásady protokolu IPsec/IKE je podporována v *standardní* a *HighPerformance* založené na trasách pouze VPN Gateway.</span><span class="sxs-lookup"><span data-stu-id="e23cd-261">IPsec/IKE policy is supported on *Standard* and *HighPerformance* route-based VPN gateways only.</span></span> <span data-ttu-id="e23cd-262">Nefunguje se na základní SKU brány nebo brány sítě VPN založené na zásadách.</span><span class="sxs-lookup"><span data-stu-id="e23cd-262">It does not work on the Basic gateway SKU or the policy-based VPN gateway.</span></span>

#### <a name="1-show-the-ipsecike-policy-of-a-connection"></a><span data-ttu-id="e23cd-263">1. Zobrazit zásady protokolu IPsec/IKE připojení</span><span class="sxs-lookup"><span data-stu-id="e23cd-263">1. Show the IPsec/IKE policy of a connection</span></span>

<span data-ttu-id="e23cd-264">Následující příklad ukazuje, jak získat zásady protokolu IPsec/IKE nakonfigurované na připojení.</span><span class="sxs-lookup"><span data-stu-id="e23cd-264">The following example shows how to get the IPsec/IKE policy configured on a connection.</span></span> <span data-ttu-id="e23cd-265">Skripty taky z výše uvedeného cvičení pokračovat.</span><span class="sxs-lookup"><span data-stu-id="e23cd-265">The scripts also continue from the exercises above.</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

<span data-ttu-id="e23cd-266">Poslední příkaz uvádí aktuální zásady protokolu IPsec/IKE, které jsou nakonfigurované na připojení, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="e23cd-266">The last command lists the current IPsec/IKE policy configured on the connection, if there is any.</span></span> <span data-ttu-id="e23cd-267">Následující ukázkový výstup je pro připojení:</span><span class="sxs-lookup"><span data-stu-id="e23cd-267">The following sample output is for the connection:</span></span>

```powershell
SALifeTimeSeconds   : 3600
SADataSizeKilobytes : 2048
IpsecEncryption     : AES256
IpsecIntegrity      : SHA256
IkeEncryption       : AES256
IkeIntegrity        : SHA384
DhGroup             : DHGroup24
PfsGroup            : PFS24
```

<span data-ttu-id="e23cd-268">Pokud neexistuje žádné zásady protokolu IPsec/IKE konfigurace, příkazu (PS > $connection6.policy) získá návratový prázdná.</span><span class="sxs-lookup"><span data-stu-id="e23cd-268">If there is no IPsec/IKE policy configured, the command (PS> $connection6.policy) gets an empty return.</span></span> <span data-ttu-id="e23cd-269">Neznamená to protokolu IPsec/IKE není konfigurován na připojení, ale, že nejsou žádné vlastní zásady protokolu IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="e23cd-269">It does not mean IPsec/IKE is not configured on the connection, but that there is no custom IPsec/IKE policy.</span></span> <span data-ttu-id="e23cd-270">Skutečné připojení používá výchozí zásady mezi vaší místní zařízení VPN a bránu Azure VPN.</span><span class="sxs-lookup"><span data-stu-id="e23cd-270">The actual connection uses the default policy negotiated between your on-premises VPN device and the Azure VPN gateway.</span></span>

#### <a name="2-add-or-update-an-ipsecike-policy-for-a-connection"></a><span data-ttu-id="e23cd-271">2. Přidat nebo aktualizovat zásady protokolu IPsec/IKE pro připojení</span><span class="sxs-lookup"><span data-stu-id="e23cd-271">2. Add or update an IPsec/IKE policy for a connection</span></span>

<span data-ttu-id="e23cd-272">Postup přidání nové zásady nebo aktualizovat existující zásady na připojení jsou stejné: Vytvořte novou zásadu, pak použijí nové zásady na připojení.</span><span class="sxs-lookup"><span data-stu-id="e23cd-272">The steps to add a new policy or update an existing policy on a connection are the same: create a new policy then apply the new policy to the connection.</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$newpolicy6   = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup None -SALifeTimeSeconds 3600 -SADataSizeKilobytes 2048

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6
```

<span data-ttu-id="e23cd-273">Chcete-li povolit "UsePolicyBasedTrafficSelectors" při připojení na místní zařízení VPN založené na zásadách, přidejte "-UsePolicyBaseTrafficSelectors" do rutiny, parametr nebo ji nastavte na $False zakázat možnost:</span><span class="sxs-lookup"><span data-stu-id="e23cd-273">To enable "UsePolicyBasedTrafficSelectors" when connecting to an on-premises policy-based VPN device, add the "-UsePolicyBaseTrafficSelectors" parameter to the cmdlet, or set it to $False to disable the option:</span></span>

```powershell
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6 -UsePolicyBasedTrafficSelectors $True
```

<span data-ttu-id="e23cd-274">Můžete získat připojení znovu ke kontrole, pokud zásada se aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="e23cd-274">You can get the connection again to check if the policy is updated.</span></span>

```powershell
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

<span data-ttu-id="e23cd-275">Výstup z posledního řádku, byste měli vidět, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="e23cd-275">You should see the output from the last line, as shown in the following example:</span></span>

```powershell
SALifeTimeSeconds   : 3600
SADataSizeKilobytes : 2048
IpsecEncryption     : GCMAES128
IpsecIntegrity      : GCMAES128
IkeEncryption       : AES128
IkeIntegrity        : SHA1
DhGroup             : DHGroup14--
PfsGroup            : None
```

#### <a name="3-remove-an-ipsecike-policy-from-a-connection"></a><span data-ttu-id="e23cd-276">3. Odebrání připojení zásady protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="e23cd-276">3. Remove an IPsec/IKE policy from a connection</span></span>

<span data-ttu-id="e23cd-277">Po připojení odeberete vlastní zásady, bránu Azure VPN se Překlopí [výchozí seznam protokolu IPsec/IKE návrhy](vpn-gateway-about-vpn-devices.md) a obnoví vyjednávání znovu s vaše místní zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="e23cd-277">Once you remove the custom policy from a connection, the Azure VPN gateway reverts back to the [default list of IPsec/IKE proposals](vpn-gateway-about-vpn-devices.md) and renegotiates again with your on-premises VPN device.</span></span>

```powershell
$RG1           = "TestPolicyRG1"
$Connection16  = "VNet1toSite6"
$connection6   = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$currentpolicy = $connection6.IpsecPolicies[0]
$connection6.IpsecPolicies.Remove($currentpolicy)

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6
```

<span data-ttu-id="e23cd-278">Stejný skriptu můžete použít ke kontrole, pokud zásada byla odebrána z připojení.</span><span class="sxs-lookup"><span data-stu-id="e23cd-278">You can use the same script to check if the policy has been removed from the connection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e23cd-279">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e23cd-279">Next steps</span></span>

<span data-ttu-id="e23cd-280">V tématu [připojení více místně na základě zásad zařízení VPN](vpn-gateway-connect-multiple-policybased-rm-ps.md) další podrobnosti týkající se provozu na základě zásad selektorů.</span><span class="sxs-lookup"><span data-stu-id="e23cd-280">See [Connect multiple on-premises policy-based VPN devices](vpn-gateway-connect-multiple-policybased-rm-ps.md) for more details regarding policy-based traffic selectors.</span></span>

<span data-ttu-id="e23cd-281">Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="e23cd-281">Once your connection is complete, you can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="e23cd-282">Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e23cd-282">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>