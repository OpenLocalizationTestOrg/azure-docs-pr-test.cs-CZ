---
title: 'Kurz: Azure Active Directory integrace s Asana | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Asana."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 837e38fe-8f55-475c-87f4-6394dc1fee2b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: jeedes
ms.openlocfilehash: a2f0cecb93cc382bcfd710c44eb70f80ae67f9b6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asana"></a><span data-ttu-id="18d34-103">Kurz: Azure Active Directory integrace s Asana</span><span class="sxs-lookup"><span data-stu-id="18d34-103">Tutorial: Azure Active Directory integration with Asana</span></span>

<span data-ttu-id="18d34-104">V tomto kurzu zjistěte, jak integrovat Asana s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="18d34-104">In this tutorial, you learn how to integrate Asana with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="18d34-105">Integrace Asana s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="18d34-105">Integrating Asana with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="18d34-106">Můžete řídit ve službě Azure AD, který má přístup k Asana</span><span class="sxs-lookup"><span data-stu-id="18d34-106">You can control in Azure AD who has access to Asana</span></span>
- <span data-ttu-id="18d34-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Asana (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="18d34-107">You can enable your users to automatically get signed-on to Asana (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="18d34-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="18d34-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="18d34-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="18d34-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="18d34-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="18d34-110">Prerequisites</span></span>

<span data-ttu-id="18d34-111">Konfigurace integrace Azure AD s Asana, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="18d34-111">To configure Azure AD integration with Asana, you need the following items:</span></span>

- <span data-ttu-id="18d34-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="18d34-112">An Azure AD subscription</span></span>
- <span data-ttu-id="18d34-113">Asana jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="18d34-113">An Asana single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="18d34-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="18d34-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="18d34-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="18d34-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="18d34-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="18d34-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="18d34-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="18d34-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="18d34-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="18d34-118">Scenario description</span></span>
<span data-ttu-id="18d34-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="18d34-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="18d34-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="18d34-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="18d34-121">Přidání Asana z Galerie</span><span class="sxs-lookup"><span data-stu-id="18d34-121">Adding Asana from the gallery</span></span>
2. <span data-ttu-id="18d34-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="18d34-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-asana-from-the-gallery"></a><span data-ttu-id="18d34-123">Přidání Asana z Galerie</span><span class="sxs-lookup"><span data-stu-id="18d34-123">Adding Asana from the gallery</span></span>
<span data-ttu-id="18d34-124">Při konfiguraci integrace Asana do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Asana z galerie.</span><span class="sxs-lookup"><span data-stu-id="18d34-124">To configure the integration of Asana into Azure AD, you need to add Asana from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="18d34-125">**Pokud chcete přidat Asana z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="18d34-125">**To add Asana from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="18d34-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="18d34-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="18d34-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="18d34-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="18d34-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="18d34-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="18d34-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="18d34-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="18d34-133">Do vyhledávacího pole zadejte **Asana**, vyberte **Asana** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="18d34-133">In the search box, type **Asana**, select **Asana** from result panel then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asana-tutorial/tutorial_asana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="18d34-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="18d34-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="18d34-136">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Asana podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="18d34-136">In this section, you configure and test Azure AD single sign-on with Asana based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="18d34-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Asana je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="18d34-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Asana is to a user in Azure AD.</span></span> <span data-ttu-id="18d34-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Asana musí navázat.</span><span class="sxs-lookup"><span data-stu-id="18d34-138">In other words, a link relationship between an Azure AD user and the related user in Asana needs to be established.</span></span>

<span data-ttu-id="18d34-139">V Asana, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="18d34-139">In Asana, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="18d34-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Asana, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="18d34-140">To configure and test Azure AD single sign-on with Asana, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="18d34-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="18d34-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="18d34-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="18d34-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="18d34-143">**[Vytvořit testovací uživatele s Asana](#create-an-asana-test-user)**  – Pokud chcete mít protějšek Britta Simon v Asana propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="18d34-143">**[Create an Asana test user](#create-an-asana-test-user)** - to have a counterpart of Britta Simon in Asana that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="18d34-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="18d34-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="18d34-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="18d34-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="18d34-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="18d34-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="18d34-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Asana.</span><span class="sxs-lookup"><span data-stu-id="18d34-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Asana application.</span></span>

<span data-ttu-id="18d34-148">**Ke konfiguraci Azure AD jednotné přihlašování s Asana, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="18d34-148">**To configure Azure AD single sign-on with Asana, perform the following steps:**</span></span>

1. <span data-ttu-id="18d34-149">Na portálu Azure na **Asana** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="18d34-149">In the Azure portal, on the **Asana** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="18d34-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="18d34-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-asana-tutorial/tutorial_asana_samlbase.png)

3. <span data-ttu-id="18d34-153">Na **Asana domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="18d34-153">On the **Asana Domain and URLs** section, perform the following steps:</span></span>

    ![Asana domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-asana-tutorial/tutorial_asana_url.png)

    <span data-ttu-id="18d34-155">a.</span><span class="sxs-lookup"><span data-stu-id="18d34-155">a.</span></span> <span data-ttu-id="18d34-156">V **přihlašovací adresa URL** textovému poli, zadat adresu URL:`https://app.asana.com/`</span><span class="sxs-lookup"><span data-stu-id="18d34-156">In the **Sign-on URL** textbox, type URL: `https://app.asana.com/`</span></span>

    <span data-ttu-id="18d34-157">b.</span><span class="sxs-lookup"><span data-stu-id="18d34-157">b.</span></span> <span data-ttu-id="18d34-158">V **identifikátor** textovému poli, hodnota typu:`https://app.asana.com/`</span><span class="sxs-lookup"><span data-stu-id="18d34-158">In the **Identifier** textbox, type value: `https://app.asana.com/`</span></span>
 
4. <span data-ttu-id="18d34-159">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="18d34-159">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-asana-tutorial/tutorial_asana_certificate.png)
    
5. <span data-ttu-id="18d34-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="18d34-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-asana-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="18d34-163">Na **Asana konfigurace** klikněte na tlačítko **konfigurace Asana** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="18d34-163">On the **Asana Configuration** section, click **Configure Asana** to open **Configure sign-on** window.</span></span> <span data-ttu-id="18d34-164">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="18d34-164">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace Asana](./media/active-directory-saas-asana-tutorial/tutorial_asana_configure.png) 

7. <span data-ttu-id="18d34-166">V okně jiný prohlížeč přihlášení do aplikace Asana.</span><span class="sxs-lookup"><span data-stu-id="18d34-166">In a different browser window, sign-on to your Asana application.</span></span> <span data-ttu-id="18d34-167">Nakonfigurovat jednotné přihlašování v Asana, přístup k nastavení pracovního prostoru klikněte na název pracovního prostoru v pravém horním rohu obrazovky.</span><span class="sxs-lookup"><span data-stu-id="18d34-167">To configure SSO in Asana, access the workspace settings by clicking the workspace name on the top right corner of the screen.</span></span> <span data-ttu-id="18d34-168">Potom klikněte na  **\<název pracovního prostoru\> nastavení**.</span><span class="sxs-lookup"><span data-stu-id="18d34-168">Then, click on **\<your workspace name\> Settings**.</span></span> 
   
    ![Nastavení jednotného přihlašování k Asana](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

8. <span data-ttu-id="18d34-170">Na **nastavení organizace** okně klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="18d34-170">On the **Organization settings** window, click **Administration**.</span></span> <span data-ttu-id="18d34-171">Potom klikněte na **členy musí přihlásit pomocí SAML** umožňující jednotného přihlašování k konfigurace.</span><span class="sxs-lookup"><span data-stu-id="18d34-171">Then, click **Members must log in via SAML** to enable the SSO configuration.</span></span> <span data-ttu-id="18d34-172">Proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="18d34-172">The perform the following steps:</span></span>
   
    ![Konfigurace nastavení jednotný přihlášení](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)  

     <span data-ttu-id="18d34-174">a.</span><span class="sxs-lookup"><span data-stu-id="18d34-174">a.</span></span> <span data-ttu-id="18d34-175">V **přihlašovací adresa URL stránky** textovému poli, Vložit **SAML jeden přihlašování adresa URL služby**.</span><span class="sxs-lookup"><span data-stu-id="18d34-175">In the **Sign-in page URL** textbox, paste the **SAML Single Sign-On Service URL**.</span></span>

     <span data-ttu-id="18d34-176">b.</span><span class="sxs-lookup"><span data-stu-id="18d34-176">b.</span></span> <span data-ttu-id="18d34-177">Klikněte pravým tlačítkem na certifikát si stáhli z portálu Azure a pak otevřete soubor certifikátu pomocí poznámkového bloku nebo upřednostňovaný textový editor.</span><span class="sxs-lookup"><span data-stu-id="18d34-177">Right click the certificate downloaded from Azure portal, then open the certificate file using Notepad or your preferred text editor.</span></span> <span data-ttu-id="18d34-178">Kopírovat obsah mezi začátku a název certifikátu end a vložte jej do **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="18d34-178">Copy the content between the begin and the end certificate title and paste it in the **X.509 Certificate** textbox.</span></span>

9. <span data-ttu-id="18d34-179">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="18d34-179">Click **Save**.</span></span> <span data-ttu-id="18d34-180">Přejděte na [Asana Průvodce pro nastavení jednotného přihlašování k](https://asana.com/guide/help/premium/authentication#gl-saml) Pokud potřebujete další pomoc.</span><span class="sxs-lookup"><span data-stu-id="18d34-180">Go to [Asana guide for setting up SSO](https://asana.com/guide/help/premium/authentication#gl-saml) if you need further assistance.</span></span>

> [!TIP]
> <span data-ttu-id="18d34-181">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="18d34-181">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="18d34-182">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="18d34-182">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="18d34-183">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="18d34-183">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="18d34-184">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="18d34-184">Create an Azure AD test user</span></span>

<span data-ttu-id="18d34-185">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="18d34-185">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="18d34-187">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="18d34-187">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="18d34-188">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="18d34-188">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-asana-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="18d34-190">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="18d34-190">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-asana-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="18d34-192">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="18d34-192">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="18d34-194">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="18d34-194">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="18d34-196">a.</span><span class="sxs-lookup"><span data-stu-id="18d34-196">a.</span></span> <span data-ttu-id="18d34-197">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="18d34-197">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="18d34-198">b.</span><span class="sxs-lookup"><span data-stu-id="18d34-198">b.</span></span> <span data-ttu-id="18d34-199">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="18d34-199">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="18d34-200">c.</span><span class="sxs-lookup"><span data-stu-id="18d34-200">c.</span></span> <span data-ttu-id="18d34-201">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="18d34-201">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="18d34-202">d.</span><span class="sxs-lookup"><span data-stu-id="18d34-202">d.</span></span> <span data-ttu-id="18d34-203">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="18d34-203">Click **Create**.</span></span>
 
### <a name="create-an-asana-test-user"></a><span data-ttu-id="18d34-204">Vytvořit uživatele s Asana testu</span><span class="sxs-lookup"><span data-stu-id="18d34-204">Create an Asana test user</span></span>

<span data-ttu-id="18d34-205">V této části vytvoříte volal Britta Simon v Asana uživatele.</span><span class="sxs-lookup"><span data-stu-id="18d34-205">In this section, you create a user called Britta Simon in Asana.</span></span>

1. <span data-ttu-id="18d34-206">Na **Asana**, přejděte na **týmy** části na levém panelu.</span><span class="sxs-lookup"><span data-stu-id="18d34-206">On **Asana**, go to the **Teams** section on the left panel.</span></span> <span data-ttu-id="18d34-207">Klikněte na tlačítko znaménko plus.</span><span class="sxs-lookup"><span data-stu-id="18d34-207">Click the plus sign button.</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. <span data-ttu-id="18d34-209">Zadejte e-mailu britta.simon@contoso.com v textovém poli a potom vyberte **pozvat**.</span><span class="sxs-lookup"><span data-stu-id="18d34-209">Type the email britta.simon@contoso.com in the text box and then select **Invite**.</span></span>

3. <span data-ttu-id="18d34-210">Klikněte na tlačítko **odeslat pozvání**.</span><span class="sxs-lookup"><span data-stu-id="18d34-210">Click **Send Invite**.</span></span> <span data-ttu-id="18d34-211">Nový uživatel obdrží e-mail do své e-mailový účet.</span><span class="sxs-lookup"><span data-stu-id="18d34-211">The new user will receive an email into her email account.</span></span> <span data-ttu-id="18d34-212">Uživatel bude muset vytvořit a ověřit účet.</span><span class="sxs-lookup"><span data-stu-id="18d34-212">She will need to create and validate the account.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="18d34-213">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="18d34-213">Assign the Azure AD test user</span></span>

<span data-ttu-id="18d34-214">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Asana.</span><span class="sxs-lookup"><span data-stu-id="18d34-214">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Asana.</span></span>

![Přiřadit role uživatele][200]

<span data-ttu-id="18d34-216">**Pokud chcete přiřadit Britta Simon Asana, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="18d34-216">**To assign Britta Simon to Asana, perform the following steps:**</span></span>

1. <span data-ttu-id="18d34-217">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="18d34-217">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="18d34-219">V seznamu aplikací vyberte **Asana**.</span><span class="sxs-lookup"><span data-stu-id="18d34-219">In the applications list, select **Asana**.</span></span>

    ![V seznamu aplikací na Asana odkaz](./media/active-directory-saas-asana-tutorial/tutorial_asana_app.png) 

3. <span data-ttu-id="18d34-221">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="18d34-221">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="18d34-223">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="18d34-223">Click **Add** button.</span></span> <span data-ttu-id="18d34-224">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="18d34-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="18d34-226">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="18d34-226">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="18d34-227">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="18d34-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="18d34-228">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="18d34-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="18d34-229">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="18d34-229">Test single sign-on</span></span>

<span data-ttu-id="18d34-230">Cílem této části je pro testování vaší služby Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="18d34-230">The objective of this section is to test your Azure AD single sign-on.</span></span>

<span data-ttu-id="18d34-231">Přejděte na stránku přihlášení Asana.</span><span class="sxs-lookup"><span data-stu-id="18d34-231">Go to Asana login page.</span></span> <span data-ttu-id="18d34-232">Do textového pole e-mailovou adresu, vložit e-mailovou adresu britta.simon@contoso.com. Ponechte textové pole hesla v prázdné a pak klikněte na **protokolu v**.</span><span class="sxs-lookup"><span data-stu-id="18d34-232">In the Email address textbox, insert the email address britta.simon@contoso.com. Leave the password textbox in blank and then click **Log In**.</span></span> <span data-ttu-id="18d34-233">Budete přesměrováni na přihlašovací stránku služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="18d34-233">You will be redirected to Azure AD login page.</span></span> <span data-ttu-id="18d34-234">Dokončení přihlašovací údaje Azure AD.</span><span class="sxs-lookup"><span data-stu-id="18d34-234">Complete your Azure AD credentials.</span></span> <span data-ttu-id="18d34-235">Nyní jste přihlášeni Asana.</span><span class="sxs-lookup"><span data-stu-id="18d34-235">Now, you are logged in on Asana.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="18d34-236">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="18d34-236">Additional resources</span></span>

* [<span data-ttu-id="18d34-237">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="18d34-237">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="18d34-238">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="18d34-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-asana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
