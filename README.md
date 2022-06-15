# LanguageLocalisation
Created for internal use at Donut Team and Rogue Crab, we decided it would benefit the greater development community to have a central way of handling language localisation

```xml
<?xml version="1.0"?>
<Localisation>
  <String Name="my_string">Hello world!</String>
  <String Name="my_string_with_params">Hello <Parameter Name="Person" Entitize="true" /></String>
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
  [
    "Link" => (fn($e) => "<a href=\"https://google.com\">" . htmlentities($e) . "</a>")
  ]
);


// echos "Hello world! This is an example! Parameters and spans work here too!"
echo $Localisation->readListAsString("my_string_list");

foreach ($Localisation->readList("my_string") as $String)
  echo $String . "<br />"; // iterates through the List
```

# Pitfalls
There are a few notes to keep in mind when using this Localisation system. 

## `Span`s do not auto `htmlentities` ever.
Period. You will need to handle all `htmlentities` on any content the developer provides through the use of a `Span` element. A `Span`'s primary incentive is for the developer to wrap links or text stylisation around the text provided from the localisation system.

## `Parameter`s do not auto `htmlentities` by default.
You should entitize all Parameters supplied to the localisation system. This is done to allow the developer freedom to handle entitization as needed. However, you can force the system entitize Parameters by default.

Add `EntitizeByDefault` to the root of your Meta.xml file.
```xml
<?xml version="1.0"?>
<Translations DefaultLanguage="en" EntitizeByDefault="true">
	<Language Id="en" LanguageString="language_english" File="English" />
</Translations>
```

You can also add this on a per `Parameter` basis:
```xml
  <!-- Parameter "Person" will be entitized. -->
  <String Name="Test">Hello <Parameter Name="Person" Entitize="true" />!</String>
```

# Installation
TODO
