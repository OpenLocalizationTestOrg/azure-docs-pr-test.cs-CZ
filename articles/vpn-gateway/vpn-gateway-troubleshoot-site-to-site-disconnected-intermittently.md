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
# <a name="troubleshooting-azure-site-to-site-vpn-disconnects-intermittently"></a>Řešení potíží: Azure Site-to-Site VPN odpojí občas

Může dojít k hello problém, že nové nebo existující připojení VPN v Microsoft Azure Point-to-Site není stabilní nebo odpojí pravidelně. Tento článek obsahuje kroky toohelp identifikovat a řešit hello příčinu hello problém vyřešit. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a>Řešení potíží

### <a name="prerequisite-step"></a>Požadovaný krok

Zkontrolujte hello typ brány virtuální sítě Azure:

1. Přejděte příliš[portál Azure](https://portal.azure.com).
2. Zkontrolujte hello **přehled** stránku hello brány virtuální sítě pro informace o typu hello.
    
    ![Přehled Hello hello brány](media\vpn-gateway-troubleshoot-site-to-site-disconnected-intermittently\gatewayoverview.png)

### <a name="step-1-check-whether-hello-on-premises-vpn-device-is-validated"></a>Krok 1 Zkontrolujte, zda text hello místního zařízení VPN se ověří

1. Zkontrolujte, zda používáte [ověřit zařízení VPN a verzi operačního systému](vpn-gateway-about-vpn-devices.md#devicetable). Pokud není ověřená zařízení VPN hello, pokud neexistuje žádné potíže s kompatibilitou mohou mít toocontact hello zařízení výrobce toosee.
2. Ujistěte se, že toto zařízení VPN hello je správně nakonfigurován. Další informace najdete v tématu [ukázky úpravy konfigurace zařízení](vpn-gateway-about-vpn-devices.md#editing).

### <a name="step-2-check-hello-security-association-settingsfor-policy-based-azure-virtual-network-gateways"></a>Krok 2 nastavení kontroly hello přidružení zabezpečení (pro brány na základě zásad Azure virtuální sítě)

1. Ujistěte se, že hello virtuální sítě, podsítě a rozsahy v hello **brány místní sítě** definice v Microsoft Azure jsou stejné jako hello konfigurace v zařízení VPN místní hello.
2. Ověřte, které odpovídají nastavení hello přidružení zabezpečení.

### <a name="step-3-check-for-user-defined-routes-or-network-security-groups-on-gateway-subnet"></a>Krok 3 kontroly pro trasy definované uživatelem nebo skupiny zabezpečení sítě na podsíť brány

Uživatelem definované trasy na podsíť brány hello mohou některé provozy, omezení a umožňuje ostatní provoz. Díky tomu se zobrazí, zda je připojení k síti VPN hello nespolehlivé pro některé provozy a vhodný pro ostatní. 

### <a name="step-4-check-hello-one-vpn-tunnel-per-subnet-pair-setting-for-policy-based-virtual-network-gateways"></a>Krok 4 kontrola hello "jeden tunelového připojení sítě VPN za pár podsítě" nastavení (pro brány virtuální sítě na základě zásad)

Ujistěte se, že hello místní zařízení VPN je nastaven toohave **jeden tunelového připojení sítě VPN za pár podsítě** pro brány virtuální sítě na základě zásad.

### <a name="step-5-check-for-security-association-limitation-for-policy-based-virtual-network-gateways"></a>Krok 5 Kontrola omezení přidružení zabezpečení (pro brány virtuální sítě na základě zásad)

brány založené na zásadách virtuální sítě Hello má limit 200 párů podsítě přidružení zabezpečení. Pokud hello počet podsítí virtuální sítě Azure násobí časy podle hello počet místní podsítě je větší než 200, najdete v části ojediněle podsítě odpojení.

### <a name="step-6-check-on-premises-vpn-device-external-interface-address"></a>Krok 6 zkontrolujte místní adresa externí rozhraní zařízení VPN

- Pokud hello internetové IP adresa zařízení VPN hello je součástí hello **brány místní sítě** definice v Azure, se může vyskytnout občasné odpojení.
- Hello externí rozhraní zařízení musí být přímo na hello Internetu. Měla by existovat žádné překlad adres (NAT) nebo brány firewall mezi hello Internet a hello zařízení.
-  Pokud nakonfigurujete Clustering brány Firewall toohave virtuální IP adresy, musí přerušení hello clusteru a vystavit zařízení VPN hello přímo tooa veřejné rozhraní, která hello brány můžete rozhraní s.

### <a name="step-7-check-whether-hello-on-premises-vpn-device-has-perfect-forward-secrecy-enabled"></a>Krok 7 zkontrolujte, zda hello místního zařízení VPN má metoda Perfect Forward Secrecy povoleno

Hello **metoda Perfect Forward Secrecy** funkce může způsobit problémy odpojení hello. Pokud má zařízení VPN hello **ideální přeposílání** hello funkci povolit, zakázat. Potom [aktualizovat zásady IPsec brány virtuální sítě hello](vpn-gateway-ipsecikepolicy-rm-powershell.md#managepolicy).

## <a name="next-steps"></a>Další kroky

- [Konfigurace virtuální sítě tooa připojení Site-to-Site](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Konfigurace zásad protokolu IPsec/IKE pro připojení VPN typu Site-to-Site](vpn-gateway-ipsecikepolicy-rm-powershell.md)

