---
title: 'Kurz: Azure Active Directory integrace s Kindling | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Kindling."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 71229751-74eb-4c2c-abb4-07caa95754c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 131c2c3f46c60193d512b1779e917c8322732fbc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kindling"></a><span data-ttu-id="5a05a-103">Kurz: Azure Active Directory integrace s Kindling</span><span class="sxs-lookup"><span data-stu-id="5a05a-103">Tutorial: Azure Active Directory integration with Kindling</span></span>

<span data-ttu-id="5a05a-104">V tomto kurzu zjistěte, jak integrovat Kindling s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5a05a-104">In this tutorial, you learn how to integrate Kindling with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5a05a-105">Integrace Kindling s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5a05a-105">Integrating Kindling with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5a05a-106">Můžete řídit ve službě Azure AD, který má přístup k Kindling</span><span class="sxs-lookup"><span data-stu-id="5a05a-106">You can control in Azure AD who has access to Kindling</span></span>
- <span data-ttu-id="5a05a-107">Můžete povolit uživatelům automaticky přihlášení k získání Kindling (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5a05a-107">You can enable your users to automatically get signed-on to Kindling (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5a05a-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5a05a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5a05a-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, přečtěte si téma.</span><span class="sxs-lookup"><span data-stu-id="5a05a-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="5a05a-110">[Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5a05a-110">[what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5a05a-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5a05a-111">Prerequisites</span></span>

<span data-ttu-id="5a05a-112">Konfigurace integrace Azure AD s Kindling, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="5a05a-112">To configure Azure AD integration with Kindling, you need the following items:</span></span>

- <span data-ttu-id="5a05a-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5a05a-113">An Azure AD subscription</span></span>
- <span data-ttu-id="5a05a-114">Kindling jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5a05a-114">A Kindling single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5a05a-115">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5a05a-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5a05a-116">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5a05a-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5a05a-117">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5a05a-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5a05a-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5a05a-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5a05a-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5a05a-119">Scenario description</span></span>
<span data-ttu-id="5a05a-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5a05a-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5a05a-121">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5a05a-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5a05a-122">Přidání Kindling z Galerie</span><span class="sxs-lookup"><span data-stu-id="5a05a-122">Adding Kindling from the gallery</span></span>
2. <span data-ttu-id="5a05a-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5a05a-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kindling-from-the-gallery"></a><span data-ttu-id="5a05a-124">Přidání Kindling z Galerie</span><span class="sxs-lookup"><span data-stu-id="5a05a-124">Adding Kindling from the gallery</span></span>
<span data-ttu-id="5a05a-125">Chcete-li nakonfigurovat integraci Kindling do služby Azure AD, přidejte Kindling z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5a05a-125">To configure the integration of Kindling into Azure AD, you need to add Kindling from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5a05a-126">**Pokud chcete přidat Kindling z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5a05a-126">**To add Kindling from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5a05a-127">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5a05a-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5a05a-129">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5a05a-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5a05a-130">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5a05a-130">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5a05a-132">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5a05a-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5a05a-134">Do vyhledávacího pole zadejte **Kindling**.</span><span class="sxs-lookup"><span data-stu-id="5a05a-134">In the search box, type **Kindling**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_search.png)

5. <span data-ttu-id="5a05a-136">Na panelu výsledků vyberte **Kindling**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5a05a-136">In the results panel, select **Kindling**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5a05a-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5a05a-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5a05a-139">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Kindling podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="5a05a-139">In this section, you configure and test Azure AD single sign-on with Kindling based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5a05a-140">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Kindling je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5a05a-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Kindling is to a user in Azure AD.</span></span> <span data-ttu-id="5a05a-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Kindling musí být vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="5a05a-141">In other words, a link relationship between an Azure AD user and the related user in Kindling needs to be established.</span></span>

<span data-ttu-id="5a05a-142">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Kindling.</span><span class="sxs-lookup"><span data-stu-id="5a05a-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Kindling.</span></span>

<span data-ttu-id="5a05a-143">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Kindling, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="5a05a-143">To configure and test Azure AD single sign-on with Kindling, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5a05a-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="5a05a-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5a05a-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5a05a-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5a05a-146">**[Vytvoření zkušebního uživatele Kindling](#creating-a-kindling-test-user)**  – Pokud chcete mít protějšek Britta Simon v Kindling propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="5a05a-146">**[Creating a Kindling test user](#creating-a-kindling-test-user)** - to have a counterpart of Britta Simon in Kindling that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5a05a-147">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5a05a-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5a05a-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5a05a-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5a05a-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5a05a-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5a05a-150">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Kindling.</span><span class="sxs-lookup"><span data-stu-id="5a05a-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kindling application.</span></span>

<span data-ttu-id="5a05a-151">**Ke konfiguraci Azure AD jednotné přihlašování s Kindling, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5a05a-151">**To configure Azure AD single sign-on with Kindling, perform the following steps:**</span></span>

1. <span data-ttu-id="5a05a-152">Na portálu Azure na **Kindling** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5a05a-152">In the Azure portal, on the **Kindling** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5a05a-154">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5a05a-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_samlbase.png)

3. <span data-ttu-id="5a05a-156">Na **Kindling domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5a05a-156">On the **Kindling Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_url.png)

    <span data-ttu-id="5a05a-158">a.</span><span class="sxs-lookup"><span data-stu-id="5a05a-158">a.</span></span> <span data-ttu-id="5a05a-159">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.kindlingapp.com`</span><span class="sxs-lookup"><span data-stu-id="5a05a-159">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.kindlingapp.com`</span></span>

    <span data-ttu-id="5a05a-160">b.</span><span class="sxs-lookup"><span data-stu-id="5a05a-160">b.</span></span>  <span data-ttu-id="5a05a-161">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.kindlingapp.com/saml/module.php/saml/sp/metadata.php/clientIDP`</span><span class="sxs-lookup"><span data-stu-id="5a05a-161">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.kindlingapp.com/saml/module.php/saml/sp/metadata.php/clientIDP`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5a05a-162">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="5a05a-162">These values are not the real.</span></span> <span data-ttu-id="5a05a-163">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="5a05a-163">Update these values with the actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="5a05a-164">Obraťte se na [tým podpory Kindling](mailto:support@kindlingapp.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5a05a-164">Contact [Kindling support team](mailto:support@kindlingapp.com) to get these values.</span></span>
 
4. <span data-ttu-id="5a05a-165">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="5a05a-165">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_certificate.png) 

5. <span data-ttu-id="5a05a-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5a05a-167">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kindling-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5a05a-169">Na **Kindling konfigurace** klikněte na tlačítko **konfigurace Kindling** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5a05a-169">On the **Kindling Configuration** section, click **Configure Kindling** to open **Configure sign-on** window.</span></span> <span data-ttu-id="5a05a-170">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5a05a-170">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_configure.png) 

7. <span data-ttu-id="5a05a-172">Konfigurace jednotného přihlašování na **Kindling** straně, budete muset odeslat stažené **certifikátu (Base64)**, **Sign-Out URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby**k [tým podpory Kindling](mailto:support@kindlingapp.com).</span><span class="sxs-lookup"><span data-stu-id="5a05a-172">To configure single sign-on on **Kindling** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Kindling support team](mailto:support@kindlingapp.com).</span></span>

> [!TIP]
> <span data-ttu-id="5a05a-173">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="5a05a-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5a05a-174">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="5a05a-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5a05a-175">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5a05a-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5a05a-176">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5a05a-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="5a05a-177">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5a05a-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5a05a-179">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5a05a-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5a05a-180">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5a05a-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kindling-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5a05a-182">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5a05a-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kindling-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5a05a-184">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5a05a-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kindling-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5a05a-186">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5a05a-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kindling-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5a05a-188">a.</span><span class="sxs-lookup"><span data-stu-id="5a05a-188">a.</span></span> <span data-ttu-id="5a05a-189">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5a05a-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5a05a-190">b.</span><span class="sxs-lookup"><span data-stu-id="5a05a-190">b.</span></span> <span data-ttu-id="5a05a-191">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5a05a-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5a05a-192">c.</span><span class="sxs-lookup"><span data-stu-id="5a05a-192">c.</span></span> <span data-ttu-id="5a05a-193">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5a05a-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5a05a-194">d.</span><span class="sxs-lookup"><span data-stu-id="5a05a-194">d.</span></span> <span data-ttu-id="5a05a-195">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5a05a-195">Click **Create**.</span></span>
 
### <a name="creating-a-kindling-test-user"></a><span data-ttu-id="5a05a-196">Vytvoření zkušebního uživatele Kindling</span><span class="sxs-lookup"><span data-stu-id="5a05a-196">Creating a Kindling test user</span></span>

<span data-ttu-id="5a05a-197">Cílem této části je vytvoření uživatele v Kindling nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5a05a-197">The objective of this section is to create a user called Britta Simon in Kindling.</span></span> <span data-ttu-id="5a05a-198">Kindling podporuje zřizování za běhu.</span><span class="sxs-lookup"><span data-stu-id="5a05a-198">Kindling supports just-in-time provisioning.</span></span> <span data-ttu-id="5a05a-199">Již je v povolíte [konfigurace Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="5a05a-199">You have already enabled it in [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

<span data-ttu-id="5a05a-200">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="5a05a-200">There is no action item for you in this section.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5a05a-201">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5a05a-201">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5a05a-202">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Kindling.</span><span class="sxs-lookup"><span data-stu-id="5a05a-202">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kindling.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5a05a-204">**Pokud chcete přiřadit Britta Simon Kindling, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5a05a-204">**To assign Britta Simon to Kindling, perform the following steps:**</span></span>

1. <span data-ttu-id="5a05a-205">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5a05a-205">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5a05a-207">V seznamu aplikací vyberte **Kindling**.</span><span class="sxs-lookup"><span data-stu-id="5a05a-207">In the applications list, select **Kindling**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_app.png) 

3. <span data-ttu-id="5a05a-209">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5a05a-209">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5a05a-211">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5a05a-211">Click **Add** button.</span></span> <span data-ttu-id="5a05a-212">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5a05a-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5a05a-214">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5a05a-214">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5a05a-215">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5a05a-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5a05a-216">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5a05a-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5a05a-217">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5a05a-217">Testing single sign-on</span></span>

<span data-ttu-id="5a05a-218">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5a05a-218">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="5a05a-219">Když kliknete na dlaždici Kindling na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Kindling.</span><span class="sxs-lookup"><span data-stu-id="5a05a-219">When you click the Kindling tile in the Access Panel, you should get automatically signed-on to your Kindling application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5a05a-220">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5a05a-220">Additional resources</span></span>

* [<span data-ttu-id="5a05a-221">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5a05a-221">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5a05a-222">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5a05a-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_203.png

