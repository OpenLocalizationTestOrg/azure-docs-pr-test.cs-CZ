---
title: "Přidání aplikace Azure AD Galerie aaaProblem | Microsoft Docs"
description: "Pochopení hello běžné problémy uživatelé setkávají při přidávání aplikací Galerie Azure AD a co můžete dělat tooresolve je"
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
ms.openlocfilehash: 654f98116176d5590563c0471b92809f8763fbd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problem-adding-an-azure-ad-gallery-application"></a>Problém, přidání aplikace Galerie Azure AD

Tento článek vám pomůže toounderstand hello běžné problémy uživatelé setkávají při přidávání aplikací Galerie Azure AD a co můžete dělat tooresolve je.

## <a name="i-clicked-hello-add-button-and-my-application-took-a-long-time-tooappear"></a>Klepnutí na tlačítko "Přidat", tlačítka a Moje aplikace trvalo dlouhou dobu tooappear hello

Za určitých okolností může trvat 1 – 2 minutách (a někdy delší) pro tooappear aplikace po jeho přidání tooyour adresáře. Přestože to není hello normální očekávaný výkon, můžete zjistit, přidání aplikace hello je v průběhu kliknutím na hello **oznámení** ikonu (hello zvonku) v hello pravém horním rohu stránky hello [portálu Azure](https://portal.azure.com/)a hledá **probíhá** nebo **dokončeno** oznámení s názvem bez přípony **vytvoření aplikace.**

Pokud vaše aplikace se nikdy přidá, nebo dojde k chybě při kliknutí na hello **přidat** tlačítko se zobrazí **oznámení** v **chyba** stavu. Pokud chcete další informace o toolearn chyba hello další tooor sdílet s engingeer podpory, zobrazí se další informace o chybě hello podle následujících kroků hello v hello [jak toosee hello podrobnosti o portálu oznámení](#how-to-see-the-details-of-a-portal-notification) části.

## <a name="i-clicked-hello-add-button-and-my-application-didnt-appear"></a>Po klepnutí tlačítko "Přidat" hello a nezobrazilo Moje aplikace

V některých případech z důvodu problémů s tootransient, problémy se sítí nebo chyby, přidání selhání aplikace. Můžete zadat, to se stane, když kliknete na tlačítko hello **oznámení** ikonu (hello zvonku) v hello pravé horní části hello portálu Azure a červený (!) najdete v části Další tooyour ikonu **vytvořit aplikaci** oznámení. To znamená, že došlo k chybě při vytváření aplikace hello.

Pokud dojde k chybě při kliknutí na hello **přidat** tlačítko se zobrazí **oznámení** v **chyba** stavu. Pokud chcete další informace o toolearn chyba hello další tooor sdílet s engingeer podpory, zobrazí se další informace o chybě hello podle následujících kroků hello v hello [jak toosee hello podrobnosti o portálu oznámení](#how-to-see-the-details-of-a-portal-notification) části.

 ## <a name="i-dont-know-how-tooset-up-my-application-once-ive-added-it"></a>Neznámého jak tooset až po Moje aplikace byly přidány ho

Pokud potřebujete pomoc, získávání informací o aplikace, hello [seznamu kurzy o tooIntegrate SaaS aplikací s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) článek je vhodné místo toostart.

V přidání toothis hello [knihovna dokumentů aplikace Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) vám pomůžou toolearn informace o jednotné přihlašování s Azure AD a jak to funguje.

## <a name="how-toosee-hello-details-of-a-portal-notification"></a>Jak toosee hello podrobnosti o portálu oznámení

Podle následujících kroků hello, můžete zjistit podrobnosti o hello všechna portálu oznámení:

1.  Klikněte na tlačítko hello **oznámení** ikonu (hello zvonku) v pravé horní hello části hello portálu Azure

2.  Vyberte všechna oznámení v **chyba** stavu (ty s další toothem červený (!)).

    >[!NOTE]
    >Oznámení v nelze kliknout **úspěšné** nebo **probíhá** stavu.
    >
    >

3.  Tento otevřený hello **podrobnosti oznámení** okno.

4.  Tyto informace použijte sami toounderstand další podrobnosti o problému hello.

5.  Pokud stále potřebujete pomoc, můžete také sdílet tyto informace se na podporu pracovníka nebo hello skupiny tooget Nápověda k produktu s váš problém.

6.  Klikněte na tlačítko hello **kopie** **ikonu** toohello napravo od hello **Chyba kopírování** textbox toocopy všechny hello tooshare podrobnosti oznámení s skupiny pracovník podpory nebo produkt

## <a name="how-tooget-help-by-sending-notification-details-tooa-support-engineer"></a>Jak pomoci tooget odesláním pracovníka podpory tooa podrobnosti oznámení

To je velmi důležité, že můžete sdílet **všechny níže uvedené podrobnosti o hello** s pracovníka podpory, pokud potřebujete pomoc, tak, aby se vám může pomoct rychle. Můžete to provést pomocí snadno **uděláte snímek,** nebo kliknutím hello **ikona chyby kopie**, najít toohello napravo od hello **Chyba kopírování** textové pole.

## <a name="notification-details-explained"></a>Vysvětlení podrobnosti oznámení

Hello níže vysvětluje více co každý hello oznámení položek znamená a poskytuje příklady každého z nich.

### <a name="essential-notification-items"></a>Základní oznámení položky

-   **Název** – hello popisný název hello oznámení

  * Příklad – **nastavení proxy aplikace**

-   **Popis** – hello popis co došlo k chybě v důsledku operace hello

    -   Příklad – **zadaná interní adresa url je již používán jinou aplikací**

-   **Id oznámení** – hello jedinečné id oznámení hello

    -   Příklad – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

-   **Id žádosti klienta** – id konkrétního požadavku hello provedené prohlížeč

    -   Příklad – **302fd775-3329-4670-a9f3-bea37004f0bc**

-   **Čas UTC razítko** – hello časové razítko, během které hello oznámení došlo k chybě, v UTC

    -   Příklad – **2017-03-23T19:50:43.7583681Z**

-   **Interní Id transakce** – hello interní ID můžeme použít toolook hello Chyba v našem systému

    -   Příklad – **71a2f329-ca29-402f-aa72-bc00a7aca603**

-   **Hlavní název uživatele** – hello uživatele, který provedl operaci hello

    -   Příklad –**tperkins@f128.info**

-   **Id klienta** – hello jedinečné ID klienta hello, který hello uživatele, který provedl operaci hello byl členem

    -   Příklad – **7918d4b5-0442-4a97-be2d-36f9f9962ece**

-   **Objekt uživatele Id** – hello jedinečné ID hello uživatele, který provedl operaci hello

    -   Příklad – **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Podrobné oznámení položky

-   **Zobrazovaný název** – **(nesmí být prázdné)** podrobnější zobrazovaný název pro chybu hello

    -   Příklad – **nastavení proxy aplikace**

-   **Stav** – hello konkrétní stav hello oznámení

    -   Příklad – **se nezdařilo**

-   **Id objektu** – **(nesmí být prázdné)** hello ID objektu, podle které hello byla provedena operace

    -   Příklad – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Podrobnosti o** – hello podrobný popis co došlo k chybě v důsledku operace hello

    -   Příklad – **interní adresa url, http://bing.com/' je neplatná, protože je již používán**

-   **Chyba při kopírování** – klikněte na tlačítko hello **ikona kopírování** toohello napravo od hello **Chyba kopírování** textbox toocopy všechny hello tooshare podrobnosti oznámení s skupiny pracovník podpory nebo produkt

    -   Příklad```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Další kroky
[Správa aplikací pomocí služby Azure Active Directory](active-directory-enable-sso-scenario.md)
