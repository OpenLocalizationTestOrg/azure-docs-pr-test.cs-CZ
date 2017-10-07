---
title: "pro zajištění vysoké dostupnosti aaaConfigure Azure MFA serveru | Microsoft Docs"
description: "Nasazení více instancí serveru Azure Multi-Factor Authentication Server v konfiguracích, které zajišťují vysokou dostupnost."
services: multi-factor-authentication
keywords: Azure MFA
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: joflore
ms.reviewer: alexwe
ms.custom: it-pro
ms.openlocfilehash: 8c6b1921c734c7a7273e443b3591fbbb15cd5e6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-multi-factor-authentication-server-for-high-availability"></a>Nakonfigurujte Server Azure Multi-Factor Authentication pro zajištění vysoké dostupnosti

tooachieve vysoká dostupnost s nasazením Azure MFA serveru, je nutné toodeploy více serverů vícefaktorového ověřování. Tato část obsahuje informace o vyrovnávání zatížení sítě návrhu tooachieve vaše cíle vysoké dostupnosti ve službě Azure MFS serveru nasazení.

## <a name="mfa-server-overview"></a>Přehled MFA serveru

Hello architektura služby Azure MFA serveru zahrnuje několik komponent, jak je znázorněno v následujícím diagramu hello:

 ![Architektura serveru MFA](./media/mfa-server-high-availability/mfa-ha-architecture.png)

MFA Server je Windows Server, který má nainstalovaný software hello Azure Multi-Factor Authentication. instance serveru MFA Hello musí aktivovat hello služby MFA v Azure toofunction. Více než jeden Server MFA může být nainstalovaná na místě.

Hello je první MFA serveru, který je nainstalován hello hlavní Server MFA po aktivaci pomocí hello služby Azure MFA ve výchozím nastavení. hlavní server MFA Hello má kopii databáze PhoneFactor.pfdata hello nelze zapisovat. Následné instalace instancí MFA serveru se označují jako slaves. Hello MFA slaves mít replikované jen pro čtení kopie databáze PhoneFactor.pfdata hello. MFA servery replikují informace o používání vzdáleného volání procedur (RPC). Všechny servery vícefaktorového ověřování musí souhrnně být buď připojený k doméně nebo samostatné tooreplicate informace.

Hlavní server MFA a podřízené servery vícefaktorového ověřování komunikovat s hello služby MFA Pokud se vyžaduje dvoufaktorové ověření. Například když se uživatel pokusí toogain přístup tooan aplikace, která vyžaduje dvoufaktorové ověřování, hello uživatele bude nejprve ověřit pomocí zprostředkovatele identity, jako je Active Directory (AD).

Po úspěšném ověření službou AD hello MFA Server komunikovat s hello služby MFA. Hello MFA Server čeká oznámení ze služby MFA tooallow hello nebo odepřít aplikace toohello přístup uživatelů hello.

Pokud hlavní server MFA hello přejde do režimu offline, ověřování stále lze zpracovat, ale operace, které vyžadují změny toohello MFA databázi nelze zpracovat. (Příklady: Přidání uživatelů samoobslužné služby změny kódu PIN a změna informace o uživateli hello)

## <a name="deployment"></a>Nasazení

Vezměte v úvahu hello následující důležité body pro vyrovnávání zatížení Azure MFA serveru a jeho souvisejících součástí.

* **Pomocí protokolu RADIUS standardní tooachieve vysokou dostupnost**. Pokud používáte Azure MFA servery jako servery RADIUS, můžete nakonfigurovat jeden Server MFA potenciálně jako primární cíl ověřování protokolu RADIUS a dalších serverů Azure MFA jako cíle sekundární ověřování. Však tato metoda tooachieve vysokou dostupnost nemusí být praktické, protože období toooccur časový limit musíte počkat, než můžete ověřovány proti sekundárního ověřování hello selhání ověření na cílové primární ověřování hello cíl. Je efektivnější tooload vyrovnávání hello RADIUS přenosů mezi klientem RADIUS hello a servery RADIUS hello (v tomto případě hello Azure MFA servery fungují jako servery RADIUS) tak, aby klienti RADIUS hello lze nakonfigurovat jednu adresu URL, která může ukazovat na.
* **Musí podporovat toomanually MFA slaves**. Pokud hlavní server Azure MFA hello přejde do režimu offline, hello sekundárních serverů Azure MFA dál tooprocess požadavky vícefaktorového ověřování. Ale dokud hlavní MFA server k dispozici, admins nelze přidat uživatele nebo změnit nastavení vícefaktorového ověřování, a uživatelé nelze provádět změny pomocí hello uživatelského portálu. Povýšení toohello podřízený vícefaktorového ověřování pro hlavní role je vždy ruční proces.
* **Separability součástí**. hello Hello Azure MFA serveru zahrnuje několik komponent, které je možné nainstalovat na stejnou instanci serveru Windows nebo na jinou instancí. Tyto součásti zahrnují hello portálu User Portal, webové služby mobilní aplikace a adaptér AD FS hello (agent). Tento separability je možné toouse hello Proxy webových aplikací toopublish hello portálu User Portal a mobilní aplikace Webový Server z hello hraniční síti. Taková konfigurace přidá toohello celkového zabezpečení návrhu, jak je znázorněno v následujícím diagramu hello. Hello MFA User Portal a mobilní aplikace Webový Server může také možné nasadit v konfiguraci Vyrovnávání zatížení sítě HA.

   ![MFA serveru s hraniční sítí](./media/mfa-server-high-availability/mfasecurity.png)

* **Jednorázovým heslem (OTP) prostřednictvím serveru SMS (neboli jednosměrné služby SMS) vyžaduje hello použití trvalé relace, pokud provoz s vyrovnáváním zatížení**. Jednosměrné služby SMS je možnost ověřování, která způsobí, že uživatelé hello toosend MFA Server hello textovou zprávu jednorázovým HESLEM. Hello uživatel zadá hello jednorázového HESLA vyzvat okno toocomplete hello ověřovací test MFA. Při načítání vyrovnávání serverů Azure MFA, hello stejný server, který obsluhuje hello počátečního požadavku na ověření musí být server hello, který přijme zprávu jednorázového HESLA hello od uživatele hello; Pokud jiný Server MFA obdrží hello odpověď ověřování jednorázovým HESLEM, hello výzvu ověřování se nezdaří. Další informace najdete v tématu [jedno heslo čas přes SMS přidat tooAzure MFA Server](https://blogs.technet.microsoft.com/enterprisemobility/2015/03/02/one-time-password-over-sms-added-to-azure-mfa-server).
* **Vyrovnávání zatížení sítě nasazení hello portálu User Portal a webová služba mobilní aplikace vyžadují trvalé relace**. Pokud jste Vyrovnávání zatížení hello MFA User Portal a hello webové služby mobilní aplikace, musí každé relaci toostay na hello stejný server.

## <a name="high-availability-deployment"></a>Nasazení vysoké dostupnosti

Hello následující diagram znázorňuje na dokončení HA Vyrovnávání zatížení sítě implementace Azure MFA a jeho komponenty, spolu s AD FS pro referenci.

 ![Azure MFA Server HA implementace](./media/mfa-server-high-availability/mfa-ha-deployment.png)

Mějte na paměti následující položky pro oblast hello odpovídajícím způsobem číslované hello předcházející diagram hello.

1. Dobrý den, které jsou dva servery vícefaktorového ověřování Azure (MFA1 a MFA2) načíst vyrovnáváním (mfaapp.contoso.com) a jsou nakonfigurované toouse statický port (4443) tooreplicate hello PhoneFactor.pfdata databáze. Hello sady SDK webové služby je nainstalován na každém hello MFA Server tooenable komunikace přes port TCP 443 se servery služby AD FS hello. Hello MFA servery jsou nasazeny v konfiguraci bezstavové Vyrovnávání zatížení sítě. Ale pokud budete chtít toouse jednorázového HESLA prostřednictvím serveru SMS, musíte použít vyrovnávání stavová zatížení.
   ![Azure MFA serveru - aplikační server HA](./media/mfa-server-high-availability/mfaapp.png)

   > [!NOTE]
   > Protože RPC používá dynamické porty, není doporučeno brány firewall tooopen až toohello rozsah dynamické porty, které mohou potenciálně používat RPC. Pokud máte bránu firewall **mezi** MFA aplikační servery, byste měli nakonfigurovat toocommunicate MFA Server hello na statický port pro přenosy replikace hello mezi hlavní data a podřízené servery a otevřete, který port v bráně firewall. Statický port hello můžete vynutit tím, že vytvoříte hodnotu DWORD registru na ```HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Positive Networks\PhoneFactor``` názvem ```Pfsvc_ncan_ip_tcp_port``` a nastavení hodnoty hello tooan k dispozici statický port. Připojení se vždy spouští hello podřízené servery vícefaktorového ověřování toohello hlavním, hello statický port je potřeba jenom na hlavní server hello, ale vzhledem k tomu, že můžete zvýšit úroveň hlavní podřízený toobe hello kdykoli, měli byste nastavit hello statický port ve všech serverech vícefaktorového ověřování.

2. servery uživatele portálu nebo vícefaktorového ověřování mobilní aplikace Hello dva (vícefaktorového ověřování. až MAS1 a MFA. až MAS2) jsou v skupinu s vyrovnáváním zatížení **stateful** konfigurace (mfa.contoso.com). Odvolat, aby trvalé relace jsou nutné pro vyrovnávání zatížení hello MFA User Portal a služby mobilní aplikace.
   ![Azure MFA serveru – portál User Portal a mobilní aplikace služby HA](./media/mfa-server-high-availability/mfaportal.png)
3. farma serverů služby AD FS Hello je skupinu s vyrovnáváním zatížení a publikovat toohello Internet prostřednictvím Vyrovnávání zatížení sítě proxy služby AD FS v hraniční síti hello. Každý Server služby AD FS používá toocommunicate agent služby AD FS hello s hello servery vícefaktorového ověřování Azure pomocí jednu Vyrovnávání zatížení sítě adresu URL (mfaapp.contoso.com) přes port 443 protokolu TCP.

## <a name="next-steps"></a>Další kroky

* [Instalace a konfigurace Azure MFA serveru](multi-factor-authentication-get-started-server.md)
