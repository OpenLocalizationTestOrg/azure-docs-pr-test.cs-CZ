---
title: "Kurz: Azure Active Directory integrace s TigerText zabezpečení Messenger | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a TigerText zabezpečení Messenger."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 03f1e128-5bcb-4e49-b6a3-fe22eedc6d5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: e101e5fc84b032b66dd0636bab8bff128791f77c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tigertext-secure-messenger"></a><span data-ttu-id="01c6d-103">Kurz: Azure Active Directory integrace s TigerText zabezpečení Messenger</span><span class="sxs-lookup"><span data-stu-id="01c6d-103">Tutorial: Azure Active Directory integration with TigerText Secure Messenger</span></span>

<span data-ttu-id="01c6d-104">V tomto kurzu zjistěte, jak integrovat TigerText zabezpečení Kurýrní službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="01c6d-104">In this tutorial, you learn how to integrate TigerText Secure Messenger with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="01c6d-105">Integrace TigerText zabezpečení Messenger s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="01c6d-105">Integrating TigerText Secure Messenger with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="01c6d-106">Můžete řídit ve službě Azure AD, který má přístup ke službě Messenger zabezpečení TigerText</span><span class="sxs-lookup"><span data-stu-id="01c6d-106">You can control in Azure AD who has access to TigerText Secure Messenger</span></span>
- <span data-ttu-id="01c6d-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k TigerText zabezpečení Messenger (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="01c6d-107">You can enable your users to automatically get signed-on to TigerText Secure Messenger (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="01c6d-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="01c6d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="01c6d-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="01c6d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01c6d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="01c6d-110">Prerequisites</span></span>

<span data-ttu-id="01c6d-111">Konfigurace integrace Azure AD s TigerText zabezpečení Messenger, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="01c6d-111">To configure Azure AD integration with TigerText Secure Messenger, you need the following items:</span></span>

- <span data-ttu-id="01c6d-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="01c6d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="01c6d-113">Zabezpečení Messenger TigerText jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="01c6d-113">A TigerText Secure Messenger single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="01c6d-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="01c6d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="01c6d-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="01c6d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="01c6d-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="01c6d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="01c6d-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="01c6d-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="01c6d-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="01c6d-118">Scenario description</span></span>
<span data-ttu-id="01c6d-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="01c6d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="01c6d-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="01c6d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="01c6d-121">Přidat TigerText zabezpečení Messenger z Galerie</span><span class="sxs-lookup"><span data-stu-id="01c6d-121">Add TigerText Secure Messenger from the gallery</span></span>
2. <span data-ttu-id="01c6d-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="01c6d-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-tigertext-secure-messenger-from-the-gallery"></a><span data-ttu-id="01c6d-123">Přidat TigerText zabezpečení Messenger z Galerie</span><span class="sxs-lookup"><span data-stu-id="01c6d-123">Add TigerText Secure Messenger from the gallery</span></span>
<span data-ttu-id="01c6d-124">Při konfiguraci integrace TigerText zabezpečení Messenger do služby Azure AD potřebujete přidat TigerText zabezpečení Messenger z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="01c6d-124">To configure the integration of TigerText Secure Messenger into Azure AD, you need to add TigerText Secure Messenger from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="01c6d-125">**Přidat TigerText zabezpečení Messenger z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="01c6d-125">**To add TigerText Secure Messenger from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="01c6d-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="01c6d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="01c6d-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="01c6d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="01c6d-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="01c6d-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="01c6d-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01c6d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="01c6d-133">Do vyhledávacího pole zadejte **TigerText zabezpečení Messenger**, vyberte **TigerText zabezpečení Messenger** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="01c6d-133">In the search box, type **TigerText Secure Messenger**, select **TigerText Secure Messenger** from result panel then click **Add** button to add the application.</span></span>

    ![Přidat z Galerie](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="01c6d-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="01c6d-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="01c6d-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s TigerText zabezpečení Messenger podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="01c6d-136">In this section, you configure and test Azure AD single sign-on with TigerText Secure Messenger based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="01c6d-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v Messenger TigerText zabezpečení je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="01c6d-137">For single sign-on to work, Azure AD needs to know what the counterpart user in TigerText Secure Messenger is to a user in Azure AD.</span></span> <span data-ttu-id="01c6d-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v programu Messenger zabezpečení TigerText musí navázat.</span><span class="sxs-lookup"><span data-stu-id="01c6d-138">In other words, a link relationship between an Azure AD user and the related user in TigerText Secure Messenger needs to be established.</span></span>

<span data-ttu-id="01c6d-139">V zabezpečené Messenger TigerText přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="01c6d-139">In TigerText Secure Messenger, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="01c6d-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s TigerText zabezpečení Messenger, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="01c6d-140">To configure and test Azure AD single sign-on with TigerText Secure Messenger, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="01c6d-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="01c6d-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="01c6d-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="01c6d-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="01c6d-143">**[Vytvoření zkušebního uživatele Messenger zabezpečení TigerText](#create-a-tigertext-secure-messenger-test-user)**  – Pokud chcete mít protějšek Britta Simon v zabezpečené Messenger TigerText, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="01c6d-143">**[Create a TigerText Secure Messenger test user](#create-a-tigertext-secure-messenger-test-user)** - to have a counterpart of Britta Simon in TigerText Secure Messenger that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="01c6d-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="01c6d-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="01c6d-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="01c6d-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="01c6d-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="01c6d-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="01c6d-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci TigerText zabezpečení Messenger.</span><span class="sxs-lookup"><span data-stu-id="01c6d-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TigerText Secure Messenger application.</span></span>

<span data-ttu-id="01c6d-148">**Ke konfiguraci Azure AD jednotné přihlašování s TigerText zabezpečení Messenger, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="01c6d-148">**To configure Azure AD single sign-on with TigerText Secure Messenger, perform the following steps:**</span></span>

1. <span data-ttu-id="01c6d-149">Na portálu Azure na **TigerText zabezpečení Messenger** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="01c6d-149">In the Azure portal, on the **TigerText Secure Messenger** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="01c6d-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="01c6d-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Na základě SAML přihlášení](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_samlbase.png)

3. <span data-ttu-id="01c6d-153">Na **TigerText zabezpečení domény Messenger a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="01c6d-153">On the **TigerText Secure Messenger Domain and URLs** section, perform the following steps:</span></span>

    ![Část TigerText zabezpečení domény Messenger a adresy URL](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_url.png)

    <span data-ttu-id="01c6d-155">a.</span><span class="sxs-lookup"><span data-stu-id="01c6d-155">a.</span></span> <span data-ttu-id="01c6d-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://home.tigertext.com`</span><span class="sxs-lookup"><span data-stu-id="01c6d-156">In the **Sign-on URL** textbox, type URL as: `https://home.tigertext.com`</span></span>

    <span data-ttu-id="01c6d-157">b.</span><span class="sxs-lookup"><span data-stu-id="01c6d-157">b.</span></span> <span data-ttu-id="01c6d-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://saml-lb.tigertext.me/v1/organization/<instance Id>`</span><span class="sxs-lookup"><span data-stu-id="01c6d-158">In the **Identifier** textbox, type a URL using the following pattern: `https://saml-lb.tigertext.me/v1/organization/<instance Id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="01c6d-159">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="01c6d-159">This value is not real.</span></span> <span data-ttu-id="01c6d-160">Aktualizujte tato hodnota se skutečným identifikátorem.</span><span class="sxs-lookup"><span data-stu-id="01c6d-160">Update this value with the actual Identifier.</span></span> <span data-ttu-id="01c6d-161">Obraťte se na [tým podpory TigerText zabezpečení Messenger klienta](mailTo:prosupport@tigertext.com) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="01c6d-161">Contact [TigerText Secure Messenger Client support team](mailTo:prosupport@tigertext.com) to get this value.</span></span> 
 
4. <span data-ttu-id="01c6d-162">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="01c6d-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Certifikát pro podpis SAML části](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_certificate.png) 

5. <span data-ttu-id="01c6d-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="01c6d-164">Click **Save** button.</span></span>

    ![Tlačítko Uložit](./media/active-directory-saas-tigertext-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="01c6d-166">Pokud chcete získat jednotné přihlašování nakonfigurovaný pro vaše aplikace, obraťte se na [tým podpory zabezpečení Messenger TigerText](mailTo:prosupport@tigertext.com) a poskytněte **stažené metadata**.</span><span class="sxs-lookup"><span data-stu-id="01c6d-166">To get single sign-on configured for your application, contact [TigerText Secure Messenger support team](mailTo:prosupport@tigertext.com) and provide them the **Downloaded metadata**.</span></span>

> [!TIP]
> <span data-ttu-id="01c6d-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="01c6d-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="01c6d-168">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="01c6d-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="01c6d-169">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="01c6d-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="01c6d-170">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="01c6d-170">Create an Azure AD test user</span></span>
<span data-ttu-id="01c6d-171">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="01c6d-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="01c6d-173">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="01c6d-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="01c6d-174">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="01c6d-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tigertext-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="01c6d-176">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="01c6d-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Uživatelé a skupiny -> všichni uživatelé](./media/active-directory-saas-tigertext-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="01c6d-178">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01c6d-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-tigertext-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="01c6d-180">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="01c6d-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Dialogové okno uživatele](./media/active-directory-saas-tigertext-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="01c6d-182">a.</span><span class="sxs-lookup"><span data-stu-id="01c6d-182">a.</span></span> <span data-ttu-id="01c6d-183">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="01c6d-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="01c6d-184">b.</span><span class="sxs-lookup"><span data-stu-id="01c6d-184">b.</span></span> <span data-ttu-id="01c6d-185">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="01c6d-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="01c6d-186">c.</span><span class="sxs-lookup"><span data-stu-id="01c6d-186">c.</span></span> <span data-ttu-id="01c6d-187">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="01c6d-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="01c6d-188">d.</span><span class="sxs-lookup"><span data-stu-id="01c6d-188">d.</span></span> <span data-ttu-id="01c6d-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="01c6d-189">Click **Create**.</span></span>
 
### <a name="create-a-tigertext-secure-messenger-test-user"></a><span data-ttu-id="01c6d-190">Vytvoření zkušebního uživatele TigerText zabezpečení Messenger</span><span class="sxs-lookup"><span data-stu-id="01c6d-190">Create a TigerText Secure Messenger test user</span></span>

<span data-ttu-id="01c6d-191">V této části vytvoříte volal Britta Simon v TigerText uživatele.</span><span class="sxs-lookup"><span data-stu-id="01c6d-191">In this section, you create a user called Britta Simon in TigerText.</span></span> <span data-ttu-id="01c6d-192">Kontaktujte [tým podpory TigerText zabezpečení Messenger klienta](mailTo:prosupport@tigertext.com) přidat uživatele do TigerText platformy.</span><span class="sxs-lookup"><span data-stu-id="01c6d-192">Please reach out to [TigerText Secure Messenger Client support team](mailTo:prosupport@tigertext.com) to add the users in the TigerText platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="01c6d-193">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="01c6d-193">Assign the Azure AD test user</span></span>

<span data-ttu-id="01c6d-194">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu ke službě Messenger, TigerText zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="01c6d-194">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TigerText Secure Messenger.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="01c6d-196">**Britta Simon přiřadit ke službě Messenger TigerText, zabezpečení, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="01c6d-196">**To assign Britta Simon to TigerText Secure Messenger, perform the following steps:**</span></span>

1. <span data-ttu-id="01c6d-197">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="01c6d-197">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="01c6d-199">V seznamu aplikací vyberte **TigerText zabezpečení Messenger**.</span><span class="sxs-lookup"><span data-stu-id="01c6d-199">In the applications list, select **TigerText Secure Messenger**.</span></span>

    ![TigerText zabezpečení Messenger v seznamu aplikací](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_app.png) 

3. <span data-ttu-id="01c6d-201">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="01c6d-201">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="01c6d-203">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="01c6d-203">Click **Add** button.</span></span> <span data-ttu-id="01c6d-204">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01c6d-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="01c6d-206">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="01c6d-206">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="01c6d-207">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01c6d-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="01c6d-208">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01c6d-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="01c6d-209">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="01c6d-209">Test single sign-on</span></span>

<span data-ttu-id="01c6d-210">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="01c6d-210">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="01c6d-211">Když kliknete na dlaždici TigerText na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci TigerText.</span><span class="sxs-lookup"><span data-stu-id="01c6d-211">When you click the TigerText tile in the Access Panel, you should get automatically signed-on to your TigerText application.</span></span> <span data-ttu-id="01c6d-212">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="01c6d-212">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="01c6d-213">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="01c6d-213">Additional resources</span></span>

* [<span data-ttu-id="01c6d-214">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="01c6d-214">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="01c6d-215">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="01c6d-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_203.png

