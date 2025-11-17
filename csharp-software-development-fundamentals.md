# C# ソフトウェア開発要素 体系的まとめ

## 目次

1. [C#言語の基礎](#1-c言語の基礎)
2. [オブジェクト指向プログラミング](#2-オブジェクト指向プログラミング)
3. [高度な言語機能](#3-高度な言語機能)
4. [.NETエコシステム](#4-netエコシステム)
5. [開発環境とツール](#5-開発環境とツール)
6. [アプリケーション開発の種類](#6-アプリケーション開発の種類)
7. [非同期プログラミング](#7-非同期プログラミング)
8. [データアクセスとORМ](#8-データアクセスとorm)
9. [デザインパターンとアーキテクチャ](#9-デザインパターンとアーキテクチャ)
10. [テストとデバッグ](#10-テストとデバッグ)
11. [パフォーマンスとメモリ管理](#11-パフォーマンスとメモリ管理)
12. [セキュリティ](#12-セキュリティ)
13. [パッケージ管理とデプロイメント](#13-パッケージ管理とデプロイメント)
14. [ベストプラクティス](#14-ベストプラクティス)

---

## 1. C#言語の基礎

### 1.1 データ型

#### 値型（Value Types）

```csharp
// 整数型
byte age = 25;           // 0 ~ 255
short year = 2025;       // -32,768 ~ 32,767
int count = 100000;      // -2,147,483,648 ~ 2,147,483,647
long population = 8000000000L; // -9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807

// 浮動小数点型
float temperature = 36.5f;
double pi = 3.14159265359;
decimal price = 1999.99m; // 高精度（金額計算に最適）

// 論理型
bool isActive = true;

// 文字型
char initial = 'A';

// 構造体
struct Point
{
    public int X;
    public int Y;
}
```

#### 参照型（Reference Types）

```csharp
// 文字列
string name = "John Doe";

// 配列
int[] numbers = { 1, 2, 3, 4, 5 };
string[] names = new string[10];

// クラス
class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}

// インターフェース
interface IRepository<T>
{
    void Add(T item);
    T GetById(int id);
}

// デリゲート
delegate void NotifyHandler(string message);
```

### 1.2 制御構造

```csharp
// if-else
if (age >= 18)
{
    Console.WriteLine("成人");
}
else if (age >= 13)
{
    Console.WriteLine("10代");
}
else
{
    Console.WriteLine("子供");
}

// switch式（C# 8.0以降）
string dayType = dayOfWeek switch
{
    DayOfWeek.Saturday or DayOfWeek.Sunday => "週末",
    _ => "平日"
};

// for文
for (int i = 0; i < 10; i++)
{
    Console.WriteLine(i);
}

// foreach文
foreach (var item in collection)
{
    ProcessItem(item);
}

// while文
while (condition)
{
    DoWork();
}

// do-while文
do
{
    DoWork();
} while (condition);
```

### 1.3 演算子

```csharp
// 算術演算子
int sum = a + b;
int difference = a - b;
int product = a * b;
int quotient = a / b;
int remainder = a % b;

// 比較演算子
bool equal = a == b;
bool notEqual = a != b;
bool greater = a > b;
bool less = a < b;

// 論理演算子
bool and = condition1 && condition2;
bool or = condition1 || condition2;
bool not = !condition;

// null合体演算子
string result = nullableValue ?? "デフォルト値";

// null条件演算子
int? length = text?.Length;

// パターンマッチング（C# 9.0以降）
if (obj is Person { Age: > 18 } person)
{
    Console.WriteLine($"{person.Name}は成人です");
}
```

---

## 2. オブジェクト指向プログラミング

### 2.1 クラスとオブジェクト

```csharp
// クラスの定義
public class Employee
{
    // フィールド
    private string _name;

    // プロパティ
    public string Name
    {
        get => _name;
        set => _name = value ?? throw new ArgumentNullException(nameof(value));
    }

    // 自動実装プロパティ
    public int Id { get; set; }
    public decimal Salary { get; set; }

    // 読み取り専用プロパティ
    public DateTime HireDate { get; init; }

    // コンストラクタ
    public Employee(int id, string name, decimal salary)
    {
        Id = id;
        Name = name;
        Salary = salary;
        HireDate = DateTime.Now;
    }

    // メソッド
    public void GiveRaise(decimal amount)
    {
        Salary += amount;
    }

    // 静的メソッド
    public static Employee Create(int id, string name)
    {
        return new Employee(id, name, 0);
    }
}

// オブジェクトの生成
var employee = new Employee(1, "田中太郎", 300000);

// オブジェクト初期化子
var employee2 = new Employee(2, "佐藤花子", 350000)
{
    // 追加のプロパティ設定
};
```

### 2.2 継承

```csharp
// 基底クラス
public class Animal
{
    public string Name { get; set; }

    public virtual void MakeSound()
    {
        Console.WriteLine("何か音を出す");
    }
}

// 派生クラス
public class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("ワンワン");
    }

    public void Fetch()
    {
        Console.WriteLine("ボールを取ってくる");
    }
}

public class Cat : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("ニャー");
    }
}
```

### 2.3 ポリモーフィズム

```csharp
// インターフェースの定義
public interface IPaymentProcessor
{
    void ProcessPayment(decimal amount);
    bool ValidatePayment();
}

// 実装
public class CreditCardProcessor : IPaymentProcessor
{
    public void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"クレジットカードで{amount}円を処理");
    }

    public bool ValidatePayment()
    {
        return true;
    }
}

public class PayPalProcessor : IPaymentProcessor
{
    public void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"PayPalで{amount}円を処理");
    }

    public bool ValidatePayment()
    {
        return true;
    }
}

// 使用
public void ProcessOrder(IPaymentProcessor processor, decimal amount)
{
    if (processor.ValidatePayment())
    {
        processor.ProcessPayment(amount);
    }
}
```

### 2.4 カプセル化

```csharp
public class BankAccount
{
    // privateフィールド
    private decimal _balance;
    private readonly string _accountNumber;

    // publicプロパティ（読み取り専用）
    public decimal Balance => _balance;
    public string AccountNumber => _accountNumber;

    public BankAccount(string accountNumber)
    {
        _accountNumber = accountNumber;
        _balance = 0;
    }

    // publicメソッドで内部状態を制御
    public void Deposit(decimal amount)
    {
        if (amount <= 0)
            throw new ArgumentException("入金額は正の値である必要があります");

        _balance += amount;
    }

    public void Withdraw(decimal amount)
    {
        if (amount <= 0)
            throw new ArgumentException("出金額は正の値である必要があります");

        if (amount > _balance)
            throw new InvalidOperationException("残高不足");

        _balance -= amount;
    }
}
```

### 2.5 抽象クラス

```csharp
public abstract class Shape
{
    public abstract double CalculateArea();
    public abstract double CalculatePerimeter();

    // 具象メソッド
    public void Display()
    {
        Console.WriteLine($"面積: {CalculateArea()}");
        Console.WriteLine($"周囲長: {CalculatePerimeter()}");
    }
}

public class Circle : Shape
{
    public double Radius { get; set; }

    public override double CalculateArea()
    {
        return Math.PI * Radius * Radius;
    }

    public override double CalculatePerimeter()
    {
        return 2 * Math.PI * Radius;
    }
}

public class Rectangle : Shape
{
    public double Width { get; set; }
    public double Height { get; set; }

    public override double CalculateArea()
    {
        return Width * Height;
    }

    public override double CalculatePerimeter()
    {
        return 2 * (Width + Height);
    }
}
```

---

## 3. 高度な言語機能

### 3.1 ジェネリクス

```csharp
// ジェネリッククラス
public class Repository<T> where T : class
{
    private readonly List<T> _items = new();

    public void Add(T item)
    {
        _items.Add(item);
    }

    public T GetById(Func<T, bool> predicate)
    {
        return _items.FirstOrDefault(predicate);
    }

    public IEnumerable<T> GetAll()
    {
        return _items;
    }
}

// ジェネリックメソッド
public T FindMax<T>(IEnumerable<T> items) where T : IComparable<T>
{
    T max = default(T);
    foreach (var item in items)
    {
        if (max == null || item.CompareTo(max) > 0)
            max = item;
    }
    return max;
}

// 複数の型パラメータ
public class Pair<TFirst, TSecond>
{
    public TFirst First { get; set; }
    public TSecond Second { get; set; }
}

// ジェネリック制約
public class Manager<T> where T : Employee, IComparable<T>, new()
{
    public T CreateInstance()
    {
        return new T();
    }
}
```

### 3.2 LINQ（Language Integrated Query）

```csharp
var numbers = new[] { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// メソッド構文
var evenNumbers = numbers
    .Where(n => n % 2 == 0)
    .Select(n => n * 2)
    .OrderByDescending(n => n)
    .ToList();

// クエリ構文
var evenNumbers2 = (from n in numbers
                    where n % 2 == 0
                    select n * 2)
                   .OrderByDescending(n => n)
                   .ToList();

// 複雑なクエリ
var employees = new List<Employee>();
var highEarners = employees
    .Where(e => e.Salary > 500000)
    .OrderBy(e => e.Name)
    .GroupBy(e => e.Department)
    .Select(g => new
    {
        Department = g.Key,
        Count = g.Count(),
        AverageSalary = g.Average(e => e.Salary)
    });

// Join
var orders = new List<Order>();
var orderDetails = from order in orders
                   join employee in employees on order.EmployeeId equals employee.Id
                   select new { order.OrderId, employee.Name, order.Total };

// GroupJoin
var departmentEmployees = from dept in departments
                          join emp in employees on dept.Id equals emp.DepartmentId into empGroup
                          select new { Department = dept.Name, Employees = empGroup };
```

### 3.3 デリゲートとイベント

```csharp
// デリゲートの定義
public delegate void NotifyEventHandler(string message);

// イベントの使用
public class Publisher
{
    // イベントの宣言
    public event NotifyEventHandler Notify;

    public void DoSomething()
    {
        // イベントの発火
        Notify?.Invoke("何かが起こりました");
    }
}

public class Subscriber
{
    public void Subscribe(Publisher publisher)
    {
        publisher.Notify += OnNotify;
    }

    private void OnNotify(string message)
    {
        Console.WriteLine($"通知を受信: {message}");
    }
}

// EventHandlerパターン
public class OrderEventArgs : EventArgs
{
    public int OrderId { get; set; }
    public decimal Amount { get; set; }
}

public class OrderProcessor
{
    public event EventHandler<OrderEventArgs> OrderPlaced;

    public void PlaceOrder(int orderId, decimal amount)
    {
        // 注文処理
        OnOrderPlaced(new OrderEventArgs { OrderId = orderId, Amount = amount });
    }

    protected virtual void OnOrderPlaced(OrderEventArgs e)
    {
        OrderPlaced?.Invoke(this, e);
    }
}
```

### 3.4 ラムダ式と式ツリー

```csharp
// ラムダ式
Func<int, int, int> add = (a, b) => a + b;
Action<string> print = message => Console.WriteLine(message);
Predicate<int> isEven = n => n % 2 == 0;

// 複数行のラムダ
Func<int, int> factorial = n =>
{
    int result = 1;
    for (int i = 2; i <= n; i++)
        result *= i;
    return result;
};

// 式ツリー
Expression<Func<int, bool>> expr = n => n > 5;

// 式ツリーの動的構築
var parameter = Expression.Parameter(typeof(int), "n");
var constant = Expression.Constant(5);
var greaterThan = Expression.GreaterThan(parameter, constant);
var lambda = Expression.Lambda<Func<int, bool>>(greaterThan, parameter);
var compiled = lambda.Compile();
```

### 3.5 レコード型（C# 9.0以降）

```csharp
// レコード型（イミュータブルなデータ構造）
public record Person(string FirstName, string LastName, int Age);

// 使用
var person1 = new Person("太郎", "田中", 30);
var person2 = person1 with { Age = 31 }; // 一部を変更した新しいインスタンス

// 等価性の比較（値ベース）
bool areEqual = person1 == person2; // false

// レコード構造体（C# 10.0以降）
public record struct Point(int X, int Y);

// クラス風のレコード
public record Employee
{
    public int Id { get; init; }
    public string Name { get; init; }
    public decimal Salary { get; init; }

    public void Deconstruct(out int id, out string name, out decimal salary)
    {
        id = Id;
        name = Name;
        salary = Salary;
    }
}
```

### 3.6 パターンマッチング

```csharp
// 型パターン
object obj = "Hello";
if (obj is string str)
{
    Console.WriteLine(str.ToUpper());
}

// プロパティパターン
if (person is { Age: >= 18, Name: not null })
{
    Console.WriteLine("成人で名前がある");
}

// リストパターン（C# 11.0以降）
int[] numbers = { 1, 2, 3, 4 };
if (numbers is [1, 2, .., var last])
{
    Console.WriteLine($"最後の要素: {last}");
}

// switch式でのパターンマッチング
string GetDiscount(Customer customer) => customer switch
{
    { Age: >= 65 } => "シニア割引",
    { IsMember: true, YearsOfMembership: > 5 } => "ゴールド会員割引",
    { IsMember: true } => "会員割引",
    _ => "割引なし"
};

// タプルパターン
string GetQuadrant(int x, int y) => (x, y) switch
{
    (0, 0) => "原点",
    (> 0, > 0) => "第1象限",
    (< 0, > 0) => "第2象限",
    (< 0, < 0) => "第3象限",
    (> 0, < 0) => "第4象限",
    _ => "軸上"
};
```

### 3.7 Null許容参照型（C# 8.0以降）

```csharp
#nullable enable

public class UserService
{
    // null非許容
    public string GetUserName(User user)
    {
        return user.Name; // userがnullだとコンパイラ警告
    }

    // null許容
    public string? FindUserName(int userId)
    {
        var user = FindUser(userId);
        return user?.Name; // nullを返す可能性がある
    }

    // null許容から非許容への変換
    public void ProcessUser(User? user)
    {
        if (user is null)
            return;

        // このブロック内ではuserはnull非許容として扱われる
        Console.WriteLine(user.Name);
    }

    // null免除演算子（注意して使用）
    public void ForceNonNull(User? user)
    {
        Console.WriteLine(user!.Name); // nullでないことを保証
    }
}
```

---

## 4. .NETエコシステム

### 4.1 .NETのバージョン

```
.NET Framework（レガシー）
├─ 4.8（最終版、Windows専用）
└─ 旧バージョン（4.7、4.6など）

.NET（現代的、クロスプラットフォーム）
├─ .NET 6（LTS - 2024年11月までサポート）
├─ .NET 7（Standard - サポート終了）
├─ .NET 8（LTS - 2026年11月までサポート）
└─ .NET 9（Standard - 2025年11月リリース）
└─ .NET 10（LTS - 2025年11月リリース予定）
```

### 4.2 主要なライブラリとフレームワーク

#### BCL（Base Class Library）

```csharp
// System.Collections.Generic
List<T>, Dictionary<TKey, TValue>, HashSet<T>, Queue<T>, Stack<T>

// System.Linq
Where, Select, OrderBy, GroupBy, Join

// System.IO
File, Directory, Path, Stream

// System.Text
StringBuilder, Encoding, RegularExpressions

// System.Threading
Task, Thread, ThreadPool, CancellationToken

// System.Net.Http
HttpClient, HttpRequestMessage, HttpResponseMessage
```

#### ASP.NET Core

```csharp
// Minimal API（.NET 6以降）
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Hello World!");
app.MapGet("/users/{id}", (int id) => new { Id = id, Name = "User" });

app.Run();

// MVC
[ApiController]
[Route("api/[controller]")]
public class UsersController : ControllerBase
{
    [HttpGet("{id}")]
    public ActionResult<User> GetUser(int id)
    {
        var user = _userService.GetById(id);
        return user != null ? Ok(user) : NotFound();
    }

    [HttpPost]
    public ActionResult<User> CreateUser(User user)
    {
        _userService.Add(user);
        return CreatedAtAction(nameof(GetUser), new { id = user.Id }, user);
    }
}
```

#### Entity Framework Core

```csharp
// DbContext
public class AppDbContext : DbContext
{
    public DbSet<User> Users { get; set; }
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .HasMany(u => u.Orders)
            .WithOne(o => o.User)
            .HasForeignKey(o => o.UserId);
    }
}

// 使用
using var context = new AppDbContext();
var users = await context.Users
    .Include(u => u.Orders)
    .Where(u => u.IsActive)
    .ToListAsync();
```

### 4.3 NuGetパッケージ管理

```xml
<!-- .csproj ファイル -->
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
    <PackageReference Include="Serilog" Version="3.1.1" />
    <PackageReference Include="AutoMapper" Version="12.0.1" />
  </ItemGroup>
</Project>
```

```bash
# コマンドライン
dotnet add package Newtonsoft.Json
dotnet restore
dotnet build
```

---

## 5. 開発環境とツール

### 5.1 IDE（統合開発環境）

#### Visual Studio

```
- Windows向けフル機能IDE
- デバッガ、プロファイラ、テストツール統合
- Azure統合
- 有償版（Professional、Enterprise）と無償版（Community）
```

#### Visual Studio Code

```
- 軽量でクロスプラットフォーム
- C# Dev Kit拡張機能
- デバッグとIntelliSense対応
- 完全無償
```

#### JetBrains Rider

```
- クロスプラットフォームIDE
- 優れたリファクタリング機能
- 有償（サブスクリプション）
```

### 5.2 .NET CLI

```bash
# プロジェクト作成
dotnet new console -n MyApp
dotnet new web -n MyWebApp
dotnet new classlib -n MyLibrary

# ビルドと実行
dotnet build
dotnet run
dotnet watch run  # ホットリロード

# テスト
dotnet test

# パッケージ管理
dotnet add package PackageName
dotnet remove package PackageName
dotnet restore

# 発行
dotnet publish -c Release
dotnet publish -r win-x64 --self-contained
```

### 5.3 デバッグツール

```csharp
// ブレークポイント
public void ProcessData()
{
    var data = GetData();
    // ここにブレークポイントを設定
    var result = Transform(data);
    Save(result);
}

// 条件付きブレークポイント
// ブレークポイントのプロパティで: i == 100

// トレースポイント
System.Diagnostics.Debug.WriteLine($"変数の値: {variable}");
System.Diagnostics.Trace.TraceInformation("情報メッセージ");

// デバッガ属性
[DebuggerDisplay("Name = {Name}, Age = {Age}")]
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}
```

---

## 6. アプリケーション開発の種類

### 6.1 Webアプリケーション

#### ASP.NET Core MVC

```csharp
// Controller
public class HomeController : Controller
{
    private readonly ILogger<HomeController> _logger;

    public HomeController(ILogger<HomeController> logger)
    {
        _logger = logger;
    }

    public IActionResult Index()
    {
        return View();
    }

    [HttpPost]
    public async Task<IActionResult> Submit(FormModel model)
    {
        if (!ModelState.IsValid)
            return View(model);

        await _service.ProcessAsync(model);
        return RedirectToAction(nameof(Success));
    }
}
```

#### Blazor

```csharp
// Blazor Component
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>
<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

### 6.2 デスクトップアプリケーション

#### WPF（Windows Presentation Foundation）

```xml
<!-- XAML -->
<Window x:Class="MyApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        Title="Main Window" Height="450" Width="800">
    <Grid>
        <Button Content="Click Me"
                Command="{Binding ClickCommand}"
                Width="100" Height="30"/>
    </Grid>
</Window>
```

```csharp
// ViewModel
public class MainViewModel : INotifyPropertyChanged
{
    public ICommand ClickCommand { get; }

    public MainViewModel()
    {
        ClickCommand = new RelayCommand(OnClick);
    }

    private void OnClick()
    {
        MessageBox.Show("Button clicked!");
    }
}
```

#### MAUI（Multi-platform App UI）

```csharp
// クロスプラットフォーム（Windows、Mac、iOS、Android）
public class MainPage : ContentPage
{
    public MainPage()
    {
        Content = new StackLayout
        {
            Children =
            {
                new Label { Text = "Welcome to .NET MAUI!" },
                new Button
                {
                    Text = "Click Me",
                    Command = new Command(() => DisplayAlert("Alert", "Button clicked", "OK"))
                }
            }
        };
    }
}
```

### 6.3 モバイルアプリケーション

```csharp
// Xamarin.Forms / .NET MAUI
public class App : Application
{
    public App()
    {
        MainPage = new NavigationPage(new MainPage());
    }
}

// Platform-specific code
#if ANDROID
    // Android固有のコード
#elif IOS
    // iOS固有のコード
#endif
```

### 6.4 コンソールアプリケーション

```csharp
// Program.cs（.NET 6以降のトップレベルステートメント）
using System;
using System.CommandLine;

var rootCommand = new RootCommand("サンプルCLIアプリ");

var nameOption = new Option<string>(
    "--name",
    "名前を指定");

rootCommand.AddOption(nameOption);

rootCommand.SetHandler((string name) =>
{
    Console.WriteLine($"Hello, {name ?? "World"}!");
}, nameOption);

return await rootCommand.InvokeAsync(args);
```

### 6.5 クラウド/サーバーレス

#### Azure Functions

```csharp
public static class HttpTriggerFunction
{
    [FunctionName("HttpTrigger")]
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", "post")] HttpRequest req,
        ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");

        string name = req.Query["name"];

        return new OkObjectResult($"Hello, {name}");
    }
}
```

### 6.6 ゲーム開発

#### Unity（C#スクリプティング）

```csharp
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public float speed = 5.0f;

    void Update()
    {
        float horizontal = Input.GetAxis("Horizontal");
        float vertical = Input.GetAxis("Vertical");

        Vector3 movement = new Vector3(horizontal, 0, vertical);
        transform.Translate(movement * speed * Time.deltaTime);
    }
}
```

---

## 7. 非同期プログラミング

### 7.1 async/await

```csharp
// 基本的な非同期メソッド
public async Task<string> FetchDataAsync(string url)
{
    using var client = new HttpClient();
    var response = await client.GetAsync(url);
    response.EnsureSuccessStatusCode();
    return await response.Content.ReadAsStringAsync();
}

// 並行実行
public async Task<(string, string)> FetchMultipleAsync()
{
    var task1 = FetchDataAsync("https://api1.example.com");
    var task2 = FetchDataAsync("https://api2.example.com");

    await Task.WhenAll(task1, task2);

    return (task1.Result, task2.Result);
}

// エラーハンドリング
public async Task ProcessWithErrorHandlingAsync()
{
    try
    {
        await FetchDataAsync("https://example.com");
    }
    catch (HttpRequestException ex)
    {
        _logger.LogError(ex, "HTTP request failed");
        throw;
    }
}
```

### 7.2 Task Parallel Library（TPL）

```csharp
// Parallel.For
Parallel.For(0, 100, i =>
{
    ProcessItem(i);
});

// Parallel.ForEach
var items = GetItems();
Parallel.ForEach(items, item =>
{
    ProcessItem(item);
});

// Task
var task1 = Task.Run(() => DoWork1());
var task2 = Task.Run(() => DoWork2());
Task.WaitAll(task1, task2);

// Continuations
var task = Task.Run(() => GetData())
    .ContinueWith(t => ProcessData(t.Result))
    .ContinueWith(t => SaveData(t.Result));
```

### 7.3 キャンセレーション

```csharp
public async Task LongRunningOperationAsync(CancellationToken cancellationToken)
{
    for (int i = 0; i < 1000; i++)
    {
        // キャンセル要求をチェック
        cancellationToken.ThrowIfCancellationRequested();

        await ProcessItemAsync(i);

        // または
        if (cancellationToken.IsCancellationRequested)
        {
            // クリーンアップ
            return;
        }
    }
}

// 使用
var cts = new CancellationTokenSource();
var task = LongRunningOperationAsync(cts.Token);

// キャンセル
cts.Cancel();

// タイムアウト付きキャンセル
var cts2 = new CancellationTokenSource(TimeSpan.FromSeconds(30));
```

### 7.4 非同期ストリーム

```csharp
// IAsyncEnumerable（C# 8.0以降）
public async IAsyncEnumerable<int> GenerateNumbersAsync(
    [EnumeratorCancellation] CancellationToken cancellationToken = default)
{
    for (int i = 0; i < 100; i++)
    {
        await Task.Delay(100, cancellationToken);
        yield return i;
    }
}

// 使用
await foreach (var number in GenerateNumbersAsync())
{
    Console.WriteLine(number);
}
```

---

## 8. データアクセスとORM

### 8.1 ADO.NET（低レベルAPI）

```csharp
using var connection = new SqlConnection(connectionString);
await connection.OpenAsync();

using var command = new SqlCommand(
    "SELECT * FROM Users WHERE Age > @age",
    connection);
command.Parameters.AddWithValue("@age", 18);

using var reader = await command.ExecuteReaderAsync();
while (await reader.ReadAsync())
{
    var name = reader.GetString("Name");
    var age = reader.GetInt32("Age");
    Console.WriteLine($"{name}, {age}");
}
```

### 8.2 Entity Framework Core

```csharp
// DbContext定義
public class AppDbContext : DbContext
{
    public DbSet<User> Users { get; set; }
    public DbSet<Order> Orders { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(connectionString);
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>(entity =>
        {
            entity.HasKey(e => e.Id);
            entity.Property(e => e.Name).IsRequired().HasMaxLength(100);
            entity.HasIndex(e => e.Email).IsUnique();
        });
    }
}

// CRUD操作
public class UserRepository
{
    private readonly AppDbContext _context;

    public async Task<User> GetByIdAsync(int id)
    {
        return await _context.Users
            .Include(u => u.Orders)
            .FirstOrDefaultAsync(u => u.Id == id);
    }

    public async Task<List<User>> GetAllAsync()
    {
        return await _context.Users.ToListAsync();
    }

    public async Task AddAsync(User user)
    {
        _context.Users.Add(user);
        await _context.SaveChangesAsync();
    }

    public async Task UpdateAsync(User user)
    {
        _context.Users.Update(user);
        await _context.SaveChangesAsync();
    }

    public async Task DeleteAsync(int id)
    {
        var user = await GetByIdAsync(id);
        if (user != null)
        {
            _context.Users.Remove(user);
            await _context.SaveChangesAsync();
        }
    }
}
```

### 8.3 Dapper（軽量ORM）

```csharp
using Dapper;

// シンプルなクエリ
using var connection = new SqlConnection(connectionString);
var users = await connection.QueryAsync<User>(
    "SELECT * FROM Users WHERE Age > @Age",
    new { Age = 18 });

// 複数結果セット
using var multi = await connection.QueryMultipleAsync(@"
    SELECT * FROM Users;
    SELECT * FROM Orders;");

var users = await multi.ReadAsync<User>();
var orders = await multi.ReadAsync<Order>();

// ストアドプロシージャ
var result = await connection.QueryAsync<User>(
    "GetUsersByAge",
    new { Age = 18 },
    commandType: CommandType.StoredProcedure);
```

---

## 9. デザインパターンとアーキテクチャ

### 9.1 SOLID原則

#### Single Responsibility Principle（単一責任の原則）

```csharp
// 悪い例
public class User
{
    public void Save() { /* DBに保存 */ }
    public void SendEmail() { /* メール送信 */ }
    public void GenerateReport() { /* レポート生成 */ }
}

// 良い例
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class UserRepository
{
    public void Save(User user) { /* DBに保存 */ }
}

public class EmailService
{
    public void SendWelcomeEmail(User user) { /* メール送信 */ }
}

public class ReportGenerator
{
    public Report Generate(User user) { /* レポート生成 */ }
}
```

#### Dependency Inversion Principle（依存性逆転の原則）

```csharp
// 抽象に依存
public interface INotificationService
{
    void Send(string message);
}

public class EmailNotificationService : INotificationService
{
    public void Send(string message)
    {
        // メール送信実装
    }
}

public class SmsNotificationService : INotificationService
{
    public void Send(string message)
    {
        // SMS送信実装
    }
}

public class OrderProcessor
{
    private readonly INotificationService _notificationService;

    public OrderProcessor(INotificationService notificationService)
    {
        _notificationService = notificationService;
    }

    public void Process(Order order)
    {
        // 注文処理
        _notificationService.Send("注文が完了しました");
    }
}
```

### 9.2 デザインパターン

#### Singleton

```csharp
public sealed class Logger
{
    private static readonly Lazy<Logger> _instance =
        new Lazy<Logger>(() => new Logger());

    public static Logger Instance => _instance.Value;

    private Logger() { }

    public void Log(string message)
    {
        Console.WriteLine($"[{DateTime.Now}] {message}");
    }
}
```

#### Factory

```csharp
public interface IPaymentProcessor
{
    void Process(decimal amount);
}

public class PaymentProcessorFactory
{
    public IPaymentProcessor Create(PaymentMethod method)
    {
        return method switch
        {
            PaymentMethod.CreditCard => new CreditCardProcessor(),
            PaymentMethod.PayPal => new PayPalProcessor(),
            PaymentMethod.BankTransfer => new BankTransferProcessor(),
            _ => throw new ArgumentException("Unknown payment method")
        };
    }
}
```

#### Repository Pattern

```csharp
public interface IRepository<T> where T : class
{
    Task<T> GetByIdAsync(int id);
    Task<IEnumerable<T>> GetAllAsync();
    Task AddAsync(T entity);
    Task UpdateAsync(T entity);
    Task DeleteAsync(int id);
}

public class GenericRepository<T> : IRepository<T> where T : class
{
    protected readonly DbContext _context;
    protected readonly DbSet<T> _dbSet;

    public GenericRepository(DbContext context)
    {
        _context = context;
        _dbSet = context.Set<T>();
    }

    public virtual async Task<T> GetByIdAsync(int id)
    {
        return await _dbSet.FindAsync(id);
    }

    public virtual async Task<IEnumerable<T>> GetAllAsync()
    {
        return await _dbSet.ToListAsync();
    }

    public virtual async Task AddAsync(T entity)
    {
        await _dbSet.AddAsync(entity);
        await _context.SaveChangesAsync();
    }

    public virtual async Task UpdateAsync(T entity)
    {
        _dbSet.Update(entity);
        await _context.SaveChangesAsync();
    }

    public virtual async Task DeleteAsync(int id)
    {
        var entity = await GetByIdAsync(id);
        if (entity != null)
        {
            _dbSet.Remove(entity);
            await _context.SaveChangesAsync();
        }
    }
}
```

#### Unit of Work

```csharp
public interface IUnitOfWork : IDisposable
{
    IRepository<User> Users { get; }
    IRepository<Order> Orders { get; }
    Task<int> SaveChangesAsync();
}

public class UnitOfWork : IUnitOfWork
{
    private readonly AppDbContext _context;

    public IRepository<User> Users { get; }
    public IRepository<Order> Orders { get; }

    public UnitOfWork(AppDbContext context)
    {
        _context = context;
        Users = new GenericRepository<User>(context);
        Orders = new GenericRepository<Order>(context);
    }

    public async Task<int> SaveChangesAsync()
    {
        return await _context.SaveChangesAsync();
    }

    public void Dispose()
    {
        _context.Dispose();
    }
}
```

### 9.3 アーキテクチャパターン

#### Clean Architecture

```
src/
├── Domain/              # エンティティ、値オブジェクト
│   ├── Entities/
│   ├── ValueObjects/
│   └── Interfaces/
├── Application/         # ユースケース、サービス
│   ├── UseCases/
│   ├── DTOs/
│   └── Interfaces/
├── Infrastructure/      # データアクセス、外部サービス
│   ├── Persistence/
│   ├── Services/
│   └── Repositories/
└── Presentation/        # UI、API
    ├── Controllers/
    └── ViewModels/
```

```csharp
// Domain Layer
public class Order
{
    public int Id { get; private set; }
    public decimal Total { get; private set; }
    public OrderStatus Status { get; private set; }

    public void CalculateTotal(IEnumerable<OrderItem> items)
    {
        Total = items.Sum(i => i.Price * i.Quantity);
    }
}

// Application Layer
public class CreateOrderUseCase
{
    private readonly IOrderRepository _orderRepository;
    private readonly IUnitOfWork _unitOfWork;

    public async Task<OrderDto> ExecuteAsync(CreateOrderDto dto)
    {
        var order = new Order();
        order.CalculateTotal(dto.Items);

        await _orderRepository.AddAsync(order);
        await _unitOfWork.SaveChangesAsync();

        return MapToDto(order);
    }
}

// Infrastructure Layer
public class OrderRepository : IOrderRepository
{
    private readonly AppDbContext _context;

    public async Task AddAsync(Order order)
    {
        await _context.Orders.AddAsync(order);
    }
}

// Presentation Layer
[ApiController]
[Route("api/[controller]")]
public class OrdersController : ControllerBase
{
    private readonly CreateOrderUseCase _createOrderUseCase;

    [HttpPost]
    public async Task<ActionResult<OrderDto>> Create(CreateOrderDto dto)
    {
        var result = await _createOrderUseCase.ExecuteAsync(dto);
        return CreatedAtAction(nameof(GetById), new { id = result.Id }, result);
    }
}
```

---

## 10. テストとデバッグ

### 10.1 ユニットテスト

#### xUnit

```csharp
public class CalculatorTests
{
    [Fact]
    public void Add_TwoNumbers_ReturnsSum()
    {
        // Arrange
        var calculator = new Calculator();

        // Act
        var result = calculator.Add(2, 3);

        // Assert
        Assert.Equal(5, result);
    }

    [Theory]
    [InlineData(2, 3, 5)]
    [InlineData(0, 0, 0)]
    [InlineData(-1, 1, 0)]
    public void Add_MultipleScenarios_ReturnsExpectedResult(int a, int b, int expected)
    {
        var calculator = new Calculator();
        var result = calculator.Add(a, b);
        Assert.Equal(expected, result);
    }
}
```

#### NUnit

```csharp
[TestFixture]
public class UserServiceTests
{
    private UserService _userService;
    private Mock<IUserRepository> _mockRepository;

    [SetUp]
    public void Setup()
    {
        _mockRepository = new Mock<IUserRepository>();
        _userService = new UserService(_mockRepository.Object);
    }

    [Test]
    public async Task GetUser_ValidId_ReturnsUser()
    {
        // Arrange
        var expectedUser = new User { Id = 1, Name = "Test" };
        _mockRepository
            .Setup(r => r.GetByIdAsync(1))
            .ReturnsAsync(expectedUser);

        // Act
        var result = await _userService.GetUserAsync(1);

        // Assert
        Assert.That(result, Is.Not.Null);
        Assert.That(result.Name, Is.EqualTo("Test"));
    }

    [TearDown]
    public void TearDown()
    {
        _userService?.Dispose();
    }
}
```

### 10.2 モッキング（Moq）

```csharp
public class OrderServiceTests
{
    [Fact]
    public async Task ProcessOrder_ValidOrder_CallsRepository()
    {
        // Arrange
        var mockRepository = new Mock<IOrderRepository>();
        var mockEmailService = new Mock<IEmailService>();
        var service = new OrderService(
            mockRepository.Object,
            mockEmailService.Object);

        var order = new Order { Id = 1, Total = 100 };

        // Act
        await service.ProcessOrderAsync(order);

        // Assert
        mockRepository.Verify(
            r => r.SaveAsync(It.IsAny<Order>()),
            Times.Once);
        mockEmailService.Verify(
            e => e.SendOrderConfirmationAsync(It.IsAny<Order>()),
            Times.Once);
    }

    [Fact]
    public async Task GetOrder_RepositoryThrows_PropagatesException()
    {
        // Arrange
        var mockRepository = new Mock<IOrderRepository>();
        mockRepository
            .Setup(r => r.GetByIdAsync(It.IsAny<int>()))
            .ThrowsAsync(new InvalidOperationException());

        var service = new OrderService(mockRepository.Object, null);

        // Act & Assert
        await Assert.ThrowsAsync<InvalidOperationException>(
            () => service.GetOrderAsync(1));
    }
}
```

### 10.3 統合テスト

```csharp
public class ApiIntegrationTests : IClassFixture<WebApplicationFactory<Program>>
{
    private readonly WebApplicationFactory<Program> _factory;
    private readonly HttpClient _client;

    public ApiIntegrationTests(WebApplicationFactory<Program> factory)
    {
        _factory = factory;
        _client = factory.CreateClient();
    }

    [Fact]
    public async Task GetUsers_ReturnsSuccessStatusCode()
    {
        // Act
        var response = await _client.GetAsync("/api/users");

        // Assert
        response.EnsureSuccessStatusCode();
        var content = await response.Content.ReadAsStringAsync();
        Assert.NotEmpty(content);
    }

    [Fact]
    public async Task CreateUser_ValidData_ReturnsCreated()
    {
        // Arrange
        var newUser = new { Name = "Test User", Email = "test@example.com" };
        var content = new StringContent(
            JsonSerializer.Serialize(newUser),
            Encoding.UTF8,
            "application/json");

        // Act
        var response = await _client.PostAsync("/api/users", content);

        // Assert
        Assert.Equal(HttpStatusCode.Created, response.StatusCode);
    }
}
```

### 10.4 カバレッジ

```bash
# .NET CLI でカバレッジ計測
dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=opencover

# ReportGeneratorでHTMLレポート生成
dotnet tool install -g dotnet-reportgenerator-globaltool
reportgenerator -reports:coverage.opencover.xml -targetdir:coveragereport
```

---

## 11. パフォーマンスとメモリ管理

### 11.1 ガベージコレクション

```csharp
// IDisposableパターン
public class ResourceManager : IDisposable
{
    private bool _disposed = false;
    private FileStream _fileStream;

    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    protected virtual void Dispose(bool disposing)
    {
        if (_disposed)
            return;

        if (disposing)
        {
            // マネージドリソースの解放
            _fileStream?.Dispose();
        }

        // アンマネージドリソースの解放

        _disposed = true;
    }

    ~ResourceManager()
    {
        Dispose(false);
    }
}

// usingステートメント
using (var resource = new ResourceManager())
{
    // リソース使用
} // 自動的にDispose呼び出し

// using宣言（C# 8.0以降）
using var resource = new ResourceManager();
// スコープ終了時に自動的にDispose
```

### 11.2 Span<T>とMemory<T>

```csharp
// Span<T>による効率的なメモリ操作
public void ProcessData(ReadOnlySpan<byte> data)
{
    // スタック割り当て
    Span<byte> buffer = stackalloc byte[256];

    // スライス（コピーなし）
    var slice = data.Slice(0, 10);

    // 操作
    for (int i = 0; i < slice.Length; i++)
    {
        buffer[i] = (byte)(slice[i] * 2);
    }
}

// Memory<T>による非同期対応
public async Task<int> ProcessAsync(Memory<byte> buffer)
{
    return await ReadDataAsync(buffer);
}

// 文字列操作の最適化
public ReadOnlySpan<char> GetFileExtension(ReadOnlySpan<char> path)
{
    int index = path.LastIndexOf('.');
    return index >= 0 ? path.Slice(index) : ReadOnlySpan<char>.Empty;
}
```

### 11.3 パフォーマンス最適化

```csharp
// StringBuilder（文字列連結）
public string BuildString(IEnumerable<string> items)
{
    var sb = new StringBuilder();
    foreach (var item in items)
    {
        sb.Append(item);
        sb.Append(", ");
    }
    return sb.ToString();
}

// ArrayPool（配列の再利用）
public void ProcessLargeData()
{
    var pool = ArrayPool<byte>.Shared;
    byte[] buffer = pool.Rent(1024);

    try
    {
        // バッファ使用
    }
    finally
    {
        pool.Return(buffer);
    }
}

// ValueTask（非同期の最適化）
public async ValueTask<int> GetCachedValueAsync(string key)
{
    if (_cache.TryGetValue(key, out int value))
    {
        // 同期的に完了（割り当てなし）
        return value;
    }

    // 非同期に取得
    value = await FetchFromDatabaseAsync(key);
    _cache[key] = value;
    return value;
}

// Lazy<T>（遅延初期化）
private readonly Lazy<ExpensiveResource> _resource =
    new Lazy<ExpensiveResource>(() => new ExpensiveResource());

public ExpensiveResource Resource => _resource.Value;
```

### 11.4 ベンチマーキング（BenchmarkDotNet）

```csharp
[MemoryDiagnoser]
public class StringConcatenationBenchmark
{
    private readonly string[] _items = Enumerable.Range(0, 100)
        .Select(i => $"Item{i}")
        .ToArray();

    [Benchmark]
    public string UsingPlus()
    {
        string result = "";
        foreach (var item in _items)
            result += item;
        return result;
    }

    [Benchmark]
    public string UsingStringBuilder()
    {
        var sb = new StringBuilder();
        foreach (var item in _items)
            sb.Append(item);
        return sb.ToString();
    }

    [Benchmark]
    public string UsingJoin()
    {
        return string.Join("", _items);
    }
}

// 実行
// dotnet run -c Release
```

---

## 12. セキュリティ

### 12.1 認証と認可

#### ASP.NET Core Identity

```csharp
// Startup/Program.cs
builder.Services.AddIdentity<ApplicationUser, IdentityRole>()
    .AddEntityFrameworkStores<ApplicationDbContext>()
    .AddDefaultTokenProviders();

builder.Services.Configure<IdentityOptions>(options =>
{
    options.Password.RequireDigit = true;
    options.Password.RequiredLength = 8;
    options.Lockout.MaxFailedAccessAttempts = 5;
});

// ユーザー登録
public class AccountController : Controller
{
    private readonly UserManager<ApplicationUser> _userManager;
    private readonly SignInManager<ApplicationUser> _signInManager;

    [HttpPost]
    public async Task<IActionResult> Register(RegisterViewModel model)
    {
        if (ModelState.IsValid)
        {
            var user = new ApplicationUser
            {
                UserName = model.Email,
                Email = model.Email
            };

            var result = await _userManager.CreateAsync(user, model.Password);

            if (result.Succeeded)
            {
                await _signInManager.SignInAsync(user, isPersistent: false);
                return RedirectToAction("Index", "Home");
            }

            foreach (var error in result.Errors)
            {
                ModelState.AddModelError(string.Empty, error.Description);
            }
        }

        return View(model);
    }
}
```

#### JWT認証

```csharp
// JWTトークン生成
public string GenerateJwtToken(User user)
{
    var claims = new[]
    {
        new Claim(ClaimTypes.NameIdentifier, user.Id.ToString()),
        new Claim(ClaimTypes.Name, user.Username),
        new Claim(ClaimTypes.Email, user.Email),
        new Claim(ClaimTypes.Role, user.Role)
    };

    var key = new SymmetricSecurityKey(
        Encoding.UTF8.GetBytes(_configuration["Jwt:Key"]));
    var credentials = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);

    var token = new JwtSecurityToken(
        issuer: _configuration["Jwt:Issuer"],
        audience: _configuration["Jwt:Audience"],
        claims: claims,
        expires: DateTime.Now.AddHours(1),
        signingCredentials: credentials);

    return new JwtSecurityTokenHandler().WriteToken(token);
}

// API認可
[Authorize(Roles = "Admin")]
[HttpGet("admin/users")]
public async Task<IActionResult> GetAllUsers()
{
    var users = await _userService.GetAllAsync();
    return Ok(users);
}

[Authorize(Policy = "MinimumAge")]
[HttpGet("restricted")]
public IActionResult RestrictedContent()
{
    return Ok("制限されたコンテンツ");
}
```

### 12.2 データ保護

```csharp
// パスワードハッシング
public class PasswordHasher
{
    public string HashPassword(string password)
    {
        return BCrypt.Net.BCrypt.HashPassword(password);
    }

    public bool VerifyPassword(string password, string hash)
    {
        return BCrypt.Net.BCrypt.Verify(password, hash);
    }
}

// データ暗号化
public class EncryptionService
{
    private readonly IDataProtector _protector;

    public EncryptionService(IDataProtectionProvider provider)
    {
        _protector = provider.CreateProtector("MyApp.EncryptionService");
    }

    public string Encrypt(string plainText)
    {
        return _protector.Protect(plainText);
    }

    public string Decrypt(string cipherText)
    {
        return _protector.Unprotect(cipherText);
    }
}
```

### 12.3 入力検証

```csharp
// データアノテーション
public class UserRegistrationDto
{
    [Required(ErrorMessage = "ユーザー名は必須です")]
    [StringLength(50, MinimumLength = 3)]
    public string Username { get; set; }

    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [StringLength(100, MinimumLength = 8)]
    [RegularExpression(@"^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).+$",
        ErrorMessage = "パスワードは大文字、小文字、数字を含む必要があります")]
    public string Password { get; set; }

    [Compare("Password")]
    public string ConfirmPassword { get; set; }

    [Range(18, 120)]
    public int Age { get; set; }
}

// カスタム検証
public class ValidUrlAttribute : ValidationAttribute
{
    protected override ValidationResult IsValid(object value, ValidationContext validationContext)
    {
        if (value is string url && Uri.TryCreate(url, UriKind.Absolute, out _))
        {
            return ValidationResult.Success;
        }

        return new ValidationResult("無効なURLです");
    }
}
```

### 12.4 SQLインジェクション対策

```csharp
// パラメータ化クエリ（推奨）
public async Task<User> GetUserAsync(string username)
{
    using var connection = new SqlConnection(_connectionString);
    var command = new SqlCommand(
        "SELECT * FROM Users WHERE Username = @username",
        connection);
    command.Parameters.AddWithValue("@username", username);

    await connection.OpenAsync();
    using var reader = await command.ExecuteReaderAsync();
    // ...
}

// Entity Framework Core（安全）
var user = await _context.Users
    .FirstOrDefaultAsync(u => u.Username == username);

// 危険な例（使用しない）
// var query = $"SELECT * FROM Users WHERE Username = '{username}'";
```

---

## 13. パッケージ管理とデプロイメント

### 13.1 NuGetパッケージ管理

```bash
# パッケージのインストール
dotnet add package Newtonsoft.Json
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 8.0.0

# パッケージの削除
dotnet remove package Newtonsoft.Json

# パッケージの更新
dotnet list package --outdated
dotnet add package PackageName --version x.x.x

# パッケージの復元
dotnet restore
```

```xml
<!-- .csproj ファイル -->
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <Nullable>enable</Nullable>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Swashbuckle.AspNetCore" Version="6.5.0" />
    <PackageReference Include="Serilog.AspNetCore" Version="8.0.0" />
    <PackageReference Include="AutoMapper.Extensions.Microsoft.DependencyInjection" Version="12.0.1" />
  </ItemGroup>
</Project>
```

### 13.2 発行とデプロイメント

```bash
# リリースビルド
dotnet publish -c Release

# 自己完結型デプロイメント
dotnet publish -c Release -r win-x64 --self-contained

# フレームワーク依存デプロイメント
dotnet publish -c Release -r linux-x64 --self-contained false

# シングルファイル発行
dotnet publish -c Release -r win-x64 --self-contained -p:PublishSingleFile=true

# トリミング（未使用コードの削除）
dotnet publish -c Release -p:PublishTrimmed=true
```

### 13.3 Docker

```dockerfile
# Dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["MyApp/MyApp.csproj", "MyApp/"]
RUN dotnet restore "MyApp/MyApp.csproj"
COPY . .
WORKDIR "/src/MyApp"
RUN dotnet build "MyApp.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MyApp.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MyApp.dll"]
```

```bash
# Dockerイメージのビルドと実行
docker build -t myapp .
docker run -d -p 8080:80 myapp
```

### 13.4 CI/CD

#### GitHub Actions

```yaml
# .github/workflows/dotnet.yml
name: .NET CI/CD

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3

    - name: Setup .NET
      uses: actions/setup-dotnet@v3
      with:
        dotnet-version: 8.0.x

    - name: Restore dependencies
      run: dotnet restore

    - name: Build
      run: dotnet build --no-restore

    - name: Test
      run: dotnet test --no-build --verbosity normal

    - name: Publish
      run: dotnet publish -c Release -o ./publish

    - name: Deploy to Azure
      uses: azure/webapps-deploy@v2
      with:
        app-name: my-web-app
        publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
        package: ./publish
```

---

## 14. ベストプラクティス

### 14.1 コーディング規約

```csharp
// 命名規則
public class ProductService  // PascalCase（クラス、メソッド、プロパティ）
{
    private readonly IProductRepository _repository;  // camelCase with _ (private)
    public const int MaxItems = 100;  // PascalCase（定数）

    public async Task<Product> GetProductAsync(int productId)  // PascalCase
    {
        var product = await _repository.GetByIdAsync(productId);  // camelCase
        return product;
    }
}

// インターフェース（I接頭辞）
public interface IProductRepository { }

// 非同期メソッド（Async接尾辞）
public async Task<string> FetchDataAsync() { }

// Boolean（Is, Has, Can接頭辞）
public bool IsValid { get; set; }
public bool HasPermission { get; set; }
public bool CanExecute { get; set; }
```

### 14.2 エラーハンドリング

```csharp
// 適切な例外処理
public async Task<User> GetUserAsync(int id)
{
    try
    {
        var user = await _repository.GetByIdAsync(id);

        if (user == null)
            throw new NotFoundException($"User with ID {id} not found");

        return user;
    }
    catch (DbException ex)
    {
        _logger.LogError(ex, "Database error while fetching user {UserId}", id);
        throw new DataAccessException("Failed to retrieve user", ex);
    }
}

// カスタム例外
public class NotFoundException : Exception
{
    public NotFoundException(string message) : base(message) { }
}

// グローバル例外ハンドラ（ASP.NET Core）
public class GlobalExceptionHandler : IExceptionHandler
{
    public async ValueTask<bool> TryHandleAsync(
        HttpContext httpContext,
        Exception exception,
        CancellationToken cancellationToken)
    {
        var response = exception switch
        {
            NotFoundException => (StatusCodes.Status404NotFound, "Not Found"),
            ValidationException => (StatusCodes.Status400BadRequest, "Validation Error"),
            _ => (StatusCodes.Status500InternalServerError, "Internal Server Error")
        };

        httpContext.Response.StatusCode = response.Item1;
        await httpContext.Response.WriteAsJsonAsync(
            new { error = response.Item2 },
            cancellationToken);

        return true;
    }
}
```

### 14.3 ロギング

```csharp
// ILogger（推奨）
public class OrderService
{
    private readonly ILogger<OrderService> _logger;

    public OrderService(ILogger<OrderService> logger)
    {
        _logger = logger;
    }

    public async Task ProcessOrderAsync(Order order)
    {
        _logger.LogInformation(
            "Processing order {OrderId} for user {UserId}",
            order.Id,
            order.UserId);

        try
        {
            await _repository.SaveAsync(order);
            _logger.LogInformation("Order {OrderId} processed successfully", order.Id);
        }
        catch (Exception ex)
        {
            _logger.LogError(
                ex,
                "Failed to process order {OrderId}",
                order.Id);
            throw;
        }
    }
}

// Serilog設定
builder.Host.UseSerilog((context, configuration) =>
{
    configuration
        .ReadFrom.Configuration(context.Configuration)
        .Enrich.FromLogContext()
        .WriteTo.Console()
        .WriteTo.File("logs/log-.txt", rollingInterval: RollingInterval.Day);
});
```

### 14.4 依存性注入

```csharp
// サービス登録（Program.cs）
var builder = WebApplication.CreateBuilder(args);

// Singleton（アプリケーション全体で1つのインスタンス）
builder.Services.AddSingleton<IConfiguration>(builder.Configuration);

// Scoped（リクエストごとに1つのインスタンス）
builder.Services.AddScoped<IUserRepository, UserRepository>();
builder.Services.AddScoped<IOrderService, OrderService>();

// Transient（毎回新しいインスタンス）
builder.Services.AddTransient<IEmailService, EmailService>();

// DbContext
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("Default")));

// HttpClient
builder.Services.AddHttpClient<IApiClient, ApiClient>();

// 使用
public class OrderController : ControllerBase
{
    private readonly IOrderService _orderService;
    private readonly ILogger<OrderController> _logger;

    // コンストラクタインジェクション
    public OrderController(
        IOrderService orderService,
        ILogger<OrderController> logger)
    {
        _orderService = orderService;
        _logger = logger;
    }
}
```

### 14.5 設定管理

```json
// appsettings.json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=MyDb;User Id=sa;Password=***;"
  },
  "JwtSettings": {
    "Key": "your-secret-key",
    "Issuer": "your-app",
    "Audience": "your-users",
    "ExpirationMinutes": 60
  },
  "EmailSettings": {
    "SmtpServer": "smtp.example.com",
    "Port": 587,
    "Username": "user@example.com"
  }
}
```

```csharp
// 設定クラス
public class JwtSettings
{
    public string Key { get; set; }
    public string Issuer { get; set; }
    public string Audience { get; set; }
    public int ExpirationMinutes { get; set; }
}

// 登録
builder.Services.Configure<JwtSettings>(
    builder.Configuration.GetSection("JwtSettings"));

// 使用
public class AuthService
{
    private readonly JwtSettings _jwtSettings;

    public AuthService(IOptions<JwtSettings> jwtSettings)
    {
        _jwtSettings = jwtSettings.Value;
    }
}
```

---

## まとめ

このドキュメントでは、C#を使用したソフトウェア開発の主要な要素を体系的にまとめました：

### 学習ロードマップ

1. **基礎レベル**
   - C#の基本文法とデータ型
   - オブジェクト指向プログラミングの概念
   - .NET CLIとVisual Studioの使い方

2. **中級レベル**
   - LINQ、ジェネリクス、デリゲート
   - 非同期プログラミング（async/await）
   - Entity Framework Coreによるデータアクセス
   - ASP.NET Core基礎

3. **上級レベル**
   - デザインパターンとアーキテクチャ
   - パフォーマンス最適化
   - セキュリティベストプラクティス
   - マイクロサービスアーキテクチャ

4. **実践レベル**
   - CI/CD パイプライン構築
   - クラウドデプロイメント（Azure、AWS）
   - 大規模アプリケーション設計
   - チーム開発とコードレビュー

### 推奨リソース

**公式ドキュメント**
- [Microsoft Learn - C#](https://learn.microsoft.com/ja-jp/dotnet/csharp/)
- [.NET Documentation](https://learn.microsoft.com/ja-jp/dotnet/)
- [ASP.NET Core Documentation](https://learn.microsoft.com/ja-jp/aspnet/core/)

**コミュニティ**
- [Stack Overflow - C#](https://stackoverflow.com/questions/tagged/c%23)
- [GitHub - dotnet](https://github.com/dotnet)
- [C# Discord](https://discord.gg/csharp)

**書籍**
- 『C# 実践開発手法』
- 『Effective C#』
- 『CLR via C#』

---

最終更新: 2025年11月
