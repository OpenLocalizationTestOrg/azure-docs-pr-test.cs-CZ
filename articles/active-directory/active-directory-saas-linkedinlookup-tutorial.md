---
title: "Kurz: Azure Active Directory integrace s LinkedIn vyhledávání | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a vyhledávání LinkedIn."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a2757a39-1ead-4a3e-91e4-270be3055683
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: e296431866a8611b30e72f286884890adf0f7e50
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-lookup"></a><span data-ttu-id="ce69b-103">Kurz: Azure Active Directory integrace s LinkedIn vyhledávání</span><span class="sxs-lookup"><span data-stu-id="ce69b-103">Tutorial: Azure Active Directory integration with LinkedIn Lookup</span></span>

<span data-ttu-id="ce69b-104">V tomto kurzu zjistěte, jak integrovat LinkedIn vyhledávání s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ce69b-104">In this tutorial, you learn how to integrate LinkedIn Lookup with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ce69b-105">Integrace vyhledávání LinkedIn s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ce69b-105">Integrating LinkedIn Lookup with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ce69b-106">Můžete řídit ve službě Azure AD, který má přístup k vyhledávání LinkedIn</span><span class="sxs-lookup"><span data-stu-id="ce69b-106">You can control in Azure AD who has access to LinkedIn Lookup</span></span>
- <span data-ttu-id="ce69b-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k vyhledávání LinkedIn (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="ce69b-107">You can enable your users to automatically get signed-on to LinkedIn Lookup (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ce69b-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ce69b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ce69b-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ce69b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ce69b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ce69b-110">Prerequisites</span></span>

<span data-ttu-id="ce69b-111">Konfigurace integrace Azure AD s LinkedIn vyhledávání, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="ce69b-111">To configure Azure AD integration with LinkedIn Lookup, you need the following items:</span></span>

- <span data-ttu-id="ce69b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ce69b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ce69b-113">Vyhledávání LinkedIn jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ce69b-113">An LinkedIn Lookup single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ce69b-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ce69b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ce69b-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ce69b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ce69b-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ce69b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ce69b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ce69b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ce69b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ce69b-118">Scenario description</span></span>
<span data-ttu-id="ce69b-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ce69b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ce69b-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ce69b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ce69b-121">Přidání LinkedIn vyhledávání z Galerie</span><span class="sxs-lookup"><span data-stu-id="ce69b-121">Adding LinkedIn Lookup from the gallery</span></span>
2. <span data-ttu-id="ce69b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ce69b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-lookup-from-the-gallery"></a><span data-ttu-id="ce69b-123">Přidání LinkedIn vyhledávání z Galerie</span><span class="sxs-lookup"><span data-stu-id="ce69b-123">Adding LinkedIn Lookup from the gallery</span></span>
<span data-ttu-id="ce69b-124">Konfigurace integrace vyhledávání LinkedIn do Azure AD, musíte přidat do seznamu spravovaných aplikací SaaS LinkedIn vyhledávání z galerie.</span><span class="sxs-lookup"><span data-stu-id="ce69b-124">To configure the integration of LinkedIn Lookup into Azure AD, you need to add LinkedIn Lookup from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ce69b-125">**Pokud chcete přidat LinkedIn vyhledávání z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ce69b-125">**To add LinkedIn Lookup from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ce69b-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ce69b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ce69b-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ce69b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ce69b-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ce69b-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="ce69b-131">Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno pro přidání nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ce69b-131">Click **New application** button on the top of the dialog to add new application.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="ce69b-133">Do vyhledávacího pole zadejte **LinkedIn vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="ce69b-133">In the search box, type **LinkedIn Lookup**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_search.png)

5. <span data-ttu-id="ce69b-135">Na panelu výsledků vyberte **LinkedIn vyhledávání**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ce69b-135">In the results panel, select **LinkedIn Lookup**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ce69b-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ce69b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ce69b-138">V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s LinkedIn vyhledávání podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="ce69b-138">In this section, you configure and test Azure AD single sign-on with LinkedIn Lookup based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ce69b-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v LinkedIn vyhledávání je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ce69b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LinkedIn Lookup is to a user in Azure AD.</span></span> <span data-ttu-id="ce69b-140">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské LinkedIn vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ce69b-140">In other words, a link relationship between an Azure AD user and the related user in LinkedIn Lookup needs to be established.</span></span>

<span data-ttu-id="ce69b-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** LinkedIn vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ce69b-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in LinkedIn Lookup.</span></span>

<span data-ttu-id="ce69b-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s LinkedIn vyhledávání, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="ce69b-142">To configure and test Azure AD single sign-on with LinkedIn Lookup, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ce69b-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="ce69b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ce69b-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ce69b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ce69b-145">**[Vytváření testovacího uživatele vyhledávání LinkedIn](#creating-an-linkedin-lookup-test-user)**  – Pokud chcete mít protějšek Britta Simon v LinkedIn vyhledávání, které je propojena k reprezentaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ce69b-145">**[Creating an LinkedIn Lookup test user](#creating-an-linkedin-lookup-test-user)** - to have a counterpart of Britta Simon in LinkedIn Lookup that is linked to the Azure AD representation.</span></span>
4. <span data-ttu-id="ce69b-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ce69b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ce69b-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ce69b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ce69b-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ce69b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ce69b-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci LinkedIn vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ce69b-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LinkedIn Lookup application.</span></span>

<span data-ttu-id="ce69b-150">**Ke konfiguraci Azure AD jednotné přihlašování s LinkedIn vyhledávání, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ce69b-150">**To configure Azure AD single sign-on with LinkedIn Lookup, perform the following steps:**</span></span>

1. <span data-ttu-id="ce69b-151">Na portálu Azure na **LinkedIn vyhledávání** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ce69b-151">In the Azure portal, on the **LinkedIn Lookup** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="ce69b-153">Na **jednotného přihlašování** dialogové okno, v **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ce69b-153">On the **Single sign-on** dialog, in **Mode** select **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_samlbase.png)

3. <span data-ttu-id="ce69b-155">V okně prohlížeče jiný web, přihlašování k vaší **LinkedIn vyhledávání** webu jako správce.</span><span class="sxs-lookup"><span data-stu-id="ce69b-155">In a different web browser window, sign-on to your **LinkedIn Lookup** website as an administrator.</span></span>

4. <span data-ttu-id="ce69b-156">V **centra účtů**, klikněte na tlačítko **globální nastavení** pod **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="ce69b-156">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="ce69b-157">Kromě toho **vyhledávání** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="ce69b-157">Also, select **Lookup** from the dropdown list.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_011.png)

5. <span data-ttu-id="ce69b-159">Klikněte na tlačítko **nebo klikněte na tlačítko sem můžete načíst a zkopírujte jednotlivých polí z formuláře** a zkopírujte **Entity Id** a **adresu Url pro přístup k příjemce Assertion (ACS)**</span><span class="sxs-lookup"><span data-stu-id="ce69b-159">Click **OR Click Here to load and copy individual fields from the form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_032.png)

6. <span data-ttu-id="ce69b-161">Na portálu Azure v části **LinkedIn vyhledávání domény a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="ce69b-161">On Azure portal, under **LinkedIn Lookup Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url.png)

    <span data-ttu-id="ce69b-163">a.</span><span class="sxs-lookup"><span data-stu-id="ce69b-163">a.</span></span> <span data-ttu-id="ce69b-164">V **identifikátor** textovému poli, zadejte **Entity ID** zkopírovat z portálu LinkedIn</span><span class="sxs-lookup"><span data-stu-id="ce69b-164">In the **Identifier** textbox, enter the **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="ce69b-165">b.</span><span class="sxs-lookup"><span data-stu-id="ce69b-165">b.</span></span> <span data-ttu-id="ce69b-166">V **adresa URL odpovědi** textovému poli, zadejte **adresu Url pro přístup k příjemce Assertion (ACS)** zkopírovat z portálu LinkedIn</span><span class="sxs-lookup"><span data-stu-id="ce69b-166">In the **Reply URL** textbox, enter the **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="ce69b-167">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="ce69b-167">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url1.png)

    <span data-ttu-id="ce69b-169">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://www.linkedIn.com/checkpoint/enterprise/login/<AccountId>?application=lookup`</span><span class="sxs-lookup"><span data-stu-id="ce69b-169">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://www.linkedIn.com/checkpoint/enterprise/login/<AccountId>?application=lookup`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="ce69b-170">Toto není skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ce69b-170">This is not real value.</span></span> <span data-ttu-id="ce69b-171">Uživatel má tyto hodnoty aktualizovat s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ce69b-171">The user has to update these values with the actual Sign-On URL.</span></span> <span data-ttu-id="ce69b-172">Obraťte se na [tým podpory klienta sítě LinkedIn vyhledávání](https://business.LinkedIn.com/lookup) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ce69b-172">Contact [LinkedIn Lookup Client support team](https://business.LinkedIn.com/lookup) to get this value.</span></span>

8. <span data-ttu-id="ce69b-173">Vaše **LinkedIn vyhledávání** aplikace očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="ce69b-173">Your **LinkedIn Lookup** application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="ce69b-174">Uživatel má můžete přidat mapování vlastních atributů ke konfiguraci atributy tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="ce69b-174">The user has to add custom attribute mappings to the SAML token attributes configuration.</span></span> <span data-ttu-id="ce69b-175">Následující snímek obrazovky ukazuje příklad.</span><span class="sxs-lookup"><span data-stu-id="ce69b-175">The following screenshot shows an example.</span></span> <span data-ttu-id="ce69b-176">Výchozí hodnota **uživatelský identifikátor** je **user.userprincipalname** ale LinkedIn vyhledávání očekává to nejde mapovat pomocí e-mailovou adresu uživatele.</span><span class="sxs-lookup"><span data-stu-id="ce69b-176">The default value of **User Identifier** is **user.userprincipalname** but LinkedIn Lookup expects this to be mapped with the user's email address.</span></span> <span data-ttu-id="ce69b-177">Můžete použít **user.mail** atribut ze seznamu, nebo použijte hodnotu odpovídajícího atributu na základě konfigurace vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="ce69b-177">You can use **user.mail** attribute from the list or use the appropriate attribute value based on your organization configuration.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/updateusermail.png)
    
9. <span data-ttu-id="ce69b-179">V **uživatelské atributy** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** a nastavit atributy.</span><span class="sxs-lookup"><span data-stu-id="ce69b-179">In **User Attributes** section, click **View and edit all other user attributes** and set the attributes.</span></span> <span data-ttu-id="ce69b-180">Uživatel musí přidat čtyři deklarace identity s názvem **e-mailu**, **oddělení**, **firstname**, a **lastname** a hodnota má být namapována na **user.mail**, **user.department**, **user.givenname**, a **user.surname** v uvedeném pořadí</span><span class="sxs-lookup"><span data-stu-id="ce69b-180">The user needs to add four claims named **email**,  **department**, **firstname**, and **lastname** and the value is to be mapped with **user.mail**, **user.department**, **user.givenname**, and **user.surname** respectively</span></span>

    | <span data-ttu-id="ce69b-181">Název atributu</span><span class="sxs-lookup"><span data-stu-id="ce69b-181">Attribute Name</span></span> | <span data-ttu-id="ce69b-182">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="ce69b-182">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="ce69b-183">E-mailu</span><span class="sxs-lookup"><span data-stu-id="ce69b-183">email</span></span>| <span data-ttu-id="ce69b-184">User.Mail</span><span class="sxs-lookup"><span data-stu-id="ce69b-184">user.mail</span></span> |    
    | <span data-ttu-id="ce69b-185">Oddělení</span><span class="sxs-lookup"><span data-stu-id="ce69b-185">department</span></span>| <span data-ttu-id="ce69b-186">User.Department</span><span class="sxs-lookup"><span data-stu-id="ce69b-186">user.department</span></span> |
    | <span data-ttu-id="ce69b-187">FirstName</span><span class="sxs-lookup"><span data-stu-id="ce69b-187">firstname</span></span>| <span data-ttu-id="ce69b-188">User.givenName</span><span class="sxs-lookup"><span data-stu-id="ce69b-188">user.givenname</span></span> |
    | <span data-ttu-id="ce69b-189">Příjmení</span><span class="sxs-lookup"><span data-stu-id="ce69b-189">lastname</span></span>| <span data-ttu-id="ce69b-190">User.Surname</span><span class="sxs-lookup"><span data-stu-id="ce69b-190">user.surname</span></span> |

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/userattribute.png)

    <span data-ttu-id="ce69b-192">a.</span><span class="sxs-lookup"><span data-stu-id="ce69b-192">a.</span></span> <span data-ttu-id="ce69b-193">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ce69b-193">Click **Add Attribute** to open the **Add Attribute** dialog.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/4.png)
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/5.png)
   
    <span data-ttu-id="ce69b-196">b.</span><span class="sxs-lookup"><span data-stu-id="ce69b-196">b.</span></span> <span data-ttu-id="ce69b-197">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="ce69b-197">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="ce69b-198">c.</span><span class="sxs-lookup"><span data-stu-id="ce69b-198">c.</span></span> <span data-ttu-id="ce69b-199">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="ce69b-199">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="ce69b-200">d.</span><span class="sxs-lookup"><span data-stu-id="ce69b-200">d.</span></span> <span data-ttu-id="ce69b-201">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="ce69b-201">Click **Ok**</span></span>

10. <span data-ttu-id="ce69b-202">Proveďte následující kroky na **název** atribut -</span><span class="sxs-lookup"><span data-stu-id="ce69b-202">Perform the following steps on the **name** attribute-</span></span>

    <span data-ttu-id="ce69b-203">a.</span><span class="sxs-lookup"><span data-stu-id="ce69b-203">a.</span></span> <span data-ttu-id="ce69b-204">Klikněte na atribut, který se otevře **Upravit atribut** okno.</span><span class="sxs-lookup"><span data-stu-id="ce69b-204">Click on the attribute to open the **Edit Attribute** window.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/url_update.png)

    <span data-ttu-id="ce69b-206">b.</span><span class="sxs-lookup"><span data-stu-id="ce69b-206">b.</span></span> <span data-ttu-id="ce69b-207">Odstranit hodnotu adresy URL **obor názvů**.</span><span class="sxs-lookup"><span data-stu-id="ce69b-207">Delete the URL value from the **namespace**.</span></span>
    
    <span data-ttu-id="ce69b-208">c.</span><span class="sxs-lookup"><span data-stu-id="ce69b-208">c.</span></span> <span data-ttu-id="ce69b-209">Klikněte na tlačítko **Ok** uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="ce69b-209">Click **Ok** to save the setting.</span></span>

10. <span data-ttu-id="ce69b-210">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ce69b-210">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_certificate.png) 

11. <span data-ttu-id="ce69b-212">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ce69b-212">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_400.png)

12. <span data-ttu-id="ce69b-214">Přejděte na **nastavení správce LinkedIn** části.</span><span class="sxs-lookup"><span data-stu-id="ce69b-214">Go to **LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="ce69b-215">Nahrát soubor XML, který jste si stáhli z portálu Azure kliknutím **soubor nahrát XML** možnost.</span><span class="sxs-lookup"><span data-stu-id="ce69b-215">Upload the XML file you downloaded from the Azure portal by clicking the **Upload XML file** option.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_metadata_03.png)

13. <span data-ttu-id="ce69b-217">Klikněte na tlačítko **na** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ce69b-217">Click **On** to enable SSO.</span></span> <span data-ttu-id="ce69b-218">Jednotné přihlašování stav se změní z **Nepřipojeno** k **připojeno**</span><span class="sxs-lookup"><span data-stu-id="ce69b-218">SSO status changes from **Not Connected** to **Connected**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_admin_05.png)

> [!TIP]
> <span data-ttu-id="ce69b-220">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="ce69b-220">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ce69b-221">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="ce69b-221">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ce69b-222">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ce69b-222">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ce69b-223">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ce69b-223">Creating an Azure AD test user</span></span>
<span data-ttu-id="ce69b-224">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ce69b-224">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="ce69b-226">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ce69b-226">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ce69b-227">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ce69b-227">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ce69b-229">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ce69b-229">Go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ce69b-231">Klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ce69b-231">Click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ce69b-233">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ce69b-233">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ce69b-235">a.</span><span class="sxs-lookup"><span data-stu-id="ce69b-235">a.</span></span> <span data-ttu-id="ce69b-236">V **název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="ce69b-236">In the **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="ce69b-237">b.</span><span class="sxs-lookup"><span data-stu-id="ce69b-237">b.</span></span> <span data-ttu-id="ce69b-238">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ce69b-238">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="ce69b-239">c.</span><span class="sxs-lookup"><span data-stu-id="ce69b-239">c.</span></span> <span data-ttu-id="ce69b-240">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="ce69b-240">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ce69b-241">d.</span><span class="sxs-lookup"><span data-stu-id="ce69b-241">d.</span></span> <span data-ttu-id="ce69b-242">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ce69b-242">Click **Create**.</span></span>
 
### <a name="creating-an-linkedin-lookup-test-user"></a><span data-ttu-id="ce69b-243">Vytváření testovacího uživatele vyhledávání LinkedIn</span><span class="sxs-lookup"><span data-stu-id="ce69b-243">Creating an LinkedIn Lookup test user</span></span>

<span data-ttu-id="ce69b-244">Propojené vyhledávání aplikace podporuje pouze v zřizování uživatelů čas (JIT) a po ověření uživatelé jsou automaticky vytvořené v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ce69b-244">Linked Lookup Application supports Just in Time (JIT) user provisioning and after authentication users are created in the application automatically.</span></span> <span data-ttu-id="ce69b-245">Aktivovat **automaticky přiřadit licence** přiřadit licence pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="ce69b-245">Activate **Automatically assign licenses** to assign a license to the user.</span></span>
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedin_admin_license.png)

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ce69b-247">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ce69b-247">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ce69b-248">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k vyhledávání LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="ce69b-248">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LinkedIn Lookup.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ce69b-250">**Pokud chcete přiřadit Britta Simon LinkedIn vyhledávání, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ce69b-250">**To assign Britta Simon to LinkedIn Lookup, perform the following steps:**</span></span>

1. <span data-ttu-id="ce69b-251">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ce69b-251">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ce69b-253">V seznamu aplikací vyberte **LinkedIn vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="ce69b-253">In the applications list, select **LinkedIn Lookup**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_app.png) 

3. <span data-ttu-id="ce69b-255">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ce69b-255">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ce69b-257">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ce69b-257">Click **Add** button.</span></span> <span data-ttu-id="ce69b-258">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ce69b-258">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ce69b-260">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ce69b-260">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ce69b-261">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ce69b-261">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ce69b-262">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ce69b-262">Click **Assign** button on **Add Assignment** dialog.</span></span>

    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ce69b-263">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ce69b-263">Testing single sign-on</span></span>

<span data-ttu-id="ce69b-264">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ce69b-264">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="ce69b-265">Když kliknete na dlaždici LinkedIn vyhledávání na přístupovém panelu, přesměrovat na organizační stránku, kde je nutné zadat osobní podrobnosti o účtu LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="ce69b-265">When you click the LinkedIn Lookup tile in the Access Panel, you should be redirected to Organizational page where you have to provide your personal LinkedIn account details.</span></span> <span data-ttu-id="ce69b-266">Ho propojí s vaším účtem obchodní LinkedIn svůj osobní účet.</span><span class="sxs-lookup"><span data-stu-id="ce69b-266">It links your personal account with your LinkedIn business account.</span></span> 

<span data-ttu-id="ce69b-267">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ce69b-267">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ce69b-268">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ce69b-268">Additional resources</span></span>

* [<span data-ttu-id="ce69b-269">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ce69b-269">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ce69b-270">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ce69b-270">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_203.png

