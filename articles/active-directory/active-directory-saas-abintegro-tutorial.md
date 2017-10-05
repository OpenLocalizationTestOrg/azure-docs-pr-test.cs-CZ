---
title: 'Kurz: Azure Active Directory integrace s Abintegro | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Abintegro."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 99287e1f-4189-494a-97c8-e1c03d047fd3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: a2a3c1a7a338ee1cb35dd08176ad3bb5f3cdc319
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-abintegro"></a><span data-ttu-id="97998-103">Kurz: Azure Active Directory integrace s Abintegro</span><span class="sxs-lookup"><span data-stu-id="97998-103">Tutorial: Azure Active Directory integration with Abintegro</span></span>

<span data-ttu-id="97998-104">V tomto kurzu zjistěte, jak integrovat Abintegro s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="97998-104">In this tutorial, you learn how to integrate Abintegro with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="97998-105">Integrace Abintegro s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="97998-105">Integrating Abintegro with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="97998-106">Můžete řídit ve službě Azure AD, který má přístup k Abintegro</span><span class="sxs-lookup"><span data-stu-id="97998-106">You can control in Azure AD who has access to Abintegro</span></span>
- <span data-ttu-id="97998-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Abintegro (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="97998-107">You can enable your users to automatically get signed-on to Abintegro (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="97998-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="97998-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="97998-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="97998-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="97998-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="97998-110">Prerequisites</span></span>

<span data-ttu-id="97998-111">Konfigurace integrace Azure AD s Abintegro, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="97998-111">To configure Azure AD integration with Abintegro, you need the following items:</span></span>

- <span data-ttu-id="97998-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="97998-112">An Azure AD subscription</span></span>
- <span data-ttu-id="97998-113">Abintegro jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="97998-113">An Abintegro single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="97998-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="97998-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="97998-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="97998-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="97998-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="97998-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="97998-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="97998-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="97998-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="97998-118">Scenario description</span></span>
<span data-ttu-id="97998-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="97998-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="97998-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="97998-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="97998-121">Přidání Abintegro z Galerie</span><span class="sxs-lookup"><span data-stu-id="97998-121">Adding Abintegro from the gallery</span></span>
2. <span data-ttu-id="97998-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="97998-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-abintegro-from-the-gallery"></a><span data-ttu-id="97998-123">Přidání Abintegro z Galerie</span><span class="sxs-lookup"><span data-stu-id="97998-123">Adding Abintegro from the gallery</span></span>
<span data-ttu-id="97998-124">Při konfiguraci integrace Abintegro do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Abintegro z galerie.</span><span class="sxs-lookup"><span data-stu-id="97998-124">To configure the integration of Abintegro into Azure AD, you need to add Abintegro from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="97998-125">**Pokud chcete přidat Abintegro z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="97998-125">**To add Abintegro from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="97998-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="97998-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="97998-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="97998-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="97998-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="97998-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="97998-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="97998-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="97998-133">Do vyhledávacího pole zadejte **Abintegro**.</span><span class="sxs-lookup"><span data-stu-id="97998-133">In the search box, type **Abintegro**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-abintegro-tutorial/tutorial_abintegro_search.png)

5. <span data-ttu-id="97998-135">Na panelu výsledků vyberte **Abintegro**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="97998-135">In the results panel, select **Abintegro**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-abintegro-tutorial/tutorial_abintegro_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="97998-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="97998-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="97998-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Abintegro podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="97998-138">In this section, you configure and test Azure AD single sign-on with Abintegro based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="97998-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Abintegro je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="97998-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Abintegro is to a user in Azure AD.</span></span> <span data-ttu-id="97998-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Abintegro musí navázat.</span><span class="sxs-lookup"><span data-stu-id="97998-140">In other words, a link relationship between an Azure AD user and the related user in Abintegro needs to be established.</span></span>

<span data-ttu-id="97998-141">V Abintegro, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="97998-141">In Abintegro, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="97998-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Abintegro, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="97998-142">To configure and test Azure AD single sign-on with Abintegro, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="97998-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="97998-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="97998-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="97998-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="97998-145">**[Vytváření testovacího uživatele Abintegro](#creating-an-abintegro-test-user)**  – Pokud chcete mít protějšek Britta Simon v Abintegro propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="97998-145">**[Creating an Abintegro test user](#creating-an-abintegro-test-user)** - to have a counterpart of Britta Simon in Abintegro that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="97998-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="97998-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="97998-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="97998-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="97998-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="97998-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="97998-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Abintegro.</span><span class="sxs-lookup"><span data-stu-id="97998-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Abintegro application.</span></span>

<span data-ttu-id="97998-150">**Ke konfiguraci Azure AD jednotné přihlašování s Abintegro, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="97998-150">**To configure Azure AD single sign-on with Abintegro, perform the following steps:**</span></span>

1. <span data-ttu-id="97998-151">Na portálu Azure na **Abintegro** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="97998-151">In the Azure portal, on the **Abintegro** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="97998-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="97998-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-abintegro-tutorial/tutorial_abintegro_samlbase.png)

3. <span data-ttu-id="97998-155">Na **Abintegro domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="97998-155">On the **Abintegro Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-abintegro-tutorial/tutorial_abintegro_url.png)

    <span data-ttu-id="97998-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://dev.abintegro.com/Shibboleth.sso/Login?entityID=<Issuer>&target=https://dev.abintegro.com/secure/`</span><span class="sxs-lookup"><span data-stu-id="97998-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://dev.abintegro.com/Shibboleth.sso/Login?entityID=<Issuer>&target=https://dev.abintegro.com/secure/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="97998-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="97998-158">This value is not real.</span></span> <span data-ttu-id="97998-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="97998-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="97998-160">Obraťte se na [tým podpory Abintegro klienta](mailto:support@abintegro.com) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="97998-160">Contact [Abintegro Client support team](mailto:support@abintegro.com) to get this value.</span></span> 
 
4. <span data-ttu-id="97998-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="97998-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-abintegro-tutorial/tutorial_abintegro_certificate.png) 

5. <span data-ttu-id="97998-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="97998-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-abintegro-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="97998-165">Konfigurace jednotného přihlašování na **Abintegro** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Abintegro](mailto:support@abintegro.com).</span><span class="sxs-lookup"><span data-stu-id="97998-165">To configure single sign-on on **Abintegro** side, you need to send the downloaded **Metadata XML** to [Abintegro support team](mailto:support@abintegro.com).</span></span> <span data-ttu-id="97998-166">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="97998-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="97998-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="97998-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="97998-168">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="97998-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="97998-169">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="97998-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="97998-170">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="97998-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="97998-171">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="97998-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="97998-173">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="97998-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="97998-174">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="97998-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-abintegro-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="97998-176">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="97998-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-abintegro-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="97998-178">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="97998-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-abintegro-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="97998-180">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="97998-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-abintegro-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="97998-182">a.</span><span class="sxs-lookup"><span data-stu-id="97998-182">a.</span></span> <span data-ttu-id="97998-183">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="97998-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="97998-184">b.</span><span class="sxs-lookup"><span data-stu-id="97998-184">b.</span></span> <span data-ttu-id="97998-185">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="97998-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="97998-186">c.</span><span class="sxs-lookup"><span data-stu-id="97998-186">c.</span></span> <span data-ttu-id="97998-187">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="97998-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="97998-188">d.</span><span class="sxs-lookup"><span data-stu-id="97998-188">d.</span></span> <span data-ttu-id="97998-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="97998-189">Click **Create**.</span></span>
 
### <a name="creating-an-abintegro-test-user"></a><span data-ttu-id="97998-190">Vytváření testovacího uživatele Abintegro</span><span class="sxs-lookup"><span data-stu-id="97998-190">Creating an Abintegro test user</span></span>

<span data-ttu-id="97998-191">Neexistuje žádná položka akce můžete nakonfigurovat na Abintegro zřizování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="97998-191">There is no action item for you to configure user provisioning to Abintegro.</span></span> <span data-ttu-id="97998-192">Když přiřazený uživatel se pokusí přihlásit pomocí přístupového panelu Abintegro, Abintegro ověří, zda uživatel existuje.</span><span class="sxs-lookup"><span data-stu-id="97998-192">When an assigned user tries to log into Abintegro using the access panel, Abintegro checks whether the user exists.</span></span>
  
<span data-ttu-id="97998-193">Pokud neexistuje žádný účet k dispozici dosud, je vytvářena automaticky nástrojem Abintegro.</span><span class="sxs-lookup"><span data-stu-id="97998-193">If there is no user account available yet, it is automatically created by Abintegro.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="97998-194">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="97998-194">Assigning the Azure AD test user</span></span>

<span data-ttu-id="97998-195">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Abintegro.</span><span class="sxs-lookup"><span data-stu-id="97998-195">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Abintegro.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="97998-197">**Pokud chcete přiřadit Britta Simon Abintegro, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="97998-197">**To assign Britta Simon to Abintegro, perform the following steps:**</span></span>

1. <span data-ttu-id="97998-198">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="97998-198">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="97998-200">V seznamu aplikací vyberte **Abintegro**.</span><span class="sxs-lookup"><span data-stu-id="97998-200">In the applications list, select **Abintegro**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-abintegro-tutorial/tutorial_abintegro_app.png) 

3. <span data-ttu-id="97998-202">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="97998-202">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="97998-204">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="97998-204">Click **Add** button.</span></span> <span data-ttu-id="97998-205">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="97998-205">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="97998-207">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="97998-207">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="97998-208">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="97998-208">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="97998-209">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="97998-209">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="97998-210">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="97998-210">Testing single sign-on</span></span>

<span data-ttu-id="97998-211">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="97998-211">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="97998-212">Když kliknete na dlaždici Abintegro na přístupovém panelu, měli byste obdržet přihlašovací stránku Abintegro aplikace.</span><span class="sxs-lookup"><span data-stu-id="97998-212">When you click the Abintegro tile in the Access Panel, you should get login page of Abintegro application.</span></span>
<span data-ttu-id="97998-213">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="97998-213">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="97998-214">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="97998-214">Additional resources</span></span>

* [<span data-ttu-id="97998-215">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="97998-215">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="97998-216">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="97998-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-abintegro-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-abintegro-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-abintegro-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-abintegro-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-abintegro-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-abintegro-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-abintegro-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-abintegro-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-abintegro-tutorial/tutorial_general_203.png

