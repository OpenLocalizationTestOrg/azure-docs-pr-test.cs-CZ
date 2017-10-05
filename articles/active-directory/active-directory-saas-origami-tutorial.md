---
title: 'Kurz: Azure Active Directory integrace s Origami | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Origami."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a28bb0ba-b564-46ba-accc-e587699295d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 3420409b72ff032e64ac59365083dd141dfc3c1b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-origami"></a><span data-ttu-id="3f021-103">Kurz: Azure Active Directory integrace s Origami</span><span class="sxs-lookup"><span data-stu-id="3f021-103">Tutorial: Azure Active Directory integration with Origami</span></span>

<span data-ttu-id="3f021-104">V tomto kurzu zjistěte, jak integrovat Origami s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3f021-104">In this tutorial, you learn how to integrate Origami with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3f021-105">Integrace Origami s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3f021-105">Integrating Origami with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3f021-106">Můžete řídit ve službě Azure AD, který má přístup k Origami</span><span class="sxs-lookup"><span data-stu-id="3f021-106">You can control in Azure AD who has access to Origami</span></span>
- <span data-ttu-id="3f021-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Origami (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3f021-107">You can enable your users to automatically get signed-on to Origami (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3f021-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3f021-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3f021-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3f021-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f021-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3f021-110">Prerequisites</span></span>

<span data-ttu-id="3f021-111">Konfigurace integrace Azure AD s Origami, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="3f021-111">To configure Azure AD integration with Origami, you need the following items:</span></span>

- <span data-ttu-id="3f021-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3f021-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3f021-113">Origami jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3f021-113">An Origami single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3f021-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3f021-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3f021-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3f021-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3f021-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3f021-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3f021-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3f021-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3f021-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3f021-118">Scenario description</span></span>
<span data-ttu-id="3f021-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3f021-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3f021-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3f021-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3f021-121">Přidání Origami z Galerie</span><span class="sxs-lookup"><span data-stu-id="3f021-121">Adding Origami from the gallery</span></span>
2. <span data-ttu-id="3f021-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3f021-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-origami-from-the-gallery"></a><span data-ttu-id="3f021-123">Přidání Origami z Galerie</span><span class="sxs-lookup"><span data-stu-id="3f021-123">Adding Origami from the gallery</span></span>
<span data-ttu-id="3f021-124">Při konfiguraci integrace Origami do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Origami z galerie.</span><span class="sxs-lookup"><span data-stu-id="3f021-124">To configure the integration of Origami into Azure AD, you need to add Origami from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3f021-125">**Pokud chcete přidat Origami z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3f021-125">**To add Origami from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3f021-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3f021-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3f021-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3f021-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3f021-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3f021-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="3f021-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3f021-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="3f021-133">Do vyhledávacího pole zadejte **Origami**.</span><span class="sxs-lookup"><span data-stu-id="3f021-133">In the search box, type **Origami**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-origami-tutorial/tutorial_origami_search.png)

5. <span data-ttu-id="3f021-135">Na panelu výsledků vyberte **Origami**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3f021-135">In the results panel, select **Origami**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-origami-tutorial/tutorial_origami_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3f021-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3f021-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3f021-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Origami podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="3f021-138">In this section, you configure and test Azure AD single sign-on with Origami based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3f021-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Origami je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3f021-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Origami is to a user in Azure AD.</span></span> <span data-ttu-id="3f021-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Origami musí navázat.</span><span class="sxs-lookup"><span data-stu-id="3f021-140">In other words, a link relationship between an Azure AD user and the related user in Origami needs to be established.</span></span>

<span data-ttu-id="3f021-141">V Origami, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="3f021-141">In Origami, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3f021-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Origami, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="3f021-142">To configure and test Azure AD single sign-on with Origami, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3f021-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="3f021-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3f021-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3f021-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3f021-145">**[Vytváření testovacího uživatele Origami](#creating-an-origami-test-user)**  – Pokud chcete mít protějšek Britta Simon v Origami propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="3f021-145">**[Creating an Origami test user](#creating-an-origami-test-user)** - to have a counterpart of Britta Simon in Origami that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3f021-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3f021-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3f021-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3f021-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3f021-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3f021-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3f021-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Origami.</span><span class="sxs-lookup"><span data-stu-id="3f021-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Origami application.</span></span>

<span data-ttu-id="3f021-150">**Ke konfiguraci Azure AD jednotné přihlašování s Origami, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3f021-150">**To configure Azure AD single sign-on with Origami, perform the following steps:**</span></span>

1. <span data-ttu-id="3f021-151">Na portálu Azure na **Origami** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3f021-151">In the Azure portal, on the **Origami** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="3f021-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3f021-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_samlbase.png)

3. <span data-ttu-id="3f021-155">Na **Origami domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3f021-155">On the **Origami Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_url.png)

    <span data-ttu-id="3f021-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://live.origamirisk.com/origami/account/login?account=<companyname>`</span><span class="sxs-lookup"><span data-stu-id="3f021-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://live.origamirisk.com/origami/account/login?account=<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3f021-158">Hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="3f021-158">The value is not real.</span></span> <span data-ttu-id="3f021-159">Aktualizujte hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3f021-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="3f021-160">Obraťte se na [tým podpory Origami klienta](https://wordpress.org/support/theme/origami) k získání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3f021-160">Contact [Origami Client support team](https://wordpress.org/support/theme/origami) to get the value.</span></span> 
 
4. <span data-ttu-id="3f021-161">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="3f021-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_certificate.png) 

5. <span data-ttu-id="3f021-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3f021-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3f021-165">Na **Origami konfigurace** klikněte na tlačítko **konfigurace Origami** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="3f021-165">On the **Origami Configuration** section, click **Configure Origami** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3f021-166">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="3f021-166">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_configure.png) 

7. <span data-ttu-id="3f021-168">Přihlaste se k účtu Origami pomocí oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="3f021-168">Log in to the Origami account with Admin rights.</span></span>

8. <span data-ttu-id="3f021-169">V nabídce v horní části, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="3f021-169">In the menu on the top, click **Admin**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

9. <span data-ttu-id="3f021-171">V jednom přihlášení na stránce instalačního programu dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3f021-171">On the Single Sign On Setup dialog page, perform the following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_531.png)

    <span data-ttu-id="3f021-173">a.</span><span class="sxs-lookup"><span data-stu-id="3f021-173">a.</span></span> <span data-ttu-id="3f021-174">Vyberte **povolení jednotného přihlašování na**.</span><span class="sxs-lookup"><span data-stu-id="3f021-174">Select **Enable Single Sign On**.</span></span>

    <span data-ttu-id="3f021-175">b.</span><span class="sxs-lookup"><span data-stu-id="3f021-175">b.</span></span> <span data-ttu-id="3f021-176">V **zprostředkovatele Identity adresu URL přihlašovací stránky** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3f021-176">In the **Identity Provider's Sign-in Page URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="3f021-177">c.</span><span class="sxs-lookup"><span data-stu-id="3f021-177">c.</span></span> <span data-ttu-id="3f021-178">V **adresa URL stránky Sign-out zprostředkovatele Identity** textovému poli, vložte hodnotu **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3f021-178">In the **Identity Provider's Sign-out Page URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="3f021-179">d.</span><span class="sxs-lookup"><span data-stu-id="3f021-179">d.</span></span> <span data-ttu-id="3f021-180">Klikněte na tlačítko **Procházet** na kterou odešlete certifikát, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3f021-180">Click **Browse** to upload the certificate you have downloaded from the Azure portal.</span></span>

    <span data-ttu-id="3f021-181">e.</span><span class="sxs-lookup"><span data-stu-id="3f021-181">e.</span></span> <span data-ttu-id="3f021-182">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="3f021-182">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="3f021-183">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="3f021-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3f021-184">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="3f021-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3f021-185">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3f021-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3f021-186">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3f021-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="3f021-187">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3f021-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="3f021-189">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3f021-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3f021-190">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3f021-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-origami-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3f021-192">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3f021-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-origami-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3f021-194">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3f021-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-origami-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3f021-196">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3f021-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-origami-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3f021-198">a.</span><span class="sxs-lookup"><span data-stu-id="3f021-198">a.</span></span> <span data-ttu-id="3f021-199">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3f021-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3f021-200">b.</span><span class="sxs-lookup"><span data-stu-id="3f021-200">b.</span></span> <span data-ttu-id="3f021-201">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3f021-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3f021-202">c.</span><span class="sxs-lookup"><span data-stu-id="3f021-202">c.</span></span> <span data-ttu-id="3f021-203">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="3f021-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3f021-204">d.</span><span class="sxs-lookup"><span data-stu-id="3f021-204">d.</span></span> <span data-ttu-id="3f021-205">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3f021-205">Click **Create**.</span></span>
 
### <a name="creating-an-origami-test-user"></a><span data-ttu-id="3f021-206">Vytváření testovacího uživatele Origami</span><span class="sxs-lookup"><span data-stu-id="3f021-206">Creating an Origami test user</span></span>

<span data-ttu-id="3f021-207">V této části vytvoříte volal Britta Simon v Origami uživatele.</span><span class="sxs-lookup"><span data-stu-id="3f021-207">In this section, you create a user called Britta Simon in Origami.</span></span> 

1. <span data-ttu-id="3f021-208">Přihlaste se k účtu Origami pomocí oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="3f021-208">Log in to the Origami account with Admin rights.</span></span>

2. <span data-ttu-id="3f021-209">V nabídce v horní části, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="3f021-209">In the menu on the top, click **Admin**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

3. <span data-ttu-id="3f021-211">Na **uživatelů a zabezpečení** dialogové okno, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3f021-211">On the **Users and Security** dialog, click **Users**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_54.png)

4. <span data-ttu-id="3f021-213">Klikněte na tlačítko **přidat nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="3f021-213">Click **Add New User**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_55.png)

5. <span data-ttu-id="3f021-215">V dialogovém okně Přidat nové uživatele proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3f021-215">On the Add New User dialog, perform the following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_56.png)

    <span data-ttu-id="3f021-217">a.</span><span class="sxs-lookup"><span data-stu-id="3f021-217">a.</span></span> <span data-ttu-id="3f021-218">V **uživatelské jméno** textovému poli, zadejte e-mailu uživatele jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="3f021-218">In the **User Name** textbox, enter the email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="3f021-219">b.</span><span class="sxs-lookup"><span data-stu-id="3f021-219">b.</span></span> <span data-ttu-id="3f021-220">V **heslo** textovému poli, zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="3f021-220">In the **Password** textbox, type a password.</span></span>

    <span data-ttu-id="3f021-221">c.</span><span class="sxs-lookup"><span data-stu-id="3f021-221">c.</span></span> <span data-ttu-id="3f021-222">V **Potvrdit heslo** textovému poli, zadejte heslo znovu.</span><span class="sxs-lookup"><span data-stu-id="3f021-222">In the **Confirm Password** textbox, type the password again.</span></span>

    <span data-ttu-id="3f021-223">d.</span><span class="sxs-lookup"><span data-stu-id="3f021-223">d.</span></span> <span data-ttu-id="3f021-224">V **křestní jméno** textovému poli, zadejte jméno uživatele jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="3f021-224">In the **First Name** textbox, enter the first name of user like **Britta**.</span></span>

    <span data-ttu-id="3f021-225">e.</span><span class="sxs-lookup"><span data-stu-id="3f021-225">e.</span></span> <span data-ttu-id="3f021-226">V **příjmení** textovému poli, zadejte příjmení uživatele jako **Simon**.</span><span class="sxs-lookup"><span data-stu-id="3f021-226">In the **Last Name** textbox, enter the last name of user like **Simon**.</span></span>

    <span data-ttu-id="3f021-227">f.</span><span class="sxs-lookup"><span data-stu-id="3f021-227">f.</span></span> <span data-ttu-id="3f021-228">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3f021-228">Click **Save**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_57.png)

6. <span data-ttu-id="3f021-230">Přiřadit **role uživatele** a **klientský přístup** uživateli.</span><span class="sxs-lookup"><span data-stu-id="3f021-230">Assign **User Roles** and **Client Access** to the user.</span></span> 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_58.png)

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3f021-232">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3f021-232">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3f021-233">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Origami.</span><span class="sxs-lookup"><span data-stu-id="3f021-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Origami.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="3f021-235">**Pokud chcete přiřadit Britta Simon Origami, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3f021-235">**To assign Britta Simon to Origami, perform the following steps:**</span></span>

1. <span data-ttu-id="3f021-236">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3f021-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3f021-238">V seznamu aplikací vyberte **Origami**.</span><span class="sxs-lookup"><span data-stu-id="3f021-238">In the applications list, select **Origami**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_app.png) 

3. <span data-ttu-id="3f021-240">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3f021-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="3f021-242">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3f021-242">Click **Add** button.</span></span> <span data-ttu-id="3f021-243">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3f021-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="3f021-245">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3f021-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3f021-246">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3f021-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3f021-247">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3f021-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3f021-248">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3f021-248">Testing single sign-on</span></span>

<span data-ttu-id="3f021-249">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3f021-249">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3f021-250">Když kliknete na dlaždici Origami na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Origami.</span><span class="sxs-lookup"><span data-stu-id="3f021-250">When you click the Origami tile in the Access Panel, you should get automatically signed-on to your Origami application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3f021-251">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3f021-251">Additional resources</span></span>

* [<span data-ttu-id="3f021-252">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3f021-252">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3f021-253">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3f021-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-origami-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-origami-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-origami-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-origami-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-origami-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-origami-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-origami-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-origami-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-origami-tutorial/tutorial_general_203.png

