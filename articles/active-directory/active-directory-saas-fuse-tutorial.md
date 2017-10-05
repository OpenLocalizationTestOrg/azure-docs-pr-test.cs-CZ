---
title: 'Kurz: Azure Active Directory integrace s pojistka | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a pojistka."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5ef34f58-863a-4b37-875c-e8efa3e18bb3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 9a91e22faced9e126043bebefd85c307dbdf933d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fuse"></a><span data-ttu-id="3cae4-103">Kurz: Azure Active Directory integrace s pojistka</span><span class="sxs-lookup"><span data-stu-id="3cae4-103">Tutorial: Azure Active Directory integration with Fuse</span></span>

<span data-ttu-id="3cae4-104">V tomto kurzu zjistěte, jak integrovat pojistka s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3cae4-104">In this tutorial, you learn how to integrate Fuse with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3cae4-105">Integrace pojistka s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3cae4-105">Integrating Fuse with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3cae4-106">Ve službě Azure AD, který má přístup k pojistku můžete řídit.</span><span class="sxs-lookup"><span data-stu-id="3cae4-106">You can control in Azure AD who has access to Fuse.</span></span>
- <span data-ttu-id="3cae4-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k pojistka (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3cae4-107">You can enable your users to automatically get signed-on to Fuse (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="3cae4-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3cae4-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="3cae4-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3cae4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3cae4-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3cae4-110">Prerequisites</span></span>

<span data-ttu-id="3cae4-111">Konfigurace integrace Azure AD s pojistka, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="3cae4-111">To configure Azure AD integration with Fuse, you need the following items:</span></span>

- <span data-ttu-id="3cae4-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3cae4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3cae4-113">Pojistka jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3cae4-113">A Fuse single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3cae4-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3cae4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3cae4-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3cae4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3cae4-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3cae4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3cae4-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3cae4-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3cae4-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3cae4-118">Scenario description</span></span>
<span data-ttu-id="3cae4-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3cae4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3cae4-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3cae4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3cae4-121">Přidat pojistka z Galerie</span><span class="sxs-lookup"><span data-stu-id="3cae4-121">Add Fuse from the gallery</span></span>
2. <span data-ttu-id="3cae4-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3cae4-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-fuse-from-the-gallery"></a><span data-ttu-id="3cae4-123">Přidat pojistka z Galerie</span><span class="sxs-lookup"><span data-stu-id="3cae4-123">Add Fuse from the gallery</span></span>
<span data-ttu-id="3cae4-124">Při konfiguraci integrace pojistka do služby Azure AD potřebujete přidat pojistka z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="3cae4-124">To configure the integration of Fuse into Azure AD, you need to add Fuse from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3cae4-125">**Pokud chcete přidat pojistka z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3cae4-125">**To add Fuse from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3cae4-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3cae4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="3cae4-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3cae4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3cae4-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3cae4-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="3cae4-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3cae4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="3cae4-133">Do vyhledávacího pole zadejte **pojistka**, vyberte **pojistka** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3cae4-133">In the search box, type **Fuse**, select **Fuse** from result panel then click **Add** button to add the application.</span></span>

    ![Pojistka v seznamu výsledků](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="3cae4-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3cae4-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="3cae4-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s pojistka podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="3cae4-136">In this section, you configure and test Azure AD single sign-on with Fuse based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3cae4-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v pojistka je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3cae4-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Fuse is to a user in Azure AD.</span></span> <span data-ttu-id="3cae4-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v pojistka je potřeba vytvořit.</span><span class="sxs-lookup"><span data-stu-id="3cae4-138">In other words, a link relationship between an Azure AD user and the related user in Fuse needs to be established.</span></span>

<span data-ttu-id="3cae4-139">V pojistka, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="3cae4-139">In Fuse, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3cae4-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s pojistka, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="3cae4-140">To configure and test Azure AD single sign-on with Fuse, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3cae4-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="3cae4-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3cae4-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3cae4-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3cae4-143">**[Vytvoření zkušebního uživatele pojistka](#create-a-fuse-test-user)**  – Pokud chcete mít protějšek Britta Simon v pojistka propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="3cae4-143">**[Create a Fuse test user](#create-a-fuse-test-user)** - to have a counterpart of Britta Simon in Fuse that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3cae4-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3cae4-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3cae4-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3cae4-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="3cae4-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3cae4-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="3cae4-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci pojistka.</span><span class="sxs-lookup"><span data-stu-id="3cae4-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Fuse application.</span></span>

<span data-ttu-id="3cae4-148">**Ke konfiguraci Azure AD jednotné přihlašování s pojistka, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3cae4-148">**To configure Azure AD single sign-on with Fuse, perform the following steps:**</span></span>

1. <span data-ttu-id="3cae4-149">Na portálu Azure na **pojistka** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3cae4-149">In the Azure portal, on the **Fuse** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="3cae4-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3cae4-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_samlbase.png)

3. <span data-ttu-id="3cae4-153">Na **pojistka domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3cae4-153">On the **Fuse Domain and URLs** section, perform the following steps:</span></span>

    ![Pojistka adresy URL jeden přihlašování informace o doméně a](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_url.png)
    
    <span data-ttu-id="3cae4-155">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenant name>.fusion-universal.com/`</span><span class="sxs-lookup"><span data-stu-id="3cae4-155">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant name>.fusion-universal.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3cae4-156">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="3cae4-156">This value is not real.</span></span> <span data-ttu-id="3cae4-157">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3cae4-157">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="3cae4-158">Obraťte se na [tým podpory pojistka klienta](mailto:support@fusion-universal.com) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3cae4-158">Contact [Fuse Client support team](mailto:support@fusion-universal.com) to get this value.</span></span> 
 
4. <span data-ttu-id="3cae4-159">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Raw)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="3cae4-159">On the **SAML Signing Certificate** section, click **Certificate (Raw)** and then save the certificate file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_certificate.png) 

5. <span data-ttu-id="3cae4-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3cae4-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-fuse-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3cae4-163">Na **pojistka konfigurace** klikněte na tlačítko **konfigurace pojistka** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="3cae4-163">On the **Fuse Configuration** section, click **Configure Fuse** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3cae4-164">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="3cae4-164">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace pojistka](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_configure.png) 

7. <span data-ttu-id="3cae4-166">Chcete-li získat jednotné přihlašování, které jsou nakonfigurované pro vaše aplikace, obraťte se na [tým podpory pojistka](mailto:support@fusion-universal.com) a poskytněte jim následující:</span><span class="sxs-lookup"><span data-stu-id="3cae4-166">To get SSO configured for your application, contact [Fuse support team](mailto:support@fusion-universal.com) and provide them with the following:</span></span>

    * <span data-ttu-id="3cae4-167">Stažené **soubor certifikátu (Raw)**</span><span class="sxs-lookup"><span data-stu-id="3cae4-167">The downloaded **Certificate (Raw) file**</span></span>
    * <span data-ttu-id="3cae4-168">**Adresa URL služby jednotného přihlašování SAML**</span><span class="sxs-lookup"><span data-stu-id="3cae4-168">The **SAML Single Sign-On Service URL**</span></span>
    * <span data-ttu-id="3cae4-169">**SAML Entity ID**</span><span class="sxs-lookup"><span data-stu-id="3cae4-169">The **SAML Entity ID**</span></span>
    * <span data-ttu-id="3cae4-170">**Odhlášení adresy URL**</span><span class="sxs-lookup"><span data-stu-id="3cae4-170">The **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="3cae4-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="3cae4-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3cae4-172">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="3cae4-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3cae4-173">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3cae4-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="3cae4-174">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3cae4-174">Create an Azure AD test user</span></span>

<span data-ttu-id="3cae4-175">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3cae4-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="3cae4-177">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3cae4-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3cae4-178">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3cae4-178">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-fuse-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="3cae4-180">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3cae4-180">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-fuse-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="3cae4-182">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3cae4-182">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-fuse-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="3cae4-184">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3cae4-184">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-fuse-tutorial/create_aaduser_04.png)

    <span data-ttu-id="3cae4-186">a.</span><span class="sxs-lookup"><span data-stu-id="3cae4-186">a.</span></span> <span data-ttu-id="3cae4-187">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3cae4-187">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3cae4-188">b.</span><span class="sxs-lookup"><span data-stu-id="3cae4-188">b.</span></span> <span data-ttu-id="3cae4-189">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3cae4-189">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="3cae4-190">c.</span><span class="sxs-lookup"><span data-stu-id="3cae4-190">c.</span></span> <span data-ttu-id="3cae4-191">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="3cae4-191">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="3cae4-192">d.</span><span class="sxs-lookup"><span data-stu-id="3cae4-192">d.</span></span> <span data-ttu-id="3cae4-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3cae4-193">Click **Create**.</span></span>
 
### <a name="create-a-fuse-test-user"></a><span data-ttu-id="3cae4-194">Vytvoření zkušebního uživatele pojistka</span><span class="sxs-lookup"><span data-stu-id="3cae4-194">Create a Fuse test user</span></span>

<span data-ttu-id="3cae4-195">V této části vytvoříte volal Britta Simon v pojistka uživatele.</span><span class="sxs-lookup"><span data-stu-id="3cae4-195">In this section, you create a user called Britta Simon in Fuse.</span></span> <span data-ttu-id="3cae4-196">Spojte se s [tým podpory pojistka](mailto:support@fusion-universal.com) přidat uživatele do pojistka platformy.</span><span class="sxs-lookup"><span data-stu-id="3cae4-196">Please work with [Fuse support team](mailto:support@fusion-universal.com) to add the users in the Fuse platform.</span></span>


### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="3cae4-197">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3cae4-197">Assign the Azure AD test user</span></span>

<span data-ttu-id="3cae4-198">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu pojistka.</span><span class="sxs-lookup"><span data-stu-id="3cae4-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Fuse.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="3cae4-200">**Pokud chcete přiřadit Britta Simon pojistka, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3cae4-200">**To assign Britta Simon to Fuse, perform the following steps:**</span></span>

1. <span data-ttu-id="3cae4-201">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3cae4-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3cae4-203">V seznamu aplikací vyberte **pojistka**.</span><span class="sxs-lookup"><span data-stu-id="3cae4-203">In the applications list, select **Fuse**.</span></span>

    ![V seznamu aplikací na pojistka odkaz](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_app.png)  

3. <span data-ttu-id="3cae4-205">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3cae4-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="3cae4-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3cae4-207">Click **Add** button.</span></span> <span data-ttu-id="3cae4-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3cae4-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="3cae4-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3cae4-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3cae4-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3cae4-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3cae4-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3cae4-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="3cae4-213">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3cae4-213">Test single sign-on</span></span>

<span data-ttu-id="3cae4-214">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3cae4-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3cae4-215">Když kliknete na dlaždici pojistka na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci pojistka.</span><span class="sxs-lookup"><span data-stu-id="3cae4-215">When you click the Fuse tile in the Access Panel, you should get automatically signed-on to your Fuse application.</span></span>
<span data-ttu-id="3cae4-216">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3cae4-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="3cae4-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3cae4-217">Additional resources</span></span>

* [<span data-ttu-id="3cae4-218">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3cae4-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3cae4-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3cae4-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_203.png

