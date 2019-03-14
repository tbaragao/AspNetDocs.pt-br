---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: Implantando associações de função de banco de dados em ambientes de teste | Microsoft Docs
author: jrjlee
description: Este tópico descreve como adicionar contas de usuário a funções de banco de dados como parte de uma implantação de solução para um ambiente de teste. Quando você implanta uma solução que contém...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: 07442b7a016ce2a32b1c9e7f44010517e40d7189
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57035493"
---
<a name="deploying-database-role-memberships-to-test-environments"></a><span data-ttu-id="69aea-104">Implantação das associações de função de banco de dados em ambientes de teste</span><span class="sxs-lookup"><span data-stu-id="69aea-104">Deploying Database Role Memberships to Test Environments</span></span>
====================
<span data-ttu-id="69aea-105">by [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="69aea-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="69aea-106">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="69aea-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="69aea-107">Este tópico descreve como adicionar contas de usuário a funções de banco de dados como parte de uma implantação de solução para um ambiente de teste.</span><span class="sxs-lookup"><span data-stu-id="69aea-107">This topic describes how to add user accounts to database roles as part of a solution deployment to a test environment.</span></span>
> 
> <span data-ttu-id="69aea-108">Quando você implanta uma solução que contém um projeto de banco de dados em um ambiente de preparo ou produção, normalmente você não quer o desenvolvedor para automatizar a adição de contas de usuário às funções de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="69aea-108">When you deploy a solution containing a database project to a staging or production environment, you typically don't want the developer to automate the addition of user accounts to database roles.</span></span> <span data-ttu-id="69aea-109">Na maioria dos casos, o desenvolvedor não saberá quais contas de usuário precisam ser adicionados a quais funções de banco de dados, e esses requisitos podem mudar a qualquer momento.</span><span class="sxs-lookup"><span data-stu-id="69aea-109">In most cases, the developer won't know which user accounts need to be added to which database roles, and these requirements could change at any time.</span></span> <span data-ttu-id="69aea-110">No entanto, quando você implanta uma solução que contém um projeto de banco de dados para um desenvolvimento ou ambiente de teste, a situação geralmente é bastante diferente:</span><span class="sxs-lookup"><span data-stu-id="69aea-110">However, when you deploy a solution containing a database project to a development or test environment, the situation is usually rather different:</span></span>
> 
> - <span data-ttu-id="69aea-111">O desenvolvedor normalmente novamente implanta a solução de forma regular, com frequência várias vezes ao dia.</span><span class="sxs-lookup"><span data-stu-id="69aea-111">The developer typically re-deploys the solution on a regular basis, often several times a day.</span></span>
> - <span data-ttu-id="69aea-112">O banco de dados geralmente é novamente criado em cada implantação, o que significa que os usuários de banco de dados devem ser criados e adicionados a funções após cada implantação.</span><span class="sxs-lookup"><span data-stu-id="69aea-112">The database is typically re-created on every deployment, which means that database users must be created and added to roles after every deployment.</span></span>
> - <span data-ttu-id="69aea-113">Normalmente, o desenvolvedor tem controle total sobre o ambiente de desenvolvimento ou teste de destino.</span><span class="sxs-lookup"><span data-stu-id="69aea-113">The developer typically has full control over the target development or test environment.</span></span>
> 
> <span data-ttu-id="69aea-114">Nesse cenário, geralmente é útil para automaticamente criar usuários de banco de dados e atribuir as associações de função de banco de dados como parte do processo de implantação.</span><span class="sxs-lookup"><span data-stu-id="69aea-114">In this scenario, it's often beneficial to automatically create database users and assign database role memberships as part of the deployment process.</span></span>
> 
> <span data-ttu-id="69aea-115">O principal fator é que essa operação precisa ser condicional com base no ambiente de destino.</span><span class="sxs-lookup"><span data-stu-id="69aea-115">The key factor is that this operation needs to be conditional based on the target environment.</span></span> <span data-ttu-id="69aea-116">Se você estiver implantando para um preparo ou de um ambiente de produção, você deseja ignorar a operação.</span><span class="sxs-lookup"><span data-stu-id="69aea-116">If you're deploying to a staging or a production environment, you want to skip the operation.</span></span> <span data-ttu-id="69aea-117">Se você estiver implantando para um desenvolvedor ou ambiente de teste, você deseja implantar as associações de função sem intervenção adicional.</span><span class="sxs-lookup"><span data-stu-id="69aea-117">If you're deploying to a developer or test environment, you want to deploy role memberships without further intervention.</span></span> <span data-ttu-id="69aea-118">Este tópico descreve uma abordagem que você pode usar para enfrentar esse desafio.</span><span class="sxs-lookup"><span data-stu-id="69aea-118">This topic describes one approach you can use to address this challenge.</span></span>


<span data-ttu-id="69aea-119">Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [entre em contato com o Gerenciador soluções](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="69aea-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="69aea-120">O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;uma contendo instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação de build.</span><span class="sxs-lookup"><span data-stu-id="69aea-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="69aea-121">No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.</span><span class="sxs-lookup"><span data-stu-id="69aea-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="69aea-122">Visão geral da tarefa</span><span class="sxs-lookup"><span data-stu-id="69aea-122">Task Overview</span></span>

<span data-ttu-id="69aea-123">Este tópico pressupõe que:</span><span class="sxs-lookup"><span data-stu-id="69aea-123">This topic assumes that:</span></span>

- <span data-ttu-id="69aea-124">Você usa a abordagem de arquivo de projeto de divisão para a implantação da solução, conforme descrito em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span><span class="sxs-lookup"><span data-stu-id="69aea-124">You use the split project file approach to solution deployment, as described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span>
- <span data-ttu-id="69aea-125">Chamar VSDBCMD do arquivo de projeto para implantar seu projeto de banco de dados, conforme descrito em [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="69aea-125">You call VSDBCMD from the project file to deploy your database project, as described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

<span data-ttu-id="69aea-126">Para criar usuários de banco de dados e atribuir as associações de função quando você implanta um projeto de banco de dados em um ambiente de teste, você precisará:</span><span class="sxs-lookup"><span data-stu-id="69aea-126">To create database users and assign role memberships when you deploy a database project to a test environment, you'll need to:</span></span>

- <span data-ttu-id="69aea-127">Crie um script Transact Structured Query Language (Transact-SQL) que faz as alterações de banco de dados necessários.</span><span class="sxs-lookup"><span data-stu-id="69aea-127">Create a Transact Structured Query Language (Transact-SQL) script that makes the necessary database changes.</span></span>
- <span data-ttu-id="69aea-128">Crie um destino de Microsoft Build Engine (MSBuild) que usa o utilitário de sqlcmd.exe para executar o script SQL.</span><span class="sxs-lookup"><span data-stu-id="69aea-128">Create a Microsoft Build Engine (MSBuild) target that uses the sqlcmd.exe utility to run the SQL script.</span></span>
- <span data-ttu-id="69aea-129">Configure seus arquivos de projeto para invocar o destino quando você estiver implantando sua solução para um ambiente de teste.</span><span class="sxs-lookup"><span data-stu-id="69aea-129">Configure your project files to invoke the target when you're deploying your solution to a test environment.</span></span>

<span data-ttu-id="69aea-130">Este tópico mostra como executar cada um desses procedimentos.</span><span class="sxs-lookup"><span data-stu-id="69aea-130">This topic will show you how to perform each of these procedures.</span></span>

## <a name="scripting-the-database-role-memberships"></a><span data-ttu-id="69aea-131">As associações de função de banco de dados de script</span><span class="sxs-lookup"><span data-stu-id="69aea-131">Scripting the Database Role Memberships</span></span>

<span data-ttu-id="69aea-132">Você pode criar um script Transact-SQL em uma grande quantidade de diferentes maneiras e em qualquer local que você escolher.</span><span class="sxs-lookup"><span data-stu-id="69aea-132">You can create a Transact-SQL script in a lot of different ways, and in any location you choose.</span></span> <span data-ttu-id="69aea-133">A abordagem mais fácil é criar o script em sua solução no Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="69aea-133">The easiest approach is to create the script within your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="69aea-134">**Para criar um script SQL**</span><span class="sxs-lookup"><span data-stu-id="69aea-134">**To create a SQL script**</span></span>

1. <span data-ttu-id="69aea-135">No **Gerenciador de soluções** janela, expanda o nó do seu projeto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="69aea-135">In the **Solution Explorer** window, expand your database project node.</span></span>
2. <span data-ttu-id="69aea-136">Clique com botão direito do **Scripts** pasta, aponte para **Add**e, em seguida, clique em **nova pasta**.</span><span class="sxs-lookup"><span data-stu-id="69aea-136">Right-click the **Scripts** folder, point to **Add**, and then click **New Folder**.</span></span>
3. <span data-ttu-id="69aea-137">Tipo de **teste** como o nome da pasta e pressione Enter.</span><span class="sxs-lookup"><span data-stu-id="69aea-137">Type **Test** as the folder name, and then press Enter.</span></span>
4. <span data-ttu-id="69aea-138">Clique com botão direito do **teste** pasta, aponte para **Add**e, em seguida, clique em **Script**.</span><span class="sxs-lookup"><span data-stu-id="69aea-138">Right-click the **Test** folder, point to **Add**, and then click **Script**.</span></span>
5. <span data-ttu-id="69aea-139">No **Adicionar Novo Item** diálogo caixa, dê um nome significativo de seu script (por exemplo, **AddRoleMemberships.sql**) e, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="69aea-139">In the **Add New Item** dialog box, give your script a meaningful name (for example, **AddRoleMemberships.sql**), and then click **Add**.</span></span>

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. <span data-ttu-id="69aea-140">No *AddRoleMemberships.sql* de arquivo, adicione instruções Transact-SQL que:</span><span class="sxs-lookup"><span data-stu-id="69aea-140">In the *AddRoleMemberships.sql* file, add Transact-SQL statements that:</span></span>

    1. <span data-ttu-id="69aea-141">Crie um usuário de banco de dados para o logon do SQL Server que irá acessar seu banco de dados.</span><span class="sxs-lookup"><span data-stu-id="69aea-141">Create a database user for the SQL Server login that will access your database.</span></span>
    2. <span data-ttu-id="69aea-142">Adicione o usuário de banco de dados para todas as funções de banco de dados necessários.</span><span class="sxs-lookup"><span data-stu-id="69aea-142">Add the database user to any required database roles.</span></span>
7. <span data-ttu-id="69aea-143">O arquivo deve ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="69aea-143">The file should resemble this:</span></span>

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. <span data-ttu-id="69aea-144">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="69aea-144">Save the file.</span></span>

## <a name="executing-the-script-on-the-target-database"></a><span data-ttu-id="69aea-145">Executar o Script no banco de dados de destino</span><span class="sxs-lookup"><span data-stu-id="69aea-145">Executing the Script on the Target Database</span></span>

<span data-ttu-id="69aea-146">O ideal é que você executaria os scripts Transact-SQL necessários como parte de um script de pós-implantação quando você implanta seu projeto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="69aea-146">Ideally, you'd run any required Transact-SQL scripts as part of a post-deployment script when you deploy your database project.</span></span> <span data-ttu-id="69aea-147">No entanto, os scripts de pós-implantação não permitem que você execute lógica condicionalmente com base em configurações da solução ou as propriedades de compilação.</span><span class="sxs-lookup"><span data-stu-id="69aea-147">However, post-deployment scripts don't allow you to execute logic conditionally based on solution configurations or build properties.</span></span> <span data-ttu-id="69aea-148">A alternativa é executar seus scripts SQL diretamente do arquivo de projeto MSBuild, criando uma **destino** elemento que executa um comando sqlcmd.exe.</span><span class="sxs-lookup"><span data-stu-id="69aea-148">The alternative is to run your SQL scripts directly from the MSBuild project file, by creating a **Target** element that executes a sqlcmd.exe command.</span></span> <span data-ttu-id="69aea-149">Você pode usar esse comando para executar o script no banco de dados de destino:</span><span class="sxs-lookup"><span data-stu-id="69aea-149">You can use this command to run your script on the target database:</span></span>


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> <span data-ttu-id="69aea-150">Para obter mais informações sobre opções de linha de comando sqlcmd, consulte [utilitário sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).</span><span class="sxs-lookup"><span data-stu-id="69aea-150">For more information on sqlcmd command-line options, see [sqlcmd Utility](https://msdn.microsoft.com/library/ms162773.aspx).</span></span>


<span data-ttu-id="69aea-151">Antes de você incorporar este comando em um destino do MSBuild, você precisa considerar sob quais condições você deseja que o script seja executado:</span><span class="sxs-lookup"><span data-stu-id="69aea-151">Before you embed this command in an MSBuild target, you need to consider under what conditions you want the script to run:</span></span>

- <span data-ttu-id="69aea-152">O banco de dados de destino deve existir antes de alterar suas associações de função.</span><span class="sxs-lookup"><span data-stu-id="69aea-152">The target database must exist before you change its role memberships.</span></span> <span data-ttu-id="69aea-153">Como tal, você precisa executar esse script *depois de* a implantação de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="69aea-153">As such, you need to run this script *after* the database deployment.</span></span>
- <span data-ttu-id="69aea-154">Você precisa incluir uma condição para que o script é executado apenas para ambientes de teste.</span><span class="sxs-lookup"><span data-stu-id="69aea-154">You need to include a condition so that the script is only executed for test environments.</span></span>
- <span data-ttu-id="69aea-155">Se você estiver executando uma implantação "what if"&#x2014;em outras palavras, se você estiver gerando scripts de implantação, mas na verdade não executá-los&#x2014;você não deve executar o script SQL.</span><span class="sxs-lookup"><span data-stu-id="69aea-155">If you're running a "what if" deployment&#x2014;in other words, if you're generating deployment scripts but not actually running them&#x2014;you shouldn't run the SQL script.</span></span>

<span data-ttu-id="69aea-156">Se você estiver usando a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), conforme demonstrado pela solução de exemplo do Contact Manager, você pode dividir as instruções de compilação para o seu script SQL como esta:</span><span class="sxs-lookup"><span data-stu-id="69aea-156">If you're using the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), as demonstrated by the Contact Manager sample solution, you can split the build instructions for your SQL script like this:</span></span>

- <span data-ttu-id="69aea-157">Todas as necessárias propriedades específicas do ambiente, junto com a propriedade que determina se é necessário implantar permissões, devem ir no arquivo de projeto específicas do ambiente (por exemplo, *Env Dev.proj*).</span><span class="sxs-lookup"><span data-stu-id="69aea-157">Any required environment-specific properties, together with the property that determines whether to deploy permissions, should go in the environment-specific project file (for example, *Env-Dev.proj*).</span></span>
- <span data-ttu-id="69aea-158">O destino do MSBuild em si, junto com quaisquer propriedades que não se altere entre ambientes de destino deve ir no arquivo de projeto universal (por exemplo, *Publish.proj*).</span><span class="sxs-lookup"><span data-stu-id="69aea-158">The MSBuild target itself, together with any properties that will not change between destination environments, should go in the universal project file (for example, *Publish.proj*).</span></span>

<span data-ttu-id="69aea-159">No arquivo de projeto específicas do ambiente, você precisa definir uma propriedade booleana que permite que o usuário especifique se deseja implantar as associações de função, o nome do banco de dados de destino e o nome do servidor de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="69aea-159">In the environment-specific project file, you need to define the database server name, the target database name, and a Boolean property that lets the user specify whether to deploy role memberships.</span></span>


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


<span data-ttu-id="69aea-160">No arquivo de projeto universal, você precisa fornecer o local do sqlcmd executável e o local do script SQL que você deseja executar.</span><span class="sxs-lookup"><span data-stu-id="69aea-160">In the universal project file, you need to provide the location of the sqlcmd executable and the location of the SQL script you want to run.</span></span> <span data-ttu-id="69aea-161">Essas propriedades permanecerá o mesmo, independentemente do ambiente de destino.</span><span class="sxs-lookup"><span data-stu-id="69aea-161">These properties will remain the same regardless of the destination environment.</span></span> <span data-ttu-id="69aea-162">Você também precisará criar um destino do MSBuild para executar o comando sqlcmd.</span><span class="sxs-lookup"><span data-stu-id="69aea-162">You also need to create an MSBuild target to execute the sqlcmd command.</span></span>


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


<span data-ttu-id="69aea-163">Observe que você adiciona o local do sqlcmd executável como uma propriedade estática, como isso pode ser útil para outros destinos.</span><span class="sxs-lookup"><span data-stu-id="69aea-163">Notice that you add the location of the sqlcmd executable as a static property, as this could be useful to other targets.</span></span> <span data-ttu-id="69aea-164">Por outro lado, você define o local do seu script SQL e a sintaxe do comando sqlcmd como propriedades dinâmicas dentro do destino, pois elas não serão necessárias antes do destino é executado.</span><span class="sxs-lookup"><span data-stu-id="69aea-164">In contrast, you define the location of your SQL script and the syntax of the sqlcmd command as dynamic properties within the target, as they will not be required before the target is executed.</span></span> <span data-ttu-id="69aea-165">Nesse caso, o **DeployTestDBPermissions** destino será executado somente se essas condições forem atendidas:</span><span class="sxs-lookup"><span data-stu-id="69aea-165">In this case, the **DeployTestDBPermissions** target will only be executed if these conditions are met:</span></span>

- <span data-ttu-id="69aea-166">O **DeployTestDBRoleMemberships** estiver definida como **verdadeiro**.</span><span class="sxs-lookup"><span data-stu-id="69aea-166">The **DeployTestDBRoleMemberships** property is set to **true**.</span></span>
- <span data-ttu-id="69aea-167">O usuário não especificou um **WhatIf = true** sinalizador.</span><span class="sxs-lookup"><span data-stu-id="69aea-167">The user hasn't specified a **WhatIf=true** flag.</span></span>

<span data-ttu-id="69aea-168">Por fim, não se esqueça de invocar o destino.</span><span class="sxs-lookup"><span data-stu-id="69aea-168">Finally, don't forget to invoke the target.</span></span> <span data-ttu-id="69aea-169">No *Publish.proj* arquivo, você pode fazer isso adicionando o destino para a lista de dependências para o padrão **FullPublish** destino.</span><span class="sxs-lookup"><span data-stu-id="69aea-169">In the *Publish.proj* file, you can do this by adding the target to the dependency list for the default **FullPublish** target.</span></span> <span data-ttu-id="69aea-170">Você precisa garantir que o **DeployTestDBPermissions** destino não é executado até que o **PublishDbPackages** destino foi executado.</span><span class="sxs-lookup"><span data-stu-id="69aea-170">You need to ensure that the **DeployTestDBPermissions** target is not executed until the **PublishDbPackages** target has been executed.</span></span>


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a><span data-ttu-id="69aea-171">Conclusão</span><span class="sxs-lookup"><span data-stu-id="69aea-171">Conclusion</span></span>

<span data-ttu-id="69aea-172">Este tópico descreveu uma maneira na qual você pode adicionar usuários de banco de dados e associações de função como uma ação pós-implantação quando você implanta um projeto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="69aea-172">This topic described one way in which you can add database users and role memberships as a post-deployment action when you deploy a database project.</span></span> <span data-ttu-id="69aea-173">Isso é geralmente útil quando você regularmente recriar um banco de dados em um ambiente de teste, mas ela geralmente deve ser evitada ao implantar bancos de dados em ambientes de preparo ou produção.</span><span class="sxs-lookup"><span data-stu-id="69aea-173">This is typically useful when you regularly re-create a database in a test environment, but it should usually be avoided when you deploy databases to staging or production environments.</span></span> <span data-ttu-id="69aea-174">Como tal, você deve garantir que você use a lógica condicional necessária para que os usuários de banco de dados e associações de função são criadas apenas quando é apropriado fazer isso.</span><span class="sxs-lookup"><span data-stu-id="69aea-174">As such, you should ensure that you use the necessary conditional logic so that database users and role memberships are only created when it's appropriate to do so.</span></span>

## <a name="further-reading"></a><span data-ttu-id="69aea-175">Leitura adicional</span><span class="sxs-lookup"><span data-stu-id="69aea-175">Further Reading</span></span>

<span data-ttu-id="69aea-176">Para obter mais informações sobre como usar VSDBCMD para implantar projetos de banco de dados, consulte [implantar projetos de banco de dados](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span><span class="sxs-lookup"><span data-stu-id="69aea-176">For more information on using VSDBCMD to deploy database projects, see [Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span></span> <span data-ttu-id="69aea-177">Para obter orientação sobre como personalizar as implantações de banco de dados para ambientes de destino diferente, consulte [Personalizando as implantações de banco de dados para vários ambientes](customizing-database-deployments-for-multiple-environments.md).</span><span class="sxs-lookup"><span data-stu-id="69aea-177">For guidance on customizing database deployments for different target environments, see [Customizing Database Deployments for Multiple Environments](customizing-database-deployments-for-multiple-environments.md).</span></span> <span data-ttu-id="69aea-178">Para obter mais informações sobre como usar arquivos de projeto personalizados do MSBuild para controlar o processo de implantação, consulte [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="69aea-178">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span> <span data-ttu-id="69aea-179">Para obter mais informações sobre opções de linha de comando sqlcmd, consulte [utilitário sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).</span><span class="sxs-lookup"><span data-stu-id="69aea-179">For more information on sqlcmd command-line options, see [sqlcmd Utility](https://msdn.microsoft.com/library/ms162773.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="69aea-180">[Anterior](customizing-database-deployments-for-multiple-environments.md)
> [Próximo](deploying-membership-databases-to-enterprise-environments.md)</span><span class="sxs-lookup"><span data-stu-id="69aea-180">[Previous](customizing-database-deployments-for-multiple-environments.md)
[Next](deploying-membership-databases-to-enterprise-environments.md)</span></span>