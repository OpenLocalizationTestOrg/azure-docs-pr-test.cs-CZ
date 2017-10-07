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
# <a name="about-cryptographic-requirements-and-azure-vpn-gateways"></a>O kryptografických požadavky a brány Azure VPN

Tento článek popisuje, jak konfigurovat Azure VPN Gateway toosatisfy kryptografických požadavky na tunelů S2S VPN mezi různými místy a připojení VNet-to-VNet v rámci Azure. 

## <a name="about-ipsec-and-ike-policy-parameters-for-azure-vpn-gateways"></a>O parametry zásad protokolu IPsec a IKE pro Azure VPN Gateway
Protokol IPsec a IKE standardní podporuje širokou škálu kryptografických algoritmů v různých kombinacích. Pokud zákazníci v požadavku konkrétní kombinaci kryptografické algoritmy a parametry, Azure VPN Gateway pomocí sady výchozí návrhů. Hello výchozí zásady sady členové toomaximize Interoperabilita s širokou škálu zařízení VPN třetích stran v výchozí konfigurace. V důsledku toho nelze hello zásady a hello počet návrhy zahrnují všechny možné kombinace dostupné kryptografické algoritmy a klíče síly.

Hello výchozí zásady nastavení pro bránu Azure VPN je uvedené v dokumentu hello: [informace o zařízeních VPN a parametry protokolu IPsec/IKE pro připojení Site-to-Site VPN Gateway](vpn-gateway-about-vpn-devices.md).

## <a name="cryptographic-requirements"></a>Kryptografické požadavky
Pro komunikaci, které vyžadují určité kryptografické algoritmy nebo parametry obvykle kvůli toocompliance nebo zabezpečení požadavky zákazníků teď můžete nakonfigurovat jejich toouse brány Azure VPN vlastní zásady protokolu IPsec/IKE s konkrétní kryptografických algoritmy a klíče síly, nikoli hello Azure výchozí zásady skupiny.

Například hello IKEv2 hlavního režimu zásady pro Azure VPN Gateway využívat pouze Diffie-Hellman Skupina 2 (1 024 bitů), zatímco zákazníkům může být nutné toospecify silnější použít v IKE, jako jsou skupiny 14 (2048 bitů), 24 skupinu (skupiny MODP 2048 bitů) nebo ECP (elliptic toobe skupiny křivkou skupin) 256 nebo 384 bit (skupiny 19 a skupiny 20). Podobné požadavky zásad tooIPsec rychlého režimu také.

## <a name="custom-ipsecike-policy-with-azure-vpn-gateways"></a>Vlastní zásady protokolu IPsec/IKE pomocí bran Azure VPN
Azure VPN Gateway teď podporují připojení, vlastní zásady protokolu IPsec/IKE. Site-to-Site nebo připojení VNet-to-VNet lze vybrat konkrétní kombinaci kryptografických algoritmů IPsec a IKE s hello potřeby síly klíče, jak je znázorněno v hello následující ukázka:

![zásady protokolu IPSec ike](./media/vpn-gateway-about-compliance-crypto/ipsecikepolicy.png)

Můžete vytvořit zásady protokolu IPsec/IKE a použít tooa nové nebo existující připojení. 

### <a name="workflow"></a>Pracovní postup

1. Jak je popsáno v dalších toodocuments jak vytvořit hello virtuální sítě, brány sítě VPN nebo brány místní sítě pro topologie připojení
2. Vytvoření zásady protokolu IPsec/IKE
3. Hello zásadu můžete použít při vytváření připojení S2S nebo VNet-to-VNet
4. Pokud hello připojení je již vytvořený, můžete použít nebo aktualizovat existující připojení tooan hello zásad


## <a name="ipsecike-policy-faq"></a>Zásady protokolu IPsec/IKE – nejčastější dotazy

[!INCLUDE [vpn-gateway-ipsecikepolicy-faq-include](../../includes/vpn-gateway-ipsecikepolicy-faq-include.md)]


## <a name="next-steps"></a>Další kroky
V tématu [zásady Konfigurace protokolu IPsec/IKE](vpn-gateway-ipsecikepolicy-rm-powershell.md) podrobné pokyny ke konfiguraci vlastních zásad protokolu IPsec/IKE na připojení.

Viz také [připojení více zařízení sítě VPN založené na zásadách](vpn-gateway-connect-multiple-policybased-rm-ps.md) toolearn více informací o hello UsePolicyBasedTrafficSelectors možnost.
