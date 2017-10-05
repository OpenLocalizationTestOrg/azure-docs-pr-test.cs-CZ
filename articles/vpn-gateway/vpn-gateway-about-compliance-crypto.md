---
title: "O kryptografických požadavcích a Azure VPN Gateway | Microsoft Docs"
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
ms.openlocfilehash: c789e6c278fc0c58c64f5d96e57f94aee5a6cefc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="about-cryptographic-requirements-and-azure-vpn-gateways"></a><span data-ttu-id="1a4e7-103">O kryptografických požadavky a brány Azure VPN</span><span class="sxs-lookup"><span data-stu-id="1a4e7-103">About cryptographic requirements and Azure VPN gateways</span></span>

<span data-ttu-id="1a4e7-104">Tento článek popisuje, jak můžete nakonfigurovat Azure VPN Gateway pro uspokojení požadavků kryptografických tunelů S2S VPN mezi různými místy a připojení VNet-to-VNet v rámci Azure.</span><span class="sxs-lookup"><span data-stu-id="1a4e7-104">This article discusses how you can configure Azure VPN gateways to satisfy your cryptographic requirements for both cross-premises S2S VPN tunnels and VNet-to-VNet connections within Azure.</span></span> 

## <a name="about-ipsec-and-ike-policy-parameters-for-azure-vpn-gateways"></a><span data-ttu-id="1a4e7-105">O parametry zásad protokolu IPsec a IKE pro Azure VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="1a4e7-105">About IPsec and IKE policy parameters for Azure VPN gateways</span></span>
<span data-ttu-id="1a4e7-106">Protokol IPsec a IKE standardní podporuje širokou škálu kryptografických algoritmů v různých kombinacích.</span><span class="sxs-lookup"><span data-stu-id="1a4e7-106">IPsec and IKE protocol standard supports a wide range of cryptographic algorithms in various combinations.</span></span> <span data-ttu-id="1a4e7-107">Pokud zákazníci v požadavku konkrétní kombinaci kryptografické algoritmy a parametry, Azure VPN Gateway pomocí sady výchozí návrhů.</span><span class="sxs-lookup"><span data-stu-id="1a4e7-107">If customers do not request a specific combination of cryptographic algorithms and parameters, Azure VPN gateways use a set of default proposals.</span></span> <span data-ttu-id="1a4e7-108">Sady výchozí zásady byly vybrány a maximalizovat tak vzájemnou spolupráci s širokou škálu zařízení VPN třetích stran v výchozí konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1a4e7-108">The default policy sets were chosen to maximize interoperability with a wide range of third-party VPN devices in default configurations.</span></span> <span data-ttu-id="1a4e7-109">Zásady a počet návrhů v důsledku toho nemůže zahrnovat všechny možné kombinace dostupné kryptografické algoritmy a klíče síly.</span><span class="sxs-lookup"><span data-stu-id="1a4e7-109">As a result, the policies and the number of proposals cannot cover all possible combinations of available cryptographic algorithms and key strengths.</span></span>

<span data-ttu-id="1a4e7-110">Výchozí zásady nastavení pro bránu Azure VPN je uveden v dokumentu: [informace o zařízeních VPN a parametry protokolu IPsec/IKE pro připojení Site-to-Site VPN Gateway](vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="1a4e7-110">The default policy set for Azure VPN gateway is listed in the document: [About VPN devices and IPsec/IKE parameters for Site-to-Site VPN Gateway connections](vpn-gateway-about-vpn-devices.md).</span></span>

## <a name="cryptographic-requirements"></a><span data-ttu-id="1a4e7-111">Kryptografické požadavky</span><span class="sxs-lookup"><span data-stu-id="1a4e7-111">Cryptographic requirements</span></span>
<span data-ttu-id="1a4e7-112">Pro komunikaci, které vyžadují určité kryptografické algoritmy nebo parametry obvykle kvůli dodržování předpisů nebo požadavky na zabezpečení, zákazníci teď můžete nakonfigurovat jejich Azure VPN Gateway používat vlastní zásady protokolu IPsec/IKE službou konkrétní kryptografické algoritmy a klíče síly, nikoli nastaví Azure výchozí zásady.</span><span class="sxs-lookup"><span data-stu-id="1a4e7-112">For communications that require specific cryptographic algorithms or parameters, typically due to compliance or security requirements, customers can now configure their Azure VPN gateways to use a custom IPsec/IKE policy with specific cryptographic algorithms and key strengths, rather than the Azure default policy sets.</span></span>

<span data-ttu-id="1a4e7-113">Například zásady hlavního režimu IKEv2 pro Azure VPN Gateway využívat pouze Diffie-Hellman Skupina 2 (1 024 bitů), zatímco zákazníkům možná muset zadat silnější skupin, které se použijí v IKE, jako je například Skupina 14 (2048 bitů), 24 skupinu (skupiny MODP 2048 bitů) nebo ECP (elliptic curve skupiny) 256 nebo 384 bit (skupiny 19 a skupiny 20).</span><span class="sxs-lookup"><span data-stu-id="1a4e7-113">For example, the IKEv2 main mode policies for Azure VPN gateways utilize only Diffie-Hellman Group 2 (1024 bits), whereas customers may need to specify stronger groups to be used in IKE, such as Group 14 (2048-bit), Group 24 (2048-bit MODP Group), or ECP (elliptic curve groups) 256 or 384 bit (Group 19 and Group 20, respectively).</span></span> <span data-ttu-id="1a4e7-114">Podobné požadavky platí pro také zásady rychlého režimu IPsec.</span><span class="sxs-lookup"><span data-stu-id="1a4e7-114">Similar requirements apply to IPsec quick mode policies as well.</span></span>

## <a name="custom-ipsecike-policy-with-azure-vpn-gateways"></a><span data-ttu-id="1a4e7-115">Vlastní zásady protokolu IPsec/IKE pomocí bran Azure VPN</span><span class="sxs-lookup"><span data-stu-id="1a4e7-115">Custom IPsec/IKE policy with Azure VPN gateways</span></span>
<span data-ttu-id="1a4e7-116">Azure VPN Gateway teď podporují připojení, vlastní zásady protokolu IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="1a4e7-116">Azure VPN gateways now support per-connection, custom IPsec/IKE policy.</span></span> <span data-ttu-id="1a4e7-117">Site-to-Site nebo připojení VNet-to-VNet lze vybrat konkrétní kombinaci kryptografických algoritmů IPsec a IKE s požadovanou síly klíče, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="1a4e7-117">For a Site-to-Site or VNet-to-VNet connection, you can choose a specific combination of cryptographic algorithms for IPsec and IKE with the desired key strength, as shown in the following example:</span></span>

![zásady protokolu IPSec ike](./media/vpn-gateway-about-compliance-crypto/ipsecikepolicy.png)

<span data-ttu-id="1a4e7-119">Můžete vytvořit zásady protokolu IPsec/IKE a použít pro nové nebo existující připojení.</span><span class="sxs-lookup"><span data-stu-id="1a4e7-119">You can create an IPsec/IKE policy and apply to a new or existing connection.</span></span> 

### <a name="workflow"></a><span data-ttu-id="1a4e7-120">Pracovní postupy</span><span class="sxs-lookup"><span data-stu-id="1a4e7-120">Workflow</span></span>

1. <span data-ttu-id="1a4e7-121">Vytvoření virtuální sítě, brány sítě VPN nebo brány místní sítě pro topologie připojení, jak je popsáno v dalších dokumenty s postupy</span><span class="sxs-lookup"><span data-stu-id="1a4e7-121">Create the virtual networks, VPN gateways, or local network gateways for your connectivity topology as described in other how-to documents</span></span>
2. <span data-ttu-id="1a4e7-122">Vytvoření zásady protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="1a4e7-122">Create an IPsec/IKE policy</span></span>
3. <span data-ttu-id="1a4e7-123">Zásady můžete použít při vytváření připojení S2S nebo VNet-to-VNet</span><span class="sxs-lookup"><span data-stu-id="1a4e7-123">You can apply the policy when you create a S2S or VNet-to-VNet connection</span></span>
4. <span data-ttu-id="1a4e7-124">Pokud je již vytvořili připojení, můžete použít nebo aktualizaci zásad na existující připojení</span><span class="sxs-lookup"><span data-stu-id="1a4e7-124">If the connection is already created, you can apply or update the policy to an existing connection</span></span>


## <a name="ipsecike-policy-faq"></a><span data-ttu-id="1a4e7-125">Zásady protokolu IPsec/IKE – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="1a4e7-125">IPsec/IKE policy FAQ</span></span>

[!INCLUDE [vpn-gateway-ipsecikepolicy-faq-include](../../includes/vpn-gateway-ipsecikepolicy-faq-include.md)]


## <a name="next-steps"></a><span data-ttu-id="1a4e7-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a4e7-126">Next steps</span></span>
<span data-ttu-id="1a4e7-127">V tématu [zásady Konfigurace protokolu IPsec/IKE](vpn-gateway-ipsecikepolicy-rm-powershell.md) podrobné pokyny ke konfiguraci vlastních zásad protokolu IPsec/IKE na připojení.</span><span class="sxs-lookup"><span data-stu-id="1a4e7-127">See [Configure IPsec/IKE policy](vpn-gateway-ipsecikepolicy-rm-powershell.md) for step-by-step instructions on configuring custom IPsec/IKE policy on a connection.</span></span>

<span data-ttu-id="1a4e7-128">Viz také [připojení více zařízení sítě VPN založené na zásadách](vpn-gateway-connect-multiple-policybased-rm-ps.md) Další informace o možnost UsePolicyBasedTrafficSelectors.</span><span class="sxs-lookup"><span data-stu-id="1a4e7-128">See also [Connect multiple policy-based VPN devices](vpn-gateway-connect-multiple-policybased-rm-ps.md) to learn more about the UsePolicyBasedTrafficSelectors option.</span></span>