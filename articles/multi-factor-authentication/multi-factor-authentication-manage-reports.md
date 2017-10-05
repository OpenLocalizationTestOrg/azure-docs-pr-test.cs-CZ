---
title: "Přístup a použití sestav pro Azure MFA | Microsoft Docs"
description: "Popisuje jak používat funkci Azure Multi-Factor Authentication - sestavy."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtand
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: kgremban
ms.openlocfilehash: f76e726c6a67de4b0472c0e97f9e72c31c14c4f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="reports-in-azure-multi-factor-authentication"></a><span data-ttu-id="b80f2-103">Sestavy v Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="b80f2-103">Reports in Azure Multi-Factor Authentication</span></span>
<span data-ttu-id="b80f2-104">Azure Multi-Factor Authentication nabízí několik sestav, které mohou být využívána vám a vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="b80f2-104">Azure Multi-Factor Authentication provides several reports that can be used by you and your organization.</span></span> <span data-ttu-id="b80f2-105">Tyto sestavy lze přistupovat prostřednictvím portálu služby Multi-Factor Authentication Management Portal.</span><span class="sxs-lookup"><span data-stu-id="b80f2-105">These reports can be accessed through the Multi-Factor Authentication Management Portal.</span></span> <span data-ttu-id="b80f2-106">Následuje seznam dostupných sestav:</span><span class="sxs-lookup"><span data-stu-id="b80f2-106">The following is a list of the available reports:</span></span>

| <span data-ttu-id="b80f2-107">Sestava</span><span class="sxs-lookup"><span data-stu-id="b80f2-107">Report</span></span> | <span data-ttu-id="b80f2-108">Popis</span><span class="sxs-lookup"><span data-stu-id="b80f2-108">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="b80f2-109">Využití</span><span class="sxs-lookup"><span data-stu-id="b80f2-109">Usage</span></span> |<span data-ttu-id="b80f2-110">Využití zprávy obsahují informace na celkové využití: uživatelský souhrn a podrobnosti o uživateli.</span><span class="sxs-lookup"><span data-stu-id="b80f2-110">The usage reports display information on overall usage, user summary, and user details.</span></span> |
| <span data-ttu-id="b80f2-111">Stav serveru</span><span class="sxs-lookup"><span data-stu-id="b80f2-111">Server Status</span></span> |<span data-ttu-id="b80f2-112">Tato sestava zobrazí stav aplikace Multi-Factor Authentication Server, která je spojená s vaším účtem.</span><span class="sxs-lookup"><span data-stu-id="b80f2-112">This report displays the status of Multi-Factor Authentication Servers associated with your account.</span></span> |
| <span data-ttu-id="b80f2-113">Historie blokování uživatelů</span><span class="sxs-lookup"><span data-stu-id="b80f2-113">Blocked User History</span></span> |<span data-ttu-id="b80f2-114">Tyto sestavy zobrazit historii žádostí o blokování nebo odblokování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b80f2-114">These reports show the history of requests to block or unblock users.</span></span> |
| <span data-ttu-id="b80f2-115">Historie přeskočených uživatelů</span><span class="sxs-lookup"><span data-stu-id="b80f2-115">Bypassed User History</span></span> |<span data-ttu-id="b80f2-116">Zobrazí historii žádostí o obejití služby Multi-Factor Authentication pro telefonní číslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="b80f2-116">Shows the history of requests to bypass Multi-Factor Authentication for a user's phone number.</span></span> |
| <span data-ttu-id="b80f2-117">Upozornění na podvod</span><span class="sxs-lookup"><span data-stu-id="b80f2-117">Fraud Alert</span></span> |<span data-ttu-id="b80f2-118">Zobrazí historii upozornění na podvod odeslaných během období, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="b80f2-118">Shows a history of fraud alerts submitted during the date range you specified.</span></span> |
| <span data-ttu-id="b80f2-119">Ve frontě</span><span class="sxs-lookup"><span data-stu-id="b80f2-119">Queued</span></span> |<span data-ttu-id="b80f2-120">Zobrazí sestavy zařazené do fronty pro zpracování a jejich stav.</span><span class="sxs-lookup"><span data-stu-id="b80f2-120">Lists reports queued for processing and their status.</span></span> <span data-ttu-id="b80f2-121">Po dokončení sestavy je k dispozici odkaz na stažení nebo zobrazení sestavy.</span><span class="sxs-lookup"><span data-stu-id="b80f2-121">A link to download or view the report is provided when the report is complete.</span></span> |

## <a name="view-reports"></a><span data-ttu-id="b80f2-122">Zobrazení sestav</span><span class="sxs-lookup"><span data-stu-id="b80f2-122">View reports</span></span>
1. <span data-ttu-id="b80f2-123">Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="b80f2-123">Sign in to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="b80f2-124">Vlevo vyberte možnost Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b80f2-124">On the left, select Active Directory.</span></span>
3. <span data-ttu-id="b80f2-125">Proveďte jeden z těchto dvou možností, v závislosti na tom, zda používáte zprostředkovatelé ověřování:</span><span class="sxs-lookup"><span data-stu-id="b80f2-125">Follow one of these two options, depending on whether you use Auth Providers:</span></span>
   * <span data-ttu-id="b80f2-126">**Možnost 1**: klikněte na kartu zprostředkovatelé vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="b80f2-126">**Option 1**: Click the Multi-Factor Auth Providers tab.</span></span> <span data-ttu-id="b80f2-127">Vyberte poskytovatele MFA a klikněte na tlačítko **spravovat** tlačítko dole.</span><span class="sxs-lookup"><span data-stu-id="b80f2-127">Select your MFA provider and click the **Manage** button at the bottom.</span></span>
   * <span data-ttu-id="b80f2-128">**Možnost 2**: Vyberte adresář, přejděte na **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="b80f2-128">**Option 2**: Select your directory and go to the **Configure** tab.</span></span> <span data-ttu-id="b80f2-129">V části ověřování vícefaktorového ověřování vyberte **Spravovat nastavení služby**.</span><span class="sxs-lookup"><span data-stu-id="b80f2-129">Under the multi-factor authentication section, select **Manage service settings**.</span></span> <span data-ttu-id="b80f2-130">V dolní části stránky nastavení vícefaktorového ověřování služby kliknutím na Přejít do portálu odkaz.</span><span class="sxs-lookup"><span data-stu-id="b80f2-130">At the bottom of the MFA Service Settings page, click the Go to the portal link.</span></span>
4. <span data-ttu-id="b80f2-131">V portálu Azure Multi-Factor Authentication Management Portal, vyberte typ sestavy, které chcete z **zobrazit sestavu** část v levém navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="b80f2-131">In the Azure Multi-Factor Authentication Management Portal, select the type of report you want from the **View a Report** section in the left navigation.</span></span>

<span data-ttu-id="b80f2-132"><center>![Cloud](./media/multi-factor-authentication-manage-reports/report.png)</center></span><span class="sxs-lookup"><span data-stu-id="b80f2-132"><center>![Cloud](./media/multi-factor-authentication-manage-reports/report.png)</center></span></span>


<span data-ttu-id="b80f2-133">**Další zdroje**</span><span class="sxs-lookup"><span data-stu-id="b80f2-133">**Additional Resources**</span></span>

* [<span data-ttu-id="b80f2-134">Pro uživatele</span><span class="sxs-lookup"><span data-stu-id="b80f2-134">For Users</span></span>](end-user/multi-factor-authentication-end-user.md)
* [<span data-ttu-id="b80f2-135">Azure Multi-Factor Authentication na webu MSDN</span><span class="sxs-lookup"><span data-stu-id="b80f2-135">Azure Multi-Factor Authentication on MSDN</span></span>](https://msdn.microsoft.com/library/azure/dn249471.aspx)
