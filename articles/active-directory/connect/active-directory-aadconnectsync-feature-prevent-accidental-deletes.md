---
title: "Synchronizace Azure AD Connect: prevence náhodného odstranění | Microsoft Docs"
description: "Toto téma popisuje hello zabránit funkci náhodného odstranění (prevence náhodného odstranění) ve službě Azure AD Connect."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6b852cb4-2850-40a1-8280-8724081601f7
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 159597f8354806fcaea1430e0ff84956338592a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Synchronizace Azure AD Connect: Prevence náhodného odstranění
Toto téma popisuje hello zabránit funkci náhodného odstranění (prevence náhodného odstranění) ve službě Azure AD Connect.

Při instalaci Azure AD Connect, zabránit náhodnému odstranění je ve výchozím nastavení povolené a nakonfigurované toonot povolit exportu s více než 500 odstranění. Tato funkce je navrženou tooprotect prostřednictvím náhodného konfigurace změny a změny tooyour místního adresáře, které má vliv na mnoho uživatelů a dalších objektů.

## <a name="what-is-prevent-accidental-deletes"></a>Co je prevence náhodného odstranění
Běžné scénáře až uvidíte mnoho odstranění patří:

* Změní příliš[filtrování](active-directory-aadconnectsync-configure-filtering.md) kde celou [OU](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) nebo [domény](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) nezaškrtnuté.
* Všechny objekty v organizační jednotce se odstraní.
* Organizační jednotka se přejmenuje tak všechny objekty v ní jsou považovány za toobe mimo rozsah pro synchronizaci.

Výchozí hodnota Hello 500 objektů lze změnit pomocí prostředí PowerShell pomocí `Enable-ADSyncExportDeletionThreshold`. Měli byste nakonfigurovat tato hodnota toofit hello velikost vaší organizace. Vzhledem k tomu, že plánovače synchronizace hello spouští každých 30 minut, je hodnota hello hello počet odstranění vidět do 30 minut.

Pokud jsou moc toobe odstranění dvoufázové instalace exportovat tooAzure AD, pak hello export zastaví a zobrazí se e-mail takto:

![Zabránit náhodného odstranění e-mailu](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

> *Hello (technický kontakt). (Během) hello Identity synchronizační služba zjistila, že hello počet odstranění překročila prahovou hodnotu hello nakonfigurované odstranění pro (název organizace). Pro odstranění v této synchronizace identit spustit byly odeslány z (number) Celkový počet objektů. To splňuje nebo překračuje prahovou hodnotu odstranění hello nakonfigurované objektů (number). Potřebujeme, abyste tooprovide potvrzení, že tyto odstraněním by měl být před jsme bude pokračovat. Podrobnosti viz hello prevence náhodného odstranění pro další informace o chybě hello uvedené v této e-mailové zprávy.*
>
> 

Můžete také zjistit stav hello `stopped-deletion-threshold-exceeded` když se podíváte v hello **Synchronization Service Manager** uživatelského rozhraní pro profil Export hello.
![Prevence náhodného odstranění uživatelského rozhraní Správce služby synchronizace](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)

Pokud jste to neočekávané, prozkoumejte a provést opravné akce. toosee, které objekty jsou o toobe odstranění hello následující:

1. Spustit **synchronizační služba** z hello nabídce Start.
2. Přejděte příliš**konektory**.
3. Vyberte hello konektor s typem **Azure Active Directory**.
4. V části **akce** toohello práva, vyberte **hledání prostoru konektoru**.
5. V automaticky otevírané okno pod hello **oboru**, vyberte **odpojen od** a vyberte čas v minulosti hello. Klikněte na tlačítko **vyhledávání**. Tato stránka obsahuje zobrazení všech objektů o toobe odstranit. Kliknutím na jednotlivé položky, můžete získat další informace o objektu hello. Můžete také kliknout na **sloupec nastavení** tooadd další atributy toobe viditelné v mřížce hello.

![Hledání prostoru konektoru](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

V případě potřeby všechny hello odstranění jsou pak hello následující:

1. tooretrieve hello aktuální prahovou hodnotu odstranění, spusťte rutinu prostředí PowerShell hello `Get-ADSyncExportDeletionThreshold`. Zadejte účet Azure AD globálního správce a heslo. Hello výchozí hodnota je 500.
2. tootemporarily zakažte tuto ochranu a nechat tyto odstranění projít, spusťte rutinu prostředí PowerShell hello: `Disable-ADSyncExportDeletionThreshold`. Zadejte účet Azure AD globálního správce a heslo.
   ![Přihlašovací údaje](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)
3. S hello Azure konektor služby Active Directory stále vybrána, vyberte akci hello **spustit** a vyberte **exportovat**.
4. toore povolení hello ochrany, spusťte rutinu prostředí PowerShell hello: `Enable-ADSyncExportDeletionThreshold -DeletionThreshold 500`. Nahraďte 500 hello hodnotu, kterou jste si všimli při načítání hello aktuální prahovou hodnotu odstranění. Zadejte účet Azure AD globálního správce a heslo.

## <a name="next-steps"></a>Další kroky
**Témata s přehledem**

* [Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
