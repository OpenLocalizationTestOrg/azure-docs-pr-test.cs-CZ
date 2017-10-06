---
title: "Služba synchronizace aaaAzure AD Connect funkcí a konfigurace | Microsoft Docs"
description: "Popisuje funkce straně služby pro službu synchronizace Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 213aab20-0a61-434a-9545-c4637628da81
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 7ad05c45bb6b5fd5deaa3466e2936b19d3d9eabb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-service-features"></a>Funkce služby Azure AD Connect sync
Funkce Hello synchronizace služby Azure AD Connect má dvě součásti:

* Hello místní součást s názvem **synchronizace Azure AD Connect**, označovaný také **synchronizační modul**.
* Služba Hello umístěný ve službě Azure AD, také známé jako **synchronizační služba Azure AD Connect**

Toto téma vysvětluje, jak hello následující funkce z hello **služba Azure AD Connect sync** práci a jak můžete nakonfigurovat pomocí prostředí Windows PowerShell.

Tato nastavení jsou konfigurována pomocí hello [Azure Active Directory modul pro prostředí Windows PowerShell](http://aka.ms/aadposh). Stáhněte a nainstalujte samostatně z Azure AD Connect. Hello rutiny popsané v tomto tématu se zavedly v hello [verze března 2016 (sestavení 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Pokud nemáte hello rutiny popsané v tomto tématu nebo nevedou hello stejné způsobit a pak zajistěte, aby spuštěním hello nejnovější verzi.

Konfigurace hello toosee v adresáři služby Azure AD, spusťte `Get-MsolDirSyncFeatures`.  
![Get-MsolDirSyncFeatures výsledek](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

Mnoho z těchto nastavení můžete změnit jenom přes Azure AD Connect.

Hello následující nastavení se dá nakonfigurovat `Set-MsolDirSyncFeature`:

| DirSyncFeature | Komentář |
| --- | --- |
| [EnableSoftMatchOnUpn](#userprincipalname-soft-match) |Umožňuje toojoin objekty v userPrincipalName přidání tooprimary SMTP adresu. |
| [SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) |Umožňuje hello synchronizační modul tooupdate hello atribut userPrincipalName spravované nebo licencovaných uživatelů (nefederovaných). |

Poté, co povolíte funkci nelze zakázat znovu.

> [!NOTE]
> Z 24 srpna 2016 hello funkce *duplicitní atribut odolnosti* je ve výchozím nastavení povolena pro nové Azure AD adresáře. Tato funkce bude také nasazuje a zapnuta adresáře vytvořené před tímto datem. Obdržíte e-mail s oznámením při adresáře je o tooget povolení této funkce.
> 
> 

Hello následující nastavení jsou nakonfigurovány přes Azure AD Connect a nelze změnit pomocí `Set-MsolDirSyncFeature`:

| DirSyncFeature | Komentář |
| --- | --- |
| DeviceWriteback |[Azure AD Connect: Povolení zpětného zápisu zařízení](active-directory-aadconnect-feature-device-writeback.md) |
| DirectoryExtensions |[Synchronizace Azure AD Connect: rozšíření adresáře](active-directory-aadconnectsync-feature-directory-extensions.md) |
| [DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) |Umožňuje atributu toobe do karantény, pokud je duplicitní jiný objekt, nikoli celý objekt hello selhání při exportu. |
| PasswordSync |[Implementace synchronizace hesel s synchronizace Azure AD Connect](active-directory-aadconnectsync-implement-password-synchronization.md) |
| UnifiedGroupWriteback |[Ve verzi Preview: Zpětný zápis skupin](active-directory-aadconnect-feature-preview.md#group-writeback) |
| UserWriteback |Není aktuálně podporováno. |

## <a name="duplicate-attribute-resiliency"></a>Odolnost proti chybám duplicitní atribut
Místo selhání tooprovision objekty s duplicitní názvy UPN nebo proxyAddresses, duplicitní atribut hello "v karanténě" a je mu přiřazená dočasnou hodnotou. Po vyřešení konfliktu hello hello dočasných UPN je změnit toohello správnou hodnotu automaticky. Další podrobnosti najdete v tématu [odolnosti Identity synchronizace a duplicitní atribut](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="userprincipalname-soft-match"></a>UserPrincipalName logicky shody
Pokud je tato funkce povolená, konfigurace soft-match je povolena pro hlavní název uživatele v přidání toohello [primární adresa SMTP](https://support.microsoft.com/kb/2641663), který je vždycky povolená. Konfigurace soft-match je použité toomatch stávající cloudu uživatele ve službě Azure AD s místními uživateli.

Pokud potřebujete toomatch místní účty AD s existující účty vytvořené v cloudu hello a nepoužíváte Exchange Online, bude tato funkce je užitečná. V tomto scénáři obecně nemáte atribut důvod tooset hello SMTP v cloudu hello.

Tato funkce je na ve výchozím nastavení pro nově vytvořený adresáře Azure AD. Se zobrazí, pokud je tato funkce povolená pro vás spuštěním:  

```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Pokud je tato funkce není povoleno pro váš adresář Azure AD, můžete ji povolit spuštěním:  

```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>Synchronizovat aktualizace userPrincipalName
V minulosti atribut UserPrincipalName toohello aktualizace pomocí služby synchronizace hello z místního je zablokovaný, pokud jsou splněny obě tyto podmínky:

* uživatel Hello je spravovaný (nefederovaných).
* Hello uživateli nebyla přiřazena licence.

Další podrobnosti najdete v tématu [uživatelského jména v Office 365, Azure nebo Intune se neshodují. hello místní UPN nebo alternativním přihlašovacím ID](https://support.microsoft.com/kb/2523192).

Povolení této funkce umožňuje hello synchronizační modul tooupdate hello userPrincipalName, pokud je změněné místní a používat synchronizaci hesla. Pokud používáte federace, tato funkce není podporována.

Tato funkce je na ve výchozím nastavení pro nově vytvořený adresáře Azure AD. Se zobrazí, pokud je tato funkce povolená pro vás spuštěním:  

```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Pokud je tato funkce není povoleno pro váš adresář Azure AD, můžete ji povolit spuštěním:  

```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Po povolení této funkce, zůstanou existující hodnoty userPrincipalName jako-je. Při příští změně hello userPrincipalName atribut místního hello normální rozdílová synchronizace na uživatele se aktualizuje hello UPN.  

## <a name="see-also"></a>Viz také
* [Synchronizace služby Azure AD Connect](active-directory-aadconnectsync-whatis.md)
* [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).

