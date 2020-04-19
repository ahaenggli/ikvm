# Windward IKVM

## Build

See HOWTO for the build instructions.  The quick start is

- Install NAnt (https://sourceforge.net/projects/nant/files/nant/0.92/).

**A note on using NAnt**.  If you encounter a System.Security.SecurityException
while running nant, apply the following fix.

From [stackoverflow](https://stackoverflow.com/questions/8605122/how-do-i-resolve-configuration-errors-with-nant-0-91)

> I found that the problem was Windows security related in that the downloaded
> NAnt zip file needed additional security related configuration to be performed:
> before extracting, one must right click on the zip file, select Properties and
> under the General tab, click the button labelled Unblock, the click OK on the
> Properties window.  Now, extract the file to your desired location.

- Must be set to use JDK 8. (https://www.oracle.com/java/technologies/javase-jdk8-downloads.html)  
- Set environment variable `JDK1.8_HOME` to your JDK8 installation.

- Download ICSharpCode.SharpZipLib.dll (https://github.com/icsharpcode/SharpZipLib/releases) and copy to the bin folder. (the nupkg package contains the dll) 

- Download openjdk-8u45-b14 (http://www.frijters.net/openjdk-8u45-b14-stripped.zip) and unpack in the peer folder to ikvm. (same level as your ikvm folder - not inside!) 

- In the root folder run nant.

```nant```

Produces a release build.

```nant -D:debug=true```

Produces a debug build with all. pdb files in the bin folder.

- To produce the strongly named assemblies

Add the ikvm-key.snk to the key container

```sn.exe -i ikvm-key.snk ikvm-key```

And run nant

```nant signed```

## Package NuGet
- build a signed version of the project
- from the nuget directory run these commands
- nant IKVM.nuspec
- nant nupkg
- To publish set your nuget Api key:
````
nuget.exe setApiKey <api key here>
````
- then use nuget push to push nug nuget gallery:
````
nuget.exe push IKVM.WINDWARD.8.5.0.2.nupkg -Source https://api.nuget.org/v3/index.json
````

## Errors 
If a newer JDK version is installed (maybe parallel?) the following errors may occur
- [exec] error: option --boot-class-path not allowed with target 14  
- External Program Failed: rmic (return code was 1)  

solution: `javac` and `rmic` have to be called within JDK 8 (fixed with commit 1c3bad9 )


