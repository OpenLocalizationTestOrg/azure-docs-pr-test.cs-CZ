---
title: "Začínáme s podmíněným přístupem v Azure Active Directory | Microsoft Docs"
description: "Otestujte podmíněný přístup pomocí podmínku umístění."
services: active-directory
keywords: "podmíněný přístup k aplikacím, podmíněného přístupu s Azure AD, zabezpečený přístup k prostředkům společnosti, zásady podmíněného přístupu"
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
ms.openlocfilehash: cd53e8be32d1e98aaf9f72177895871dba69df86
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-conditional-access-in-azure-active-directory"></a><span data-ttu-id="f3267-104">Začínáme s podmíněným přístupem v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f3267-104">Get started with conditional access in Azure Active Directory</span></span>

<span data-ttu-id="f3267-105">Podmíněný přístup je funkce služby Azure Active Directory, který umožňuje definovat podmínky, za kterých oprávněným uživatelům přístup k vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3267-105">Conditional access is a capability of Azure Active Directory that enables you to define conditions under which authorized users can access your apps.</span></span> 

<span data-ttu-id="f3267-106">Toto téma obsahuje pokyny pro testování podmíněného přístupu na základě podmínky umístění ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="f3267-106">This topic provides you with instructions for testing a conditional access based on a location condition in your environment.</span></span>  


## <a name="scenario-description"></a><span data-ttu-id="f3267-107">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f3267-107">Scenario description</span></span>

<span data-ttu-id="f3267-108">Jeden požadavek na běžné v mnoha organizacích je pouze vyžadovat vícefaktorové ověřování pro přístup k aplikacím, který neprovádí k podnikovému intranetu.</span><span class="sxs-lookup"><span data-stu-id="f3267-108">One common requirement in many organizations is to only require multi-factor authentication for access to apps that is not performed from the corporate intranet.</span></span> <span data-ttu-id="f3267-109">S Azure Active Directory můžete snadno dosažení tohoto cíle konfigurací zásady podmíněného přístupu na základě polohy.</span><span class="sxs-lookup"><span data-stu-id="f3267-109">With Azure Active Directory, you can easily accomplish this goal by configuring a location-based conditional access policy.</span></span> <span data-ttu-id="f3267-110">Toto téma poskytuje podrobné pokyny ke konfiguraci související zásad.</span><span class="sxs-lookup"><span data-stu-id="f3267-110">This topic provides you with detailed instructions for configuring a related policy.</span></span> <span data-ttu-id="f3267-111">Využívá zásady [důvěryhodné IP adresy](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips) k rozlišení mezi pokusy o přístup z podnikové síti je intranetu a všechny ostatní umístění.</span><span class="sxs-lookup"><span data-stu-id="f3267-111">The policy leverages [Trusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips) to distinguish between access attempts made from the corporate's intranet and all other locations.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="f3267-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f3267-112">Prerequisites</span></span>

<span data-ttu-id="f3267-113">Scénáře uvedené v tomto tématu se předpokládá, že jste obeznámeni s koncepty uvedených v [podmíněného přístupu Azure Active Directory](active-directory-conditional-access-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f3267-113">The scenario outlined in this topic assumes that you are familiar with the concepts outlined in [Azure Active Directory conditional access](active-directory-conditional-access-azure-portal.md).</span></span>

<span data-ttu-id="f3267-114">Chcete-li tento scénář otestovat, je potřeba:</span><span class="sxs-lookup"><span data-stu-id="f3267-114">To test this scenario, you need to:</span></span>

- <span data-ttu-id="f3267-115">Vytvoření zkušebního uživatele</span><span class="sxs-lookup"><span data-stu-id="f3267-115">Create a test user</span></span> 

- <span data-ttu-id="f3267-116">Přiřazení licence Azure AD Premium k testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="f3267-116">Assign an Azure AD Premium license to the test user</span></span>

- <span data-ttu-id="f3267-117">Konfigurace spravované aplikace a přiřadit testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="f3267-117">Configure a managed app and assign your test user to it</span></span>

- <span data-ttu-id="f3267-118">Konfigurovat důvěryhodné IP adresy</span><span class="sxs-lookup"><span data-stu-id="f3267-118">Configure trusted IPs</span></span>

<span data-ttu-id="f3267-119">Pokud potřebujete další podrobnosti o důvěryhodné IP adresy, přečtěte si [důvěryhodné IP adresy](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span><span class="sxs-lookup"><span data-stu-id="f3267-119">If you need more details about Trusted IPs, see [Trusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span></span>


## <a name="policy-configuration-steps"></a><span data-ttu-id="f3267-120">Postup konfigurace zásad</span><span class="sxs-lookup"><span data-stu-id="f3267-120">Policy configuration steps</span></span>

<span data-ttu-id="f3267-121">**Zásady podmíněného přístupu, postupujte při konfiguraci:**</span><span class="sxs-lookup"><span data-stu-id="f3267-121">**To configure your conditional access policy, do:**</span></span>

1. <span data-ttu-id="f3267-122">Na portálu Azure v levé navigační panel, klikněte na tlačítko **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f3267-122">In the Azure portal, on the left navbar, click **Azure Active Directory**.</span></span> 

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/01.png)

2. <span data-ttu-id="f3267-124">Na **Azure Active Directory** okno v **spravovat** klikněte na tlačítko **podmíněného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="f3267-124">On the **Azure Active Directory** blade, in the **Manage** section, click **Conditional access**.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/02.png)
 
3. <span data-ttu-id="f3267-126">Na **podmíněného přístupu** okno Otevřít **nový** , na panelu nástrojů v horní části klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="f3267-126">On the **Conditional Access** blade, to open the **New** blade, in the toolbar on the top, click **Add**.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/03.png)

4. <span data-ttu-id="f3267-128">Na **nový** okno v **název** textovému poli, zadejte název zásady.</span><span class="sxs-lookup"><span data-stu-id="f3267-128">On the **New** blade, in the **Name** textbox, type a name for your policy.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/04.png)

5. <span data-ttu-id="f3267-130">V **přiřazení** klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f3267-130">In the **Assignment** section, click **Users and groups**.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/05.png)

6. <span data-ttu-id="f3267-132">Na **uživatelů a skupin** okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f3267-132">On the **Users and groups** blade, perform the following steps:</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/06.png)

    <span data-ttu-id="f3267-134">a.</span><span class="sxs-lookup"><span data-stu-id="f3267-134">a.</span></span> <span data-ttu-id="f3267-135">Klikněte na tlačítko **vyberte uživatele a skupiny**.</span><span class="sxs-lookup"><span data-stu-id="f3267-135">Click **Select users and groups**.</span></span>

    <span data-ttu-id="f3267-136">b.</span><span class="sxs-lookup"><span data-stu-id="f3267-136">b.</span></span> <span data-ttu-id="f3267-137">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="f3267-137">Click **Select**.</span></span>

    <span data-ttu-id="f3267-138">c.</span><span class="sxs-lookup"><span data-stu-id="f3267-138">c.</span></span> <span data-ttu-id="f3267-139">Na **vyberte** okně vyberte svého testovacího uživatele a pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="f3267-139">On the **Select** blade, select your test user, and then click **Select**.</span></span>

    <span data-ttu-id="f3267-140">d.</span><span class="sxs-lookup"><span data-stu-id="f3267-140">d.</span></span> <span data-ttu-id="f3267-141">Na **uživatelů a skupin** okně klikněte na tlačítko **provádí**.</span><span class="sxs-lookup"><span data-stu-id="f3267-141">On the **Users and groups** blade, click **Done**.</span></span>

7. <span data-ttu-id="f3267-142">Na **nový** okno Otevřít **cloudových aplikací** okno v **přiřazení** klikněte na tlačítko **cloudových aplikací**.</span><span class="sxs-lookup"><span data-stu-id="f3267-142">On the **New** blade, to open the **Cloud apps** blade, in the **Assignment** section, click **Cloud apps**.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

8. <span data-ttu-id="f3267-144">Na **cloudových aplikací** okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f3267-144">On the **Cloud apps** blade, perform the following steps:</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/08.png)

    <span data-ttu-id="f3267-146">a.</span><span class="sxs-lookup"><span data-stu-id="f3267-146">a.</span></span> <span data-ttu-id="f3267-147">Klikněte na tlačítko **vybrat aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f3267-147">Click **Select apps**.</span></span>

    <span data-ttu-id="f3267-148">b.</span><span class="sxs-lookup"><span data-stu-id="f3267-148">b.</span></span> <span data-ttu-id="f3267-149">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="f3267-149">Click **Select**.</span></span>

    <span data-ttu-id="f3267-150">c.</span><span class="sxs-lookup"><span data-stu-id="f3267-150">c.</span></span> <span data-ttu-id="f3267-151">Na **vyberte** okně vyberte cloudové aplikace a pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="f3267-151">On the **Select** blade, select your cloud app, and then click **Select**.</span></span>

    <span data-ttu-id="f3267-152">d.</span><span class="sxs-lookup"><span data-stu-id="f3267-152">d.</span></span> <span data-ttu-id="f3267-153">Na **cloudových aplikací** okno, klikněte na **provést**.</span><span class="sxs-lookup"><span data-stu-id="f3267-153">On the **Cloud apps** blade, click **Done**.</span></span>

9. <span data-ttu-id="f3267-154">Na **nový** okno Otevřít **podmínky** okno v **přiřazení** klikněte na tlačítko **podmínky**.</span><span class="sxs-lookup"><span data-stu-id="f3267-154">On the **New** blade, to open the **Conditions** blade, in the **Assignment** section, click **Conditions**.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/09.png)

10. <span data-ttu-id="f3267-156">Na **podmínky** okno Otevřít **umístění** okně klikněte na tlačítko **umístění**.</span><span class="sxs-lookup"><span data-stu-id="f3267-156">On the **Conditions** blade, to open the **Locations** blade, click **Locations**.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/10.png)

11. <span data-ttu-id="f3267-158">Na **umístění** okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f3267-158">On the **Locations** blade, perform the following steps:</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/11.png)

    <span data-ttu-id="f3267-160">a.</span><span class="sxs-lookup"><span data-stu-id="f3267-160">a.</span></span> <span data-ttu-id="f3267-161">V části **konfigurace**, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="f3267-161">Under **Configure**, click **Yes**.</span></span>

    <span data-ttu-id="f3267-162">b.</span><span class="sxs-lookup"><span data-stu-id="f3267-162">b.</span></span> <span data-ttu-id="f3267-163">V části **zahrnout**, klikněte na tlačítko **všech umístění**.</span><span class="sxs-lookup"><span data-stu-id="f3267-163">Under **Include**, click **All locations**.</span></span>

    <span data-ttu-id="f3267-164">c.</span><span class="sxs-lookup"><span data-stu-id="f3267-164">c.</span></span> <span data-ttu-id="f3267-165">Klikněte na tlačítko **vyloučit**a potom klikněte na **všechny důvěryhodné IP adresy**.</span><span class="sxs-lookup"><span data-stu-id="f3267-165">Click **Exclude**, and then click **All trusted IPs**.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/12.png)

    <span data-ttu-id="f3267-167">d.</span><span class="sxs-lookup"><span data-stu-id="f3267-167">d.</span></span> <span data-ttu-id="f3267-168">Klikněte na **Done** (Hotovo).</span><span class="sxs-lookup"><span data-stu-id="f3267-168">Click **Done**.</span></span>

12. <span data-ttu-id="f3267-169">Na **podmínky** okně klikněte na tlačítko **provádí**.</span><span class="sxs-lookup"><span data-stu-id="f3267-169">On the **Conditions** blade, click **Done**.</span></span>

13. <span data-ttu-id="f3267-170">Na **nový** okno Otevřít **Grant** okno v **ovládací prvky** klikněte na tlačítko **Grant**.</span><span class="sxs-lookup"><span data-stu-id="f3267-170">On the **New** blade, to open the **Grant** blade, in the **Controls** section, click **Grant**.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. <span data-ttu-id="f3267-172">Na **Grant** okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f3267-172">On the **Grant** blade, perform the following steps:</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    <span data-ttu-id="f3267-174">a.</span><span class="sxs-lookup"><span data-stu-id="f3267-174">a.</span></span> <span data-ttu-id="f3267-175">Vyberte **vyžadovat vícefaktorové ověřování**.</span><span class="sxs-lookup"><span data-stu-id="f3267-175">Select **Require multi-factor authentication**.</span></span>

    <span data-ttu-id="f3267-176">b.</span><span class="sxs-lookup"><span data-stu-id="f3267-176">b.</span></span> <span data-ttu-id="f3267-177">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="f3267-177">Click **Select**.</span></span>

15. <span data-ttu-id="f3267-178">Na **nový** okno, v části **povolit zásady**, klikněte na tlačítko **na**.</span><span class="sxs-lookup"><span data-stu-id="f3267-178">On the **New** blade, under **Enable policy**, click **On**.</span></span>

    ![Podmíněný přístup](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. <span data-ttu-id="f3267-180">Na **nový** okně klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f3267-180">On the **New** blade, click **Create**.</span></span>


## <a name="testing-the-policy"></a><span data-ttu-id="f3267-181">Testování zásad</span><span class="sxs-lookup"><span data-stu-id="f3267-181">Testing the policy</span></span>

<span data-ttu-id="f3267-182">K otestování vaše zásady, musí aplikace přístup ze zařízení, která:</span><span class="sxs-lookup"><span data-stu-id="f3267-182">To test your policy, you should access your app from a device that:</span></span> 

1. <span data-ttu-id="f3267-183">IP adresu, která je v rámci vaší nakonfigurované důvěryhodné IP adresy</span><span class="sxs-lookup"><span data-stu-id="f3267-183">Has an IP address that is within your configured Trusted IPs</span></span> 

1. <span data-ttu-id="f3267-184">IP adresu, která není v rámci vaší nakonfigurované důvěryhodné IP adresy</span><span class="sxs-lookup"><span data-stu-id="f3267-184">Has an IP address that is not within your configured Trusted IPs</span></span>

<span data-ttu-id="f3267-185">Služba Multi-Factor authentication musí být pouze požadované při pokusu o připojení, která byla vytvořená ze zařízení, které není v rámci vaší nakonfigurované důvěryhodné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="f3267-185">Multi-factor authentication should only be required during a connection attempt that was made from a device that is not within your configured Trusted IPs.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="f3267-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f3267-186">Next steps</span></span>

<span data-ttu-id="f3267-187">Pokud chcete získat další informace o podmíněného přístupu, přečtěte si téma [podmíněného přístupu Azure Active Directory](active-directory-conditional-access-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f3267-187">If you would like to learn more about conditional access, see [Azure Active Directory conditional access](active-directory-conditional-access-azure-portal.md).</span></span>

