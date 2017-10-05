---
title: 'Kurz: Azure Active Directory integrace s LinkedInSalesNavigator | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a LinkedInSalesNavigator."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7a9fa8f3-d611-4ffe-8d50-04e9586b24da
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: ef26a16e79d9c9b0654634960b57dc59827b2c24
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-sales-navigator"></a><span data-ttu-id="4a552-103">Kurz: Azure Active Directory integrace s LinkedIn prodej Navigátor</span><span class="sxs-lookup"><span data-stu-id="4a552-103">Tutorial: Azure Active Directory integration with LinkedIn Sales Navigator</span></span>

<span data-ttu-id="4a552-104">V tomto kurzu zjistěte, jak integrovat LinkedIn prodej Navigátor s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4a552-104">In this tutorial, you learn how to integrate LinkedIn Sales Navigator with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4a552-105">Integrace LinkedIn prodej Navigátor s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="4a552-105">Integrating LinkedIn Sales Navigator with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="4a552-106">Můžete řídit ve službě Azure AD, který má přístup k LinkedIn prodej Navigátor</span><span class="sxs-lookup"><span data-stu-id="4a552-106">You can control in Azure AD who has access to LinkedIn Sales Navigator</span></span>
- <span data-ttu-id="4a552-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k LinkedIn prodej Navigátor (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="4a552-107">You can enable your users to automatically get signed-on to LinkedIn Sales Navigator (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4a552-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4a552-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="4a552-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, Procházet [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4a552-109">If you want to know more details about SaaS app integration with Azure AD, browse [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4a552-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4a552-110">Prerequisites</span></span>

<span data-ttu-id="4a552-111">Konfigurace integrace Azure AD s Navigátor prodej LinkedIn, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="4a552-111">To configure Azure AD integration with LinkedIn Sales Navigator, you need the following items:</span></span>

- <span data-ttu-id="4a552-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="4a552-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4a552-113">Organizační jednotky prodej Navigátor LinkedIn jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="4a552-113">A LinkedIn Sales Navigator single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4a552-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4a552-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4a552-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="4a552-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4a552-116">Pokud to není nezbytné, vyhněte se použití produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="4a552-116">Avoid using your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4a552-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4a552-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4a552-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="4a552-118">Scenario description</span></span>
<span data-ttu-id="4a552-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="4a552-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4a552-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="4a552-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4a552-121">Přidání LinkedIn prodej Navigátor z Galerie</span><span class="sxs-lookup"><span data-stu-id="4a552-121">Adding LinkedIn Sales Navigator from the gallery</span></span>
2. <span data-ttu-id="4a552-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="4a552-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-sales-navigator-from-the-gallery"></a><span data-ttu-id="4a552-123">Přidání LinkedIn prodej Navigátor z Galerie</span><span class="sxs-lookup"><span data-stu-id="4a552-123">Adding LinkedIn Sales Navigator from the gallery</span></span>
<span data-ttu-id="4a552-124">Při konfiguraci integrace LinkedIn prodej Navigátor do služby Azure AD potřebujete přidat LinkedIn prodej Navigátor z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="4a552-124">To configure the integration of LinkedIn Sales Navigator into Azure AD, you need to add LinkedIn Sales Navigator from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="4a552-125">**Chcete-li přidat LinkedIn prodej Navigátor z galerie, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="4a552-125">**To add LinkedIn Sales Navigator from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="4a552-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="4a552-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4a552-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="4a552-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="4a552-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4a552-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="4a552-131">Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4a552-131">Click **New application** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="4a552-133">Do vyhledávacího pole zadejte **LinkedIn prodej Navigátor**.</span><span class="sxs-lookup"><span data-stu-id="4a552-133">In the search box, type **LinkedIn Sales Navigator**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_search.png)

5. <span data-ttu-id="4a552-135">Na panelu výsledků vyberte **LinkedIn prodej Navigátor**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4a552-135">In the results panel, select **LinkedIn Sales Navigator**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4a552-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="4a552-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4a552-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Navigátor LinkedIn prodeje podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="4a552-138">In this section, you configure and test Azure AD single sign-on with LinkedIn Sales Navigator based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4a552-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v LinkedIn prodej navigátoru je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4a552-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LinkedIn Sales Navigator is to a user in Azure AD.</span></span> <span data-ttu-id="4a552-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v LinkedIn prodej Navigátor musí navázat.</span><span class="sxs-lookup"><span data-stu-id="4a552-140">In other words, a link relationship between an Azure AD user and the related user in LinkedIn Sales Navigator needs to be established.</span></span>

<span data-ttu-id="4a552-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Navigátor prodej LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="4a552-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in LinkedIn Sales Navigator.</span></span>

<span data-ttu-id="4a552-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Navigátor LinkedIn prodeje, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="4a552-142">To configure and test Azure AD single sign-on with LinkedIn Sales Navigator, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="4a552-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="4a552-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="4a552-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4a552-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4a552-145">**[Vytvoření zkušebního uživatele LinkedIn prodej Navigátor](#creating-a-linkedin-sales-navigator-test-user)**  – Pokud chcete mít protějšek Britta Simon v Navigátor LinkedIn prodej, propojené služby Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="4a552-145">**[Creating a LinkedIn Sales Navigator test user](#creating-a-linkedin-sales-navigator-test-user)** - to have a counterpart of Britta Simon in LinkedIn Sales Navigator that is linked to the Azure AD representation of the user.</span></span>
4. <span data-ttu-id="4a552-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4a552-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4a552-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4a552-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4a552-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="4a552-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4a552-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Navigátor prodej LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="4a552-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LinkedIn Sales Navigator application.</span></span>

<span data-ttu-id="4a552-150">**Ke konfiguraci Azure AD jednotné přihlašování s Navigátor prodej LinkedIn, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="4a552-150">**To configure Azure AD single sign-on with LinkedIn Sales Navigator, perform the following steps:**</span></span>

1. <span data-ttu-id="4a552-151">Na portálu Azure na **LinkedIn prodej Navigátor** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="4a552-151">In the Azure portal, on the **LinkedIn Sales Navigator** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="4a552-153">Na **jednotného přihlašování** dialogové okno, v **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4a552-153">On the **Single sign-on** dialog, in **Mode** select **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_samlbase.png)

3. <span data-ttu-id="4a552-155">V okně prohlížeče jiný web, přihlašování k vaší **LinkedIn prodej Navigátor** webu jako správce.</span><span class="sxs-lookup"><span data-stu-id="4a552-155">In a different web browser window, sign-on to your **LinkedIn Sales Navigator** website as an administrator.</span></span>

4. <span data-ttu-id="4a552-156">V **centra účtů**, klikněte na tlačítko **globální nastavení** pod **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="4a552-156">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="4a552-157">Kromě toho **prodej Navigátor** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="4a552-157">Also, select **Sales Navigator** from the dropdown list.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

5. <span data-ttu-id="4a552-159">Klikněte na tlačítko **nebo klikněte na tlačítko sem můžete načíst a zkopírujte jednotlivých polí z formuláře** a zkopírujte **Entity Id** a **adresu Url pro přístup k příjemce Assertion (ACS)**.</span><span class="sxs-lookup"><span data-stu-id="4a552-159">Click **OR Click Here to load and copy individual fields from the form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

6. <span data-ttu-id="4a552-161">Na portálu Azure v části **LinkedIn prodej Navigátor domény a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu.</span><span class="sxs-lookup"><span data-stu-id="4a552-161">On Azure portal, under **LinkedIn Sales Navigator Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url1.png)

    <span data-ttu-id="4a552-163">a.</span><span class="sxs-lookup"><span data-stu-id="4a552-163">a.</span></span> <span data-ttu-id="4a552-164">V **identifikátor** textovému poli, zadejte **Entity ID** zkopírovat z portálu LinkedIn</span><span class="sxs-lookup"><span data-stu-id="4a552-164">In the **Identifier** textbox, enter the **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="4a552-165">b.</span><span class="sxs-lookup"><span data-stu-id="4a552-165">b.</span></span> <span data-ttu-id="4a552-166">V **adresa URL odpovědi** textovému poli, zadejte **adresu Url pro přístup k příjemce Assertion (ACS)** zkopírovat z portálu LinkedIn</span><span class="sxs-lookup"><span data-stu-id="4a552-166">In the **Reply URL** textbox, enter the **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="4a552-167">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu.</span><span class="sxs-lookup"><span data-stu-id="4a552-167">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url2.png)

    <span data-ttu-id="4a552-169">V **přihlašovací adresa URL** textovému poli, zadejte hodnotu pomocí následujícího vzorce:`https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`</span><span class="sxs-lookup"><span data-stu-id="4a552-169">In the **Sign-on URL** textbox, type the value using the following pattern: `https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`</span></span>

8. <span data-ttu-id="4a552-170">Vaše **LinkedIn prodej Navigátor** aplikace očekává SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, můžete přidat mapování vlastních atributů do vaší konfigurace atributy tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="4a552-170">Your **LinkedIn Sales Navigator** application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="4a552-171">Následující snímek obrazovky ukazuje příklad.</span><span class="sxs-lookup"><span data-stu-id="4a552-171">The following screenshot shows an example.</span></span> <span data-ttu-id="4a552-172">Výchozí hodnota **uživatelský identifikátor** je **user.userprincipalname** ale LinkedIn prodej Navigátor očekává, že nejde mapovat pomocí e-mailovou adresu uživatele.</span><span class="sxs-lookup"><span data-stu-id="4a552-172">The default value of **User Identifier** is **user.userprincipalname** but LinkedIn Sales Navigator expects it to be mapped with the user's email address.</span></span> <span data-ttu-id="4a552-173">Můžete použít **user.mail** atribut ze seznamu, nebo použijte hodnotu odpovídajícího atributu na základě konfigurace vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="4a552-173">You can use **user.mail** attribute from the list or use the appropriate attribute value based on your organization configuration.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/updateusermail.png)
    
9. <span data-ttu-id="4a552-175">V **uživatelské atributy** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** a nastavit atributy.</span><span class="sxs-lookup"><span data-stu-id="4a552-175">In **User Attributes** section, click **View and edit all other user attributes** and set the attributes.</span></span> <span data-ttu-id="4a552-176">Uživatel musí přidat čtyři deklarace identity s názvem **e-mailu**, **oddělení**, **firstname**, a **lastname** a hodnota má být namapována na **user.mail**, **user.department**, **user.givenname**, a **user.surname** v uvedeném pořadí</span><span class="sxs-lookup"><span data-stu-id="4a552-176">The user needs to add four claims named **email**, **department**, **firstname**, and **lastname** and the value is to be mapped with **user.mail**, **user.department**, **user.givenname**, and **user.surname** respectively</span></span>

    | <span data-ttu-id="4a552-177">Název atributu</span><span class="sxs-lookup"><span data-stu-id="4a552-177">Attribute Name</span></span> | <span data-ttu-id="4a552-178">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="4a552-178">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="4a552-179">E-mailu</span><span class="sxs-lookup"><span data-stu-id="4a552-179">email</span></span>| <span data-ttu-id="4a552-180">User.Mail</span><span class="sxs-lookup"><span data-stu-id="4a552-180">user.mail</span></span> |
    | <span data-ttu-id="4a552-181">Oddělení</span><span class="sxs-lookup"><span data-stu-id="4a552-181">department</span></span>| <span data-ttu-id="4a552-182">User.Department</span><span class="sxs-lookup"><span data-stu-id="4a552-182">user.department</span></span> |
    | <span data-ttu-id="4a552-183">FirstName</span><span class="sxs-lookup"><span data-stu-id="4a552-183">firstname</span></span>| <span data-ttu-id="4a552-184">User.givenName</span><span class="sxs-lookup"><span data-stu-id="4a552-184">user.givenname</span></span> |
    | <span data-ttu-id="4a552-185">Příjmení</span><span class="sxs-lookup"><span data-stu-id="4a552-185">lastname</span></span>| <span data-ttu-id="4a552-186">User.Surname</span><span class="sxs-lookup"><span data-stu-id="4a552-186">user.surname</span></span> |
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/userattribute.png)
    
    <span data-ttu-id="4a552-188">a.</span><span class="sxs-lookup"><span data-stu-id="4a552-188">a.</span></span> <span data-ttu-id="4a552-189">Klikněte na **přidat atribut** tím otevřete dialogové okno atribut.</span><span class="sxs-lookup"><span data-stu-id="4a552-189">Click on **Add Attribute** to open the attribute dialog.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_04.png)
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_05.png)
   
    <span data-ttu-id="4a552-192">b.</span><span class="sxs-lookup"><span data-stu-id="4a552-192">b.</span></span> <span data-ttu-id="4a552-193">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="4a552-193">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="4a552-194">c.</span><span class="sxs-lookup"><span data-stu-id="4a552-194">c.</span></span> <span data-ttu-id="4a552-195">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="4a552-195">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="4a552-196">d.</span><span class="sxs-lookup"><span data-stu-id="4a552-196">d.</span></span> <span data-ttu-id="4a552-197">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="4a552-197">Click **Ok**</span></span>

10. <span data-ttu-id="4a552-198">Proveďte následující kroky na **název** atribut -</span><span class="sxs-lookup"><span data-stu-id="4a552-198">Perform the following steps on the **name** attribute-</span></span>

    <span data-ttu-id="4a552-199">a.</span><span class="sxs-lookup"><span data-stu-id="4a552-199">a.</span></span> <span data-ttu-id="4a552-200">Klikněte na atribut, který se otevře **Upravit atribut** okno.</span><span class="sxs-lookup"><span data-stu-id="4a552-200">Click on the attribute to open the **Edit Attribute** window.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/url_update.png)

    <span data-ttu-id="4a552-202">b.</span><span class="sxs-lookup"><span data-stu-id="4a552-202">b.</span></span> <span data-ttu-id="4a552-203">Odstranit hodnotu adresy URL **obor názvů**.</span><span class="sxs-lookup"><span data-stu-id="4a552-203">Delete the URL value from the **namespace**.</span></span>
    
    <span data-ttu-id="4a552-204">c.</span><span class="sxs-lookup"><span data-stu-id="4a552-204">c.</span></span> <span data-ttu-id="4a552-205">Klikněte na tlačítko **Ok** uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="4a552-205">Click **Ok** to save the setting.</span></span>

11. <span data-ttu-id="4a552-206">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="4a552-206">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_certificate.png) 

12. <span data-ttu-id="4a552-208">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4a552-208">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_400.png)

13. <span data-ttu-id="4a552-210">Přejděte na **nastavení správce LinkedIn** části.</span><span class="sxs-lookup"><span data-stu-id="4a552-210">Go to **LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="4a552-211">Klikněte na tlačítko **soubor nahrát XML** nahrát soubor XML s metadaty soubor, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4a552-211">Click **Upload XML file** to upload the Metadata XML file that you have downloaded from the Azure portal.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

14. <span data-ttu-id="4a552-213">Klikněte na tlačítko **na** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4a552-213">Click **On** to enable SSO.</span></span> <span data-ttu-id="4a552-214">Jednotné přihlašování stav se změní z **Nepřipojeno** k **připojeno**</span><span class="sxs-lookup"><span data-stu-id="4a552-214">SSO status changes from **Not Connected** to **Connected**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)


> [!TIP]
> <span data-ttu-id="4a552-216">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="4a552-216">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="4a552-217">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="4a552-217">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="4a552-218">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4a552-218">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4a552-219">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="4a552-219">Creating an Azure AD test user</span></span>
<span data-ttu-id="4a552-220">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4a552-220">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="4a552-222">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="4a552-222">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="4a552-223">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="4a552-223">In the **Azure  portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4a552-225">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="4a552-225">Go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4a552-227">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4a552-227">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4a552-229">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4a552-229">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4a552-231">a.</span><span class="sxs-lookup"><span data-stu-id="4a552-231">a.</span></span> <span data-ttu-id="4a552-232">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4a552-232">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4a552-233">b.</span><span class="sxs-lookup"><span data-stu-id="4a552-233">b.</span></span> <span data-ttu-id="4a552-234">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4a552-234">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4a552-235">c.</span><span class="sxs-lookup"><span data-stu-id="4a552-235">c.</span></span> <span data-ttu-id="4a552-236">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="4a552-236">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="4a552-237">d.</span><span class="sxs-lookup"><span data-stu-id="4a552-237">d.</span></span> <span data-ttu-id="4a552-238">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4a552-238">Click **Create**.</span></span>
 
### <a name="creating-a-linkedin-sales-navigator-test-user"></a><span data-ttu-id="4a552-239">Vytvoření zkušebního uživatele LinkedIn prodej Navigátor</span><span class="sxs-lookup"><span data-stu-id="4a552-239">Creating a LinkedIn Sales Navigator test user</span></span>

<span data-ttu-id="4a552-240">Propojené aplikace Navigátor prodej podporuje pouze v zřizování uživatelů čas (JIT) a po ověření uživatelé jsou automaticky vytvořené v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4a552-240">Linked Sales Navigator Application supports Just in Time (JIT) user provisioning and after authentication users are created in the application automatically.</span></span> <span data-ttu-id="4a552-241">Aktivovat **automaticky přiřadit licence** přiřadit licence pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="4a552-241">Activate **Automatically assign licenses** to assign a license to the user.</span></span>
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="4a552-243">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="4a552-243">Assigning the Azure AD test user</span></span>

<span data-ttu-id="4a552-244">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Navigátor prodej LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="4a552-244">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LinkedIn Sales Navigator.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="4a552-246">**Pokud chcete přiřadit Britta Simon Navigátor prodej LinkedIn, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="4a552-246">**To assign Britta Simon to LinkedIn Sales Navigator, perform the following steps:**</span></span>

1. <span data-ttu-id="4a552-247">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4a552-247">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="4a552-249">V seznamu aplikací vyberte **LinkedIn prodej Navigátor**.</span><span class="sxs-lookup"><span data-stu-id="4a552-249">In the applications list, select **LinkedIn Sales Navigator**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_app.png) 

3. <span data-ttu-id="4a552-251">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="4a552-251">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="4a552-253">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4a552-253">Click **Add** button.</span></span> <span data-ttu-id="4a552-254">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4a552-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="4a552-256">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="4a552-256">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="4a552-257">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4a552-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4a552-258">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4a552-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4a552-259">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="4a552-259">Testing single sign-on</span></span>

<span data-ttu-id="4a552-260">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="4a552-260">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="4a552-261">Když kliknete na dlaždici LinkedIn prodej Navigátor na přístupovém panelu, přesměrovat na organizační stránku, kde je nutné zadat osobní podrobnosti o účtu LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="4a552-261">When you click the LinkedIn Sales Navigator tile in the Access Panel, you should be redirected to Organizational page where you have to provide your personal LinkedIn account details.</span></span> <span data-ttu-id="4a552-262">Ho propojí s vaším účtem obchodní LinkedIn svůj osobní účet.</span><span class="sxs-lookup"><span data-stu-id="4a552-262">It links your personal account with your LinkedIn business account.</span></span> <span data-ttu-id="4a552-263">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="4a552-263">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="4a552-264">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4a552-264">Additional resources</span></span>

* [<span data-ttu-id="4a552-265">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4a552-265">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4a552-266">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4a552-266">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_203.png

