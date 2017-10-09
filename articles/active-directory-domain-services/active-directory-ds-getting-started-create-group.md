---
title: "Azure Active Directory Domain Services: Vytvoření skupiny administrators řadič domény hello Azure AD | Microsoft Docs"
description: "Povolit Azure Active Directory Domain Services pomocí hello portál Azure classic"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: maheshu
ms.openlocfilehash: d629ab9632ef6a45b549630999ff9a122d60c040
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-classic-portal"></a><span data-ttu-id="eac68-103">Povolit Azure Active Directory Domain Services pomocí hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="eac68-103">Enable Azure Active Directory Domain Services using hello Azure classic portal</span></span>
<span data-ttu-id="eac68-104">Tento článek popisuje a provede hello úlohy konfigurace, které jsou požadovány pro vás tooenable Azure Active Directory Domain Services (Azure AD DS) pro klienta služby Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="eac68-104">This article describes and walks through hello configuration tasks that are required for you tooenable Azure Active Directory Domain Services (Azure AD DS) for your Azure Active Directory (Azure AD) tenant.</span></span>

> [!NOTE]
> <span data-ttu-id="eac68-105">[**Místo toho zkuste hello nové (preview) Azure portálu prostředí**](active-directory-ds-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="eac68-105">[**Try hello new (preview) Azure portal experience instead**](active-directory-ds-getting-started.md).</span></span> 
>

## <a name="task-1-create-hello-azure-ad-dc-administrators-group"></a><span data-ttu-id="eac68-106">Úloha 1: vytvoření skupiny administrators řadič domény hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="eac68-106">Task 1: create hello Azure AD DC administrators group</span></span>
<span data-ttu-id="eac68-107">první úlohou Hello je toocreate skupiny pro správu v klientovi služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eac68-107">hello first task is toocreate an administrative group in your Azure AD tenant.</span></span> <span data-ttu-id="eac68-108">Tuto speciální skupinu pro správu se nazývá *AAD řadič domény správci*.</span><span class="sxs-lookup"><span data-stu-id="eac68-108">This special administrative group is called *AAD DC Administrators*.</span></span> <span data-ttu-id="eac68-109">Členové této skupiny mají oprávnění správce na počítačích, které jsou připojené k doméně toohello Azure Active Directory Domain Services spravované domény.</span><span class="sxs-lookup"><span data-stu-id="eac68-109">Members of this group are granted administrative permissions on machines that are domain-joined toohello Azure Active Directory Domain Services-managed domain.</span></span> <span data-ttu-id="eac68-110">Na počítačích připojených k doméně je této skupiny přidat toohello skupiny administrators.</span><span class="sxs-lookup"><span data-stu-id="eac68-110">On domain-joined machines, this group is added toohello administrators group.</span></span> <span data-ttu-id="eac68-111">Členové této skupiny můžou navíc použít počítače vzdáleně připojené k toodomain tooconnect vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="eac68-111">Additionally, members of this group can use Remote Desktop tooconnect remotely toodomain-joined machines.</span></span>  

> [!NOTE]
> <span data-ttu-id="eac68-112">Nemáte oprávnění správce domény nebo správce podnikové sítě na hello spravované domény, který jste vytvořili pomocí Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="eac68-112">You do not have Domain Administrator or Enterprise Administrator permissions on hello managed domain that you created by using Azure Active Directory Domain Services.</span></span> <span data-ttu-id="eac68-113">Na spravovaných domén tato oprávnění jsou vyhrazené hello služby a nejsou k dispozici toousers v rámci klienta hello provedeny.</span><span class="sxs-lookup"><span data-stu-id="eac68-113">On managed domains, these permissions are reserved by hello service and are not made available toousers within hello tenant.</span></span> <span data-ttu-id="eac68-114">Můžete však použít hello speciální skupinu pro správu vytvořit v této konfigurace úloh tooperform některé privilegované operace.</span><span class="sxs-lookup"><span data-stu-id="eac68-114">However, you can use hello special administrative group created in this configuration task tooperform some privileged operations.</span></span> <span data-ttu-id="eac68-115">Tyto operace zahrnují připojení počítače toohello doméně, patřící toohello skupiny správy na počítačích připojených k doméně a konfigurace zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="eac68-115">These operations include joining computers toohello domain, belonging toohello administration group on domain-joined machines, and configuring Group Policy.</span></span>
>

<span data-ttu-id="eac68-116">V této úloze konfigurace vytvořte skupiny pro správu hello a přidejte jeden nebo více uživatelů ve vaší skupině toohello directory.</span><span class="sxs-lookup"><span data-stu-id="eac68-116">In this configuration task, you create hello administrative group and add one or more users in your directory toohello group.</span></span> <span data-ttu-id="eac68-117">toocreate hello skupiny pro správu pro Azure Active Directory Domain Services, hello následující:</span><span class="sxs-lookup"><span data-stu-id="eac68-117">toocreate hello administrative group for Azure Active Directory Domain Services, do hello following:</span></span>

1. <span data-ttu-id="eac68-118">Přejděte toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="eac68-118">Go toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="eac68-119">V levém podokně hello vyberte hello **služby Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="eac68-119">In hello left pane, select hello **Active Directory** button.</span></span>
3. <span data-ttu-id="eac68-120">Vyberte, pro které chcete tooenable Azure Active Directory Domain Services klienta hello Azure AD (adresář).</span><span class="sxs-lookup"><span data-stu-id="eac68-120">Select hello Azure AD tenant (directory) for which you want tooenable Azure Active Directory Domain Services.</span></span> <span data-ttu-id="eac68-121">Můžete vytvořit pouze jednu doménu pro každý adresář Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eac68-121">You can create only one domain for each Azure AD directory.</span></span>

    ![Vyberte adresář Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. <span data-ttu-id="eac68-123">Na hello **preview directory** klikněte na tlačítko hello **skupiny** kartě.</span><span class="sxs-lookup"><span data-stu-id="eac68-123">On hello **preview directory** page, click hello **Groups** tab.</span></span>
5. <span data-ttu-id="eac68-124">Klikněte na klienta skupiny tooyour Azure AD, v podokně úloh hello v hello dolní části okna hello tooadd **přidat skupinu**.</span><span class="sxs-lookup"><span data-stu-id="eac68-124">tooadd a group tooyour Azure AD tenant, on hello task pane at hello bottom of hello window, click **Add Group**.</span></span>

    ![tlačítko Přidat skupinu Hello](./media/active-directory-domain-services-getting-started/add-group-button.png)
6. <span data-ttu-id="eac68-126">V hello **přidat skupinu** dialogové okno pole, vytvořte skupinu s názvem **AAD řadič domény správci**a poté nastavte **typ skupiny** příliš**zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="eac68-126">In hello **Add Group** dialog box, create a group named **AAD DC Administrators**, and then set **Group Type** too**Security**.</span></span>

   > [!WARNING]
   > <span data-ttu-id="eac68-127">tooenable přístupu ve vaší doméně, spravovat Azure Active Directory Domain Services, vytvořte skupinu s tímto názvem přesný.</span><span class="sxs-lookup"><span data-stu-id="eac68-127">tooenable access within your Azure Active Directory Domain Services-managed domain, create a group with this exact name.</span></span>
   >
   >

    ![Dialogové okno Přidat skupinu Hello](./media/active-directory-domain-services-getting-started/create-admin-group.png)
7. <span data-ttu-id="eac68-129">V hello **popis** zadejte popis, který umožňuje ostatním toounderstand že této skupině uděluje oprávnění pro správu v rámci Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="eac68-129">In hello **Description** box, enter a description that enables others toounderstand that this group grants administrative permissions within Azure Active Directory Domain Services.</span></span>
8. <span data-ttu-id="eac68-130">Po vytvoření skupiny hello, klikněte na tlačítko tooview název skupiny hello jeho vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="eac68-130">After you've created hello group, click hello group name tooview its properties.</span></span>
9. <span data-ttu-id="eac68-131">tooadd uživatele jako členy skupiny hello v hello dolní části okna hello, klikněte na tlačítko hello **přidat členy** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="eac68-131">tooadd users as members of hello group, at hello bottom of hello window, click hello **Add Members** button.</span></span>

    ![Přidání tlačítka členy skupiny](./media/active-directory-domain-services-getting-started/add-group-members-button.png)
10. <span data-ttu-id="eac68-133">V hello **přidat členy** dialogové okno, vyberte hello uživatelů, kteří by měl být členy této skupiny a potom klikněte na ikonu zaškrtnutí hello v hello dolní pravá.</span><span class="sxs-lookup"><span data-stu-id="eac68-133">In hello **Add members** dialog box, select hello users who should be members of this group, and then click hello checkmark icon at hello lower right.</span></span>

    ![Přidat skupinu uživatelů tooadministrators](./media/active-directory-domain-services-getting-started/add-group-members.png)


## <a name="next-step"></a><span data-ttu-id="eac68-135">Další krok</span><span class="sxs-lookup"><span data-stu-id="eac68-135">Next step</span></span>
[<span data-ttu-id="eac68-136">Úloha 2: vytvoření nebo výběr virtuální sítě Azure</span><span class="sxs-lookup"><span data-stu-id="eac68-136">Task 2: create or select an Azure virtual network</span></span>](active-directory-ds-getting-started-vnet.md)
