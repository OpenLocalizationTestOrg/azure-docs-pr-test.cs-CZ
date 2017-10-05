---
title: 'Kurz: Azure Active Directory integrace s @Task| Microsoft Docs'
description: "Naučte se konfigurovat jednotné přihlašování mezi Azure Active Directory a @Task."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: ebb19ca6cbaf04106fbce937d95651e709854cfd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-task"></a><span data-ttu-id="e2803-103">Kurz: Azure Active Directory integrace s@Task</span><span class="sxs-lookup"><span data-stu-id="e2803-103">Tutorial: Azure Active Directory integration with @Task</span></span>
<span data-ttu-id="e2803-104">Cílem tohoto kurzu je ukazují, jak integrovat @Task službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e2803-104">The objective of this tutorial is to show you how to integrate @Task with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="e2803-105">Integrace @Task s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e2803-105">Integrating @Task with Azure AD provides you with the following benefits:</span></span> 

* <span data-ttu-id="e2803-106">Můžete řídit ve službě Azure AD, kdo má přístup k@Task</span><span class="sxs-lookup"><span data-stu-id="e2803-106">You can control in Azure AD who has access to @Task</span></span>
* <span data-ttu-id="e2803-107">Můžete povolit uživatelům automaticky přihlášení k získání @Task (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e2803-107">You can enable your users to automatically get signed-on to @Task (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="e2803-108">Můžete spravovat vaše účty v jednom centrálním místě – portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="e2803-108">You can manage your accounts in one central location - the Azure classic Portal</span></span>

<span data-ttu-id="e2803-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e2803-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e2803-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e2803-110">Prerequisites</span></span>
<span data-ttu-id="e2803-111">Při konfiguraci integrace Azure AD s @Task, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="e2803-111">To configure Azure AD integration with @Task, you need the following items:</span></span>

* <span data-ttu-id="e2803-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e2803-112">An Azure AD subscription</span></span>
* <span data-ttu-id="e2803-113">@Task Jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e2803-113">An @Task single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e2803-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e2803-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="e2803-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e2803-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="e2803-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="e2803-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="e2803-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e2803-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="e2803-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e2803-118">Scenario Description</span></span>
<span data-ttu-id="e2803-119">Cílem tohoto kurzu je vám umožní testování Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e2803-119">The objective of this tutorial is to enable you to test Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="e2803-120">Scénář uvedených v tomto kurzu se skládá ze tří hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e2803-120">The scenario outlined in this tutorial consists of three main building blocks:</span></span>

1. <span data-ttu-id="e2803-121">Přidání @Task z Galerie</span><span class="sxs-lookup"><span data-stu-id="e2803-121">Adding @Task from the gallery</span></span> 
2. <span data-ttu-id="e2803-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e2803-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-task-from-the-gallery"></a><span data-ttu-id="e2803-123">Přidání @Task z Galerie</span><span class="sxs-lookup"><span data-stu-id="e2803-123">Adding @Task from the gallery</span></span>
<span data-ttu-id="e2803-124">Konfigurace integrace @Task do služby Azure AD, je nutné přidat @Task z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="e2803-124">To configure the integration of @Task into Azure AD, you need to add @Task from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e2803-125">**Chcete-li přidat @Task z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e2803-125">**To add @Task from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e2803-126">V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e2803-126">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1] 
2. <span data-ttu-id="e2803-128">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="e2803-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="e2803-129">Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="e2803-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Aplikace][2] 
4. <span data-ttu-id="e2803-131">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="e2803-131">Click **Add** at the bottom of the page.</span></span>
   
    ![Aplikace][3] 
5. <span data-ttu-id="e2803-133">Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="e2803-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Aplikace][4] 
6. <span data-ttu-id="e2803-135">Do vyhledávacího pole zadejte  **@Task** .</span><span class="sxs-lookup"><span data-stu-id="e2803-135">In the search box, type **@Task**.</span></span>
   
    ![Aplikace][5] 
7. <span data-ttu-id="e2803-137">V podokně výsledků vyberte  **@Task** a potom klikněte na **Complete** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="e2803-137">In the results pane, select **@Task**, and then click **Complete** to add the application.</span></span>
   
    ![Aplikace][30] 

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e2803-139">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e2803-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e2803-140">Cílem této části je návod, jak nakonfigurovat a otestovat Azure AD jednotné přihlašování s @Task podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="e2803-140">The objective of this section is to show you how to configure and test Azure AD single sign-on with @Task based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e2803-141">Pro jednotné přihlašování pro práci, musí vědět, jaké příslušného uživatele v Azure AD @Task pro uživatele ve službě Azure AD je.</span><span class="sxs-lookup"><span data-stu-id="e2803-141">For single sign-on to work, Azure AD needs to know what the counterpart user in @Task to an user in Azure AD is.</span></span> <span data-ttu-id="e2803-142">Jinými slovy, odkaz vztah mezi uživatele Azure AD a související uživatelské v @Task musí být vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="e2803-142">In other words, a link relationship between an Azure AD user and the related user in @Task needs to be established.</span></span>   
<span data-ttu-id="e2803-143">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v @Task.</span><span class="sxs-lookup"><span data-stu-id="e2803-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in @Task.</span></span>

<span data-ttu-id="e2803-144">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s @Task, je potřeba provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="e2803-144">To configure and test Azure AD single sign-on with @Task, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e2803-145">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="e2803-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e2803-146">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e2803-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e2803-147">**[Vytváření @Tasktest uživatele](#creating-a-halogen-software-test-user)**  – Pokud chcete mít protějšek Britta Simon v @Taskthat je propojený s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="e2803-147">**[Creating a @Tasktest user](#creating-a-halogen-software-test-user)** - to have a counterpart of Britta Simon in @Taskthat is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="e2803-148">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e2803-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e2803-149">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e2803-149">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e2803-150">Konfigurace Azure AD jednotné přihlášení</span><span class="sxs-lookup"><span data-stu-id="e2803-150">Configuring Azure AD Single Sign-On</span></span>
<span data-ttu-id="e2803-151">Cílem této části je chcete povolit Azure AD jednotného přihlašování na portálu Azure classic a nakonfigurovat jednotné přihlašování v vaší @Task aplikace.</span><span class="sxs-lookup"><span data-stu-id="e2803-151">The objective of this section is to enable Azure AD single sign-on in the Azure classic portal and to configure single sign-on in your @Task application.</span></span>

<span data-ttu-id="e2803-152">**Ke konfiguraci Azure AD jednotné přihlašování s @Task, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e2803-152">**To configure Azure AD single sign-on with @Task, perform the following steps:**</span></span>

1. <span data-ttu-id="e2803-153">Na portálu Azure classic na  **@Task**  stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e2803-153">In the Azure classic portal, on the **@Task** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurovat jednotné přihlašování][6] 
2. <span data-ttu-id="e2803-155">Na **jak chcete uživatelům se přihlásit na @Task**  vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e2803-155">On the **How would you like users to sign on to @Task** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Azure AD jednotné přihlášení][7] 
3. <span data-ttu-id="e2803-157">Na **nakonfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e2803-157">On the **Configure App Settings** dialog page, perform the following steps:</span></span>
   
    ![Konfigurovat nastavení aplikace][8] 
   
     <span data-ttu-id="e2803-159">a.</span><span class="sxs-lookup"><span data-stu-id="e2803-159">a.</span></span> <span data-ttu-id="e2803-160">V **přihlašovací adresa URL** textové pole, zadejte adresu URL používá uživatelům přihlášení k vaší @Task aplikace (např:*https://<Tenant name>.attask ondemand.com*).</span><span class="sxs-lookup"><span data-stu-id="e2803-160">In the **Sign On URL** textbox, type the URL used by your users to sign-on to your @Task application (e.g.:*https://<Tenant name>.attask-ondemand.com*).</span></span>
   
     <span data-ttu-id="e2803-161">b.</span><span class="sxs-lookup"><span data-stu-id="e2803-161">b.</span></span> <span data-ttu-id="e2803-162">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e2803-162">Click **Next**.</span></span>
4. <span data-ttu-id="e2803-163">Na **nakonfigurovat jednotné přihlašování v @Task**  klikněte na **stáhnout metadata**, uložte soubor metadat místně na vašem počítači a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e2803-163">On the **Configure single sign-on at @Task** page, click **Download metadata**, save the metadata file locally on your computer, and then click **Next**.</span></span>
   
    ![Co je služba Azure AD Connect][9] 
5. <span data-ttu-id="e2803-165">Přihlášení k vaší @Task společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="e2803-165">Sign-on to your @Task company site as administrator.</span></span>
6. <span data-ttu-id="e2803-166">Přejděte na **jednotné přihlašování v konfiguraci**.</span><span class="sxs-lookup"><span data-stu-id="e2803-166">Go to **Single Sign On Configuration**.</span></span>
7. <span data-ttu-id="e2803-167">Na **jednotné přihlašování** dialogové okno, proveďte následující kroky</span><span class="sxs-lookup"><span data-stu-id="e2803-167">On the **Single Sign-On** dialog, perform the following steps</span></span>
   
    ![Konfigurovat jednotné přihlašování][23]
   
    <span data-ttu-id="e2803-169">a.</span><span class="sxs-lookup"><span data-stu-id="e2803-169">a.</span></span> <span data-ttu-id="e2803-170">Jako **typ**, vyberte **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="e2803-170">As **Type**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="e2803-171">b.</span><span class="sxs-lookup"><span data-stu-id="e2803-171">b.</span></span> <span data-ttu-id="e2803-172">Vyberte **služby ID zprostředkovatele**.</span><span class="sxs-lookup"><span data-stu-id="e2803-172">Select **Service Provider ID**.</span></span>
   
    <span data-ttu-id="e2803-173">c.</span><span class="sxs-lookup"><span data-stu-id="e2803-173">c.</span></span> <span data-ttu-id="e2803-174">Na portálu Azure classic, zkopírujte **vzdálené adresy URL pro přihlášení**a vložte ji do **adresu URL pro přihlášení portálu** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e2803-174">On the Azure classic portal, copy the **Remote Login URL**, and then paste it into the **Login Portal URL** textbox.</span></span>
   
    <span data-ttu-id="e2803-175">d.</span><span class="sxs-lookup"><span data-stu-id="e2803-175">d.</span></span> <span data-ttu-id="e2803-176">Na portálu Azure classic, zkopírujte **jednu adresu URL služby Sign-Out**a vložte ji do **Sign-Out URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e2803-176">On the Azure classic portal, copy the **Single Sign-Out Service URL**, and then paste it into the **Sign-Out URL** textbox.</span></span>
   
    <span data-ttu-id="e2803-177">e.</span><span class="sxs-lookup"><span data-stu-id="e2803-177">e.</span></span> <span data-ttu-id="e2803-178">Na portálu Azure classic, zkopírujte **heslo změnit adresu URL**a vložte ji do **heslo změnit adresu URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e2803-178">On the Azure classic portal, copy the **Change Password URL**, and then paste it into the **Change Password URL** textbox.</span></span>
   
    <span data-ttu-id="e2803-179">f.</span><span class="sxs-lookup"><span data-stu-id="e2803-179">f.</span></span> <span data-ttu-id="e2803-180">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e2803-180">Click **Save**.</span></span>
8. <span data-ttu-id="e2803-181">Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e2803-181">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    ![Co je služba Azure AD Connect][10]
9. <span data-ttu-id="e2803-183">Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="e2803-183">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
   
    ![Co je služba Azure AD Connect][11]

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e2803-185">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e2803-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="e2803-186">Cílem této části je vytvoření zkušebního uživatele na portálu Azure classic názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e2803-186">The objective of this section is to create a test user in the Azure classic portal called Britta Simon.</span></span>  

![Vytvořit uživatele Azure AD][20]

<span data-ttu-id="e2803-188">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e2803-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e2803-189">V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e2803-189">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 
2. <span data-ttu-id="e2803-191">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="e2803-191">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="e2803-192">Chcete-li zobrazit seznam uživatelů, v nabídce v horní části, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e2803-192">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
4. <span data-ttu-id="e2803-194">Chcete-li otevřít **přidat uživatele** dialogovém okně, na panelu nástrojů v dolní části, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="e2803-194">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 
5. <span data-ttu-id="e2803-196">Na **Povězte nám o tohoto uživatele** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e2803-196">On the **Tell us about this user** dialog page, perform the following steps:</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 
   
    <span data-ttu-id="e2803-198">a.</span><span class="sxs-lookup"><span data-stu-id="e2803-198">a.</span></span> <span data-ttu-id="e2803-199">Jako typ uživatele vyberte nového uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="e2803-199">As Type Of User, select New user in your organization.</span></span>
   
    <span data-ttu-id="e2803-200">b.</span><span class="sxs-lookup"><span data-stu-id="e2803-200">b.</span></span> <span data-ttu-id="e2803-201">V uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e2803-201">In the User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="e2803-202">c.</span><span class="sxs-lookup"><span data-stu-id="e2803-202">c.</span></span> <span data-ttu-id="e2803-203">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e2803-203">Click **Next**.</span></span>
6. <span data-ttu-id="e2803-204">Na **profil uživatele** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e2803-204">On the **User Profile** dialog page, perform the following steps:</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
   
    <span data-ttu-id="e2803-206">a.</span><span class="sxs-lookup"><span data-stu-id="e2803-206">a.</span></span> <span data-ttu-id="e2803-207">V **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="e2803-207">In the **First Name** textbox, type **Britta**.</span></span>  
   
    <span data-ttu-id="e2803-208">b.</span><span class="sxs-lookup"><span data-stu-id="e2803-208">b.</span></span> <span data-ttu-id="e2803-209">V **příjmení** textovému poli, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="e2803-209">In the **Last Name** textbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="e2803-210">c.</span><span class="sxs-lookup"><span data-stu-id="e2803-210">c.</span></span> <span data-ttu-id="e2803-211">V **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="e2803-211">In the **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="e2803-212">d.</span><span class="sxs-lookup"><span data-stu-id="e2803-212">d.</span></span> <span data-ttu-id="e2803-213">V **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="e2803-213">In the **Role** list, select **User**.</span></span>

    <span data-ttu-id="e2803-214">e.</span><span class="sxs-lookup"><span data-stu-id="e2803-214">e.</span></span> <span data-ttu-id="e2803-215">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e2803-215">Click **Next**.</span></span>

7. <span data-ttu-id="e2803-216">Na **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e2803-216">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
8. <span data-ttu-id="e2803-218">Na **získat dočasné heslo** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e2803-218">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
   
    <span data-ttu-id="e2803-220">a.</span><span class="sxs-lookup"><span data-stu-id="e2803-220">a.</span></span> <span data-ttu-id="e2803-221">Poznamenejte si hodnotu **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="e2803-221">Write down the value of the **New Password**.</span></span>
   
    <span data-ttu-id="e2803-222">b.</span><span class="sxs-lookup"><span data-stu-id="e2803-222">b.</span></span> <span data-ttu-id="e2803-223">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="e2803-223">Click **Complete**.</span></span>   

### <a name="creating-an-task-test-user"></a><span data-ttu-id="e2803-224">Vytvoření @Task testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="e2803-224">Creating an @Task test user</span></span>
<span data-ttu-id="e2803-225">Cílem této části je vytvoření uživatele volal Britta Simon v @Task.</span><span class="sxs-lookup"><span data-stu-id="e2803-225">The objective of this section is to create a user called Britta Simon in @Task.</span></span>

<span data-ttu-id="e2803-226">**Vytvoření uživatele volal Britta Simon v @Task, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e2803-226">**To create a user called Britta Simon in @Task, perform the following steps:**</span></span>

1. <span data-ttu-id="e2803-227">Přihlaste se k vaší @Task společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="e2803-227">Sign on to your @Task company site as administrator.</span></span>
2. <span data-ttu-id="e2803-228">V nabídce v horní části, klikněte na tlačítko **osoby**.</span><span class="sxs-lookup"><span data-stu-id="e2803-228">In the menu on the top, click **People**.</span></span>
3. <span data-ttu-id="e2803-229">Klikněte na tlačítko **nové osobě**.</span><span class="sxs-lookup"><span data-stu-id="e2803-229">Click **New Person**.</span></span> 
4. <span data-ttu-id="e2803-230">V dialogovém okně nové osobě proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e2803-230">On the New Person dialog, perform the following steps:</span></span>
   
    ![Vytvoření @Task testovacího uživatele][21] 
   
    <span data-ttu-id="e2803-232">a.</span><span class="sxs-lookup"><span data-stu-id="e2803-232">a.</span></span> <span data-ttu-id="e2803-233">V **křestní jméno** textovému poli, zadejte "Britta".</span><span class="sxs-lookup"><span data-stu-id="e2803-233">In the **First Name** textbox, type "Britta".</span></span>
   
    <span data-ttu-id="e2803-234">b.</span><span class="sxs-lookup"><span data-stu-id="e2803-234">b.</span></span> <span data-ttu-id="e2803-235">V **příjmení** textovému poli, zadejte "Simon".</span><span class="sxs-lookup"><span data-stu-id="e2803-235">In the **Last Name** textbox, type "Simon".</span></span>
   
    <span data-ttu-id="e2803-236">c.</span><span class="sxs-lookup"><span data-stu-id="e2803-236">c.</span></span> <span data-ttu-id="e2803-237">V **e-mailovou adresu** textovému poli, zadejte e-mailovou adresu Britta Simon v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e2803-237">In the **Email Address** textbox, type Britta Simon's email address in Azure Active Directory.</span></span>
   
    <span data-ttu-id="e2803-238">d.</span><span class="sxs-lookup"><span data-stu-id="e2803-238">d.</span></span> <span data-ttu-id="e2803-239">Klikněte na tlačítko **přidat osobu**.</span><span class="sxs-lookup"><span data-stu-id="e2803-239">Click **Add Person**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e2803-240">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e2803-240">Assigning the Azure AD test user</span></span>
<span data-ttu-id="e2803-241">Cílem této části je povolení Britta Simon používat Azure jednotné přihlašování, poskytněte svůj přístup k @Task.</span><span class="sxs-lookup"><span data-stu-id="e2803-241">The objective of this section is to enabling Britta Simon to use Azure single sign-on by granting her access to @Task.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e2803-243">**Přiřadit Britta Simon k @Task, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e2803-243">**To assign Britta Simon to @Task, perform the following steps:**</span></span>

1. <span data-ttu-id="e2803-244">Na portálu Azure classic, otevřete zobrazení aplikací, v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="e2803-244">On the Azure classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Přiřadit uživatele][201] 
2. <span data-ttu-id="e2803-246">V seznamu aplikací vyberte  **@Task** .</span><span class="sxs-lookup"><span data-stu-id="e2803-246">In the applications list, select **@Task**.</span></span>
   
    ![Přiřadit uživatele][202] 
3. <span data-ttu-id="e2803-248">V nabídce v horní části, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e2803-248">In the menu on the top, click **Users**.</span></span>
   
    ![Přiřadit uživatele][203] 
4. <span data-ttu-id="e2803-250">V seznamu uživatelů vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="e2803-250">In the Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="e2803-251">Na panelu nástrojů v dolní části klikněte na tlačítko **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="e2803-251">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Přiřadit uživatele][205]

### <a name="testing-single-sign-on"></a><span data-ttu-id="e2803-253">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e2803-253">Testing Single Sign-On</span></span>
<span data-ttu-id="e2803-254">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="e2803-254">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="e2803-255">Když kliknete @Task dlaždici na přístupovém panelu, měli byste obdržet automaticky přihlášení k vaší @Task aplikace.</span><span class="sxs-lookup"><span data-stu-id="e2803-255">When you click the @Task tile in the Access Panel, you should get automatically signed-on to your @Task application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e2803-256">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e2803-256">Additional Resources</span></span>
* [<span data-ttu-id="e2803-257">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e2803-257">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e2803-258">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e2803-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






