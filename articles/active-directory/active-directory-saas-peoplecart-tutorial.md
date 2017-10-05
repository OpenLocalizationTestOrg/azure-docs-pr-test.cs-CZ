---
title: 'Kurz: Azure Active Directory integrace s Peoplecart | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Peoplecart."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c83b5d9d-2638-4689-b9f0-f56a9159e7a0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: b83a1621263cac0b23bbd35a49fda213d2e4271a
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-peoplecart"></a><span data-ttu-id="e814b-103">Kurz: Azure Active Directory integrace s Peoplecart</span><span class="sxs-lookup"><span data-stu-id="e814b-103">Tutorial: Azure Active Directory integration with Peoplecart</span></span>

<span data-ttu-id="e814b-104">V tomto kurzu zjistěte, jak integrovat Peoplecart s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e814b-104">In this tutorial, you learn how to integrate Peoplecart with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e814b-105">Integrace Peoplecart s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e814b-105">Integrating Peoplecart with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e814b-106">Můžete řídit ve službě Azure AD, který má přístup k Peoplecart</span><span class="sxs-lookup"><span data-stu-id="e814b-106">You can control in Azure AD who has access to Peoplecart</span></span>
- <span data-ttu-id="e814b-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Peoplecart (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e814b-107">You can enable your users to automatically get signed-on to Peoplecart (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e814b-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e814b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e814b-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e814b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e814b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e814b-110">Prerequisites</span></span>

<span data-ttu-id="e814b-111">Konfigurace integrace Azure AD s Peoplecart, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="e814b-111">To configure Azure AD integration with Peoplecart, you need the following items:</span></span>

- <span data-ttu-id="e814b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e814b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e814b-113">Peoplecart jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e814b-113">A Peoplecart single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e814b-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e814b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e814b-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e814b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e814b-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e814b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e814b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e814b-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e814b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e814b-118">Scenario description</span></span>
<span data-ttu-id="e814b-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e814b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e814b-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e814b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e814b-121">Přidání Peoplecart z Galerie</span><span class="sxs-lookup"><span data-stu-id="e814b-121">Adding Peoplecart from the gallery</span></span>
2. <span data-ttu-id="e814b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e814b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-peoplecart-from-the-gallery"></a><span data-ttu-id="e814b-123">Přidání Peoplecart z Galerie</span><span class="sxs-lookup"><span data-stu-id="e814b-123">Adding Peoplecart from the gallery</span></span>
<span data-ttu-id="e814b-124">Při konfiguraci integrace Peoplecart do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Peoplecart z galerie.</span><span class="sxs-lookup"><span data-stu-id="e814b-124">To configure the integration of Peoplecart into Azure AD, you need to add Peoplecart from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e814b-125">**Pokud chcete přidat Peoplecart z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e814b-125">**To add Peoplecart from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e814b-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e814b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="e814b-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e814b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e814b-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e814b-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="e814b-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e814b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="e814b-133">Do vyhledávacího pole zadejte **Peoplecart**, vyberte **Peoplecart** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e814b-133">In the search box, type **Peoplecart**, select **Peoplecart** from result panel then click **Add** button to add the application.</span></span>

    ![Peoplecart v seznamu výsledků](./media/active-directory-saas-peoplecart-tutorial/tutorial_peoplecart_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="e814b-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e814b-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="e814b-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Peoplecart podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="e814b-136">In this section, you configure and test Azure AD single sign-on with Peoplecart based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e814b-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Peoplecart je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e814b-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Peoplecart is to a user in Azure AD.</span></span> <span data-ttu-id="e814b-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Peoplecart musí navázat.</span><span class="sxs-lookup"><span data-stu-id="e814b-138">In other words, a link relationship between an Azure AD user and the related user in Peoplecart needs to be established.</span></span>

<span data-ttu-id="e814b-139">V Peoplecart, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="e814b-139">In Peoplecart, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e814b-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Peoplecart, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="e814b-140">To configure and test Azure AD single sign-on with Peoplecart, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e814b-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="e814b-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e814b-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e814b-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e814b-143">**[Vytvoření zkušebního uživatele Peoplecart](#create-a-peoplecart-test-user)**  – Pokud chcete mít protějšek Britta Simon v Peoplecart propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="e814b-143">**[Create a Peoplecart test user](#create-a-peoplecart-test-user)** - to have a counterpart of Britta Simon in Peoplecart that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e814b-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e814b-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e814b-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e814b-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="e814b-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e814b-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="e814b-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Peoplecart.</span><span class="sxs-lookup"><span data-stu-id="e814b-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Peoplecart application.</span></span>

<span data-ttu-id="e814b-148">**Ke konfiguraci Azure AD jednotné přihlašování s Peoplecart, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e814b-148">**To configure Azure AD single sign-on with Peoplecart, perform the following steps:**</span></span>

1. <span data-ttu-id="e814b-149">Na portálu Azure na **Peoplecart** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e814b-149">In the Azure portal, on the **Peoplecart** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e814b-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e814b-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-peoplecart-tutorial/tutorial_peoplecart_samlbase.png)

3. <span data-ttu-id="e814b-153">Na **Peoplecart domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e814b-153">On the **Peoplecart Domain and URLs** section, perform the following steps:</span></span>

    ![Peoplecart domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-peoplecart-tutorial/tutorial_peoplecart_url.png)

    <span data-ttu-id="e814b-155">a.</span><span class="sxs-lookup"><span data-stu-id="e814b-155">a.</span></span> <span data-ttu-id="e814b-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenantname>.peoplecart.com/SignIn.aspx`</span><span class="sxs-lookup"><span data-stu-id="e814b-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenantname>.peoplecart.com/SignIn.aspx`</span></span>

    <span data-ttu-id="e814b-157">b.</span><span class="sxs-lookup"><span data-stu-id="e814b-157">b.</span></span> <span data-ttu-id="e814b-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenantname>.peoplecart.com`</span><span class="sxs-lookup"><span data-stu-id="e814b-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenantname>.peoplecart.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e814b-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="e814b-159">These values are not real.</span></span> <span data-ttu-id="e814b-160">Tyto hodnoty aktualizujte s skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="e814b-160">Update these values with the actual Sign-On URL, and Identifier.</span></span> <span data-ttu-id="e814b-161">Obraťte se na [tým podpory Peoplecart klienta](https://peoplecart.com/ContactUs.aspx) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="e814b-161">Contact [Peoplecart Client support team](https://peoplecart.com/ContactUs.aspx) to get these values.</span></span> 
 
4. <span data-ttu-id="e814b-162">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="e814b-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-peoplecart-tutorial/tutorial_peoplecart_certificate.png) 

5. <span data-ttu-id="e814b-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e814b-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-peoplecart-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e814b-166">Na **Peoplecart konfigurace** klikněte na tlačítko **konfigurace Peoplecart** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="e814b-166">On the **Peoplecart Configuration** section, click **Configure Peoplecart** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e814b-167">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="e814b-167">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace Peoplecart](./media/active-directory-saas-peoplecart-tutorial/tutorial_peoplecart_configure.png) 

7. <span data-ttu-id="e814b-169">Konfigurace jednotného přihlašování na **Peoplecart** straně, budete muset odeslat stažené **soubor XML s metadaty** a **SAML jeden přihlašování adresa URL služby** k [Peoplecart tým podpory](https://peoplecart.com/ContactUs.aspx).</span><span class="sxs-lookup"><span data-stu-id="e814b-169">To configure single sign-on on **Peoplecart** side, you need to send the downloaded **Metadata XML** and **SAML Single Sign-On Service URL** to [Peoplecart support team](https://peoplecart.com/ContactUs.aspx).</span></span> <span data-ttu-id="e814b-170">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="e814b-170">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="e814b-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="e814b-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e814b-172">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="e814b-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e814b-173">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e814b-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="e814b-174">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e814b-174">Create an Azure AD test user</span></span>
<span data-ttu-id="e814b-175">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e814b-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="e814b-177">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e814b-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e814b-178">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e814b-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-peoplecart-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e814b-180">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e814b-180">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-peoplecart-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e814b-182">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e814b-182">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-peoplecart-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e814b-184">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e814b-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Dialogové okno uživatele](./media/active-directory-saas-peoplecart-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e814b-186">a.</span><span class="sxs-lookup"><span data-stu-id="e814b-186">a.</span></span> <span data-ttu-id="e814b-187">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e814b-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e814b-188">b.</span><span class="sxs-lookup"><span data-stu-id="e814b-188">b.</span></span> <span data-ttu-id="e814b-189">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e814b-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e814b-190">c.</span><span class="sxs-lookup"><span data-stu-id="e814b-190">c.</span></span> <span data-ttu-id="e814b-191">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e814b-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e814b-192">d.</span><span class="sxs-lookup"><span data-stu-id="e814b-192">d.</span></span> <span data-ttu-id="e814b-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e814b-193">Click **Create**.</span></span>
 
### <a name="create-a-peoplecart-test-user"></a><span data-ttu-id="e814b-194">Vytvoření zkušebního uživatele Peoplecart</span><span class="sxs-lookup"><span data-stu-id="e814b-194">Create a Peoplecart test user</span></span>

<span data-ttu-id="e814b-195">V této části vytvoříte volal Britta Simon v Peoplecart uživatele.</span><span class="sxs-lookup"><span data-stu-id="e814b-195">In this section, you create a user called Britta Simon in Peoplecart.</span></span> <span data-ttu-id="e814b-196">Práce s [tým podpory Peoplecart](https://peoplecart.com/ContactUs.aspx) přidat uživatele do Peoplecart platformy.</span><span class="sxs-lookup"><span data-stu-id="e814b-196">Work with [Peoplecart support team](https://peoplecart.com/ContactUs.aspx) to add the users in the Peoplecart platform.</span></span> <span data-ttu-id="e814b-197">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e814b-197">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="e814b-198">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e814b-198">Assign the Azure AD test user</span></span>

<span data-ttu-id="e814b-199">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Peoplecart.</span><span class="sxs-lookup"><span data-stu-id="e814b-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Peoplecart.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="e814b-201">**Pokud chcete přiřadit Britta Simon Peoplecart, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e814b-201">**To assign Britta Simon to Peoplecart, perform the following steps:**</span></span>

1. <span data-ttu-id="e814b-202">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e814b-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e814b-204">V seznamu aplikací vyberte **Peoplecart**.</span><span class="sxs-lookup"><span data-stu-id="e814b-204">In the applications list, select **Peoplecart**.</span></span>

    ![V seznamu aplikací na Peoplecart odkaz](./media/active-directory-saas-peoplecart-tutorial/tutorial_peoplecart_app.png) 

3. <span data-ttu-id="e814b-206">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e814b-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202] 

4. <span data-ttu-id="e814b-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e814b-208">Click **Add** button.</span></span> <span data-ttu-id="e814b-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e814b-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="e814b-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e814b-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e814b-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e814b-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e814b-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e814b-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="e814b-214">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e814b-214">Test single sign-on</span></span>

<span data-ttu-id="e814b-215">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="e814b-215">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="e814b-216">Když kliknete na dlaždici Peoplecart na přístupovém panelu, měli byste obdržet přihlašovací stránku Peoplecart aplikace.</span><span class="sxs-lookup"><span data-stu-id="e814b-216">When you click the Peoplecart tile in the Access Panel, you should get login page of Peoplecart application.</span></span>
<span data-ttu-id="e814b-217">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e814b-217">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e814b-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e814b-218">Additional resources</span></span>

* [<span data-ttu-id="e814b-219">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e814b-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e814b-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e814b-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-peoplecart-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-peoplecart-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-peoplecart-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-peoplecart-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-peoplecart-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-peoplecart-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-peoplecart-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-peoplecart-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-peoplecart-tutorial/tutorial_general_203.png

