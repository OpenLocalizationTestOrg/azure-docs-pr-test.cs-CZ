---
title: 'Kurz: Azure Active Directory integrace s Moxtra | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Moxtra."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2aed2d4b-1dcd-4839-8fed-9419d107c61c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: db2f041a44b6771b0a4f734e58d899298ef0847b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moxtra"></a><span data-ttu-id="990c2-103">Kurz: Azure Active Directory integrace s Moxtra</span><span class="sxs-lookup"><span data-stu-id="990c2-103">Tutorial: Azure Active Directory integration with Moxtra</span></span>

<span data-ttu-id="990c2-104">V tomto kurzu zjistěte, jak integrovat Moxtra s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="990c2-104">In this tutorial, you learn how to integrate Moxtra with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="990c2-105">Integrace Moxtra s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="990c2-105">Integrating Moxtra with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="990c2-106">Můžete řídit ve službě Azure AD, který má přístup k Moxtra</span><span class="sxs-lookup"><span data-stu-id="990c2-106">You can control in Azure AD who has access to Moxtra</span></span>
- <span data-ttu-id="990c2-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Moxtra (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="990c2-107">You can enable your users to automatically get signed-on to Moxtra (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="990c2-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="990c2-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="990c2-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="990c2-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="990c2-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="990c2-110">Prerequisites</span></span>

<span data-ttu-id="990c2-111">Konfigurace integrace Azure AD s Moxtra, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="990c2-111">To configure Azure AD integration with Moxtra, you need the following items:</span></span>

- <span data-ttu-id="990c2-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="990c2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="990c2-113">Moxtra jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="990c2-113">A Moxtra single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="990c2-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="990c2-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="990c2-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="990c2-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="990c2-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="990c2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="990c2-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="990c2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="990c2-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="990c2-118">Scenario description</span></span>
<span data-ttu-id="990c2-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="990c2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="990c2-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="990c2-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="990c2-121">Přidání Moxtra z Galerie</span><span class="sxs-lookup"><span data-stu-id="990c2-121">Adding Moxtra from the gallery</span></span>
2. <span data-ttu-id="990c2-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="990c2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-moxtra-from-the-gallery"></a><span data-ttu-id="990c2-123">Přidání Moxtra z Galerie</span><span class="sxs-lookup"><span data-stu-id="990c2-123">Adding Moxtra from the gallery</span></span>
<span data-ttu-id="990c2-124">Při konfiguraci integrace Moxtra do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Moxtra z galerie.</span><span class="sxs-lookup"><span data-stu-id="990c2-124">To configure the integration of Moxtra into Azure AD, you need to add Moxtra from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="990c2-125">**Pokud chcete přidat Moxtra z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="990c2-125">**To add Moxtra from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="990c2-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="990c2-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="990c2-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="990c2-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="990c2-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="990c2-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="990c2-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="990c2-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="990c2-133">Do vyhledávacího pole zadejte **Moxtra**.</span><span class="sxs-lookup"><span data-stu-id="990c2-133">In the search box, type **Moxtra**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_search.png)

5. <span data-ttu-id="990c2-135">Na panelu výsledků vyberte **Moxtra**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="990c2-135">In the results panel, select **Moxtra**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="990c2-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="990c2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="990c2-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Moxtra podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="990c2-138">In this section, you configure and test Azure AD single sign-on with Moxtra based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="990c2-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Moxtra je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="990c2-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Moxtra is to a user in Azure AD.</span></span> <span data-ttu-id="990c2-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Moxtra musí navázat.</span><span class="sxs-lookup"><span data-stu-id="990c2-140">In other words, a link relationship between an Azure AD user and the related user in Moxtra needs to be established.</span></span>

<span data-ttu-id="990c2-141">V Moxtra, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="990c2-141">In Moxtra, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="990c2-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Moxtra, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="990c2-142">To configure and test Azure AD single sign-on with Moxtra, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="990c2-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="990c2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="990c2-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="990c2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="990c2-145">**[Vytvoření zkušebního uživatele Moxtra](#creating-a-moxtra-test-user)**  – Pokud chcete mít protějšek Britta Simon v Moxtra propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="990c2-145">**[Creating a Moxtra test user](#creating-a-moxtra-test-user)** - to have a counterpart of Britta Simon in Moxtra that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="990c2-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="990c2-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="990c2-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="990c2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="990c2-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="990c2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="990c2-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Moxtra.</span><span class="sxs-lookup"><span data-stu-id="990c2-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Moxtra application.</span></span>

<span data-ttu-id="990c2-150">**Ke konfiguraci Azure AD jednotné přihlašování s Moxtra, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="990c2-150">**To configure Azure AD single sign-on with Moxtra, perform the following steps:**</span></span>

1. <span data-ttu-id="990c2-151">Na portálu Azure na **Moxtra** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="990c2-151">In the Azure portal, on the **Moxtra** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="990c2-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="990c2-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_samlbase.png)

3. <span data-ttu-id="990c2-155">Na **Moxtra domény a adresy URL** část, proveďte následující krok:</span><span class="sxs-lookup"><span data-stu-id="990c2-155">On the **Moxtra Domain and URLs** section, perform the following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_url.png)

    <span data-ttu-id="990c2-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://www.moxtra.com/service/#login`</span><span class="sxs-lookup"><span data-stu-id="990c2-157">In the **Sign-on URL** textbox, type a URL as: `https://www.moxtra.com/service/#login`</span></span>

4. <span data-ttu-id="990c2-158">Aplikace Moxtra očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="990c2-158">Moxtra application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="990c2-159">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="990c2-159">Configure the following claims for this application.</span></span> <span data-ttu-id="990c2-160">Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="990c2-160">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="990c2-161">Následující snímek obrazovky ukazuje příklad pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="990c2-161">The following screenshot shows an example for this configuration.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_attributes.png)
    
5. <span data-ttu-id="990c2-163">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="990c2-163">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="990c2-164">Název atributu</span><span class="sxs-lookup"><span data-stu-id="990c2-164">Attribute Name</span></span> | <span data-ttu-id="990c2-165">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="990c2-165">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="990c2-166">FirstName</span><span class="sxs-lookup"><span data-stu-id="990c2-166">firstname</span></span> | <span data-ttu-id="990c2-167">User.givenName</span><span class="sxs-lookup"><span data-stu-id="990c2-167">user.givenname</span></span> |
    | <span data-ttu-id="990c2-168">Příjmení</span><span class="sxs-lookup"><span data-stu-id="990c2-168">lastname</span></span> | <span data-ttu-id="990c2-169">User.Surname</span><span class="sxs-lookup"><span data-stu-id="990c2-169">user.surname</span></span> |
    | <span data-ttu-id="990c2-170">idpid</span><span class="sxs-lookup"><span data-stu-id="990c2-170">idpid</span></span>    | <span data-ttu-id="990c2-171">< SAML Entity ID ></span><span class="sxs-lookup"><span data-stu-id="990c2-171">< SAML Entity ID ></span></span> 

    > [!Note]
    > <span data-ttu-id="990c2-172">Hodnota **idpid** atribut není skutečné.</span><span class="sxs-lookup"><span data-stu-id="990c2-172">The value of **idpid** attribute is not real.</span></span> <span data-ttu-id="990c2-173">Můžete získat skutečná hodnota z **Stručná referenční příručka** oddílu pod **Moxtra konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="990c2-173">You can get the actual value from **Quick reference** section under **Moxtra Configuration**.</span></span>
    
    <span data-ttu-id="990c2-174">a.</span><span class="sxs-lookup"><span data-stu-id="990c2-174">a.</span></span> <span data-ttu-id="990c2-175">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="990c2-175">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="990c2-177">b.</span><span class="sxs-lookup"><span data-stu-id="990c2-177">b.</span></span> <span data-ttu-id="990c2-178">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="990c2-178">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="990c2-180">c.</span><span class="sxs-lookup"><span data-stu-id="990c2-180">c.</span></span> <span data-ttu-id="990c2-181">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="990c2-181">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="990c2-182">d.</span><span class="sxs-lookup"><span data-stu-id="990c2-182">d.</span></span> <span data-ttu-id="990c2-183">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="990c2-183">Click **Ok**.</span></span>
    
5. <span data-ttu-id="990c2-184">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="990c2-184">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_certificate.png) 

6. <span data-ttu-id="990c2-186">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="990c2-186">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="990c2-188">Na **Moxtra konfigurace** klikněte na tlačítko **konfigurace Moxtra** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="990c2-188">On the **Moxtra Configuration** section, click **Configure Moxtra** to open **Configure sign-on** window.</span></span> <span data-ttu-id="990c2-189">Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="990c2-189">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_configure.png) 

8. <span data-ttu-id="990c2-191">V jiném okně prohlížeče Přihlaste se k serveru vaší společnosti Moxtra jako správce.</span><span class="sxs-lookup"><span data-stu-id="990c2-191">In another browser window, sign on to your Moxtra company site as an administrator.</span></span>

9. <span data-ttu-id="990c2-192">Na panelu nástrojů na levé straně klikněte na tlačítko **konzoly pro správu > SAML jednotné přihlašování**a potom klikněte na **nový**.</span><span class="sxs-lookup"><span data-stu-id="990c2-192">In the toolbar on the left, click **Admin Console > SAML Single Sign-on**, and then click **New**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_06.png) 

10. <span data-ttu-id="990c2-194">Na **SAML** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="990c2-194">On the **SAML** page, perform the following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_08.png)   
 
    <span data-ttu-id="990c2-196">a.</span><span class="sxs-lookup"><span data-stu-id="990c2-196">a.</span></span> <span data-ttu-id="990c2-197">V **název** textovému poli, zadejte název pro svou konfiguraci (například: *SAML*).</span><span class="sxs-lookup"><span data-stu-id="990c2-197">In the **Name** textbox, type a name for your configuration (e.g.: *SAML*).</span></span> 
  
    <span data-ttu-id="990c2-198">b.</span><span class="sxs-lookup"><span data-stu-id="990c2-198">b.</span></span> <span data-ttu-id="990c2-199">V **IdP Entity ID** textovému poli, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="990c2-199">In the **IdP Entity ID** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span> 
 
    <span data-ttu-id="990c2-200">c.</span><span class="sxs-lookup"><span data-stu-id="990c2-200">c.</span></span> <span data-ttu-id="990c2-201">V **přihlašovací adresa URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="990c2-201">In **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 
 
    <span data-ttu-id="990c2-202">d.</span><span class="sxs-lookup"><span data-stu-id="990c2-202">d.</span></span> <span data-ttu-id="990c2-203">V **AuthnContextClassRef** textovému poli, typ **urn: oasis: názvy: tc: SAML:2.0:ac:classes:Password**.</span><span class="sxs-lookup"><span data-stu-id="990c2-203">In the **AuthnContextClassRef** textbox, type **urn:oasis:names:tc:SAML:2.0:ac:classes:Password**.</span></span> 
 
    <span data-ttu-id="990c2-204">e.</span><span class="sxs-lookup"><span data-stu-id="990c2-204">e.</span></span> <span data-ttu-id="990c2-205">V **NameID formátu** textovému poli, typ **urn: oasis: názvy: tc: SAML:1.1:nameid-formátu: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="990c2-205">In the **NameID Format** textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span> 
 
    <span data-ttu-id="990c2-206">f.</span><span class="sxs-lookup"><span data-stu-id="990c2-206">f.</span></span> <span data-ttu-id="990c2-207">Otevřete certifikát, který jste si stáhli z portálu Azure v poznámkovém bloku kopírovat obsah a vložte ji do **certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="990c2-207">Open certificate which you have downloaded from Azure portal in notepad, copy the content, and then paste it into the **Certificate** textbox.</span></span>    
 
    <span data-ttu-id="990c2-208">g.</span><span class="sxs-lookup"><span data-stu-id="990c2-208">g.</span></span> <span data-ttu-id="990c2-209">Do textového pole SAML e-mailové domény zadejte e-mailovou doménu SAML.</span><span class="sxs-lookup"><span data-stu-id="990c2-209">In the SAML email domain textbox, type your SAML email domain.</span></span>    
  
    >[!NOTE]
    ><span data-ttu-id="990c2-210">Postup ověření domény zobrazíte kliknutím "**i**" níže.</span><span class="sxs-lookup"><span data-stu-id="990c2-210">To see the steps to verify the domain, click the "**i**" below.</span></span>

    <span data-ttu-id="990c2-211">h.</span><span class="sxs-lookup"><span data-stu-id="990c2-211">h.</span></span> <span data-ttu-id="990c2-212">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="990c2-212">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="990c2-213">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="990c2-213">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="990c2-214">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="990c2-214">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="990c2-215">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="990c2-215">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="990c2-216">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="990c2-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="990c2-217">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="990c2-217">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="990c2-219">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="990c2-219">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="990c2-220">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="990c2-220">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="990c2-222">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="990c2-222">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="990c2-224">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="990c2-224">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="990c2-226">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="990c2-226">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="990c2-228">a.</span><span class="sxs-lookup"><span data-stu-id="990c2-228">a.</span></span> <span data-ttu-id="990c2-229">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="990c2-229">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="990c2-230">b.</span><span class="sxs-lookup"><span data-stu-id="990c2-230">b.</span></span> <span data-ttu-id="990c2-231">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="990c2-231">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="990c2-232">c.</span><span class="sxs-lookup"><span data-stu-id="990c2-232">c.</span></span> <span data-ttu-id="990c2-233">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="990c2-233">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="990c2-234">d.</span><span class="sxs-lookup"><span data-stu-id="990c2-234">d.</span></span> <span data-ttu-id="990c2-235">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="990c2-235">Click **Create**.</span></span>
 
### <a name="creating-a-moxtra-test-user"></a><span data-ttu-id="990c2-236">Vytvoření zkušebního uživatele Moxtra</span><span class="sxs-lookup"><span data-stu-id="990c2-236">Creating a Moxtra test user</span></span>

<span data-ttu-id="990c2-237">Cílem této části je vytvoření uživatele v Moxtra nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="990c2-237">The objective of this section is to create a user called Britta Simon in Moxtra.</span></span>

<span data-ttu-id="990c2-238">**Vytvoření uživatele v Moxtra nazývá Britta Simon, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="990c2-238">**To create a user called Britta Simon in Moxtra, perform the following steps:**</span></span>

1. <span data-ttu-id="990c2-239">Přihlásit se k serveru vaší společnosti Moxtra jako správce.</span><span class="sxs-lookup"><span data-stu-id="990c2-239">Sign on to your Moxtra company site as an administrator.</span></span>

2. <span data-ttu-id="990c2-240">Na panelu nástrojů na levé straně klikněte na tlačítko **konzoly pro správu > Správa uživatelů**a potom **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="990c2-240">In the toolbar on the left, click **Admin Console > User Management**, and then **Add User**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_10.png) 

3. <span data-ttu-id="990c2-242">Na **přidat uživatele** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="990c2-242">On the **Add User** dialog, perform the following steps:</span></span>
  
    <span data-ttu-id="990c2-243">a.</span><span class="sxs-lookup"><span data-stu-id="990c2-243">a.</span></span> <span data-ttu-id="990c2-244">V **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="990c2-244">In the **First Name** textbox, type **Britta**.</span></span>
  
    <span data-ttu-id="990c2-245">b.</span><span class="sxs-lookup"><span data-stu-id="990c2-245">b.</span></span> <span data-ttu-id="990c2-246">V **příjmení** textovému poli, typ **Simon**.</span><span class="sxs-lookup"><span data-stu-id="990c2-246">In the **Last Name** textbox, type **Simon**.</span></span>
  
    <span data-ttu-id="990c2-247">c.</span><span class="sxs-lookup"><span data-stu-id="990c2-247">c.</span></span> <span data-ttu-id="990c2-248">V **e-mailu** textovému poli, typ Britta na e-mailová adresa, aby se pro stejné jako na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="990c2-248">In the **Email** textbox, type Britta's email address same as on Azure portal.</span></span>
  
    <span data-ttu-id="990c2-249">d.</span><span class="sxs-lookup"><span data-stu-id="990c2-249">d.</span></span> <span data-ttu-id="990c2-250">V **dělení** textovému poli, typ **Dev**.</span><span class="sxs-lookup"><span data-stu-id="990c2-250">In the **Division** textbox, type **Dev**.</span></span>
  
    <span data-ttu-id="990c2-251">e.</span><span class="sxs-lookup"><span data-stu-id="990c2-251">e.</span></span> <span data-ttu-id="990c2-252">V **oddělení** textovému poli, typ **IT**.</span><span class="sxs-lookup"><span data-stu-id="990c2-252">In the **Department** textbox, type **IT**.</span></span>
  
    <span data-ttu-id="990c2-253">f.</span><span class="sxs-lookup"><span data-stu-id="990c2-253">f.</span></span> <span data-ttu-id="990c2-254">Vyberte **správce**.</span><span class="sxs-lookup"><span data-stu-id="990c2-254">Select **Administrator**.</span></span>
  
    <span data-ttu-id="990c2-255">g.</span><span class="sxs-lookup"><span data-stu-id="990c2-255">g.</span></span> <span data-ttu-id="990c2-256">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="990c2-256">Click **Add**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="990c2-257">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="990c2-257">Assigning the Azure AD test user</span></span>

<span data-ttu-id="990c2-258">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Moxtra.</span><span class="sxs-lookup"><span data-stu-id="990c2-258">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Moxtra.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="990c2-260">**Pokud chcete přiřadit Britta Simon Moxtra, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="990c2-260">**To assign Britta Simon to Moxtra, perform the following steps:**</span></span>

1. <span data-ttu-id="990c2-261">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="990c2-261">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="990c2-263">V seznamu aplikací vyberte **Moxtra**.</span><span class="sxs-lookup"><span data-stu-id="990c2-263">In the applications list, select **Moxtra**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_app.png) 

3. <span data-ttu-id="990c2-265">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="990c2-265">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="990c2-267">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="990c2-267">Click **Add** button.</span></span> <span data-ttu-id="990c2-268">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="990c2-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="990c2-270">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="990c2-270">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="990c2-271">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="990c2-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="990c2-272">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="990c2-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="990c2-273">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="990c2-273">Testing single sign-on</span></span>

<span data-ttu-id="990c2-274">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="990c2-274">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="990c2-275">Když kliknete na dlaždici Moxtra na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Moxtra.</span><span class="sxs-lookup"><span data-stu-id="990c2-275">When you click the Moxtra tile in the Access Panel, you should get automatically signed-on to your Moxtra application.</span></span>
<span data-ttu-id="990c2-276">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="990c2-276">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="990c2-277">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="990c2-277">Additional resources</span></span>

* [<span data-ttu-id="990c2-278">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="990c2-278">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="990c2-279">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="990c2-279">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_203.png

