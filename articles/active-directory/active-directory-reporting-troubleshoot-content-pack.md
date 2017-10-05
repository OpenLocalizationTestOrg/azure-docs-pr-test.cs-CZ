---
title: "Řešení potíží s aktivita služby Azure Active Directory protokolů chyb balíček obsahu | Microsoft Docs"
description: "Poskytuje seznam chybových zpráv balíček obsahu aktivita služby Azure Active Directory a kroky k jejich řešení."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: c880e9eb6d48bd1e38075fbd867d3906ec67b547
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a><span data-ttu-id="09238-103">Řešení potíží s aktivita služby Azure Active Directory protokolů chyb balíček obsahu</span><span class="sxs-lookup"><span data-stu-id="09238-103">Troubleshooting Azure Active Directory Activity logs content pack errors</span></span> 


<span data-ttu-id="09238-104">Při práci s balíček obsahu Power BI pro Azure Active Directory Preview, je možné, že můžete spustit do následující chyby:</span><span class="sxs-lookup"><span data-stu-id="09238-104">When working with the Power BI Content Pack for Azure Active Directory Preview, it is possible that you run into the following errors:</span></span> 

- [<span data-ttu-id="09238-105">Aktualizace se nezdařila</span><span class="sxs-lookup"><span data-stu-id="09238-105">Refresh failed</span></span>](active-directory-reporting-troubleshoot-content-pack.md#refresh-failed) 
- [<span data-ttu-id="09238-106">Nepodařilo se aktualizovat pověření ke zdroji dat</span><span class="sxs-lookup"><span data-stu-id="09238-106">Failed to update data source credentials</span></span>](active-directory-reporting-troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [<span data-ttu-id="09238-107">Import dat trvá příliš dlouho</span><span class="sxs-lookup"><span data-stu-id="09238-107">Importing of data is taking too long</span></span>](active-directory-reporting-troubleshoot-content-pack.md#importing-of-data-is-taking-too-long) 
 
<span data-ttu-id="09238-108">Toto téma poskytuje informace o možných příčinách a jak tyto chyby opravit.</span><span class="sxs-lookup"><span data-stu-id="09238-108">This topic provides you with information about the possible causes and how to fix these errors.</span></span>
 
## <a name="refresh-failed"></a><span data-ttu-id="09238-109">Aktualizace se nezdařila</span><span class="sxs-lookup"><span data-stu-id="09238-109">Refresh failed</span></span> 
 
<span data-ttu-id="09238-110">**Jak se tato chyba prezentované**: e-mailu z Power BI nebo stav selhání v historii aktualizace.</span><span class="sxs-lookup"><span data-stu-id="09238-110">**How this error is surfaced**: Email from Power BI or failed status in the refresh history.</span></span> 


| <span data-ttu-id="09238-111">Příčina</span><span class="sxs-lookup"><span data-stu-id="09238-111">Cause</span></span> | <span data-ttu-id="09238-112">Informace o vyřešení</span><span class="sxs-lookup"><span data-stu-id="09238-112">How to fix</span></span> |
| ---   | ---        |
| <span data-ttu-id="09238-113">Aktualizujte selhání chyby může dojít, když přihlašovací údaje uživatelů, kteří se připojují k balíčku obsahu byly resetovat, ale neaktualizuje v nastavení připojení obsahu sady.</span><span class="sxs-lookup"><span data-stu-id="09238-113">Refresh failure errors can be caused when the credentials of the users connecting to the content pack have been reset but not updated in the connection settings of the of the content pack.</span></span> | <span data-ttu-id="09238-114">V Power BI vyhledejte sadu dat odpovídající řídicí panel protokoly aktivita služby Azure Active Directory (protokoly aktivita služby Azure Active Directory), vyberte plán aktualizace a pak zadejte přihlašovací údaje Azure AD.</span><span class="sxs-lookup"><span data-stu-id="09238-114">In Power BI, locate the dataset corresponding to the Azure Active Directory Activity logs dashboard (Azure Active Directory Activity logs), choose schedule refresh, and then enter your Azure AD credentials.</span></span> |
| <span data-ttu-id="09238-115">Obnovení může selhat z důvodu problémů dat v základní balíček obsahu.</span><span class="sxs-lookup"><span data-stu-id="09238-115">A refresh can fail due to data issues in the underlying content pack.</span></span> | <span data-ttu-id="09238-116">Soubor lístek podpory.</span><span class="sxs-lookup"><span data-stu-id="09238-116">File a support ticket.</span></span> <span data-ttu-id="09238-117">Další podrobnosti najdete v tématu [jak získat podporu pro Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span><span class="sxs-lookup"><span data-stu-id="09238-117">For more details, see [How to get support for Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span></span>|
 
 
## <a name="failed-to-update-data-source-credentials"></a><span data-ttu-id="09238-118">Nepodařilo se aktualizovat pověření ke zdroji dat</span><span class="sxs-lookup"><span data-stu-id="09238-118">Failed to update data source credentials</span></span> 
 
<span data-ttu-id="09238-119">**Jak se tato chyba prezentované**: V Power BI, když se připojujete k balíčku obsahu protokoly (preview) aktivita služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="09238-119">**How this error is surfaced**: In Power BI, when you are connecting to the Azure Active Directory Activity logs (preview) content pack.</span></span> 

| <span data-ttu-id="09238-120">Příčina</span><span class="sxs-lookup"><span data-stu-id="09238-120">Cause</span></span> | <span data-ttu-id="09238-121">Informace o vyřešení</span><span class="sxs-lookup"><span data-stu-id="09238-121">How to fix</span></span> |
| ---   | ---        |
| <span data-ttu-id="09238-122">Je připojující se uživatel je ani globální správce ani čtečku zabezpečení nebo správce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="09238-122">The connecting user is neither a global admin nor a security reader or a security admin.</span></span> | <span data-ttu-id="09238-123">Použijte účet, který je buď globální správce nebo čtečku zabezpečení nebo správce zabezpečení pro přístup k obsahu sady.</span><span class="sxs-lookup"><span data-stu-id="09238-123">Use an account that is either a global admin or a security reader or a security admin to access the content packs.</span></span> |
| <span data-ttu-id="09238-124">Váš klient není klienta Premium nebo nemá alespoň jeden uživatel s licencí Premium souboru.</span><span class="sxs-lookup"><span data-stu-id="09238-124">Your tenant is not a Premium tenant or doesn't have at least one user with Premium license File.</span></span> | <span data-ttu-id="09238-125">Soubor lístek podpory.</span><span class="sxs-lookup"><span data-stu-id="09238-125">File a support ticket.</span></span> <span data-ttu-id="09238-126">Další podrobnosti najdete v tématu [jak získat podporu pro Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span><span class="sxs-lookup"><span data-stu-id="09238-126">For more details, see [How to get support for Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span></span>|
 

 

## <a name="importing-of-data-is-taking-too-long"></a><span data-ttu-id="09238-127">Import dat trvá příliš dlouho</span><span class="sxs-lookup"><span data-stu-id="09238-127">Importing of data is taking too long</span></span> 
 
<span data-ttu-id="09238-128">**Jak se tato chyba prezentované**: V Power BI, jakmile se připojíte svůj balíček obsahu procesu importu dat spustí Příprava řídicího panelu pro protokol aktivita služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="09238-128">**How this error is surfaced**: In Power BI, once you have connected your content pack, the data import process starts to prepare your dashboard for Azure Active Directory Activity log.</span></span> <span data-ttu-id="09238-129">Zobrazí se zpráva: "*importu dat...* "</span><span class="sxs-lookup"><span data-stu-id="09238-129">You see the message: “*Importing data...*”</span></span>  

| <span data-ttu-id="09238-130">Příčina</span><span class="sxs-lookup"><span data-stu-id="09238-130">Cause</span></span> | <span data-ttu-id="09238-131">Informace o vyřešení</span><span class="sxs-lookup"><span data-stu-id="09238-131">How to fix</span></span> |
| ---   | ---        |
| <span data-ttu-id="09238-132">V závislosti na velikosti tenanta tento krok může trvat od několika minut až 30 minut.</span><span class="sxs-lookup"><span data-stu-id="09238-132">Depending on the size of your tenant, this step could take anywhere from a few mins to 30 minutes.</span></span> | <span data-ttu-id="09238-133">Právě se pacienta.</span><span class="sxs-lookup"><span data-stu-id="09238-133">Just be patient.</span></span> <span data-ttu-id="09238-134">Pokud zpráva nezmění na zobrazení řídicího panelu v rámci hodiny, prosím soubor lístek podpory.</span><span class="sxs-lookup"><span data-stu-id="09238-134">If the message does not change to showing your dashboard within an hour, please file a support ticket.</span></span> <span data-ttu-id="09238-135">Další podrobnosti najdete v tématu [jak získat podporu pro Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span><span class="sxs-lookup"><span data-stu-id="09238-135">For more details, see [How to get support for Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span></span>|

## <a name="next-steps"></a><span data-ttu-id="09238-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="09238-136">Next steps</span></span>

<span data-ttu-id="09238-137">Chcete-li nainstalovat balíček obsahu Power BI pro Azure Active Directory Preview, klikněte na tlačítko [zde](https://powerbi.microsoft.com/en-us/blog/azure-active-directory-meets-power-bi/).</span><span class="sxs-lookup"><span data-stu-id="09238-137">To install the Power BI Content Pack for Azure Active Directory Preview, click [here](https://powerbi.microsoft.com/en-us/blog/azure-active-directory-meets-power-bi/).</span></span>


