---
title: "aaaWindows ověřování a Azure MFA serveru | Microsoft Docs"
description: "Toto je stránka Azure Multi-Factor authentication hello, která vám pomůže při nasazení ověření Windows a serveru Azure Multi-Factor Authentication Server."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 19a4043f-c4ce-43c0-80e7-2548ee92cb74
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/06/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 0fc38fd751966bf883d4eae7c48055988922af80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>Ověření Windows a server Azure Multi-Factor Authentication
Použijte část ověřování systému Windows hello tooenable Azure Multi-Factor Authentication Server hello a konfigurovat ověřování systému Windows pro aplikace. Před nastavením ověřování systému Windows, Udržovat hello seznamu na paměti následující:

* Po dokončení instalace restartování hello Azure Multi-Factor Authentication pro Terminálové služby tootake vliv.
* Pokud je zaškrtnuta možnost 'shodu uživatele vyžadují ověřování Azure Multi-Factor Authentication, a nejsou v seznamu uživatelů hello, nebudete moct toolog do počítače hello po restartování.
* Důvěryhodné že IP adresy jsou závislé na tom, zda text hello aplikace může poskytovat hello IP adresu klienta s ověřováním hello. Momentálně jsou podporovány pouze terminálové služby.  

> [!NOTE]
> Tato funkce není podporovaná toosecure Terminálové služby v systému Windows Server 2012 R2.

## <a name="toosecure-an-application-with-windows-authentication-use-hello-following-procedure"></a>toosecure aplikaci pomocí ověřování systému Windows hello použijte následující postup.
1. V hello Azure Multi-Factor Authentication serveru klikněte na ikonu ověřování Windows hello.
   ![Ověřování systému Windows](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)
2. Zkontrolujte hello **povolit ověřování systému Windows** zaškrtávací políčko. Ve výchozím nastavení je toto políčko zaškrtnuté.
3. na kartě aplikace Hello může správce tooconfigure hello jeden nebo více aplikací pro ověřování systému Windows.
4. Vyberte server nebo aplikaci – určete, zda hello server/aplikace povolena. Klikněte na **OK**.
5. Klikněte na **Přidat...**
6. Karta Důvěryhodné IP adresy Hello vám umožní tooskip Azure vícefaktorového ověřování pro relace systému Windows pocházející z konkrétních IP adres. Například pokud zaměstnanci používají aplikace hello z hello kanceláře i z domova, můžete rozhodnout, že nechcete, aby jejich telefony vyzváněly pro ověřování Azure Multi-Factor Authentication v hello office. V takovém případě zadáte podsíť office hello jako položku důvěryhodných IP adres.
7. Klikněte na **Přidat...**
8. Vyberte **jednu IP adresu** Pokud byste chtěli tooskip jednu IP adresu.
9. Vyberte **rozsah IP adres** Pokud byste chtěli tooskip celý rozsah IP adres. Příklad 10.63.193.1–10.63.193.100.
10. Vyberte **podsíť** Pokud byste chtěli toospecify rozsah IP adres pomocí notace podsítě. Zadejte počáteční IP adresu podsítě hello a vyberte příslušnou síťovou masku hello z rozevíracího seznamu hello.
11. Klikněte na **OK**.

## <a name="next-steps"></a>Další kroky

- [Konfigurace zařízení sítě VPN třetích stran pro Azure MFA Server](multi-factor-authentication-advanced-vpn-configurations.md)

- [Posílení vaší stávající infrastruktury pro ověřování s hello NPS rozšíření pro Azure MFA](multi-factor-authentication-nps-extension.md)
