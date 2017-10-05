---
title: "Azure AD Connect v Microsoft Cloudu Německo"
description: "Azure AD Connect integruje vaše místní adresáře do služby Azure Active Directory. To umožní poskytovat společnou identitu pro aplikace Office 365, Azure a SaaS integrované s Azure AD."
keywords: "Úvod do služby Azure AD Connect, přehled služby Azure AD Connect, co je Azure AD Connect, instalace služby Active Directory, Německo, Černý les"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 2bcb0caf-5d97-46cb-8c32-bda66cc22dad
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 4c55b116c0dc080ae316caca873a7b693caa793b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a><span data-ttu-id="ee6e1-105">Azure AD Connect v Microsoft Cloudu Německo – verze Public Preview</span><span class="sxs-lookup"><span data-stu-id="ee6e1-105">Azure AD Connect in Microsoft Cloud Germany - Public Preview</span></span>
## <a name="introduction"></a><span data-ttu-id="ee6e1-106">Úvod</span><span class="sxs-lookup"><span data-stu-id="ee6e1-106">Introduction</span></span>
<span data-ttu-id="ee6e1-107">Azure AD Connect poskytuje synchronizaci mezi vaší místní službou Active Directory a službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ee6e1-107">Azure AD Connect provides synchronization between your on-premises Active Directory and Azure Active Directory.</span></span>
<span data-ttu-id="ee6e1-108">V současné době je nutné, aby mnoho scénářů v [Microsoft Cloudu Německo](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) prováděl operátor.</span><span class="sxs-lookup"><span data-stu-id="ee6e1-108">Currently, many of the scenarios in [Microsoft Cloud Germany](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) must be done by the operator.</span></span> <span data-ttu-id="ee6e1-109">Při používání Microsoft Cloudu Německo je třeba brát v úvahu následující:</span><span class="sxs-lookup"><span data-stu-id="ee6e1-109">When using Microsoft Cloud Germany, you must be aware of the following:</span></span>

* <span data-ttu-id="ee6e1-110">Aby mohlo dojít k úspěšné synchronizaci, je nutné, aby na proxy serveru byly otevřené následující adresy URL:</span><span class="sxs-lookup"><span data-stu-id="ee6e1-110">The following URLs must be opened on a proxy server for synchronization to occur successfully:</span></span>
  
  * <span data-ttu-id="ee6e1-111">*.microsoftonline.de</span><span class="sxs-lookup"><span data-stu-id="ee6e1-111">*.microsoftonline.de</span></span>
  * <span data-ttu-id="ee6e1-112">*.windows.net</span><span class="sxs-lookup"><span data-stu-id="ee6e1-112">*.windows.net</span></span>
  * * <span data-ttu-id="ee6e1-113">Seznamy odvolaných certifikátů</span><span class="sxs-lookup"><span data-stu-id="ee6e1-113">Certificate Revocation Lists</span></span>
* <span data-ttu-id="ee6e1-114">Při přihlášení do adresáře služby Azure AD je nutné použít účet v doméně onmicrosoft.de.</span><span class="sxs-lookup"><span data-stu-id="ee6e1-114">When you sign in to your Azure AD directory, you must use an account in the onmicrosoft.de domain.</span></span>
* <span data-ttu-id="ee6e1-115">Následující funkce nejsou k dispozici:</span><span class="sxs-lookup"><span data-stu-id="ee6e1-115">The following features are not available:</span></span>
  * <span data-ttu-id="ee6e1-116">Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="ee6e1-116">Azure AD Connect Health</span></span>
  * <span data-ttu-id="ee6e1-117">Automatické aktualizace</span><span class="sxs-lookup"><span data-stu-id="ee6e1-117">Automatic updates</span></span>
 
## <a name="download"></a><span data-ttu-id="ee6e1-118">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="ee6e1-118">Download</span></span>
<span data-ttu-id="ee6e1-119">Službu Azure AD Connect lze stáhnout z okna Azure AD Connect v rámci portálu.</span><span class="sxs-lookup"><span data-stu-id="ee6e1-119">You can download Azure AD Connect from the Azure AD Connect blade within the portal.</span></span>  <span data-ttu-id="ee6e1-120">Pomocí pokynů níže vyhledejte okno Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="ee6e1-120">Use the instructions below to locate the Azure AD Connect blade.</span></span>

### <a name="the-azure-ad-connect-blade"></a><span data-ttu-id="ee6e1-121">Okno Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="ee6e1-121">The Azure AD Connect Blade</span></span>
<span data-ttu-id="ee6e1-122">Po přihlášení na webu Azure Portal postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="ee6e1-122">Once you have signed in to the Azure portal, do the following:</span></span>

1. <span data-ttu-id="ee6e1-123">Přejděte do Procházet.</span><span class="sxs-lookup"><span data-stu-id="ee6e1-123">Go to Browse</span></span>
2. <span data-ttu-id="ee6e1-124">Vyberte Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ee6e1-124">Select Azure Active Directory</span></span>
3. <span data-ttu-id="ee6e1-125">Poté vyberte Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="ee6e1-125">Then select Azure AD Connect</span></span>

<span data-ttu-id="ee6e1-126">Měli byste vidět následující:</span><span class="sxs-lookup"><span data-stu-id="ee6e1-126">You should see the following:</span></span>

![Okno Azure AD Connect](media/active-directory-aadconnect-germany/germany1.png)

<span data-ttu-id="ee6e1-128">Následující tabulka popisuje funkce zobrazené v okně.</span><span class="sxs-lookup"><span data-stu-id="ee6e1-128">The following table describes the features shown in the blade.</span></span>

| <span data-ttu-id="ee6e1-129">Název</span><span class="sxs-lookup"><span data-stu-id="ee6e1-129">Title</span></span> | <span data-ttu-id="ee6e1-130">Popis</span><span class="sxs-lookup"><span data-stu-id="ee6e1-130">Description</span></span> |
| --- | --- |
| <span data-ttu-id="ee6e1-131">STAV SYNCHRONIZACE</span><span class="sxs-lookup"><span data-stu-id="ee6e1-131">SYNC STATUS</span></span> |<span data-ttu-id="ee6e1-132">Uvádí, zda je synchronizace povolená nebo zakázaná.</span><span class="sxs-lookup"><span data-stu-id="ee6e1-132">Let's you know whether synchronization is enabled or disabled.</span></span> |
| <span data-ttu-id="ee6e1-133">POSLEDNÍ SYNCHRONIZACE</span><span class="sxs-lookup"><span data-stu-id="ee6e1-133">LAST SYNC</span></span> |<span data-ttu-id="ee6e1-134">Čas dokončení poslední úspěšné synchronizace.</span><span class="sxs-lookup"><span data-stu-id="ee6e1-134">The last time a successful sync completed.</span></span> |
| <span data-ttu-id="ee6e1-135">FEDEROVANÉ DOMÉNY</span><span class="sxs-lookup"><span data-stu-id="ee6e1-135">FEDERATED DOMAINS</span></span> |<span data-ttu-id="ee6e1-136">Zobrazuje počet aktuálně nakonfigurovaných federovaných domén.</span><span class="sxs-lookup"><span data-stu-id="ee6e1-136">Shows the number of federated domains currently configured.</span></span> |

## <a name="installation"></a><span data-ttu-id="ee6e1-137">Instalace</span><span class="sxs-lookup"><span data-stu-id="ee6e1-137">Installation</span></span>
<span data-ttu-id="ee6e1-138">Chcete-li nainstalovat službu Azure AD Connect, můžete využít dokumentaci, kterou najdete [tady](active-directory-aadconnect.md#install-azure-ad-connect).</span><span class="sxs-lookup"><span data-stu-id="ee6e1-138">To install Azure AD Connect, you can use the documentation [here](active-directory-aadconnect.md#install-azure-ad-connect).</span></span>

## <a name="advanced-features-and-additional-information"></a><span data-ttu-id="ee6e1-139">Pokročilé funkce a další informace</span><span class="sxs-lookup"><span data-stu-id="ee6e1-139">Advanced features and Additional Information</span></span>
<span data-ttu-id="ee6e1-140">Hledáte-li další informace a doprovodné materiály k vlastním nastavením nebo pokročilým konfiguracím, začněte s tématem [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="ee6e1-140">For additional information and guidance on custom settings or advanced configurations, start with [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>  <span data-ttu-id="ee6e1-141">Tato stránka poskytuje informace a odkazy na další doprovodné materiály.</span><span class="sxs-lookup"><span data-stu-id="ee6e1-141">This page provides information and links to additional guidance.</span></span>

