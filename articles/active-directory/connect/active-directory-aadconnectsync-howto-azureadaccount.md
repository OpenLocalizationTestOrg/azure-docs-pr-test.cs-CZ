---
title: "Synchronizace Azure AD Connect: Správa účtu služby Azure AD | Microsoft Docs"
description: "Toto téma popisuje postup obnovení účtu služby Azure AD."
services: active-directory
keywords: "AADSTS70002, AADSTS50054, jak resetovat heslo pro účet konektoru služby synchronizace Azure AD Connect"
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 8e9e8192ee4fcb636b5be91d2616acbc9120c8c0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a><span data-ttu-id="8a5d9-104">Synchronizace Azure AD Connect: Správa účtu služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8a5d9-104">Azure AD Connect sync: How to manage the Azure AD service account</span></span>
<span data-ttu-id="8a5d9-105">Účet služby používaný nástrojem konektor služby Azure AD by měla být služba volné.</span><span class="sxs-lookup"><span data-stu-id="8a5d9-105">The service account used by the Azure AD Connector is supposed to be service free.</span></span> <span data-ttu-id="8a5d9-106">Pokud budete potřebovat resetovat svoje přihlašovací údaje, pak toto téma je pro vás.</span><span class="sxs-lookup"><span data-stu-id="8a5d9-106">If you need to reset its credentials, then this topic is for you.</span></span> <span data-ttu-id="8a5d9-107">Například pokud má globálního správce tak, že resetovat heslo u účtu služby pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8a5d9-107">For example, if a Global Administrator has by mistake reset the password on the service account using PowerShell.</span></span>

## <a name="reset-the-credentials"></a><span data-ttu-id="8a5d9-108">Resetování přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="8a5d9-108">Reset the credentials</span></span>
<span data-ttu-id="8a5d9-109">Pokud účet služby definované v Azure AD Connector nemůže kontaktovat kvůli potížím s ověřováním Azure AD, můžete resetovat heslo.</span><span class="sxs-lookup"><span data-stu-id="8a5d9-109">If the service account defined on the Azure AD Connector cannot contact Azure AD due to authentication problems, the password can be reset.</span></span>

1. <span data-ttu-id="8a5d9-110">Přihlaste se k Azure AD Connect synchronizačního serveru a spusťte prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8a5d9-110">Sign in to the Azure AD Connect sync server and start PowerShell.</span></span>
2. <span data-ttu-id="8a5d9-111">Spusťte `Add-ADSyncAADServiceAccount`.</span><span class="sxs-lookup"><span data-stu-id="8a5d9-111">Run `Add-ADSyncAADServiceAccount`.</span></span>  
   <span data-ttu-id="8a5d9-112">![Addadsyncaadserviceaccount rutiny prostředí PowerShell](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)</span><span class="sxs-lookup"><span data-stu-id="8a5d9-112">![PowerShell cmdlet addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)</span></span>
3. <span data-ttu-id="8a5d9-113">Zadejte přihlašovací údaje Azure AD globálního správce.</span><span class="sxs-lookup"><span data-stu-id="8a5d9-113">Provide Azure AD Global admin credentials.</span></span>

<span data-ttu-id="8a5d9-114">Tato rutina resetuje heslo pro účet služby a aktualizovat ji ve službě Azure AD i v synchronizační modul.</span><span class="sxs-lookup"><span data-stu-id="8a5d9-114">This cmdlet resets the password for the service account and update it both in Azure AD and in the sync engine.</span></span>

## <a name="known-issues-these-steps-can-solve"></a><span data-ttu-id="8a5d9-115">Známé problémy těchto kroků můžete vyřešit.</span><span class="sxs-lookup"><span data-stu-id="8a5d9-115">Known issues these steps can solve</span></span>
<span data-ttu-id="8a5d9-116">Tato část se seznam chyb ohlášených zákazníky, které byly odstraněny přihlašovací údaje resetování na účet služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8a5d9-116">This section is a list of errors reported by customers that were fixed by a credentials reset on the Azure AD service account.</span></span>

- - -
<span data-ttu-id="8a5d9-117">Událost 6900</span><span class="sxs-lookup"><span data-stu-id="8a5d9-117">Event 6900</span></span>  
<span data-ttu-id="8a5d9-118">Serveru došlo k neočekávané chybě během zpracování oznámení o změně hesla:</span><span class="sxs-lookup"><span data-stu-id="8a5d9-118">The server encountered an unexpected error while processing a password change notification:</span></span>  
<span data-ttu-id="8a5d9-119">AADSTS70002: Chyba ověřuje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="8a5d9-119">AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="8a5d9-120">AADSTS50054: Staré heslo se používá k ověřování.</span><span class="sxs-lookup"><span data-stu-id="8a5d9-120">AADSTS50054: Old password is used for authentication.</span></span>

- - -
<span data-ttu-id="8a5d9-121">Událost 659</span><span class="sxs-lookup"><span data-stu-id="8a5d9-121">Event 659</span></span>  
<span data-ttu-id="8a5d9-122">Chyba při načítání konfiguraci synchronizace hesel zásad.</span><span class="sxs-lookup"><span data-stu-id="8a5d9-122">Error while retrieving password policy sync configuration.</span></span> <span data-ttu-id="8a5d9-123">Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:</span><span class="sxs-lookup"><span data-stu-id="8a5d9-123">Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:</span></span>  
<span data-ttu-id="8a5d9-124">AADSTS70002: Chyba ověřuje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="8a5d9-124">AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="8a5d9-125">AADSTS50054: Staré heslo se používá k ověřování.</span><span class="sxs-lookup"><span data-stu-id="8a5d9-125">AADSTS50054: Old password is used for authentication.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8a5d9-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8a5d9-126">Next steps</span></span>
<span data-ttu-id="8a5d9-127">**Témata s přehledem**</span><span class="sxs-lookup"><span data-stu-id="8a5d9-127">**Overview topics**</span></span>

* [<span data-ttu-id="8a5d9-128">Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace</span><span class="sxs-lookup"><span data-stu-id="8a5d9-128">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="8a5d9-129">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8a5d9-129">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)

