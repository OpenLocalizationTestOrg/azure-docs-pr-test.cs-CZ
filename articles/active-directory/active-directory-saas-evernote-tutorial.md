---
title: 'Kurz: Azure Active Directory integrace s Evernote | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Evernote."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: be94152a84bbbeacb623d7dd8b540e3981931a8e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-evernote"></a><span data-ttu-id="d3f29-103">Kurz: Azure Active Directory integrace s Evernote</span><span class="sxs-lookup"><span data-stu-id="d3f29-103">Tutorial: Azure Active Directory integration with Evernote</span></span>

<span data-ttu-id="d3f29-104">V tomto kurzu zjistěte, jak integrovat Evernote s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d3f29-104">In this tutorial, you learn how to integrate Evernote with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d3f29-105">Integrace Evernote s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d3f29-105">Integrating Evernote with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d3f29-106">Můžete ovládat ve službě Azure AD, který má přístup k Evernote.</span><span class="sxs-lookup"><span data-stu-id="d3f29-106">You can control in Azure AD who has access to Evernote.</span></span>
- <span data-ttu-id="d3f29-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Evernote (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d3f29-107">You can enable your users to automatically get signed-on to Evernote (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="d3f29-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d3f29-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="d3f29-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d3f29-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d3f29-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d3f29-110">Prerequisites</span></span>

<span data-ttu-id="d3f29-111">Konfigurace integrace Azure AD s Evernote, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="d3f29-111">To configure Azure AD integration with Evernote, you need the following items:</span></span>

- <span data-ttu-id="d3f29-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d3f29-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d3f29-113">Evernote jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d3f29-113">An Evernote single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d3f29-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d3f29-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d3f29-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d3f29-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d3f29-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d3f29-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d3f29-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d3f29-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d3f29-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d3f29-118">Scenario description</span></span>
<span data-ttu-id="d3f29-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d3f29-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d3f29-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d3f29-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d3f29-121">Přidání Evernote z Galerie</span><span class="sxs-lookup"><span data-stu-id="d3f29-121">Adding Evernote from the gallery</span></span>
2. <span data-ttu-id="d3f29-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d3f29-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-evernote-from-the-gallery"></a><span data-ttu-id="d3f29-123">Přidání Evernote z Galerie</span><span class="sxs-lookup"><span data-stu-id="d3f29-123">Adding Evernote from the gallery</span></span>
<span data-ttu-id="d3f29-124">Při konfiguraci integrace Evernote do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Evernote z galerie.</span><span class="sxs-lookup"><span data-stu-id="d3f29-124">To configure the integration of Evernote into Azure AD, you need to add Evernote from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d3f29-125">**Pokud chcete přidat Evernote z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d3f29-125">**To add Evernote from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d3f29-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d3f29-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="d3f29-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d3f29-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d3f29-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d3f29-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="d3f29-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d3f29-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="d3f29-133">Do vyhledávacího pole zadejte **Evernote**, vyberte **Evernote** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d3f29-133">In the search box, type **Evernote**, select **Evernote** from result panel then click **Add** button to add the application.</span></span>

    ![Evernote v seznamu výsledků](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="d3f29-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d3f29-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="d3f29-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Evernote podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="d3f29-136">In this section, you configure and test Azure AD single sign-on with Evernote based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d3f29-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Evernote je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d3f29-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Evernote is to a user in Azure AD.</span></span> <span data-ttu-id="d3f29-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Evernote musí navázat.</span><span class="sxs-lookup"><span data-stu-id="d3f29-138">In other words, a link relationship between an Azure AD user and the related user in Evernote needs to be established.</span></span>

<span data-ttu-id="d3f29-139">V Evernote, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="d3f29-139">In Evernote, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d3f29-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Evernote, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="d3f29-140">To configure and test Azure AD single sign-on with Evernote, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d3f29-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="d3f29-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d3f29-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d3f29-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d3f29-143">**[Vytvořit testovací uživatele s Evernote](#create-an-evernote-test-user)**  – Pokud chcete mít protějšek Britta Simon v Evernote propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="d3f29-143">**[Create an Evernote test user](#create-an-evernote-test-user)** - to have a counterpart of Britta Simon in Evernote that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d3f29-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d3f29-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d3f29-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d3f29-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="d3f29-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d3f29-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="d3f29-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Evernote.</span><span class="sxs-lookup"><span data-stu-id="d3f29-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Evernote application.</span></span>

<span data-ttu-id="d3f29-148">**Ke konfiguraci Azure AD jednotné přihlašování s Evernote, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d3f29-148">**To configure Azure AD single sign-on with Evernote, perform the following steps:**</span></span>

1. <span data-ttu-id="d3f29-149">Na portálu Azure na **Evernote** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d3f29-149">In the Azure portal, on the **Evernote** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="d3f29-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d3f29-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_samlbase.png)

3. <span data-ttu-id="d3f29-153">Na **Evernote domény a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace v režimu rozšíření IDP iniciované:</span><span class="sxs-lookup"><span data-stu-id="d3f29-153">On the **Evernote Domain and URLs** section, perform the following steps if you wish to configure the application in IDP initiated mode:</span></span>

    ![Evernote domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_url.png)

    <span data-ttu-id="d3f29-155">V **identifikátor** textovému poli, zadejte adresu URL:`https://www.evernote.com/saml2`</span><span class="sxs-lookup"><span data-stu-id="d3f29-155">In the **Identifier** textbox, type the URL: `https://www.evernote.com/saml2`</span></span>

4. <span data-ttu-id="d3f29-156">Zkontrolujte **zobrazit upřesňující nastavení adresy URL** a provést následující krok, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="d3f29-156">Check **Show advanced URL settings** and perform the following step if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Evernote domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_url1.png)

    <span data-ttu-id="d3f29-158">V **přihlásit na adrese URL** textovému poli, zadejte adresu URL:`https://www.evernote.com/Login.action`</span><span class="sxs-lookup"><span data-stu-id="d3f29-158">In the **Sign on URL** textbox, type the URL: `https://www.evernote.com/Login.action`</span></span>   

5. <span data-ttu-id="d3f29-159">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="d3f29-159">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_certificate.png) 

6. <span data-ttu-id="d3f29-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d3f29-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-evernote-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="d3f29-163">Na **Evernote konfigurace** klikněte na tlačítko **konfigurace Evernote** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="d3f29-163">On the **Evernote Configuration** section, click **Configure Evernote** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d3f29-164">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="d3f29-164">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace Evernote](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_configure.png) 

8. <span data-ttu-id="d3f29-166">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Evernote.</span><span class="sxs-lookup"><span data-stu-id="d3f29-166">In a different web browser window, log into your Evernote company site as an administrator.</span></span>

9. <span data-ttu-id="d3f29-167">Přejděte na **'konzoly pro správu.**</span><span class="sxs-lookup"><span data-stu-id="d3f29-167">Go to **'Admin Console'**</span></span>

    ![Konzole pro správu](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_adminconsole.png)

10. <span data-ttu-id="d3f29-169">Z **'Konzoly pro správu'**, přejděte na **, zabezpečení,** a vyberte **' jednotné přihlašování.**</span><span class="sxs-lookup"><span data-stu-id="d3f29-169">From the **'Admin Console'**, go to **‘Security’** and select **‘Single Sign-On’**</span></span>

    ![Nastavení jednotného přihlašování](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_sso.png)

11. <span data-ttu-id="d3f29-171">Nakonfigurujte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="d3f29-171">Configure the following values:</span></span>

    ![Nastavení certifikátu](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_certx.png)
    
    <span data-ttu-id="d3f29-173">a.</span><span class="sxs-lookup"><span data-stu-id="d3f29-173">a.</span></span>  <span data-ttu-id="d3f29-174">**Povolit jednotné přihlašování:** je ve výchozím nastavení povolené jednotné přihlašování (klikněte na tlačítko **zakázat jednotné přihlašování** odebrat požadavek na jednotné přihlašování)</span><span class="sxs-lookup"><span data-stu-id="d3f29-174">**Enable SSO:** SSO is enabled by default (Click **Disable Single Sign-on** to remove the SSO requirement)</span></span>

    <span data-ttu-id="d3f29-175">b.</span><span class="sxs-lookup"><span data-stu-id="d3f29-175">b.</span></span> <span data-ttu-id="d3f29-176">Vložení **SAML jednotné přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali z portálu Azure do **URL požadavku HTTP SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="d3f29-176">Paste **SAML Single sign-on Service URL** value, which you have copied from the Azure portal into the **SAML HTTP Request URL** textbox.</span></span>

    <span data-ttu-id="d3f29-177">c.</span><span class="sxs-lookup"><span data-stu-id="d3f29-177">c.</span></span> <span data-ttu-id="d3f29-178">Otevřete certifikát stažený z Azure AD v programu Poznámkový blok a zkopírujte obsah, včetně "Začít certifikát" a "END CERTIFICATE" a vložte ji do **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="d3f29-178">Open the downloaded certificate from Azure AD in a notepad and copy the content including "BEGIN CERTIFICATE" and "END CERTIFICATE" and paste it into the **X.509 Certificate** textbox.</span></span> 

    <span data-ttu-id="d3f29-179">d.Click **uložit změny**</span><span class="sxs-lookup"><span data-stu-id="d3f29-179">d.Click **Save Changes**</span></span>

> [!TIP]
> <span data-ttu-id="d3f29-180">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="d3f29-180">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d3f29-181">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="d3f29-181">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d3f29-182">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d3f29-182">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="d3f29-183">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d3f29-183">Create an Azure AD test user</span></span>

<span data-ttu-id="d3f29-184">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d3f29-184">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="d3f29-186">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d3f29-186">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d3f29-187">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d3f29-187">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-evernote-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="d3f29-189">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d3f29-189">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-evernote-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="d3f29-191">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d3f29-191">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-evernote-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="d3f29-193">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d3f29-193">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-evernote-tutorial/create_aaduser_04.png)

    <span data-ttu-id="d3f29-195">a.</span><span class="sxs-lookup"><span data-stu-id="d3f29-195">a.</span></span> <span data-ttu-id="d3f29-196">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d3f29-196">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d3f29-197">b.</span><span class="sxs-lookup"><span data-stu-id="d3f29-197">b.</span></span> <span data-ttu-id="d3f29-198">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d3f29-198">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="d3f29-199">c.</span><span class="sxs-lookup"><span data-stu-id="d3f29-199">c.</span></span> <span data-ttu-id="d3f29-200">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="d3f29-200">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="d3f29-201">d.</span><span class="sxs-lookup"><span data-stu-id="d3f29-201">d.</span></span> <span data-ttu-id="d3f29-202">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d3f29-202">Click **Create**.</span></span>
 
### <a name="create-an-evernote-test-user"></a><span data-ttu-id="d3f29-203">Vytvořit uživatele s Evernote testu</span><span class="sxs-lookup"><span data-stu-id="d3f29-203">Create an Evernote test user</span></span>

<span data-ttu-id="d3f29-204">Pokud chcete povolit uživatelům Azure AD přihlášení do Evernote, musí být zřízená do Evernote.</span><span class="sxs-lookup"><span data-stu-id="d3f29-204">In order to enable Azure AD users to log into Evernote, they must be provisioned into Evernote.</span></span>  
<span data-ttu-id="d3f29-205">V případě Evernote zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="d3f29-205">In the case of Evernote, provisioning is a manual task.</span></span>

<span data-ttu-id="d3f29-206">**Ke zřízení uživatelských účtů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d3f29-206">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="d3f29-207">Přihlaste se k serveru vaší společnosti Evernote jako správce.</span><span class="sxs-lookup"><span data-stu-id="d3f29-207">Log in to your Evernote company site as an administrator.</span></span>

2. <span data-ttu-id="d3f29-208">Klikněte **'Konzoly pro správu'**.</span><span class="sxs-lookup"><span data-stu-id="d3f29-208">Click the **'Admin Console'**.</span></span>

    ![Konzole pro správu](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_adminconsole.png)

3. <span data-ttu-id="d3f29-210">Z **'Konzoly pro správu'**, přejděte na **"Přidat uživatele,**.</span><span class="sxs-lookup"><span data-stu-id="d3f29-210">From the **'Admin Console'**, go to **‘Add users’**.</span></span>

    ![Přidat testUser](./media/active-directory-saas-evernote-tutorial/create_aaduser_0001.png)

4. <span data-ttu-id="d3f29-212">**Přidání členů týmu** v **e-mailu** textovému poli, zadejte e-mailovou adresu uživatelského účtu a klikněte na **pozvat.**</span><span class="sxs-lookup"><span data-stu-id="d3f29-212">**Add team members** in the **Email** textbox, type the email address of user account and click **Invite.**</span></span>

    ![Přidat testUser](./media/active-directory-saas-evernote-tutorial/create_aaduser_0002.png)
    
5. <span data-ttu-id="d3f29-214">Po odeslání pozvánky držitel účtu Azure Active Directory obdrží e-mailu pro přijetí pozvánky.</span><span class="sxs-lookup"><span data-stu-id="d3f29-214">After invitation is sent, the Azure Active Directory account holder will receive an email to accept the invitation.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="d3f29-215">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d3f29-215">Assign the Azure AD test user</span></span>

<span data-ttu-id="d3f29-216">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Evernote.</span><span class="sxs-lookup"><span data-stu-id="d3f29-216">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Evernote.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="d3f29-218">**Pokud chcete přiřadit Britta Simon Evernote, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d3f29-218">**To assign Britta Simon to Evernote, perform the following steps:**</span></span>

1. <span data-ttu-id="d3f29-219">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d3f29-219">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d3f29-221">V seznamu aplikací vyberte **Evernote**.</span><span class="sxs-lookup"><span data-stu-id="d3f29-221">In the applications list, select **Evernote**.</span></span>

    ![V seznamu aplikací na Evernote odkaz](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_app.png)  

3. <span data-ttu-id="d3f29-223">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d3f29-223">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="d3f29-225">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d3f29-225">Click **Add** button.</span></span> <span data-ttu-id="d3f29-226">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d3f29-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="d3f29-228">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d3f29-228">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d3f29-229">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d3f29-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d3f29-230">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d3f29-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="d3f29-231">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d3f29-231">Test single sign-on</span></span>

<span data-ttu-id="d3f29-232">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d3f29-232">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d3f29-233">Když kliknete na dlaždici Evernote na přístupovém panelu, jste měli získat přihlášení k aplikaci Evernote.</span><span class="sxs-lookup"><span data-stu-id="d3f29-233">When you click the Evernote tile in the Access Panel, you should get signed-on to your Evernote application.</span></span> <span data-ttu-id="d3f29-234">Se budete přihlašovat jako účet, ale pak nutné se přihlásit pomocí svého osobního účtu organizace.</span><span class="sxs-lookup"><span data-stu-id="d3f29-234">You'll be logging in as an Organization account but then need to log in with your personal account.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d3f29-235">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d3f29-235">Additional resources</span></span>

* [<span data-ttu-id="d3f29-236">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d3f29-236">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d3f29-237">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d3f29-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_203.png

