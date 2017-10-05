---
title: 'Kurz: Azure Active Directory integrace s Hightail | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Hightail."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e15206ac-74b0-46e4-9329-892c7d242ec0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: ba55f9b62d274aa3eb91723c62b53f54de0891b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hightail"></a><span data-ttu-id="db9b9-103">Kurz: Azure Active Directory integrace s Hightail</span><span class="sxs-lookup"><span data-stu-id="db9b9-103">Tutorial: Azure Active Directory integration with Hightail</span></span>

<span data-ttu-id="db9b9-104">V tomto kurzu zjistěte, jak integrovat Hightail s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="db9b9-104">In this tutorial, you learn how to integrate Hightail with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="db9b9-105">Integrace Hightail s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="db9b9-105">Integrating Hightail with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="db9b9-106">Můžete řídit ve službě Azure AD, který má přístup k Hightail</span><span class="sxs-lookup"><span data-stu-id="db9b9-106">You can control in Azure AD who has access to Hightail</span></span>
- <span data-ttu-id="db9b9-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Hightail (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="db9b9-107">You can enable your users to automatically get signed-on to Hightail (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="db9b9-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="db9b9-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="db9b9-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="db9b9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db9b9-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="db9b9-110">Prerequisites</span></span>

<span data-ttu-id="db9b9-111">Konfigurace integrace Azure AD s Hightail, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="db9b9-111">To configure Azure AD integration with Hightail, you need the following items:</span></span>

- <span data-ttu-id="db9b9-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="db9b9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="db9b9-113">Hightail jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="db9b9-113">A Hightail single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="db9b9-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="db9b9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="db9b9-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="db9b9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="db9b9-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="db9b9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="db9b9-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="db9b9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="db9b9-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="db9b9-118">Scenario description</span></span>
<span data-ttu-id="db9b9-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="db9b9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="db9b9-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="db9b9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="db9b9-121">Přidání Hightail z Galerie</span><span class="sxs-lookup"><span data-stu-id="db9b9-121">Adding Hightail from the gallery</span></span>
2. <span data-ttu-id="db9b9-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="db9b9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hightail-from-the-gallery"></a><span data-ttu-id="db9b9-123">Přidání Hightail z Galerie</span><span class="sxs-lookup"><span data-stu-id="db9b9-123">Adding Hightail from the gallery</span></span>
<span data-ttu-id="db9b9-124">Při konfiguraci integrace Hightail do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Hightail z galerie.</span><span class="sxs-lookup"><span data-stu-id="db9b9-124">To configure the integration of Hightail into Azure AD, you need to add Hightail from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="db9b9-125">**Pokud chcete přidat Hightail z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="db9b9-125">**To add Hightail from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="db9b9-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="db9b9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="db9b9-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="db9b9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="db9b9-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="db9b9-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="db9b9-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="db9b9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="db9b9-133">Do vyhledávacího pole zadejte **Hightail**.</span><span class="sxs-lookup"><span data-stu-id="db9b9-133">In the search box, type **Hightail**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_search.png)

5. <span data-ttu-id="db9b9-135">Na panelu výsledků vyberte **Hightail**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="db9b9-135">In the results panel, select **Hightail**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="db9b9-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="db9b9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="db9b9-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Hightail podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="db9b9-138">In this section, you configure and test Azure AD single sign-on with Hightail based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="db9b9-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Hightail je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="db9b9-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Hightail is to a user in Azure AD.</span></span> <span data-ttu-id="db9b9-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Hightail musí navázat.</span><span class="sxs-lookup"><span data-stu-id="db9b9-140">In other words, a link relationship between an Azure AD user and the related user in Hightail needs to be established.</span></span>

<span data-ttu-id="db9b9-141">V Hightail, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="db9b9-141">In Hightail, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="db9b9-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Hightail, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="db9b9-142">To configure and test Azure AD single sign-on with Hightail, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="db9b9-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="db9b9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="db9b9-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="db9b9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="db9b9-145">**[Vytvoření zkušebního uživatele Hightail](#creating-a-hightail-test-user)**  – Pokud chcete mít protějšek Britta Simon v Hightail propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="db9b9-145">**[Creating a Hightail test user](#creating-a-hightail-test-user)** - to have a counterpart of Britta Simon in Hightail that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="db9b9-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="db9b9-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="db9b9-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="db9b9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="db9b9-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="db9b9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="db9b9-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Hightail.</span><span class="sxs-lookup"><span data-stu-id="db9b9-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Hightail application.</span></span>

<span data-ttu-id="db9b9-150">**Ke konfiguraci Azure AD jednotné přihlašování s Hightail, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="db9b9-150">**To configure Azure AD single sign-on with Hightail, perform the following steps:**</span></span>

1. <span data-ttu-id="db9b9-151">Na portálu Azure na **Hightail** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="db9b9-151">In the Azure portal, on the **Hightail** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="db9b9-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="db9b9-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_samlbase.png)

3. <span data-ttu-id="db9b9-155">Na **Hightail domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="db9b9-155">On the **Hightail Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url.png)

     <span data-ttu-id="db9b9-157">V **adresa URL odpovědi** textovému poli, zadejte adresu URL jako:`https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`</span><span class="sxs-lookup"><span data-stu-id="db9b9-157">In the **Reply URL** textbox, type the URL as: `https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="db9b9-158">Předchozí hodnota není skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="db9b9-158">The preceding value is not real value.</span></span> <span data-ttu-id="db9b9-159">Hodnota bude aktualizován skutečná adresa URL odpovědi, který je vysvětlen později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="db9b9-159">You will update the value with the actual Reply URL, which is explained later in the tutorial.</span></span>
 
4. <span data-ttu-id="db9b9-160">Na **Hightail domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **SP iniciované režimu**, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="db9b9-160">On the **Hightail Domain and URLs** section, If you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url1.png)

    <span data-ttu-id="db9b9-162">a.</span><span class="sxs-lookup"><span data-stu-id="db9b9-162">a.</span></span> <span data-ttu-id="db9b9-163">Klikněte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="db9b9-163">Click the **Show advanced URL settings**.</span></span>

    <span data-ttu-id="db9b9-164">b.</span><span class="sxs-lookup"><span data-stu-id="db9b9-164">b.</span></span> <span data-ttu-id="db9b9-165">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://www.hightail.com/loginSSO`</span><span class="sxs-lookup"><span data-stu-id="db9b9-165">In the **Sign On URL** textbox, type the URL as: `https://www.hightail.com/loginSSO`</span></span>

4. <span data-ttu-id="db9b9-166">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="db9b9-166">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_certificate.png) 

5. <span data-ttu-id="db9b9-168">Hightail aplikace očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="db9b9-168">Hightail application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="db9b9-169">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="db9b9-169">Please configure the following claims for this application.</span></span> <span data-ttu-id="db9b9-170">Můžete spravovat hodnoty těchto atributů z **"Atrribute"** aplikace.</span><span class="sxs-lookup"><span data-stu-id="db9b9-170">You can manage the values of these attributes from the **"Atrribute"** tab of the application.</span></span> <span data-ttu-id="db9b9-171">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="db9b9-171">The following screenshot shows an example for this.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_attribute.png) 

6. <span data-ttu-id="db9b9-173">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="db9b9-173">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="db9b9-174">Název atributu</span><span class="sxs-lookup"><span data-stu-id="db9b9-174">Attribute Name</span></span> | <span data-ttu-id="db9b9-175">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="db9b9-175">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="db9b9-176">FirstName</span><span class="sxs-lookup"><span data-stu-id="db9b9-176">FirstName</span></span> | <span data-ttu-id="db9b9-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="db9b9-177">user.givenname</span></span> |
    | <span data-ttu-id="db9b9-178">Příjmení</span><span class="sxs-lookup"><span data-stu-id="db9b9-178">LastName</span></span> | <span data-ttu-id="db9b9-179">User.Surname</span><span class="sxs-lookup"><span data-stu-id="db9b9-179">user.surname</span></span> |
    | <span data-ttu-id="db9b9-180">E-mail</span><span class="sxs-lookup"><span data-stu-id="db9b9-180">Email</span></span> | <span data-ttu-id="db9b9-181">User.Mail</span><span class="sxs-lookup"><span data-stu-id="db9b9-181">user.mail</span></span> |    
    | <span data-ttu-id="db9b9-182">Identity uživatele</span><span class="sxs-lookup"><span data-stu-id="db9b9-182">UserIdentity</span></span> | <span data-ttu-id="db9b9-183">User.Mail</span><span class="sxs-lookup"><span data-stu-id="db9b9-183">user.mail</span></span> |
    
    <span data-ttu-id="db9b9-184">a.</span><span class="sxs-lookup"><span data-stu-id="db9b9-184">a.</span></span> <span data-ttu-id="db9b9-185">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="db9b9-185">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_05.png)

    <span data-ttu-id="db9b9-188">b.</span><span class="sxs-lookup"><span data-stu-id="db9b9-188">b.</span></span> <span data-ttu-id="db9b9-189">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="db9b9-189">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="db9b9-190">c.</span><span class="sxs-lookup"><span data-stu-id="db9b9-190">c.</span></span> <span data-ttu-id="db9b9-191">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="db9b9-191">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="db9b9-192">d.</span><span class="sxs-lookup"><span data-stu-id="db9b9-192">d.</span></span> <span data-ttu-id="db9b9-193">Ponechte **Namespace** prázdné.</span><span class="sxs-lookup"><span data-stu-id="db9b9-193">Leave the **Namespace** blank.</span></span>
    
    <span data-ttu-id="db9b9-194">e.</span><span class="sxs-lookup"><span data-stu-id="db9b9-194">e.</span></span> <span data-ttu-id="db9b9-195">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="db9b9-195">Click **Ok**.</span></span>

7. <span data-ttu-id="db9b9-196">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="db9b9-196">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="db9b9-198">Na **Hightail konfigurace** klikněte na tlačítko **konfigurace Hightail** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="db9b9-198">On the **Hightail Configuration** section, click **Configure Hightail** to open **Configure sign-on** window.</span></span> <span data-ttu-id="db9b9-199">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="db9b9-199">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_configure.png) 

    >[!NOTE] 
    ><span data-ttu-id="db9b9-201">Před konfigurací jednotné přihlašování v aplikaci Hightail, prosím seznamu povolených vaše e-mailovou doménu s Hightail týmu, aby všichni uživatelé, kteří používají tuto doménu můžete použít funkci jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="db9b9-201">Before configuring the Single Sign On at Hightail app, please white list your email domain with Hightail team so that all the users who are using this domain can use Single Sign On functionality.</span></span>


9. <span data-ttu-id="db9b9-202">Pokud chcete získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, musíte k přihlašování ke klientovi Hightail jako správce.</span><span class="sxs-lookup"><span data-stu-id="db9b9-202">To get SSO configured for your application, you need to sign-on to your Hightail tenant as an administrator.</span></span>
   
    <span data-ttu-id="db9b9-203">a.</span><span class="sxs-lookup"><span data-stu-id="db9b9-203">a.</span></span> <span data-ttu-id="db9b9-204">V nabídce v horní části, klikněte **účet** a vyberte **konfigurace SAML**.</span><span class="sxs-lookup"><span data-stu-id="db9b9-204">In the menu on the top, click the **Account** tab and select **Configure SAML**.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_001.png) 

    <span data-ttu-id="db9b9-206">b.</span><span class="sxs-lookup"><span data-stu-id="db9b9-206">b.</span></span> <span data-ttu-id="db9b9-207">Zaškrtněte políčko z **povolit ověřování SAML**.</span><span class="sxs-lookup"><span data-stu-id="db9b9-207">Select the checkbox of **Enable SAML Authentication**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_002.png) 

    <span data-ttu-id="db9b9-209">c.</span><span class="sxs-lookup"><span data-stu-id="db9b9-209">c.</span></span> <span data-ttu-id="db9b9-210">Otevření kódovaného certifikátu kódování base-64 v poznámkovém bloku stáhli z portálu Azure, zkopírujte obsah ho do schránky a vložte jej do **SAML podpisový certifikát tokenů** textové pole.</span><span class="sxs-lookup"><span data-stu-id="db9b9-210">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **SAML Token Signing Certificate** textbox.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_003.png) 

    <span data-ttu-id="db9b9-212">d.</span><span class="sxs-lookup"><span data-stu-id="db9b9-212">d.</span></span> <span data-ttu-id="db9b9-213">V **autority SAML (zprostředkovatele Identity)** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** zkopírovaných z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="db9b9-213">In the **SAML Authority (Identity Provider)** textbox, paste the value of **SAML Single Sign-On Service URL** copied from Azure portal.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_004.png)

    <span data-ttu-id="db9b9-215">e.</span><span class="sxs-lookup"><span data-stu-id="db9b9-215">e.</span></span> <span data-ttu-id="db9b9-216">Pokud chcete nakonfigurovat aplikace **IDP iniciované režimu** vyberte **"Zprostředkovatele Identity (IdP) iniciované přihlášení"**.</span><span class="sxs-lookup"><span data-stu-id="db9b9-216">If you wish to configure the application in **IDP initiated mode** select **"Identity Provider (IdP) initiated log in"**.</span></span> <span data-ttu-id="db9b9-217">Pokud **SP iniciované režimu** vyberte **"Přihlášení iniciované poskytovatelem služeb (SP)"**.</span><span class="sxs-lookup"><span data-stu-id="db9b9-217">If **SP initiated mode** select **"Service Provider (SP) initiated log in"**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_006.png)

    <span data-ttu-id="db9b9-219">f.</span><span class="sxs-lookup"><span data-stu-id="db9b9-219">f.</span></span> <span data-ttu-id="db9b9-220">Zkopírujte adresu URL příjemce SAML pro vaše instance a vložte jej do **adresa URL odpovědi** textového pole v **Hightail domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="db9b9-220">Copy the SAML consumer URL for your instance and paste it in **Reply URL** textbox in **Hightail Domain and URLs** section on Azure portal.</span></span>
    
    <span data-ttu-id="db9b9-221">g.</span><span class="sxs-lookup"><span data-stu-id="db9b9-221">g.</span></span> <span data-ttu-id="db9b9-222">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="db9b9-222">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="db9b9-223">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="db9b9-223">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="db9b9-224">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="db9b9-224">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="db9b9-225">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="db9b9-225">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="db9b9-226">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="db9b9-226">Creating an Azure AD test user</span></span>
<span data-ttu-id="db9b9-227">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="db9b9-227">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="db9b9-229">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="db9b9-229">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="db9b9-230">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="db9b9-230">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="db9b9-232">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="db9b9-232">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="db9b9-234">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="db9b9-234">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="db9b9-236">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="db9b9-236">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="db9b9-238">a.</span><span class="sxs-lookup"><span data-stu-id="db9b9-238">a.</span></span> <span data-ttu-id="db9b9-239">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="db9b9-239">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="db9b9-240">b.</span><span class="sxs-lookup"><span data-stu-id="db9b9-240">b.</span></span> <span data-ttu-id="db9b9-241">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="db9b9-241">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="db9b9-242">c.</span><span class="sxs-lookup"><span data-stu-id="db9b9-242">c.</span></span> <span data-ttu-id="db9b9-243">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="db9b9-243">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="db9b9-244">d.</span><span class="sxs-lookup"><span data-stu-id="db9b9-244">d.</span></span> <span data-ttu-id="db9b9-245">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="db9b9-245">Click **Create**.</span></span>
 
### <a name="creating-a-hightail-test-user"></a><span data-ttu-id="db9b9-246">Vytvoření zkušebního uživatele Hightail</span><span class="sxs-lookup"><span data-stu-id="db9b9-246">Creating a Hightail test user</span></span>

<span data-ttu-id="db9b9-247">Cílem této části je vytvoření uživatele v Hightail nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="db9b9-247">The objective of this section is to create a user called Britta Simon in Hightail.</span></span> 

<span data-ttu-id="db9b9-248">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="db9b9-248">There is no action item for you in this section.</span></span> <span data-ttu-id="db9b9-249">Hightail podporuje zřizování uživatelů za běhu v závislosti na vlastní deklarace.</span><span class="sxs-lookup"><span data-stu-id="db9b9-249">Hightail supports just-in-time user provisioning based on the custom claims.</span></span> <span data-ttu-id="db9b9-250">Pokud jste nakonfigurovali vlastní deklarace identity, jak je uvedeno v části  **[konfigurace Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  vyšší, se automaticky vytvoří uživatele v aplikaci ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="db9b9-250">If you have configured the custom claims as shown in the section **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** above, a user is automatically created in the application it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="db9b9-251">Pokud potřebujete ručně vytvořit uživatele, budete muset kontaktovat [tým podpory Hightail](mailto:support@hightail.com).</span><span class="sxs-lookup"><span data-stu-id="db9b9-251">If you need to create a user manually, you need to contact the [Hightail support team](mailto:support@hightail.com).</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="db9b9-252">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="db9b9-252">Assigning the Azure AD test user</span></span>

<span data-ttu-id="db9b9-253">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Hightail.</span><span class="sxs-lookup"><span data-stu-id="db9b9-253">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Hightail.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="db9b9-255">**Pokud chcete přiřadit Britta Simon Hightail, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="db9b9-255">**To assign Britta Simon to Hightail, perform the following steps:**</span></span>

1. <span data-ttu-id="db9b9-256">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="db9b9-256">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="db9b9-258">V seznamu aplikací vyberte **Hightail**.</span><span class="sxs-lookup"><span data-stu-id="db9b9-258">In the applications list, select **Hightail**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_app.png) 

3. <span data-ttu-id="db9b9-260">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="db9b9-260">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="db9b9-262">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="db9b9-262">Click **Add** button.</span></span> <span data-ttu-id="db9b9-263">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="db9b9-263">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="db9b9-265">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="db9b9-265">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="db9b9-266">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="db9b9-266">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="db9b9-267">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="db9b9-267">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="db9b9-268">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="db9b9-268">Testing single sign-on</span></span>

<span data-ttu-id="db9b9-269">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="db9b9-269">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="db9b9-270">Když kliknete na dlaždici Hightail na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Hightail.</span><span class="sxs-lookup"><span data-stu-id="db9b9-270">When you click the Hightail tile in the Access Panel, you should get automatically signed-on to your Hightail application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="db9b9-271">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="db9b9-271">Additional resources</span></span>

* [<span data-ttu-id="db9b9-272">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="db9b9-272">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="db9b9-273">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="db9b9-273">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_203.png

