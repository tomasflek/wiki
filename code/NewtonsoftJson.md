# Json.NET

https://www.newtonsoft.com/json

## Serialization / Deserialization of enum as string

``` csharp
enum Gender { Male, Female }

class Person
{
    int Age { get; set; }
    Gender Gender { get; set; }
}
```

``` csharp
[JsonConverter(typeof(StringEnumConverter))]
public Gender Gender { get; set; }
```

## Property name attribute

``` csharp
public class Videogame
{
    [JsonProperty("name")]
    public string Name { get; set; }

    [JsonProperty("release_date")]
    public DateTime ReleaseDate { get; set; }
}
```