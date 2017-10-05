---
title: "Zobrazení SAML vrácený službě Řízení přístupu (Java)"
description: "Zjistěte, jak chcete-li zobrazit SAML vrácený službě Řízení přístupu v aplikace Java hostované v Azure."
services: active-directory
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 6cd216f9-eb43-46b4-b30d-f194d0ae2d48
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.custom: aaddev
ms.openlocfilehash: 1552e624a4703138ab82f7133ceaec3dbd04e1db
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-view-saml-returned-by-the-azure-access-control-service"></a><span data-ttu-id="3326a-103">Postup zobrazení SAML vrácený službě Řízení přístupu Azure</span><span class="sxs-lookup"><span data-stu-id="3326a-103">How to view SAML returned by the Azure Access Control Service</span></span>
<span data-ttu-id="3326a-104">Tento průvodce vám ukáže, jak chcete zobrazit základní zabezpečení kontrolního výrazu SAML (Markup Language) vrátíte zpět do aplikace služby Řízení přístupu (ACS) Azure.</span><span class="sxs-lookup"><span data-stu-id="3326a-104">This guide will show you how to view the underlying Security Assertion Markup Language (SAML) returned to your application by the Azure Access Control Service (ACS).</span></span> <span data-ttu-id="3326a-105">V příručce vychází [postup ověření webového uživatele s Azure přístup k řízení služby pomocí Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) tématu, tím, že poskytuje kód, který zobrazí informace o tomto SAML.</span><span class="sxs-lookup"><span data-stu-id="3326a-105">The guide builds on the [How to Authenticate Web Users with Azure Access Control Service Using Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) topic, by providing code that displays the SAML information.</span></span> <span data-ttu-id="3326a-106">Hotová aplikace bude vypadat podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="3326a-106">The completed application will look similar to the following.</span></span>

![Příklad výstupu SAML][saml_output]

<span data-ttu-id="3326a-108">Další informace o služby ACS, najdete v článku [další kroky](#next_steps) části.</span><span class="sxs-lookup"><span data-stu-id="3326a-108">For more information on ACS, see the [Next steps](#next_steps) section.</span></span>

> [!NOTE]
> <span data-ttu-id="3326a-109">Ovládací prvek filtru Azure přístup služeb je náhled technologie komunity.</span><span class="sxs-lookup"><span data-stu-id="3326a-109">The Azure Access Services Control Filter is a community technology preview.</span></span> <span data-ttu-id="3326a-110">Jako předběžné verze softwaru není oficiálně společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3326a-110">As pre-release software, it is not formally supported by Microsoft.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="3326a-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3326a-111">Prerequisites</span></span>
<span data-ttu-id="3326a-112">K dokončení úkolů v tomto průvodci, dokončete ukázku najdete na adrese [postup ověření webového uživatele s Azure přístup k řízení služby pomocí Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) a použít jej jako výchozí bod pro tento kurz.</span><span class="sxs-lookup"><span data-stu-id="3326a-112">To complete the tasks in this guide, complete the sample at [How to Authenticate Web Users with Azure Access Control Service Using Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) and use it as the starting point for this tutorial.</span></span>

## <a name="add-the-jspwriter-library-to-your-build-path-and-deployment-assembly"></a><span data-ttu-id="3326a-113">Přidání knihovny JspWriter na vaše sestavení a nasazení cesta sestavení</span><span class="sxs-lookup"><span data-stu-id="3326a-113">Add the JspWriter library to your build path and deployment assembly</span></span>
<span data-ttu-id="3326a-114">Přidání knihovny, který obsahuje **javax.servlet.jsp.JspWriter** třída pro vaše nasazení a cesta k sestavení sestavení.</span><span class="sxs-lookup"><span data-stu-id="3326a-114">Add the library that contains the **javax.servlet.jsp.JspWriter** class to your build path and deployment assembly.</span></span> <span data-ttu-id="3326a-115">Pokud používáte Tomcat, je knihovna **jsp api.jar**, který se nachází v Apache **lib** složky.</span><span class="sxs-lookup"><span data-stu-id="3326a-115">If you are using Tomcat, the library is **jsp-api.jar**, which is located in the Apache **lib** folder.</span></span>

1. <span data-ttu-id="3326a-116">V prostředí Eclipse v prohlížeči projektu klikněte pravým tlačítkem na **MyACSHelloWorld**, klikněte na tlačítko **cesta sestavení**, klikněte na tlačítko **konfigurovat cestu sestavení**, klikněte na tlačítko **knihovny** a pak klikněte **přidat externí JARs**.</span><span class="sxs-lookup"><span data-stu-id="3326a-116">In Eclipse's Project Explorer, right-click **MyACSHelloWorld**, click **Build Path**, click **Configure Build Path**, click the **Libraries** tab, and then click **Add External JARs**.</span></span>
2. <span data-ttu-id="3326a-117">V **JAR výběr** dialogové okno, přejděte na nezbytné JAR, vyberte ho a pak klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="3326a-117">In the **JAR Selection** dialog, navigate to the necessary JAR, select it, and then click **Open**.</span></span>
3. <span data-ttu-id="3326a-118">S **vlastnosti pro MyACSHelloWorld** stále otevřená, klikněte na dialogové okno **nasazení sestavení**.</span><span class="sxs-lookup"><span data-stu-id="3326a-118">With the **Properties for MyACSHelloWorld** dialog still open, click **Deployment Assembly**.</span></span>
4. <span data-ttu-id="3326a-119">V **webové nasazení sestavení** dialogové okno, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="3326a-119">In the **Web Deployment Assembly** dialog, click **Add**.</span></span>
5. <span data-ttu-id="3326a-120">V **nové – Direktiva Assembly** dialogové okno, klikněte na tlačítko **položky cesta sestavení Java** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="3326a-120">In the **New Assembly Directive** dialog, click **Java Build Path Entries** and then click **Next**.</span></span>
6. <span data-ttu-id="3326a-121">Vyberte příslušnou knihovnu a klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="3326a-121">Select the appropriate library and click **Finish**.</span></span>
7. <span data-ttu-id="3326a-122">Klikněte na tlačítko **OK** zavřete **vlastnosti pro MyACSHelloWorld** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3326a-122">Click **OK** to close the **Properties for MyACSHelloWorld** dialog.</span></span>

## <a name="modify-the-jsp-file-to-display-saml"></a><span data-ttu-id="3326a-123">Upravte soubor JSP zobrazíte SAML</span><span class="sxs-lookup"><span data-stu-id="3326a-123">Modify the JSP file to display SAML</span></span>
<span data-ttu-id="3326a-124">Upravit **index.jsp** použít následující kód.</span><span class="sxs-lookup"><span data-stu-id="3326a-124">Modify **index.jsp** to use the following code.</span></span>

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {

            try
            {
                String nodeName;
                int nChild, i;

                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");

                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }

                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                    {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    

                                 // If it is a text node, just print the text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out the child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                        for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);

                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate the names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish the sentence.
                                            out.print(".");
                                        }

                                     }
                                     out.println("<br>");

                                     // Process the child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>

        <%
        try
        {
            String data  = (String) request.getAttribute("ACSSAML");

            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();

            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA);
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();

            // Iterate the child nodes of the doc.
            NodeList list = doc.getChildNodes();

            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e)
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }

        %>
    </body>
    </html>

## <a name="run-the-application"></a><span data-ttu-id="3326a-125">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="3326a-125">Run the application</span></span>
1. <span data-ttu-id="3326a-126">Spusťte aplikaci v emulátoru počítače nebo nasadit do Azure, pomocí kroků popsaných v [postup ověření webového uživatele s Azure přístup k řízení služby pomocí Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md).</span><span class="sxs-lookup"><span data-stu-id="3326a-126">Run your application in the computer emulator or deploy to Azure, using the steps documented at [How to Authenticate Web Users with Azure Access Control Service Using Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md).</span></span>
2. <span data-ttu-id="3326a-127">Spusťte prohlížeč a otevřete webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3326a-127">Launch a browser and open your web application.</span></span> <span data-ttu-id="3326a-128">Po přihlášení do aplikace se zobrazí informace o SAML, včetně assertion zabezpečení poskytované poskytovatelem identity.</span><span class="sxs-lookup"><span data-stu-id="3326a-128">After you log on to your application, you'll see SAML information, including the security assertion provided by the identity provider.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3326a-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3326a-129">Next steps</span></span>
<span data-ttu-id="3326a-130">K dalšímu prozkoumat funkce služby ACS a experimentovat s sofistikovanější scénáři, najdete v tématu [2.0 služby Řízení přístupu][Access Control Service 2.0].</span><span class="sxs-lookup"><span data-stu-id="3326a-130">To further explore ACS's functionality and to experiment with more sophisticated scenarios, see [Access Control Service 2.0][Access Control Service 2.0].</span></span>

[Prerequisites]: #pre
[Modify the JSP file to display SAML]: #modify_jsp
[Add the JspWriter library to your build path and deployment assembly]: #add_library
[Run the application]: #run_application
[Next steps]: #next_steps
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[How to Authenticate Web Users with Azure Access Control Service Using Eclipse]: active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
