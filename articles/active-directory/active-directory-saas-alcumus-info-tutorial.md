---
title: "Kurz: Azure Active Directory integrace s Alcumus údaje Exchange | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Alcumus údaje Exchange."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d26034b8-f0d5-4f65-aa56-0fc168ceec8c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jeedes
ms.openlocfilehash: 1f67682111de0bea1b18fd97d739492661ebbfd9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-alcumus-info-exchange"></a><span data-ttu-id="a92b3-103">Kurz: Azure Active Directory integrace s Alcumus údaje Exchange</span><span class="sxs-lookup"><span data-stu-id="a92b3-103">Tutorial: Azure Active Directory integration with Alcumus Info Exchange</span></span>

<span data-ttu-id="a92b3-104">V tomto kurzu zjistěte, jak integrovat Exchange Alcumus informace o službě Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a92b3-104">In this tutorial, you learn how to integrate Alcumus Info Exchange with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a92b3-105">Integrace Alcumus údaje Exchange s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a92b3-105">Integrating Alcumus Info Exchange with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a92b3-106">Můžete řídit ve službě Azure AD, který má přístup k systému Exchange Alcumus informace</span><span class="sxs-lookup"><span data-stu-id="a92b3-106">You can control in Azure AD who has access to Alcumus Info Exchange</span></span>
- <span data-ttu-id="a92b3-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Alcumus údaje Exchange (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a92b3-107">You can enable your users to automatically get signed-on to Alcumus Info Exchange (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a92b3-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a92b3-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a92b3-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a92b3-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a92b3-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a92b3-110">Prerequisites</span></span>

<span data-ttu-id="a92b3-111">Konfigurace integrace Azure AD s Alcumus údaje Exchange, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="a92b3-111">To configure Azure AD integration with Alcumus Info Exchange, you need the following items:</span></span>

- <span data-ttu-id="a92b3-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a92b3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a92b3-113">Alcumus údaje Exchange jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a92b3-113">An Alcumus Info Exchange single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a92b3-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a92b3-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a92b3-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a92b3-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a92b3-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a92b3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a92b3-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a92b3-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a92b3-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a92b3-118">Scenario description</span></span>
<span data-ttu-id="a92b3-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a92b3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a92b3-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a92b3-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a92b3-121">Přidání Alcumus údaje Exchange z Galerie</span><span class="sxs-lookup"><span data-stu-id="a92b3-121">Adding Alcumus Info Exchange from the gallery</span></span>
2. <span data-ttu-id="a92b3-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a92b3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-alcumus-info-exchange-from-the-gallery"></a><span data-ttu-id="a92b3-123">Přidání Alcumus údaje Exchange z Galerie</span><span class="sxs-lookup"><span data-stu-id="a92b3-123">Adding Alcumus Info Exchange from the gallery</span></span>
<span data-ttu-id="a92b3-124">Při konfiguraci integrace systému Exchange Alcumus informace do služby Azure AD, potřebujete přidat Exchange Alcumus informace z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="a92b3-124">To configure the integration of Alcumus Info Exchange into Azure AD, you need to add Alcumus Info Exchange from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a92b3-125">**Pokud chcete přidat Exchange Alcumus informace z galerie, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="a92b3-125">**To add Alcumus Info Exchange from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a92b3-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a92b3-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a92b3-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a92b3-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a92b3-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a92b3-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a92b3-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a92b3-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a92b3-133">Do vyhledávacího pole zadejte **Alcumus údaje Exchange**.</span><span class="sxs-lookup"><span data-stu-id="a92b3-133">In the search box, type **Alcumus Info Exchange**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_search.png)

5. <span data-ttu-id="a92b3-135">Na panelu výsledků vyberte **Alcumus údaje Exchange**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a92b3-135">In the results panel, select **Alcumus Info Exchange**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a92b3-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a92b3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a92b3-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování se serverem Exchange Alcumus informace podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="a92b3-138">In this section, you configure and test Azure AD single sign-on with Alcumus Info Exchange based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a92b3-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v Alcumus údaje Exchange je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a92b3-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Alcumus Info Exchange is to a user in Azure AD.</span></span> <span data-ttu-id="a92b3-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Alcumus údaje Exchange musí navázat.</span><span class="sxs-lookup"><span data-stu-id="a92b3-140">In other words, a link relationship between an Azure AD user and the related user in Alcumus Info Exchange needs to be established.</span></span>

<span data-ttu-id="a92b3-141">V systému Exchange Alcumus informace, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="a92b3-141">In Alcumus Info Exchange, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a92b3-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Alcumus údaje Exchange, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="a92b3-142">To configure and test Azure AD single sign-on with Alcumus Info Exchange, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a92b3-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="a92b3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a92b3-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a92b3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a92b3-145">**[Vytváření testovacího uživatele Exchange informace Alcumus](#creating-an-alcumus-info-exchange-test-user)**  – Pokud chcete mít protějšek Britta Simon v systému Exchange Alcumus informace, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="a92b3-145">**[Creating an Alcumus Info Exchange test user](#creating-an-alcumus-info-exchange-test-user)** - to have a counterpart of Britta Simon in Alcumus Info Exchange that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a92b3-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a92b3-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a92b3-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a92b3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a92b3-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a92b3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a92b3-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Alcumus údaje Exchange.</span><span class="sxs-lookup"><span data-stu-id="a92b3-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Alcumus Info Exchange application.</span></span>

<span data-ttu-id="a92b3-150">**O konfiguraci Azure AD jednotné přihlašování s Alcumus informace, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a92b3-150">**To configure Azure AD single sign-on with Alcumus Info Exchange, perform the following steps:**</span></span>

1. <span data-ttu-id="a92b3-151">Na portálu Azure na **Alcumus údaje Exchange** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a92b3-151">In the Azure portal, on the **Alcumus Info Exchange** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a92b3-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a92b3-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_samlbase.png)

3. <span data-ttu-id="a92b3-155">Na **Alcumus údaje Exchange domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a92b3-155">On the **Alcumus Info Exchange Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_url.png)

    <span data-ttu-id="a92b3-157">a.</span><span class="sxs-lookup"><span data-stu-id="a92b3-157">a.</span></span> <span data-ttu-id="a92b3-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.info-exchange.com`</span><span class="sxs-lookup"><span data-stu-id="a92b3-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.info-exchange.com`</span></span>

    <span data-ttu-id="a92b3-159">b.</span><span class="sxs-lookup"><span data-stu-id="a92b3-159">b.</span></span> <span data-ttu-id="a92b3-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.info-exchange.com/Auth/`</span><span class="sxs-lookup"><span data-stu-id="a92b3-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.info-exchange.com/Auth/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a92b3-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="a92b3-161">These values are not real.</span></span> <span data-ttu-id="a92b3-162">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a92b3-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="a92b3-163">Obraťte se na [tým podpory Alcumus údaje Exchange](mailto:helpdesk@alcumusgroup.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="a92b3-163">Contact [Alcumus Info Exchange support team](mailto:helpdesk@alcumusgroup.com) to get these values.</span></span>
 
4. <span data-ttu-id="a92b3-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="a92b3-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_certificate.png) 

5. <span data-ttu-id="a92b3-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a92b3-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a92b3-168">Konfigurace jednotného přihlašování na **Alcumus údaje Exchange** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Alcumus údaje Exchange](mailto:helpdesk@alcumusgroup.com).</span><span class="sxs-lookup"><span data-stu-id="a92b3-168">To configure single sign-on on **Alcumus Info Exchange** side, you need to send the downloaded **Metadata XML** to [Alcumus Info Exchange support team](mailto:helpdesk@alcumusgroup.com).</span></span>

> [!TIP]
> <span data-ttu-id="a92b3-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="a92b3-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a92b3-170">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="a92b3-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a92b3-171">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a92b3-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a92b3-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a92b3-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="a92b3-173">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a92b3-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a92b3-175">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a92b3-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a92b3-176">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a92b3-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a92b3-178">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a92b3-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a92b3-180">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a92b3-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a92b3-182">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a92b3-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a92b3-184">a.</span><span class="sxs-lookup"><span data-stu-id="a92b3-184">a.</span></span> <span data-ttu-id="a92b3-185">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a92b3-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a92b3-186">b.</span><span class="sxs-lookup"><span data-stu-id="a92b3-186">b.</span></span> <span data-ttu-id="a92b3-187">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a92b3-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a92b3-188">c.</span><span class="sxs-lookup"><span data-stu-id="a92b3-188">c.</span></span> <span data-ttu-id="a92b3-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a92b3-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a92b3-190">d.</span><span class="sxs-lookup"><span data-stu-id="a92b3-190">d.</span></span> <span data-ttu-id="a92b3-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a92b3-191">Click **Create**.</span></span>
 
### <a name="creating-an-alcumus-info-exchange-test-user"></a><span data-ttu-id="a92b3-192">Vytvoření Exchange Alcumus údaje testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="a92b3-192">Creating an Alcumus Info Exchange test user</span></span>

<span data-ttu-id="a92b3-193">Cílem této části je vytvoření uživatele volal Britta Simon v Alcumus údaje Exchange.</span><span class="sxs-lookup"><span data-stu-id="a92b3-193">The objective of this section is to create a user called Britta Simon in Alcumus Info Exchange.</span></span>

<span data-ttu-id="a92b3-194">Chcete-li vytvořit uživatele volal Britta Simon v Alcumus údaje Exchange, obraťte se [tým podpory Alcumus údaje Exchange](mailto:helpdesk@alcumusgroup.com).</span><span class="sxs-lookup"><span data-stu-id="a92b3-194">To create a user called Britta Simon in Alcumus Info Exchange, Contact the [Alcumus Info Exchange support team](mailto:helpdesk@alcumusgroup.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a92b3-195">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a92b3-195">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a92b3-196">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k systému Exchange Alcumus informace.</span><span class="sxs-lookup"><span data-stu-id="a92b3-196">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Alcumus Info Exchange.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a92b3-198">**Pokud chcete přiřadit Britta Simon Alcumus údaje Exchange, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a92b3-198">**To assign Britta Simon to Alcumus Info Exchange, perform the following steps:**</span></span>

1. <span data-ttu-id="a92b3-199">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a92b3-199">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a92b3-201">V seznamu aplikací vyberte **Alcumus údaje Exchange**.</span><span class="sxs-lookup"><span data-stu-id="a92b3-201">In the applications list, select **Alcumus Info Exchange**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_app.png) 

3. <span data-ttu-id="a92b3-203">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a92b3-203">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a92b3-205">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a92b3-205">Click **Add** button.</span></span> <span data-ttu-id="a92b3-206">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a92b3-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a92b3-208">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a92b3-208">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a92b3-209">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a92b3-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a92b3-210">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a92b3-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a92b3-211">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a92b3-211">Testing single sign-on</span></span>

<span data-ttu-id="a92b3-212">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a92b3-212">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="a92b3-213">Když kliknete na dlaždici Alcumus údaje Exchange na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Alcumus údaje Exchange.</span><span class="sxs-lookup"><span data-stu-id="a92b3-213">When you click the Alcumus Info Exchange tile in the Access Panel, you should get automatically signed-on to your Alcumus Info Exchange application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a92b3-214">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a92b3-214">Additional resources</span></span>

* [<span data-ttu-id="a92b3-215">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a92b3-215">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a92b3-216">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a92b3-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_203.png
