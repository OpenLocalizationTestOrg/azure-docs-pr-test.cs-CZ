---
title: "Žádné předplatné Nenalezeno Chyba při pokusu o přihlášení k portálu Azure nebo centra účtů Azure | Microsoft Docs"
description: "Poskytuje řešení problému, ve kterém žádné předplatné Nenalezeno, dojde k chybě při přihlášení k portálu Azure nebo centra účtů Azure."
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
ms.openlocfilehash: a4ce9b219c05f8469379c2aac5241fcfffd16033
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="no-subscriptions-found-error-in-azure-portal-or-azure-account-center"></a><span data-ttu-id="e4abd-103">Žádná předplatná nalezena chyba na portálu Azure nebo centra účtů Azure</span><span class="sxs-lookup"><span data-stu-id="e4abd-103">No subscriptions found error in Azure portal or Azure account center</span></span>
<span data-ttu-id="e4abd-104">Může zobrazit chybová zpráva "Nebyla nalezena žádná předplatná", při pokusu o přihlášení k aplikaci [portál Azure](https://portal.azure.com/) nebo [centra účtů Azure](https://account.windowsazure.com/Subscriptions).</span><span class="sxs-lookup"><span data-stu-id="e4abd-104">You might receive a "No subscriptions found" error message when you try to sign in to the [Azure portal](https://portal.azure.com/) or the [Azure account center](https://account.windowsazure.com/Subscriptions).</span></span> <span data-ttu-id="e4abd-105">Tento článek poskytuje řešení tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="e4abd-105">This article provides a solution for this problem.</span></span>

## <a name="symptom"></a><span data-ttu-id="e4abd-106">Příznaky</span><span class="sxs-lookup"><span data-stu-id="e4abd-106">Symptom</span></span>

<span data-ttu-id="e4abd-107">Při pokusu o přihlášení k aplikaci [portál Azure](https://portal.azure.com/) nebo [centra účtů Azure](https://account.windowsazure.com/Subscriptions), zobrazí se následující chybová zpráva: "Nebyla nalezena žádná předplatná".</span><span class="sxs-lookup"><span data-stu-id="e4abd-107">When you try to sign in to the [Azure portal](https://portal.azure.com/) or the [Azure account center](https://account.windowsazure.com/Subscriptions), you receive the following error message: "No subscriptions found".</span></span>

## <a name="cause"></a><span data-ttu-id="e4abd-108">Příčina</span><span class="sxs-lookup"><span data-stu-id="e4abd-108">Cause</span></span>

<span data-ttu-id="e4abd-109">K tomuto problému dochází, pokud váš účet nemá dostatečná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="e4abd-109">This problem occurs if your account doesn’t have sufficient permissions.</span></span> 

## <a name="solution"></a><span data-ttu-id="e4abd-110">Řešení</span><span class="sxs-lookup"><span data-stu-id="e4abd-110">Solution</span></span>

<span data-ttu-id="e4abd-111">Ujistěte se, přihlásit se jako správce správné.</span><span class="sxs-lookup"><span data-stu-id="e4abd-111">Make sure that you log in as the correct administrator.</span></span> <span data-ttu-id="e4abd-112">Účet správce můžete přistupovat pouze centra účtů.</span><span class="sxs-lookup"><span data-stu-id="e4abd-112">An Account Administrator can access only the Account Center.</span></span> <span data-ttu-id="e4abd-113">Správci služby (SA) a Spolusprávci (CA) mají oprávnění k jenom na portálu Azure nebo portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="e4abd-113">Service Administrators (SA) and Co-Administrators (CA) have access permission only to the Azure portal or the Azure classic portal.</span></span>

### <a name="scenario-1-error-message-is-received-in-the-azure-portalhttpsportalazurecom"></a><span data-ttu-id="e4abd-114">Scénář 1: Chybová zpráva se dostali [portálu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="e4abd-114">Scenario 1: Error message is received in the [Azure portal](https://portal.azure.com)</span></span>

<span data-ttu-id="e4abd-115">Pokud chcete tento problém vyřešit:</span><span class="sxs-lookup"><span data-stu-id="e4abd-115">To fix this issue:</span></span>

* <span data-ttu-id="e4abd-116">Zkontrolujte, že správné adresář Azure je vybrána kliknutím na váš účet v pravé horní.</span><span class="sxs-lookup"><span data-stu-id="e4abd-116">Make sure that the correct Azure directory is selected by clicking your account at the top right.</span></span>

  ![Vyberte adresář, v horní pravé části portálu Azure](./media/billing-no-subscriptions-found/directory-switch.png)

* <span data-ttu-id="e4abd-118">Pokud je zaškrtnuto vpravo Azure directory, ale stále zobrazí se chybová zpráva, [byl váš účet přidán jako vlastníka](billing-add-change-azure-subscription-administrator.md).</span><span class="sxs-lookup"><span data-stu-id="e4abd-118">If the right Azure directory is selected but you still receive the error message, [have your account added as an Owner](billing-add-change-azure-subscription-administrator.md).</span></span>

### <a name="scenario-2-error-message-is-received-in-the-azure-account-centerhttpsaccountwindowsazurecomsubscriptions"></a><span data-ttu-id="e4abd-119">Scénář 2: Chybová zpráva se dostali [centra účtů Azure](https://account.windowsazure.com/Subscriptions)</span><span class="sxs-lookup"><span data-stu-id="e4abd-119">Scenario 2: Error message is received in the [Azure account center](https://account.windowsazure.com/Subscriptions)</span></span>

<span data-ttu-id="e4abd-120">Zkontrolujte, zda je účet, který jste použili účet správce.</span><span class="sxs-lookup"><span data-stu-id="e4abd-120">Check whether the account that you used is the Account Administrator.</span></span> <span data-ttu-id="e4abd-121">Pokud chcete ověřit, kdo je účet správce, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e4abd-121">To verify who the Account Administrator is, follow these steps:</span></span>

1. <span data-ttu-id="e4abd-122">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e4abd-122">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e4abd-123">V nabídce centra vyberte **předplatné**.</span><span class="sxs-lookup"><span data-stu-id="e4abd-123">On the Hub menu, select **Subscription**.</span></span>
3. <span data-ttu-id="e4abd-124">Vyberte předplatné, které chcete kontrolovat, a potom vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="e4abd-124">Select the subscription that you want to check, and then select **Settings**.</span></span>
4. <span data-ttu-id="e4abd-125">Vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="e4abd-125">Select **Properties**.</span></span> <span data-ttu-id="e4abd-126">Správce účtu předplatného se zobrazí v **správce účtu** pole.</span><span class="sxs-lookup"><span data-stu-id="e4abd-126">The account administrator of the subscription is displayed in the **Account Admin** box.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="e4abd-127">Potřebujete pomoct?</span><span class="sxs-lookup"><span data-stu-id="e4abd-127">Need help?</span></span> <span data-ttu-id="e4abd-128">Obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="e4abd-128">Contact support.</span></span>
<span data-ttu-id="e4abd-129">Pokud stále potřebujete pomoc, [obraťte se na podporu](http://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409) získat rychle vyřešit problém.</span><span class="sxs-lookup"><span data-stu-id="e4abd-129">If you still need help, [contact support](http://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409) to get your issue resolved quickly.</span></span> 
