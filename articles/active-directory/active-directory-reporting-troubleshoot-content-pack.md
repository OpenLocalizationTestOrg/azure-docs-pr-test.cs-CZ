---
title: "Řešení potíží s aktivita služby Azure Active Directory protokolů chyb balíček obsahu | Microsoft Docs"
description: "Poskytuje seznam chybových zpráv toofix aktivita služby Azure Active Directory hello obsahu pack a postup je."
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
ms.openlocfilehash: 325de65ff1572a2f8f8319c0a52350bda03af3de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a><span data-ttu-id="c74f4-103">Řešení potíží s aktivita služby Azure Active Directory protokolů chyb balíček obsahu</span><span class="sxs-lookup"><span data-stu-id="c74f4-103">Troubleshooting Azure Active Directory Activity logs content pack errors</span></span> 


<span data-ttu-id="c74f4-104">Při práci s hello balíček obsahu Power BI pro Azure Active Directory Preview, je možné, že spustíte do hello následujícím chybám:</span><span class="sxs-lookup"><span data-stu-id="c74f4-104">When working with hello Power BI Content Pack for Azure Active Directory Preview, it is possible that you run into hello following errors:</span></span> 

- [<span data-ttu-id="c74f4-105">Aktualizace se nezdařila</span><span class="sxs-lookup"><span data-stu-id="c74f4-105">Refresh failed</span></span>](active-directory-reporting-troubleshoot-content-pack.md#refresh-failed) 
- [<span data-ttu-id="c74f4-106">Pověření ke zdroji dat neúspěšné tooupdate</span><span class="sxs-lookup"><span data-stu-id="c74f4-106">Failed tooupdate data source credentials</span></span>](active-directory-reporting-troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [<span data-ttu-id="c74f4-107">Import dat trvá příliš dlouho</span><span class="sxs-lookup"><span data-stu-id="c74f4-107">Importing of data is taking too long</span></span>](active-directory-reporting-troubleshoot-content-pack.md#importing-of-data-is-taking-too-long) 
 
<span data-ttu-id="c74f4-108">Toto téma poskytuje informace o možných příčinách hello a jak toofix tyto chyby.</span><span class="sxs-lookup"><span data-stu-id="c74f4-108">This topic provides you with information about hello possible causes and how toofix these errors.</span></span>
 
## <a name="refresh-failed"></a><span data-ttu-id="c74f4-109">Aktualizace se nezdařila</span><span class="sxs-lookup"><span data-stu-id="c74f4-109">Refresh failed</span></span> 
 
<span data-ttu-id="c74f4-110">**Jak se tato chyba prezentované**: e-mailu z Power BI nebo stav selhání v historii aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="c74f4-110">**How this error is surfaced**: Email from Power BI or failed status in hello refresh history.</span></span> 


| <span data-ttu-id="c74f4-111">Příčina</span><span class="sxs-lookup"><span data-stu-id="c74f4-111">Cause</span></span> | <span data-ttu-id="c74f4-112">Jak toofix</span><span class="sxs-lookup"><span data-stu-id="c74f4-112">How toofix</span></span> |
| ---   | ---        |
| <span data-ttu-id="c74f4-113">Aktualizujte selhání chyby může být způsobeno při hello přihlašovací údaje uživatelů hello připojení toohello balíček obsahu byly resetovat, ale neaktualizuje v nastavení připojení hello hello hello balíček obsahu.</span><span class="sxs-lookup"><span data-stu-id="c74f4-113">Refresh failure errors can be caused when hello credentials of hello users connecting toohello content pack have been reset but not updated in hello connection settings of hello of hello content pack.</span></span> | <span data-ttu-id="c74f4-114">V Power BI Najděte odpovídající toohello datovou sadu hello řídicí panel protokoly aktivita služby Azure Active Directory (protokoly aktivita služby Azure Active Directory), zvolte plán aktualizace a pak zadejte přihlašovací údaje Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c74f4-114">In Power BI, locate hello dataset corresponding toohello Azure Active Directory Activity logs dashboard (Azure Active Directory Activity logs), choose schedule refresh, and then enter your Azure AD credentials.</span></span> |
| <span data-ttu-id="c74f4-115">Obnovení může selhat z důvodu problémů toodata v hello základní balíček obsahu.</span><span class="sxs-lookup"><span data-stu-id="c74f4-115">A refresh can fail due toodata issues in hello underlying content pack.</span></span> | <span data-ttu-id="c74f4-116">Soubor lístek podpory.</span><span class="sxs-lookup"><span data-stu-id="c74f4-116">File a support ticket.</span></span> <span data-ttu-id="c74f4-117">Další podrobnosti najdete v tématu [jak tooget podporu pro Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span><span class="sxs-lookup"><span data-stu-id="c74f4-117">For more details, see [How tooget support for Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span></span>|
 
 
## <a name="failed-tooupdate-data-source-credentials"></a><span data-ttu-id="c74f4-118">Pověření ke zdroji dat neúspěšné tooupdate</span><span class="sxs-lookup"><span data-stu-id="c74f4-118">Failed tooupdate data source credentials</span></span> 
 
<span data-ttu-id="c74f4-119">**Jak se tato chyba prezentované**: V Power BI, když se připojujete toohello aktivita služby Azure Active Directory balíček obsahu protokoly (preview).</span><span class="sxs-lookup"><span data-stu-id="c74f4-119">**How this error is surfaced**: In Power BI, when you are connecting toohello Azure Active Directory Activity logs (preview) content pack.</span></span> 

| <span data-ttu-id="c74f4-120">Příčina</span><span class="sxs-lookup"><span data-stu-id="c74f4-120">Cause</span></span> | <span data-ttu-id="c74f4-121">Jak toofix</span><span class="sxs-lookup"><span data-stu-id="c74f4-121">How toofix</span></span> |
| ---   | ---        |
| <span data-ttu-id="c74f4-122">Hello uživatel připojuje, patří ani globální správce ani čtečku zabezpečení nebo správce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="c74f4-122">hello connecting user is neither a global admin nor a security reader or a security admin.</span></span> | <span data-ttu-id="c74f4-123">Použijte účet, který je globálním správcem nebo čtečku zabezpečení nebo zabezpečení Správce tooaccess hello balíčky obsahu.</span><span class="sxs-lookup"><span data-stu-id="c74f4-123">Use an account that is either a global admin or a security reader or a security admin tooaccess hello content packs.</span></span> |
| <span data-ttu-id="c74f4-124">Váš klient není klienta Premium nebo nemá alespoň jeden uživatel s licencí Premium souboru.</span><span class="sxs-lookup"><span data-stu-id="c74f4-124">Your tenant is not a Premium tenant or doesn't have at least one user with Premium license File.</span></span> | <span data-ttu-id="c74f4-125">Soubor lístek podpory.</span><span class="sxs-lookup"><span data-stu-id="c74f4-125">File a support ticket.</span></span> <span data-ttu-id="c74f4-126">Další podrobnosti najdete v tématu [jak tooget podporu pro Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span><span class="sxs-lookup"><span data-stu-id="c74f4-126">For more details, see [How tooget support for Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span></span>|
 

 

## <a name="importing-of-data-is-taking-too-long"></a><span data-ttu-id="c74f4-127">Import dat trvá příliš dlouho</span><span class="sxs-lookup"><span data-stu-id="c74f4-127">Importing of data is taking too long</span></span> 
 
<span data-ttu-id="c74f4-128">**Jak se tato chyba prezentované**: V Power BI, jakmile se připojíte svůj balíček obsahu proces importu dat hello spustí tooprepare řídicího panelu pro protokol aktivita služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c74f4-128">**How this error is surfaced**: In Power BI, once you have connected your content pack, hello data import process starts tooprepare your dashboard for Azure Active Directory Activity log.</span></span> <span data-ttu-id="c74f4-129">Zobrazí zpráva hello: "*importu dat...* "</span><span class="sxs-lookup"><span data-stu-id="c74f4-129">You see hello message: “*Importing data...*”</span></span>  

| <span data-ttu-id="c74f4-130">Příčina</span><span class="sxs-lookup"><span data-stu-id="c74f4-130">Cause</span></span> | <span data-ttu-id="c74f4-131">Jak toofix</span><span class="sxs-lookup"><span data-stu-id="c74f4-131">How toofix</span></span> |
| ---   | ---        |
| <span data-ttu-id="c74f4-132">V závislosti na velikosti hello klienta služby tento krok může trvat několik minut too30 minut.</span><span class="sxs-lookup"><span data-stu-id="c74f4-132">Depending on hello size of your tenant, this step could take anywhere from a few mins too30 minutes.</span></span> | <span data-ttu-id="c74f4-133">Právě se pacienta.</span><span class="sxs-lookup"><span data-stu-id="c74f4-133">Just be patient.</span></span> <span data-ttu-id="c74f4-134">Pokud zpráva hello nezmění tooshowing řídicího panelu v rámci hodiny, prosím soubor lístek podpory.</span><span class="sxs-lookup"><span data-stu-id="c74f4-134">If hello message does not change tooshowing your dashboard within an hour, please file a support ticket.</span></span> <span data-ttu-id="c74f4-135">Další podrobnosti najdete v tématu [jak tooget podporu pro Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span><span class="sxs-lookup"><span data-stu-id="c74f4-135">For more details, see [How tooget support for Azure Active Directory](active-directory-troubleshooting-support-howto.md).</span></span>|

## <a name="next-steps"></a><span data-ttu-id="c74f4-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c74f4-136">Next steps</span></span>

<span data-ttu-id="c74f4-137">Klikněte na tlačítko tooinstall hello balíček obsahu Power BI pro Azure Active Directory ve verzi Preview [zde](https://powerbi.microsoft.com/en-us/blog/azure-active-directory-meets-power-bi/).</span><span class="sxs-lookup"><span data-stu-id="c74f4-137">tooinstall hello Power BI Content Pack for Azure Active Directory Preview, click [here](https://powerbi.microsoft.com/en-us/blog/azure-active-directory-meets-power-bi/).</span></span>


