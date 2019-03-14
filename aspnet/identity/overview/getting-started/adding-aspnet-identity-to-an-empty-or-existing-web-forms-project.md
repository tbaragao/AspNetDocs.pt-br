---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Adicionar projeto de formulários do ASP.NET Identity a uma Web vazia ou existente | Microsoft Docs
author: raquelsa
description: Este tutorial mostra como adicionar a identidade do ASP.NET (o novo sistema de associação do ASP.NET) para um aplicativo ASP.NET. Quando você cria um novo Web Forms ou MVC...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: cd28cc68db96b52eb205b8764aa2af014ffad9c3
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57038273"
---
# <a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a><span data-ttu-id="e25c5-104">Adição de Identidade do ASP.NET a um projeto vazio ou existente do Web Forms</span><span class="sxs-lookup"><span data-stu-id="e25c5-104">Adding ASP.NET Identity to an Empty or Existing Web Forms Project</span></span>


> <span data-ttu-id="e25c5-105">Este tutorial mostra como adicionar [ASP.NET Identity](introduction-to-aspnet-identity.md) (o novo sistema de associação do ASP.NET) para um aplicativo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e25c5-105">This tutorial shows you how to add [ASP.NET Identity](introduction-to-aspnet-identity.md) (the new membership system for ASP.NET) to an ASP.NET application.</span></span>
> 
> <span data-ttu-id="e25c5-106">Quando você cria um novo projeto de Web Forms ou MVC no Visual Studio 2017 RTM com contas individuais, Visual Studio instalará todos os pacotes necessários e adicionar todas as classes necessárias para você.</span><span class="sxs-lookup"><span data-stu-id="e25c5-106">When you create a new Web Forms or MVC project in Visual Studio 2017 RTM with Individual Accounts, Visual Studio will install all the required packages and add all necessary classes for you.</span></span> <span data-ttu-id="e25c5-107">Este tutorial irá ilustrar as etapas para adicionar suporte à identidade do ASP.NET ao seu projeto de formulários da Web existente ou um novo projeto vazio.</span><span class="sxs-lookup"><span data-stu-id="e25c5-107">This tutorial will illustrate the steps to add ASP.NET Identity support to your existing Web Forms project or a new empty project.</span></span> <span data-ttu-id="e25c5-108">Vou descrever todos os pacotes do NuGet que necessários para instalar e as classes que você precisa adicionar.</span><span class="sxs-lookup"><span data-stu-id="e25c5-108">I will outline all the NuGet packages you need to install, and classes you need to add.</span></span> <span data-ttu-id="e25c5-109">Entrarei em formulários da Web de exemplo para registrar novos usuários e registro em log enquanto destaca todas as APIs de ponto de entrada principal para autenticação e gerenciamento de usuário.</span><span class="sxs-lookup"><span data-stu-id="e25c5-109">I will go over sample Web Forms for registering new users and logging in while highlighting all main entry point APIs for user management and authentication.</span></span> <span data-ttu-id="e25c5-110">Este exemplo usará a implementação do padrão de identidade do ASP.NET para o armazenamento de dados SQL que é criado no Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e25c5-110">This sample will use the ASP.NET Identity default implementation for SQL data storage which is built on Entity Framework.</span></span> <span data-ttu-id="e25c5-111">Neste tutorial, usaremos o LocalDB para o banco de dados SQL.</span><span class="sxs-lookup"><span data-stu-id="e25c5-111">This tutorial, we will use LocalDB for the SQL database.</span></span>
> 

## <a name="get-started-with-aspnet-identity"></a><span data-ttu-id="e25c5-112">Introdução ao ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="e25c5-112">Get started with ASP.NET Identity</span></span>

1. <span data-ttu-id="e25c5-113">Comece instalando e executando [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="e25c5-113">Start by installing and running [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="e25c5-114">Selecione **novo projeto** desde o início página, ou você pode usar o menu e selecione **arquivo**e então **novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="e25c5-114">Select **New Project** from the Start page, or you can use the menu and select **File**, and then **New Project**.</span></span>
3. <span data-ttu-id="e25c5-115">No painel esquerdo, expanda **Visual C#** , em seguida, selecione **Web**, em seguida, **aplicativo Web ASP.NET (.Net Framework)**.</span><span class="sxs-lookup"><span data-stu-id="e25c5-115">In the left pane, expand **Visual C#**, then select **Web**, then **ASP.NET Web Application (.Net Framework)**.</span></span> <span data-ttu-id="e25c5-116">Nomeie o projeto "WebFormsIdentity" e selecione **Okey**.</span><span class="sxs-lookup"><span data-stu-id="e25c5-116">Name your project "WebFormsIdentity" and select **OK**.</span></span>
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image17.png)
4. <span data-ttu-id="e25c5-117">Na caixa de diálogo **Novo Aplicativo Web ASP.NET**, selecione o modelo **Vazio**.</span><span class="sxs-lookup"><span data-stu-id="e25c5-117">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   <span data-ttu-id="e25c5-118">Observe que o **alterar autenticação** botão está desabilitado e não há suporte de autenticação é fornecida neste modelo.</span><span class="sxs-lookup"><span data-stu-id="e25c5-118">Notice the **Change Authentication** button is disabled and no authentication support is provided in this template.</span></span> <span data-ttu-id="e25c5-119">Os modelos de Web Forms, MVC e API da Web permitem que você selecionar a abordagem de autenticação.</span><span class="sxs-lookup"><span data-stu-id="e25c5-119">The Web Forms, MVC and Web API templates allow you to select the authentication approach.</span></span>

## <a name="add-identity-packages-to-your-app"></a><span data-ttu-id="e25c5-120">Adicionar pacotes de identidade ao seu aplicativo</span><span class="sxs-lookup"><span data-stu-id="e25c5-120">Add Identity packages to your app</span></span>

<span data-ttu-id="e25c5-121">No Gerenciador de soluções, clique em seu projeto e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e25c5-121">In Solution Explorer, right-click your project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="e25c5-122">Procure e instale o **ASPNET** pacote.</span><span class="sxs-lookup"><span data-stu-id="e25c5-122">Search for and install the **Microsoft.AspNet.Identity.EntityFramework** package.</span></span> 
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image15.png)
  
<span data-ttu-id="e25c5-123">Observe que esse pacote instalará os pacotes de dependência: **EntityFramework** e **identidade Microsoft ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e25c5-123">Note that this package will install the dependency packages: **EntityFramework** and **Microsoft ASP.NET Identity Core**.</span></span>

## <a name="add-a-web-form-to-register-users"></a><span data-ttu-id="e25c5-124">Adicionar um formulário da web para registrar usuários</span><span class="sxs-lookup"><span data-stu-id="e25c5-124">Add a web form to register users</span></span>

1. <span data-ttu-id="e25c5-125">Na **Gerenciador de soluções**, clique em seu projeto e selecione **Add**e então **Web Form**.</span><span class="sxs-lookup"><span data-stu-id="e25c5-125">In **Solution Explorer**, right-click your project and select **Add**, and then **Web Form**.</span></span>
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. <span data-ttu-id="e25c5-126">No **especificar nome para o Item** caixa de diálogo, o nome do novo formulário da web **registrar**e, em seguida, selecione **Okey**</span><span class="sxs-lookup"><span data-stu-id="e25c5-126">In the **Specify Name for Item** dialog box, name the new web form **Register**, and then select **OK**</span></span>
3. <span data-ttu-id="e25c5-127">Substitua a marcação em gerado *Register* arquivo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="e25c5-127">Replace the markup in the generated *Register.aspx* file with the code below.</span></span> <span data-ttu-id="e25c5-128">As alterações de código são realçadas.</span><span class="sxs-lookup"><span data-stu-id="e25c5-128">The code changes are highlighted.</span></span> 

    [!code-html[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > <span data-ttu-id="e25c5-129">Isso é apenas uma versão simplificada do *Register* arquivo que é criado quando você cria um novo projeto de Web Forms do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e25c5-129">This is just a simplified version of the *Register.aspx* file that is created when you create a new ASP.NET Web Forms project.</span></span> <span data-ttu-id="e25c5-130">A marcação acima adiciona campos de formulário e um botão para registrar um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="e25c5-130">The markup above adds form fields and a button to register a new user.</span></span>
4. <span data-ttu-id="e25c5-131">Abra o *Register.aspx.cs* de arquivo e substitua o conteúdo do arquivo pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="e25c5-131">Open the *Register.aspx.cs* file and replace the contents of the file with the following code:</span></span>

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. <span data-ttu-id="e25c5-132">O código acima é uma versão simplificada do *Register.aspx.cs* arquivo que é criado quando você cria um novo projeto de Web Forms do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e25c5-132">The code above is a simplified version of the *Register.aspx.cs* file that is created when you create a new ASP.NET Web Forms project.</span></span>
    > 2. <span data-ttu-id="e25c5-133">O *IdentityUser* classe é a implementação EntityFramework padrão de *IUser* interface.</span><span class="sxs-lookup"><span data-stu-id="e25c5-133">The *IdentityUser* class is the default EntityFramework implementation of the *IUser* interface.</span></span> <span data-ttu-id="e25c5-134">*IUser* interface é a interface mínima para um usuário no ASP.NET Core de identidade.</span><span class="sxs-lookup"><span data-stu-id="e25c5-134">*IUser* interface is the minimal interface for a user on ASP.NET Identity Core.</span></span>
    > 3. <span data-ttu-id="e25c5-135">O *UserStore* classe é a implementação do EntityFramework padrão de um repositório de usuário.</span><span class="sxs-lookup"><span data-stu-id="e25c5-135">The *UserStore* class is the default EntityFramework implementation of a user store.</span></span> <span data-ttu-id="e25c5-136">Essa classe implementa as interfaces mínimas da identidade do ASP.NET Core: *IUserStore*, *IUserLoginStore*, *IUserClaimStore* e *IUserRoleStore*.</span><span class="sxs-lookup"><span data-stu-id="e25c5-136">This class implements the ASP.NET Identity Core's minimal interfaces: *IUserStore*, *IUserLoginStore*, *IUserClaimStore* and *IUserRoleStore*.</span></span>
    > 4. <span data-ttu-id="e25c5-137">O *UserManager* classe expõe usuário relacionadas a APIs que salvará automaticamente as alterações para o *UserStore*.</span><span class="sxs-lookup"><span data-stu-id="e25c5-137">The *UserManager* class exposes user related APIs which will automatically save changes to the *UserStore*.</span></span>
    > 5. <span data-ttu-id="e25c5-138">O *IdentityResult* classe representa o resultado de uma operação de identidade.</span><span class="sxs-lookup"><span data-stu-id="e25c5-138">The *IdentityResult* class represents the result of an identity operation.</span></span>
5. <span data-ttu-id="e25c5-139">Na **Gerenciador de soluções**, clique em seu projeto e selecione **Add**, **adicionar pasta ASP.NET** e, em seguida, **aplicativo\_dados**.</span><span class="sxs-lookup"><span data-stu-id="e25c5-139">In **Solution Explorer**, right-click your project and select **Add**, **Add ASP.NET Folder** and then **App\_Data**.</span></span>
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. <span data-ttu-id="e25c5-140">Abra o *Web. config* arquivo e adicione uma entrada de cadeia de caracteres de conexão para o banco de dados que usaremos para armazenar informações do usuário.</span><span class="sxs-lookup"><span data-stu-id="e25c5-140">Open the *Web.config* file and add a connection string entry for the database we will use to store user information.</span></span> <span data-ttu-id="e25c5-141">O banco de dados será criado no tempo de execução por EntityFramework para as entidades de identidade.</span><span class="sxs-lookup"><span data-stu-id="e25c5-141">The database will be created at runtime by EntityFramework for the Identity entities.</span></span> <span data-ttu-id="e25c5-142">A cadeia de caracteres de conexão é semelhante a um criado quando você cria um novo projeto de formulários da Web.</span><span class="sxs-lookup"><span data-stu-id="e25c5-142">The connection string is similar to one created for you when you create a new Web Forms project.</span></span> <span data-ttu-id="e25c5-143">O código realçado mostra a marcação, que você deve adicionar:</span><span class="sxs-lookup"><span data-stu-id="e25c5-143">The highlighted code shows the markup you should add:</span></span>

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > <span data-ttu-id="e25c5-144">Para o Visual Studio 2015 ou superior, substitua `(localdb)\v11.0` com `(localdb)\MSSQLLocalDB` em sua cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="e25c5-144">For Visual Studio 2015 or higher, replace `(localdb)\v11.0` with `(localdb)\MSSQLLocalDB` in your connection string.</span></span>
    
7. <span data-ttu-id="e25c5-145">Clique com botão direito no arquivo *Register* em seu projeto e selecione **definir como página inicial**.</span><span class="sxs-lookup"><span data-stu-id="e25c5-145">Right click file *Register.aspx* in your project and select **Set as Start Page**.</span></span> <span data-ttu-id="e25c5-146">Pressione Ctrl + F5 para compilar e executar o aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="e25c5-146">Press Ctrl + F5 to build and run the web application.</span></span> <span data-ttu-id="e25c5-147">Insira um novo nome de usuário e senha e, em seguida, selecione **registrar**.</span><span class="sxs-lookup"><span data-stu-id="e25c5-147">Enter a new user name and password and then select **Register**.</span></span>
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > <span data-ttu-id="e25c5-148">Identidade do ASP.NET tem suporte para validação e neste exemplo, você pode verificar o comportamento padrão no usuário e senha validadores que vêm de pacote do núcleo de identidade.</span><span class="sxs-lookup"><span data-stu-id="e25c5-148">ASP.NET Identity has support for validation and in this sample you can verify the default behavior on User and Password validators that come from the Identity Core package.</span></span> <span data-ttu-id="e25c5-149">O validador padrão para o usuário (`UserValidator`) tem uma propriedade `AllowOnlyAlphanumericUserNames` que tem o valor padrão definido como `true`.</span><span class="sxs-lookup"><span data-stu-id="e25c5-149">The default validator for User (`UserValidator`) has a property `AllowOnlyAlphanumericUserNames` that has default value set to `true`.</span></span> <span data-ttu-id="e25c5-150">O validador de padrão para a senha (`MinimumLengthValidator`) garante que essa senha tem pelo menos 6 caracteres.</span><span class="sxs-lookup"><span data-stu-id="e25c5-150">The default validator for Password (`MinimumLengthValidator`) ensures that password has at least 6 characters.</span></span> <span data-ttu-id="e25c5-151">Esses validadores são propriedades no `UserManager` que pode ser substituído se você quiser ter validação personalizada,</span><span class="sxs-lookup"><span data-stu-id="e25c5-151">These validators are properties on `UserManager` that can be overridden if you want to have custom validation,</span></span>

## <a name="verify-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a><span data-ttu-id="e25c5-152">Verifique se as tabelas geradas pelo Entity Framework e o banco de dados de identidade do LocalDb</span><span class="sxs-lookup"><span data-stu-id="e25c5-152">Verify the LocalDb Identity database and tables generated by Entity Framework</span></span>

1. <span data-ttu-id="e25c5-153">No **modo de exibição** menu, selecione **Gerenciador de servidores**.</span><span class="sxs-lookup"><span data-stu-id="e25c5-153">In the **View** menu, select **Server Explorer**.</span></span>
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. <span data-ttu-id="e25c5-154">Expandir **DefaultConnection (WebFormsIdentity)**, expanda **tabelas**, clique com botão direito **AspNetUsers** e, em seguida, selecione **Mostrar dados da tabela**.</span><span class="sxs-lookup"><span data-stu-id="e25c5-154">Expand **DefaultConnection (WebFormsIdentity)**, expand **Tables**, right-click **AspNetUsers** and then select **Show Table Data**.</span></span>
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configure-the-application-for-owin-authentication"></a><span data-ttu-id="e25c5-155">Configurar o aplicativo para autenticação OWIN</span><span class="sxs-lookup"><span data-stu-id="e25c5-155">Configure the application for OWIN authentication</span></span>

<span data-ttu-id="e25c5-156">Neste ponto, apenas adicionamos suporte para a criação de usuários.</span><span class="sxs-lookup"><span data-stu-id="e25c5-156">At this point we have only added support for creating users.</span></span> <span data-ttu-id="e25c5-157">Agora, vamos demonstrar como é possível adicionar a autenticação para fazer logon de um usuário.</span><span class="sxs-lookup"><span data-stu-id="e25c5-157">Now, we are going to demonstrate how we can add authentication to login a user.</span></span> <span data-ttu-id="e25c5-158">Identidade do ASP.NET usa a autenticação do Microsoft OWIN middleware para autenticação de formulários.</span><span class="sxs-lookup"><span data-stu-id="e25c5-158">ASP.NET Identity uses Microsoft OWIN Authentication middleware for forms authentication.</span></span> <span data-ttu-id="e25c5-159">A autenticação de Cookie do OWIN é um cookie e declarações de mecanismo de autenticação baseada em que pode ser usado por qualquer estrutura hospedada no [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) ou IIS.</span><span class="sxs-lookup"><span data-stu-id="e25c5-159">The OWIN Cookie Authentication is a cookie and claims based authentication mechanism that can be used by any framework hosted on [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) or IIS.</span></span> <span data-ttu-id="e25c5-160">Com esse modelo, os mesmos pacotes de autenticação podem ser usados em várias estruturas, incluindo o ASP.NET MVC e Web Forms.</span><span class="sxs-lookup"><span data-stu-id="e25c5-160">With this model, the same authentication packages can be used across multiple frameworks including ASP.NET MVC and Web Forms.</span></span> <span data-ttu-id="e25c5-161">Para obter mais informações sobre o projeto Katana e como executar o middleware em uma consulte independente de host [Introdução ao projeto Katana](https://msdn.microsoft.com/magazine/dn451439.aspx).</span><span class="sxs-lookup"><span data-stu-id="e25c5-161">For more information on project Katana and how to run middleware in a host agnostic see [Getting Started with the Katana Project](https://msdn.microsoft.com/magazine/dn451439.aspx).</span></span>

## <a name="install-authentication-packages-to-your-application"></a><span data-ttu-id="e25c5-162">Instalar pacotes de autenticação ao seu aplicativo</span><span class="sxs-lookup"><span data-stu-id="e25c5-162">Install authentication packages to your application</span></span>

1. <span data-ttu-id="e25c5-163">No Gerenciador de soluções, clique em seu projeto e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e25c5-163">In Solution Explorer, right-click your project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="e25c5-164">Procure e instale o ***Microsoft.AspNet.Identity.Owin*** pacote.</span><span class="sxs-lookup"><span data-stu-id="e25c5-164">Search for and install the ***Microsoft.AspNet.Identity.Owin*** package.</span></span> 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image16.png)

2. <span data-ttu-id="e25c5-165">Procure e instale o ***systemweb*** pacote.</span><span class="sxs-lookup"><span data-stu-id="e25c5-165">Search for and install the ***Microsoft.Owin.Host.SystemWeb*** package.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e25c5-166">O **Microsoft.Aspnet.Identity.Owin** pacote contém um conjunto de classes de extensão do OWIN para gerenciar e configurar o middleware de autenticação OWIN a ser consumida por pacotes do ASP.NET Core de identidade.</span><span class="sxs-lookup"><span data-stu-id="e25c5-166">The **Microsoft.Aspnet.Identity.Owin** package contains a set of OWIN extension classes to manage and configure OWIN authentication middleware to be consumed by ASP.NET Identity Core packages.</span></span>
    > <span data-ttu-id="e25c5-167">O **systemweb** pacote contém um servidor OWIN que permite que aplicativos baseados no OWIN sejam executados no IIS usando o pipeline de solicitação do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e25c5-167">The **Microsoft.Owin.Host.SystemWeb** package contains an OWIN server that enables OWIN-based applications to run on IIS using the ASP.NET request pipeline.</span></span> <span data-ttu-id="e25c5-168">Para obter mais informações, consulte [pipeline integrado do Middleware do OWIN no IIS](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="e25c5-168">For more information see [OWIN Middleware in the IIS integrated pipeline](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).</span></span>

## <a name="add-owin-startup-and-authentication-configuration-classes"></a><span data-ttu-id="e25c5-169">Adicionar classes de configuração de inicialização e autenticação OWIN</span><span class="sxs-lookup"><span data-stu-id="e25c5-169">Add OWIN startup and authentication configuration classes</span></span>

1. <span data-ttu-id="e25c5-170">Na **Gerenciador de soluções**, clique em seu projeto, selecione **Add**e então **Add New Item**.</span><span class="sxs-lookup"><span data-stu-id="e25c5-170">In **Solution Explorer**, right-click your project, select **Add**, and then **Add New Item**.</span></span> <span data-ttu-id="e25c5-171">No diálogo de caixa de texto de pesquisa, digite "*owin*".</span><span class="sxs-lookup"><span data-stu-id="e25c5-171">In the search text box dialog, type "*owin*".</span></span> <span data-ttu-id="e25c5-172">Nomeie a classe "*inicialização*" e selecione **Add**.</span><span class="sxs-lookup"><span data-stu-id="e25c5-172">Name the class "*Startup*" and select **Add**.</span></span> 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. <span data-ttu-id="e25c5-173">No arquivo Startup.cs, adicione o código realçado abaixo para configurar a autenticação de cookie do OWIN.</span><span class="sxs-lookup"><span data-stu-id="e25c5-173">In the Startup.cs file, add the highlighted code shown below to configure OWIN cookie authentication.</span></span>

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > <span data-ttu-id="e25c5-174">Essa classe contém o `OwinStartup` atributo para especificar a classe de inicialização do OWIN.</span><span class="sxs-lookup"><span data-stu-id="e25c5-174">This class contains the `OwinStartup` attribute for specifying the OWIN startup class.</span></span> <span data-ttu-id="e25c5-175">Todos os aplicativos OWIN tem uma classe de inicialização na qual você pode especificar componentes para o pipeline do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e25c5-175">Every OWIN application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="e25c5-176">Ver [detecção de classe de inicialização OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) para obter mais informações sobre esse modelo.</span><span class="sxs-lookup"><span data-stu-id="e25c5-176">See [OWIN Startup Class Detection](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) for more info on this model.</span></span>

## <a name="add-web-forms-for-registering-and-signing-in-users"></a><span data-ttu-id="e25c5-177">Adicionar formulários da web para o registro e os usuários de assinatura</span><span class="sxs-lookup"><span data-stu-id="e25c5-177">Add web forms for registering and signing in users</span></span>

1. <span data-ttu-id="e25c5-178">Abra o *Register.aspx.cs* de arquivo e adicione o seguinte código que conecta o usuário quando o registro terá êxito.</span><span class="sxs-lookup"><span data-stu-id="e25c5-178">Open the *Register.aspx.cs* file and add the following code which signs in the user when registration succeeds.</span></span>

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - <span data-ttu-id="e25c5-179">Como o ASP.NET Identity e autenticação de Cookie OWIN são sistema baseada em declarações, o framework exige que o desenvolvedor de aplicativo gerar uma [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) para o usuário.</span><span class="sxs-lookup"><span data-stu-id="e25c5-179">Since ASP.NET Identity and OWIN Cookie Authentication are claims based system, the framework requires the app developer to generate a [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) for the user.</span></span> <span data-ttu-id="e25c5-180">ClaimsIdentity tem informações sobre todas as declarações do usuário, como as funções que o usuário pertence.</span><span class="sxs-lookup"><span data-stu-id="e25c5-180">ClaimsIdentity has information about all the claims for the user such as what Roles the user belongs to.</span></span> <span data-ttu-id="e25c5-181">Você também pode adicionar mais declarações para o usuário neste estágio.</span><span class="sxs-lookup"><span data-stu-id="e25c5-181">You can also add more claims for the user at this stage.</span></span>
    > - <span data-ttu-id="e25c5-182">Você pode conectar o usuário usando o AuthenticationManager do OWIN e chamar `SignIn` e passando o ClaimsIdentity conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="e25c5-182">You can sign in the user by using the AuthenticationManager from OWIN and calling `SignIn` and passing in the ClaimsIdentity as shown above.</span></span> <span data-ttu-id="e25c5-183">Esse código será a entrada do usuário e gerar um cookie também.</span><span class="sxs-lookup"><span data-stu-id="e25c5-183">This code will sign in the user and generate a cookie as well.</span></span> <span data-ttu-id="e25c5-184">Essa chamada é análoga à [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) usado pelas [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) módulo.</span><span class="sxs-lookup"><span data-stu-id="e25c5-184">This call is analogous to [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) used by the [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) module.</span></span>
2. <span data-ttu-id="e25c5-185">Na **Gerenciador de soluções**, clique em seu projeto, selecione **Add**e então **Web Form**.</span><span class="sxs-lookup"><span data-stu-id="e25c5-185">In **Solution Explorer**, right-click your project, select **Add**, and then **Web Form**.</span></span> <span data-ttu-id="e25c5-186">Nomeie o formulário da web **Login**.</span><span class="sxs-lookup"><span data-stu-id="e25c5-186">Name the web form **Login**.</span></span>
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. <span data-ttu-id="e25c5-187">Substitua o conteúdo do *login. aspx* arquivo pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="e25c5-187">Replace the contents of the *Login.aspx* file with the following code:</span></span>

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. <span data-ttu-id="e25c5-188">Substitua o conteúdo do *Login.aspx.cs* arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="e25c5-188">Replace the contents of the *Login.aspx.cs* file with the following:</span></span>

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - <span data-ttu-id="e25c5-189">O `Page_Load` agora verifica o status do usuário atual e entra em ação com base em seu `Context.User.Identity.IsAuthenticated` status.</span><span class="sxs-lookup"><span data-stu-id="e25c5-189">The `Page_Load` now checks for the status of current user and takes action based on its `Context.User.Identity.IsAuthenticated` status.</span></span>
    >     <span data-ttu-id="e25c5-190">**Exibição registrada no nome de usuário** : A estrutura de identidade do Microsoft ASP.NET tenha adicionado os métodos de extensão em [IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) que permite que você obtenha o `UserName` e `UserId` para o usuário conectado.</span><span class="sxs-lookup"><span data-stu-id="e25c5-190">**Display Logged in User Name** : The Microsoft ASP.NET Identity Framework has added extension methods on [System.Security.Principal.IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) that allows you to get the `UserName` and `UserId`  for the logged in User.</span></span> <span data-ttu-id="e25c5-191">Esses métodos de extensão são definidos na `Microsoft.AspNet.Identity.Core` assembly.</span><span class="sxs-lookup"><span data-stu-id="e25c5-191">These extension methods are defined in the `Microsoft.AspNet.Identity.Core` assembly.</span></span> <span data-ttu-id="e25c5-192">Esses métodos de extensão são a substituição de [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .</span><span class="sxs-lookup"><span data-stu-id="e25c5-192">These extension methods are the replacement for [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .</span></span>
    > - <span data-ttu-id="e25c5-193">Método SignIn: `This` substitui o método anterior `CreateUser_Click` método neste exemplo e agora os sinais do usuário depois de criar com êxito o usuário.</span><span class="sxs-lookup"><span data-stu-id="e25c5-193">SignIn method: `This` method replaces the previous `CreateUser_Click` method in this sample and now signs in the user after successfully creating the user.</span></span>   
    >   <span data-ttu-id="e25c5-194">A estrutura do Microsoft OWIN tenha adicionado os métodos de extensão em `System.Web.HttpContext` que permite que você obtenha uma referência a um `IOwinContext`.</span><span class="sxs-lookup"><span data-stu-id="e25c5-194">The Microsoft OWIN Framework has added extension methods on `System.Web.HttpContext` that allows you to get a reference to an `IOwinContext`.</span></span> <span data-ttu-id="e25c5-195">Esses métodos de extensão são definidos no `Microsoft.Owin.Host.SystemWeb` assembly.</span><span class="sxs-lookup"><span data-stu-id="e25c5-195">These extension methods are defined in `Microsoft.Owin.Host.SystemWeb` assembly.</span></span> <span data-ttu-id="e25c5-196">O `OwinContext` classe expõe um `IAuthenticationManager` propriedade que representa a funcionalidade de middleware de autenticação disponível na solicitação atual.</span><span class="sxs-lookup"><span data-stu-id="e25c5-196">The `OwinContext` class exposes an `IAuthenticationManager` property that represents the Authentication middleware functionality available on the current request.</span></span> <span data-ttu-id="e25c5-197">Você pode conectar o usuário usando o `AuthenticationManager` do OWIN e chamar `SignIn` e passando o `ClaimsIdentity` conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="e25c5-197">You can sign in the user by using the `AuthenticationManager` from OWIN and calling `SignIn` and passing in the `ClaimsIdentity` as shown above.</span></span> <span data-ttu-id="e25c5-198">Como o ASP.NET Identity e autenticação de Cookie OWIN são sistema baseado em declarações, o framework requer que o aplicativo para gerar um `ClaimsIdentity` para o usuário.</span><span class="sxs-lookup"><span data-stu-id="e25c5-198">Because ASP.NET Identity and OWIN Cookie Authentication are claims-based system, the framework requires the app to generate a `ClaimsIdentity` for the user.</span></span> <span data-ttu-id="e25c5-199">O `ClaimsIdentity` tem informações sobre todas as declarações para o usuário, como as funções que o usuário pertence.</span><span class="sxs-lookup"><span data-stu-id="e25c5-199">The `ClaimsIdentity` has information about all the claims for the user, such as what roles the user belongs to.</span></span> <span data-ttu-id="e25c5-200">Você também pode adicionar mais declarações para o usuário neste estágio, esse código será a entrada do usuário e gerar um cookie também.</span><span class="sxs-lookup"><span data-stu-id="e25c5-200">You can also add more claims for the user at this stage This code will sign in the user and generate a cookie as well.</span></span> <span data-ttu-id="e25c5-201">Essa chamada é análoga à [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) usado pelas [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) módulo.</span><span class="sxs-lookup"><span data-stu-id="e25c5-201">This call is analogous to [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) used by the [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) module.</span></span>
    > - <span data-ttu-id="e25c5-202">`SignOut` método: Obtém uma referência para o `AuthenticationManager` do OWIN e chamadas `SignOut`.</span><span class="sxs-lookup"><span data-stu-id="e25c5-202">`SignOut` method: Gets a reference to the `AuthenticationManager` from OWIN and calls `SignOut`.</span></span> <span data-ttu-id="e25c5-203">Isso é análogo à [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) método usado pelas [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) módulo.</span><span class="sxs-lookup"><span data-stu-id="e25c5-203">This is analogous to [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) method used by the [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) module.</span></span>
5. <span data-ttu-id="e25c5-204">Pressione **Ctrl + F5** para compilar e executar o aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="e25c5-204">Press **Ctrl + F5** to build and run the web application.</span></span> <span data-ttu-id="e25c5-205">Insira um novo nome de usuário e senha e, em seguida, selecione **registrar**.</span><span class="sxs-lookup"><span data-stu-id="e25c5-205">Enter a new user name and password and then select **Register**.</span></span>
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   <span data-ttu-id="e25c5-206">Observação: Neste ponto, o novo usuário é criado e conectado.</span><span class="sxs-lookup"><span data-stu-id="e25c5-206">Note: At this point, the new user is created and logged in.</span></span>
6. <span data-ttu-id="e25c5-207">Selecione o **fazer logoff** botão.</span><span class="sxs-lookup"><span data-stu-id="e25c5-207">Select the **Log out** button.</span></span> <span data-ttu-id="e25c5-208">Você será redirecionado para o Log no formulário.</span><span class="sxs-lookup"><span data-stu-id="e25c5-208">You are redirected to the Log in form.</span></span>
7. <span data-ttu-id="e25c5-209">Insira um nome de usuário inválido ou a senha e selecione o **faça logon no** botão.</span><span class="sxs-lookup"><span data-stu-id="e25c5-209">Enter an invalid user name or password and select the **Log in** button.</span></span> 
   <span data-ttu-id="e25c5-210">O `UserManager.Find` método retornará null e a mensagem de erro: " *Nome de usuário inválido ou a senha* "será exibida.</span><span class="sxs-lookup"><span data-stu-id="e25c5-210">The `UserManager.Find`  method will return null and the error message: " *Invalid user name or password* " will be displayed.</span></span>
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)