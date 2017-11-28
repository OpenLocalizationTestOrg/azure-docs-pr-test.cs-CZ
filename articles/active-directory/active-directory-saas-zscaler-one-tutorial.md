---
title: 'Kurz: Azure Active Directory integrace s Zscaler jeden | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Zscaler jeden."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f352e00d-68d3-4a77-bb92-717d055da56f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 7d655c482a16c991a819eec84c84556d2f288a75
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-one"></a><span data-ttu-id="3e623-103">Kurz: Azure Active Directory integrace s Zscaler jeden</span><span class="sxs-lookup"><span data-stu-id="3e623-103">Tutorial: Azure Active Directory integration with Zscaler One</span></span>

<span data-ttu-id="3e623-104">V tomto kurzu zjistěte, jak integrovat Zscaler jeden s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3e623-104">In this tutorial, you learn how to integrate Zscaler One with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3e623-105">Integrace s Azure AD Zscaler jeden poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3e623-105">Integrating Zscaler One with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3e623-106">Můžete řídit ve službě Azure AD, který má přístup k jedné Zscaler</span><span class="sxs-lookup"><span data-stu-id="3e623-106">You can control in Azure AD who has access to Zscaler One</span></span>
- <span data-ttu-id="3e623-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Zscaler jeden (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3e623-107">You can enable your users to automatically get signed-on to Zscaler One (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3e623-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3e623-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3e623-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3e623-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3e623-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3e623-110">Prerequisites</span></span>

<span data-ttu-id="3e623-111">Konfigurace integrace Azure AD s Zscaler jeden, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="3e623-111">To configure Azure AD integration with Zscaler One, you need the following items:</span></span>

- <span data-ttu-id="3e623-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3e623-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3e623-113">Jednu Zscaler jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3e623-113">A Zscaler One single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3e623-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3e623-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3e623-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3e623-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3e623-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3e623-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3e623-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3e623-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3e623-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3e623-118">Scenario description</span></span>
<span data-ttu-id="3e623-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3e623-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3e623-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3e623-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3e623-121">Přidání Zscaler jeden z Galerie</span><span class="sxs-lookup"><span data-stu-id="3e623-121">Adding Zscaler One from the gallery</span></span>
2. <span data-ttu-id="3e623-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3e623-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-one-from-the-gallery"></a><span data-ttu-id="3e623-123">Přidání Zscaler jeden z Galerie</span><span class="sxs-lookup"><span data-stu-id="3e623-123">Adding Zscaler One from the gallery</span></span>
<span data-ttu-id="3e623-124">Chcete-li nakonfigurovat integraci Zscaler jeden do služby Azure AD, přidejte Zscaler jeden z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="3e623-124">To configure the integration of Zscaler One into Azure AD, you need to add Zscaler One from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3e623-125">**Pokud chcete přidat Zscaler jeden z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3e623-125">**To add Zscaler One from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3e623-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3e623-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3e623-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3e623-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3e623-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3e623-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="3e623-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e623-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="3e623-133">Do vyhledávacího pole zadejte **Zscaler jeden**.</span><span class="sxs-lookup"><span data-stu-id="3e623-133">In the search box, type **Zscaler One**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_search.png)

5. <span data-ttu-id="3e623-135">Na panelu výsledků vyberte **Zscaler jeden**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3e623-135">In the results panel, select **Zscaler One**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3e623-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3e623-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3e623-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Zscaler jeden podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="3e623-138">In this section, you configure and test Azure AD single sign-on with Zscaler One based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3e623-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Zscaler jeden je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3e623-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Zscaler One is to a user in Azure AD.</span></span> <span data-ttu-id="3e623-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Zscaler jeden musí navázat.</span><span class="sxs-lookup"><span data-stu-id="3e623-140">In other words, a link relationship between an Azure AD user and the related user in Zscaler One needs to be established.</span></span>

<span data-ttu-id="3e623-141">V jedné Zscaler přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="3e623-141">In Zscaler One, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3e623-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Zscaler jeden, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="3e623-142">To configure and test Azure AD single sign-on with Zscaler One, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3e623-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="3e623-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3e623-144">**[Konfigurace nastavení proxy serveru](#configuring-proxy-settings)**  – Pokud chcete nakonfigurovat nastavení proxy serveru v Internet Exploreru</span><span class="sxs-lookup"><span data-stu-id="3e623-144">**[Configuring proxy settings](#configuring-proxy-settings)** - to configure the proxy settings in Internet Explorer</span></span>
3. <span data-ttu-id="3e623-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3e623-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="3e623-146">**[Vytváření Zscaler jeden testovací uživatele](#creating-a-zscaler-one-test-user)**  – Pokud chcete mít protějšek Britta Simon v Zscaler jednu propojenou s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="3e623-146">**[Creating a Zscaler One test user](#creating-a-zscaler-one-test-user)** - to have a counterpart of Britta Simon in Zscaler One that is linked to the Azure AD representation of user.</span></span>
5. <span data-ttu-id="3e623-147">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3e623-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
6. <span data-ttu-id="3e623-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3e623-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3e623-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3e623-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3e623-150">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v Zscaler jednu aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3e623-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Zscaler One application.</span></span>

<span data-ttu-id="3e623-151">**Ke konfiguraci Azure AD jednotné přihlašování s Zscaler jeden, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3e623-151">**To configure Azure AD single sign-on with Zscaler One, perform the following steps:**</span></span>

1. <span data-ttu-id="3e623-152">Na portálu Azure na **Zscaler jeden** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3e623-152">In the Azure portal, on the **Zscaler One** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="3e623-154">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3e623-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_samlbase.png)

3. <span data-ttu-id="3e623-156">Na **Zscaler jednu doménu a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3e623-156">On the **Zscaler One Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_url.png)

    <span data-ttu-id="3e623-158">Do textového pole Přihlašovací adresa URL zadejte adresu URL používá uživatelům přihlášení do aplikace Zscaler jeden.</span><span class="sxs-lookup"><span data-stu-id="3e623-158">In the Sign-on URL textbox, type the URL used by your users to sign-on to your Zscaler One application.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3e623-159">Budete muset aktualizovat tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3e623-159">You have to update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="3e623-160">Obraťte se na [tým podpory pro jednoho klienta Zscaler](https://www.zscaler.com/company/contact) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="3e623-160">Contact [Zscaler One Client support team](https://www.zscaler.com/company/contact) to get these values.</span></span>

4. <span data-ttu-id="3e623-161">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="3e623-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_certificate.png) 

5. <span data-ttu-id="3e623-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3e623-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3e623-165">Na **jednu konfiguraci Zscaler** klikněte na tlačítko **nakonfigurovat jeden Zscaler** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="3e623-165">On the **Zscaler One Configuration** section, click **Configure Zscaler One** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3e623-166">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="3e623-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_configure.png) 

7. <span data-ttu-id="3e623-168">V okně prohlížeče jiný web Přihlaste se k vaší Zscaler jedné lokalitě společnosti jako správce.</span><span class="sxs-lookup"><span data-stu-id="3e623-168">In a different web browser window, log in to your Zscaler One company site as an administrator.</span></span>

8. <span data-ttu-id="3e623-169">V nabídce v horní části, klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="3e623-169">In the menu on the top, click **Administration**.</span></span>
   
    <span data-ttu-id="3e623-170">![Správa](./media/active-directory-saas-zscaler-one-tutorial/ic800206.png "správy")</span><span class="sxs-lookup"><span data-stu-id="3e623-170">![Administration](./media/active-directory-saas-zscaler-one-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="3e623-171">V části **spravovat správci & role**, klikněte na tlačítko **Správa uživatelů a ověřování**.</span><span class="sxs-lookup"><span data-stu-id="3e623-171">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="3e623-172">![Správa uživatelů a ověřování](./media/active-directory-saas-zscaler-one-tutorial/ic800207.png "správu uživatelů a ověřování")</span><span class="sxs-lookup"><span data-stu-id="3e623-172">![Manage Users & Authentication](./media/active-directory-saas-zscaler-one-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="3e623-173">V **zvolte možnosti ověřování pro vaši organizaci** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3e623-173">In the **Choose Authentication Options for your Organization** section, perform the following steps:</span></span>   
                
    <span data-ttu-id="3e623-174">![Ověřování](./media/active-directory-saas-zscaler-one-tutorial/ic800208.png "ověřování")</span><span class="sxs-lookup"><span data-stu-id="3e623-174">![Authentication](./media/active-directory-saas-zscaler-one-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="3e623-175">a.</span><span class="sxs-lookup"><span data-stu-id="3e623-175">a.</span></span> <span data-ttu-id="3e623-176">Vyberte **ověření pomocí SAML Single Sign-On**.</span><span class="sxs-lookup"><span data-stu-id="3e623-176">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="3e623-177">b.</span><span class="sxs-lookup"><span data-stu-id="3e623-177">b.</span></span> <span data-ttu-id="3e623-178">Klikněte na tlačítko **konfigurace SAML jeden přihlašování parametrů**.</span><span class="sxs-lookup"><span data-stu-id="3e623-178">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="3e623-179">Na **konfigurace SAML jeden přihlašování parametry** dialogové okno stránky, proveďte následující kroky a pak klikněte na **provést**</span><span class="sxs-lookup"><span data-stu-id="3e623-179">On the **Configure SAML Single Sign-On Parameters** dialog page, perform the following steps, and then click **Done**</span></span>

    <span data-ttu-id="3e623-180">![Jednotné přihlašování](./media/active-directory-saas-zscaler-one-tutorial/ic800209.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="3e623-180">![Single Sign-On](./media/active-directory-saas-zscaler-one-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="3e623-181">a.</span><span class="sxs-lookup"><span data-stu-id="3e623-181">a.</span></span> <span data-ttu-id="3e623-182">Vložení **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali z portálu Azure do **URL SAML portálu, ke které se odesílají uživatele pro ověřování** textové pole.</span><span class="sxs-lookup"><span data-stu-id="3e623-182">Paste the **SAML Single Sign-On Service URL** value, which you have copied from the Azure portal into the **URL of the SAML Portal to which users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="3e623-183">b.</span><span class="sxs-lookup"><span data-stu-id="3e623-183">b.</span></span> <span data-ttu-id="3e623-184">V **atribut obsahující přihlašovací jméno** textovému poli, typ **NameID**.</span><span class="sxs-lookup"><span data-stu-id="3e623-184">In the **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="3e623-185">c.</span><span class="sxs-lookup"><span data-stu-id="3e623-185">c.</span></span> <span data-ttu-id="3e623-186">Chcete-li nahrát stažený certifikát, klikněte na tlačítko **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="3e623-186">To upload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="3e623-187">d.</span><span class="sxs-lookup"><span data-stu-id="3e623-187">d.</span></span> <span data-ttu-id="3e623-188">Vyberte **zapnout automatické zřizování SAML**.</span><span class="sxs-lookup"><span data-stu-id="3e623-188">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="3e623-189">Na **konfigurace ověřování uživatele** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3e623-189">On the **Configure User Authentication** dialog page, perform the following steps:</span></span>

    <span data-ttu-id="3e623-190">![Správa](./media/active-directory-saas-zscaler-one-tutorial/ic800210.png "správy")</span><span class="sxs-lookup"><span data-stu-id="3e623-190">![Administration](./media/active-directory-saas-zscaler-one-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="3e623-191">a.</span><span class="sxs-lookup"><span data-stu-id="3e623-191">a.</span></span> <span data-ttu-id="3e623-192">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3e623-192">Click **Save**.</span></span>

    <span data-ttu-id="3e623-193">b.</span><span class="sxs-lookup"><span data-stu-id="3e623-193">b.</span></span> <span data-ttu-id="3e623-194">Klikněte na tlačítko **aktivovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="3e623-194">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="3e623-195">Konfigurace nastavení proxy serveru</span><span class="sxs-lookup"><span data-stu-id="3e623-195">Configuring proxy settings</span></span>
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a><span data-ttu-id="3e623-196">Konfigurace nastavení proxy serveru v Internet Exploreru</span><span class="sxs-lookup"><span data-stu-id="3e623-196">To configure the proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="3e623-197">Spustit **aplikace Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="3e623-197">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="3e623-198">Vyberte **Možnosti Internetu** z **nástroje** nabídku pro otevření **Možnosti Internetu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e623-198">Select **Internet options** from the **Tools** menu for open the **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="3e623-199">![Možnosti Internetu](./media/active-directory-saas-zscaler-one-tutorial/ic769492.png "Možnosti Internetu")</span><span class="sxs-lookup"><span data-stu-id="3e623-199">![Internet Options](./media/active-directory-saas-zscaler-one-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="3e623-200">Klikněte **připojení** kartě.</span><span class="sxs-lookup"><span data-stu-id="3e623-200">Click the **Connections** tab.</span></span>   
  
     <span data-ttu-id="3e623-201">![Připojení](./media/active-directory-saas-zscaler-one-tutorial/ic769493.png "připojení")</span><span class="sxs-lookup"><span data-stu-id="3e623-201">![Connections](./media/active-directory-saas-zscaler-one-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="3e623-202">Klikněte na tlačítko **nastavení místní sítě** otevřete **nastavení místní sítě** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e623-202">Click **LAN settings** to open the **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="3e623-203">V části Proxy server proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3e623-203">In the Proxy server section, perform the following steps:</span></span>   
   
    <span data-ttu-id="3e623-204">![Proxy server](./media/active-directory-saas-zscaler-one-tutorial/ic769494.png "Proxy serveru")</span><span class="sxs-lookup"><span data-stu-id="3e623-204">![Proxy server](./media/active-directory-saas-zscaler-one-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="3e623-205">a.</span><span class="sxs-lookup"><span data-stu-id="3e623-205">a.</span></span> <span data-ttu-id="3e623-206">Vyberte **použít proxy server pro síť LAN**.</span><span class="sxs-lookup"><span data-stu-id="3e623-206">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="3e623-207">b.</span><span class="sxs-lookup"><span data-stu-id="3e623-207">b.</span></span> <span data-ttu-id="3e623-208">Do textového pole adresu zadejte **gateway.zscalerone.net**.</span><span class="sxs-lookup"><span data-stu-id="3e623-208">In the Address textbox, type **gateway.zscalerone.net**.</span></span>

    <span data-ttu-id="3e623-209">c.</span><span class="sxs-lookup"><span data-stu-id="3e623-209">c.</span></span> <span data-ttu-id="3e623-210">Do textového pole Port zadejte **80**.</span><span class="sxs-lookup"><span data-stu-id="3e623-210">In the Port textbox, type **80**.</span></span>

    <span data-ttu-id="3e623-211">d.</span><span class="sxs-lookup"><span data-stu-id="3e623-211">d.</span></span> <span data-ttu-id="3e623-212">Vyberte **Nepoužívat proxy server pro místní adresy**.</span><span class="sxs-lookup"><span data-stu-id="3e623-212">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="3e623-213">e.</span><span class="sxs-lookup"><span data-stu-id="3e623-213">e.</span></span> <span data-ttu-id="3e623-214">Klikněte na tlačítko **OK** zavřete **nastavení místní sítě (LAN)** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e623-214">Click **OK** to close the **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="3e623-215">Klikněte na tlačítko **OK** zavřete **Možnosti Internetu** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e623-215">Click **OK** to close the **Internet Options** dialog.</span></span>

> [!TIP]
> <span data-ttu-id="3e623-216">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="3e623-216">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3e623-217">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="3e623-217">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3e623-218">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3e623-218">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3e623-219">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3e623-219">Creating an Azure AD test user</span></span>
<span data-ttu-id="3e623-220">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3e623-220">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="3e623-222">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3e623-222">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3e623-223">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3e623-223">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3e623-225">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3e623-225">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3e623-227">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e623-227">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3e623-229">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3e623-229">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3e623-231">a.</span><span class="sxs-lookup"><span data-stu-id="3e623-231">a.</span></span> <span data-ttu-id="3e623-232">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3e623-232">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3e623-233">b.</span><span class="sxs-lookup"><span data-stu-id="3e623-233">b.</span></span> <span data-ttu-id="3e623-234">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3e623-234">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3e623-235">c.</span><span class="sxs-lookup"><span data-stu-id="3e623-235">c.</span></span> <span data-ttu-id="3e623-236">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="3e623-236">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3e623-237">d.</span><span class="sxs-lookup"><span data-stu-id="3e623-237">d.</span></span> <span data-ttu-id="3e623-238">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3e623-238">Click **Create**.</span></span>
 
### <a name="creating-a-zscaler-one-test-user"></a><span data-ttu-id="3e623-239">Vytváření Zscaler jeden testovací uživatele</span><span class="sxs-lookup"><span data-stu-id="3e623-239">Creating a Zscaler One test user</span></span>

<span data-ttu-id="3e623-240">Povolit uživatelům Azure AD přihlášení k Zscaler jeden, musí být zřízená Zscaler k jednomu.</span><span class="sxs-lookup"><span data-stu-id="3e623-240">To enable Azure AD users to log in to Zscaler One, they must be provisioned to Zscaler One.</span></span> <span data-ttu-id="3e623-241">V případě jedné Zscaler zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="3e623-241">In the case of Zscaler One, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="3e623-242">Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3e623-242">To configure user provisioning, perform the following steps:</span></span>

1. <span data-ttu-id="3e623-243">Přihlaste se k vaší **Zscaler jeden** klienta.</span><span class="sxs-lookup"><span data-stu-id="3e623-243">Log in to your **Zscaler One** tenant.</span></span>

2. <span data-ttu-id="3e623-244">Klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="3e623-244">Click **Administration**.</span></span>   
   
    <span data-ttu-id="3e623-245">![Správa](./media/active-directory-saas-zscaler-one-tutorial/ic781035.png "správy")</span><span class="sxs-lookup"><span data-stu-id="3e623-245">![Administration](./media/active-directory-saas-zscaler-one-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="3e623-246">Klikněte na tlačítko **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="3e623-246">Click **User Management**.</span></span>   
        
     <span data-ttu-id="3e623-247">![Přidat](./media/active-directory-saas-zscaler-one-tutorial/ic781036.png "přidat")</span><span class="sxs-lookup"><span data-stu-id="3e623-247">![Add](./media/active-directory-saas-zscaler-one-tutorial/ic781036.png "Add")</span></span>

4. <span data-ttu-id="3e623-248">V **uživatelé** , klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="3e623-248">In the **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="3e623-249">![Přidat](./media/active-directory-saas-zscaler-one-tutorial/ic781037.png "přidat")</span><span class="sxs-lookup"><span data-stu-id="3e623-249">![Add](./media/active-directory-saas-zscaler-one-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="3e623-250">V části přidat uživatele proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3e623-250">In the Add User section, perform the following steps:</span></span>
        
    <span data-ttu-id="3e623-251">![Přidat uživatele](./media/active-directory-saas-zscaler-one-tutorial/ic781038.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="3e623-251">![Add User](./media/active-directory-saas-zscaler-one-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="3e623-252">a.</span><span class="sxs-lookup"><span data-stu-id="3e623-252">a.</span></span> <span data-ttu-id="3e623-253">Typ **UserID**, **zobrazované uživatelské jméno**, **heslo**, **Potvrdit heslo**a potom vyberte **skupiny** a **oddělení** platný Azure AD účet chcete zřídit.</span><span class="sxs-lookup"><span data-stu-id="3e623-253">Type the **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and the **Department** of a valid Azure AD account you want to provision.</span></span>

    <span data-ttu-id="3e623-254">b.</span><span class="sxs-lookup"><span data-stu-id="3e623-254">b.</span></span> <span data-ttu-id="3e623-255">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3e623-255">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="3e623-256">Další nástroje pro vytvoření účtu Zscaler jeden uživatel nebo rozhraní API poskytovaných Zscaler jeden můžete použít ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3e623-256">You can use any other Zscaler One user account creation tools or APIs provided by Zscaler One to provision Azure AD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3e623-257">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3e623-257">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3e623-258">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Zscaler jeden.</span><span class="sxs-lookup"><span data-stu-id="3e623-258">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Zscaler One.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="3e623-260">**Pokud chcete přiřadit Britta Simon Zscaler jeden, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3e623-260">**To assign Britta Simon to Zscaler One, perform the following steps:**</span></span>

1. <span data-ttu-id="3e623-261">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3e623-261">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3e623-263">V seznamu aplikací vyberte **Zscaler jeden**.</span><span class="sxs-lookup"><span data-stu-id="3e623-263">In the applications list, select **Zscaler One**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_app.png) 

3. <span data-ttu-id="3e623-265">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3e623-265">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="3e623-267">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3e623-267">Click **Add** button.</span></span> <span data-ttu-id="3e623-268">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e623-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="3e623-270">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3e623-270">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3e623-271">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e623-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3e623-272">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3e623-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3e623-273">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3e623-273">Testing single sign-on</span></span>

<span data-ttu-id="3e623-274">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3e623-274">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3e623-275">Když kliknete na dlaždici Zscaler jeden na přístupovém panelu, jste měli získat automaticky přihlášení k Zscaler jedné aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e623-275">When you click the Zscaler One tile in the Access Panel, you should get automatically signed-on to your Zscaler One application.</span></span>
<span data-ttu-id="3e623-276">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3e623-276">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3e623-277">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3e623-277">Additional resources</span></span>

* [<span data-ttu-id="3e623-278">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3e623-278">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3e623-279">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3e623-279">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_203.png

