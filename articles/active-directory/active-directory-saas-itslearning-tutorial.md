---
title: 'Kurz: Azure Active Directory integrace s itslearning | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a itslearning."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 60587ba3-1396-4b8a-9ac1-e22a98e5e0ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 32c8a8dff533f726784fb52b9496c2cb50ecfde7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-itslearning"></a><span data-ttu-id="012f2-103">Kurz: Azure Active Directory integrace s itslearning</span><span class="sxs-lookup"><span data-stu-id="012f2-103">Tutorial: Azure Active Directory integration with itslearning</span></span>

<span data-ttu-id="012f2-104">V tomto kurzu zjistěte, jak integrovat itslearning s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="012f2-104">In this tutorial, you learn how to integrate itslearning with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="012f2-105">Integrace itslearning s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="012f2-105">Integrating itslearning with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="012f2-106">Můžete řídit ve službě Azure AD, který má přístup k itslearning</span><span class="sxs-lookup"><span data-stu-id="012f2-106">You can control in Azure AD who has access to itslearning</span></span>
- <span data-ttu-id="012f2-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k itslearning (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="012f2-107">You can enable your users to automatically get signed-on to itslearning (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="012f2-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="012f2-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="012f2-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="012f2-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="012f2-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="012f2-110">Prerequisites</span></span>

<span data-ttu-id="012f2-111">Konfigurace integrace Azure AD s itslearning, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="012f2-111">To configure Azure AD integration with itslearning, you need the following items:</span></span>

- <span data-ttu-id="012f2-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="012f2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="012f2-113">Itslearning jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="012f2-113">An itslearning single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="012f2-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="012f2-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="012f2-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="012f2-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="012f2-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="012f2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="012f2-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="012f2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="012f2-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="012f2-118">Scenario description</span></span>
<span data-ttu-id="012f2-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="012f2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="012f2-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="012f2-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="012f2-121">Přidání itslearning z Galerie</span><span class="sxs-lookup"><span data-stu-id="012f2-121">Adding itslearning from the gallery</span></span>
2. <span data-ttu-id="012f2-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="012f2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-itslearning-from-the-gallery"></a><span data-ttu-id="012f2-123">Přidání itslearning z Galerie</span><span class="sxs-lookup"><span data-stu-id="012f2-123">Adding itslearning from the gallery</span></span>
<span data-ttu-id="012f2-124">Při konfiguraci integrace itslearning do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS itslearning z galerie.</span><span class="sxs-lookup"><span data-stu-id="012f2-124">To configure the integration of itslearning into Azure AD, you need to add itslearning from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="012f2-125">**Pokud chcete přidat itslearning z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="012f2-125">**To add itslearning from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="012f2-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="012f2-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="012f2-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="012f2-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="012f2-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="012f2-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="012f2-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="012f2-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="012f2-133">Do vyhledávacího pole zadejte **itslearning**.</span><span class="sxs-lookup"><span data-stu-id="012f2-133">In the search box, type **itslearning**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-itslearning-tutorial/tutorial_itslearning_search.png)

5. <span data-ttu-id="012f2-135">Na panelu výsledků vyberte **itslearning**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="012f2-135">In the results panel, select **itslearning**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-itslearning-tutorial/tutorial_itslearning_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="012f2-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="012f2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="012f2-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s itslearning podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="012f2-138">In this section, you configure and test Azure AD single sign-on with itslearning based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="012f2-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v itslearning je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="012f2-139">For single sign-on to work, Azure AD needs to know what the counterpart user in itslearning is to a user in Azure AD.</span></span> <span data-ttu-id="012f2-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v itslearning musí navázat.</span><span class="sxs-lookup"><span data-stu-id="012f2-140">In other words, a link relationship between an Azure AD user and the related user in itslearning needs to be established.</span></span>

<span data-ttu-id="012f2-141">V itslearning, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="012f2-141">In itslearning, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="012f2-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s itslearning, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="012f2-142">To configure and test Azure AD single sign-on with itslearning, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="012f2-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="012f2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="012f2-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="012f2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="012f2-145">**[Vytváření testovacího uživatele itslearning](#creating-an-itslearning-test-user)**  – Pokud chcete mít protějšek Britta Simon v itslearning propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="012f2-145">**[Creating an itslearning test user](#creating-an-itslearning-test-user)** - to have a counterpart of Britta Simon in itslearning that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="012f2-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="012f2-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="012f2-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="012f2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="012f2-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="012f2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="012f2-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci itslearning.</span><span class="sxs-lookup"><span data-stu-id="012f2-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your itslearning application.</span></span>

<span data-ttu-id="012f2-150">**Ke konfiguraci Azure AD jednotné přihlašování s itslearning, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="012f2-150">**To configure Azure AD single sign-on with itslearning, perform the following steps:**</span></span>

1. <span data-ttu-id="012f2-151">Na portálu Azure na **itslearning** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="012f2-151">In the Azure portal, on the **itslearning** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="012f2-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="012f2-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-itslearning-tutorial/tutorial_itslearning_samlbase.png)

3. <span data-ttu-id="012f2-155">Na **itslearning domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="012f2-155">On the **itslearning Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-itslearning-tutorial/tutorial_itslearning_url.png)

    <span data-ttu-id="012f2-157">a.</span><span class="sxs-lookup"><span data-stu-id="012f2-157">a.</span></span> <span data-ttu-id="012f2-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:</span><span class="sxs-lookup"><span data-stu-id="012f2-158">In the **Sign-on URL** textbox, type a URL as:</span></span>
    | |
    |--| 
    | `https://www.itslearning.com/index.aspx`|
    | `https://us1.itslearning.com/index.aspx`|

    <span data-ttu-id="012f2-159">b.</span><span class="sxs-lookup"><span data-stu-id="012f2-159">b.</span></span> <span data-ttu-id="012f2-160">V **identifikátor** textovému poli, zadejte adresu URL jako:`urn:mace:saml2v2.no:services:com.itslearning`</span><span class="sxs-lookup"><span data-stu-id="012f2-160">In the **Identifier** textbox, type a URL as: `urn:mace:saml2v2.no:services:com.itslearning`</span></span>

4. <span data-ttu-id="012f2-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="012f2-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-itslearning-tutorial/tutorial_itslearning_certificate.png) 

5. <span data-ttu-id="012f2-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="012f2-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-itslearning-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="012f2-165">Konfigurace jednotného přihlašování na **itslearning** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory itslearning](mailto:support@itslearning.com).</span><span class="sxs-lookup"><span data-stu-id="012f2-165">To configure single sign-on on **itslearning** side, you need to send the downloaded **Metadata XML** to [itslearning support team](mailto:support@itslearning.com).</span></span> <span data-ttu-id="012f2-166">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="012f2-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="012f2-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="012f2-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="012f2-168">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="012f2-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="012f2-169">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="012f2-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="012f2-170">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="012f2-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="012f2-171">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="012f2-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="012f2-173">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="012f2-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="012f2-174">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="012f2-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-itslearning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="012f2-176">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="012f2-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-itslearning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="012f2-178">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="012f2-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-itslearning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="012f2-180">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="012f2-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-itslearning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="012f2-182">a.</span><span class="sxs-lookup"><span data-stu-id="012f2-182">a.</span></span> <span data-ttu-id="012f2-183">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="012f2-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="012f2-184">b.</span><span class="sxs-lookup"><span data-stu-id="012f2-184">b.</span></span> <span data-ttu-id="012f2-185">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="012f2-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="012f2-186">c.</span><span class="sxs-lookup"><span data-stu-id="012f2-186">c.</span></span> <span data-ttu-id="012f2-187">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="012f2-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="012f2-188">d.</span><span class="sxs-lookup"><span data-stu-id="012f2-188">d.</span></span> <span data-ttu-id="012f2-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="012f2-189">Click **Create**.</span></span>
 
### <a name="creating-an-itslearning-test-user"></a><span data-ttu-id="012f2-190">Vytváření testovacího uživatele itslearning</span><span class="sxs-lookup"><span data-stu-id="012f2-190">Creating an itslearning test user</span></span>

<span data-ttu-id="012f2-191">V této části vytvoříte volal Britta Simon v itslearning uživatele.</span><span class="sxs-lookup"><span data-stu-id="012f2-191">In this section, you create a user called Britta Simon in itslearning.</span></span> <span data-ttu-id="012f2-192">Práce s [tým podpory klienta itslearning](mailto:support@itslearning.com) přidat uživatele do itslearning platformy.</span><span class="sxs-lookup"><span data-stu-id="012f2-192">Work with [itslearning Client support team](mailto:support@itslearning.com) to add the users in the itslearning platform.</span></span> <span data-ttu-id="012f2-193">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="012f2-193">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="012f2-194">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="012f2-194">Assigning the Azure AD test user</span></span>

<span data-ttu-id="012f2-195">V této části povolíte Britta Simon chcete použít Azure jednotného přihlašování k udělení přístupu k itslearning.</span><span class="sxs-lookup"><span data-stu-id="012f2-195">In this section, you enable Britta Simon to use Azure single sign-on by granting access to itslearning.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="012f2-197">**Pokud chcete přiřadit Britta Simon itslearning, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="012f2-197">**To assign Britta Simon to itslearning, perform the following steps:**</span></span>

1. <span data-ttu-id="012f2-198">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="012f2-198">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="012f2-200">V seznamu aplikací vyberte **itslearning**.</span><span class="sxs-lookup"><span data-stu-id="012f2-200">In the applications list, select **itslearning**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-itslearning-tutorial/tutorial_itslearning_app.png) 

3. <span data-ttu-id="012f2-202">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="012f2-202">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="012f2-204">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="012f2-204">Click **Add** button.</span></span> <span data-ttu-id="012f2-205">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="012f2-205">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="012f2-207">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="012f2-207">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="012f2-208">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="012f2-208">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="012f2-209">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="012f2-209">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="012f2-210">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="012f2-210">Testing single sign-on</span></span>

<span data-ttu-id="012f2-211">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="012f2-211">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="012f2-212">Když kliknete na dlaždici itslearning na přístupovém panelu, měli byste obdržet přihlašovací stránku itslearning aplikace.</span><span class="sxs-lookup"><span data-stu-id="012f2-212">When you click the itslearning tile in the Access Panel, you should get login page of itslearning application.</span></span> <span data-ttu-id="012f2-213">Klikněte na tlačítko **přihlásit se přes Windows Azure ACS1** pro úspěšné přihlášení do aplikace.</span><span class="sxs-lookup"><span data-stu-id="012f2-213">Click **Log in with Windows Azure ACS1** for successful login into the application.</span></span>

  ![Přihlásit](./media/active-directory-saas-itslearning-tutorial/login.png)

<span data-ttu-id="012f2-215">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="012f2-215">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="012f2-216">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="012f2-216">Additional resources</span></span>

* [<span data-ttu-id="012f2-217">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="012f2-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="012f2-218">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="012f2-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_203.png

