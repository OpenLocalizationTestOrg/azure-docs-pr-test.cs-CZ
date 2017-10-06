---
title: "aaaConfigure ověřování pomocí Amazon Web Services | Microsoft Docs"
description: "Tento článek popisuje, jak toocreate a ověření pověření AWS pro sady runbook ve službě Azure Automation, které spravují prostředky AWS."
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
ms.openlocfilehash: 6edaa000c1b206d80fe64b18c729dac124849070
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-runbooks-with-amazon-web-services"></a>Ověření runbooků pomocí Amazon Web Services
Automatizaci běžných úkolů pomocí prostředků ve službě Amazon Web Services můžete provést pomocí runbooků Automation v Azure.  V AWS můžete automatizovat celou řadu úloh pomocí runbooků Automation, stejně to děláte s prostředky v Azure.  Potřebujete jen dvě věci:

* Předplatné AWS a sadu přihlašovacích údajů.  Zejména přístupový klíč a tajný klíč AWS.  Další informace najdete v článku hello [pomocí přihlašovacích údajů AWS](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).
* Předplatné Azure a účet Automation.  Další informace o nastavení účtu Azure Automation, přečtěte si článek hello [konfigurace Azure účet Spustit jako](automation-sec-configure-azure-runas-account.md).  

tooauthenticate pomocí AWS, zadejte sadu tooauthenticate přihlašovacích údajů AWS svoje runbooky spuštěné ve službě Azure Automation. Pokud už máte vytvořený účet Automation a chcete toouse této tooauthenticate pomocí AWS, můžete provést hello kroky v následující části hello.  Pokud chcete toodedicated účet pro prostředky AWS které se budou zaměřovat sady runbook, měli byste nejprve vytvořit novou [účtu Automation spustit jako](automation-sec-configure-azure-runas-account.md) (přeskočit hello možnost toocreate instanční objekt) a pak postupujte podle následujících kroků hello.

## <a name="configure-automation-account"></a>Konfigurace účtu Automation
Pro Azure Automation toocommunicate pomocí AWS bude nejprve nutné tooretrieve přihlašovací údaje AWS a uložit je jako assety ve službě Azure Automation.  Proveďte následující kroky popsané v dokumentu AWS hello hello [Správa přístupových klíčů k vašemu účtu AWS](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) toocreate přístupový klíč a zkopírujte hello **Access Key ID** a **tajný přístupový klíč** (případně stáhnout váš soubor klíče toostore bezpečné místo).

Poté, co jste vytvořili a zkopírovali zabezpečovacích klíčů AWS, budete potřebovat toocreate asset přihlašovacích údajů s toosecurely účet Azure Automation. Uložte a odkazujte na ně ve svých runboocích.  Postupujte podle kroků hello v části hello **vytvoření nového prostředku pověření** v hello [assety přihlašovacích údajů ve službě Azure Automation](automation-credentials.md) a zadejte hello následující informace:

1. V hello **název** zadejte **AWScred** nebo odpovídající hodnotu v souladu standardy pro vytváření názvů.  
2. V hello **uživatelské jméno** zadejte vaše **ID přístupu** a **tajný přístupový klíč** v hello **heslo** a **potvrzení heslo** pole.   

## <a name="next-steps"></a>Další kroky
* Hello článku najdete [automatizace nasazení virtuálního počítače ve službě Amazon Web Services](automation-scenario-aws-deployment.md) toolearn jak toocreate sady runbook tooautomate úloh v AWS.

