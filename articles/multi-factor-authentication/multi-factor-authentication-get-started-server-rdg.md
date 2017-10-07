---
title: "aaaRDG a Azure MFA serveru pomocí protokolu RADIUS | Microsoft Docs"
description: "Toto je stránka Azure Multi-Factor authentication hello, která vám pomůže při nasazení brány vzdálené plochy (RD) a pomocí protokolu RADIUS serveru Azure Multi-Factor Authentication Server."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f2354ac4-a3a7-48e5-a86d-84a9e5682b42
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/27/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: fd280e9b6ff90c82927cffe593c4f1fda7047842
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="remote-desktop-gateway-and-azure-multi-factor-authentication-server-using-radius"></a>Brána vzdálené plochy Azure Multi-Factor Authentication Server pomocí protokolu RADIUS
Brána vzdálené plochy (RD) často používá hello místní služby NPS (Network Policy Server) tooauthenticate uživatele. Tento článek popisuje, jak tooroute RADIUS požadavků na hello Brána vzdálené plochy (prostřednictvím místního serveru NPS hello) toohello aplikace Multi-Factor Authentication Server. Hello kombinaci Azure MFA a Brána VP znamená, že uživatelé může přístup svých prostředích pracovní odkudkoli při provádění silné ověřování. 

Vzhledem k tomu, že ověřování systému Windows pro Terminálové služby není podporována pro Server 2012 R2, s MFA Server použijte toointegrate brány VP a protokolu RADIUS. 

Nainstalujte hello Azure Multi-Factor Authentication Server na samostatném serveru, které proxy požadavku hello protokolu RADIUS zpět toohello NPS na hello serveru brány vzdálené plochy. Když NPS ověří hello uživatelské jméno a heslo, vrátí odpověď toohello aplikace Multi-Factor Authentication Server. Hello MFA Server pak provede hello druhý faktor ověřování a vrátí výsledek toohello brány.

## <a name="prerequisites"></a>Požadavky

- Azure MFA Server připojený k doméně. Pokud nemáte již nainstalována, postupujte podle kroků hello v [Začínáme s Azure Multi-Factor Authentication Server hello](multi-factor-authentication-get-started-server.md).
- Brána vzdálené plochy, která provádí ověřování pomocí serveru NPS.

## <a name="configure-hello-remote-desktop-gateway"></a>Konfigurace hello Brána vzdálené plochy
Nakonfigurujte hello Brána VP toosend RADIUS ověřování tooan Azure Multi-Factor Authentication Server. 

1. Klikněte pravým tlačítkem na název serveru hello v Správce brány VP a vyberte **vlastnosti**.
2. Přejděte toohello **úložiště CAP k vzdálené ploše** a vyberte **centrálního serveru NPS**. 
3. Přidejte jeden nebo více Azure Multi-Factor Authentication Server jako servery RADIUS zadáním hello název nebo IP adresu každého serveru. 
4. Vytvořte pro každý server sdílený tajný klíč.

## <a name="configure-nps"></a>Konfigurace NPS
Hello Brána VP používá server NPS toosend hello RADIUS požadavek tooAzure služby Multi-Factor Authentication. tooconfigure NPS, nejprve změníte nastavení časového limitu hello tooprevent hello vypršení časového limitu služby Brána VP před dokončením má hello dvoustupňové ověřování. Poté aktualizujete ověření protokolu RADIUS serveru NPS tooreceive z MFA serveru. Použijte následující postup tooconfigure NPS hello:

### <a name="modify-hello-timeout-policy"></a>Změna zásad vypršení časového limitu hello

1. Na serveru NPS otevřete hello **klientů RADIUS a Server** nabídky v hello zbývající sloupce a vyberte **skupin vzdálených serverů RADIUS**. 
2. Vyberte hello **skupiny serverů brány TS**. 
3. Přejděte toohello **Vyrovnávání zatížení** kartě. 
4. Změnit i hello **počet sekund bez odpovědi, než je žádost považována za zrušených** a hello **počet sekund mezi požadavky, když je server identifikován jako nedostupný** toobetween 30 a 60 sekund. (Pokud zjistíte, že tento server hello stále vyprší během ověřování, můžete vraťte se sem a zvýšit hello počet sekund.)
5. Přejděte toohello **účet pro ověřování** kartě a zkontrolujte, jestli porty protokolu RADIUS hello zadaná této hello aplikace Multi-Factor Authentication Server naslouchá na porty hello shodu.

### <a name="prepare-nps-tooreceive-authentications-from-hello-mfa-server"></a>Příprava tooreceive ověřování serveru NPS ze hello MFA serveru

1. Klikněte pravým tlačítkem na **klientů RADIUS** pod klientů RADIUS a servery v hello zbývající sloupce a vyberte **nový**.
2. Přidejte hello Server Azure Multi-Factor Authentication jako klienta protokolu RADIUS. Zvolte popisný název a určete sdílený tajný klíč.
3. Otevřete hello **zásady** nabídky v hello zbývající sloupce a vyberte **zásady vyžádání nového připojení**. Měli byste vidět zásadu TS GATEWAY AUTHORIZATION POLICY, která byla vytvořena při konfiguraci služby Brána VP. Tato zásada předá toohello požadavky protokolu RADIUS serveru Multi-Factor Authentication Server.
4. Klikněte pravým tlačítkem na **TS GATEWAY AUTHORIZATION POLICY** a vyberte **Duplikovat zásadu**. 
5. Otevřete nové zásady a přejděte toohello hello **podmínky** kartě.
6. Přidejte podmínku, která odpovídá popisnému názvu klienta s popisným názvem hello nastavení v kroku 2 pro klienta RADIUS serveru Azure Multi-Factor Authentication hello hello. 
7. Přejděte toohello **nastavení** a vyberte **ověřování**.
8. Změnit hello zprostředkovatele ověřování příliš**ověřování požadavků na tomto serveru**. Tato zásada zajistí, že pokud NPS přijme požadavek protokolu RADIUS z hello Azure MFA serveru, hello dochází k ověřování místní místo odesílání RADIUS požadavek back toohello Azure Multi-Factor Authentication Server, které by vytvořilo smyčku. 
9. tooprevent smyčku, ujistěte se, že je nové zásady hello přikázána nad původní zásadou hello v hello **zásady vyžádání nového připojení** podokně.

## <a name="configure-azure-multi-factor-authentication"></a>Konfigurace Azure Multi-Factor Authentication

Hello Azure Multi-Factor Authentication Server je nakonfigurován jako proxy server RADIUS mezi bránou VP a serveru NPS.  Musí být nainstalován na server připojený k doméně, který je oddělený od serveru služby Brána VP hello. Použijte následující postup tooconfigure hello Azure Multi-Factor Authentication Server hello.

1. Otevřete hello Azure Multi-Factor Authentication Server a vyberte ikonu pro ověřování pomocí protokolu RADIUS hello. 
2. Zkontrolujte hello **ověřování RADIUS povolit** zaškrtávací políčko.
3. Na kartě klienti hello zkontrolujte hello porty shodují, co je nakonfigurováno na serveru NPS a vyberte položku **přidat**.
4. Přidáte IP adresu serveru brány VP hello, název aplikace (volitelné) a sdílený tajný klíč. Hello sdílený tajný potřebám toobe stejné hello na hello Azure Multi-Factor Authentication Server a Brána VP.
3. Přejděte toohello **cíl** kartě a vyberte hello **servery RADIUS** přepínač.
4. Vyberte **přidat** a zadejte hello IP adresu, sdílený tajný klíč a porty serveru NPS hello. Pokud nepoužíváte centrální server NPS, jsou hello stejné hello klienta protokolu RADIUS a cíl protokolu RADIUS. sdílený tajný klíč Hello musí odpovídat jednomu nastavení hello v oddílu klienta protokolu RADIUS serveru NPS hello hello.

![Ověřování Radius](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="next-steps"></a>Další kroky

- Integrace Azure MFA a [webových aplikací IIS](multi-factor-authentication-get-started-server-iis.md)

- Získejte odpovědi v hello [Azure Multi-Factor Authentication – nejčastější dotazy](multi-factor-authentication-faq.md)
