
## Installing OCLint

There are multiple ways to install OClint. Instructions for each method are listed below:

### 1. HomeBrew Installation Method


1. Install Homebrew if needed 	
`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
2. Add the oclint tap to brew using the terminal  `$ brew tap oclint/formulae`
3. Install Oclint from the tap `$ brew install oclint`

### 2. Copy to /usr/local Method

1. 	Download OCLint (darwin for Mac OSX) -> https://github.com/oclint/oclint/releases
2. 	Move to downloaded OCLint folder via terminal 
3.    Check that lib and bin folders are present in /usr/local/ by running command 
`$ ls /usr/local/`
4.    If any of the folder dont exist create using the relevant command
`$ sudo mkdir /usr/local/bin`	
`$ sudo mkdir /usr/local/lib`
5. Run the following two commands while still in downloaded OCLint folder
`sudo cp bin/oclint* /usr/local/bin/`
`sudo cp -RP lib/* /usr/local/lib/`

### 3. Add to Path Method

1. Download OCLint (darwin for Mac OSX) -> https://github.com/oclint/oclint/releases
2. Add the following lines to .bashrc or any other terminal configuration files


`OCLINT_HOME=/Users/faisal/Downloads/oclint-0.11.1 
export PATH=$OCLINT_HOME/bin:$PATH`


NOTE: This methods requires the user to not delete the OCLint download folder as it is now added to path

Open another instance of terminal and run the following command to check if OCLint is installed correctly for the user. 

$ oclint

you should see the following message if installed correctly.
**oclint: Not enough positional command line arguments specified!**	
Must specify at least 1 positional arguments: See: oclint -help



## INSTALLING XCPretty

XCPretty is a formatter for the xcodebuild output. Run the following command in the terminal to install XCPretty.
`$ gem install xcpretty`



## XCODE INTEGRATION

1. Create an aggregate target that will be used to execute the OCLint command from Xcode.
2. Add new run script phase in build phases.
3. Add the relevant script that suits your project configurations.
- No workspace, single target 
`cd ${SRCROOT}`
```
xcodebuild ONLY_ACTIVE_ARCH=NO -project ${PROJECT_NAME}.xcodeproj -target target1 -configuration Debug clean build | xcpretty -r json-compilation-database --output compile_commands.json
oclint-json-compilation-database -- -report-type xcode
```
- No workspace, multiple target
`cd ${SRCROOT}`
```
xcodebuild ONLY_ACTIVE_ARCH=NO -project ${PROJECT_NAME}.xcodeproj -target target1 -target target2 -configuration Debug clean build | xcpretty -r json-compilation-database --output compile_commands.json
oclint-json-compilation-database -- -report-type xcode
```
- Workspace
`cd ${SRCROOT}`
```
xcodebuild ONLY_ACTIVE_ARCH=NO -workspace ${PROJECT_NAME}.xcworkspace -scheme ${PROJECT_NAME} -configuration Debug clean build | xcpretty -r json-compilation-database --output compile_commands.json
oclint-json-compilation-database -e Pods -- -report-type xcode
```
4. Switch to the OCLint target and build.

NOTE: if your project contains too many warnings, building of OCLint target will be shown as ending in error.


A sample project where each commit caters to a specific project configuration can be found at https://github.com/ecspress/OCLintTest.