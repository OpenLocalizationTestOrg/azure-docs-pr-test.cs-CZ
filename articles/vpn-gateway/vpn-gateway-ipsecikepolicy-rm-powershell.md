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
ms.openlocfilehash: f8d2e29276efdec7071f2aa0d463b1abd64a5253
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-ipsecike-policy-for-s2s-vpn-or-vnet-to-vnet-connections"></a><span data-ttu-id="4ac7c-103">Konfigurace zásad protokolu IPsec/IKE pro připojení S2S VPN nebo VNet-to-VNet</span><span class="sxs-lookup"><span data-stu-id="4ac7c-103">Configure IPsec/IKE policy for S2S VPN or VNet-to-VNet connections</span></span>

<span data-ttu-id="4ac7c-104">Tento článek vás provede kroky tooconfigure hello zásady protokolu IPsec/IKE pro připojení Site-to-Site VPN nebo VNet-to-VNet pomocí modelu nasazení Resource Manager hello a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-104">This article walks you through hello steps tooconfigure IPsec/IKE policy for Site-to-Site VPN or VNet-to-VNet connections using hello Resource Manager deployment model and PowerShell.</span></span>

## <span data-ttu-id="4ac7c-105"><a name="about"></a>O parametry zásad protokolu IPsec a IKE pro Azure VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="4ac7c-105"><a name="about"></a>About IPsec and IKE policy parameters for Azure VPN gateways</span></span>
<span data-ttu-id="4ac7c-106">Protokol IPsec a IKE standardní podporuje širokou škálu kryptografických algoritmů v různých kombinacích.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-106">IPsec and IKE protocol standard supports a wide range of cryptographic algorithms in various combinations.</span></span> <span data-ttu-id="4ac7c-107">Odkazovat příliš[o kryptografických požadavky a brány Azure VPN](vpn-gateway-about-compliance-crypto.md) toosee, jak to může pomoct zajistit mezi různými místy a připojení VNet-to-VNet splňují vaše požadavky na dodržování předpisů nebo zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-107">Refer too[About cryptographic requirements and Azure VPN gateways](vpn-gateway-about-compliance-crypto.md) toosee how this can help ensuring cross-premises and VNet-to-VNet connectivity satisfy your compliance or security requirements.</span></span>

<span data-ttu-id="4ac7c-108">Tento článek obsahuje pokyny toocreate a konfigurovat zásady protokolu IPsec/IKE a použít tooa nové nebo existující připojení:</span><span class="sxs-lookup"><span data-stu-id="4ac7c-108">This article provides instructions toocreate and configure an IPsec/IKE policy and apply tooa new or existing connection:</span></span>

* [<span data-ttu-id="4ac7c-109">Část 1 - toocreate pracovního postupu a nastavit zásady protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="4ac7c-109">Part 1 - Workflow toocreate and set IPsec/IKE policy</span></span>](#workflow)
* [<span data-ttu-id="4ac7c-110">Část 2 - podporované kryptografické algoritmy a klíče síly</span><span class="sxs-lookup"><span data-stu-id="4ac7c-110">Part 2 - Supported cryptographic algorithms and key strengths</span></span>](#params)
* [<span data-ttu-id="4ac7c-111">Část 3 – vytvoření nového připojení S2S VPN pomocí zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="4ac7c-111">Part 3 - Create a new S2S VPN connection with IPsec/IKE policy</span></span>](#crossprem)
* [<span data-ttu-id="4ac7c-112">Součástí 4 – vytvoření nového připojení VNet-to-VNet s zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="4ac7c-112">Part 4 - Create a new VNet-to-VNet connection with IPsec/IKE policy</span></span>](#vnet2vnet)
* [<span data-ttu-id="4ac7c-113">Část 5 – spravovat (vytvořit, přidat, odebrat) zásady protokolu IPsec/IKE pro připojení</span><span class="sxs-lookup"><span data-stu-id="4ac7c-113">Part 5 - Manage (create, add, remove) IPsec/IKE policy for a connection</span></span>](#managepolicy)

> [!IMPORTANT]
> 1. <span data-ttu-id="4ac7c-114">Všimněte si, že zásady protokolu IPsec/IKE funguje pouze na hello následující SKU brány:</span><span class="sxs-lookup"><span data-stu-id="4ac7c-114">Note that IPsec/IKE policy only works on hello following gateway SKUs:</span></span>
>    * <span data-ttu-id="4ac7c-115">***VpnGw1, VpnGw2, VpnGw3*** (trasové)</span><span class="sxs-lookup"><span data-stu-id="4ac7c-115">***VpnGw1, VpnGw2, VpnGw3*** (route-based)</span></span>
>    * <span data-ttu-id="4ac7c-116">***Standardní*** a ***HighPerformance*** (trasové)</span><span class="sxs-lookup"><span data-stu-id="4ac7c-116">***Standard*** and ***HighPerformance*** (route-based)</span></span>
> 2. <span data-ttu-id="4ac7c-117">Pro jedno připojení můžete zadat pouze ***jednu*** kombinaci zásad.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-117">You can only specify ***one*** policy combination for a given connection.</span></span>
> 3. <span data-ttu-id="4ac7c-118">Je nutné zadat všechny algoritmy a parametry pro IKE (hlavní režim) a protokolu IPsec (rychlý režim).</span><span class="sxs-lookup"><span data-stu-id="4ac7c-118">You must specify all algorithms and parameters for both IKE (Main Mode) and IPsec (Quick Mode).</span></span> <span data-ttu-id="4ac7c-119">Zadání částečných zásad není povoleno.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-119">Partial policy specification is not allowed.</span></span>
> 4. <span data-ttu-id="4ac7c-120">Poraďte se s VPN dodavatele dokumentaci zásad tooensure hello je podporované na vaše místní zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-120">Consult with your VPN device vendor specifications tooensure hello policy is supported on your on-premises VPN devices.</span></span> <span data-ttu-id="4ac7c-121">S2S nebo připojení VNet-to-VNet nejde vytvořit, pokud jsou zásady hello nekompatibilní.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-121">S2S or VNet-to-VNet connections cannot establish if hello policies are incompatible.</span></span>

## <span data-ttu-id="4ac7c-122"><a name ="workflow"></a>Část 1 - toocreate pracovního postupu a nastavit zásady protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="4ac7c-122"><a name ="workflow"></a>Part 1 - Workflow toocreate and set IPsec/IKE policy</span></span>
<span data-ttu-id="4ac7c-123">Tato část obsahuje přehled hello pracovního postupu toocreate a aktualizace protokolu IPsec/IKE zásady na připojení S2S VPN nebo VNet-to-VNet:</span><span class="sxs-lookup"><span data-stu-id="4ac7c-123">This section outlines hello workflow toocreate and update IPsec/IKE policy on a S2S VPN or VNet-to-VNet connection:</span></span>
1. <span data-ttu-id="4ac7c-124">Vytvoření virtuální sítě a brány VPN</span><span class="sxs-lookup"><span data-stu-id="4ac7c-124">Create a virtual network and a VPN gateway</span></span>
2. <span data-ttu-id="4ac7c-125">Vytvoření brány místní sítě pro mezi místní připojení nebo jinou virtuální sítí a Brána pro propojení VNet-to-vnet</span><span class="sxs-lookup"><span data-stu-id="4ac7c-125">Create a local network gateway for cross premises connection, or another virtual network and gateway for VNet-to-VNet connection</span></span>
3. <span data-ttu-id="4ac7c-126">Vytvoření zásady protokolu IPsec/IKE s vybranou algoritmů hash a parametry</span><span class="sxs-lookup"><span data-stu-id="4ac7c-126">Create an IPsec/IKE policy with selected algorithms and parameters</span></span>
4. <span data-ttu-id="4ac7c-127">Vytvoření připojení (protokol IPsec nebo VNet2VNet) pomocí zásad protokolu IPsec/IKE hello</span><span class="sxs-lookup"><span data-stu-id="4ac7c-127">Create a connection (IPsec or VNet2VNet) with hello IPsec/IKE policy</span></span>
5. <span data-ttu-id="4ac7c-128">Přidání nebo aktualizace nebo odebrání zásad protokolu IPsec/IKE pro existující připojení</span><span class="sxs-lookup"><span data-stu-id="4ac7c-128">Add/update/remove an IPsec/IKE policy for an existing connection</span></span>

<span data-ttu-id="4ac7c-129">Hello pokyny v tomto článku vám pomůže nastavit a konfigurovat zásady protokolu IPsec/IKE, jak je vidět v diagramu hello:</span><span class="sxs-lookup"><span data-stu-id="4ac7c-129">hello instructions in this article helps you set up and configure IPsec/IKE policies as shown in hello diagram:</span></span>

![zásady protokolu IPSec ike](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)

## <span data-ttu-id="4ac7c-131"><a name ="params"></a>Část 2 - podporované kryptografické algoritmy & klíče síly</span><span class="sxs-lookup"><span data-stu-id="4ac7c-131"><a name ="params"></a>Part 2 - Supported cryptographic algorithms & key strengths</span></span>

<span data-ttu-id="4ac7c-132">Hello následující tabulka uvádí hello podporované kryptografické algoritmy a klíče síly konfigurovat zákazníci hello:</span><span class="sxs-lookup"><span data-stu-id="4ac7c-132">hello following table lists hello supported cryptographic algorithms and key strengths configurable by hello customers:</span></span>

| <span data-ttu-id="4ac7c-133">**IPsec/IKEv2**</span><span class="sxs-lookup"><span data-stu-id="4ac7c-133">**IPsec/IKEv2**</span></span>  | <span data-ttu-id="4ac7c-134">**Možnosti**</span><span class="sxs-lookup"><span data-stu-id="4ac7c-134">**Options**</span></span>    |
| ---  | --- 
| <span data-ttu-id="4ac7c-135">Šifrování protokolem IKEv2</span><span class="sxs-lookup"><span data-stu-id="4ac7c-135">IKEv2 Encryption</span></span> | <span data-ttu-id="4ac7c-136">AES256, AES192, AES128, DES3, DES</span><span class="sxs-lookup"><span data-stu-id="4ac7c-136">AES256, AES192, AES128, DES3, DES</span></span>  
| <span data-ttu-id="4ac7c-137">Integrita protokolu IKEv2</span><span class="sxs-lookup"><span data-stu-id="4ac7c-137">IKEv2 Integrity</span></span>  | <span data-ttu-id="4ac7c-138">SHA384, SHA256, SHA1, MD5</span><span class="sxs-lookup"><span data-stu-id="4ac7c-138">SHA384, SHA256, SHA1, MD5</span></span>  |
| <span data-ttu-id="4ac7c-139">Skupina DH</span><span class="sxs-lookup"><span data-stu-id="4ac7c-139">DH Group</span></span>         | <span data-ttu-id="4ac7c-140">DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, None</span><span class="sxs-lookup"><span data-stu-id="4ac7c-140">DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, None</span></span> |
| <span data-ttu-id="4ac7c-141">Šifrování protokolem IPsec</span><span class="sxs-lookup"><span data-stu-id="4ac7c-141">IPsec Encryption</span></span> | <span data-ttu-id="4ac7c-142">GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, Žádné</span><span class="sxs-lookup"><span data-stu-id="4ac7c-142">GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, None</span></span>    |
| <span data-ttu-id="4ac7c-143">Integrita protokolu IPsec</span><span class="sxs-lookup"><span data-stu-id="4ac7c-143">IPsec Integrity</span></span>  | <span data-ttu-id="4ac7c-144">GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5</span><span class="sxs-lookup"><span data-stu-id="4ac7c-144">GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5</span></span> |
| <span data-ttu-id="4ac7c-145">Skupina PFS</span><span class="sxs-lookup"><span data-stu-id="4ac7c-145">PFS Group</span></span>        | <span data-ttu-id="4ac7c-146">PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, Žádná</span><span class="sxs-lookup"><span data-stu-id="4ac7c-146">PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, None</span></span> 
| <span data-ttu-id="4ac7c-147">Doba života přidružení zabezpečení v rychlém režimu</span><span class="sxs-lookup"><span data-stu-id="4ac7c-147">QM SA Lifetime</span></span>   | <span data-ttu-id="4ac7c-148">(**Volitelné**: výchozí hodnoty jsou použít, pokud není zadána)</span><span class="sxs-lookup"><span data-stu-id="4ac7c-148">(**Optional**: default values are used if not specified)</span></span><br><span data-ttu-id="4ac7c-149">Sekundy (integer; **min. 300** / výchozí hodnota 27 000 sekund)</span><span class="sxs-lookup"><span data-stu-id="4ac7c-149">Seconds (integer; **min. 300**/default 27000 seconds)</span></span><br><span data-ttu-id="4ac7c-150">Kilobajty (integer; **min. 1024** / výchozí hodnota 102 400 000 kilobajtů)</span><span class="sxs-lookup"><span data-stu-id="4ac7c-150">KBytes (integer; **min. 1024**/default 102400000 KBytes)</span></span>   |
| <span data-ttu-id="4ac7c-151">Selektor provozu</span><span class="sxs-lookup"><span data-stu-id="4ac7c-151">Traffic Selector</span></span> | <span data-ttu-id="4ac7c-152">UsePolicyBasedTrafficSelectors ** ($True nebo $False; **Volitelné**, výchozí $False není-li zadána)</span><span class="sxs-lookup"><span data-stu-id="4ac7c-152">UsePolicyBasedTrafficSelectors** ($True/$False; **Optional**, default $False if not specified)</span></span>    |
|  |  |

> [!IMPORTANT]
> 1. <span data-ttu-id="4ac7c-153">**Pokud GCMAES slouží jako algoritmus šifrování pomocí protokolu IPsec, je nutné vybrat hello stejné GCMAES algoritmus a délku klíče pro protokol IPsec Integrity; například pro obě pomocí GCMAES128**</span><span class="sxs-lookup"><span data-stu-id="4ac7c-153">**If GCMAES is used as for IPsec Encryption algorithm, you must select hello same GCMAES algorithm and key length for IPsec Integrity; for example, using GCMAES128 for both**</span></span>
> 2. <span data-ttu-id="4ac7c-154">Životnost přidružení zabezpečení hlavního režimu IKEv2 vyřešen v 28 800 sekund na branách Azure VPN hello</span><span class="sxs-lookup"><span data-stu-id="4ac7c-154">IKEv2 Main Mode SA lifetime is fixed at 28,800 seconds on hello Azure VPN gateways</span></span>
> 3. <span data-ttu-id="4ac7c-155">Nastavení "UsePolicyBasedTrafficSelectors" příliš$ True na připojení nakonfigurujete hello Azure VPN gateway tooconnect toopolicy VPN brány firewall založená na místní.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-155">Setting "UsePolicyBasedTrafficSelectors" too$True on a connection will configure hello Azure VPN gateway tooconnect toopolicy-based VPN firewall on premises.</span></span> <span data-ttu-id="4ac7c-156">Pokud povolíte PolicyBasedTrafficSelectors, je třeba tooensure hello odpovídající provoz selektory definovaný s všechny kombinace předponami vaší místní sítě (brány místní sítě) z předpony hello virtuální síť Azure, má vaše zařízení VPN místo any-to-any.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-156">If you enable PolicyBasedTrafficSelectors, you need tooensure your VPN device has hello matching traffic selectors defined with all combinations of your on-premises network (local network gateway) prefixes to/from hello Azure virtual network prefixes, instead of any-to-any.</span></span> <span data-ttu-id="4ac7c-157">Například pokud předponami vaší místní sítě se 10.1.0.0/16 a 10.2.0.0/16 a předponami vaší virtuální sítě jsou 192.168.0.0/16 a 172.16.0.0/16, je třeba toospecify hello následující selektory provozu:</span><span class="sxs-lookup"><span data-stu-id="4ac7c-157">For example, if your on-premises network prefixes are 10.1.0.0/16 and 10.2.0.0/16, and your virtual network prefixes are 192.168.0.0/16 and 172.16.0.0/16, you need toospecify hello following traffic selectors:</span></span>
>    * <span data-ttu-id="4ac7c-158">10.1.0.0/16 <====> 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="4ac7c-158">10.1.0.0/16 <====> 192.168.0.0/16</span></span>
>    * <span data-ttu-id="4ac7c-159">10.1.0.0/16 <====> 172.16.0.0/16</span><span class="sxs-lookup"><span data-stu-id="4ac7c-159">10.1.0.0/16 <====> 172.16.0.0/16</span></span>
>    * <span data-ttu-id="4ac7c-160">10.2.0.0/16 <====> 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="4ac7c-160">10.2.0.0/16 <====> 192.168.0.0/16</span></span>
>    * <span data-ttu-id="4ac7c-161">10.2.0.0/16 <====> 172.16.0.0/16</span><span class="sxs-lookup"><span data-stu-id="4ac7c-161">10.2.0.0/16 <====> 172.16.0.0/16</span></span>

<span data-ttu-id="4ac7c-162">Další informace týkající se provozu na základě zásad selektory najdete v tématu [připojení více místně na základě zásad zařízení VPN](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="4ac7c-162">For more information regarding policy-based traffic selectors, see [Connect multiple on-premises policy-based VPN devices](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span></span>

<span data-ttu-id="4ac7c-163">Následující tabulka seznamy Hello hello odpovídající skupin Diffie-Hellman podporovaná hello vlastní zásady:</span><span class="sxs-lookup"><span data-stu-id="4ac7c-163">hello following table lists hello corresponding Diffie-Hellman Groups supported by hello custom policy:</span></span>

| <span data-ttu-id="4ac7c-164">**Skupina Diffie-Hellman**</span><span class="sxs-lookup"><span data-stu-id="4ac7c-164">**Diffie-Hellman Group**</span></span>  | <span data-ttu-id="4ac7c-165">**DHGroup**</span><span class="sxs-lookup"><span data-stu-id="4ac7c-165">**DHGroup**</span></span>              | <span data-ttu-id="4ac7c-166">**PFSGroup**</span><span class="sxs-lookup"><span data-stu-id="4ac7c-166">**PFSGroup**</span></span> | <span data-ttu-id="4ac7c-167">**Délka klíče**</span><span class="sxs-lookup"><span data-stu-id="4ac7c-167">**Key length**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4ac7c-168">1</span><span class="sxs-lookup"><span data-stu-id="4ac7c-168">1</span></span>                         | <span data-ttu-id="4ac7c-169">DHGroup1</span><span class="sxs-lookup"><span data-stu-id="4ac7c-169">DHGroup1</span></span>                 | <span data-ttu-id="4ac7c-170">PFS1</span><span class="sxs-lookup"><span data-stu-id="4ac7c-170">PFS1</span></span>         | <span data-ttu-id="4ac7c-171">768bitová skupina MODP</span><span class="sxs-lookup"><span data-stu-id="4ac7c-171">768-bit MODP</span></span>   |
| <span data-ttu-id="4ac7c-172">2</span><span class="sxs-lookup"><span data-stu-id="4ac7c-172">2</span></span>                         | <span data-ttu-id="4ac7c-173">DHGroup2</span><span class="sxs-lookup"><span data-stu-id="4ac7c-173">DHGroup2</span></span>                 | <span data-ttu-id="4ac7c-174">PFS2</span><span class="sxs-lookup"><span data-stu-id="4ac7c-174">PFS2</span></span>         | <span data-ttu-id="4ac7c-175">1024bitová skupina MODP</span><span class="sxs-lookup"><span data-stu-id="4ac7c-175">1024-bit MODP</span></span>  |
| <span data-ttu-id="4ac7c-176">14</span><span class="sxs-lookup"><span data-stu-id="4ac7c-176">14</span></span>                        | <span data-ttu-id="4ac7c-177">DHGroup14</span><span class="sxs-lookup"><span data-stu-id="4ac7c-177">DHGroup14</span></span><br><span data-ttu-id="4ac7c-178">DHGroup2048</span><span class="sxs-lookup"><span data-stu-id="4ac7c-178">DHGroup2048</span></span> | <span data-ttu-id="4ac7c-179">PFS2048</span><span class="sxs-lookup"><span data-stu-id="4ac7c-179">PFS2048</span></span>      | <span data-ttu-id="4ac7c-180">2048bitová skupina MODP</span><span class="sxs-lookup"><span data-stu-id="4ac7c-180">2048-bit MODP</span></span>  |
| <span data-ttu-id="4ac7c-181">19</span><span class="sxs-lookup"><span data-stu-id="4ac7c-181">19</span></span>                        | <span data-ttu-id="4ac7c-182">ECP256</span><span class="sxs-lookup"><span data-stu-id="4ac7c-182">ECP256</span></span>                   | <span data-ttu-id="4ac7c-183">ECP256</span><span class="sxs-lookup"><span data-stu-id="4ac7c-183">ECP256</span></span>       | <span data-ttu-id="4ac7c-184">256bitová skupina ECP</span><span class="sxs-lookup"><span data-stu-id="4ac7c-184">256-bit ECP</span></span>    |
| <span data-ttu-id="4ac7c-185">20</span><span class="sxs-lookup"><span data-stu-id="4ac7c-185">20</span></span>                        | <span data-ttu-id="4ac7c-186">ECP384</span><span class="sxs-lookup"><span data-stu-id="4ac7c-186">ECP384</span></span>                   | <span data-ttu-id="4ac7c-187">ECP284</span><span class="sxs-lookup"><span data-stu-id="4ac7c-187">ECP284</span></span>       | <span data-ttu-id="4ac7c-188">384bitová skupina ECP</span><span class="sxs-lookup"><span data-stu-id="4ac7c-188">384-bit ECP</span></span>    |
| <span data-ttu-id="4ac7c-189">24</span><span class="sxs-lookup"><span data-stu-id="4ac7c-189">24</span></span>                        | <span data-ttu-id="4ac7c-190">DHGroup24</span><span class="sxs-lookup"><span data-stu-id="4ac7c-190">DHGroup24</span></span>                | <span data-ttu-id="4ac7c-191">PFS24</span><span class="sxs-lookup"><span data-stu-id="4ac7c-191">PFS24</span></span>        | <span data-ttu-id="4ac7c-192">2048bitová skupina MODP</span><span class="sxs-lookup"><span data-stu-id="4ac7c-192">2048-bit MODP</span></span>  |

<span data-ttu-id="4ac7c-193">Odkazovat příliš[RFC3526](https://tools.ietf.org/html/rfc3526) a [RFC5114](https://tools.ietf.org/html/rfc5114) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-193">Refer too[RFC3526](https://tools.ietf.org/html/rfc3526) and [RFC5114](https://tools.ietf.org/html/rfc5114) for more details.</span></span>

## <span data-ttu-id="4ac7c-194"><a name ="crossprem"></a>Část 3 – vytvoření nového připojení S2S VPN pomocí zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="4ac7c-194"><a name ="crossprem"></a>Part 3 - Create a new S2S VPN connection with IPsec/IKE policy</span></span>

<span data-ttu-id="4ac7c-195">Tato část vás provede kroky hello k vytvoření připojení S2S VPN pomocí zásad protokolu IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-195">This section walks you through hello steps of creating a S2S VPN connection with an IPsec/IKE policy.</span></span> <span data-ttu-id="4ac7c-196">Hello následujících kroků vytvořte připojení hello jak je znázorněno v diagramu hello:</span><span class="sxs-lookup"><span data-stu-id="4ac7c-196">hello following steps create hello connection as shown in hello diagram:</span></span>

![zásady s2s](./media/vpn-gateway-ipsecikepolicy-rm-powershell/s2spolicy.png)

<span data-ttu-id="4ac7c-198">V tématu [vytvoření připojení S2S VPN](vpn-gateway-create-site-to-site-rm-powershell.md) podrobnější podrobné pokyny pro vytvoření připojení S2S VPN.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-198">See [Create a S2S VPN connection](vpn-gateway-create-site-to-site-rm-powershell.md) for more detailed step-by-step instructions for creating a S2S VPN connection.</span></span>

### <span data-ttu-id="4ac7c-199"><a name="before"></a>Než začnete</span><span class="sxs-lookup"><span data-stu-id="4ac7c-199"><a name="before"></a>Before you begin</span></span>

* <span data-ttu-id="4ac7c-200">Ověřte, že máte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-200">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="4ac7c-201">Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4ac7c-201">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="4ac7c-202">Nainstalujte rutiny Powershellu pro Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-202">Install hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="4ac7c-203">V tématu [Přehled prostředí Azure PowerShell](/powershell/azure/overview) Další informace o instalaci rutin prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-203">See [Overview of Azure PowerShell](/powershell/azure/overview) for more information about installing hello PowerShell cmdlets.</span></span>

### <span data-ttu-id="4ac7c-204"><a name="createvnet1"></a>Krok 1 – Vytvoření hello virtuální sítě, brána sítě VPN a bránu místní sítě</span><span class="sxs-lookup"><span data-stu-id="4ac7c-204"><a name="createvnet1"></a>Step 1 - Create hello virtual network, VPN gateway, and local network gateway</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="4ac7c-205">1. Deklarace proměnných</span><span class="sxs-lookup"><span data-stu-id="4ac7c-205">1. Declare your variables</span></span>

<span data-ttu-id="4ac7c-206">Pro toto cvičení začneme deklarací proměnných.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-206">For this exercise, we start by declaring our variables.</span></span> <span data-ttu-id="4ac7c-207">Zda tooreplace hello hodnoty vlastními být při konfiguraci pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-207">Be sure tooreplace hello values with your own when configuring for production.</span></span>

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

#### <a name="2-connect-tooyour-subscription-and-create-a-new-resource-group"></a><span data-ttu-id="4ac7c-208">2. Připojení tooyour odběru a vytvořit novou skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="4ac7c-208">2. Connect tooyour subscription and create a new resource group</span></span>

<span data-ttu-id="4ac7c-209">Ujistěte se, že jste přešli tooPowerShell režimu toouse hello rutiny správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-209">Make sure you switch tooPowerShell mode toouse hello Resource Manager cmdlets.</span></span> <span data-ttu-id="4ac7c-210">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="4ac7c-210">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="4ac7c-211">Otevřete konzolu prostředí PowerShell a připojte tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-211">Open your PowerShell console and connect tooyour account.</span></span> <span data-ttu-id="4ac7c-212">Použijte následující ukázka toohelp, ke kterým se připojujete hello:</span><span class="sxs-lookup"><span data-stu-id="4ac7c-212">Use hello following sample toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-hello-virtual-network-vpn-gateway-and-local-network-gateway"></a><span data-ttu-id="4ac7c-213">3. Vytvoření virtuální sítě hello, brána sítě VPN a bránu místní sítě</span><span class="sxs-lookup"><span data-stu-id="4ac7c-213">3. Create hello virtual network, VPN gateway, and local network gateway</span></span>

<span data-ttu-id="4ac7c-214">Hello následující ukázka vytvoří hello virtuální sítě, virtuální sítě TestVNet1, se tři podsítě a brány VPN hello.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-214">hello following sample creates hello virtual network, TestVNet1, with three subnets, and hello VPN gateway.</span></span> <span data-ttu-id="4ac7c-215">Při nahrazování hodnot je důležité vždy přiřadit podsíti brány konkrétní název GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-215">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="4ac7c-216">Pokud použijete jiný název, vytvoření brány se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-216">If you name it something else, your gateway creation fails.</span></span>

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

### <span data-ttu-id="4ac7c-217"><a name="s2sconnection"></a>Krok 2 – Vytvoření připojení S2S VPN pomocí zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="4ac7c-217"><a name="s2sconnection"></a>Step 2 - Create a S2S VPN connection with an IPsec/IKE policy</span></span>

#### <a name="1-create-an-ipsecike-policy"></a><span data-ttu-id="4ac7c-218">1. Vytvoření zásady protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="4ac7c-218">1. Create an IPsec/IKE policy</span></span>

<span data-ttu-id="4ac7c-219">Následující ukázkový skript Hello vytvoří zásadu protokolu IPsec/IKE se hello následující parametry a algoritmy:</span><span class="sxs-lookup"><span data-stu-id="4ac7c-219">hello following sample script creates an IPsec/IKE policy with hello following algorithms and parameters:</span></span>

* <span data-ttu-id="4ac7c-220">IKEv2: DHGroup24 AES256, SHA384</span><span class="sxs-lookup"><span data-stu-id="4ac7c-220">IKEv2: AES256, SHA384, DHGroup24</span></span>
* <span data-ttu-id="4ac7c-221">Protokol IPsec: AES256, SHA256, PFS24 7200 životnost přidružení zabezpečení sekund & 2048KB</span><span class="sxs-lookup"><span data-stu-id="4ac7c-221">IPsec: AES256, SHA256, PFS24, SA Lifetime 7200 seconds & 2048KB</span></span>

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 2048
```

<span data-ttu-id="4ac7c-222">Pokud používáte GCMAES pro IPsec, musíte použít hello stejné GCMAES algoritmus a délku klíče pro šifrování pomocí protokolu IPsec a integritu, například:</span><span class="sxs-lookup"><span data-stu-id="4ac7c-222">If you use GCMAES for IPsec, you must use hello same GCMAES algorithm and key length for both IPsec encryption and integrity, for example:</span></span>

* <span data-ttu-id="4ac7c-223">IKEv2: DHGroup24 AES256, SHA384</span><span class="sxs-lookup"><span data-stu-id="4ac7c-223">IKEv2: AES256, SHA384, DHGroup24</span></span>
* <span data-ttu-id="4ac7c-224">Protokol IPsec: **GCMAES256, GCMAES256**, PFS24 7200 životnost přidružení zabezpečení sekund & 2048 KB</span><span class="sxs-lookup"><span data-stu-id="4ac7c-224">IPsec: **GCMAES256, GCMAES256**, PFS24, SA Lifetime 7200 seconds & 2048KB</span></span>

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption GCMAES256 -IpsecIntegrity GCMAES256 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 2048
```

#### <a name="2-create-hello-s2s-vpn-connection-with-hello-ipsecike-policy"></a><span data-ttu-id="4ac7c-225">2. Vytvoření připojení S2S VPN hello s hello zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="4ac7c-225">2. Create hello S2S VPN connection with hello IPsec/IKE policy</span></span>

<span data-ttu-id="4ac7c-226">Vytvoření připojení k síti VPN S2S a použití zásad protokolu IPsec/IKE hello vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-226">Create an S2S VPN connection and apply hello IPsec/IKE policy created earlier.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

<span data-ttu-id="4ac7c-227">Volitelně můžete přidat "-UsePolicyBasedTrafficSelectors $True" toohello vytvořit připojení rutiny tooenable Azure VPN gateway tooconnect na základě toopolicy zařízení VPN místně, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-227">You can optionally add "-UsePolicyBasedTrafficSelectors $True" toohello create connection cmdlet tooenable Azure VPN gateway tooconnect toopolicy-based VPN devices on premises, as described above.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4ac7c-228">Po připojení je zadán pro zásady protokolu IPsec/IKE, hello Azure VPN gateway pouze odeslat nebo přijmout hello protokolu IPsec/IKE návrh s zadané kryptografické algoritmy a klíče síly na konkrétní připojení.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-228">Once an IPsec/IKE policy is specified on a connection, hello Azure VPN gateway will only send or accept hello IPsec/IKE proposal with specified cryptographic algorithms and key strengths on that particular connection.</span></span> <span data-ttu-id="4ac7c-229">Zajistěte, aby vaše místní zařízení VPN pro připojení hello používá nebo přijímá hello kombinaci přesný zásad, jinak nebude vytvořit tunel S2S VPN hello.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-229">Make sure your on-premises VPN device for hello connection uses or accepts hello exact policy combination, otherwise hello S2S VPN tunnel will not establish.</span></span>


## <span data-ttu-id="4ac7c-230"><a name ="vnet2vnet"></a>Součástí 4 – vytvoření nového připojení VNet-to-VNet s zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="4ac7c-230"><a name ="vnet2vnet"></a>Part 4 - Create a new VNet-to-VNet connection with IPsec/IKE policy</span></span>

<span data-ttu-id="4ac7c-231">Hello kroky k vytvoření připojení VNet-to-VNet pomocí zásad protokolu IPsec/IKE jsou podobné toothat připojení S2S VPN.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-231">hello steps of creating a VNet-to-VNet connection with an IPsec/IKE policy are similar toothat of a S2S VPN connection.</span></span> <span data-ttu-id="4ac7c-232">Hello následující ukázkové skripty vytvořit hello připojení jak je vidět v diagramu hello:</span><span class="sxs-lookup"><span data-stu-id="4ac7c-232">hello following sample scripts create hello connection as shown in hello diagram:</span></span>

![zásady v2v](./media/vpn-gateway-ipsecikepolicy-rm-powershell/v2vpolicy.png)

<span data-ttu-id="4ac7c-234">V tématu [vytvořit připojení VNet-to-VNet](vpn-gateway-vnet-vnet-rm-ps.md) podrobné kroky pro vytvoření připojení VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-234">See [Create a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md) for more detailed steps for creating a VNet-to-VNet connection.</span></span> <span data-ttu-id="4ac7c-235">Je třeba provést [část 3](#crossprem) toocreate a konfigurace virtuální sítě TestVNet1 a hello brány VPN.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-235">You must complete [Part 3](#crossprem) toocreate and configure TestVNet1 and hello VPN Gateway.</span></span>

### <span data-ttu-id="4ac7c-236"><a name="createvnet2"></a>Krok 1 – Vytvoření hello druhý virtuální sítě a brány VPN</span><span class="sxs-lookup"><span data-stu-id="4ac7c-236"><a name="createvnet2"></a>Step 1 - Create hello second virtual network and VPN gateway</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="4ac7c-237">1. Deklarace proměnných</span><span class="sxs-lookup"><span data-stu-id="4ac7c-237">1. Declare your variables</span></span>

<span data-ttu-id="4ac7c-238">Být jisti tooreplace hello hodnoty hello ty, které jsou chcete toouse pro vaši konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-238">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

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

#### <a name="2-create-hello-second-virtual-network-and-vpn-gateway-in-hello-new-resource-group"></a><span data-ttu-id="4ac7c-239">2. Vytvoření hello druhý virtuální sítě a brány VPN v hello novou skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="4ac7c-239">2. Create hello second virtual network and VPN gateway in hello new resource group</span></span>

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

### <a name="step-2---create-a-vnet-tovnet-connection-with-hello-ipsecike-policy"></a><span data-ttu-id="4ac7c-240">Krok 2 – Vytvoření připojení virtuální sítě toVNet pomocí hello zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="4ac7c-240">Step 2 - Create a VNet-toVNet connection with hello IPsec/IKE policy</span></span>

<span data-ttu-id="4ac7c-241">Podobně jako toohello připojení S2S VPN, vytvoření zásady protokolu IPsec/IKE potom použít toopolicy toohello nové připojení.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-241">Similar toohello S2S VPN connection, create an IPsec/IKE policy then apply toopolicy toohello new connection.</span></span>

#### <a name="1-create-an-ipsecike-policy"></a><span data-ttu-id="4ac7c-242">1. Vytvoření zásady protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="4ac7c-242">1. Create an IPsec/IKE policy</span></span>

<span data-ttu-id="4ac7c-243">Následující ukázkový skript Hello vytvoří jiné zásady protokolu IPsec/IKE s hello následující parametry a algoritmy:</span><span class="sxs-lookup"><span data-stu-id="4ac7c-243">hello following sample script creates a different IPsec/IKE policy with hello following algorithms and parameters:</span></span>
* <span data-ttu-id="4ac7c-244">IKEv2: DHGroup14 AES128, SHA1,</span><span class="sxs-lookup"><span data-stu-id="4ac7c-244">IKEv2: AES128, SHA1, DHGroup14</span></span>
* <span data-ttu-id="4ac7c-245">Protokol IPsec: GCMAES128, GCMAES128, PFS14, životnost přidružení zabezpečení 7200 sekund a 4096KB</span><span class="sxs-lookup"><span data-stu-id="4ac7c-245">IPsec: GCMAES128, GCMAES128, PFS14, SA Lifetime 7200 seconds & 4096KB</span></span>

```powershell
$ipsecpolicy2 = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup PFS14 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 4096
```

#### <a name="2-create-vnet-to-vnet-connections-with-hello-ipsecike-policy"></a><span data-ttu-id="4ac7c-246">2. Vytvoření připojení VNet-to-VNet s hello zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="4ac7c-246">2. Create VNet-to-VNet connections with hello IPsec/IKE policy</span></span>

<span data-ttu-id="4ac7c-247">Umožňuje vytvořit připojení VNet-to-VNet a použít zásady protokolu IPsec/IKE hello, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-247">Create a VNet-to-VNet connection and apply hello IPsec/IKE policy you created.</span></span> <span data-ttu-id="4ac7c-248">V tomto příkladu jsou obě brány v hello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-248">In this example, both gateways are in hello same subscription.</span></span> <span data-ttu-id="4ac7c-249">Proto je možné toocreate a nakonfigurovat obě připojení s hello stejné zásady protokolu IPsec/IKE v hello stejné relace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-249">So it is possible toocreate and configure both connections with hello same IPsec/IKE policy in hello same PowerShell session.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2  -ResourceGroupName $RG2

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'
```

> [!IMPORTANT]
> <span data-ttu-id="4ac7c-250">Po připojení je zadán pro zásady protokolu IPsec/IKE, hello Azure VPN gateway pouze odeslat nebo přijmout hello protokolu IPsec/IKE návrh s zadané kryptografické algoritmy a klíče síly na konkrétní připojení.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-250">Once an IPsec/IKE policy is specified on a connection, hello Azure VPN gateway will only send or accept hello IPsec/IKE proposal with specified cryptographic algorithms and key strengths on that particular connection.</span></span> <span data-ttu-id="4ac7c-251">Ujistěte se, zda text hello zásady protokolu IPsec pro obě připojení jsou hello stejné, jinak nebude navázání připojení VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-251">Make sure hello IPsec policies for both connections are hello same, otherwise the VNet-to-VNet connection will not establish.</span></span>

<span data-ttu-id="4ac7c-252">Po dokončení těchto kroků, hello připojení za pár minut a budete mít hello následující topologie sítě, jak je znázorněno v začátku hello:</span><span class="sxs-lookup"><span data-stu-id="4ac7c-252">After completing these steps, hello connection is established in a few minutes, and you will have hello following network topology as shown in hello beginning:</span></span>

![zásady protokolu IPSec ike](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)


## <span data-ttu-id="4ac7c-254"><a name ="managepolicy"></a>Část 5 – zásady aktualizace protokolu IPsec/IKE pro připojení</span><span class="sxs-lookup"><span data-stu-id="4ac7c-254"><a name ="managepolicy"></a>Part 5 - Update IPsec/IKE policy for a connection</span></span>

<span data-ttu-id="4ac7c-255">poslední část Hello se dozvíte, jak toomanage zásad protokolu IPsec/IKE pro existující připojení S2S nebo VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-255">hello last section shows you how toomanage IPsec/IKE policy for an existing S2S or VNet-to-VNet connection.</span></span> <span data-ttu-id="4ac7c-256">cvičení Hello níže vás provede následující operace na připojení hello:</span><span class="sxs-lookup"><span data-stu-id="4ac7c-256">hello exercise below walks you through hello following operations on a connection:</span></span>

1. <span data-ttu-id="4ac7c-257">Zobrazit zásady protokolu IPsec/IKE hello připojení</span><span class="sxs-lookup"><span data-stu-id="4ac7c-257">Show hello IPsec/IKE policy of a connection</span></span>
2. <span data-ttu-id="4ac7c-258">Přidat nebo aktualizovat připojení tooa zásad protokolu IPsec/IKE hello</span><span class="sxs-lookup"><span data-stu-id="4ac7c-258">Add or update hello IPsec/IKE policy tooa connection</span></span>
3. <span data-ttu-id="4ac7c-259">Odebrání zásad protokolu IPsec/IKE hello připojení</span><span class="sxs-lookup"><span data-stu-id="4ac7c-259">Remove hello IPsec/IKE policy from a connection</span></span>

<span data-ttu-id="4ac7c-260">Hello stejný postup použijte tooboth S2S a připojení VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-260">hello same steps apply tooboth S2S and VNet-to-VNet connections.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4ac7c-261">Zásady protokolu IPsec/IKE je podporována v *standardní* a *HighPerformance* založené na trasách pouze VPN Gateway.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-261">IPsec/IKE policy is supported on *Standard* and *HighPerformance* route-based VPN gateways only.</span></span> <span data-ttu-id="4ac7c-262">Nefunguje se na základní skladová položka brány hello nebo brány sítě VPN založené na zásadách hello.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-262">It does not work on hello Basic gateway SKU or hello policy-based VPN gateway.</span></span>

#### <a name="1-show-hello-ipsecike-policy-of-a-connection"></a><span data-ttu-id="4ac7c-263">1. Zobrazit zásady protokolu IPsec/IKE hello připojení</span><span class="sxs-lookup"><span data-stu-id="4ac7c-263">1. Show hello IPsec/IKE policy of a connection</span></span>

<span data-ttu-id="4ac7c-264">Hello následující příklad ukazuje, jak tooget hello zásady protokolu IPsec/IKE nakonfigurované na připojení.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-264">hello following example shows how tooget hello IPsec/IKE policy configured on a connection.</span></span> <span data-ttu-id="4ac7c-265">skripty Hello také pokračovat od hello cvičení výše.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-265">hello scripts also continue from hello exercises above.</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

<span data-ttu-id="4ac7c-266">poslední příkaz Hello uvádí aktuální zásady protokolu IPsec/IKE hello nakonfigurované na hello připojení, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-266">hello last command lists hello current IPsec/IKE policy configured on hello connection, if there is any.</span></span> <span data-ttu-id="4ac7c-267">Následující ukázkový výstup Hello je pro připojení hello:</span><span class="sxs-lookup"><span data-stu-id="4ac7c-267">hello following sample output is for hello connection:</span></span>

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

<span data-ttu-id="4ac7c-268">Pokud nejsou nakonfigurovány žádné zásady protokolu IPsec/IKE, hello příkazu (PS > $connection6.policy) získá návratový prázdná.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-268">If there is no IPsec/IKE policy configured, hello command (PS> $connection6.policy) gets an empty return.</span></span> <span data-ttu-id="4ac7c-269">Neznamená to protokolu IPsec/IKE není konfigurován na připojení hello, ale že se žádné vlastní zásady protokolu IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-269">It does not mean IPsec/IKE is not configured on hello connection, but that there is no custom IPsec/IKE policy.</span></span> <span data-ttu-id="4ac7c-270">skutečné připojení Hello používá výchozí zásady hello mezi místní zařízení VPN a hello brány Azure VPN.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-270">hello actual connection uses hello default policy negotiated between your on-premises VPN device and hello Azure VPN gateway.</span></span>

#### <a name="2-add-or-update-an-ipsecike-policy-for-a-connection"></a><span data-ttu-id="4ac7c-271">2. Přidat nebo aktualizovat zásady protokolu IPsec/IKE pro připojení</span><span class="sxs-lookup"><span data-stu-id="4ac7c-271">2. Add or update an IPsec/IKE policy for a connection</span></span>

<span data-ttu-id="4ac7c-272">Hello kroky tooadd novou zásadu nebo aktualizovat existující zásady na připojení jsou hello stejné: Vytvořte novou zásadu a použít hello nové zásady toohello připojení.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-272">hello steps tooadd a new policy or update an existing policy on a connection are hello same: create a new policy then apply hello new policy toohello connection.</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$newpolicy6   = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup None -SALifeTimeSeconds 3600 -SADataSizeKilobytes 2048

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6
```

<span data-ttu-id="4ac7c-273">tooenable "UsePolicyBasedTrafficSelectors" při připojování tooan místní zařízení VPN založené na zásadách přidat hello "-UsePolicyBaseTrafficSelectors" parametr toohello rutiny, nebo ji nastavte příliš$ False toodisable hello možnost:</span><span class="sxs-lookup"><span data-stu-id="4ac7c-273">tooenable "UsePolicyBasedTrafficSelectors" when connecting tooan on-premises policy-based VPN device, add hello "-UsePolicyBaseTrafficSelectors" parameter toohello cmdlet, or set it too$False toodisable hello option:</span></span>

```powershell
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6 -UsePolicyBasedTrafficSelectors $True
```

<span data-ttu-id="4ac7c-274">Můžete získat hello připojení znovu toocheck Pokud hello zásada se aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-274">You can get hello connection again toocheck if hello policy is updated.</span></span>

```powershell
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

<span data-ttu-id="4ac7c-275">Výstup hello hello poslední řádek, byste měli vidět, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="4ac7c-275">You should see hello output from hello last line, as shown in hello following example:</span></span>

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

#### <a name="3-remove-an-ipsecike-policy-from-a-connection"></a><span data-ttu-id="4ac7c-276">3. Odebrání připojení zásady protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="4ac7c-276">3. Remove an IPsec/IKE policy from a connection</span></span>

<span data-ttu-id="4ac7c-277">Vlastní zásady hello odebraný z připojení brány Azure VPN hello vrátí zpět toohello [výchozí seznam protokolu IPsec/IKE návrhy](vpn-gateway-about-vpn-devices.md) a obnoví vyjednávání znovu s vaše místní zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-277">Once you remove hello custom policy from a connection, hello Azure VPN gateway reverts back toohello [default list of IPsec/IKE proposals](vpn-gateway-about-vpn-devices.md) and renegotiates again with your on-premises VPN device.</span></span>

```powershell
$RG1           = "TestPolicyRG1"
$Connection16  = "VNet1toSite6"
$connection6   = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$currentpolicy = $connection6.IpsecPolicies[0]
$connection6.IpsecPolicies.Remove($currentpolicy)

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6
```

<span data-ttu-id="4ac7c-278">Můžete použít stejný skriptu toocheck text hello, pokud hello zásada byla odebrána z hello připojení.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-278">You can use hello same script toocheck if hello policy has been removed from hello connection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4ac7c-279">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4ac7c-279">Next steps</span></span>

<span data-ttu-id="4ac7c-280">V tématu [připojení více místně na základě zásad zařízení VPN](vpn-gateway-connect-multiple-policybased-rm-ps.md) další podrobnosti týkající se provozu na základě zásad selektorů.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-280">See [Connect multiple on-premises policy-based VPN devices](vpn-gateway-connect-multiple-policybased-rm-ps.md) for more details regarding policy-based traffic selectors.</span></span>

<span data-ttu-id="4ac7c-281">Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4ac7c-281">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="4ac7c-282">Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4ac7c-282">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>
