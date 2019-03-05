# Practical C#

A collection of mostly reasonable guidelines and best practices to write clean, readable, understandable and maintainable C# code.

# Coding Conventions

## Types

### Use `var` whenever appropriate and possible

✅ GOOD
```C#
var httpClient = new HttpClient();
```

❌ BAD
```C#
HttpClient httpClient = new HttpClient();
```

---

## Instantiation

### Use object initializers over direct property assignment

✅ GOOD
```C#
var user = new User
{
  Username = "admin",
  Age = 31
};
```

❌ BAD
```C#
var user = new User();
user.Username = "admin";
user.Age = 31;
```

---

## String literals

### Use string interpolation instead of `string.Format`

✅ GOOD
```C#
var url = "http://localhost/api";
var resource = "users";
var path = $"{url}/{resource}";
```

❌ BAD
```C#
var url = "http://localhost/api";
var resource = "users";
var path = string.Format("{0}/{1}", url, resource);
```

### Use constants when using string literals more than once

✅ GOOD
```C#
const string error = "user_not_found";

Log.Error(error);
return BadRequest(error);
```

❌ BAD
```C#
Log.Error("user_not_found");
return BadRequest("user_not_found");
```

---

## Naming

### Write code where the purpose and the member type can be inferred from the semantics

✅ GOOD
```C#
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
```C#
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
```C#
// Clearly an interface
public interface IOrderManager { }

// Clearly a list of orders
private IList<Order> orders;
```

❌ BAD
```C#
// Clearly an interface => no "Interface" postfix necessary
public interface IOrderManagerInterface { }

// Clearly a list of oders => no "List" postfix necessary
private IList<Order> orderList;
```

### Write code that is self-explanatory, avoid unnecessary comments

✅ GOOD
```C#
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
```C#
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
```C#
public class JsonParser { }
```

❌ BAD
```C#
public class JSONParser { }
```

---

## Properties

### Use the expression body definition for readonly properties

✅ GOOD
```C#
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
```C#
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
```C#
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
```C#
try
{
  System.IO.File.Open(path);
}
catch (Exception ex)
{
  // SOMETHING went wrong
}
```

### Always use custom (or predefined) exceptions, never throw an `Exception` yourself

✅ GOOD
```C#
try
{
  var result = orderManager.ProcessOrder();
}
catch (Exception ex)
{
  // Throw custom exception => pass on exception
  throw new OrderManagerException("Could not process order.", ex);
}
```

❌ BAD
```C#
try
{
  var result = orderManager.ProcessOrder();
}
catch (Exception ex)
{
  throw new Exception("Could not process order.");
}
```

---

## Private members

### Use one underscore as prefix

✅ GOOD
```C#
private string _username;
```

❌ BAD
```C#
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
```C#
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
```C#


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
```C#
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
```C#
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
```C#
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
```C#
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
```C#
using (var connection = new SqlConnection(connectionString))
{

}
```

❌ BAD
```C#
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
```C#
if (user.IsElevated)
{
  // Do something
}
```

❌ BAD
```C#
if (user.IsElevated)
  // Do something
```
