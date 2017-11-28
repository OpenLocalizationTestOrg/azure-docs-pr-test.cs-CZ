---
title: "aaaGet spuštění pomocí podmíněného přístupu v Azure Active Directory | Microsoft Docs"
description: "Otestujte podmíněný přístup pomocí podmínku umístění."
services: active-directory
keywords: "podmíněný přístup tooapps, podmíněného přístupu s Azure AD, zabezpečení přístupu k prostředkům toocompany, zásady podmíněného přístupu"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 4521f5a34f5882e026f5e58a7127d8c55cba2f0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-conditional-access-in-azure-active-directory"></a><span data-ttu-id="47f3f-104">Začínáme s podmíněným přístupem v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="47f3f-104">Get started with conditional access in Azure Active Directory</span></span>

<span data-ttu-id="47f3f-105">Podmíněný přístup je funkce služby Azure Active Directory, která umožňuje vám podmínky toodefine pod nimiž oprávněným uživatelům přístup k vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="47f3f-105">Conditional access is a capability of Azure Active Directory that enables you toodefine conditions under which authorized users can access your apps.</span></span> 

<span data-ttu-id="47f3f-106">Toto téma obsahuje pokyny pro testování podmíněného přístupu na základě podmínky umístění ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="47f3f-106">This topic provides you with instructions for testing a conditional access based on a location condition in your environment.</span></span>  


## <a name="scenario-description"></a><span data-ttu-id="47f3f-107">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="47f3f-107">Scenario description</span></span>

<span data-ttu-id="47f3f-108">Jeden požadavek na běžné v mnoha organizacích je tooonly vyžadovat vícefaktorové ověřování pro přístup k tooapps, který není provést z podnikové sítě intranet hello.</span><span class="sxs-lookup"><span data-stu-id="47f3f-108">One common requirement in many organizations is tooonly require multi-factor authentication for access tooapps that is not performed from hello corporate intranet.</span></span> <span data-ttu-id="47f3f-109">S Azure Active Directory můžete snadno dosažení tohoto cíle konfigurací zásady podmíněného přístupu na základě polohy.</span><span class="sxs-lookup"><span data-stu-id="47f3f-109">With Azure Active Directory, you can easily accomplish this goal by configuring a location-based conditional access policy.</span></span> <span data-ttu-id="47f3f-110">Toto téma poskytuje podrobné pokyny ke konfiguraci související zásad.</span><span class="sxs-lookup"><span data-stu-id="47f3f-110">This topic provides you with detailed instructions for configuring a related policy.</span></span> <span data-ttu-id="47f3f-111">Hello využívá zásady [důvěryhodné IP adresy](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips) toodistinguish mezi pokusy o přístup z podnikové hello žádost je intranetu a všechny ostatní umístění.</span><span class="sxs-lookup"><span data-stu-id="47f3f-111">hello policy leverages [Trusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips) toodistinguish between access attempts made from hello corporate's intranet and all other locations.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="47f3f-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="47f3f-112">Prerequisites</span></span>

<span data-ttu-id="47f3f-113">Hello scénáři uvedeném v tomto tématu se předpokládá, že jste obeznámeni s koncepty hello uvedených v [podmíněného přístupu Azure Active Directory](active-directory-conditional-access-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="47f3f-113">hello scenario outlined in this topic assumes that you are familiar with hello concepts outlined in [Azure Active Directory conditional access](active-directory-conditional-access-azure-portal.md).</span></span>

<span data-ttu-id="47f3f-114">tootest tento scénář, potřebujete:</span><span class="sxs-lookup"><span data-stu-id="47f3f-114">tootest this scenario, you need to:</span></span>

- <span data-ttu-id="47f3f-115">Vytvoření zkušebního uživatele</span><span class="sxs-lookup"><span data-stu-id="47f3f-115">Create a test user</span></span> 

- <span data-ttu-id="47f3f-116">Přiřadit Azure AD Premium licence toohello testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="47f3f-116">Assign an Azure AD Premium license toohello test user</span></span>

- <span data-ttu-id="47f3f-117">Nakonfigurujte spravovanou aplikaci a přiřaďte tooit vaše testovací uživatele</span><span class="sxs-lookup"><span data-stu-id="47f3f-117">Configure a managed app and assign your test user tooit</span></span>

- <span data-ttu-id="47f3f-118">Konfigurovat důvěryhodné IP adresy</span><span class="sxs-lookup"><span data-stu-id="47f3f-118">Configure trusted IPs</span></span>

<span data-ttu-id="47f3f-119">Pokud potřebujete další podrobnosti o důvěryhodné IP adresy, přečtěte si [důvěryhodné IP adresy](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span><span class="sxs-lookup"><span data-stu-id="47f3f-119">If you need more details about Trusted IPs, see [Trusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span></span>


## <a name="policy-configuration-steps"></a><span data-ttu-id="47f3f-120">Postup konfigurace zásad</span><span class="sxs-lookup"><span data-stu-id="47f3f-120">Policy configuration steps</span></span>

<span data-ttu-id="47f3f-121">**tooconfigure zásady podmíněného přístupu, udělat:**</span><span class="sxs-lookup"><span data-stu-id="47f3f-121">**tooconfigure your conditional access policy, do:**</span></span>

1. <span data-ttu-id="47f3f-122">V hello portál Azure, na hello levé navigační panel, klikněte na **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-122">In hello Azure portal, on hello left navbar, click **Azure Active Directory**.</span></span> 

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/01.png)

2. <span data-ttu-id="47f3f-124">Na hello **Azure Active Directory** okno, v hello **spravovat** klikněte na tlačítko **podmíněného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-124">On hello **Azure Active Directory** blade, in hello **Manage** section, click **Conditional access**.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/02.png)
 
3. <span data-ttu-id="47f3f-126">Na hello **podmíněného přístupu** okno, tooopen hello **nový** klikněte na okno na hello nástrojů v horní části hello **přidat**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-126">On hello **Conditional Access** blade, tooopen hello **New** blade, in hello toolbar on hello top, click **Add**.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/03.png)

4. <span data-ttu-id="47f3f-128">Na hello **nový** okno, v hello **název** textovému poli, zadejte název zásady.</span><span class="sxs-lookup"><span data-stu-id="47f3f-128">On hello **New** blade, in hello **Name** textbox, type a name for your policy.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/04.png)

5. <span data-ttu-id="47f3f-130">V hello **přiřazení** klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-130">In hello **Assignment** section, click **Users and groups**.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/05.png)

6. <span data-ttu-id="47f3f-132">Na hello **uživatelů a skupin** okně provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="47f3f-132">On hello **Users and groups** blade, perform hello following steps:</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/06.png)

    <span data-ttu-id="47f3f-134">a.</span><span class="sxs-lookup"><span data-stu-id="47f3f-134">a.</span></span> <span data-ttu-id="47f3f-135">Klikněte na tlačítko **vyberte uživatele a skupiny**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-135">Click **Select users and groups**.</span></span>

    <span data-ttu-id="47f3f-136">b.</span><span class="sxs-lookup"><span data-stu-id="47f3f-136">b.</span></span> <span data-ttu-id="47f3f-137">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-137">Click **Select**.</span></span>

    <span data-ttu-id="47f3f-138">c.</span><span class="sxs-lookup"><span data-stu-id="47f3f-138">c.</span></span> <span data-ttu-id="47f3f-139">Na hello **vyberte** okně vyberte svého testovacího uživatele a pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-139">On hello **Select** blade, select your test user, and then click **Select**.</span></span>

    <span data-ttu-id="47f3f-140">d.</span><span class="sxs-lookup"><span data-stu-id="47f3f-140">d.</span></span> <span data-ttu-id="47f3f-141">Na hello **uživatelů a skupin** okně klikněte na tlačítko **provádí**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-141">On hello **Users and groups** blade, click **Done**.</span></span>

7. <span data-ttu-id="47f3f-142">Na hello **nový** okno, tooopen hello **cloudových aplikací** okno, v hello **přiřazení** klikněte na tlačítko **cloudových aplikací**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-142">On hello **New** blade, tooopen hello **Cloud apps** blade, in hello **Assignment** section, click **Cloud apps**.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

8. <span data-ttu-id="47f3f-144">Na hello **cloudových aplikací** okně provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="47f3f-144">On hello **Cloud apps** blade, perform hello following steps:</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/08.png)

    <span data-ttu-id="47f3f-146">a.</span><span class="sxs-lookup"><span data-stu-id="47f3f-146">a.</span></span> <span data-ttu-id="47f3f-147">Klikněte na tlačítko **vybrat aplikace**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-147">Click **Select apps**.</span></span>

    <span data-ttu-id="47f3f-148">b.</span><span class="sxs-lookup"><span data-stu-id="47f3f-148">b.</span></span> <span data-ttu-id="47f3f-149">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-149">Click **Select**.</span></span>

    <span data-ttu-id="47f3f-150">c.</span><span class="sxs-lookup"><span data-stu-id="47f3f-150">c.</span></span> <span data-ttu-id="47f3f-151">Na hello **vyberte** okně vyberte cloudové aplikace a pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-151">On hello **Select** blade, select your cloud app, and then click **Select**.</span></span>

    <span data-ttu-id="47f3f-152">d.</span><span class="sxs-lookup"><span data-stu-id="47f3f-152">d.</span></span> <span data-ttu-id="47f3f-153">Na hello **cloudových aplikací** okně klikněte na tlačítko **provádí**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-153">On hello **Cloud apps** blade, click **Done**.</span></span>

9. <span data-ttu-id="47f3f-154">Na hello **nový** okno, tooopen hello **podmínky** okno, v hello **přiřazení** klikněte na tlačítko **podmínky**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-154">On hello **New** blade, tooopen hello **Conditions** blade, in hello **Assignment** section, click **Conditions**.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/09.png)

10. <span data-ttu-id="47f3f-156">Na hello **podmínky** okno, tooopen hello **umístění** okně klikněte na tlačítko **umístění**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-156">On hello **Conditions** blade, tooopen hello **Locations** blade, click **Locations**.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/10.png)

11. <span data-ttu-id="47f3f-158">Na hello **umístění** okně provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="47f3f-158">On hello **Locations** blade, perform hello following steps:</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/11.png)

    <span data-ttu-id="47f3f-160">a.</span><span class="sxs-lookup"><span data-stu-id="47f3f-160">a.</span></span> <span data-ttu-id="47f3f-161">V části **konfigurace**, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-161">Under **Configure**, click **Yes**.</span></span>

    <span data-ttu-id="47f3f-162">b.</span><span class="sxs-lookup"><span data-stu-id="47f3f-162">b.</span></span> <span data-ttu-id="47f3f-163">V části **zahrnout**, klikněte na tlačítko **všech umístění**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-163">Under **Include**, click **All locations**.</span></span>

    <span data-ttu-id="47f3f-164">c.</span><span class="sxs-lookup"><span data-stu-id="47f3f-164">c.</span></span> <span data-ttu-id="47f3f-165">Klikněte na tlačítko **vyloučit**a potom klikněte na **všechny důvěryhodné IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-165">Click **Exclude**, and then click **All trusted IPs**.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/12.png)

    <span data-ttu-id="47f3f-167">d.</span><span class="sxs-lookup"><span data-stu-id="47f3f-167">d.</span></span> <span data-ttu-id="47f3f-168">Klikněte na **Done** (Hotovo).</span><span class="sxs-lookup"><span data-stu-id="47f3f-168">Click **Done**.</span></span>

12. <span data-ttu-id="47f3f-169">Na hello **podmínky** okně klikněte na tlačítko **provádí**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-169">On hello **Conditions** blade, click **Done**.</span></span>

13. <span data-ttu-id="47f3f-170">Na hello **nový** okno, tooopen hello **Grant** okno, v hello **ovládací prvky** klikněte na tlačítko **Grant**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-170">On hello **New** blade, tooopen hello **Grant** blade, in hello **Controls** section, click **Grant**.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. <span data-ttu-id="47f3f-172">Na hello **Grant** okně provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="47f3f-172">On hello **Grant** blade, perform hello following steps:</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    <span data-ttu-id="47f3f-174">a.</span><span class="sxs-lookup"><span data-stu-id="47f3f-174">a.</span></span> <span data-ttu-id="47f3f-175">Vyberte **vyžadovat vícefaktorové ověřování**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-175">Select **Require multi-factor authentication**.</span></span>

    <span data-ttu-id="47f3f-176">b.</span><span class="sxs-lookup"><span data-stu-id="47f3f-176">b.</span></span> <span data-ttu-id="47f3f-177">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-177">Click **Select**.</span></span>

15. <span data-ttu-id="47f3f-178">Na hello **nový** okno, v části **povolit zásady**, klikněte na tlačítko **na**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-178">On hello **New** blade, under **Enable policy**, click **On**.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. <span data-ttu-id="47f3f-180">Na hello **nový** okně klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="47f3f-180">On hello **New** blade, click **Create**.</span></span>


## <a name="testing-hello-policy"></a><span data-ttu-id="47f3f-181">Testování zásad hello</span><span class="sxs-lookup"><span data-stu-id="47f3f-181">Testing hello policy</span></span>

<span data-ttu-id="47f3f-182">tootest zásady vaší aplikace by měla přístup ze zařízení, která:</span><span class="sxs-lookup"><span data-stu-id="47f3f-182">tootest your policy, you should access your app from a device that:</span></span> 

1. <span data-ttu-id="47f3f-183">IP adresu, která je v rámci vaší nakonfigurované důvěryhodné IP adresy</span><span class="sxs-lookup"><span data-stu-id="47f3f-183">Has an IP address that is within your configured Trusted IPs</span></span> 

1. <span data-ttu-id="47f3f-184">IP adresu, která není v rámci vaší nakonfigurované důvěryhodné IP adresy</span><span class="sxs-lookup"><span data-stu-id="47f3f-184">Has an IP address that is not within your configured Trusted IPs</span></span>

<span data-ttu-id="47f3f-185">Služba Multi-Factor authentication musí být pouze požadované při pokusu o připojení, která byla vytvořená ze zařízení, které není v rámci vaší nakonfigurované důvěryhodné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="47f3f-185">Multi-factor authentication should only be required during a connection attempt that was made from a device that is not within your configured Trusted IPs.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="47f3f-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="47f3f-186">Next steps</span></span>

<span data-ttu-id="47f3f-187">Pokud chcete toolearn více informací o podmíněného přístupu, přečtěte si téma [podmíněného přístupu Azure Active Directory](active-directory-conditional-access-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="47f3f-187">If you would like toolearn more about conditional access, see [Azure Active Directory conditional access](active-directory-conditional-access-azure-portal.md).</span></span>

