---
title: 'Kurz: Azure Active Directory integrace s 360 Online | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a 360 Online."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cda8eba6-843f-4a09-8c55-0aaf6e593d75
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 629c7db04b0f9c880da6dfa8eac7fe14ecd8a215
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-360-online"></a><span data-ttu-id="b2e5b-103">Kurz: Azure Active Directory integrace s 360 Online</span><span class="sxs-lookup"><span data-stu-id="b2e5b-103">Tutorial: Azure Active Directory integration with 360 Online</span></span>

<span data-ttu-id="b2e5b-104">V tomto kurzu zjistěte, jak integrovat 360 Online s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b2e5b-104">In this tutorial, you learn how to integrate 360 Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b2e5b-105">Integrace 360 Online s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b2e5b-105">Integrating 360 Online with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b2e5b-106">Můžete řídit ve službě Azure AD, který má přístup k 360 Online</span><span class="sxs-lookup"><span data-stu-id="b2e5b-106">You can control in Azure AD who has access to 360 Online</span></span>
- <span data-ttu-id="b2e5b-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k 360 Online (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b2e5b-107">You can enable your users to automatically get signed-on to 360 Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b2e5b-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b2e5b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b2e5b-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b2e5b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b2e5b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b2e5b-110">Prerequisites</span></span>

<span data-ttu-id="b2e5b-111">Konfigurace integrace Azure AD s 360 Online, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="b2e5b-111">To configure Azure AD integration with 360 Online, you need the following items:</span></span>

- <span data-ttu-id="b2e5b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b2e5b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b2e5b-113">Předplatné povolené 360 Online jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b2e5b-113">A 360 Online single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b2e5b-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b2e5b-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b2e5b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b2e5b-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b2e5b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b2e5b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b2e5b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b2e5b-118">Scenario description</span></span>
<span data-ttu-id="b2e5b-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b2e5b-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b2e5b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b2e5b-121">Přidání 360 Online z Galerie</span><span class="sxs-lookup"><span data-stu-id="b2e5b-121">Adding 360 Online from the gallery</span></span>
2. <span data-ttu-id="b2e5b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b2e5b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-360-online-from-the-gallery"></a><span data-ttu-id="b2e5b-123">Přidání 360 Online z Galerie</span><span class="sxs-lookup"><span data-stu-id="b2e5b-123">Adding 360 Online from the gallery</span></span>
<span data-ttu-id="b2e5b-124">Při konfiguraci integrace 360 Online do služby Azure AD potřebujete přidat 360 Online z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-124">To configure the integration of 360 Online into Azure AD, you need to add 360 Online from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b2e5b-125">**Pokud chcete přidat 360 Online z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b2e5b-125">**To add 360 Online from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b2e5b-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b2e5b-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b2e5b-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b2e5b-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b2e5b-133">Do vyhledávacího pole zadejte **360 Online**.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-133">In the search box, type **360 Online**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-360online-tutorial/tutorial_360online_search.png)

5. <span data-ttu-id="b2e5b-135">Na panelu výsledků vyberte **360 Online**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-135">In the results panel, select **360 Online**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-360online-tutorial/tutorial_360online_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b2e5b-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b2e5b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b2e5b-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s 360 Online na základě testovací uživatele, nazývá "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="b2e5b-138">In this section, you configure and test Azure AD single sign-on with 360 Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b2e5b-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v 360 Online je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in 360 Online is to a user in Azure AD.</span></span> <span data-ttu-id="b2e5b-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v 360 Online musí být vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-140">In other words, a link relationship between an Azure AD user and the related user in 360 Online needs to be established.</span></span>

<span data-ttu-id="b2e5b-141">V 360 Online přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-141">In 360 Online, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="b2e5b-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s 360 Online, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="b2e5b-142">To configure and test Azure AD single sign-on with 360 Online, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b2e5b-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b2e5b-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b2e5b-145">**[Vytváření 360 Online testovacího uživatele](#creating-a-360-online-test-user)**  – Pokud chcete mít protějšek Britta Simon v 360 Online, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-145">**[Creating a 360 Online test user](#creating-a-360-online-test-user)** - to have a counterpart of Britta Simon in 360 Online that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b2e5b-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b2e5b-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b2e5b-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b2e5b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b2e5b-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci 360 Online.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your 360 Online application.</span></span>

<span data-ttu-id="b2e5b-150">**Ke konfiguraci Azure AD jednotné přihlašování s 360 Online, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b2e5b-150">**To configure Azure AD single sign-on with 360 Online, perform the following steps:**</span></span>

1. <span data-ttu-id="b2e5b-151">Na portálu Azure na **360 Online** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-151">In the Azure portal, on the **360 Online** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b2e5b-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-360online-tutorial/tutorial_360online_samlbase.png)

3. <span data-ttu-id="b2e5b-155">Na **360 Online domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b2e5b-155">On the **360 Online Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-360online-tutorial/tutorial_360online_url.png)

    <span data-ttu-id="b2e5b-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.public360online.com`</span><span class="sxs-lookup"><span data-stu-id="b2e5b-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.public360online.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b2e5b-158">Hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-158">The value is not real.</span></span> <span data-ttu-id="b2e5b-159">Aktualizujte hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="b2e5b-160">Obraťte se na [360 tým podpory Online klienta](mailto:360online@software-innovation.com) k získání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-160">Contact [360 Online Client support team](mailto:360online@software-innovation.com) to get the value.</span></span> 
 
4. <span data-ttu-id="b2e5b-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-360online-tutorial/tutorial_360online_certificate.png) 

5. <span data-ttu-id="b2e5b-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-360online-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b2e5b-165">Konfigurace jednotného přihlašování na **360 Online** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory 360 Online](mailto:360online@software-innovation.com).</span><span class="sxs-lookup"><span data-stu-id="b2e5b-165">To configure single sign-on on **360 Online** side, you need to send the downloaded **Metadata XML** to [360 Online support team](mailto:360online@software-innovation.com).</span></span> 

> [!TIP]
> <span data-ttu-id="b2e5b-166">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="b2e5b-166">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b2e5b-167">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-167">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b2e5b-168">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b2e5b-168">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b2e5b-169">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b2e5b-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="b2e5b-170">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-170">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b2e5b-172">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b2e5b-172">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b2e5b-173">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-173">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b2e5b-175">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-175">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b2e5b-177">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-177">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b2e5b-179">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b2e5b-179">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b2e5b-181">a.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-181">a.</span></span> <span data-ttu-id="b2e5b-182">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-182">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b2e5b-183">b.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-183">b.</span></span> <span data-ttu-id="b2e5b-184">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-184">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b2e5b-185">c.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-185">c.</span></span> <span data-ttu-id="b2e5b-186">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-186">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b2e5b-187">d.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-187">d.</span></span> <span data-ttu-id="b2e5b-188">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-188">Click **Create**.</span></span>
 
### <a name="creating-a-360-online-test-user"></a><span data-ttu-id="b2e5b-189">Vytváření 360 Online zkušebního uživatele</span><span class="sxs-lookup"><span data-stu-id="b2e5b-189">Creating a 360 Online test user</span></span>

<span data-ttu-id="b2e5b-190">V této části vytvoříte uživatele volal Britta Simon v 360 Online.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-190">In this section, you create a user called Britta Simon in 360 Online.</span></span> <span data-ttu-id="b2e5b-191">budete muset kontaktovat [tým podpory 360 Online](mailto:360online@software-innovation.com).</span><span class="sxs-lookup"><span data-stu-id="b2e5b-191">you need to contact [360 Online support team](mailto:360online@software-innovation.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b2e5b-192">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b2e5b-192">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b2e5b-193">V této části povolíte Britta Simon tak, že udělíte přístup k 360 Online používat Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-193">In this section, you enable Britta Simon to use Azure single sign-on by granting access to 360 Online.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b2e5b-195">**Pokud chcete přiřadit Britta Simon 360 Online, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b2e5b-195">**To assign Britta Simon to 360 Online, perform the following steps:**</span></span>

1. <span data-ttu-id="b2e5b-196">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-196">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b2e5b-198">V seznamu aplikací vyberte **360 Online**.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-198">In the applications list, select **360 Online**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-360online-tutorial/tutorial_360online_app.png) 

3. <span data-ttu-id="b2e5b-200">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-200">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b2e5b-202">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-202">Click **Add** button.</span></span> <span data-ttu-id="b2e5b-203">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-203">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b2e5b-205">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-205">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b2e5b-206">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-206">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b2e5b-207">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-207">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b2e5b-208">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b2e5b-208">Testing single sign-on</span></span>

<span data-ttu-id="b2e5b-209">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-209">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b2e5b-210">Když kliknete na dlaždici 360 Online na přístupovém panelu jste měli získat automaticky přihlášení k aplikaci 360 Online.</span><span class="sxs-lookup"><span data-stu-id="b2e5b-210">When you click the 360 Online tile in the Access Panel, you should get automatically signed-on to your 360 Online application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="b2e5b-211">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b2e5b-211">Additional resources</span></span>

* [<span data-ttu-id="b2e5b-212">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b2e5b-212">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b2e5b-213">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b2e5b-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-360online-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-360online-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-360online-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-360online-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-360online-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-360online-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-360online-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-360online-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-360online-tutorial/tutorial_general_203.png
