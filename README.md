# Java lib export

## Motivation

Writing software is only half the task. Genearted artifacts often require libraries in certain versions. Programs have to be executed by some kind of script or executable. These scripts often also hold configuration parameters and so on. This project shows an example of a Java application, build with Gradle. It offers certain Gradle tasks to export the dependencies, the generated jar file and also generates the scripts to run the application (bat/cmd, PowerShell and Bash).

## Usage

The only task required to start it all is

Powershell full command:
```powershell
Start-Process -FilePath "cmd" -ArgumentList @("/c", "gradlew.bat", "writeExecScripts") -Wait -NoNewWindow
```

Powershell simple command:
```powershell
.\gradlew.bat writeExecScripts
```

Batch/CMD:
```dos
gradlew.bat writeExecScripts
```

Bash:
```bash
./gradlew writeExecScripts
```

Because of task dependencies the tasks start in the correct order.

```
> Task :clean
> Task :compileJava
> Task :processResources
> Task :classes
> Task :exportDependencies
> Task :jar
> Task :writeExecScripts
```

## Output

The commands above create a few things in the build folder.

```
build
├─executables
| │─javalibexport.bat
| │─javalibexport.ps1
| │─javalibexport.sh
├─export
  │─gson-2.8.6.jar
  │─javalibexport-0.0.1.jar
  │─logback-classic-1.2.3.jar
  │─logback-core-1.2.3.jar
  │─slf4j-api-1.7.30.jar
```

The required libraries are exported to the folder "export" and the executable scripts are stored in the folder "executables". The executables are designed to change into the correct directory when executed. This way a shortcut can be placed anywhere on the system.

## How it works

The executing command basically looks like this:

```
java -cp "..\export\*" javalibexport.JavaLibExport
```

Considering that the script changes into the folder with the executables when executed, the classpath (Parameter "-cp") is correctly set by switching to the directory one level higher ("../") and then into the folder "export". The last part is the full qualified name of the package and class containing the main method.