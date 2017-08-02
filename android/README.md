# Static Code Analysis Guide For Android - Android Studio

Android Lint is a new tool introduced in ADT 16 (and Tools 16) which scans Android project sources for potential bugs. It is available both as a command line tool, as well as integrated with Eclipse (described below), and IntelliJ (details). The architecture is deliberately IDE independent so it will hopefully be integrated with other IDEs, with other build tools and with continuous integration systems as well.


Here are some examples of the types of errors that it looks for:

- Missing translations (and unused translations)
- Layout performance problems (all the issues the old layoutopt tool used to find, and more)
- Unused resources
- Inconsistent array sizes (when arrays are defined in multiple configurations)
- Accessibility and internationalization problems (hardcoded strings, missing contentDescription, etc)
- Icon problems (like missing densities, duplicate icons, wrong sizes, etc)
- Usability problems (like not specifying an input type on a text field)
- Manifest errors

### How to import lint.xml in Android Studio

1. File -> Settings
2. Editor -> Inspection 
3. In "Manage" dropdown -> Import
4. Select "10PLint.xml" file 
5. Select "Apply"

#### How to run Lint?

If your project includes build variants, you can use the Gradle wrapper to invoke the lint task for all your variants by entering one of the following commands from the root directory of your project:

#### On Windows:

gradlew lint

#### On Linux or Mac:
./gradlew lint

If you instead want to run the lint task for only a specific build variant, you must capitalize the variant name and prefix it with lint.

gradlew lintDebug

#### How to run Lint from GUI?

1. Analyze -> Inspect Code
2. From "Inspection scope" select "Whole project"
3. From "Inspection profile" select "10pLint"
4. Select "ok"

Note: Only TL should update the rules as per requirement.
