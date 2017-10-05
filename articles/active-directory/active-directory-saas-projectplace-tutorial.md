---
title: 'Kurz: Azure Active Directory integrace s Projectplace | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Projectplace."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 298059ca-b652-4577-916a-c31393d53d7a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: bb9dd10c887cb0e42e544066d9b0dcfa554e10ce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-projectplace"></a><span data-ttu-id="ff4f4-103">Kurz: Azure Active Directory integrace s Projectplace</span><span class="sxs-lookup"><span data-stu-id="ff4f4-103">Tutorial: Azure Active Directory integration with Projectplace</span></span>

<span data-ttu-id="ff4f4-104">V tomto kurzu zjistěte, jak integrovat Projectplace s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ff4f4-104">In this tutorial, you learn how to integrate Projectplace with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ff4f4-105">Integrace Projectplace s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ff4f4-105">Integrating Projectplace with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ff4f4-106">Můžete řídit ve službě Azure AD, který má přístup k Projectplace</span><span class="sxs-lookup"><span data-stu-id="ff4f4-106">You can control in Azure AD who has access to Projectplace</span></span>
- <span data-ttu-id="ff4f4-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Projectplace (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff4f4-107">You can enable your users to automatically get signed-on to Projectplace (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ff4f4-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ff4f4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ff4f4-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ff4f4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff4f4-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ff4f4-110">Prerequisites</span></span>

<span data-ttu-id="ff4f4-111">Konfigurace integrace Azure AD s Projectplace, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="ff4f4-111">To configure Azure AD integration with Projectplace, you need the following items:</span></span>

- <span data-ttu-id="ff4f4-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff4f4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ff4f4-113">Projectplace jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ff4f4-113">A Projectplace single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ff4f4-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ff4f4-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ff4f4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ff4f4-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ff4f4-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ff4f4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ff4f4-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ff4f4-118">Scenario description</span></span>
<span data-ttu-id="ff4f4-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ff4f4-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ff4f4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ff4f4-121">Přidání Projectplace z Galerie</span><span class="sxs-lookup"><span data-stu-id="ff4f4-121">Adding Projectplace from the gallery</span></span>
2. <span data-ttu-id="ff4f4-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ff4f4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-projectplace-from-the-gallery"></a><span data-ttu-id="ff4f4-123">Přidání Projectplace z Galerie</span><span class="sxs-lookup"><span data-stu-id="ff4f4-123">Adding Projectplace from the gallery</span></span>
<span data-ttu-id="ff4f4-124">Při konfiguraci integrace Projectplace do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Projectplace z galerie.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-124">To configure the integration of Projectplace into Azure AD, you need to add Projectplace from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ff4f4-125">**Pokud chcete přidat Projectplace z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ff4f4-125">**To add Projectplace from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ff4f4-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ff4f4-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ff4f4-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="ff4f4-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="ff4f4-133">Do vyhledávacího pole zadejte **Projectplace**.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-133">In the search box, type **Projectplace**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_search.png)

5. <span data-ttu-id="ff4f4-135">Na panelu výsledků vyberte **Projectplace**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-135">In the results panel, select **Projectplace**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ff4f4-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ff4f4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ff4f4-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Projectplace podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="ff4f4-138">In this section, you configure and test Azure AD single sign-on with Projectplace based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ff4f4-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Projectplace je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Projectplace is to a user in Azure AD.</span></span> <span data-ttu-id="ff4f4-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Projectplace musí navázat.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-140">In other words, a link relationship between an Azure AD user and the related user in Projectplace needs to be established.</span></span>

<span data-ttu-id="ff4f4-141">V Projectplace, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-141">In Projectplace, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ff4f4-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Projectplace, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="ff4f4-142">To configure and test Azure AD single sign-on with Projectplace, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ff4f4-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ff4f4-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ff4f4-145">**[Vytvoření zkušebního uživatele Projectplace](#creating-a-projectplace-test-user)**  – Pokud chcete mít protějšek Britta Simon v Projectplace propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-145">**[Creating a Projectplace test user](#creating-a-projectplace-test-user)** - to have a counterpart of Britta Simon in Projectplace that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ff4f4-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ff4f4-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ff4f4-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ff4f4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ff4f4-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v vaše Projectplace aplikace.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Projectplace application.</span></span>

<span data-ttu-id="ff4f4-150">**Ke konfiguraci Azure AD jednotné přihlašování s Projectplace, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ff4f4-150">**To configure Azure AD single sign-on with Projectplace, perform the following steps:**</span></span>

1. <span data-ttu-id="ff4f4-151">Na portálu Azure na **Projectplace** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-151">In the Azure portal, on the **Projectplace** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="ff4f4-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_samlbase.png)

3. <span data-ttu-id="ff4f4-155">Na **Projectplace domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ff4f4-155">On the **Projectplace Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_url.png)

    <span data-ttu-id="ff4f4-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company>.projectplace.com`</span><span class="sxs-lookup"><span data-stu-id="ff4f4-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company>.projectplace.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ff4f4-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-158">This value is not real.</span></span> <span data-ttu-id="ff4f4-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="ff4f4-160">Obraťte se na [tým podpory Projectplace klienta](https://success.planview.com/Projectplace/Support) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-160">Contact [Projectplace Client support team](https://success.planview.com/Projectplace/Support) to get this value.</span></span> 
 
4. <span data-ttu-id="ff4f4-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_certificate.png) 

5. <span data-ttu-id="ff4f4-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-projectplace-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="ff4f4-165">Konfigurace jednotného přihlašování na **Projectplace** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Projectplace](https://success.planview.com/Projectplace/Support).</span><span class="sxs-lookup"><span data-stu-id="ff4f4-165">To configure single sign-on on **Projectplace** side, you need to send the downloaded **Metadata XML** to [Projectplace support team](https://success.planview.com/Projectplace/Support).</span></span> <span data-ttu-id="ff4f4-166">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

>[!NOTE]
><span data-ttu-id="ff4f4-167">Konfigurace přihlášení musí provést [tým podpory Projectplace](https://success.planview.com/Projectplace/Support).</span><span class="sxs-lookup"><span data-stu-id="ff4f4-167">The single sign-on configuration has to be performed by the [Projectplace support team](https://success.planview.com/Projectplace/Support).</span></span> <span data-ttu-id="ff4f4-168">Zobrazí se oznámení a také konfigurace byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-168">You will get a notification as soon as the configuration has been completed.</span></span>

> [!TIP]
> <span data-ttu-id="ff4f4-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="ff4f4-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ff4f4-170">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ff4f4-171">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ff4f4-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ff4f4-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff4f4-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="ff4f4-173">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="ff4f4-175">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ff4f4-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ff4f4-176">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-projectplace-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ff4f4-178">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-projectplace-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ff4f4-180">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-projectplace-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ff4f4-182">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ff4f4-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-projectplace-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ff4f4-184">a.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-184">a.</span></span> <span data-ttu-id="ff4f4-185">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ff4f4-186">b.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-186">b.</span></span> <span data-ttu-id="ff4f4-187">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ff4f4-188">c.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-188">c.</span></span> <span data-ttu-id="ff4f4-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ff4f4-190">d.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-190">d.</span></span> <span data-ttu-id="ff4f4-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-191">Click **Create**.</span></span>
 
### <a name="creating-a-projectplace-test-user"></a><span data-ttu-id="ff4f4-192">Vytvoření zkušebního uživatele Projectplace</span><span class="sxs-lookup"><span data-stu-id="ff4f4-192">Creating a Projectplace test user</span></span>

<span data-ttu-id="ff4f4-193">Pokud chcete povolit uživatelům Azure AD přihlášení do Projectplace, musí být zřízená do Projectplace.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-193">In order to enable Azure AD users to log into Projectplace, they must be provisioned into Projectplace.</span></span> <span data-ttu-id="ff4f4-194">V případě Projectplace zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-194">In the case of Projectplace, provisioning is a manual task.</span></span>

<span data-ttu-id="ff4f4-195">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ff4f4-195">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="ff4f4-196">Přihlaste se k vaší **Projectplace** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-196">Log in to your **Projectplace** company site as an administrator.</span></span>

2. <span data-ttu-id="ff4f4-197">Přejděte na **osoby**a potom klikněte na **členy**.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-197">Go to **People**, and then click **Members**.</span></span>
   
    <span data-ttu-id="ff4f4-198">![Lidé](./media/active-directory-saas-projectplace-tutorial/ic790228.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="ff4f4-198">![People](./media/active-directory-saas-projectplace-tutorial/ic790228.png "People")</span></span>

3. <span data-ttu-id="ff4f4-199">Klikněte na tlačítko **přidat člena**.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-199">Click **Add Member**.</span></span>
   
    <span data-ttu-id="ff4f4-200">![Přidání členů](./media/active-directory-saas-projectplace-tutorial/ic790232.png "přidat členy")</span><span class="sxs-lookup"><span data-stu-id="ff4f4-200">![Add Members](./media/active-directory-saas-projectplace-tutorial/ic790232.png "Add Members")</span></span>

4. <span data-ttu-id="ff4f4-201">V **přidat člena** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ff4f4-201">In the **Add Member** section, perform the following steps:</span></span>
   
    <span data-ttu-id="ff4f4-202">![Nové členy](./media/active-directory-saas-projectplace-tutorial/ic790233.png "nové členy")</span><span class="sxs-lookup"><span data-stu-id="ff4f4-202">![New Members](./media/active-directory-saas-projectplace-tutorial/ic790233.png "New Members")</span></span>
   
    <span data-ttu-id="ff4f4-203">a.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-203">a.</span></span> <span data-ttu-id="ff4f4-204">V **nové členy** textovému poli, zadejte e-mailovou adresu platného účtu AAD chcete mají být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-204">In the **New Members** textbox, type the email address of a valid AAD account you want to provision into the related textboxes.</span></span>
   
    <span data-ttu-id="ff4f4-205">b.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-205">b.</span></span> <span data-ttu-id="ff4f4-206">Klikněte na tlačítko **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-206">Click **Send**.</span></span>

   <span data-ttu-id="ff4f4-207">Zahrnutím odkazu pro potvrzení účtu před stane aktivní e-mail je odeslán držitel účtu Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-207">An email including a link to confirm the account before it becomes active is sent to the Azure Active Directory account holder.</span></span>

>[!NOTE]
><span data-ttu-id="ff4f4-208">Můžete použít všechny ostatní Projectplace uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Projectplace zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-208">You can use any other Projectplace user account creation tools or APIs provided by Projectplace to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ff4f4-209">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff4f4-209">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ff4f4-210">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Projectplace.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-210">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Projectplace.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ff4f4-212">**Pokud chcete přiřadit Britta Simon Projectplace, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ff4f4-212">**To assign Britta Simon to Projectplace, perform the following steps:**</span></span>

1. <span data-ttu-id="ff4f4-213">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-213">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ff4f4-215">V seznamu aplikací vyberte **Projectplace**.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-215">In the applications list, select **Projectplace**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_app.png) 

3. <span data-ttu-id="ff4f4-217">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-217">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ff4f4-219">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-219">Click **Add** button.</span></span> <span data-ttu-id="ff4f4-220">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-220">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ff4f4-222">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-222">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ff4f4-223">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-223">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ff4f4-224">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-224">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ff4f4-225">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ff4f4-225">Testing single sign-on</span></span>

<span data-ttu-id="ff4f4-226">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-226">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="ff4f4-227">Když kliknete na dlaždici Projectplace na přístupovém panelu, jste měli získat automaticky přihlášení k vaše Projectplace aplikace.</span><span class="sxs-lookup"><span data-stu-id="ff4f4-227">When you click the Projectplace tile in the Access Panel, you should get automatically signed-on to your Projectplace application.</span></span>
<span data-ttu-id="ff4f4-228">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ff4f4-228">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff4f4-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ff4f4-229">Additional resources</span></span>

* [<span data-ttu-id="ff4f4-230">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ff4f4-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ff4f4-231">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ff4f4-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_203.png

