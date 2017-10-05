---
title: 'Kurz: Azure Active Directory integrace s ClickTime | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ClickTime."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d437b5ab-4d71-4c13-96d0-79018cebbbd4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jeedes
ms.openlocfilehash: 0e0123a40d52dfd7a2e29c29cb2239e979089ca9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clicktime"></a><span data-ttu-id="d9645-103">Kurz: Azure Active Directory integrace s ClickTime</span><span class="sxs-lookup"><span data-stu-id="d9645-103">Tutorial: Azure Active Directory integration with ClickTime</span></span>

<span data-ttu-id="d9645-104">V tomto kurzu zjistěte, jak integrovat ClickTime s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d9645-104">In this tutorial, you learn how to integrate ClickTime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d9645-105">Integrace ClickTime s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d9645-105">Integrating ClickTime with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d9645-106">Můžete řídit ve službě Azure AD, který má přístup k ClickTime</span><span class="sxs-lookup"><span data-stu-id="d9645-106">You can control in Azure AD who has access to ClickTime</span></span>
- <span data-ttu-id="d9645-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k ClickTime (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="d9645-107">You can enable your users to automatically get signed-on to ClickTime (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d9645-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d9645-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d9645-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d9645-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d9645-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d9645-110">Prerequisites</span></span>

<span data-ttu-id="d9645-111">Konfigurace integrace Azure AD s ClickTime, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="d9645-111">To configure Azure AD integration with ClickTime, you need the following items:</span></span>

- <span data-ttu-id="d9645-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d9645-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d9645-113">ClickTime jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d9645-113">A ClickTime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d9645-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d9645-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d9645-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d9645-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d9645-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d9645-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d9645-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d9645-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d9645-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d9645-118">Scenario description</span></span>
<span data-ttu-id="d9645-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d9645-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d9645-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d9645-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d9645-121">Přidání ClickTime z Galerie</span><span class="sxs-lookup"><span data-stu-id="d9645-121">Adding ClickTime from the gallery</span></span>
2. <span data-ttu-id="d9645-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d9645-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-clicktime-from-the-gallery"></a><span data-ttu-id="d9645-123">Přidání ClickTime z Galerie</span><span class="sxs-lookup"><span data-stu-id="d9645-123">Adding ClickTime from the gallery</span></span>
<span data-ttu-id="d9645-124">Při konfiguraci integrace ClickTime do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS ClickTime z galerie.</span><span class="sxs-lookup"><span data-stu-id="d9645-124">To configure the integration of ClickTime into Azure AD, you need to add ClickTime from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d9645-125">**Pokud chcete přidat ClickTime z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d9645-125">**To add ClickTime from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d9645-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d9645-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="d9645-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d9645-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d9645-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d9645-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="d9645-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d9645-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="d9645-133">Do vyhledávacího pole zadejte **ClickTime**, vyberte **ClickTime** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d9645-133">In the search box, type **ClickTime**, select **ClickTime** from result panel then click **Add** button to add the application.</span></span>

    ![ClickTime v seznamu výsledků](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="d9645-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d9645-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="d9645-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s ClickTime podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="d9645-136">In this section, you configure and test Azure AD single sign-on with ClickTime based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d9645-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v ClickTime je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d9645-137">For single sign-on to work, Azure AD needs to know what the counterpart user in ClickTime is to a user in Azure AD.</span></span> <span data-ttu-id="d9645-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v ClickTime musí navázat.</span><span class="sxs-lookup"><span data-stu-id="d9645-138">In other words, a link relationship between an Azure AD user and the related user in ClickTime needs to be established.</span></span>

<span data-ttu-id="d9645-139">V ClickTime, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="d9645-139">In ClickTime, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d9645-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s ClickTime, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="d9645-140">To configure and test Azure AD single sign-on with ClickTime, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d9645-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="d9645-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d9645-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d9645-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d9645-143">**[Vytvoření zkušebního uživatele ClickTime](#create-a-clicktime-test-user)**  – Pokud chcete mít protějšek Britta Simon v ClickTime propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="d9645-143">**[Create a ClickTime test user](#create-a-clicktime-test-user)** - to have a counterpart of Britta Simon in ClickTime that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d9645-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d9645-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d9645-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d9645-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="d9645-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d9645-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="d9645-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci ClickTime.</span><span class="sxs-lookup"><span data-stu-id="d9645-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ClickTime application.</span></span>

<span data-ttu-id="d9645-148">**Ke konfiguraci Azure AD jednotné přihlašování s ClickTime, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d9645-148">**To configure Azure AD single sign-on with ClickTime, perform the following steps:**</span></span>

1. <span data-ttu-id="d9645-149">Na portálu Azure na **ClickTime** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d9645-149">In the Azure portal, on the **ClickTime** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="d9645-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d9645-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_samlbase.png)

3. <span data-ttu-id="d9645-153">Na **ClickTime domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d9645-153">On the **ClickTime Domain and URLs** section, perform the following steps:</span></span>

    ![ClickTime domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_url.png)

    <span data-ttu-id="d9645-155">a.</span><span class="sxs-lookup"><span data-stu-id="d9645-155">a.</span></span> <span data-ttu-id="d9645-156">V **identifikátor** textovému poli, zadejte adresu URL jako:`https://app.clicktime.com/sp/`</span><span class="sxs-lookup"><span data-stu-id="d9645-156">In the **Identifier** textbox, type a URL as: `https://app.clicktime.com/sp/`</span></span>
    
    <span data-ttu-id="d9645-157">b.</span><span class="sxs-lookup"><span data-stu-id="d9645-157">b.</span></span> <span data-ttu-id="d9645-158">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="d9645-158">In the **Reply URL** textbox, type a URL using the following patterns:</span></span> 

    | |
    |--|
    | `https://app.clicktime.com/Login/` |
    | `https://app.clicktime.com/App/Login/Consume.aspx` |

4. <span data-ttu-id="d9645-159">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="d9645-159">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_certificate.png) 

5. <span data-ttu-id="d9645-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d9645-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-clicktime-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d9645-163">Na **ClickTime konfigurace** klikněte na tlačítko **konfigurace ClickTime** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="d9645-163">On the **ClickTime Configuration** section, click **Configure ClickTime** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d9645-164">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="d9645-164">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace ClickTime](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_configure.png) 

7. <span data-ttu-id="d9645-166">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti ClickTime.</span><span class="sxs-lookup"><span data-stu-id="d9645-166">In a different web browser window, log into your ClickTime company site as an administrator.</span></span>

8. <span data-ttu-id="d9645-167">Na panelu nástrojů v horní části klikněte na tlačítko **Předvolby**a potom klikněte na **nastavení zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="d9645-167">In the toolbar on the top, click **Preferences**, and then click **Security Settings**.</span></span>

9. <span data-ttu-id="d9645-168">V **předvolby přihlašování** konfigurace části, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d9645-168">In the **Single Sign-On Preferences** configuration section, perform the following steps:</span></span>
   
    <span data-ttu-id="d9645-169">![Nastavení zabezpečení](./media/active-directory-saas-clicktime-tutorial/tic777280.png "nastavení zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="d9645-169">![Security Settings](./media/active-directory-saas-clicktime-tutorial/tic777280.png "Security Settings")</span></span>
   
    <span data-ttu-id="d9645-170">a.</span><span class="sxs-lookup"><span data-stu-id="d9645-170">a.</span></span>  <span data-ttu-id="d9645-171">Vyberte **povolit** přihlášení pomocí jednotné přihlašování (SSO) s **Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="d9645-171">Select **Allow** sign-in using Single Sign-On (SSO) with **Azure AD**.</span></span>
   
    <span data-ttu-id="d9645-172">b.</span><span class="sxs-lookup"><span data-stu-id="d9645-172">b.</span></span> <span data-ttu-id="d9645-173">V **koncový bod zprostředkovatele Identity** textovému poli, vložte **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d9645-173">In the **Identity Provider Endpoint** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="d9645-174">c.</span><span class="sxs-lookup"><span data-stu-id="d9645-174">c.</span></span>  <span data-ttu-id="d9645-175">Otevřete **certifikátu s kódováním base-64** stáhli z portálu Azure v **Poznámkový blok**, kopírovat obsah a vložte ji do **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="d9645-175">Open the **base-64 encoded certificate** downloaded from Azure portal in **Notepad**, copy the content, and then paste it into the **X.509 Certificate** textbox.</span></span>
   
    <span data-ttu-id="d9645-176">d.</span><span class="sxs-lookup"><span data-stu-id="d9645-176">d.</span></span>  <span data-ttu-id="d9645-177">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d9645-177">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="d9645-178">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="d9645-178">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d9645-179">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="d9645-179">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d9645-180">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d9645-180">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="d9645-181">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d9645-181">Create an Azure AD test user</span></span>
<span data-ttu-id="d9645-182">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d9645-182">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="d9645-184">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d9645-184">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d9645-185">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d9645-185">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-clicktime-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d9645-187">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d9645-187">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>
    
    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-clicktime-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d9645-189">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d9645-189">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-clicktime-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d9645-191">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d9645-191">In the **User** dialog box, perform the following steps:</span></span>
 
    ![Dialogové okno uživatele](./media/active-directory-saas-clicktime-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d9645-193">a.</span><span class="sxs-lookup"><span data-stu-id="d9645-193">a.</span></span> <span data-ttu-id="d9645-194">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d9645-194">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d9645-195">b.</span><span class="sxs-lookup"><span data-stu-id="d9645-195">b.</span></span> <span data-ttu-id="d9645-196">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d9645-196">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d9645-197">c.</span><span class="sxs-lookup"><span data-stu-id="d9645-197">c.</span></span> <span data-ttu-id="d9645-198">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="d9645-198">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d9645-199">d.</span><span class="sxs-lookup"><span data-stu-id="d9645-199">d.</span></span> <span data-ttu-id="d9645-200">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d9645-200">Click **Create**.</span></span>
 
### <a name="create-a-clicktime-test-user"></a><span data-ttu-id="d9645-201">Vytvoření zkušebního uživatele ClickTime</span><span class="sxs-lookup"><span data-stu-id="d9645-201">Create a ClickTime test user</span></span>

<span data-ttu-id="d9645-202">Pokud chcete povolit uživatelům Azure AD přihlášení do ClickTime, musí být zřízená do ClickTime.</span><span class="sxs-lookup"><span data-stu-id="d9645-202">In order to enable Azure AD users to log into ClickTime, they must be provisioned into ClickTime.</span></span>  
<span data-ttu-id="d9645-203">V případě ClickTime zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="d9645-203">In the case of ClickTime, provisioning is a manual task.</span></span>

> [!NOTE]
> <span data-ttu-id="d9645-204">Můžete použít všechny ostatní ClickTime uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované ClickTime ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d9645-204">You can use any other ClickTime user account creation tools or APIs provided by ClickTime to provision Azure AD user accounts.</span></span>

<span data-ttu-id="d9645-205">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d9645-205">**To provision a user account, perform the following steps:**</span></span>
1. <span data-ttu-id="d9645-206">Přihlaste se k vaší **ClickTime** klienta.</span><span class="sxs-lookup"><span data-stu-id="d9645-206">Log in to your **ClickTime** tenant.</span></span>
2. <span data-ttu-id="d9645-207">Na panelu nástrojů v horní části klikněte na tlačítko **společnosti**a potom klikněte na **osoby**.</span><span class="sxs-lookup"><span data-stu-id="d9645-207">In the toolbar on the top, click **Company**, and then click **People**.</span></span>
   
    <span data-ttu-id="d9645-208">![Lidé](./media/active-directory-saas-clicktime-tutorial/tic777282.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="d9645-208">![People](./media/active-directory-saas-clicktime-tutorial/tic777282.png "People")</span></span>
3. <span data-ttu-id="d9645-209">Klikněte na tlačítko **přidat osobu**.</span><span class="sxs-lookup"><span data-stu-id="d9645-209">Click **Add Person**.</span></span>
   
    <span data-ttu-id="d9645-210">![Přidat osobu](./media/active-directory-saas-clicktime-tutorial/tic777283.png "přidat osoba")</span><span class="sxs-lookup"><span data-stu-id="d9645-210">![Add Person](./media/active-directory-saas-clicktime-tutorial/tic777283.png "Add Person")</span></span>
4. <span data-ttu-id="d9645-211">V části nové osobě proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d9645-211">In the New Person section, perform the following steps:</span></span>
   
    <span data-ttu-id="d9645-212">![Lidé](./media/active-directory-saas-clicktime-tutorial/tic777284.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="d9645-212">![People](./media/active-directory-saas-clicktime-tutorial/tic777284.png "People")</span></span>
   
    <span data-ttu-id="d9645-213">a.</span><span class="sxs-lookup"><span data-stu-id="d9645-213">a.</span></span>  <span data-ttu-id="d9645-214">V **celý název** textovému poli, celý název typ uživatele jako **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="d9645-214">In the **full name** textbox, type full name of user like **Britta Simon**.</span></span> 
  
    <span data-ttu-id="d9645-215">b.</span><span class="sxs-lookup"><span data-stu-id="d9645-215">b.</span></span>  <span data-ttu-id="d9645-216">V **e-mailová adresa** jako typ e-mailu uživatele k textovému poli,  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="d9645-216">In the **email address** textbox, type the email of user like **brittasimon@contoso.com**.</span></span>
       
    > [!NOTE]
    > <span data-ttu-id="d9645-217">Pokud chcete, můžete nastavit další vlastnosti nového objektu osoby.</span><span class="sxs-lookup"><span data-stu-id="d9645-217">If you want to, you can set additional properties of the new person object.</span></span>
   
    <span data-ttu-id="d9645-218">c.</span><span class="sxs-lookup"><span data-stu-id="d9645-218">c.</span></span>  <span data-ttu-id="d9645-219">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d9645-219">Click **Save**.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="d9645-220">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d9645-220">Assign the Azure AD test user</span></span>

<span data-ttu-id="d9645-221">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu ClickTime.</span><span class="sxs-lookup"><span data-stu-id="d9645-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ClickTime.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="d9645-223">**Pokud chcete přiřadit Britta Simon ClickTime, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d9645-223">**To assign Britta Simon to ClickTime, perform the following steps:**</span></span>

1. <span data-ttu-id="d9645-224">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d9645-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d9645-226">V seznamu aplikací vyberte **ClickTime**.</span><span class="sxs-lookup"><span data-stu-id="d9645-226">In the applications list, select **ClickTime**.</span></span>

    ![Odkaz ClickTimne v seznamu aplikací](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_app.png) 

3. <span data-ttu-id="d9645-228">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d9645-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202] 

4. <span data-ttu-id="d9645-230">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d9645-230">Click **Add** button.</span></span> <span data-ttu-id="d9645-231">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d9645-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="d9645-233">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d9645-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d9645-234">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d9645-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d9645-235">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d9645-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="d9645-236">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d9645-236">Test single sign-on</span></span>

<span data-ttu-id="d9645-237">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d9645-237">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d9645-238">Když kliknete na dlaždici ClickTime na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci ClickTime.</span><span class="sxs-lookup"><span data-stu-id="d9645-238">When you click the ClickTime tile in the Access Panel, you should get automatically signed-on to your ClickTime application.</span></span>
<span data-ttu-id="d9645-239">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d9645-239">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d9645-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d9645-240">Additional resources</span></span>

* [<span data-ttu-id="d9645-241">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d9645-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d9645-242">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d9645-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png

