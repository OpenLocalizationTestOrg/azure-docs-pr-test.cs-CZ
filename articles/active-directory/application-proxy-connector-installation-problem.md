---
title: "instalace aaaProblem hello konektor agenta Proxy aplikací. | Microsoft Docs"
description: "Jak tootroubleshoot problémů může hello setkávají při instalaci agenta konektor Proxy aplikace"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 07ac366a429083af0c9b87aa9df9cf3876132b90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problem-installing-hello-application-proxy-agent-connector"></a>Problém instalace hello agenta konektor Proxy aplikace

Microsoft AAD Application Proxy Connector je součást interní doméně, která používá připojení hello tooestablish odchozí připojení z hello cloudu dostupný koncový bod toohello interní domény.

## <a name="general-problem-areas-with-connector-installation"></a>Obecné problémových oblastí pomocí instalace konektoru

Při hello instalace konektoru selže, hello hlavní příčinou je obvykle jednu z následujících oblastí hello:

1.  **Připojení** – toocomplete úspěšnou instalaci hello tooregister potřebám nový konektor a vytvořit vlastnosti budoucí vztah důvěryhodnosti. K tomu je potřeba připojení toohello cloudové služby AAD Application Proxy.

2.  **Navázání vztahu důvěryhodnosti** – nový konektor hello vytvoří certifikátu podepsaného svým držitelem a zaregistruje toohello cloudové služby.

3.  **Ověřování Dobrý den, správce** – během instalace, hello uživatel musí poskytnout přihlašovací údaje Správce instalace konektoru toocomplete hello.

## <a name="verify-connectivity-toohello-cloud-application-proxy-service-and-microsoft-login-page"></a>Zkontrolujte připojení k toohello Cloudový Proxy aplikací služby a Microsoft Login stránku

**Cíl:** ověřte, zda tento počítač konektor hello můžete připojit koncový bod registrace AAD aplikace Proxy toohello, jakož i Microsoft přihlašovací stránku.

1.  Otevřete prohlížeč a přejděte toohello následující webové stránce: <https://aadap-portcheck.connectorporttest.msappproxy.net> a ověřte, že hello připojení tooCentral USA a datová centra východní USA s porty 9090 a 9091 pracuje.

2.  Pokud některé z těchto portů neproběhne úspěšně (nemá zeleného zaškrtnutí), ověřte, že hello brány Firewall nebo proxy serveru back-end má \*. msappproxy.net s porty 9090 a 9091 správně definované.

3.  Otevřete prohlížeč (samostatné kartě) a přejděte na následující webové stránce toohello: <https://login.microsoftonline.com>, ujistěte se, že můžete se přihlásit toothat stránky.

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-cert"></a>Ověřte, zda počítač a back-end součásti Podpora pro cert vztah důvěryhodnosti Proxy aplikace

**Cíl:** ověřte, zda počítač hello konektor, back-end proxy a firewall podporuje hello certifikát vytvořený hello konektor pro budoucí vztah důvěryhodnosti.

>[!NOTE]
>konektor Hello pokusí toocreate SHA512 certifikátu, který podporuje TLS1.2. Pokud počítač hello nebo hello back-end brány firewall a proxy server nepodporuje TLS1.2, hello instalace se nezdaří.
>
>

**problém tooresolve hello:**

1.  Ověřte hello počítač podporuje TLS1.2 – verze všechny systémy Windows po 2012 R2 by měly podporovat protokol TLS 1.2. Pokud je váš počítač konektor z verze 2012 R2 nebo před, ujistěte se, že hello následující články znalostní báze jsou nainstalovány na počítači hello: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2>

2.  Obraťte se na správce sítě a požádejte tooverify jestli hello back-end proxy a firewall neblokují SHA512 pro odchozí provoz.

## <a name="verify-admin-is-used-tooinstall-hello-connector"></a>Ujistěte se, že správce použít tooinstall hello konektoru

**Cíl:** ověřte, že hello uživatel při pokusu o tooinstall hello konektor je správce se správnými přihlašovacími údaji. V současné době hello uživatel musí být globálním správcem pro instalaci toosucceed hello.

**tooverify hello pověření jsou správná:**

Připojit příliš<https://login.microsoftonline.com> a používání hello stejné přihlašovací údaje. Ujistěte se, že hello přihlášení úspěšné. Role uživatele hello můžete zkontrolovat tak, že přejdete příliš**Azure Active Directory**  - &gt; **uživatelů a skupin**  - &gt; **všichni uživatelé**. 

Vyberte uživatelský účet, pak "Directory Role" v nabídce výsledné hello. Ověřte, zda že je tento hello vybranou roli "Globální správce". Pokud jste nelze tooaccess žádné hello stránky společně tyto kroky, nejste globálním správcem.

## <a name="next-steps"></a>Další kroky
[Pochopení konektory proxy aplikace služby Azure AD](application-proxy-understand-connectors.md)
