---
title: "Kurz: Azure Active Directory integrace s zvýšení oprávnění LinkedIn | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a zvýšení oprávnění LinkedIn."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeedes
ms.openlocfilehash: 5336543e06d60be555722a615568b12048c2aa2f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-elevate"></a><span data-ttu-id="03895-103">Kurz: Azure Active Directory integrace s LinkedIn zvýšení oprávnění</span><span class="sxs-lookup"><span data-stu-id="03895-103">Tutorial: Azure Active Directory integration with LinkedIn Elevate</span></span>

<span data-ttu-id="03895-104">V tomto kurzu zjistěte, jak integrovat LinkedIn zvýšení oprávnění v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="03895-104">In this tutorial, you learn how to integrate LinkedIn Elevate with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="03895-105">Integrace zvýšení oprávnění LinkedIn s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="03895-105">Integrating LinkedIn Elevate with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="03895-106">Můžete řídit ve službě Azure AD, který má přístup ke zvýšení oprávnění LinkedIn</span><span class="sxs-lookup"><span data-stu-id="03895-106">You can control in Azure AD who has access to LinkedIn Elevate</span></span>
- <span data-ttu-id="03895-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k LinkedIn zvýšení oprávnění (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="03895-107">You can enable your users to automatically get signed-on to LinkedIn Elevate (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="03895-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="03895-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="03895-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="03895-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="03895-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="03895-110">Prerequisites</span></span>

<span data-ttu-id="03895-111">Konfigurace integrace Azure AD s LinkedIn zvýšení oprávnění, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="03895-111">To configure Azure AD integration with LinkedIn Elevate, you need the following items:</span></span>

- <span data-ttu-id="03895-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="03895-112">An Azure AD subscription</span></span>
- <span data-ttu-id="03895-113">Zvýšení oprávnění LinkedIn jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="03895-113">A LinkedIn Elevate single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="03895-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="03895-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="03895-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="03895-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="03895-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="03895-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="03895-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="03895-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="03895-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="03895-118">Scenario description</span></span>
<span data-ttu-id="03895-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="03895-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="03895-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="03895-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="03895-121">Přidání LinkedIn zvýšení oprávnění z Galerie</span><span class="sxs-lookup"><span data-stu-id="03895-121">Adding LinkedIn Elevate from the gallery</span></span>
2. <span data-ttu-id="03895-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="03895-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-elevate-from-the-gallery"></a><span data-ttu-id="03895-123">Přidání LinkedIn zvýšení oprávnění z Galerie</span><span class="sxs-lookup"><span data-stu-id="03895-123">Adding LinkedIn Elevate from the gallery</span></span>
<span data-ttu-id="03895-124">Chcete-li nakonfigurovat integraci zvýšení oprávnění LinkedIn do služby Azure AD, přidejte LinkedIn zvýšení oprávnění z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="03895-124">To configure the integration of LinkedIn Elevate into Azure AD, you need to add LinkedIn Elevate from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="03895-125">**Chcete-li přidat LinkedIn zvýšení oprávnění z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="03895-125">**To add LinkedIn Elevate from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="03895-126">V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="03895-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="03895-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="03895-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="03895-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="03895-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="03895-131">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="03895-131">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="03895-133">Do vyhledávacího pole zadejte **zvýšení oprávnění LinkedIn**.</span><span class="sxs-lookup"><span data-stu-id="03895-133">In the search box, type **LinkedIn Elevate**.</span></span> <span data-ttu-id="03895-134">Na panelu výsledků klikněte na **zvýšení oprávnění LinkedIn** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="03895-134">From results panel, click **LinkedIn Elevate** to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="03895-136">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="03895-136">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="03895-137">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s LinkedIn zvýšení oprávnění na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="03895-137">In this section, you configure and test Azure AD single sign-on with LinkedIn Elevate based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="03895-138">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v LinkedIn zvýšení oprávnění je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="03895-138">For single sign-on to work, Azure AD needs to know what the counterpart user in LinkedIn Elevate is to a user in Azure AD.</span></span> <span data-ttu-id="03895-139">Jinými slovy musí navázat odkaz vztah mezi uživatele Azure AD a související uživatelské v LinkedIn zvýšení oprávnění.</span><span class="sxs-lookup"><span data-stu-id="03895-139">In other words, a link relationship between an Azure AD user and the related user in LinkedIn Elevate needs to be established.</span></span>

<span data-ttu-id="03895-140">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v LinkedIn zvýšení oprávnění.</span><span class="sxs-lookup"><span data-stu-id="03895-140">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in LinkedIn Elevate.</span></span>

<span data-ttu-id="03895-141">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s LinkedIn zvýšení oprávnění, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="03895-141">To configure and test Azure AD single sign-on with LinkedIn Elevate, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="03895-142">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="03895-142">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="03895-143">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="03895-143">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="03895-144">**[Vytvoření zkušebního uživatele zvýšení oprávnění LinkedIn](#creating-a-linkedin-elevate-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="03895-144">**[Creating a LinkedIn Elevate test user](#creating-a-linkedin-elevate-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="03895-145">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="03895-145">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="03895-146">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="03895-146">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="03895-147">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="03895-147">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="03895-148">V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci LinkedIn zvýšení oprávnění.</span><span class="sxs-lookup"><span data-stu-id="03895-148">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your LinkedIn Elevate application.</span></span>

<span data-ttu-id="03895-149">**Ke konfiguraci Azure AD jednotné přihlašování s LinkedIn zvýšení oprávnění, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="03895-149">**To configure Azure AD single sign-on with LinkedIn Elevate, perform the following steps:**</span></span>

1. <span data-ttu-id="03895-150">Na portálu Azure Management portal na **zvýšení oprávnění LinkedIn** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="03895-150">In the Azure Management portal, on the **LinkedIn Elevate** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="03895-152">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="03895-152">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedin_01.png)

3. <span data-ttu-id="03895-154">V okně prohlížeče jiný web přihlášení jako správce klienta LinkedIn zvýšení oprávnění.</span><span class="sxs-lookup"><span data-stu-id="03895-154">In a different web browser window, sign-on to your LinkedIn Elevate tenant as an administrator.</span></span>

4. <span data-ttu-id="03895-155">V **centra účtů**, klikněte na tlačítko **globální nastavení** pod **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="03895-155">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="03895-156">Kromě toho **zvýšení - zvýšení oprávnění Test AAD** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="03895-156">Also, select **Elevate - Elevate AAD Test** from the dropdown list.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_01.png)

5. <span data-ttu-id="03895-158">Klikněte na **nebo klikněte na tlačítko sem můžete načíst a zkopírujte jednotlivých polí z formuláře** a zkopírujte **Entity Id** a **adresu Url pro přístup k příjemce Assertion (ACS)**</span><span class="sxs-lookup"><span data-stu-id="03895-158">Click on **OR Click Here to load and copy individual fields from the form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_03.png)

6. <span data-ttu-id="03895-160">Na portálu Azure v části **LinkedIn zvýšení oprávnění domény a adresy URL**, proveďte následující kroky, pokud chcete nakonfigurovat jednotné přihlašování v **IdP iniciované** režimu</span><span class="sxs-lookup"><span data-stu-id="03895-160">On Azure Portal, under **LinkedIn Elevate Domain and URLs**, perform the following steps if you want to configure SSO in **IdP Initiated** mode</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_01.png)

    <span data-ttu-id="03895-162">a.</span><span class="sxs-lookup"><span data-stu-id="03895-162">a.</span></span> <span data-ttu-id="03895-163">V **identifikátor** textovému poli, zadejte **Entity ID** zkopírovat z portálu LinkedIn</span><span class="sxs-lookup"><span data-stu-id="03895-163">In the **Identifier** textbox, enter the **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="03895-164">b.</span><span class="sxs-lookup"><span data-stu-id="03895-164">b.</span></span> <span data-ttu-id="03895-165">V **adresa URL odpovědi** textovému poli, zadejte **adresu Url pro přístup k příjemce Assertion (ACS)** zkopírovat z portálu LinkedIn</span><span class="sxs-lookup"><span data-stu-id="03895-165">In the **Reply URL** textbox, enter the **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="03895-166">Pokud chcete nakonfigurovat jednotné přihlašování v **iniciované SP**, klikněte na možnost zobrazit Advanced adresy URL v části Konfigurace a konfigurovat přihlášení na adresu URL s vzoru následující:</span><span class="sxs-lookup"><span data-stu-id="03895-166">If you want to configure SSO in **SP Initiated**, then click Show Advanced URL setting option in the configuration section and configure the sign on URL with the following pattern:</span></span>

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>` 
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_02.png) 
    
8. <span data-ttu-id="03895-168">Zvýšení oprávnění LinkedIn aplikace očekává SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, můžete přidat mapování vlastních atributů do vaší konfigurace atributy tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="03895-168">Your LinkedIn Elevate application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="03895-169">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="03895-169">The following screenshot shows an example for this.</span></span> <span data-ttu-id="03895-170">Výchozí hodnota **uživatelský identifikátor** je **user.userprincipalname** ale zvýšení oprávnění LinkedIn očekává to nejde mapovat pomocí e-mailovou adresu uživatele.</span><span class="sxs-lookup"><span data-stu-id="03895-170">The default value of **User Identifier** is **user.userprincipalname** but LinkedIn Elevate expects this to be mapped with the user's email address.</span></span> <span data-ttu-id="03895-171">K tomu můžete použít **user.mail** atribut ze seznamu, nebo použijte hodnotu odpovídajícího atributu na základě konfigurace vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="03895-171">For that you can use **user.mail** attribute from the list or use the appropriate attribute value based on your organization configuration.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/updateusermail.png)

9. <span data-ttu-id="03895-173">V **uživatelské atributy** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** a nastavit atributy.</span><span class="sxs-lookup"><span data-stu-id="03895-173">In **User Attributes** section, click **View and edit all other user attributes** and set the attributes.</span></span> <span data-ttu-id="03895-174">Budete muset přidat další deklarace identity s názvem **oddělení** a hodnota musí být namapovaný na **user.department**.</span><span class="sxs-lookup"><span data-stu-id="03895-174">You need to add another claim named **department** and the value needs to be mapped to **user.department**.</span></span>

    | <span data-ttu-id="03895-175">Název atributu</span><span class="sxs-lookup"><span data-stu-id="03895-175">Attribute Name</span></span> | <span data-ttu-id="03895-176">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="03895-176">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="03895-177">Oddělení</span><span class="sxs-lookup"><span data-stu-id="03895-177">department</span></span>| <span data-ttu-id="03895-178">User.Department</span><span class="sxs-lookup"><span data-stu-id="03895-178">user.department</span></span> |

      ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/userattribute.png)

      <span data-ttu-id="03895-180">a.</span><span class="sxs-lookup"><span data-stu-id="03895-180">a.</span></span> <span data-ttu-id="03895-181">Kliknutím na Přidat atribut chcete otevřít stránku Podrobnosti o atribut přidat atribut oddělení, jak je znázorněno níže-</span><span class="sxs-lookup"><span data-stu-id="03895-181">Click on Add attribute to open the attribute details page add the department attribute as shown below-</span></span>

      ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/adduserattribute.png)

      <span data-ttu-id="03895-183">b.</span><span class="sxs-lookup"><span data-stu-id="03895-183">b.</span></span> <span data-ttu-id="03895-184">Klikněte na **Ok** do atribut uložit.</span><span class="sxs-lookup"><span data-stu-id="03895-184">Click on **Ok** to save the attribute.</span></span>

      <span data-ttu-id="03895-185">c.</span><span class="sxs-lookup"><span data-stu-id="03895-185">c.</span></span> <span data-ttu-id="03895-186">Změňte název atributu **emailaddress** k **e-mailu**.</span><span class="sxs-lookup"><span data-stu-id="03895-186">Change the name of the attribute **emailaddress** to **email**.</span></span>


10. <span data-ttu-id="03895-187">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="03895-187">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_certificate.png) 

11. <span data-ttu-id="03895-189">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="03895-189">Click **Save**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_400.png)

12. <span data-ttu-id="03895-191">Přejděte na **nastavení správce LinkedIn** části.</span><span class="sxs-lookup"><span data-stu-id="03895-191">Go to **LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="03895-192">Nahrajte soubor XML, který jste právě stáhli z portálu Azure klikněte na možnost soubor nahrát XML.</span><span class="sxs-lookup"><span data-stu-id="03895-192">Upload the XML file you just downloaded from the Azure portal by clicking on the Upload XML file option.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_metadata_03.png)

13. <span data-ttu-id="03895-194">Klikněte na tlačítko **na** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="03895-194">Click **On** to enable SSO.</span></span> <span data-ttu-id="03895-195">Jednotné přihlašování stav se změní z **Nepřipojeno** k **připojeno**</span><span class="sxs-lookup"><span data-stu-id="03895-195">SSO status will change from **Not Connected** to **Connected**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="03895-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="03895-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="03895-198">Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="03895-198">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="03895-200">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="03895-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="03895-201">V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="03895-201">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="03895-203">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="03895-203">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="03895-205">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="03895-205">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="03895-207">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="03895-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="03895-209">a.</span><span class="sxs-lookup"><span data-stu-id="03895-209">a.</span></span> <span data-ttu-id="03895-210">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="03895-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="03895-211">b.</span><span class="sxs-lookup"><span data-stu-id="03895-211">b.</span></span> <span data-ttu-id="03895-212">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="03895-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="03895-213">c.</span><span class="sxs-lookup"><span data-stu-id="03895-213">c.</span></span> <span data-ttu-id="03895-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="03895-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="03895-215">d.</span><span class="sxs-lookup"><span data-stu-id="03895-215">d.</span></span> <span data-ttu-id="03895-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="03895-216">Click **Create**.</span></span> 

### <a name="creating-a-linkedin-elevate-test-user"></a><span data-ttu-id="03895-217">Vytvoření zkušebního uživatele LinkedIn zvýšení oprávnění</span><span class="sxs-lookup"><span data-stu-id="03895-217">Creating a LinkedIn Elevate test user</span></span>

<span data-ttu-id="03895-218">Propojené zvýšení oprávnění aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatele automaticky se vytvoří v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="03895-218">Linked Elevate Application supports Just in time user provisioning and after authentication users will be created in the application automatically.</span></span> <span data-ttu-id="03895-219">Nastavení na správce stránky na zvýšení oprávnění LinkedIn portálu překlopit přepínač **automaticky přiřadit licence** jako aktivní, chcete-li povolit pouze v čase zřizování a to bude také licenci uživateli přiřadit.</span><span class="sxs-lookup"><span data-stu-id="03895-219">On the admin settings page on the LinkedIn Elevate portal flip the switch **Automatically Assign licenses** to active to enable Just in time provisioning and this will also assign a license to the user.</span></span>

   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="03895-221">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="03895-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="03895-222">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup ke zvýšení oprávnění LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="03895-222">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to LinkedIn Elevate.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="03895-224">**Přiřadit Britta Simon LinkedIn zvýšení oprávnění, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="03895-224">**To assign Britta Simon to LinkedIn Elevate, perform the following steps:**</span></span>

1. <span data-ttu-id="03895-225">V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="03895-225">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="03895-227">V seznamu aplikací vyberte **zvýšení oprávnění LinkedIn**.</span><span class="sxs-lookup"><span data-stu-id="03895-227">In the applications list, select **LinkedIn Elevate**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_0001.png) 

3. <span data-ttu-id="03895-229">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="03895-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="03895-231">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="03895-231">Click **Add** button.</span></span> <span data-ttu-id="03895-232">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="03895-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="03895-234">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="03895-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="03895-235">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="03895-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="03895-236">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="03895-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="03895-237">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="03895-237">Testing single sign-on</span></span>

<span data-ttu-id="03895-238">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="03895-238">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="03895-239">Když kliknete na dlaždici LinkedIn zvýšení oprávnění na přístupovém panelu, měli byste obdržet stránku Azure přihlášení a na po úspěšném přihlášení, měli byste obdržet do své aplikace LinkedIn zvýšení oprávnění.</span><span class="sxs-lookup"><span data-stu-id="03895-239">When you click the LinkedIn Elevate tile in the Access Panel, you should get the Azure Sign-on page and on after successful sign-on, you should get into your LinkedIn Elevate application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="03895-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="03895-240">Additional resources</span></span>

* [<span data-ttu-id="03895-241">Kurz: Konfigurace LinkedIn zvýšení oprávnění pro automatické zřizování s Azure Active Directory uživatelů</span><span class="sxs-lookup"><span data-stu-id="03895-241">Tutorial: Configuring LinkedIn Elevate for automatic user provisioning with Azure Active Directory</span></span>](active-directory-saas-linkedinelevate-provisioning-tutorial.md)
* [<span data-ttu-id="03895-242">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="03895-242">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="03895-243">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="03895-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_203.png
