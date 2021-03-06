---
title: ASP.NET Core 中的 WebSockets 支援
author: rick-anderson
description: 了解如何在 ASP.NET Core 中開始使用 WebSocket。
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/28/2018
uid: fundamentals/websockets
ms.openlocfilehash: b0f1aeff6c7a5777993459274293ba23f2d9dc12
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206736"
---
# <a name="websockets-support-in-aspnet-core"></a>ASP.NET Core 中的 WebSockets 支援

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Andrew Stanton-Nurse](https://github.com/anurse)

本文說明如何在 ASP.NET Core 中開始使用 WebSocket。 [WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) 為通訊協定，其可在 TCP 連線下啟用雙向的持續性通訊通道。 它用於受益於快速且即時通訊的應用程式，例如聊天、儀表板和遊戲應用程式。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([如何下載](xref:index#how-to-download-a-sample))。 如需詳細資訊，請參閱[後續步驟](#next-steps)一節。

## <a name="prerequisites"></a>必要條件

* ASP.NET Core 1.1 或更新版本
* 支援 ASP.NET Core 的任何作業系統：
  
  * Windows 7/Windows Server 2008 或更新版本
  * Linux
  * macOS
  
* 如果應用程式在 Windows 上與 IIS 搭配執行：

  * Windows 8 / Windows Server 2012 或更新版本
  * IIS 8 / IIS 8 Express
  * 必須啟用 WebSocket (請參閱 [IIS/IIS Express 支援](#iisiis-express-support)一節。)。
  
* 如果應用程式在 [HTTP.sys](xref:fundamentals/servers/httpsys) 上執行：

  * Windows 8 / Windows Server 2012 或更新版本

* 如需支援的瀏覽器，請請參閱 https://caniuse.com/#feat=websockets。

## <a name="when-to-use-websockets"></a>WebSocket 使用時機

請使用 WebSocket 以直接使用通訊端連線。 例如，若要取得即時遊戲的可能最佳效能，請使用 WebSocket。

[ASP.NET Core SignalR](xref:signalr/introduction) 是程式庫，可簡化將即時 Web 功能新增至應用程式的程序。 它會盡可能使用 WebSockets。

## <a name="how-to-use-websockets"></a>如何使用 WebSocket

* 安裝 [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) 套件。
* 設定中介軟體。
* 接受 WebSocket 要求。
* 傳送和接收訊息。

### <a name="configure-the-middleware"></a>設定中介軟體

在 `Startup` 類別的 `Configure` 方法中新增 WebSocket 中介軟體：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

您可以設定下列設定：

* `KeepAliveInterval` - 要將 "ping" 框架傳送到用戶端，以確保 Proxy 保持連線開啟的頻率。
* `ReceiveBufferSize` - 用來接收資料的緩衝區大小。 進階使用者可能需要變更此設定，以便根據資料的大小進行效能調整。

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a>接受 WebSocket 要求

在要求生命週期的後半部某處 (例如，在 `Configure` 方法或 MVC 動作的後半部)，檢查它是否為 WebSocket 要求並接受 WebSocket 要求。

下列範例取自 `Configure` 方法的後半部：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

WebSocket 要求可以傳入任何 URL，但此範例程式碼只接受 `/ws` 的要求。

### <a name="send-and-receive-messages"></a>傳送和接收訊息

`AcceptWebSocketAsync` 方法可將 TCP 連線升級為 WebSocket 連線，並提供 [WebSocket](/dotnet/core/api/system.net.websockets.websocket) 物件。 請使用 `WebSocket` 物件來傳送和接收訊息。

稍早所示接受 WebSocket 要求的程式碼會將 `WebSocket` 物件傳遞給 `Echo` 方法。 程式碼會收到一則訊息，並立即傳送回相同的訊息。 在用戶端關閉連線之前，訊息會在迴圈中傳送和接收：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

如果在開始此迴圈之前接受 WebSocket 連線，中介軟體管線就會結束。 在關閉通訊端後，管線會回溯。 也就是說，在接受 WebSocket 後，要求就會在管線中停止向前移動。 當迴圈完成且通訊端關閉時，要求會繼續備份管線。

## <a name="iisiis-express-support"></a>IIS/IIS Express 支援

搭配 IIS/IIS Express 8 或更新版本的 Windows Server 2012 或更新版本和 Windows 8 或更新版本具有 WebSocket 通訊協定的支援。

> [!NOTE]
> 使用 IIS Express 時一律會啟用 WebSockets。

### <a name="enabling-websockets-on-iis"></a>在 IIS 上啟用 WebSockets

若要在 Windows Server 2012 或更新版本中啟用 WebSocket 通訊協定的支援：

> [!NOTE]
> 使用 IIS Express 時，不需要這些步驟

1. 使用來自 [管理] 功能表的 [新增角色及功能] 精靈，或是 [伺服器管理員] 中的連結。
1. 選取 [角色型或功能型安裝]。 選取 [下一步]。
1. 選取適當的伺服器 (預設會選取本機伺服器)。 選取 [下一步]。
1. 展開 [角色] 樹狀目錄中的 [網頁伺服器 (IIS)]，展開 [網頁伺服器]，然後展開 [應用程式開發]。
1. 選取 [WebSocket 通訊協定]。 選取 [下一步]。
1. 如果不需要額外的功能，請選取 [下一步]。
1. 選取 [安裝] 。
1. 當安裝完成時，選取 [關閉] 來結束精靈。

若要在 Windows 8 或更新版本中啟用 WebSocket 通訊協定的支援：

> [!NOTE]
> 使用 IIS Express 時，不需要這些步驟

1. 瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。
1. 開啟下列節點：[Internet Information Services] > [全球資訊網服務] > [應用程式開發功能]。
1. 選取 [WebSocket 通訊協定] 功能。 選取 [確定]。

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a>在 Node.js 上使用 socket.io 時停用 WebSocket

如果在 [Node.js](https://nodejs.org/) 的 [socket.io](https://socket.io/) 中使用 WebSocket 支援，請在 *web.config* 或 *applicationHost.config* 中使用 `webSocket`　項目，以停用預設的 IIS WebSocket 模組。如果未執行此步驟，IIS WebSocket 模組會嘗試處理 WebSocket 通訊，而非 Node.js 和應用程式。

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a>後續步驟

本文附帶的[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples)是回應應用程式。 其具有一個進行 WebSocket 連線的網頁，而伺服器會將其接收的任何訊息重新傳送回用戶端。 請從命令提示字元執行應用程式 (它未設定為從 Visual Studio 搭配 IIS Express 執行)，並巡覽至 http://localhost:5000。 網頁會在左上方顯示連線狀態：

![網頁的初始狀態](websockets/_static/start.png)

選取 [連線] 將 WebSocket 要求傳送到顯示的 URL。 輸入測試訊息，然後選取 [傳送]。 完成後，請選取 [關閉通訊端]。 [通訊記錄檔] 區段會報告每次進行的開啟、傳送和關閉動作。

![網頁的初始狀態](websockets/_static/end.png)
