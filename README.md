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

Any text using the provided variable is html entitized.

For example:
```xml
<String Name="Test"><Span Name="my_span">David &amp; Sophia are <Parameter Name="Activity" /></Span></String>
```

"David & Sophia are " will be automatically entitized, however any content supplied using the `Span` and the `Parameter` will not be. If you are using `EntitizeByDefault` or the `Parameter` "Activity" is marked to auto-entitize, you will run into issues where the `Parameter` will be doubly encoded if your `Span`'s callback encoded the entire bit. Be careful.

## `Parameter`s do not auto `htmlentities` by default.
You should entitize all Parameters supplied to the localisation system. This is done to allow the developer freedom to handle entitization as needed. However, you can force the system entitize Parameters by default.

You can call this function to entitize all Parameters by default:
```php
$Localisation->SetParamEntitiesMode(bool $Mode = false);
```

Or, alternatively add `EntitizeByDefault` to the root of your Meta.xml file.
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

# Constructor Options
LanguageLocalisation takes the following constructor arguments:

```php
new LanguageLocalisation\LanguageLocalisation(string $Path, string $SetLanguage = null, bool $Debug = false);
```

## string $Path
- Required

The path to your localisation files. At the root of this directory, ensure you have a "Meta.xml" file ready to parse.

## string $SetLanguage = null
- Defaults to null

Used to assign locale by script instead of the class automatically picking the best locale for the browser. Helpful for things such as WebSockets or accounts setting their own perferred language.

## bool $Debug = false
- Defaults to false

This can be used for debugging. Alternatively, you can call `SetDebugMode`. However this has the pitfall of not allowing certain things at start up as debug mode has not yet been enabled.

```php
$Localisation->SetDebugMode(bool $Mode);
```

# Cookies
LanguageLocalisation uses cookies that can be set to adjust settings on a per browser basis.

## `LLSetting_Lang`
Can be used to set the language forcefully using a cookie (i.e. setting this value to "es" will result in the website returning Spanish text)

# Installation
TODO
