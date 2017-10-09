---
title: "aaaUse Office 365 klienta předplatné | Microsoft Docs"
description: "Zjistěte, jak tooadd Office 365 adresáře (klient) tooan předplatného Azure."
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
ms.openlocfilehash: e560370417bd074a7e37ceb7c60da45dbbbcf775
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="associate-an-office-365-tenant-tooan-azure-subscription"></a><span data-ttu-id="cd2ad-103">Přidružit Office 365 klienta tooan předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="cd2ad-103">Associate an Office 365 tenant tooan Azure subscription</span></span>
<span data-ttu-id="cd2ad-104">Propojte samostatné předplatných Azure a Office 365, máte přístup klienta hello Office 365 z vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-104">Link your separate Azure and Office 365 subscriptions so that you can access hello Office 365 tenant from your Azure subscription.</span></span> <span data-ttu-id="cd2ad-105">toolink vaše předplatné, přihlaste se tooAzure s hello účet správce služby Azure, přidejte adresáře a přidejte klienta Azure Active Directory toohello účty organizace hello Office 365.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-105">toolink your subscriptions, sign in tooAzure with hello Azure service administrator account, add a directory, and add hello Office 365 organizational accounts toohello Azure Active Directory tenant.</span></span>

<span data-ttu-id="cd2ad-106">Pokud chcete mít předplatné služeb Office 365 pro uživatele ve vaší instanci Azure Active Directory nebo máte účet Office 365, ale není účet Azure, najdete v části [registraci do Azure pomocí účtu Office 365](billing-use-existing-office-365-account-azure-subscription.md).</span><span class="sxs-lookup"><span data-stu-id="cd2ad-106">If you want an Office 365 subscription for users in your Azure Active Directory instance or you have an Office 365 account but not an Azure account, see [Sign up for Azure with Office 365 account](billing-use-existing-office-365-account-azure-subscription.md).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="cd2ad-107">Než začnete</span><span class="sxs-lookup"><span data-stu-id="cd2ad-107">Before you begin</span></span>
* <span data-ttu-id="cd2ad-108">Musíte mít hello přihlašovací údaje Správce služby hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-108">You must have hello credentials of hello Azure subscription service administrator.</span></span> <span data-ttu-id="cd2ad-109">Účty společné správce nemůže provádět některé hello kroky v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-109">Co-administrator accounts can't do some of hello steps in this article.</span></span> <span data-ttu-id="cd2ad-110">toochange Správce služby, najdete v části [jak tooadd nebo změna role Správce služby Azure](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).</span><span class="sxs-lookup"><span data-stu-id="cd2ad-110">toochange your service administrator, see [How tooadd or change Azure administrator roles](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).</span></span>
* <span data-ttu-id="cd2ad-111">Musí mít hello pověření pro globálního správce klienta Office 365 hello.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-111">You must have hello credentials of a global administrator of hello Office 365 tenant.</span></span>
* <span data-ttu-id="cd2ad-112">Hello e-mailovou adresu správce služeb hello nesmí být v klientovi hello Office 365.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-112">hello email address of hello service administrator must not be in hello Office 365 tenant.</span></span>
* <span data-ttu-id="cd2ad-113">Hello e-mailovou adresu správce služeb hello nesmí shodovat s globálním administrátorem klienta hello Office 365.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-113">hello email address of hello service administrator must not match that of any global administrator of hello Office 365 tenant.</span></span>
* <span data-ttu-id="cd2ad-114">Pokud používáte e-mailovou adresu, která je účet Microsoft a účet organizace, dočasně změňte hello služby správce vašeho předplatného Azure toouse jiný účet Microsoft.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-114">If you use an email address that is both a Microsoft account and an organizational account, temporarily change hello service administrator of your Azure subscription toouse another Microsoft account.</span></span> <span data-ttu-id="cd2ad-115">Můžete vytvořit účet Microsoft v hello [stránky registraci účtu Microsoft](https://signup.live.com/).</span><span class="sxs-lookup"><span data-stu-id="cd2ad-115">You can create a Microsoft account at hello [Microsoft account signup page](https://signup.live.com/).</span></span>

## <a name="link-office-365-tenant-tooazure-subscription"></a><span data-ttu-id="cd2ad-116">Odkaz předplatné tooAzure klienta Office 365</span><span class="sxs-lookup"><span data-stu-id="cd2ad-116">Link Office 365 tenant tooAzure subscription</span></span>
<span data-ttu-id="cd2ad-117">tooassociate hello Office 365 klienta toohello předplatné Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="cd2ad-117">tooassociate hello Office 365 tenant toohello Azure subscription, follow these steps:</span></span>

### <a name="step-1-add-office-365-tenant-tooyour-azure-subscription"></a><span data-ttu-id="cd2ad-118">Krok 1: Přidání tooyour klienta Office 365 předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="cd2ad-118">Step 1: Add Office 365 tenant tooyour Azure subscription</span></span>

1. <span data-ttu-id="cd2ad-119">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com/) s hello přihlašovací údaje Správce služby.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-119">Sign in toohello [Azure classic portal](https://manage.windowsazure.com/) with hello service administrator credentials.</span></span>

    ![Snímek obrazovky Azure přihlásit](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

2. <span data-ttu-id="cd2ad-121">V levém podokně hello vyberte **služby ACTIVE DIRECTORY**.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-121">In hello left pane, select **ACTIVE DIRECTORY**.</span></span> <span data-ttu-id="cd2ad-122">Byste neměli vidět klienta hello Office 365.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-122">You shouldn't see hello Office 365 tenant.</span></span> <span data-ttu-id="cd2ad-123">Pokud se zobrazí, přeskočte příliš[krok 2: změnit adresář hello hello předplatného Azure přidružené](#Step2).</span><span class="sxs-lookup"><span data-stu-id="cd2ad-123">If you see it, skip too[Step 2: Change hello directory associated with hello Azure subscription](#Step2).</span></span>
   
   ![Snímek obrazovky Active Directory položka](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

3. <span data-ttu-id="cd2ad-125">Vyberte **nový** > **DIRECTORY** > **vlastní vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-125">Select **NEW** > **DIRECTORY** > **CUSTOM CREATE**.</span></span>
   
    ![Vytvořit vlastní snímek obrazovky nástroje Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)
   
4. <span data-ttu-id="cd2ad-127">Na hello **adresáře přidat** v části **DIRECTORY**, vyberte **použít existující adresář**.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-127">On hello **Add directory** page, under **DIRECTORY**, select **Use existing directory**.</span></span> <span data-ttu-id="cd2ad-128">Potom vyberte **jsem připravené toobe Odhlásit**a vyberte **Complete** ![dokončení ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span><span class="sxs-lookup"><span data-stu-id="cd2ad-128">Then select **I am ready toobe signed out now**, and select **Complete** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span></span>
   
    ![Snímek obrazovky "Použít existující adresář"](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)
   
5. <span data-ttu-id="cd2ad-130">Poté, co jste se odhlásili, přihlaste se pomocí přihlašovacích údajů hello globálního správce pro vašeho tenanta Office 365.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-130">After you are signed out, sign in with hello global administrator’s credentials of your Office 365 tenant.</span></span>
   
    ![Přihlášení globálního správce Office 365 – snímek obrazovky](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)
   
6. <span data-ttu-id="cd2ad-132">Vyberte **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-132">Select **Continue**.</span></span>
   
    ![Snímek obrazovky ověření](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)
   
7. <span data-ttu-id="cd2ad-134">Vyberte **Odhlásit**.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-134">Select **Sign out now**.</span></span>
   
    ![Snímek obrazovky odhlášení](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)
   
8. <span data-ttu-id="cd2ad-136">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com/) s hello přihlašovací údaje Správce služby.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-136">Sign in toohello [Azure classic portal](https://manage.windowsazure.com/) with hello service administrator credentials.</span></span>
   
    ![Snímek obrazovky Azure přihlásit](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)
   
9. <span data-ttu-id="cd2ad-138">Měli byste vidět vašeho tenanta Office 365 na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-138">You should see your Office 365 tenant in hello dashboard.</span></span>
   
    ![Snímek obrazovky řídicí panel](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

### <span data-ttu-id="cd2ad-140"><a name="Step2"></a>Krok 2: Změnit adresář hello přidružené hello předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="cd2ad-140"><a name="Step2"></a>Step 2: Change hello directory associated with hello Azure subscription</span></span>
   
1. <span data-ttu-id="cd2ad-141">Vyberte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-141">Select **Settings**.</span></span>
   
    ![Ikona snímek Azure classic nastavení portálu](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)
   
2. <span data-ttu-id="cd2ad-143">Vyberte předplatné Azure a pak vyberte **upravit adresář**.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-143">Select your Azure subscription, and then select **EDIT DIRECTORY**.</span></span>

    ![Snímek obrazovky Azure předplatné upravit adresář](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)
   
3. <span data-ttu-id="cd2ad-145">Vyberte **Další** ![další ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).</span><span class="sxs-lookup"><span data-stu-id="cd2ad-145">Select **Next** ![Next icon](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).</span></span>
   
    ![Snímek obrazovky "Změna hello přidružené adresáře"](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)
   
4. <span data-ttu-id="cd2ad-147">Zkontrolujte účty hello vliv.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-147">Review hello affected accounts.</span></span> <span data-ttu-id="cd2ad-148">Všechny spolusprávci a [řízení přístupu na základě Role (RBAC)](../active-directory/role-based-access-control-configure.md) uživatelé s přiřazenou přístupem v hello existující skupiny prostředků se odeberou.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-148">All co-administrators and [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md) users with assigned access in hello existing resource groups are removed.</span></span> <span data-ttu-id="cd2ad-149">uvádí, hello odebrání spolusprávci pouze Hello upozornění, které se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-149">hello warning you receive only mentions hello removal of co-administrators.</span></span>
      
    ![Snímek obrazovky, který zobrazuje hello spolusprávcem účty toobe odebrat.](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)
   
    ![Snímek obrazovky, který ukazuje příklad uživatelského účtu toobe odebrat.](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)
   
5. <span data-ttu-id="cd2ad-152">Vyberte **Complete** ![dokončení ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span><span class="sxs-lookup"><span data-stu-id="cd2ad-152">Select **Complete** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span></span>

### <a name="step-3-add-your-office-365-organizational-accounts-as-co-administrators-toohello-azure-active-directory-tenant"></a><span data-ttu-id="cd2ad-153">Krok 3: Přidáte jako klienta Azure Active Directory toohello spolusprávci vaší organizace účtů Office 365</span><span class="sxs-lookup"><span data-stu-id="cd2ad-153">Step 3: Add your Office 365 organizational accounts as co-administrators toohello Azure Active Directory tenant</span></span>
   
1. <span data-ttu-id="cd2ad-154">Vyberte hello **správci** a pak vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-154">Select hello **ADMINISTRATORS** tab, and then select **ADD**.</span></span>
   
    ![Karta správci snímek Azure classic nastavení portálu](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)
   
2. <span data-ttu-id="cd2ad-156">Zadejte účet organizace klienta služby Office 365, vyberte hello předplatného Azure a pak vyberte **Complete** ![dokončení ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span><span class="sxs-lookup"><span data-stu-id="cd2ad-156">Enter an organizational account of your Office 365 tenant, select hello Azure subscription, and then select **Complete** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span></span>
   
    ![Snímek obrazovky Azure spolusprávcem dialogové okno Přidat](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)
   
3. <span data-ttu-id="cd2ad-158">Přejděte zpět toohello **správci** kartě. Měli byste vidět účet organizace hello zobrazí jako spolusprávce.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-158">Go back toohello **ADMINISTRATORS** tab. You should see hello organizational account displayed as co-administrator.</span></span>
   
    ![Snímek obrazovky s kartou správce](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)
4.  <span data-ttu-id="cd2ad-160">Test přístupu tooAzure pomocí účtu správce společné hello.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-160">Test access tooAzure with hello co-administrator account.</span></span>
   
    <span data-ttu-id="cd2ad-161">a.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-161">a.</span></span> <span data-ttu-id="cd2ad-162">Odhlásit z hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-162">Sign out of hello Azure classic portal.</span></span>
   
    <span data-ttu-id="cd2ad-163">b.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-163">b.</span></span> <span data-ttu-id="cd2ad-164">Otevřete hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="cd2ad-164">Open hello [Azure portal](https://portal.azure.com/).</span></span>
   
    <span data-ttu-id="cd2ad-165">c.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-165">c.</span></span> <span data-ttu-id="cd2ad-166">Zadejte přihlašovací údaje hello hello společné správce a potom vyberte **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-166">Enter hello credentials of hello co-administrator, and then select **Sign in**.</span></span>
   
    ![Snímek obrazovky Azure přihlašovací stránky](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="need-help-contact-support"></a><span data-ttu-id="cd2ad-168">Potřebujete pomoct?</span><span class="sxs-lookup"><span data-stu-id="cd2ad-168">Need help?</span></span> <span data-ttu-id="cd2ad-169">Obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-169">Contact support.</span></span>
<span data-ttu-id="cd2ad-170">Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit problém.</span><span class="sxs-lookup"><span data-stu-id="cd2ad-170">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your issue resolved quickly.</span></span>


