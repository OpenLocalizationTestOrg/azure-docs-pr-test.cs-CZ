---
title: 'Kurz: Azure Active Directory integrace s Panopto | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Panopto."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 89c88e23-93ce-4970-9baa-1104c4e8fe4a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 725fba1227cfc9c4850f9e2d6fd0b13e88eafa20
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-panopto"></a><span data-ttu-id="6afd0-103">Kurz: Azure Active Directory integrace s Panopto</span><span class="sxs-lookup"><span data-stu-id="6afd0-103">Tutorial: Azure Active Directory integration with Panopto</span></span>

<span data-ttu-id="6afd0-104">V tomto kurzu zjistěte, jak integrovat Panopto s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6afd0-104">In this tutorial, you learn how to integrate Panopto with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6afd0-105">Integrace Panopto s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="6afd0-105">Integrating Panopto with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="6afd0-106">Můžete řídit ve službě Azure AD, který má přístup k Panopto</span><span class="sxs-lookup"><span data-stu-id="6afd0-106">You can control in Azure AD who has access to Panopto</span></span>
- <span data-ttu-id="6afd0-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Panopto (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="6afd0-107">You can enable your users to automatically get signed-on to Panopto (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6afd0-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6afd0-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="6afd0-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6afd0-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6afd0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6afd0-110">Prerequisites</span></span>

<span data-ttu-id="6afd0-111">Konfigurace integrace Azure AD s Panopto, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="6afd0-111">To configure Azure AD integration with Panopto, you need the following items:</span></span>

- <span data-ttu-id="6afd0-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="6afd0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6afd0-113">Panopto jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="6afd0-113">A Panopto single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6afd0-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6afd0-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6afd0-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="6afd0-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6afd0-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="6afd0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6afd0-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6afd0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6afd0-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="6afd0-118">Scenario description</span></span>
<span data-ttu-id="6afd0-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="6afd0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6afd0-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="6afd0-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6afd0-121">Přidání Panopto z Galerie</span><span class="sxs-lookup"><span data-stu-id="6afd0-121">Adding Panopto from the gallery</span></span>
2. <span data-ttu-id="6afd0-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6afd0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-panopto-from-the-gallery"></a><span data-ttu-id="6afd0-123">Přidání Panopto z Galerie</span><span class="sxs-lookup"><span data-stu-id="6afd0-123">Adding Panopto from the gallery</span></span>
<span data-ttu-id="6afd0-124">Při konfiguraci integrace Panopto do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Panopto z galerie.</span><span class="sxs-lookup"><span data-stu-id="6afd0-124">To configure the integration of Panopto into Azure AD, you need to add Panopto from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6afd0-125">**Pokud chcete přidat Panopto z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6afd0-125">**To add Panopto from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="6afd0-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6afd0-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6afd0-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="6afd0-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="6afd0-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6afd0-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="6afd0-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6afd0-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="6afd0-133">Do vyhledávacího pole zadejte **Panopto**.</span><span class="sxs-lookup"><span data-stu-id="6afd0-133">In the search box, type **Panopto**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_search.png)

5. <span data-ttu-id="6afd0-135">Na panelu výsledků vyberte **Panopto**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6afd0-135">In the results panel, select **Panopto**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6afd0-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6afd0-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="6afd0-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Panopto podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="6afd0-138">In this section, you configure and test Azure AD single sign-on with Panopto based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6afd0-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Panopto je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6afd0-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Panopto is to a user in Azure AD.</span></span> <span data-ttu-id="6afd0-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Panopto musí navázat.</span><span class="sxs-lookup"><span data-stu-id="6afd0-140">In other words, a link relationship between an Azure AD user and the related user in Panopto needs to be established.</span></span>

<span data-ttu-id="6afd0-141">V Panopto, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="6afd0-141">In Panopto, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="6afd0-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Panopto, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="6afd0-142">To configure and test Azure AD single sign-on with Panopto, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="6afd0-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="6afd0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="6afd0-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6afd0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6afd0-145">**[Vytvoření zkušebního uživatele Panopto](#creating-a-panopto-test-user)**  – Pokud chcete mít protějšek Britta Simon v Panopto propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="6afd0-145">**[Creating a Panopto test user](#creating-a-panopto-test-user)** - to have a counterpart of Britta Simon in Panopto that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="6afd0-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6afd0-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6afd0-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6afd0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6afd0-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6afd0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6afd0-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Panopto.</span><span class="sxs-lookup"><span data-stu-id="6afd0-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Panopto application.</span></span>

<span data-ttu-id="6afd0-150">**Ke konfiguraci Azure AD jednotné přihlašování s Panopto, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6afd0-150">**To configure Azure AD single sign-on with Panopto, perform the following steps:**</span></span>

1. <span data-ttu-id="6afd0-151">Na portálu Azure na **Panopto** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6afd0-151">In the Azure portal, on the **Panopto** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="6afd0-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6afd0-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_samlbase.png)

3. <span data-ttu-id="6afd0-155">Na **Panopto domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6afd0-155">On the **Panopto Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_url.png)

    <span data-ttu-id="6afd0-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenant-name>.panopto.com`</span><span class="sxs-lookup"><span data-stu-id="6afd0-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.panopto.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6afd0-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="6afd0-158">This value is not real.</span></span> <span data-ttu-id="6afd0-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6afd0-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="6afd0-160">Obraťte se na [tým podpory Panopto klienta](mailto:support@panopto.com‎) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6afd0-160">Contact [Panopto Client support team](mailto:support@panopto.com‎) to get this value.</span></span> 
 
4. <span data-ttu-id="6afd0-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="6afd0-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_certificate.png) 

5. <span data-ttu-id="6afd0-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6afd0-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panopto-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6afd0-165">Na **Panopto konfigurace** klikněte na tlačítko **konfigurace Panopto** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="6afd0-165">On the **Panopto Configuration** section, click **Configure Panopto** to open **Configure sign-on** window.</span></span> <span data-ttu-id="6afd0-166">Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="6afd0-166">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_configure.png) 

7. <span data-ttu-id="6afd0-168">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti Panopto jako správce.</span><span class="sxs-lookup"><span data-stu-id="6afd0-168">In a different web browser window, log in to your Panopto company site as an administrator.</span></span>

8. <span data-ttu-id="6afd0-169">Na panelu nástrojů na levé straně klikněte na tlačítko **systému**a potom klikněte na **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="6afd0-169">In the toolbar on the left, click **System**, and then click **Identity Providers**.</span></span>
   
   <span data-ttu-id="6afd0-170">![Systém](./media/active-directory-saas-panopto-tutorial/ic777670.png "systému")</span><span class="sxs-lookup"><span data-stu-id="6afd0-170">![System](./media/active-directory-saas-panopto-tutorial/ic777670.png "System")</span></span>
9. <span data-ttu-id="6afd0-171">Klikněte na tlačítko **přidejte zprostředkovatele**.</span><span class="sxs-lookup"><span data-stu-id="6afd0-171">Click **Add Provider**.</span></span>
   
   <span data-ttu-id="6afd0-172">![Zprostředkovatelé identity](./media/active-directory-saas-panopto-tutorial/ic777671.png "poskytovatelů identit")</span><span class="sxs-lookup"><span data-stu-id="6afd0-172">![Identity Providers](./media/active-directory-saas-panopto-tutorial/ic777671.png "Identity Providers")</span></span>
   
10. <span data-ttu-id="6afd0-173">V části zprostředkovatele SAML proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6afd0-173">In the SAML provider section, perform the following steps:</span></span>
   
    <span data-ttu-id="6afd0-174">![Konfigurace SaaS](./media/active-directory-saas-panopto-tutorial/ic777672.png "konfigurace SaaS")</span><span class="sxs-lookup"><span data-stu-id="6afd0-174">![SaaS configuration](./media/active-directory-saas-panopto-tutorial/ic777672.png "SaaS configuration")</span></span>
    
    <span data-ttu-id="6afd0-175">a.</span><span class="sxs-lookup"><span data-stu-id="6afd0-175">a.</span></span> <span data-ttu-id="6afd0-176">Z **typ zprostředkovatele** seznamu, vyberte **SAML20**.</span><span class="sxs-lookup"><span data-stu-id="6afd0-176">From the **Provider Type** list, select **SAML20**.</span></span>    
    
    <span data-ttu-id="6afd0-177">b.</span><span class="sxs-lookup"><span data-stu-id="6afd0-177">b.</span></span> <span data-ttu-id="6afd0-178">V **název Instance** textovému poli, zadejte název pro instanci.</span><span class="sxs-lookup"><span data-stu-id="6afd0-178">In the **Instance Name** textbox, type a name for the instance.</span></span>

    <span data-ttu-id="6afd0-179">c.</span><span class="sxs-lookup"><span data-stu-id="6afd0-179">c.</span></span> <span data-ttu-id="6afd0-180">V **stručný popis** textovému poli, zadejte popisný název.</span><span class="sxs-lookup"><span data-stu-id="6afd0-180">In the **Friendly Description** textbox, type a friendly description.</span></span>
    
    <span data-ttu-id="6afd0-181">d.</span><span class="sxs-lookup"><span data-stu-id="6afd0-181">d.</span></span> <span data-ttu-id="6afd0-182">V **kolísají adresa Url stránky** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6afd0-182">In **Bounce Page Url** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="6afd0-183">e.</span><span class="sxs-lookup"><span data-stu-id="6afd0-183">e.</span></span> <span data-ttu-id="6afd0-184">V **vystavitele** textovému poli, vložte hodnotu **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6afd0-184">In the **Issuer** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="6afd0-185">f.</span><span class="sxs-lookup"><span data-stu-id="6afd0-185">f.</span></span> <span data-ttu-id="6afd0-186">Otevření kódování base-64 kódovaného certifikátu, který jste si stáhli z portálu Azure, obsah ho zkopírujte do schránky a vložte jej do **veřejný klíč** textové pole.</span><span class="sxs-lookup"><span data-stu-id="6afd0-186">Open your base-64 encoded certificate, which you have downloaded from Azure portal, copy the content of it in to your clipboard, and then paste it to the **Public Key**  textbox.</span></span>

11. <span data-ttu-id="6afd0-187">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6afd0-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="6afd0-188">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="6afd0-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="6afd0-189">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="6afd0-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="6afd0-190">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6afd0-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6afd0-191">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6afd0-191">Creating an Azure AD test user</span></span>

<span data-ttu-id="6afd0-192">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6afd0-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="6afd0-194">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6afd0-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="6afd0-195">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6afd0-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panopto-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6afd0-197">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="6afd0-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panopto-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6afd0-199">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6afd0-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panopto-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6afd0-201">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6afd0-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panopto-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6afd0-203">a.</span><span class="sxs-lookup"><span data-stu-id="6afd0-203">a.</span></span> <span data-ttu-id="6afd0-204">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6afd0-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6afd0-205">b.</span><span class="sxs-lookup"><span data-stu-id="6afd0-205">b.</span></span> <span data-ttu-id="6afd0-206">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6afd0-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6afd0-207">c.</span><span class="sxs-lookup"><span data-stu-id="6afd0-207">c.</span></span> <span data-ttu-id="6afd0-208">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="6afd0-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="6afd0-209">d.</span><span class="sxs-lookup"><span data-stu-id="6afd0-209">d.</span></span> <span data-ttu-id="6afd0-210">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6afd0-210">Click **Create**.</span></span>
 
### <a name="creating-a-panopto-test-user"></a><span data-ttu-id="6afd0-211">Vytvoření zkušebního uživatele Panopto</span><span class="sxs-lookup"><span data-stu-id="6afd0-211">Creating a Panopto test user</span></span>

<span data-ttu-id="6afd0-212">Neexistuje žádná položka akce můžete nakonfigurovat na Panopto zřizování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6afd0-212">There is no action item for you to configure user provisioning to Panopto.</span></span>  
<span data-ttu-id="6afd0-213">Když přiřazený uživatel se pokusí přihlásit k Panopto pomocí přístupového panelu, Panopto ověří, zda uživatel existuje.</span><span class="sxs-lookup"><span data-stu-id="6afd0-213">When an assigned user tries to log in to Panopto using the access panel, Panopto checks whether the user exists.</span></span>  

<span data-ttu-id="6afd0-214">Pokud neexistuje žádný účet k dispozici dosud, je vytvářena automaticky nástrojem Panopto.</span><span class="sxs-lookup"><span data-stu-id="6afd0-214">If there is no user account available yet, it is automatically created by Panopto.</span></span>

>[!NOTE]
><span data-ttu-id="6afd0-215">Můžete použít všechny ostatní Panopto uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Panopto ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6afd0-215">You can use any other Panopto user account creation tools or APIs provided by Panopto to provision Azure AD user accounts.</span></span>
>
>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="6afd0-216">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6afd0-216">Assigning the Azure AD test user</span></span>

<span data-ttu-id="6afd0-217">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Panopto.</span><span class="sxs-lookup"><span data-stu-id="6afd0-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Panopto.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="6afd0-219">**Pokud chcete přiřadit Britta Simon Panopto, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6afd0-219">**To assign Britta Simon to Panopto, perform the following steps:**</span></span>

1. <span data-ttu-id="6afd0-220">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6afd0-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="6afd0-222">V seznamu aplikací vyberte **Panopto**.</span><span class="sxs-lookup"><span data-stu-id="6afd0-222">In the applications list, select **Panopto**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_app.png) 

3. <span data-ttu-id="6afd0-224">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="6afd0-224">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="6afd0-226">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6afd0-226">Click **Add** button.</span></span> <span data-ttu-id="6afd0-227">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6afd0-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="6afd0-229">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6afd0-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="6afd0-230">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6afd0-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6afd0-231">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6afd0-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6afd0-232">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6afd0-232">Testing single sign-on</span></span>

<span data-ttu-id="6afd0-233">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="6afd0-233">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="6afd0-234">Když kliknete na dlaždici Panopto na přístupovém panelu, měli byste obdržet automaticky přihlašovací stránku Panopto aplikace.</span><span class="sxs-lookup"><span data-stu-id="6afd0-234">When you click the Panopto tile in the Access Panel, you should get automatically login page of Panopto application.</span></span>
<span data-ttu-id="6afd0-235">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6afd0-235">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6afd0-236">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6afd0-236">Additional resources</span></span>

* [<span data-ttu-id="6afd0-237">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6afd0-237">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6afd0-238">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6afd0-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_203.png

