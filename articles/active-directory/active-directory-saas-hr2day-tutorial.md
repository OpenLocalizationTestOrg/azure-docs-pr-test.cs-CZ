---
title: 'Kurz: Azure Active Directory integrace s HR2day podle Merces | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a HR2day podle Merces."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 853d08c9-27b1-48d4-b8e7-3705140eb67f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: 77e2fcf9c306259286b15767f3a992510d6d79d8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a><span data-ttu-id="9e9bb-103">Kurz: Azure Active Directory integrace s HR2day podle Merces</span><span class="sxs-lookup"><span data-stu-id="9e9bb-103">Tutorial: Azure Active Directory integration with HR2day by Merces</span></span>

<span data-ttu-id="9e9bb-104">V tomto kurzu zjistěte, jak integrovat HR2day podle Merces s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9e9bb-104">In this tutorial, you learn how to integrate HR2day by Merces with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9e9bb-105">Integrace HR2day podle Merces s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9e9bb-105">Integrating HR2day by Merces with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9e9bb-106">Můžete ovládat ve službě Azure AD, který má přístup k HR2day podle Merces.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-106">You can control in Azure AD who has access to HR2day by Merces.</span></span>
- <span data-ttu-id="9e9bb-107">Můžete povolit uživatelům automaticky získat přihlášení k HR2day podle Merces s účty služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-107">You can enable your users to automatically get signed in to HR2day by Merces with their Azure AD accounts.</span></span>
- <span data-ttu-id="9e9bb-108">Můžete spravovat vaše účty v jednom centrálním místě – portál Azure.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-108">You can manage your accounts in one central location--the Azure portal.</span></span>

<span data-ttu-id="9e9bb-109">Další informace o integraci aplikací SaaS v Azure AD najdete v tématu [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9e9bb-109">For more information about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9e9bb-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9e9bb-110">Prerequisites</span></span>

<span data-ttu-id="9e9bb-111">Konfigurace integrace Azure AD s HR2day podle Merces, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="9e9bb-111">To configure Azure AD integration with HR2day by Merces, you need the following items:</span></span>

- <span data-ttu-id="9e9bb-112">Předplatné služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-112">An Azure AD subscription.</span></span>
- <span data-ttu-id="9e9bb-113">HR2day podle Merces jednotné přihlašování povolené předplatné.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-113">An HR2day by Merces single sign-on enabled subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="9e9bb-114">Nedoporučujeme používat produkčním prostředí pro testování kroky v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-114">We don't recommend using a production environment to test the steps in this tutorial.</span></span>

<span data-ttu-id="9e9bb-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle následujících doporučení:</span><span class="sxs-lookup"><span data-stu-id="9e9bb-115">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="9e9bb-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-116">Don't use your production environment unless it's necessary.</span></span>
- <span data-ttu-id="9e9bb-117">Získání [jeden měsíc bezplatnou zkušební verzi Azure AD](https://azure.microsoft.com/pricing/free-trial/) Pokud ještě nemáte ho.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-117">Get a [one-month free trial of Azure AD](https://azure.microsoft.com/pricing/free-trial/) if you don't already have it.</span></span>  

## <a name="scenario-description"></a><span data-ttu-id="9e9bb-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9e9bb-118">Scenario description</span></span>
<span data-ttu-id="9e9bb-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9e9bb-120">Scénář, který je zde uvedených zahrnuje dva hlavní stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="9e9bb-120">The scenario that's outlined here consists of two main building blocks:</span></span>

1. <span data-ttu-id="9e9bb-121">Přidání HR2day podle Merces z galerie.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-121">Adding HR2day by Merces from the gallery.</span></span>
2. <span data-ttu-id="9e9bb-122">Konfigurace a testování Azure AD jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-122">Configuring and testing Azure AD single sign-on.</span></span>

## <a name="add-hr2day-by-merces-from-the-gallery"></a><span data-ttu-id="9e9bb-123">Přidat HR2day podle Merces z Galerie</span><span class="sxs-lookup"><span data-stu-id="9e9bb-123">Add HR2day by Merces from the gallery</span></span>
<span data-ttu-id="9e9bb-124">Při konfiguraci integrace HR2day podle Merces do služby Azure AD přidáte do seznamu spravovaných aplikací SaaS HR2day podle Merces z galerie.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-124">To configure the integration of HR2day by Merces into Azure AD, add HR2day by Merces from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9e9bb-125">**Pokud chcete přidat HR2day podle Merces z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9e9bb-125">**To add HR2day by Merces from the gallery, take the following steps:**</span></span>

1. <span data-ttu-id="9e9bb-126">V [portál Azure](https://portal.azure.com), na levém navigačním podokně, vyberte **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-126">In the [Azure portal](https://portal.azure.com), on the left navigation pane, select the **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9e9bb-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-128">Go to **Enterprise applications**.</span></span> <span data-ttu-id="9e9bb-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="9e9bb-131">Chcete-li přidat novou aplikaci, vyberte **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-131">To add a new application, select the **New application** button on the top of the dialog box.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="9e9bb-133">Do vyhledávacího pole zadejte **HR2day podle Merces**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-133">In the search box, type **HR2day by Merces**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_search.png)

5. <span data-ttu-id="9e9bb-135">Na panelu výsledků vyberte **HR2day podle Merces**a pak vyberte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-135">In the results panel, select **HR2day by Merces**, and then select the **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="9e9bb-137">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9e9bb-137">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="9e9bb-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s HR2day podle Merces podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="9e9bb-138">In this section, you configure and test Azure AD single sign-on with HR2day by Merces based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9e9bb-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, kdo příslušného uživatele v HR2day podle Merces je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-139">For single sign-on to work, Azure AD needs to know who the counterpart user in HR2day by Merces is to a user in Azure AD.</span></span> <span data-ttu-id="9e9bb-140">Jinými slovy budete muset vytvořit propojení mezi uživatele Azure AD a související uživatelské v HR2day podle Merces.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-140">In other words, you need to establish a link between an Azure AD user and the related user in HR2day by Merces.</span></span>

<span data-ttu-id="9e9bb-141">V HR2day podle Merces přiřadit **uživatelské jméno** ve službě Azure AD **uživatelské jméno** k vytvoření relace.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-141">In HR2day by Merces, assign the **user name** in Azure AD to  **Username** to establish the relationship.</span></span>

<span data-ttu-id="9e9bb-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s HR2day podle Merces, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="9e9bb-142">To configure and test Azure AD single sign-on with HR2day by Merces, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9e9bb-143">[Konfigurovat Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on): Povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-143">[Configure Azure AD single sign-on](#configuring-azure-ad-single-sign-on): Enable your users to use this feature.</span></span>
2. <span data-ttu-id="9e9bb-144">[Vytvořit testovací uživatele Azure AD](#creating-an-azure-ad-test-user): Test Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-144">[Create an Azure AD test user](#creating-an-azure-ad-test-user): Test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9e9bb-145">[Vytvoření HR2day uživatelem Merces testovací](#creating-an-hr2day-by-merces-test-user): vytvoření protějšek Britta Simon v HR2day podle Merces propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-145">[Create an HR2day by Merces test user](#creating-an-hr2day-by-merces-test-user): Create a counterpart of Britta Simon in HR2day by Merces that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9e9bb-146">[Přiřadit testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user): Povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-146">[Assign the Azure AD test user](#assigning-the-azure-ad-test-user): Enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9e9bb-147">[Test jednotného přihlašování](#testing-single-sign-on): Ověřte, zda funguje konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-147">[Test single sign-on](#testing-single-sign-on): Verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="9e9bb-148">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9e9bb-148">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="9e9bb-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v vaše HR2day Merces aplikací.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your HR2day by Merces application.</span></span>

<span data-ttu-id="9e9bb-150">**Ke konfiguraci Azure AD jednotné přihlašování s HR2day podle Merces, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9e9bb-150">**To configure Azure AD single sign-on with HR2day by Merces, take the following steps:**</span></span>

1. <span data-ttu-id="9e9bb-151">Na portálu Azure na **HR2day podle Merces** stránky integrace aplikací, vyberte **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-151">In the Azure portal, on the **HR2day by Merces** application integration page, select **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9e9bb-153">Pro povolení jednotného přihlašování, v **jednotného přihlašování** dialogové okno, vyberte **režimu** jako **na základě SAML přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-153">To enable single sign-on, in the **Single sign-on** dialog box, select **Mode** as **SAML-based Sign-on**.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_samlbase.png)

3. <span data-ttu-id="9e9bb-155">V **HR2day Merces domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9e9bb-155">In the **HR2day by Merces Domain and URLs** section, take the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_url.png)

    <span data-ttu-id="9e9bb-157">a.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-157">a.</span></span> <span data-ttu-id="9e9bb-158">V **přihlašovací adresa URL** pole, zadejte adresu URL pomocí následujícího vzorce: `https://<tenantname>.force.com/<instancename>`.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-158">In the **Sign-on URL** box, type a URL by using the following pattern: `https://<tenantname>.force.com/<instancename>`.</span></span>

    <span data-ttu-id="9e9bb-159">b.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-159">b.</span></span> <span data-ttu-id="9e9bb-160">V **identifikátor** pole, zadejte adresu URL pomocí následujícího vzorce: `https://hr2day.force.com/<companyname>`.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-160">In the **Identifier** box, type a URL by using the following pattern: `https://hr2day.force.com/<companyname>`.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9e9bb-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-161">These values are not real.</span></span> <span data-ttu-id="9e9bb-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-162">Update these values with the actual sign-on URL and identifier.</span></span> <span data-ttu-id="9e9bb-163">Obraťte se [HR2day tým podpory klienta Merces](mailto:servicedesk@merces.nl) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-163">Contact the [HR2day by Merces client support team](mailto:servicedesk@merces.nl) to get these values.</span></span> 
 


4. <span data-ttu-id="9e9bb-164">Na **SAML podpisový certifikát** vyberte **Certificate(Base64)**a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-164">On the **SAML Signing Certificate** section, select **Certificate(Base64)**, and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_certificate.png) 

5. <span data-ttu-id="9e9bb-166">Tato část popisuje, jak uživatelům povolit ověřování na HR2day podle Merces ke svému účtu ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-166">This section describes how to enable users to authenticate to HR2day by Merces with their account in Azure AD.</span></span> <span data-ttu-id="9e9bb-167">Budou to provedete pomocí federace, která je založená na protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-167">They do this by using federation that's based on the SAML protocol.</span></span>

    <span data-ttu-id="9e9bb-168">Vaše HR2day aplikací Merces očekává SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, můžete přidat mapování vlastních atributů do tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-168">Your HR2day by Merces application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token.</span></span> <span data-ttu-id="9e9bb-169">Následující snímek obrazovky ukazuje příklad tohoto objektu.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-169">The following screenshot shows an example of this.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_00.png)
    
    > [!NOTE] 
    <span data-ttu-id="9e9bb-171">Před konfigurací kontrolního výrazu SAML, bude nutné se obrátit [HR2day tým podpory Merces klienta](mailto:servicedesk@merces.nl) a požadovat hodnotu atributu jedinečný identifikátor pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-171">Before you can configure the SAML assertion, you must contact the [HR2day by Merces Client support team](mailto:servicedesk@merces.nl) and request the value of the unique identifier attribute for your tenant.</span></span> <span data-ttu-id="9e9bb-172">Je nutné tuto hodnotu pro dokončení kroků v další části.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-172">You need this value to complete the steps in the next section.</span></span> 

6. <span data-ttu-id="9e9bb-173">V **jednotného přihlašování** dialogu **uživatelské atributy** nakonfigurujte atribut tokenu SAML, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-173">In the **Single sign-on** dialog box, in the **User Attributes** section, configure the SAML token attribute as shown in the following image.</span></span> <span data-ttu-id="9e9bb-174">Proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-174">Then take the following steps.</span></span>
    
      | <span data-ttu-id="9e9bb-175">Název atributu</span><span class="sxs-lookup"><span data-stu-id="9e9bb-175">Attribute name</span></span>    |   <span data-ttu-id="9e9bb-176">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="9e9bb-176">Attribute value</span></span> |  
    | ------------------- | -------------------- |    
    | <span data-ttu-id="9e9bb-177">ATTR_LOGINCLAIM</span><span class="sxs-lookup"><span data-stu-id="9e9bb-177">ATTR_LOGINCLAIM</span></span> | <span data-ttu-id="9e9bb-178">spojení ([pošty] "102938475Z", "@"</span><span class="sxs-lookup"><span data-stu-id="9e9bb-178">join([mail],"102938475Z","@"</span></span> |
    
      <span data-ttu-id="9e9bb-179">a.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-179">a.</span></span> <span data-ttu-id="9e9bb-180">Chcete-li otevřít **přidat atribut** dialogovém okně, vyberte **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-180">To open the **Add Attribute** dialog, select **Add attribute**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="9e9bb-183">b.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-183">b.</span></span> <span data-ttu-id="9e9bb-184">V **název** zadejte **ATTR_LOGINCLAIM**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-184">In the **Name** box, type **ATTR_LOGINCLAIM**.</span></span>

    <span data-ttu-id="9e9bb-185">c.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-185">c.</span></span> <span data-ttu-id="9e9bb-186">Z **hodnotu** seznamu, vyberte **Join()**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-186">From the **Value** list, select **Join()**.</span></span>

    <span data-ttu-id="9e9bb-187">d.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-187">d.</span></span> <span data-ttu-id="9e9bb-188">Z **řetězec1** seznamu, vyberte **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-188">From the **String1** list, select **user.mail**.</span></span>

    <span data-ttu-id="9e9bb-189">e.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-189">e.</span></span> <span data-ttu-id="9e9bb-190">Pro **řetězec2**, zadejte jedinečný identifikátor, který zajišťuje HR2day týmu.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-190">For **String2**, type the unique identifier that's provided by your HR2day team.</span></span>

    <span data-ttu-id="9e9bb-191">f.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-191">f.</span></span> <span data-ttu-id="9e9bb-192">V **oddělovače** zadejte  **@** .</span><span class="sxs-lookup"><span data-stu-id="9e9bb-192">In the **Separator** box, type **@**.</span></span>
    
    <span data-ttu-id="9e9bb-193">g.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-193">g.</span></span> <span data-ttu-id="9e9bb-194">Vyberte **Ok**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-194">Select **Ok**.</span></span>

7. <span data-ttu-id="9e9bb-195">Vyberte tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-195">Select the **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="9e9bb-197">V **HR2day Merces konfigurace** vyberte **konfigurace HR2day podle Merces** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-197">In the **HR2day by Merces Configuration** section, select **Configure HR2day by Merces** to open the **Configure sign-on** window.</span></span> <span data-ttu-id="9e9bb-198">Kopírování **Sign-Out URL**, **SAML Entity ID**, a **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka** části.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-198">Copy the **Sign-Out URL**, **SAML Entity ID**, and **SAML Single Sign-On Service URL** from the **Quick Reference** section.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_configure.png) 

9. <span data-ttu-id="9e9bb-200">Chcete-li nakonfigurovat jednotné přihlašování pro aplikace, obraťte se [HR2day tým podpory klienta Merces](mailTo:servicedesk@merces.nl).</span><span class="sxs-lookup"><span data-stu-id="9e9bb-200">To configure SSO  for your application, contact the [HR2day by Merces client support team](mailTo:servicedesk@merces.nl).</span></span> <span data-ttu-id="9e9bb-201">Připojení stažené **Certificate(Base64)** souborů k e-mailu.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-201">Attach the downloaded **Certificate(Base64)** file to your email.</span></span> <span data-ttu-id="9e9bb-202">Zadat taky **Sign-Out URL**, **SAML Entity ID**, a **SAML jeden přihlašování adresa URL služby** tak, aby mohly být konfigurovány pro integraci jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-202">Also provide the **Sign-Out URL**, **SAML Entity ID**, and **SAML Single Sign-On Service URL** so that they can be configured for SSO integration.</span></span>

    > [!NOTE]
    ><span data-ttu-id="9e9bb-203">Merces týmu zmínili, že tato integrace vyžaduje ID Entity na hodnotu se vzorem **https://hr2day.force.com/INSTANCENAME**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-203">Mention to the Merces team that this integration needs the Entity ID to be set with the pattern **https://hr2day.force.com/INSTANCENAME**.</span></span>

    > [!TIP]
    ><span data-ttu-id="9e9bb-204">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="9e9bb-204">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9e9bb-205">Po přidání této aplikace z **služby Active Directory** > **podnikové aplikace, které** vyberte **jednotné přihlašování** kartě.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-205">After you add this app from the **Active Directory** > **Enterprise Applications** section, select the **Single Sign-On** tab.</span></span> <span data-ttu-id="9e9bb-206">Přejděte k embedded dokumentace prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-206">Then access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9e9bb-207">Si můžete přečíst informace o funkci embedded dokumentace v [Azure AD vložených dokumentaci]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="9e9bb-207">You can read more about the embedded documentation feature in the [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="9e9bb-208">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9e9bb-208">Create an Azure AD test user</span></span>
<span data-ttu-id="9e9bb-209">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-209">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="9e9bb-211">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9e9bb-211">**To create a test user in Azure AD, take the following steps:**</span></span>

1. <span data-ttu-id="9e9bb-212">V **portál Azure**, na levém navigačním podokně, vyberte **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-212">In the **Azure portal**, on the left navigation pane, select the **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9e9bb-214">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom vyberte **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-214">To display the list of users, go to **Users and groups**, and then select **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9e9bb-216">Chcete-li otevřít **uživatele** dialogové okno, vyberte **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-216">To open the **User** dialog box, select **Add** on the top of the dialog box.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9e9bb-218">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9e9bb-218">In the **User** dialog box, take the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9e9bb-220">a.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-220">a.</span></span> <span data-ttu-id="9e9bb-221">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-221">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9e9bb-222">b.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-222">b.</span></span> <span data-ttu-id="9e9bb-223">V **uživatelské jméno** zadejte **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-223">In the **User name** box, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9e9bb-224">c.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-224">c.</span></span> <span data-ttu-id="9e9bb-225">Vyberte **zobrazit hesla**a zapište si heslo.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-225">Select **Show Password**, and then write down the password.</span></span>

    <span data-ttu-id="9e9bb-226">d.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-226">d.</span></span> <span data-ttu-id="9e9bb-227">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-227">Select **Create**.</span></span>
 
### <a name="create-an-hr2day-by-merces-test-user"></a><span data-ttu-id="9e9bb-228">Vytvoření HR2day Merces testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="9e9bb-228">Create an HR2day by Merces test user</span></span>

<span data-ttu-id="9e9bb-229">Cílem této části je vytvoření uživatele volá Britta Simon v HR2day Merces.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-229">The objective of this section is to create a user called Britta Simon in HR2day by Merces.</span></span> <span data-ttu-id="9e9bb-230">Chcete-li přidat uživatele v účtu HR2day, pracovat s [HR2day tým podpory klienta Merces](mailto:servicedesk@merces.nl).</span><span class="sxs-lookup"><span data-stu-id="9e9bb-230">To add the users in the HR2day account, work with the [HR2day by Merces client support team](mailto:servicedesk@merces.nl).</span></span> 

> [!NOTE]
> <span data-ttu-id="9e9bb-231">Pokud je potřeba ručně vytvořit uživateli, obraťte se [HR2day tým podpory klienta Merces](mailto:servicedesk@merces.nl).</span><span class="sxs-lookup"><span data-stu-id="9e9bb-231">If you need to create a user manually, contact the [HR2day by Merces client support team](mailto:servicedesk@merces.nl).</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="9e9bb-232">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9e9bb-232">Assign the Azure AD test user</span></span>

<span data-ttu-id="9e9bb-233">V této části povolíte Britta Simon používat tak, že udělíte přístup k HR2day podle Merces Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-233">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to HR2day by Merces.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9e9bb-235">**Pokud chcete přiřadit Britta Simon HR2day podle Merces, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9e9bb-235">**To assign Britta Simon to HR2day by Merces, take the following steps:**</span></span>

1. <span data-ttu-id="9e9bb-236">Na portálu Azure otevřete zobrazení aplikace, přejděte do zobrazení adresáře a potom přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-236">In the Azure portal, open the applications view, go to the directory view, and then go to **Enterprise applications**.</span></span> <span data-ttu-id="9e9bb-237">Potom vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-237">Next, select **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9e9bb-239">V seznamu aplikací vyberte **HR2day podle Merces**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-239">In the applications list, select **HR2day by Merces**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_app.png) 

3. <span data-ttu-id="9e9bb-241">V nabídce na levé straně vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-241">In the menu on the left, select **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9e9bb-243">Vyberte **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-243">Select the **Add** button.</span></span> <span data-ttu-id="9e9bb-244">Potom v **přidat přiřazení** dialogové okno, vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-244">Then, in the **Add Assignment** dialog box, select **Users and groups**.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="9e9bb-246">V **uživatelů a skupin** v dialogovém **uživatelé** seznamu, vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-246">In the **Users and groups** dialog box, in the **Users** list, select **Britta Simon**.</span></span>

6. <span data-ttu-id="9e9bb-247">Klikněte **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-247">Click the **Select** button.</span></span>

7. <span data-ttu-id="9e9bb-248">V **přidat přiřazení** dialogové okno, vyberte **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-248">In the **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="9e9bb-249">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9e9bb-249">Test single sign-on</span></span>

<span data-ttu-id="9e9bb-250">Cílem této části je testování Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-250">The objective of this section is to test your Azure AD single sign-on configuration by using the Access Panel.</span></span>  

<span data-ttu-id="9e9bb-251">Když vyberete HR2day podle Merces dlaždice na přístupovém panelu, budete automaticky získat přihlášení k vaší HR2day Merces aplikací.</span><span class="sxs-lookup"><span data-stu-id="9e9bb-251">When you select the HR2day by Merces tile in the Access Panel, you automatically get signed in  to your HR2day by Merces application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9e9bb-252">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9e9bb-252">Additional resources</span></span>

* [<span data-ttu-id="9e9bb-253">Seznam kurzů o tom, jak integrovat SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9e9bb-253">List of tutorials about how to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9e9bb-254">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9e9bb-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_203.png

