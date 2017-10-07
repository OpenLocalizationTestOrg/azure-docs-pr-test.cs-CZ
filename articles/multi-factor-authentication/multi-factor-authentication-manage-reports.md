---
title: "sestavy využití a aaaAccess pro Azure MFA | Microsoft Docs"
description: Popisuje, jak toouse hello funkce Azure Multi-Factor Authentication - sestavy.
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
ms.openlocfilehash: ae7ccceca4968d7ec7cf0cb1cf9e041d9997c840
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reports-in-azure-multi-factor-authentication"></a><span data-ttu-id="8d5ac-103">Sestavy v Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="8d5ac-103">Reports in Azure Multi-Factor Authentication</span></span>
<span data-ttu-id="8d5ac-104">Azure Multi-Factor Authentication nabízí několik sestav, které mohou být využívána vám a vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="8d5ac-104">Azure Multi-Factor Authentication provides several reports that can be used by you and your organization.</span></span> <span data-ttu-id="8d5ac-105">Tyto sestavy je přístupná prostřednictvím hello Multi-Factor Authentication Management Portal.</span><span class="sxs-lookup"><span data-stu-id="8d5ac-105">These reports can be accessed through hello Multi-Factor Authentication Management Portal.</span></span> <span data-ttu-id="8d5ac-106">Hello tady je seznam dostupných sestav hello:</span><span class="sxs-lookup"><span data-stu-id="8d5ac-106">hello following is a list of hello available reports:</span></span>

| <span data-ttu-id="8d5ac-107">Sestava</span><span class="sxs-lookup"><span data-stu-id="8d5ac-107">Report</span></span> | <span data-ttu-id="8d5ac-108">Popis</span><span class="sxs-lookup"><span data-stu-id="8d5ac-108">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="8d5ac-109">Využití</span><span class="sxs-lookup"><span data-stu-id="8d5ac-109">Usage</span></span> |<span data-ttu-id="8d5ac-110">využití Hello zprávy obsahují informace na celkové využití: uživatelský souhrn a podrobnosti o uživateli.</span><span class="sxs-lookup"><span data-stu-id="8d5ac-110">hello usage reports display information on overall usage, user summary, and user details.</span></span> |
| <span data-ttu-id="8d5ac-111">Stav serveru</span><span class="sxs-lookup"><span data-stu-id="8d5ac-111">Server Status</span></span> |<span data-ttu-id="8d5ac-112">Tato sestava zobrazuje stav hello aplikace Multi-Factor Authentication Server, která je spojená s vaším účtem.</span><span class="sxs-lookup"><span data-stu-id="8d5ac-112">This report displays hello status of Multi-Factor Authentication Servers associated with your account.</span></span> |
| <span data-ttu-id="8d5ac-113">Historie blokování uživatelů</span><span class="sxs-lookup"><span data-stu-id="8d5ac-113">Blocked User History</span></span> |<span data-ttu-id="8d5ac-114">Tyto sestavy zobrazit historii hello tooblock požadavky nebo odblokovat uživatele.</span><span class="sxs-lookup"><span data-stu-id="8d5ac-114">These reports show hello history of requests tooblock or unblock users.</span></span> |
| <span data-ttu-id="8d5ac-115">Historie přeskočených uživatelů</span><span class="sxs-lookup"><span data-stu-id="8d5ac-115">Bypassed User History</span></span> |<span data-ttu-id="8d5ac-116">Zobrazí historii hello toobypass žádosti o službu Multi-Factor Authentication pro telefonní číslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="8d5ac-116">Shows hello history of requests toobypass Multi-Factor Authentication for a user's phone number.</span></span> |
| <span data-ttu-id="8d5ac-117">Upozornění na podvod</span><span class="sxs-lookup"><span data-stu-id="8d5ac-117">Fraud Alert</span></span> |<span data-ttu-id="8d5ac-118">Zobrazí historii upozornění na podvod odeslaných během hello rozsah, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="8d5ac-118">Shows a history of fraud alerts submitted during hello date range you specified.</span></span> |
| <span data-ttu-id="8d5ac-119">Ve frontě</span><span class="sxs-lookup"><span data-stu-id="8d5ac-119">Queued</span></span> |<span data-ttu-id="8d5ac-120">Zobrazí sestavy zařazené do fronty pro zpracování a jejich stav.</span><span class="sxs-lookup"><span data-stu-id="8d5ac-120">Lists reports queued for processing and their status.</span></span> <span data-ttu-id="8d5ac-121">Po dokončení sestavy hello je vypracována sestava odkaz toodownload nebo zobrazení hello.</span><span class="sxs-lookup"><span data-stu-id="8d5ac-121">A link toodownload or view hello report is provided when hello report is complete.</span></span> |

## <a name="view-reports"></a><span data-ttu-id="8d5ac-122">Zobrazení sestav</span><span class="sxs-lookup"><span data-stu-id="8d5ac-122">View reports</span></span>
1. <span data-ttu-id="8d5ac-123">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="8d5ac-123">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="8d5ac-124">Na levé straně hello vyberte služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8d5ac-124">On hello left, select Active Directory.</span></span>
3. <span data-ttu-id="8d5ac-125">Proveďte jeden z těchto dvou možností, v závislosti na tom, zda používáte zprostředkovatelé ověřování:</span><span class="sxs-lookup"><span data-stu-id="8d5ac-125">Follow one of these two options, depending on whether you use Auth Providers:</span></span>
   * <span data-ttu-id="8d5ac-126">**Možnost 1**: klikněte na kartu hello zprostředkovatelé vícefaktorového ověřování. Vyberte poskytovatele MFA a klikněte na tlačítko hello **spravovat** tlačítko dole v hello.</span><span class="sxs-lookup"><span data-stu-id="8d5ac-126">**Option 1**: Click hello Multi-Factor Auth Providers tab. Select your MFA provider and click hello **Manage** button at hello bottom.</span></span>
   * <span data-ttu-id="8d5ac-127">**Možnost 2**: vyberte váš adresář a přejděte toohello **konfigurace** kartě. V části služby Multi-Factor authentication hello vyberte **spravovat nastavení služby**.</span><span class="sxs-lookup"><span data-stu-id="8d5ac-127">**Option 2**: Select your directory and go toohello **Configure** tab. Under hello multi-factor authentication section, select **Manage service settings**.</span></span> <span data-ttu-id="8d5ac-128">V hello dolní části stránky hello nastavení vícefaktorového ověřování služby klikněte na položku hello přejděte toohello portálu odkaz.</span><span class="sxs-lookup"><span data-stu-id="8d5ac-128">At hello bottom of hello MFA Service Settings page, click hello Go toohello portal link.</span></span>
4. <span data-ttu-id="8d5ac-129">V hello Azure Multi-Factor Authentication Management Portal, vyberte typ hello sestavy, které chcete z hello **zobrazit sestavu** část v levé navigační hello.</span><span class="sxs-lookup"><span data-stu-id="8d5ac-129">In hello Azure Multi-Factor Authentication Management Portal, select hello type of report you want from hello **View a Report** section in hello left navigation.</span></span>

<span data-ttu-id="8d5ac-130"><center>![Cloud](./media/multi-factor-authentication-manage-reports/report.png)</center></span><span class="sxs-lookup"><span data-stu-id="8d5ac-130"><center>![Cloud](./media/multi-factor-authentication-manage-reports/report.png)</center></span></span>


<span data-ttu-id="8d5ac-131">**Další zdroje**</span><span class="sxs-lookup"><span data-stu-id="8d5ac-131">**Additional Resources**</span></span>

* [<span data-ttu-id="8d5ac-132">Pro uživatele</span><span class="sxs-lookup"><span data-stu-id="8d5ac-132">For Users</span></span>](end-user/multi-factor-authentication-end-user.md)
* [<span data-ttu-id="8d5ac-133">Azure Multi-Factor Authentication na webu MSDN</span><span class="sxs-lookup"><span data-stu-id="8d5ac-133">Azure Multi-Factor Authentication on MSDN</span></span>](https://msdn.microsoft.com/library/azure/dn249471.aspx)
