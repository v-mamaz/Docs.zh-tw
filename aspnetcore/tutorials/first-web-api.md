---
title: 使用 ASP.NET Core 和 Visual Studio 來建立 Web API
author: rick-anderson
description: 使用 ASP.NET Core MVC 和 Visual Studio 在 Windows 上建置 Web API
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: 88d1958ce5c42d559754972a855c1ffe22ab45a6
ms.sourcegitcommit: 2ef32676c16f76282f7c23154d13affce8c8bf35
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "50234575"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio"></a>使用 ASP.NET Core 和 Visual Studio 來建立 Web API

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson) 提供

本教學課程將建置 Web API 來管理「待辦事項」項目清單， 並不會建立使用者介面 (UI)。

本教學課程有 3 個版本：

* Windows：使用 Visual Studio 在 Windows 上建立 Web API (針對此教學課程，請參閱[影片版本](https://www.youtube.com/watch?v=TTkhEyGBfAk))
* macOS：[使用 Visual Studio for Mac 建立 Web API](xref:tutorials/first-web-api-mac)
* macOS、Linux、Windows：[使用 Visual Studio Code 建立 Web API](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>必要條件

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a>建立專案

請在 Visual Studio 中遵循下列步驟：

* 從 [檔案] 功能表選取 [新增] > [專案]。
* 選取 [ASP.NET Core Web 應用程式] 範本。 將專案命名為 *TodoApi*，然後按一下 [確定]。
* 在 [新增 ASP.NET Core Web 應用程式 - TodoApi] 對話方塊中，選擇 ASP.NET Core 版本。 選取 [API] 範本，然後按一下 [確定]。 請**勿**選取 [Enable Docker Support] (啟用 Docker 支援)。

### <a name="launch-the-app"></a>啟動應用程式

在 Visual Studio 中，按 CTRL + F5 來啟動應用程式。 Visual Studio 會啟動瀏覽器並巡覽至 `http://localhost:<port>/api/values`，其中 `<port>` 是隨機選擇的通訊埠編號。 Chrome、Microsoft Edge 和 Firefox 顯示下列輸出：

```json
["value1","value2"]
```

若使用 Internet Explorer，您會收到儲存 *values.json* 檔案的提示。

### <a name="add-a-model-class"></a>新增模型類別

模型是代表應用程式中資料的物件。 在此情況下，唯一的模型是待辦事項。

在方案總管中，以滑鼠右鍵按一下專案。 選取 [新增] > [新增資料夾]。 將資料夾命名為 *Models*。

> [!NOTE]
> 模型類別可位於專案中的任何位置。 根據慣例，*Models* 資料夾是供模型類別使用。

在 [方案總管] 中，以滑鼠右鍵按一下 *Models* 資料夾，然後選擇 [新增] > [類別]。 將類別命名為 *TodoItem*，然後按一下 [新增]。

使用下列程式碼更新 `TodoItem` 類別：

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

此資料庫會在建立 `TodoItem` 時產生 `Id`。

### <a name="create-the-database-context"></a>建立資料庫內容

「資料庫內容」是為指定的資料模型協調 Entity Framework 功能的主要類別。 此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。

在 [方案總管] 中，以滑鼠右鍵按一下 *Models* 資料夾，然後選擇 [新增] > [類別]。 將類別命名為 *TodoContext*，然後按一下 [新增]。

使用下列程式碼取代該類別：

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>新增控制器

在方案總管中，以滑鼠右鍵按一下 *Controllers* 資料夾。 選取 [新增] > [新增項目]。 在 [新增項目] 對話方塊中，選取 [API 控制器類別] 範本。 將類別命名為 *TodoController*，然後按一下 [新增]。

![在搜尋方塊中輸入 controller 且已選取 Web API 控制器的 [新增項目] 對話方塊](first-web-api/_static/new_controller.png)

使用下列程式碼取代該類別：

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>啟動應用程式

在 Visual Studio 中，按 CTRL + F5 來啟動應用程式。 Visual Studio 會啟動瀏覽器並巡覽至 `http://localhost:<port>/api/values`，其中 `<port>` 是隨機選擇的通訊埠編號。 瀏覽至位於 `http://localhost:<port>/api/todo` 的 `Todo` 控制器。

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
