---
title: "Kurz: Azure Active Directory integrace s Wizergos produkční Software | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi softwarem Wizergos produktivitu a Azure Active Directory."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: acc04396-13c5-4c24-ab9a-30fbc9234ebd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2017
ms.author: jeedes
ms.openlocfilehash: 73b3bc05aeb337c12acb7e47c0dbebe6d0196530
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a><span data-ttu-id="9f185-103">Kurz: Azure Active Directory integrace s Wizergos produktivitu softwaru</span><span class="sxs-lookup"><span data-stu-id="9f185-103">Tutorial: Azure Active Directory integration with Wizergos Productivity Software</span></span>
<span data-ttu-id="9f185-104">Cílem tohoto kurzu je ukazují, jak integrovat Wizergos produkční Software s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9f185-104">The objective of this tutorial is to show you how to integrate Wizergos Productivity Software  with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9f185-105">Integrace Wizergos produkční Software s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9f185-105">Integrating Wizergos Productivity Software with Azure AD provides you with the following benefits:</span></span>

* <span data-ttu-id="9f185-106">Můžete řídit ve službě Azure AD, který má přístup k softwaru Wizergos produktivitu</span><span class="sxs-lookup"><span data-stu-id="9f185-106">You can control in Azure AD who has access to Wizergos Productivity Software</span></span>
* <span data-ttu-id="9f185-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k softwaru produktivitu Wizergos jednotné přihlašování (SSO) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f185-107">You can enable your users to automatically get signed-on to Wizergos Productivity Software single sign-on (SSO) with their Azure AD accounts</span></span>
* <span data-ttu-id="9f185-108">Můžete spravovat vaše účty v jednom centrálním místě – portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="9f185-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="9f185-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9f185-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9f185-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9f185-110">Prerequisites</span></span>
<span data-ttu-id="9f185-111">Konfigurace integrace Azure AD s Wizergos produkční Software, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="9f185-111">To configure Azure AD integration with Wizergos Productivity Software, you need the following items:</span></span>

* <span data-ttu-id="9f185-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f185-112">An Azure AD subscription</span></span>
* <span data-ttu-id="9f185-113">Povolené předplatné produktivity Wizergos, jednotného přihlašování k softwaru</span><span class="sxs-lookup"><span data-stu-id="9f185-113">A Wizergos Productivity Software SSO enabled subscription</span></span>

>[!NOTE]
><span data-ttu-id="9f185-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9f185-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span> 
> 

<span data-ttu-id="9f185-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9f185-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="9f185-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="9f185-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="9f185-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9f185-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9f185-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9f185-118">Scenario description</span></span>
<span data-ttu-id="9f185-119">Cílem tohoto kurzu je vám umožní Azure AD jednotného přihlašování k testování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9f185-119">The objective of this tutorial is to enable you to test Azure AD SSO in a test environment.</span></span>

<span data-ttu-id="9f185-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9f185-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9f185-121">Přidání softwaru produktivitu Wizergos z Galerie</span><span class="sxs-lookup"><span data-stu-id="9f185-121">Adding Wizergos Productivity Software from the gallery</span></span>
2. <span data-ttu-id="9f185-122">Konfigurace a testování Azure AD SSO</span><span class="sxs-lookup"><span data-stu-id="9f185-122">Configuring and testing Azure AD SSO</span></span>

## <a name="adding-wizergos-productivity-software-from-the-gallery"></a><span data-ttu-id="9f185-123">Přidání softwaru produktivitu Wizergos z Galerie</span><span class="sxs-lookup"><span data-stu-id="9f185-123">Adding Wizergos Productivity Software from the gallery</span></span>
<span data-ttu-id="9f185-124">Při konfiguraci integrace Wizergos produktivitu softwaru do služby Azure AD, potřebujete přidat Wizergos produktivitu softwaru z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9f185-124">To configure the integration of Wizergos Productivity Software into Azure AD, you need to add Wizergos Productivity Software from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9f185-125">**Chcete-li přidat Wizergos produktivitu softwaru z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9f185-125">**To add Wizergos Productivity Software from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9f185-126">V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9f185-126">In the **Azure classic Portal**, on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]
2. <span data-ttu-id="9f185-128">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="9f185-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="9f185-129">Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="9f185-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Aplikace][2]
4. <span data-ttu-id="9f185-131">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="9f185-131">Click **Add** at the bottom of the page.</span></span>
   
    ![Aplikace][3]
5. <span data-ttu-id="9f185-133">Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="9f185-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Aplikace][4]
6. <span data-ttu-id="9f185-135">Do vyhledávacího pole zadejte **Wizergos produktivitu softwaru**.</span><span class="sxs-lookup"><span data-stu-id="9f185-135">In the search box, type **Wizergos Productivity Software**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_01.png)
7. <span data-ttu-id="9f185-137">Na panelu výsledků vyberte **Wizergos produktivitu softwaru**a potom klikněte na **Complete** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="9f185-137">In the results panel, select **Wizergos Productivity Software**, and then click **Complete** to add the application.</span></span>
   
    ![Výběr aplikace v galerii](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_001.png)

## <a name="configure-and-test-azure-ad-sso"></a><span data-ttu-id="9f185-139">Konfigurace a otestování Azure AD SSO</span><span class="sxs-lookup"><span data-stu-id="9f185-139">Configure and test Azure AD SSO</span></span>
<span data-ttu-id="9f185-140">Cílem této části je ukazují, jak nakonfigurovat a otestovat Azure AD přihlášení SSO se Wizergos produktivitu Software založený na testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="9f185-140">The objective of this section is to show you how to configure and test Azure AD SSO with Wizergos Productivity Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9f185-141">Pro jednotné přihlašování pro práci je potřeba vědět, co uživatel protějškem v Wizergos produktivitu softwaru pro uživatele ve službě Azure AD je Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f185-141">For SSO to work, Azure AD needs to know what the counterpart user in Wizergos Productivity Software to an user in Azure AD is.</span></span> <span data-ttu-id="9f185-142">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Wizergos produktivitu softwaru je nutné stanovit.</span><span class="sxs-lookup"><span data-stu-id="9f185-142">In other words, a link relationship between an Azure AD user and the related user in Wizergos Productivity Software needs to be established.</span></span>

<span data-ttu-id="9f185-143">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Wizergos produktivitu softwaru.</span><span class="sxs-lookup"><span data-stu-id="9f185-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Wizergos Productivity Software.</span></span>

<span data-ttu-id="9f185-144">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Softwareder BynWizergos produktivitu, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="9f185-144">To configure and test Azure AD single sign-on with BynWizergos Productivity Softwareder, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9f185-145">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="9f185-145">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9f185-146">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9f185-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9f185-147">**[Vytvoření zkušebního uživatele softwaru produktivitu Wizergos](#creating-a-wizergos-productivity-software-test-user)**  – Pokud chcete mít protějšek Britta Simon v Wizergos produktivitu Software, který je propojený s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="9f185-147">**[Creating a Wizergos Productivity Software test user](#creating-a-wizergos-productivity-software-test-user)** - to have a counterpart of Britta Simon in Wizergos Productivity Software that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="9f185-148">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9f185-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9f185-149">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9f185-149">**[Testing single sign-on](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-sso"></a><span data-ttu-id="9f185-150">Konfigurace Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9f185-150">Configuring Azure AD SSO</span></span>
<span data-ttu-id="9f185-151">V této části můžete povolit Azure AD jednotného přihlašování na portálu classic a nakonfigurovat jednotné přihlašování v aplikaci Wizergos produktivitu softwaru.</span><span class="sxs-lookup"><span data-stu-id="9f185-151">In this section, you enable Azure AD single sign-on in the classic portal and configure single sign-on in your Wizergos Productivity Software application.</span></span>

<span data-ttu-id="9f185-152">**Ke konfiguraci Azure AD jednotné přihlašování s Wizergos produkční Software, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9f185-152">**To configure Azure AD single sign-on with Wizergos Productivity Software, perform the following steps:**</span></span>

1. <span data-ttu-id="9f185-153">Na portálu classic na **Wizergos produktivitu softwaru** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9f185-153">In the classic portal, on the **Wizergos Productivity Software** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurovat jednotné přihlašování][6] 
2. <span data-ttu-id="9f185-155">Na **jak chcete uživatelům se přihlásit Software pro zvýšení produktivity Wizergos** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**:</span><span class="sxs-lookup"><span data-stu-id="9f185-155">On the **How would you like users to sign on to Wizergos Productivity Software** page, select **Azure AD Single Sign-On**, and then click **Next**:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_03.png)
3. <span data-ttu-id="9f185-157">Na **nakonfigurovat nastavení aplikace** dialogové okno stránky, klikněte na tlačítko **Další**:</span><span class="sxs-lookup"><span data-stu-id="9f185-157">On the **Configure App Settings** dialog page, click **Next**:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_04.png)
4. <span data-ttu-id="9f185-159">Na **nakonfigurovat jednotné přihlašování v softwaru produktivitu Wizergos** klikněte na tlačítko **stažení certifikátu**a potom uložte soubor v počítači:</span><span class="sxs-lookup"><span data-stu-id="9f185-159">On the **Configure single sign-on at Wizergos Productivity Software** page, click **Download certificate**, and then save the file on your computer:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_05.png)
5. <span data-ttu-id="9f185-161">V okně prohlížeče jiných webových přihlašování ke klientovi Wizergos produktivitu softwaru jako správce.</span><span class="sxs-lookup"><span data-stu-id="9f185-161">In a different web browser window, sign-on to your Wizergos Productivity Software tenant as an administrator.</span></span>
6. <span data-ttu-id="9f185-162">Vyberte z nabídky hamburger **správce**.</span><span class="sxs-lookup"><span data-stu-id="9f185-162">From the hamburger menu, select **Admin**.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)
7. <span data-ttu-id="9f185-164">Správce stránce v nabídce vlevo vyberte **ověřování** a klikněte na **Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="9f185-164">In Admin page on left hand menu select **AUTHENTICATION** and click on **Azure AD**.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)
8. <span data-ttu-id="9f185-166">Proveďte následující kroky na **ověřování** části.</span><span class="sxs-lookup"><span data-stu-id="9f185-166">Perform the following steps on **AUTHENTICATION** section.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)
  1. <span data-ttu-id="9f185-168">Klikněte na tlačítko **NAHRÁT** tlačítko, na kterou odešlete certifikát stažený z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f185-168">Click **UPLOAD** button to upload the downloaded certificate from Azure AD.</span></span> 
  2. <span data-ttu-id="9f185-169">V **URL vystavitele** textbox vložte hodnotu **URL vystavitele** z Průvodce konfigurací aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f185-169">In the **Issuer URL** textbox put the value of **Issuer URL** from Azure AD application configuration wizard.</span></span>
  3. <span data-ttu-id="9f185-170">V **adresy jednotného přihlašování** textbox vložte hodnotu **jeden přihlašování adresa URL služby** z Průvodce konfigurací aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f185-170">In the **Single Sign-On URL** textbox put the value of **Single Sign-on Service URL** from Azure AD application configuration wizard.</span></span>
  4. <span data-ttu-id="9f185-171">V **jednu adresu URL Sign-Out** textbox vložte hodnotu **jednu adresu URL služby Sign-out** z Průvodce konfigurací aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f185-171">In the **Single Sign-Out URL** textbox put the value of **Single Sign-out Service URL** from Azure AD application configuration wizard.</span></span>
  5. <span data-ttu-id="9f185-172">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9f185-172">Click **Save** button.</span></span>
9. <span data-ttu-id="9f185-173">Klasický portál, vyberte potvrzení konfigurace přihlášení a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="9f185-173">In the classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span>
   
    ![Azure AD jednotné přihlášení][10]
10. <span data-ttu-id="9f185-175">Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="9f185-175">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
    
    ![Azure AD jednotné přihlášení][11]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="9f185-177">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f185-177">Create an Azure AD test user</span></span>
<span data-ttu-id="9f185-178">Cílem této části je vytvoření zkušebního uživatele na portálu classic názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9f185-178">The objective of this section is to create a test user in the classic portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][20]

<span data-ttu-id="9f185-180">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9f185-180">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9f185-181">V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9f185-181">In the **Azure classic Portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_09.png)
2. <span data-ttu-id="9f185-183">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="9f185-183">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="9f185-184">Chcete-li zobrazit seznam uživatelů, v nabídce v horní části, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9f185-184">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_03.png)
4. <span data-ttu-id="9f185-186">Chcete-li otevřít **přidat uživatele** dialogovém okně, na panelu nástrojů v dolní části, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="9f185-186">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_04.png)
5. <span data-ttu-id="9f185-188">Na **Povězte nám o tohoto uživatele** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9f185-188">On the **Tell us about this user** dialog page, perform the following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_05.png) 
  1. <span data-ttu-id="9f185-190">Jako typ uživatele vyberte nového uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="9f185-190">As Type Of User, select New user in your organization.</span></span>
  2. <span data-ttu-id="9f185-191">V uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9f185-191">In the User Name **textbox**, type **BrittaSimon**.</span></span>
  3. <span data-ttu-id="9f185-192">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="9f185-192">Click **Next**.</span></span>
6. <span data-ttu-id="9f185-193">Na **profil uživatele** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9f185-193">On the **User Profile** dialog page, perform the following steps:</span></span>
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_06.png)
  1. <span data-ttu-id="9f185-195">V **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="9f185-195">In the **First Name** textbox, type **Britta**.</span></span>  
  2. <span data-ttu-id="9f185-196">V **příjmení** textovému poli, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="9f185-196">In the **Last Name** textbox, type, **Simon**.</span></span>
  3. <span data-ttu-id="9f185-197">V **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="9f185-197">In the **Display Name** textbox, type **Britta Simon**.</span></span>
  4. <span data-ttu-id="9f185-198">V **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="9f185-198">In the **Role** list, select **User**.</span></span>
  5. <span data-ttu-id="9f185-199">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="9f185-199">Click **Next**.</span></span>
7. <span data-ttu-id="9f185-200">Na **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9f185-200">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_07.png)
8. <span data-ttu-id="9f185-202">Na **získat dočasné heslo** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9f185-202">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_08.png)
  1. <span data-ttu-id="9f185-204">Poznamenejte si hodnotu **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="9f185-204">Write down the value of the **New Password**.</span></span>
  2. <span data-ttu-id="9f185-205">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="9f185-205">Click **Complete**.</span></span>   

### <a name="create-a-wizergos-productivity-software-test-user"></a><span data-ttu-id="9f185-206">Vytvoření zkušebního uživatele Wizergos produktivitu softwaru</span><span class="sxs-lookup"><span data-stu-id="9f185-206">Create a Wizergos Productivity Software test user</span></span>
<span data-ttu-id="9f185-207">V této části vytvoříte volal Britta Simon v softwaru Wizergos produktivitu uživatele.</span><span class="sxs-lookup"><span data-stu-id="9f185-207">In this section, you create a user called Britta Simon in Wizergos Productivity Software.</span></span> <span data-ttu-id="9f185-208">Spojte se s tým podpory Wizergos produkční Software prostřednictvím [ support@wizergos.com ](emailTo:support@wizergos.com) přidejte uživatele v platformě Wizergos produktivitu softwaru.</span><span class="sxs-lookup"><span data-stu-id="9f185-208">Please work with Wizergos Productivity Software support team via [support@wizergos.com](emailTo:support@wizergos.com) to add the users in the Wizergos Productivity Software platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="9f185-209">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f185-209">Assign the Azure AD test user</span></span>
<span data-ttu-id="9f185-210">Cílem této části je povolení Britta Simon používat jednotného přihlašování k Azure tak, že udělíte přístup k Wizergos produktivitu softwaru.</span><span class="sxs-lookup"><span data-stu-id="9f185-210">The objective of this section is to enabling Britta Simon to use Azure SSO by granting her access to Wizergos Productivity Software.</span></span>

  ![Přiřadit uživatele][200]

<span data-ttu-id="9f185-212">**Pokud chcete přiřadit Britta Simon Wizergos produkční Software, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9f185-212">**To assign Britta Simon to Wizergos Productivity Software, perform the following steps:**</span></span>

1. <span data-ttu-id="9f185-213">Na portálu classic k otevření zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="9f185-213">On the classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Přiřadit uživatele][201]
2. <span data-ttu-id="9f185-215">V seznamu aplikací vyberte **Wizergos produktivitu softwaru**.</span><span class="sxs-lookup"><span data-stu-id="9f185-215">In the applications list, select **Wizergos Productivity Software**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_50.png)
3. <span data-ttu-id="9f185-217">V nabídce v horní části, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9f185-217">In the menu on the top, click **Users**.</span></span>
   
    ![Přiřadit uživatele][203]
4. <span data-ttu-id="9f185-219">V seznamu uživatelů vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="9f185-219">In the Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="9f185-220">Na panelu nástrojů v dolní části klikněte na tlačítko **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="9f185-220">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Přiřadit uživatele][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="9f185-222">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9f185-222">Test single sign-on</span></span>
<span data-ttu-id="9f185-223">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="9f185-223">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="9f185-224">Když kliknete na dlaždici Wizergos produktivitu softwaru na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Wizergos produktivitu softwaru.</span><span class="sxs-lookup"><span data-stu-id="9f185-224">When you click the Wizergos Productivity Software tile in the Access Panel, you should get automatically signed-on to your Wizergos Productivity Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9f185-225">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9f185-225">Additional resources</span></span>
* [<span data-ttu-id="9f185-226">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9f185-226">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9f185-227">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9f185-227">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_205.png
