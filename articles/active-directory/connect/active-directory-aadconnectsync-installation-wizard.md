---
title: "Opětovným spuštěním Průvodce instalací hello Azure AD Connect | Microsoft Docs"
description: "Vysvětluje, jak Průvodce instalací hello funguje hello druhý čas spuštění."
keywords: "Průvodce instalací Hello Azure AD Connect vám umožní nakonfigurovat nastavení hello údržby druhý čas spuštění"
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: d800214e-e591-4297-b9b5-d0b1581cc36a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 83cc74aca471ef9b4f65f7f3582e3e48d3d81cfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-running-hello-installation-wizard-a-second-time"></a>Synchronizace Azure AD Connect: spuštění Průvodce instalací hello podruhé
Hello poprvé spustíte Průvodce instalací hello Azure AD Connect, provede vás procesem tooconfigure instalace. Pokud znovu spustíte Průvodce instalací hello, nabízí možnosti pro údržbu.

Průvodce instalací hello můžete najít v nabídce start hello s názvem **Azure AD Connect**.

![Nabídka Start](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

Když spustíte Průvodce instalací hello, zobrazí se stránka pomocí těchto možností:

![Stránka s seznam dalších úloh](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

Pokud jste nainstalovali služby AD FS službou Azure AD Connect, máte i další možnosti. Další možnosti, které máte služby AD FS jsou dokumentovány v článku Hello [správu služby AD FS](active-directory-aadconnect-federation-management.md#manage-ad-fs).

Vyberte jednu z úloh hello a klikněte na tlačítko **Další** toocontinue.

> [!IMPORTANT]
> Při otevření Průvodce instalací hello, jsou všechny operace v hello synchronizační modul pozastavit. Ujistěte se, že zavřete průvodce instalací hello Jakmile dokončíte změny konfigurace.
>
>

## <a name="view-current-configuration"></a>Aktuální konfigurace zobrazení
Tato možnost poskytuje rychlý přehled o aktuálně nakonfigurované možnosti.

![Stránka se seznam všech možností a jejich stavu](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

Klikněte na tlačítko **předchozí** toogo zpět. Pokud vyberete **ukončení**, zavřete průvodce instalací hello.

## <a name="customize-synchronization-options"></a>Přizpůsobit možnosti synchronizace
Tato možnost je použít toomake změny toohello synchronizace konfigurace. Zobrazí podmnožinu možnosti z cesty instalace hello vlastní konfigurace. Tato možnost zobrazí i v případě začátku použít rychlé instalace.

* [Další adresáře přidat](active-directory-aadconnect-get-started-custom.md#connect-your-directories). Odebírání adresáře, najdete v části [odstranit konektor](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).
* [Změnit doménu a organizační jednotku filtrování](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).
* Odstranit skupinu filtrování.
* [Změnit volitelné funkce](active-directory-aadconnect-get-started-custom.md#optional-features).

Hello další možnosti z hello počáteční instalaci nelze změnit a nejsou k dispozici. Tyto možnosti jsou:

* Změnit hello toouse atribut userPrincipalName a sourceAnchor.
* Změnit hello propojení metody pro objekty z jiné doménové struktuře.
* Povolte filtrování podle skupiny.

## <a name="refresh-directory-schema"></a>Aktualizace schématu adresáře
Tato možnost se používá, pokud jste změnili hello schéma v jednom z místními doménovými strukturami služby AD DS. Například by mohl mít nainstalován Exchange nebo upgradovat schématu tooa systému Windows Server 2012 s objekty zařízení. V takovém případě budete potřebovat tooinstruct Azure AD Connect tooread hello schéma znovu ze služby AD DS a aktualizovat své mezipaměti. Tato akce také regeneruje hello synchronizačního pravidla. Pokud přidáte hello Exchange schématu, jako příklad, přidána hello synchronizační pravidla pro Exchange toohello konfigurace.

Když vyberete tuto možnost, jsou uvedeny všechny adresáře hello ve vaší konfiguraci. Můžete ponechat výchozí nastavení hello a aktualizovat všechny doménové struktury nebo zrušit výběr některé z nich.

![Stránka s seznamu všech adresářů v prostředí hello](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>Konfigurovat pracovní režim
Tato možnost umožňuje tooenable a vypněte pracovní režim na serveru hello. Další informace o pracovním režimu a způsobu jejich použití naleznete v [operace](active-directory-aadconnectsync-operations.md#staging-mode).

Hello možnost zobrazí, pokud pracovní aktuálně povolená nebo zakázaná:  
![Možnost, která se také zobrazuje aktuální stav hello pracovní režim](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

toochange hello stavu, vyberte tuto možnost a vyberte nebo zrušit výběr hello zaškrtávací políčko.  
![Možnost, která se také zobrazuje aktuální stav hello pracovní režim](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Změnit přihlášení uživatele
Tato možnost umožňuje toochange z toofederation synchronizace hesel nebo hello opačným způsobem. Nelze změnit, příliš**nekonfigurujte**.

Další informace o této možnosti najdete v tématu [přihlášení uživatele](active-directory-aadconnect-user-signin.md#changing-the-user-sign-in-method).

## <a name="next-steps"></a>Další kroky
* Další informace o hello konfigurační model používá synchronizaci Azure AD Connect v [Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

**Témata s přehledem**

* [Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
