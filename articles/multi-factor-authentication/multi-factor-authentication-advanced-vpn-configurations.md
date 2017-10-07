---
title: "scénáře aaaAdvanced s vícefaktorovým Ověřováním Azure a sítě VPN třetí strany"
description: "Krok za krokem konfigurace příručky pro Azure MFA toointegrate s Cisco, Citrix a Juniper."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 1f94a214-d6f6-48a8-8a12-006b5896ae45
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/13/2017
ms.author: kgremban
ms.openlocfilehash: e23960ca4977cc01271f99fa2bec70449e9acfff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-scenarios-with-azure-multi-factor-authentication-and-third-party-vpn-solutions"></a>Složitější scénáře s Azure Multi-Factor Authentication a řešení VPN jiných výrobců
Můžete použít Azure Multi-Factor Authentication tooseamlessly připojit pomocí různých řešení VPN jiných výrobců. Tento článek se týká zařízení Cisco® ASA VPN, Citrix NetScaler SSL VPN zařízení a hello Juniper sítě zabezpečit přístup/Pulse Secure připojit zabezpečení SSL zařízení VPN. Jsme vytvořili tooaddress příručky konfigurace těchto tří běžné zařízení, ale aplikace Multi-Factor Authentication Server může integrovat většina systémů, které pomocí protokolu RADIUS, protokolu LDAP, IIS nebo ověřování založené na deklaracích tooAD služby FS. Můžete najít další podrobnosti v [konfigurací MFA Server](multi-factor-authentication-get-started-server.md#next-steps).

## <a name="cisco-asa-vpn-appliance-and-azure-multi-factor-authentication"></a>Zařízení Cisco ASA VPN a ověřování Azure Multi-Factor Authentication
Azure Multi-Factor Authentication se integruje s Cisco® ASA VPN zařízení tooprovide další zabezpečení pro přihlášení Cisco AnyConnect® VPN a přístup k portálu.  To lze provést pomocí buď hello protokol LDAP nebo RADIUS.  Vyberte jednu z hello následující toodownload hello podrobné krok za krokem konfigurace provede.

| Průvodce konfigurací | Popis |
| --- | --- |
| [Cisco ASA s Anyconnect VPN a Azure MFA konfigurací protokolu LDAP](http://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | Vaše zařízení Cisco ASA VPN integrovat Azure MFA pomocí protokolu LDAP |
| [Cisco ASA s Anyconnect VPN a Azure MFA konfigurací protokolu RADIUS](http://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Vaše zařízení Cisco ASA VPN integrovat Azure MFA pomocí protokolu RADIUS |

## <a name="citrix-netscaler-ssl-vpn-and-azure-multi-factor-authentication"></a>Citrix NetScaler SSL VPN a Azure Multi-Factor Authentication
Azure Multi-Factor Authentication se integruje s Citrix NetScaler SSL VPN zařízení tooprovide další zabezpečení pro přihlášení Citrix NetScaler SSL VPN a přístup k portálu.  To lze provést pomocí buď hello protokol LDAP nebo RADIUS.  Vyberte jednu z hello následující toodownload hello podrobné krok za krokem konfigurace provede.

| Průvodce konfigurací | Popis |
| --- | --- |
| [Konfigurace SSL VPN nejen Citrix NetScaler a Azure MFA pro LDAP](http://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | Integrovat váš Citrix NetScaler SSL VPN Azure MFA zařízení pomocí protokolu LDAP |
| [Konfigurace Citrix NetScaler SSL VPN a Azure MFA pro server RADIUS](http://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Vaše zařízení Citrix NetScaler SSL VPN integrovat Azure MFA pomocí protokolu RADIUS |

## <a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-multi-factor-authentication"></a>Juniper/Pulse Secure SSL VPN zařízení a ověřování Azure Multi-Factor Authentication
Azure Multi-Factor Authentication se integruje s Juniper/Pulse Secure SSL VPN zařízení tooprovide další zabezpečení pro přihlášení Juniper/Pulse Secure SSL VPN a přístup k portálu.  To lze provést pomocí buď hello protokol LDAP nebo RADIUS.  Vyberte jednu z hello následující toodownload hello podrobné krok za krokem konfigurace provede.

| Průvodce konfigurací | Popis |
| --- | --- |
| [Konfigurace Juniper/Pulse zabezpečení SSL VPN a Azure MFA pro LDAP](http://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx) | Integrovat váš Juniper/Pulse Secure SSL VPN Azure MFA zařízení pomocí protokolu LDAP |
| [Konfigurace zabezpečení SSL VPN nejen Juniper/Pulse a Azure MFA protokolu RADIUS](http://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Vaše zařízení Juniper/Pulse Secure SSL VPN integrovat Azure MFA pomocí protokolu RADIUS |

## <a name="next-steps"></a>Další kroky

- [Posílení vaší stávající infrastruktury pro ověřování s hello NPS rozšíření pro Azure Multi-Factor Authentication](multi-factor-authentication-nps-extension.md)

- [Konfigurace nastavení služby Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md)