---
title: "Synchronizace Azure AD Connect: jak účet služby toomanage hello Azure AD | Microsoft Docs"
description: "Toto téma popisuje, jak účet služby toorestore hello Azure AD."
services: active-directory
keywords: "AADSTS70002, AADSTS50054, jak tooreset hello heslo pro hello synchronizace Azure AD Connect účet služby konektoru"
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
ms.openlocfilehash: e563518eae173de42a1d40bb5a76e63f29f9da42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-how-toomanage-hello-azure-ad-service-account"></a><span data-ttu-id="f9a25-104">Synchronizace Azure AD Connect: jak účet služby toomanage hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="f9a25-104">Azure AD Connect sync: How toomanage hello Azure AD service account</span></span>
<span data-ttu-id="f9a25-105">účet služby Hello používá hello konektoru služby Azure AD by měla služba toobe volné.</span><span class="sxs-lookup"><span data-stu-id="f9a25-105">hello service account used by hello Azure AD Connector is supposed toobe service free.</span></span> <span data-ttu-id="f9a25-106">Pokud potřebujete tooreset svoje přihlašovací údaje, toto téma je pro vás.</span><span class="sxs-lookup"><span data-stu-id="f9a25-106">If you need tooreset its credentials, then this topic is for you.</span></span> <span data-ttu-id="f9a25-107">Například pokud má globálního správce tak, že resetovat hello heslo u účtu služby hello pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f9a25-107">For example, if a Global Administrator has by mistake reset hello password on hello service account using PowerShell.</span></span>

## <a name="reset-hello-credentials"></a><span data-ttu-id="f9a25-108">Resetovat přihlašovací údaje hello</span><span class="sxs-lookup"><span data-stu-id="f9a25-108">Reset hello credentials</span></span>
<span data-ttu-id="f9a25-109">Pokud účet služby hello definované na hello Azure AD Connector nemůže kontaktovat Azure AD kvůli tooauthentication problémy, můžete resetovat heslo hello.</span><span class="sxs-lookup"><span data-stu-id="f9a25-109">If hello service account defined on hello Azure AD Connector cannot contact Azure AD due tooauthentication problems, hello password can be reset.</span></span>

1. <span data-ttu-id="f9a25-110">Přihlaste se toohello server synchronizace Azure AD Connect a spusťte prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f9a25-110">Sign in toohello Azure AD Connect sync server and start PowerShell.</span></span>
2. <span data-ttu-id="f9a25-111">Spusťte `Add-ADSyncAADServiceAccount`.</span><span class="sxs-lookup"><span data-stu-id="f9a25-111">Run `Add-ADSyncAADServiceAccount`.</span></span>  
   <span data-ttu-id="f9a25-112">![Addadsyncaadserviceaccount rutiny prostředí PowerShell](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)</span><span class="sxs-lookup"><span data-stu-id="f9a25-112">![PowerShell cmdlet addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)</span></span>
3. <span data-ttu-id="f9a25-113">Zadejte přihlašovací údaje Azure AD globálního správce.</span><span class="sxs-lookup"><span data-stu-id="f9a25-113">Provide Azure AD Global admin credentials.</span></span>

<span data-ttu-id="f9a25-114">Tato rutina vytvoří hello heslo pro účet služby hello a aktualizovat ji ve službě Azure AD i v hello synchronizační modul.</span><span class="sxs-lookup"><span data-stu-id="f9a25-114">This cmdlet resets hello password for hello service account and update it both in Azure AD and in hello sync engine.</span></span>

## <a name="known-issues-these-steps-can-solve"></a><span data-ttu-id="f9a25-115">Známé problémy těchto kroků můžete vyřešit.</span><span class="sxs-lookup"><span data-stu-id="f9a25-115">Known issues these steps can solve</span></span>
<span data-ttu-id="f9a25-116">Tato část se seznam chyb ohlášených zákazníky, které byly odstraněny přihlašovací údaje resetování na hello účet služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f9a25-116">This section is a list of errors reported by customers that were fixed by a credentials reset on hello Azure AD service account.</span></span>

- - -
<span data-ttu-id="f9a25-117">Událost 6900</span><span class="sxs-lookup"><span data-stu-id="f9a25-117">Event 6900</span></span>  
<span data-ttu-id="f9a25-118">Hello server došlo k neočekávané chybě během zpracování oznámení o změně hesla:</span><span class="sxs-lookup"><span data-stu-id="f9a25-118">hello server encountered an unexpected error while processing a password change notification:</span></span>  
<span data-ttu-id="f9a25-119">AADSTS70002: Chyba ověřuje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="f9a25-119">AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="f9a25-120">AADSTS50054: Staré heslo se používá k ověřování.</span><span class="sxs-lookup"><span data-stu-id="f9a25-120">AADSTS50054: Old password is used for authentication.</span></span>

- - -
<span data-ttu-id="f9a25-121">Událost 659</span><span class="sxs-lookup"><span data-stu-id="f9a25-121">Event 659</span></span>  
<span data-ttu-id="f9a25-122">Chyba při načítání konfiguraci synchronizace hesel zásad.</span><span class="sxs-lookup"><span data-stu-id="f9a25-122">Error while retrieving password policy sync configuration.</span></span> <span data-ttu-id="f9a25-123">Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:</span><span class="sxs-lookup"><span data-stu-id="f9a25-123">Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:</span></span>  
<span data-ttu-id="f9a25-124">AADSTS70002: Chyba ověřuje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="f9a25-124">AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="f9a25-125">AADSTS50054: Staré heslo se používá k ověřování.</span><span class="sxs-lookup"><span data-stu-id="f9a25-125">AADSTS50054: Old password is used for authentication.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f9a25-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f9a25-126">Next steps</span></span>
<span data-ttu-id="f9a25-127">**Témata s přehledem**</span><span class="sxs-lookup"><span data-stu-id="f9a25-127">**Overview topics**</span></span>

* [<span data-ttu-id="f9a25-128">Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace</span><span class="sxs-lookup"><span data-stu-id="f9a25-128">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="f9a25-129">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f9a25-129">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)

