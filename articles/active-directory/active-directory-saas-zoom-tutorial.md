---
title: "Kurz: Azure Active Directory integrace s přiblížení | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a přiblížení."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0ebdab6c-83a8-4737-a86a-974f37269c31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: aab491f162fd4d24c6ff4d8858f2edd96dda30d4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zoom"></a><span data-ttu-id="8b4ca-103">Kurz: Azure Active Directory integrace s přiblížení</span><span class="sxs-lookup"><span data-stu-id="8b4ca-103">Tutorial: Azure Active Directory integration with Zoom</span></span>

<span data-ttu-id="8b4ca-104">V tomto kurzu zjistěte, jak integrovat přiblížení s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8b4ca-104">In this tutorial, you learn how to integrate Zoom with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8b4ca-105">Integrace přiblížení s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8b4ca-105">Integrating Zoom with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8b4ca-106">Můžete ovládat ve službě Azure AD, který má přístup k přiblížení.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-106">You can control in Azure AD who has access to Zoom.</span></span>
- <span data-ttu-id="8b4ca-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k zvětšení (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-107">You can enable your users to automatically get signed-on to Zoom (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="8b4ca-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="8b4ca-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8b4ca-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8b4ca-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8b4ca-110">Prerequisites</span></span>

<span data-ttu-id="8b4ca-111">Konfigurace integrace Azure AD s přiblížení, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="8b4ca-111">To configure Azure AD integration with Zoom, you need the following items:</span></span>

- <span data-ttu-id="8b4ca-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8b4ca-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8b4ca-113">Přiblížení jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="8b4ca-113">A Zoom single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8b4ca-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8b4ca-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="8b4ca-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8b4ca-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8b4ca-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8b4ca-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8b4ca-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="8b4ca-118">Scenario description</span></span>
<span data-ttu-id="8b4ca-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8b4ca-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="8b4ca-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8b4ca-121">Přidání přiblížení z Galerie</span><span class="sxs-lookup"><span data-stu-id="8b4ca-121">Adding Zoom from the gallery</span></span>
2. <span data-ttu-id="8b4ca-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8b4ca-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zoom-from-the-gallery"></a><span data-ttu-id="8b4ca-123">Přidání přiblížení z Galerie</span><span class="sxs-lookup"><span data-stu-id="8b4ca-123">Adding Zoom from the gallery</span></span>
<span data-ttu-id="8b4ca-124">Při konfiguraci integrace přiblížení či oddálení do služby Azure AD, potřebujete přidat přiblížení z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-124">To configure the integration of Zoom into Azure AD, you need to add Zoom from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8b4ca-125">**Pokud chcete přidat přiblížení z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8b4ca-125">**To add Zoom from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8b4ca-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="8b4ca-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8b4ca-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="8b4ca-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="8b4ca-133">Do vyhledávacího pole zadejte **zvětšení**, vyberte **zvětšení** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-133">In the search box, type **Zoom**, select **Zoom** from result panel then click **Add** button to add the application.</span></span>

    ![Zvětšení v seznamu výsledků](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="8b4ca-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8b4ca-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="8b4ca-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s přiblížení podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="8b4ca-136">In this section, you configure and test Azure AD single sign-on with Zoom based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8b4ca-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v přiblížení je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Zoom is to a user in Azure AD.</span></span> <span data-ttu-id="8b4ca-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v přiblížení musí navázat.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-138">In other words, a link relationship between an Azure AD user and the related user in Zoom needs to be established.</span></span>

<span data-ttu-id="8b4ca-139">V přiblížení, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-139">In Zoom, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8b4ca-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s přiblížení, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="8b4ca-140">To configure and test Azure AD single sign-on with Zoom, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8b4ca-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8b4ca-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8b4ca-143">**[Vytvoření zkušebního uživatele přiblížení](#create-a-zoom-test-user)**  – Pokud chcete mít protějšek Britta Simon v přiblížení, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-143">**[Create a Zoom test user](#create-a-zoom-test-user)** - to have a counterpart of Britta Simon in Zoom that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8b4ca-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8b4ca-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="8b4ca-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8b4ca-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="8b4ca-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci přiblížení.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Zoom application.</span></span>

<span data-ttu-id="8b4ca-148">**Ke konfiguraci Azure AD jednotné přihlašování s přiblížení, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8b4ca-148">**To configure Azure AD single sign-on with Zoom, perform the following steps:**</span></span>

1. <span data-ttu-id="8b4ca-149">Na portálu Azure na **zvětšení** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-149">In the Azure portal, on the **Zoom** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="8b4ca-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_samlbase.png)

3. <span data-ttu-id="8b4ca-153">Na **zvětšení domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8b4ca-153">On the **Zoom Domain and URLs** section, perform the following steps:</span></span>

    ![Přiblížení domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_url.png)

    <span data-ttu-id="8b4ca-155">a.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-155">a.</span></span> <span data-ttu-id="8b4ca-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.zoom.us`</span><span class="sxs-lookup"><span data-stu-id="8b4ca-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.zoom.us`</span></span>

    <span data-ttu-id="8b4ca-157">b.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-157">b.</span></span> <span data-ttu-id="8b4ca-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.zoom.us`</span><span class="sxs-lookup"><span data-stu-id="8b4ca-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.zoom.us`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8b4ca-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-159">These values are not real.</span></span> <span data-ttu-id="8b4ca-160">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="8b4ca-161">Obraťte se na [tým podpory zvětšení klienta](https://support.zoom.us/hc) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-161">Contact [Zoom Client support team](https://support.zoom.us/hc) to get these values.</span></span> 
 
4. <span data-ttu-id="8b4ca-162">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-162">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_certificate.png) 

5. <span data-ttu-id="8b4ca-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-zoom-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8b4ca-166">Na **zvětšení konfigurace** klikněte na tlačítko **konfigurace zvětšení** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-166">On the **Zoom Configuration** section, click **Configure Zoom** to open **Configure sign-on** window.</span></span> <span data-ttu-id="8b4ca-167">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="8b4ca-167">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace přiblížení](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_configure.png) 

7. <span data-ttu-id="8b4ca-169">V okně prohlížeče jiný web Přihlaste se na váš web společnosti přiblížení jako správce.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-169">In a different web browser window, log in to your Zoom company site as an administrator.</span></span>

8. <span data-ttu-id="8b4ca-170">Klikněte **jednotné přihlašování** kartě.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-170">Click the **Single Sign-On** tab.</span></span>
   
    <span data-ttu-id="8b4ca-171">![Karta přihlašování](./media/active-directory-saas-zoom-tutorial/IC784700.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="8b4ca-171">![Single sign-on tab](./media/active-directory-saas-zoom-tutorial/IC784700.png "Single sign-on")</span></span>

9. <span data-ttu-id="8b4ca-172">Klikněte na tlačítko **řízení zabezpečení** kartě a potom přejděte na **jednotné přihlašování** nastavení.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-172">Click the **Security Control** tab, and then go to the **Single Sign-On** settings.</span></span>

10. <span data-ttu-id="8b4ca-173">V části jednotné přihlašování proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8b4ca-173">In the Single Sign-On section, perform the following steps:</span></span>
   
    <span data-ttu-id="8b4ca-174">![Jednotné přihlašování v části](./media/active-directory-saas-zoom-tutorial/IC784701.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="8b4ca-174">![Single sign-on section](./media/active-directory-saas-zoom-tutorial/IC784701.png "Single sign-on")</span></span>
   
    <span data-ttu-id="8b4ca-175">a.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-175">a.</span></span> <span data-ttu-id="8b4ca-176">V **přihlašovací adresa URL stránky** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-176">In the **Sign-in page URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="8b4ca-177">b.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-177">b.</span></span> <span data-ttu-id="8b4ca-178">V **adresy URL odhlašovací stránky** textovému poli, vložte hodnotu **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-178">In the **Sign-out page URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>
     
    <span data-ttu-id="8b4ca-179">c.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-179">c.</span></span> <span data-ttu-id="8b4ca-180">V poznámkovém bloku otevřete váš kódování base-64 kódovaného certifikátu, zkopírujte obsah ho do schránky a vložte jej do **certifikát zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-180">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **Identity provider certificate** textbox.</span></span>

    <span data-ttu-id="8b4ca-181">d.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-181">d.</span></span> <span data-ttu-id="8b4ca-182">V **vystavitele** textovému poli, vložte hodnotu **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-182">In the **Issuer** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="8b4ca-183">e.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-183">e.</span></span> <span data-ttu-id="8b4ca-184">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-184">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="8b4ca-185">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="8b4ca-185">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8b4ca-186">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-186">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8b4ca-187">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8b4ca-187">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="8b4ca-188">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8b4ca-188">Create an Azure AD test user</span></span>

<span data-ttu-id="8b4ca-189">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-189">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="8b4ca-191">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8b4ca-191">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8b4ca-192">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-192">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-zoom-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="8b4ca-194">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-194">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-zoom-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="8b4ca-196">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-196">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-zoom-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="8b4ca-198">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8b4ca-198">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-zoom-tutorial/create_aaduser_04.png)

    <span data-ttu-id="8b4ca-200">a.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-200">a.</span></span> <span data-ttu-id="8b4ca-201">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-201">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8b4ca-202">b.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-202">b.</span></span> <span data-ttu-id="8b4ca-203">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-203">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="8b4ca-204">c.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-204">c.</span></span> <span data-ttu-id="8b4ca-205">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-205">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="8b4ca-206">d.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-206">d.</span></span> <span data-ttu-id="8b4ca-207">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-207">Click **Create**.</span></span>
 
### <a name="create-a-zoom-test-user"></a><span data-ttu-id="8b4ca-208">Vytvoření zkušebního uživatele přiblížení</span><span class="sxs-lookup"><span data-stu-id="8b4ca-208">Create a Zoom test user</span></span>

<span data-ttu-id="8b4ca-209">Pokud chcete povolit uživatelům Azure AD přihlášení pro přiblížení, musí být zřízená do přiblížení.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-209">In order to enable Azure AD users to log in to Zoom, they must be provisioned into Zoom.</span></span> <span data-ttu-id="8b4ca-210">V případě přiblížení zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-210">In the case of Zoom, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-account-perform-the-following-steps"></a><span data-ttu-id="8b4ca-211">K poskytnutí uživatelského účtu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8b4ca-211">To provision a user account, perform the following steps:</span></span>

1. <span data-ttu-id="8b4ca-212">Přihlaste se k vaší **zvětšení** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-212">Log in to your **Zoom** company site as an administrator.</span></span>
 
2. <span data-ttu-id="8b4ca-213">Klikněte **správy účtů** a pak klikněte **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-213">Click the **Account Management** tab, and then click **User Management**.</span></span>

3. <span data-ttu-id="8b4ca-214">V části Správa uživatelů, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-214">In the User Management section, click **Add users**.</span></span>
   
    <span data-ttu-id="8b4ca-215">![Správa uživatelů](./media/active-directory-saas-zoom-tutorial/IC784703.png "Správa uživatelů")</span><span class="sxs-lookup"><span data-stu-id="8b4ca-215">![User management](./media/active-directory-saas-zoom-tutorial/IC784703.png "User management")</span></span>

4. <span data-ttu-id="8b4ca-216">Na **přidat uživatele** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8b4ca-216">On the **Add users** page, perform the following steps:</span></span>
   
    <span data-ttu-id="8b4ca-217">![Přidání uživatelů](./media/active-directory-saas-zoom-tutorial/IC784704.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="8b4ca-217">![Add users](./media/active-directory-saas-zoom-tutorial/IC784704.png "Add users")</span></span>
   
    <span data-ttu-id="8b4ca-218">a.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-218">a.</span></span> <span data-ttu-id="8b4ca-219">Jako **typ uživatele**, vyberte **základní**.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-219">As **User Type**, select **Basic**.</span></span>

    <span data-ttu-id="8b4ca-220">b.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-220">b.</span></span> <span data-ttu-id="8b4ca-221">V **e-mailů** textové pole, zadejte e-mailovou adresu, platný Azure AD účet určené ke zřízení.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-221">In the **Emails** textbox, type the email address of a valid Azure AD account you want to provision.</span></span>

    <span data-ttu-id="8b4ca-222">c.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-222">c.</span></span> <span data-ttu-id="8b4ca-223">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-223">Click **Add**.</span></span>

> [!NOTE]
> <span data-ttu-id="8b4ca-224">Můžete použít jakékoli jiné přiblížení uživatel účet vytvoření nástroje nebo rozhraní API poskytované přiblížení zřídit služby Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-224">You can use any other Zoom user account creation tools or APIs provided by Zoom to provision Azure Active Directory user accounts.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="8b4ca-225">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8b4ca-225">Assign the Azure AD test user</span></span>

<span data-ttu-id="8b4ca-226">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu přiblížení.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-226">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Zoom.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="8b4ca-228">**Pokud chcete přiřadit Britta Simon přiblížení, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8b4ca-228">**To assign Britta Simon to Zoom, perform the following steps:**</span></span>

1. <span data-ttu-id="8b4ca-229">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-229">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="8b4ca-231">V seznamu aplikací vyberte **zvětšení**.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-231">In the applications list, select **Zoom**.</span></span>

    ![V seznamu aplikací na odkaz přiblížení](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_app.png)  

3. <span data-ttu-id="8b4ca-233">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="8b4ca-235">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-235">Click **Add** button.</span></span> <span data-ttu-id="8b4ca-236">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="8b4ca-238">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8b4ca-239">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8b4ca-240">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="8b4ca-241">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8b4ca-241">Test single sign-on</span></span>

<span data-ttu-id="8b4ca-242">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-242">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="8b4ca-243">Když kliknete na dlaždici přiblížení na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci přiblížení.</span><span class="sxs-lookup"><span data-stu-id="8b4ca-243">When you click the Zoom tile in the Access Panel, you should get automatically signed-on to your Zoom application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8b4ca-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8b4ca-244">Additional resources</span></span>

* [<span data-ttu-id="8b4ca-245">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8b4ca-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8b4ca-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8b4ca-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_203.png

