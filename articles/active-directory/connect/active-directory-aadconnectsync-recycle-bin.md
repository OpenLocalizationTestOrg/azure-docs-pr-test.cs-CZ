---
title: "Synchronizace Azure AD Connect: zapnutí koše AD | Microsoft Docs"
description: "Toto téma doporučuje použití funkce Koš služby AD s Azure AD Connect."
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
ms.openlocfilehash: eb455477547f3db8245cf3601576eba9c6fdc56f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a><span data-ttu-id="0762a-104">Synchronizace Azure AD Connect: zapnutí koše AD</span><span class="sxs-lookup"><span data-stu-id="0762a-104">Azure AD Connect sync: Enable AD recycle bin</span></span>
<span data-ttu-id="0762a-105">Doporučujeme, abyste povolili funkce Koš služby AD pro adresáře Active na místě, které jsou synchronizovány do Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0762a-105">It is recommended that you enable the AD Recycle Bin feature for your on-premises Active Directories, which are synchronized to Azure AD.</span></span> 

<span data-ttu-id="0762a-106">Pokud jste omylem odstranili místní objekt uživatele AD a obnovení je pomocí funkce Azure AD obnoví odpovídající objekt uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0762a-106">If you accidentally deleted an on-premises AD user object and restore it using the feature, Azure AD restores the corresponding Azure AD user object.</span></span>  <span data-ttu-id="0762a-107">Informace o funkci Koš služby AD, najdete v článku [přehled scénářů pro obnovení odstranit objekty služby Active Directory](https://technet.microsoft.com/library/dd379542.aspx).</span><span class="sxs-lookup"><span data-stu-id="0762a-107">For information about the AD Recycle Bin feature, refer to article [Scenario Overview for Restoring Deleted Active Directory Objects](https://technet.microsoft.com/library/dd379542.aspx).</span></span>

## <a name="benefits-of-enabling-the-ad-recycle-bin"></a><span data-ttu-id="0762a-108">Výhody zapnutí koše AD</span><span class="sxs-lookup"><span data-stu-id="0762a-108">Benefits of enabling the AD recycle bin</span></span>
<span data-ttu-id="0762a-109">Tato funkce vám pomůže s obnovováním objekty uživatele Azure AD následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0762a-109">This feature helps with restoring Azure AD user objects by doing the following:</span></span>

* <span data-ttu-id="0762a-110">Pokud jste omylem odstranili místní uživatele AD objektu, se odstraní odpovídající objekt uživatele Azure AD v příštím synchronizačním cyklu.</span><span class="sxs-lookup"><span data-stu-id="0762a-110">If you accidentally deleted an on-premises AD user object, the corresponding Azure AD user object will be deleted in the next sync cycle.</span></span> <span data-ttu-id="0762a-111">Ve výchozím nastavení, Azure AD udržuje na odstraněný objekt uživatele Azure AD ve stavu odstraněno po dobu 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="0762a-111">By default, Azure AD keeps the deleted Azure AD user object in soft-deleted state for 30 days.</span></span>

* <span data-ttu-id="0762a-112">Pokud máte místní recyklovat povolena funkce Koš služby AD, můžete obnovit odstraněné místní objekt uživatele AD beze změny jeho hodnota zdrojové ukotvení.</span><span class="sxs-lookup"><span data-stu-id="0762a-112">If you have on-premises AD Recycle Bin feature enabled, you can restore the deleted on-premises AD user object without changing its Source Anchor value.</span></span> <span data-ttu-id="0762a-113">Když obnovené místní objekt uživatele AD synchronizována do Azure AD, Azure AD bude objekt uživatele obnovení odpovídající dočasně odstraněné Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0762a-113">When the recovered on-premises AD user object is synchronized to Azure AD, Azure AD will restore the corresponding soft-deleted Azure AD user object.</span></span> <span data-ttu-id="0762a-114">Informace o atributu zdrojové ukotvení najdete v článku [Azure AD Connect: konceptech návrhu](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).</span><span class="sxs-lookup"><span data-stu-id="0762a-114">For information about Source Anchor attribute, refer to article [Azure AD Connect: Design concepts](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).</span></span>

* <span data-ttu-id="0762a-115">Pokud nemáte místní povolena funkce Koš služby AD, může být potřeba vytvořit objekt uživatele AD nahradit odstraněného objektu.</span><span class="sxs-lookup"><span data-stu-id="0762a-115">If you do not have on-premises AD Recycle Bin feature enabled, you may be required to create an AD user object to replace the deleted object.</span></span> <span data-ttu-id="0762a-116">Pokud služba Azure AD Connect synchronizace je nakonfigurovaný na použití generována atribut AD (například identifikátoru GUID) pro atribut zdrojové ukotvení, nově vytvořený objekt uživatele AD nebude mít stejnou hodnotu jako odstraněného objektu uživatele AD zdrojové ukotvení.</span><span class="sxs-lookup"><span data-stu-id="0762a-116">If Azure AD Connect Synchronization Service is configured to use system-generated AD attribute (such as ObjectGuid) for the Source Anchor attribute, the newly created AD user object will not have the same Source Anchor value as the deleted AD user object.</span></span> <span data-ttu-id="0762a-117">Když je nově vytvořený objekt uživatele AD synchronizovány do Azure AD, Azure AD vytvoří nový objekt uživatele Azure AD, místo obnovení obnovitelné odstranění objektu uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0762a-117">When the newly created AD user object is synchronized to Azure AD, Azure AD creates a new Azure AD user object instead of restoring the soft-deleted Azure AD user object.</span></span>

> [!NOTE]
> <span data-ttu-id="0762a-118">Ve výchozím nastavení Azure AD udržuje odstraněných objektů uživatele Azure AD ve stavu odstraněno 30 dní, než se trvale odstraní.</span><span class="sxs-lookup"><span data-stu-id="0762a-118">By default, Azure AD keeps deleted Azure AD user objects in soft-deleted state for 30 days before they are permanently deleted.</span></span> <span data-ttu-id="0762a-119">Správci mohou však urychlit odstranění tyto objekty.</span><span class="sxs-lookup"><span data-stu-id="0762a-119">However, administrators can accelerate the deletion of such objects.</span></span> <span data-ttu-id="0762a-120">Jakmile objekty jsou trvale odstraněny, se již nebude možné obnovit, i když místní, že je povolena funkce Koš služby AD.</span><span class="sxs-lookup"><span data-stu-id="0762a-120">Once the objects are permanently deleted, they can no longer be recovered, even if on-premises AD Recycle Bin feature is enabled.</span></span>



## <a name="next-steps"></a><span data-ttu-id="0762a-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0762a-121">Next steps</span></span>
<span data-ttu-id="0762a-122">**Témata s přehledem**</span><span class="sxs-lookup"><span data-stu-id="0762a-122">**Overview topics**</span></span>

* [<span data-ttu-id="0762a-123">Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace</span><span class="sxs-lookup"><span data-stu-id="0762a-123">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)

* [<span data-ttu-id="0762a-124">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0762a-124">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
