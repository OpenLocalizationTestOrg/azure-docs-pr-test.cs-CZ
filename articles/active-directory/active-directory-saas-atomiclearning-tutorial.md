---
title: 'Kurz: Azure Active Directory integrace s Atomic Learning | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Atomic učení."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 495f54a6-e6c4-41b0-aafa-a6283d33efc8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: 6cce8fc839e60eb6498ab48bf68e9906c98889a2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atomic-learning"></a><span data-ttu-id="83dcd-103">Kurz: Azure Active Directory integrace s Atomic učení</span><span class="sxs-lookup"><span data-stu-id="83dcd-103">Tutorial: Azure Active Directory integration with Atomic Learning</span></span>

<span data-ttu-id="83dcd-104">V tomto kurzu zjistěte, jak integrovat Atomic Learning s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="83dcd-104">In this tutorial, you learn how to integrate Atomic Learning with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="83dcd-105">Integrace Atomic Learning s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="83dcd-105">Integrating Atomic Learning with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="83dcd-106">Můžete řídit ve službě Azure AD, který má přístup k Atomic učení</span><span class="sxs-lookup"><span data-stu-id="83dcd-106">You can control in Azure AD who has access to Atomic Learning</span></span>
- <span data-ttu-id="83dcd-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Atomic Learning (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="83dcd-107">You can enable your users to automatically get signed-on to Atomic Learning (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="83dcd-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="83dcd-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="83dcd-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="83dcd-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83dcd-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="83dcd-110">Prerequisites</span></span>

<span data-ttu-id="83dcd-111">Konfigurace integrace Azure AD s Atomic Learning, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="83dcd-111">To configure Azure AD integration with Atomic Learning, you need the following items:</span></span>

- <span data-ttu-id="83dcd-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="83dcd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="83dcd-113">Atomic Learning jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="83dcd-113">An Atomic Learning single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="83dcd-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="83dcd-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="83dcd-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="83dcd-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="83dcd-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="83dcd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="83dcd-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="83dcd-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="83dcd-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="83dcd-118">Scenario description</span></span>
<span data-ttu-id="83dcd-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="83dcd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="83dcd-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="83dcd-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="83dcd-121">Přidání Atomic Learning z Galerie</span><span class="sxs-lookup"><span data-stu-id="83dcd-121">Adding Atomic Learning from the gallery</span></span>
2. <span data-ttu-id="83dcd-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="83dcd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-atomic-learning-from-the-gallery"></a><span data-ttu-id="83dcd-123">Přidání Atomic Learning z Galerie</span><span class="sxs-lookup"><span data-stu-id="83dcd-123">Adding Atomic Learning from the gallery</span></span>
<span data-ttu-id="83dcd-124">Při konfiguraci integrace Atomic Learning do služby Azure AD potřebujete přidat Atomic Learning z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="83dcd-124">To configure the integration of Atomic Learning into Azure AD, you need to add Atomic Learning from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="83dcd-125">**Pokud chcete přidat Atomic Learning z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="83dcd-125">**To add Atomic Learning from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="83dcd-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="83dcd-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="83dcd-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="83dcd-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="83dcd-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="83dcd-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="83dcd-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="83dcd-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="83dcd-133">Do vyhledávacího pole zadejte **Atomic Learning**.</span><span class="sxs-lookup"><span data-stu-id="83dcd-133">In the search box, type **Atomic Learning**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atomiclearning-tutorial/tutorial_atomiclearning_search.png)

5. <span data-ttu-id="83dcd-135">Na panelu výsledků vyberte **Atomic Learning**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="83dcd-135">In the results panel, select **Atomic Learning**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atomiclearning-tutorial/tutorial_atomiclearning_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="83dcd-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="83dcd-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="83dcd-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Atomic Learning podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="83dcd-138">In this section, you configure and test Azure AD single sign-on with Atomic Learning based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="83dcd-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Atomic Learning je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="83dcd-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Atomic Learning is to a user in Azure AD.</span></span> <span data-ttu-id="83dcd-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Atomic Learning musí navázat.</span><span class="sxs-lookup"><span data-stu-id="83dcd-140">In other words, a link relationship between an Azure AD user and the related user in Atomic Learning needs to be established.</span></span>

<span data-ttu-id="83dcd-141">V Atomic Learning přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="83dcd-141">In Atomic Learning, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="83dcd-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Atomic Learning, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="83dcd-142">To configure and test Azure AD single sign-on with Atomic Learning, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="83dcd-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="83dcd-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="83dcd-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="83dcd-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="83dcd-145">**[Vytváření testovacího uživatele Atomic Learning](#creating-an-atomic-learning-test-user)**  – Pokud chcete mít protějšek Britta Simon v Atomic Learning propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="83dcd-145">**[Creating an Atomic Learning test user](#creating-an-atomic-learning-test-user)** - to have a counterpart of Britta Simon in Atomic Learning that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="83dcd-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="83dcd-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="83dcd-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="83dcd-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="83dcd-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="83dcd-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="83dcd-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Atomic učení.</span><span class="sxs-lookup"><span data-stu-id="83dcd-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Atomic Learning application.</span></span>

<span data-ttu-id="83dcd-150">**Ke konfiguraci Azure AD jednotné přihlašování s Atomic Learning, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="83dcd-150">**To configure Azure AD single sign-on with Atomic Learning, perform the following steps:**</span></span>

1. <span data-ttu-id="83dcd-151">Na portálu Azure na **Atomic Learning** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="83dcd-151">In the Azure portal, on the **Atomic Learning** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="83dcd-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="83dcd-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atomiclearning-tutorial/tutorial_atomiclearning_samlbase.png)

3. <span data-ttu-id="83dcd-155">Na **Atomic Learning domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="83dcd-155">On the **Atomic Learning Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atomiclearning-tutorial/tutorial_atomiclearning_url.png)

     <span data-ttu-id="83dcd-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://secure2.atomiclearning.com/sso/shibboleth/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="83dcd-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://secure2.atomiclearning.com/sso/shibboleth/<companyname>`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="83dcd-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="83dcd-158">This value is not real.</span></span> <span data-ttu-id="83dcd-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="83dcd-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="83dcd-160">Obraťte se na [tým podpory Atomic klientů Learning](mailto:cs@atomiclearning.com) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="83dcd-160">Contact [Atomic Learning Client support team](mailto:cs@atomiclearning.com) to get this value.</span></span> 
 
4. <span data-ttu-id="83dcd-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="83dcd-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atomiclearning-tutorial/tutorial_atomiclearning_certificate.png) 

5. <span data-ttu-id="83dcd-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="83dcd-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="83dcd-165">Konfigurace jednotného přihlašování na **Atomic Learning** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Atomic Learning](mailto:cs@atomiclearning.com).</span><span class="sxs-lookup"><span data-stu-id="83dcd-165">To configure single sign-on on **Atomic Learning** side, you need to send the downloaded **Metadata XML** to [Atomic Learning support team](mailto:cs@atomiclearning.com).</span></span> <span data-ttu-id="83dcd-166">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="83dcd-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="83dcd-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="83dcd-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="83dcd-168">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="83dcd-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="83dcd-169">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="83dcd-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="83dcd-170">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="83dcd-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="83dcd-171">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="83dcd-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="83dcd-173">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="83dcd-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="83dcd-174">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="83dcd-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atomiclearning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="83dcd-176">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="83dcd-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atomiclearning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="83dcd-178">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="83dcd-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atomiclearning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="83dcd-180">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="83dcd-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atomiclearning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="83dcd-182">a.</span><span class="sxs-lookup"><span data-stu-id="83dcd-182">a.</span></span> <span data-ttu-id="83dcd-183">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="83dcd-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="83dcd-184">b.</span><span class="sxs-lookup"><span data-stu-id="83dcd-184">b.</span></span> <span data-ttu-id="83dcd-185">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="83dcd-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="83dcd-186">c.</span><span class="sxs-lookup"><span data-stu-id="83dcd-186">c.</span></span> <span data-ttu-id="83dcd-187">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="83dcd-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="83dcd-188">d.</span><span class="sxs-lookup"><span data-stu-id="83dcd-188">d.</span></span> <span data-ttu-id="83dcd-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="83dcd-189">Click **Create**.</span></span>
 
### <a name="creating-an-atomic-learning-test-user"></a><span data-ttu-id="83dcd-190">Vytváření testovacího uživatele Atomic učení</span><span class="sxs-lookup"><span data-stu-id="83dcd-190">Creating an Atomic Learning test user</span></span>

<span data-ttu-id="83dcd-191">V této části vytvoříte uživatele volal Britta Simon v Atomic učení.</span><span class="sxs-lookup"><span data-stu-id="83dcd-191">In this section, you create a user called Britta Simon in Atomic Learning.</span></span> <span data-ttu-id="83dcd-192">Atomic Learning podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="83dcd-192">Atomic Learning supports just-in-time provisioning, which is by default enabled.</span></span> 

<span data-ttu-id="83dcd-193">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="83dcd-193">There is no action item for you in this section.</span></span> <span data-ttu-id="83dcd-194">Nový uživatel se vytvoří během pokusu o přístup k Atomic Learning, pokud neexistuje, ještě pomocí e-mailovou adresu uživatele.</span><span class="sxs-lookup"><span data-stu-id="83dcd-194">A new user is created during an attempt to access Atomic Learning if it doesn't exist yet using the email address for the user.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="83dcd-195">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="83dcd-195">Assigning the Azure AD test user</span></span>

<span data-ttu-id="83dcd-196">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Atomic učení.</span><span class="sxs-lookup"><span data-stu-id="83dcd-196">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Atomic Learning.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="83dcd-198">**Pokud chcete přiřadit Britta Simon Atomic Learning, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="83dcd-198">**To assign Britta Simon to Atomic Learning, perform the following steps:**</span></span>

1. <span data-ttu-id="83dcd-199">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="83dcd-199">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="83dcd-201">V seznamu aplikací vyberte **Atomic Learning**.</span><span class="sxs-lookup"><span data-stu-id="83dcd-201">In the applications list, select **Atomic Learning**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atomiclearning-tutorial/tutorial_atomiclearning_app.png) 

3. <span data-ttu-id="83dcd-203">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="83dcd-203">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="83dcd-205">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="83dcd-205">Click **Add** button.</span></span> <span data-ttu-id="83dcd-206">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="83dcd-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="83dcd-208">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="83dcd-208">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="83dcd-209">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="83dcd-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="83dcd-210">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="83dcd-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="83dcd-211">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="83dcd-211">Testing single sign-on</span></span>

<span data-ttu-id="83dcd-212">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="83dcd-212">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="83dcd-213">Když kliknete na dlaždici Atomic Learning na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Atomic učení.</span><span class="sxs-lookup"><span data-stu-id="83dcd-213">When you click the Atomic Learning tile in the Access Panel, you should get automatically signed-on to your Atomic Learning application.</span></span>
<span data-ttu-id="83dcd-214">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="83dcd-214">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="83dcd-215">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="83dcd-215">Additional resources</span></span>

* [<span data-ttu-id="83dcd-216">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="83dcd-216">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="83dcd-217">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="83dcd-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atomiclearning-tutorial/tutorial_general_203.png

