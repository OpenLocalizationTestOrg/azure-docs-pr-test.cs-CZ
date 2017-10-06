---
title: "Synchronizace Azure AD Connect: Změna hesla účtu hello AD DS | Microsoft Docs"
description: "Tento dokument téma popisuje, jak změnit tooupdate Azure AD Connect po hello heslo účtu hello služby AD DS."
services: active-directory
keywords: "AD DS účet, heslo a účet služby Active Directory"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 2707c9246446612f8d083ecde876b3b4fb2435ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="changing-hello-ad-ds-account-password"></a><span data-ttu-id="1e349-104">Změna hesla účtu hello AD DS</span><span class="sxs-lookup"><span data-stu-id="1e349-104">Changing hello AD DS account password</span></span>
<span data-ttu-id="1e349-105">Hello AD DS účtu odkazuje toohello uživatel účet používaný službou Azure AD Connect toocommunicate s místní služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1e349-105">hello AD DS account refers toohello user account used by Azure AD Connect toocommunicate with on-premises Active Directory.</span></span> <span data-ttu-id="1e349-106">Pokud změníte heslo hello hello účet služby AD DS, je třeba aktualizovat službu Azure AD Connect synchronizace s hello nové heslo.</span><span class="sxs-lookup"><span data-stu-id="1e349-106">If you change hello password of hello AD DS account, you must update Azure AD Connect Synchronization Service with hello new password.</span></span> <span data-ttu-id="1e349-107">Jinak hello synchronizace mohou už správně synchronizovat s hello místní služby Active Directory a můžete narazit hello následujícím chybám:</span><span class="sxs-lookup"><span data-stu-id="1e349-107">Otherwise, hello Synchronization can no longer synchronize correctly with hello on-premises Active Directory and you will encounter hello following errors:</span></span>

* <span data-ttu-id="1e349-108">V hello Synchronization Service Manager, všechny import nebo export operaci s místní AD selže s **bez start pověření** chyby.</span><span class="sxs-lookup"><span data-stu-id="1e349-108">In hello Synchronization Service Manager, any import or export operation with on-premises AD fails with **no-start-credentials** error.</span></span>

* <span data-ttu-id="1e349-109">V prohlížeči událostí systému Windows, protokolu událostí aplikace hello obsahuje chybu s **6000 ID události** a zpráva **'agenta pro správu hello "contoso.com" neúspěšné toorun, protože nebyly platné přihlašovací údaje hello'** .</span><span class="sxs-lookup"><span data-stu-id="1e349-109">Under Windows Event Viewer, hello application event log contains an error with **Event ID 6000** and message **'hello management agent "contoso.com" failed toorun because hello credentials were invalid'**.</span></span>


## <a name="how-tooupdate-hello-synchronization-service-with-new-password-for-ad-ds-account"></a><span data-ttu-id="1e349-110">Jak tooupdate hello synchronizační služba s nové heslo pro účet služby AD DS</span><span class="sxs-lookup"><span data-stu-id="1e349-110">How tooupdate hello Synchronization Service with new password for AD DS account</span></span>
<span data-ttu-id="1e349-111">tooupdate hello synchronizační služba s hello nové heslo:</span><span class="sxs-lookup"><span data-stu-id="1e349-111">tooupdate hello Synchronization Service with hello new password:</span></span>

1. <span data-ttu-id="1e349-112">Spusťte hello Synchronization Service Manager (START → synchronizace Service).</span><span class="sxs-lookup"><span data-stu-id="1e349-112">Start hello Synchronization Service Manager (START → Synchronization Service).</span></span>
</br><span data-ttu-id="1e349-113">![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span><span class="sxs-lookup"><span data-stu-id="1e349-113">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span></span>  

2. <span data-ttu-id="1e349-114">Přejděte toohello **konektory** kartě.</span><span class="sxs-lookup"><span data-stu-id="1e349-114">Go toohello **Connectors** tab.</span></span>

3. <span data-ttu-id="1e349-115">Vyberte hello **AD Connector.** odpovídající účet toohello AD DS, pro kterou byl změněn jeho heslo.</span><span class="sxs-lookup"><span data-stu-id="1e349-115">Select hello **AD Connector** that corresponds toohello AD DS account for which its password was changed.</span></span>

4. <span data-ttu-id="1e349-116">V části **akce**, vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="1e349-116">Under **Actions**, select **Properties**.</span></span>

5. <span data-ttu-id="1e349-117">V místním dialogovém okně hello, vyberte **připojit doménové struktury Directory tooActive**:</span><span class="sxs-lookup"><span data-stu-id="1e349-117">In hello pop-up dialog, select **Connect tooActive Directory Forest**:</span></span>

6. <span data-ttu-id="1e349-118">Zadejte nové heslo účtu hello služby AD DS hello v hello **heslo** textové pole.</span><span class="sxs-lookup"><span data-stu-id="1e349-118">Enter hello new password of hello AD DS account in hello **Password** textbox.</span></span>

7. <span data-ttu-id="1e349-119">Klikněte na tlačítko **OK** toosave hello nové heslo a automaticky otevíraný dialog zavřít hello.</span><span class="sxs-lookup"><span data-stu-id="1e349-119">Click **OK** toosave hello new password and close hello pop-up dialog.</span></span>

8. <span data-ttu-id="1e349-120">Restartování hello službu Azure AD Connect synchronizace v modulu snap-in Správce řízení služeb systému Windows.</span><span class="sxs-lookup"><span data-stu-id="1e349-120">Restart hello Azure AD Connect Synchronization Service under Windows Service Control Manager.</span></span> <span data-ttu-id="1e349-121">Toto je tooensure která žádné odkaz toohello staré heslo bude odebrána z mezipaměti paměti hello.</span><span class="sxs-lookup"><span data-stu-id="1e349-121">This is tooensure that any reference toohello old password is removed from hello memory cache.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e349-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1e349-122">Next steps</span></span>
<span data-ttu-id="1e349-123">**Témata s přehledem**</span><span class="sxs-lookup"><span data-stu-id="1e349-123">**Overview topics**</span></span>

* [<span data-ttu-id="1e349-124">Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace</span><span class="sxs-lookup"><span data-stu-id="1e349-124">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)

* [<span data-ttu-id="1e349-125">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1e349-125">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
