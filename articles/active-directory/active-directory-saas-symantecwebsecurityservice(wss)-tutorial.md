---
title: "Kurz: Azure Active Directory integrace s Symantec webové zabezpečení služby (WSS) | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Symantec webové zabezpečení služby (WSS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: d0738bb750a82863b2f77540e8e7b94cca4b64e3
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a><span data-ttu-id="edd90-103">Kurz: Azure Active Directory integrace s Symantec webové zabezpečení služby (WSS)</span><span class="sxs-lookup"><span data-stu-id="edd90-103">Tutorial: Azure Active Directory integration with Symantec Web Security Service (WSS)</span></span>

<span data-ttu-id="edd90-104">V tomto kurzu zjistěte, jak integrovat Symantec webové zabezpečení služby (WSS) s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="edd90-104">In this tutorial, you learn how to integrate Symantec Web Security Service (WSS) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="edd90-105">Integrace Symantec webové zabezpečení služby (WSS) s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="edd90-105">Integrating Symantec Web Security Service (WSS) with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="edd90-106">Můžete řídit ve službě Azure AD, který má přístup k Symantec webové zabezpečení služby (WSS)</span><span class="sxs-lookup"><span data-stu-id="edd90-106">You can control in Azure AD who has access to Symantec Web Security Service (WSS)</span></span>
- <span data-ttu-id="edd90-107">Můžete povolit uživatelům, aby automaticky získat přihlášeného k Symantec webové zabezpečení služby (WSS) (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="edd90-107">You can enable your users to automatically get signed-on to Symantec Web Security Service (WSS) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="edd90-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="edd90-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="edd90-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="edd90-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="edd90-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="edd90-110">Prerequisites</span></span>

<span data-ttu-id="edd90-111">Ke konfiguraci integrace služby Azure AD s Symantec webové zabezpečení služby (WSS), potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="edd90-111">To configure Azure AD integration with Symantec Web Security Service (WSS), you need the following items:</span></span>

- <span data-ttu-id="edd90-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="edd90-112">An Azure AD subscription</span></span>
- <span data-ttu-id="edd90-113">Symantec webové zabezpečení služby (WSS) jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="edd90-113">A Symantec Web Security Service (WSS) single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="edd90-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="edd90-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="edd90-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="edd90-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="edd90-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="edd90-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="edd90-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="edd90-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="edd90-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="edd90-118">Scenario description</span></span>
<span data-ttu-id="edd90-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="edd90-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="edd90-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="edd90-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="edd90-121">Přidání Symantec webové zabezpečení služby (WSS) z Galerie</span><span class="sxs-lookup"><span data-stu-id="edd90-121">Adding Symantec Web Security Service (WSS) from the gallery</span></span>
2. <span data-ttu-id="edd90-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="edd90-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-symantec-web-security-service-wss-from-the-gallery"></a><span data-ttu-id="edd90-123">Přidání Symantec webové zabezpečení služby (WSS) z Galerie</span><span class="sxs-lookup"><span data-stu-id="edd90-123">Adding Symantec Web Security Service (WSS) from the gallery</span></span>
<span data-ttu-id="edd90-124">Při konfiguraci integrace Symantec webové zabezpečení služby (WSS) do služby Azure AD, potřebujete přidat Symantec webové zabezpečení služby (WSS) z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="edd90-124">To configure the integration of Symantec Web Security Service (WSS) into Azure AD, you need to add Symantec Web Security Service (WSS) from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="edd90-125">**Pokud chcete přidat Symantec webové zabezpečení služby (WSS) z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="edd90-125">**To add Symantec Web Security Service (WSS) from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="edd90-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="edd90-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="edd90-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="edd90-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="edd90-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="edd90-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="edd90-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="edd90-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="edd90-133">Do vyhledávacího pole zadejte **Symantec webové zabezpečení služby (WSS)**.</span><span class="sxs-lookup"><span data-stu-id="edd90-133">In the search box, type **Symantec Web Security Service (WSS)**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_search.png)

5. <span data-ttu-id="edd90-135">Na panelu výsledků vyberte **Symantec webové zabezpečení služby (WSS)**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="edd90-135">In the results panel, select **Symantec Web Security Service (WSS)**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="edd90-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="edd90-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="edd90-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Symantec webové zabezpečení služby (WSS) podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="edd90-138">In this section, you configure and test Azure AD single sign-on with Symantec Web Security Service (WSS) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="edd90-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Symantec webové zabezpečení služby (WSS) je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="edd90-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Symantec Web Security Service (WSS) is to a user in Azure AD.</span></span> <span data-ttu-id="edd90-140">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v Symantec webové zabezpečení služby (WSS).</span><span class="sxs-lookup"><span data-stu-id="edd90-140">In other words, a link relationship between an Azure AD user and the related user in Symantec Web Security Service (WSS) needs to be established.</span></span>

<span data-ttu-id="edd90-141">V Symantec webové zabezpečení služby (WSS) přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="edd90-141">In Symantec Web Security Service (WSS), assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="edd90-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Symantec webové zabezpečení služby (WSS), je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="edd90-142">To configure and test Azure AD single sign-on with Symantec Web Security Service (WSS), you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="edd90-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="edd90-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="edd90-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="edd90-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="edd90-145">**[Vytvoření zkušebního uživatele Symantec webové zabezpečení služby (WSS)](#creating-a-symantec-web-security-service-wss-test-user)**  – Pokud chcete mít protějšek Britta Simon v zabezpečení webové Symantec služby (WSS) propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="edd90-145">**[Creating a Symantec Web Security Service (WSS) test user](#creating-a-symantec-web-security-service-wss-test-user)** - to have a counterpart of Britta Simon in Symantec Web Security Service (WSS) that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="edd90-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="edd90-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="edd90-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="edd90-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="edd90-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="edd90-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="edd90-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Symantec webové zabezpečení služby (WSS).</span><span class="sxs-lookup"><span data-stu-id="edd90-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Symantec Web Security Service (WSS) application.</span></span>

<span data-ttu-id="edd90-150">**Pokud chcete konfigurovat Azure AD jednotné přihlašování s Symantec webové zabezpečení služby (WSS), proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="edd90-150">**To configure Azure AD single sign-on with Symantec Web Security Service (WSS), perform the following steps:**</span></span>

1. <span data-ttu-id="edd90-151">Na portálu Azure na **Symantec webové zabezpečení služby (WSS)** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="edd90-151">In the Azure portal, on the **Symantec Web Security Service (WSS)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="edd90-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="edd90-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

3. <span data-ttu-id="edd90-155">Na **Symantec webové zabezpečení služby (WSS) domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="edd90-155">On the **Symantec Web Security Service (WSS) Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    <span data-ttu-id="edd90-157">a.</span><span class="sxs-lookup"><span data-stu-id="edd90-157">a.</span></span> <span data-ttu-id="edd90-158">V **přihlašovací adresa URL** textovému poli, zmínili adresu url, které chcete blokovat podle zásady pro blokování služby pro zabezpečení webové Symantec (WSS).</span><span class="sxs-lookup"><span data-stu-id="edd90-158">In the **Sign-on URL** textbox, mention the url, which you want to block according to the blocking policy of the Symantec Web Security Service (WSS).</span></span>  
    
    <span data-ttu-id="edd90-159">b.</span><span class="sxs-lookup"><span data-stu-id="edd90-159">b.</span></span> <span data-ttu-id="edd90-160">V **identifikátor** textovému poli, zadejte adresu URL:`https://saml.threatpulse.net:8443/saml/saml_realm`</span><span class="sxs-lookup"><span data-stu-id="edd90-160">In the **Identifier** textbox, type the URL: `https://saml.threatpulse.net:8443/saml/saml_realm`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="edd90-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="edd90-161">These values are not real.</span></span> <span data-ttu-id="edd90-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="edd90-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="edd90-163">Obraťte se na [tým podpory Symantec webové zabezpečení služby (WSS) klienta](https://www.symantec.com/contact-us) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="edd90-163">Contact [Symantec Web Security Service (WSS) Client support team](https://www.symantec.com/contact-us) to get these values.</span></span> 

4. <span data-ttu-id="edd90-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="edd90-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

5. <span data-ttu-id="edd90-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="edd90-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="edd90-168">Konfigurace jednotného přihlašování na **Symantec webové zabezpečení služby (WSS)** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Symantec webové zabezpečení služby (WSS)](https://www.symantec.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="edd90-168">To configure single sign-on on **Symantec Web Security Service (WSS)** side, you need to send the downloaded **Metadata XML** to [Symantec Web Security Service (WSS) support team](https://www.symantec.com/contact-us).</span></span>

> [!TIP]
> <span data-ttu-id="edd90-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="edd90-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="edd90-170">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="edd90-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="edd90-171">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="edd90-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="edd90-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="edd90-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="edd90-173">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="edd90-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="edd90-175">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="edd90-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="edd90-176">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="edd90-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="edd90-178">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="edd90-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="edd90-180">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="edd90-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="edd90-182">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="edd90-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="edd90-184">a.</span><span class="sxs-lookup"><span data-stu-id="edd90-184">a.</span></span> <span data-ttu-id="edd90-185">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="edd90-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="edd90-186">b.</span><span class="sxs-lookup"><span data-stu-id="edd90-186">b.</span></span> <span data-ttu-id="edd90-187">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="edd90-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="edd90-188">c.</span><span class="sxs-lookup"><span data-stu-id="edd90-188">c.</span></span> <span data-ttu-id="edd90-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="edd90-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="edd90-190">d.</span><span class="sxs-lookup"><span data-stu-id="edd90-190">d.</span></span> <span data-ttu-id="edd90-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="edd90-191">Click **Create**.</span></span>
 
### <a name="creating-a-symantec-web-security-service-wss-test-user"></a><span data-ttu-id="edd90-192">Vytvoření zkušebního uživatele Symantec webové zabezpečení služby (WSS)</span><span class="sxs-lookup"><span data-stu-id="edd90-192">Creating a Symantec Web Security Service (WSS) test user</span></span>

<span data-ttu-id="edd90-193">V této části vytvoříte uživatele volat Britta Simon v Symantec webové zabezpečení služby (WSS).</span><span class="sxs-lookup"><span data-stu-id="edd90-193">In this section, you create a user called Britta Simon in Symantec Web Security Service (WSS).</span></span> <span data-ttu-id="edd90-194">Práce s [tým podpory Symantec webové zabezpečení služby (WSS)](https://www.symantec.com/contact-us) přidat uživatele do platformy Symantec webové zabezpečení služby (WSS).</span><span class="sxs-lookup"><span data-stu-id="edd90-194">Work with [Symantec Web Security Service (WSS) support team](https://www.symantec.com/contact-us) to add the users in the Symantec Web Security Service (WSS) platform.</span></span> <span data-ttu-id="edd90-195">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="edd90-195">Users must be created and activated before you use single sign-on.</span></span> <span data-ttu-id="edd90-196">Také společně s uživatelskými informacemi, které budete muset odeslat veřejná IP adresa počítače, ze kterého se pokoušíte získat přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="edd90-196">Also along with the user details you need to send the public IPaddress of the machine from which you are trying to access the application.</span></span>

> [!NOTE]
> <span data-ttu-id="edd90-197">Prosím [kliknutím sem](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) získat váš počítač je veřejná IP adresa.</span><span class="sxs-lookup"><span data-stu-id="edd90-197">Please [click here](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) to get your machine's public IPaddress.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="edd90-198">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="edd90-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="edd90-199">V této části povolíte Britta Simon chcete použít Azure jednotného přihlašování k udělení přístupu k Symantec webové zabezpečení služby (WSS).</span><span class="sxs-lookup"><span data-stu-id="edd90-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Symantec Web Security Service (WSS).</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="edd90-201">**Pokud chcete přiřadit Britta Simon Symantec webové zabezpečení služby (WSS), proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="edd90-201">**To assign Britta Simon to Symantec Web Security Service (WSS), perform the following steps:**</span></span>

1. <span data-ttu-id="edd90-202">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="edd90-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="edd90-204">V seznamu aplikací vyberte **Symantec webové zabezpečení služby (WSS)**.</span><span class="sxs-lookup"><span data-stu-id="edd90-204">In the applications list, select **Symantec Web Security Service (WSS)**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_app.png) 

3. <span data-ttu-id="edd90-206">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="edd90-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="edd90-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="edd90-208">Click **Add** button.</span></span> <span data-ttu-id="edd90-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="edd90-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="edd90-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="edd90-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="edd90-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="edd90-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="edd90-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="edd90-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="edd90-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="edd90-214">Testing single sign-on</span></span>

<span data-ttu-id="edd90-215">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="edd90-215">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="edd90-216">Když kliknete na dlaždici Symantec webové zabezpečení služby (WSS) na přístupovém panelu, budete přesměrováni na stránku blokování, pro který je nakonfigurovaný zásady pro blokování.</span><span class="sxs-lookup"><span data-stu-id="edd90-216">When you click the Symantec Web Security Service (WSS) tile in the Access Panel, you get redirected to the blocking page for which the blocking policy has been configured.</span></span>
<span data-ttu-id="edd90-217">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="edd90-217">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="edd90-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="edd90-218">Additional resources</span></span>

* [<span data-ttu-id="edd90-219">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="edd90-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="edd90-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="edd90-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_203.png

