---
title: "Azure Active Directory Domain Services: Povolení synchronizace hesel | Dokumentace Microsoftu"
description: "Začínáme se službou Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 5a32a0df-a3ca-4ebe-b980-91f58f8030fc
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/30/2017
ms.author: maheshu
ms.openlocfilehash: 8e073df9de2996f5ad159dda746881c014ea3d66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-password-synchronization-tooazure-active-directory-domain-services"></a><span data-ttu-id="36b29-103">Povolit tooAzure synchronizace hesla Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="36b29-103">Enable password synchronization tooAzure Active Directory Domain Services</span></span>
<span data-ttu-id="36b29-104">V předchozích úlohách jste povolili službu Azure Active Directory Domain Services pro tenanta služby Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="36b29-104">In preceding tasks, you enabled Azure Active Directory Domain Services for your Azure Active Directory (Azure AD) tenant.</span></span> <span data-ttu-id="36b29-105">Další úlohou Hello je, že požadované tooenable synchronizace hodnoty hash přihlašovacích údajů pro ověřování tooAzure NT LAN Manager (NTLM) a protokolu Kerberos AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="36b29-105">hello next task is tooenable synchronization of credential hashes required for NT LAN Manager (NTLM) and Kerberos authentication tooAzure AD Domain Services.</span></span> <span data-ttu-id="36b29-106">Po nastavení synchronizace přihlašovacích údajů, uživatelé můžou přihlásit toohello spravované doméně pomocí podnikových přihlašovacích.</span><span class="sxs-lookup"><span data-stu-id="36b29-106">After you've set up credential synchronization, users can sign in toohello managed domain with their corporate credentials.</span></span>

<span data-ttu-id="36b29-107">potřebný postup Hello se liší pro uživatele jen cloudové účty vs uživatelské účty, které jsou synchronizované z vašeho místního adresáře pomocí služby Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="36b29-107">hello steps involved are different for cloud-only user accounts vs user accounts that are synchronized from your on-premises directory using Azure AD Connect.</span></span>  <span data-ttu-id="36b29-108">Pokud se klientovi služby Azure AD má kombinaci cloudu pouze uživatele a uživatele z místní AD, je nutné tooperform oba kroky.</span><span class="sxs-lookup"><span data-stu-id="36b29-108">If your Azure AD tenant has a combination of cloud only users and users from your on-premises AD, you need tooperform both steps.</span></span>

<br>

> [!div class="op_single_selector"]
> * <span data-ttu-id="36b29-109">**Jenom pro cloud uživatelské účty**: [synchronizovat hesla pro cloudové uživatelské účty, tooyour spravované doméně](active-directory-ds-getting-started-password-sync.md)</span><span class="sxs-lookup"><span data-stu-id="36b29-109">**Cloud-only user accounts**: [Synchronize passwords for cloud-only user accounts tooyour managed domain](active-directory-ds-getting-started-password-sync.md)</span></span>
> * <span data-ttu-id="36b29-110">**Místní uživatelské účty**: [synchronizovat hesla uživatelských účtů, které jsou synchronizované z místní AD tooyour spravované domény](active-directory-ds-getting-started-password-sync-synced-tenant.md)</span><span class="sxs-lookup"><span data-stu-id="36b29-110">**On-premises user accounts**: [Synchronize passwords for user accounts synced from your on-premises AD tooyour managed domain](active-directory-ds-getting-started-password-sync-synced-tenant.md)</span></span>
>
>

<br>

## <a name="task-5-enable-password-synchronization-tooyour-managed-domain-for-cloud-only-user-accounts"></a><span data-ttu-id="36b29-111">Úloha 5: povolení tooyour synchronizace hesla pro cloudové uživatelské účty spravované domény.</span><span class="sxs-lookup"><span data-stu-id="36b29-111">Task 5: enable password synchronization tooyour managed domain for cloud-only user accounts</span></span>
<span data-ttu-id="36b29-112">Uživatelé tooauthenticate na hello spravované domény, Azure Active Directory Domain Services vyžaduje pověření hodnoty hash ve formátu, který je vhodný pro ověřování NTLM a Kerberos.</span><span class="sxs-lookup"><span data-stu-id="36b29-112">tooauthenticate users on hello managed domain, Azure Active Directory Domain Services needs credential hashes in a format that's suitable for NTLM and Kerberos authentication.</span></span> <span data-ttu-id="36b29-113">Azure AD nebudou generovat a ukládat hodnoty hash přihlašovacích údajů hello formát, který má požadované pro ověřování protokolem NTLM nebo Kerberos, dokud nepovolíte Azure Active Directory Domain Services pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="36b29-113">Azure AD does not generate or store credential hashes in hello format that's required for NTLM or Kerberos authentication, until you enable Azure Active Directory Domain Services for your tenant.</span></span> <span data-ttu-id="36b29-114">Ze zřejmých bezpečnostních důvodů Azure AD také neukládá přihlašovací hesla jako nešifrovaný text.</span><span class="sxs-lookup"><span data-stu-id="36b29-114">For obvious security reasons, Azure AD also does not store any password credentials in clear-text form.</span></span> <span data-ttu-id="36b29-115">Azure AD proto nemá tooautomatically způsob, jak vygenerovat tyto protokolu NTLM nebo Kerberos hodnoty hash přihlašovacích údajů podle existující přihlašovací údaje uživatelů.</span><span class="sxs-lookup"><span data-stu-id="36b29-115">Therefore, Azure AD does not have a way tooautomatically generate these NTLM or Kerberos credential hashes based on users' existing credentials.</span></span>

> [!NOTE]
> <span data-ttu-id="36b29-116">Pokud má vaše organizace výhradně cloudový uživatelské účty, uživatele, kteří potřebují toouse Azure Active Directory Domain Services, musíte změnit své heslo.</span><span class="sxs-lookup"><span data-stu-id="36b29-116">If your organization has cloud-only user accounts, users who need toouse Azure Active Directory Domain Services must change their passwords.</span></span> <span data-ttu-id="36b29-117">Jenom pro cloud uživatelský účet je účet, který byl vytvořen v adresáři služby Azure AD pomocí portálu Azure hello nebo rutin prostředí Azure AD PowerShell.</span><span class="sxs-lookup"><span data-stu-id="36b29-117">A cloud-only user account is an account that was created in your Azure AD directory using either hello Azure portal or Azure AD PowerShell cmdlets.</span></span> <span data-ttu-id="36b29-118">Takové uživatelské účty se nesynchronizují z místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="36b29-118">Such user accounts aren't synchronized from an on-premises directory.</span></span>
>
>

<span data-ttu-id="36b29-119">Tento proces změny hesla způsobí, že pověření hello hodnoty hash, které jsou vyžadované Azure Active Directory Domain Services pro Azure AD vygeneruje protokolu Kerberos a NTLM toobe ověřování.</span><span class="sxs-lookup"><span data-stu-id="36b29-119">This password change process causes hello credential hashes that are required by Azure Active Directory Domain Services for Kerberos and NTLM authentication toobe generated in Azure AD.</span></span> <span data-ttu-id="36b29-120">Můžete buď vyprší hello hesla pro všechny uživatele v hello klientů, kteří potřebují toouse Azure Active Directory Domain Services, nebo instruovat je toochange jejich hesla.</span><span class="sxs-lookup"><span data-stu-id="36b29-120">You can either expire hello passwords for all users in hello tenant who need toouse Azure Active Directory Domain Services or instruct them toochange their passwords.</span></span>

### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-user-account"></a><span data-ttu-id="36b29-121">Povolení generování hodnot hash přihlašovacích údajů protokolů NTLM a Kerberos u uživatelského účtu jenom cloudu</span><span class="sxs-lookup"><span data-stu-id="36b29-121">Enable NTLM and Kerberos credential hash generation for a cloud-only user account</span></span>
<span data-ttu-id="36b29-122">Zde jsou hello pokyny, které že budete potřebovat tooprovide uživatelům, změnit jejich hesla:</span><span class="sxs-lookup"><span data-stu-id="36b29-122">Here are hello instructions you need tooprovide users, so they can change their passwords:</span></span>

1. <span data-ttu-id="36b29-123">Přejděte toohello [přístupový Panel Azure AD](http://myapps.microsoft.com) stránku vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="36b29-123">Go toohello [Azure AD Access Panel](http://myapps.microsoft.com) page for your organization.</span></span>

    ![Spusťte přístupový panel Azure AD hello](./media/active-directory-domain-services-getting-started/access-panel.png)

2. <span data-ttu-id="36b29-125">V hello pravém horním rohu klikněte na název a vyberte **profil** nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="36b29-125">In hello top right corner, click on your name and select **Profile** from hello menu.</span></span>

    ![Výběr profilu](./media/active-directory-domain-services-getting-started/select-profile.png)

3. <span data-ttu-id="36b29-127">Na hello **profil** klikněte na **změnit heslo**.</span><span class="sxs-lookup"><span data-stu-id="36b29-127">On hello **Profile** page, click on **Change password**.</span></span>

    ![Kliknutí na Změnit heslo](./media/active-directory-domain-services-getting-started/user-change-password.png)

   > [!NOTE]
   > <span data-ttu-id="36b29-129">Pokud hello **změnit heslo** možnost není zobrazena v okně přístupový Panel hello, zajistěte, aby vaše organizace má nakonfigurovaný [Správa hesel ve službě Azure AD](../active-directory/active-directory-passwords-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="36b29-129">If hello **Change password** option is not displayed in hello Access Panel window, ensure that your organization has configured [password management in Azure AD](../active-directory/active-directory-passwords-getting-started.md).</span></span>
   >
   >
4. <span data-ttu-id="36b29-130">Na hello **změnit heslo** stránky, zadejte stávající (staré) heslo, zadejte nové heslo a potvrďte ho.</span><span class="sxs-lookup"><span data-stu-id="36b29-130">On hello **change password** page, type your existing (old) password, type a new password, and then confirm it.</span></span>

    ![Vytvoření virtuální sítě pro službu Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

5. <span data-ttu-id="36b29-132">Klikněte na **Odeslat**.</span><span class="sxs-lookup"><span data-stu-id="36b29-132">Click **submit**.</span></span>

<span data-ttu-id="36b29-133">Několik minut poté, co jste změnili heslo, nové heslo hello je použitelná v Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="36b29-133">A few minutes after you have changed your password, hello new password is usable in Azure Active Directory Domain Services.</span></span> <span data-ttu-id="36b29-134">Za několik minut (obvykle přibližně 20 minut), se můžete přihlásit toocomputers, které jsou připojené k toohello spravované doméně pomocí hello nově změnit heslo.</span><span class="sxs-lookup"><span data-stu-id="36b29-134">After a few more minutes (typically, about 20 minutes), you can sign in toocomputers that are joined toohello managed domain by using hello newly changed password.</span></span>

## <a name="related-content"></a><span data-ttu-id="36b29-135">Související obsah</span><span class="sxs-lookup"><span data-stu-id="36b29-135">Related Content</span></span>
* [<span data-ttu-id="36b29-136">Jak tooupdate své vlastní heslo</span><span class="sxs-lookup"><span data-stu-id="36b29-136">How tooupdate your own password</span></span>](../active-directory/active-directory-passwords-update-your-own-password.md)
* [<span data-ttu-id="36b29-137">Začínáme se správou hesel v Azure AD</span><span class="sxs-lookup"><span data-stu-id="36b29-137">Getting started with Password Management in Azure AD</span></span>](../active-directory/active-directory-passwords-getting-started.md)
* [<span data-ttu-id="36b29-138">Povolení synchronizace hesel klienta tooAzure Active Directory Domain Services u synchronizovaného Azure AD</span><span class="sxs-lookup"><span data-stu-id="36b29-138">Enable password synchronization tooAzure Active Directory Domain Services for a synced Azure AD tenant</span></span>](active-directory-ds-getting-started-password-sync-synced-tenant.md)
* [<span data-ttu-id="36b29-139">Správa spravované domény služby Azure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="36b29-139">Administer an Azure Active Directory Domain Services-managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="36b29-140">Připojení k Azure Active Directory Domain Services spravované domény tooan systému Windows virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="36b29-140">Join a Windows virtual machine tooan Azure Active Directory Domain Services-managed domain</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
* [<span data-ttu-id="36b29-141">Připojení k Red Hat Enterprise Linux virtuálního počítače tooan Azure Active Directory Domain Services spravované doméně</span><span class="sxs-lookup"><span data-stu-id="36b29-141">Join a Red Hat Enterprise Linux virtual machine tooan Azure Active Directory Domain Services-managed domain</span></span>](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
