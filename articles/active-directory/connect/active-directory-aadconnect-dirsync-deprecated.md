---
title: "aaaUpgrade z nástroje DirSync a Azure AD Sync | Microsoft Docs"
description: "Popisuje, jak tooupgrade z nástroje DirSync a Azure AD Sync tooAzure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: bd68fb88-110b-4d76-978a-233e15590803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d465b137fb54f0c6e28ccf73429c4bd3aafd8698
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-windows-azure-active-directory-sync-and-azure-active-directory-sync"></a>Upgrade systému Windows Azure Active Directory Sync a Azure Active Directory Sync.
Azure AD Connect je nejlepší způsob, jak tooconnect hello vašeho místního adresáře s Azure AD a Office 365. Toto je tooupgrade tooAzure nejvhodnější doba, AD Connect z Windows Azure Active Directory Sync (DirSync) nebo Azure AD Sync tyto nástroje jsou nyní zastaralé a bude dosáhnout konce podporu na 13. dubna 2017.

Hello dvě identity synchronizace nástroje, které jsou zastaralé byly nabízeny pro zákazníky s jednou doménovou strukturou (DirSync) a pro více doménovými strukturami a další pokročilé zákazníky (Azure AD Sync). Tyto starší nástroje nahradil jeden řešení, které je k dispozici ve všech scénářích: Azure AD Connect. Nabízí nové funkce, nové funkce a vylepšení a podporu pro nové scénáře. možnost toocontinue toosynchronize toobe tooAzure dat identity místní AD a Office 365, doporučujeme upgradu tooAzure AD Connect.

poslední verzi nástroje DirSync Hello byla vydána v červenec 2014 a hello poslední verzi Azure AD Sync byla vydaná v květnu 2015.

## <a name="what-is-azure-ad-connect"></a>Co je služba Azure AD Connect
Azure AD Connect je hello následníka tooDirSync a Azure AD Sync. Ve všech scénářích je kombinací tyto dva podporována. Další informace o něm v [integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).

## <a name="deprecation-schedule"></a>Vyřazení plán
| Datum | Komentář |
| --- | --- |
| 13. dubna 2016 |Windows Azure Active Directory Sync (DirSync") a Microsoft Azure Active Directory Sync (Azure AD Sync") jsou oznámila jako zastaralé. |
| 13. dubna 2017 |Podporují elementy end. Zákazníci již nebude možné tooopen případu podpory bez nutnosti upgradu tooAzure AD nejprve připojit. |
|31. prosince 2017|Azure AD nebude přijímat komunikaci od Windows Azure Active Directory Sync (DirSync") a Microsoft Azure Active Directory Sync (Azure AD Sync").

## <a name="how-tootransition-tooazure-ad-connect"></a>Jak tootransition tooAzure AD Connect
Pokud používáte nástroje DirSync, existují dva způsoby, můžete upgradovat: místní upgrade a paralelního nasazení. Místní upgrade se doporučuje pro většinu zákazníků a pokud máte poslední operační systém a menší než 50 000 objektů. V ostatních případech se doporučuje, že toodo paralelní nasazení, kde je konfiguraci nástroje DirSync přesouvána tooa nový server se službou Azure AD Connect.

Pokud používáte Azure AD Sync, se doporučuje místní upgrade. Pokud chcete, je možné tooinstall nový server Azure AD Connect paralelně a provést migraci dráha z vaší tooAzure server Azure AD Sync AD Connect.

| Řešení | Scénář |
| --- | --- |
| [Upgrade z nástroje DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) |<li>Pokud máte existující server DirSync již byla spuštěna.</li> |
| [Upgrade z Azure AD Sync](active-directory-aadconnect-upgrade-previous-version.md) |<li>Pokud přecházíte z Azure AD Sync.</li> |

Pokud chcete toosee jak toodo místní upgrade z nástroje DirSync tooAzure AD Connect a pak najdete v tomto videu Channel 9:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-in-place-upgrade-from-legacy-tools/player]
>
>

## <a name="faq"></a>Nejčastější dotazy
**Otázka: obdrželi jste e-mailových oznámení z hello týmu Azure a/nebo zprávy z Centra zpráv hello Office 365, ale používám připojit.**  
Hello oznámení byla odeslána také toocustomers pomocí Azure AD Connect s číslem sestavení 1.0. \*.0 (s použitím verze pre-1.1). Společnost Microsoft doporučuje zákazníky, které uvolní toostay aktuální službou Azure AD Connect. Hello [automatický upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) funkce zavedená v 1.1 umožňuje snadno tooalways mají nejnovější verzi nainstalovanou službu Azure AD Connect.

**Otázka: bude DirSync nebo Azure AD Sync přestat pracovat na 13. dubna 2017?**  
DirSync nebo Azure AD Sync bude toowork na 2017 13. dubna.  Azure AD už nebude přijímat komunikaci z nástroje DirSync nebo Azure AD Sync na 2017 31. prosince.

**Otázka: které verze nástroje DirSync můžete upgradovat z?**  
Je podporované tooupgrade z jakékoli verze nástroje DirSync aktuálně používá.

**Otázka: co o hello Azure AD Connector pro FIM nebo MIM?**  
má Hello Azure AD Connector pro FIM nebo MIM **není** byla oznámeno jako zastaralé. Je na **funkce zablokování**; je přidány žádné nové funkce a obdrží žádné opravy chyb. Společnost Microsoft doporučuje zákazníkům, kteří používají ho tooplan toomove z něj tooAzure AD Connect. Je důrazně doporučuje toonot počáteční všechna nová nasazení použití. Tento konektor budou oznámeny v hello budoucí nepoužívá.

## <a name="additional-resources"></a>Další zdroje
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
