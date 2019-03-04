# Coding Guidelines

A mostly reasonable approach to write clean, readable, understandable and maintainable C# code.

## Implicit types

### Use `var` whenever appropriate and possible

✅ DO
```C#
var httpClient = new HttpClient();
```

❌ DON'T
```C#
HttpClient httpClient = new HttpClient();
```

---

## String literals

### Use string interpolation instead of `string.Format`

✅ DO
```C#
var url = "http://localhost/api";
var resource = "users";
var path = $"{url}/{resource}";
```

❌ DON'T
```C#
var url = "http://localhost/api";
var resource = "users";
var path = string.Format("{0}/{1}", url, resource);
```

### Use constants when using string literals more than once

✅ DO
```C#
const string error = "user_not_found";

Log.Error(error);
return BadRequest(error);
```

❌ DON'T
```C#
Log.Error("user_not_found");
return BadRequest("user_not_found");
```

---

## Naming

### Don't use all uppercase letters for abbreviations

✅ DO
```C#
public class JsonParser { }
```

❌ DON'T
```C#
public class JSONParser { }
```

---

## Properties

### Use the expression body definition for readonly properties

✅ DO
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

❌ DON'T
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

## Instantiation

### Use object initializers over direct property assignment

✅ DO
```C#
var user = new User
{
  Username = "admin",
  Age = 31
};
```

❌ DON'T
```C#
var user = new User();
user.Username = "admin";
user.Age = 31;
```

---

## Private members

### Use one underscore as prefix

✅ DO
```C#
private string _username;
```

❌ DON'T
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

✅ DO
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

❌ DON'T
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

---

## Immutability

### Use get-only (readonly) properties whenever possible

✅ DO
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

❌ DON'T
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

✅ DO
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

❌ DON'T
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

✅ DO
```C#
using (var connection = new SqlConnection(connectionString))
{

}
```

❌ DON'T
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

✅ DO
```C#
if (user.IsElevated)
{
  // Do something
}
```

❌ DON'T
```C#
if (user.IsElevated)
  // Do something
```
