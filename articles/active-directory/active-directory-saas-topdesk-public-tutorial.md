---
title: "Kurz: Azure Active Directory integrace s TOPdesk - veřejné | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a TOPdesk - veřejné."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: f21fe0b363776974108ff460060e4c15a51a58a3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a><span data-ttu-id="20ff5-103">Kurz: Azure Active Directory integrace s TOPdesk - veřejné</span><span class="sxs-lookup"><span data-stu-id="20ff5-103">Tutorial: Azure Active Directory integration with TOPdesk - Public</span></span>

<span data-ttu-id="20ff5-104">V tomto kurzu zjistěte, jak integrovat TOPdesk - veřejné službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="20ff5-104">In this tutorial, you learn how to integrate TOPdesk - Public with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="20ff5-105">Integrace TOPdesk - veřejné s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="20ff5-105">Integrating TOPdesk - Public with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="20ff5-106">Můžete ovládat ve službě Azure AD, který má přístup k TOPdesk - veřejné.</span><span class="sxs-lookup"><span data-stu-id="20ff5-106">You can control in Azure AD who has access to TOPdesk - Public.</span></span>
- <span data-ttu-id="20ff5-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k TOPdesk - veřejné (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="20ff5-107">You can enable your users to automatically get signed-on to TOPdesk - Public (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="20ff5-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="20ff5-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="20ff5-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="20ff5-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="20ff5-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="20ff5-110">Prerequisites</span></span>

<span data-ttu-id="20ff5-111">Konfigurace integrace Azure AD s TOPdesk - veřejné, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="20ff5-111">To configure Azure AD integration with TOPdesk - Public, you need the following items:</span></span>

- <span data-ttu-id="20ff5-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="20ff5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="20ff5-113">TOPdesk - veřejné jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="20ff5-113">A TOPdesk - Public single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="20ff5-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="20ff5-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="20ff5-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="20ff5-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="20ff5-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="20ff5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="20ff5-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="20ff5-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="20ff5-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="20ff5-118">Scenario description</span></span>
<span data-ttu-id="20ff5-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="20ff5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="20ff5-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="20ff5-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="20ff5-121">Přidání TOPdesk - veřejné z Galerie</span><span class="sxs-lookup"><span data-stu-id="20ff5-121">Adding TOPdesk - Public from the gallery</span></span>
2. <span data-ttu-id="20ff5-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="20ff5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-topdesk---public-from-the-gallery"></a><span data-ttu-id="20ff5-123">Přidání TOPdesk - veřejné z Galerie</span><span class="sxs-lookup"><span data-stu-id="20ff5-123">Adding TOPdesk - Public from the gallery</span></span>
<span data-ttu-id="20ff5-124">Konfigurace integrace TOPdesk - veřejné do služby Azure AD, potřebujete přidat TOPdesk - veřejné z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="20ff5-124">To configure the integration of TOPdesk - Public into Azure AD, you need to add TOPdesk - Public from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="20ff5-125">**Chcete-li přidat TOPdesk - veřejné z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="20ff5-125">**To add TOPdesk - Public from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="20ff5-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="20ff5-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="20ff5-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="20ff5-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="20ff5-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="20ff5-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="20ff5-133">Do vyhledávacího pole zadejte **TOPdesk - veřejné**, vyberte **TOPdesk - veřejné** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="20ff5-133">In the search box, type **TOPdesk - Public**, select **TOPdesk - Public** from result panel then click **Add** button to add the application.</span></span>

    ![TOPdesk - veřejné v seznamu výsledků](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="20ff5-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="20ff5-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="20ff5-136">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s TOPdesk – veřejné podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="20ff5-136">In this section, you configure and test Azure AD single sign-on with TOPdesk - Public based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="20ff5-137">Pro jednotné přihlašování pro práci Azure AD je potřeba vědět, jaké příslušného uživatele v TOPdesk – veřejné je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="20ff5-137">For single sign-on to work, Azure AD needs to know what the counterpart user in TOPdesk - Public is to a user in Azure AD.</span></span> <span data-ttu-id="20ff5-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v TOPdesk - veřejné musí navázat.</span><span class="sxs-lookup"><span data-stu-id="20ff5-138">In other words, a link relationship between an Azure AD user and the related user in TOPdesk - Public needs to be established.</span></span>

<span data-ttu-id="20ff5-139">V TOPdesk - veřejným přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="20ff5-139">In TOPdesk - Public, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="20ff5-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s TOPdesk - veřejné, je potřeba provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="20ff5-140">To configure and test Azure AD single sign-on with TOPdesk - Public, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="20ff5-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="20ff5-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="20ff5-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="20ff5-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="20ff5-143">**[Vytvoření TOPdesk - veřejné testovacího uživatele](#create-a-topdesk---public-test-user)**  – Pokud chcete mít protějšek Britta Simon v TOPdesk - veřejné propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="20ff5-143">**[Create a TOPdesk - Public test user](#create-a-topdesk---public-test-user)** - to have a counterpart of Britta Simon in TOPdesk - Public that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="20ff5-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="20ff5-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="20ff5-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="20ff5-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="20ff5-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="20ff5-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="20ff5-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v vaší TOPdesk - veřejné aplikace.</span><span class="sxs-lookup"><span data-stu-id="20ff5-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TOPdesk - Public application.</span></span>

<span data-ttu-id="20ff5-148">**Ke konfiguraci Azure AD jednotné přihlašování s TOPdesk - veřejné, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="20ff5-148">**To configure Azure AD single sign-on with TOPdesk - Public, perform the following steps:**</span></span>

1. <span data-ttu-id="20ff5-149">Na portálu Azure na **TOPdesk - veřejné** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-149">In the Azure portal, on the **TOPdesk - Public** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="20ff5-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="20ff5-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_samlbase.png)

3. <span data-ttu-id="20ff5-153">Na **TOPdesk - veřejnou doménu a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="20ff5-153">On the **TOPdesk - Public Domain and URLs** section, perform the following steps:</span></span>

    ![Jednotné přihlašování informace TOPdesk - veřejnou doménu a adresy URL](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_url.png)

    <span data-ttu-id="20ff5-155">a.</span><span class="sxs-lookup"><span data-stu-id="20ff5-155">a.</span></span> <span data-ttu-id="20ff5-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.topdesk.net`</span><span class="sxs-lookup"><span data-stu-id="20ff5-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.topdesk.net`</span></span>
    
    <span data-ttu-id="20ff5-157">b.</span><span class="sxs-lookup"><span data-stu-id="20ff5-157">b.</span></span> <span data-ttu-id="20ff5-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.topdesk.net/tas/public/login/verify`</span><span class="sxs-lookup"><span data-stu-id="20ff5-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.topdesk.net/tas/public/login/verify`</span></span>

    <span data-ttu-id="20ff5-159">c.</span><span class="sxs-lookup"><span data-stu-id="20ff5-159">c.</span></span> <span data-ttu-id="20ff5-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.topdesk.net/tas/public/login/saml`</span><span class="sxs-lookup"><span data-stu-id="20ff5-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyname>.topdesk.net/tas/public/login/saml`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="20ff5-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="20ff5-161">These values are not real.</span></span> <span data-ttu-id="20ff5-162">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="20ff5-162">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="20ff5-163">Adresa URL odpovědi je explaned později v kurzu.</span><span class="sxs-lookup"><span data-stu-id="20ff5-163">Reply URL is explaned later in tutorial.</span></span> <span data-ttu-id="20ff5-164">Obraťte se na [TOPdesk - tým podpory veřejné klienta](https://help.topdesk.com/saas/enterprise/user/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="20ff5-164">Contact [TOPdesk - Public Client support team](https://help.topdesk.com/saas/enterprise/user/) to get these values.</span></span>  

4. <span data-ttu-id="20ff5-165">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="20ff5-165">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_certificate.png) 

5. <span data-ttu-id="20ff5-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="20ff5-167">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="20ff5-169">Na **TOPdesk - veřejné konfigurace** klikněte na tlačítko **konfigurace TOPdesk - veřejné** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="20ff5-169">On the **TOPdesk - Public Configuration** section, click **Configure TOPdesk - Public** to open **Configure sign-on** window.</span></span> <span data-ttu-id="20ff5-170">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="20ff5-170">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![TOPdesk - veřejné konfigurace](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_configure.png) 

7. <span data-ttu-id="20ff5-172">Přihlaste se k vaší **TOPdesk - veřejné** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="20ff5-172">Sign on to your **TOPdesk - Public** company site as an administrator.</span></span>

8. <span data-ttu-id="20ff5-173">V **TOPdesk** nabídky, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-173">In the **TOPdesk** menu, click **Settings**.</span></span>
   
    <span data-ttu-id="20ff5-174">![Nastavení](./media/active-directory-saas-topdesk-public-tutorial/ic790598.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="20ff5-174">![Settings](./media/active-directory-saas-topdesk-public-tutorial/ic790598.png "Settings")</span></span>

9. <span data-ttu-id="20ff5-175">Klikněte na tlačítko **nastavení přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-175">Click **Login Settings**.</span></span>
   
    <span data-ttu-id="20ff5-176">![Nastavení přihlášení](./media/active-directory-saas-topdesk-public-tutorial/ic790599.png "nastavení přihlášení")</span><span class="sxs-lookup"><span data-stu-id="20ff5-176">![Login Settings](./media/active-directory-saas-topdesk-public-tutorial/ic790599.png "Login Settings")</span></span>

10. <span data-ttu-id="20ff5-177">Rozbalte **nastavení přihlášení** nabídce a pak klikněte na tlačítko **Obecné**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-177">Expand the **Login Settings** menu, and then click **General**.</span></span>
   
    <span data-ttu-id="20ff5-178">![Obecné](./media/active-directory-saas-topdesk-public-tutorial/ic790600.png "obecné")</span><span class="sxs-lookup"><span data-stu-id="20ff5-178">![General](./media/active-directory-saas-topdesk-public-tutorial/ic790600.png "General")</span></span>

11. <span data-ttu-id="20ff5-179">V **veřejné** části **SAML přihlášení** konfigurace části, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="20ff5-179">In the **Public** section of the **SAML login** configuration section, perform the following steps:</span></span>
   
    <span data-ttu-id="20ff5-180">![Technické nastavení](./media/active-directory-saas-topdesk-public-tutorial/ic790601.png "technické nastavení")</span><span class="sxs-lookup"><span data-stu-id="20ff5-180">![Technical Settings](./media/active-directory-saas-topdesk-public-tutorial/ic790601.png "Technical Settings")</span></span>
   
    <span data-ttu-id="20ff5-181">a.</span><span class="sxs-lookup"><span data-stu-id="20ff5-181">a.</span></span> <span data-ttu-id="20ff5-182">Klikněte na tlačítko **Stáhnout** ke stažení souboru metadat veřejné a poté je uložit místně v počítači.</span><span class="sxs-lookup"><span data-stu-id="20ff5-182">Click **Download** to download the public metadata file, and then save it locally on your computer.</span></span>
   
    <span data-ttu-id="20ff5-183">b.</span><span class="sxs-lookup"><span data-stu-id="20ff5-183">b.</span></span> <span data-ttu-id="20ff5-184">Otevřete soubor stažený metadat a vyhledejte **AssertionConsumerService** uzlu.</span><span class="sxs-lookup"><span data-stu-id="20ff5-184">Open the downloaded metadata file, and then locate the **AssertionConsumerService** node.</span></span>

    <span data-ttu-id="20ff5-185">![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/ic790619.png "AssertionConsumerService")</span><span class="sxs-lookup"><span data-stu-id="20ff5-185">![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/ic790619.png "AssertionConsumerService")</span></span>
   
    <span data-ttu-id="20ff5-186">c.</span><span class="sxs-lookup"><span data-stu-id="20ff5-186">c.</span></span> <span data-ttu-id="20ff5-187">Kopírování **AssertionConsumerService** hodnotu, vložte tuto hodnotu v **adresa URL odpovědi** textového pole v **TOPdesk - veřejnou doménu a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="20ff5-187">Copy the **AssertionConsumerService** value, paste this value in the **Reply URL** textbox in **TOPdesk - Public Domain and URLs** section.</span></span>      
   
12. <span data-ttu-id="20ff5-188">Pokud chcete vytvořit soubor s certifikátem, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="20ff5-188">To create a certificate file, perform the following steps:</span></span>
    
    <span data-ttu-id="20ff5-189">![Certifikát](./media/active-directory-saas-topdesk-public-tutorial/ic790606.png "certifikátu")</span><span class="sxs-lookup"><span data-stu-id="20ff5-189">![Certificate](./media/active-directory-saas-topdesk-public-tutorial/ic790606.png "Certificate")</span></span>
    
    <span data-ttu-id="20ff5-190">a.</span><span class="sxs-lookup"><span data-stu-id="20ff5-190">a.</span></span> <span data-ttu-id="20ff5-191">Otevřete soubor metadat stažený z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="20ff5-191">Open the downloaded metadata file from Azure portal.</span></span>
    
    <span data-ttu-id="20ff5-192">b.</span><span class="sxs-lookup"><span data-stu-id="20ff5-192">b.</span></span> <span data-ttu-id="20ff5-193">Rozbalte **RoleDescriptor** uzlu, který má **xsi: type** z **dodáni: ApplicationServiceType**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-193">Expand the **RoleDescriptor** node that has a **xsi:type** of **fed:ApplicationServiceType**.</span></span>
    
    <span data-ttu-id="20ff5-194">c.</span><span class="sxs-lookup"><span data-stu-id="20ff5-194">c.</span></span> <span data-ttu-id="20ff5-195">Zkopírujte hodnotu **certifikátu x 509** uzlu.</span><span class="sxs-lookup"><span data-stu-id="20ff5-195">Copy the value of the **X509Certificate** node.</span></span>
    
    <span data-ttu-id="20ff5-196">d.</span><span class="sxs-lookup"><span data-stu-id="20ff5-196">d.</span></span> <span data-ttu-id="20ff5-197">Uložit zkopírovaný **certifikátu x 509** hodnota místně na vašem počítači v souboru.</span><span class="sxs-lookup"><span data-stu-id="20ff5-197">Save the copied **X509Certificate** value locally on your computer in a file.</span></span>

13. <span data-ttu-id="20ff5-198">V **veřejné** klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-198">In the **Public** section, click **Add**.</span></span>
    
    <span data-ttu-id="20ff5-199">![Přihlašování SAML](./media/active-directory-saas-topdesk-public-tutorial/ic790625.png "SAML přihlášení")</span><span class="sxs-lookup"><span data-stu-id="20ff5-199">![SAML Login](./media/active-directory-saas-topdesk-public-tutorial/ic790625.png "SAML Login")</span></span>

14. <span data-ttu-id="20ff5-200">Na **pomocníka konfigurace SAML** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="20ff5-200">On the **SAML configuration assistant** dialog page, perform the following steps:</span></span>
    
    <span data-ttu-id="20ff5-201">![Pomocník pro konfigurace SAML](./media/active-directory-saas-topdesk-public-tutorial/ic790608.png "pomocníka konfigurace SAML")</span><span class="sxs-lookup"><span data-stu-id="20ff5-201">![SAML Configuration Assistant](./media/active-directory-saas-topdesk-public-tutorial/ic790608.png "SAML Configuration Assistant")</span></span>
    
    <span data-ttu-id="20ff5-202">a.</span><span class="sxs-lookup"><span data-stu-id="20ff5-202">a.</span></span> <span data-ttu-id="20ff5-203">K odeslání souboru metadat stažený z portálu Azure, v části **federačních metadat**, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-203">To upload your downloaded metadata file from Azure portal, under **Federation Metadata**, click **Browse**.</span></span>

    <span data-ttu-id="20ff5-204">b.</span><span class="sxs-lookup"><span data-stu-id="20ff5-204">b.</span></span> <span data-ttu-id="20ff5-205">Nahrát soubor certifikátu, v části **certifikát (RSA)**, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-205">To upload your certificate file, under **Certificate (RSA)**, click **Browse**.</span></span>

    <span data-ttu-id="20ff5-206">c.</span><span class="sxs-lookup"><span data-stu-id="20ff5-206">c.</span></span> <span data-ttu-id="20ff5-207">Nahrát soubor logo jste získali od týmu podpory TOPdesk, v části **Logo ikonu**, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-207">To upload the logo file you got from the TOPdesk support team, under **Logo icon**, click **Browse**.</span></span>

    <span data-ttu-id="20ff5-208">d.</span><span class="sxs-lookup"><span data-stu-id="20ff5-208">d.</span></span> <span data-ttu-id="20ff5-209">V **atribut uživatelského jména** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="20ff5-209">In the **User name attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>

    <span data-ttu-id="20ff5-210">e.</span><span class="sxs-lookup"><span data-stu-id="20ff5-210">e.</span></span> <span data-ttu-id="20ff5-211">V **zobrazovaný název** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="20ff5-211">In the **Display name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="20ff5-212">f.</span><span class="sxs-lookup"><span data-stu-id="20ff5-212">f.</span></span> <span data-ttu-id="20ff5-213">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-213">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="20ff5-214">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="20ff5-214">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="20ff5-215">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="20ff5-215">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="20ff5-216">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="20ff5-216">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="20ff5-217">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="20ff5-217">Create an Azure AD test user</span></span>

<span data-ttu-id="20ff5-218">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="20ff5-218">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="20ff5-220">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="20ff5-220">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="20ff5-221">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="20ff5-221">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="20ff5-223">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-223">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="20ff5-225">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="20ff5-225">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="20ff5-227">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="20ff5-227">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_04.png)

    <span data-ttu-id="20ff5-229">a.</span><span class="sxs-lookup"><span data-stu-id="20ff5-229">a.</span></span> <span data-ttu-id="20ff5-230">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-230">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="20ff5-231">b.</span><span class="sxs-lookup"><span data-stu-id="20ff5-231">b.</span></span> <span data-ttu-id="20ff5-232">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="20ff5-232">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="20ff5-233">c.</span><span class="sxs-lookup"><span data-stu-id="20ff5-233">c.</span></span> <span data-ttu-id="20ff5-234">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="20ff5-234">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="20ff5-235">d.</span><span class="sxs-lookup"><span data-stu-id="20ff5-235">d.</span></span> <span data-ttu-id="20ff5-236">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-236">Click **Create**.</span></span>
 
### <a name="create-a-topdesk---public-test-user"></a><span data-ttu-id="20ff5-237">Vytvoření TOPdesk - veřejné testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="20ff5-237">Create a TOPdesk - Public test user</span></span>

<span data-ttu-id="20ff5-238">Chcete-li povolit uživatelům Azure AD přihlášení do TOPdesk - veřejné, musí být zřízená do TOPdesk - veřejné.</span><span class="sxs-lookup"><span data-stu-id="20ff5-238">In order to enable Azure AD users to log into TOPdesk - Public, they must be provisioned into TOPdesk - Public.</span></span>  
<span data-ttu-id="20ff5-239">V případě TOPdesk - veřejné, zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="20ff5-239">In the case of TOPdesk - Public, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="20ff5-240">Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="20ff5-240">To configure user provisioning, perform the following steps:</span></span>
1. <span data-ttu-id="20ff5-241">Přihlaste se k vaší **TOPdesk - veřejné** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="20ff5-241">Sign on to your **TOPdesk - Public** company site as administrator.</span></span>

2. <span data-ttu-id="20ff5-242">V nabídce v horní části, klikněte na tlačítko **TOPdesk \> nový \> soubory podpory \> osoba**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-242">In the menu on the top, click **TOPdesk \> New \> Support Files \> Person**.</span></span>
   
    <span data-ttu-id="20ff5-243">![Osoba](./media/active-directory-saas-topdesk-public-tutorial/ic790628.png "osoba")</span><span class="sxs-lookup"><span data-stu-id="20ff5-243">![Person](./media/active-directory-saas-topdesk-public-tutorial/ic790628.png "Person")</span></span>

3. <span data-ttu-id="20ff5-244">V dialogovém okně nové osobě proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="20ff5-244">On the New Person dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="20ff5-245">![Nové osobě](./media/active-directory-saas-topdesk-public-tutorial/ic790629.png "nové osobě")</span><span class="sxs-lookup"><span data-stu-id="20ff5-245">![New Person](./media/active-directory-saas-topdesk-public-tutorial/ic790629.png "New Person")</span></span>
   
    <span data-ttu-id="20ff5-246">a.</span><span class="sxs-lookup"><span data-stu-id="20ff5-246">a.</span></span> <span data-ttu-id="20ff5-247">Klikněte na kartu Obecné.</span><span class="sxs-lookup"><span data-stu-id="20ff5-247">Click the General tab.</span></span>

    <span data-ttu-id="20ff5-248">b.</span><span class="sxs-lookup"><span data-stu-id="20ff5-248">b.</span></span> <span data-ttu-id="20ff5-249">V **Přezdívka** textovému poli, zadejte příjmení uživatele jako Simon</span><span class="sxs-lookup"><span data-stu-id="20ff5-249">In the **Surname** textbox, type Surname of the user like Simon</span></span>
 
    <span data-ttu-id="20ff5-250">c.</span><span class="sxs-lookup"><span data-stu-id="20ff5-250">c.</span></span> <span data-ttu-id="20ff5-251">Vyberte **lokality** pro účet.</span><span class="sxs-lookup"><span data-stu-id="20ff5-251">Select a **Site** for the account.</span></span>
 
    <span data-ttu-id="20ff5-252">d.</span><span class="sxs-lookup"><span data-stu-id="20ff5-252">d.</span></span> <span data-ttu-id="20ff5-253">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-253">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="20ff5-254">Můžete použít všechny ostatní TOPdesk - zadáním hodnoty nástroje pro tvorbu veřejné uživatelského účtu nebo rozhraní API poskytované TOPdesk - veřejné ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="20ff5-254">You can use any other TOPdesk - Public user account creation tools or APIs provided by TOPdesk - Public to provision Azure AD user accounts.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="20ff5-255">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="20ff5-255">Assign the Azure AD test user</span></span>

<span data-ttu-id="20ff5-256">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu TOPdesk - veřejné.</span><span class="sxs-lookup"><span data-stu-id="20ff5-256">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TOPdesk - Public.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="20ff5-258">**Britta Simon přiřadit TOPdesk - veřejné, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="20ff5-258">**To assign Britta Simon to TOPdesk - Public, perform the following steps:**</span></span>

1. <span data-ttu-id="20ff5-259">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-259">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="20ff5-261">V seznamu aplikací vyberte **TOPdesk - veřejné**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-261">In the applications list, select **TOPdesk - Public**.</span></span>

    ![TOPdesk - veřejné odkaz v seznamu aplikací](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_app.png)  

3. <span data-ttu-id="20ff5-263">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="20ff5-263">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="20ff5-265">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="20ff5-265">Click **Add** button.</span></span> <span data-ttu-id="20ff5-266">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="20ff5-266">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="20ff5-268">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="20ff5-268">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="20ff5-269">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="20ff5-269">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="20ff5-270">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="20ff5-270">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="20ff5-271">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="20ff5-271">Test single sign-on</span></span>

<span data-ttu-id="20ff5-272">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="20ff5-272">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="20ff5-273">Při kliknutí TOPdesk – veřejné dlaždici na přístupovém panelu, jste měli získat automaticky přihlášení k vaší TOPdesk - veřejné aplikace.</span><span class="sxs-lookup"><span data-stu-id="20ff5-273">When you click the TOPdesk - Public tile in the Access Panel, you should get automatically signed-on to your TOPdesk - Public application.</span></span>
<span data-ttu-id="20ff5-274">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="20ff5-274">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="20ff5-275">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="20ff5-275">Additional resources</span></span>

* [<span data-ttu-id="20ff5-276">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="20ff5-276">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="20ff5-277">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="20ff5-277">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_203.png

