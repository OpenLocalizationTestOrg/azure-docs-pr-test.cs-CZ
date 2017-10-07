---
title: "odběry aaaNo zjištěna chyba při akci toosign v tooAzure portálu nebo centra účtů Azure | Microsoft Docs"
description: "Poskytuje hello řešení problému, ve kterém žádné předplatné Nenalezeno, dojde k chybě při přihlášení tooAzure portálu nebo centra účtů Azure."
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: d1545298-99db-4941-8e97-f24a06bb7cb6
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: genli
ms.openlocfilehash: def4d4a1f883dd948fe8132f2d85abc4c23ae624
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="no-subscriptions-found-error-in-azure-portal-or-azure-account-center"></a><span data-ttu-id="a8d28-103">Žádná předplatná nalezena chyba na portálu Azure nebo centra účtů Azure</span><span class="sxs-lookup"><span data-stu-id="a8d28-103">No subscriptions found error in Azure portal or Azure account center</span></span>
<span data-ttu-id="a8d28-104">Může zobrazit chybová zpráva "Nebyla nalezena žádná předplatná", když zkusíte toosign v toohello [portál Azure](https://portal.azure.com/) nebo hello [centra účtů Azure](https://account.windowsazure.com/Subscriptions).</span><span class="sxs-lookup"><span data-stu-id="a8d28-104">You might receive a "No subscriptions found" error message when you try toosign in toohello [Azure portal](https://portal.azure.com/) or hello [Azure account center](https://account.windowsazure.com/Subscriptions).</span></span> <span data-ttu-id="a8d28-105">Tento článek poskytuje řešení tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="a8d28-105">This article provides a solution for this problem.</span></span>

## <a name="symptom"></a><span data-ttu-id="a8d28-106">Příznaky</span><span class="sxs-lookup"><span data-stu-id="a8d28-106">Symptom</span></span>

<span data-ttu-id="a8d28-107">Když se pokusíte toosign v toohello [portál Azure](https://portal.azure.com/) nebo hello [centra účtů Azure](https://account.windowsazure.com/Subscriptions), obdržíte hello následující chybová zpráva: "Nebyla nalezena žádná předplatná".</span><span class="sxs-lookup"><span data-stu-id="a8d28-107">When you try toosign in toohello [Azure portal](https://portal.azure.com/) or hello [Azure account center](https://account.windowsazure.com/Subscriptions), you receive hello following error message: "No subscriptions found".</span></span>

## <a name="cause"></a><span data-ttu-id="a8d28-108">Příčina</span><span class="sxs-lookup"><span data-stu-id="a8d28-108">Cause</span></span>

<span data-ttu-id="a8d28-109">K tomuto problému dochází, pokud váš účet nemá dostatečná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a8d28-109">This problem occurs if your account doesn’t have sufficient permissions.</span></span> 

## <a name="solution"></a><span data-ttu-id="a8d28-110">Řešení</span><span class="sxs-lookup"><span data-stu-id="a8d28-110">Solution</span></span>

<span data-ttu-id="a8d28-111">Ujistěte se, přihlásit se jako správce správné hello.</span><span class="sxs-lookup"><span data-stu-id="a8d28-111">Make sure that you log in as hello correct administrator.</span></span> <span data-ttu-id="a8d28-112">Účet správce můžete přistupovat pouze centra účtů hello.</span><span class="sxs-lookup"><span data-stu-id="a8d28-112">An Account Administrator can access only hello Account Center.</span></span> <span data-ttu-id="a8d28-113">Správci služby (SA) a Spolusprávci (CA) mají přístup oprávnění pouze toohello portál Azure nebo hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="a8d28-113">Service Administrators (SA) and Co-Administrators (CA) have access permission only toohello Azure portal or hello Azure classic portal.</span></span>

### <a name="scenario-1-error-message-is-received-in-hello-azure-portalhttpsportalazurecom"></a><span data-ttu-id="a8d28-114">Scénář 1: Chybová zpráva se získaly v hello [portálu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="a8d28-114">Scenario 1: Error message is received in hello [Azure portal](https://portal.azure.com)</span></span>

<span data-ttu-id="a8d28-115">toofix tohoto problému:</span><span class="sxs-lookup"><span data-stu-id="a8d28-115">toofix this issue:</span></span>

* <span data-ttu-id="a8d28-116">Ujistěte se, že hello správný, že adresáře Azure je vybrán kliknutím svůj účet na vpravo nahoře hello.</span><span class="sxs-lookup"><span data-stu-id="a8d28-116">Make sure that hello correct Azure directory is selected by clicking your account at hello top right.</span></span>

  ![Vyberte hello adresář v hello top napravo od hello portálu Azure](./media/billing-no-subscriptions-found/directory-switch.png)

* <span data-ttu-id="a8d28-118">Pokud hello právo Azure directory je vybrána, ale stále hello chybová zpráva, [byl váš účet přidán jako vlastníka](billing-add-change-azure-subscription-administrator.md).</span><span class="sxs-lookup"><span data-stu-id="a8d28-118">If hello right Azure directory is selected but you still receive hello error message, [have your account added as an Owner](billing-add-change-azure-subscription-administrator.md).</span></span>

### <a name="scenario-2-error-message-is-received-in-hello-azure-account-centerhttpsaccountwindowsazurecomsubscriptions"></a><span data-ttu-id="a8d28-119">Scénář 2: Chybová zpráva se získaly v hello [centra účtů Azure](https://account.windowsazure.com/Subscriptions)</span><span class="sxs-lookup"><span data-stu-id="a8d28-119">Scenario 2: Error message is received in hello [Azure account center](https://account.windowsazure.com/Subscriptions)</span></span>

<span data-ttu-id="a8d28-120">Zkontrolujte, zda text hello účet, který jste použili je hello správce účtu.</span><span class="sxs-lookup"><span data-stu-id="a8d28-120">Check whether hello account that you used is hello Account Administrator.</span></span> <span data-ttu-id="a8d28-121">je tooverify, kdo hello účet správce, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="a8d28-121">tooverify who hello Account Administrator is, follow these steps:</span></span>

1. <span data-ttu-id="a8d28-122">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a8d28-122">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="a8d28-123">V nabídce centra hello vyberte **předplatné**.</span><span class="sxs-lookup"><span data-stu-id="a8d28-123">On hello Hub menu, select **Subscription**.</span></span>
3. <span data-ttu-id="a8d28-124">Vyberte předplatné hello má toocheck a pak vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="a8d28-124">Select hello subscription that you want toocheck, and then select **Settings**.</span></span>
4. <span data-ttu-id="a8d28-125">Vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="a8d28-125">Select **Properties**.</span></span> <span data-ttu-id="a8d28-126">Správce účtu Hello hello předplatného se zobrazí v hello **správce účtu** pole.</span><span class="sxs-lookup"><span data-stu-id="a8d28-126">hello account administrator of hello subscription is displayed in hello **Account Admin** box.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="a8d28-127">Potřebujete pomoct?</span><span class="sxs-lookup"><span data-stu-id="a8d28-127">Need help?</span></span> <span data-ttu-id="a8d28-128">Obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="a8d28-128">Contact support.</span></span>
<span data-ttu-id="a8d28-129">Pokud stále potřebujete pomoc, [obraťte se na podporu](http://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409) tooget rychle vyřešit problém.</span><span class="sxs-lookup"><span data-stu-id="a8d28-129">If you still need help, [contact support](http://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409) tooget your issue resolved quickly.</span></span> 
