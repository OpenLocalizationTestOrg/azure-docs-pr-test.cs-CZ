---
title: 'Kurz: Azure Active Directory integrace s UserEcho | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a UserEcho."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bedd916b-8f69-4b50-9b8d-56f4ee3bd3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 2d824d8d5eb8a25db128397b908a126bd87213ea
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-userecho"></a><span data-ttu-id="6d184-103">Kurz: Azure Active Directory integrace s UserEcho</span><span class="sxs-lookup"><span data-stu-id="6d184-103">Tutorial: Azure Active Directory integration with UserEcho</span></span>

<span data-ttu-id="6d184-104">V tomto kurzu zjistěte, jak integrovat UserEcho s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6d184-104">In this tutorial, you learn how to integrate UserEcho with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6d184-105">Integrace UserEcho s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="6d184-105">Integrating UserEcho with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="6d184-106">Můžete řídit ve službě Azure AD, který má přístup k UserEcho</span><span class="sxs-lookup"><span data-stu-id="6d184-106">You can control in Azure AD who has access to UserEcho</span></span>
- <span data-ttu-id="6d184-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k UserEcho (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="6d184-107">You can enable your users to automatically get signed-on to UserEcho (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6d184-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6d184-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="6d184-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6d184-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6d184-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6d184-110">Prerequisites</span></span>

<span data-ttu-id="6d184-111">Konfigurace integrace Azure AD s UserEcho, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="6d184-111">To configure Azure AD integration with UserEcho, you need the following items:</span></span>

- <span data-ttu-id="6d184-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="6d184-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6d184-113">UserEcho jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="6d184-113">A UserEcho single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6d184-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6d184-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6d184-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="6d184-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6d184-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="6d184-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6d184-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6d184-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6d184-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="6d184-118">Scenario description</span></span>
<span data-ttu-id="6d184-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="6d184-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6d184-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="6d184-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6d184-121">Přidání UserEcho z Galerie</span><span class="sxs-lookup"><span data-stu-id="6d184-121">Adding UserEcho from the gallery</span></span>
2. <span data-ttu-id="6d184-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6d184-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-userecho-from-the-gallery"></a><span data-ttu-id="6d184-123">Přidání UserEcho z Galerie</span><span class="sxs-lookup"><span data-stu-id="6d184-123">Adding UserEcho from the gallery</span></span>
<span data-ttu-id="6d184-124">Při konfiguraci integrace UserEcho do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS UserEcho z galerie.</span><span class="sxs-lookup"><span data-stu-id="6d184-124">To configure the integration of UserEcho into Azure AD, you need to add UserEcho from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6d184-125">**Pokud chcete přidat UserEcho z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6d184-125">**To add UserEcho from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="6d184-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6d184-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6d184-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="6d184-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="6d184-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6d184-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="6d184-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6d184-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="6d184-133">Do vyhledávacího pole zadejte **UserEcho**.</span><span class="sxs-lookup"><span data-stu-id="6d184-133">In the search box, type **UserEcho**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_search.png)

5. <span data-ttu-id="6d184-135">Na panelu výsledků vyberte **UserEcho**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6d184-135">In the results panel, select **UserEcho**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6d184-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6d184-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6d184-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s UserEcho podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="6d184-138">In this section, you configure and test Azure AD single sign-on with UserEcho based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6d184-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v UserEcho je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6d184-139">For single sign-on to work, Azure AD needs to know what the counterpart user in UserEcho is to a user in Azure AD.</span></span> <span data-ttu-id="6d184-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v UserEcho musí navázat.</span><span class="sxs-lookup"><span data-stu-id="6d184-140">In other words, a link relationship between an Azure AD user and the related user in UserEcho needs to be established.</span></span>

<span data-ttu-id="6d184-141">V UserEcho, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="6d184-141">In UserEcho, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="6d184-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s UserEcho, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="6d184-142">To configure and test Azure AD single sign-on with UserEcho, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="6d184-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="6d184-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="6d184-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6d184-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6d184-145">**[Vytvoření zkušebního uživatele UserEcho](#creating-a-userecho-test-user)**  – Pokud chcete mít protějšek Britta Simon v UserEcho propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="6d184-145">**[Creating a UserEcho test user](#creating-a-userecho-test-user)** - to have a counterpart of Britta Simon in UserEcho that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="6d184-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6d184-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6d184-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6d184-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6d184-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6d184-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6d184-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci UserEcho.</span><span class="sxs-lookup"><span data-stu-id="6d184-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your UserEcho application.</span></span>

<span data-ttu-id="6d184-150">**Ke konfiguraci Azure AD jednotné přihlašování s UserEcho, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6d184-150">**To configure Azure AD single sign-on with UserEcho, perform the following steps:**</span></span>

1. <span data-ttu-id="6d184-151">Na portálu Azure na **UserEcho** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6d184-151">In the Azure portal, on the **UserEcho** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="6d184-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6d184-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_samlbase.png)

3. <span data-ttu-id="6d184-155">Na **UserEcho domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6d184-155">On the **UserEcho Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_url.png)

    <span data-ttu-id="6d184-157">a.</span><span class="sxs-lookup"><span data-stu-id="6d184-157">a.</span></span> <span data-ttu-id="6d184-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.userecho.com/`</span><span class="sxs-lookup"><span data-stu-id="6d184-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.userecho.com/`</span></span>

    <span data-ttu-id="6d184-159">b.</span><span class="sxs-lookup"><span data-stu-id="6d184-159">b.</span></span> <span data-ttu-id="6d184-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.userecho.com/saml/metadata/`</span><span class="sxs-lookup"><span data-stu-id="6d184-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.userecho.com/saml/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6d184-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="6d184-161">These values are not real.</span></span> <span data-ttu-id="6d184-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="6d184-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="6d184-163">Obraťte se na [tým podpory UserEcho klienta](https://feedback.userecho.com/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="6d184-163">Contact [UserEcho Client support team](https://feedback.userecho.com/) to get these values.</span></span> 

4. <span data-ttu-id="6d184-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="6d184-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_certificate.png) 

5. <span data-ttu-id="6d184-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6d184-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6d184-168">Na **UserEcho konfigurace** klikněte na tlačítko **konfigurace UserEcho** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="6d184-168">On the **UserEcho Configuration** section, click **Configure UserEcho** to open **Configure sign-on** window.</span></span> <span data-ttu-id="6d184-169">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="6d184-169">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_configure.png) 

7. <span data-ttu-id="6d184-171">V jiném okně prohlížeče Přihlaste se k serveru vaší společnosti UserEcho jako správce.</span><span class="sxs-lookup"><span data-stu-id="6d184-171">In another browser window, sign on to your UserEcho company site as an administrator.</span></span>

8. <span data-ttu-id="6d184-172">Na panelu nástrojů v horní části na své uživatelské jméno, rozbalte nabídku a pak klikněte na **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="6d184-172">In the toolbar on the top, click your user name to expand the menu, and then click **Setup**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png) 

9. <span data-ttu-id="6d184-174">Klikněte na tlačítko **integrace**.</span><span class="sxs-lookup"><span data-stu-id="6d184-174">Click **Integrations**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_07.png) 

10. <span data-ttu-id="6d184-176">Klikněte na tlačítko **webu**a potom klikněte na **jednotné přihlašování (typu SAML2)**.</span><span class="sxs-lookup"><span data-stu-id="6d184-176">Click **Website**, and then click **Single sign-on (SAML2)**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_08.png) 

11. <span data-ttu-id="6d184-178">Na **jednotné přihlašování (SAML)** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6d184-178">On the **Single sign-on (SAML)** page, perform the following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_09.png)
    
    <span data-ttu-id="6d184-180">a.</span><span class="sxs-lookup"><span data-stu-id="6d184-180">a.</span></span> <span data-ttu-id="6d184-181">Jako **povoleno SAML**, vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="6d184-181">As **SAML-enabled**, select **Yes**.</span></span>
    
    <span data-ttu-id="6d184-182">b.</span><span class="sxs-lookup"><span data-stu-id="6d184-182">b.</span></span> <span data-ttu-id="6d184-183">Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure do **URL jednotné přihlašování SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="6d184-183">Paste **SAML Single Sign-On Service URL**, which you have copied from the Azure portal into the **SAML SSO URL** textbox.</span></span>
    
    <span data-ttu-id="6d184-184">c.</span><span class="sxs-lookup"><span data-stu-id="6d184-184">c.</span></span> <span data-ttu-id="6d184-185">Vložení **Sign-Out URL**, který jste zkopírovali z portálu Azure do **adresy URL vzdálené logoout** textové pole.</span><span class="sxs-lookup"><span data-stu-id="6d184-185">Paste **Sign-Out URL**, which you have copied from the Azure portal into the **Remote logoout URL** textbox.</span></span>
    
    <span data-ttu-id="6d184-186">d.</span><span class="sxs-lookup"><span data-stu-id="6d184-186">d.</span></span> <span data-ttu-id="6d184-187">V poznámkovém bloku otevřete stažený certifikát, kopírovat obsah a vložte ji do **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="6d184-187">Open your downloaded certificate in Notepad, copy the content, and then paste it into the **X.509 Certificate** textbox.</span></span>
    
    <span data-ttu-id="6d184-188">e.</span><span class="sxs-lookup"><span data-stu-id="6d184-188">e.</span></span> <span data-ttu-id="6d184-189">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6d184-189">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="6d184-190">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="6d184-190">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="6d184-191">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="6d184-191">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="6d184-192">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6d184-192">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6d184-193">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6d184-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="6d184-194">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6d184-194">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="6d184-196">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6d184-196">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="6d184-197">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6d184-197">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6d184-199">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="6d184-199">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6d184-201">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6d184-201">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6d184-203">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6d184-203">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6d184-205">a.</span><span class="sxs-lookup"><span data-stu-id="6d184-205">a.</span></span> <span data-ttu-id="6d184-206">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6d184-206">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6d184-207">b.</span><span class="sxs-lookup"><span data-stu-id="6d184-207">b.</span></span> <span data-ttu-id="6d184-208">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6d184-208">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6d184-209">c.</span><span class="sxs-lookup"><span data-stu-id="6d184-209">c.</span></span> <span data-ttu-id="6d184-210">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="6d184-210">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="6d184-211">d.</span><span class="sxs-lookup"><span data-stu-id="6d184-211">d.</span></span> <span data-ttu-id="6d184-212">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6d184-212">Click **Create**.</span></span>
 
### <a name="creating-a-userecho-test-user"></a><span data-ttu-id="6d184-213">Vytvoření zkušebního uživatele UserEcho</span><span class="sxs-lookup"><span data-stu-id="6d184-213">Creating a UserEcho test user</span></span>

<span data-ttu-id="6d184-214">Cílem této části je vytvoření uživatele v UserEcho nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6d184-214">The objective of this section is to create a user called Britta Simon in UserEcho.</span></span>

<span data-ttu-id="6d184-215">**Vytvoření uživatele v UserEcho nazývá Britta Simon, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6d184-215">**To create a user called Britta Simon in UserEcho, perform the following steps:**</span></span>

1. <span data-ttu-id="6d184-216">Přihlašování k webu společnosti UserEcho jako správce.</span><span class="sxs-lookup"><span data-stu-id="6d184-216">Sign-on to your UserEcho company site as an administrator.</span></span>

2. <span data-ttu-id="6d184-217">Na panelu nástrojů v horní části na své uživatelské jméno, rozbalte nabídku a pak klikněte na **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="6d184-217">In the toolbar on the top, click your user name to expand the menu, and then click **Setup**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png)

3. <span data-ttu-id="6d184-219">Klikněte na tlačítko **uživatelé**, chcete-li rozšířit **uživatelé** části.</span><span class="sxs-lookup"><span data-stu-id="6d184-219">Click **Users**, to expand the **Users** section.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_10.png)

4. <span data-ttu-id="6d184-221">Klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="6d184-221">Click **Users**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_11.png)

5. <span data-ttu-id="6d184-223">Klikněte na tlačítko **pozvat nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="6d184-223">Click **Invite a new user**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_12.png)

6. <span data-ttu-id="6d184-225">Na **pozvat nového uživatele** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6d184-225">On the **Invite a new user** dialog, perform the following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_13.png)

    <span data-ttu-id="6d184-227">a.</span><span class="sxs-lookup"><span data-stu-id="6d184-227">a.</span></span> <span data-ttu-id="6d184-228">V **název** textovému poli, název typu uživatele jako Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6d184-228">In the **Name** textbox, type name of the user like Britta Simon.</span></span>
    
    <span data-ttu-id="6d184-229">b.</span><span class="sxs-lookup"><span data-stu-id="6d184-229">b.</span></span>  <span data-ttu-id="6d184-230">V **e-mailu** jako typ e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="6d184-230">In the **Email** textbox, type the email address of user like Brittasimon@contoso.com.</span></span>
    
    <span data-ttu-id="6d184-231">c.</span><span class="sxs-lookup"><span data-stu-id="6d184-231">c.</span></span> <span data-ttu-id="6d184-232">Klikněte na tlačítko **pozvat**.</span><span class="sxs-lookup"><span data-stu-id="6d184-232">Click **Invite**.</span></span>

<span data-ttu-id="6d184-233">Pozvánka se odesílají do Britta, který mu pomůže začít používat UserEcho umožňuje.</span><span class="sxs-lookup"><span data-stu-id="6d184-233">An invitation is sent to Britta, which enables her to start using UserEcho.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="6d184-234">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6d184-234">Assigning the Azure AD test user</span></span>

<span data-ttu-id="6d184-235">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu UserEcho.</span><span class="sxs-lookup"><span data-stu-id="6d184-235">In this section, you enable Britta Simon to use Azure single sign-on by granting access to UserEcho.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="6d184-237">**Pokud chcete přiřadit Britta Simon UserEcho, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6d184-237">**To assign Britta Simon to UserEcho, perform the following steps:**</span></span>

1. <span data-ttu-id="6d184-238">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6d184-238">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="6d184-240">V seznamu aplikací vyberte **UserEcho**.</span><span class="sxs-lookup"><span data-stu-id="6d184-240">In the applications list, select **UserEcho**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_app.png) 

3. <span data-ttu-id="6d184-242">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="6d184-242">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="6d184-244">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6d184-244">Click **Add** button.</span></span> <span data-ttu-id="6d184-245">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6d184-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="6d184-247">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6d184-247">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="6d184-248">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6d184-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6d184-249">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6d184-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6d184-250">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6d184-250">Testing single sign-on</span></span>

<span data-ttu-id="6d184-251">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="6d184-251">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="6d184-252">Když kliknete na dlaždici UserEcho na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci UserEcho.</span><span class="sxs-lookup"><span data-stu-id="6d184-252">When you click the UserEcho tile in the Access Panel, you should get automatically signed-on to your UserEcho application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6d184-253">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6d184-253">Additional resources</span></span>

* [<span data-ttu-id="6d184-254">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6d184-254">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6d184-255">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6d184-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_203.png

