---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Implantando bancos de dados de associação em ambientes empresariais | Microsoft Docs
author: jrjlee
description: Este tópico explica as principais considerações e desafios que você precisará superar quando você provisiona os bancos de dados do serviços de aplicativos ASP.NET (mais comum...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 307375843c51f31d3d8ae0f2ef0a17a3e58d3a64
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57028183"
---
<a name="deploying-membership-databases-to-enterprise-environments"></a><span data-ttu-id="b693c-103">Implantação do bancos de dados de associação em ambientes corporativos</span><span class="sxs-lookup"><span data-stu-id="b693c-103">Deploying Membership Databases to Enterprise Environments</span></span>
====================
<span data-ttu-id="b693c-104">by [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="b693c-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="b693c-105">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="b693c-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="b693c-106">Este tópico explica as principais considerações e desafios que você precisará superar quando você provisionar aplicativo ASP.NET serviços bancos de dados (mais comumente conhecidos como bancos de dados de associação) em ambientes de teste, preparo ou produção.</span><span class="sxs-lookup"><span data-stu-id="b693c-106">This topic explains the key considerations and challenges you'll need to overcome when you provision ASP.NET application services databases (more commonly referred to as membership databases) in test, staging, or production environments.</span></span> <span data-ttu-id="b693c-107">Ele também descreve as abordagens que você pode usar para atender a esses desafios.</span><span class="sxs-lookup"><span data-stu-id="b693c-107">It also describes approaches you can use to meet these challenges.</span></span>


<span data-ttu-id="b693c-108">Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [entre em contato com o Gerenciador soluções](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b693c-108">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="b693c-109">O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;uma contendo instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação de build.</span><span class="sxs-lookup"><span data-stu-id="b693c-109">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="b693c-110">No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.</span><span class="sxs-lookup"><span data-stu-id="b693c-110">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a><span data-ttu-id="b693c-111">Quais são os problemas quando você implanta um banco de dados de associação?</span><span class="sxs-lookup"><span data-stu-id="b693c-111">What Are the Issues When You Deploy a Membership Database?</span></span>

<span data-ttu-id="b693c-112">Na maioria dos casos, quando você criar uma estratégia de implantação para um banco de dados, a primeira coisa que você precisa considerar é quais dados você deseja implantar.</span><span class="sxs-lookup"><span data-stu-id="b693c-112">In most cases, when you devise a deployment strategy for a database, the first thing you need to consider is what data you want to deploy.</span></span> <span data-ttu-id="b693c-113">Em um ambiente de desenvolvimento ou teste, você talvez queira implantar dados da conta de usuário para facilitar o teste rápida e fácil.</span><span class="sxs-lookup"><span data-stu-id="b693c-113">In a development or test environment, you might want to deploy user account data to facilitate quick and easy testing.</span></span> <span data-ttu-id="b693c-114">Em um ambiente de preparo ou produção, é muito improvável que você desejaria implantar dados de conta de usuário.</span><span class="sxs-lookup"><span data-stu-id="b693c-114">In a staging or production environment, it's very unlikely that you'd want to deploy user account data.</span></span>

<span data-ttu-id="b693c-115">Infelizmente, os bancos de dados de associação ASP.NET apresentam alguns desafios específicos que tornam essa decisão muito mais complexo:</span><span class="sxs-lookup"><span data-stu-id="b693c-115">Unfortunately, ASP.NET membership databases introduce some specific challenges that make this decision a lot more complex:</span></span>

- <span data-ttu-id="b693c-116">Uma implantação somente de esquema deixará o banco de dados de associação em um estado não operacional.</span><span class="sxs-lookup"><span data-stu-id="b693c-116">A schema-only deployment will leave the membership database in a non-operational state.</span></span> <span data-ttu-id="b693c-117">Isso ocorre porque o banco de dados de associação inclui alguns dados de configuração (na **aspnet\_SchemaVersions** tabela) que o banco de dados requer para funcionar.</span><span class="sxs-lookup"><span data-stu-id="b693c-117">This is because the membership database includes some configuration data (in the **aspnet\_SchemaVersions** table) that the database requires in order to function.</span></span> <span data-ttu-id="b693c-118">Dessa forma, se você executar uma implantação somente de esquema do banco de dados de associação para excluir dados da conta de usuário, você precisará executar um script de pós-implantação para adicionar os dados de configuração do essential.</span><span class="sxs-lookup"><span data-stu-id="b693c-118">As such, if you perform a schema-only deployment of your membership database in order to exclude user account data, you'll need to run a post-deployment script to add the essential configuration data.</span></span>
- <span data-ttu-id="b693c-119">Dependendo de como seu banco de dados de associação é configurado, o provedor de associação pode usar a chave do computador para criptografar senhas e armazená-los no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b693c-119">Depending on how your membership database is configured, the membership provider may use the machine key to encrypt passwords and store them in the database.</span></span> <span data-ttu-id="b693c-120">Nesse caso, quaisquer dados de conta de usuário que você implantar com o banco de dados poderá ficar inutilizáveis no servidor de destino.</span><span class="sxs-lookup"><span data-stu-id="b693c-120">In this case, any user account data you deploy with the database will become unusable on the destination server.</span></span> <span data-ttu-id="b693c-121">Por esse motivo, a implantação de dados de conta de usuário não é um cenário com suporte.</span><span class="sxs-lookup"><span data-stu-id="b693c-121">For this reason, deploying user account data is not a supported scenario.</span></span>

## <a name="choosing-a-membership-database-strategy"></a><span data-ttu-id="b693c-122">Escolhendo uma estratégia de banco de dados de associação</span><span class="sxs-lookup"><span data-stu-id="b693c-122">Choosing a Membership Database Strategy</span></span>

<span data-ttu-id="b693c-123">Use estas diretrizes ao escolher como provisionar um banco de dados de associação em um ambiente de servidor corporativo:</span><span class="sxs-lookup"><span data-stu-id="b693c-123">Use these guidelines when you choose how to provision a membership database in an enterprise server environment:</span></span>

- <span data-ttu-id="b693c-124">Sempre que possível, não implante bancos de dados de associação.</span><span class="sxs-lookup"><span data-stu-id="b693c-124">Wherever possible, do not deploy membership databases.</span></span> <span data-ttu-id="b693c-125">Em vez disso, crie o banco de dados de associação manualmente no servidor de banco de dados de destino.</span><span class="sxs-lookup"><span data-stu-id="b693c-125">Instead, create the membership database manually on the target database server.</span></span> <span data-ttu-id="b693c-126">Se você ainda não tiver personalizado seu esquema de banco de dados de associação, você pode simplesmente criar um novo em situ de destino usando o [ferramenta de registro do ASP.NET SQL Server (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="b693c-126">If you haven't customized your membership database schema, you can simply create a new one in situ at the destination using the [ASP.NET SQL Server Registration Tool (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span></span>
- <span data-ttu-id="b693c-127">Se você não tem opção, mas para implantar um banco de dados de associação&#x2014;por exemplo, se você tiver feito modificações abrangentes para o esquema de banco de dados&#x2014;você deve executar uma implantação somente de esquema do banco de dados de associação, para excluir dados da conta de usuário e, em seguida, Execute um script de pós-implantação para adicionar dados de qualquer configuração necessária.</span><span class="sxs-lookup"><span data-stu-id="b693c-127">If you have no option but to deploy a membership database&#x2014;for example, if you've made extensive modifications to the database schema&#x2014;you should perform a schema-only deployment of the membership database, to exclude user account data, and then run a post-deployment script to add any required configuration data.</span></span> <span data-ttu-id="b693c-128">Você pode encontrar diretrizes amplas sobre essas abordagens em [como: Implantar o banco de dados de associação do ASP.NET sem incluir contas de usuário](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="b693c-128">You can find broad guidance on these approaches in [How to: Deploy the ASP.NET Membership Database Without Including User Accounts](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).</span></span>

<span data-ttu-id="b693c-129">É importante lembrar-se de que *o esquema de banco de dados de associação é provavelmente será razoavelmente estático*.</span><span class="sxs-lookup"><span data-stu-id="b693c-129">It's important to remember that *the schema of your membership database is likely to be fairly static*.</span></span> <span data-ttu-id="b693c-130">Mesmo se você personalizou o banco de dados de associação, é improvável que você precisará atualizar o esquema regularmente&#x2014;ele não será alterado com a mesma frequência que o código em um aplicativo web ou um projeto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b693c-130">Even if you've customized the membership database, it's unlikely that you'll need to update the schema on a regular basis&#x2014;it's not going to change with the same frequency as the code in a web application or a database project.</span></span> <span data-ttu-id="b693c-131">Como tal, não deve ser necessário incluir o banco de dados de associação em todos os processos automatizados ou passo a passo de implantação.</span><span class="sxs-lookup"><span data-stu-id="b693c-131">As such, you shouldn't need to include the membership database in any automated or single-step deployment processes.</span></span>

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a><span data-ttu-id="b693c-132">Usando VSDBCMD para atualizar um esquema de banco de dados de associação</span><span class="sxs-lookup"><span data-stu-id="b693c-132">Using VSDBCMD to Update a Membership Database Schema</span></span>

<span data-ttu-id="b693c-133">Se você modificar a estrutura do seu banco de dados de associação após sua primeira implantação, não convém usar a ferramenta de implantação do Internet Information Services (IIS) da Web (implantação da Web) para reimplantar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b693c-133">If you modify the structure of your membership database after your first deployment, you may not want to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to redeploy the database.</span></span> <span data-ttu-id="b693c-134">A funcionalidade de implantação de banco de dados na implantação da Web não inclui a capacidade de fazer atualizações diferenciais para um banco de dados de destino&#x2014;em vez disso, o Web Deploy deve descartar e recriar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b693c-134">The database deployment functionality in Web Deploy doesn't include the capability to make differential updates to a destination database&#x2014;instead, Web Deploy must drop and re-create the database.</span></span> <span data-ttu-id="b693c-135">Isso significa que você perderá quaisquer dados de conta de usuário existente, que é normalmente indesejáveis em ambientes de preparo ou produção.</span><span class="sxs-lookup"><span data-stu-id="b693c-135">This means that you lose any existing user account data, which is typically undesirable in staging or production environments.</span></span>

<span data-ttu-id="b693c-136">A alternativa é usar o utilitário VSDBCMD para atualizar o esquema de banco de dados de destino.</span><span class="sxs-lookup"><span data-stu-id="b693c-136">The alternative is to use the VSDBCMD utility to update the schema of your destination database.</span></span> <span data-ttu-id="b693c-137">VSDBCMD inclui dois recursos importantes.</span><span class="sxs-lookup"><span data-stu-id="b693c-137">VSDBCMD includes two important capabilities.</span></span> <span data-ttu-id="b693c-138">Em primeiro lugar, ele pode importar o esquema do banco de dados existente para um arquivo .dbschema.</span><span class="sxs-lookup"><span data-stu-id="b693c-138">First, it can import the schema of an existing database into a .dbschema file.</span></span> <span data-ttu-id="b693c-139">Em segundo lugar, ele pode implantar um arquivo de .dbschema para um banco de dados como uma atualização diferencial, o que significa que ele só faz as alterações necessárias para colocar o banco de dados de destino atualizado e você não perderá os dados.</span><span class="sxs-lookup"><span data-stu-id="b693c-139">Second, it can deploy a .dbschema file to an existing database as a differential update, which means that it only makes the changes required to bring the target database up to date and you don't lose any data.</span></span>

<span data-ttu-id="b693c-140">Você pode usar essas etapas de alto nível para atualizar um esquema de banco de dados de associação:</span><span class="sxs-lookup"><span data-stu-id="b693c-140">You can use these high-level steps to update a membership database schema:</span></span>

1. <span data-ttu-id="b693c-141">Use o VSDBCMD **importação** ação para gerar um arquivo .dbschema do banco de dados de associação de origem.</span><span class="sxs-lookup"><span data-stu-id="b693c-141">Use the VSDBCMD **Import** action to generate a .dbschema file for your source membership database.</span></span> <span data-ttu-id="b693c-142">Esse procedimento é descrito em [como: Importar um esquema de um Prompt de comando](https://msdn.microsoft.com/library/dd172135.aspx).</span><span class="sxs-lookup"><span data-stu-id="b693c-142">This procedure is described in [How to: Import a Schema from a Command Prompt](https://msdn.microsoft.com/library/dd172135.aspx).</span></span>
2. <span data-ttu-id="b693c-143">Use o VSDBCMD **Deploy** ação para implantar o arquivo .dbschema em seu banco de dados de associação de destino.</span><span class="sxs-lookup"><span data-stu-id="b693c-143">Use the VSDBCMD **Deploy** action to deploy the .dbschema file to your destination membership database.</span></span> <span data-ttu-id="b693c-144">Esse procedimento é descrito na [referência de linha de comando para VSDBCMD. EXE (implantação e importação de esquema)](https://msdn.microsoft.com/library/dd193283.aspx).</span><span class="sxs-lookup"><span data-stu-id="b693c-144">This procedure is described in [Command-Line Reference for VSDBCMD.EXE (Deployment and Schema Import)](https://msdn.microsoft.com/library/dd193283.aspx).</span></span>

## <a name="conclusion"></a><span data-ttu-id="b693c-145">Conclusão</span><span class="sxs-lookup"><span data-stu-id="b693c-145">Conclusion</span></span>

<span data-ttu-id="b693c-146">Este tópico descreveu alguns dos desafios que você pode enfrentar quando você precisa provisionar bancos de dados de associação ASP.NET em vários ambientes de destino.</span><span class="sxs-lookup"><span data-stu-id="b693c-146">This topic described some of the challenges you may face when you need to provision ASP.NET membership databases in various target environments.</span></span> <span data-ttu-id="b693c-147">Em particular, explicado por que as implantações de somente esquema deixará o banco de dados de associação em um estado não operacional e por que implantar dados da conta de usuário não tem suporte.</span><span class="sxs-lookup"><span data-stu-id="b693c-147">In particular, it explained why schema-only deployments will leave the membership database in a non-operational state and why deploying user account data is not supported.</span></span> <span data-ttu-id="b693c-148">O tópico também apresentadas orientações sobre como provisionar, implantar e atualizar bancos de dados de associação em cenários diferentes.</span><span class="sxs-lookup"><span data-stu-id="b693c-148">The topic also presented guidance on how to provision, deploy, and update membership databases in different scenarios.</span></span>

## <a name="further-reading"></a><span data-ttu-id="b693c-149">Leitura adicional</span><span class="sxs-lookup"><span data-stu-id="b693c-149">Further Reading</span></span>

<span data-ttu-id="b693c-150">Para obter mais diretrizes e exemplos de como usar VSDBCMD, consulte [referência de linha de comando para VSDBCMD. EXE (implantação e importação de esquema)](https://msdn.microsoft.com/library/dd193283.aspx) e [como: Importar um esquema de um Prompt de comando](https://msdn.microsoft.com/library/dd172135.aspx).</span><span class="sxs-lookup"><span data-stu-id="b693c-150">For more guidance and examples of how to use VSDBCMD, see [Command-Line Reference for VSDBCMD.EXE (Deployment and Schema Import)](https://msdn.microsoft.com/library/dd193283.aspx) and [How to: Import a Schema from a Command Prompt](https://msdn.microsoft.com/library/dd172135.aspx).</span></span> <span data-ttu-id="b693c-151">Para obter mais informações sobre como usar aspnet\_regsql.exe para criar bancos de dados de associação, consulte [ferramenta de registro do ASP.NET SQL Server (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="b693c-151">For more information on using aspnet\_regsql.exe to create membership databases, see [ASP.NET SQL Server Registration Tool (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span></span> <span data-ttu-id="b693c-152">Para obter orientação geral sobre a implantação de bancos de dados de associação, consulte [como: Implantar o banco de dados de associação do ASP.NET sem incluir contas de usuário](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="b693c-152">For more general guidance on deploying membership databases, see [How to: Deploy the ASP.NET Membership Database Without Including User Accounts](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b693c-153">[Anterior](deploying-database-role-memberships-to-test-environments.md)
> [Próximo](excluding-files-and-folders-from-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="b693c-153">[Previous](deploying-database-role-memberships-to-test-environments.md)
[Next](excluding-files-and-folders-from-deployment.md)</span></span>