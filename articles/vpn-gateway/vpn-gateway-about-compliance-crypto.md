---
title: "aaaAbout kryptografických požadavky a Azure VPN Gateway | Microsoft Docs"
description: "Tento článek popisuje požadavky na šifrování a brány Azure VPN"
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
ms.date: 05/22/2017
ms.author: yushwang
ms.openlocfilehash: af5f14d66beeea5316218f9788c4ad7876826162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="about-cryptographic-requirements-and-azure-vpn-gateways"></a><span data-ttu-id="17792-103">O kryptografických požadavky a brány Azure VPN</span><span class="sxs-lookup"><span data-stu-id="17792-103">About cryptographic requirements and Azure VPN gateways</span></span>

<span data-ttu-id="17792-104">Tento článek popisuje, jak konfigurovat Azure VPN Gateway toosatisfy kryptografických požadavky na tunelů S2S VPN mezi různými místy a připojení VNet-to-VNet v rámci Azure.</span><span class="sxs-lookup"><span data-stu-id="17792-104">This article discusses how you can configure Azure VPN gateways toosatisfy your cryptographic requirements for both cross-premises S2S VPN tunnels and VNet-to-VNet connections within Azure.</span></span> 

## <a name="about-ipsec-and-ike-policy-parameters-for-azure-vpn-gateways"></a><span data-ttu-id="17792-105">O parametry zásad protokolu IPsec a IKE pro Azure VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="17792-105">About IPsec and IKE policy parameters for Azure VPN gateways</span></span>
<span data-ttu-id="17792-106">Protokol IPsec a IKE standardní podporuje širokou škálu kryptografických algoritmů v různých kombinacích.</span><span class="sxs-lookup"><span data-stu-id="17792-106">IPsec and IKE protocol standard supports a wide range of cryptographic algorithms in various combinations.</span></span> <span data-ttu-id="17792-107">Pokud zákazníci v požadavku konkrétní kombinaci kryptografické algoritmy a parametry, Azure VPN Gateway pomocí sady výchozí návrhů.</span><span class="sxs-lookup"><span data-stu-id="17792-107">If customers do not request a specific combination of cryptographic algorithms and parameters, Azure VPN gateways use a set of default proposals.</span></span> <span data-ttu-id="17792-108">Hello výchozí zásady sady členové toomaximize Interoperabilita s širokou škálu zařízení VPN třetích stran v výchozí konfigurace.</span><span class="sxs-lookup"><span data-stu-id="17792-108">hello default policy sets were chosen toomaximize interoperability with a wide range of third-party VPN devices in default configurations.</span></span> <span data-ttu-id="17792-109">V důsledku toho nelze hello zásady a hello počet návrhy zahrnují všechny možné kombinace dostupné kryptografické algoritmy a klíče síly.</span><span class="sxs-lookup"><span data-stu-id="17792-109">As a result, hello policies and hello number of proposals cannot cover all possible combinations of available cryptographic algorithms and key strengths.</span></span>

<span data-ttu-id="17792-110">Hello výchozí zásady nastavení pro bránu Azure VPN je uvedené v dokumentu hello: [informace o zařízeních VPN a parametry protokolu IPsec/IKE pro připojení Site-to-Site VPN Gateway](vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="17792-110">hello default policy set for Azure VPN gateway is listed in hello document: [About VPN devices and IPsec/IKE parameters for Site-to-Site VPN Gateway connections](vpn-gateway-about-vpn-devices.md).</span></span>

## <a name="cryptographic-requirements"></a><span data-ttu-id="17792-111">Kryptografické požadavky</span><span class="sxs-lookup"><span data-stu-id="17792-111">Cryptographic requirements</span></span>
<span data-ttu-id="17792-112">Pro komunikaci, které vyžadují určité kryptografické algoritmy nebo parametry obvykle kvůli toocompliance nebo zabezpečení požadavky zákazníků teď můžete nakonfigurovat jejich toouse brány Azure VPN vlastní zásady protokolu IPsec/IKE s konkrétní kryptografických algoritmy a klíče síly, nikoli hello Azure výchozí zásady skupiny.</span><span class="sxs-lookup"><span data-stu-id="17792-112">For communications that require specific cryptographic algorithms or parameters, typically due toocompliance or security requirements, customers can now configure their Azure VPN gateways toouse a custom IPsec/IKE policy with specific cryptographic algorithms and key strengths, rather than hello Azure default policy sets.</span></span>

<span data-ttu-id="17792-113">Například hello IKEv2 hlavního režimu zásady pro Azure VPN Gateway využívat pouze Diffie-Hellman Skupina 2 (1 024 bitů), zatímco zákazníkům může být nutné toospecify silnější použít v IKE, jako jsou skupiny 14 (2048 bitů), 24 skupinu (skupiny MODP 2048 bitů) nebo ECP (elliptic toobe skupiny křivkou skupin) 256 nebo 384 bit (skupiny 19 a skupiny 20).</span><span class="sxs-lookup"><span data-stu-id="17792-113">For example, hello IKEv2 main mode policies for Azure VPN gateways utilize only Diffie-Hellman Group 2 (1024 bits), whereas customers may need toospecify stronger groups toobe used in IKE, such as Group 14 (2048-bit), Group 24 (2048-bit MODP Group), or ECP (elliptic curve groups) 256 or 384 bit (Group 19 and Group 20, respectively).</span></span> <span data-ttu-id="17792-114">Podobné požadavky zásad tooIPsec rychlého režimu také.</span><span class="sxs-lookup"><span data-stu-id="17792-114">Similar requirements apply tooIPsec quick mode policies as well.</span></span>

## <a name="custom-ipsecike-policy-with-azure-vpn-gateways"></a><span data-ttu-id="17792-115">Vlastní zásady protokolu IPsec/IKE pomocí bran Azure VPN</span><span class="sxs-lookup"><span data-stu-id="17792-115">Custom IPsec/IKE policy with Azure VPN gateways</span></span>
<span data-ttu-id="17792-116">Azure VPN Gateway teď podporují připojení, vlastní zásady protokolu IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="17792-116">Azure VPN gateways now support per-connection, custom IPsec/IKE policy.</span></span> <span data-ttu-id="17792-117">Site-to-Site nebo připojení VNet-to-VNet lze vybrat konkrétní kombinaci kryptografických algoritmů IPsec a IKE s hello potřeby síly klíče, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="17792-117">For a Site-to-Site or VNet-to-VNet connection, you can choose a specific combination of cryptographic algorithms for IPsec and IKE with hello desired key strength, as shown in hello following example:</span></span>

![zásady protokolu IPSec ike](./media/vpn-gateway-about-compliance-crypto/ipsecikepolicy.png)

<span data-ttu-id="17792-119">Můžete vytvořit zásady protokolu IPsec/IKE a použít tooa nové nebo existující připojení.</span><span class="sxs-lookup"><span data-stu-id="17792-119">You can create an IPsec/IKE policy and apply tooa new or existing connection.</span></span> 

### <a name="workflow"></a><span data-ttu-id="17792-120">Pracovní postup</span><span class="sxs-lookup"><span data-stu-id="17792-120">Workflow</span></span>

1. <span data-ttu-id="17792-121">Jak je popsáno v dalších toodocuments jak vytvořit hello virtuální sítě, brány sítě VPN nebo brány místní sítě pro topologie připojení</span><span class="sxs-lookup"><span data-stu-id="17792-121">Create hello virtual networks, VPN gateways, or local network gateways for your connectivity topology as described in other how-toodocuments</span></span>
2. <span data-ttu-id="17792-122">Vytvoření zásady protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="17792-122">Create an IPsec/IKE policy</span></span>
3. <span data-ttu-id="17792-123">Hello zásadu můžete použít při vytváření připojení S2S nebo VNet-to-VNet</span><span class="sxs-lookup"><span data-stu-id="17792-123">You can apply hello policy when you create a S2S or VNet-to-VNet connection</span></span>
4. <span data-ttu-id="17792-124">Pokud hello připojení je již vytvořený, můžete použít nebo aktualizovat existující připojení tooan hello zásad</span><span class="sxs-lookup"><span data-stu-id="17792-124">If hello connection is already created, you can apply or update hello policy tooan existing connection</span></span>


## <a name="ipsecike-policy-faq"></a><span data-ttu-id="17792-125">Zásady protokolu IPsec/IKE – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="17792-125">IPsec/IKE policy FAQ</span></span>

[!INCLUDE [vpn-gateway-ipsecikepolicy-faq-include](../../includes/vpn-gateway-ipsecikepolicy-faq-include.md)]


## <a name="next-steps"></a><span data-ttu-id="17792-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="17792-126">Next steps</span></span>
<span data-ttu-id="17792-127">V tématu [zásady Konfigurace protokolu IPsec/IKE](vpn-gateway-ipsecikepolicy-rm-powershell.md) podrobné pokyny ke konfiguraci vlastních zásad protokolu IPsec/IKE na připojení.</span><span class="sxs-lookup"><span data-stu-id="17792-127">See [Configure IPsec/IKE policy](vpn-gateway-ipsecikepolicy-rm-powershell.md) for step-by-step instructions on configuring custom IPsec/IKE policy on a connection.</span></span>

<span data-ttu-id="17792-128">Viz také [připojení více zařízení sítě VPN založené na zásadách](vpn-gateway-connect-multiple-policybased-rm-ps.md) toolearn více informací o hello UsePolicyBasedTrafficSelectors možnost.</span><span class="sxs-lookup"><span data-stu-id="17792-128">See also [Connect multiple policy-based VPN devices](vpn-gateway-connect-multiple-policybased-rm-ps.md) toolearn more about hello UsePolicyBasedTrafficSelectors option.</span></span>
