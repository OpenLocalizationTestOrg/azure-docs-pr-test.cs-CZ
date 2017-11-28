---
title: 'Kurz: Azure Active Directory integrace s Zoho | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Zoho."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 9874e1f3-ade5-42e7-a700-e08b3731236a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.openlocfilehash: f0688cb75584ada805b944d2ef5409d66ab37339
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zoho"></a><span data-ttu-id="db09a-103">Kurz: Azure Active Directory integrace s Zoho</span><span class="sxs-lookup"><span data-stu-id="db09a-103">Tutorial: Azure Active Directory integration with Zoho</span></span>

<span data-ttu-id="db09a-104">V tomto kurzu zjistěte, jak integrovat Zoho s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="db09a-104">In this tutorial, you learn how to integrate Zoho with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="db09a-105">Integrace Zoho s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="db09a-105">Integrating Zoho with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="db09a-106">Můžete ovládat ve službě Azure AD, který má přístup k Zoho.</span><span class="sxs-lookup"><span data-stu-id="db09a-106">You can control in Azure AD who has access to Zoho.</span></span>
- <span data-ttu-id="db09a-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Zoho (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="db09a-107">You can enable your users to automatically get signed-on to Zoho (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="db09a-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="db09a-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="db09a-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="db09a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db09a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="db09a-110">Prerequisites</span></span>

<span data-ttu-id="db09a-111">Konfigurace integrace Azure AD s Zoho, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="db09a-111">To configure Azure AD integration with Zoho, you need the following items:</span></span>

- <span data-ttu-id="db09a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="db09a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="db09a-113">Zoho jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="db09a-113">A Zoho single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="db09a-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="db09a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="db09a-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="db09a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="db09a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="db09a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="db09a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="db09a-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="db09a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="db09a-118">Scenario description</span></span>
<span data-ttu-id="db09a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="db09a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="db09a-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="db09a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="db09a-121">Přidání Zoho z Galerie</span><span class="sxs-lookup"><span data-stu-id="db09a-121">Adding Zoho from the gallery</span></span>
2. <span data-ttu-id="db09a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="db09a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zoho-from-the-gallery"></a><span data-ttu-id="db09a-123">Přidání Zoho z Galerie</span><span class="sxs-lookup"><span data-stu-id="db09a-123">Adding Zoho from the gallery</span></span>
<span data-ttu-id="db09a-124">Při konfiguraci integrace Zoho do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Zoho z galerie.</span><span class="sxs-lookup"><span data-stu-id="db09a-124">To configure the integration of Zoho into Azure AD, you need to add Zoho from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="db09a-125">**Pokud chcete přidat Zoho z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="db09a-125">**To add Zoho from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="db09a-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="db09a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="db09a-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="db09a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="db09a-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="db09a-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="db09a-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="db09a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="db09a-133">Do vyhledávacího pole zadejte **Zoho**, vyberte **Zoho** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="db09a-133">In the search box, type **Zoho**, select **Zoho** from result panel then click **Add** button to add the application.</span></span>

    ![Zoho v seznamu výsledků](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="db09a-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="db09a-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="db09a-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Zoho podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="db09a-136">In this section, you configure and test Azure AD single sign-on with Zoho based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="db09a-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Zoho je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="db09a-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Zoho is to a user in Azure AD.</span></span> <span data-ttu-id="db09a-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Zoho musí navázat.</span><span class="sxs-lookup"><span data-stu-id="db09a-138">In other words, a link relationship between an Azure AD user and the related user in Zoho needs to be established.</span></span>

<span data-ttu-id="db09a-139">V Zoho, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="db09a-139">In Zoho, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="db09a-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Zoho, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="db09a-140">To configure and test Azure AD single sign-on with Zoho, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="db09a-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="db09a-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="db09a-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="db09a-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="db09a-143">**[Vytvoření zkušebního uživatele Zoho](#create-a-zoho-test-user)**  – Pokud chcete mít protějšek Britta Simon v Zoho propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="db09a-143">**[Create a Zoho test user](#create-a-zoho-test-user)** - to have a counterpart of Britta Simon in Zoho that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="db09a-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="db09a-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="db09a-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="db09a-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="db09a-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="db09a-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="db09a-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Zoho.</span><span class="sxs-lookup"><span data-stu-id="db09a-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Zoho application.</span></span>

<span data-ttu-id="db09a-148">**Ke konfiguraci Azure AD jednotné přihlašování s Zoho, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="db09a-148">**To configure Azure AD single sign-on with Zoho, perform the following steps:**</span></span>

1. <span data-ttu-id="db09a-149">Na portálu Azure na **Zoho** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="db09a-149">In the Azure portal, on the **Zoho** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="db09a-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="db09a-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_samlbase.png)

3. <span data-ttu-id="db09a-153">Na **Zoho domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="db09a-153">On the **Zoho Domain and URLs** section, perform the following steps:</span></span>

    ![Zoho domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_url.png)

    <span data-ttu-id="db09a-155">a.</span><span class="sxs-lookup"><span data-stu-id="db09a-155">a.</span></span> <span data-ttu-id="db09a-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.zohomail.com`</span><span class="sxs-lookup"><span data-stu-id="db09a-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.zohomail.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="db09a-157">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="db09a-157">This value is not real.</span></span> <span data-ttu-id="db09a-158">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="db09a-158">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="db09a-159">Obraťte se na [tým podpory Zoho klienta](https://www.zoho.com/mail/contact.html) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="db09a-159">Contact [Zoho Client support team](https://www.zoho.com/mail/contact.html) to get this value.</span></span> 
 
4. <span data-ttu-id="db09a-160">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="db09a-160">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_certificate.png) 

5. <span data-ttu-id="db09a-162">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="db09a-162">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="db09a-164">Na **Zoho konfigurace** klikněte na tlačítko **konfigurace Zoho** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="db09a-164">On the **Zoho Configuration** section, click **Configure Zoho** to open **Configure sign-on** window.</span></span> <span data-ttu-id="db09a-165">Kopírování **Sign-Out adresu URL, adresy URL pro změnu hesla a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="db09a-165">Copy the **Sign-Out URL, Change Password URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace Zoho](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_configure.png) 

7. <span data-ttu-id="db09a-167">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Zoho e-mailu.</span><span class="sxs-lookup"><span data-stu-id="db09a-167">In a different web browser window, log into your Zoho Mail company site as an administrator.</span></span>

8. <span data-ttu-id="db09a-168">Přejděte na **ovládací panely**.</span><span class="sxs-lookup"><span data-stu-id="db09a-168">Go to the **Control panel**.</span></span>
   
    <span data-ttu-id="db09a-169">![Ovládací panely](./media/active-directory-saas-zoho-mail-tutorial/ic789607.png "ovládací panely")</span><span class="sxs-lookup"><span data-stu-id="db09a-169">![Control Panel](./media/active-directory-saas-zoho-mail-tutorial/ic789607.png "Control Panel")</span></span>

9. <span data-ttu-id="db09a-170">Klikněte **ověřování SAML** kartě.</span><span class="sxs-lookup"><span data-stu-id="db09a-170">Click the **SAML Authentication** tab.</span></span>
   
    <span data-ttu-id="db09a-171">![Ověřování SAML](./media/active-directory-saas-zoho-mail-tutorial/ic789608.png "ověřování SAML")</span><span class="sxs-lookup"><span data-stu-id="db09a-171">![SAML Authentication](./media/active-directory-saas-zoho-mail-tutorial/ic789608.png "SAML Authentication")</span></span>

10. <span data-ttu-id="db09a-172">V **podrobnosti ověřování SAML** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="db09a-172">In the **SAML Authentication Details** section, perform the following steps:</span></span>
   
    <span data-ttu-id="db09a-173">![Podrobnosti o ověřování SAML](./media/active-directory-saas-zoho-mail-tutorial/ic789609.png "podrobnosti ověřování SAML")</span><span class="sxs-lookup"><span data-stu-id="db09a-173">![SAML Authentication Details](./media/active-directory-saas-zoho-mail-tutorial/ic789609.png "SAML Authentication Details")</span></span>
   
    <span data-ttu-id="db09a-174">a.</span><span class="sxs-lookup"><span data-stu-id="db09a-174">a.</span></span> <span data-ttu-id="db09a-175">V **přihlašovací adresa URL** textovému poli, vložte **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="db09a-175">In the **Login URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="db09a-176">b.</span><span class="sxs-lookup"><span data-stu-id="db09a-176">b.</span></span> <span data-ttu-id="db09a-177">V **adresy URL odhlašovací** textovému poli, vložte **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="db09a-177">In the **Logout URL** textbox, paste **Sign-Out URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="db09a-178">c.</span><span class="sxs-lookup"><span data-stu-id="db09a-178">c.</span></span> <span data-ttu-id="db09a-179">V **heslo změnit adresu URL** textovému poli, vložte **heslo změnit adresu URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="db09a-179">In the **Change Password URL** textbox, paste **Change Password URL** which you have copied from Azure portal.</span></span>
       
    <span data-ttu-id="db09a-180">d.</span><span class="sxs-lookup"><span data-stu-id="db09a-180">d.</span></span> <span data-ttu-id="db09a-181">Otevření kódovaného certifikátu kódování base-64 stáhli z portálu Azure v programu Poznámkový blok, zkopírujte obsah ho do schránky a vložte jej do **PublicKey** textové pole.</span><span class="sxs-lookup"><span data-stu-id="db09a-181">Open your base-64 encoded certificate downloaded from Azure portal in notepad, copy the content of it into your clipboard, and then paste it to the **PublicKey** textbox.</span></span>
   
    <span data-ttu-id="db09a-182">e.</span><span class="sxs-lookup"><span data-stu-id="db09a-182">e.</span></span> <span data-ttu-id="db09a-183">Jako **algoritmus**, vyberte **RSA**.</span><span class="sxs-lookup"><span data-stu-id="db09a-183">As **Algorithm**, select **RSA**.</span></span>
   
    <span data-ttu-id="db09a-184">f.</span><span class="sxs-lookup"><span data-stu-id="db09a-184">f.</span></span> <span data-ttu-id="db09a-185">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="db09a-185">Click **OK**.</span></span>

> [!TIP]
> <span data-ttu-id="db09a-186">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="db09a-186">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="db09a-187">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="db09a-187">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="db09a-188">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="db09a-188">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="db09a-189">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="db09a-189">Create an Azure AD test user</span></span>

<span data-ttu-id="db09a-190">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="db09a-190">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="db09a-192">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="db09a-192">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="db09a-193">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="db09a-193">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="db09a-195">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="db09a-195">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="db09a-197">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="db09a-197">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="db09a-199">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="db09a-199">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_04.png)

    <span data-ttu-id="db09a-201">a.</span><span class="sxs-lookup"><span data-stu-id="db09a-201">a.</span></span> <span data-ttu-id="db09a-202">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="db09a-202">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="db09a-203">b.</span><span class="sxs-lookup"><span data-stu-id="db09a-203">b.</span></span> <span data-ttu-id="db09a-204">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="db09a-204">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="db09a-205">c.</span><span class="sxs-lookup"><span data-stu-id="db09a-205">c.</span></span> <span data-ttu-id="db09a-206">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="db09a-206">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="db09a-207">d.</span><span class="sxs-lookup"><span data-stu-id="db09a-207">d.</span></span> <span data-ttu-id="db09a-208">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="db09a-208">Click **Create**.</span></span>
 
### <a name="create-a-zoho-test-user"></a><span data-ttu-id="db09a-209">Vytvoření zkušebního uživatele Zoho</span><span class="sxs-lookup"><span data-stu-id="db09a-209">Create a Zoho test user</span></span>

<span data-ttu-id="db09a-210">Pokud chcete povolit uživatelům Azure AD Přihlaste se k e-mailu Zoho, musí být zřízená do Zoho e-mailu.</span><span class="sxs-lookup"><span data-stu-id="db09a-210">In order to enable Azure AD users to log into Zoho Mail, they must be provisioned into Zoho Mail.</span></span> <span data-ttu-id="db09a-211">V případě Zoho pošta zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="db09a-211">In the case of Zoho Mail, provisioning is a manual task.</span></span>

> [!NOTE]
> <span data-ttu-id="db09a-212">Můžete použít jakékoli jiné e-mailu Zoho uživatele účtu vytvoření nástroje nebo rozhraní API poskytované e-mailu Zoho zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="db09a-212">You can use any other Zoho Mail user account creation tools or APIs provided by Zoho Mail to provision AAD user accounts.</span></span>

### <a name="to-provision-a-user-account-perform-the-following-steps"></a><span data-ttu-id="db09a-213">K poskytnutí uživatelského účtu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="db09a-213">To provision a user account, perform the following steps:</span></span>

1. <span data-ttu-id="db09a-214">Přihlaste se k vaší **Zoho e-mailu** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="db09a-214">Log in to your **Zoho Mail** company site as an administrator.</span></span>

2. <span data-ttu-id="db09a-215">Přejděte na **ovládací panely \> e-mailu a dokumentů**.</span><span class="sxs-lookup"><span data-stu-id="db09a-215">Go to **Control Panel \> Mail & Docs**.</span></span>

3. <span data-ttu-id="db09a-216">Přejděte na **podrobné informace o uživateli \> přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="db09a-216">Go to **User Details \> Add User**.</span></span>
   
    <span data-ttu-id="db09a-217">![Přidat uživatele](./media/active-directory-saas-zoho-mail-tutorial/ic789611.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="db09a-217">![Add User](./media/active-directory-saas-zoho-mail-tutorial/ic789611.png "Add User")</span></span>

4. <span data-ttu-id="db09a-218">Na **přidat uživatele** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="db09a-218">On the **Add users** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="db09a-219">![Přidat uživatele](./media/active-directory-saas-zoho-mail-tutorial/ic789612.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="db09a-219">![Add User](./media/active-directory-saas-zoho-mail-tutorial/ic789612.png "Add User")</span></span>
   
    <span data-ttu-id="db09a-220">a.</span><span class="sxs-lookup"><span data-stu-id="db09a-220">a.</span></span> <span data-ttu-id="db09a-221">V **křestní jméno** jako typ křestní jméno uživatele k textovému poli, **Britta**.</span><span class="sxs-lookup"><span data-stu-id="db09a-221">In the **First Name** textbox, type the first name of user like **Britta**.</span></span>

    <span data-ttu-id="db09a-222">b.</span><span class="sxs-lookup"><span data-stu-id="db09a-222">b.</span></span> <span data-ttu-id="db09a-223">V **příjmení** jako typ příjmení uživatele k textovému poli, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="db09a-223">In the **Last Name** textbox, type the last name of user like **Simon**.</span></span>

    <span data-ttu-id="db09a-224">c.</span><span class="sxs-lookup"><span data-stu-id="db09a-224">c.</span></span> <span data-ttu-id="db09a-225">V **ID e-mailu** jako typ e-mailu id uživatele k textovému poli,  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="db09a-225">In the **Email ID** textbox, type the email id of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="db09a-226">d.</span><span class="sxs-lookup"><span data-stu-id="db09a-226">d.</span></span> <span data-ttu-id="db09a-227">V **heslo** textovému poli, zadejte heslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="db09a-227">In the **Password** textbox, enter password of user.</span></span>
   
    <span data-ttu-id="db09a-228">e.</span><span class="sxs-lookup"><span data-stu-id="db09a-228">e.</span></span> <span data-ttu-id="db09a-229">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="db09a-229">Click **OK**.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="db09a-230">Držitel účtu Azure Active Directory obdrží e-mail s odkazem pro potvrzení účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="db09a-230">The Azure Active Directory account holder will receive an email with a link to confirm the account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="db09a-231">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="db09a-231">Assign the Azure AD test user</span></span>

<span data-ttu-id="db09a-232">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Zoho.</span><span class="sxs-lookup"><span data-stu-id="db09a-232">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Zoho.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="db09a-234">**Pokud chcete přiřadit Britta Simon Zoho, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="db09a-234">**To assign Britta Simon to Zoho, perform the following steps:**</span></span>

1. <span data-ttu-id="db09a-235">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="db09a-235">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="db09a-237">V seznamu aplikací vyberte **Zoho**.</span><span class="sxs-lookup"><span data-stu-id="db09a-237">In the applications list, select **Zoho**.</span></span>

    ![V seznamu aplikací na Zoho odkaz](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_app.png)  

3. <span data-ttu-id="db09a-239">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="db09a-239">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="db09a-241">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="db09a-241">Click **Add** button.</span></span> <span data-ttu-id="db09a-242">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="db09a-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="db09a-244">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="db09a-244">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="db09a-245">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="db09a-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="db09a-246">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="db09a-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="db09a-247">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="db09a-247">Test single sign-on</span></span>

<span data-ttu-id="db09a-248">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="db09a-248">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="db09a-249">Když kliknete na dlaždici Zoho na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Zoho.</span><span class="sxs-lookup"><span data-stu-id="db09a-249">When you click the Zoho tile in the Access Panel, you should get automatically signed-on to your Zoho application.</span></span>
<span data-ttu-id="db09a-250">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="db09a-250">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="db09a-251">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="db09a-251">Additional resources</span></span>

* [<span data-ttu-id="db09a-252">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="db09a-252">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="db09a-253">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="db09a-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_203.png

