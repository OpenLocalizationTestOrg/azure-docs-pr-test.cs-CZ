---
title: 'Azure AD Connect: Vyberte typ instalace | Microsoft Docs'
description: "Toto téma vás provede procesem instalace hello tooselect jak zadejte toouse pro Azure AD Connect"
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: a700e59eb05947ee1dbd9993141200c9340b2f1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="select-which-installation-type-toouse-for-azure-ad-connect"></a>Vyberte typ toouse které instalace Azure AD Connect
Azure AD Connect má dva typy instalace pro novou instalaci: Express a přizpůsobit. Toto téma vám pomůže toodecide, která možnost toouse během instalace.

## <a name="express"></a>Express
Express je hello nejběžnější možnost a používá přibližně 90 % všechny nové instalace. Bylo navrženou tooprovide konfigurace, který je použitelný pro hello nejběžnějších scénářů zákazníka.

Předpokládá:

- Máte jeden Active Directory doménové struktury na místě.
- Máte účet správce podnikové sítě, které můžete použít pro instalaci hello.
- Máte méně než 100 000 objektů ve vaší místní službě Active Directory.

Můžete získat:

- [Synchronizace hesel](active-directory-aadconnectsync-implement-password-synchronization.md) z tooAzure místní AD pro jednotné přihlašování.
- Konfigurace, který se synchronizuje [uživatelé, skupiny, kontakty a počítače s Windows 10](active-directory-aadconnectsync-understanding-default-configuration.md).
- Synchronizace všech oprávněných objektů ve všech doménách a ve všech organizačních.
- [Automatický upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) je povoleno toomake se vždy používáte nejnovější dostupnou verzi hello.

Možnosti, kde můžete dál používat Express:

- Pokud nechcete, aby toosynchronize všechny organizační jednotky, můžete stále používají systém Express a na poslední stránku hello, zrušte výběr **spustit proces synchronizace hello...** *. Pak znovu spusťte Průvodce instalací hello a změnit hello organizačních jednotek ve [možnosti konfigurace](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options) a povolte plánované synchronizaci.
- Chcete tooenable jedna z funkcí hello v Azure AD Premium, jako je například zpětný zápis hesla. Nejdřív projít express tooget hello počáteční instalace byla dokončena. Pak znovu spusťte Průvodce instalací hello a změnit hello [možnosti konfigurace](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options).

## <a name="custom"></a>Vlastní
Vlastní cesta Hello umožňuje mnohem víc možností než express. By být používána ve všech případech, kdy není hello konfigurace popsané v předchozí části pro expresní reprezentativní pro vaši organizaci.

Použijte, když:

- Nemáte účet správce podnikové tooan přístupu ve službě Active Directory.
- Máte více než jedné doménové struktuře nebo plánujete toosynchronize více než jednu doménovou strukturu v budoucnu hello.
- Máte domén v doménové struktuře není dosažitelný z hello Connect server.
- Máte v plánu toouse federaci nebo předávací ověřování pro přihlášení uživatele.
- Máte více než 100 000 objektů a potřebujete toouse plnou instalaci systému SQL Server.
- Máte v plánu toouse na základě skupiny filtrování a nejen domény nebo filtrování založené na organizační jednotku.

## <a name="upgrade-from-dirsync"></a>Upgrade z nástroje DirSync
Pokud aktuálně používáte DirSync, pak postupujte podle kroků hello v [upgradu z nástroje DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) tooupgrade vaší stávající konfigurace. Existují dva různé možnosti upgradu:

- Místní upgrade tooinstall připojit na hello stejný server.
- Paralelní nasazení tooinstall Connect na nový server, zatímco existující server DirSync hello je stále v provozu.

## <a name="upgrade-from-azure-ad-sync"></a>Upgrade z Azure AD Sync
Pokud používáte Azure AD Sync, pak můžete postupovat podle hello [stejný postup](active-directory-aadconnect-upgrade-previous-version.md) jako při upgradu z jedné verze Connect tooa novější. Existují dva různé možnosti upgradu:

- Místní upgrade tooinstall připojit na hello stejný server.
- Migrace dráha tooinstall Connect na nový server při hello existující server Azure AD Sync je stále v provozu.

## <a name="migrate-from-fim2010-or-mim2016"></a>Migrace z FIM2010 nebo MIM2016
Pokud aktuálně používáte produktu Forefront Identity Manager 2010 nebo Microsoft Identity Manager 2016 s hello konektoru služby Azure AD, je jedinou možností migrace. Postupujte podle kroků hello popsaných v [dráha migrace](active-directory-aadconnect-upgrade-previous-version.md#swing-migration). V krocích hello nahraďte FIM2010/MIM2016 uvedeny žádné informace o Azure AD Sync.

## <a name="next-steps"></a>Další kroky
V závislosti na hello možnost jste vybrali toouse, použijte hello tabulku z obsahu toohello levém toofind váš článek s hello podrobné kroky.
