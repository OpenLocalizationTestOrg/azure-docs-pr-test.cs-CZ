---
title: "Řešení potíží s aktivita služby Azure Active Directory protokolů chyb balíček obsahu | Microsoft Docs"
description: "Poskytuje seznam chybových zpráv toofix aktivita služby Azure Active Directory hello obsahu pack a postup je."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 325de65ff1572a2f8f8319c0a52350bda03af3de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a>Řešení potíží s aktivita služby Azure Active Directory protokolů chyb balíček obsahu 


Při práci s hello balíček obsahu Power BI pro Azure Active Directory Preview, je možné, že spustíte do hello následujícím chybám: 

- [Aktualizace se nezdařila](active-directory-reporting-troubleshoot-content-pack.md#refresh-failed) 
- [Pověření ke zdroji dat neúspěšné tooupdate](active-directory-reporting-troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [Import dat trvá příliš dlouho](active-directory-reporting-troubleshoot-content-pack.md#importing-of-data-is-taking-too-long) 
 
Toto téma poskytuje informace o možných příčinách hello a jak toofix tyto chyby.
 
## <a name="refresh-failed"></a>Aktualizace se nezdařila 
 
**Jak se tato chyba prezentované**: e-mailu z Power BI nebo stav selhání v historii aktualizace hello. 


| Příčina | Jak toofix |
| ---   | ---        |
| Aktualizujte selhání chyby může být způsobeno při hello přihlašovací údaje uživatelů hello připojení toohello balíček obsahu byly resetovat, ale neaktualizuje v nastavení připojení hello hello hello balíček obsahu. | V Power BI Najděte odpovídající toohello datovou sadu hello řídicí panel protokoly aktivita služby Azure Active Directory (protokoly aktivita služby Azure Active Directory), zvolte plán aktualizace a pak zadejte přihlašovací údaje Azure AD. |
| Obnovení může selhat z důvodu problémů toodata v hello základní balíček obsahu. | Soubor lístek podpory. Další podrobnosti najdete v tématu [jak tooget podporu pro Azure Active Directory](active-directory-troubleshooting-support-howto.md).|
 
 
## <a name="failed-tooupdate-data-source-credentials"></a>Pověření ke zdroji dat neúspěšné tooupdate 
 
**Jak se tato chyba prezentované**: V Power BI, když se připojujete toohello aktivita služby Azure Active Directory balíček obsahu protokoly (preview). 

| Příčina | Jak toofix |
| ---   | ---        |
| Hello uživatel připojuje, patří ani globální správce ani čtečku zabezpečení nebo správce zabezpečení. | Použijte účet, který je globálním správcem nebo čtečku zabezpečení nebo zabezpečení Správce tooaccess hello balíčky obsahu. |
| Váš klient není klienta Premium nebo nemá alespoň jeden uživatel s licencí Premium souboru. | Soubor lístek podpory. Další podrobnosti najdete v tématu [jak tooget podporu pro Azure Active Directory](active-directory-troubleshooting-support-howto.md).|
 

 

## <a name="importing-of-data-is-taking-too-long"></a>Import dat trvá příliš dlouho 
 
**Jak se tato chyba prezentované**: V Power BI, jakmile se připojíte svůj balíček obsahu proces importu dat hello spustí tooprepare řídicího panelu pro protokol aktivita služby Azure Active Directory. Zobrazí zpráva hello: "*importu dat...* "  

| Příčina | Jak toofix |
| ---   | ---        |
| V závislosti na velikosti hello klienta služby tento krok může trvat několik minut too30 minut. | Právě se pacienta. Pokud zpráva hello nezmění tooshowing řídicího panelu v rámci hodiny, prosím soubor lístek podpory. Další podrobnosti najdete v tématu [jak tooget podporu pro Azure Active Directory](active-directory-troubleshooting-support-howto.md).|

## <a name="next-steps"></a>Další kroky

Klikněte na tlačítko tooinstall hello balíček obsahu Power BI pro Azure Active Directory ve verzi Preview [zde](https://powerbi.microsoft.com/en-us/blog/azure-active-directory-meets-power-bi/).


