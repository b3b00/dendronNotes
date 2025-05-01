---
id: Perso.projects.CSLY.stack
title: Perso.projects.CSLY.stack
desc: stack parser
updated: 1746118764105
created: 0
---
# Write something really smart here.

```
 Non-Terminal<<0>> choices [0/4 : choices :  IDENTIFIER  ] @0
      <<0>> @0 :: choices : >IDENTIFIER(T) 
        Terminal IDENTIFIER @0
          OK IDENTIFIER [True] @line 0, column 0 on channel 0
      <<0>> @0 :: choices : IDENTIFIER(T)  DONE : OK
        OK
		
    Non-Terminal<<0>> choices [1/4 : choices :  STRING] @0
      end of token stream not reached, looking forward
        KO rule choices :  STRING does not match IDENTIFIER [True] @line 0, column 0 on channel 0
		
    Non-Terminal<<0>> choices [2/4 : choices :  IDENTIFIER OR choices ] @0
      end of token stream not reached, looking forward
      <<1>> @0 :: choices : >IDENTIFIER(T) OR(T) choices(NT) 
        Terminal IDENTIFIER @0
          OK IDENTIFIER [True] @line 0, column 0 on channel 0
      <<1>> @0 :: choices : IDENTIFIER(T) >OR(T) choices(NT) 
        Terminal OR @1
          OK OR [|] @line 0, column 4 on channel 0
      <<1>> @1 :: choices : IDENTIFIER(T) OR(T) >choices(NT) 
        Non-Terminal<<1>> choices [0/4 : choices :  IDENTIFIER  ] @2
          <<2>> @2 :: choices : >IDENTIFIER(T) 
            Terminal IDENTIFIER @2
              OK IDENTIFIER [False] @line 0, column 5 on channel 0
          <<2>> @2 :: choices : IDENTIFIER(T)  DONE : OK
            OK
        Non-Terminal<<1>> choices [1/4 : choices :  STRING] @2
          end of token stream not reached, looking forward <<<<< here is the issue (new pos should be 3 ???) NT choices [1] Id=1
		  
            KO rule choices :  STRING does not match IDENTIFIER [False] @line 0, column 5 on channel 0
        Non-Terminal<<1>> choices [2/4 : choices :  IDENTIFIER OR choices ] @2
          end of token stream not reached, looking forward 
		  
          <<3>> @2 :: choices : >IDENTIFIER(T) OR(T) choices(NT)
            Terminal IDENTIFIER @2
              OK IDENTIFIER [False] @line 0, column 5 on channel 0
          <<3>> @2 :: choices : IDENTIFIER(T) >OR(T) choices(NT)
            Terminal OR @3
              error found <<EOS>> expected OR
          <<3>> @3 :: choices : IDENTIFIER(T) OR(T) >choices(NT)
            KO unexpected end of stream. Expecting OR, .
        Non-Terminal<<1>> choices [3/4 : choices :  STRING OR choices ] @2
            KO rule choices :  STRING OR choices  does not match IDENTIFIER [False] @line 0, column 5 on channel 0
        Non-Terminal<<1>> choices [4/4 : ] @2
      <<1>> @2 :: choices : IDENTIFIER(T) OR(T) choices(NT)  DONE : KO
        KO unexpected end of stream. Expecting OR, .
    Non-Terminal<<0>> choices [3/4 : choices :  STRING OR choices ] @0
        KO rule choices :  STRING OR choices  does not match IDENTIFIER [True] @line 0, column 0 on channel 0
    Non-Terminal<<0>> choices [4/4 : ] @0

parse failed The checked value: [parse failed : unexpected end of stream. Expecting OR, .]



```


