---
title: 'Kurz: Azure Active Directory integrace s Wikispaces | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Wikispaces."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 665b95aa-f7f5-4406-9e2a-6fc299a1599c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: d01543955bdf6a274571f67eafdff5f637863d5c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wikispaces"></a><span data-ttu-id="b5fb5-103">Kurz: Azure Active Directory integrace s Wikispaces</span><span class="sxs-lookup"><span data-stu-id="b5fb5-103">Tutorial: Azure Active Directory integration with Wikispaces</span></span>

<span data-ttu-id="b5fb5-104">V tomto kurzu zjistěte, jak integrovat Wikispaces s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b5fb5-104">In this tutorial, you learn how to integrate Wikispaces with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b5fb5-105">Integrace Wikispaces s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b5fb5-105">Integrating Wikispaces with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b5fb5-106">Můžete řídit ve službě Azure AD, který má přístup k Wikispaces</span><span class="sxs-lookup"><span data-stu-id="b5fb5-106">You can control in Azure AD who has access to Wikispaces</span></span>
- <span data-ttu-id="b5fb5-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Wikispaces (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b5fb5-107">You can enable your users to automatically get signed-on to Wikispaces (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b5fb5-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b5fb5-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b5fb5-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b5fb5-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b5fb5-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b5fb5-110">Prerequisites</span></span>

<span data-ttu-id="b5fb5-111">Konfigurace integrace Azure AD s Wikispaces, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="b5fb5-111">To configure Azure AD integration with Wikispaces, you need the following items:</span></span>

- <span data-ttu-id="b5fb5-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b5fb5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b5fb5-113">Wikispaces jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b5fb5-113">A Wikispaces single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b5fb5-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b5fb5-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b5fb5-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b5fb5-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b5fb5-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b5fb5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b5fb5-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b5fb5-118">Scenario description</span></span>
<span data-ttu-id="b5fb5-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b5fb5-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b5fb5-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b5fb5-121">Přidání Wikispaces z Galerie</span><span class="sxs-lookup"><span data-stu-id="b5fb5-121">Adding Wikispaces from the gallery</span></span>
2. <span data-ttu-id="b5fb5-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b5fb5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-wikispaces-from-the-gallery"></a><span data-ttu-id="b5fb5-123">Přidání Wikispaces z Galerie</span><span class="sxs-lookup"><span data-stu-id="b5fb5-123">Adding Wikispaces from the gallery</span></span>
<span data-ttu-id="b5fb5-124">Při konfiguraci integrace Wikispaces do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Wikispaces z galerie.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-124">To configure the integration of Wikispaces into Azure AD, you need to add Wikispaces from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b5fb5-125">**Pokud chcete přidat Wikispaces z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b5fb5-125">**To add Wikispaces from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b5fb5-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b5fb5-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b5fb5-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b5fb5-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b5fb5-133">Do vyhledávacího pole zadejte **Wikispaces**.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-133">In the search box, type **Wikispaces**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_search.png)

5. <span data-ttu-id="b5fb5-135">Na panelu výsledků vyberte **Wikispaces**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-135">In the results panel, select **Wikispaces**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b5fb5-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b5fb5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b5fb5-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Wikispaces podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="b5fb5-138">In this section, you configure and test Azure AD single sign-on with Wikispaces based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b5fb5-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Wikispaces je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Wikispaces is to a user in Azure AD.</span></span> <span data-ttu-id="b5fb5-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Wikispaces musí navázat.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-140">In other words, a link relationship between an Azure AD user and the related user in Wikispaces needs to be established.</span></span>

<span data-ttu-id="b5fb5-141">V Wikispaces, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-141">In Wikispaces, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="b5fb5-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Wikispaces, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="b5fb5-142">To configure and test Azure AD single sign-on with Wikispaces, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b5fb5-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b5fb5-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b5fb5-145">**[Vytvoření zkušebního uživatele Wikispaces](#creating-a-wikispaces-test-user)**  – Pokud chcete mít protějšek Britta Simon v Wikispaces propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-145">**[Creating a Wikispaces test user](#creating-a-wikispaces-test-user)** - to have a counterpart of Britta Simon in Wikispaces that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b5fb5-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b5fb5-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b5fb5-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b5fb5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b5fb5-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Wikispaces.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Wikispaces application.</span></span>

<span data-ttu-id="b5fb5-150">**Ke konfiguraci Azure AD jednotné přihlašování s Wikispaces, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b5fb5-150">**To configure Azure AD single sign-on with Wikispaces, perform the following steps:**</span></span>

1. <span data-ttu-id="b5fb5-151">Na portálu Azure na **Wikispaces** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-151">In the Azure portal, on the **Wikispaces** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b5fb5-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_samlbase.png)

3. <span data-ttu-id="b5fb5-155">Na **Wikispaces domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b5fb5-155">On the **Wikispaces Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_url.png)

    <span data-ttu-id="b5fb5-157">a.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-157">a.</span></span> <span data-ttu-id="b5fb5-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.wikispaces.net`</span><span class="sxs-lookup"><span data-stu-id="b5fb5-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.wikispaces.net`</span></span>

    <span data-ttu-id="b5fb5-159">b.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-159">b.</span></span> <span data-ttu-id="b5fb5-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://session.wikispaces.net/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="b5fb5-160">In the **Identifier** textbox, type a URL using the following pattern: `https://session.wikispaces.net/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b5fb5-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-161">These values are not real.</span></span> <span data-ttu-id="b5fb5-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b5fb5-163">Obraťte se na [tým podpory Wikispaces klienta](https://www.wikispaces.com/site/help) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-163">Contact [Wikispaces Client support team](https://www.wikispaces.com/site/help) to get these values.</span></span> 

4. <span data-ttu-id="b5fb5-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_certificate.png) 

5. <span data-ttu-id="b5fb5-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wikispaces-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b5fb5-168">Konfigurace jednotného přihlašování na **Wikispaces** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Wikispaces](https://www.wikispaces.com/site/help).</span><span class="sxs-lookup"><span data-stu-id="b5fb5-168">To configure single sign-on on **Wikispaces** side, you need to send the downloaded **Metadata XML** to [Wikispaces support team](https://www.wikispaces.com/site/help).</span></span> <span data-ttu-id="b5fb5-169">Zobrazí se oznámení a také konfigurace byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-169">You will get a notification as soon as the configuration has been completed.</span></span>

> [!TIP]
> <span data-ttu-id="b5fb5-170">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="b5fb5-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b5fb5-171">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b5fb5-172">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b5fb5-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b5fb5-173">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b5fb5-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="b5fb5-174">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b5fb5-176">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b5fb5-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b5fb5-177">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b5fb5-179">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b5fb5-181">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b5fb5-183">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b5fb5-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b5fb5-185">a.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-185">a.</span></span> <span data-ttu-id="b5fb5-186">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b5fb5-187">b.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-187">b.</span></span> <span data-ttu-id="b5fb5-188">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b5fb5-189">c.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-189">c.</span></span> <span data-ttu-id="b5fb5-190">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b5fb5-191">d.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-191">d.</span></span> <span data-ttu-id="b5fb5-192">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-192">Click **Create**.</span></span>
 
### <a name="creating-a-wikispaces-test-user"></a><span data-ttu-id="b5fb5-193">Vytvoření zkušebního uživatele Wikispaces</span><span class="sxs-lookup"><span data-stu-id="b5fb5-193">Creating a Wikispaces test user</span></span>

<span data-ttu-id="b5fb5-194">Pokud chcete povolit uživatelům Azure AD přihlášení k Wikispaces, musí být zřízená do Wikispaces.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-194">In order to enable Azure AD users to log in to Wikispaces, they must be provisioned into Wikispaces.</span></span> <span data-ttu-id="b5fb5-195">V případě Wikispaces zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-195">In the case of Wikispaces, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a><span data-ttu-id="b5fb5-196">Ke zřízení uživatelských účtů, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b5fb5-196">To provision a user accounts, perform the following steps:</span></span>
1. <span data-ttu-id="b5fb5-197">Přihlaste se k vaší **Wikispaces** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-197">Log in to your **Wikispaces** company site as an administrator.</span></span>

2. <span data-ttu-id="b5fb5-198">Přejděte na **členy**.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-198">Go to **Members**.</span></span>
   
    <span data-ttu-id="b5fb5-199">![Členové](./media/active-directory-saas-wikispaces-tutorial/ic787193.png "členy")</span><span class="sxs-lookup"><span data-stu-id="b5fb5-199">![Members](./media/active-directory-saas-wikispaces-tutorial/ic787193.png "Members")</span></span>

3. <span data-ttu-id="b5fb5-200">Klikněte **pozvat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-200">Click the **Invite People**.</span></span>
   
    <span data-ttu-id="b5fb5-201">![Pozvěte lidi, kteří](./media/active-directory-saas-wikispaces-tutorial/ic787194.png "pozvat uživatele")</span><span class="sxs-lookup"><span data-stu-id="b5fb5-201">![Invite People](./media/active-directory-saas-wikispaces-tutorial/ic787194.png "Invite People")</span></span>

4. <span data-ttu-id="b5fb5-202">V **pozvat osoby** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b5fb5-202">In the **Invite People** section, perform the following steps:</span></span>
   
    <span data-ttu-id="b5fb5-203">![Pozvěte lidi, kteří](./media/active-directory-saas-wikispaces-tutorial/ic787208.png "pozvat uživatele")</span><span class="sxs-lookup"><span data-stu-id="b5fb5-203">![Invite People](./media/active-directory-saas-wikispaces-tutorial/ic787208.png "Invite People")</span></span>
   
    <span data-ttu-id="b5fb5-204">a.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-204">a.</span></span> <span data-ttu-id="b5fb5-205">Typ **uživatelská jména nebo e-mailovou adresu** platného účtu AAD chcete mají být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-205">Type the **Usernames or Email Address** of a valid AAD account you want to provision into the related textboxes.</span></span>
   
    <span data-ttu-id="b5fb5-206">b.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-206">b.</span></span> <span data-ttu-id="b5fb5-207">Klikněte na tlačítko **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-207">Click **Send**.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="b5fb5-208">Držitel účtu Azure Active Directory obdrží e-mail, včetně odkaz pro potvrzení účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-208">The Azure Active Directory account holder receives an email including a link to confirm the account before it becomes active.</span></span>
    
> [!NOTE]
> <span data-ttu-id="b5fb5-209">Můžete použít všechny ostatní Wikispaces uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Wikispaces zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-209">You can use any other Wikispaces user account creation tools or APIs provided by Wikispaces to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b5fb5-210">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b5fb5-210">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b5fb5-211">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Wikispaces.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-211">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Wikispaces.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b5fb5-213">**Pokud chcete přiřadit Britta Simon Wikispaces, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b5fb5-213">**To assign Britta Simon to Wikispaces, perform the following steps:**</span></span>

1. <span data-ttu-id="b5fb5-214">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-214">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b5fb5-216">V seznamu aplikací vyberte **Wikispaces**.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-216">In the applications list, select **Wikispaces**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_app.png) 

3. <span data-ttu-id="b5fb5-218">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-218">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b5fb5-220">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-220">Click **Add** button.</span></span> <span data-ttu-id="b5fb5-221">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b5fb5-223">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b5fb5-224">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b5fb5-225">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b5fb5-226">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b5fb5-226">Testing single sign-on</span></span>

<span data-ttu-id="b5fb5-227">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-227">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b5fb5-228">Když kliknete na dlaždici Wikispaces na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Wikispaces.</span><span class="sxs-lookup"><span data-stu-id="b5fb5-228">When you click the Wikispaces tile in the Access Panel, you should get automatically signed-on to your Wikispaces application.</span></span>
<span data-ttu-id="b5fb5-229">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b5fb5-229">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5fb5-230">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b5fb5-230">Additional resources</span></span>

* [<span data-ttu-id="b5fb5-231">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b5fb5-231">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b5fb5-232">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b5fb5-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_203.png

