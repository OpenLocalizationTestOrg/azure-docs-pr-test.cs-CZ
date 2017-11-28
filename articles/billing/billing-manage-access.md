---
title: "tooAzure přístup aaaManage fakturace použití rolí | Microsoft Docs"
description: 
services: 
documentationcenter: 
author: vikramdesai01
manager: vikdesai
editor: 
tags: billing
ms.assetid: e4c4d136-2826-4938-868f-a7e67ff6b025
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: vikdesai
ms.openlocfilehash: 5937fac5ffa5ca204eb03a1dcbc5e800b3d5eb74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-access-toobilling-information-for-azure-using-role-based-access-control"></a><span data-ttu-id="8a4ef-102">Spravovat přístup toobilling informace pro Azure pomocí řízení přístupu na základě rolí</span><span class="sxs-lookup"><span data-stu-id="8a4ef-102">Manage access toobilling information for Azure using role-based access control</span></span>

<span data-ttu-id="8a4ef-103">Můžete udělit přístup Azure fakturační informace toomembers týmu přiřazením mezi hello následující uživatelské role tooyour předplatné: správce účtu, Správce služby, spolusprávcem, vlastník, Přispěvatel, Čtenář a fakturace čtečky.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-103">You can grant access for Azure billing information toomembers of your team by assigning one of hello following user roles tooyour subscription: Account Administrator, Service Administrator, Co-administrator, Owner, Contributor, Reader, and Billing Reader.</span></span> <span data-ttu-id="8a4ef-104">By mají přístup k informacím toobilling v hello [portál Azure](https://portal.azure.com/), a používají hello [fakturace rozhraní API](billing-usage-rate-card-overview.md) tooprogrammatically stažení faktury (jednou přihlásí) a podrobnosti o použití.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-104">They would have access toobilling information in hello [Azure portal](https://portal.azure.com/), and they can use hello [Billing APIs](billing-usage-rate-card-overview.md) tooprogrammatically get invoices (once opted-in) and usage details.</span></span> <span data-ttu-id="8a4ef-105">Další informace o, který můžete udělit role, a které role co naleznete na adrese [role v Azure RBAC](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="8a4ef-105">For more information about who can grant roles, and which roles can do what, see [Roles in Azure RBAC](../active-directory/role-based-access-built-in-roles.md).</span></span>

## <span data-ttu-id="8a4ef-106"><a name="opt-in"></a>Povolení dalších uživatelů tooaccess faktury</span><span class="sxs-lookup"><span data-stu-id="8a4ef-106"><a name="opt-in"></a> Allowing additional users tooaccess invoices</span></span>

<span data-ttu-id="8a4ef-107">Hello správce účtu musí vyjádřit výslovný souhlas pomocí hello [portál Azure](https://portal.azure.com/) povolit přístup tooinvoices pro ostatní uživatele nebo přes rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-107">hello Account Administrator must opt in using hello [Azure portal](https://portal.azure.com/) allow access tooinvoices for other users and via API.</span></span>

1. <span data-ttu-id="8a4ef-108">Jako hello účet správce, vyberte své předplatné z hello [odběry okno](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-108">As hello Account Administrator, select your subscription from hello [Subscriptions blade](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal.</span></span>

1. <span data-ttu-id="8a4ef-109">Vyberte **faktury** a potom **přístup tooinvoices**.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-109">Select **Invoices** and then **Access tooinvoices**.</span></span>

    ![Snímek obrazovky ukazuje, jak toodelegate přistupovat k tooinvoices](./media/billing-manage-access/AA-optin.png)

1. <span data-ttu-id="8a4ef-111">Zapnout **na** hello přístup a ukládají se změny hello, uživatelé tooallow v odběru vymezená role toodownload faktury.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-111">Turn **On** hello access followed by saving hello changes, tooallow users in subscription scoped roles toodownload invoice.</span></span>

    ![Snímek obrazovky ukazuje tooinvoice přístup toodelegate zapnutí a vypnutí](./media/billing-manage-access/AA-optinAllow.png)

<span data-ttu-id="8a4ef-113">Výslovným souhlasem umožňuje služby správce, spolusprávce, vlastník, Přispěvatel, Čtenář a fakturace čtečky u hello předplatné toodownload PDF faktury v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-113">Opting in allows Service Administrator, Co-administrator, Owner, Contributor, Reader, and Billing Reader on hello subscription toodownload PDF invoices in hello Azure portal.</span></span> <span data-ttu-id="8a4ef-114">Faktury starší než prosinec 2016 jsou však k dispozici pouze toohello správce účtu teď.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-114">However, invoices older than December 2016 are available only toohello Account Administrator for now.</span></span>

<span data-ttu-id="8a4ef-115">Hello správce účtu můžete taky nakonfigurovat toohave faktury odesílaly e-mailem.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-115">hello Account Administrator can also configure toohave invoices sent via email.</span></span> <span data-ttu-id="8a4ef-116">Další, najdete v části toolearn [získat faktury v e-mailu](billing-download-azure-invoice-daily-usage-date.md).</span><span class="sxs-lookup"><span data-stu-id="8a4ef-116">toolearn more, see [Get your invoice in email](billing-download-azure-invoice-daily-usage-date.md).</span></span>

## <a name="adding-users-toohello-billing-reader-role"></a><span data-ttu-id="8a4ef-117">Přidání role fakturace čtečky toohello uživatelé</span><span class="sxs-lookup"><span data-stu-id="8a4ef-117">Adding users toohello Billing Reader role</span></span>

<span data-ttu-id="8a4ef-118">role čtenáře fakturace Hello má oprávnění jen pro čtení toosubscription fakturační informace na portálu Azure a žádné tooservices přístup, jako je například virtuální počítače a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-118">hello Billing Reader role has read-only access toosubscription billing information in Azure portal, and no access tooservices such as VMs and storage accounts.</span></span> <span data-ttu-id="8a4ef-119">Přiřadit toosomeone hello fakturace čtečky role, která potřebuje přístup k informacím fakturace předplatného toohello ale není hello toomanage možnost Azure services.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-119">Assign hello Billing Reader role toosomeone that needs access toohello subscription billing information but not hello ability toomanage Azure services.</span></span> <span data-ttu-id="8a4ef-120">Tato role je vhodný pro uživatele v organizaci, kteří pouze provádět správu finanční a nákladů pro předplatná Azure.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-120">This role is appropriate for users in an organization who only perform financial and cost management for Azure subscriptions.</span></span>

1. <span data-ttu-id="8a4ef-121">Vyberte své předplatné z hello [odběry okno](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-121">Select your subscription from hello [Subscriptions blade](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal.</span></span>

1. <span data-ttu-id="8a4ef-122">Vyberte **přístup k ovládacímu prvku (IAM)** a pak klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-122">Select **Access control (IAM)** and then click **Add**.</span></span>

    ![Snímek obrazovky ukazuje IAM v okně předplatné hello](./media/billing-manage-access/select-iam.PNG)

1. <span data-ttu-id="8a4ef-124">Zvolte **fakturace čtečky** v hello **vyberte roli** stránky.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-124">Choose **Billing Reader** in hello **Select a role** page.</span></span>

    ![Snímek obrazovky ukazuje fakturace čtečky v automaticky otevřeném okně zobrazení hello](./media/billing-manage-access/select-roles.PNG)

1. <span data-ttu-id="8a4ef-126">Zadejte hello e-mailu pro uživatele hello tooinvite a pak klikněte na tlačítko **OK** toosend hello pozvánku.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-126">Type hello email for hello user you want tooinvite, then click **OK** toosend hello invitation.</span></span>

    ![Snímek obrazovky zobrazující tooinvite tooenter e-mailu, někdo](./media/billing-manage-access/add-user.PNG)

1. <span data-ttu-id="8a4ef-128">Postupujte podle pokynů v hello pozvání e-mailu toolog v jako čtečku fakturace.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-128">Follow instructions in hello invite email toolog in as a Billing Reader.</span></span>

    ![Snímek obrazovky, který zobrazuje, co hello čtečky fakturace můžete zobrazit na portálu Azure](./media/billing-manage-access/billing-reader-view.png)

> [!NOTE]
> <span data-ttu-id="8a4ef-130">Hello fakturace čtečky funkce je ve verzi preview a zatím nepodporuje předplatného typu enterprise (EA) nebo neglobální cloudy.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-130">hello Billing Reader feature is in preview, and does not yet support enterprise (EA) subscriptions or non-global clouds.</span></span>

## <a name="adding-users-tooother-roles"></a><span data-ttu-id="8a4ef-131">Přidávání uživatelů tooother rolí</span><span class="sxs-lookup"><span data-stu-id="8a4ef-131">Adding users tooother roles</span></span>

<span data-ttu-id="8a4ef-132">Uživatelé v jiných rolí, například vlastníka nebo přispěvatele, můžou používat jenom fakturační informace, ale také služby Azure.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-132">Users in other roles, such as Owner or Contributor, can access not just billing information, but Azure services as well.</span></span> <span data-ttu-id="8a4ef-133">najdete v těchto rolí toomanage [přidání nebo změna role Správce služby Azure, které spravují předplatné hello nebo služby](billing-add-change-azure-subscription-administrator.md).</span><span class="sxs-lookup"><span data-stu-id="8a4ef-133">toomanage these roles, see [Add or change Azure administrator roles that manage hello subscription or services](billing-add-change-azure-subscription-administrator.md).</span></span>

## <a name="who-can-access-hello-account-centerhttpsaccountwindowsazurecom"></a><span data-ttu-id="8a4ef-134">Kdo má přístup k hello [centra účtů](https://account.windowsazure.com)?</span><span class="sxs-lookup"><span data-stu-id="8a4ef-134">Who can access hello [Account Center](https://account.windowsazure.com)?</span></span>

<span data-ttu-id="8a4ef-135">V centru účtů toohello můžou přihlásit pouze hello správce účtu.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-135">Only hello Account Administrator can log in toohello Account center.</span></span> <span data-ttu-id="8a4ef-136">Hello správce účtu je hello právní vlastníka předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-136">hello Account Administrator is hello legal owner of hello subscription.</span></span> <span data-ttu-id="8a4ef-137">Ve výchozím nastavení, hello osobě, která registraci aplikace nebo koupili hello předplatné Azure je hello účet správce, pokud hello [byl přenos vlastnictví předplatného](billing-subscription-transfer.md) toosomebody else.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-137">By default, hello person who signed up for or bought hello Azure subscription is hello Account Administrator, unless hello [subscription ownership was transferred](billing-subscription-transfer.md) toosomebody else.</span></span> <span data-ttu-id="8a4ef-138">Hello správce účtu můžete vytvářet odběry, zrušit předplatné, změnit hello fakturační adresu pro přihlášení k odběru a spravovat zásady přístupu pro předplatné hello.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-138">hello Account Administrator can create subscriptions, cancel subscriptions, change hello billing address for a subscription, and manage access policies for hello subscription.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="8a4ef-139">Potřebujete pomoct?</span><span class="sxs-lookup"><span data-stu-id="8a4ef-139">Need help?</span></span> <span data-ttu-id="8a4ef-140">Obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-140">Contact support.</span></span>

<span data-ttu-id="8a4ef-141">Pokud máte další otázky, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit problém.</span><span class="sxs-lookup"><span data-stu-id="8a4ef-141">If you still have further questions, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your issue resolved quickly.</span></span>
