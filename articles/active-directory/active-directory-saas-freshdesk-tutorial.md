---
title: 'Kurz: Azure Active Directory integrace s FreshDesk | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a FreshDesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: f4b47e345a40b64f69ad8a4618564662b4a6c879
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a><span data-ttu-id="77415-103">Kurz: Azure Active Directory integrace s FreshDesk</span><span class="sxs-lookup"><span data-stu-id="77415-103">Tutorial: Azure Active Directory integration with FreshDesk</span></span>

<span data-ttu-id="77415-104">V tomto kurzu zjistěte, jak integrovat FreshDesk s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="77415-104">In this tutorial, you learn how to integrate FreshDesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="77415-105">Integrace FreshDesk s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="77415-105">Integrating FreshDesk with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="77415-106">Můžete řídit ve službě Azure AD, který má přístup k FreshDesk</span><span class="sxs-lookup"><span data-stu-id="77415-106">You can control in Azure AD who has access to FreshDesk</span></span>
- <span data-ttu-id="77415-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k FreshDesk (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="77415-107">You can enable your users to automatically get signed-on to FreshDesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="77415-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="77415-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="77415-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="77415-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="77415-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="77415-110">Prerequisites</span></span>

<span data-ttu-id="77415-111">Konfigurace integrace Azure AD s FreshDesk, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="77415-111">To configure Azure AD integration with FreshDesk, you need the following items:</span></span>

- <span data-ttu-id="77415-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="77415-112">An Azure AD subscription</span></span>
- <span data-ttu-id="77415-113">FreshDesk jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="77415-113">A FreshDesk single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="77415-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="77415-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="77415-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="77415-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="77415-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="77415-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="77415-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="77415-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="77415-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="77415-118">Scenario description</span></span>
<span data-ttu-id="77415-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="77415-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="77415-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="77415-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="77415-121">Přidání FreshDesk z Galerie</span><span class="sxs-lookup"><span data-stu-id="77415-121">Adding FreshDesk from the gallery</span></span>
2. <span data-ttu-id="77415-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="77415-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-freshdesk-from-the-gallery"></a><span data-ttu-id="77415-123">Přidání FreshDesk z Galerie</span><span class="sxs-lookup"><span data-stu-id="77415-123">Adding FreshDesk from the gallery</span></span>
<span data-ttu-id="77415-124">Při konfiguraci integrace FreshDesk do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS FreshDesk z galerie.</span><span class="sxs-lookup"><span data-stu-id="77415-124">To configure the integration of FreshDesk into Azure AD, you need to add FreshDesk from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="77415-125">**Pokud chcete přidat FreshDesk z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="77415-125">**To add FreshDesk from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="77415-126">V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="77415-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="77415-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="77415-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="77415-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="77415-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="77415-131">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="77415-131">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="77415-133">Do vyhledávacího pole zadejte **FreshDesk**.</span><span class="sxs-lookup"><span data-stu-id="77415-133">In the search box, type **FreshDesk**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_search.png)

5. <span data-ttu-id="77415-135">Na panelu výsledků vyberte **FreshDesk**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="77415-135">In the results panel, select **FreshDesk**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="77415-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="77415-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="77415-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s FreshDesk podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="77415-138">In this section, you configure and test Azure AD single sign-on with FreshDesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="77415-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v FreshDesk je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="77415-139">For single sign-on to work, Azure AD needs to know what the counterpart user in FreshDesk is to a user in Azure AD.</span></span> <span data-ttu-id="77415-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v FreshDesk musí navázat.</span><span class="sxs-lookup"><span data-stu-id="77415-140">In other words, a link relationship between an Azure AD user and the related user in FreshDesk needs to be established.</span></span>

<span data-ttu-id="77415-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v FreshDesk.</span><span class="sxs-lookup"><span data-stu-id="77415-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in FreshDesk.</span></span>

<span data-ttu-id="77415-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s FreshDesk, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="77415-142">To configure and test Azure AD single sign-on with FreshDesk, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="77415-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="77415-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="77415-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="77415-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="77415-145">**[Vytvoření zkušebního uživatele FreshDesk](#creating-a-freshdesk-test-user)**  – Pokud chcete mít protějšek Britta Simon v FreshDesk propojeném s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="77415-145">**[Creating a FreshDesk test user](#creating-a-freshdesk-test-user)** - to have a counterpart of Britta Simon in FreshDesk that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="77415-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="77415-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="77415-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="77415-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="77415-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="77415-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="77415-149">V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci FreshDesk.</span><span class="sxs-lookup"><span data-stu-id="77415-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your FreshDesk application.</span></span>

<span data-ttu-id="77415-150">**Ke konfiguraci Azure AD jednotné přihlašování s FreshDesk, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="77415-150">**To configure Azure AD single sign-on with FreshDesk, perform the following steps:**</span></span>

1. <span data-ttu-id="77415-151">Na portálu Azure Management portal na **FreshDesk** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="77415-151">In the Azure Management portal, on the **FreshDesk** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="77415-153">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="77415-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_samlbase.png)

3. <span data-ttu-id="77415-155">Na **FreshDesk domény a adresy URL** části, zadejte **přihlašovací adresa URL** jako: `https://<tenant-name>.freshdesk.com` nebo jakoukoli jinou hodnotu Freshdesk navrhl.</span><span class="sxs-lookup"><span data-stu-id="77415-155">On the **FreshDesk Domain and URLs** section, please enter the **Sign-on URL** as: `https://<tenant-name>.freshdesk.com` or any other value Freshdesk has suggested.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_url.png)

    > [!NOTE] 
    > <span data-ttu-id="77415-157">Upozorňujeme, že se nejedná skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="77415-157">Please note that this is not the real value.</span></span> <span data-ttu-id="77415-158">Budete muset aktualizovat hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="77415-158">You have to update the value with the actual Sign-on URL.</span></span> <span data-ttu-id="77415-159">Obraťte se na [tým podpory FreshDesk klienta](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="77415-159">Contact [FreshDesk Client support team](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) to get this value.</span></span>  

4. <span data-ttu-id="77415-160">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikát** a potom uložte certifikát v počítači.</span><span class="sxs-lookup"><span data-stu-id="77415-160">On the **SAML Signing Certificate** section, click **Certificate** and then save the certificate on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_certificate.png) 

5. <span data-ttu-id="77415-162">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="77415-162">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="77415-164">Na **FreshDesk konfigurace** klikněte na tlačítko **konfigurace FreshDesk** otevřete konfigurovat přihlašování okno.</span><span class="sxs-lookup"><span data-stu-id="77415-164">On the **FreshDesk Configuration** section, click **Configure FreshDesk** to open Configure sign-on window.</span></span> <span data-ttu-id="77415-165">Zkopírujte SAML jeden přihlašování adresa URL služby a adresa URL Sign-Out z **Stručná referenční příručka** části.</span><span class="sxs-lookup"><span data-stu-id="77415-165">Copy the SAML Single Sign-On Service URL and Sign-Out URL from the **Quick Reference** section.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_configure.png)

7. <span data-ttu-id="77415-167">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Freshdesk.</span><span class="sxs-lookup"><span data-stu-id="77415-167">In a different web browser window, log into your Freshdesk company site as an administrator.</span></span>

8. <span data-ttu-id="77415-168">V nabídce v horní části, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="77415-168">In the menu on the top, click **Admin**.</span></span>
   
   <span data-ttu-id="77415-169">![Správce](./media/active-directory-saas-freshdesk-tutorial/IC776768.png "správce")</span><span class="sxs-lookup"><span data-stu-id="77415-169">![Admin](./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Admin")</span></span>

9. <span data-ttu-id="77415-170">V **obecné nastavení** , klikněte na **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="77415-170">In the **General Settings** tab, click **Security**.</span></span>
   
   <span data-ttu-id="77415-171">![Zabezpečení](./media/active-directory-saas-freshdesk-tutorial/IC776769.png "zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="77415-171">![Security](./media/active-directory-saas-freshdesk-tutorial/IC776769.png "Security")</span></span>

10. <span data-ttu-id="77415-172">V **zabezpečení** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="77415-172">In the **Security** section, perform the following steps:</span></span>
   
    <span data-ttu-id="77415-173">![Jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/IC776770.png "jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="77415-173">![Single Sign On](./media/active-directory-saas-freshdesk-tutorial/IC776770.png "Single Sign On")</span></span>
   
    <span data-ttu-id="77415-174">a.</span><span class="sxs-lookup"><span data-stu-id="77415-174">a.</span></span> <span data-ttu-id="77415-175">Pro **jednotné přihlašování na (SSO)**, vyberte **na**.</span><span class="sxs-lookup"><span data-stu-id="77415-175">For **Single Sign On (SSO)**, select **On**.</span></span>

    <span data-ttu-id="77415-176">b.</span><span class="sxs-lookup"><span data-stu-id="77415-176">b.</span></span> <span data-ttu-id="77415-177">Vyberte **jednotné přihlašování SAML**.</span><span class="sxs-lookup"><span data-stu-id="77415-177">Select **SAML SSO**.</span></span>

    <span data-ttu-id="77415-178">c.</span><span class="sxs-lookup"><span data-stu-id="77415-178">c.</span></span> <span data-ttu-id="77415-179">Typ **SAML jeden přihlašování adresa URL služby** jste zkopírovali z portálu Azure do **SAML přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="77415-179">Type the **SAML Single Sign-On Service URL** you copied from Azure portal into the **SAML Login URL** textbox.</span></span>

    <span data-ttu-id="77415-180">d.</span><span class="sxs-lookup"><span data-stu-id="77415-180">d.</span></span> <span data-ttu-id="77415-181">Typ **Sign-Out URL** jste zkopírovali z portálu Azure do **adresy URL odhlašovací** textové pole.</span><span class="sxs-lookup"><span data-stu-id="77415-181">Type the **Sign-Out URL**  you copied from Azure portal into the **Logout URL** textbox.</span></span>

    <span data-ttu-id="77415-182">e.</span><span class="sxs-lookup"><span data-stu-id="77415-182">e.</span></span> <span data-ttu-id="77415-183">Kopírování **kryptografický otisk** hodnotu z certifikát stažený z portálu Azure a vložte ji do **otisků certifikátu zabezpečení** textové pole.</span><span class="sxs-lookup"><span data-stu-id="77415-183">Copy the **Thumbprint** value from the downloaded certificate from Azure portal and paste it into the **Security Certificate Fingerprint** textbox.</span></span>  
 
    >[!TIP]
    ><span data-ttu-id="77415-184">Další podrobnosti najdete v tématu [jak načíst hodnoty kryptografického otisku certifikátu](http://youtu.be/YKQF266SAxI).</span><span class="sxs-lookup"><span data-stu-id="77415-184">For more details, see [How to retrieve a certificate's thumbprint value](http://youtu.be/YKQF266SAxI).</span></span> 
    
    <span data-ttu-id="77415-185">f.</span><span class="sxs-lookup"><span data-stu-id="77415-185">f.</span></span> <span data-ttu-id="77415-186">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="77415-186">Click **Save**.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="77415-187">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="77415-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="77415-188">Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="77415-188">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="77415-190">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="77415-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="77415-191">V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="77415-191">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="77415-193">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="77415-193">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="77415-195">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="77415-195">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="77415-197">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="77415-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="77415-199">a.</span><span class="sxs-lookup"><span data-stu-id="77415-199">a.</span></span> <span data-ttu-id="77415-200">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="77415-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="77415-201">b.</span><span class="sxs-lookup"><span data-stu-id="77415-201">b.</span></span> <span data-ttu-id="77415-202">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="77415-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="77415-203">c.</span><span class="sxs-lookup"><span data-stu-id="77415-203">c.</span></span> <span data-ttu-id="77415-204">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="77415-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="77415-205">d.</span><span class="sxs-lookup"><span data-stu-id="77415-205">d.</span></span> <span data-ttu-id="77415-206">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="77415-206">Click **Create**.</span></span>
 
### <a name="creating-a-freshdesk-test-user"></a><span data-ttu-id="77415-207">Vytvoření zkušebního uživatele FreshDesk</span><span class="sxs-lookup"><span data-stu-id="77415-207">Creating a FreshDesk test user</span></span>

<span data-ttu-id="77415-208">Pokud chcete povolit uživatelům Azure AD přihlášení do FreshDesk, musí být zřízená do FreshDesk.</span><span class="sxs-lookup"><span data-stu-id="77415-208">In order to enable Azure AD users to log into FreshDesk, they must be provisioned into FreshDesk.</span></span>  
<span data-ttu-id="77415-209">V případě FreshDesk zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="77415-209">In the case of FreshDesk, provisioning is a manual task.</span></span>

<span data-ttu-id="77415-210">**Ke zřízení uživatelských účtů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="77415-210">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="77415-211">Přihlaste se k vaší **Freshdesk** klienta.</span><span class="sxs-lookup"><span data-stu-id="77415-211">Log in to your **Freshdesk** tenant.</span></span>
2. <span data-ttu-id="77415-212">V nabídce v horní části, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="77415-212">In the menu on the top, click **Admin**.</span></span>
   
   <span data-ttu-id="77415-213">![Správce](./media/active-directory-saas-freshdesk-tutorial/IC776772.png "správce")</span><span class="sxs-lookup"><span data-stu-id="77415-213">![Admin](./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Admin")</span></span>

3. <span data-ttu-id="77415-214">V **obecné nastavení** , klikněte na **agenti**.</span><span class="sxs-lookup"><span data-stu-id="77415-214">In the **General Settings** tab, click **Agents**.</span></span>
   
   <span data-ttu-id="77415-215">![Agenti](./media/active-directory-saas-freshdesk-tutorial/IC776773.png "agentů")</span><span class="sxs-lookup"><span data-stu-id="77415-215">![Agents](./media/active-directory-saas-freshdesk-tutorial/IC776773.png "Agents")</span></span>

4. <span data-ttu-id="77415-216">Klikněte na tlačítko **nového agenta**.</span><span class="sxs-lookup"><span data-stu-id="77415-216">Click **New Agent**.</span></span>
   
    <span data-ttu-id="77415-217">![Nový Agent](./media/active-directory-saas-freshdesk-tutorial/IC776774.png "nového agenta.")</span><span class="sxs-lookup"><span data-stu-id="77415-217">![New Agent](./media/active-directory-saas-freshdesk-tutorial/IC776774.png "New Agent")</span></span>

5. <span data-ttu-id="77415-218">V dialogovém okně informace o agentovi proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="77415-218">On the Agent Information dialog, perform the following steps:</span></span>
   
   <span data-ttu-id="77415-219">![Informace o agentovi](./media/active-directory-saas-freshdesk-tutorial/IC776775.png "informace o agentovi")</span><span class="sxs-lookup"><span data-stu-id="77415-219">![Agent Information](./media/active-directory-saas-freshdesk-tutorial/IC776775.png "Agent Information")</span></span>
   
   <span data-ttu-id="77415-220">a.</span><span class="sxs-lookup"><span data-stu-id="77415-220">a.</span></span> <span data-ttu-id="77415-221">V **úplný název** textovému poli, zadejte název účtu Azure AD, které chcete zřídit.</span><span class="sxs-lookup"><span data-stu-id="77415-221">In the **Full Name** textbox, type the name of the Azure AD account you want to provision.</span></span>

   <span data-ttu-id="77415-222">b.</span><span class="sxs-lookup"><span data-stu-id="77415-222">b.</span></span> <span data-ttu-id="77415-223">V **e-mailu** textovému poli, typ Azure AD e-mailovou adresu chcete zřídit účet Azure AD.</span><span class="sxs-lookup"><span data-stu-id="77415-223">In the **Email** textbox, type the Azure AD email address of the Azure AD account you want to provision.</span></span>

   <span data-ttu-id="77415-224">c.</span><span class="sxs-lookup"><span data-stu-id="77415-224">c.</span></span> <span data-ttu-id="77415-225">V **název** textovému poli, zadejte název chcete zřídit účet Azure AD.</span><span class="sxs-lookup"><span data-stu-id="77415-225">In the **Title** textbox, type the title of the Azure AD account you want to provision.</span></span>

   <span data-ttu-id="77415-226">d.</span><span class="sxs-lookup"><span data-stu-id="77415-226">d.</span></span> <span data-ttu-id="77415-227">Vyberte **agenti role**a potom klikněte na **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="77415-227">Select **Agents role**, and then click **Assign**.</span></span>
       
   <span data-ttu-id="77415-228">e.</span><span class="sxs-lookup"><span data-stu-id="77415-228">e.</span></span> <span data-ttu-id="77415-229">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="77415-229">Click **Save**.</span></span>     
   
    >[!NOTE]
    ><span data-ttu-id="77415-230">Držitel účtu Azure AD dostane e-mail, který obsahuje odkaz pro potvrzení účtu předtím, než se aktivuje.</span><span class="sxs-lookup"><span data-stu-id="77415-230">The Azure AD account holder will get an email that includes a link to confirm the account before it is activated.</span></span> 
    > 
    
    >[!NOTE]
    ><span data-ttu-id="77415-231">Můžete použít všechny ostatní Freshdesk uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Freshdesk zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="77415-231">You can use any other Freshdesk user account creation tools or APIs provided by Freshdesk to provision AAD user accounts.</span></span> <span data-ttu-id="77415-232">k FreshDesk.</span><span class="sxs-lookup"><span data-stu-id="77415-232">to FreshDesk.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="77415-233">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="77415-233">Assigning the Azure AD test user</span></span>

<span data-ttu-id="77415-234">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup k poli.</span><span class="sxs-lookup"><span data-stu-id="77415-234">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Box.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="77415-236">**Pokud chcete přiřadit Britta Simon FreshDesk, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="77415-236">**To assign Britta Simon to FreshDesk, perform the following steps:**</span></span>

1. <span data-ttu-id="77415-237">V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="77415-237">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="77415-239">V seznamu aplikací vyberte **FreshDesk**.</span><span class="sxs-lookup"><span data-stu-id="77415-239">In the applications list, select **FreshDesk**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_app.png) 

3. <span data-ttu-id="77415-241">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="77415-241">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="77415-243">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="77415-243">Click **Add** button.</span></span> <span data-ttu-id="77415-244">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="77415-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="77415-246">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="77415-246">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="77415-247">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="77415-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="77415-248">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="77415-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="77415-249">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="77415-249">Testing single sign-on</span></span>

<span data-ttu-id="77415-250">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="77415-250">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="77415-251">Když kliknete na dlaždici FreshDesk na přístupovém panelu, měli byste obdržet přihlašovací stránku k získání přihlášení k aplikaci FreshDesk.</span><span class="sxs-lookup"><span data-stu-id="77415-251">When you click the FreshDesk tile in the Access Panel, you should get login page to get signed-on to your FreshDesk application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="77415-252">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="77415-252">Additional resources</span></span>

* [<span data-ttu-id="77415-253">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="77415-253">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="77415-254">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="77415-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_203.png

