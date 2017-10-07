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
# <a name="enable-password-synchronization-tooazure-active-directory-domain-services"></a><span data-ttu-id="b6bd8-103">Povolit tooAzure synchronizace hesla Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="b6bd8-103">Enable password synchronization tooAzure Active Directory Domain Services</span></span>
<span data-ttu-id="b6bd8-104">V předchozích úlohách jste povolili službu Azure Active Directory Domain Services pro tenanta služby Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b6bd8-104">In preceding tasks, you enabled Azure Active Directory Domain Services for your Azure Active Directory (Azure AD) tenant.</span></span> <span data-ttu-id="b6bd8-105">Další úlohou Hello je, že požadované tooenable synchronizace hodnoty hash přihlašovacích údajů pro ověřování tooAzure NT LAN Manager (NTLM) a protokolu Kerberos AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-105">hello next task is tooenable synchronization of credential hashes required for NT LAN Manager (NTLM) and Kerberos authentication tooAzure AD Domain Services.</span></span> <span data-ttu-id="b6bd8-106">Po nastavení synchronizace přihlašovacích údajů, uživatelé můžou přihlásit toohello spravované doméně pomocí podnikových přihlašovacích.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-106">After you've set up credential synchronization, users can sign in toohello managed domain with their corporate credentials.</span></span>

<span data-ttu-id="b6bd8-107">potřebný postup Hello se liší pro uživatele jen cloudové účty vs uživatelské účty, které jsou synchronizované z vašeho místního adresáře pomocí služby Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-107">hello steps involved are different for cloud-only user accounts vs user accounts that are synchronized from your on-premises directory using Azure AD Connect.</span></span> <span data-ttu-id="b6bd8-108">Pokud se klientovi služby Azure AD má kombinaci cloudu pouze uživatele a uživatele z místní AD, je nutné tooperform oba kroky.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-108">If your Azure AD tenant has a combination of cloud only users and users from your on-premises AD, you need tooperform both steps.</span></span>

<br>

> [!div class="op_single_selector"]
> * <span data-ttu-id="b6bd8-109">**Jenom pro cloud uživatelské účty**: [synchronizovat hesla pro cloudové uživatelské účty, tooyour spravované doméně](active-directory-ds-getting-started-password-sync.md)</span><span class="sxs-lookup"><span data-stu-id="b6bd8-109">**Cloud-only user accounts**: [Synchronize passwords for cloud-only user accounts tooyour managed domain](active-directory-ds-getting-started-password-sync.md)</span></span>
> * <span data-ttu-id="b6bd8-110">**Místní uživatelské účty**: [synchronizovat hesla uživatelských účtů, které jsou synchronizované z místní AD tooyour spravované domény](active-directory-ds-getting-started-password-sync-synced-tenant.md)</span><span class="sxs-lookup"><span data-stu-id="b6bd8-110">**On-premises user accounts**: [Synchronize passwords for user accounts synced from your on-premises AD tooyour managed domain](active-directory-ds-getting-started-password-sync-synced-tenant.md)</span></span>
>
>

<br>

## <a name="task-5-enable-password-synchronization-tooyour-managed-domain-for-user-accounts-synced-with-your-on-premises-ad"></a><span data-ttu-id="b6bd8-111">Úloha 5: povolení heslo synchronizace tooyour spravované doméně pro uživatelské účty synchronizované s místní AD</span><span class="sxs-lookup"><span data-stu-id="b6bd8-111">Task 5: enable password synchronization tooyour managed domain for user accounts synced with your on-premises AD</span></span>
<span data-ttu-id="b6bd8-112">Synchronizovat, A Azure AD klienta je nastaveno toosynchronize s místními adresáři vaší organizace pomocí služby Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-112">A synced Azure AD tenant is set toosynchronize with your organization's on-premises directory using Azure AD Connect.</span></span> <span data-ttu-id="b6bd8-113">Ve výchozím nastavení Azure AD Connect nesynchronizuje NTLM a Kerberos pověření hodnoty hash tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-113">By default, Azure AD Connect does not synchronize NTLM and Kerberos credential hashes tooAzure AD.</span></span> <span data-ttu-id="b6bd8-114">toouse Azure AD Domain Services, musíte tooconfigure Azure AD Connect toosynchronize hodnoty hash přihlašovacích údajů požadované pro ověřování NTLM a Kerberos.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-114">toouse Azure AD Domain Services, you need tooconfigure Azure AD Connect toosynchronize credential hashes required for NTLM and Kerberos authentication.</span></span> <span data-ttu-id="b6bd8-115">Následující kroky Hello povolit synchronizaci hello požadované hodnoty hash přihlašovacích údajů z klientovi místní tooyour adresář Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-115">hello following steps enable synchronization of hello required credential hashes from your on-premises directory tooyour Azure AD tenant.</span></span>

> [!NOTE]
> <span data-ttu-id="b6bd8-116">Pokud má vaše organizace uživatelské účty, které jsou synchronizované z vašeho místního adresáře, vaše musí povolit synchronizaci hodnot hash NTLM a Kerberos v pořadí toouse hello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-116">If your organization has user accounts that are synchronized from your on-premises directory, your must enable synchronization of NTLM and Kerberos hashes in order toouse hello managed domain.</span></span> <span data-ttu-id="b6bd8-117">Synchronizovaná uživatelský účet je, že účet, který byl vytvořen v místním adresářem a je synchronizován tooyour klienta Azure AD pomocí služby Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-117">A synced user account is an account that was created in your on-premises directory and is synchronized tooyour Azure AD tenant using Azure AD Connect.</span></span>
>
>

### <a name="install-or-update-azure-ad-connect"></a><span data-ttu-id="b6bd8-118">Instalace nebo aktualizace služby Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="b6bd8-118">Install or update Azure AD Connect</span></span>
<span data-ttu-id="b6bd8-119">Nainstalujte nejnovější doporučená hello verzi služby Azure AD Connect v doméně připojené k počítači.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-119">Install hello latest recommended release of Azure AD Connect on a domain joined computer.</span></span> <span data-ttu-id="b6bd8-120">Pokud máte existující instanci instalace Azure AD Connect, je nutné tooupdate ho toouse hello nejnovější verzi služby Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-120">If you have an existing instance of Azure AD Connect setup, you need tooupdate it toouse hello latest version of Azure AD Connect.</span></span> <span data-ttu-id="b6bd8-121">tooavoid známé problémům nebo chybám, které mohou již byly opraveny, ujistěte se, že vždy používáte nejnovější verzi Azure AD Connect hello.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-121">tooavoid known issues/bugs that may have already been fixed, ensure you always use hello latest version of Azure AD Connect.</span></span>

<span data-ttu-id="b6bd8-122">**[Stažení služby Azure AD Connect](http://www.microsoft.com/download/details.aspx?id=47594)**</span><span class="sxs-lookup"><span data-stu-id="b6bd8-122">**[Download Azure AD Connect](http://www.microsoft.com/download/details.aspx?id=47594)**</span></span>

<span data-ttu-id="b6bd8-123">Doporučená verze: **1.1.553.0** – publikováno 27. června 2017.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-123">Recommended version: **1.1.553.0** - published on June 27, 2017.</span></span>

> [!WARNING]
> <span data-ttu-id="b6bd8-124">Je nutné nainstalovat hello doporučujeme nejnovější verzi Azure AD Connect tooenable hello starší verze hesla přihlašovací údaje (požadováno pro ověřování NTLM a Kerberos) toosynchronize tooyour Azure AD klienta.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-124">You MUST install hello latest recommended release of Azure AD Connect tooenable hello legacy password credentials (required for NTLM and Kerberos authentication) toosynchronize tooyour Azure AD tenant.</span></span> <span data-ttu-id="b6bd8-125">Tato funkce není k dispozici v předchozích verzích služby Azure AD Connect nebo s hello starší verze nástroje DirSync.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-125">This functionality is not available in prior releases of Azure AD Connect or with hello legacy DirSync tool.</span></span>
>
>

<span data-ttu-id="b6bd8-126">Pokyny k instalaci Azure AD Connect jsou k dispozici v následujících hello článku – [Začínáme s Azure AD Connect](../active-directory/active-directory-aadconnect.md)</span><span class="sxs-lookup"><span data-stu-id="b6bd8-126">Installation instructions for Azure AD Connect are available in hello following article - [Getting started with Azure AD Connect](../active-directory/active-directory-aadconnect.md)</span></span>

### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-tooazure-ad"></a><span data-ttu-id="b6bd8-127">Povolit synchronizaci NTLM a Kerberos pověření hodnoty hash tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="b6bd8-127">Enable synchronization of NTLM and Kerberos credential hashes tooAzure AD</span></span>
<span data-ttu-id="b6bd8-128">Spuštění hello následující skript prostředí PowerShell v každé doménové struktuře AD tooforce úplné synchronizace hesel a povolit všechny uživatele místní přihlašovací údaje hodnoty hash toosync tooyour Azure AD klienta.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-128">Execute hello following PowerShell script on each AD forest, tooforce full password synchronization, and enable all on-premises users’ credential hashes toosync tooyour Azure AD tenant.</span></span> <span data-ttu-id="b6bd8-129">Tento skript umožňuje hodnoty hash přihlašovacích údajů hello požadované pro klienta Azure AD synchronizované tooyour toobe ověřování NTLM nebo Kerberos.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-129">This script enables hello credential hashes required for NTLM/Kerberos authentication toobe synchronized tooyour Azure AD tenant.</span></span>

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

<span data-ttu-id="b6bd8-130">V závislosti na velikosti hello adresáře (počtu uživatelů, skupin atd), synchronizaci přihlašovacích údajů hashuje tooAzure AD trvá určitou dobu.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-130">Depending on hello size of your directory (number of users, groups etc.), synchronization of credential hashes tooAzure AD takes time.</span></span> <span data-ttu-id="b6bd8-131">Hello hesla bude možné použít ve spravované doméně služby Azure AD Domain Services hello krátce po hodnoty hash přihlašovacích údajů hello byly synchronizované tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="b6bd8-131">hello passwords will be usable on hello Azure AD Domain Services managed domain shortly after hello credential hashes have synchronized tooAzure AD.</span></span>

<br>

## <a name="related-content"></a><span data-ttu-id="b6bd8-132">Související obsah</span><span class="sxs-lookup"><span data-stu-id="b6bd8-132">Related Content</span></span>
* [<span data-ttu-id="b6bd8-133">Povolit tooAAD synchronizace hesla Domain Services pro Azure výhradně cloudový adresář AD</span><span class="sxs-lookup"><span data-stu-id="b6bd8-133">Enable password synchronization tooAAD Domain Services for a cloud-only Azure AD directory</span></span>](active-directory-ds-getting-started-password-sync.md)
* [<span data-ttu-id="b6bd8-134">Správa spravované domény služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="b6bd8-134">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="b6bd8-135">Připojení k systému Windows virtuálního počítače tooan Azure AD Domain Services spravované doméně</span><span class="sxs-lookup"><span data-stu-id="b6bd8-135">Join a Windows virtual machine tooan Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
* [<span data-ttu-id="b6bd8-136">Připojení k Red Hat Enterprise Linux virtuálního počítače tooan Azure AD Domain Services spravované doméně</span><span class="sxs-lookup"><span data-stu-id="b6bd8-136">Join a Red Hat Enterprise Linux virtual machine tooan Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
