---
title: "Synchronizace Azure AD Connect: zapnutí koše AD | Microsoft Docs"
description: "Toto téma doporučuje použití hello funkce Koš služby AD s Azure AD Connect."
services: active-directory
keywords: "Koš služby AD, náhodným odstraněním, zdrojové ukotvení"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 2bb4827d677ccecfd8d2861f2a2fcf73b8cc2d95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a><span data-ttu-id="8931d-104">Synchronizace Azure AD Connect: zapnutí koše AD</span><span class="sxs-lookup"><span data-stu-id="8931d-104">Azure AD Connect sync: Enable AD recycle bin</span></span>
<span data-ttu-id="8931d-105">Doporučujeme, abyste povolili funkce Koš hello AD pro vaše Active Directory v místě, které jsou synchronizované tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="8931d-105">It is recommended that you enable hello AD Recycle Bin feature for your on-premises Active Directories, which are synchronized tooAzure AD.</span></span> 

<span data-ttu-id="8931d-106">Pokud jste omylem odstranili místní objekt uživatele AD a obnovení pomocí funkce hello, Azure AD obnoví hello odpovídající objekt uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8931d-106">If you accidentally deleted an on-premises AD user object and restore it using hello feature, Azure AD restores hello corresponding Azure AD user object.</span></span>  <span data-ttu-id="8931d-107">Informace o funkci Koš hello AD, najdete v části tooarticle [přehled scénářů pro obnovení odstranit objekty služby Active Directory](https://technet.microsoft.com/library/dd379542.aspx).</span><span class="sxs-lookup"><span data-stu-id="8931d-107">For information about hello AD Recycle Bin feature, refer tooarticle [Scenario Overview for Restoring Deleted Active Directory Objects](https://technet.microsoft.com/library/dd379542.aspx).</span></span>

## <a name="benefits-of-enabling-hello-ad-recycle-bin"></a><span data-ttu-id="8931d-108">Výhody povolení hello AD Koš.</span><span class="sxs-lookup"><span data-stu-id="8931d-108">Benefits of enabling hello AD recycle bin</span></span>
<span data-ttu-id="8931d-109">Tato funkce vám pomůže s obnovováním objekty uživatele Azure AD díky hello následující:</span><span class="sxs-lookup"><span data-stu-id="8931d-109">This feature helps with restoring Azure AD user objects by doing hello following:</span></span>

* <span data-ttu-id="8931d-110">Pokud jste omylem odstranili místní uživatele AD objektu hello odpovídající objekt uživatele Azure AD budou odstraněny v hello příštím synchronizačním cyklu.</span><span class="sxs-lookup"><span data-stu-id="8931d-110">If you accidentally deleted an on-premises AD user object, hello corresponding Azure AD user object will be deleted in hello next sync cycle.</span></span> <span data-ttu-id="8931d-111">Ve výchozím nastavení zajišťuje Azure AD objekt uživatele Azure AD hello odstranit dočasně odstraněné stavu po dobu 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="8931d-111">By default, Azure AD keeps hello deleted Azure AD user object in soft-deleted state for 30 days.</span></span>

* <span data-ttu-id="8931d-112">Pokud máte místní povolena funkce Koš služby AD, můžete obnovit hello odstranit místní objekt uživatele AD beze změny jeho hodnota zdrojové ukotvení.</span><span class="sxs-lookup"><span data-stu-id="8931d-112">If you have on-premises AD Recycle Bin feature enabled, you can restore hello deleted on-premises AD user object without changing its Source Anchor value.</span></span> <span data-ttu-id="8931d-113">Když hello obnovit místní objekt uživatele AD se synchronizují tooAzure AD, Azure AD bude obnovení hello odpovídající obnovitelné odstranění objektu uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8931d-113">When hello recovered on-premises AD user object is synchronized tooAzure AD, Azure AD will restore hello corresponding soft-deleted Azure AD user object.</span></span> <span data-ttu-id="8931d-114">Informace o atributu zdrojové ukotvení najdete v části tooarticle [Azure AD Connect: konceptech návrhu](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).</span><span class="sxs-lookup"><span data-stu-id="8931d-114">For information about Source Anchor attribute, refer tooarticle [Azure AD Connect: Design concepts](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).</span></span>

* <span data-ttu-id="8931d-115">Pokud nemáte místní recyklovat povolena funkce Koš služby AD, můžete být požadované toocreate AD uživatele objektu tooreplace hello odstraněného objektu.</span><span class="sxs-lookup"><span data-stu-id="8931d-115">If you do not have on-premises AD Recycle Bin feature enabled, you may be required toocreate an AD user object tooreplace hello deleted object.</span></span> <span data-ttu-id="8931d-116">Pokud službu synchronizace Azure AD Connect je nakonfigurované toouse generována AD atribut (například identifikátoru GUID) pro atribut zdrojové ukotvení hello, hello nově vytvořený objekt uživatele AD nebude mít hello stejnou hodnotu zdrojové ukotvení jako hello odstraněný objekt uživatele AD.</span><span class="sxs-lookup"><span data-stu-id="8931d-116">If Azure AD Connect Synchronization Service is configured toouse system-generated AD attribute (such as ObjectGuid) for hello Source Anchor attribute, hello newly created AD user object will not have hello same Source Anchor value as hello deleted AD user object.</span></span> <span data-ttu-id="8931d-117">Když hello nově vytvořený objekt uživatele AD synchronizované tooAzure AD, Azure AD vytvoří nový objekt uživatele Azure AD, místo obnovení hello obnovitelné odstranění objektu uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8931d-117">When hello newly created AD user object is synchronized tooAzure AD, Azure AD creates a new Azure AD user object instead of restoring hello soft-deleted Azure AD user object.</span></span>

> [!NOTE]
> <span data-ttu-id="8931d-118">Ve výchozím nastavení Azure AD udržuje odstraněných objektů uživatele Azure AD ve stavu odstraněno 30 dní, než se trvale odstraní.</span><span class="sxs-lookup"><span data-stu-id="8931d-118">By default, Azure AD keeps deleted Azure AD user objects in soft-deleted state for 30 days before they are permanently deleted.</span></span> <span data-ttu-id="8931d-119">Správci mohou však urychlit hello odstranění těchto objektů.</span><span class="sxs-lookup"><span data-stu-id="8931d-119">However, administrators can accelerate hello deletion of such objects.</span></span> <span data-ttu-id="8931d-120">Jakmile hello objekty jsou trvale odstraněny, se již nebude možné obnovit, i když místní, že je povolena funkce Koš služby AD.</span><span class="sxs-lookup"><span data-stu-id="8931d-120">Once hello objects are permanently deleted, they can no longer be recovered, even if on-premises AD Recycle Bin feature is enabled.</span></span>



## <a name="next-steps"></a><span data-ttu-id="8931d-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8931d-121">Next steps</span></span>
<span data-ttu-id="8931d-122">**Témata s přehledem**</span><span class="sxs-lookup"><span data-stu-id="8931d-122">**Overview topics**</span></span>

* [<span data-ttu-id="8931d-123">Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace</span><span class="sxs-lookup"><span data-stu-id="8931d-123">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)

* [<span data-ttu-id="8931d-124">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8931d-124">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
