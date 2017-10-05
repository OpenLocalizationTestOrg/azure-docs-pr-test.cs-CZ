---
title: 'Kurz: Azure Active Directory integrace s AirWatch | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a AirWatch."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 1996ec97e7c0d94c5606ca43bb5956548f1f3712
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a><span data-ttu-id="b78a9-103">Kurz: Azure Active Directory integrace s AirWatch</span><span class="sxs-lookup"><span data-stu-id="b78a9-103">Tutorial: Azure Active Directory integration with AirWatch</span></span>

<span data-ttu-id="b78a9-104">V tomto kurzu zjistěte, jak integrovat AirWatch s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b78a9-104">In this tutorial, you learn how to integrate AirWatch with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b78a9-105">Integrace AirWatch s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b78a9-105">Integrating AirWatch with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b78a9-106">Můžete řídit ve službě Azure AD, který má přístup k AirWatch</span><span class="sxs-lookup"><span data-stu-id="b78a9-106">You can control in Azure AD who has access to AirWatch</span></span>
- <span data-ttu-id="b78a9-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k AirWatch (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b78a9-107">You can enable your users to automatically get signed-on to AirWatch (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b78a9-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b78a9-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b78a9-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b78a9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b78a9-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b78a9-110">Prerequisites</span></span>

<span data-ttu-id="b78a9-111">Konfigurace integrace Azure AD s AirWatch, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="b78a9-111">To configure Azure AD integration with AirWatch, you need the following items:</span></span>

- <span data-ttu-id="b78a9-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b78a9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b78a9-113">AirWatch jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b78a9-113">An AirWatch single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b78a9-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b78a9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b78a9-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b78a9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b78a9-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b78a9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b78a9-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b78a9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b78a9-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b78a9-118">Scenario description</span></span>
<span data-ttu-id="b78a9-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b78a9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b78a9-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b78a9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b78a9-121">Přidání AirWatch z Galerie</span><span class="sxs-lookup"><span data-stu-id="b78a9-121">Adding AirWatch from the gallery</span></span>
2. <span data-ttu-id="b78a9-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b78a9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-airwatch-from-the-gallery"></a><span data-ttu-id="b78a9-123">Přidání AirWatch z Galerie</span><span class="sxs-lookup"><span data-stu-id="b78a9-123">Adding AirWatch from the gallery</span></span>
<span data-ttu-id="b78a9-124">Při konfiguraci integrace AirWatch do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS AirWatch z galerie.</span><span class="sxs-lookup"><span data-stu-id="b78a9-124">To configure the integration of AirWatch into Azure AD, you need to add AirWatch from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b78a9-125">**Pokud chcete přidat AirWatch z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b78a9-125">**To add AirWatch from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b78a9-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b78a9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b78a9-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b78a9-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b78a9-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b78a9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b78a9-133">Do vyhledávacího pole zadejte **AirWatch**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-133">In the search box, type **AirWatch**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_search.png)

5. <span data-ttu-id="b78a9-135">Na panelu výsledků vyberte **AirWatch**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b78a9-135">In the results panel, select **AirWatch**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b78a9-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b78a9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b78a9-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s AirWatch podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="b78a9-138">In this section, you configure and test Azure AD single sign-on with AirWatch based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b78a9-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v AirWatch je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b78a9-139">For single sign-on to work, Azure AD needs to know what the counterpart user in AirWatch is to a user in Azure AD.</span></span> <span data-ttu-id="b78a9-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v AirWatch musí navázat.</span><span class="sxs-lookup"><span data-stu-id="b78a9-140">In other words, a link relationship between an Azure AD user and the related user in AirWatch needs to be established.</span></span>

<span data-ttu-id="b78a9-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v AirWatch.</span><span class="sxs-lookup"><span data-stu-id="b78a9-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in AirWatch.</span></span>

<span data-ttu-id="b78a9-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s AirWatch, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="b78a9-142">To configure and test Azure AD single sign-on with AirWatch, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b78a9-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="b78a9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b78a9-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b78a9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b78a9-145">**[Vytvoření zkušebního uživatele AirWatch](#creating-a-airwatch-test-user)**  – Pokud chcete mít protějšek Britta Simon v AirWatch propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="b78a9-145">**[Creating a AirWatch test user](#creating-a-airwatch-test-user)** - to have a counterpart of Britta Simon in AirWatch that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b78a9-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b78a9-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b78a9-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b78a9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b78a9-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b78a9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b78a9-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci AirWatch.</span><span class="sxs-lookup"><span data-stu-id="b78a9-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your AirWatch application.</span></span>

<span data-ttu-id="b78a9-150">**Ke konfiguraci Azure AD jednotné přihlašování s AirWatch, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b78a9-150">**To configure Azure AD single sign-on with AirWatch, perform the following steps:**</span></span>

1. <span data-ttu-id="b78a9-151">Na portálu Azure na **AirWatch** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-151">In the Azure portal, on the **AirWatch** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b78a9-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b78a9-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_samlbase.png)

3. <span data-ttu-id="b78a9-155">Na **AirWatch domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b78a9-155">On the **AirWatch Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_url.png)

    <span data-ttu-id="b78a9-157">a.</span><span class="sxs-lookup"><span data-stu-id="b78a9-157">a.</span></span> <span data-ttu-id="b78a9-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`</span><span class="sxs-lookup"><span data-stu-id="b78a9-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`</span></span>

    <span data-ttu-id="b78a9-159">b.</span><span class="sxs-lookup"><span data-stu-id="b78a9-159">b.</span></span> <span data-ttu-id="b78a9-160">V **identifikátor** textovému poli, zadejte hodnotu jako`AirWatch`</span><span class="sxs-lookup"><span data-stu-id="b78a9-160">In the **Identifier** textbox, type the value as `AirWatch`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b78a9-161">Tato hodnota není reálné.</span><span class="sxs-lookup"><span data-stu-id="b78a9-161">This value is not the real.</span></span> <span data-ttu-id="b78a9-162">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b78a9-162">Update this value with the actual Sign-on URL.</span></span> <span data-ttu-id="b78a9-163">Obraťte se na [tým podpory AirWatch klienta](http://www.air-watch.com/company/contact-us/) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b78a9-163">Contact [AirWatch Client support team](http://www.air-watch.com/company/contact-us/) to get this value.</span></span> 
 
4. <span data-ttu-id="b78a9-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b78a9-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_certificate.png) 

5. <span data-ttu-id="b78a9-166">Na **AirWatch konfigurace** klikněte na tlačítko **konfigurace AirWatch** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="b78a9-166">On the **AirWatch Configuration** section, click **Configure AirWatch** to open **Configure sign-on** window.</span></span> <span data-ttu-id="b78a9-167">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="b78a9-167">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_configure.png) 

6. <span data-ttu-id="b78a9-169">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b78a9-169">Click **Save** button.</span></span>

    <span data-ttu-id="b78a9-170">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-airwatch-tutorial/tutorial_general_400.png)
<CS></span><span class="sxs-lookup"><span data-stu-id="b78a9-170">![Configure Single Sign-On](./media/active-directory-saas-airwatch-tutorial/tutorial_general_400.png)
<CS></span></span>
7. <span data-ttu-id="b78a9-171">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti AirWatch jako správce.</span><span class="sxs-lookup"><span data-stu-id="b78a9-171">In a different web browser window, log in to your AirWatch company site as an administrator.</span></span>

8. <span data-ttu-id="b78a9-172">V levém navigačním podokně klikněte na tlačítko **účty**a potom klikněte na **správci**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-172">In the left navigation pane, click **Accounts**, and then click **Administrators**.</span></span>
   
   <span data-ttu-id="b78a9-173">![Správci](./media/active-directory-saas-airwatch-tutorial/ic791920.png "správci")</span><span class="sxs-lookup"><span data-stu-id="b78a9-173">![Administrators](./media/active-directory-saas-airwatch-tutorial/ic791920.png "Administrators")</span></span>

9. <span data-ttu-id="b78a9-174">Rozbalte **nastavení** nabídce a pak klikněte na tlačítko **Directory Services**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-174">Expand the **Settings** menu, and then click **Directory Services**.</span></span>
   
   <span data-ttu-id="b78a9-175">![Nastavení](./media/active-directory-saas-airwatch-tutorial/ic791921.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="b78a9-175">![Settings](./media/active-directory-saas-airwatch-tutorial/ic791921.png "Settings")</span></span>

10. <span data-ttu-id="b78a9-176">Klikněte na tlačítko **uživatele** ve **Base DN** textovému poli, zadejte název domény a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-176">Click the **User** tab, in the **Base DN** textbox, type your domain name, and then click **Save**.</span></span>
   
   <span data-ttu-id="b78a9-177">![Uživatel](./media/active-directory-saas-airwatch-tutorial/ic791922.png "uživatele")</span><span class="sxs-lookup"><span data-stu-id="b78a9-177">![User](./media/active-directory-saas-airwatch-tutorial/ic791922.png "User")</span></span>

11. <span data-ttu-id="b78a9-178">Klikněte **Server** kartě.</span><span class="sxs-lookup"><span data-stu-id="b78a9-178">Click the **Server** tab.</span></span>
   
   <span data-ttu-id="b78a9-179">![Server](./media/active-directory-saas-airwatch-tutorial/ic791923.png "serveru")</span><span class="sxs-lookup"><span data-stu-id="b78a9-179">![Server](./media/active-directory-saas-airwatch-tutorial/ic791923.png "Server")</span></span>

12. <span data-ttu-id="b78a9-180">Proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b78a9-180">Perform the following steps:</span></span>
    
    <span data-ttu-id="b78a9-181">![Nahrát](./media/active-directory-saas-airwatch-tutorial/ic791924.png "nahrát")</span><span class="sxs-lookup"><span data-stu-id="b78a9-181">![Upload](./media/active-directory-saas-airwatch-tutorial/ic791924.png "Upload")</span></span>   
    
    <span data-ttu-id="b78a9-182">a.</span><span class="sxs-lookup"><span data-stu-id="b78a9-182">a.</span></span> <span data-ttu-id="b78a9-183">Jako **typu adresáře**, vyberte **žádné**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-183">As **Directory Type**, select **None**.</span></span>

    <span data-ttu-id="b78a9-184">b.</span><span class="sxs-lookup"><span data-stu-id="b78a9-184">b.</span></span> <span data-ttu-id="b78a9-185">Vyberte **pomocí SAML pro ověřování**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-185">Select **Use SAML For Authentication**.</span></span>

    <span data-ttu-id="b78a9-186">c.</span><span class="sxs-lookup"><span data-stu-id="b78a9-186">c.</span></span> <span data-ttu-id="b78a9-187">Pokud chcete uložit stažený certifikát, klikněte na tlačítko **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-187">To upload the downloaded certificate, click **Upload**.</span></span>

13. <span data-ttu-id="b78a9-188">V **požadavku** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b78a9-188">In the **Request** section, perform the following steps:</span></span>
    
    <span data-ttu-id="b78a9-189">![Žádosti o](./media/active-directory-saas-airwatch-tutorial/ic791925.png "požadavku")</span><span class="sxs-lookup"><span data-stu-id="b78a9-189">![Request](./media/active-directory-saas-airwatch-tutorial/ic791925.png "Request")</span></span>  

    <span data-ttu-id="b78a9-190">a.</span><span class="sxs-lookup"><span data-stu-id="b78a9-190">a.</span></span> <span data-ttu-id="b78a9-191">Jako **typ vazby žádosti**, vyberte **POST**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-191">As **Request Binding Type**, select **POST**.</span></span>

    <span data-ttu-id="b78a9-192">b.</span><span class="sxs-lookup"><span data-stu-id="b78a9-192">b.</span></span> <span data-ttu-id="b78a9-193">Na portálu Azure na **nakonfigurovat jednotné přihlašování v Airwatch** dialogové okno stránky, kopírování **SAML jeden přihlašování adresa URL služby** hodnotu a vložte ji do **Identity zprostředkovatele jednotné přihlašování Adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="b78a9-193">In the Azure portal, on the **Configure single sign-on at Airwatch** dialog page, copy the **SAML Single Sign-On Service URL** value, and then paste it into the **Identity Provider Single Sign On URL** textbox.</span></span>

    <span data-ttu-id="b78a9-194">c.</span><span class="sxs-lookup"><span data-stu-id="b78a9-194">c.</span></span> <span data-ttu-id="b78a9-195">Jako **NameID formátu**, vyberte **e-mailovou adresu**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-195">As **NameID Format**, select **Email Address**.</span></span>

    <span data-ttu-id="b78a9-196">d.</span><span class="sxs-lookup"><span data-stu-id="b78a9-196">d.</span></span> <span data-ttu-id="b78a9-197">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-197">Click **Save**.</span></span>

14. <span data-ttu-id="b78a9-198">Klikněte **uživatele** kartě znovu.</span><span class="sxs-lookup"><span data-stu-id="b78a9-198">Click the **User** tab again.</span></span>
    
    <span data-ttu-id="b78a9-199">![Uživatel](./media/active-directory-saas-airwatch-tutorial/ic791926.png "uživatele")</span><span class="sxs-lookup"><span data-stu-id="b78a9-199">![User](./media/active-directory-saas-airwatch-tutorial/ic791926.png "User")</span></span>

15. <span data-ttu-id="b78a9-200">V **atribut** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b78a9-200">In the **Attribute** section, perform the following steps:</span></span>
    
    <span data-ttu-id="b78a9-201">![Atribut](./media/active-directory-saas-airwatch-tutorial/ic791927.png "atribut")</span><span class="sxs-lookup"><span data-stu-id="b78a9-201">![Attribute](./media/active-directory-saas-airwatch-tutorial/ic791927.png "Attribute")</span></span>

    <span data-ttu-id="b78a9-202">a.</span><span class="sxs-lookup"><span data-stu-id="b78a9-202">a.</span></span> <span data-ttu-id="b78a9-203">V **identifikátor objektu** textovému poli, typ **http://schemas.microsoft.com/identity/claims/objectidentifier**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-203">In the **Object Identifier** textbox, type **http://schemas.microsoft.com/identity/claims/objectidentifier**.</span></span>

    <span data-ttu-id="b78a9-204">b.</span><span class="sxs-lookup"><span data-stu-id="b78a9-204">b.</span></span> <span data-ttu-id="b78a9-205">V **uživatelské jméno** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-205">In the **Username** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span>

    <span data-ttu-id="b78a9-206">c.</span><span class="sxs-lookup"><span data-stu-id="b78a9-206">c.</span></span> <span data-ttu-id="b78a9-207">V **zobrazovaný název** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-207">In the **Display Name** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span></span>

    <span data-ttu-id="b78a9-208">d.</span><span class="sxs-lookup"><span data-stu-id="b78a9-208">d.</span></span> <span data-ttu-id="b78a9-209">V **křestní jméno** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-209">In the **First Name** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span></span>

    <span data-ttu-id="b78a9-210">e.</span><span class="sxs-lookup"><span data-stu-id="b78a9-210">e.</span></span> <span data-ttu-id="b78a9-211">V **příjmení** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-211">In the **Last Name** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span></span>

    <span data-ttu-id="b78a9-212">f.</span><span class="sxs-lookup"><span data-stu-id="b78a9-212">f.</span></span> <span data-ttu-id="b78a9-213">V **e-mailu** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-213">In the **Email** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span>

    <span data-ttu-id="b78a9-214">g.</span><span class="sxs-lookup"><span data-stu-id="b78a9-214">g.</span></span> <span data-ttu-id="b78a9-215">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-215">Click **Save**.</span></span>

<CE>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b78a9-216">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b78a9-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="b78a9-217">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b78a9-217">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b78a9-219">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b78a9-219">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b78a9-220">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b78a9-220">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b78a9-222">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-222">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b78a9-224">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b78a9-224">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b78a9-226">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b78a9-226">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b78a9-228">a.</span><span class="sxs-lookup"><span data-stu-id="b78a9-228">a.</span></span> <span data-ttu-id="b78a9-229">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-229">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b78a9-230">b.</span><span class="sxs-lookup"><span data-stu-id="b78a9-230">b.</span></span> <span data-ttu-id="b78a9-231">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b78a9-231">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="b78a9-232">c.</span><span class="sxs-lookup"><span data-stu-id="b78a9-232">c.</span></span> <span data-ttu-id="b78a9-233">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-233">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b78a9-234">d.</span><span class="sxs-lookup"><span data-stu-id="b78a9-234">d.</span></span> <span data-ttu-id="b78a9-235">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-235">Click **Create**.</span></span>
 
### <a name="creating-a-airwatch-test-user"></a><span data-ttu-id="b78a9-236">Vytvoření zkušebního uživatele AirWatch</span><span class="sxs-lookup"><span data-stu-id="b78a9-236">Creating a AirWatch test user</span></span>

<span data-ttu-id="b78a9-237">Pokud chcete povolit uživatelům Azure AD přihlášení k AirWatch, se musí být zřízená v k AirWatch.</span><span class="sxs-lookup"><span data-stu-id="b78a9-237">To enable Azure AD users to log in to AirWatch, they must be provisioned in to AirWatch.</span></span>

* <span data-ttu-id="b78a9-238">Při zřizování AirWatch, je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="b78a9-238">When AirWatch, provisioning is a manual task.</span></span>

<span data-ttu-id="b78a9-239">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b78a9-239">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="b78a9-240">Přihlaste se k vaší **AirWatch** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="b78a9-240">Log in to your **AirWatch** company site as administrator.</span></span>
2. <span data-ttu-id="b78a9-241">V navigačním podokně na levé straně klikněte na **účty**a potom klikněte na **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-241">In the navigation pane on the left side, click **Accounts**, and then click **Users**.</span></span>
   
   <span data-ttu-id="b78a9-242">![Uživatelé](./media/active-directory-saas-airwatch-tutorial/ic791929.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="b78a9-242">![Users](./media/active-directory-saas-airwatch-tutorial/ic791929.png "Users")</span></span>
3. <span data-ttu-id="b78a9-243">V **uživatelé** nabídky, klikněte na tlačítko **zobrazení seznamu**a potom klikněte na **přidat \> přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-243">In the **Users** menu, click **List View**, and then click **Add \> Add User**.</span></span>
   
   <span data-ttu-id="b78a9-244">![Přidat uživatele](./media/active-directory-saas-airwatch-tutorial/ic791930.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="b78a9-244">![Add User](./media/active-directory-saas-airwatch-tutorial/ic791930.png "Add User")</span></span>
4. <span data-ttu-id="b78a9-245">Na **přidat nebo upravit uživatele** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b78a9-245">On the **Add / Edit User** dialog, perform the following steps:</span></span>

   <span data-ttu-id="b78a9-246">![Přidat uživatele](./media/active-directory-saas-airwatch-tutorial/ic791931.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="b78a9-246">![Add User](./media/active-directory-saas-airwatch-tutorial/ic791931.png "Add User")</span></span>   
   1. <span data-ttu-id="b78a9-247">Typ **uživatelské jméno**, **heslo**, **potvrzení hesla**, **křestní jméno**, **příjmení**,  **E-mailová adresa** platný účtu Azure Active Directory má mají být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="b78a9-247">Type the **Username**, **Password**, **Confirm Password**, **First Name**, **Last Name**, **Email Address** of a valid Azure Active Directory account you want to provision into the related textboxes.</span></span>
   2. <span data-ttu-id="b78a9-248">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-248">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="b78a9-249">Můžete použít všechny ostatní AirWatch uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované AirWatch zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="b78a9-249">You can use any other AirWatch user account creation tools or APIs provided by AirWatch to provision AAD user accounts.</span></span>
>  

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b78a9-250">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b78a9-250">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b78a9-251">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu AirWatch.</span><span class="sxs-lookup"><span data-stu-id="b78a9-251">In this section, you enable Britta Simon to use Azure single sign-on by granting access to AirWatch.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b78a9-253">**Pokud chcete přiřadit Britta Simon AirWatch, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b78a9-253">**To assign Britta Simon to AirWatch, perform the following steps:**</span></span>

1. <span data-ttu-id="b78a9-254">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-254">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b78a9-256">V seznamu aplikací vyberte **AirWatch**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-256">In the applications list, select **AirWatch**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_app.png) 

3. <span data-ttu-id="b78a9-258">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b78a9-258">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b78a9-260">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b78a9-260">Click **Add** button.</span></span> <span data-ttu-id="b78a9-261">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b78a9-261">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b78a9-263">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b78a9-263">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b78a9-264">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b78a9-264">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b78a9-265">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b78a9-265">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b78a9-266">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b78a9-266">Testing single sign-on</span></span>

<span data-ttu-id="b78a9-267">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b78a9-267">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b78a9-268">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="b78a9-268">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="b78a9-269">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b78a9-269">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="b78a9-270">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b78a9-270">Additional resources</span></span>

* [<span data-ttu-id="b78a9-271">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b78a9-271">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b78a9-272">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b78a9-272">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_203.png

