---
title: 'Kurz: Azure Active Directory integrace s SAP Business ByDesign | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SAP Business ByDesign."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 82938920-33ba-47cb-b141-511b46d19e66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: ab76a0ac1ef954efd3c66e6f565514b889ed9444
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a><span data-ttu-id="99fc0-103">Kurz: Integrace Azure Active Directory se službou SAP Business ByDesign</span><span class="sxs-lookup"><span data-stu-id="99fc0-103">Tutorial: Azure Active Directory integration with SAP Business ByDesign</span></span>

<span data-ttu-id="99fc0-104">V tomto kurzu zjistěte, jak integrovat SAP Business ByDesign s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="99fc0-104">In this tutorial, you learn how to integrate SAP Business ByDesign with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="99fc0-105">Integrace SAP Business ByDesign s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="99fc0-105">Integrating SAP Business ByDesign with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="99fc0-106">Můžete ovládat ve službě Azure AD, který má přístup k SAP Business ByDesign.</span><span class="sxs-lookup"><span data-stu-id="99fc0-106">You can control in Azure AD who has access to SAP Business ByDesign.</span></span>
- <span data-ttu-id="99fc0-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k prostředí SAP Business ByDesign (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="99fc0-107">You can enable your users to automatically get signed-on to SAP Business ByDesign (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="99fc0-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="99fc0-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="99fc0-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="99fc0-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="99fc0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="99fc0-110">Prerequisites</span></span>

<span data-ttu-id="99fc0-111">Konfigurace integrace Azure AD s SAP Business ByDesign, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="99fc0-111">To configure Azure AD integration with SAP Business ByDesign, you need the following items:</span></span>

- <span data-ttu-id="99fc0-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="99fc0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="99fc0-113">SAP Business ByDesign jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="99fc0-113">An SAP Business ByDesign single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="99fc0-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="99fc0-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="99fc0-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="99fc0-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="99fc0-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="99fc0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="99fc0-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="99fc0-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="99fc0-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="99fc0-118">Scenario description</span></span>
<span data-ttu-id="99fc0-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="99fc0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="99fc0-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="99fc0-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="99fc0-121">Přidání SAP Business ByDesign z Galerie</span><span class="sxs-lookup"><span data-stu-id="99fc0-121">Adding SAP Business ByDesign from the gallery</span></span>
2. <span data-ttu-id="99fc0-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="99fc0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sap-business-bydesign-from-the-gallery"></a><span data-ttu-id="99fc0-123">Přidání SAP Business ByDesign z Galerie</span><span class="sxs-lookup"><span data-stu-id="99fc0-123">Adding SAP Business ByDesign from the gallery</span></span>
<span data-ttu-id="99fc0-124">Konfigurace integrace ByDesign obchodní SAP do Azure AD, potřebujete přidat SAP Business ByDesign z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="99fc0-124">To configure the integration of SAP Business ByDesign into Azure AD, you need to add SAP Business ByDesign from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="99fc0-125">**Pokud chcete přidat SAP Business ByDesign z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="99fc0-125">**To add SAP Business ByDesign from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="99fc0-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="99fc0-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="99fc0-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="99fc0-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="99fc0-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="99fc0-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="99fc0-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="99fc0-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="99fc0-133">Do vyhledávacího pole zadejte **SAP Business ByDesign**, vyberte **SAP Business ByDesign** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="99fc0-133">In the search box, type **SAP Business ByDesign**, select **SAP Business ByDesign** from result panel then click **Add** button to add the application.</span></span>

    ![SAP Business ByDesign v seznamu výsledků](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="99fc0-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="99fc0-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="99fc0-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s SAP Business ByDesign podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="99fc0-136">In this section, you configure and test Azure AD single sign-on with SAP Business ByDesign based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="99fc0-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v SAP Business ByDesign je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="99fc0-137">For single sign-on to work, Azure AD needs to know what the counterpart user in SAP Business ByDesign is to a user in Azure AD.</span></span> <span data-ttu-id="99fc0-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v SAP Business ByDesign musí navázat.</span><span class="sxs-lookup"><span data-stu-id="99fc0-138">In other words, a link relationship between an Azure AD user and the related user in SAP Business ByDesign needs to be established.</span></span>

<span data-ttu-id="99fc0-139">V SAP Business ByDesign, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="99fc0-139">In SAP Business ByDesign, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="99fc0-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s SAP Business ByDesign, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="99fc0-140">To configure and test Azure AD single sign-on with SAP Business ByDesign, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="99fc0-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="99fc0-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="99fc0-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="99fc0-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="99fc0-143">**[Vytvořit testovací uživatele s SAP Business ByDesign](#create-an-sap-business-bydesign-test-user)**  – Pokud chcete mít protějšek Britta Simon v ByDesign SAP Business, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="99fc0-143">**[Create an SAP Business ByDesign test user](#create-an-sap-business-bydesign-test-user)** - to have a counterpart of Britta Simon in SAP Business ByDesign that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="99fc0-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="99fc0-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="99fc0-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="99fc0-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="99fc0-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="99fc0-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="99fc0-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci SAP Business ByDesign.</span><span class="sxs-lookup"><span data-stu-id="99fc0-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SAP Business ByDesign application.</span></span>

<span data-ttu-id="99fc0-148">**Ke konfiguraci Azure AD jednotné přihlašování s SAP Business ByDesign, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="99fc0-148">**To configure Azure AD single sign-on with SAP Business ByDesign, perform the following steps:**</span></span>

1. <span data-ttu-id="99fc0-149">Na portálu Azure na **SAP Business ByDesign** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="99fc0-149">In the Azure portal, on the **SAP Business ByDesign** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="99fc0-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="99fc0-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_samlbase.png)

3. <span data-ttu-id="99fc0-153">Na **SAP Business ByDesign domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="99fc0-153">On the **SAP Business ByDesign Domain and URLs** section, perform the following steps:</span></span>

    ![SAP Business ByDesign domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_url.png)

    <span data-ttu-id="99fc0-155">a.</span><span class="sxs-lookup"><span data-stu-id="99fc0-155">a.</span></span> <span data-ttu-id="99fc0-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<servername>.sapbydesign.com`</span><span class="sxs-lookup"><span data-stu-id="99fc0-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<servername>.sapbydesign.com`</span></span>

    <span data-ttu-id="99fc0-157">b.</span><span class="sxs-lookup"><span data-stu-id="99fc0-157">b.</span></span> <span data-ttu-id="99fc0-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<servername>.sapbydesign.com`</span><span class="sxs-lookup"><span data-stu-id="99fc0-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<servername>.sapbydesign.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="99fc0-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="99fc0-159">These values are not real.</span></span> <span data-ttu-id="99fc0-160">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="99fc0-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="99fc0-161">Obraťte se na [tým podpory SAP Business ByDesign klienta](https://www.sap.com/products/cloud-analytics.support.html) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="99fc0-161">Contact [SAP Business ByDesign Client support team](https://www.sap.com/products/cloud-analytics.support.html) to get these values.</span></span>

4. <span data-ttu-id="99fc0-162">Na **uživatelské atributy** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="99fc0-162">On the **User Attributes** section, perform the following steps:</span></span>

    ![SAP Business ByDesign atribut části](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_attribute.png)
    
    <span data-ttu-id="99fc0-164">a.</span><span class="sxs-lookup"><span data-stu-id="99fc0-164">a.</span></span> <span data-ttu-id="99fc0-165">V **uživatelský identifikátor** seznamu, vyberte **ExtractMailPrefix()** funkce.</span><span class="sxs-lookup"><span data-stu-id="99fc0-165">In **User Identifier** list, select the **ExtractMailPrefix()** function.</span></span>
    
    <span data-ttu-id="99fc0-166">b.</span><span class="sxs-lookup"><span data-stu-id="99fc0-166">b.</span></span> <span data-ttu-id="99fc0-167">Z **e-mailu** vyberte atribut uživatele, kterou chcete použít týkající se vaší implementace.</span><span class="sxs-lookup"><span data-stu-id="99fc0-167">From the **Mail** list, select the user attribute you want to use for your implementation.</span></span> <span data-ttu-id="99fc0-168">Například pokud chcete použít EmployeeID jako jedinečný identifikátor uživatele a hodnota atributu jsou uloženy v ExtensionAttribute2, pak vyberte user.extensionattribute2.</span><span class="sxs-lookup"><span data-stu-id="99fc0-168">For example, if you want to use the EmployeeID as unique user identifier and you have stored the attribute value in the ExtensionAttribute2, then select user.extensionattribute2.</span></span>     

5. <span data-ttu-id="99fc0-169">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="99fc0-169">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_certificate.png) 

6. <span data-ttu-id="99fc0-171">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="99fc0-171">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="99fc0-173">Na **SAP Business ByDesign konfigurace** klikněte na tlačítko **konfigurace SAP Business ByDesign** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="99fc0-173">On the **SAP Business ByDesign Configuration** section, click **Configure SAP Business ByDesign** to open **Configure sign-on** window.</span></span> <span data-ttu-id="99fc0-174">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="99fc0-174">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![SAP Business ByDesign konfigurace](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_configure.png) 

8. <span data-ttu-id="99fc0-176">Pokud chcete získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="99fc0-176">To get SSO configured for your application, perform the following steps:</span></span>
   
    <span data-ttu-id="99fc0-177">a.</span><span class="sxs-lookup"><span data-stu-id="99fc0-177">a.</span></span> <span data-ttu-id="99fc0-178">Přihlásit se k portálu SAP Business ByDesign s právy správce.</span><span class="sxs-lookup"><span data-stu-id="99fc0-178">Sign on to your SAP Business ByDesign portal with administrator rights.</span></span>
   
    <span data-ttu-id="99fc0-179">b.</span><span class="sxs-lookup"><span data-stu-id="99fc0-179">b.</span></span> <span data-ttu-id="99fc0-180">Přejděte na **aplikace a běžné úlohy správy uživatele** a klikněte na tlačítko **zprostředkovatele Identity** kartě.</span><span class="sxs-lookup"><span data-stu-id="99fc0-180">Navigate to **Application and User Management Common Task** and click the **Identity Provider** tab.</span></span>
   
    <span data-ttu-id="99fc0-181">c.</span><span class="sxs-lookup"><span data-stu-id="99fc0-181">c.</span></span> <span data-ttu-id="99fc0-182">Klikněte na tlačítko **nového poskytovatele Identity** a vyberte soubor XML metadat, kterou jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="99fc0-182">Click **New Identity Provider** and select the metadata XML file that you have downloaded from the Azure portal.</span></span> <span data-ttu-id="99fc0-183">Importováním metadata systém automaticky nahrává dodejka certifikát a certifikát pro šifrování.</span><span class="sxs-lookup"><span data-stu-id="99fc0-183">By importing the metadata, the system automatically uploads the required signature certificate and encryption certificate.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)
   
    <span data-ttu-id="99fc0-185">d.</span><span class="sxs-lookup"><span data-stu-id="99fc0-185">d.</span></span> <span data-ttu-id="99fc0-186">Zahrnout **adresa URL služby příjemce Assertion** do žádost SAML, vyberte **zahrnují Assertion příjemce adresa URL služby**.</span><span class="sxs-lookup"><span data-stu-id="99fc0-186">To include the **Assertion Consumer Service URL** into the SAML request, select **Include Assertion Consumer Service URL**.</span></span>
   
    <span data-ttu-id="99fc0-187">e.</span><span class="sxs-lookup"><span data-stu-id="99fc0-187">e.</span></span> <span data-ttu-id="99fc0-188">Klikněte na tlačítko **aktivovat jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="99fc0-188">Click **Activate Single Sign-On**.</span></span>
   
    <span data-ttu-id="99fc0-189">f.</span><span class="sxs-lookup"><span data-stu-id="99fc0-189">f.</span></span> <span data-ttu-id="99fc0-190">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="99fc0-190">Save your changes.</span></span>
   
    <span data-ttu-id="99fc0-191">g.</span><span class="sxs-lookup"><span data-stu-id="99fc0-191">g.</span></span> <span data-ttu-id="99fc0-192">Klikněte **Moje systému** kartě.</span><span class="sxs-lookup"><span data-stu-id="99fc0-192">Click the **My System** tab.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)
   
    <span data-ttu-id="99fc0-194">h.</span><span class="sxs-lookup"><span data-stu-id="99fc0-194">h.</span></span> <span data-ttu-id="99fc0-195">Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure do **Azure AD adresa URL přihlašování** textové pole.</span><span class="sxs-lookup"><span data-stu-id="99fc0-195">Paste **SAML Single Sign-On Service URL**, which you have copied from the Azure portal it into the **Azure AD Sign On URL** textbox.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)
   
    <span data-ttu-id="99fc0-197">i.</span><span class="sxs-lookup"><span data-stu-id="99fc0-197">i.</span></span> <span data-ttu-id="99fc0-198">Zadejte, zda zaměstnanec ručně vybrat mezi přihlásit pomocí ID uživatele a heslo nebo jednotného přihlašování k výběrem **výběr zprostředkovatele Identity ruční**.</span><span class="sxs-lookup"><span data-stu-id="99fc0-198">Specify whether the employee can manually choose between logging on with user ID and password or SSO by selecting **Manual Identity Provider Selection**.</span></span>
   
    <span data-ttu-id="99fc0-199">j.</span><span class="sxs-lookup"><span data-stu-id="99fc0-199">j.</span></span> <span data-ttu-id="99fc0-200">V **jednotného přihlašování k adrese URL** části, zadejte adresu URL, který se má použít pro zaměstnance k přihlašování do systému.</span><span class="sxs-lookup"><span data-stu-id="99fc0-200">In the **SSO URL** section, specify the URL that should be used by the employee to logon to the system.</span></span> 
    <span data-ttu-id="99fc0-201">V adresu URL odeslání zaměstnanec rozevíracího seznamu můžete zvolit z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="99fc0-201">In the URL Sent to Employee dropdown list, you can choose between the following options:</span></span>
   
    <span data-ttu-id="99fc0-202">**Adresa URL jednotného přihlašování**</span><span class="sxs-lookup"><span data-stu-id="99fc0-202">**Non-SSO URL**</span></span>
   
    <span data-ttu-id="99fc0-203">Systém odešle jenom adresu URL normální systému zaměstnanci.</span><span class="sxs-lookup"><span data-stu-id="99fc0-203">The system sends only the normal system URL to the employee.</span></span> <span data-ttu-id="99fc0-204">Zaměstnanec nemůže přihlášení pomocí jednotného přihlašování a musí používat heslo nebo místo toho certifikátu.</span><span class="sxs-lookup"><span data-stu-id="99fc0-204">The employee cannot log on using SSO, and must use password or certificate instead.</span></span>
   
    <span data-ttu-id="99fc0-205">**ADRESA URL JEDNOTNÉHO PŘIHLAŠOVÁNÍ**</span><span class="sxs-lookup"><span data-stu-id="99fc0-205">**SSO URL**</span></span> 
   
    <span data-ttu-id="99fc0-206">Systém odešle zaměstnanec pouze adresy URL pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="99fc0-206">The system sends only the SSO URL to the employee.</span></span> <span data-ttu-id="99fc0-207">Zaměstnanec může přihlásit pomocí jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="99fc0-207">The employee can log on using SSO.</span></span> <span data-ttu-id="99fc0-208">Prostřednictvím rozšíření IdP přesměruje požadavek na ověření.</span><span class="sxs-lookup"><span data-stu-id="99fc0-208">Authentication request is redirected through the IdP.</span></span>
   
    <span data-ttu-id="99fc0-209">**Automatický výběr**</span><span class="sxs-lookup"><span data-stu-id="99fc0-209">**Automatic Selection**</span></span>
   
    <span data-ttu-id="99fc0-210">Pokud jednotného přihlašování není aktivní, odešle systému zaměstnanec adresu URL normální systému.</span><span class="sxs-lookup"><span data-stu-id="99fc0-210">If SSO is not active, the system sends the normal system URL to the employee.</span></span> <span data-ttu-id="99fc0-211">Pokud je aktivní jednotné přihlašování, systém kontroluje, zda zaměstnanec má heslo.</span><span class="sxs-lookup"><span data-stu-id="99fc0-211">If SSO is active, the system checks whether the employee has a password.</span></span> <span data-ttu-id="99fc0-212">Pokud heslo je k dispozici, jednotného přihlašování k URL a adresy URL bez jednotného přihlašování k odešlou do jednotlivých zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="99fc0-212">If a password is available, both SSO URL and Non-SSO URL are sent to the employee.</span></span> <span data-ttu-id="99fc0-213">Ale pokud zaměstnanec, nemá žádné heslo, pouze adresy URL pro jednotné přihlašování odešle zaměstnanci.</span><span class="sxs-lookup"><span data-stu-id="99fc0-213">However, if the employee has no password, only the SSO URL is sent to the employee.</span></span>
   
    <span data-ttu-id="99fc0-214">kB.</span><span class="sxs-lookup"><span data-stu-id="99fc0-214">k.</span></span> <span data-ttu-id="99fc0-215">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="99fc0-215">Save your changes.</span></span>

> [!TIP]
> <span data-ttu-id="99fc0-216">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="99fc0-216">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="99fc0-217">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="99fc0-217">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="99fc0-218">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="99fc0-218">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="99fc0-219">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="99fc0-219">Create an Azure AD test user</span></span>

<span data-ttu-id="99fc0-220">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="99fc0-220">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="99fc0-222">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="99fc0-222">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="99fc0-223">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="99fc0-223">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="99fc0-225">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="99fc0-225">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="99fc0-227">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="99fc0-227">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="99fc0-229">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="99fc0-229">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png)

    <span data-ttu-id="99fc0-231">a.</span><span class="sxs-lookup"><span data-stu-id="99fc0-231">a.</span></span> <span data-ttu-id="99fc0-232">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="99fc0-232">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="99fc0-233">b.</span><span class="sxs-lookup"><span data-stu-id="99fc0-233">b.</span></span> <span data-ttu-id="99fc0-234">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="99fc0-234">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="99fc0-235">c.</span><span class="sxs-lookup"><span data-stu-id="99fc0-235">c.</span></span> <span data-ttu-id="99fc0-236">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="99fc0-236">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="99fc0-237">d.</span><span class="sxs-lookup"><span data-stu-id="99fc0-237">d.</span></span> <span data-ttu-id="99fc0-238">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="99fc0-238">Click **Create**.</span></span>
 
### <a name="create-an-sap-business-bydesign-test-user"></a><span data-ttu-id="99fc0-239">Vytvořit uživatele s SAP Business ByDesign testu</span><span class="sxs-lookup"><span data-stu-id="99fc0-239">Create an SAP Business ByDesign test user</span></span>

<span data-ttu-id="99fc0-240">V této části vytvoříte uživatele volal Britta Simon v SAP Business ByDesign.</span><span class="sxs-lookup"><span data-stu-id="99fc0-240">In this section, you create a user called Britta Simon in SAP Business ByDesign.</span></span> <span data-ttu-id="99fc0-241">Spojte se s [tým podpory SAP Business ByDesign klienta](https://www.sap.com/products/cloud-analytics.support.html) přidat uživatele do SAP Business ByDesign platformy.</span><span class="sxs-lookup"><span data-stu-id="99fc0-241">Please work with [SAP Business ByDesign Client support team](https://www.sap.com/products/cloud-analytics.support.html) to add the users in the SAP Business ByDesign platform.</span></span> 

> [!NOTE]
> <span data-ttu-id="99fc0-242">Ujistěte se, že NameID hodnotu shodovat s pole uživatelské jméno v platformě SAP Business ByDesign.</span><span class="sxs-lookup"><span data-stu-id="99fc0-242">Please make sure that NameID value should match with the username field in the SAP Business ByDesign platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="99fc0-243">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="99fc0-243">Assign the Azure AD test user</span></span>

<span data-ttu-id="99fc0-244">V této části povolíte Britta Simon používat tak, že udělíte přístup k SAP Business ByDesign Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="99fc0-244">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SAP Business ByDesign.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="99fc0-246">**Pokud chcete přiřadit Britta Simon SAP Business ByDesign, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="99fc0-246">**To assign Britta Simon to SAP Business ByDesign, perform the following steps:**</span></span>

1. <span data-ttu-id="99fc0-247">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="99fc0-247">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="99fc0-249">V seznamu aplikací vyberte **SAP Business ByDesign**.</span><span class="sxs-lookup"><span data-stu-id="99fc0-249">In the applications list, select **SAP Business ByDesign**.</span></span>

    ![V seznamu aplikací na SAP Business ByDesign odkaz](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_app.png)  

3. <span data-ttu-id="99fc0-251">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="99fc0-251">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="99fc0-253">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="99fc0-253">Click **Add** button.</span></span> <span data-ttu-id="99fc0-254">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="99fc0-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="99fc0-256">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="99fc0-256">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="99fc0-257">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="99fc0-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="99fc0-258">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="99fc0-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="99fc0-259">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="99fc0-259">Test single sign-on</span></span>

<span data-ttu-id="99fc0-260">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="99fc0-260">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="99fc0-261">Když kliknete na dlaždici SAP Business ByDesign na přístupovém panelu, jste měli získat automaticky přihlášení k SAP Business ByDesign aplikace.</span><span class="sxs-lookup"><span data-stu-id="99fc0-261">When you click the SAP Business ByDesign tile in the Access Panel, you should get automatically signed-on to your SAP Business ByDesign application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="99fc0-262">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="99fc0-262">Additional resources</span></span>

* [<span data-ttu-id="99fc0-263">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="99fc0-263">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="99fc0-264">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="99fc0-264">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png

