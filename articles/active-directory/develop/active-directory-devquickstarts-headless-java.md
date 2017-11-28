---
title: "aaaAzure AD Java příkazového řádku Začínáme | Microsoft Docs"
description: "Jak toobuild Java příkazového řádku aplikace, která podepisuje uživatele v tooaccess rozhraní API."
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
ms.openlocfilehash: 9ba1d1e794928a39ca1f091bd0e6eba57ce3d6aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-java-command-line-app-tooaccess-an-api-with-azure-ad"></a><span data-ttu-id="08639-103">Aplikace příkazového řádku v jazyce Java tooAccess k rozhraní API pomocí Azure AD</span><span class="sxs-lookup"><span data-stu-id="08639-103">Using Java Command Line App tooAccess An API with Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="08639-104">Díky Azure AD je jednoduchá a přímočará toooutsource Správa identit vaší webové aplikace, poskytuje jednotné přihlašování a odhlašování jenom pár řádků kódu.</span><span class="sxs-lookup"><span data-stu-id="08639-104">Azure AD makes it simple and straightforward toooutsource your web app's identity management, providing single sign-in and sign-out with only a few lines of code.</span></span>  <span data-ttu-id="08639-105">V jazyce Java webové aplikace můžete to provést pomocí implementace hello komunitou vytvářený ADAL4J společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="08639-105">In Java web apps, you can accomplish this using Microsoft's implementation of hello community-driven ADAL4J.</span></span>

  <span data-ttu-id="08639-106">Zde použijeme ADAL4J na:</span><span class="sxs-lookup"><span data-stu-id="08639-106">Here we'll use ADAL4J to:</span></span>

* <span data-ttu-id="08639-107">Uživatel hello přihlášení do aplikace hello pomocí služby Azure AD jako zprostředkovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="08639-107">Sign hello user into hello app using Azure AD as hello identity provider.</span></span>
* <span data-ttu-id="08639-108">Zobrazí některé informace o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="08639-108">Display some information about hello user.</span></span>
* <span data-ttu-id="08639-109">Přihlašovací hello uživatele mimo aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="08639-109">Sign hello user out of hello app.</span></span>

<span data-ttu-id="08639-110">V pořadí toodo to, budete muset:</span><span class="sxs-lookup"><span data-stu-id="08639-110">In order toodo this, you'll need to:</span></span>

1. <span data-ttu-id="08639-111">Zaregistrovat aplikaci s Azure AD</span><span class="sxs-lookup"><span data-stu-id="08639-111">Register an application with Azure AD</span></span>
2. <span data-ttu-id="08639-112">Nastavte své aplikace toouse hello ADAL4J knihovny.</span><span class="sxs-lookup"><span data-stu-id="08639-112">Set up your app toouse hello ADAL4J library.</span></span>
3. <span data-ttu-id="08639-113">Použijte hello ADAL4J knihovny tooissue přihlášení a odhlášení požadavky tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="08639-113">Use hello ADAL4J library tooissue sign-in and sign-out requests tooAzure AD.</span></span>
4. <span data-ttu-id="08639-114">Data o uživateli hello vytiskněte.</span><span class="sxs-lookup"><span data-stu-id="08639-114">Print out data about hello user.</span></span>

<span data-ttu-id="08639-115">spuštění, tooget [stáhnout kostru aplikace hello](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) nebo [stažení ukázky hello Dokončit](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="08639-115">tooget started, [download hello app skeleton](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) or [download hello completed sample](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).</span></span>  <span data-ttu-id="08639-116">Musíte také klienta služby Azure AD, ve které tooregister vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="08639-116">You'll also need an Azure AD tenant in which tooregister your application.</span></span>  <span data-ttu-id="08639-117">Pokud již, nemáte [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="08639-117">If you don't have one already, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

## <a name="1--register-an-application-with-azure-ad"></a><span data-ttu-id="08639-118">1.  Zaregistrovat aplikaci s Azure AD</span><span class="sxs-lookup"><span data-stu-id="08639-118">1.  Register an Application with Azure AD</span></span>
<span data-ttu-id="08639-119">tooenable tooauthenticate uživatelům aplikace, je nutné nejdříve tooregister novou aplikaci v klientovi.</span><span class="sxs-lookup"><span data-stu-id="08639-119">tooenable your app tooauthenticate users, you'll first need tooregister a new application in your tenant.</span></span>

1. <span data-ttu-id="08639-120">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="08639-120">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="08639-121">Na horním panelu hello, klikněte na tlačítko na vašem účtu a v části hello **Directory** vyberte klienta hello služby Active Directory, kde chcete tooregister vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="08639-121">On hello top bar, click on your account and under hello **Directory** list, choose hello Active Directory tenant where you wish tooregister your application.</span></span>
3. <span data-ttu-id="08639-122">Klikněte na **více služeb** v hello navigaci vlevo a zvolte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="08639-122">Click on **More Services** in hello left hand nav, and choose **Azure Active Directory**.</span></span>
4. <span data-ttu-id="08639-123">Klikněte na **registrace aplikace** a zvolte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="08639-123">Click on **App registrations** and choose **Add**.</span></span>
5. <span data-ttu-id="08639-124">Postupujte podle pokynů hello a vytvořte novou **webové aplikace nebo WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="08639-124">Follow hello prompts and create a new **Web Application and/or WebAPI**.</span></span>
  * <span data-ttu-id="08639-125">Hello **název** z hello aplikace popíše aplikaci tooend uživatelé</span><span class="sxs-lookup"><span data-stu-id="08639-125">hello **name** of hello application will describe your application tooend-users</span></span>
  * <span data-ttu-id="08639-126">Hello **přihlašovací adresa URL** je hello základní adresu URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="08639-126">hello **Sign-On URL** is hello base URL of your app.</span></span>  <span data-ttu-id="08639-127">kostru Hello výchozí hodnota je `http://localhost:8080/adal4jsample/`.</span><span class="sxs-lookup"><span data-stu-id="08639-127">hello skeleton's default is `http://localhost:8080/adal4jsample/`.</span></span>
6. <span data-ttu-id="08639-128">Po dokončení registrace AAD přiřadí vaší aplikace, jedinečné ID aplikace</span><span class="sxs-lookup"><span data-stu-id="08639-128">Once you've completed registration, AAD will assign your app a unique Application ID.</span></span>  <span data-ttu-id="08639-129">Tuto hodnotu budete potřebovat v dalších částech hello, takže zkopírujte jej z karty aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="08639-129">You'll need this value in hello next sections, so copy it from hello application tab.</span></span>
7. <span data-ttu-id="08639-130">Z hello **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte hello identifikátor ID URI aplikace.</span><span class="sxs-lookup"><span data-stu-id="08639-130">From hello **Settings** -> **Properties** page for your application, update hello App ID URI.</span></span> <span data-ttu-id="08639-131">Hello **identifikátor ID URI aplikace** je jedinečný identifikátor pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="08639-131">hello **App ID URI** is a unique identifier for your application.</span></span>  <span data-ttu-id="08639-132">konvence Hello je toouse `https://<tenant-domain>/<app-name>`, například `http://localhost:8080/adal4jsample/`.</span><span class="sxs-lookup"><span data-stu-id="08639-132">hello convention is toouse `https://<tenant-domain>/<app-name>`, e.g. `http://localhost:8080/adal4jsample/`.</span></span>

<span data-ttu-id="08639-133">Po vytvoření hello portálu pro vaši aplikaci **klíč** z hello **nastavení** stránky pro aplikace a zkopírujte ho dolů.</span><span class="sxs-lookup"><span data-stu-id="08639-133">Once in hello portal for your app create a **Key** from hello **Settings** page for your application and copy it down.</span></span>  <span data-ttu-id="08639-134">Budete ho potřebovat za chvíli.</span><span class="sxs-lookup"><span data-stu-id="08639-134">You will need it shortly.</span></span>

## <a name="2-set-up-your-app-toouse-adal4j-library-and-prerequisites-using-maven"></a><span data-ttu-id="08639-135">2. Nastavení aplikace toouse ADAL4J knihovny a požadavky pomocí nástroje Maven</span><span class="sxs-lookup"><span data-stu-id="08639-135">2. Set up your app toouse ADAL4J library and prerequisites using Maven</span></span>
<span data-ttu-id="08639-136">Zde nakonfigurujeme ADAL4J toouse hello ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="08639-136">Here, we'll configure ADAL4J toouse hello OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="08639-137">ADAL4J bude použité tooissue požadavků na přihlášení a odhlášení, spravovat hello uživatelské relace a získat informace o uživateli hello, mimo jiné.</span><span class="sxs-lookup"><span data-stu-id="08639-137">ADAL4J will be used tooissue sign-in and sign-out requests, manage hello user's session, and get information about hello user, amongst other things.</span></span>

* <span data-ttu-id="08639-138">V kořenovém adresáři hello projektu, otevírání nebo vytváření `pom.xml` a vyhledejte hello `// TODO: provide dependencies for Maven` a nahraďte hello následující:</span><span class="sxs-lookup"><span data-stu-id="08639-138">In hello root directory of your project, open/create `pom.xml` and locate hello `// TODO: provide dependencies for Maven` and replace with hello following:</span></span>

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



## <a name="3-create-hello-java-publicclient-file"></a><span data-ttu-id="08639-139">3. Vytvoření souboru Java PublicClient hello</span><span class="sxs-lookup"><span data-stu-id="08639-139">3. Create hello Java PublicClient file</span></span>
<span data-ttu-id="08639-140">Jak je uvedeno výše, použijeme hello rozhraní Graph API tooget data o hello přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="08639-140">As indicated above, we will be using hello Graph API tooget data about hello logged in user.</span></span> <span data-ttu-id="08639-141">Pro tento toobe snadno nám jsme měli vytvořit i soubor toorepresent **objektu adresáře** a jednotlivých souborů toorepresent hello **uživatele** tak, aby hello ú vzor Java lze použít.</span><span class="sxs-lookup"><span data-stu-id="08639-141">For this toobe easy for us we should create both a file toorepresent a **Directory Object** and an individual file toorepresent hello **User** so that hello OO pattern of Java can be used.</span></span>

* <span data-ttu-id="08639-142">Vytvořte soubor s názvem `DirectoryObject.java` který použijeme toostore základní data o žádné DirectoryObject (můžete být volné toouse to později pro jiné dotazy grafu, může provádět).</span><span class="sxs-lookup"><span data-stu-id="08639-142">Create a file called `DirectoryObject.java` which we will use toostore basic data about any DirectoryObject (you can feel free toouse this later for any other Graph Queries you may do).</span></span> <span data-ttu-id="08639-143">Vám může vyjímání a vkládání toto z následujícího seznamu:</span><span class="sxs-lookup"><span data-stu-id="08639-143">You can cut/paste this from below:</span></span>

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


## <a name="compile-and-run-hello-sample"></a><span data-ttu-id="08639-144">Zkompilování a spuštění ukázkových hello</span><span class="sxs-lookup"><span data-stu-id="08639-144">Compile and run hello sample</span></span>
<span data-ttu-id="08639-145">Změňte zpět na tooyour kořenového adresáře a spusťte následující příkaz toobuild hello ukázka můžete zadat pouze společně s použitím hello `maven`.</span><span class="sxs-lookup"><span data-stu-id="08639-145">Change back out tooyour root directory and run hello following command toobuild hello sample you just put together using `maven`.</span></span> <span data-ttu-id="08639-146">Tímto dojde k použití hello `pom.xml` soubor jste napsali závislosti.</span><span class="sxs-lookup"><span data-stu-id="08639-146">This will use hello `pom.xml` file you wrote for dependencies.</span></span>

`$ mvn package`

<span data-ttu-id="08639-147">Teď byste měli mít `adal4jsample.war` ve vaší `/targets` adresáře.</span><span class="sxs-lookup"><span data-stu-id="08639-147">You should now have a `adal4jsample.war` file in your `/targets` directory.</span></span> <span data-ttu-id="08639-148">Může nasazení, v kontejneru vaší Tomcat a navštivte adresu URL hello</span><span class="sxs-lookup"><span data-stu-id="08639-148">You may deploy that in your Tomcat container and visit hello URL</span></span> 

`http://localhost:8080/adal4jsample/`

> [!NOTE]
> <span data-ttu-id="08639-149">Je velmi snadné toodeploy WAR s nejnovější serverů Tomcat hello.</span><span class="sxs-lookup"><span data-stu-id="08639-149">It is very easy toodeploy a WAR with hello latest Tomcat servers.</span></span> <span data-ttu-id="08639-150">Jednoduše přejděte příliš`http://localhost:8080/manager/` a postupujte podle pokynů hello na odesílání vašeho '' adal4jsample.war' souboru.</span><span class="sxs-lookup"><span data-stu-id="08639-150">Simply navigate too`http://localhost:8080/manager/` and follow hello instructions on uploading your ``adal4jsample.war\` file.</span></span> <span data-ttu-id="08639-151">Zruší autodeploy pro vás správný koncový bod hello.</span><span class="sxs-lookup"><span data-stu-id="08639-151">It will autodeploy for you with hello correct endpoint.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="08639-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="08639-152">Next Steps</span></span>
<span data-ttu-id="08639-153">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="08639-153">Congratulations!</span></span> <span data-ttu-id="08639-154">Teď máte funkční aplikaci Java, která má hello možnost tooauthenticate uživatelé, bezpečně volání webového rozhraní API pomocí OAuth 2.0 a získat základní informace o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="08639-154">You now have a working Java application that has hello ability tooauthenticate users, securely call Web APIs using OAuth 2.0, and get basic information about hello user.</span></span>  <span data-ttu-id="08639-155">Pokud jste to ještě neudělali, nyní je čas toopopulate hello vašeho klienta s některými uživateli.</span><span class="sxs-lookup"><span data-stu-id="08639-155">If you haven't already, now is hello time toopopulate your tenant with some users.</span></span>

<span data-ttu-id="08639-156">Pro referenci hello dokončit ukázka (bez vašich hodnot nastavení) [je k dispozici jako ZIP zde](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), nebo ji můžete klonovat z Githubu:</span><span class="sxs-lookup"><span data-stu-id="08639-156">For reference, hello completed sample (without your configuration values) [is provided as a .zip here](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

