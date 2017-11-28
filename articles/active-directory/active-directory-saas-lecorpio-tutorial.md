---
title: 'Kurz: Azure Active Directory integrace s Lecorpio | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Lecorpio."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.openlocfilehash: 35c94e2d9d8a938971f85ea732a74a7e1655545e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lecorpio"></a><span data-ttu-id="31a8a-103">Kurz: Azure Active Directory integrace s Lecorpio</span><span class="sxs-lookup"><span data-stu-id="31a8a-103">Tutorial: Azure Active Directory integration with Lecorpio</span></span>

<span data-ttu-id="31a8a-104">V tomto kurzu zjistěte, jak integrovat Lecorpio s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="31a8a-104">In this tutorial, you learn how to integrate Lecorpio with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="31a8a-105">Integrace Lecorpio s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="31a8a-105">Integrating Lecorpio with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="31a8a-106">Můžete řídit ve službě Azure AD, který má přístup k Lecorpio</span><span class="sxs-lookup"><span data-stu-id="31a8a-106">You can control in Azure AD who has access to Lecorpio</span></span>
- <span data-ttu-id="31a8a-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Lecorpio (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="31a8a-107">You can enable your users to automatically get signed-on to Lecorpio (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="31a8a-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="31a8a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="31a8a-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="31a8a-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="31a8a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="31a8a-110">Prerequisites</span></span>

<span data-ttu-id="31a8a-111">Konfigurace integrace Azure AD s Lecorpio, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="31a8a-111">To configure Azure AD integration with Lecorpio, you need the following items:</span></span>

- <span data-ttu-id="31a8a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="31a8a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="31a8a-113">Lecorpio jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="31a8a-113">A Lecorpio single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="31a8a-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="31a8a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="31a8a-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="31a8a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="31a8a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="31a8a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="31a8a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="31a8a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="31a8a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="31a8a-118">Scenario description</span></span>
<span data-ttu-id="31a8a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="31a8a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="31a8a-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="31a8a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="31a8a-121">Přidání Lecorpio z Galerie</span><span class="sxs-lookup"><span data-stu-id="31a8a-121">Adding Lecorpio from the gallery</span></span>
2. <span data-ttu-id="31a8a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="31a8a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lecorpio-from-the-gallery"></a><span data-ttu-id="31a8a-123">Přidání Lecorpio z Galerie</span><span class="sxs-lookup"><span data-stu-id="31a8a-123">Adding Lecorpio from the gallery</span></span>
<span data-ttu-id="31a8a-124">Při konfiguraci integrace Lecorpio do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Lecorpio z galerie.</span><span class="sxs-lookup"><span data-stu-id="31a8a-124">To configure the integration of Lecorpio into Azure AD, you need to add Lecorpio from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="31a8a-125">**Pokud chcete přidat Lecorpio z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="31a8a-125">**To add Lecorpio from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="31a8a-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="31a8a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="31a8a-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="31a8a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="31a8a-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="31a8a-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="31a8a-131">Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="31a8a-131">Click **New application** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="31a8a-133">Do vyhledávacího pole zadejte **Lecorpio**.</span><span class="sxs-lookup"><span data-stu-id="31a8a-133">In the search box, type **Lecorpio**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_search.png)

5. <span data-ttu-id="31a8a-135">Na panelu výsledků vyberte **Lecorpio**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="31a8a-135">In the results panel, select **Lecorpio**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="31a8a-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="31a8a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="31a8a-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Lecorpio podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="31a8a-138">In this section, you configure and test Azure AD single sign-on with Lecorpio based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="31a8a-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Lecorpio je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="31a8a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Lecorpio is to a user in Azure AD.</span></span> <span data-ttu-id="31a8a-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Lecorpio musí navázat.</span><span class="sxs-lookup"><span data-stu-id="31a8a-140">In other words, a link relationship between an Azure AD user and the related user in Lecorpio needs to be established.</span></span>

<span data-ttu-id="31a8a-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Lecorpio.</span><span class="sxs-lookup"><span data-stu-id="31a8a-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Lecorpio.</span></span>

<span data-ttu-id="31a8a-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Lecorpio, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="31a8a-142">To configure and test Azure AD single sign-on with Lecorpio, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="31a8a-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="31a8a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="31a8a-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="31a8a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="31a8a-145">**[Vytvoření zkušebního uživatele Lecorpio](#creating-a-lecorpio-test-user)**  – Pokud chcete mít protějšek Britta Simon v Lecorpio propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="31a8a-145">**[Creating a Lecorpio test user](#creating-a-lecorpio-test-user)** - to have a counterpart of Britta Simon in Lecorpio that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="31a8a-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="31a8a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="31a8a-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="31a8a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="31a8a-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="31a8a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="31a8a-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Lecorpio.</span><span class="sxs-lookup"><span data-stu-id="31a8a-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Lecorpio application.</span></span>

<span data-ttu-id="31a8a-150">**Ke konfiguraci Azure AD jednotné přihlašování s Lecorpio, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="31a8a-150">**To configure Azure AD single sign-on with Lecorpio, perform the following steps:**</span></span>

1. <span data-ttu-id="31a8a-151">Na portálu Azure na **Lecorpio** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="31a8a-151">In the Azure portal, on the **Lecorpio** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="31a8a-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="31a8a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_samlbase.png)

3. <span data-ttu-id="31a8a-155">Na **Lecorpio domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="31a8a-155">On the **Lecorpio Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_url.png)

    <span data-ttu-id="31a8a-157">a.</span><span class="sxs-lookup"><span data-stu-id="31a8a-157">a.</span></span> <span data-ttu-id="31a8a-158">V **přihlašovací adresa URL** textovému poli, zadejte hodnotu pomocí následujícího vzorce:`https://<instance name>.lecorpio.com/<customer name>`</span><span class="sxs-lookup"><span data-stu-id="31a8a-158">In the **Sign-on URL** textbox, type the value using the following pattern: `https://<instance name>.lecorpio.com/<customer name>`</span></span>

    <span data-ttu-id="31a8a-159">b.</span><span class="sxs-lookup"><span data-stu-id="31a8a-159">b.</span></span> <span data-ttu-id="31a8a-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<instance name>.lecorpio.com/<customer name>`</span><span class="sxs-lookup"><span data-stu-id="31a8a-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<instance name>.lecorpio.com/<customer name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="31a8a-161">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="31a8a-161">These values are not the real.</span></span> <span data-ttu-id="31a8a-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="31a8a-162">Update these values with the actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="31a8a-163">Zde, doporučujeme vám použít jedinečnou hodnotu řetězce v identifikátoru.</span><span class="sxs-lookup"><span data-stu-id="31a8a-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="31a8a-164">Obraťte se na [tým podpory Lecorpio klienta](mailto:info@lecorpio.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="31a8a-164">Contact [Lecorpio Client support team](mailto:info@lecorpio.com) to get these values.</span></span> 
 
4. <span data-ttu-id="31a8a-165">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="31a8a-165">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_certificate.png) 

5. <span data-ttu-id="31a8a-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="31a8a-167">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lecorpio-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="31a8a-169">Konfigurace jednotného přihlašování na **Lecorpio** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Lecorpio](mailto:info@lecorpio.com).</span><span class="sxs-lookup"><span data-stu-id="31a8a-169">To configure single sign-on on **Lecorpio** side, you need to send the downloaded **Metadata XML** to [Lecorpio support team](mailto:info@lecorpio.com).</span></span>

> [!TIP]
> <span data-ttu-id="31a8a-170">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="31a8a-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="31a8a-171">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="31a8a-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="31a8a-172">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="31a8a-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="31a8a-173">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="31a8a-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="31a8a-174">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="31a8a-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="31a8a-176">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="31a8a-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="31a8a-177">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="31a8a-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="31a8a-179">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="31a8a-179">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="31a8a-181">V horní části okna klikněte na položku **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="31a8a-181">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="31a8a-183">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="31a8a-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="31a8a-185">a.</span><span class="sxs-lookup"><span data-stu-id="31a8a-185">a.</span></span> <span data-ttu-id="31a8a-186">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="31a8a-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="31a8a-187">b.</span><span class="sxs-lookup"><span data-stu-id="31a8a-187">b.</span></span> <span data-ttu-id="31a8a-188">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="31a8a-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="31a8a-189">c.</span><span class="sxs-lookup"><span data-stu-id="31a8a-189">c.</span></span> <span data-ttu-id="31a8a-190">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="31a8a-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="31a8a-191">d.</span><span class="sxs-lookup"><span data-stu-id="31a8a-191">d.</span></span> <span data-ttu-id="31a8a-192">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="31a8a-192">Click **Create**.</span></span>
 
### <a name="creating-a-lecorpio-test-user"></a><span data-ttu-id="31a8a-193">Vytvoření zkušebního uživatele Lecorpio</span><span class="sxs-lookup"><span data-stu-id="31a8a-193">Creating a Lecorpio test user</span></span>

<span data-ttu-id="31a8a-194">V této části vytvoříte volal Britta Simon v Lecorpio uživatele.</span><span class="sxs-lookup"><span data-stu-id="31a8a-194">In this section, you create a user called Britta Simon in Lecorpio.</span></span> 

<span data-ttu-id="31a8a-195">Obraťte se na [tým podpory Lecorpio klienta](mailto:info@lecorpio.com) přidejte uživatele v aplikaci Lecorpio.</span><span class="sxs-lookup"><span data-stu-id="31a8a-195">Contact [Lecorpio Client support team](mailto:info@lecorpio.com) to add the users in the Lecorpio application.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="31a8a-196">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="31a8a-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="31a8a-197">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Lecorpio.</span><span class="sxs-lookup"><span data-stu-id="31a8a-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Lecorpio.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="31a8a-199">**Pokud chcete přiřadit Britta Simon Lecorpio, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="31a8a-199">**To assign Britta Simon to Lecorpio, perform the following steps:**</span></span>

1. <span data-ttu-id="31a8a-200">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="31a8a-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="31a8a-202">V seznamu aplikací vyberte **Lecorpio**.</span><span class="sxs-lookup"><span data-stu-id="31a8a-202">In the applications list, select **Lecorpio**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_app.png) 

3. <span data-ttu-id="31a8a-204">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="31a8a-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="31a8a-206">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="31a8a-206">Click **Add** button.</span></span> <span data-ttu-id="31a8a-207">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="31a8a-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="31a8a-209">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="31a8a-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="31a8a-210">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="31a8a-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="31a8a-211">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="31a8a-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="31a8a-212">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="31a8a-212">Testing single sign-on</span></span>

<span data-ttu-id="31a8a-213">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="31a8a-213">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="31a8a-214">Když kliknete na dlaždici Lecorpio na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Lecorpio.</span><span class="sxs-lookup"><span data-stu-id="31a8a-214">When you click the Lecorpio tile in the Access Panel, you should get automatically signed-on to your Lecorpio application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="31a8a-215">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="31a8a-215">Additional resources</span></span>

* [<span data-ttu-id="31a8a-216">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="31a8a-216">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="31a8a-217">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="31a8a-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_203.png

