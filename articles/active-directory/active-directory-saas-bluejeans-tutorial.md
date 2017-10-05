---
title: 'Kurz: Azure Active Directory integrace s BlueJeans | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a BlueJeans."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: jeedes
ms.openlocfilehash: 03bf65852b8d3cf14aebf155891a028db86e78d0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a><span data-ttu-id="b492c-103">Kurz: Azure Active Directory integrace s BlueJeans</span><span class="sxs-lookup"><span data-stu-id="b492c-103">Tutorial: Azure Active Directory integration with BlueJeans</span></span>

<span data-ttu-id="b492c-104">V tomto kurzu zjistěte, jak integrovat BlueJeans s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b492c-104">In this tutorial, you learn how to integrate BlueJeans with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b492c-105">Integrace BlueJeans s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b492c-105">Integrating BlueJeans with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b492c-106">Můžete řídit ve službě Azure AD, který má přístup k BlueJeans</span><span class="sxs-lookup"><span data-stu-id="b492c-106">You can control in Azure AD who has access to BlueJeans</span></span>
- <span data-ttu-id="b492c-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k BlueJeans (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b492c-107">You can enable your users to automatically get signed-on to BlueJeans (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b492c-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b492c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b492c-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b492c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b492c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b492c-110">Prerequisites</span></span>

<span data-ttu-id="b492c-111">Konfigurace integrace Azure AD s BlueJeans, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="b492c-111">To configure Azure AD integration with BlueJeans, you need the following items:</span></span>

- <span data-ttu-id="b492c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b492c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b492c-113">BlueJeans jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b492c-113">A BlueJeans single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b492c-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b492c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b492c-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b492c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b492c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b492c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b492c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b492c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b492c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b492c-118">Scenario description</span></span>
<span data-ttu-id="b492c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b492c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b492c-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b492c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b492c-121">Přidání BlueJeans z Galerie</span><span class="sxs-lookup"><span data-stu-id="b492c-121">Adding BlueJeans from the gallery</span></span>
2. <span data-ttu-id="b492c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b492c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bluejeans-from-the-gallery"></a><span data-ttu-id="b492c-123">Přidání BlueJeans z Galerie</span><span class="sxs-lookup"><span data-stu-id="b492c-123">Adding BlueJeans from the gallery</span></span>
<span data-ttu-id="b492c-124">Při konfiguraci integrace BlueJeans do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS BlueJeans z galerie.</span><span class="sxs-lookup"><span data-stu-id="b492c-124">To configure the integration of BlueJeans into Azure AD, you need to add BlueJeans from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b492c-125">**Pokud chcete přidat BlueJeans z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b492c-125">**To add BlueJeans from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b492c-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b492c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b492c-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b492c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b492c-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b492c-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b492c-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b492c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b492c-133">Do vyhledávacího pole zadejte **BlueJeans**.</span><span class="sxs-lookup"><span data-stu-id="b492c-133">In the search box, type **BlueJeans**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_search.png)

5. <span data-ttu-id="b492c-135">Na panelu výsledků vyberte **BlueJeans**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b492c-135">In the results panel, select **BlueJeans**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b492c-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b492c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b492c-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s BlueJeans podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="b492c-138">In this section, you configure and test Azure AD single sign-on with BlueJeans based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b492c-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v BlueJeans je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b492c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BlueJeans is to a user in Azure AD.</span></span> <span data-ttu-id="b492c-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v BlueJeans musí navázat.</span><span class="sxs-lookup"><span data-stu-id="b492c-140">In other words, a link relationship between an Azure AD user and the related user in BlueJeans needs to be established.</span></span>

<span data-ttu-id="b492c-141">V BlueJeans, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="b492c-141">In BlueJeans, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="b492c-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s BlueJeans, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="b492c-142">To configure and test Azure AD single sign-on with BlueJeans, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b492c-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="b492c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b492c-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b492c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b492c-145">**[Vytvoření zkušebního uživatele BlueJeans](#creating-a-bluejeans-test-user)**  – Pokud chcete mít protějšek Britta Simon v BlueJeans propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="b492c-145">**[Creating a BlueJeans test user](#creating-a-bluejeans-test-user)** - to have a counterpart of Britta Simon in BlueJeans that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b492c-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b492c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b492c-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b492c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b492c-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b492c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b492c-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci BlueJeans.</span><span class="sxs-lookup"><span data-stu-id="b492c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BlueJeans application.</span></span>

<span data-ttu-id="b492c-150">**Ke konfiguraci Azure AD jednotné přihlašování s BlueJeans, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b492c-150">**To configure Azure AD single sign-on with BlueJeans, perform the following steps:**</span></span>

1. <span data-ttu-id="b492c-151">Na portálu Azure na **BlueJeans** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b492c-151">In the Azure portal, on the **BlueJeans** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b492c-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b492c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. <span data-ttu-id="b492c-155">Na **BlueJeans domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b492c-155">On the **BlueJeans Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_url.png)

    <span data-ttu-id="b492c-157">a.</span><span class="sxs-lookup"><span data-stu-id="b492c-157">a.</span></span> <span data-ttu-id="b492c-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.BlueJeans.com`</span><span class="sxs-lookup"><span data-stu-id="b492c-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.BlueJeans.com`</span></span>

    <span data-ttu-id="b492c-159">b.</span><span class="sxs-lookup"><span data-stu-id="b492c-159">b.</span></span> <span data-ttu-id="b492c-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.BlueJeans.com`</span><span class="sxs-lookup"><span data-stu-id="b492c-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.BlueJeans.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b492c-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="b492c-161">These values are not real.</span></span> <span data-ttu-id="b492c-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="b492c-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b492c-163">Obraťte se na [tým podpory BlueJeans klienta](https://support.bluejeans.com/contact) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="b492c-163">Contact [BlueJeans Client support team](https://support.bluejeans.com/contact) to get these values.</span></span> 
 
4. <span data-ttu-id="b492c-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="b492c-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. <span data-ttu-id="b492c-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b492c-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b492c-168">Na **BlueJeans konfigurace** klikněte na tlačítko **konfigurace BlueJeans** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="b492c-168">On the **BlueJeans Configuration** section, click **Configure BlueJeans** to open **Configure sign-on** window.</span></span> <span data-ttu-id="b492c-169">Kopírování **Sign-Out adresu URL, adresy URL pro změnu hesla a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="b492c-169">Copy the **Sign-Out URL, Change Password URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. <span data-ttu-id="b492c-171">V okně prohlížeče jiný web, přihlaste se k vaší **BlueJeans** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="b492c-171">In a different web browser window, log in to your **BlueJeans** company site as an administrator.</span></span>

8. <span data-ttu-id="b492c-172">Přejděte na **správce \> nastavení skupiny \> zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="b492c-172">Go to **ADMIN \> Group Settings \> Security**.</span></span>
   
   <span data-ttu-id="b492c-173">![Správce](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "správce")</span><span class="sxs-lookup"><span data-stu-id="b492c-173">![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Admin")</span></span>

9. <span data-ttu-id="b492c-174">V **zabezpečení** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b492c-174">In the **Security** section, perform the following steps:</span></span>
   
   <span data-ttu-id="b492c-175">![Jednotné přihlašování SAML na](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "jednotného přihlašování k SAML")</span><span class="sxs-lookup"><span data-stu-id="b492c-175">![SAML Single Sign On](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML Single Sign On")</span></span>   
   
   <span data-ttu-id="b492c-176">a.</span><span class="sxs-lookup"><span data-stu-id="b492c-176">a.</span></span> <span data-ttu-id="b492c-177">Vyberte **jednotného přihlašování k SAML**.</span><span class="sxs-lookup"><span data-stu-id="b492c-177">Select **SAML Single Sign On**.</span></span>
  
   <span data-ttu-id="b492c-178">b.</span><span class="sxs-lookup"><span data-stu-id="b492c-178">b.</span></span> <span data-ttu-id="b492c-179">Vyberte **zapněte automatické zřizování**.</span><span class="sxs-lookup"><span data-stu-id="b492c-179">Select **Enable automatic provisioning**.</span></span>

10. <span data-ttu-id="b492c-180">Přesunout pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="b492c-180">Move on with the following steps:</span></span>

    <span data-ttu-id="b492c-181">![Certifikát cesta](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "certifikátu cesta")</span><span class="sxs-lookup"><span data-stu-id="b492c-181">![Certificate Path](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Certificate Path")</span></span>
    
    <span data-ttu-id="b492c-182">a.</span><span class="sxs-lookup"><span data-stu-id="b492c-182">a.</span></span> <span data-ttu-id="b492c-183">Klikněte na tlačítko **zvolit soubor**a pak nahrajte stažený certifikát.</span><span class="sxs-lookup"><span data-stu-id="b492c-183">Click **Choose File**, and then upload the downloaded certificate.</span></span>
   
    <span data-ttu-id="b492c-184">b.</span><span class="sxs-lookup"><span data-stu-id="b492c-184">b.</span></span> <span data-ttu-id="b492c-185">Vložení **SAML jeden přihlašování adresa URL služby** do **přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="b492c-185">Paste **SAML Single Sign-On Service URL** into the **Login URL** textbox.</span></span>
   
    <span data-ttu-id="b492c-186">c.</span><span class="sxs-lookup"><span data-stu-id="b492c-186">c.</span></span> <span data-ttu-id="b492c-187">Vložení **heslo změnit adresu URL** do **heslo změnit adresu URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="b492c-187">Paste **Change Password URL** into the **Password Change URL** textbox.</span></span>
   
    <span data-ttu-id="b492c-188">d.</span><span class="sxs-lookup"><span data-stu-id="b492c-188">d.</span></span> <span data-ttu-id="b492c-189">Vložení **Sign-Out URL** do **adresy URL odhlašovací** textové pole.</span><span class="sxs-lookup"><span data-stu-id="b492c-189">Paste **Sign-Out URL** into the **Logout URL** textbox.</span></span>

11. <span data-ttu-id="b492c-190">Přesunout pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="b492c-190">Move on with the following steps:</span></span>
    
    <span data-ttu-id="b492c-191">![Uložit změny](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "uložit změny")</span><span class="sxs-lookup"><span data-stu-id="b492c-191">![Save Changes](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Save Changes")</span></span>
    
    <span data-ttu-id="b492c-192">a.</span><span class="sxs-lookup"><span data-stu-id="b492c-192">a.</span></span> <span data-ttu-id="b492c-193">V **id uživatele** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="b492c-193">In the **User id** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
   
    <span data-ttu-id="b492c-194">b.</span><span class="sxs-lookup"><span data-stu-id="b492c-194">b.</span></span> <span data-ttu-id="b492c-195">V **e-mailu** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="b492c-195">In the **Email** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
   
    <span data-ttu-id="b492c-196">c.</span><span class="sxs-lookup"><span data-stu-id="b492c-196">c.</span></span> <span data-ttu-id="b492c-197">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="b492c-197">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="b492c-198">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="b492c-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b492c-199">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="b492c-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b492c-200">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b492c-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b492c-201">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b492c-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="b492c-202">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b492c-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b492c-204">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b492c-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b492c-205">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b492c-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b492c-207">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b492c-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b492c-209">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b492c-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b492c-211">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b492c-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b492c-213">a.</span><span class="sxs-lookup"><span data-stu-id="b492c-213">a.</span></span> <span data-ttu-id="b492c-214">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b492c-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b492c-215">b.</span><span class="sxs-lookup"><span data-stu-id="b492c-215">b.</span></span> <span data-ttu-id="b492c-216">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b492c-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b492c-217">c.</span><span class="sxs-lookup"><span data-stu-id="b492c-217">c.</span></span> <span data-ttu-id="b492c-218">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b492c-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b492c-219">d.</span><span class="sxs-lookup"><span data-stu-id="b492c-219">d.</span></span> <span data-ttu-id="b492c-220">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b492c-220">Click **Create**.</span></span>
 
### <a name="creating-a-bluejeans-test-user"></a><span data-ttu-id="b492c-221">Vytvoření zkušebního uživatele BlueJeans</span><span class="sxs-lookup"><span data-stu-id="b492c-221">Creating a BlueJeans test user</span></span>

<span data-ttu-id="b492c-222">Pokud chcete povolit uživatelům Azure AD přihlášení k BlueJeans, musí být zřízená do BlueJeans.</span><span class="sxs-lookup"><span data-stu-id="b492c-222">To enable Azure AD users to log in to BlueJeans, they must be provisioned into BlueJeans.</span></span>  

<span data-ttu-id="b492c-223">V případě BlueJeans zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="b492c-223">In case of BlueJeans, provisioning is a manual task.</span></span>

<span data-ttu-id="b492c-224">**Ke zřízení uživatelských účtů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b492c-224">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="b492c-225">Přihlaste se k vaší **BlueJeans** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="b492c-225">Log in to your **BlueJeans** company site as an administrator.</span></span>

2. <span data-ttu-id="b492c-226">Přejděte na **správce \> Správa uživatelů \> přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b492c-226">Go to **ADMIN \> Manage Users \> Add User**.</span></span>
   
   <span data-ttu-id="b492c-227">![Správce](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "správce")</span><span class="sxs-lookup"><span data-stu-id="b492c-227">![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Admin")</span></span>
   
   >[!IMPORTANT]
   ><span data-ttu-id="b492c-228">**Přidat uživatele** karta je dostupná pouze v případě, v **karta zabezpečení**, **zapněte automatické zřizování** není zaškrtnuta.</span><span class="sxs-lookup"><span data-stu-id="b492c-228">The **Add User** tab is only available if, in the **Security tab**, **Enable automatic provisioning** is unchecked.</span></span> 
   
3. <span data-ttu-id="b492c-229">V **přidat uživatele** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b492c-229">In the **Add User** section, perform the following steps:</span></span>

    <span data-ttu-id="b492c-230">![Přidat uživatele](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="b492c-230">![Add User](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Add User")</span></span>
    
    <span data-ttu-id="b492c-231">a.</span><span class="sxs-lookup"><span data-stu-id="b492c-231">a.</span></span> <span data-ttu-id="b492c-232">Zadejte **uživatelské jméno BlueJeans**, **e-mailová adresa**, **BlueJeans schůzku ID**, **moderátora hesla**, **úplný název**, **společnosti** platného účtu AAD chcete mají být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="b492c-232">Type a **BlueJeans Username**, an **Email address**, a **BlueJeans Meeting ID**, a **Moderator Passcode**, a **Full Name**, the **Company** of a valid AAD account you want to provision into the related textboxes.</span></span>
    
    <span data-ttu-id="b492c-233">b.</span><span class="sxs-lookup"><span data-stu-id="b492c-233">b.</span></span> <span data-ttu-id="b492c-234">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b492c-234">Click **Add User**.</span></span>

>[!NOTE]
><span data-ttu-id="b492c-235">Můžete použít všechny ostatní BlueJeans uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované BlueJeans zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="b492c-235">You can use any other BlueJeans user account creation tools or APIs provided by BlueJeans to provision AAD user accounts.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b492c-236">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b492c-236">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b492c-237">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu BlueJeans.</span><span class="sxs-lookup"><span data-stu-id="b492c-237">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BlueJeans.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b492c-239">**Pokud chcete přiřadit Britta Simon BlueJeans, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b492c-239">**To assign Britta Simon to BlueJeans, perform the following steps:**</span></span>

1. <span data-ttu-id="b492c-240">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b492c-240">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b492c-242">V seznamu aplikací vyberte **BlueJeans**.</span><span class="sxs-lookup"><span data-stu-id="b492c-242">In the applications list, select **BlueJeans**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_app.png) 

3. <span data-ttu-id="b492c-244">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b492c-244">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b492c-246">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b492c-246">Click **Add** button.</span></span> <span data-ttu-id="b492c-247">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b492c-247">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b492c-249">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b492c-249">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b492c-250">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b492c-250">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b492c-251">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b492c-251">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b492c-252">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b492c-252">Testing single sign-on</span></span>

<span data-ttu-id="b492c-253">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b492c-253">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b492c-254">Když kliknete na dlaždici BlueJeans na přístupovém panelu, měli byste obdržet přihlašovací stránku BlueJeans aplikace.</span><span class="sxs-lookup"><span data-stu-id="b492c-254">When you click the BlueJeans tile in the Access Panel, you should get login page of BlueJeans application.</span></span>
<span data-ttu-id="b492c-255">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b492c-255">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="b492c-256">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b492c-256">Additional resources</span></span>

* [<span data-ttu-id="b492c-257">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b492c-257">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b492c-258">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b492c-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_203.png

