---
title: "sestavy využití a aaaAccess pro Azure MFA | Microsoft Docs"
description: Popisuje, jak toouse hello funkce Azure Multi-Factor Authentication - sestavy.
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtand
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: kgremban
ms.openlocfilehash: ae7ccceca4968d7ec7cf0cb1cf9e041d9997c840
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Sestavy v Azure Multi-Factor Authentication
Azure Multi-Factor Authentication nabízí několik sestav, které mohou být využívána vám a vaší organizaci. Tyto sestavy je přístupná prostřednictvím hello Multi-Factor Authentication Management Portal. Hello tady je seznam dostupných sestav hello:

| Sestava | Popis |
|:--- |:--- |
| Využití |využití Hello zprávy obsahují informace na celkové využití: uživatelský souhrn a podrobnosti o uživateli. |
| Stav serveru |Tato sestava zobrazuje stav hello aplikace Multi-Factor Authentication Server, která je spojená s vaším účtem. |
| Historie blokování uživatelů |Tyto sestavy zobrazit historii hello tooblock požadavky nebo odblokovat uživatele. |
| Historie přeskočených uživatelů |Zobrazí historii hello toobypass žádosti o službu Multi-Factor Authentication pro telefonní číslo uživatele. |
| Upozornění na podvod |Zobrazí historii upozornění na podvod odeslaných během hello rozsah, který jste zadali. |
| Ve frontě |Zobrazí sestavy zařazené do fronty pro zpracování a jejich stav. Po dokončení sestavy hello je vypracována sestava odkaz toodownload nebo zobrazení hello. |

## <a name="view-reports"></a>Zobrazení sestav
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. Na levé straně hello vyberte služby Active Directory.
3. Proveďte jeden z těchto dvou možností, v závislosti na tom, zda používáte zprostředkovatelé ověřování:
   * **Možnost 1**: klikněte na kartu hello zprostředkovatelé vícefaktorového ověřování. Vyberte poskytovatele MFA a klikněte na tlačítko hello **spravovat** tlačítko dole v hello.
   * **Možnost 2**: vyberte váš adresář a přejděte toohello **konfigurace** kartě. V části služby Multi-Factor authentication hello vyberte **spravovat nastavení služby**. V hello dolní části stránky hello nastavení vícefaktorového ověřování služby klikněte na položku hello přejděte toohello portálu odkaz.
4. V hello Azure Multi-Factor Authentication Management Portal, vyberte typ hello sestavy, které chcete z hello **zobrazit sestavu** část v levé navigační hello.

<center>![Cloud](./media/multi-factor-authentication-manage-reports/report.png)</center>


**Další zdroje**

* [Pro uživatele](end-user/multi-factor-authentication-end-user.md)
* [Azure Multi-Factor Authentication na webu MSDN](https://msdn.microsoft.com/library/azure/dn249471.aspx)
