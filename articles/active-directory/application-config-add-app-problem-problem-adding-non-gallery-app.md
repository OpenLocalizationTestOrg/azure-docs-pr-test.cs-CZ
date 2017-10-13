---
title: "Přidání aplikace bez Galerie problém | Microsoft Docs"
description: "Pochopení tučné osoby běžné problémy při přidávání vlastních aplikací bez Galerie"
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
ms.openlocfilehash: bb3fc7877f4e7cafc3904fc67abd87b897874d8a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problem-adding-a-non-gallery-application"></a>Přidání aplikace bez Galerie problém

Tento článek vám pomůže lépe porozumět tučné osoby běžné problémy při přidávání **vlastních aplikací bez Galerie** a jak je vyřešit. 

## <a name="i-clicked-the-add-button-and-my-application-took-a-long-time-to-appear"></a>Po klepnutí na tlačítko "Přidat" a Moje aplikace trvalo dlouhou dobu, než se objeví

Za určitých okolností může trvat 1 – 2 minutách (a někdy delší) pro aplikace se objeví po přidání do vašeho adresáře. Přestože to není normální očekávaný výkon, můžete zjistit, přidání aplikace je v průběhu kliknutím na **oznámení** ikonu (zvonku) v pravém horním rohu stránky [portálu Azure](https://portal.azure.com/) a hledá **probíhá** nebo **dokončeno** oznámení s názvem bez přípony **vytvořit aplikaci**.

Pokud vaše aplikace se nikdy přidá, nebo dojde k chybě při kliknutí na **přidat** tlačítko se zobrazí **oznámení** v **chyba** stavu. Pokud chcete další informace o této chybě Další informace o nebo ke sdílení s engingeer podpory, zobrazí se další informace o této chybě podle kroků v [postup najdete v části Podrobnosti o portálu oznámení](#how-to-see-the-details-of-a-portal-notification) části.

## <a name="i-clicked-the-add-button-and-my-application-didnt-appear"></a>Po klepnutí na tlačítko "Přidat" a nezobrazilo Moje aplikace

V některých případech kvůli přechodným potížím, problémy se sítí nebo chyby, přidání selhání aplikace. Můžete zjistit, to se stane, když kliknete **oznámení** ikonu (zvonku) v pravém horním rohu stránky na portálu Azure a v tématu ikonou červený (!) vedle vaše **vytvořit aplikaci** oznámení. To znamená, že došlo k chybě při vytváření aplikace.

Pokud dojde k chybě při kliknutí na **přidat** tlačítko se zobrazí **oznámení** v **chyba** stavu. Pokud chcete další informace o této chybě Další informace o nebo ke sdílení s engingeer podpory, zobrazí se další informace o této chybě podle kroků v [postup najdete v části Podrobnosti o portálu oznámení](#how-to-see-the-details-of-a-portal-notification) části.

## <a name="i-dont-know-how-to-set-up-my-application-once-ive-added-it"></a>I nemusíte vědět, jak nastavit Moje aplikace, když jste jej přidali

Pokud potřebujete pomoc, získávání informací o vlastních aplikací, [knihovna dokumentů aplikace Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) pomoci při další informace o jednotné přihlašování s Azure AD a jak to funguje.

## <a name="how-to-see-the-details-of-a-portal-notification"></a>Postup najdete v části Podrobnosti o portálu oznámení

Pomocí následujícího postupu můžete zobrazit podrobnosti o portálu oznámení:

1.  klikněte **oznámení** ikonu (zvonku) v pravém horním rohu stránky na portálu Azure

2.  Vyberte všechna oznámení v **chyba** stavu (ty se zobrazí červený (!) vedle je).

   >[!NOTE]
   >Oznámení v nelze kliknout **úspěšné** nebo **probíhá** stavu.
   >
   >

3.  Tento otevřený **podrobnosti oznámení** okno.

4.  Tyto informace použít sami, abyste porozuměli další podrobnosti o problému.

5.  Pokud stále potřebujete pomoc, můžete také sdílet tyto informace se pracovníka podpory nebo product group získat pomoc s váš problém.

6.  Klikněte na tlačítko **ikona kopírování** napravo **Chyba při kopírování** textbox zkopírovat všechny podrobnosti oznámení sdílet s skupiny pracovník podpory nebo produktu.

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a>Získání nápovědy poslat podrobnosti oznámení na pracovníka podpory

To je velmi důležité, že můžete sdílet **všechny níže uvedené podrobnosti** s pracovníka podpory, pokud potřebujete pomoc, tak, aby se vám může pomoct rychle. Můžete to provést pomocí snadno **uděláte snímek,** nebo kliknutím **ikona chyby kopie**, který se nachází vpravo od **Chyba kopírování** textové pole.

## <a name="notification-details-explained"></a>Vysvětlení podrobnosti oznámení

Níže se dozvíte více co každý oznámení položky znamená a jsou uvedeny příklady každý z nich.

### <a name="essential-notification-items"></a>Základní oznámení položky

-   **Název** – popisný název oznámení.
   *  Příklad – **nastavení proxy aplikace**

-   **Popis** – popis co došlo k chybě v důsledku operaci

   *  Příklad – **zadaná interní adresa url je již používán jinou aplikací**

-   **Id oznámení** – jedinečné id oznámení.

   *  Příklad – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

-   **Id žádosti klienta** – id konkrétní žádosti provedené prohlížeč

   *  Příklad – **302fd775-3329-4670-a9f3-bea37004f0bc**

-   **Čas UTC razítko** – časové razítko, během které oznámení došlo k chybě, v UTC

   *  Příklad – **2017-03-23T19:50:43.7583681Z**

-   **Interní Id transakce** – interní ID můžeme použít k vyhledání Chyba v našem systému

   *  Příklad – **71a2f329-ca29-402f-aa72-bc00a7aca603**

-   **Hlavní název uživatele** – uživatel, který provedl operaci

   *  Příklad –**tperkins@f128.info**

-   **Id klienta** – jedinečné ID klienta, který uživatel, který provedl operaci byl uživatel členem

   *  Příklad – **7918d4b5-0442-4a97-be2d-36f9f9962ece**

-   **Objekt uživatele Id** – jedinečné ID uživatele, který provádí operaci

 *  Příklad – **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Podrobné oznámení položky

-   **Zobrazovaný název** – **(nesmí být prázdné)** podrobnější zobrazovaný název chyby

  *  Příklad – **nastavení proxy aplikace**

-   **Stav** – konkrétní stav oznámení.

   *  Příklad – **se nezdařilo**

-   **Id objektu** – **(nesmí být prázdné)** ID objektu, pro kterou byla provedena operace

   *  Příklad – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Podrobnosti o** – podrobný popis co došlo k chybě v důsledku operaci

   *  Příklad – **interní adresa url, http://bing.com/' je neplatná, protože je již používán**

-   **Chyba při kopírování** – klikněte na tlačítko **ikona kopírování** napravo od **Chyba kopírování** textbox zkopírovat všechny podrobnosti oznámení sdílet s skupiny pracovník podpory nebo produkt

   *  Příklad```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Další kroky
[Správa aplikací pomocí služby Azure Active Directory](active-directory-enable-sso-scenario.md)



