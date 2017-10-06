---
title: aaaEnable Enterprise State Roaming v Azure Active Directory | Microsoft Docs
description: "Nejčastější dotazy o nastavení Enterprise State Roaming v zařízení se systémem Windows. Enterprise State Roaming poskytuje uživatelům v jednotném rozhraní mezi jejich zařízení se systémem Windows a snižuje hello čas potřebný pro konfiguraci nové zařízení."
services: active-directory
keywords: "Stav Enterprise roaming, cloudu systému windows, jak tooenable enterprise stavu roaming"
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
ms.openlocfilehash: c69a9cfa8055f418a3b81c62939885d5bec8a6f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Povolení služby Enterprise State Roaming v Azure Active Directory
Enterprise State Roaming je k dispozici tooany organizace s předplatným Premium Azure Active Directory (Azure AD). Další podrobnosti o tom tooget předplatné služby Azure AD, najdete v části hello [stránku produktů Azure AD](https://azure.microsoft.com/services/active-directory).

Když povolíte Enterprise State Roaming, vaše organizace bude automaticky udělen licencí pro bezplatné omezeného použití předplatného tooAzure Rights Management. Tato bezplatná registrace je omezená tooencrypting a dešifrování enterprise nastavení a data aplikací synchronizaci tím hello služby Enterprise State Roaming; musíte mít placené předplatné toouse hello veškeré funkce služby Azure Rights Management.

Po získání předplatného Azure AD Premium, postupujte podle těchto kroků tooenable Enterprise State Roaming:

1. Přihlášení toohello portál Azure classic.
2. Na levé straně hello vyberte **služby ACTIVE DIRECTORY**a pak vyberte hello adresář, pro které chcete tooenable Enterprise State Roaming.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming.png)
3. Přejděte toohello **konfigurace** karty v horní části hello.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-configure.png)
4. Přejděte dolů a vyberte hello **uživatelé se mohou SYNCHRONIZOVAT nastavení a dat aplikací ENTERPRISE**a potom klikněte na **Uložit**.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-select-all-sync-settings.png)

Pro Windows 10 nastavení tooroam zařízení s hello služby Enterprise State Roaming hello zařízení musí ověřit pomocí služby Azure AD identity. Pro zařízení, které jsou připojené k tooAzure AD je primární přihlášení uživatele hello hello Azure AD identity, takže není nutná žádná další konfigurace. Pro zařízení, která používají tradiční v místní službě Active Directory, hello správce IT musí [připojit tooAzure hello připojená k doméně AD pro Windows 10 vyskytne](active-directory-azureadjoin-devices-group-policy.md).

## <a name="sync-data-storage"></a>Úložiště dat synchronizace
Enterprise State Roaming dat je hostovaná v jednom nebo více [oblastí Azure](https://azure.microsoft.com/regions/) že nejvhodnější zarovnaná s hello země nebo oblast hodnotu nastavenou v instanci Azure Active Directory hello. Enterprise State Roaming dat je rozdělený do oddílů založené na tři hlavní zeměpisné oblasti: Severní Americe, regionu EMEA a APAC. Enterprise State Roaming dat pro klienta hello místně nachází s hello zeměpisné oblasti a v oblastech se nereplikuje.  Například Zákazníci, kteří mají jejich země nebo oblast hodnota sadu tooone EMEA zemí jako "Francie" nebo "Zambie" budou mít svoje data v jednom nebo hostovaným z hello oblastí Azure v rámci Evropa.  Zákazníci, kteří nastavit jejich hodnotu země nebo oblast v Azure AD tooone Severní Ameriku zemí jako "USA" nebo "Kanada" budou mít svoje data hostované v jedné nebo více hello oblastí Azure v rámci hello USA.  Zákazníci, kteří nastavit jejich hodnotu země nebo oblast v Azure AD tooone APAC zemí jako "Austrálie" nebo "Nový Zéland" budou mít svoje data hostované v jedné nebo více hello Azure oblastem v Asii.  Jižní Ameriky a Antarktida data bude hostován v jedné nebo více oblastech Azure v rámci hello USA.  Hodnota země nebo oblast Hello je nastaven jako součást hello proces vytvoření adresáře Azure AD a nemůže být upravena následně. 

Pokud potřebujete další informace o umístění úložiště dat, prosím soubor lístek s [podporu Azure](https://azure.microsoft.com/support/options/).

## <a name="manage-enterprise-state-roaming"></a>Spravovat Enterprise State Roaming
Globální správci služby Azure AD můžete povolit a zakázat Enterprise State Roaming v hello portál Azure classic.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-manage.png)

Globální správci můžou omezit skupiny zabezpečení toospecific nastavení synchronizace.

Globální správci můžete také zobrazit zprávu o stavu synchronizace zařízení na uživatele výběrem konkrétního uživatele v instanci Active Directory hello **uživatelé** seznamu a kliknete na **zařízení** kartě a vyberete zobrazení **Zařízení synchronizaci nastavení a dat aplikací enterprise**.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-device-sync-settings.png)

## <a name="data-retention"></a>Uchovávání dat
TooAzure data synchronizovat prostřednictvím Enterprise State Roaming bude uchována po neomezenou dobu, pokud se provádí operace ruční odstranění nebo hello data v určené toobe zastaralé. 

**Explicitní odstranění:** hello data budou odstraněna, jakmile správce explicitně požádá, aby data byla toobe odstranit nebo Azure správce odstraní uživatele nebo adresář.

* **Odstranění uživatele**: Při odstranění uživatele ve službě Azure AD hello uživatelský účet roaming dat bude označena pro odstranění a odstraní mezi 90 dní too180. 
* **Odstranění adresáře**: odstranění celého adresáře ve službě Azure AD je okamžitou operace. Všechna data hello nastavení související s které adresář bude označena pro odstranění a odstraní mezi 90 dní too180. 
* **Na žádost o odstranění**: Pokud hello Azure AD správce chce toomanually odstranit data nebo data nastavení konkrétního uživatele, dobrý den, Správce může soubor lístek s [podporu Azure](https://azure.microsoft.com/support/). 

**Odstranění dat zastaralých**: Data, která nepoužil, jeden rok ("hello dobu uchování"), budou považovány za zastaralé a budou odstraněny z Azure. Doba uchování Hello je toochange subjektu, ale nesmí být dřívější než 90 dní. zastaralá data Hello může být konkrétní sadu nastavení Windows/aplikace nebo všechna nastavení pro uživatele. Například:

* Pokud žádná zařízení přistupovat k kolekci konkrétní nastavení (například aplikace se odebere z hello zařízení nebo skupinu nastavení, jako je například "Motivu" vypnutá pro všechny uživatele zařízení), pak této kolekce se stane po dobu uchování hello zastaralé a může být odstranit. 
* Pokud uživatel má vypnuté nastavení synchronizace ve všech jejich zařízeních, pak žádná data hello nastavení budou mít přístup, a všechna data hello nastavení pro tohoto uživatele bude zastaralé a budou odstraněny po dobu uchování hello. 
* Pokud správce adresáře Azure AD hello vypne Enterprise State Roaming hello celý adresář, pak všechny uživatele v, že adresář se zastaví synchronizaci nastavení a všechna nastavení data pro všechny uživatele bude zastaralé a budou odstraněny po dobu uchování hello. 

**Odstranit obnovení dat**: hello zásad uchovávání dat se nedá konfigurovat. Jakmile hello data byla trvale odstraněna, nebude použitelná pro obnovení. Je ale důležité toonote, který hello nastavení data odstraní pouze z Azure, hello zařízení koncového uživatele. Pokud žádné zařízení později znovu připojí služby Enterprise State Roaming toohello, bude nastavení hello znovu synchronizovat a uložených v Azure.

## <a name="related-topics"></a>Související témata
* [Přehled Enterprise State Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
* [Nastavení a datový roaming – nejčastější dotazy](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Nastavení zásad a správu mobilních zařízení pro synchronizaci nastavení skupiny](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Odkaz nastavení roamingu Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [Řešení potíží](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
