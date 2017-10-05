---
title: 'Kurz: Azure Active Directory integrace s Work.com | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Work.com."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 7cfec8e9ac12d43095483696a15c0580776d3114
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a><span data-ttu-id="fe352-103">Kurz: Azure Active Directory integrace s Work.com</span><span class="sxs-lookup"><span data-stu-id="fe352-103">Tutorial: Azure Active Directory integration with Work.com</span></span>

<span data-ttu-id="fe352-104">V tomto kurzu zjistěte, jak integrovat Work.com s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fe352-104">In this tutorial, you learn how to integrate Work.com with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fe352-105">Integrace Work.com s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="fe352-105">Integrating Work.com with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="fe352-106">Můžete řídit ve službě Azure AD, který má přístup k Work.com</span><span class="sxs-lookup"><span data-stu-id="fe352-106">You can control in Azure AD who has access to Work.com</span></span>
- <span data-ttu-id="fe352-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Work.com (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="fe352-107">You can enable your users to automatically get signed-on to Work.com (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fe352-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fe352-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="fe352-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fe352-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fe352-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fe352-110">Prerequisites</span></span>

<span data-ttu-id="fe352-111">Konfigurace integrace Azure AD s Work.com, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="fe352-111">To configure Azure AD integration with Work.com, you need the following items:</span></span>

- <span data-ttu-id="fe352-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="fe352-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fe352-113">Work.com jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="fe352-113">A Work.com single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fe352-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="fe352-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fe352-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="fe352-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fe352-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="fe352-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fe352-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fe352-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fe352-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="fe352-118">Scenario description</span></span>
<span data-ttu-id="fe352-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="fe352-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fe352-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="fe352-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fe352-121">Přidat Work.com z Galerie</span><span class="sxs-lookup"><span data-stu-id="fe352-121">Add Work.com from the gallery</span></span>
2. <span data-ttu-id="fe352-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="fe352-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-workcom-from-the-gallery"></a><span data-ttu-id="fe352-123">Přidat Work.com z Galerie</span><span class="sxs-lookup"><span data-stu-id="fe352-123">Add Work.com from the gallery</span></span>
<span data-ttu-id="fe352-124">Při konfiguraci integrace Work.com do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Work.com z galerie.</span><span class="sxs-lookup"><span data-stu-id="fe352-124">To configure the integration of Work.com into Azure AD, you need to add Work.com from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="fe352-125">**Pokud chcete přidat Work.com z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="fe352-125">**To add Work.com from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="fe352-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="fe352-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="fe352-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="fe352-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="fe352-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="fe352-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="fe352-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fe352-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="fe352-133">Do vyhledávacího pole zadejte **Work.com**, vyberte **Work.com** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fe352-133">In the search box, type **Work.com**, select **Work.com** from results panel then click **Add** button to add the application.</span></span>

    ![Přidat z Galerie](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="fe352-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="fe352-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="fe352-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Work.com podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="fe352-136">In this section, you configure and test Azure AD single sign-on with Work.com based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="fe352-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Work.com je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fe352-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Work.com is to a user in Azure AD.</span></span> <span data-ttu-id="fe352-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Work.com musí navázat.</span><span class="sxs-lookup"><span data-stu-id="fe352-138">In other words, a link relationship between an Azure AD user and the related user in Work.com needs to be established.</span></span>

<span data-ttu-id="fe352-139">V Work.com, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="fe352-139">In Work.com, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="fe352-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Work.com, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="fe352-140">To configure and test Azure AD single sign-on with Work.com, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="fe352-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="fe352-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="fe352-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fe352-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fe352-143">**[Vytvoření zkušebního uživatele Work.com](#create-a-workcom-test-user)**  – Pokud chcete mít protějšek Britta Simon v Work.com propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="fe352-143">**[Create a Work.com test user](#create-a-workcom-test-user)** - to have a counterpart of Britta Simon in Work.com that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="fe352-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="fe352-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fe352-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fe352-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="fe352-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="fe352-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="fe352-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Work.com.</span><span class="sxs-lookup"><span data-stu-id="fe352-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Work.com application.</span></span>

>[!NOTE]
><span data-ttu-id="fe352-148">Pokud chcete konfigurovat jednotné přihlašování, budete muset nastavit vlastní název domény Work.com ještě.</span><span class="sxs-lookup"><span data-stu-id="fe352-148">To configure single sign-on, you need to setup a custom Work.com domain name yet.</span></span> <span data-ttu-id="fe352-149">Musíte definovat alespoň název domény, testovací název domény a nasadíte ho do celé organizace.</span><span class="sxs-lookup"><span data-stu-id="fe352-149">You need to define at least a domain name, test your domain name, and deploy it to your entire organization.</span></span>

<span data-ttu-id="fe352-150">**Ke konfiguraci Azure AD jednotné přihlašování s Work.com, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="fe352-150">**To configure Azure AD single sign-on with Work.com, perform the following steps:**</span></span>

1. <span data-ttu-id="fe352-151">Na portálu Azure na **Work.com** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="fe352-151">In the Azure portal, on the **Work.com** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="fe352-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="fe352-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Na základě SAML přihlášení](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_samlbase.png)

3. <span data-ttu-id="fe352-155">Na **Work.com domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fe352-155">On the **Work.com Domain and URLs** section, perform the following:</span></span>

    ![Část Work.com domény a adresy URL](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_url.png)

    <span data-ttu-id="fe352-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`http://<companyname>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="fe352-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `http://<companyname>.my.salesforce.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="fe352-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="fe352-158">This value is not real.</span></span> <span data-ttu-id="fe352-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="fe352-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="fe352-160">Obraťte se na [tým podpory Work.com klienta](https://help.salesforce.com/articleView?id=000159855&type=3) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="fe352-160">Contact [Work.com Client support team](https://help.salesforce.com/articleView?id=000159855&type=3) to get this value.</span></span> 

4. <span data-ttu-id="fe352-161">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="fe352-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Certifikát pro podpis SAML části](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_certificate.png) 

5. <span data-ttu-id="fe352-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fe352-163">Click **Save** button.</span></span>

    ![Tlačítko Uložit](./media/active-directory-saas-work-com-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="fe352-165">Na **Work.com konfigurace** klikněte na tlačítko **konfigurace Work.com** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="fe352-165">On the **Work.com Configuration** section, click **Configure Work.com** to open **Configure sign-on** window.</span></span> <span data-ttu-id="fe352-166">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="fe352-166">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Work.com konfigurační oddíl](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_configure.png) 
7. <span data-ttu-id="fe352-168">Přihlaste se ke klientovi Work.com jako správce.</span><span class="sxs-lookup"><span data-stu-id="fe352-168">Log in to your Work.com tenant as administrator.</span></span>

8. <span data-ttu-id="fe352-169">Přejděte na **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="fe352-169">Go to **Setup**.</span></span>
   
    <span data-ttu-id="fe352-170">![Instalační program](./media/active-directory-saas-work-com-tutorial/ic794108.png "instalační program")</span><span class="sxs-lookup"><span data-stu-id="fe352-170">![Setup](./media/active-directory-saas-work-com-tutorial/ic794108.png "Setup")</span></span>

9. <span data-ttu-id="fe352-171">V levém navigačním podokně v **Správa** klikněte na tlačítko **Správa domén** rozbalte související část, a potom klikněte na **Moje domény** otevřete **Moje domény** stránky.</span><span class="sxs-lookup"><span data-stu-id="fe352-171">On the left navigation pane, in the **Administer** section, click **Domain Management** to expand the related section, and then click **My Domain** to open the **My Domain** page.</span></span> 
   
    <span data-ttu-id="fe352-172">![Moje doména](./media/active-directory-saas-work-com-tutorial/ic767825.png "Moje doména")</span><span class="sxs-lookup"><span data-stu-id="fe352-172">![My Domain](./media/active-directory-saas-work-com-tutorial/ic767825.png "My Domain")</span></span>

10. <span data-ttu-id="fe352-173">Chcete-li ověřit, zda vaše doména byla nastavena správně, ujistěte se, že je v "**krok 4 nasazení uživatelům**" a zkontrolovat vaše "**Moje nastavení domény**".</span><span class="sxs-lookup"><span data-stu-id="fe352-173">To verify that your domain has been set up correctly, make sure that it is in “**Step 4 Deployed to Users**” and review your “**My Domain Settings**”.</span></span>
   
    <span data-ttu-id="fe352-174">![Domény uživatele nejsou nasazené](./media/active-directory-saas-work-com-tutorial/ic784377.png "doménou nasazení pro uživatele")</span><span class="sxs-lookup"><span data-stu-id="fe352-174">![Domain Deployed to User](./media/active-directory-saas-work-com-tutorial/ic784377.png "Domain Deployed to User")</span></span>

11. <span data-ttu-id="fe352-175">Přihlaste se ke klientovi Work.com.</span><span class="sxs-lookup"><span data-stu-id="fe352-175">Log in to your Work.com tenant.</span></span>

12. <span data-ttu-id="fe352-176">Přejděte na **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="fe352-176">Go to **Setup**.</span></span>
    
    <span data-ttu-id="fe352-177">![Instalační program](./media/active-directory-saas-work-com-tutorial/ic794108.png "instalační program")</span><span class="sxs-lookup"><span data-stu-id="fe352-177">![Setup](./media/active-directory-saas-work-com-tutorial/ic794108.png "Setup")</span></span>

13. <span data-ttu-id="fe352-178">Rozbalte **ovládacích prvků zabezpečení** nabídce a pak klikněte na tlačítko **nastavení jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="fe352-178">Expand the **Security Controls** menu, and then click **Single Sign-On Settings**.</span></span>
    
    <span data-ttu-id="fe352-179">![Jednotné přihlašování v nastavení](./media/active-directory-saas-work-com-tutorial/ic794113.png "jednotné přihlašování v nastavení")</span><span class="sxs-lookup"><span data-stu-id="fe352-179">![Single Sign-On Settings](./media/active-directory-saas-work-com-tutorial/ic794113.png "Single Sign-On Settings")</span></span>

14. <span data-ttu-id="fe352-180">Na **nastavení jednotného přihlašování** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fe352-180">On the **Single Sign-On Settings** dialog page, perform the following steps:</span></span>
    
    <span data-ttu-id="fe352-181">![Povolit SAML](./media/active-directory-saas-work-com-tutorial/ic781026.png "povoleno SAML")</span><span class="sxs-lookup"><span data-stu-id="fe352-181">![SAML Enabled](./media/active-directory-saas-work-com-tutorial/ic781026.png "SAML Enabled")</span></span>
    
    <span data-ttu-id="fe352-182">a.</span><span class="sxs-lookup"><span data-stu-id="fe352-182">a.</span></span> <span data-ttu-id="fe352-183">Vyberte **povoleno SAML**.</span><span class="sxs-lookup"><span data-stu-id="fe352-183">Select **SAML Enabled**.</span></span>
    
    <span data-ttu-id="fe352-184">b.</span><span class="sxs-lookup"><span data-stu-id="fe352-184">b.</span></span> <span data-ttu-id="fe352-185">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="fe352-185">Click **New**.</span></span>

15. <span data-ttu-id="fe352-186">V **SAML jeden nastavení přihlášení** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fe352-186">In the **SAML Single Sign-On Settings** section, perform the following steps:</span></span>
    
    <span data-ttu-id="fe352-187">![SAML jeden přihlašování nastavení](./media/active-directory-saas-work-com-tutorial/ic794114.png "SAML jeden přihlašování nastavení")</span><span class="sxs-lookup"><span data-stu-id="fe352-187">![SAML Single Sign-On Setting](./media/active-directory-saas-work-com-tutorial/ic794114.png "SAML Single Sign-On Setting")</span></span>
    
    <span data-ttu-id="fe352-188">a.</span><span class="sxs-lookup"><span data-stu-id="fe352-188">a.</span></span> <span data-ttu-id="fe352-189">V **název** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="fe352-189">In the **Name** textbox, type a name for your configuration.</span></span>  
       
    > [!NOTE]
    > <span data-ttu-id="fe352-190">Poskytuje hodnotu pro **název** automaticky vyplnit **název rozhraní API** textové pole.</span><span class="sxs-lookup"><span data-stu-id="fe352-190">Providing a value for **Name** does automatically populate the **API Name** textbox.</span></span>
    
    <span data-ttu-id="fe352-191">b.</span><span class="sxs-lookup"><span data-stu-id="fe352-191">b.</span></span> <span data-ttu-id="fe352-192">V **vystavitele** textovému poli, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fe352-192">In **Issuer** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="fe352-193">c.</span><span class="sxs-lookup"><span data-stu-id="fe352-193">c.</span></span> <span data-ttu-id="fe352-194">Chcete-li nahrát certifikát stažený z portálu Azure, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="fe352-194">To upload the downloaded certificate from Azure portal, click **Browse**.</span></span>
    
    <span data-ttu-id="fe352-195">d.</span><span class="sxs-lookup"><span data-stu-id="fe352-195">d.</span></span> <span data-ttu-id="fe352-196">V **Entity Id** textovému poli, typ `https://salesforce-work.com`.</span><span class="sxs-lookup"><span data-stu-id="fe352-196">In the **Entity Id** textbox, type `https://salesforce-work.com`.</span></span>
    
    <span data-ttu-id="fe352-197">e.</span><span class="sxs-lookup"><span data-stu-id="fe352-197">e.</span></span> <span data-ttu-id="fe352-198">Jako **typ Identity SAML**, vyberte **kontrolní výraz obsahuje ID federace z objektu uživatele**.</span><span class="sxs-lookup"><span data-stu-id="fe352-198">As **SAML Identity Type**, select **Assertion contains the Federation ID from the User object**.</span></span>
    
    <span data-ttu-id="fe352-199">f.</span><span class="sxs-lookup"><span data-stu-id="fe352-199">f.</span></span> <span data-ttu-id="fe352-200">Jako **umístění Identity SAML**, vyberte **identita je v elementu NameIdentfier příkaz subjektu**.</span><span class="sxs-lookup"><span data-stu-id="fe352-200">As **SAML Identity Location**, select **Identity is in the NameIdentfier element of the Subject statement**.</span></span>
    
    <span data-ttu-id="fe352-201">g.</span><span class="sxs-lookup"><span data-stu-id="fe352-201">g.</span></span> <span data-ttu-id="fe352-202">V **adresu URL pro přihlášení zprostředkovatele Identity** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fe352-202">In **Identity Provider Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="fe352-203">h.</span><span class="sxs-lookup"><span data-stu-id="fe352-203">h.</span></span> <span data-ttu-id="fe352-204">V **adresa URL odhlašovací zprostředkovatele Identity** textovému poli, vložte hodnotu **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fe352-204">In **Identity Provider Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="fe352-205">i.</span><span class="sxs-lookup"><span data-stu-id="fe352-205">i.</span></span> <span data-ttu-id="fe352-206">Jako **zprostředkovatele iniciované žádosti vazby služby**, vyberte **HTTP Post**.</span><span class="sxs-lookup"><span data-stu-id="fe352-206">As **Service Provider Initiated Request Binding**, select **HTTP Post**.</span></span>
    
    <span data-ttu-id="fe352-207">j.</span><span class="sxs-lookup"><span data-stu-id="fe352-207">j.</span></span> <span data-ttu-id="fe352-208">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fe352-208">Click **Save**.</span></span>

16. <span data-ttu-id="fe352-209">Portál classic Work.com, v levém navigačním podokně klikněte na tlačítko **Správa domén** rozbalte související část, a potom klikněte na **Moje domény** otevřete **Moje domény** stránky.</span><span class="sxs-lookup"><span data-stu-id="fe352-209">In your Work.com classic portal, on the left navigation pane, click **Domain Management** to expand the related section, and then click **My Domain** to open the **My Domain** page.</span></span> 
    
    <span data-ttu-id="fe352-210">![Moje doména](./media/active-directory-saas-work-com-tutorial/ic794115.png "Moje doména")</span><span class="sxs-lookup"><span data-stu-id="fe352-210">![My Domain](./media/active-directory-saas-work-com-tutorial/ic794115.png "My Domain")</span></span>

17. <span data-ttu-id="fe352-211">Na **Moje domény** stránky v **Branding přihlašovací stránky** klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="fe352-211">On the **My Domain** page, in the **Login Page Branding** section, click **Edit**.</span></span>
    
    <span data-ttu-id="fe352-212">![Branding přihlašovací stránky](./media/active-directory-saas-work-com-tutorial/ic767826.png "Branding přihlašovací stránky")</span><span class="sxs-lookup"><span data-stu-id="fe352-212">![Login Page Branding](./media/active-directory-saas-work-com-tutorial/ic767826.png "Login Page Branding")</span></span>

14. <span data-ttu-id="fe352-213">Na **Branding přihlašovací stránky** stránky v **ověřovací služby** tématu, názvu vaší **nastavení jednotného přihlašování SAML** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="fe352-213">On the **Login Page Branding** page, in the **Authentication Service** section, the name of your **SAML SSO Settings** is displayed.</span></span> <span data-ttu-id="fe352-214">Vyberte ji a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fe352-214">Select it, and then click **Save**.</span></span>
    
    <span data-ttu-id="fe352-215">![Branding přihlašovací stránky](./media/active-directory-saas-work-com-tutorial/ic784366.png "Branding přihlašovací stránky")</span><span class="sxs-lookup"><span data-stu-id="fe352-215">![Login Page Branding](./media/active-directory-saas-work-com-tutorial/ic784366.png "Login Page Branding")</span></span>

> [!TIP]
> <span data-ttu-id="fe352-216">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="fe352-216">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="fe352-217">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="fe352-217">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="fe352-218">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fe352-218">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="fe352-219">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="fe352-219">Create an Azure AD test user</span></span>
<span data-ttu-id="fe352-220">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fe352-220">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="fe352-222">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="fe352-222">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="fe352-223">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="fe352-223">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-work-com-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="fe352-225">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="fe352-225">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Uživatelé a skupiny -> všichni uživatelé](./media/active-directory-saas-work-com-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="fe352-227">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fe352-227">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Přidat](./media/active-directory-saas-work-com-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fe352-229">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fe352-229">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Dialogové okno stránka uživatel](./media/active-directory-saas-work-com-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="fe352-231">a.</span><span class="sxs-lookup"><span data-stu-id="fe352-231">a.</span></span> <span data-ttu-id="fe352-232">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fe352-232">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fe352-233">b.</span><span class="sxs-lookup"><span data-stu-id="fe352-233">b.</span></span> <span data-ttu-id="fe352-234">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="fe352-234">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="fe352-235">c.</span><span class="sxs-lookup"><span data-stu-id="fe352-235">c.</span></span> <span data-ttu-id="fe352-236">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="fe352-236">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="fe352-237">d.</span><span class="sxs-lookup"><span data-stu-id="fe352-237">d.</span></span> <span data-ttu-id="fe352-238">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fe352-238">Click **Create**.</span></span>
 
### <a name="create-a-workcom-test-user"></a><span data-ttu-id="fe352-239">Vytvoření zkušebního uživatele Work.com</span><span class="sxs-lookup"><span data-stu-id="fe352-239">Create a Work.com test user</span></span>
<span data-ttu-id="fe352-240">Uživatelé Azure Active Directory se moct přihlásit musí být zřízená k Work.com.</span><span class="sxs-lookup"><span data-stu-id="fe352-240">For Azure Active Directory users to be able to sign in, they must be provisioned to Work.com.</span></span> <span data-ttu-id="fe352-241">V případě Work.com zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="fe352-241">In the case of Work.com, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="fe352-242">Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fe352-242">To configure user provisioning, perform the following steps:</span></span>
1. <span data-ttu-id="fe352-243">Přihlásit se k serveru vaší společnosti Work.com jako správce.</span><span class="sxs-lookup"><span data-stu-id="fe352-243">Sign on to your Work.com company site as an administrator.</span></span>

2. <span data-ttu-id="fe352-244">Přejděte na **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="fe352-244">Go to **Setup**.</span></span>
   
    <span data-ttu-id="fe352-245">![Instalační program](./media/active-directory-saas-work-com-tutorial/IC794108.png "instalační program")</span><span class="sxs-lookup"><span data-stu-id="fe352-245">![Setup](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")</span></span>
3. <span data-ttu-id="fe352-246">Přejděte na **Správa uživatelů \> uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="fe352-246">Go to **Manage Users \> Users**.</span></span>
   
    <span data-ttu-id="fe352-247">![Správa uživatelů](./media/active-directory-saas-work-com-tutorial/IC784369.png "Správa uživatelů")</span><span class="sxs-lookup"><span data-stu-id="fe352-247">![Manage Users](./media/active-directory-saas-work-com-tutorial/IC784369.png "Manage Users")</span></span>

4. <span data-ttu-id="fe352-248">Klikněte na tlačítko **nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="fe352-248">Click **New User**.</span></span>
   
    <span data-ttu-id="fe352-249">![Všichni uživatelé](./media/active-directory-saas-work-com-tutorial/IC794117.png "všichni uživatelé")</span><span class="sxs-lookup"><span data-stu-id="fe352-249">![All Users](./media/active-directory-saas-work-com-tutorial/IC794117.png "All Users")</span></span>

5. <span data-ttu-id="fe352-250">V části Upravit uživatele, proveďte následující kroky, v atributech platný Azure AD účtu chcete mají být zahrnuty do související textových polí:</span><span class="sxs-lookup"><span data-stu-id="fe352-250">In the User Edit section, perform the following steps, in attributes of a valid Azure AD account you want to provision into the related textboxes:</span></span>
   
    <span data-ttu-id="fe352-251">![Úprava uživatele](./media/active-directory-saas-work-com-tutorial/ic794118.png "Úprava uživatele")</span><span class="sxs-lookup"><span data-stu-id="fe352-251">![User Edit](./media/active-directory-saas-work-com-tutorial/ic794118.png "User Edit")</span></span>
   
    <span data-ttu-id="fe352-252">a.</span><span class="sxs-lookup"><span data-stu-id="fe352-252">a.</span></span> <span data-ttu-id="fe352-253">V **křestní jméno** textovému poli, typ **křestní jméno** uživatele **Britta**.</span><span class="sxs-lookup"><span data-stu-id="fe352-253">In the **First Name** textbox, type the **first name** of the user **Britta**.</span></span>
    
    <span data-ttu-id="fe352-254">b.</span><span class="sxs-lookup"><span data-stu-id="fe352-254">b.</span></span> <span data-ttu-id="fe352-255">V **příjmení** textovému poli, typ **příjmení** uživatele **Simon**.</span><span class="sxs-lookup"><span data-stu-id="fe352-255">In the **Last Name** textbox, type the **last name** of the user **Simon**.</span></span>
    
    <span data-ttu-id="fe352-256">c.</span><span class="sxs-lookup"><span data-stu-id="fe352-256">c.</span></span> <span data-ttu-id="fe352-257">V **Alias** textovému poli, typ **název** uživatele **BrittaS**.</span><span class="sxs-lookup"><span data-stu-id="fe352-257">In the **Alias** textbox, type the **name** of the user **BrittaS**.</span></span>
    
    <span data-ttu-id="fe352-258">d.</span><span class="sxs-lookup"><span data-stu-id="fe352-258">d.</span></span> <span data-ttu-id="fe352-259">V **e-mailu** textovému poli, typ **e-mailová adresa** uživatele  **Brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="fe352-259">In the **Email** textbox, type the **email address** of user **Brittasimon@contoso.com**.</span></span>
    
    <span data-ttu-id="fe352-260">e.</span><span class="sxs-lookup"><span data-stu-id="fe352-260">e.</span></span> <span data-ttu-id="fe352-261">V **uživatelské jméno** textové pole, zadejte uživatelské jméno uživatele jako  **Brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="fe352-261">In the **User Name** textbox, type a user name of user like **Brittasimon@contoso.com**.</span></span>
    
    <span data-ttu-id="fe352-262">f.</span><span class="sxs-lookup"><span data-stu-id="fe352-262">f.</span></span> <span data-ttu-id="fe352-263">V **Přezdívka** textovému poli, zadejte **Přezdívka** uživatele **Simon**.</span><span class="sxs-lookup"><span data-stu-id="fe352-263">In the **Nick Name** textbox, type a **nick name** of user **Simon**.</span></span>
    
    <span data-ttu-id="fe352-264">g.</span><span class="sxs-lookup"><span data-stu-id="fe352-264">g.</span></span> <span data-ttu-id="fe352-265">Vyberte **Role**, **uživatelské licence pro**, a **profil**.</span><span class="sxs-lookup"><span data-stu-id="fe352-265">Select **Role**, **User License**, and **Profile**.</span></span>
    
    <span data-ttu-id="fe352-266">h.</span><span class="sxs-lookup"><span data-stu-id="fe352-266">h.</span></span> <span data-ttu-id="fe352-267">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fe352-267">Click **Save**.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="fe352-268">Držitel účtu Azure AD dostane e-mail zahrnutím odkazu pro potvrzení účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="fe352-268">The Azure AD account holder will get an email including a link to confirm the account before it becomes active.</span></span>
    > 
    > 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="fe352-269">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="fe352-269">Assign the Azure AD test user</span></span>

<span data-ttu-id="fe352-270">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Work.com.</span><span class="sxs-lookup"><span data-stu-id="fe352-270">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Work.com.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="fe352-272">**Pokud chcete přiřadit Britta Simon Work.com, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="fe352-272">**To assign Britta Simon to Work.com, perform the following steps:**</span></span>

1. <span data-ttu-id="fe352-273">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="fe352-273">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="fe352-275">V seznamu aplikací vyberte **Work.com**.</span><span class="sxs-lookup"><span data-stu-id="fe352-275">In the applications list, select **Work.com**.</span></span>

    ![Work.com v seznamu aplikace](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_app.png) 

3. <span data-ttu-id="fe352-277">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="fe352-277">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="fe352-279">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fe352-279">Click **Add** button.</span></span> <span data-ttu-id="fe352-280">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fe352-280">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="fe352-282">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="fe352-282">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="fe352-283">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fe352-283">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fe352-284">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fe352-284">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="fe352-285">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="fe352-285">Test single sign-on</span></span>

<span data-ttu-id="fe352-286">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="fe352-286">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="fe352-287">Když kliknete na dlaždici Work.com na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Work.com.</span><span class="sxs-lookup"><span data-stu-id="fe352-287">When you click the Work.com tile in the Access Panel, you should get automatically signed-on to your Work.com application.</span></span>
<span data-ttu-id="fe352-288">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fe352-288">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fe352-289">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="fe352-289">Additional resources</span></span>

* [<span data-ttu-id="fe352-290">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fe352-290">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fe352-291">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="fe352-291">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_203.png

