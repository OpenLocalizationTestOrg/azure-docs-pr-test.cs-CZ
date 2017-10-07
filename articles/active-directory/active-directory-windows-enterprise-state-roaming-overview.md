---
title: "Přehled stavu aaaEnterprise Roaming | Microsoft Docs"
description: "Poskytuje informace o nastavení Enterprise State Roaming v zařízení se systémem Windows. Enterprise State Roaming poskytuje uživatelům v jednotném rozhraní mezi jejich zařízení se systémem Windows a snižuje hello čas potřebný pro konfiguraci nové zařízení."
services: active-directory
keywords: Co je Enterprise State Roaming, synchronizace enterprise, windows cloudu
documentationcenter: 
author: tanning
manager: femila
editor: curtand
ms.assetid: 83b3b58f-94c1-4ab0-be05-20e01f5ae3f0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 436076383b70b7cd65a612e3928f62f26ac5fa84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enterprise-state-roaming-overview"></a>Přehled služby Enterprise State Roaming
S Windows 10 [Azure Active Directory (Azure AD)](active-directory-whatis.md) uživatelé získat hello možnost toosecurely synchronizovat jejich uživatelská nastavení a nastavení toohello dat aplikací v cloudu. Enterprise State Roaming poskytuje uživatelům v jednotném rozhraní mezi jejich zařízení se systémem Windows a snižuje hello čas potřebný pro konfiguraci nové zařízení. Enterprise State Roaming funguje podobně jako standardní toohello [příjemce nastavení synchronizace](http://windows.microsoft.com/en-US/windows-8/sync-settings-pcs) který bylo poprvé dostupné ve Windows 8. Enterprise State Roaming dále nabízí:

* **Oddělení podnikové a uživatelských dat** – organizace jsou v ovládacím prvku svých dat a neexistuje žádný kombinování podnikových dat v cloudu uživatelský účet nebo uživatelských dat v cloudu účet podnikové sítě.
* **Rozšířené zabezpečení** – Data se šifrují automaticky před opuštěním hello uživatele zařízení s Windows 10 pomocí Azure Rights Management (Azure RMS) a data zůstává zašifrovaný v klidovém stavu uložených v cloudu hello. Veškerý obsah zůstává zašifrovaný v klidovém stavu uložených v cloudu hello, s výjimkou hello obory názvů, jako jsou názvy nastavení a aplikace systému Windows.  
* **Lepší správa a sledování** – poskytuje kontrolu a přehled, přes který se má synchronizovat nastavení ve vaší organizaci a v zařízení, která díky integraci portálu hello Azure AD. 

Enterprise State Roaming je k dispozici v několika oblastmi Azure. Můžete najít hello aktualizovat seznam dostupných oblastí na hello [služby Azure podle oblasti](https://azure.microsoft.com/regions/#services) stránky v rámci Azure Active Directory.

| Článek | Popis |
| --- | --- |
| [Povolit Enterprise State Roaming v Azure Active Directory](active-directory-windows-enterprise-state-roaming-enable.md) |Enterprise State Roaming je k dispozici tooany organizace s předplatným Premium Azure Active Directory (Azure AD). Další podrobnosti o tom tooget předplatné služby Azure AD, najdete v části hello [Azure AD produktu](https://azure.microsoft.com/services/active-directory) stránky. |
| [Nastavení a datový roaming – nejčastější dotazy](active-directory-windows-enterprise-state-roaming-faqs.md) |Toto téma odpovědi na některé dotazy, které správci IT mohou mít o nastavení a synchronizaci dat aplikací. |
| [Nastavení MDM pro synchronizaci nastavení zásad skupiny a](active-directory-windows-enterprise-state-roaming-group-policy-settings.md) |Windows 10 obsahuje zásady skupiny a synchronizace toolimit nastavení zásad nastavení mobilních zařízení management (MDM). |
| [Odkaz nastavení roamingu Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md) |Hello následuje úplný seznam všech hello nastavení, které budou roamované nebo by se měl zálohovaná ve Windows 10. |
| [Řešení potíží](active-directory-windows-enterprise-state-roaming-troubleshooting.md) |Toto téma prochází některé základní kroky pro řešení potíží a obsahuje seznam známých problémů. |

