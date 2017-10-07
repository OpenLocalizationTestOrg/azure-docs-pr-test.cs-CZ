---
title: "Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace | Microsoft Docs"
description: "Vysvětluje, jak funguje synchronizace Azure AD Connect a toocustomize."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: ee4bf802-045b-4da0-986e-90aba2de58d6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2016
ms.author: markvi
ms.openlocfilehash: 97e4bd9904b077f2628e5f8dcbaa6f1a76168a12
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace
Hello Azure Active Directory Connect synchronizačních služeb (Azure AD Connect sync) je hlavní součást služby Azure AD Connect. Se stará o všechny hello operace, které jsou data identit související toosynchronize mezi místním prostředím a Azure AD. Synchronizace Azure AD Connect je hello nástupcem nástroje DirSync, Azure AD Sync a produktu Forefront Identity Manager s hello nakonfigurovat konektor služby Azure Active Directory.

Toto téma je hello domácí pro **synchronizace Azure AD Connect** (také nazývané **synchronizační modul**) a seznamy odkazů tooall jiných tooit související témata. Pro odkazy tooAzure AD Connect, najdete v části [integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).

Hello synchronizační služby se skládá ze dvou komponent, hello místní **synchronizace Azure AD Connect** volá komponenta a hello straně služby ve službě Azure AD **služba Azure AD Connect sync**. Služba Hello je běžné, že DirSync, Azure AD Sync a Azure AD Connect.

## <a name="azure-ad-connect-sync-topics"></a>Témata týkající se synchronizace Azure AD Connect
| Téma | Co pokrývá a kdy tooread |
| --- | --- |
| **Základy synchronizace Azure AD Connect** | |
| [Principy hello architektura](active-directory-aadconnectsync-understanding-architecture.md) |Pro těch, které jste kdo jsou nové toohello synchronizační modul a má toolearn o architektuře hello a hello termínů používaných. |
| [Technické koncepty](active-directory-aadconnectsync-technical-concepts.md) |Zkrácený hello architektura tématu a stručně popisuje hello termínů používaných. |
| [Topologie pro Azure AD Connect](active-directory-aadconnect-topologies.md) |Popisuje hello různé scénáře a topologie hello synchronizační modul podporuje. |
| **Vlastní konfigurace** | |
| [Spuštěné hello Průvodce instalací znovu](active-directory-aadconnectsync-installation-wizard.md) |Vysvětluje, co možnosti budete mít k dispozici, pokud znovu spustíte Průvodce instalací hello Azure AD Connect. |
| [Seznámení s deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md) |Popisuje hello konfigurační model názvem deklarativní zřizování. |
| [Principy výrazů deklarativního zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |Popisuje hello syntaxe hello výraz jazyk použitý v deklarativní zřizování. |
| [Principy hello výchozí konfigurace](active-directory-aadconnectsync-understanding-default-configuration.md) |Popisuje pravidla out-of-box hello a hello výchozí konfigurace. Také popisuje, jak spolupracují hello pravidla pro scénáře out-of-box toowork hello. |
| [Principy uživatelů a kontaktů](active-directory-aadconnectsync-understanding-users-and-contacts.md) |Pokračuje v předchozí téma hello a popisuje, jak hello konfigurace pro uživatele a kontakty funguje společně, zejména v prostředí s více doménovými strukturami. |
| [Jak toomake toohello změnu výchozí konfigurace](active-directory-aadconnectsync-change-the-configuration.md) |Vás provede procesem tok toomake běžné tooattribute změny konfigurace. |
| [Osvědčené postupy pro změnu výchozí konfigurace hello](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) |Omezení podpory a pro provedení změny toohello out-of-box konfigurace. |
| [Konfigurace filtrování](active-directory-aadconnectsync-configure-filtering.md) |Popisuje, jak synchronizovat toolimit, který bude objekty tooAzure AD a podrobné postupy hello různé možnosti tooconfigure tyto možnosti. |
| **Scénáře a funkce** | |
| [Prevence náhodného odstranění](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) |Popisuje hello *prevence náhodného odstranění* funkce a jak tooconfigure ho. |
| [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) |Popisuje předdefinované scheduler hello, která je importování, synchronizaci a export dat. |
| [Implementace synchronizace hesel](active-directory-aadconnectsync-implement-password-synchronization.md) |Popisuje, jak funguje synchronizace hesel, jak tooimplement a jak toooperate a odstraňování potíží. |
| [Zpětný zápis zařízení.](active-directory-aadconnect-feature-device-writeback.md) |Popisuje, jak funguje zpětný zápis zařízení v Azure AD Connect. |
| [Rozšíření adresáře](active-directory-aadconnectsync-feature-directory-extensions.md) |Popisuje, jak tooextend hello schéma služby Azure AD s vlastní atributy. |
| **Synchronizační služba mim** | |
| [Funkce služby Azure AD Connect sync](active-directory-aadconnectsyncservice-features.md) |Popisuje hello synchronizační služby straně a jak toochange synchronizovat nastavení ve službě Azure AD. |
| [Odolnost proti chybám duplicitní atribut](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) |Popisuje, jak tooenable a používat **userPrincipalName** a **proxyAddresses** duplicitní atribut hodnoty odolnost proti chybám. |
| **Operace a uživatelského rozhraní** | |
| [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md) |Popisuje hello Synchronization Service Manager uživatelského rozhraní, včetně [operace](active-directory-aadconnectsync-service-manager-ui-operations.md), [konektory](active-directory-aadconnectsync-service-manager-ui-connectors.md), [Metaverse designeru](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md), a [hledání úložiště Metaverse](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) karty. |
| [Provozní úlohy a důležité informace](active-directory-aadconnectsync-operations.md) |Popisuje provozní otázky, jako je například zotavení po havárii. |
| **Jak...** | |
| [Obnovení účtu hello Azure AD](active-directory-aadconnectsync-howto-azureadaccount.md) |Použití tooreset hello pověření účtu služby hello tooconnect z Azure AD Connect sync tooAzure AD. |
| **Další informace a odkazy** | |
| [Porty](active-directory-aadconnect-ports.md) |Seznamy, které porty budete potřebovat tooopen mezi hello synchronizační modul a místních adresářů a Azure AD. |
| [Atributy synchronizované tooAzure služby Active Directory](active-directory-aadconnectsync-attributes-synchronized.md) |Zobrazuje seznam všech atributů synchronizovaných mezi místní AD a Azure AD. |
| [Reference k funkcím](active-directory-aadconnectsync-functions-reference.md) |Obsahuje seznam všech funkcí, které jsou k dispozici v deklarativní zřizování. |

## <a name="additional-resources"></a>Další zdroje
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)

