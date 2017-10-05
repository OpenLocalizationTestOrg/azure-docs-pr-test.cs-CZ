---
title: "Začínáme se službou Azure AD Java příkazového řádku | Microsoft Docs"
description: "Jak vytvořit aplikaci Java příkazového řádku, který se přihlásí uživatele pro přístup k rozhraní API."
services: active-directory
documentationcenter: java
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 51e1a8f9-6ff0-4643-a350-0ba794e26fd1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 01/23/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 91e4a7b2ac454465d5cce4948a4d5f0b542d2b55
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-java-command-line-app-to-access-an-api-with-azure-ad"></a><span data-ttu-id="ce203-103">Pomocí aplikace v jazyce Java příkazového řádku pro přístup k rozhraní API s Azure AD</span><span class="sxs-lookup"><span data-stu-id="ce203-103">Using Java Command Line App To Access An API with Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="ce203-104">Azure AD je jednoduchá a přímočará využít Správa identit pro webové aplikace, k poskytování jednoho přihlášení a odhlášení jenom pár řádků kódu.</span><span class="sxs-lookup"><span data-stu-id="ce203-104">Azure AD makes it simple and straightforward to outsource your web app's identity management, providing single sign-in and sign-out with only a few lines of code.</span></span>  <span data-ttu-id="ce203-105">V jazyce Java webové aplikace můžete to provést pomocí implementace ADAL4J komunitou vytvářený společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ce203-105">In Java web apps, you can accomplish this using Microsoft's implementation of the community-driven ADAL4J.</span></span>

  <span data-ttu-id="ce203-106">Zde použijeme ADAL4J na:</span><span class="sxs-lookup"><span data-stu-id="ce203-106">Here we'll use ADAL4J to:</span></span>

* <span data-ttu-id="ce203-107">Přihlášení uživatele do aplikace pomocí Azure AD jako zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="ce203-107">Sign the user into the app using Azure AD as the identity provider.</span></span>
* <span data-ttu-id="ce203-108">Zobrazí některé informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="ce203-108">Display some information about the user.</span></span>
* <span data-ttu-id="ce203-109">Přihlášení uživatele mimo aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ce203-109">Sign the user out of the app.</span></span>

<span data-ttu-id="ce203-110">Chcete-li to provést, budete muset:</span><span class="sxs-lookup"><span data-stu-id="ce203-110">In order to do this, you'll need to:</span></span>

1. <span data-ttu-id="ce203-111">Zaregistrovat aplikaci s Azure AD</span><span class="sxs-lookup"><span data-stu-id="ce203-111">Register an application with Azure AD</span></span>
2. <span data-ttu-id="ce203-112">Nastavte aplikaci pro použití knihovny ADAL4J.</span><span class="sxs-lookup"><span data-stu-id="ce203-112">Set up your app to use the ADAL4J library.</span></span>
3. <span data-ttu-id="ce203-113">V knihovně ADAL4J pro zasílání požadavků na přihlášení a odhlášení do Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ce203-113">Use the ADAL4J library to issue sign-in and sign-out requests to Azure AD.</span></span>
4. <span data-ttu-id="ce203-114">Tisknout data o uživateli.</span><span class="sxs-lookup"><span data-stu-id="ce203-114">Print out data about the user.</span></span>

<span data-ttu-id="ce203-115">Abyste mohli začít, [stáhnout kostru aplikace](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) nebo [stažení je hotová ukázka](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="ce203-115">To get started, [download the app skeleton](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) or [download the completed sample](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).</span></span>  <span data-ttu-id="ce203-116">Budete také potřebovat klient služby Azure AD, do kterého chcete registrace vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ce203-116">You'll also need an Azure AD tenant in which to register your application.</span></span>  <span data-ttu-id="ce203-117">Pokud již, nemáte [zjistěte, jak získat](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="ce203-117">If you don't have one already, [learn how to get one](active-directory-howto-tenant.md).</span></span>

## <a name="1--register-an-application-with-azure-ad"></a><span data-ttu-id="ce203-118">1.  Zaregistrovat aplikaci s Azure AD</span><span class="sxs-lookup"><span data-stu-id="ce203-118">1.  Register an Application with Azure AD</span></span>
<span data-ttu-id="ce203-119">Pokud chcete povolit aplikaci k ověřování uživatelů, budete nejprve muset zaregistrujte novou aplikaci v klientovi.</span><span class="sxs-lookup"><span data-stu-id="ce203-119">To enable your app to authenticate users, you'll first need to register a new application in your tenant.</span></span>

1. <span data-ttu-id="ce203-120">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ce203-120">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ce203-121">Na horním panelu klikněte na tlačítko na vašem účtu a v části **Directory** vyberte klienta služby Active Directory, kam chcete registrace vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ce203-121">On the top bar, click on your account and under the **Directory** list, choose the Active Directory tenant where you wish to register your application.</span></span>
3. <span data-ttu-id="ce203-122">Klikněte na **více služeb** v navigaci vlevo a zvolte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ce203-122">Click on **More Services** in the left hand nav, and choose **Azure Active Directory**.</span></span>
4. <span data-ttu-id="ce203-123">Klikněte na **registrace aplikace** a zvolte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ce203-123">Click on **App registrations** and choose **Add**.</span></span>
5. <span data-ttu-id="ce203-124">Postupujte podle výzev a vytvořte novou **webové aplikace nebo WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="ce203-124">Follow the prompts and create a new **Web Application and/or WebAPI**.</span></span>
  * <span data-ttu-id="ce203-125">**Název** aplikace popíše aplikaci pro koncové uživatele</span><span class="sxs-lookup"><span data-stu-id="ce203-125">The **name** of the application will describe your application to end-users</span></span>
  * <span data-ttu-id="ce203-126">**Přihlašovací adresa URL** je základní adresu URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="ce203-126">The **Sign-On URL** is the base URL of your app.</span></span>  <span data-ttu-id="ce203-127">Výchozí hodnota kostru je `http://localhost:8080/adal4jsample/`.</span><span class="sxs-lookup"><span data-stu-id="ce203-127">The skeleton's default is `http://localhost:8080/adal4jsample/`.</span></span>
6. <span data-ttu-id="ce203-128">Po dokončení registrace AAD přiřadí vaší aplikace, jedinečné ID aplikace</span><span class="sxs-lookup"><span data-stu-id="ce203-128">Once you've completed registration, AAD will assign your app a unique Application ID.</span></span>  <span data-ttu-id="ce203-129">Tuto hodnotu budete potřebovat v další části, zkopírujte jej na kartě aplikace.</span><span class="sxs-lookup"><span data-stu-id="ce203-129">You'll need this value in the next sections, so copy it from the application tab.</span></span>
7. <span data-ttu-id="ce203-130">Z **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte identifikátor ID URI aplikace.</span><span class="sxs-lookup"><span data-stu-id="ce203-130">From the **Settings** -> **Properties** page for your application, update the App ID URI.</span></span> <span data-ttu-id="ce203-131">**Identifikátor ID URI aplikace** je jedinečný identifikátor pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ce203-131">The **App ID URI** is a unique identifier for your application.</span></span>  <span data-ttu-id="ce203-132">Konvence, je použít `https://<tenant-domain>/<app-name>`, například `http://localhost:8080/adal4jsample/`.</span><span class="sxs-lookup"><span data-stu-id="ce203-132">The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `http://localhost:8080/adal4jsample/`.</span></span>

<span data-ttu-id="ce203-133">Jednou na portálu pro vaši aplikaci vytvořit **klíč** z **nastavení** stránky pro aplikace a zkopírujte ho dolů.</span><span class="sxs-lookup"><span data-stu-id="ce203-133">Once in the portal for your app create a **Key** from the **Settings** page for your application and copy it down.</span></span>  <span data-ttu-id="ce203-134">Budete ho potřebovat za chvíli.</span><span class="sxs-lookup"><span data-stu-id="ce203-134">You will need it shortly.</span></span>

## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisites-using-maven"></a><span data-ttu-id="ce203-135">2. Nastavit aplikaci používat knihovny ADAL4J a požadavky pomocí nástroje Maven</span><span class="sxs-lookup"><span data-stu-id="ce203-135">2. Set up your app to use ADAL4J library and prerequisites using Maven</span></span>
<span data-ttu-id="ce203-136">Zde nakonfigurujeme ADAL4J pro použití ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="ce203-136">Here, we'll configure ADAL4J to use the OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="ce203-137">ADAL4J se použije k vydávání požadavků na přihlášení a odhlášení, spravovat relace uživatele a získat informace o uživateli, mimo jiné.</span><span class="sxs-lookup"><span data-stu-id="ce203-137">ADAL4J will be used to issue sign-in and sign-out requests, manage the user's session, and get information about the user, amongst other things.</span></span>

* <span data-ttu-id="ce203-138">V kořenovém adresáři projektu, otevírání nebo vytváření `pom.xml` a najděte `// TODO: provide dependencies for Maven` a nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ce203-138">In the root directory of your project, open/create `pom.xml` and locate the `// TODO: provide dependencies for Maven` and replace with the following:</span></span>

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-the-java-publicclient-file"></a><span data-ttu-id="ce203-139">3. Vytvoření souboru Java PublicClient</span><span class="sxs-lookup"><span data-stu-id="ce203-139">3. Create the Java PublicClient file</span></span>
<span data-ttu-id="ce203-140">Jak je uvedeno výše, použijeme rozhraní Graph API k získání dat o přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="ce203-140">As indicated above, we will be using the Graph API to get data about the logged in user.</span></span> <span data-ttu-id="ce203-141">To měla být pro nás měli vytvoříme i soubor představují **objektu adresáře** a soubor představují **uživatele** tak, aby bylo možné použít vzor ú jazyka Java.</span><span class="sxs-lookup"><span data-stu-id="ce203-141">For this to be easy for us we should create both a file to represent a **Directory Object** and an individual file to represent the **User** so that the OO pattern of Java can be used.</span></span>

* <span data-ttu-id="ce203-142">Vytvořte soubor s názvem `DirectoryObject.java` které použijete k uložení základní data o žádné DirectoryObject (obav později využít pro jiné dotazy grafu, může provádět).</span><span class="sxs-lookup"><span data-stu-id="ce203-142">Create a file called `DirectoryObject.java` which we will use to store basic data about any DirectoryObject (you can feel free to use this later for any other Graph Queries you may do).</span></span> <span data-ttu-id="ce203-143">Vám může vyjímání a vkládání toto z následujícího seznamu:</span><span class="sxs-lookup"><span data-stu-id="ce203-143">You can cut/paste this from below:</span></span>

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


## <a name="compile-and-run-the-sample"></a><span data-ttu-id="ce203-144">Zkompilování a spuštění vzorku</span><span class="sxs-lookup"><span data-stu-id="ce203-144">Compile and run the sample</span></span>
<span data-ttu-id="ce203-145">Přejděte zpět na kořenového adresáře a spusťte následující příkaz k sestavit ukázku, můžete zadat pouze společně s použitím `maven`.</span><span class="sxs-lookup"><span data-stu-id="ce203-145">Change back out to your root directory and run the following command to build the sample you just put together using `maven`.</span></span> <span data-ttu-id="ce203-146">Tímto dojde k použití `pom.xml` soubor jste napsali závislosti.</span><span class="sxs-lookup"><span data-stu-id="ce203-146">This will use the `pom.xml` file you wrote for dependencies.</span></span>

`$ mvn package`

<span data-ttu-id="ce203-147">Teď byste měli mít `adal4jsample.war` ve vaší `/targets` adresáře.</span><span class="sxs-lookup"><span data-stu-id="ce203-147">You should now have a `adal4jsample.war` file in your `/targets` directory.</span></span> <span data-ttu-id="ce203-148">Můžete nasadit, v kontejneru vaší Tomcat a navštívíte adresu</span><span class="sxs-lookup"><span data-stu-id="ce203-148">You may deploy that in your Tomcat container and visit the URL</span></span> 

`http://localhost:8080/adal4jsample/`

> [!NOTE]
> <span data-ttu-id="ce203-149">Je velmi snadno nasadit WAR s nejnovější serverů Tomcat.</span><span class="sxs-lookup"><span data-stu-id="ce203-149">It is very easy to deploy a WAR with the latest Tomcat servers.</span></span> <span data-ttu-id="ce203-150">Jednoduše přejděte na `http://localhost:8080/manager/` a postupujte podle pokynů na odesílání vašeho '' adal4jsample.war' souboru.</span><span class="sxs-lookup"><span data-stu-id="ce203-150">Simply navigate to `http://localhost:8080/manager/` and follow the instructions on uploading your ``adal4jsample.war\` file.</span></span> <span data-ttu-id="ce203-151">Autodeploy ho bude pro vás správný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="ce203-151">It will autodeploy for you with the correct endpoint.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="ce203-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ce203-152">Next Steps</span></span>
<span data-ttu-id="ce203-153">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="ce203-153">Congratulations!</span></span> <span data-ttu-id="ce203-154">Teď máte funkční aplikaci Java, která má schopnost ověřovat uživatele bezpečně volání webového rozhraní API pomocí OAuth 2.0 a získat základní informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="ce203-154">You now have a working Java application that has the ability to authenticate users, securely call Web APIs using OAuth 2.0, and get basic information about the user.</span></span>  <span data-ttu-id="ce203-155">Pokud jste to ještě neudělali, nyní je čas k naplnění vašeho klienta s některými uživateli.</span><span class="sxs-lookup"><span data-stu-id="ce203-155">If you haven't already, now is the time to populate your tenant with some users.</span></span>

<span data-ttu-id="ce203-156">Pro srovnání je hotová ukázka (bez vašich hodnot nastavení) [je k dispozici jako ZIP zde](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), nebo ji můžete klonovat z Githubu:</span><span class="sxs-lookup"><span data-stu-id="ce203-156">For reference, the completed sample (without your configuration values) [is provided as a .zip here](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

