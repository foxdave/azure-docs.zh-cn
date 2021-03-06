---
title: Azure Cosmos DB：SQL API 入门教程 | Microsoft Docs
description: 使用 SQL API 创建联机数据库和 C# 控制台应用程序的教程。
keywords: nosql 教程, 联机数据库, c# 控制台应用程序
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 08/16/2017
ms.author: sngun
ms.openlocfilehash: 40a80a049e6eb94390bab0b47e1f5ed49b4606b9
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991192"
---
# <a name="azure-cosmos-db-sql-api-getting-started-tutorial"></a>Azure Cosmos DB：SQL API 入门教程

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [异步 Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

欢迎使用 Azure Cosmos DB SQL API 入门教程！ 学习本教程后，将拥有一个可创建并查询 Azure Cosmos DB 资源的控制台应用程序。

本教程涉及：

* 创建并连接到 Azure Cosmos DB 帐户
* 配置 Visual Studio 解决方案
* 创建联机数据库
* 创建集合
* 创建 JSON 文档
* 查询集合
* 替换文档
* 删除文档
* 删除数据库

没有时间？ 不必担心！ 可在 [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started)上获取完整的解决方案。 有关快速说明，请转到[“获取完整的 NoSQL 教程解决方案”部分](#GetSolution)。

现在，让我们开始吧！

## <a name="prerequisites"></a>先决条件

* 有效的 Azure 帐户。 如果没有，可以注册 [免费帐户](https://azure.microsoft.com/free/)。 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]

## <a name="step-1-create-an-azure-cosmos-db-account"></a>步骤 1：创建 Azure Cosmos DB 帐户
让我们创建一个 Azure Cosmos DB 帐户。 如果已经有一个想要使用的帐户，可以跳到 [设置 Visual Studio 解决方案](#SetupVS)。 如果使用 Azure Cosmos DB 模拟器，请遵循 [Azure Cosmos DB 模拟器](local-emulator.md)中的步骤设置该模拟器，并直接跳到[设置 Visual Studio 解决方案](#SetupVS)。

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>步骤 2：设置 Visual Studio 解决方案
1. 在计算机上打开 **Visual Studio 2017**。
2. 在“文件”菜单中，选择“新建”，并选择“项目”。
3. 在“新建项目”对话框中，选择“模板” / “Visual C#” / “控制台应用程序”，为项目命名，并单击“确定”。
   ![“新建项目”窗口屏幕截图](./media/sql-api-get-started/nosql-tutorial-new-project-2.png)
4. 在“解决方案资源管理器”中，右键单击 Visual Studio 解决方案下方的新控制台应用程序，并单击“管理 NuGet 包...”
    
    ![“项目”右键菜单屏幕截图](./media/sql-api-get-started/nosql-tutorial-manage-nuget-pacakges.png)
5. 在“NuGet”选项卡上，单击“浏览”，并在搜索框中键入 **azure documentdb**。
6. 在结果中找到 **Microsoft.Azure.DocumentDB**，并单击“安装”。
   Azure Cosmos DB SQL API 客户端库的程序包 ID 是 [Microsoft Azure Cosmos DB 客户端库](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)。
   ![用于查找 Azure Cosmos DB 客户端 SDK 的 NuGet 菜单的屏幕截图](./media/sql-api-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)

    如果获得有关查看解决方案更改的消息，请单击“确定”。 如果获得有关接受许可证的消息，请单击“我接受”。

很好！ 现在，我们已完成安装，让我们开始编写一些代码。 可以在 [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs)上找到本教程的完整代码项目。

## <a id="Connect"></a>步骤 3：连接到 Azure Cosmos DB 帐户
首先，在 Program.cs 文件中 C# 应用程序的开始位置添加这些引用︰

    using System;
    using System.Linq;
    using System.Threading.Tasks;

    // ADD THIS PART TO YOUR CODE
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;

> [!IMPORTANT]
> 为了完成本教程，请确保添加以上依赖关系。
> 
> 

现在，在公共类 *Program* 下面添加这两个常量和*客户端*变量。

    public class Program
    {
        // ADD THIS PART TO YOUR CODE
        private const string EndpointUrl = "<your endpoint URL>";
        private const string PrimaryKey = "<your primary key>";
        private DocumentClient client;

接下来，返回到 [Azure 门户](https://portal.azure.com)检索终结点 URL 和主密钥。 终结点 URL 和主密钥是必需的，可让应用程序知道要连接的对象，使 Azure Cosmos DB 信任应用程序的连接。

在 Azure 门户中，导航到 Azure Cosmos DB 帐户，然后单击“密钥”。

从门户中复制该 URI 并将它粘贴到 program.cs 文件中的 `<your endpoint URL>`。 然后从门户中复制“主密钥”并将它粘贴到 `<your primary key>`。

![NoSQL 教程用于创建 C# 控制台应用程序的 Azure 门户的屏幕截图。 显示一个 Azure Cosmos DB 帐户，在“Azure Cosmos DB 帐户”页上突出显示“活动”中心、“密钥”按钮，在“密钥”页上突出显示 URI、主密钥和辅助密钥的值][keys]

接下来，我们开始使用应用程序时，请首先创建一个新的 **DocumentClient** 实例。

在 **Main** 方法下面，添加这个名为 **GetStartedDemo** 的新异步任务，将新的 **DocumentClient** 实例化。

    static void Main(string[] args)
    {
    }

    // ADD THIS PART TO YOUR CODE
    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
    }

添加以下代码，从 **Main** 方法中运行异步任务。 **Main** 方法将捕获异常并将它们写到控制台上。

    static void Main(string[] args)
    {
            // ADD THIS PART TO YOUR CODE
            try
            {
                    Program p = new Program();
                    p.GetStartedDemo().Wait();
            }
            catch (DocumentClientException de)
            {
                    Exception baseException = de.GetBaseException();
                    Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
            }
            catch (Exception e)
            {
                    Exception baseException = e.GetBaseException();
                    Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
            }
            finally
            {
                    Console.WriteLine("End of demo, press any key to exit.");
                    Console.ReadKey();
            }

按 **F5** 运行应用程序。 控制台窗口输出会显示消息`End of demo, press any key to exit.`，确认已建立连接。  然后即可关闭控制台窗口。 

祝贺你！ 已成功连接到 Azure Cosmos DB 帐户，现在让我们看看如何使用 Azure Cosmos DB 资源。  

## <a name="step-4-create-a-database"></a>第 4 步：创建数据库
在添加创建数据库的代码之前，添加一个用于向控制台写入的帮助器方法。

将 **WriteToConsoleAndPromptToContinue** 方法复制并粘贴到 **GetStartedDemo** 方法后面。

    // ADD THIS PART TO YOUR CODE
    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
            Console.WriteLine(format, args);
            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }

可以通过使用 **DocumentClient** 类的 [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) 方法来创建 Azure Cosmos DB [数据库](sql-api-resources.md#databases)。 数据库是跨集合分区的 JSON 文档存储的逻辑容器。

将以下代码复制并粘贴到客户端创建后的 **GetStartedDemo** 方法。 这会创建一个名为 *FamilyDB*的数据库。

    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        // ADD THIS PART TO YOUR CODE
        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

按 **F5** 运行应用程序。

祝贺你！ 已成功创建了 Azure Cosmos DB 数据库。  

## <a id="CreateColl"></a>步骤 5：创建集合
> [!WARNING]
> **CreateDocumentCollectionIfNotExistsAsync** 将创建一个具有保留吞吐量的新集合，它牵涉定价。 有关详细信息，请访问 [定价页](https://azure.microsoft.com/pricing/details/cosmos-db/)。
> 
> 

可以通过使用 **DocumentClient** 类的 [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) 方法来创建[集合](sql-api-resources.md#collections)。 集合是 JSON 文档和相关联的 JavaScript 应用程序逻辑的容器。

将以下代码复制并粘贴到数据库创建后的 **GetStartedDemo** 方法。 这会创建一个名为 *FamilyCollection* 的文档集合。

        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

        // ADD THIS PART TO YOUR CODE
         await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });

按 **F5** 运行应用程序。

祝贺你！ 已成功创建了 Azure Cosmos DB 文档集合。  

## <a id="CreateDoc"></a>第 6 步：创建 JSON 文档
可以通过使用 **DocumentClient** 类的 [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) 方法创建[文档](sql-api-resources.md#documents)。 文档为用户定义的（任意）JSON 内容。 现在，我们可以插入一个或多个文档。 如果已有要在数据库中存储的数据，则可以使用 Azure Cosmos DB 的[数据迁移工具](import-data.md)将数据导入数据库。

在本例中，首先需要创建 **Family** 类来表示存储在 Azure Cosmos DB 中的对象。 此外还将创建 **Family** 中使用的 **Parent**、**Child**、**Pet** 和 **Address** 子类。 请注意，文档必须将 **ID** 属性序列化为 JSON 格式的 **ID**。 通过在 **GetStartedDemo** 方法后添加以下内部子类来创建这些类。

将 **Family**、**Parent**、**Child**、**Pet** 和 **Address** 类复制并粘贴到 **WriteToConsoleAndPromptToContinue** 方法后面。

    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
    }

    // ADD THIS PART TO YOUR CODE
    public class Family
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        public string LastName { get; set; }
        public Parent[] Parents { get; set; }
        public Child[] Children { get; set; }
        public Address Address { get; set; }
        public bool IsRegistered { get; set; }
        public override string ToString()
        {
            return JsonConvert.SerializeObject(this);
        }
    }

    public class Parent
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
    }

    public class Child
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
        public string Gender { get; set; }
        public int Grade { get; set; }
        public Pet[] Pets { get; set; }
    }

    public class Pet
    {
        public string GivenName { get; set; }
    }

    public class Address
    {
        public string State { get; set; }
        public string County { get; set; }
        public string City { get; set; }
    }

将 **CreateFamilyDocumentIfNotExists** 方法复制并粘贴到 **Address** 类下面。

    // ADD THIS PART TO YOUR CODE
    private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
    {
        try
        {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
            this.WriteToConsoleAndPromptToContinue("Found {0}", family.Id);
        }
        catch (DocumentClientException de)
        {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
                this.WriteToConsoleAndPromptToContinue("Created Family {0}", family.Id);
            }
            else
            {
                throw;
            }
        }
    }

然后插入两个文档，Andersen Family 和 Wakefield Family 各一个。

将以下代码复制并粘贴到文档集合创建后的 **GetStartedDemo** 方法。

    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });
    
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });


    // ADD THIS PART TO YOUR CODE
    Family andersenFamily = new Family
    {
            Id = "Andersen.1",
            LastName = "Andersen",
            Parents = new Parent[]
            {
                    new Parent { FirstName = "Thomas" },
                    new Parent { FirstName = "Mary Kay" }
            },
            Children = new Child[]
            {
                    new Child
                    {
                            FirstName = "Henriette Thaulow",
                            Gender = "female",
                            Grade = 5,
                            Pets = new Pet[]
                            {
                                    new Pet { GivenName = "Fluffy" }
                            }
                    }
            },
            Address = new Address { State = "WA", County = "King", City = "Seattle" },
            IsRegistered = true
    };

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", andersenFamily);

    Family wakefieldFamily = new Family
    {
            Id = "Wakefield.7",
            LastName = "Wakefield",
            Parents = new Parent[]
            {
                    new Parent { FamilyName = "Wakefield", FirstName = "Robin" },
                    new Parent { FamilyName = "Miller", FirstName = "Ben" }
            },
            Children = new Child[]
            {
                    new Child
                    {
                            FamilyName = "Merriam",
                            FirstName = "Jesse",
                            Gender = "female",
                            Grade = 8,
                            Pets = new Pet[]
                            {
                                    new Pet { GivenName = "Goofy" },
                                    new Pet { GivenName = "Shadow" }
                            }
                    },
                    new Child
                    {
                            FamilyName = "Miller",
                            FirstName = "Lisa",
                            Gender = "female",
                            Grade = 1
                    }
            },
            Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
            IsRegistered = false
    };

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

按 **F5** 运行应用程序。

祝贺你！ 已成功创建了两个 Azure Cosmos DB 文档。  

![说明 NoSQL 教程创建 C# 控制台应用程序所用帐户、联机数据库、集合和文档的层次关系的图表。](./media/sql-api-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>步骤 7：查询 Azure Cosmos DB 资源
Azure Cosmos DB 支持对存储在每个集合中的 JSON 文档进行[各种查询](sql-api-sql-query.md)。  下面的示例代码演示了各种查询（使用 Azure Cosmos DB SQL 语法以及 LINQ），我们可以针对上一步中插入的文档执行查询。

将 **ExecuteSimpleQuery** 方法复制并粘贴到 **CreateFamilyDocumentIfNotExists** 方法后面。

    // ADD THIS PART TO YOUR CODE
    private void ExecuteSimpleQuery(string databaseName, string collectionName)
    {
        // Set some common query options
        FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

            // Here we find the Andersen family via its LastName
            IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                    .Where(f => f.LastName == "Andersen");

            // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
            Console.WriteLine("Running LINQ query...");
            foreach (Family family in familyQuery)
            {
                    Console.WriteLine("\tRead {0}", family);
            }

            // Now execute the same query via direct SQL
            IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                    "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
                    queryOptions);

            Console.WriteLine("Running direct SQL query...");
            foreach (Family family in familyQueryInSql)
            {
                    Console.WriteLine("\tRead {0}", family);
            }

            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }

将以下代码复制并粘贴到第二个文档创建后的 **GetStartedDemo** 方法。

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    // ADD THIS PART TO YOUR CODE
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

按 **F5** 运行应用程序。

祝贺你！ 已成功完成了对 Azure Cosmos DB 集合的查询。

下图说明了如何针对创建的集合调用 Azure Cosmos DB SQL 查询语法，相同的逻辑也适用于 LINQ 查询。

![说明 NoSQL 教程创建 C# 控制台应用程序所用查询的范围和意义的图表。](./media/sql-api-get-started/nosql-tutorial-collection-documents.png)

查询中的关键字 [FROM](sql-api-sql-query.md#FromClause) 是可选项，因为 Azure Cosmos DB 查询已限制为单个集合。 因此，“FROM Families f”可与“FROM root r”或者任何其他所选变量名进行交换。 Azure Cosmos DB 将推断所选 Families、root 或变量名，并默认引用当前集合。

## <a id="ReplaceDocument"></a>步骤 8：替换 JSON 文档
Azure Cosmos DB 支持替换 JSON 文档。  

将 **ReplaceFamilyDocument** 方法复制并粘贴到 **ExecuteSimpleQuery** 方法后面。

    // ADD THIS PART TO YOUR CODE
    private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
    {
         await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
         this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
    }

将以下代码复制并粘贴到查询执行后的 **GetStartedDemo** 方法结尾。 替换文档之后，将再次运行相同查询以查看更改后的文档。

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    // ADD THIS PART TO YOUR CODE
    // Update the Grade of the Andersen Family child
    andersenFamily.Children[0].Grade = 6;

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

按 **F5** 运行应用程序。

祝贺你！ 已成功替换了 Azure Cosmos DB 文档。

## <a id="DeleteDocument"></a>步骤 9：删除 JSON 文档
Azure Cosmos DB 支持删除 JSON 文档。  

将 **DeleteFamilyDocument** 方法复制并粘贴到 **ReplaceFamilyDocument** 方法后面。

    // ADD THIS PART TO YOUR CODE
    private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
    {
         await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
         Console.WriteLine("Deleted Family {0}", documentName);
    }

将以下代码复制并粘贴到第二个查询执行后的 **GetStartedDemo** 方法结尾。

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);
    
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");
    
    // ADD THIS PART TO CODE
    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

按 **F5** 运行应用程序。

祝贺你！ 已成功删除了 Azure Cosmos DB 文档。

## <a id="DeleteDatabase"></a>步骤 10：删除数据库
删除已创建的数据库将删除该数据库及其所有子资源（集合、文档等）。

将以下代码复制并粘贴到文档删除后的 **GetStartedDemo** 方法，删除整个数据库和所有子资源。

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

    // ADD THIS PART TO CODE
    // Clean up/delete the database
    await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB"));

按 **F5** 运行应用程序。

祝贺你！ 已成功删除了 Azure Cosmos DB 数据库。

## <a id="Run"></a>步骤 11：一起运行 C# 控制台应用程序！
在 Visual Studio 中按 F5，即可在调试模式下构建应用程序。

应会在控制台窗口中看到已启动应用的输出。 输出会显示我们所添加的查询的结果，并且应与下面的示例文本相匹配。

    Created FamilyDB
    Press any key to continue ...
    Created FamilyCollection
    Press any key to continue ...
    Created Family Andersen.1
    Press any key to continue ...
    Created Family Wakefield.7
    Press any key to continue ...
    Running LINQ query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Running direct SQL query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Replaced Family Andersen.1
    Press any key to continue ...
    Running LINQ query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Running direct SQL query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Deleted Family Andersen.1
    End of demo, press any key to exit.

祝贺你！ 已经完成了本教程，并且获得了一个正常工作的 C# 控制台应用程序！

## <a id="GetSolution"></a>获取完整的教程解决方案
如果没有时间完成本教程中的步骤，或者只需下载代码示例，则可从 [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started) 获取。 

若要生成 GetStarted 解决方案，需备齐以下各项：

* 有效的 Azure 帐户。 如果没有，可以注册 [免费帐户](https://azure.microsoft.com/free/)。
* 一个 [Azure Cosmos DB 帐户][cosmos-db-create-account]。
* GitHub 上提供的 [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-getting-started) 解决方案。

若要在 Visual Studio 中还原 Azure Cosmos DB .NET SDK 的引用，请在解决方案资源管理器中右键单击“GetStarted”解决方案，并单击“启用 NuGet 程序包还原”。 接下来，按照[连接到 Azure Cosmos DB 帐户](#Connect)中所述的方法在 App.config 文件中更新 EndpointUrl 和 AuthorizationKey 值。

就这么简单，生成以后即可开始操作！


## <a name="next-steps"></a>后续步骤
* 需要更复杂的 ASP.NET MVC 教程？ 请参阅 [ASP.NET MVC 教程：使用 Azure Cosmos DB 开发 Web 应用程序](sql-api-dotnet-application.md)。
* 希望使用 Azure Cosmos DB 执行规模和性能测试？ 请参阅[使用 Azure Cosmos DB 执行性能和扩展测试](performance-testing.md)
* 了解如何[监视 Azure Cosmos DB 请求、使用情况和存储](monitor-accounts.md)。
* 在 [Query Playground](https://www.documentdb.com/sql/demo)中对示例数据集运行查询。
* 若要了解有关 Azure Cosmos DB 的详细信息，请参阅[欢迎使用 Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction)。

[keys]: media/sql-api-get-started/nosql-tutorial-keys.png
[cosmos-db-create-account]: create-sql-api-dotnet.md#create-account
