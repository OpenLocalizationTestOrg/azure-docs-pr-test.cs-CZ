---
title: "Azure AD Domain Services: Povolení synchronizace hesel | Dokumentace Microsoftu"
description: "Začínáme se službou Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 8731f2b2-661c-4f3d-adba-2c9e06344537
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/30/2017
ms.author: maheshu
ms.openlocfilehash: a7a6ee0f83d3d9bdaf236717efb39155a26934e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-password-synchronization-tooazure-active-directory-domain-services"></a>Povolit tooAzure synchronizace hesla Active Directory Domain Services
V předchozích úlohách jste povolili službu Azure Active Directory Domain Services pro tenanta služby Azure Active Directory (Azure AD). Další úlohou Hello je, že požadované tooenable synchronizace hodnoty hash přihlašovacích údajů pro ověřování tooAzure NT LAN Manager (NTLM) a protokolu Kerberos AD Domain Services. Po nastavení synchronizace přihlašovacích údajů, uživatelé můžou přihlásit toohello spravované doméně pomocí podnikových přihlašovacích.

potřebný postup Hello se liší pro uživatele jen cloudové účty vs uživatelské účty, které jsou synchronizované z vašeho místního adresáře pomocí služby Azure AD Connect. Pokud se klientovi služby Azure AD má kombinaci cloudu pouze uživatele a uživatele z místní AD, je nutné tooperform oba kroky.

<br>

> [!div class="op_single_selector"]
> * **Jenom pro cloud uživatelské účty**: [synchronizovat hesla pro cloudové uživatelské účty, tooyour spravované doméně](active-directory-ds-getting-started-password-sync.md)
> * **Místní uživatelské účty**: [synchronizovat hesla uživatelských účtů, které jsou synchronizované z místní AD tooyour spravované domény](active-directory-ds-getting-started-password-sync-synced-tenant.md)
>
>

<br>

## <a name="task-5-enable-password-synchronization-tooyour-managed-domain-for-user-accounts-synced-with-your-on-premises-ad"></a>Úloha 5: povolení heslo synchronizace tooyour spravované doméně pro uživatelské účty synchronizované s místní AD
Synchronizovat, A Azure AD klienta je nastaveno toosynchronize s místními adresáři vaší organizace pomocí služby Azure AD Connect. Ve výchozím nastavení Azure AD Connect nesynchronizuje NTLM a Kerberos pověření hodnoty hash tooAzure AD. toouse Azure AD Domain Services, musíte tooconfigure Azure AD Connect toosynchronize hodnoty hash přihlašovacích údajů požadované pro ověřování NTLM a Kerberos. Následující kroky Hello povolit synchronizaci hello požadované hodnoty hash přihlašovacích údajů z klientovi místní tooyour adresář Azure AD.

> [!NOTE]
> Pokud má vaše organizace uživatelské účty, které jsou synchronizované z vašeho místního adresáře, vaše musí povolit synchronizaci hodnot hash NTLM a Kerberos v pořadí toouse hello spravované domény. Synchronizovaná uživatelský účet je, že účet, který byl vytvořen v místním adresářem a je synchronizován tooyour klienta Azure AD pomocí služby Azure AD Connect.
>
>

### <a name="install-or-update-azure-ad-connect"></a>Instalace nebo aktualizace služby Azure AD Connect
Nainstalujte nejnovější doporučená hello verzi služby Azure AD Connect v doméně připojené k počítači. Pokud máte existující instanci instalace Azure AD Connect, je nutné tooupdate ho toouse hello nejnovější verzi služby Azure AD Connect. tooavoid známé problémům nebo chybám, které mohou již byly opraveny, ujistěte se, že vždy používáte nejnovější verzi Azure AD Connect hello.

**[Stažení služby Azure AD Connect](http://www.microsoft.com/download/details.aspx?id=47594)**

Doporučená verze: **1.1.553.0** – publikováno 27. června 2017.

> [!WARNING]
> Je nutné nainstalovat hello doporučujeme nejnovější verzi Azure AD Connect tooenable hello starší verze hesla přihlašovací údaje (požadováno pro ověřování NTLM a Kerberos) toosynchronize tooyour Azure AD klienta. Tato funkce není k dispozici v předchozích verzích služby Azure AD Connect nebo s hello starší verze nástroje DirSync.
>
>

Pokyny k instalaci Azure AD Connect jsou k dispozici v následujících hello článku – [Začínáme s Azure AD Connect](../active-directory/active-directory-aadconnect.md)

### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-tooazure-ad"></a>Povolit synchronizaci NTLM a Kerberos pověření hodnoty hash tooAzure AD
Spuštění hello následující skript prostředí PowerShell v každé doménové struktuře AD tooforce úplné synchronizace hesel a povolit všechny uživatele místní přihlašovací údaje hodnoty hash toosync tooyour Azure AD klienta. Tento skript umožňuje hodnoty hash přihlašovacích údajů hello požadované pro klienta Azure AD synchronizované tooyour toobe ověřování NTLM nebo Kerberos.

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

V závislosti na velikosti hello adresáře (počtu uživatelů, skupin atd), synchronizaci přihlašovacích údajů hashuje tooAzure AD trvá určitou dobu. Hello hesla bude možné použít ve spravované doméně služby Azure AD Domain Services hello krátce po hodnoty hash přihlašovacích údajů hello byly synchronizované tooAzure AD.

<br>

## <a name="related-content"></a>Související obsah
* [Povolit tooAAD synchronizace hesla Domain Services pro Azure výhradně cloudový adresář AD](active-directory-ds-getting-started-password-sync.md)
* [Správa spravované domény služby Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)
* [Připojení k systému Windows virtuálního počítače tooan Azure AD Domain Services spravované doméně](active-directory-ds-admin-guide-join-windows-vm.md)
* [Připojení k Red Hat Enterprise Linux virtuálního počítače tooan Azure AD Domain Services spravované doméně](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
