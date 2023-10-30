---
id: e9l8togkv1n4q2sxdai6zzt
title: embedGitHash
desc: 'How to embed git hash in a .net assembly.'
updated: 1698684998605
created: 1691651184259
---

How to embed git commit hash in a .net assembly

# set the hash in the csproj

```xml
<Target Name="SetSourceRevisionId" BeforeTargets="InitializeSourceControlInformation">
    <Exec
      Command="git rev-parse HEAD"
      ConsoleToMSBuild="True"
      IgnoreExitCode="False"
    >
      <Output PropertyName="SourceRevisionId" TaskParameter="ConsoleOutput"/>
    </Exec>
  </Target>
```

This will concat the hash to the `<Version>` property (separated by a '+')

# how to get the hash

```c#

var version = ""; 
            
var appAssembly = this.GetType().Assembly;
var infoVerAttr = (AssemblyInformationalVersionAttribute)appAssembly
    .GetCustomAttributes(typeof(AssemblyInformationalVersionAttribute)).FirstOrDefault();

if (infoVerAttr != null && infoVerAttr.InformationalVersion.Length > 6)
{
    // something like "1.0+a34a913742f8845d3da5309b7b17242222d41a21";
    version = infoVerAttr.InformationalVersion;
}

// get git hash
var gitHash = version.Substring( version.IndexOf('+') + 1);
// get version 
version = version.Substring(0, version.IndexOf('+'));

```