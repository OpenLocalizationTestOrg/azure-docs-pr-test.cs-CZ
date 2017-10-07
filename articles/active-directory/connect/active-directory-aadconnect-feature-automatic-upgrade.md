---
title: "Azure AD Connect: Automatický upgrade | Microsoft Docs"
description: "Toto téma popisuje hello integrovanou automatického upgradu funkcí v synchronizaci Azure AD Connect."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6b395e8f-fa3c-4e55-be54-392dd303c472
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 70d15eb3adf7758d8a43d278157daa504e059a98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-automatic-upgrade"></a>Azure AD Connect: Automatický upgrade
Tato funkce byla zavedená s sestavení 1.1.105.0 (vydané. února 2016).

## <a name="overview"></a>Přehled
Instalace služby Azure AD Connect je vždy až toodate má ještě nikdy nebylo snazší s hello **automatický upgrade** funkce. Tato funkce povolena ve výchozím nastavení pro expresní instalace a upgradu nástroje DirSync. Po vydání nové verze, instalace automaticky upgradován.

Ve výchozím nastavení pro následující hello je povolen automatický upgrade:

* Expresní nastavení instalace a upgrade nástroje DirSync.
* Pomocí SQL Express LocalDB, což je, co se expresní nastavení vždy použít. DirSync s SQL Express také použít LocalDB.
* Hello účet AD je hello výchozí MSOL_ účet vytvořený pomocí expresního nastavení a DirSync.
* Máte méně než 100 000 objektů v hello metaverse.

aktuální stav Hello automatický upgrade lze zobrazit pomocí rutiny prostředí PowerShell hello `Get-ADSyncAutoUpgrade`. Má hello následující stavy:

| Stav | Komentář |
| --- | --- |
| Povoleno |Je povolen automatický upgrade. |
| pozastaveno |Nastavte pouze systémem hello. Hello systému už není vhodné tooreceive automatický upgrade. |
| Zakázáno |Automatický upgrade je zakázána. |

Můžete volit mezi **povoleno** a **zakázané** s `Set-ADSyncAutoUpgrade`. Pouze hello systému měli nastavit stav hello **pozastaveno**.

Automatický upgrade používá Azure AD Connect Health pro upgrade infrastruktury hello. Pro automatické aktualizace toowork, ujistěte se, máte otevřený hello adresy URL v vašeho proxy serveru pro **Azure AD Connect Health** podle postupu uvedeného v [Office 365 adresy URL a rozsahy IP adres](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

Pokud hello **Synchronization Service Manager** uživatelského rozhraní je spuštěn na serveru hello pak hello upgradu je pozastaveno, dokud je uzavřený hello uživatelského rozhraní.

## <a name="troubleshooting"></a>Řešení potíží
Pokud instalace připojení není sám sebe aktualizovat podle očekávání, podle těchto kroků toofind se co může být nesprávný.

První není pravděpodobné, že hello automatického upgradu toobe pokus hello první den, kdy je vydání nové verze. Před pokusem o upgrade, nemusíte sleduje výstražné zařízení, pokud vaše instalace není upgradována okamžitě je úmyslné náhodnost.

Pokud si myslíte, něco není pravé, nejprve spusťte `Get-ADSyncAutoUpgrade` tooensure automatický upgrade je povoleno.

Ujistěte se, že máte otevřený hello požadované adresy URL v proxy serveru nebo brány firewall. Automatické aktualizace se pomocí Azure AD Connect Health, jak je popsáno v hello [přehled](#overview). Pokud používáte proxy server, ujistěte se, stavu, musí být nakonfigurované toouse [proxy serveru](../connect-health/active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). Taky testovat, hello [připojení stavu](../connect-health/active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) tooAzure AD.

S hello připojení tooAzure AD ověřit je čas toolook do hello protokoly událostí. Spusťte Prohlížeč událostí hello a hledat v hello **aplikace** protokolu událostí. Přidat filtr protokolu událostí pro zdroj hello **Azure AD Connect Upgrade** a rozsah id událostí hello **300-399**.  
![Filtr protokolu událostí pro automatický upgrade](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogfilter.png)  

Nyní můžete vidět hello protokoly událostí související se stavem hello pro automatický upgrade.  
![Filtr protokolu událostí pro automatický upgrade](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogresult.png)  

Kód výsledku Hello má předponu přehled stavu hello.

| Předpona kód výsledku | Popis |
| --- | --- |
| Úspěch |Hello instalace byla úspěšně upgradována. |
| UpgradeAborted |Dočasné podmínce zastavena hello upgradu. Bude akci opakovat a hello očekává se, že úspěšné později. |
| UpgradeNotSupported |systém Hello obsahuje konfigurace, která blokuje hello systém automaticky upgraduje. Pokud je změna stavu hello, ale hello očekává se, že hello systému musí být upgradovány ručně bude opakován toosee. |

Tady je seznam hello nejběžnější zprávy, které najít. Neobsahuje všechny seznam, ale zpráva výsledku hello by měla být zrušte s jaké potíže hello je.

| Zpráva výsledku | Popis |
| --- | --- |
| **UpgradeAborted** | |
| UpgradeAbortedCouldNotSetUpgradeMarker |Nelze zapsat toohello registru. |
| UpgradeAbortedInsufficientDatabasePermissions |Hello předdefinované skupiny administrators nemá oprávnění toohello databáze. Nejnovější verzi Azure AD Connect tooaddress toohello ručně upgradujte tento problém. |
| UpgradeAbortedInsufficientDiskSpace |Upgrade není nedostatek toosupport místa na disku. |
| UpgradeAbortedSecurityGroupsNotPresent |Nelze najít a vyřešte všechny skupiny zabezpečení používané hello synchronizační modul. |
| UpgradeAbortedServiceCanNotBeStarted |Hello služba NT **Microsoft Azure AD Sync** toostart se nezdařilo. |
| UpgradeAbortedServiceCanNotBeStopped |Hello služba NT **Microsoft Azure AD Sync** toostop se nezdařilo. |
| UpgradeAbortedServiceIsNotRunning |Hello služba NT **Microsoft Azure AD Sync** neběží. |
| UpgradeAbortedSyncCycleDisabled |Hello SyncCycle možnost v hello [Plánovač](active-directory-aadconnectsync-feature-scheduler.md) byla zakázána. |
| UpgradeAbortedSyncExeInUse |Hello [uživatelské rozhraní Správce služby synchronizace](active-directory-aadconnectsync-service-manager-ui.md) na hello serveru je otevřený. |
| UpgradeAbortedSyncOrConfigurationInProgress |Průvodce instalací Hello běží nebo synchronizace byla naplánována mimo hello plánovače. |
| **UpgradeNotSupported** | |
| UpgradeNotSupportedCustomizedSyncRules |Jste přidali vlastní konfigurace toohello vlastní pravidla. |
| UpgradeNotSupportedDeviceWritebackEnabled |Jste povolili hello [zpětný zápis zařízení](active-directory-aadconnect-feature-device-writeback.md) funkce. |
| UpgradeNotSupportedGroupWritebackEnabled |Jste povolili hello [zpětný zápis skupin](active-directory-aadconnect-feature-preview.md#group-writeback) funkce. |
| UpgradeNotSupportedInvalidPersistedState |instalace Hello není Expresní nastavení nebo upgradu nástroje DirSync. |
| UpgradeNotSupportedMetaverseSizeExceeeded |Máte více než 100 000 objektů v hello metaverse. |
| UpgradeNotSupportedMultiForestSetup |Připojujete toomore než jedné doménové struktuře. Expresní instalace pouze připojí tooone doménové struktury. |
| UpgradeNotSupportedNonLocalDbInstall |Nepoužíváte databázi SQL serveru Express LocalDB. |
| UpgradeNotSupportedNonMsolAccount |Hello [účet konektoru AD](active-directory-aadconnect-accounts-permissions.md#active-directory-account) není hello výchozí MSOL_ účet už. |
| UpgradeNotSupportedStagingModeEnabled |Hello server je nastaven toobe [pracovním režimu](active-directory-aadconnectsync-operations.md#staging-mode). |
| UpgradeNotSupportedUserWritebackEnabled |Jste povolili hello [zpětný zápis uživatelů](active-directory-aadconnect-feature-preview.md#user-writeback) funkce. |

## <a name="next-steps"></a>Další kroky
Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
