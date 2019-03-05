# Practical C#

A collection of mostly reasonable guidelines and best practices to write clean, readable, understandable and maintainable C# code.

# Coding Conventions

## Types

### Use `var` whenever appropriate and possible

✅ GOOD
```csharp
var httpClient = new HttpClient();
```

❌ BAD
```csharp
HttpClient httpClient = new HttpClient();
```

---

## Instantiation

### Use object initializers over direct property assignment

✅ GOOD
```csharp
var user = new User
{
  Username = "admin",
  Age = 31
};
```

❌ BAD
```csharp
var user = new User();
user.Username = "admin";
user.Age = 31;
```

---

## String literals

### Use string interpolation instead of `string.Format`

✅ GOOD
```csharp
var url = "http://localhost/api";
var resource = "users";
var path = $"{url}/{resource}";
```

❌ BAD
```csharp
var url = "http://localhost/api";
var resource = "users";
var path = string.Format("{0}/{1}", url, resource);
```

### Use the [verbatim string identifier](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/tokens/verbatim) where appropriate

✅ GOOD
```csharp
var path = @"C:\Users\Administrator\Documents";
```

❌ BAD
```csharp
var path = "C:\\Users\\Administrator\\Documents";
```

### Use constants when using string literals more than once

✅ GOOD
```csharp
const string error = "user_not_found";

Log.Error(error);
return BadRequest(error);
```

❌ BAD
```csharp
Log.Error("user_not_found");
return BadRequest("user_not_found");
```

---

## Naming

### Avoid unreasonable abbreviations and meaningless, short names. It's 2019 and everyone has code completion. Please don't say that you're saving time by naming your variables `xyz`. It's just ridiculous.

✅ GOOD
```csharp
// Nice
var validationResult = validator.Validate();

// Nice
var stringBuilder = new StringBuilder();

// Nice
const string directorySeparator = "/";
```

❌ BAD
```csharp
// 🤔
var res = validator.Validate();

// 🤔
var sbd = new StringBuilder();

// 😢 Seriously?
const string dsep = "/";
```

### Write code where the purpose and the member type can be inferred from the semantics

✅ GOOD
```csharp
// ✅ The purpose of this class can be easily inferred
public class OrderManager
{
  // ✅ Using "Is" or "Has" as prefix clearly indicates that a method returns a boolean value
  public bool IsFulfilled(Order order) 
  {
  }

  public bool HasPositions(Order order)
  {
  }

  // ✅ Using a verb clearly indicates that a method performs some action
  public void ProcessOrder(Order order)
  {
  }

  public void CancelOrder(Order order)
  {
  }
}
```

❌ BAD
```csharp
// ❌ Purpose of this class can not be easily inferred
public class OrderHelper
{
  // ❌ Unclear
  public bool Fulfilled(Order order) 
  {
  }

  // ❌ Unclear => users would likely expect a method that retrieves the positions of the order due to the verb "Get"
  public bool GetPositions(Order order)
  {
  }

  // ❌ Unclear
  public void Order(Order order)
  {
  }

  // ❌ Unclear
  public void StopOrder(Order order)
  {
  }
}
```

### Avoid redundancy and providing information that can be inferred from the context

✅ GOOD
```csharp
// Clearly an interface
public interface IOrderManager { }

// Clearly a list of orders
private IList<Order> orders;
```

❌ BAD
```csharp
// Clearly an interface => no "Interface" postfix necessary
public interface IOrderManagerInterface { }

// Clearly a list of oders => no "List" postfix necessary
private IList<Order> orderList;
```

### Write code that is self-explanatory, avoid unnecessary comments

✅ GOOD
```csharp
public class OrderManager 
{
  public void ProcessOrder(Order order) 
  {
    var items = order.GetItems();

    foreach (var item in items) 
    {
      var availability = item.GetAvailability();
    }
  }
}
```

❌ BAD
```csharp
public class OrderManager 
{
  public void ExecuteOrder(Order order) 
  {
    // Get all available items from the order
    var data = order.GetData();

    foreach (var x in data) 
    {
      // Determine the availability of the item
      var available = item.CheckAvailability();
    }
  }
}
```

### Don't use all uppercase letters for abbreviations

✅ GOOD
```csharp
public class JsonParser { }
```

❌ BAD
```csharp
public class JSONParser { }
```

### Use singular for class and enum names
_Purpose: `enum`s are always addressed individually. For example `EmploymentType.FullTime` is much cleaner than `EmploymentTypes.FullTime`. Furthermore, classes always represent a single instance of an object, for example an individual `User`._

✅ GOOD
```csharp
public enum EmploymentType
{
  FullTime,
  PartTime
}

public class User
{

}
```

❌ BAD
```csharp
public enum EmploymentTypes
{
  FullTime,
  PartTime
}

public class Users
{

}
```

---

## Properties

### Use the expression body definition for readonly properties

✅ GOOD
```csharp
public class User
{
  public string Username { get; }
  public int Age { get; }

  public string DisplayName => $"{Username} ({Age})";

  public User(string username, int age)
  {
    Username = username;
    Age = age;
  }
}
```

❌ BAD
```csharp
public class User
{
  public string Username { get; }
  public int Age { get; }

  public string DisplayName
  {
    get
    {
      return $"{Username} ({Age})";
    }
  }

  public User(string username, int age)
  {
    Username = username;
    Age = age;
  }
}
```

---

## Exceptions

### Catch exceptions as specific as possible

✅ GOOD
```csharp
try
{
  System.IO.File.Open(path);
}
catch (FileNotFoundException ex)
{
  // Specific
}
catch (IOException ex)
{
  // Specific
}
catch (Exception ex)
{
  // Default
}
```

❌ BAD
```csharp
try
{
  System.IO.File.Open(path);
}
catch (Exception ex)
{
  // SOMETHING went wrong
}
```

### Always use predefined (if applicable) or custom exceptions, never throw an `Exception` yourself

✅ GOOD
```csharp
public void ProcessOrder(Order order)
{
  if (order == null)
  {
    throw new ArgumentNullException(nameof(order));
  }
}
```

❌ BAD
```csharp
public void ProcessOrder(Order order)
{
  if (order == null)
  {
    throw new Exception("Order is null.");
  }
}
```

---

## Private members

### Use one underscore as prefix

✅ GOOD
```csharp
private string _username;
```

❌ BAD
```csharp
private string mUsername__;

// OR

private string username;

// OR

private string username_;
```

---

## Spacing

### Don't add unnecessary empty lines or whitespaces

✅ GOOD
```csharp
public class JsonParser
{
  private readonly string _json;

  public JsonParser(string json)
  {
    _json = json;
  }
}
```

❌ BAD
```csharp


public class JsonParser
{

  private  readonly string  _json;

  public JsonParser(string json)
  {


    _json =    json;
  }

}

```

---

## Immutability

### Use get-only (readonly) properties whenever possible

✅ GOOD
```csharp
public class JsonParser
{
  public string Json { get; }

  public JsonParser(string json)
  {
    Json = json;
  }
}
```

❌ BAD
```csharp
public class JsonParser
{
  public string Json { get; set; }

  public JsonParser(string json)
  {
    Json = json;
  }
}
```

### Use readonly collections whenever possible

✅ GOOD
```csharp
public class KeywordProvider
{
  public IReadOnlyCollection<string> Keywords { get; }

  public KeywordProvider()
  {
    Keywords = new ReadOnlyCollection<string>(new List<string>
    {
      "public",
      "string"
    });
  }
}
```

❌ BAD
```csharp
public class KeywordProvider
{
  public IList<string> Keywords { get; }

  public KeywordProvider()
  {
    Keywords = new List<string>
    {
      "public",
      "string"
    };
  }
}
```

---

## Usings

### Always use `using` blocks

✅ GOOD
```csharp
using (var connection = new SqlConnection(connectionString))
{

}
```

❌ BAD
```csharp
try
{
  var connection = new SqlConnection(connectionString);
}
finally
{
  connection.Close();
  connection.Dispose();
}
```

## Braces

### Always use braces

✅ GOOD
```csharp
if (user.IsElevated)
{
  // Do something
}
```

❌ BAD
```csharp
if (user.IsElevated)
  // Do something
```
