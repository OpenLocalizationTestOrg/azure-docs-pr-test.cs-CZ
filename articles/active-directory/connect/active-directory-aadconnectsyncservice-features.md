---
title: "Funkce služby synchronizace Azure AD Connect a konfigurace | Microsoft Docs"
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
ms.openlocfilehash: c2873510c280a2683c235cfdce3d2617c3b665cd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-service-features"></a>Funkce služby Azure AD Connect sync
Funkce synchronizace služby Azure AD Connect má dvě součásti:

* Místní součást s názvem **synchronizace Azure AD Connect**, označovaný také **synchronizační modul**.
* Službu umístěný ve službě Azure AD, také známé jako **synchronizační služba Azure AD Connect**

Toto téma vysvětluje, jak tyto funkce **služba Azure AD Connect sync** práci a jak můžete nakonfigurovat pomocí prostředí Windows PowerShell.

Tato nastavení jsou konfigurována pomocí [Azure Active Directory modul pro prostředí Windows PowerShell](http://aka.ms/aadposh). Stáhněte a nainstalujte samostatně z Azure AD Connect. Rutiny popsané v tomto tématu se zavedly v [verze března 2016 (sestavení 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Pokud nemáte rutiny popsané v tomto tématu nebo nevedou stejný výsledek, zkontrolujte, že spustíte na nejnovější verzi.

Chcete-li zobrazit konfigurace v adresáři služby Azure AD, spusťte `Get-MsolDirSyncFeatures`.  
![Get-MsolDirSyncFeatures výsledek](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

Mnoho z těchto nastavení můžete změnit jenom přes Azure AD Connect.

Následující nastavení se dá nakonfigurovat pomocí `Set-MsolDirSyncFeature`:

| DirSyncFeature | Komentář |
| --- | --- |
| [EnableSoftMatchOnUpn](#userprincipalname-soft-match) |Umožňuje objekty, které chcete připojit na userPrincipalName kromě primární adresa SMTP. |
| [SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) |Umožňuje synchronizační modul aktualizovat atribut userPrincipalName pro spravované nebo licencovaných uživatelů (nefederovaných). |

Poté, co povolíte funkci nelze zakázat znovu.

> [!NOTE]
> Z 24 srpna 2016 funkci *duplicitní atribut odolnosti* je ve výchozím nastavení povolena pro nové Azure AD adresáře. Tato funkce bude také nasazuje a zapnuta adresáře vytvořené před tímto datem. Obdržíte e-mail s oznámením při adresáře je získat povolení této funkce.
> 
> 

Následující nastavení jsou nakonfigurované přes Azure AD Connect a nelze změnit pomocí `Set-MsolDirSyncFeature`:

| DirSyncFeature | Komentář |
| --- | --- |
| DeviceWriteback |[Azure AD Connect: Povolení zpětného zápisu zařízení](active-directory-aadconnect-feature-device-writeback.md) |
| DirectoryExtensions |[Synchronizace Azure AD Connect: rozšíření adresáře](active-directory-aadconnectsync-feature-directory-extensions.md) |
| [DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) |Umožňuje atributu umístí do karantény, pokud je duplicitní jiného objektu, nikoli celý objekt selhání během exportu. |
| PasswordSync |[Implementace synchronizace hesel s synchronizace Azure AD Connect](active-directory-aadconnectsync-implement-password-synchronization.md) |
| UnifiedGroupWriteback |[Ve verzi Preview: Zpětný zápis skupin](active-directory-aadconnect-feature-preview.md#group-writeback) |
| UserWriteback |Není aktuálně podporováno. |

## <a name="duplicate-attribute-resiliency"></a>Odolnost proti chybám duplicitní atribut
Místo selhání zřídit objekty s duplicitní názvy UPN nebo proxyAddresses, duplicitní atribut "v karanténě" a je mu přiřazená dočasnou hodnotou. Po vyřešení konfliktu dočasné UPN se automaticky změní na správnou hodnotu. Další podrobnosti najdete v tématu [odolnosti Identity synchronizace a duplicitní atribut](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="userprincipalname-soft-match"></a>UserPrincipalName logicky shody
Pokud je tato funkce povolena, konfigurace soft-match je povolený pro UPN kromě [primární adresa SMTP](https://support.microsoft.com/kb/2641663), který je vždycky povolená. Konfigurace soft-match slouží k vyhledání existující cloudu uživatelů ve službě Azure AD s místních uživatelů.

Pokud potřebujete shodu místní účty AD s existující účty vytvořené v cloudu a nepoužíváte Exchange Online, bude tato funkce je užitečná. V tomto scénáři obecně nemáte důvod k nastavení SMTP atributu v cloudu.

Tato funkce je na ve výchozím nastavení pro nově vytvořený adresáře Azure AD. Se zobrazí, pokud je tato funkce povolená pro vás spuštěním:  

```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Pokud je tato funkce není povoleno pro váš adresář Azure AD, můžete ji povolit spuštěním:  

```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>Synchronizovat aktualizace userPrincipalName
V minulosti aktualizace pomocí služby synchronizace z místního atribut UserPrincipalName je zablokovaný, pokud jsou splněny obě tyto podmínky:

* Uživatel je spravovaný (nefederovaných).
* Uživateli nebyla přiřazena licence.

Další podrobnosti najdete v tématu [uživatelská jména v Office 365, Azure nebo Intune se neshodují místní UPN nebo alternativním přihlašovacím ID](https://support.microsoft.com/kb/2523192).

Povolení této funkce umožňuje synchronizační modul aktualizovat userPrincipalName, pokud je změněné místní a používat synchronizaci hesla. Pokud používáte federace, tato funkce není podporována.

Tato funkce je na ve výchozím nastavení pro nově vytvořený adresáře Azure AD. Se zobrazí, pokud je tato funkce povolená pro vás spuštěním:  

```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Pokud je tato funkce není povoleno pro váš adresář Azure AD, můžete ji povolit spuštěním:  

```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Po povolení této funkce, zůstanou existující hodnoty userPrincipalName jako-je. Při další změně userPrincipalName atribut místní normální rozdílová synchronizace na uživatele se aktualizuje název UPN.  

## <a name="see-also"></a>Viz také
* [Synchronizace služby Azure AD Connect](active-directory-aadconnectsync-whatis.md)
* [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).

