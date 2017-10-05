---
title: 'Kurz: Azure Active Directory integrace s Thoughtworks Mingle | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Thoughtworks Mingle."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 268ae5affb88a718f68c08daa94fe7aba4a99c11
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a><span data-ttu-id="c0261-103">Kurz: Azure Active Directory integrace s Thoughtworks Mingle</span><span class="sxs-lookup"><span data-stu-id="c0261-103">Tutorial: Azure Active Directory integration with Thoughtworks Mingle</span></span>

<span data-ttu-id="c0261-104">V tomto kurzu zjistěte, jak integrovat Thoughtworks Mingle s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c0261-104">In this tutorial, you learn how to integrate Thoughtworks Mingle with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c0261-105">Integrace Thoughtworks Mingle s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c0261-105">Integrating Thoughtworks Mingle with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c0261-106">Můžete řídit ve službě Azure AD, který má přístup k Thoughtworks Mingle</span><span class="sxs-lookup"><span data-stu-id="c0261-106">You can control in Azure AD who has access to Thoughtworks Mingle</span></span>
- <span data-ttu-id="c0261-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Thoughtworks Mingle (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c0261-107">You can enable your users to automatically get signed-on to Thoughtworks Mingle (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c0261-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c0261-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c0261-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c0261-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c0261-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c0261-110">Prerequisites</span></span>

<span data-ttu-id="c0261-111">Konfigurace integrace Azure AD s Thoughtworks Mingle, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="c0261-111">To configure Azure AD integration with Thoughtworks Mingle, you need the following items:</span></span>

- <span data-ttu-id="c0261-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c0261-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c0261-113">Thoughtworks Mingle jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c0261-113">A Thoughtworks Mingle single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c0261-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c0261-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c0261-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c0261-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c0261-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c0261-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c0261-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c0261-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c0261-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c0261-118">Scenario description</span></span>
<span data-ttu-id="c0261-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c0261-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c0261-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c0261-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c0261-121">Přidání Thoughtworks Mingle z Galerie</span><span class="sxs-lookup"><span data-stu-id="c0261-121">Adding Thoughtworks Mingle from the gallery</span></span>
2. <span data-ttu-id="c0261-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c0261-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-thoughtworks-mingle-from-the-gallery"></a><span data-ttu-id="c0261-123">Přidání Thoughtworks Mingle z Galerie</span><span class="sxs-lookup"><span data-stu-id="c0261-123">Adding Thoughtworks Mingle from the gallery</span></span>
<span data-ttu-id="c0261-124">Chcete-li nakonfigurovat integraci Thoughtworks Mingle do služby Azure AD, přidejte Thoughtworks Mingle z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="c0261-124">To configure the integration of Thoughtworks Mingle into Azure AD, you need to add Thoughtworks Mingle from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c0261-125">**Pokud chcete přidat Thoughtworks Mingle z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c0261-125">**To add Thoughtworks Mingle from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c0261-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c0261-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="c0261-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c0261-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c0261-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c0261-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="c0261-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c0261-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="c0261-133">Do vyhledávacího pole zadejte **Thoughtworks Mingle**, vyberte **Thoughtworks Mingle** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c0261-133">In the search box, type **Thoughtworks Mingle**, select **Thoughtworks Mingle** from result panel then click **Add** button to add the application.</span></span>

    ![Thoughtworks Mingle v seznamu výsledků](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c0261-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c0261-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="c0261-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Thoughtworks Mingle podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="c0261-136">In this section, you configure and test Azure AD single sign-on with Thoughtworks Mingle based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c0261-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Thoughtworks Mingle je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c0261-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Thoughtworks Mingle is to a user in Azure AD.</span></span> <span data-ttu-id="c0261-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Thoughtworks Mingle musí navázat.</span><span class="sxs-lookup"><span data-stu-id="c0261-138">In other words, a link relationship between an Azure AD user and the related user in Thoughtworks Mingle needs to be established.</span></span>

<span data-ttu-id="c0261-139">V Thoughtworks Mingle, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="c0261-139">In Thoughtworks Mingle, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c0261-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Thoughtworks Mingle, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="c0261-140">To configure and test Azure AD single sign-on with Thoughtworks Mingle, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c0261-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="c0261-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c0261-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c0261-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c0261-143">**[Vytvoření zkušebního uživatele Thoughtworks Mingle](#create-a-thoughtworks-mingle-test-user)**  – Pokud chcete mít protějšek Britta Simon v Thoughtworks Mingle propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="c0261-143">**[Create a Thoughtworks Mingle test user](#create-a-thoughtworks-mingle-test-user)** - to have a counterpart of Britta Simon in Thoughtworks Mingle that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c0261-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c0261-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c0261-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c0261-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c0261-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c0261-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c0261-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Thoughtworks Mingle.</span><span class="sxs-lookup"><span data-stu-id="c0261-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Thoughtworks Mingle application.</span></span>

<span data-ttu-id="c0261-148">**Ke konfiguraci Azure AD jednotné přihlašování s Thoughtworks Mingle, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c0261-148">**To configure Azure AD single sign-on with Thoughtworks Mingle, perform the following steps:**</span></span>

1. <span data-ttu-id="c0261-149">Na portálu Azure na **Thoughtworks Mingle** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c0261-149">In the Azure portal, on the **Thoughtworks Mingle** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c0261-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c0261-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_samlbase.png)

3. <span data-ttu-id="c0261-153">Na **Thoughtworks Mingle domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c0261-153">On the **Thoughtworks Mingle Domain and URLs** section, perform the following steps:</span></span>

    ![Thoughtworks Mingle domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_url.png)

    <span data-ttu-id="c0261-155">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.mingle.thoughtworks.com`</span><span class="sxs-lookup"><span data-stu-id="c0261-155">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.mingle.thoughtworks.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c0261-156">Hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="c0261-156">The value is not real.</span></span> <span data-ttu-id="c0261-157">Aktualizujte hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c0261-157">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="c0261-158">Obraťte se na [tým podpory klienta Mingle Thoughtworks](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) k získání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c0261-158">Contact [Thoughtworks Mingle Client support team](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) to get the value.</span></span> 
 
4. <span data-ttu-id="c0261-159">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c0261-159">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_certificate.png) 

5. <span data-ttu-id="c0261-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c0261-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c0261-163">Přihlaste se k vaší **Thoughtworks Mingle** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="c0261-163">Log in to your **Thoughtworks Mingle** company site as administrator.</span></span>

7. <span data-ttu-id="c0261-164">Klikněte **správce** kartě a potom klikněte na **Konfigurace jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="c0261-164">Click the **Admin** tab, and then, click **SSO Config**.</span></span>
   
    <span data-ttu-id="c0261-165">![Karta správce](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785157.png "Konfigurace jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="c0261-165">![Admin tab](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785157.png "SSO Config")</span></span>

8. <span data-ttu-id="c0261-166">V **Konfigurace jednotného přihlašování k** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c0261-166">In the **SSO Config** section, perform the following steps:</span></span>
   
    <span data-ttu-id="c0261-167">![Konfigurace jednotného přihlašování k](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785158.png "Konfigurace jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="c0261-167">![SSO Config](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785158.png "SSO Config")</span></span>
    
    <span data-ttu-id="c0261-168">a.</span><span class="sxs-lookup"><span data-stu-id="c0261-168">a.</span></span> <span data-ttu-id="c0261-169">Chcete-li nahrát soubor metadat, klikněte na tlačítko **zvolte soubor**.</span><span class="sxs-lookup"><span data-stu-id="c0261-169">To upload the metadata file, click **Choose file**.</span></span> 

    <span data-ttu-id="c0261-170">b.</span><span class="sxs-lookup"><span data-stu-id="c0261-170">b.</span></span> <span data-ttu-id="c0261-171">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="c0261-171">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="c0261-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="c0261-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c0261-173">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="c0261-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c0261-174">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c0261-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c0261-175">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c0261-175">Create an Azure AD test user</span></span>
<span data-ttu-id="c0261-176">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c0261-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="c0261-178">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c0261-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c0261-179">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c0261-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c0261-181">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c0261-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c0261-183">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c0261-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c0261-185">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c0261-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Dialogové okno uživatele](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c0261-187">a.</span><span class="sxs-lookup"><span data-stu-id="c0261-187">a.</span></span> <span data-ttu-id="c0261-188">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c0261-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c0261-189">b.</span><span class="sxs-lookup"><span data-stu-id="c0261-189">b.</span></span> <span data-ttu-id="c0261-190">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c0261-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c0261-191">c.</span><span class="sxs-lookup"><span data-stu-id="c0261-191">c.</span></span> <span data-ttu-id="c0261-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c0261-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c0261-193">d.</span><span class="sxs-lookup"><span data-stu-id="c0261-193">d.</span></span> <span data-ttu-id="c0261-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c0261-194">Click **Create**.</span></span>
 
### <a name="create-a-thoughtworks-mingle-test-user"></a><span data-ttu-id="c0261-195">Vytvoření zkušebního uživatele Thoughtworks Mingle</span><span class="sxs-lookup"><span data-stu-id="c0261-195">Create a Thoughtworks Mingle test user</span></span>

<span data-ttu-id="c0261-196">Azure AD uživatelé moci přihlásit musí být zřízená do Thoughtworks Mingle aplikace pomocí jejich názvy uživatele Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c0261-196">For Azure AD users to be able to sign in, they must be provisioned to the Thoughtworks Mingle application using their Azure Active Directory user names.</span></span> <span data-ttu-id="c0261-197">V případě Thoughtworks Mingle zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="c0261-197">In the case of Thoughtworks Mingle, provisioning is a manual task.</span></span>

<span data-ttu-id="c0261-198">**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c0261-198">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="c0261-199">Přihlaste se k serveru vaší společnosti Thoughtworks Mingle jako správce.</span><span class="sxs-lookup"><span data-stu-id="c0261-199">Log in to your Thoughtworks Mingle company site as administrator.</span></span>

2. <span data-ttu-id="c0261-200">Klikněte na tlačítko **profil**.</span><span class="sxs-lookup"><span data-stu-id="c0261-200">Click **Profile**.</span></span>
   
    <span data-ttu-id="c0261-201">![Svůj první projekt](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785160.png "prvního projektu")</span><span class="sxs-lookup"><span data-stu-id="c0261-201">![Your First Project](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785160.png "Your First Project")</span></span>

3. <span data-ttu-id="c0261-202">Klikněte **správce** a pak klikněte **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c0261-202">Click the **Admin** tab, and then click **Users**.</span></span>
   
    <span data-ttu-id="c0261-203">![Uživatelé](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785161.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="c0261-203">![Users](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785161.png "Users")</span></span>

4. <span data-ttu-id="c0261-204">Klikněte na tlačítko **nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="c0261-204">Click **New User**.</span></span>
   
    <span data-ttu-id="c0261-205">![Nový uživatel](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785162.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="c0261-205">![New User](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785162.png "New User")</span></span>

5. <span data-ttu-id="c0261-206">Na **nového uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c0261-206">On the **New User** dialog page, perform the following steps:</span></span>
   
    <span data-ttu-id="c0261-207">![Dialogové okno Nový uživatel](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785163.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="c0261-207">![New User dialog](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785163.png "New User")</span></span>  
 
    <span data-ttu-id="c0261-208">a.</span><span class="sxs-lookup"><span data-stu-id="c0261-208">a.</span></span> <span data-ttu-id="c0261-209">Typ **přihlašovací jméno**, **zobrazovaný název**, **zvolte heslo**, **potvrzení hesla** platný Azure AD účtu chcete mají být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="c0261-209">Type the **Sign-in name**, **Display name**, **Choose password**, **Confirm password** of a valid Azure AD account you want to provision into the related textboxes.</span></span> 

    <span data-ttu-id="c0261-210">b.</span><span class="sxs-lookup"><span data-stu-id="c0261-210">b.</span></span> <span data-ttu-id="c0261-211">Jako **typ uživatele**, vyberte **úplné uživatelské**.</span><span class="sxs-lookup"><span data-stu-id="c0261-211">As **User type**, select **Full user**.</span></span>

    <span data-ttu-id="c0261-212">c.</span><span class="sxs-lookup"><span data-stu-id="c0261-212">c.</span></span> <span data-ttu-id="c0261-213">Klikněte na tlačítko **vytvořit tento profil**.</span><span class="sxs-lookup"><span data-stu-id="c0261-213">Click **Create This Profile**.</span></span>

>[!NOTE]
><span data-ttu-id="c0261-214">Můžete použít všechny ostatní Thoughtworks Mingle uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Thoughtworks Mingle zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="c0261-214">You can use any other Thoughtworks Mingle user account creation tools or APIs provided by Thoughtworks Mingle to provision AAD user accounts.</span></span>
> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="c0261-215">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c0261-215">Assign the Azure AD test user</span></span>

<span data-ttu-id="c0261-216">V této části povolíte Britta Simon k používání Azure jednotné přihlašování v Thoughtworks Mingle udělení přístupu.</span><span class="sxs-lookup"><span data-stu-id="c0261-216">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Thoughtworks Mingle.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="c0261-218">**Chcete-li Thoughtworks Mingle přiřadit Britta Simon, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c0261-218">**To assign Britta Simon to Thoughtworks Mingle, perform the following steps:**</span></span>

1. <span data-ttu-id="c0261-219">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c0261-219">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c0261-221">V seznamu aplikací vyberte **Thoughtworks Mingle**.</span><span class="sxs-lookup"><span data-stu-id="c0261-221">In the applications list, select **Thoughtworks Mingle**.</span></span>

    ![Odkaz Thoughtworks Mingle v seznamu aplikací](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_app.png) 

3. <span data-ttu-id="c0261-223">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c0261-223">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202] 

4. <span data-ttu-id="c0261-225">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c0261-225">Click **Add** button.</span></span> <span data-ttu-id="c0261-226">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c0261-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="c0261-228">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c0261-228">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c0261-229">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c0261-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c0261-230">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c0261-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c0261-231">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c0261-231">Test single sign-on</span></span>

<span data-ttu-id="c0261-232">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c0261-232">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c0261-233">Když kliknete na dlaždici Thoughtworks Mingle na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Thoughtworks Mingle.</span><span class="sxs-lookup"><span data-stu-id="c0261-233">When you click the Thoughtworks Mingle tile in the Access Panel, you should get automatically signed-on to your Thoughtworks Mingle application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c0261-234">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c0261-234">Additional resources</span></span>

* [<span data-ttu-id="c0261-235">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c0261-235">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c0261-236">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c0261-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_203.png

