---
title: 'Kurz: Azure Active Directory integrace s Kintone | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Kintone."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2b947dc-e1a8-4f5f-b40e-2c5180648e4f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: e5e847c12cba3611ce7ea2c3e956dbd55b1e0cac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kintone"></a><span data-ttu-id="971c5-103">Kurz: Azure Active Directory integrace s Kintone</span><span class="sxs-lookup"><span data-stu-id="971c5-103">Tutorial: Azure Active Directory integration with Kintone</span></span>

<span data-ttu-id="971c5-104">V tomto kurzu zjistěte, jak integrovat Kintone s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="971c5-104">In this tutorial, you learn how to integrate Kintone with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="971c5-105">Integrace Kintone s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="971c5-105">Integrating Kintone with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="971c5-106">Můžete řídit ve službě Azure AD, který má přístup k Kintone</span><span class="sxs-lookup"><span data-stu-id="971c5-106">You can control in Azure AD who has access to Kintone</span></span>
- <span data-ttu-id="971c5-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Kintone (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="971c5-107">You can enable your users to automatically get signed-on to Kintone (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="971c5-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="971c5-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="971c5-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="971c5-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="971c5-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="971c5-110">Prerequisites</span></span>

<span data-ttu-id="971c5-111">Konfigurace integrace Azure AD s Kintone, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="971c5-111">To configure Azure AD integration with Kintone, you need the following items:</span></span>

- <span data-ttu-id="971c5-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="971c5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="971c5-113">Kintone jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="971c5-113">A Kintone single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="971c5-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="971c5-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="971c5-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="971c5-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="971c5-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="971c5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="971c5-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="971c5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="971c5-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="971c5-118">Scenario description</span></span>
<span data-ttu-id="971c5-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="971c5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="971c5-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="971c5-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="971c5-121">Přidání Kintone z Galerie</span><span class="sxs-lookup"><span data-stu-id="971c5-121">Adding Kintone from the gallery</span></span>
2. <span data-ttu-id="971c5-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="971c5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kintone-from-the-gallery"></a><span data-ttu-id="971c5-123">Přidání Kintone z Galerie</span><span class="sxs-lookup"><span data-stu-id="971c5-123">Adding Kintone from the gallery</span></span>
<span data-ttu-id="971c5-124">Při konfiguraci integrace Kintone do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Kintone z galerie.</span><span class="sxs-lookup"><span data-stu-id="971c5-124">To configure the integration of Kintone into Azure AD, you need to add Kintone from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="971c5-125">**Pokud chcete přidat Kintone z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="971c5-125">**To add Kintone from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="971c5-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="971c5-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="971c5-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="971c5-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="971c5-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="971c5-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="971c5-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="971c5-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="971c5-133">Do vyhledávacího pole zadejte **Kintone**.</span><span class="sxs-lookup"><span data-stu-id="971c5-133">In the search box, type **Kintone**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_search.png)

5. <span data-ttu-id="971c5-135">Na panelu výsledků vyberte **Kintone**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="971c5-135">In the results panel, select **Kintone**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="971c5-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="971c5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="971c5-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Kintone podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="971c5-138">In this section, you configure and test Azure AD single sign-on with Kintone based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="971c5-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Kintone je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="971c5-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Kintone is to a user in Azure AD.</span></span> <span data-ttu-id="971c5-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Kintone musí navázat.</span><span class="sxs-lookup"><span data-stu-id="971c5-140">In other words, a link relationship between an Azure AD user and the related user in Kintone needs to be established.</span></span>

<span data-ttu-id="971c5-141">V Kintone, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="971c5-141">In Kintone, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="971c5-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Kintone, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="971c5-142">To configure and test Azure AD single sign-on with Kintone, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="971c5-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="971c5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="971c5-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="971c5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="971c5-145">**[Vytvoření zkušebního uživatele Kintone](#creating-a-kintone-test-user)**  – Pokud chcete mít protějšek Britta Simon v Kintone propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="971c5-145">**[Creating a Kintone test user](#creating-a-kintone-test-user)** - to have a counterpart of Britta Simon in Kintone that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="971c5-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="971c5-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="971c5-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="971c5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="971c5-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="971c5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="971c5-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Kintone.</span><span class="sxs-lookup"><span data-stu-id="971c5-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kintone application.</span></span>

<span data-ttu-id="971c5-150">**Ke konfiguraci Azure AD jednotné přihlašování s Kintone, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="971c5-150">**To configure Azure AD single sign-on with Kintone, perform the following steps:**</span></span>

1. <span data-ttu-id="971c5-151">Na portálu Azure na **Kintone** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="971c5-151">In the Azure portal, on the **Kintone** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="971c5-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="971c5-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_samlbase.png)

3. <span data-ttu-id="971c5-155">Na **Kintone domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="971c5-155">On the **Kintone Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_url.png)

    <span data-ttu-id="971c5-157">a.</span><span class="sxs-lookup"><span data-stu-id="971c5-157">a.</span></span> <span data-ttu-id="971c5-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.kintone.com`</span><span class="sxs-lookup"><span data-stu-id="971c5-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.kintone.com`</span></span>

    <span data-ttu-id="971c5-159">b.</span><span class="sxs-lookup"><span data-stu-id="971c5-159">b.</span></span> <span data-ttu-id="971c5-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="971c5-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.cybozu.com`|
    | `https://<companyname>.kintone.com`|

    > [!NOTE] 
    > <span data-ttu-id="971c5-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="971c5-161">These values are not real.</span></span> <span data-ttu-id="971c5-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="971c5-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="971c5-163">Obraťte se na [tým podpory Kintone klienta](https://www.kintone.com/contact/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="971c5-163">Contact [Kintone Client support team](https://www.kintone.com/contact/) to get these values.</span></span> 
 
4. <span data-ttu-id="971c5-164">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="971c5-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_certificate.png) 

5. <span data-ttu-id="971c5-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="971c5-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kintone-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="971c5-168">Na **Kintone konfigurace** klikněte na tlačítko **konfigurace Kintone** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="971c5-168">On the **Kintone Configuration** section, click **Configure Kintone** to open **Configure sign-on** window.</span></span> <span data-ttu-id="971c5-169">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="971c5-169">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_configure.png) 

7. <span data-ttu-id="971c5-171">V okně prohlížeče jiný web, přihlaste se k vaší **Kintone** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="971c5-171">In a different web browser window, log into your **Kintone** company site as an administrator.</span></span>

8. <span data-ttu-id="971c5-172">Klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="971c5-172">Click **Settings**.</span></span>
   
    <span data-ttu-id="971c5-173">![Nastavení](./media/active-directory-saas-kintone-tutorial/ic785879.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="971c5-173">![Settings](./media/active-directory-saas-kintone-tutorial/ic785879.png "Settings")</span></span>

9. <span data-ttu-id="971c5-174">Klikněte na tlačítko **uživatelů a správu systému**.</span><span class="sxs-lookup"><span data-stu-id="971c5-174">Click **Users & System Administration**.</span></span>
   
    <span data-ttu-id="971c5-175">![Uživatelé a správu systému](./media/active-directory-saas-kintone-tutorial/ic785880.png "uživatelů a správu systému")</span><span class="sxs-lookup"><span data-stu-id="971c5-175">![Users & System Administration](./media/active-directory-saas-kintone-tutorial/ic785880.png "Users & System Administration")</span></span>

10. <span data-ttu-id="971c5-176">V části **systému správy \> zabezpečení** klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="971c5-176">Under **System Administration \> Security** click **Login**.</span></span>
   
    <span data-ttu-id="971c5-177">![Přihlášení](./media/active-directory-saas-kintone-tutorial/ic785881.png "přihlášení")</span><span class="sxs-lookup"><span data-stu-id="971c5-177">![Login](./media/active-directory-saas-kintone-tutorial/ic785881.png "Login")</span></span>

11. <span data-ttu-id="971c5-178">Klikněte na tlačítko **ověřování povolit SAML**.</span><span class="sxs-lookup"><span data-stu-id="971c5-178">Click **Enable SAML authentication**.</span></span>
   
    <span data-ttu-id="971c5-179">![Ověřování SAML](./media/active-directory-saas-kintone-tutorial/ic785882.png "ověřování SAML")</span><span class="sxs-lookup"><span data-stu-id="971c5-179">![SAML Authentication](./media/active-directory-saas-kintone-tutorial/ic785882.png "SAML Authentication")</span></span>

12. <span data-ttu-id="971c5-180">V části ověřování SAML proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="971c5-180">In the SAML Authentication section, perform the following steps:</span></span>
    
    <span data-ttu-id="971c5-181">![Ověřování SAML](./media/active-directory-saas-kintone-tutorial/ic785883.png "ověřování SAML")</span><span class="sxs-lookup"><span data-stu-id="971c5-181">![SAML Authentication](./media/active-directory-saas-kintone-tutorial/ic785883.png "SAML Authentication")</span></span>
    
    <span data-ttu-id="971c5-182">a.</span><span class="sxs-lookup"><span data-stu-id="971c5-182">a.</span></span> <span data-ttu-id="971c5-183">V **přihlašovací adresa URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="971c5-183">In the **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="971c5-184">b.</span><span class="sxs-lookup"><span data-stu-id="971c5-184">b.</span></span> <span data-ttu-id="971c5-185">V **adresy URL odhlašovací** textovému poli, vložte hodnotu **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="971c5-185">In the **Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="971c5-186">c.</span><span class="sxs-lookup"><span data-stu-id="971c5-186">c.</span></span> <span data-ttu-id="971c5-187">Klikněte na tlačítko **Procházet** nahrát stažený certifikát.</span><span class="sxs-lookup"><span data-stu-id="971c5-187">Click **Browse** to upload your downloaded certificate.</span></span>
    
    <span data-ttu-id="971c5-188">d.</span><span class="sxs-lookup"><span data-stu-id="971c5-188">d.</span></span> <span data-ttu-id="971c5-189">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="971c5-189">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="971c5-190">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="971c5-190">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="971c5-191">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="971c5-191">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="971c5-192">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="971c5-192">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="971c5-193">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="971c5-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="971c5-194">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="971c5-194">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="971c5-196">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="971c5-196">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="971c5-197">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="971c5-197">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kintone-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="971c5-199">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="971c5-199">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kintone-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="971c5-201">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="971c5-201">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kintone-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="971c5-203">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="971c5-203">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kintone-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="971c5-205">a.</span><span class="sxs-lookup"><span data-stu-id="971c5-205">a.</span></span> <span data-ttu-id="971c5-206">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="971c5-206">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="971c5-207">b.</span><span class="sxs-lookup"><span data-stu-id="971c5-207">b.</span></span> <span data-ttu-id="971c5-208">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="971c5-208">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="971c5-209">c.</span><span class="sxs-lookup"><span data-stu-id="971c5-209">c.</span></span> <span data-ttu-id="971c5-210">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="971c5-210">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="971c5-211">d.</span><span class="sxs-lookup"><span data-stu-id="971c5-211">d.</span></span> <span data-ttu-id="971c5-212">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="971c5-212">Click **Create**.</span></span>
 
### <a name="creating-a-kintone-test-user"></a><span data-ttu-id="971c5-213">Vytvoření zkušebního uživatele Kintone</span><span class="sxs-lookup"><span data-stu-id="971c5-213">Creating a Kintone test user</span></span>

<span data-ttu-id="971c5-214">Pokud chcete povolit uživatelům Azure AD přihlášení k Kintone, musí být zřízená do Kintone.</span><span class="sxs-lookup"><span data-stu-id="971c5-214">To enable Azure AD users to log in to Kintone, they must be provisioned into Kintone.</span></span>  
<span data-ttu-id="971c5-215">V případě Kintone zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="971c5-215">In the case of Kintone, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-account-perform-the-following-steps"></a><span data-ttu-id="971c5-216">K poskytnutí uživatelského účtu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="971c5-216">To provision a user account, perform the following steps:</span></span>

1. <span data-ttu-id="971c5-217">Přihlaste se k vaší **Kintone** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="971c5-217">Log in to your **Kintone** company site as an administrator.</span></span>

2. <span data-ttu-id="971c5-218">Klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="971c5-218">Click **Setting**.</span></span>
   
    <span data-ttu-id="971c5-219">![Nastavení](./media/active-directory-saas-kintone-tutorial/ic785879.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="971c5-219">![Settings](./media/active-directory-saas-kintone-tutorial/ic785879.png "Settings")</span></span>

3. <span data-ttu-id="971c5-220">Klikněte na tlačítko **uživatelů a správu systému**.</span><span class="sxs-lookup"><span data-stu-id="971c5-220">Click **Users & System Administration**.</span></span>
   
    <span data-ttu-id="971c5-221">![& Systém správy uživatelského](./media/active-directory-saas-kintone-tutorial/ic785880.png "uživatele & správu systému")</span><span class="sxs-lookup"><span data-stu-id="971c5-221">![User & System Administration](./media/active-directory-saas-kintone-tutorial/ic785880.png "User & System Administration")</span></span>

4. <span data-ttu-id="971c5-222">V části **Správa uživatele**, klikněte na tlačítko **oddělení a uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="971c5-222">Under **User Administration**, click **Departments & Users**.</span></span>
   
    <span data-ttu-id="971c5-223">![Oddělení a uživatelé](./media/active-directory-saas-kintone-tutorial/ic785888.png "oddělení a uživatelé")</span><span class="sxs-lookup"><span data-stu-id="971c5-223">![Department & Users](./media/active-directory-saas-kintone-tutorial/ic785888.png "Department & Users")</span></span>

5. <span data-ttu-id="971c5-224">Klikněte na tlačítko **nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="971c5-224">Click **New User**.</span></span>
   
    <span data-ttu-id="971c5-225">![Noví uživatelé](./media/active-directory-saas-kintone-tutorial/ic785889.png "noví uživatelé")</span><span class="sxs-lookup"><span data-stu-id="971c5-225">![New Users](./media/active-directory-saas-kintone-tutorial/ic785889.png "New Users")</span></span>

6. <span data-ttu-id="971c5-226">V **nového uživatele** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="971c5-226">In the **New User** section, perform the following steps:</span></span>
   
    <span data-ttu-id="971c5-227">![Noví uživatelé](./media/active-directory-saas-kintone-tutorial/ic785890.png "noví uživatelé")</span><span class="sxs-lookup"><span data-stu-id="971c5-227">![New Users](./media/active-directory-saas-kintone-tutorial/ic785890.png "New Users")</span></span>
   
    <span data-ttu-id="971c5-228">a.</span><span class="sxs-lookup"><span data-stu-id="971c5-228">a.</span></span> <span data-ttu-id="971c5-229">Zadejte **zobrazovaný název**, **přihlašovací jméno**, **nové heslo**, **potvrzení hesla**, **e-mailová adresa**, a další podrobnosti o platný účet AAD chcete mají být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="971c5-229">Type a **Display Name**, **Login Name**, **New Password**, **Confirm Password**, **E-mail Address**, and other details of a valid AAD account you want to provision into the related textboxes.</span></span>
 
    <span data-ttu-id="971c5-230">b.</span><span class="sxs-lookup"><span data-stu-id="971c5-230">b.</span></span> <span data-ttu-id="971c5-231">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="971c5-231">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="971c5-232">Můžete použít všechny ostatní Kintone uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Kintone zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="971c5-232">You can use any other Kintone user account creation tools or APIs provided by Kintone to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="971c5-233">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="971c5-233">Assigning the Azure AD test user</span></span>

<span data-ttu-id="971c5-234">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Kintone.</span><span class="sxs-lookup"><span data-stu-id="971c5-234">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kintone.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="971c5-236">**Pokud chcete přiřadit Britta Simon Kintone, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="971c5-236">**To assign Britta Simon to Kintone, perform the following steps:**</span></span>

1. <span data-ttu-id="971c5-237">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="971c5-237">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="971c5-239">V seznamu aplikací vyberte **Kintone**.</span><span class="sxs-lookup"><span data-stu-id="971c5-239">In the applications list, select **Kintone**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_app.png) 

3. <span data-ttu-id="971c5-241">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="971c5-241">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="971c5-243">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="971c5-243">Click **Add** button.</span></span> <span data-ttu-id="971c5-244">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="971c5-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="971c5-246">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="971c5-246">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="971c5-247">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="971c5-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="971c5-248">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="971c5-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="971c5-249">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="971c5-249">Testing single sign-on</span></span>

<span data-ttu-id="971c5-250">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="971c5-250">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="971c5-251">Když kliknete na dlaždici Kintone na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Kintone.</span><span class="sxs-lookup"><span data-stu-id="971c5-251">When you click the Kintone tile in the Access Panel, you should get automatically signed-on to your Kintone application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="971c5-252">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="971c5-252">Additional resources</span></span>

* [<span data-ttu-id="971c5-253">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="971c5-253">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="971c5-254">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="971c5-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_203.png

