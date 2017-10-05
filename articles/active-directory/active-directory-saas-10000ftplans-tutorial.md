---
title: "Kurz: Azure Active Directory integrace s 10 000 ft plány | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a 10 000 ft plány."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b60c955e-8fa3-4872-a897-c4e81fd7beac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: c81a6adb3abad58af149bbef6f5dff736c142f51
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-10000ft-plans"></a><span data-ttu-id="0949e-103">Kurz: Azure Active Directory integrace s 10 000 ft plány</span><span class="sxs-lookup"><span data-stu-id="0949e-103">Tutorial: Azure Active Directory integration with 10,000ft Plans</span></span>

<span data-ttu-id="0949e-104">V tomto kurzu zjistíte integrace 10 000 ft plánů služby Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0949e-104">In this tutorial, you learn how to integrate 10,000ft Plans with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0949e-105">Integrace 10 000 ft plány s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="0949e-105">Integrating 10,000ft Plans with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0949e-106">Můžete řídit ve službě Azure AD, který má přístup do 10 000 ft plánů</span><span class="sxs-lookup"><span data-stu-id="0949e-106">You can control in Azure AD who has access to 10,000ft Plans</span></span>
- <span data-ttu-id="0949e-107">Můžete povolit uživatelům, aby automaticky získat přihlášeného do 10 000 ft plánů (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="0949e-107">You can enable your users to automatically get signed-on to 10,000ft Plans (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0949e-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0949e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="0949e-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0949e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0949e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0949e-110">Prerequisites</span></span>

<span data-ttu-id="0949e-111">Konfigurace integrace Azure AD s 10 000 ft plány, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="0949e-111">To configure Azure AD integration with 10,000ft Plans, you need the following items:</span></span>

- <span data-ttu-id="0949e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="0949e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0949e-113">10 000 ft plány jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="0949e-113">A 10,000ft Plans single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0949e-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0949e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0949e-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="0949e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0949e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="0949e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0949e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0949e-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0949e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="0949e-118">Scenario description</span></span>
<span data-ttu-id="0949e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0949e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0949e-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="0949e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0949e-121">Přidání 10 000 ft plány z Galerie</span><span class="sxs-lookup"><span data-stu-id="0949e-121">Adding 10,000ft Plans from the gallery</span></span>
2. <span data-ttu-id="0949e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0949e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-10000ft-plans-from-the-gallery"></a><span data-ttu-id="0949e-123">Přidání 10 000 ft plány z Galerie</span><span class="sxs-lookup"><span data-stu-id="0949e-123">Adding 10,000ft Plans from the gallery</span></span>
<span data-ttu-id="0949e-124">Při konfiguraci integrace 10 000 ft plány do služby Azure AD potřebujete přidat 10 000 ft plány z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="0949e-124">To configure the integration of 10,000ft Plans into Azure AD, you need to add 10,000ft Plans from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0949e-125">**Pokud chcete přidat 10 000 ft plány z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0949e-125">**To add 10,000ft Plans from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0949e-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0949e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0949e-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="0949e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0949e-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0949e-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="0949e-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0949e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="0949e-133">Do vyhledávacího pole zadejte **10 000 ft plány**.</span><span class="sxs-lookup"><span data-stu-id="0949e-133">In the search box, type **10,000ft Plans**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_search.png)

5. <span data-ttu-id="0949e-135">Na panelu výsledků vyberte **10 000 ft plány**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0949e-135">In the results panel, select **10,000ft Plans**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0949e-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0949e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0949e-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s 10 000 ft plány podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="0949e-138">In this section, you configure and test Azure AD single sign-on with 10,000ft Plans based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0949e-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v plánech úrovně 10 000 ft je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0949e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in 10,000ft Plans is to a user in Azure AD.</span></span> <span data-ttu-id="0949e-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v 10 000 ft plány musí navázat.</span><span class="sxs-lookup"><span data-stu-id="0949e-140">In other words, a link relationship between an Azure AD user and the related user in 10,000ft Plans needs to be established.</span></span>

<span data-ttu-id="0949e-141">V plánech 10 000 ft přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="0949e-141">In 10,000ft Plans, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="0949e-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s 10 000 ft plány, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="0949e-142">To configure and test Azure AD single sign-on with 10,000ft Plans, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0949e-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="0949e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0949e-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0949e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0949e-145">**[Vytváření 10 000 ft testovací plány uživatel](#creating-a-10000ft-plans-test-user)**  – Pokud chcete mít protějšek Britta Simon v 10 000 ft plány propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="0949e-145">**[Creating a 10,000ft Plans test user](#creating-a-10000ft-plans-test-user)** - to have a counterpart of Britta Simon in 10,000ft Plans that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0949e-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0949e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0949e-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0949e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0949e-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0949e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0949e-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci 10 000 ft plány.</span><span class="sxs-lookup"><span data-stu-id="0949e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your 10,000ft Plans application.</span></span>

<span data-ttu-id="0949e-150">**Ke konfiguraci Azure AD jednotné přihlašování s 10 000 ft plány, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0949e-150">**To configure Azure AD single sign-on with 10,000ft Plans, perform the following steps:**</span></span>

1. <span data-ttu-id="0949e-151">Na portálu Azure na **10 000 ft plány** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="0949e-151">In the Azure portal, on the **10,000ft Plans** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="0949e-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0949e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_samlbase.png)

3. <span data-ttu-id="0949e-155">Na **10 000 ft plány domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0949e-155">On the **10,000ft Plans Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_url.png)

    <span data-ttu-id="0949e-157">a.</span><span class="sxs-lookup"><span data-stu-id="0949e-157">a.</span></span> <span data-ttu-id="0949e-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL:`https://app.10000ft.com`</span><span class="sxs-lookup"><span data-stu-id="0949e-158">In the **Sign-on URL** textbox, type the URL: `https://app.10000ft.com`</span></span>

    <span data-ttu-id="0949e-159">b.</span><span class="sxs-lookup"><span data-stu-id="0949e-159">b.</span></span> <span data-ttu-id="0949e-160">V **identifikátor** textovému poli, zadejte adresu URL:`https://app.10000ft.com/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="0949e-160">In the **Identifier** textbox, type the URL: `https://app.10000ft.com/saml/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0949e-161">Hodnota **identifikátor** se liší, pokud máte vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="0949e-161">The value for **Identifier** is different if you have a custom domain.</span></span> <span data-ttu-id="0949e-162">Obraťte se na [tým podpory plány 10 000 ft](https://www.10000ft.com/plans/support) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0949e-162">Contact [10,000ft Plans support team](https://www.10000ft.com/plans/support) to get this value.</span></span> 
 
4. <span data-ttu-id="0949e-163">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Raw)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="0949e-163">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_certificate.png) 

5. <span data-ttu-id="0949e-165">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0949e-165">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0949e-167">Na **10 000 ft plány konfigurace** klikněte na tlačítko **10 000 ft plány nakonfigurovat** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="0949e-167">On the **10,000ft Plans Configuration** section, click **Configure 10,000ft Plans** to open **Configure sign-on** window.</span></span> <span data-ttu-id="0949e-168">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="0949e-168">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_configure.png) 

7. <span data-ttu-id="0949e-170">Konfigurace jednotného přihlašování na **10 000 ft plány** straně, budete muset odeslat stažené **Certificate(Raw), adresa URL Sign-Out, SAML Entity ID a SAML jeden přihlašování adresa URL služby** k [tým podpory plány 10 000 ft](https://www.10000ft.com/plans/support).</span><span class="sxs-lookup"><span data-stu-id="0949e-170">To configure single sign-on on **10,000ft Plans** side, you need to send the downloaded **Certificate(Raw), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [10,000ft Plans support team](https://www.10000ft.com/plans/support).</span></span>

> [!TIP]
> <span data-ttu-id="0949e-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="0949e-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0949e-172">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="0949e-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0949e-173">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0949e-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0949e-174">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0949e-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="0949e-175">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0949e-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="0949e-177">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0949e-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0949e-178">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0949e-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0949e-180">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="0949e-180">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0949e-182">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0949e-182">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0949e-184">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0949e-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0949e-186">a.</span><span class="sxs-lookup"><span data-stu-id="0949e-186">a.</span></span> <span data-ttu-id="0949e-187">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0949e-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0949e-188">b.</span><span class="sxs-lookup"><span data-stu-id="0949e-188">b.</span></span> <span data-ttu-id="0949e-189">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0949e-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0949e-190">c.</span><span class="sxs-lookup"><span data-stu-id="0949e-190">c.</span></span> <span data-ttu-id="0949e-191">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="0949e-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0949e-192">d.</span><span class="sxs-lookup"><span data-stu-id="0949e-192">d.</span></span> <span data-ttu-id="0949e-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0949e-193">Click **Create**.</span></span>
 
### <a name="creating-a-10000ft-plans-test-user"></a><span data-ttu-id="0949e-194">Vytváření 10 000 ft plány testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="0949e-194">Creating a 10,000ft Plans test user</span></span>

<span data-ttu-id="0949e-195">Cílem této části je vytvoření uživatele názvem Britta Simon v plánech úrovně odolnosti proti selhání 10 000.</span><span class="sxs-lookup"><span data-stu-id="0949e-195">The objective of this section is to create a user called Britta Simon in 10,000ft Plans.</span></span> <span data-ttu-id="0949e-196">10 000 ft plány podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="0949e-196">10,000ft Plans supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="0949e-197">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="0949e-197">There is no action item for you in this section.</span></span> <span data-ttu-id="0949e-198">Nový uživatel se vytvoří během pokusu o přístup k 10 000 ft plány, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="0949e-198">A new user is created during an attempt to access 10,000ft Plans if it doesn't exist yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="0949e-199">Pokud potřebujete ručně vytvořit uživatele, budete muset kontaktovat [tým podpory plány 10 000 ft](https://www.10000ft.com/plans/support).</span><span class="sxs-lookup"><span data-stu-id="0949e-199">If you need to create a user manually, you need to contact the [10,000ft Plans support team](https://www.10000ft.com/plans/support).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="0949e-200">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0949e-200">Assigning the Azure AD test user</span></span>

<span data-ttu-id="0949e-201">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu do 10 000 ft plánů.</span><span class="sxs-lookup"><span data-stu-id="0949e-201">In this section, you enable Britta Simon to use Azure single sign-on by granting access to 10,000ft Plans.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="0949e-203">**Přiřadit Britta Simon do 10 000 ft plánů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0949e-203">**To assign Britta Simon to 10,000ft Plans, perform the following steps:**</span></span>

1. <span data-ttu-id="0949e-204">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0949e-204">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="0949e-206">V seznamu aplikací vyberte **10 000 ft plány**.</span><span class="sxs-lookup"><span data-stu-id="0949e-206">In the applications list, select **10,000ft Plans**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_app.png) 

3. <span data-ttu-id="0949e-208">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="0949e-208">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="0949e-210">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0949e-210">Click **Add** button.</span></span> <span data-ttu-id="0949e-211">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0949e-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="0949e-213">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0949e-213">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0949e-214">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0949e-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0949e-215">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0949e-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0949e-216">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0949e-216">Testing single sign-on</span></span>

<span data-ttu-id="0949e-217">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="0949e-217">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="0949e-218">Když kliknete na dlaždici 10 000 ft plány na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci 10 000 ft plány.</span><span class="sxs-lookup"><span data-stu-id="0949e-218">When you click the 10,000ft Plans tile in the Access Panel, you should get automatically signed-on to your 10,000ft Plans application.</span></span>
 
## <a name="additional-resources"></a><span data-ttu-id="0949e-219">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0949e-219">Additional resources</span></span>

* [<span data-ttu-id="0949e-220">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0949e-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0949e-221">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0949e-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_203.png

