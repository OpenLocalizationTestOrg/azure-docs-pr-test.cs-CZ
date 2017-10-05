---
title: 'Kurz: Azure Active Directory integrace s pole | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a pole."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5f3517f8-30f2-4be7-9e47-43d702701797
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 2cc2afe8ff3f0063224c94eb0b8135347051b0aa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-box"></a><span data-ttu-id="9a14e-103">Kurz: Azure Active Directory integrace s poli</span><span class="sxs-lookup"><span data-stu-id="9a14e-103">Tutorial: Azure Active Directory integration with Box</span></span>

<span data-ttu-id="9a14e-104">V tomto kurzu zjistěte, jak integrovat pole s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9a14e-104">In this tutorial, you learn how to integrate Box with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9a14e-105">Integrace pole s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9a14e-105">Integrating Box with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9a14e-106">Můžete řídit ve službě Azure AD, kdo má přístup k poli</span><span class="sxs-lookup"><span data-stu-id="9a14e-106">You can control in Azure AD who has access to Box</span></span>
- <span data-ttu-id="9a14e-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k pole (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="9a14e-107">You can enable your users to automatically get signed-on to Box (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9a14e-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9a14e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9a14e-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9a14e-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9a14e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9a14e-110">Prerequisites</span></span>

<span data-ttu-id="9a14e-111">Konfigurace integrace Azure AD s poli, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="9a14e-111">To configure Azure AD integration with Box, you need the following items:</span></span>

- <span data-ttu-id="9a14e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9a14e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9a14e-113">Pole jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="9a14e-113">A Box single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9a14e-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9a14e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9a14e-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9a14e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9a14e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9a14e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9a14e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9a14e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9a14e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9a14e-118">Scenario description</span></span>
<span data-ttu-id="9a14e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9a14e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9a14e-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9a14e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9a14e-121">Přidání pole z Galerie</span><span class="sxs-lookup"><span data-stu-id="9a14e-121">Adding Box from the gallery</span></span>
2. <span data-ttu-id="9a14e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9a14e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-box-from-the-gallery"></a><span data-ttu-id="9a14e-123">Přidání pole z Galerie</span><span class="sxs-lookup"><span data-stu-id="9a14e-123">Adding Box from the gallery</span></span>
<span data-ttu-id="9a14e-124">Při konfiguraci integrace pole do služby Azure AD, musíte přidat pole z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9a14e-124">To configure the integration of Box into Azure AD, you need to add Box from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9a14e-125">**Chcete-li přidat pole z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9a14e-125">**To add Box from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9a14e-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9a14e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9a14e-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9a14e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9a14e-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9a14e-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="9a14e-131">Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9a14e-131">Click **New application** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="9a14e-133">Do vyhledávacího pole zadejte **pole**.</span><span class="sxs-lookup"><span data-stu-id="9a14e-133">In the search box, type **Box**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-box-tutorial/tutorial_box_search.png)

5. <span data-ttu-id="9a14e-135">Na panelu výsledků vyberte **pole**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9a14e-135">In the results panel, select **Box**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-box-tutorial/tutorial_box_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9a14e-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9a14e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9a14e-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s pole podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="9a14e-138">In this section, you configure and test Azure AD single sign-on with Box based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9a14e-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v poli je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9a14e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Box is to a user in Azure AD.</span></span> <span data-ttu-id="9a14e-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské pole musí navázat.</span><span class="sxs-lookup"><span data-stu-id="9a14e-140">In other words, a link relationship between an Azure AD user and the related user in Box needs to be established.</span></span>

<span data-ttu-id="9a14e-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** pole.</span><span class="sxs-lookup"><span data-stu-id="9a14e-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Box.</span></span>

<span data-ttu-id="9a14e-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s pole, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="9a14e-142">To configure and test Azure AD single sign-on with Box, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9a14e-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="9a14e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9a14e-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9a14e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9a14e-145">**[Vytvoření zkušebního uživatele pole](#creating-a-box-test-user)**  – Pokud chcete mít protějšek Britta Simon pole, ve kterém je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="9a14e-145">**[Creating a Box test user](#creating-a-box-test-user)** - to have a counterpart of Britta Simon in Box that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9a14e-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9a14e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9a14e-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9a14e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9a14e-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9a14e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9a14e-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci pole.</span><span class="sxs-lookup"><span data-stu-id="9a14e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Box application.</span></span>

<span data-ttu-id="9a14e-150">**Ke konfiguraci Azure AD jednotné přihlašování s poli, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9a14e-150">**To configure Azure AD single sign-on with Box, perform the following steps:**</span></span>

1. <span data-ttu-id="9a14e-151">Na portálu Azure na **pole** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9a14e-151">In the Azure portal, on the **Box** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9a14e-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9a14e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-box-tutorial/tutorial_box_samlbase.png)

3. <span data-ttu-id="9a14e-155">Na **pole domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9a14e-155">On the **Box Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-box-tutorial/tutorial_box_url.png)

    <span data-ttu-id="9a14e-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.box.com`</span><span class="sxs-lookup"><span data-stu-id="9a14e-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.box.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9a14e-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="9a14e-158">This value is not real.</span></span> <span data-ttu-id="9a14e-159">Aktualizujte hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9a14e-159">Update the value with the actual Sign-on URL.</span></span> <span data-ttu-id="9a14e-160">Obraťte se na [tým podpory pole klienta](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9a14e-160">Contact [Box Client support team](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) to get this value.</span></span> 
 
4. <span data-ttu-id="9a14e-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="9a14e-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-box-tutorial/tutorial_box_certificate.png) 

5. <span data-ttu-id="9a14e-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9a14e-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-box-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9a14e-165">Chcete-li získat jednotné přihlašování, které jsou nakonfigurované pro vaše aplikace, obraťte se na [tým podpory pole klienta](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) a poskytnout stažený soubor XML.</span><span class="sxs-lookup"><span data-stu-id="9a14e-165">To get SSO configured for your application, Contact [Box Client support team](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) and provide them with the downloaded XML file.</span></span>

> [!TIP]
> <span data-ttu-id="9a14e-166">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="9a14e-166">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9a14e-167">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="9a14e-167">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9a14e-168">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9a14e-168">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9a14e-169">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9a14e-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="9a14e-170">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9a14e-170">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="9a14e-172">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9a14e-172">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9a14e-173">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9a14e-173">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-box-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9a14e-175">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9a14e-175">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-box-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9a14e-177">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9a14e-177">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-box-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9a14e-179">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9a14e-179">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-box-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9a14e-181">a.</span><span class="sxs-lookup"><span data-stu-id="9a14e-181">a.</span></span> <span data-ttu-id="9a14e-182">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9a14e-182">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9a14e-183">b.</span><span class="sxs-lookup"><span data-stu-id="9a14e-183">b.</span></span> <span data-ttu-id="9a14e-184">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9a14e-184">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9a14e-185">c.</span><span class="sxs-lookup"><span data-stu-id="9a14e-185">c.</span></span> <span data-ttu-id="9a14e-186">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9a14e-186">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9a14e-187">d.</span><span class="sxs-lookup"><span data-stu-id="9a14e-187">d.</span></span> <span data-ttu-id="9a14e-188">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9a14e-188">Click **Create**.</span></span>
 
### <a name="creating-a-box-test-user"></a><span data-ttu-id="9a14e-189">Vytvoření zkušebního uživatele pole</span><span class="sxs-lookup"><span data-stu-id="9a14e-189">Creating a Box test user</span></span>

<span data-ttu-id="9a14e-190">V této části se vytvoří uživatele volat Britta Simon pole.</span><span class="sxs-lookup"><span data-stu-id="9a14e-190">In this section, a user called Britta Simon is created in Box.</span></span> <span data-ttu-id="9a14e-191">Pole podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="9a14e-191">Box supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="9a14e-192">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="9a14e-192">There is no action item for you in this section.</span></span> <span data-ttu-id="9a14e-193">Pokud uživatel v poli ještě neexistuje, vytvoří se nový při pokusu o přístup k poli.</span><span class="sxs-lookup"><span data-stu-id="9a14e-193">If a user doesn't already exist in Box, a new one is created when you attempt to access Box.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9a14e-194">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9a14e-194">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9a14e-195">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup k poli.</span><span class="sxs-lookup"><span data-stu-id="9a14e-195">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Box.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9a14e-197">**Přiřadit Britta Simon pole, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9a14e-197">**To assign Britta Simon to Box, perform the following steps:**</span></span>

1. <span data-ttu-id="9a14e-198">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9a14e-198">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9a14e-200">V seznamu aplikací vyberte **pole**.</span><span class="sxs-lookup"><span data-stu-id="9a14e-200">In the applications list, select **Box**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-box-tutorial/tutorial_box_app.png) 

3. <span data-ttu-id="9a14e-202">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9a14e-202">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9a14e-204">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9a14e-204">Click **Add** button.</span></span> <span data-ttu-id="9a14e-205">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9a14e-205">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="9a14e-207">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9a14e-207">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9a14e-208">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9a14e-208">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9a14e-209">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9a14e-209">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9a14e-210">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9a14e-210">Testing single sign-on</span></span>

<span data-ttu-id="9a14e-211">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9a14e-211">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="9a14e-212">Když kliknete na dlaždici pole na přístupovém panelu, měli byste obdržet přihlašovací stránku k získání přihlášení k aplikaci pole.</span><span class="sxs-lookup"><span data-stu-id="9a14e-212">When you click the Box tile in the Access Panel, you should get login page to get signed-on to your Box application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9a14e-213">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9a14e-213">Additional resources</span></span>

* [<span data-ttu-id="9a14e-214">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9a14e-214">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9a14e-215">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9a14e-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="9a14e-216">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="9a14e-216">Configure User Provisioning</span></span>](active-directory-saas-box-userprovisioning-tutorial.md)


<!--Image references-->

[1]: ./media/active-directory-saas-box-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-box-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-box-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-box-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-box-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-box-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-box-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-box-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-box-tutorial/tutorial_general_203.png

