---
title: "Kurz: Azure Active Directory integrace s síti na pracovišti Autotask | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Autotask síti na pracovišti."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a9a7ff71-c389-4169-aafd-d7a505244797
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 45130162271b20860607497ff93c6a668c415233
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-autotask-workplace"></a><span data-ttu-id="3fa9a-103">Kurz: Azure Active Directory integrace s Autotask síti na pracovišti</span><span class="sxs-lookup"><span data-stu-id="3fa9a-103">Tutorial: Azure Active Directory integration with Autotask Workplace</span></span>

<span data-ttu-id="3fa9a-104">V tomto kurzu zjistěte, jak k síti na pracovišti Autotask integraci s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3fa9a-104">In this tutorial, you learn how to integrate Autotask Workplace with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3fa9a-105">Integrace Autotask síti na pracovišti s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3fa9a-105">Integrating Autotask Workplace with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3fa9a-106">Můžete řídit ve službě Azure AD, který má přístup k síti na pracovišti Autotask</span><span class="sxs-lookup"><span data-stu-id="3fa9a-106">You can control in Azure AD who has access to Autotask Workplace</span></span>
- <span data-ttu-id="3fa9a-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k síti na pracovišti Autotask (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3fa9a-107">You can enable your users to automatically get signed-on to Autotask Workplace (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3fa9a-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3fa9a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3fa9a-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3fa9a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3fa9a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3fa9a-110">Prerequisites</span></span>

<span data-ttu-id="3fa9a-111">Konfigurace integrace Azure AD s Autotask síti na pracovišti, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="3fa9a-111">To configure Azure AD integration with Autotask Workplace, you need the following items:</span></span>

- <span data-ttu-id="3fa9a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3fa9a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3fa9a-113">Síti na pracovišti Autotask jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3fa9a-113">An Autotask Workplace single-sign on enabled subscription</span></span>
- <span data-ttu-id="3fa9a-114">Musíte být správce nebo správce super v síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-114">You must be an administrator or super administrator in Workplace.</span></span>
- <span data-ttu-id="3fa9a-115">Musí mít účet správce ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-115">You must have an administrator account in the Azure AD.</span></span>
- <span data-ttu-id="3fa9a-116">Uživatele, kteří budou tuto funkci využít, musí mít účty v síti na pracovišti a Azure AD a jejich e-mailové adresy pro obě se musí shodovat.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-116">The users that will utilize this feature must have accounts within Workplace and the Azure AD, and their email addresses for both must match.</span></span>

> [!NOTE]
> <span data-ttu-id="3fa9a-117">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-117">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3fa9a-118">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3fa9a-118">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3fa9a-119">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-119">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3fa9a-120">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3fa9a-120">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3fa9a-121">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3fa9a-121">Scenario description</span></span>
<span data-ttu-id="3fa9a-122">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-122">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3fa9a-123">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3fa9a-123">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3fa9a-124">Přidání Autotask síti na pracovišti z Galerie</span><span class="sxs-lookup"><span data-stu-id="3fa9a-124">Adding Autotask Workplace from the gallery</span></span>
2. <span data-ttu-id="3fa9a-125">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3fa9a-125">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-autotask-workplace-from-the-gallery"></a><span data-ttu-id="3fa9a-126">Přidání Autotask síti na pracovišti z Galerie</span><span class="sxs-lookup"><span data-stu-id="3fa9a-126">Adding Autotask Workplace from the gallery</span></span>
<span data-ttu-id="3fa9a-127">Při konfiguraci integrace síti na pracovišti Autotask do služby Azure AD potřebujete přidat Autotask síti na pracovišti z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-127">To configure the integration of Autotask Workplace into Azure AD, you need to add Autotask Workplace from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3fa9a-128">**Pokud chcete přidat Autotask síti na pracovišti z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3fa9a-128">**To add Autotask Workplace from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3fa9a-129">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-129">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="3fa9a-131">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-131">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3fa9a-132">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-132">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="3fa9a-134">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-134">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="3fa9a-136">Do vyhledávacího pole zadejte **síti na pracovišti Autotask**, vyberte **síti na pracovišti Autotask** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-136">In the search box, type **Autotask Workplace**, select  **Autotask Workplace**  from result panel then click **Add** button to add the application.</span></span>

    ![Autotask síti na pracovišti v seznamu výsledků](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="3fa9a-138">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3fa9a-138">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="3fa9a-139">V této části nakonfigurujete a testovací Azure AD jednotné přihlašování se Autotask pracovišti podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="3fa9a-139">In this section, you configure and test Azure AD single sign-on with Autotask Workplace based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3fa9a-140">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v síti na pracovišti Autotask je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Autotask Workplace is to a user in Azure AD.</span></span> <span data-ttu-id="3fa9a-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživateli v síti na pracovišti Autotask musí navázat.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-141">In other words, a link relationship between an Azure AD user and the related user in Autotask Workplace needs to be established.</span></span>

<span data-ttu-id="3fa9a-142">V síti na pracovišti Autotask, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-142">In Autotask Workplace, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3fa9a-143">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Autotask síti na pracovišti, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="3fa9a-143">To configure and test Azure AD single sign-on with Autotask Workplace, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3fa9a-144">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-144">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3fa9a-145">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-145">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3fa9a-146">**[Vytvoření zkušebního uživatele síti na pracovišti Autotask](#create-an-autotask-workplace-test-user)**  – Pokud chcete mít protějšek Britta Simon v síti na pracovišti Autotask propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-146">**[Create an Autotask Workplace test user](#create-an-autotask-workplace-test-user)** - to have a counterpart of Britta Simon in Autotask Workplace that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3fa9a-147">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-147">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3fa9a-148">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-148">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="3fa9a-149">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3fa9a-149">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="3fa9a-150">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Autotask síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Autotask Workplace application.</span></span>

<span data-ttu-id="3fa9a-151">**Ke konfiguraci Azure AD jednotné přihlašování s Autotask síti na pracovišti, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3fa9a-151">**To configure Azure AD single sign-on with Autotask Workplace, perform the following steps:**</span></span>

1. <span data-ttu-id="3fa9a-152">Na portálu Azure na **síti na pracovišti Autotask** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-152">In the Azure portal, on the **Autotask Workplace** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="3fa9a-154">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_samlbase.png)

3. <span data-ttu-id="3fa9a-156">Pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu, proveďte následující kroky na **Autotask síti na pracovišti domény a adresy URL** části:</span><span class="sxs-lookup"><span data-stu-id="3fa9a-156">If you wish to configure the application in **IDP** initiated mode, perform the following steps on the **Autotask Workplace Domain and URLs** section:</span></span>

    ![Autotask síti na pracovišti domény a adresy URL jeden přihlašování informace o rozšíření IDP](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_url.png)

    <span data-ttu-id="3fa9a-158">a.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-158">a.</span></span> <span data-ttu-id="3fa9a-159">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="3fa9a-159">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`</span></span>

    <span data-ttu-id="3fa9a-160">b.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-160">b.</span></span> <span data-ttu-id="3fa9a-161">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="3fa9a-161">In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`</span></span>

4. <span data-ttu-id="3fa9a-162">Pokud chcete nakonfigurovat aplikace **SP** initiated režimu, zkontrolujte **zobrazit upřesňující nastavení adresy URL** a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3fa9a-162">If you wish to configure the application in **SP** initiated mode, check **Show advanced URL settings** and perform the following steps:</span></span>

    ![Autotask síti na pracovišti domény a adresy URL jeden přihlašování informace pro poskytovatele služeb](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_url1.png)

    <span data-ttu-id="3fa9a-164">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.awp.autotask.net/loginsso`</span><span class="sxs-lookup"><span data-stu-id="3fa9a-164">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.awp.autotask.net/loginsso`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="3fa9a-165">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-165">These values are not real.</span></span> <span data-ttu-id="3fa9a-166">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-166">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="3fa9a-167">Obraťte se na [tým podpory Autotask pracoviště klienta](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-167">Contact [Autotask Workplace Client support team](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) to get these values.</span></span> 

5. <span data-ttu-id="3fa9a-168">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-168">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_certificate.png) 

6. <span data-ttu-id="3fa9a-170">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-170">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="3fa9a-172">V okně prohlížeče jiný web Přihlaste se k síti na pracovišti Online pomocí přihlašovacích údajů správce.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-172">In a different web browser window, Log in to Workplace Online using the administrator credentials.</span></span>

    >[!Note]
    ><span data-ttu-id="3fa9a-173">Při konfiguraci rozšíření IdP, bude potřeba zadat subdoména.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-173">When configuring the IdP, a subdomain will need to be specified.</span></span> <span data-ttu-id="3fa9a-174">Potvrďte správné subdomény přihlášení k síti na pracovišti Online.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-174">To confirm the correct subdomain, login to Workplace Online.</span></span> <span data-ttu-id="3fa9a-175">Po přihlášení, poznamenejte si poddomény v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-175">Once logged in, make note to the subdomain in the URL.</span></span>
    ><span data-ttu-id="3fa9a-176">Subdoméně je součástí mezi "https://" a ".awp.autotask.net/" a musí být v USA, Evropa, certifikační autority nebo au.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-176">The subdomain is the part between the “https://“ and “.awp.autotask.net/“ and should be us, eu, ca, or au.</span></span>

8. <span data-ttu-id="3fa9a-177">Přejděte na **konfigurace** > **jednotné přihlašování** a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3fa9a-177">Go to **Configuration** > **Single Sign-On** and perform the following steps:</span></span>

    ![Autotask Single Sign-on konfigurace](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskssoconfig1.png)
 
    <span data-ttu-id="3fa9a-179">a.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-179">a.</span></span> <span data-ttu-id="3fa9a-180">Vyberte **Metadata souboru XML** možnost a pak nahrajte **soubor XML s metadaty** stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-180">Select the **XML Metadata File** option, and then upload the **Metadata XML** downloaded from Azure portal.</span></span>

    <span data-ttu-id="3fa9a-181">b.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-181">b.</span></span> <span data-ttu-id="3fa9a-182">Klikněte na tlačítko **povolení jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-182">Click **Enable SSO**.</span></span>
    
    ![Autotask jednotné přihlášení schválit konfigurace](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskssoconfig2.png)

    <span data-ttu-id="3fa9a-184">c.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-184">c.</span></span> <span data-ttu-id="3fa9a-185">Vyberte **potvrzuji, tyto informace jsou správné a důvěřovat této IdP** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-185">Select the **I confirm this information is correct and I trust this IdP** check box.</span></span>

    <span data-ttu-id="3fa9a-186">d.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-186">d.</span></span> <span data-ttu-id="3fa9a-187">Klikněte na tlačítko **schválit**.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-187">Click **Approve**.</span></span>
     
>[!Note]
><span data-ttu-id="3fa9a-188">Pokud potřebujete pomoc s konfigurací Autotask síti na pracovišti, najdete v tématu [tuto stránku](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) získat pomoc s pracovního účtu.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-188">If you require assistance with configuring Autotask Workplace, please see [this page](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) to get assistance with your Workplace account.</span></span>

> [!TIP]
> <span data-ttu-id="3fa9a-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="3fa9a-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3fa9a-190">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3fa9a-191">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3fa9a-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="3fa9a-192">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3fa9a-192">Create an Azure AD test user</span></span>

<span data-ttu-id="3fa9a-193">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="3fa9a-195">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3fa9a-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3fa9a-196">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-196">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="3fa9a-198">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-198">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="3fa9a-200">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-200">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="3fa9a-202">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3fa9a-202">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_04.png)

    <span data-ttu-id="3fa9a-204">a.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-204">a.</span></span> <span data-ttu-id="3fa9a-205">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-205">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3fa9a-206">b.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-206">b.</span></span> <span data-ttu-id="3fa9a-207">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-207">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="3fa9a-208">c.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-208">c.</span></span> <span data-ttu-id="3fa9a-209">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-209">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="3fa9a-210">d.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-210">d.</span></span> <span data-ttu-id="3fa9a-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-211">Click **Create**.</span></span>

### <a name="create-an-autotask-workplace-test-user"></a><span data-ttu-id="3fa9a-212">Vytvoření zkušebního uživatele síti na pracovišti Autotask</span><span class="sxs-lookup"><span data-stu-id="3fa9a-212">Create an Autotask Workplace test user</span></span>

<span data-ttu-id="3fa9a-213">V této části vytvoříte volal Britta Simon v Autotask uživatele.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-213">In this section, you create a user called Britta Simon in Autotask.</span></span> <span data-ttu-id="3fa9a-214">Spojte se s [tým podpory síti na pracovišti Autotask](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) přidání uživatelů v síti na pracovišti Autotask platformy.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-214">Please work with [Autotask Workplace support team](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) to add the users in the Autotask Workplace platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="3fa9a-215">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3fa9a-215">Assign the Azure AD test user</span></span>

<span data-ttu-id="3fa9a-216">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k síti na pracovišti Autotask.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-216">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Autotask Workplace.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="3fa9a-218">**Britta Simon přiřadit k síti na pracovišti Autotask, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3fa9a-218">**To assign Britta Simon to Autotask Workplace, perform the following steps:**</span></span>

1. <span data-ttu-id="3fa9a-219">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-219">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3fa9a-221">V seznamu aplikací vyberte **Autotask síti na pracovišti**.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-221">In the applications list, select **Autotask Workplace**.</span></span>

    ![V seznamu aplikací na odkaz Autotask síti na pracovišti](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_app.png) 

3. <span data-ttu-id="3fa9a-223">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-223">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="3fa9a-225">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-225">Click **Add** button.</span></span> <span data-ttu-id="3fa9a-226">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="3fa9a-228">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-228">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3fa9a-229">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3fa9a-230">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="3fa9a-231">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3fa9a-231">Test single sign-on</span></span>

<span data-ttu-id="3fa9a-232">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-232">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3fa9a-233">Když kliknete na dlaždici Autotask síti na pracovišti na přístupovém panelu, můžete by měl získat automaticky přihlášení k síti na pracovišti Autotask aplikace.</span><span class="sxs-lookup"><span data-stu-id="3fa9a-233">When you click the Autotask Workplace tile in the Access Panel, you should get automatically signed-on to your Autotask Workplace application.</span></span>
<span data-ttu-id="3fa9a-234">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3fa9a-234">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3fa9a-235">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3fa9a-235">Additional resources</span></span>

* [<span data-ttu-id="3fa9a-236">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3fa9a-236">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3fa9a-237">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3fa9a-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_203.png

