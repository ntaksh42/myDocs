# C# 14 新機能まとめ

## 概要

C# 14は.NET 10と共にリリースされた最新バージョンで、2025年11月11日に正式リリースされました。.NET 10はLTS（Long Term Support）リリースとして3年間サポートされます。

公式ドキュメント：[What's new in C# 14 | Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-14)

---

## 主要な新機能

### 1. Extension Members（拡張メンバー）

従来のC#では拡張メソッドのみが利用可能でしたが、C# 14では拡張プロパティや静的拡張メンバーも定義できるようになりました。

#### 従来の拡張メソッド（C# 3.0以降）

```csharp
public static class StringExtensions
{
    public static bool IsNullOrEmpty(this string str)
    {
        return string.IsNullOrEmpty(str);
    }
}

// 使用例
string text = "Hello";
bool isEmpty = text.IsNullOrEmpty(); // false
```

#### 新しい拡張メンバー構文（C# 14）

```csharp
// 拡張プロパティの定義
public implicit extension StringExtension for string
{
    // インスタンス拡張プロパティ
    public int WordCount => this.Split(' ', StringSplitOptions.RemoveEmptyEntries).Length;

    // 静的拡張プロパティ
    public static string DefaultValue => string.Empty;

    // 静的拡張メソッド
    public static string Combine(params string[] values)
    {
        return string.Join(" ", values);
    }
}

// 使用例
string sentence = "Hello World from C# 14";
int count = sentence.WordCount; // 5

string defaultStr = string.DefaultValue; // "" (拡張された静的プロパティ)
string combined = string.Combine("C#", "14", "is", "great"); // "C# 14 is great"
```

---

### 2. First-class Span Support（Span型のファーストクラスサポート）

`System.Span<T>`と`System.ReadOnlySpan<T>`に対する暗黙的な型変換がサポートされ、より自然なプログラミングが可能になりました。

```csharp
using System;

public class SpanExample
{
    // C# 14では配列からSpanへの暗黙的変換がより自然に
    public void ProcessData()
    {
        int[] numbers = { 1, 2, 3, 4, 5 };

        // 暗黙的な変換が改善
        ProcessSpan(numbers); // 直接渡せる

        // ReadOnlySpanも同様
        ReadOnlySpan<int> readOnlyNumbers = numbers;
        PrintSum(readOnlyNumbers);
    }

    public void ProcessSpan(Span<int> data)
    {
        for (int i = 0; i < data.Length; i++)
        {
            data[i] *= 2;
        }
    }

    public void PrintSum(ReadOnlySpan<int> data)
    {
        int sum = 0;
        foreach (var item in data)
        {
            sum += item;
        }
        Console.WriteLine($"Sum: {sum}");
    }

    // インライン配列からSpanへの変換もサポート
    public void InlineArrayExample()
    {
        Span<int> numbers = [1, 2, 3, 4, 5]; // コレクション式からSpanへ
        ProcessSpan(numbers);
    }
}
```

---

### 3. Field-backed Properties（フィールド支援プロパティ）

明示的なバッキングフィールドを宣言せずに、`field`キーワードでコンパイラが自動生成するバッキングフィールドにアクセスできます。

#### 従来の方法（C# 13まで）

```csharp
public class Person
{
    private string _name; // 明示的なバッキングフィールド

    public string Name
    {
        get => _name;
        set
        {
            if (string.IsNullOrWhiteSpace(value))
                throw new ArgumentException("Name cannot be empty");
            _name = value;
        }
    }
}
```

#### 新しい方法（C# 14）

```csharp
public class Person
{
    // fieldキーワードでバッキングフィールドに直接アクセス
    public string Name
    {
        get => field;
        set
        {
            if (string.IsNullOrWhiteSpace(value))
                throw new ArgumentException("Name cannot be empty");
            field = value;
        }
    }

    // 初期値の設定も可能
    public int Age
    {
        get => field;
        set => field = value < 0 ? 0 : value;
    } = 0; // 初期値
}

// 使用例
public class Example
{
    public void Demo()
    {
        var person = new Person { Name = "Alice", Age = 30 };
        Console.WriteLine($"{person.Name} is {person.Age} years old");

        // person.Name = ""; // 例外がスローされる
        person.Age = -5; // 0に設定される
    }
}
```

---

### 4. Unbound Generic Types in nameof()（nameof()での非バインドジェネリック型）

`nameof()`演算子で非バインド型（型パラメータを指定しないジェネリック型）を使用できるようになりました。

```csharp
using System;
using System.Collections.Generic;

public class NameofExample
{
    public void Demo()
    {
        // C# 14: 非バインドジェネリック型が使用可能
        string listName = nameof(List<>); // "List"
        string dictName = nameof(Dictionary<,>); // "Dictionary"

        Console.WriteLine(listName);  // 出力: List
        Console.WriteLine(dictName);  // 出力: Dictionary

        // 従来通り、バインドされた型も使用可能
        string boundListName = nameof(List<int>); // "List"

        // カスタムジェネリック型でも使用可能
        string myGenericName = nameof(MyGenericClass<,>); // "MyGenericClass"
    }
}

public class MyGenericClass<T, U>
{
    public void PrintTypeName()
    {
        // 型パラメータ名も取得可能
        Console.WriteLine(nameof(T)); // "T"
        Console.WriteLine(nameof(U)); // "U"
    }
}
```

---

### 5. Lambda Parameter Modifiers（ラムダパラメータ修飾子）

ラムダ式のパラメータに`ref`、`in`、`out`、`ref readonly`、`scoped`などの修飾子を型指定なしで使用できるようになりました。

```csharp
using System;

public class LambdaModifiersExample
{
    public void Demo()
    {
        // refパラメータを使用したラムダ
        var incrementer = (ref int x) => x++;

        int value = 5;
        incrementer(ref value);
        Console.WriteLine(value); // 出力: 6

        // inパラメータ（読み取り専用参照）
        var reader = (in int x) => Console.WriteLine($"Value: {x}");
        reader(in value);

        // outパラメータ
        var initializer = (out int x) => x = 100;
        initializer(out int result);
        Console.WriteLine(result); // 出力: 100

        // scopedパラメータ（スコープ制限）
        var scopedLambda = (scoped Span<int> span) =>
        {
            // spanはこのラムダ内でのみ有効
            foreach (var item in span)
            {
                Console.Write($"{item} ");
            }
        };

        Span<int> numbers = stackalloc int[] { 1, 2, 3, 4, 5 };
        scopedLambda(numbers);
        Console.WriteLine();

        // ref readonlyパラメータ
        var readOnlyReader = (ref readonly int x) =>
        {
            Console.WriteLine($"Read-only value: {x}");
            // x = 10; // コンパイルエラー: 読み取り専用
        };

        readOnlyReader(in value);
    }

    // デリゲートと組み合わせた使用例
    public delegate void RefAction<T>(ref T value);

    public void DelegateExample()
    {
        RefAction<int> action = (ref int x) => x *= 2;

        int number = 10;
        action(ref number);
        Console.WriteLine(number); // 出力: 20
    }
}
```

---

### 6. Null-conditional Assignment（null条件付き代入）

null条件演算子（`?.`と`?[]`）を代入の左辺で使用できるようになりました。

```csharp
using System;
using System.Collections.Generic;

public class NullConditionalAssignmentExample
{
    public class Person
    {
        public string Name { get; set; }
        public Address Address { get; set; }
    }

    public class Address
    {
        public string City { get; set; }
        public string Street { get; set; }
    }

    public void Demo()
    {
        Person person = new Person { Name = "Alice" };

        // C# 14: null条件代入
        // Addressがnullでない場合のみCityを設定
        person.Address?.City = "Tokyo";
        Console.WriteLine(person.Address?.City ?? "No address"); // 出力: No address

        // Addressを初期化
        person.Address = new Address();
        person.Address?.City = "Tokyo";
        Console.WriteLine(person.Address.City); // 出力: Tokyo

        // 配列やコレクションでも使用可能
        List<int> numbers = null;
        numbers?[0] = 100; // numbersがnullなので何もしない

        numbers = new List<int> { 1, 2, 3 };
        numbers?[0] = 100; // numbersがnullでないので代入
        Console.WriteLine(numbers[0]); // 出力: 100

        // 複合代入演算子も使用可能
        person.Address?.City += " - Shibuya";
        Console.WriteLine(person.Address.City); // 出力: Tokyo - Shibuya

        // チェーンも可能
        Dictionary<string, Person> people = new()
        {
            ["key1"] = new Person { Name = "Bob", Address = new Address() }
        };

        people?["key1"]?.Address.Street = "Main Street";
        Console.WriteLine(people["key1"].Address.Street); // 出力: Main Street
    }

    // 実践的な使用例
    public void PracticalExample()
    {
        var config = GetConfiguration(); // nullを返す可能性がある

        // C# 14: null安全な設定更新
        config?.Settings?["timeout"] = "30";
        config?.Settings?["retries"] = "3";

        // 従来の方法（C# 13まで）
        if (config != null && config.Settings != null)
        {
            config.Settings["timeout"] = "30";
            config.Settings["retries"] = "3";
        }
    }

    private Configuration GetConfiguration() => null;

    public class Configuration
    {
        public Dictionary<string, string> Settings { get; set; }
    }
}
```

---

## C# 14の利点

1. **コードの簡潔性**: Field-backed PropertiesやNull-conditional Assignmentにより、ボイラープレートコードが削減されます

2. **拡張性の向上**: Extension Membersにより、プロパティや静的メンバーも拡張できるようになり、より柔軟なAPI設計が可能です

3. **パフォーマンス**: First-class Span Supportにより、メモリ効率の良いコードがより書きやすくなります

4. **型安全性**: Lambda Parameter Modifiersにより、ラムダ式でもより厳密な型制約を表現できます

5. **保守性**: Unbound Generic Types in nameof()により、リファクタリングに強いコードが書けます

---

## 動作環境

- **.NET 10 SDK**: C# 14を使用するには.NET 10 SDKが必要です
- **Visual Studio 2022**: 最新バージョンで.NET 10 SDKが含まれています
- **LTSサポート**: .NET 10は3年間のLTSサポートが提供されます

---

## 参考リンク

- [What's new in C# 14 | Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-14)
- [Announcing .NET 10 - .NET Blog](https://devblogs.microsoft.com/dotnet/announcing-dotnet-10/)
- [C# 14 - Exploring extension members - .NET Blog](https://devblogs.microsoft.com/dotnet/csharp-exploring-extension-members/)
- [What's new in .NET 10 | Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-10/overview)

---

## まとめ

C# 14は、コードの簡潔性、パフォーマンス、拡張性を大幅に向上させる重要なアップデートです。特にExtension MembersとField-backed Propertiesは、日常的なコーディングを大きく改善する機能と言えます。.NET 10のLTSサポートと合わせて、長期的に安定した開発環境が提供されます。
