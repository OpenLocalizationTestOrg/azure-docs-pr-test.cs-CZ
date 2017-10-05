---
title: "Kurz: Azure Active Directory integrace s správy Portfolia certifikační Autority | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a správy Portfolia certifikační Autority."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ca9d5e71-e429-4891-8d10-3498e7210e89
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 4ca9268c26f681fcc96955b6161fe4a119b2dcf4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ca-ppm"></a><span data-ttu-id="354f7-103">Kurz: Azure Active Directory integrace s správy Portfolia certifikační Autority</span><span class="sxs-lookup"><span data-stu-id="354f7-103">Tutorial: Azure Active Directory integration with CA PPM</span></span>

<span data-ttu-id="354f7-104">V tomto kurzu zjistěte, jak integrovat správy Portfolia certifikační Autority s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="354f7-104">In this tutorial, you learn how to integrate CA PPM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="354f7-105">Integrace správy Portfolia certifikační Autority s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="354f7-105">Integrating CA PPM with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="354f7-106">Můžete řídit ve službě Azure AD, který má přístup k certifikační Autority stran za MINUTU</span><span class="sxs-lookup"><span data-stu-id="354f7-106">You can control in Azure AD who has access to CA PPM</span></span>
- <span data-ttu-id="354f7-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k správy Portfolia certifikační Autority (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="354f7-107">You can enable your users to automatically get signed-on to CA PPM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="354f7-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="354f7-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="354f7-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="354f7-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="354f7-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="354f7-110">Prerequisites</span></span>

<span data-ttu-id="354f7-111">Konfigurace integrace Azure AD s správy Portfolia certifikační Autority, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="354f7-111">To configure Azure AD integration with CA PPM, you need the following items:</span></span>

- <span data-ttu-id="354f7-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="354f7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="354f7-113">Certifikační Autoritu správy Portfolia jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="354f7-113">A CA PPM single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="354f7-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="354f7-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="354f7-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="354f7-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="354f7-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="354f7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="354f7-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="354f7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="354f7-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="354f7-118">Scenario description</span></span>
<span data-ttu-id="354f7-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="354f7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="354f7-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="354f7-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="354f7-121">Přidání správy Portfolia certifikační Autority z Galerie</span><span class="sxs-lookup"><span data-stu-id="354f7-121">Adding CA PPM from the gallery</span></span>
2. <span data-ttu-id="354f7-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="354f7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ca-ppm-from-the-gallery"></a><span data-ttu-id="354f7-123">Přidání správy Portfolia certifikační Autority z Galerie</span><span class="sxs-lookup"><span data-stu-id="354f7-123">Adding CA PPM from the gallery</span></span>
<span data-ttu-id="354f7-124">Při konfiguraci integrace správy Portfolia certifikační Autority do služby Azure AD potřebujete přidat certifikační Autoritu správy Portfolia z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="354f7-124">To configure the integration of CA PPM into Azure AD, you need to add CA PPM from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="354f7-125">**Pokud chcete přidat certifikační Autoritu správy Portfolia z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="354f7-125">**To add CA PPM from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="354f7-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="354f7-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="354f7-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="354f7-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="354f7-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="354f7-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="354f7-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="354f7-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="354f7-133">Do vyhledávacího pole zadejte **správy Portfolia certifikační Autority**.</span><span class="sxs-lookup"><span data-stu-id="354f7-133">In the search box, type **CA PPM**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_search.png)

5. <span data-ttu-id="354f7-135">Na panelu výsledků vyberte **správy Portfolia certifikační Autority**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="354f7-135">In the results panel, select **CA PPM**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="354f7-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="354f7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="354f7-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s správy Portfolia certifikační Autority podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="354f7-138">In this section, you configure and test Azure AD single sign-on with CA PPM based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="354f7-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v správy Portfolia certifikační Autority je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="354f7-139">For single sign-on to work, Azure AD needs to know what the counterpart user in CA PPM is to a user in Azure AD.</span></span> <span data-ttu-id="354f7-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v správy Portfolia certifikační Autority je potřeba vytvořit.</span><span class="sxs-lookup"><span data-stu-id="354f7-140">In other words, a link relationship between an Azure AD user and the related user in CA PPM needs to be established.</span></span>

<span data-ttu-id="354f7-141">V CA správy Portfolia, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="354f7-141">In CA PPM, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="354f7-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s správy Portfolia certifikační Autority, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="354f7-142">To configure and test Azure AD single sign-on with CA PPM, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="354f7-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="354f7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="354f7-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="354f7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="354f7-145">**[Vytvoření zkušebního uživatele správy Portfolia certifikační Autority](#creating-a-ca-ppm-test-user)**  – Pokud chcete mít protějšek Britta Simon v CA správy Portfolia, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="354f7-145">**[Creating a CA PPM test user](#creating-a-ca-ppm-test-user)** - to have a counterpart of Britta Simon in CA PPM that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="354f7-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="354f7-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="354f7-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="354f7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="354f7-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="354f7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="354f7-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci správy Portfolia certifikační Autority.</span><span class="sxs-lookup"><span data-stu-id="354f7-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your CA PPM application.</span></span>

<span data-ttu-id="354f7-150">**Ke konfiguraci Azure AD jednotné přihlašování s správy Portfolia certifikační Autority, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="354f7-150">**To configure Azure AD single sign-on with CA PPM, perform the following steps:**</span></span>

1. <span data-ttu-id="354f7-151">Na portálu Azure na **správy Portfolia certifikační Autority** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="354f7-151">In the Azure portal, on the **CA PPM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="354f7-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="354f7-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_samlbase.png)

3. <span data-ttu-id="354f7-155">Na **certifikační Autority správy Portfolia domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="354f7-155">On the **CA PPM Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_url.png)

    <span data-ttu-id="354f7-157">a.</span><span class="sxs-lookup"><span data-stu-id="354f7-157">a.</span></span> <span data-ttu-id="354f7-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://ca.ondemand.saml.20.post.<companyname>`</span><span class="sxs-lookup"><span data-stu-id="354f7-158">In the **Identifier** textbox, type a URL using the following pattern: `https://ca.ondemand.saml.20.post.<companyname>`</span></span>
    
    <span data-ttu-id="354f7-159">b.</span><span class="sxs-lookup"><span data-stu-id="354f7-159">b.</span></span> <span data-ttu-id="354f7-160">V **adresa URL odpovědi** textovému poli, typ jako:`https://fedsso.ondemand.ca.com/affwebservices/public/saml2assertionconsumer`</span><span class="sxs-lookup"><span data-stu-id="354f7-160">In the **Reply URL** textbox, type as: `https://fedsso.ondemand.ca.com/affwebservices/public/saml2assertionconsumer`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="354f7-161">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="354f7-161">This value is not real.</span></span> <span data-ttu-id="354f7-162">Aktualizujte tato hodnota se skutečným identifikátorem.</span><span class="sxs-lookup"><span data-stu-id="354f7-162">Update this value with the actual Identifier.</span></span> <span data-ttu-id="354f7-163">Obraťte se na [tým podpory správy Portfolia certifikační Autority](mailto:catechnicalsupport@ca.com) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="354f7-163">Contact [CA PPM support team](mailto:catechnicalsupport@ca.com) to get this value.</span></span>
 
4. <span data-ttu-id="354f7-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="354f7-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_certificate.png) 

5. <span data-ttu-id="354f7-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="354f7-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="354f7-168">Na **konfiguraci certifikační Autority správy Portfolia** klikněte na tlačítko **konfigurace certifikační Autority správy Portfolia** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="354f7-168">On the **CA PPM Configuration** section, click **Configure CA PPM** to open **Configure sign-on** window.</span></span> <span data-ttu-id="354f7-169">Kopírování **SAML Entity ID** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="354f7-169">Copy the **SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_configure.png) 

7. <span data-ttu-id="354f7-171">Konfigurace jednotného přihlašování na **správy Portfolia certifikační Autority** straně, budete muset odeslat stažené **Certificate(Base64)** a **SAML Entity ID** k [tým podpory správy Portfolia certifikační Autority](mailto:catechnicalsupport@ca.com).</span><span class="sxs-lookup"><span data-stu-id="354f7-171">To configure single sign-on on **CA PPM** side, you need to send the downloaded **Certificate(Base64)** and **SAML Entity ID** to [CA PPM support team](mailto:catechnicalsupport@ca.com).</span></span>

> [!TIP]
> <span data-ttu-id="354f7-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="354f7-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="354f7-173">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="354f7-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="354f7-174">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="354f7-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="354f7-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="354f7-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="354f7-176">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="354f7-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="354f7-178">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="354f7-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="354f7-179">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="354f7-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="354f7-181">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="354f7-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="354f7-183">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="354f7-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="354f7-185">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="354f7-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="354f7-187">a.</span><span class="sxs-lookup"><span data-stu-id="354f7-187">a.</span></span> <span data-ttu-id="354f7-188">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="354f7-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="354f7-189">b.</span><span class="sxs-lookup"><span data-stu-id="354f7-189">b.</span></span> <span data-ttu-id="354f7-190">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="354f7-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="354f7-191">c.</span><span class="sxs-lookup"><span data-stu-id="354f7-191">c.</span></span> <span data-ttu-id="354f7-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="354f7-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="354f7-193">d.</span><span class="sxs-lookup"><span data-stu-id="354f7-193">d.</span></span> <span data-ttu-id="354f7-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="354f7-194">Click **Create**.</span></span>
 
### <a name="creating-a-ca-ppm-test-user"></a><span data-ttu-id="354f7-195">Vytvoření zkušebního uživatele správy Portfolia certifikační Autority</span><span class="sxs-lookup"><span data-stu-id="354f7-195">Creating a CA PPM test user</span></span>

<span data-ttu-id="354f7-196">V této části vytvoříte uživatele volal Britta Simon v správy Portfolia certifikační Autority.</span><span class="sxs-lookup"><span data-stu-id="354f7-196">In this section, you create a user called Britta Simon in CA PPM.</span></span> <span data-ttu-id="354f7-197">Práce s [tým podpory správy Portfolia certifikační Autority](mailto:catechnicalsupport@ca.com) přidat uživatele do platformy správy Portfolia certifikační Autority.</span><span class="sxs-lookup"><span data-stu-id="354f7-197">Work with [CA PPM support team](mailto:catechnicalsupport@ca.com) to add the users in the CA PPM platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="354f7-198">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="354f7-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="354f7-199">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu správy Portfolia certifikační Autority.</span><span class="sxs-lookup"><span data-stu-id="354f7-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to CA PPM.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="354f7-201">**Pokud chcete přiřadit Britta Simon správy Portfolia certifikační Autority, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="354f7-201">**To assign Britta Simon to CA PPM, perform the following steps:**</span></span>

1. <span data-ttu-id="354f7-202">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="354f7-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="354f7-204">V seznamu aplikací vyberte **správy Portfolia certifikační Autority**.</span><span class="sxs-lookup"><span data-stu-id="354f7-204">In the applications list, select **CA PPM**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_app.png) 

3. <span data-ttu-id="354f7-206">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="354f7-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="354f7-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="354f7-208">Click **Add** button.</span></span> <span data-ttu-id="354f7-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="354f7-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="354f7-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="354f7-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="354f7-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="354f7-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="354f7-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="354f7-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="354f7-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="354f7-214">Testing single sign-on</span></span>

<span data-ttu-id="354f7-215">V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="354f7-215">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="354f7-216">Když kliknete na dlaždici správy Portfolia certifikační Autority na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci správy Portfolia certifikační Autority.</span><span class="sxs-lookup"><span data-stu-id="354f7-216">When you click the CA PPM tile in the Access Panel, you should get automatically signed-on to your CA PPM application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="354f7-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="354f7-217">Additional resources</span></span>

* [<span data-ttu-id="354f7-218">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="354f7-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="354f7-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="354f7-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_203.png

