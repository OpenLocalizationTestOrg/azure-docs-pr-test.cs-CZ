---
title: Povolit Enterprise State Roaming v Azure Active Directory | Microsoft Docs
description: "Nejčastější dotazy o nastavení Enterprise State Roaming v zařízení se systémem Windows. Enterprise State Roaming poskytuje uživatelům v jednotném rozhraní mezi jejich zařízení se systémem Windows a snižuje čas potřebný pro konfiguraci nové zařízení."
services: active-directory
keywords: "Stav Enterprise roaming, cloudu systému windows, jak povolit roaming stavu enterprise"
documentationcenter: 
author: tanning
manager: femila
editor: curtand
ms.assetid: f71d66fd-7f9e-45eb-9cfe-5d989870f8a4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 77d75f4a647e44f27cd9ba8db5286f1456c3ac66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Povolení služby Enterprise State Roaming v Azure Active Directory
Enterprise State Roaming je k dispozici pro všechny organizace s předplatným Premium Azure Active Directory (Azure AD). Další podrobnosti o tom, jak získat předplatné Azure AD najdete v tématu [stránku produktů Azure AD](https://azure.microsoft.com/services/active-directory).

Když povolíte Enterprise State Roaming, vaše organizace bude automaticky udělen licencí předplatného zdarma, omezeného použití na Azure Rights Management. Tato bezplatná registrace je omezený na šifrování a dešifrování enterprise nastavení a data aplikací, které jsou synchronizované se službou Enterprise State Roaming. musíte mít placené předplatné používat úplné funkce služby Azure Rights Management.

Po získání předplatného Azure AD Premium, použijte následující postup povolení Enterprise State Roaming:

1. Přihlášení k portálu Azure classic.
2. Na levé straně vyberte **služby ACTIVE DIRECTORY**a poté vyberte adresář, pro který chcete povolit Enterprise State Roaming.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming.png)
3. Přejděte na **konfigurace** karty v horní části.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-configure.png)
4. Přejděte dolů na stránku a vyberte **uživatelé se mohou SYNCHRONIZOVAT nastavení a dat aplikací ENTERPRISE**a potom klikněte na **Uložit**.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-select-all-sync-settings.png)

Pro zařízení Windows 10 se bude používat roaming nastavení u služby Enterprise State Roaming zařízení musí ověřit pomocí služby Azure AD identity. Pro zařízení, které jsou připojené ke službě Azure AD je primární přihlášení uživatele Azure AD identity, takže není nutná žádná další konfigurace. Pro zařízení, které používají tradiční v místní službě Active Directory, musí správce IT [připojení zařízení připojených k doméně ke službě Azure AD pro Windows 10 vyskytne](active-directory-azureadjoin-devices-group-policy.md).

## <a name="sync-data-storage"></a>Úložiště dat synchronizace
Enterprise State Roaming dat je hostovaná v jednom nebo více [oblastí Azure](https://azure.microsoft.com/regions/) že nejvhodnější zarovnaná s země nebo oblast hodnotu nastavenou v instanci služby Azure Active Directory. Enterprise State Roaming dat je rozdělený do oddílů založené na tři hlavní zeměpisné oblasti: Severní Americe, regionu EMEA a APAC. Enterprise State Roaming dat pro klienta je místně umístěn v zeměpisné oblasti a v oblastech se nereplikuje.  Například Zákazníci, kteří mají jejich země nebo oblast hodnotu nastavit na jedno ze zemí EMEA jako "Francie" nebo "Zambie" budou mít svoje data hostovány v jednom nebo z regionů Azure v rámci Evropa.  Zákazníci, kteří nastavit jejich země nebo oblast hodnotu ve službě Azure AD pro Severní Ameriku zemi jako "USA" nebo "Kanada" bude mít svá data hostované v jedné nebo více z regionů Azure v rámci USA.  Zákazníci, kteří nastavit jejich země nebo oblast hodnotu ve službě Azure AD na jednu z jiných zemí APAC jako "Austrálie" nebo "Nový Zéland" bude mít svá data hostované v jedné nebo více z regionů Azure v Asii.  Jižní Ameriky a Antarktida data bude hostován v jedné nebo více oblastech Azure v rámci USA.  Hodnota země nebo oblast je nastaven jako součást procesu vytváření adresáře Azure AD a nemůže být upravena následně. 

Pokud potřebujete další informace o umístění úložiště dat, prosím soubor lístek s [podporu Azure](https://azure.microsoft.com/support/options/).

## <a name="manage-enterprise-state-roaming"></a>Spravovat Enterprise State Roaming
Globální správci služby Azure AD můžete povolit a zakázat Enterprise State Roaming v portálu Azure classic.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-manage.png)

Globální správci můžou omezit nastavení synchronizace konkrétní skupiny zabezpečení.

Globální správci můžete také zobrazit zprávu o stavu synchronizace zařízení na uživatele výběrem konkrétního uživatele v instanci Active Directory **uživatelé** seznamu a kliknete na **zařízení** kartě a vyberete zobrazení **zařízení synchronizaci nastavení a dat aplikací enterprise**.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-device-sync-settings.png)

## <a name="data-retention"></a>Uchovávání dat
Data synchronizovat s Azure přes Enterprise State Roaming bude uchována po neomezenou dobu, pokud se provádí operace ruční odstranění nebo dotyčném dat je vyhodnocen jako zastaralé. 

**Explicitní odstranění:** data se odstraní při Azure správce odstraní uživatele nebo adresář nebo správce explicitně žádostí, že dat má být odstraněn.

* **Odstranění uživatele**: Při odstranění uživatele ve službě Azure AD uživatelský účet, roaming dat bude označena pro odstranění a odstraní mezi 90 na 180 dní. 
* **Odstranění adresáře**: odstranění celého adresáře ve službě Azure AD je okamžitou operace. Všechna nastavení data související s které adresář bude označena pro odstranění a odstraní mezi 90 na 180 dní. 
* **Na žádost o odstranění**: Pokud chce správce Azure AD ručně odstranit data nebo data nastavení konkrétního uživatele, může správce souboru lístek s [podporu Azure](https://azure.microsoft.com/support/). 

**Odstranění dat zastaralých**: Data, která nepoužil, jeden rok ("dobu uchování"), budou považovány za zastaralé a budou odstraněny z Azure. Doba uchovávání se může změnit, ale nesmí být menší než 90 dnů. Zastaralá data mohou být konkrétní sadu nastavení Windows/aplikace nebo všechna nastavení pro uživatele. Například:

* Pokud žádná zařízení přistupovat k kolekci konkrétní nastavení (například aplikace je odebráno ze zařízení nebo skupinu nastavení, jako je například "Motivu" vypnutá pro všechny uživatele zařízení), pak této kolekce se stane po dobu uchování zastaralé a budou odstraněny. 
* Pokud je uživatel vypnul nastavení synchronizace ve všech jejich zařízeních, pak budou mít přístup žádná nastavení data a všechna data nastavení pro tohoto uživatele bude zastaralé a budou odstraněny po dobu uchování. 
* Pokud správce Azure AD directory vypne Enterprise State Roaming pro celý adresář, pak všechny uživatele v, že adresář se zastaví synchronizaci nastavení a všechna nastavení data pro všechny uživatele bude zastaralé a budou odstraněny po dobu uchování. 

**Odstranit obnovení dat**: zásad uchovávání dat se nedá konfigurovat. Jakmile data byla trvale odstraněna, nebude použitelná pro obnovení. Je ale důležité si uvědomit, že nastavení data pouze se odstraní z Azure, ne zařízení koncového uživatele. Pokud žádné zařízení později znovu připojí ke službě Enterprise State Roaming, bude nastavení znovu synchronizovat a ukládají v Azure.

## <a name="related-topics"></a>Související témata
* [Přehled Enterprise State Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
* [Nastavení a datový roaming – nejčastější dotazy](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Nastavení zásad a správu mobilních zařízení pro synchronizaci nastavení skupiny](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Odkaz nastavení roamingu Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [Řešení potíží](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
