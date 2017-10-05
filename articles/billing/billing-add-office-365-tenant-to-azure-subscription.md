---
title: "Předplatné služby Azure pomocí klienta služby Office 365 | Microsoft Docs"
description: "Informace o postupu přidání adresář služby Office 365 (klient) k předplatnému Azure."
services: 
documentationcenter: 
author: JiangChen79
manager: jlian
editor: 
tags: billing,top-support-issue
ms.assetid: cc9c57c6-7bfd-4dea-9027-c75ef3737589
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: cjiang
ms.openlocfilehash: 7affb4a83cdb8ababef60e786a3c824e7477ed68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="associate-an-office-365-tenant-to-an-azure-subscription"></a><span data-ttu-id="29888-103">Přidružení klienta služby Office 365 k předplatnému Azure</span><span class="sxs-lookup"><span data-stu-id="29888-103">Associate an Office 365 tenant to an Azure subscription</span></span>
<span data-ttu-id="29888-104">Propojte samostatné předplatných Azure a Office 365, máte přístup klienta Office 365 z vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="29888-104">Link your separate Azure and Office 365 subscriptions so that you can access the Office 365 tenant from your Azure subscription.</span></span> <span data-ttu-id="29888-105">Odkaz vašich předplatných, přihlaste se k Azure pomocí účtu správce služby Azure, přidat adresář a přidejte účty organizace Office 365 pro klienta služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="29888-105">To link your subscriptions, sign in to Azure with the Azure service administrator account, add a directory, and add the Office 365 organizational accounts to the Azure Active Directory tenant.</span></span>

<span data-ttu-id="29888-106">Pokud chcete mít předplatné služeb Office 365 pro uživatele ve vaší instanci Azure Active Directory nebo máte účet Office 365, ale není účet Azure, najdete v části [registraci do Azure pomocí účtu Office 365](billing-use-existing-office-365-account-azure-subscription.md).</span><span class="sxs-lookup"><span data-stu-id="29888-106">If you want an Office 365 subscription for users in your Azure Active Directory instance or you have an Office 365 account but not an Azure account, see [Sign up for Azure with Office 365 account](billing-use-existing-office-365-account-azure-subscription.md).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="29888-107">Než začnete</span><span class="sxs-lookup"><span data-stu-id="29888-107">Before you begin</span></span>
* <span data-ttu-id="29888-108">Musíte mít pověření správce služby předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="29888-108">You must have the credentials of the Azure subscription service administrator.</span></span> <span data-ttu-id="29888-109">Účty správce společné nelze provést některé kroky v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="29888-109">Co-administrator accounts can't do some of the steps in this article.</span></span> <span data-ttu-id="29888-110">Chcete-li změnit správce služby, [postup přidání nebo změna role Správce služby Azure](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).</span><span class="sxs-lookup"><span data-stu-id="29888-110">To change your service administrator, see [How to add or change Azure administrator roles](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).</span></span>
* <span data-ttu-id="29888-111">Musíte mít pověření pro globálního správce klienta Office 365.</span><span class="sxs-lookup"><span data-stu-id="29888-111">You must have the credentials of a global administrator of the Office 365 tenant.</span></span>
* <span data-ttu-id="29888-112">E-mailovou adresu správce služby nesmí být v klientovi Office 365.</span><span class="sxs-lookup"><span data-stu-id="29888-112">The email address of the service administrator must not be in the Office 365 tenant.</span></span>
* <span data-ttu-id="29888-113">E-mailovou adresu správce služeb nesmí odpovídat všechny globální správce klienta Office 365.</span><span class="sxs-lookup"><span data-stu-id="29888-113">The email address of the service administrator must not match that of any global administrator of the Office 365 tenant.</span></span>
* <span data-ttu-id="29888-114">Pokud používáte e-mailovou adresu, která je účet Microsoft a účet organizace, dočasně změňte na služby správce vašeho předplatného Azure a použít jiný účet Microsoft.</span><span class="sxs-lookup"><span data-stu-id="29888-114">If you use an email address that is both a Microsoft account and an organizational account, temporarily change the service administrator of your Azure subscription to use another Microsoft account.</span></span> <span data-ttu-id="29888-115">Můžete vytvořit účet Microsoft v [stránky registraci účtu Microsoft](https://signup.live.com/).</span><span class="sxs-lookup"><span data-stu-id="29888-115">You can create a Microsoft account at the [Microsoft account signup page](https://signup.live.com/).</span></span>

## <a name="link-office-365-tenant-to-azure-subscription"></a><span data-ttu-id="29888-116">Klienta Office 365 odkaz pro předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="29888-116">Link Office 365 tenant to Azure subscription</span></span>
<span data-ttu-id="29888-117">Chcete-li přidružit klienta Office 365 pro předplatné Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="29888-117">To associate the Office 365 tenant to the Azure subscription, follow these steps:</span></span>

### <a name="step-1-add-office-365-tenant-to-your-azure-subscription"></a><span data-ttu-id="29888-118">Krok 1: Přidání klienta Office 365 k předplatnému Azure</span><span class="sxs-lookup"><span data-stu-id="29888-118">Step 1: Add Office 365 tenant to your Azure subscription</span></span>

1. <span data-ttu-id="29888-119">Přihlaste se k [portál Azure classic](https://manage.windowsazure.com/) s přihlašovacími údaji správce služby.</span><span class="sxs-lookup"><span data-stu-id="29888-119">Sign in to the [Azure classic portal](https://manage.windowsazure.com/) with the service administrator credentials.</span></span>

    ![Snímek obrazovky Azure přihlásit](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

2. <span data-ttu-id="29888-121">V levém podokně vyberte **služby ACTIVE DIRECTORY**.</span><span class="sxs-lookup"><span data-stu-id="29888-121">In the left pane, select **ACTIVE DIRECTORY**.</span></span> <span data-ttu-id="29888-122">Byste neměli vidět klienta Office 365.</span><span class="sxs-lookup"><span data-stu-id="29888-122">You shouldn't see the Office 365 tenant.</span></span> <span data-ttu-id="29888-123">Pokud se zobrazí, přejděte k [krok 2: Změňte adresář přidružený k předplatnému Azure](#Step2).</span><span class="sxs-lookup"><span data-stu-id="29888-123">If you see it, skip to [Step 2: Change the directory associated with the Azure subscription](#Step2).</span></span>
   
   ![Snímek obrazovky Active Directory položka](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

3. <span data-ttu-id="29888-125">Vyberte **nový** > **DIRECTORY** > **vlastní vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="29888-125">Select **NEW** > **DIRECTORY** > **CUSTOM CREATE**.</span></span>
   
    ![Vytvořit vlastní snímek obrazovky nástroje Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)
   
4. <span data-ttu-id="29888-127">Na **adresáře přidat** v části **DIRECTORY**, vyberte **použít existující adresář**.</span><span class="sxs-lookup"><span data-stu-id="29888-127">On the **Add directory** page, under **DIRECTORY**, select **Use existing directory**.</span></span> <span data-ttu-id="29888-128">Potom vyberte **nyní mě můžete odhlásit**a vyberte **Complete** ![dokončení ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span><span class="sxs-lookup"><span data-stu-id="29888-128">Then select **I am ready to be signed out now**, and select **Complete** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span></span>
   
    ![Snímek obrazovky "Použít existující adresář"](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)
   
5. <span data-ttu-id="29888-130">Poté, co jste se odhlásili, přihlaste se pomocí přihlašovacích údajů globálního správce vašeho tenanta Office 365.</span><span class="sxs-lookup"><span data-stu-id="29888-130">After you are signed out, sign in with the global administrator’s credentials of your Office 365 tenant.</span></span>
   
    ![Přihlášení globálního správce Office 365 – snímek obrazovky](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)
   
6. <span data-ttu-id="29888-132">Vyberte **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="29888-132">Select **Continue**.</span></span>
   
    ![Snímek obrazovky ověření](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)
   
7. <span data-ttu-id="29888-134">Vyberte **Odhlásit**.</span><span class="sxs-lookup"><span data-stu-id="29888-134">Select **Sign out now**.</span></span>
   
    ![Snímek obrazovky odhlášení](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)
   
8. <span data-ttu-id="29888-136">Přihlaste se k [portál Azure classic](https://manage.windowsazure.com/) s přihlašovacími údaji správce služby.</span><span class="sxs-lookup"><span data-stu-id="29888-136">Sign in to the [Azure classic portal](https://manage.windowsazure.com/) with the service administrator credentials.</span></span>
   
    ![Snímek obrazovky Azure přihlásit](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)
   
9. <span data-ttu-id="29888-138">Měli byste vidět vašeho tenanta Office 365 v řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="29888-138">You should see your Office 365 tenant in the dashboard.</span></span>
   
    ![Snímek obrazovky řídicí panel](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

### <span data-ttu-id="29888-140"><a name="Step2"></a>Krok 2: Změňte adresář přidružený k předplatnému Azure</span><span class="sxs-lookup"><span data-stu-id="29888-140"><a name="Step2"></a>Step 2: Change the directory associated with the Azure subscription</span></span>
   
1. <span data-ttu-id="29888-141">Vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="29888-141">Select **Settings**.</span></span>
   
    ![Ikona snímek Azure classic nastavení portálu](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)
   
2. <span data-ttu-id="29888-143">Vyberte předplatné Azure a pak vyberte **upravit adresář**.</span><span class="sxs-lookup"><span data-stu-id="29888-143">Select your Azure subscription, and then select **EDIT DIRECTORY**.</span></span>

    ![Snímek obrazovky Azure předplatné upravit adresář](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)
   
3. <span data-ttu-id="29888-145">Vyberte **Další** ![další ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).</span><span class="sxs-lookup"><span data-stu-id="29888-145">Select **Next** ![Next icon](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).</span></span>
   
    ![Snímek obrazovky "Změna přidružené adresáře"](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)
   
4. <span data-ttu-id="29888-147">Zkontrolujte ovlivněných účty.</span><span class="sxs-lookup"><span data-stu-id="29888-147">Review the affected accounts.</span></span> <span data-ttu-id="29888-148">Všechny spolusprávci a [řízení přístupu na základě Role (RBAC)](../active-directory/role-based-access-control-configure.md) uživatelé s přiřazenou přístupem v existující skupiny prostředků se odeberou.</span><span class="sxs-lookup"><span data-stu-id="29888-148">All co-administrators and [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md) users with assigned access in the existing resource groups are removed.</span></span> <span data-ttu-id="29888-149">Uvádí, odebrání spolusprávci pouze upozornění, které se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="29888-149">The warning you receive only mentions the removal of co-administrators.</span></span>
      
    ![Snímek obrazovky, který zobrazuje účty spolusprávcem odeberou.](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)
   
    ![Snímek obrazovky zobrazující uživatelský účet příklad odeberou.](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)
   
5. <span data-ttu-id="29888-152">Vyberte **Complete** ![dokončení ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span><span class="sxs-lookup"><span data-stu-id="29888-152">Select **Complete** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span></span>

### <a name="step-3-add-your-office-365-organizational-accounts-as-co-administrators-to-the-azure-active-directory-tenant"></a><span data-ttu-id="29888-153">Krok 3: Přidání vaší organizace účtů Office 365 jako spolusprávce pro klienta služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="29888-153">Step 3: Add your Office 365 organizational accounts as co-administrators to the Azure Active Directory tenant</span></span>
   
1. <span data-ttu-id="29888-154">Vyberte **správci** a pak vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="29888-154">Select the **ADMINISTRATORS** tab, and then select **ADD**.</span></span>
   
    ![Karta správci snímek Azure classic nastavení portálu](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)
   
2. <span data-ttu-id="29888-156">Zadejte účet organizace klienta služby Office 365, vyberte předplatné Azure a potom vyberte **Complete** ![dokončení ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span><span class="sxs-lookup"><span data-stu-id="29888-156">Enter an organizational account of your Office 365 tenant, select the Azure subscription, and then select **Complete** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span></span>
   
    ![Snímek obrazovky Azure spolusprávcem dialogové okno Přidat](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)
   
3. <span data-ttu-id="29888-158">Přejděte zpět **správci** kartě.</span><span class="sxs-lookup"><span data-stu-id="29888-158">Go back to the **ADMINISTRATORS** tab.</span></span> <span data-ttu-id="29888-159">Měli byste vidět účet organizace zobrazí jako spolusprávce.</span><span class="sxs-lookup"><span data-stu-id="29888-159">You should see the organizational account displayed as co-administrator.</span></span>
   
    ![Snímek obrazovky s kartou správce](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)
4.  <span data-ttu-id="29888-161">Test přístupu k Azure pomocí účtu správce společné.</span><span class="sxs-lookup"><span data-stu-id="29888-161">Test access to Azure with the co-administrator account.</span></span>
   
    <span data-ttu-id="29888-162">a.</span><span class="sxs-lookup"><span data-stu-id="29888-162">a.</span></span> <span data-ttu-id="29888-163">Odhlásit z portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="29888-163">Sign out of the Azure classic portal.</span></span>
   
    <span data-ttu-id="29888-164">b.</span><span class="sxs-lookup"><span data-stu-id="29888-164">b.</span></span> <span data-ttu-id="29888-165">Otevřete web [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="29888-165">Open the [Azure portal](https://portal.azure.com/).</span></span>
   
    <span data-ttu-id="29888-166">c.</span><span class="sxs-lookup"><span data-stu-id="29888-166">c.</span></span> <span data-ttu-id="29888-167">Zadejte přihlašovací údaje správce společné a potom vyberte **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="29888-167">Enter the credentials of the co-administrator, and then select **Sign in**.</span></span>
   
    ![Snímek obrazovky Azure přihlašovací stránky](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="need-help-contact-support"></a><span data-ttu-id="29888-169">Potřebujete pomoct?</span><span class="sxs-lookup"><span data-stu-id="29888-169">Need help?</span></span> <span data-ttu-id="29888-170">Obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="29888-170">Contact support.</span></span>
<span data-ttu-id="29888-171">Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém.</span><span class="sxs-lookup"><span data-stu-id="29888-171">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your issue resolved quickly.</span></span>


