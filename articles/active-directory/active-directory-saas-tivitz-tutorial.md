---
title: 'Kurz: Azure Active Directory integrace s TiViTz | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a TiViTz."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b97ed88f-7888-4185-be22-41d1f62cbbf1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: b1b27918bb5fcff1d19f4711ea70fe46a5697933
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tivitz"></a><span data-ttu-id="36c29-103">Kurz: Azure Active Directory integrace s TiViTz</span><span class="sxs-lookup"><span data-stu-id="36c29-103">Tutorial: Azure Active Directory integration with TiViTz</span></span>

<span data-ttu-id="36c29-104">V tomto kurzu zjistěte, jak integrovat TiViTz s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="36c29-104">In this tutorial, you learn how to integrate TiViTz with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="36c29-105">Integrace TiViTz s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="36c29-105">Integrating TiViTz with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="36c29-106">Můžete řídit ve službě Azure AD, který má přístup k TiViTz</span><span class="sxs-lookup"><span data-stu-id="36c29-106">You can control in Azure AD who has access to TiViTz</span></span>
- <span data-ttu-id="36c29-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k TiViTz (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="36c29-107">You can enable your users to automatically get signed-on to TiViTz (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="36c29-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="36c29-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="36c29-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="36c29-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="36c29-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="36c29-110">Prerequisites</span></span>

<span data-ttu-id="36c29-111">Konfigurace integrace Azure AD s TiViTz, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="36c29-111">To configure Azure AD integration with TiViTz, you need the following items:</span></span>

- <span data-ttu-id="36c29-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="36c29-112">An Azure AD subscription</span></span>
- <span data-ttu-id="36c29-113">TiViTz jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="36c29-113">A TiViTz single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="36c29-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="36c29-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="36c29-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="36c29-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="36c29-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="36c29-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="36c29-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="36c29-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="36c29-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="36c29-118">Scenario description</span></span>
<span data-ttu-id="36c29-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="36c29-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="36c29-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="36c29-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="36c29-121">Přidání TiViTz z Galerie</span><span class="sxs-lookup"><span data-stu-id="36c29-121">Adding TiViTz from the gallery</span></span>
2. <span data-ttu-id="36c29-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="36c29-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tivitz-from-the-gallery"></a><span data-ttu-id="36c29-123">Přidání TiViTz z Galerie</span><span class="sxs-lookup"><span data-stu-id="36c29-123">Adding TiViTz from the gallery</span></span>
<span data-ttu-id="36c29-124">Při konfiguraci integrace TiViTz do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS TiViTz z galerie.</span><span class="sxs-lookup"><span data-stu-id="36c29-124">To configure the integration of TiViTz into Azure AD, you need to add TiViTz from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="36c29-125">**Pokud chcete přidat TiViTz z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="36c29-125">**To add TiViTz from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="36c29-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="36c29-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="36c29-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="36c29-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="36c29-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="36c29-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="36c29-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="36c29-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="36c29-133">Do vyhledávacího pole zadejte **TiViTz**.</span><span class="sxs-lookup"><span data-stu-id="36c29-133">In the search box, type **TiViTz**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tivitz-tutorial/tutorial_tivitz_search.png)

5. <span data-ttu-id="36c29-135">Na panelu výsledků vyberte **TiViTz**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="36c29-135">In the results panel, select **TiViTz**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tivitz-tutorial/tutorial_tivitz_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="36c29-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="36c29-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="36c29-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s TiViTz podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="36c29-138">In this section, you configure and test Azure AD single sign-on with TiViTz based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="36c29-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v TiViTz je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="36c29-139">For single sign-on to work, Azure AD needs to know what the counterpart user in TiViTz is to a user in Azure AD.</span></span> <span data-ttu-id="36c29-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v TiViTz musí navázat.</span><span class="sxs-lookup"><span data-stu-id="36c29-140">In other words, a link relationship between an Azure AD user and the related user in TiViTz needs to be established.</span></span>

<span data-ttu-id="36c29-141">V TiViTz, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="36c29-141">In TiViTz, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="36c29-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s TiViTz, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="36c29-142">To configure and test Azure AD single sign-on with TiViTz, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="36c29-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="36c29-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="36c29-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="36c29-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="36c29-145">**[Vytvoření zkušebního uživatele TiViTz](#creating-a-tivitz-test-user)**  – Pokud chcete mít protějšek Britta Simon v TiViTz propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="36c29-145">**[Creating a TiViTz test user](#creating-a-tivitz-test-user)** - to have a counterpart of Britta Simon in TiViTz that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="36c29-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="36c29-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="36c29-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="36c29-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="36c29-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="36c29-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="36c29-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci TiViTz.</span><span class="sxs-lookup"><span data-stu-id="36c29-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TiViTz application.</span></span>

<span data-ttu-id="36c29-150">**Ke konfiguraci Azure AD jednotné přihlašování s TiViTz, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="36c29-150">**To configure Azure AD single sign-on with TiViTz, perform the following steps:**</span></span>

1. <span data-ttu-id="36c29-151">Na portálu Azure na **TiViTz** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="36c29-151">In the Azure portal, on the **TiViTz** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="36c29-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="36c29-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tivitz-tutorial/tutorial_tivitz_samlbase.png)

3. <span data-ttu-id="36c29-155">Na **TiViTz domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="36c29-155">On the **TiViTz Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tivitz-tutorial/tutorial_tivitz_url.png)

    <span data-ttu-id="36c29-157">a.</span><span class="sxs-lookup"><span data-stu-id="36c29-157">a.</span></span> <span data-ttu-id="36c29-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.o365.tivitz.com/`</span><span class="sxs-lookup"><span data-stu-id="36c29-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.o365.tivitz.com/`</span></span>

    <span data-ttu-id="36c29-159">b.</span><span class="sxs-lookup"><span data-stu-id="36c29-159">b.</span></span> <span data-ttu-id="36c29-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.o365.tivitz.com/`</span><span class="sxs-lookup"><span data-stu-id="36c29-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.o365.tivitz.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="36c29-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="36c29-161">These values are not real.</span></span> <span data-ttu-id="36c29-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="36c29-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="36c29-163">Obraťte se na [tým podpory TiViTz klienta](mailto:info@tivitz.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="36c29-163">Contact [TiViTz Client support team](mailto:info@tivitz.com) to get these values.</span></span> 

4. <span data-ttu-id="36c29-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="36c29-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tivitz-tutorial/tutorial_tivitz_certificate.png) 

5. <span data-ttu-id="36c29-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="36c29-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tivitz-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="36c29-168">Konfigurace jednotného přihlašování na **TiViTz** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory TiViTz](mailto:info@tivitz.com).</span><span class="sxs-lookup"><span data-stu-id="36c29-168">To configure single sign-on on **TiViTz** side, you need to send the downloaded **Metadata XML** to [TiViTz support team](mailto:info@tivitz.com).</span></span>

> [!TIP]
> <span data-ttu-id="36c29-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="36c29-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="36c29-170">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="36c29-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="36c29-171">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="36c29-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="36c29-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="36c29-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="36c29-173">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="36c29-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="36c29-175">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="36c29-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="36c29-176">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="36c29-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tivitz-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="36c29-178">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="36c29-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tivitz-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="36c29-180">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="36c29-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tivitz-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="36c29-182">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="36c29-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tivitz-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="36c29-184">a.</span><span class="sxs-lookup"><span data-stu-id="36c29-184">a.</span></span> <span data-ttu-id="36c29-185">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="36c29-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="36c29-186">b.</span><span class="sxs-lookup"><span data-stu-id="36c29-186">b.</span></span> <span data-ttu-id="36c29-187">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="36c29-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="36c29-188">c.</span><span class="sxs-lookup"><span data-stu-id="36c29-188">c.</span></span> <span data-ttu-id="36c29-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="36c29-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="36c29-190">d.</span><span class="sxs-lookup"><span data-stu-id="36c29-190">d.</span></span> <span data-ttu-id="36c29-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="36c29-191">Click **Create**.</span></span>
 
### <a name="creating-a-tivitz-test-user"></a><span data-ttu-id="36c29-192">Vytvoření zkušebního uživatele TiViTz</span><span class="sxs-lookup"><span data-stu-id="36c29-192">Creating a TiViTz test user</span></span>

<span data-ttu-id="36c29-193">Cílem této části je vytvoření uživatele v TiViTz nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="36c29-193">The objective of this section is to create a user called Britta Simon in TiViTz.</span></span> <span data-ttu-id="36c29-194">TiViTz podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="36c29-194">TiViTz supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="36c29-195">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="36c29-195">There is no action item for you in this section.</span></span> <span data-ttu-id="36c29-196">Nový uživatel se vytvoří během pokusu o přístup k TiViTz, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="36c29-196">A new user is created during an attempt to access TiViTz if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="36c29-197">Pokud potřebujete vytvořit uživatele s ručně, budete muset kontaktovat [tým podpory TiViTz](mailto:info@tivitz.com).</span><span class="sxs-lookup"><span data-stu-id="36c29-197">If you need to create an user manually, you need to contact [TiViTz support team](mailto:info@tivitz.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="36c29-198">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="36c29-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="36c29-199">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu TiViTz.</span><span class="sxs-lookup"><span data-stu-id="36c29-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TiViTz.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="36c29-201">**Pokud chcete přiřadit Britta Simon TiViTz, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="36c29-201">**To assign Britta Simon to TiViTz, perform the following steps:**</span></span>

1. <span data-ttu-id="36c29-202">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="36c29-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="36c29-204">V seznamu aplikací vyberte **TiViTz**.</span><span class="sxs-lookup"><span data-stu-id="36c29-204">In the applications list, select **TiViTz**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tivitz-tutorial/tutorial_tivitz_app.png) 

3. <span data-ttu-id="36c29-206">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="36c29-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="36c29-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="36c29-208">Click **Add** button.</span></span> <span data-ttu-id="36c29-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="36c29-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="36c29-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="36c29-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="36c29-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="36c29-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="36c29-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="36c29-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="36c29-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="36c29-214">Testing single sign-on</span></span>

<span data-ttu-id="36c29-215">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="36c29-215">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="36c29-216">Když kliknete na dlaždici TiViTz na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci TiViTz.</span><span class="sxs-lookup"><span data-stu-id="36c29-216">When you click the TiViTz tile in the Access Panel, you should get automatically signed-on to your TiViTz application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="36c29-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="36c29-217">Additional resources</span></span>

* [<span data-ttu-id="36c29-218">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="36c29-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="36c29-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="36c29-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_203.png

