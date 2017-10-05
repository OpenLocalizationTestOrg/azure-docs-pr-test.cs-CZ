---
title: 'Kurz: Azure Active Directory integrace s ICIMS | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ICIMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 72dbd649-e4b1-4d72-ad76-636d84922596
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 26a6b41a0e59924d007855ca548f22ed00bd7e23
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-icims"></a><span data-ttu-id="9c275-103">Kurz: Azure Active Directory integrace s ICIMS</span><span class="sxs-lookup"><span data-stu-id="9c275-103">Tutorial: Azure Active Directory integration with ICIMS</span></span>

<span data-ttu-id="9c275-104">V tomto kurzu zjistěte, jak integrovat ICIMS s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9c275-104">In this tutorial, you learn how to integrate ICIMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9c275-105">Integrace ICIMS s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9c275-105">Integrating ICIMS with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9c275-106">Můžete řídit ve službě Azure AD, který má přístup k ICIMS</span><span class="sxs-lookup"><span data-stu-id="9c275-106">You can control in Azure AD who has access to ICIMS</span></span>
- <span data-ttu-id="9c275-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k ICIMS (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c275-107">You can enable your users to automatically get signed-on to ICIMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9c275-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9c275-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9c275-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9c275-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c275-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9c275-110">Prerequisites</span></span>

<span data-ttu-id="9c275-111">Konfigurace integrace Azure AD s ICIMS, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="9c275-111">To configure Azure AD integration with ICIMS, you need the following items:</span></span>

- <span data-ttu-id="9c275-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c275-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9c275-113">ICIMS jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="9c275-113">An ICIMS single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9c275-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9c275-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9c275-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9c275-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9c275-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9c275-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9c275-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9c275-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9c275-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9c275-118">Scenario description</span></span>
<span data-ttu-id="9c275-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9c275-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9c275-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9c275-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9c275-121">Přidání ICIMS z Galerie</span><span class="sxs-lookup"><span data-stu-id="9c275-121">Adding ICIMS from the gallery</span></span>
2. <span data-ttu-id="9c275-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9c275-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-icims-from-the-gallery"></a><span data-ttu-id="9c275-123">Přidání ICIMS z Galerie</span><span class="sxs-lookup"><span data-stu-id="9c275-123">Adding ICIMS from the gallery</span></span>
<span data-ttu-id="9c275-124">Při konfiguraci integrace ICIMS do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS ICIMS z galerie.</span><span class="sxs-lookup"><span data-stu-id="9c275-124">To configure the integration of ICIMS into Azure AD, you need to add ICIMS from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9c275-125">**Pokud chcete přidat ICIMS z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9c275-125">**To add ICIMS from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9c275-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9c275-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="9c275-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9c275-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9c275-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9c275-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="9c275-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c275-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="9c275-133">Do vyhledávacího pole zadejte **ICIMS**, vyberte **ICIMS** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9c275-133">In the search box, type **ICIMS**, select **ICIMS** from result panel then click **Add** button to add the application.</span></span>

    ![ICIMS v seznamu výsledků](./media/active-directory-saas-icims-tutorial/tutorial_icims_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="9c275-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9c275-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="9c275-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s ICIMS podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="9c275-136">In this section, you configure and test Azure AD single sign-on with ICIMS based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9c275-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v ICIMS je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9c275-137">For single sign-on to work, Azure AD needs to know what the counterpart user in ICIMS is to a user in Azure AD.</span></span> <span data-ttu-id="9c275-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v ICIMS musí navázat.</span><span class="sxs-lookup"><span data-stu-id="9c275-138">In other words, a link relationship between an Azure AD user and the related user in ICIMS needs to be established.</span></span>

<span data-ttu-id="9c275-139">V ICIMS, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="9c275-139">In ICIMS, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9c275-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s ICIMS, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="9c275-140">To configure and test Azure AD single sign-on with ICIMS, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9c275-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="9c275-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9c275-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9c275-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9c275-143">**[Vytvořit testovací uživatele s ICIMS](#create-an-icims-test-user)**  – Pokud chcete mít protějšek Britta Simon v ICIMS propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="9c275-143">**[Create an ICIMS test user](#create-an-icims-test-user)** - to have a counterpart of Britta Simon in ICIMS that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9c275-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9c275-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9c275-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9c275-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="9c275-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9c275-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="9c275-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci ICIMS.</span><span class="sxs-lookup"><span data-stu-id="9c275-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ICIMS application.</span></span>

<span data-ttu-id="9c275-148">**Ke konfiguraci Azure AD jednotné přihlašování s ICIMS, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9c275-148">**To configure Azure AD single sign-on with ICIMS, perform the following steps:**</span></span>

1. <span data-ttu-id="9c275-149">Na portálu Azure na **ICIMS** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9c275-149">In the Azure portal, on the **ICIMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9c275-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9c275-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-icims-tutorial/tutorial_icims_samlbase.png)

3. <span data-ttu-id="9c275-153">Na **ICIMS domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9c275-153">On the **ICIMS Domain and URLs** section, perform the following steps:</span></span>

    ![ICIMS domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-icims-tutorial/tutorial_icims_url.png)

    <span data-ttu-id="9c275-155">a.</span><span class="sxs-lookup"><span data-stu-id="9c275-155">a.</span></span> <span data-ttu-id="9c275-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenant name>.icims.com`</span><span class="sxs-lookup"><span data-stu-id="9c275-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant name>.icims.com`</span></span>

    <span data-ttu-id="9c275-157">b.</span><span class="sxs-lookup"><span data-stu-id="9c275-157">b.</span></span> <span data-ttu-id="9c275-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenant name>.icims.com`</span><span class="sxs-lookup"><span data-stu-id="9c275-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenant name>.icims.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9c275-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="9c275-159">These values are not real.</span></span> <span data-ttu-id="9c275-160">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="9c275-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="9c275-161">Obraťte se na [tým podpory ICIMS klienta](https://www.icims.com/contact-us) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="9c275-161">Contact [ICIMS Client support team](https://www.icims.com/contact-us) to get these values.</span></span> 
 
4. <span data-ttu-id="9c275-162">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="9c275-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-icims-tutorial/tutorial_icims_certificate.png) 

5. <span data-ttu-id="9c275-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9c275-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-icims-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9c275-166">Na **ICIMS konfigurace** klikněte na tlačítko **konfigurace ICIMS** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="9c275-166">On the **ICIMS Configuration** section, click **Configure ICIMS** to open **Configure sign-on** window.</span></span> <span data-ttu-id="9c275-167">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="9c275-167">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace ICIMS](./media/active-directory-saas-icims-tutorial/tutorial_icims_configure.png) 

7. <span data-ttu-id="9c275-169">Konfigurace jednotného přihlašování na **ICIMS** straně, budete muset odeslat stažené **soubor XML s metadaty**, **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** k [ Tým podpory ICIMS](https://www.icims.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="9c275-169">To configure single sign-on on **ICIMS** side, you need to send the downloaded **Metadata XML**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [ICIMS support team](https://www.icims.com/contact-us).</span></span> <span data-ttu-id="9c275-170">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="9c275-170">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="9c275-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="9c275-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9c275-172">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="9c275-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9c275-173">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9c275-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="9c275-174">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c275-174">Create an Azure AD test user</span></span>
<span data-ttu-id="9c275-175">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9c275-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="9c275-177">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9c275-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9c275-178">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9c275-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-icims-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9c275-180">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9c275-180">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-icims-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9c275-182">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c275-182">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-icims-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9c275-184">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9c275-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Dialogové okno uživatele](./media/active-directory-saas-icims-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9c275-186">a.</span><span class="sxs-lookup"><span data-stu-id="9c275-186">a.</span></span> <span data-ttu-id="9c275-187">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9c275-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9c275-188">b.</span><span class="sxs-lookup"><span data-stu-id="9c275-188">b.</span></span> <span data-ttu-id="9c275-189">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9c275-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9c275-190">c.</span><span class="sxs-lookup"><span data-stu-id="9c275-190">c.</span></span> <span data-ttu-id="9c275-191">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9c275-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9c275-192">d.</span><span class="sxs-lookup"><span data-stu-id="9c275-192">d.</span></span> <span data-ttu-id="9c275-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9c275-193">Click **Create**.</span></span>
 
### <a name="create-an-icims-test-user"></a><span data-ttu-id="9c275-194">Vytvořit uživatele s ICIMS testu</span><span class="sxs-lookup"><span data-stu-id="9c275-194">Create an ICIMS test user</span></span>

<span data-ttu-id="9c275-195">Cílem této části je vytvoření uživatele v ICIMS nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9c275-195">The objective of this section is to create a user called Britta Simon in ICIMS.</span></span> <span data-ttu-id="9c275-196">Práce s [tým podpory ICIMS](https://www.icims.com/contact-us) přidat uživatele do ICIMS účtu.</span><span class="sxs-lookup"><span data-stu-id="9c275-196">Work with [ICIMS support team](https://www.icims.com/contact-us) to add the users in the ICIMS account.</span></span> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="9c275-197">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c275-197">Assign the Azure AD test user</span></span>

<span data-ttu-id="9c275-198">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu ICIMS.</span><span class="sxs-lookup"><span data-stu-id="9c275-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ICIMS.</span></span>

![Přiřadit role uživatele][200]

<span data-ttu-id="9c275-200">**Pokud chcete přiřadit Britta Simon ICIMS, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9c275-200">**To assign Britta Simon to ICIMS, perform the following steps:**</span></span>

1. <span data-ttu-id="9c275-201">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9c275-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9c275-203">V seznamu aplikací vyberte **ICIMS**.</span><span class="sxs-lookup"><span data-stu-id="9c275-203">In the applications list, select **ICIMS**.</span></span>

    ![V seznamu aplikací na ICIMS odkaz](./media/active-directory-saas-icims-tutorial/tutorial_icims_app.png) 

3. <span data-ttu-id="9c275-205">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9c275-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202] 

4. <span data-ttu-id="9c275-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9c275-207">Click **Add** button.</span></span> <span data-ttu-id="9c275-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c275-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="9c275-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9c275-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9c275-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c275-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9c275-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9c275-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="9c275-213">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9c275-213">Test single sign-on</span></span>

<span data-ttu-id="9c275-214">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="9c275-214">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="9c275-215">Když kliknete na dlaždici ICIMS na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci ICIMS.</span><span class="sxs-lookup"><span data-stu-id="9c275-215">When you click the ICIMS tile in the Access Panel, you should get automatically signed-on to your ICIMS application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9c275-216">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9c275-216">Additional resources</span></span>

* [<span data-ttu-id="9c275-217">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9c275-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9c275-218">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9c275-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-icims-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-icims-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-icims-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-icims-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-icims-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-icims-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-icims-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-icims-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-icims-tutorial/tutorial_general_203.png

