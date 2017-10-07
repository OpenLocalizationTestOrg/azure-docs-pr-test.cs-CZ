---
title: "aaaAccess Key Vault za bránou firewall | Microsoft Docs"
description: "Zjistěte, jak tooaccess Azure Key Vault z aplikace za bránou firewall"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 50d21774-2ee1-4212-8995-570c9de603c5
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: ambapat
ms.openlocfilehash: ab4bb0c27a41fef878a20dace6cab203df04e579
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="access-azure-key-vault-behind-a-firewall"></a>Přístup ke službě Azure Key Vault za bránou firewall
### <a name="q-my-key-vault-client-application-needs-toobe-behind-a-firewall-what-ports-hosts-or-ip-addresses-should-i-open-tooenable-access-tooa-key-vault"></a>Otázka: Moje aplikace klienta trezoru klíčů musí toobe za bránou firewall. Jaké porty, hostitele nebo IP adresy by měla otevřít tooenable přístup tooa trezoru klíčů?
tooaccess trezoru klíčů, klientská aplikace trezoru klíčů má tooaccess několik koncových bodů pro různé funkce:

* Ověřování prostřednictvím Azure Active Directory (Azure AD)
* Správa služby Azure Key Vault. Jedná se o vytváření, čtení, aktualizaci, odstraňování a nastavování zásad přístupu prostřednictvím Azure Resource Manageru.
* Přístup a Správa objektů (klíče a tajné klíče) uložených v Key Vault sám sebe, projít hello specifické pro klíč trezoru koncového bodu (například [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)).  

V závislosti na vaší konfiguraci a prostředí existuje několik variant.   

## <a name="ports"></a>Porty
Všechny přenosy tooa klíče trezoru pro všechny tři funkce (ověřování, Správa a datové roviny přístup) prochází přes HTTPS: port 443. Nicméně u seznamu CRL může občas docházet k provozu přes protokol HTTP (na portu 80). Klienti podporující protokol OCSP by se na seznam CRL dostat neměli, občas se ale mohou dostat na [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl).  

## <a name="authentication"></a>Authentication
Trezor klíčů klientské aplikace bude potřebovat tooaccess koncové body Azure Active Directory pro ověřování. koncový bod Hello používá závisí na hello konfigurace klienta služby Azure AD, hello typ objektu zabezpečení (hlavní název uživatele nebo instanční objekt) a hello typ účtu – například k účtu Microsoft nebo pracovní nebo školní účet.  

| Typ objektu zabezpečení | Koncový bod:port |
| --- | --- |
| Uživatel používající účet Microsoft<br> (například user@hotmail.com) |**Globální:**<br> login.microsoftonline.com:443<br><br> **Azure China:**<br> login.chinacloudapi.cn:443<br><br>**Azure US Government:**<br> login-us.microsoftonline.com:443<br><br>**Azure Germany:**<br> login.microsoftonline.de:443<br><br> a <br>login.live.com:443 |
| Uživatel nebo objektu zabezpečení pomocí pracovní nebo školní účet s Azure AD (například user@contoso.com) |**Globální:**<br> login.microsoftonline.com:443<br><br> **Azure China:**<br> login.chinacloudapi.cn:443<br><br>**Azure US Government:**<br> login-us.microsoftonline.com:443<br><br>**Azure Germany:**<br> login.microsoftonline.de:443 |
| Uživatel nebo objektu zabezpečení pomocí pracovní nebo školní účet a služby Active Directory Federation Services (AD FS) nebo jiný federované koncový bod (například user@contoso.com) |Všechny koncové body pro pracovní nebo školní účet a AD FS nebo jiné federované koncové body |

Existují i další možné komplexní scénáře. Odkazovat příliš[Azure Active Directory Authentication toku](/documentation/articles/active-directory-authentication-scenarios/), [integrace aplikací s Azure Active Directory](/documentation/articles/active-directory-integrating-applications/), a [protokoly pro ověřování Active Directory](https://msdn.microsoft.com/library/azure/dn151124.aspx) Další informace.  

## <a name="key-vault-management"></a>Správa služby Key Vault
Hello trezoru klíčů klientskou aplikaci pro správu Key Vault (CRUD a nastavení zásad přístupu), musí tooaccess koncový bod Azure Resource Manager.  

| Typ operace | Koncový bod:port |
| --- | --- |
| Operace roviny řízení služby Key Vault<br> prostřednictvím Azure Resource Manageru |**Globální:**<br> management.azure.com:443<br><br> **Azure China:**<br> management.chinacloudapi.cn:443<br><br> **Azure US Government:**<br> management.usgovcloudapi.net:443<br><br> **Azure Germany:**<br> management.microsoftazure.de:443 |
| Azure Active Directory Graph API |**Globální:**<br> graph.windows.net:443<br><br> **Azure China:**<br> graph.chinacloudapi.cn:443<br><br> **Azure US Government:**<br> graph.windows.net:443<br><br> **Azure Germany:**<br> graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Operace služby Key Vault
Pro všechny Správa trezoru klíčů objektu (klíče a tajné klíče) a kryptografické operace hello trezoru klíčů Klient potřebuje tooaccess hello trezoru klíčů koncový bod. přípona DNS Hello koncového bodu se liší v závislosti na umístění hello trezoru klíčů. koncový bod trezoru klíčů Hello je hello formátu *název trezoru*. *oblast –--příponu dns*, jak je popsáno v následující tabulce hello.  

| Typ operace | Koncový bod:port |
| --- | --- |
| Operace, včetně kryptografických operací na klíčích; vytváření, čtení, aktualizace nebo odstraňování klíčů a tajných kódů; nastavování nebo získávání značek a jiných atributů objektů trezoru klíčů (klíče a tajné kódy) |**Globální:**<br> &lt;název_trezoru&gt;.vault.azure.net:443<br><br> **Azure China:**<br> &lt;název_trezoru&gt;.vault.azure.cn:443<br><br> **Azure US Government:**<br> &lt;název_trezoru&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Germany:**<br> &lt;název_trezoru&gt;.vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>Rozsahy IP adres
Hello služby Key Vault používá jiných prostředků Azure stejně jako PaaS infrastruktury. Takže není možné tooprovide konkrétní rozsah IP adres, že Key Vault koncové body služby bude mít konkrétní kdykoli. Pokud brána firewall podporuje pouze rozsahy IP adres, přečtěte si téma toohello [rozsahy IP Datacentra Azure Microsoft](https://www.microsoft.com/download/details.aspx?id=41653) dokumentu. Pro ověřování a identita (Azure Active Directory), vaše aplikace musí být schopný tooconnect toohello koncové body popsané v [ověřování a identita adresy](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="next-steps"></a>Další kroky
Pokud máte dotazy týkající se Key Vault, navštivte hello [Azure Key Vault fóra](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).

