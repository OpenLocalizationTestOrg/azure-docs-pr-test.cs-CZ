---
title: 'Kurz: Azure Active Directory integrace s Workstars | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Workstars."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 51a4a4e4-ff60-4971-b3f8-a0367b70d220
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: e17c85689fa3aebf00ebf559185032b90103b4a5
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workstars"></a><span data-ttu-id="828f8-103">Kurz: Azure Active Directory integrace s Workstars</span><span class="sxs-lookup"><span data-stu-id="828f8-103">Tutorial: Azure Active Directory integration with Workstars</span></span>

<span data-ttu-id="828f8-104">V tomto kurzu zjistěte, jak integrovat Workstars s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="828f8-104">In this tutorial, you learn how to integrate Workstars with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="828f8-105">Integrace Workstars s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="828f8-105">Integrating Workstars with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="828f8-106">Můžete ovládat ve službě Azure AD, který má přístup k Workstars.</span><span class="sxs-lookup"><span data-stu-id="828f8-106">You can control in Azure AD who has access to Workstars.</span></span>
- <span data-ttu-id="828f8-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Workstars (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="828f8-107">You can enable your users to automatically get signed-on to Workstars (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="828f8-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="828f8-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="828f8-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="828f8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="828f8-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="828f8-110">Prerequisites</span></span>

<span data-ttu-id="828f8-111">Konfigurace integrace Azure AD s Workstars, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="828f8-111">To configure Azure AD integration with Workstars, you need the following items:</span></span>

- <span data-ttu-id="828f8-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="828f8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="828f8-113">Workstars jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="828f8-113">A Workstars single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="828f8-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="828f8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="828f8-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="828f8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="828f8-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="828f8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="828f8-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="828f8-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="828f8-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="828f8-118">Scenario description</span></span>
<span data-ttu-id="828f8-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="828f8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="828f8-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="828f8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="828f8-121">Přidání Workstars z Galerie</span><span class="sxs-lookup"><span data-stu-id="828f8-121">Adding Workstars from the gallery</span></span>
2. <span data-ttu-id="828f8-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="828f8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workstars-from-the-gallery"></a><span data-ttu-id="828f8-123">Přidání Workstars z Galerie</span><span class="sxs-lookup"><span data-stu-id="828f8-123">Adding Workstars from the gallery</span></span>
<span data-ttu-id="828f8-124">Při konfiguraci integrace Workstars do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Workstars z galerie.</span><span class="sxs-lookup"><span data-stu-id="828f8-124">To configure the integration of Workstars into Azure AD, you need to add Workstars from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="828f8-125">**Pokud chcete přidat Workstars z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="828f8-125">**To add Workstars from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="828f8-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="828f8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="828f8-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="828f8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="828f8-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="828f8-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="828f8-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="828f8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="828f8-133">Do vyhledávacího pole zadejte **Workstars**, vyberte **Workstars** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="828f8-133">In the search box, type **Workstars**, select **Workstars** from result panel then click **Add** button to add the application.</span></span>

    ![Workstars v seznamu výsledků](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="828f8-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="828f8-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="828f8-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Workstars podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="828f8-136">In this section, you configure and test Azure AD single sign-on with Workstars based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="828f8-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Workstars je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="828f8-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Workstars is to a user in Azure AD.</span></span> <span data-ttu-id="828f8-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Workstars musí navázat.</span><span class="sxs-lookup"><span data-stu-id="828f8-138">In other words, a link relationship between an Azure AD user and the related user in Workstars needs to be established.</span></span>

<span data-ttu-id="828f8-139">V Workstars, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="828f8-139">In Workstars, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="828f8-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Workstars, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="828f8-140">To configure and test Azure AD single sign-on with Workstars, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="828f8-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="828f8-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="828f8-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="828f8-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="828f8-143">**[Vytvoření zkušebního uživatele Workstars](#create-a-workstars-test-user)**  – Pokud chcete mít protějšek Britta Simon v Workstars propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="828f8-143">**[Create a Workstars test user](#create-a-workstars-test-user)** - to have a counterpart of Britta Simon in Workstars that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="828f8-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="828f8-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="828f8-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="828f8-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="828f8-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="828f8-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="828f8-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Workstars.</span><span class="sxs-lookup"><span data-stu-id="828f8-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Workstars application.</span></span>

<span data-ttu-id="828f8-148">**Ke konfiguraci Azure AD jednotné přihlašování s Workstars, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="828f8-148">**To configure Azure AD single sign-on with Workstars, perform the following steps:**</span></span>

1. <span data-ttu-id="828f8-149">Na portálu Azure na **Workstars** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="828f8-149">In the Azure portal, on the **Workstars** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="828f8-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="828f8-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_samlbase.png)

3. <span data-ttu-id="828f8-153">Na **Workstars domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="828f8-153">On the **Workstars Domain and URLs** section, perform the following steps:</span></span>

    ![Workstars domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_url.png)

    <span data-ttu-id="828f8-155">a.</span><span class="sxs-lookup"><span data-stu-id="828f8-155">a.</span></span> <span data-ttu-id="828f8-156">V **identifikátor** textovému poli, zadejte adresu URL:`https://workstars.com`</span><span class="sxs-lookup"><span data-stu-id="828f8-156">In the **Identifier** textbox, type the URL: `https://workstars.com`</span></span>

    <span data-ttu-id="828f8-157">b.</span><span class="sxs-lookup"><span data-stu-id="828f8-157">b.</span></span> <span data-ttu-id="828f8-158">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.workstars.com/saml/login_check`</span><span class="sxs-lookup"><span data-stu-id="828f8-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.workstars.com/saml/login_check`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="828f8-159">Hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="828f8-159">The value is not real.</span></span> <span data-ttu-id="828f8-160">Aktualizujte hodnotu s skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="828f8-160">Update the value with the actual Reply URL.</span></span> <span data-ttu-id="828f8-161">Obraťte se na [tým podpory Workstars](https://support.workstars.com) k získání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="828f8-161">Contact [Workstars support team](https://support.workstars.com) to get the value.</span></span>
 
4. <span data-ttu-id="828f8-162">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="828f8-162">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_certificate.png) 

5. <span data-ttu-id="828f8-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="828f8-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-workstars-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="828f8-166">Na **Workstars konfigurace** klikněte na tlačítko **konfigurace Workstars** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="828f8-166">On the **Workstars Configuration** section, click **Configure Workstars** to open **Configure sign-on** window.</span></span> <span data-ttu-id="828f8-167">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="828f8-167">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace Workstars](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_configure.png) 

7. <span data-ttu-id="828f8-169">V jiném okně prohlížeče Přihlaste se k serveru vaší společnosti Workstars jako správce.</span><span class="sxs-lookup"><span data-stu-id="828f8-169">In another browser window, sign on to your Workstars company site as an administrator.</span></span>

8. <span data-ttu-id="828f8-170">Na hlavním panelu nástrojů klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="828f8-170">In the main toolbar, click **Settings**.</span></span>

    ![Nastavení Workstars](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_sett.png)

9. <span data-ttu-id="828f8-172">Přejděte na **přihlásit** > **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="828f8-172">Go to **Sign On** > **Settings**.</span></span>

    ![Sign-on Workstars](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_signon.png)

    ![Nastavení Workstars](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_settings.png)

10. <span data-ttu-id="828f8-175">Na **jeden znak na (SAML) – nastavení** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="828f8-175">On the **Single Sign On (SAML) - Settings** page, perform the following steps:</span></span>
    
    ![Workstars saml](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_saml.png)

    <span data-ttu-id="828f8-177">a.</span><span class="sxs-lookup"><span data-stu-id="828f8-177">a.</span></span> <span data-ttu-id="828f8-178">V **název zprostředkovatele Identity** textovému poli, typ **Office 365**.</span><span class="sxs-lookup"><span data-stu-id="828f8-178">In **Identity Provider Name** textbox, type **Office 365**.</span></span>

    <span data-ttu-id="828f8-179">b.</span><span class="sxs-lookup"><span data-stu-id="828f8-179">b.</span></span> <span data-ttu-id="828f8-180">V **ID Entity zprostředkovatele Identity** textovému poli, vložte hodnotu **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="828f8-180">In the **Identity Provider Entity ID** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="828f8-181">c.</span><span class="sxs-lookup"><span data-stu-id="828f8-181">c.</span></span> <span data-ttu-id="828f8-182">Kopírovat obsah souboru stažený certifikát v programu Poznámkový blok a vložte ji do **x509 certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="828f8-182">Copy the content of the downloaded certificate file in notepad, and then paste it into the **x509 Certificate** textbox.</span></span> 

    <span data-ttu-id="828f8-183">d.</span><span class="sxs-lookup"><span data-stu-id="828f8-183">d.</span></span> <span data-ttu-id="828f8-184">V **URL jednotné přihlašování SAML** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="828f8-184">In the **SAML SSO URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="828f8-185">e.</span><span class="sxs-lookup"><span data-stu-id="828f8-185">e.</span></span> <span data-ttu-id="828f8-186">V **vzdálené adresy URL odhlašovací** textovému poli, vložte hodnotu **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="828f8-186">In the **Remote Logout URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="828f8-187">f.</span><span class="sxs-lookup"><span data-stu-id="828f8-187">f.</span></span> <span data-ttu-id="828f8-188">Vyberte **ID názvu** jako **e-mailu (výchozí)**.</span><span class="sxs-lookup"><span data-stu-id="828f8-188">select **Name ID** as **Email (Default)**.</span></span>

    <span data-ttu-id="828f8-189">g.</span><span class="sxs-lookup"><span data-stu-id="828f8-189">g.</span></span> <span data-ttu-id="828f8-190">Klikněte na tlačítko **potvrďte**.</span><span class="sxs-lookup"><span data-stu-id="828f8-190">Click **Confirm**.</span></span>
    
> [!TIP]
> <span data-ttu-id="828f8-191">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="828f8-191">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="828f8-192">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="828f8-192">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="828f8-193">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="828f8-193">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="828f8-194">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="828f8-194">Create an Azure AD test user</span></span>

<span data-ttu-id="828f8-195">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="828f8-195">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="828f8-197">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="828f8-197">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="828f8-198">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="828f8-198">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-workstars-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="828f8-200">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="828f8-200">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-workstars-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="828f8-202">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="828f8-202">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-workstars-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="828f8-204">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="828f8-204">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-workstars-tutorial/create_aaduser_04.png)

    <span data-ttu-id="828f8-206">a.</span><span class="sxs-lookup"><span data-stu-id="828f8-206">a.</span></span> <span data-ttu-id="828f8-207">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="828f8-207">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="828f8-208">b.</span><span class="sxs-lookup"><span data-stu-id="828f8-208">b.</span></span> <span data-ttu-id="828f8-209">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="828f8-209">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="828f8-210">c.</span><span class="sxs-lookup"><span data-stu-id="828f8-210">c.</span></span> <span data-ttu-id="828f8-211">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="828f8-211">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="828f8-212">d.</span><span class="sxs-lookup"><span data-stu-id="828f8-212">d.</span></span> <span data-ttu-id="828f8-213">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="828f8-213">Click **Create**.</span></span>
  
### <a name="create-a-workstars-test-user"></a><span data-ttu-id="828f8-214">Vytvoření zkušebního uživatele Workstars</span><span class="sxs-lookup"><span data-stu-id="828f8-214">Create a Workstars test user</span></span>

<span data-ttu-id="828f8-215">V této části vytvoříte volal Britta Simon v Workstars uživatele.</span><span class="sxs-lookup"><span data-stu-id="828f8-215">In this section, you create a user called Britta Simon in Workstars.</span></span> <span data-ttu-id="828f8-216">Práce s [tým podpory Workstars](https://support.workstars.com) přidat uživatele do Workstars platformy.</span><span class="sxs-lookup"><span data-stu-id="828f8-216">Work with [Workstars support team](https://support.workstars.com) to add the users in the Workstars platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="828f8-217">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="828f8-217">Assign the Azure AD test user</span></span>

<span data-ttu-id="828f8-218">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Workstars.</span><span class="sxs-lookup"><span data-stu-id="828f8-218">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Workstars.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="828f8-220">**Pokud chcete přiřadit Britta Simon Workstars, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="828f8-220">**To assign Britta Simon to Workstars, perform the following steps:**</span></span>

1. <span data-ttu-id="828f8-221">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="828f8-221">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="828f8-223">V seznamu aplikací vyberte **Workstars**.</span><span class="sxs-lookup"><span data-stu-id="828f8-223">In the applications list, select **Workstars**.</span></span>

    ![V seznamu aplikací na Workstars odkaz](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_app.png)  

3. <span data-ttu-id="828f8-225">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="828f8-225">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="828f8-227">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="828f8-227">Click **Add** button.</span></span> <span data-ttu-id="828f8-228">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="828f8-228">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="828f8-230">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="828f8-230">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="828f8-231">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="828f8-231">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="828f8-232">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="828f8-232">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="828f8-233">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="828f8-233">Test single sign-on</span></span>

<span data-ttu-id="828f8-234">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="828f8-234">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="828f8-235">Když kliknete na dlaždici Workstars na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Workstars.</span><span class="sxs-lookup"><span data-stu-id="828f8-235">When you click the Workstars tile in the Access Panel, you should get automatically signed-on to your Workstars application.</span></span>
<span data-ttu-id="828f8-236">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="828f8-236">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="828f8-237">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="828f8-237">Additional resources</span></span>

* [<span data-ttu-id="828f8-238">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="828f8-238">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="828f8-239">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="828f8-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_203.png

