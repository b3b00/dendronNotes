---
id: Perso.projects.CSLY.stack
title: Perso.projects.CSLY.stack
desc: stack parser
updated: 1746194348385
created: 0
---
# Write something really smart here.

```
  start :: [ PLUS | MINUS ] 
    Non-Terminal<<0>> choiceclause [0/1 : choiceclause :  LCROG  choices RCROG  ] @0
      <<0>>[0] @0 :: choiceclause : >LCROG(T) choices(NT) RCROG(T) 
          OK LCROG [[] @line 0, column 0 on channel 0
      <<0>>[1] @0 :: choiceclause : LCROG(T) >choices(NT) RCROG(T) 
        Non-Terminal<<1>> choices [0/4 : choices :  IDENTIFIER  ] @1
          <<1>>[0] @1 :: choices : >IDENTIFIER(T) 
              OK IDENTIFIER [PLUS] @line 0, column 2 on channel 0
          <<1>>[1] @1 :: choices : IDENTIFIER(T)  DONE : OK
            OK
        Non-Terminal<<1>> choices [1/4 : choices :  STRING] @1
            KO rule (( choices :  STRING )) does not match IDENTIFIER [PLUS] @line 0, column 2 on channel 0
        Non-Terminal<<1>> choices [2/4 : choices :  IDENTIFIER OR choices ] @1
          <<2>>[0] @1 :: choices : >IDENTIFIER(T) OR(T) choices(NT) 
              OK IDENTIFIER [PLUS] @line 0, column 2 on channel 0
          <<2>>[1] @1 :: choices : IDENTIFIER(T) >OR(T) choices(NT) 
              OK OR [|] @line 0, column 7 on channel 0
          <<2>>[2] @2 :: choices : IDENTIFIER(T) OR(T) >choices(NT) 
            Non-Terminal<<2>> choices [0/4 : choices :  IDENTIFIER  ] @3
              <<3>>[0] @3 :: choices : >IDENTIFIER(T) 
                  OK IDENTIFIER [MINUS] @line 0, column 9 on channel 0
              <<3>>[1] @3 :: choices : IDENTIFIER(T)  DONE : OK
                OK
            Non-Terminal<<2>> choices [1/4 : choices :  STRING] @3
                KO rule (( choices :  STRING )) does not match IDENTIFIER [MINUS] @line 0, column 9 on channel 0
            Non-Terminal<<2>> choices [2/4 : choices :  IDENTIFIER OR choices ] @3
              <<4>>[0] @3 :: choices : >IDENTIFIER(T) OR(T) choices(NT) 
                  OK IDENTIFIER [MINUS] @line 0, column 9 on channel 0
              <<4>>[1] @3 :: choices : IDENTIFIER(T) >OR(T) choices(NT) 
                  error found RCROG []] @line 0, column 15 on channel 0 expected OR
              <<4>>[2] @4 :: choices : IDENTIFIER(T) OR(T) >choices(NT) 
            Non-Terminal<<2>> choices [3/4 : choices :  STRING OR choices ] @3
                KO rule (( choices :  STRING OR choices  )) does not match IDENTIFIER [MINUS] @line 0, column 9 on channel 0
            Non-Terminal<<2>> choices [4/4 : ] @3
          <<2>>[3] @3 :: choices : IDENTIFIER(T) OR(T) choices(NT)  DONE : KO
        Non-Terminal<<1>> choices [3/4 : choices :  STRING OR choices ] @1
            KO rule (( choices :  STRING OR choices  )) does not match IDENTIFIER [PLUS] @line 0, column 2 on channel 0
        Non-Terminal<<1>> choices [4/4 : ] @1
      <<0>>[2] @1 :: choiceclause : LCROG(T) choices(NT) >RCROG(T) 
    Non-Terminal<<0>> choiceclause [1/1 : ] @0
  start :: [ PLUS | MINUS ] 
    Non-Terminal<<0>> choiceclause [0/1 : choiceclause :  LCROG  choices RCROG  ] @0
      <<0>>[0] @0 :: choiceclause : >LCROG(T) choices(NT) RCROG(T) 
          OK LCROG [[] @line 0, column 0 on channel 0
      <<0>>[1] @0 :: choiceclause : LCROG(T) >choices(NT) RCROG(T) 
        Non-Terminal<<1>> choices [0/4 : choices :  IDENTIFIER  ] @1
          <<1>>[0] @1 :: choices : >IDENTIFIER(T) 
              OK IDENTIFIER [PLUS] @line 0, column 2 on channel 0
          <<1>>[1] @1 :: choices : IDENTIFIER(T)  DONE : OK
            OK
        Non-Terminal<<1>> choices [1/4 : choices :  STRING] @1
            KO rule (( choices :  STRING )) does not match IDENTIFIER [PLUS] @line 0, column 2 on channel 0
        Non-Terminal<<1>> choices [2/4 : choices :  IDENTIFIER OR choices ] @1
  start :: [ PLUS | MINUS ] 
    Non-Terminal<<0>> choiceclause [0/1 : choiceclause :  LCROG  choices RCROG  ] @0
      <<0>>[0] @0 :: choiceclause : >LCROG(T) choices(NT) RCROG(T) 
          OK LCROG [[] @line 0, column 0 on channel 0
      <<0>>[1] @0 :: choiceclause : LCROG(T) >choices(NT) RCROG(T) 
        Non-Terminal<<1>> choices [0/4 : choices :  IDENTIFIER  ] @1
          <<1>>[0] @1 :: choices : >IDENTIFIER(T) 
              OK IDENTIFIER [PLUS] @line 0, column 2 on channel 0
          <<1>>[1] @1 :: choices : IDENTIFIER(T)  DONE : OK
            OK
        Non-Terminal<<1>> choices [1/4 : choices :  STRING] @1
            KO rule (( choices :  STRING )) does not match IDENTIFIER [PLUS] @line 0, column 2 on channel 0
        Non-Terminal<<1>> choices [2/4 : choices :  IDENTIFIER OR choices ] @1
          <<2>>[0] @1 :: choices : >IDENTIFIER(T) OR(T) choices(NT) 
              OK IDENTIFIER [PLUS] @line 0, column 2 on channel 0
          <<2>>[1] @1 :: choices : IDENTIFIER(T) >OR(T) choices(NT) 
              OK OR [|] @line 0, column 7 on channel 0
          <<2>>[2] @2 :: choices : IDENTIFIER(T) OR(T) >choices(NT) 
            Non-Terminal<<2>> choices [0/4 : choices :  IDENTIFIER  ] @3
              <<3>>[0] @3 :: choices : >IDENTIFIER(T) 
                  OK IDENTIFIER [MINUS] @line 0, column 9 on channel 0
              <<3>>[1] @3 :: choices : IDENTIFIER(T)  DONE : OK
                OK
            Non-Terminal<<2>> choices [1/4 : choices :  STRING] @3
                KO rule (( choices :  STRING )) does not match IDENTIFIER [MINUS] @line 0, column 9 on channel 0
            Non-Terminal<<2>> choices [2/4 : choices :  IDENTIFIER OR choices ] @3
              <<4>>[0] @3 :: choices : >IDENTIFIER(T) OR(T) choices(NT) 
                  OK IDENTIFIER [MINUS] @line 0, column 9 on channel 0
              <<4>>[1] @3 :: choices : IDENTIFIER(T) >OR(T) choices(NT) 
                  error found RCROG []] @line 0, column 15 on channel 0 expected OR
              <<4>>[2] @4 :: choices : IDENTIFIER(T) OR(T) >choices(NT) 
            Non-Terminal<<2>> choices [3/4 : choices :  STRING OR choices ] @3
                KO rule (( choices :  STRING OR choices  )) does not match IDENTIFIER [MINUS] @line 0, column 9 on channel 0
            Non-Terminal<<2>> choices [4/4 : ] @3
          <<2>>[3] @3 :: choices : IDENTIFIER(T) OR(T) choices(NT)  DONE : KO
        Non-Terminal<<1>> choices [3/4 : choices :  STRING OR choices ] @1
            KO rule (( choices :  STRING OR choices  )) does not match IDENTIFIER [PLUS] @line 0, column 2 on channel 0
        Non-Terminal<<1>> choices [4/4 : ] @1
      <<0>>[2] @1 :: choiceclause : LCROG(T) choices(NT) >RCROG(T) 
    Non-Terminal<<0>> choiceclause [1/1 : ] @0




```


