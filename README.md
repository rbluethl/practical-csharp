# Coding Guidelines

A mostly reasonable approach to write clean, readable, understandable and maintainable C# code.

## Implicit types

### Use `var` whenever appropriate and possible

❌ DON'T
```C#
HttpClient httpClient = new HttpClient();
```

✅ DO
```C#
var httpClient = new HttpClient();
```

---

## String literals

### Use string interpolation instead of `string.Format`

❌ DON'T
```C#
var url = "http://localhost/api";
var resource = "users";
var path = string.Format("{0}/{1}", url, resource);
```

✅ DO
```C#
var url = "http://localhost/api";
var resource = "users";
var path = $"{url}/{path}";
```

---

## Naming

### Don't use all uppercase letters for abbreviations

❌ DON'T
```C#
public class JSONParser { }
```

✅ DO
```C#
public class JsonParser { }
```

---

## Instantiation

### Use object initializers over direct property assignment

❌ DON'T
```C#
var user = new User();
user.Username = "admin";
user.Age = 31;
```

✅ DO
```C#
var user = new User
{
  Username = "admin",
  Age = 31
};
```

---

## Private members

### Use one underscore as prefix

❌ DON'T
```C#
private string mUsername__;

// OR

private string username;

// OR

private string username_;
```

✅ DO
```C#
private string _username;
```

---

## Spacing

### Don't add unnecessary empty lines or whitespaces

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

---

## Immutability

### Consider immutability where applicable and reasonable (for example get-only properties)

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

---

## Usings

### Always use `using` blocks

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

✅ DO
```C#
using (var connection = new SqlConnection(connectionString))
{

}
```
