---
title: "aaaConfigure hello rozšíření Azure MFA serveru NPS | Microsoft Docs"
description: "Po instalaci rozšíření NPS hello tyto kroky použijte pro pokročilou konfiguraci, jako je vytvoření seznamu povolených IP a nahrazení UPN."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: c3aed077b23c95f874861eb00c8e6dca668329c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-configuration-options-for-hello-nps-extension-for-multi-factor-authentication"></a>Upřesnit možnosti konfigurace pro hello NPS rozšíření pro službu Multi-Factor Authentication

Hello rozšíření serveru NPS (Network Policy Server) rozšiřuje vaše cloudové funkce Azure Multi-Factor Authentication do místní infrastruktury. Tento článek předpokládá už máte hello rozšíření nainstalovat a teď chcete tooknow jak musí toocustomize hello rozšíření pro vás. 

## <a name="alternate-login-id"></a>Alternativním přihlašovacím ID

Vzhledem k tomu, že hello NPS rozšíření připojí tooboth místní a cloudové adresáře, můžete setkat s problémem, kdy hlavní názvy vaše místní uživatele (UPN) se neshodují hello názvy v cloudu hello. toosolve tento problém, pomocí alternativního přihlašovacího ID. 

V rámci hello rozšíření serveru NPS můžete určit toobe atribut Active Directory použít místo hello UPN pro Azure Multi-Factor Authentication. Díky tomu můžete tooprotect vaše místní prostředky s dvoustupňové ověření beze změny vaší místní UPN. 

tooconfigure alternativního přihlašovacího ID, přejděte příliš`HKLM\SOFTWARE\Microsoft\AzureMfa` a upravit hello následující hodnoty registru:

| Name (Název) | Typ | Výchozí hodnota | Popis |
| ---- | ---- | ------------- | ----------- |
| LDAP_ALTERNATE_LOGINID_ATTRIBUTE | Řetězec | prázdný | Určete název hello atributu služby Active Directory, které chcete toouse místo hello UPN. Tento atribut slouží jako atribut AlternateLoginId hello. Pokud je tato hodnota registru nastavená tooa [platný atribut služby Active Directory](https://msdn.microsoft.com/library/ms675090.aspx) (například e-mailu nebo displayName), pak hodnota atributu hello je použít místo hello uživatele (UPN) pro ověřování. Pokud tato hodnota registru není prázdný, nebo není nakonfigurováno, pak je zakázána AlternateLoginId a hello uživatele (UPN) se používá k ověřování. |
| LDAP_FORCE_GLOBAL_CATALOG | Logická hodnota | False | Při vyhledávání AlternateLoginId, použijte tento příznak tooforce hello použití globálního katalogu pro hledání LDAP. Konfigurace řadiče domény jako globální katalog, přidejte hello AlternateLoginId atribut toohello globálního katalogu a pak povolte tento příznak. <br><br> Pokud je nakonfigurovaný (není prázdná), LDAP_LOOKUP_FORESTS **tento příznak se vynucuje jako true**, bez ohledu na to hello hodnota nastavení registru hello. V takovém případě hello NPS rozšíření vyžaduje toobe globálního katalogu hello nakonfigurované hello AlternateLoginId atribut pro jednotlivé doménové struktury. |
| LDAP_LOOKUP_FORESTS | Řetězec | prázdný | Zadejte středníky oddělený seznam toosearch doménové struktury. Například *contoso.com;foobar.com*. Pokud je tato hodnota registru nakonfigurovaný, vyhledá hello NPS rozšíření interaktivně všech doménových struktur hello v hello pořadí, ve kterém byly uvedeny a vrátí hodnotu AlternateLoginId první úspěšné hello. Pokud tato hodnota registru není nakonfigurováno, je vyhledávání AlternateLoginId hello uzavřeného toohello aktuální domény.|

tootroubleshoot problémy s alternativním přihlašovacím ID, použijte hello doporučené kroky pro [alternativní přihlašovací ID chyby](multi-factor-authentication-nps-errors.md#alternate-login-id-errors).

## <a name="ip-exceptions"></a>Výjimky IP

Pokud potřebujete toomonitor dostupnost serveru, například pokud ověřte nástroje pro vyrovnávání zatížení serverů, které jsou spuštěny před odesláním úloh, nechcete, aby tyto kontroly toobe blokována žádosti o ověření. Místo toho vytvořte seznam IP adres, které znáte používají účty služeb a zakázání služby Multi-Factor Authentication požadavky pro tento seznam. 

přejděte seznam povolených IP adres tooconfigure příliš`HKLM\SOFTWARE\Microsoft\AzureMfa` a nakonfigurujte hello následující hodnotu registru: 

| Name (Název) | Typ | Výchozí hodnota | Popis |
| ---- | ---- | ------------- | ----------- |
| IP_WHITELIST | Řetězec | prázdný | Zadejte středníky oddělený seznam IP adres. Zahrnout hello IP adresy počítačů, kde žádosti o služby mají původ, jako hello server NAS a VPN. Rozsahy IP jsou podsítě nejsou podporovány. <br><br> Například *10.0.0.1;10.0.0.2;10.0.0.3*.

Když přijde žádost z IP adresy, která existuje v seznamu povolených IP adres hello, bylo přeskočeno dvoustupňové ověřování. Hello seznam povolených IP adres se porovná s toohello IP adresu, která je součástí hello *ratNASIPAddress* atribut hello požadavku protokolu RADIUS. Pokud požadavku protokolu RADIUS dodává bez hello ratNASIPAddress atribut, zaznamená se hello následující upozornění: "P_WHITE_LIST_WARNING::IP povolených je ignorován jako zdrojové IP adresy chybí v požadavku protokolu RADIUS v atributu NasIpAddress."

## <a name="next-steps"></a>Další kroky

[Vyřešte chybové zprávy z hello NPS rozšíření pro Azure Multi-Factor Authentication](multi-factor-authentication-nps-errors.md)
