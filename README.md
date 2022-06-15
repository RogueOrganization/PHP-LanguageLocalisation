# LanguageLocalisation
Created for internal use at Donut Team and Rogue Crab, we decided it would benefit the greater development community to have a central way of handling language localisation

```xml
<?xml version="1.0"?>
<Localisation>
  <String Name="my_string">Hello world!</String>
  <String Name="my_string_with_params">Hello <Parameter Name="Person" /></String>
  <String Name="my_string_with_callbacks">Hello <Span Name="Link"><Parameter Name="Person" /></Span></String>
  <List Name="my_string_list">
    <String>Hello world!</String>
    <String>This is an example!</String>
    <String>Parameters and spans work here too!</String>
  </List>
</Localisation>
```

```php
$Localisation = new \LanguageLocalisation\LanguageLocalisation("/path/to/localisation/files/");

// echos "Hello world!"
echo $Localisation->read("my_string");

// echos "Hello Dave"
echo $Localisation->read("my_string_with_params", [
  "Person" => "Dave"
]);

// echos "Hello <a href="https://google.com">Dave</a>"

echo $Localisation->read(
  "my_string_with_callbacks",
  [
    "Person" => "Dave"
  ],
  
  // Pitfall: Make sure to htmlentitize the callback of $e here, as the language localisation
  // system will not handle anything that isn't directly included to it.
  [
    "Link" => (fn($e) => "<a href=\"https://google.com\">" . htmlentities($e) . "</a>")
  ]
);


// echos "Hello world! This is an example! Parameters and spans work here too!"
echo $Localisation->readListAsString("my_string_list");

foreach ($Localisation->readList("my_string") as $String)
  echo $String . "<br />"; // iterates through the List
```

# Installation
TODO
