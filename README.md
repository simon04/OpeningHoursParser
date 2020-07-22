[![Build Status](https://travis-ci.org/simonpoole/OpeningHoursParser.svg?branch=master)](https://travis-ci.org/simonpoole/OpeningHoursParser)

# OpeningHoursParser

This is a very simplistic parser for string values according to the [OSM opening hours specification][opening-hours-specification].

It parses 146'825 (91%) of 161'268 unique test strings in non-strict mode. The remaining 14'443 are likely valid errors, spot checking shows that they have obvious issues. In strict mode further 22'440 fail (total 36'883).

Deviations from the grammar as of [this version of the opening hours specification][opening-hours-grammar-specification] in all modes:

 * case-insensitive
 * leading 0s in times optional
 * unicode EN DASH (U+2013) character is allowed for hyphen
 * various unicode whitespace characters are ignored

In non-strict mode the following further differences are allowed:

 * three-character weekday abbreviations
 * German two-letter weekday abbreviations
 * times extending in to the next day that are missing the extra 24 hours are corrected
 * single 0 for minutes
 * minutes in times optional
 * "." and "h" as minutes separators
 * AM and PM time specifications are allowed (plus A.M. and P.M.) 
 * holidays in weekday range
 * 24/7 rules with preceding selectors are corrected to 00:00-24:00 time spans
 * list of month days after months ( Jan 1,4,5 )
 * " to " in lieu of a hyphen for ranges
 * date ranges that do not have the month day specified are corrected (Jan - Feb 15 -> Jan 1 - Feb 15)

Converting the data structures generated by parsing back to strings will result in correct data according to the specification.

## Usage

``` java
try {
	OpeningHoursParser parser = new OpeningHoursParser(
		new ByteArrayInputStream(line.getBytes()));
	List<Rule> rules = parser.rules(strict);
	// ...
} catch(ParseException e) {
	// ...
}
```

Detailed documentation can be found in the [JavaDoc](http://www.javadoc.io/doc/ch.poole/OpeningHoursParser/0.22.0).


## Including in your project

We publish releases to [Bintray/JCenter](https://bintray.com/simonpoole/osm/OpeningHoursParser).
The following snippets for `build.gradle` will make OpeningHoursParser available in your Gradle project:

``` groovy
repositories {
    jcenter()
}
```

``` groovy
dependencies {
    compile "ch.poole:OpeningHoursParser:0.22.0"
}
```


[opening-hours-specification]: http://wiki.openstreetmap.org/wiki/Key:opening_hours/specification
[opening-hours-grammar-specification]: http://wiki.openstreetmap.org/w/index.php?title=Key:opening_hours/specification&oldid=1075290

## Tests

Besides some unit tests, we run the parser on ~160'000 and compare with previous output to detect any changes in behaviour. If you are running these on windows, you may need to add
``org.gradle.jvmargs=-Dfile.encoding=UTF-8``
to your gradle.properties to force correct use of the UTF-8 encoded test input. 

## Building

The project uses [gradle](https://gradle.org/) for building. Standard gradle tasks for the java plugin can be found here [https://docs.gradle.org/current/userguide/java_plugin.html](https://docs.gradle.org/current/userguide/java_plugin.html). They can be invoked on the command line by running ``gradlew`` or ``gradlew.bat`` with the name of the task, for example
``gradlew jar`` to create the jar archive. 

Note: the project has no runtime dependencies, and the jar file resulting from the build process is self-sufficient.

## Contributing

Pull requests are welcome. 

As this library is used in a number of Android projects please restrict the use of Java 8 features and APIs to the Android supported subset see https://developer.android.com/studio/write/java8-support. Check that any APIs used are supported on all Android platform API versions 10 and later, this is mainly an issue for APIs that were introduced with Java 7. This restriction will likely be relaxed for future versions. 

Currently, the library is self-contained and doesn't have any runtime dependencies outside of basic Java support, it would be nice if we can keep it like that.

## Translation

The error messages produced by the library are translated on [transifex](https://transifex.com/), see [https://www.transifex.com/openstreetmap/openinghoursparser/](https://www.transifex.com/openstreetmap/openinghoursparser/).
