---
title: 'Kurz: Azure Active Directory integrace s Jostle | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Jostle."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9ca4ca1f-8f68-4225-81a6-1666b486d6a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 0ca8aca1446a38643ce9f6751b6fe9cae1eaa5b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jostle"></a><span data-ttu-id="d128d-103">Kurz: Azure Active Directory integrace s Jostle</span><span class="sxs-lookup"><span data-stu-id="d128d-103">Tutorial: Azure Active Directory integration with Jostle</span></span>

<span data-ttu-id="d128d-104">V tomto kurzu zjistěte, jak integrovat Jostle s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d128d-104">In this tutorial, you learn how to integrate Jostle with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d128d-105">Integrace Jostle s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d128d-105">Integrating Jostle with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d128d-106">Můžete řídit ve službě Azure AD, který má přístup k Jostle</span><span class="sxs-lookup"><span data-stu-id="d128d-106">You can control in Azure AD who has access to Jostle</span></span>
- <span data-ttu-id="d128d-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Jostle (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="d128d-107">You can enable your users to automatically get signed-on to Jostle (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d128d-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d128d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d128d-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d128d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d128d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d128d-110">Prerequisites</span></span>

<span data-ttu-id="d128d-111">Konfigurace integrace Azure AD s Jostle, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="d128d-111">To configure Azure AD integration with Jostle, you need the following items:</span></span>

- <span data-ttu-id="d128d-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d128d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d128d-113">Jostle jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d128d-113">A Jostle single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d128d-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d128d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d128d-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d128d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d128d-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d128d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d128d-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d128d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d128d-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d128d-118">Scenario description</span></span>
<span data-ttu-id="d128d-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d128d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d128d-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d128d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d128d-121">Přidání Jostle z Galerie</span><span class="sxs-lookup"><span data-stu-id="d128d-121">Adding Jostle from the gallery</span></span>
2. <span data-ttu-id="d128d-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d128d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jostle-from-the-gallery"></a><span data-ttu-id="d128d-123">Přidání Jostle z Galerie</span><span class="sxs-lookup"><span data-stu-id="d128d-123">Adding Jostle from the gallery</span></span>
<span data-ttu-id="d128d-124">Při konfiguraci integrace Jostle do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Jostle z galerie.</span><span class="sxs-lookup"><span data-stu-id="d128d-124">To configure the integration of Jostle into Azure AD, you need to add Jostle from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d128d-125">**Pokud chcete přidat Jostle z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d128d-125">**To add Jostle from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d128d-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d128d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d128d-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d128d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d128d-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d128d-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="d128d-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d128d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="d128d-133">Do vyhledávacího pole zadejte **Jostle**.</span><span class="sxs-lookup"><span data-stu-id="d128d-133">In the search box, type **Jostle**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_search.png)

5. <span data-ttu-id="d128d-135">Na panelu výsledků vyberte **Jostle**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d128d-135">In the results panel, select **Jostle**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d128d-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d128d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d128d-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Jostle podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="d128d-138">In this section, you configure and test Azure AD single sign-on with Jostle based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d128d-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Jostle je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d128d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Jostle is to a user in Azure AD.</span></span> <span data-ttu-id="d128d-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Jostle musí navázat.</span><span class="sxs-lookup"><span data-stu-id="d128d-140">In other words, a link relationship between an Azure AD user and the related user in Jostle needs to be established.</span></span>

<span data-ttu-id="d128d-141">V Jostle, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="d128d-141">In Jostle, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d128d-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Jostle, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="d128d-142">To configure and test Azure AD single sign-on with Jostle, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d128d-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="d128d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d128d-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d128d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d128d-145">**[Vytvoření zkušebního uživatele Jostle](#creating-a-jostle-test-user)**  – Pokud chcete mít protějšek Britta Simon v Jostle propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="d128d-145">**[Creating a Jostle test user](#creating-a-jostle-test-user)** - to have a counterpart of Britta Simon in Jostle that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d128d-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d128d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d128d-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d128d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d128d-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d128d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d128d-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Jostle.</span><span class="sxs-lookup"><span data-stu-id="d128d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Jostle application.</span></span>

<span data-ttu-id="d128d-150">**Ke konfiguraci Azure AD jednotné přihlašování s Jostle, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d128d-150">**To configure Azure AD single sign-on with Jostle, perform the following steps:**</span></span>

1. <span data-ttu-id="d128d-151">Na portálu Azure na **Jostle** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d128d-151">In the Azure portal, on the **Jostle** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="d128d-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d128d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_samlbase.png)

3. <span data-ttu-id="d128d-155">Na **Jostle domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d128d-155">On the **Jostle Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_url.png)

    <span data-ttu-id="d128d-157">a.</span><span class="sxs-lookup"><span data-stu-id="d128d-157">a.</span></span> <span data-ttu-id="d128d-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tanent name>.jostle.us/jostle-prod/`</span><span class="sxs-lookup"><span data-stu-id="d128d-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tanent name>.jostle.us/jostle-prod/`</span></span>

    <span data-ttu-id="d128d-159">b.</span><span class="sxs-lookup"><span data-stu-id="d128d-159">b.</span></span> <span data-ttu-id="d128d-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tanent name>.jostle.us`</span><span class="sxs-lookup"><span data-stu-id="d128d-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<tanent name>.jostle.us`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d128d-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="d128d-161">These values are not real.</span></span> <span data-ttu-id="d128d-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="d128d-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d128d-163">Obraťte se na [tým podpory Jostle](mailto:support@jostle.me) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="d128d-163">Contact [Jostle support team](mailto:support@jostle.me) to get these values.</span></span> 
 


4. <span data-ttu-id="d128d-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="d128d-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_certificate.png) 

5. <span data-ttu-id="d128d-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d128d-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jostle-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="d128d-168">Na straně Jostle nakonfigurovat jednotné přihlašování, budete muset odeslat stažené metadata XML do [tým podpory Jostle](mailto:support@jostle.me).</span><span class="sxs-lookup"><span data-stu-id="d128d-168">To configure single sign-on on Jostle side, you need to send the downloaded metadata XML to [Jostle support team](mailto:support@jostle.me).</span></span> <span data-ttu-id="d128d-169">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="d128d-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span> 

> [!TIP]
> <span data-ttu-id="d128d-170">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="d128d-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d128d-171">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="d128d-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d128d-172">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d128d-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d128d-173">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d128d-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="d128d-174">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d128d-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="d128d-176">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d128d-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d128d-177">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d128d-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jostle-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d128d-179">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d128d-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jostle-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d128d-181">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d128d-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jostle-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d128d-183">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d128d-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jostle-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d128d-185">a.</span><span class="sxs-lookup"><span data-stu-id="d128d-185">a.</span></span> <span data-ttu-id="d128d-186">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d128d-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d128d-187">b.</span><span class="sxs-lookup"><span data-stu-id="d128d-187">b.</span></span> <span data-ttu-id="d128d-188">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d128d-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d128d-189">c.</span><span class="sxs-lookup"><span data-stu-id="d128d-189">c.</span></span> <span data-ttu-id="d128d-190">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="d128d-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d128d-191">d.</span><span class="sxs-lookup"><span data-stu-id="d128d-191">d.</span></span> <span data-ttu-id="d128d-192">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d128d-192">Click **Create**.</span></span>
 
### <a name="creating-a-jostle-test-user"></a><span data-ttu-id="d128d-193">Vytvoření zkušebního uživatele Jostle</span><span class="sxs-lookup"><span data-stu-id="d128d-193">Creating a Jostle test user</span></span>

<span data-ttu-id="d128d-194">V této části vytvoříte volal Britta Simon v Jostle uživatele.</span><span class="sxs-lookup"><span data-stu-id="d128d-194">In this section, you create a user called Britta Simon in Jostle.</span></span> <span data-ttu-id="d128d-195">Pokud nevíte jak přidat Britta Simon v Jostle, kontaktujte prosím s [tým podpory Jostle](mailto:support@jostle.me) k přidání testovacího uživatele a povolení jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d128d-195">If you don't know how to add Britta Simon in Jostle, please contact with [Jostle support team](mailto:support@jostle.me) to add the test user and enable SSO.</span></span>

> [!NOTE]
> <span data-ttu-id="d128d-196">Držitel účtu Azure Active Directory obdrží e-mailu a dodržuje odkaz potvrďte svůj účet, pak se změní na aktivní.</span><span class="sxs-lookup"><span data-stu-id="d128d-196">The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d128d-197">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d128d-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d128d-198">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Jostle.</span><span class="sxs-lookup"><span data-stu-id="d128d-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Jostle.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="d128d-200">**Pokud chcete přiřadit Britta Simon Jostle, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d128d-200">**To assign Britta Simon to Jostle, perform the following steps:**</span></span>

1. <span data-ttu-id="d128d-201">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d128d-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d128d-203">V seznamu aplikací vyberte **Jostle**.</span><span class="sxs-lookup"><span data-stu-id="d128d-203">In the applications list, select **Jostle**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_app.png) 

3. <span data-ttu-id="d128d-205">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d128d-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="d128d-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d128d-207">Click **Add** button.</span></span> <span data-ttu-id="d128d-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d128d-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="d128d-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d128d-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d128d-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d128d-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d128d-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d128d-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d128d-213">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d128d-213">Testing single sign-on</span></span>

<span data-ttu-id="d128d-214">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d128d-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d128d-215">Když kliknete na dlaždici Jostle na přístupovém panelu, měli byste obdržet automaticky přihlašovací stránku Jostle aplikace.</span><span class="sxs-lookup"><span data-stu-id="d128d-215">When you click the Jostle tile in the Access Panel, you should get automatically login page of Jostle application.</span></span>
<span data-ttu-id="d128d-216">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d128d-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d128d-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d128d-217">Additional resources</span></span>

* [<span data-ttu-id="d128d-218">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d128d-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d128d-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d128d-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_203.png

