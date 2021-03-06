---
title: 教程：Azure Active Directory 与 Oneteam 的集成 | Microsoft 文档
description: 了解如何在 Azure Active Directory 和 Oneteam 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 2e94916c-64ae-4e1a-a8b5-bc6ef7d28c29
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 76b7c2ac18a683ccbe07c7c4cdc750399d8466c2
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2018
ms.locfileid: "39446965"
---
# <a name="tutorial-azure-active-directory-integration-with-oneteam"></a>教程：Azure Active Directory 与 Oneteam 的集成

本教程介绍了如何将 Oneteam 与 Azure Active Directory (Azure AD) 进行集成。

将 Oneteam 与 Azure AD 集成可提供以下优势：

- 可在 Azure AD 中控制谁有权访问 Oneteam
- 可以让用户使用其 Azure AD 帐户自动登录到 Oneteam（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Oneteam 的集成，需要具有以下项：

- Azure AD 订阅
- 启用了 Oneteam 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Oneteam
1. 配置和测试 Azure AD 单一登录

## <a name="adding-oneteam-from-the-gallery"></a>从库中添加 Oneteam
要配置 Oneteam 与 Azure AD 的集成，需要从库中将 Oneteam 添加到托管 SaaS 应用列表。

**若要从库中添加 Oneteam，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

1. 在搜索框中，键入“Oneteam”。

    ![创建 Azure AD 测试用户](./media/oneteam-tutorial/tutorial_oneteam_search.png)

1. 在结果窗格中，选择“Oneteam”，然后单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/oneteam-tutorial/tutorial_oneteam_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，将基于名为“Britta Simon”的测试用户配置并测试 Oneteam 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 Oneteam 用户。 换句话说，需要在 Azure AD 用户与 Oneteam 中的相关用户之间建立链接关系。

可通过将 Azure AD 中“用户名”的值指定为 Oneteam 中“用户名”的值来建立此链接关系。

若要配置并测试 Oneteam 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
1. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. [创建 Oneteam 测试用户](#creating-a-oneteam-test-user) - 在 Oneteam 中创建 Britta Simon 的对应用户，将其链接到其 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
1. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

本部分将在 Azure 门户中启用 Azure AD 单一登录并在 Oneteam 应用程序中配置单一登录。

**若要配置 Oneteam 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的 Oneteam 应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![配置单一登录](./media/oneteam-tutorial/tutorial_oneteam_samlbase.png)

1. 在“Oneteam 域和 URL”部分中，如果要在“IDP 启动的模式”下配置应用程序，请执行以下步骤：

    ![配置单一登录](./media/oneteam-tutorial/tutorial_oneteam_url.png)

    a. 在“标识符”文本框中，使用以下模式键入 URL：`https://api.one-team.io/teams/<team name>`

    b. 在 **“回复 URL”** 文本框中，使用以下模式键入 URL：`https://api.one-team.io/teams/<team name>/auth/saml/callback`

1. 如果要在“SP”发起的模式下配置应用程序，请选中“显示高级 URL 设置”：

    ![配置单一登录](./media/oneteam-tutorial/tutorial_oneteam_url1.png)

    在“登录 URL”文本框中，使用以下模式键入 URL： `https://<team name>.one-team.io/`
     
    > [!NOTE] 
    > 这些不是实际值。 请使用实际的“标识符”、“回复 URL”和“登录 URL”更新这些值。 请联系 [Oneteam 客户端支持团队](https://support.one-team.com/hc/requests/new)获取这些值。 



1. 在“SAML 签名证书”部分中，单击“元数据 XML”，并在计算机上保存元数据文件。

    ![配置单一登录](./media/oneteam-tutorial/tutorial_oneteam_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录](./media/oneteam-tutorial/tutorial_general_400.png)
    
1. 若要为应用程序配置 SSO，可以通过 [Oneteam 支持团队](https://support.one-team.com/hc/requests/new)提出支持票证，并为其提供下载的元数据。 

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[ Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/oneteam-tutorial/create_aaduser_01.png) 

1. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。
    
    ![创建 Azure AD 测试用户](./media/oneteam-tutorial/create_aaduser_02.png) 

1. 若要打开“用户”对话框，请在对话框顶部单击“添加”。
 
    ![创建 Azure AD 测试用户](./media/oneteam-tutorial/create_aaduser_03.png) 

1. 在“用户”对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/oneteam-tutorial/create_aaduser_04.png) 

    a. 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。
 
### <a name="creating-a-oneteam-test-user"></a>创建 Oneteam 测试用户

本部分的目的是在 Oneteam 中创建名为 Britta Simon 的用户。 Oneteam 支持在默认情况下启用的实时预配。

此部分不存在任何操作项。 如果尚不存在用户，则在尝试访问 Oneteam 期间会创建一个新用户。

>[!NOTE]
>如果需要手动创建用户，请向 [Oneteam 支持团队](https://support.one-team.com/hc/requests/new)提交支持票证。

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 Oneteam 的权限，允许她使用 Azure 单一登录。

![分配用户][200] 

**要将 Britta Simon 分配到 Oneteam，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“Oneteam”。

    ![配置单一登录](./media/oneteam-tutorial/tutorial_oneteam_app.png) 

1. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

当在访问面板中单击 Oneteam 磁贴时，应当会自动登录到 Oneteam 应用程序。
有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。 

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [什么是使用 Azure Active Directory 的应用程序访问和单一登录？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/oneteam-tutorial/tutorial_general_01.png
[2]: ./media/oneteam-tutorial/tutorial_general_02.png
[3]: ./media/oneteam-tutorial/tutorial_general_03.png
[4]: ./media/oneteam-tutorial/tutorial_general_04.png

[100]: ./media/oneteam-tutorial/tutorial_general_100.png

[200]: ./media/oneteam-tutorial/tutorial_general_200.png
[201]: ./media/oneteam-tutorial/tutorial_general_201.png
[202]: ./media/oneteam-tutorial/tutorial_general_202.png
[203]: ./media/oneteam-tutorial/tutorial_general_203.png

