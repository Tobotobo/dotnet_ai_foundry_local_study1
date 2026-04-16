# dotnet_ai_foundry_local_study1

## 概要
* Foundry Local を試してみる


Foundry Local  
https://github.com/microsoft/Foundry-Local  

> Foundry Localは、ユーザーのデバイス上で完全に動作するアプリケーションを構築するための、エンドツーエンドのローカルAIソリューションです。ネイティブSDK（C#、JavaScript、Python、Rust）、最適化されたモデルの厳選されたカタログ、自動ハードウェアアクセラレーションを、すべて軽量パッケージ（約20MB）で提供します。コンパクトなサイズなので、アプリケーションへの統合やエンドユーザーへの配布が容易です。
> 
> ユーザーデータはデバイスから外部に送信されることはなく、応答はネットワーク遅延ゼロで即座に開始され、アプリはオフラインでも動作します。トークンごとのコスト、APIキー、維持管理が必要なバックエンドインフラストラクチャ、Azureサブスクリプションは一切不要です。

## 詳細

Foundry Local SDK リファレンス  
https://learn.microsoft.com/ja-jp/azure/foundry-local/reference/reference-sdk-current?tabs=windows&pivots=programming-language-csharp  

```
> dotnet --version
10.0.201
```

```sh
dotnet new console
# dotnet add package Microsoft.AI.Foundry.Local.WinML 
# ※↑ net9.0-windows10.0.26100 で net10.0 と互換性エラーで追加できず...
dotnet add package Microsoft.AI.Foundry.Local
# dotnet add package OpenAI
# ※↑ 入れなくても動いた
```

プロジェクトファイルに以下を追加  
```xml
  <PropertyGroup>
    ...
    <RuntimeIdentifier>win-x64</RuntimeIdentifier>
  </PropertyGroup>
```

Program.cs
```cs
using Microsoft.AI.Foundry.Local;
using Microsoft.Extensions.Logging;
using System.Linq;

var config = new Configuration
{
  AppName = "app-name",
  LogLevel = Microsoft.AI.Foundry.Local.LogLevel.Information,
};

using var loggerFactory = LoggerFactory.Create(builder =>
{
  builder.SetMinimumLevel(Microsoft.Extensions.Logging.LogLevel.Information);
});
var logger = loggerFactory.CreateLogger<Program>();

await FoundryLocalManager.CreateAsync(config, logger);
var manager = FoundryLocalManager.Instance;

var catalog = await manager.GetCatalogAsync();
var models = await catalog.ListModelsAsync();

Console.WriteLine($"Models available: {models.Count()}");
```

### 実行結果
```
> dotnet run          
Models available: 24
```