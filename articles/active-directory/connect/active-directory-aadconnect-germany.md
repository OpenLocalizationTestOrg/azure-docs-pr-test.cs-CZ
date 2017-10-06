---
title: "aaaAzure AD Connect v Německu Microsoft Cloud"
description: "Azure AD Connect integruje vaše místní adresáře do služby Azure Active Directory. To vám umožní tooprovide společnou identitu pro aplikace Office 365, Azure a SaaS integrované s Azure AD."
keywords: "Úvod tooAzure AD Connect, Azure AD Connect přehled, co je Azure AD Connect, nainstalovat službu active directory, Německo, černé doménové struktury"
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
ms.openlocfilehash: f32194fa6c365614f68e5d1ddcf0dac44d223292
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a><span data-ttu-id="6b777-105">Azure AD Connect v Microsoft Cloudu Německo – verze Public Preview</span><span class="sxs-lookup"><span data-stu-id="6b777-105">Azure AD Connect in Microsoft Cloud Germany - Public Preview</span></span>
## <a name="introduction"></a><span data-ttu-id="6b777-106">Úvod</span><span class="sxs-lookup"><span data-stu-id="6b777-106">Introduction</span></span>
<span data-ttu-id="6b777-107">Azure AD Connect poskytuje synchronizaci mezi vaší místní službou Active Directory a službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6b777-107">Azure AD Connect provides synchronization between your on-premises Active Directory and Azure Active Directory.</span></span>
<span data-ttu-id="6b777-108">V současné době mnoho scénářů hello v [Microsoft Cloud Německo](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) je třeba provést hello operátorem.</span><span class="sxs-lookup"><span data-stu-id="6b777-108">Currently, many of hello scenarios in [Microsoft Cloud Germany](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) must be done by hello operator.</span></span> <span data-ttu-id="6b777-109">Pokud používáte Microsoft Cloud Německo, musí mít přehled o hello následující:</span><span class="sxs-lookup"><span data-stu-id="6b777-109">When using Microsoft Cloud Germany, you must be aware of hello following:</span></span>

* <span data-ttu-id="6b777-110">Hello následující adresy URL musí být otevřen na proxy server pro synchronizaci toooccur úspěšně:</span><span class="sxs-lookup"><span data-stu-id="6b777-110">hello following URLs must be opened on a proxy server for synchronization toooccur successfully:</span></span>
  
  * <span data-ttu-id="6b777-111">*.microsoftonline.de</span><span class="sxs-lookup"><span data-stu-id="6b777-111">*.microsoftonline.de</span></span>
  * <span data-ttu-id="6b777-112">*.windows.net</span><span class="sxs-lookup"><span data-stu-id="6b777-112">*.windows.net</span></span>
  * * <span data-ttu-id="6b777-113">Seznamy odvolaných certifikátů</span><span class="sxs-lookup"><span data-stu-id="6b777-113">Certificate Revocation Lists</span></span>
* <span data-ttu-id="6b777-114">Při přihlášení tooyour adresář Azure AD, musíte použít účet v doméně onmicrosoft.de hello.</span><span class="sxs-lookup"><span data-stu-id="6b777-114">When you sign in tooyour Azure AD directory, you must use an account in hello onmicrosoft.de domain.</span></span>
* <span data-ttu-id="6b777-115">nejsou k dispozici Hello následující funkce:</span><span class="sxs-lookup"><span data-stu-id="6b777-115">hello following features are not available:</span></span>
  * <span data-ttu-id="6b777-116">Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="6b777-116">Azure AD Connect Health</span></span>
  * <span data-ttu-id="6b777-117">Automatické aktualizace</span><span class="sxs-lookup"><span data-stu-id="6b777-117">Automatic updates</span></span>
 
## <a name="download"></a><span data-ttu-id="6b777-118">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="6b777-118">Download</span></span>
<span data-ttu-id="6b777-119">Azure AD Connect si můžete stáhnout z okna Azure AD Connect hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="6b777-119">You can download Azure AD Connect from hello Azure AD Connect blade within hello portal.</span></span>  <span data-ttu-id="6b777-120">Použijte pokyny hello níže toolocate hello Azure AD Connect okno.</span><span class="sxs-lookup"><span data-stu-id="6b777-120">Use hello instructions below toolocate hello Azure AD Connect blade.</span></span>

### <a name="hello-azure-ad-connect-blade"></a><span data-ttu-id="6b777-121">Hello Azure AD Connect okno</span><span class="sxs-lookup"><span data-stu-id="6b777-121">hello Azure AD Connect Blade</span></span>
<span data-ttu-id="6b777-122">Po přihlášení toohello portál Azure hello následující:</span><span class="sxs-lookup"><span data-stu-id="6b777-122">Once you have signed in toohello Azure portal, do hello following:</span></span>

1. <span data-ttu-id="6b777-123">Přejděte tooBrowse</span><span class="sxs-lookup"><span data-stu-id="6b777-123">Go tooBrowse</span></span>
2. <span data-ttu-id="6b777-124">Vyberte Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6b777-124">Select Azure Active Directory</span></span>
3. <span data-ttu-id="6b777-125">Poté vyberte Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="6b777-125">Then select Azure AD Connect</span></span>

<span data-ttu-id="6b777-126">Měli byste vidět hello následující:</span><span class="sxs-lookup"><span data-stu-id="6b777-126">You should see hello following:</span></span>

![Okno Azure AD Connect](media/active-directory-aadconnect-germany/germany1.png)

<span data-ttu-id="6b777-128">Hello následující tabulka popisuje funkce hello zobrazí v okně hello.</span><span class="sxs-lookup"><span data-stu-id="6b777-128">hello following table describes hello features shown in hello blade.</span></span>

| <span data-ttu-id="6b777-129">Název</span><span class="sxs-lookup"><span data-stu-id="6b777-129">Title</span></span> | <span data-ttu-id="6b777-130">Popis</span><span class="sxs-lookup"><span data-stu-id="6b777-130">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6b777-131">STAV SYNCHRONIZACE</span><span class="sxs-lookup"><span data-stu-id="6b777-131">SYNC STATUS</span></span> |<span data-ttu-id="6b777-132">Uvádí, zda je synchronizace povolená nebo zakázaná.</span><span class="sxs-lookup"><span data-stu-id="6b777-132">Let's you know whether synchronization is enabled or disabled.</span></span> |
| <span data-ttu-id="6b777-133">POSLEDNÍ SYNCHRONIZACE</span><span class="sxs-lookup"><span data-stu-id="6b777-133">LAST SYNC</span></span> |<span data-ttu-id="6b777-134">Hello čas poslední úspěšné synchronizace byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="6b777-134">hello last time a successful sync completed.</span></span> |
| <span data-ttu-id="6b777-135">FEDEROVANÉ DOMÉNY</span><span class="sxs-lookup"><span data-stu-id="6b777-135">FEDERATED DOMAINS</span></span> |<span data-ttu-id="6b777-136">Zobrazuje počet hello federované domény aktuálně konfigurována.</span><span class="sxs-lookup"><span data-stu-id="6b777-136">Shows hello number of federated domains currently configured.</span></span> |

## <a name="installation"></a><span data-ttu-id="6b777-137">Instalace</span><span class="sxs-lookup"><span data-stu-id="6b777-137">Installation</span></span>
<span data-ttu-id="6b777-138">tooinstall Azure AD Connect, můžete použít dokumentaci hello [zde](active-directory-aadconnect.md#install-azure-ad-connect).</span><span class="sxs-lookup"><span data-stu-id="6b777-138">tooinstall Azure AD Connect, you can use hello documentation [here](active-directory-aadconnect.md#install-azure-ad-connect).</span></span>

## <a name="advanced-features-and-additional-information"></a><span data-ttu-id="6b777-139">Pokročilé funkce a další informace</span><span class="sxs-lookup"><span data-stu-id="6b777-139">Advanced features and Additional Information</span></span>
<span data-ttu-id="6b777-140">Hledáte-li další informace a doprovodné materiály k vlastním nastavením nebo pokročilým konfiguracím, začněte s tématem [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="6b777-140">For additional information and guidance on custom settings or advanced configurations, start with [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>  <span data-ttu-id="6b777-141">Tato stránka obsahuje informace a odkazy tooadditional pokyny.</span><span class="sxs-lookup"><span data-stu-id="6b777-141">This page provides information and links tooadditional guidance.</span></span>

