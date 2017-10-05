---
title: "Kurz: Azure Active Directory integrace s prodejní informací – správu informace | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a prodejní informací – informace správy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5ff49168-ef81-4169-8e5e-dc86e24dd5e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 1ab8b7e98324ba4f4ae95775f89df0461058fe4b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-infor-retail--information-management"></a><span data-ttu-id="b8eb3-103">Kurz: Azure Active Directory integrace s prodejní informací – informace správy</span><span class="sxs-lookup"><span data-stu-id="b8eb3-103">Tutorial: Azure Active Directory integration with Infor Retail – Information Management</span></span>

<span data-ttu-id="b8eb3-104">V tomto kurzu zjistěte, jak integrovat prodejní informací – informace správy pomocí služby Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b8eb3-104">In this tutorial, you learn how to integrate Infor Retail – Information Management with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b8eb3-105">Integrace informací prodejní – informace správy s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b8eb3-105">Integrating Infor Retail – Information Management with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b8eb3-106">Můžete ovládat ve službě Azure AD, který má přístup k informací prodejní – informace správy.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-106">You can control in Azure AD who has access to Infor Retail – Information Management.</span></span>
- <span data-ttu-id="b8eb3-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k prodejní informací – informací správy (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-107">You can enable your users to automatically get signed-on to Infor Retail – Information Management (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="b8eb3-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="b8eb3-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b8eb3-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b8eb3-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b8eb3-110">Prerequisites</span></span>

<span data-ttu-id="b8eb3-111">Konfigurace integrace Azure AD s prodejní informací – informace správy, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="b8eb3-111">To configure Azure AD integration with Infor Retail – Information Management, you need the following items:</span></span>

- <span data-ttu-id="b8eb3-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b8eb3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b8eb3-113">Prodejní informací – informace správy jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b8eb3-113">An Infor Retail – Information Management single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b8eb3-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b8eb3-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b8eb3-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b8eb3-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b8eb3-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b8eb3-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b8eb3-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b8eb3-118">Scenario description</span></span>
<span data-ttu-id="b8eb3-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b8eb3-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b8eb3-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b8eb3-121">Přidání informací prodejní – správu informace z Galerie</span><span class="sxs-lookup"><span data-stu-id="b8eb3-121">Adding Infor Retail – Information Management from the gallery</span></span>
2. <span data-ttu-id="b8eb3-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b8eb3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-infor-retail--information-management-from-the-gallery"></a><span data-ttu-id="b8eb3-123">Přidání informací prodejní – správu informace z Galerie</span><span class="sxs-lookup"><span data-stu-id="b8eb3-123">Adding Infor Retail – Information Management from the gallery</span></span>
<span data-ttu-id="b8eb3-124">Konfigurace integrace informací prodejní – informace správy do služby Azure AD, musíte přidat informací prodejní – správu informace z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-124">To configure the integration of Infor Retail – Information Management into Azure AD, you need to add Infor Retail – Information Management from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b8eb3-125">**Chcete-li přidat prodejní informací – správu informace z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b8eb3-125">**To add Infor Retail – Information Management from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b8eb3-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="b8eb3-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b8eb3-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="b8eb3-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="b8eb3-133">Do vyhledávacího pole zadejte **informací prodejní – informace o správu**, vyberte **informací prodejní – informace o správu** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikace.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-133">In the search box, type **Infor Retail – Information Management**, select **Infor Retail – Information Management** from result panel then click **Add** button to add the application.</span></span>

    ![Prodejní informací – informace správy v seznamu výsledků](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="b8eb3-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b8eb3-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="b8eb3-136">V této části konfiguraci a testování Azure AD jednotné přihlašování s prodejní informací – informace správy na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="b8eb3-136">In this section, you configure and test Azure AD single sign-on with Infor Retail – Information Management based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b8eb3-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v prodejní informací – informace správy je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Infor Retail – Information Management is to a user in Azure AD.</span></span> <span data-ttu-id="b8eb3-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v prodejní informací – informace správy je nutné stanovit.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-138">In other words, a link relationship between an Azure AD user and the related user in Infor Retail – Information Management needs to be established.</span></span>

<span data-ttu-id="b8eb3-139">V prodejní informací – informace o správě přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-139">In Infor Retail – Information Management, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="b8eb3-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s prodejní informací – informace správy, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="b8eb3-140">To configure and test Azure AD single sign-on with Infor Retail – Information Management, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b8eb3-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b8eb3-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b8eb3-143">**[Vytvoření informací prodejní – informace o správu testovacího uživatele](#create-an-infor-retail--information-management-test-user)**  – Pokud chcete mít protějšek Britta Simon v prodejní informací – informace správy propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-143">**[Create an Infor Retail – Information Management test user](#create-an-infor-retail--information-management-test-user)** - to have a counterpart of Britta Simon in Infor Retail – Information Management that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b8eb3-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b8eb3-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="b8eb3-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b8eb3-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="b8eb3-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v prodejní informací – informace správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Infor Retail – Information Management application.</span></span>

<span data-ttu-id="b8eb3-148">**Ke konfiguraci Azure AD jednotné přihlašování s prodejní informací – informace o správu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b8eb3-148">**To configure Azure AD single sign-on with Infor Retail – Information Management, perform the following steps:**</span></span>

1. <span data-ttu-id="b8eb3-149">Na portálu Azure na **informací prodejní – informace o správu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-149">In the Azure portal, on the **Infor Retail – Information Management** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="b8eb3-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_samlbase.png)

3. <span data-ttu-id="b8eb3-153">Na **informací prodejní – informace o doméně pro správu a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace v režimu rozšíření IDP iniciované:</span><span class="sxs-lookup"><span data-stu-id="b8eb3-153">On the **Infor Retail – Information Management Domain and URLs** section, perform the following steps if you wish to configure the application in IDP initiated mode:</span></span>

    ![Jednotné přihlašování informace IDP informací prodejní – informace o doméně pro správu a adresy URL](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_url.png)

    <span data-ttu-id="b8eb3-155">a.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-155">a.</span></span> <span data-ttu-id="b8eb3-156">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="b8eb3-156">In the **Identifier** textbox, type a URL using the following patterns:</span></span> 
    |   |
    | -- |
    | `https://<company name>.mingle.infor.com` |
    | `http://<company name>.mingledev.infor.com` |

    <span data-ttu-id="b8eb3-157">b.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-157">b.</span></span> <span data-ttu-id="b8eb3-158">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.mingle.infor.com/sp/ACS.saml2`</span><span class="sxs-lookup"><span data-stu-id="b8eb3-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.mingle.infor.com/sp/ACS.saml2`</span></span>

4. <span data-ttu-id="b8eb3-159">Zkontrolujte **zobrazit upřesňující nastavení adresy URL** a provést následující krok, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="b8eb3-159">Check **Show advanced URL settings** and perform the following step if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Prodejní informací – informace o doméně pro správu a adresy URL jeden přihlašování informace SP](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_url1.png)

    <span data-ttu-id="b8eb3-161">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.mingle.infor.com/<company code>`</span><span class="sxs-lookup"><span data-stu-id="b8eb3-161">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.mingle.infor.com/<company code>`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="b8eb3-162">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-162">These values are not real.</span></span> <span data-ttu-id="b8eb3-163">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-163">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="b8eb3-164">Obraťte se na [informací prodejní – tým podpory klienta pro správu informace](mailto:innovate@infor.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-164">Contact [Infor Retail – Information Management Client support team](mailto:innovate@infor.com) to get these values.</span></span> 

5. <span data-ttu-id="b8eb3-165">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-165">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_certificate.png) 

6. <span data-ttu-id="b8eb3-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-167">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="b8eb3-169">Konfigurace jednotného přihlašování na **informací prodejní – informace o správu** straně, budete muset odeslat stažené **soubor XML s metadaty** k [informací prodejní – tým podpory správy informace](mailto:innovate@infor.com).</span><span class="sxs-lookup"><span data-stu-id="b8eb3-169">To configure single sign-on on **Infor Retail – Information Management** side, you need to send the downloaded **Metadata XML** to [Infor Retail – Information Management support team](mailto:innovate@infor.com).</span></span> <span data-ttu-id="b8eb3-170">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-170">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="b8eb3-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="b8eb3-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b8eb3-172">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b8eb3-173">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b8eb3-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="b8eb3-174">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b8eb3-174">Create an Azure AD test user</span></span>

<span data-ttu-id="b8eb3-175">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="b8eb3-177">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b8eb3-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b8eb3-178">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-178">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="b8eb3-180">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-180">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="b8eb3-182">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-182">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="b8eb3-184">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b8eb3-184">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_04.png)

    <span data-ttu-id="b8eb3-186">a.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-186">a.</span></span> <span data-ttu-id="b8eb3-187">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-187">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b8eb3-188">b.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-188">b.</span></span> <span data-ttu-id="b8eb3-189">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-189">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="b8eb3-190">c.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-190">c.</span></span> <span data-ttu-id="b8eb3-191">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-191">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="b8eb3-192">d.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-192">d.</span></span> <span data-ttu-id="b8eb3-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-193">Click **Create**.</span></span>
 
### <a name="create-an-infor-retail--information-management-test-user"></a><span data-ttu-id="b8eb3-194">Vytvoření informací prodejní – informace o správu testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="b8eb3-194">Create an Infor Retail – Information Management test user</span></span>

<span data-ttu-id="b8eb3-195">V této části vytvoříte uživatele volal Britta Simon v prodejní informací – informace správy.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-195">In this section, you create a user called Britta Simon in Infor Retail – Information Management.</span></span> <span data-ttu-id="b8eb3-196">Spojte se s [informací prodejní – tým podpory správy informace](mailto:innovate@infor.com) přidat uživatele do prodejní informací – platforma pro správu informace.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-196">Please work with [Infor Retail – Information Management support team](mailto:innovate@infor.com) to add the users in the Infor Retail – Information Management platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="b8eb3-197">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b8eb3-197">Assign the Azure AD test user</span></span>

<span data-ttu-id="b8eb3-198">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu informací prodejní – informace správy.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Infor Retail – Information Management.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="b8eb3-200">**Britta Simon přiřadit informací prodejní – informace o správu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b8eb3-200">**To assign Britta Simon to Infor Retail – Information Management, perform the following steps:**</span></span>

1. <span data-ttu-id="b8eb3-201">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b8eb3-203">V seznamu aplikací vyberte **informací prodejní – informace o správu**.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-203">In the applications list, select **Infor Retail – Information Management**.</span></span>

    ![Prodejní informací – odkaz Správa informace v seznamu aplikací](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_app.png)  

3. <span data-ttu-id="b8eb3-205">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="b8eb3-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-207">Click **Add** button.</span></span> <span data-ttu-id="b8eb3-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="b8eb3-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b8eb3-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b8eb3-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="b8eb3-213">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b8eb3-213">Test single sign-on</span></span>

<span data-ttu-id="b8eb3-214">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b8eb3-215">Po kliknutí na tlačítko prodejní informací – informace o správu dlaždice na přístupovém panelu jste měli získat automaticky přihlášení k prodejní informací – informace správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="b8eb3-215">When you click the Infor Retail – Information Management tile in the Access Panel, you should get automatically signed-on to your Infor Retail – Information Management application.</span></span>
<span data-ttu-id="b8eb3-216">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b8eb3-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="b8eb3-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b8eb3-217">Additional resources</span></span>

* [<span data-ttu-id="b8eb3-218">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b8eb3-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b8eb3-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b8eb3-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_203.png

