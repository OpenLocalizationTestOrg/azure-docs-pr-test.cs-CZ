---
title: "Konfigurace ověřování pomocí Amazon Web Services | Dokumentace Microsoftu"
description: "Tento článek popisuje postup vytvoření a ověření přihlašovacích údajů Amazon Web Services (AWS) pro runbooky ve službě Azure Automation, které spravují prostředky AWS."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
keywords: "ověřování aws, konfigurace aws"
ms.assetid: b6dde4bb-26ac-4876-9aa9-e586bed30d6b
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/11/2016
ms.author: magoedte
ms.openlocfilehash: fe590e7fc551c175d2f41f5b98e1558a756df806
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-runbooks-with-amazon-web-services"></a><span data-ttu-id="2754b-104">Ověření runbooků pomocí Amazon Web Services</span><span class="sxs-lookup"><span data-stu-id="2754b-104">Authenticate Runbooks with Amazon Web Services</span></span>
<span data-ttu-id="2754b-105">Automatizaci běžných úkolů pomocí prostředků ve službě Amazon Web Services můžete provést pomocí runbooků Automation v Azure.</span><span class="sxs-lookup"><span data-stu-id="2754b-105">Automating common tasks with resources in Amazon Web Services (AWS) can be accomplished with Automation runbooks in Azure.</span></span>  <span data-ttu-id="2754b-106">V AWS můžete automatizovat celou řadu úloh pomocí runbooků Automation, stejně to děláte s prostředky v Azure.</span><span class="sxs-lookup"><span data-stu-id="2754b-106">You can automate many tasks in AWS using Automation runbooks just like you can with resources in Azure.</span></span>  <span data-ttu-id="2754b-107">Potřebujete jen dvě věci:</span><span class="sxs-lookup"><span data-stu-id="2754b-107">All that is required are two things:</span></span>

* <span data-ttu-id="2754b-108">Předplatné AWS a sadu přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="2754b-108">An AWS subscription and a set of credentials.</span></span>  <span data-ttu-id="2754b-109">Zejména přístupový klíč a tajný klíč AWS.</span><span class="sxs-lookup"><span data-stu-id="2754b-109">Specifically your AWS Access Key and Secret Key.</span></span>  <span data-ttu-id="2754b-110">Další informace najdete v článku [Použití přihlašovacích údajů AWS](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).</span><span class="sxs-lookup"><span data-stu-id="2754b-110">For more information, please review the article [Using AWS Credentials](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).</span></span>
* <span data-ttu-id="2754b-111">Předplatné Azure a účet Automation.</span><span class="sxs-lookup"><span data-stu-id="2754b-111">An Azure subscription and Automation account.</span></span>  <span data-ttu-id="2754b-112">Další informace o nastavení účtu Azure Automation najdete v článku [Konfigurace účtu Spustit v Azure jako](automation-sec-configure-azure-runas-account.md).</span><span class="sxs-lookup"><span data-stu-id="2754b-112">For more information on setting up an Azure Automation account, please review the article [Configure Azure Run As Account](automation-sec-configure-azure-runas-account.md).</span></span>  

<span data-ttu-id="2754b-113">Abyste mohli ověřovat pomocí AWS, musíte zadat sadu přihlašovacích údajů AWS a ověřit svoje runbooky spuštěné ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="2754b-113">To authenticate with AWS, you must specify a set of AWS credentials to authenticate your runbooks running from Azure Automation.</span></span> <span data-ttu-id="2754b-114">Pokud už máte vytvořený účet Automation a chcete ho použít k ověřování pomocí AWS, postupujte podle návodu v následující části.</span><span class="sxs-lookup"><span data-stu-id="2754b-114">If you already have an Automation account created and you want to use that to authenticate with AWS, you can follow the steps in the following section.</span></span>  <span data-ttu-id="2754b-115">Pokud chcete mít vyhrazený účet pro runbooky, které se budou zaměřovat na prostředky AWS, vytvořte nejdřív nový [účet Spustit v Automation jako](automation-sec-configure-azure-runas-account.md) (vynechte možnost vytvořit objekt služby) a potom postupujte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="2754b-115">If you want to dedicated an account for runbooks targetting AWS resources, you should first create a new [Automation Run As account](automation-sec-configure-azure-runas-account.md) (skip the option to create a service principal) and then follow the steps below.</span></span>

## <a name="configure-automation-account"></a><span data-ttu-id="2754b-116">Konfigurace účtu Automation</span><span class="sxs-lookup"><span data-stu-id="2754b-116">Configure Automation account</span></span>
<span data-ttu-id="2754b-117">Pokud má Azure Automation komunikovat s AWS, musíte nejdřív načíst přihlašovací údaje AWS a uložit je jako assety ve službě Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="2754b-117">For Azure Automation to communicate with AWS, you will first need to retrieve your AWS credentials and store them as assets in Azure Automation.</span></span>  <span data-ttu-id="2754b-118">Proveďte následující kroky, které jsou popsány v dokumentu AWS [Správa přístupových klíčů k vašemu účtu AWS](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html), vytvořte přístupový klíč a zkopírujte **Access Key ID** (ID přístupového klíče) a **Secret Access Key** (Tajný přístupový klíč) (případně si můžete soubor s klíčem stáhnout a uložit na bezpečné místo).</span><span class="sxs-lookup"><span data-stu-id="2754b-118">Perform the following steps documented in the AWS document [Managing Access Keys for your AWS Account](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) to create an Access Key and copy the **Access Key ID** and **Secret Access Key** (optionally download your key file to store it somewhere safe).</span></span>

<span data-ttu-id="2754b-119">Po vytvoření a zkopírování bezpečnostních klíčů AWS vytvořte pomocí účtu Azure Automation asset přihlašovacích údajů, bezpečně klíče uložte a odkazujte na ně ve svých runboocích.</span><span class="sxs-lookup"><span data-stu-id="2754b-119">After you have created and copied your AWS security keys, you will need to create a Credential asset with an Azure Automation account to securely store them and reference them with your runbooks.</span></span>  <span data-ttu-id="2754b-120">Postupujte podle kroků v části **Vytvoření nových assetů přihlašovacích údajů** v článku [Assety přihlašovacích údajů ve službě Azure Automation](automation-credentials.md) a zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="2754b-120">Follow the steps in the section **Creating a new credential asset** in the [Credential assets in Azure Automation](automation-credentials.md) article and enter the following information:</span></span>

1. <span data-ttu-id="2754b-121">Do pole **Název** zadejte **AWScred** nebo odpovídající hodnotu v souladu se svými standardy pro vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="2754b-121">In the **Name** box, enter **AWScred** or an appropriate value following your naming standards.</span></span>  
2. <span data-ttu-id="2754b-122">Do pole **Uživatelské jméno** zadejte svoje **Přístupové ID**, do polí **Heslo** a **Potvrzení hesla** zadejte svůj **Tajný přístupový klíč**.</span><span class="sxs-lookup"><span data-stu-id="2754b-122">In the **User name** box type your **Access ID** and your **Secret Access Key** in the **Password** and **Confirm password** box.</span></span>   

## <a name="next-steps"></a><span data-ttu-id="2754b-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2754b-123">Next steps</span></span>
* <span data-ttu-id="2754b-124">Další informace o vytváření runbooků pro automatizaci úloh v AWS najdete v článku [Automatizace nasazení virtuálního počítače ve službě Amazon Web Services](automation-scenario-aws-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="2754b-124">Reivew the solution article [Automating deployment of a VM in Amazon Web Services](automation-scenario-aws-deployment.md) to learn how to create runbooks to automate tasks in AWS.</span></span>
