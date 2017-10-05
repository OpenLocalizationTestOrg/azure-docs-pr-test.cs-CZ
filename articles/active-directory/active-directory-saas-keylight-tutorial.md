---
title: 'Kurz: Azure Active Directory integrace s LockPath Keylight | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a LockPath Keylight."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 234a32f1-9f56-4650-9e31-7b38ad734b1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: e64a966f24411818abc4cc4ab29a428b5577d012
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lockpath-keylight"></a><span data-ttu-id="c4855-103">Kurz: Azure Active Directory integrace s LockPath Keylight</span><span class="sxs-lookup"><span data-stu-id="c4855-103">Tutorial: Azure Active Directory integration with LockPath Keylight</span></span>

<span data-ttu-id="c4855-104">V tomto kurzu zjistěte, jak integrovat LockPath Keylight s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c4855-104">In this tutorial, you learn how to integrate LockPath Keylight with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c4855-105">Integrace LockPath Keylight s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c4855-105">Integrating LockPath Keylight with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c4855-106">Můžete řídit ve službě Azure AD, který má přístup k LockPath Keylight</span><span class="sxs-lookup"><span data-stu-id="c4855-106">You can control in Azure AD who has access to LockPath Keylight</span></span>
- <span data-ttu-id="c4855-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k LockPath Keylight (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c4855-107">You can enable your users to automatically get signed-on to LockPath Keylight (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c4855-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c4855-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c4855-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c4855-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4855-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c4855-110">Prerequisites</span></span>

<span data-ttu-id="c4855-111">Konfigurace integrace Azure AD s LockPath Keylight, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="c4855-111">To configure Azure AD integration with LockPath Keylight, you need the following items:</span></span>

- <span data-ttu-id="c4855-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c4855-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c4855-113">LockPath Keylight jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c4855-113">A LockPath Keylight single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c4855-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c4855-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c4855-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c4855-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c4855-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c4855-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c4855-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c4855-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c4855-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c4855-118">Scenario description</span></span>
<span data-ttu-id="c4855-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c4855-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c4855-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c4855-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c4855-121">Přidání LockPath Keylight z Galerie</span><span class="sxs-lookup"><span data-stu-id="c4855-121">Adding LockPath Keylight from the gallery</span></span>
2. <span data-ttu-id="c4855-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c4855-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lockpath-keylight-from-the-gallery"></a><span data-ttu-id="c4855-123">Přidání LockPath Keylight z Galerie</span><span class="sxs-lookup"><span data-stu-id="c4855-123">Adding LockPath Keylight from the gallery</span></span>
<span data-ttu-id="c4855-124">Při konfiguraci integrace LockPath Keylight do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS LockPath Keylight z galerie.</span><span class="sxs-lookup"><span data-stu-id="c4855-124">To configure the integration of LockPath Keylight into Azure AD, you need to add LockPath Keylight from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c4855-125">**Pokud chcete přidat LockPath Keylight z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c4855-125">**To add LockPath Keylight from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c4855-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c4855-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c4855-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c4855-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c4855-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c4855-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c4855-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c4855-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c4855-133">Do vyhledávacího pole zadejte **LockPath Keylight**.</span><span class="sxs-lookup"><span data-stu-id="c4855-133">In the search box, type **LockPath Keylight**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_search.png)

5. <span data-ttu-id="c4855-135">Na panelu výsledků vyberte **LockPath Keylight**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c4855-135">In the results panel, select **LockPath Keylight**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c4855-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c4855-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c4855-138">V této části nakonfigurujete a testu Azure AD jednotné přihlašování s LockPath Keylight podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="c4855-138">In this section, you configure and test Azure AD single sign-on with LockPath Keylight based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c4855-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v LockPath Keylight je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c4855-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LockPath Keylight is to a user in Azure AD.</span></span> <span data-ttu-id="c4855-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v LockPath Keylight musí navázat.</span><span class="sxs-lookup"><span data-stu-id="c4855-140">In other words, a link relationship between an Azure AD user and the related user in LockPath Keylight needs to be established.</span></span>

<span data-ttu-id="c4855-141">V LockPath Keylight přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="c4855-141">In LockPath Keylight, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c4855-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s LockPath Keylight, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="c4855-142">To configure and test Azure AD single sign-on with LockPath Keylight, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c4855-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="c4855-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c4855-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c4855-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c4855-145">**[Vytvoření zkušebního uživatele LockPath Keylight](#creating-a-lockpath-keylight-test-user)**  – Pokud chcete mít protějšek Britta Simon v LockPath Keylight propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4855-145">**[Creating a LockPath Keylight test user](#creating-a-lockpath-keylight-test-user)** - to have a counterpart of Britta Simon in LockPath Keylight that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c4855-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c4855-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c4855-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c4855-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c4855-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c4855-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c4855-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci LockPath Keylight.</span><span class="sxs-lookup"><span data-stu-id="c4855-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LockPath Keylight application.</span></span>

<span data-ttu-id="c4855-150">**Ke konfiguraci Azure AD jednotné přihlašování s LockPath Keylight, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c4855-150">**To configure Azure AD single sign-on with LockPath Keylight, perform the following steps:**</span></span>

1. <span data-ttu-id="c4855-151">Na portálu Azure na **LockPath Keylight** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c4855-151">In the Azure portal, on the **LockPath Keylight** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c4855-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c4855-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_samlbase.png)

3. <span data-ttu-id="c4855-155">Na **LockPath Keylight domény a adresy URL** část, proveďte následující kroky::</span><span class="sxs-lookup"><span data-stu-id="c4855-155">On the **LockPath Keylight Domain and URLs** section, perform the following steps::</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_url.png)

    <span data-ttu-id="c4855-157">a.</span><span class="sxs-lookup"><span data-stu-id="c4855-157">a.</span></span> <span data-ttu-id="c4855-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.keylightgrc.com/`</span><span class="sxs-lookup"><span data-stu-id="c4855-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.keylightgrc.com/`</span></span>

    <span data-ttu-id="c4855-159">b.</span><span class="sxs-lookup"><span data-stu-id="c4855-159">b.</span></span> <span data-ttu-id="c4855-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.keylightgrc.com`</span><span class="sxs-lookup"><span data-stu-id="c4855-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.keylightgrc.com`</span></span>

    <span data-ttu-id="c4855-161">c.</span><span class="sxs-lookup"><span data-stu-id="c4855-161">c.</span></span> <span data-ttu-id="c4855-162">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.keylightgrc.com/Login.aspx`</span><span class="sxs-lookup"><span data-stu-id="c4855-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.keylightgrc.com/Login.aspx`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="c4855-163">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="c4855-163">These values are not real.</span></span> <span data-ttu-id="c4855-164">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="c4855-164">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="c4855-165">Obraťte se na [tým podpory klienta Keylight LockPath](https://www.lockpath.com/contact/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="c4855-165">Contact [LockPath Keylight Client support team](https://www.lockpath.com/contact/) to get these values.</span></span> 

4. <span data-ttu-id="c4855-166">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Raw)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="c4855-166">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_certificate.png) 

5. <span data-ttu-id="c4855-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c4855-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="c4855-170">Na **LockPath Keylight konfigurace** klikněte na tlačítko **konfigurace LockPath Keylight** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="c4855-170">On the **LockPath Keylight Configuration** section, click **Configure LockPath Keylight** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c4855-171">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="c4855-171">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_configure.png) 

7. <span data-ttu-id="c4855-173">Pokud chcete povolit jednotné přihlašování v LockPath Keylight, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c4855-173">To enable SSO in LockPath Keylight, perform the following steps:</span></span>
   
    <span data-ttu-id="c4855-174">a.</span><span class="sxs-lookup"><span data-stu-id="c4855-174">a.</span></span> <span data-ttu-id="c4855-175">Přihlášení k účtu LockPath Keylight jako správce.</span><span class="sxs-lookup"><span data-stu-id="c4855-175">Sign-on to your LockPath Keylight account as administrator.</span></span>
    
    <span data-ttu-id="c4855-176">b.</span><span class="sxs-lookup"><span data-stu-id="c4855-176">b.</span></span> <span data-ttu-id="c4855-177">V nabídce v horní části, klikněte na tlačítko **osoba**a vyberte **Keylight instalace**.</span><span class="sxs-lookup"><span data-stu-id="c4855-177">In the menu on the top, click **Person**, and select **Keylight Setup**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/401.png) 

    <span data-ttu-id="c4855-179">c.</span><span class="sxs-lookup"><span data-stu-id="c4855-179">c.</span></span> <span data-ttu-id="c4855-180">Ve stromovém zobrazení na levé straně, klikněte na tlačítko **SAML**.</span><span class="sxs-lookup"><span data-stu-id="c4855-180">In the treeview on the left, click **SAML**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/402.png) 

    <span data-ttu-id="c4855-182">d.</span><span class="sxs-lookup"><span data-stu-id="c4855-182">d.</span></span> <span data-ttu-id="c4855-183">Na **SAML nastavení** dialogové okno, klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="c4855-183">On the **SAML Settings** dialog, click **Edit**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/404.png) 

8. <span data-ttu-id="c4855-185">Na **upravit nastavení SAML** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c4855-185">On the **Edit SAML Settings** dialog page, perform the following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/405.png) 
   
    <span data-ttu-id="c4855-187">a.</span><span class="sxs-lookup"><span data-stu-id="c4855-187">a.</span></span> <span data-ttu-id="c4855-188">Nastavit **ověřování SAML** k **Active**.</span><span class="sxs-lookup"><span data-stu-id="c4855-188">Set **SAML authentication** to **Active**.</span></span>

    <span data-ttu-id="c4855-189">b.</span><span class="sxs-lookup"><span data-stu-id="c4855-189">b.</span></span> <span data-ttu-id="c4855-190">Vložení **SAML jeden přihlašování adresa URL služby** hodnotu, která jste zkopírovali z portálu Azure do **adresu URL pro přihlášení zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c4855-190">Paste the **SAML Single Sign-On Service URL** value which you have copied from the Azure portal into the **Identity Provider Login URL** textbox.</span></span>

    <span data-ttu-id="c4855-191">c.</span><span class="sxs-lookup"><span data-stu-id="c4855-191">c.</span></span> <span data-ttu-id="c4855-192">Vložení **jednu adresu URL služby Sign-Out** hodnotu, která jste zkopírovali z portálu Azure do **adresa URL odhlašovací zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c4855-192">Paste the **Single Sign-Out Service URL** value which you have copied from the Azure portal into the **Identity Provider Logout URL** textbox.</span></span>

    <span data-ttu-id="c4855-193">d.</span><span class="sxs-lookup"><span data-stu-id="c4855-193">d.</span></span> <span data-ttu-id="c4855-194">Klikněte na tlačítko **zvolit soubor** vyberte svůj stažený certifikát LockPath Keylight, a pak klikněte na **otevřete** na kterou odešlete certifikát.</span><span class="sxs-lookup"><span data-stu-id="c4855-194">Click **Choose File** to select your downloaded LockPath Keylight certificate, and then click **Open** to upload the certificate.</span></span>

    <span data-ttu-id="c4855-195">e.</span><span class="sxs-lookup"><span data-stu-id="c4855-195">e.</span></span> <span data-ttu-id="c4855-196">Nastavit **Id uživatele SAML umístění** k **NameIdentifier element příkaz subjektu**.</span><span class="sxs-lookup"><span data-stu-id="c4855-196">Set **SAML User Id location** to **NameIdentifier element of the subject statement**.</span></span>
    
    <span data-ttu-id="c4855-197">f.</span><span class="sxs-lookup"><span data-stu-id="c4855-197">f.</span></span> <span data-ttu-id="c4855-198">Zadejte **poskytovatele služeb Keylight** pomocí následujícího vzorce: **https://&lt;#companyname&gt;. keylightgrc.com**.</span><span class="sxs-lookup"><span data-stu-id="c4855-198">Provide the **Keylight Service Provider** using the following pattern: **https://&lt;CompanyName&gt;.keylightgrc.com**.</span></span>
    
    <span data-ttu-id="c4855-199">g.</span><span class="sxs-lookup"><span data-stu-id="c4855-199">g.</span></span> <span data-ttu-id="c4855-200">Nastavit **automatického zřizování uživatelů** k **Active**.</span><span class="sxs-lookup"><span data-stu-id="c4855-200">Set **Auto-provision users** to **Active**.</span></span>

    <span data-ttu-id="c4855-201">h.</span><span class="sxs-lookup"><span data-stu-id="c4855-201">h.</span></span> <span data-ttu-id="c4855-202">Nastavit **typ účtu automatického zřizování** k **úplné uživatelské**.</span><span class="sxs-lookup"><span data-stu-id="c4855-202">Set **Auto-provision account type** to **Full User**.</span></span>

    <span data-ttu-id="c4855-203">i.</span><span class="sxs-lookup"><span data-stu-id="c4855-203">i.</span></span> <span data-ttu-id="c4855-204">Nastavit **role zabezpečení automatického zřizování**, vyberte **standardní uživatel s SAML**.</span><span class="sxs-lookup"><span data-stu-id="c4855-204">Set **Auto-provision security role**, select **Standard User with SAML**.</span></span>
    
    <span data-ttu-id="c4855-205">j.</span><span class="sxs-lookup"><span data-stu-id="c4855-205">j.</span></span> <span data-ttu-id="c4855-206">Nastavit **konfigurace zabezpečení automatického zřizování**, vyberte **standardní konfigurace uživatele**.</span><span class="sxs-lookup"><span data-stu-id="c4855-206">Set **Auto-provision security config**, select **Standard User Configuration**.</span></span>
     
    <span data-ttu-id="c4855-207">kB.</span><span class="sxs-lookup"><span data-stu-id="c4855-207">k.</span></span> <span data-ttu-id="c4855-208">V **atribut e-mailu** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="c4855-208">In the **Email attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
    
    <span data-ttu-id="c4855-209">l.</span><span class="sxs-lookup"><span data-stu-id="c4855-209">l.</span></span> <span data-ttu-id="c4855-210">V **křestní jméno atribut** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="c4855-210">In the **First name attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>
    
    <span data-ttu-id="c4855-211">m.</span><span class="sxs-lookup"><span data-stu-id="c4855-211">m.</span></span> <span data-ttu-id="c4855-212">V **poslední atribut name** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="c4855-212">In the **Last name attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>
    
    <span data-ttu-id="c4855-213">n.</span><span class="sxs-lookup"><span data-stu-id="c4855-213">n.</span></span> <span data-ttu-id="c4855-214">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c4855-214">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="c4855-215">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="c4855-215">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c4855-216">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="c4855-216">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c4855-217">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c4855-217">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c4855-218">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c4855-218">Creating an Azure AD test user</span></span>
<span data-ttu-id="c4855-219">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c4855-219">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c4855-221">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c4855-221">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c4855-222">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c4855-222">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c4855-224">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c4855-224">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c4855-226">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c4855-226">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c4855-228">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c4855-228">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c4855-230">a.</span><span class="sxs-lookup"><span data-stu-id="c4855-230">a.</span></span> <span data-ttu-id="c4855-231">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c4855-231">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c4855-232">b.</span><span class="sxs-lookup"><span data-stu-id="c4855-232">b.</span></span> <span data-ttu-id="c4855-233">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c4855-233">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c4855-234">c.</span><span class="sxs-lookup"><span data-stu-id="c4855-234">c.</span></span> <span data-ttu-id="c4855-235">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c4855-235">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c4855-236">d.</span><span class="sxs-lookup"><span data-stu-id="c4855-236">d.</span></span> <span data-ttu-id="c4855-237">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c4855-237">Click **Create**.</span></span>
 
### <a name="creating-a-lockpath-keylight-test-user"></a><span data-ttu-id="c4855-238">Vytvoření zkušebního uživatele LockPath Keylight</span><span class="sxs-lookup"><span data-stu-id="c4855-238">Creating a LockPath Keylight test user</span></span>

<span data-ttu-id="c4855-239">V této části vytvoříte volal Britta Simon v LockPath Keylight uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4855-239">In this section, you create a user called Britta Simon in LockPath Keylight.</span></span> <span data-ttu-id="c4855-240">LockPath Keylight podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="c4855-240">LockPath Keylight supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="c4855-241">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="c4855-241">There is no action item for you in this section.</span></span> <span data-ttu-id="c4855-242">Nový uživatel se vytvoří při přístupu k LockPath Keylight, pokud uživatel ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c4855-242">A new user is created when accessing LockPath Keylight if the user doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="c4855-243">Pokud potřebujete ručně vytvořit uživatele, budete muset kontaktovat [tým podpory klienta Keylight LockPath](https://www.lockpath.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="c4855-243">If you need to create a user manually, you need to contact the [LockPath Keylight Client support team](https://www.lockpath.com/contact/).</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c4855-244">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c4855-244">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c4855-245">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu LockPath Keylight.</span><span class="sxs-lookup"><span data-stu-id="c4855-245">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LockPath Keylight.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c4855-247">**Pokud chcete přiřadit Britta Simon LockPath Keylight, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c4855-247">**To assign Britta Simon to LockPath Keylight, perform the following steps:**</span></span>

1. <span data-ttu-id="c4855-248">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c4855-248">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c4855-250">V seznamu aplikací vyberte **LockPath Keylight**.</span><span class="sxs-lookup"><span data-stu-id="c4855-250">In the applications list, select **LockPath Keylight**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_app.png) 

3. <span data-ttu-id="c4855-252">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c4855-252">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c4855-254">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c4855-254">Click **Add** button.</span></span> <span data-ttu-id="c4855-255">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c4855-255">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c4855-257">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c4855-257">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c4855-258">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c4855-258">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c4855-259">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c4855-259">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c4855-260">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c4855-260">Testing single sign-on</span></span>

<span data-ttu-id="c4855-261">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c4855-261">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c4855-262">Když kliknete na dlaždici LockPath Keylight na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci LockPath Keylight.</span><span class="sxs-lookup"><span data-stu-id="c4855-262">When you click the LockPath Keylight tile in the Access Panel, you should get automatically signed-on to your LockPath Keylight application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c4855-263">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c4855-263">Additional resources</span></span>

* [<span data-ttu-id="c4855-264">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c4855-264">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c4855-265">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c4855-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png

