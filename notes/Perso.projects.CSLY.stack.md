---
id: Perso.projects.CSLY.stack
title: Perso.projects.CSLY.stack
desc: new note
updated: 0
created: 0
---
# Write something really smart here.

```
C:\Users\olduh\AppData\Local\Programs\Rider\plugins\dpa\DotFiles\JetBrains.DPA.Runner.exe --handle=29036 --backend-pid=12776 --etw-collect-flags=67108622 --detach-event-name=dpa.detach.12776.131 --refresh-interval=1 -- "C:\Program Files\dotnet\dotnet.exe" --fx-version 8.0.15 C:\Users\olduh\AppData\Local\Programs\Rider\lib\ReSharperHost\JetLauncherILc.exe /Launcher::NoSplash /Launcher::Mode:InvokeMethod /Launcher::Target:Assembly /Launcher::AssemblyFile:C:/Users/olduh/dev/csly/src/samples/ParserExample/bin/Debug/net8.0/ParserExample.dll /Launcher::ClassName:ParserExample.Stacker /Launcher::MethodName:List
        A B 
    Non-Terminal<<0>> cs [0] @0
      <<0>> @0 :: cs : >c(NT) cs(NT) 
        Non-Terminal<<1>> c [0] @0
          <<1>> @0 :: c : >A(T) 
            Terminal A @0
              OK A [A] @line 0, column 0 on channel 0
          <<1>> @0 :: c : A(T)  DONE : OK
            OK
        Non-Terminal<<1>> c [1] @0
      <<0>> @0 :: cs : c(NT) >cs(NT) 
        Non-Terminal<<2>> cs [0] @1
          <<2>> @1 :: cs : >c(NT) cs(NT) 
            Non-Terminal<<3>> c [0] @1
                KO rule c : A does not match B [B] @line 0, column 3 on channel 0
            Non-Terminal<<3>> c [1] @1
              <<3>> @1 :: c : >B(T) 
                Terminal B @1
                  OK B [B] @line 0, column 3 on channel 0
              <<3>> @1 :: c : B(T)  DONE : OK
                OK
            Non-Terminal<<3>> c [2] @1
          <<2>> @1 :: cs : c(NT) >cs(NT)
            Non-Terminal<<4>> cs [0] @2
                KO rule cs : c cs does not match <<EOS>>
            Non-Terminal<<4>> cs [1] @2
                KO rule cs : c does not match <<EOS>>
            Non-Terminal<<4>> cs [2] @2
          <<2>> @2 :: cs : c(NT) cs(NT)  DONE : KO
            KO unexpected end of stream. Expecting A, B, .
        Non-Terminal<<2>> cs [1] @1
          <<4>> @1 :: cs : >c(NT)
            Non-Terminal<<5>> c [0] @1
                KO rule c : A does not match B [B] @line 0, column 3 on channel 0 <<<- ici on ajoute une erreur en index 0
            Non-Terminal<<5>> c [1] @1
              <<5>> @1 :: c : >B(T)
                Terminal B @1
                  OK B [B] @line 0, column 3 on channel 0 <<<- ici on ajoute un OK en index 1 (alors que ca devrait être 0
              <<5>> @1 :: c : B(T)  DONE : OK
                OK
            Non-Terminal<<5>> c [2] @1
          <<4>> @1 :: cs : c(NT)  DONE : OK
            OK
        Non-Terminal<<2>> cs [2] @1
      <<0>> @1 :: cs : c(NT) cs(NT)  DONE : OK
        OK
    Non-Terminal<<0>> cs [1] @0
```

When adding a child to a rule (from T or NT) it should be an insert @rule.Index
