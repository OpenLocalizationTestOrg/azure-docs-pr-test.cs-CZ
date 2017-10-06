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
# <a name="troubleshooting-an-azure-site-to-site-vpn-connection-cannot-connect-and-stops-working"></a>Řešení potíží: Připojení k Azure site-to-site VPN se nemůže připojit a zastaví práce

Po dokončení konfigurace připojení site-to-site VPN mezi místní sítí a virtuální síť Azure, hello připojení k síti VPN najednou přestane fungovat a nelze znovu připojit. Tento článek obsahuje řešení problémů s kroky toohelp vyřešíte tento problém. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a>Řešení potíží

tooresolve hello problém, zkuste je napřed příliš[resetování brány Azure VPN hello](vpn-gateway-resetgw-classic.md) a resetovat hello tunel ze zařízení VPN místní hello. Pokud hello problém přetrvává, postupujte podle těchto kroků tooidentify hello příčinu problému hello.

### <a name="prerequisite-step"></a>Požadovaný krok

Zkontrolujte hello typ brány Azure VPN hello.

1. Přejděte toohello [portál Azure](https://portal.azure.com).

2. Zkontrolujte hello **přehled** stránku hello brány VPN pro informace o typu hello.
    
    ![Přehled brány hello](media\vpn-gateway-troubleshoot-site-to-site-cannot-connect\gatewayoverview.png)

### <a name="step-1-check-whether-hello-on-premises-vpn-device-is-validated"></a>Krok 1. Zkontrolujte, zda je ověřen zařízení VPN místní hello

1. Zkontrolujte, zda používáte [ověřit zařízení VPN a verzi operačního systému](vpn-gateway-about-vpn-devices.md#devicetable). Pokud zařízení hello není ověřená zařízení VPN, může být toocontact hello zařízení výrobce toosee, pokud nastane problém s kompatibilitou.

2. Ujistěte se, že toto zařízení VPN hello je správně nakonfigurován. Další informace najdete v tématu [upravit ukázky konfigurace zařízení](/vpn-gateway-about-vpn-devices.md#editing).

### <a name="step-2-verify-hello-shared-key"></a>Krok 2. Ověřte sdílený klíč hello

Porovnejte hello sdílený klíč pro hello místní VPN zařízení toohello virtuální sítě VPN Azure toomake jistotu, že hello klíče shodují. 

tooview hello sdílený klíč pro hello připojení Azure VPN, použijte jednu z následujících metod hello:

**Azure Portal**

1. Přejděte toohello připojení site-to-site brány v sítě VPN, který jste vytvořili.

2. V hello **nastavení** klikněte na tlačítko **sdílený klíč**.
    
    ![Sdílený klíč](media/vpn-gateway-troubleshoot-site-to-site-cannot-connect/sharedkey.png)

**Azure PowerShell**

Pro model nasazení Azure Resource Manager hello:

    Get-AzureRmVirtualNetworkGatewayConnectionSharedKey -Name <Connection name> -ResourceGroupName <Resource group name>

Pro model nasazení classic hello:

    Get-AzureVNetGatewayKey -VNetName -LocalNetworkSiteName

### <a name="step-3-verify-hello-vpn-peer-ips"></a>Krok 3. Ověřte hello VPN sdílené IP adresy

-   Hello IP definice v hello **bránu místní sítě** objektu v Azure by měl odpovídat hello místní zařízení IP.
-   Služba Azure gateway IP definice, které jsou nastavené v hello Hello místního zařízení by měl odpovídat hello služba Azure gateway IP.

### <a name="step-4-check-udr-and-nsgs-on-hello-gateway-subnet"></a>Krok 4. Zkontrolujte UDR a skupiny Nsg na podsítě brány hello

Zkontrolujte a odeberte uživatelem definované směrování (UDR) nebo skupiny zabezpečení sítě (Nsg) pro podsíť brány hello a poté otestujte hello výsledek. Pokud hello problém vyřešit, ověřte nastavení hello, použité UDR nebo NSG.

### <a name="step-5-check-hello-on-premises-vpn-device-external-interface-address"></a>Krok 5. Kontrola hello místní adresa externí rozhraní zařízení VPN

- Pokud hello internetového IP adresa zařízení VPN hello je součástí hello **místní sítě** definice v Azure, můžete zaznamenat ojediněle odpojení.
- Hello externí rozhraní zařízení musí být přímo na hello Internetu. Měla by existovat žádné překlad síťových adres nebo brány firewall mezi hello Internet a hello zařízení.
- tooconfigure brány firewall clustering toohave virtuální IP adresy, je nutné rozdělit hello clusteru a vystavit zařízení VPN hello přímo veřejné rozhraní tooa této brány hello můžete rozhraní s.

### <a name="step-6-verify-that-hello-subnets-match-exactly-azure-policy-based-gateways"></a>Krok 6. Ověřte, zda text hello podsítě odpovídají přesně (Azure na základě zásad brány)

-   Ověřte, zda text hello podsítě odpovídají přesně mezi hello virtuální síť Azure a místními definice pro hello virtuální síť Azure.
-   Ověřte, zda text hello podsítě odpovídají přesně mezi hello **bránu místní sítě** a místní definice pro hello do místní sítě.

### <a name="step-7-verify-hello-azure-gateway-health-probe"></a>Krok 7. Ověřte test stavu služba Azure gateway hello

1. Přejděte toohello [test stavu](https://&lt;YourVirtualNetworkGatewayIP&gt;:8081/healthprobe).

2. Proklikejte se prostřednictvím hello upozornění certifikátu.
3. Pokud se zobrazí odpověď, považuje za hello brána sítě VPN v pořádku. Pokud jste neobdrželi odpověď, nemusí být hello brány v pořádku nebo skupinu NSG na podsítě brány hello způsobuje problém hello. Následující text Hello je ukázková odpověď:

    &lt;? xml verze = "1.0"? > <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">primární Instance: GatewayTenantWorker_IN_1 GatewayTenantVersion: 14.7.24.6 < / řetězec&gt;

### <a name="step-8-check-whether-hello-on-premises-vpn-device-has-hello-perfect-forward-secrecy-feature-enabled"></a>Krok 8. Zkontrolujte, zda text hello místního zařízení VPN má povolenou funkci metoda perfect forward secrecy hello

Hello metoda perfect forward secrecy funkce může způsobit problémy odpojení. Pokud zařízení VPN hello má metoda perfect forward secrecy povolit, zakážete funkci hello. Aktualizujte zásady IPsec brány sítě VPN hello.

## <a name="next-steps"></a>Další kroky

-   [Konfigurace virtuální sítě tooa připojení site-to-site](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
-   [Konfigurace zásady protokolu IPsec/IKE pro připojení VPN typu site-to-site](vpn-gateway-ipsecikepolicy-rm-powershell.md)
