# Lists
Lists can be used to group similar strings together. It is helpful when more information is available in one language, but you don't want to alter the design of your application.

## XML
```xml
<List Name="my_list">
  <String>My string</String>
  <String>My second string: Hi <Parameter Name="Person" />!</String>
  <String>My third string</String>
</List>
```

Unlike normal `String` elements, `String`s within a list do not require a `Name` attribute. You can however set one to be accessed in your code.

## PHP
### readList(string $Name, ?array $Parameters = null, ?array $Callbacks = null)
You can iterate through a list using the `readList` function.
```php
foreach($Loc->readList("my_list", ["Person" => "Sophia"]) as $String)
  echo "<p>" . $String . "</p>\n";
```

#### Result
```html
<p>My string</p>
<p>My second string: Hi Sophia!</p>
<p>My third string</p>
```

### readListAsObj(string $Name)

```php
foreach ($Loc->readListAsObj("my_list") as $String)
{
   $Name = $String->GetAttribute("Name"); // returns the Name attribute, or null if undefined.
   $Value = $String->ToString(?array $Parameters = null, ?array $Callbacks = null);
   
   // Do something with the $Name and $Value
}
```

### readListAsString(string $Name, ?array $Parameters = null, ?array $Callbacks = null, string $Delimiter = " ")
```php
$MyStringList = $Loc->readListAsString("my_list", ["Person" => "Chris"], null, " | ");
echo $MyStringList;
```

#### Result
```html
My first string | My second string: Hi Chris! | My third string
```
