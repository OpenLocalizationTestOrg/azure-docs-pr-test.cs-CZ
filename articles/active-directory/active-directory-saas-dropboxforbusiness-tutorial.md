---
title: 'Kurz: Azure Active Directory integrace s Dropbox pro firmy | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Dropbox pro firmy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: a56a5af171eaca259db29f25fee4331a77313420
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a><span data-ttu-id="dfbbe-103">Kurz: Azure Active Directory integrace s Dropbox pro firmy</span><span class="sxs-lookup"><span data-stu-id="dfbbe-103">Tutorial: Azure Active Directory integration with Dropbox for Business</span></span>

<span data-ttu-id="dfbbe-104">V tomto kurzu zjistěte, jak integrovat Dropbox pro firmy s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="dfbbe-104">In this tutorial, you learn how to integrate Dropbox for Business with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dfbbe-105">Integrace Dropbox pro firmy s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="dfbbe-105">Integrating Dropbox for Business with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="dfbbe-106">Můžete řídit ve službě Azure AD, který má přístup k Dropboxu pro firmy</span><span class="sxs-lookup"><span data-stu-id="dfbbe-106">You can control in Azure AD who has access to Dropbox for Business</span></span>
- <span data-ttu-id="dfbbe-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Dropboxu pro firmy (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="dfbbe-107">You can enable your users to automatically get signed-on to Dropbox for Business (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="dfbbe-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="dfbbe-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="dfbbe-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dfbbe-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dfbbe-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dfbbe-110">Prerequisites</span></span>

<span data-ttu-id="dfbbe-111">Konfigurace integrace Azure AD s Dropbox pro firmy, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="dfbbe-111">To configure Azure AD integration with Dropbox for Business, you need the following items:</span></span>

- <span data-ttu-id="dfbbe-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="dfbbe-112">An Azure AD subscription</span></span>
- <span data-ttu-id="dfbbe-113">Dropbox pro obchodní jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="dfbbe-113">A Dropbox for Business single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dfbbe-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="dfbbe-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="dfbbe-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="dfbbe-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dfbbe-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dfbbe-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dfbbe-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="dfbbe-118">Scenario description</span></span>
<span data-ttu-id="dfbbe-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="dfbbe-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="dfbbe-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dfbbe-121">Přidání Dropbox pro firmy z Galerie</span><span class="sxs-lookup"><span data-stu-id="dfbbe-121">Adding Dropbox for Business from the gallery</span></span>
2. <span data-ttu-id="dfbbe-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="dfbbe-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-dropbox-for-business-from-the-gallery"></a><span data-ttu-id="dfbbe-123">Přidání Dropbox pro firmy z Galerie</span><span class="sxs-lookup"><span data-stu-id="dfbbe-123">Adding Dropbox for Business from the gallery</span></span>
<span data-ttu-id="dfbbe-124">Pokud chcete nakonfigurovat integraci Dropbox pro firmy do služby Azure AD, potřebujete přidat Dropbox pro firmy z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-124">To configure the integration of Dropbox for Business into Azure AD, you need to add Dropbox for Business from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="dfbbe-125">**Pokud chcete přidat Dropbox pro firmy z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="dfbbe-125">**To add Dropbox for Business from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="dfbbe-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="dfbbe-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="dfbbe-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="dfbbe-131">Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-131">Click **New application** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="dfbbe-133">Do vyhledávacího pole zadejte **Dropbox pro firmy**.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-133">In the search box, type **Dropbox for Business**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_search.png)

5. <span data-ttu-id="dfbbe-135">Na panelu výsledků vyberte **Dropbox pro firmy**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-135">In the results panel, select **Dropbox for Business**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="dfbbe-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="dfbbe-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="dfbbe-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Dropbox pro firmy na základě testovací uživatele, nazývá "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="dfbbe-138">In this section, you configure and test Azure AD single sign-on with Dropbox for Business based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="dfbbe-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Dropbox pro firmy je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Dropbox for Business is to a user in Azure AD.</span></span> <span data-ttu-id="dfbbe-140">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v Dropbox pro firmy.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-140">In other words, a link relationship between an Azure AD user and the related user in Dropbox for Business needs to be established.</span></span>

<span data-ttu-id="dfbbe-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Dropbox pro firmy.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Dropbox for Business.</span></span>

<span data-ttu-id="dfbbe-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Dropbox pro firmy, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="dfbbe-142">To configure and test Azure AD single sign-on with Dropbox for Business, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="dfbbe-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="dfbbe-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dfbbe-145">**[Vytváření Dropbox pro obchodní testovacího uživatele](#creating-a-dropbox-for-business-test-user)**  – Pokud chcete mít protějšek Britta Simon v Dropbox pro podnikání, které je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-145">**[Creating a Dropbox for Business test user](#creating-a-dropbox-for-business-test-user)** - to have a counterpart of Britta Simon in Dropbox for Business that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="dfbbe-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dfbbe-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="dfbbe-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="dfbbe-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="dfbbe-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování do vaší schránky pro obchodní aplikace.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Dropbox for Business application.</span></span>

<span data-ttu-id="dfbbe-150">**Ke konfiguraci Azure AD jednotné přihlašování s Dropbox pro firmy, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="dfbbe-150">**To configure Azure AD single sign-on with Dropbox for Business, perform the following steps:**</span></span>

1. <span data-ttu-id="dfbbe-151">Na portálu Azure na **Dropbox pro firmy** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-151">In the Azure portal, on the **Dropbox for Business** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="dfbbe-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

3. <span data-ttu-id="dfbbe-155">Na **Dropbox obchodní domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="dfbbe-155">On the **Dropbox for Business Domain and URLs** section, perform the following steps:</span></span>

    <span data-ttu-id="dfbbe-156">a.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-156">a.</span></span> <span data-ttu-id="dfbbe-157">Přihlásit se do vaší schránky pro obchodní klienta.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-157">Sign on to your Dropbox for business tenant.</span></span> 
   
    <span data-ttu-id="dfbbe-158">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769509.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="dfbbe-158">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769509.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="dfbbe-159">b.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-159">b.</span></span> <span data-ttu-id="dfbbe-160">V navigačním podokně na levé straně klikněte na **konzoly pro správu**.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-160">In the navigation pane on the left side, click **Admin Console**.</span></span> 
   
    <span data-ttu-id="dfbbe-161">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769510.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="dfbbe-161">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769510.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="dfbbe-162">c.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-162">c.</span></span> <span data-ttu-id="dfbbe-163">Na **konzoly pro správu**, klikněte na tlačítko **ověřování** v levém navigačním podokně.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-163">On the **Admin Console**, click **Authentication** in the left navigation pane.</span></span> 
   
    <span data-ttu-id="dfbbe-164">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769511.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="dfbbe-164">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769511.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="dfbbe-165">d.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-165">d.</span></span> <span data-ttu-id="dfbbe-166">V **jednotného přihlašování** vyberte **povolit jednotné přihlašování**a potom klikněte na **Další** rozbalte v této části.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-166">In the **Single sign-on** section, select **Enable single sign-on**, and then click **More** to expand this section.</span></span>  
   
    <span data-ttu-id="dfbbe-167">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769512.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="dfbbe-167">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769512.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="dfbbe-168">e.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-168">e.</span></span> <span data-ttu-id="dfbbe-169">Zkopírujte adresu URL do **uživatelé se mohou přihlásit zadáním e-mailové adresy nebo můžete přejít přímo na**.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-169">Copy the URL next to **Users can sign in by entering their email address or they can go directly to**.</span></span> 
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769513.png)
    
    <span data-ttu-id="dfbbe-171">f.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-171">f.</span></span> <span data-ttu-id="dfbbe-172">Na portálu Azure v **přihlašovací adresa URL** textovému poli, vložte adresu URL.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-172">On the Azure portal, in the **Sign-on URL** textbox, paste the URL.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url.png)

     <span data-ttu-id="dfbbe-174">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://www.dropbox.com/sso/<id>`</span><span class="sxs-lookup"><span data-stu-id="dfbbe-174">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://www.dropbox.com/sso/<id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="dfbbe-175">Tato hodnota není skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-175">This value is not real value.</span></span> <span data-ttu-id="dfbbe-176">Aktualizujte hodnotu adresou URL skutečné přihlášení můžete získat z jejich části jednom přihlášení.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-176">Update the value with the actual Sign-on URL you get from their Single sign-on section.</span></span> <span data-ttu-id="dfbbe-177">Obraťte se na [Dropbox pro klienta obchodní tým podpory](https://www.dropbox.com/business/contact) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-177">Contact [Dropbox for Business Client support team](https://www.dropbox.com/business/contact) to get this value.</span></span> 
 
4. <span data-ttu-id="dfbbe-178">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-178">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

5. <span data-ttu-id="dfbbe-180">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-180">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="dfbbe-182">Na **Dropbox pro konfiguraci obchodní** klikněte na tlačítko **Dropbox nakonfigurovat pro firmy** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-182">On the **Dropbox for Business Configuration** section, click **Configure Dropbox for Business** to open **Configure sign-on** window.</span></span> <span data-ttu-id="dfbbe-183">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="dfbbe-183">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

7. <span data-ttu-id="dfbbe-185">Konfigurace jednotného přihlašování na **Dropbox pro firmy** straně, přejděte na vaší schránky pro obchodní klienta v **jednotného přihlašování** části **ověřování** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="dfbbe-185">To configure single sign-on on **Dropbox for Business** side, Go on your Dropbox for Business tenant, in the **Single sign-on** section of the **Authentication** page, perform the following steps:</span></span> 
   
    <span data-ttu-id="dfbbe-186">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="dfbbe-186">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="dfbbe-187">a.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-187">a.</span></span> <span data-ttu-id="dfbbe-188">Klikněte na tlačítko **požadované**.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-188">Click **Required**.</span></span>
   
    <span data-ttu-id="dfbbe-189">b.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-189">b.</span></span> <span data-ttu-id="dfbbe-190">Na portálu Azure na **konfigurovat přihlášení** okno, kopie **SAML jeden přihlašování adresa URL služby** hodnotu a vložte ji do **přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-190">In the Azure portal, on the **Configure sign-on** window, copy the **SAML Single Sign-On Service URL** value, and then paste it into the **Sign-in URL** textbox.</span></span>

    <span data-ttu-id="dfbbe-191">c.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-191">c.</span></span> <span data-ttu-id="dfbbe-192">Klikněte na tlačítko **vyberte certifikát**a pak přejděte do vaší **Base64 kódovaného certifikátu souboru**.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-192">Click **Choose certificate**, and then browse to your **Base64 encoded certificate file**.</span></span>

    <span data-ttu-id="dfbbe-193">d.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-193">d.</span></span> <span data-ttu-id="dfbbe-194">Klikněte na tlačítko **uložit změny** k dokončení konfigurace na vaší schránky pro obchodní klienta.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-194">Click **Save changes** to complete the configuration on your DropBox for Business tenant.</span></span>

> [!TIP]
> <span data-ttu-id="dfbbe-195">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="dfbbe-195">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="dfbbe-196">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-196">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="dfbbe-197">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="dfbbe-197">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="dfbbe-198">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="dfbbe-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="dfbbe-199">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-199">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="dfbbe-201">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="dfbbe-201">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="dfbbe-202">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-202">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_01.png) 

2.  <span data-ttu-id="dfbbe-204">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-204">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="dfbbe-206">V horní části okna klikněte na položku **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-206">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="dfbbe-208">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="dfbbe-208">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="dfbbe-210">a.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-210">a.</span></span> <span data-ttu-id="dfbbe-211">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-211">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dfbbe-212">b.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-212">b.</span></span> <span data-ttu-id="dfbbe-213">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-213">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="dfbbe-214">c.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-214">c.</span></span> <span data-ttu-id="dfbbe-215">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-215">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="dfbbe-216">d.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-216">d.</span></span> <span data-ttu-id="dfbbe-217">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-217">Click **Create**.</span></span>
 
### <a name="creating-a-dropbox-for-business-test-user"></a><span data-ttu-id="dfbbe-218">Vytváření Dropbox pro obchodní testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="dfbbe-218">Creating a Dropbox for Business test user</span></span>

<span data-ttu-id="dfbbe-219">V této části se uživatel volá Britta Simon vytvoří v Dropbox pro firmy.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-219">In this section, a user called Britta Simon is created in Dropbox for Business.</span></span> <span data-ttu-id="dfbbe-220">Dropbox pro firmy podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-220">Dropbox for Business supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="dfbbe-221">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-221">There is no action item for you in this section.</span></span> <span data-ttu-id="dfbbe-222">Pokud uživatel v Dropbox pro firmy ještě neexistuje, vytvoří se nový při pokusu o přístup k Dropboxu pro firmy.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-222">If a user doesn't already exist in Dropbox for Business, a new one is created when you attempt to access Dropbox for Business.</span></span>

>[!Note]
><span data-ttu-id="dfbbe-223">Pokud je potřeba ručně vytvořit uživatele, obraťte se na [Dropbox pro tým podpory obchodní klienta](https://www.dropbox.com/business/contact)</span><span class="sxs-lookup"><span data-stu-id="dfbbe-223">If you need to create a user manually, Contact [Dropbox for Business Client support team](https://www.dropbox.com/business/contact)</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="dfbbe-224">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="dfbbe-224">Assigning the Azure AD test user</span></span>

<span data-ttu-id="dfbbe-225">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Dropbox pro firmy.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-225">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Dropbox for Business.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="dfbbe-227">**Pokud chcete přiřadit Britta Simon Dropbox pro firmy, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="dfbbe-227">**To assign Britta Simon to Dropbox for Business, perform the following steps:**</span></span>

1. <span data-ttu-id="dfbbe-228">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-228">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="dfbbe-230">V seznamu aplikací vyberte **Dropbox pro firmy**.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-230">In the applications list, select **Dropbox for Business**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png) 

3. <span data-ttu-id="dfbbe-232">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-232">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="dfbbe-234">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-234">Click **Add** button.</span></span> <span data-ttu-id="dfbbe-235">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="dfbbe-237">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-237">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="dfbbe-238">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="dfbbe-239">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="dfbbe-240">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="dfbbe-240">Testing single sign-on</span></span>

<span data-ttu-id="dfbbe-241">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-241">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="dfbbe-242">Po kliknutí na tlačítko Dropbox pro obchodní dlaždici na přístupovém panelu, měli byste obdržet přihlašovací stránku vaší schránky pro obchodní aplikace.</span><span class="sxs-lookup"><span data-stu-id="dfbbe-242">When you click the Dropbox for Business tile in the Access Panel, you should get login page of your Dropbox for Business application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dfbbe-243">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="dfbbe-243">Additional resources</span></span>

* [<span data-ttu-id="dfbbe-244">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dfbbe-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dfbbe-245">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="dfbbe-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="dfbbe-246">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="dfbbe-246">Configure User Provisioning</span></span>](active-directory-saas-dropboxforbusiness-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_203.png

