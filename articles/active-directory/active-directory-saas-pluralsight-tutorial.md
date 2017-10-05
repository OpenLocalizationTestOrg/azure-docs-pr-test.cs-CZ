---
title: 'Kurz: Azure Active Directory integrace s Pluralsight | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Pluralsight."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4c3f07d2-4e1f-4ea3-9025-c663f1f2b7b4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 62429643a108665544e42001d264046b5db1ec97
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pluralsight"></a><span data-ttu-id="8cec6-103">Kurz: Azure Active Directory integrace s Pluralsight</span><span class="sxs-lookup"><span data-stu-id="8cec6-103">Tutorial: Azure Active Directory integration with Pluralsight</span></span>

<span data-ttu-id="8cec6-104">V tomto kurzu zjistěte, jak integrovat Pluralsight s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8cec6-104">In this tutorial, you learn how to integrate Pluralsight with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8cec6-105">Integrace Pluralsight s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8cec6-105">Integrating Pluralsight with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8cec6-106">Můžete řídit ve službě Azure AD, který má přístup k Pluralsight</span><span class="sxs-lookup"><span data-stu-id="8cec6-106">You can control in Azure AD who has access to Pluralsight</span></span>
- <span data-ttu-id="8cec6-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Pluralsight (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="8cec6-107">You can enable your users to automatically get signed-on to Pluralsight (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8cec6-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8cec6-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8cec6-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8cec6-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8cec6-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8cec6-110">Prerequisites</span></span>

<span data-ttu-id="8cec6-111">Konfigurace integrace Azure AD s Pluralsight, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="8cec6-111">To configure Azure AD integration with Pluralsight, you need the following items:</span></span>

- <span data-ttu-id="8cec6-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8cec6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8cec6-113">Pluralsight jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="8cec6-113">A Pluralsight single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8cec6-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8cec6-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8cec6-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="8cec6-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8cec6-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="8cec6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8cec6-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8cec6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8cec6-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="8cec6-118">Scenario description</span></span>
<span data-ttu-id="8cec6-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="8cec6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8cec6-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="8cec6-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8cec6-121">Přidání Pluralsight z Galerie</span><span class="sxs-lookup"><span data-stu-id="8cec6-121">Adding Pluralsight from the gallery</span></span>
2. <span data-ttu-id="8cec6-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8cec6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pluralsight-from-the-gallery"></a><span data-ttu-id="8cec6-123">Přidání Pluralsight z Galerie</span><span class="sxs-lookup"><span data-stu-id="8cec6-123">Adding Pluralsight from the gallery</span></span>
<span data-ttu-id="8cec6-124">Při konfiguraci integrace Pluralsight do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Pluralsight z galerie.</span><span class="sxs-lookup"><span data-stu-id="8cec6-124">To configure the integration of Pluralsight into Azure AD, you need to add Pluralsight from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8cec6-125">**Pokud chcete přidat Pluralsight z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8cec6-125">**To add Pluralsight from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8cec6-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8cec6-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8cec6-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="8cec6-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8cec6-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8cec6-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="8cec6-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8cec6-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="8cec6-133">Do vyhledávacího pole zadejte **Pluralsight**.</span><span class="sxs-lookup"><span data-stu-id="8cec6-133">In the search box, type **Pluralsight**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_search.png)

5. <span data-ttu-id="8cec6-135">Na panelu výsledků vyberte **Pluralsight**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8cec6-135">In the results panel, select **Pluralsight**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8cec6-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8cec6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8cec6-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Pluralsight podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="8cec6-138">In this section, you configure and test Azure AD single sign-on with Pluralsight based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8cec6-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Pluralsight je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8cec6-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Pluralsight is to a user in Azure AD.</span></span> <span data-ttu-id="8cec6-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Pluralsight musí navázat.</span><span class="sxs-lookup"><span data-stu-id="8cec6-140">In other words, a link relationship between an Azure AD user and the related user in Pluralsight needs to be established.</span></span>

<span data-ttu-id="8cec6-141">V Pluralsight, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="8cec6-141">In Pluralsight, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8cec6-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Pluralsight, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="8cec6-142">To configure and test Azure AD single sign-on with Pluralsight, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8cec6-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="8cec6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8cec6-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8cec6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8cec6-145">**[Vytvoření zkušebního uživatele Pluralsight](#creating-a-pluralsight-test-user)**  – Pokud chcete mít protějšek Britta Simon v Pluralsight propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="8cec6-145">**[Creating a Pluralsight test user](#creating-a-pluralsight-test-user)** - to have a counterpart of Britta Simon in Pluralsight that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8cec6-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8cec6-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8cec6-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8cec6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8cec6-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8cec6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8cec6-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Pluralsight.</span><span class="sxs-lookup"><span data-stu-id="8cec6-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Pluralsight application.</span></span>

<span data-ttu-id="8cec6-150">**Ke konfiguraci Azure AD jednotné přihlašování s Pluralsight, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8cec6-150">**To configure Azure AD single sign-on with Pluralsight, perform the following steps:**</span></span>

1. <span data-ttu-id="8cec6-151">Na portálu Azure na **Pluralsight** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8cec6-151">In the Azure portal, on the **Pluralsight** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="8cec6-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8cec6-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_samlbase.png)

3. <span data-ttu-id="8cec6-155">Na **Pluralsight domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8cec6-155">On the **Pluralsight Domain and URLs** section, perform the following:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_url.png)

    <span data-ttu-id="8cec6-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<instance name>.pluralsight.com/sso/<company name>`</span><span class="sxs-lookup"><span data-stu-id="8cec6-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<instance name>.pluralsight.com/sso/<company name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8cec6-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="8cec6-158">This value is not real.</span></span> <span data-ttu-id="8cec6-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8cec6-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="8cec6-160">Obraťte se na [tým podpory Pluralsight klienta](mailto:support@pluralsight.com) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8cec6-160">Contact [Pluralsight Client support team](mailto:support@pluralsight.com) to get this value.</span></span> 
 


4. <span data-ttu-id="8cec6-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="8cec6-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_certificate.png) 

5. <span data-ttu-id="8cec6-163">Cílem této části je chcete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Pluralsight.</span><span class="sxs-lookup"><span data-stu-id="8cec6-163">The objective of this section is to enable Azure AD single sign-on in the Azure portal and to configure SSO in the Pluralsight application.</span></span>

    <span data-ttu-id="8cec6-164">Aplikace Pluralsight očekává SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, můžete přidat mapování vlastních atributů do vaší konfigurace atributy tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="8cec6-164">The Pluralsight application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="8cec6-165">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="8cec6-165">The following screenshot shows an example for this.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_attribute.png)

    >[!NOTE]
    ><span data-ttu-id="8cec6-167">Můžete také přidat **"Jedinečné ID"** atribut s příslušnou hodnotu jako EmployeeID nebo něco jiného, která vyhovuje pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="8cec6-167">You can also add the **"Unique ID"** attribute with the appropriate value like EmployeeID or something else which suits for your organization.</span></span> <span data-ttu-id="8cec6-168">Všimněte si, že to není povinný atribut; ale můžete přidat ji k identifikaci uživatele jedinečný.</span><span class="sxs-lookup"><span data-stu-id="8cec6-168">Also note that this is not the required attribute; however, you can add it to  identify the unique user.</span></span> 

6. <span data-ttu-id="8cec6-169">Chcete-li přidat požadované **atributy tokenu SAML**, pro každý řádek v tabulce níže, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8cec6-169">To add the required **SAML token attributes**, for each row shown in the table below, perform the following steps:</span></span>
   
   | <span data-ttu-id="8cec6-170">Název atributu</span><span class="sxs-lookup"><span data-stu-id="8cec6-170">Attribute Name</span></span> | <span data-ttu-id="8cec6-171">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="8cec6-171">Attribute Value</span></span> |
   | ---| --- |
   | <span data-ttu-id="8cec6-172">Jméno</span><span class="sxs-lookup"><span data-stu-id="8cec6-172">First Name</span></span> |<span data-ttu-id="8cec6-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="8cec6-173">user.givenname</span></span> |
   | <span data-ttu-id="8cec6-174">Příjmení</span><span class="sxs-lookup"><span data-stu-id="8cec6-174">Last Name</span></span> |<span data-ttu-id="8cec6-175">User.Surname</span><span class="sxs-lookup"><span data-stu-id="8cec6-175">user.surname</span></span> |
   | <span data-ttu-id="8cec6-176">E-mail</span><span class="sxs-lookup"><span data-stu-id="8cec6-176">Email</span></span> |<span data-ttu-id="8cec6-177">User.Mail</span><span class="sxs-lookup"><span data-stu-id="8cec6-177">user.mail</span></span> |
   
   <span data-ttu-id="8cec6-178">a.</span><span class="sxs-lookup"><span data-stu-id="8cec6-178">a.</span></span> <span data-ttu-id="8cec6-179">Klikněte na tlačítko **přidat atribut uživatele** otevřete **přidat uživatele Attribure** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8cec6-179">Click **add user attribute** to open the **Add User Attribure** dialog.</span></span>
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_addattribute.png)
  
   <span data-ttu-id="8cec6-181">b.</span><span class="sxs-lookup"><span data-stu-id="8cec6-181">b.</span></span> <span data-ttu-id="8cec6-182">V **název atributu** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="8cec6-182">In the **Attribute Name** textbox, type the attribute name shown for that row.</span></span>
  
   <span data-ttu-id="8cec6-183">c.</span><span class="sxs-lookup"><span data-stu-id="8cec6-183">c.</span></span> <span data-ttu-id="8cec6-184">Z **hodnota atributu** vyberte hodnotu atributu zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="8cec6-184">From the **Attribute Value** list, select the attribute value shown for that row.</span></span>
  
   <span data-ttu-id="8cec6-185">d.</span><span class="sxs-lookup"><span data-stu-id="8cec6-185">d.</span></span> <span data-ttu-id="8cec6-186">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="8cec6-186">Click **Ok**.</span></span>    

7. <span data-ttu-id="8cec6-187">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8cec6-187">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="8cec6-189">Chcete-li získat jednotné přihlašování, které jsou nakonfigurované pro vaše aplikace, obraťte se na [Pluralsight Professional služby](mailTo:professionalservices@pluralsight.com) týmu a zadejte soubor stažený metadat.</span><span class="sxs-lookup"><span data-stu-id="8cec6-189">To get SSO configured for your application, contact [Pluralsight Professional Services](mailTo:professionalservices@pluralsight.com) team and provide the downloaded metadata file.</span></span>

> [!TIP]
> <span data-ttu-id="8cec6-190">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="8cec6-190">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8cec6-191">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="8cec6-191">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8cec6-192">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8cec6-192">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8cec6-193">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8cec6-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="8cec6-194">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8cec6-194">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="8cec6-196">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8cec6-196">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8cec6-197">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8cec6-197">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8cec6-199">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="8cec6-199">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8cec6-201">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8cec6-201">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8cec6-203">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8cec6-203">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8cec6-205">a.</span><span class="sxs-lookup"><span data-stu-id="8cec6-205">a.</span></span> <span data-ttu-id="8cec6-206">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8cec6-206">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8cec6-207">b.</span><span class="sxs-lookup"><span data-stu-id="8cec6-207">b.</span></span> <span data-ttu-id="8cec6-208">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8cec6-208">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8cec6-209">c.</span><span class="sxs-lookup"><span data-stu-id="8cec6-209">c.</span></span> <span data-ttu-id="8cec6-210">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="8cec6-210">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8cec6-211">d.</span><span class="sxs-lookup"><span data-stu-id="8cec6-211">d.</span></span> <span data-ttu-id="8cec6-212">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8cec6-212">Click **Create**.</span></span>
 
### <a name="creating-a-pluralsight-test-user"></a><span data-ttu-id="8cec6-213">Vytvoření zkušebního uživatele Pluralsight</span><span class="sxs-lookup"><span data-stu-id="8cec6-213">Creating a Pluralsight test user</span></span>

<span data-ttu-id="8cec6-214">Cílem této části je vytvoření uživatele v Pluralsight nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8cec6-214">The objective of this section is to create a user called Britta Simon in Pluralsight.</span></span> <span data-ttu-id="8cec6-215">Spojte se s [tým podpory Pluralsight klienta](mailto:support@pluralsight.com) přidat uživatele do Pluralsight účtu.</span><span class="sxs-lookup"><span data-stu-id="8cec6-215">Please work with [Pluralsight Client support team](mailto:support@pluralsight.com) to add the users in the Pluralsight account.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8cec6-216">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8cec6-216">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8cec6-217">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Pluralsight.</span><span class="sxs-lookup"><span data-stu-id="8cec6-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Pluralsight.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="8cec6-219">**Pokud chcete přiřadit Britta Simon Pluralsight, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8cec6-219">**To assign Britta Simon to Pluralsight, perform the following steps:**</span></span>

1. <span data-ttu-id="8cec6-220">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8cec6-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="8cec6-222">V seznamu aplikací vyberte **Pluralsight**.</span><span class="sxs-lookup"><span data-stu-id="8cec6-222">In the applications list, select **Pluralsight**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_app.png) 

3. <span data-ttu-id="8cec6-224">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="8cec6-224">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="8cec6-226">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8cec6-226">Click **Add** button.</span></span> <span data-ttu-id="8cec6-227">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8cec6-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="8cec6-229">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="8cec6-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8cec6-230">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8cec6-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8cec6-231">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8cec6-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8cec6-232">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8cec6-232">Testing single sign-on</span></span>

<span data-ttu-id="8cec6-233">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="8cec6-233">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="8cec6-234">Když kliknete na dlaždici Pluralsight na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Pluralsight.</span><span class="sxs-lookup"><span data-stu-id="8cec6-234">When you click the Pluralsight tile in the Access Panel, you should get automatically signed-on to your Pluralsight application.</span></span> <span data-ttu-id="8cec6-235">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8cec6-235">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8cec6-236">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8cec6-236">Additional resources</span></span>

* [<span data-ttu-id="8cec6-237">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8cec6-237">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8cec6-238">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8cec6-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_203.png

