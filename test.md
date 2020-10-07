## Benchmarks Result

| client | vc    | inferred spec | assertion |  axioms |
|--------|-------|---------------|-----------|---------|
| [custom-stack](#custom-stack-prog) | [vc](#custom-stack-vc) |[cons](#custom-stack-libs-cons),[is_empty](#custom-stack-libs-is-empty), [stack_head](#custom-stack-libs-stack-head), [stack_tail](#custom-stack-libs-stack-tail)|          [assertion](#custom-stack-assertion-1) |    [axiom](#custom-stack-axiom-1)         |

| [custom-stack](#custom-stack-prog) | [vc](#custom-stack-vc) |[cons](#custom-stack-libs-cons),[is_empty](#custom-stack-libs-is-empty), [stack_head](#custom-stack-libs-stack-head), [stack_tail](#custom-stack-libs-stack-tail)|          [assertion](#custom-stack-assertion-2) |    [axiom](#custom-stack-axiom-2)         |

| [custom-stack](#custom-stack-prog) | [vc](#custom-stack-vc) |[cons](#custom-stack-libs-cons),[is_empty](#custom-stack-libs-is-empty), [stack_head](#custom-stack-libs-stack-head), [stack_tail](#custom-stack-libs-stack-tail)|          [assertion](#custom-stack-assertion-3) |    [axiom](#custom-stack-axiom-3)         |

| [stream](#stream-prog) | [vc](#stream-vc) |[cons](#stream-libs-cons),[nil](#stream-libs-nil), [force](#stream-libs-force), [lazy](#stream-libs-lazy)| [assertion](#stream-assertion-1) |    [axiom](#stream-axiom-1)         |

| [stream](#stream-prog) | [vc](#stream-vc) |[cons](#stream-libs-cons),[nil](#stream-libs-nil), [force](#stream-libs-force), [lazy](#stream-libs-lazy)| [assertion](#stream-assertion-2) |    [axiom](#stream-axiom-2)         |

## custom-stack

#### custom-stack-prog

```
let rec concat l1 l2 =
  if is_empty l1 then l2
  else cons (stack_head l1) (concat (stack_tail l1) l2)
```

#### custom-stack-vc

```
(ite IsEmpty(l1)
    Concat(l1,l2,l2)
    ((
 StackHead(l1,x) /\
 StackTail(l1,l3) /\
 Concat(l3,l2,l4) /\
 Cons(x,l4,l5)
) =>
     Concat(l1,l2,l5)
    ))
```

#### specs

##### custom-stack-libs-cons

```
Cons(x0,l0,l1):=
forall u_0 u_1,(
 (list_member(l1,u_0) <=>
  (list_order(l1,x0,u_0) \/ list_head(l1,u_0))) /\
 (list_head(l1,u_0) <=>
  (x0==u_0)) /\
 (list_order(l1,u_0,u_1) <=>
  (
   list_order(l0,u_0,u_1) \/
   (list_head(l1,u_0) /\ list_member(l0,u_1))
  ))
)
```

##### custom-stack-libs-is-empty

```

IsEmpty(l2):=
forall u_0 u_1,(
true /\
 ~(list_member(l2,u_0) \/ list_member(l2,u_1))
)

```

##### custom-stack-libs-stack-head

```

StackHead(l3,x1):=
forall u_0 u_1,((x1==u_0) <=>
 list_head(l3,u_0))
```

##### custom-stack-libs-stack-tail

```
StackTail(l4,l5):=
forall u_0 u_1,(
 (
  (
   list_member(l5,u_0) =>
   (
    list_order(l4,u_1,u_0) \/
    (
     list_member(l4,u_0) /\
     (
      ~list_head(l4,u_0) \/
      (
       list_head(l5,u_0) \/
       ~list_member(l4,u_1)
      )
     )
    )
   )
  ) /\
  (
   (
    list_order(l4,u_1,u_0) \/
    (
     list_head(l5,u_0) \/
     (
      list_member(l4,u_0) /\
      ~list_head(l4,u_0)
     )
    )
   ) =>
   list_member(l5,u_0)
  )
 ) /\
 (
  (
   list_head(l5,u_0) =>
   (
    list_member(l5,u_0) /\
    (
     ~list_order(l4,u_1,u_0) \/
     (
      (u_0==u_1) \/
      (ite list_order(l5,u_0,u_1)
          list_order(l5,u_1,u_0)
          ~list_member(l5,u_1))
     )
    )
   )
  ) /\
  (
   (
    list_order(l5,u_0,u_1) /\
    (
     ~list_order(l4,u_1,u_0) /\
     list_head(l4,u_0)
    )
   ) =>
   list_head(l5,u_0)
  )
 ) /\
 (
  (
   list_order(l5,u_0,u_1) =>
   (list_order(l4,u_0,u_1) /\ list_member(l5,u_0))
  ) /\
  (
   (
    list_order(l4,u_0,u_1) /\
    (
     ~list_head(l4,u_0) \/
     (
      list_member(l5,u_0) /\
      (
       ~list_head(l4,u_1) /\
       (list_head(l5,u_0) \/ list_head(l5,u_1))
      )
     )
    )
   ) =>
   list_order(l5,u_0,u_1)
  )
 )
)
```

#### custom-stack-assertion-1

```
Concat(l1,l2,l3):=
forall u,(list_member(l3,u) <=>
 (list_member(l1,u) \/ list_member(l2,u)))
```

#### custom-stack-axiom-1

```
forall dt u_0 u_1,(ite list_member(dt,u_0)
    (
     list_member(dt,u_1) \/
     (
      ~list_order(dt,u_0,u_1) /\
      (
       ~list_head(dt,u_1) /\
       ~list_order(dt,u_1,u_0)
      )
     )
    )
    (
     ~list_head(dt,u_0) /\
     (
      ~list_order(dt,u_1,u_0) /\
      (
       ~list_order(dt,u_0,u_1) /\
       (
        ~list_head(dt,u_1) \/
        list_member(dt,u_1)
       )
      )
     )
    ))
```

#### custom-stack-assertion-2

```
Concat(l1,l2,l3):=
forall u v,(
 (list_member(l3,u) <=>
  (list_member(l1,u) \/ list_member(l2,u))) /\
 (
  (list_order(l1,u,v) \/ list_order(l2,u,v)) =>
  list_order(l3,u,v)
 )
)
```

#### custom-stack-axiom-2

```
forall dt u_0 u_1,(ite list_member(dt,u_0)
    (
     list_member(dt,u_1) \/
     (
      list_head(dt,u_0) \/
      (
       ~list_head(dt,u_1) /\
       ~list_order(dt,u_0,u_1)
      )
     )
    )
    (
     ~list_order(dt,u_1,u_0) /\
     (
      ~list_head(dt,u_0) /\
      (
       list_member(dt,u_1) \/
       (
        (u_0==u_1) \/
        (
         ~list_head(dt,u_1) /\
         ~list_order(dt,u_0,u_1)
        )
       )
      )
     )
    ))
```

#### custom-stack-assertion-3

```
Concat(l1,l2,l3):=
forall u,(
 (list_member(l3,u) <=>
  (list_member(l1,u) \/ list_member(l2,u))) /\
 (
  list_head(l3,u) =>
  (list_head(l1,u) \/ list_head(l2,u))
 )
)
```

#### custom-stack-axiom-3

```
	forall dt u_0 u_1,(ite list_member(dt,u_0)
    (
     list_order(dt,u_1,u_0) \/
     (
      (u_0==u_1) \/
      ~list_head(dt,u_1)
     )
    )
    (
     ~list_head(dt,u_0) /\
     (
      ~list_order(dt,u_1,u_0) /\
      (
       ~list_head(dt,u_1) \/
       list_member(dt,u_1)
      )
     )
    ))
```

## stream

#### stream-prog

```
let rec reverse' acc s =
  lazy (
    match s with
    | lazy Nil -> Lazy.force acc
    | lazy (Cons (hd, tl)) -> Lazy.force (reverse' (lazy (Cons (hd, acc))) tl))
```

#### stream-vc

```
(
 ((
 Nil(tmp1) /\
 Lazy(tmp1,s) /\
 Force(acc,tmp3) /\
 Lazy(tmp3,l3)
) =>
  Reverse(acc,s,l3)
 ) /\
 ((
 Cons(hd,tl,tmp1) /\
 Lazy(tmp1,s) /\
 Cons(hd,acc,tmp2) /\
 Lazy(tmp2,tmp3) /\
 Reverse(tmp3,tl,tmp4) /\
 Force(tmp4,tmp5) /\
 Lazy(tmp5,l3)
) =>
  Reverse(acc,s,l3)
 )
)
```

#### specs

##### stream-libs-cons

```
Cons(x0,l0,l1):=
forall u_0 u_1,(
 (list_member(l1,u_0) <=>
  (list_order(l1,x0,u_0) \/ list_head(l1,u_0))) /\
 (list_head(l1,u_0) <=>
  (x0==u_0)) /\
 (list_order(l1,u_0,u_1) <=>
  (
   list_order(l0,u_0,u_1) \/
   (list_head(l1,u_0) /\ list_member(l0,u_1))
  ))
)
```

##### stream-libs-nil

```
Nil(l2):=
forall u_0 u_1,(
true /\
 ~(list_member(l2,u_0) \/ list_member(l2,u_1))
)
```

##### stream-libs-force

```
Force(l5,l6):=
forall u_0 u_1,(
 (list_member(l6,u_0) <=>
  list_member(l5,u_0)) /\
 (list_head(l6,u_0) <=>
  list_head(l5,u_0)) /\
 (list_order(l6,u_0,u_1) <=>
  list_order(l5,u_0,u_1))
)
```

##### stream-libs-lazy

```
Lazy(l3,l4):=
forall u_0 u_1,(
 (list_member(l4,u_0) <=>
  list_member(l3,u_0)) /\
 (list_head(l4,u_0) <=>
  list_head(l3,u_0)) /\
 (list_order(l4,u_0,u_1) <=>
  list_order(l3,u_0,u_1))
)
```

#### stream-assertion-1

```
Reverse(l1,l2,l3):=
forall u v,(list_member(l3,u) <=>
 (list_member(l1,u) \/ list_member(l2,u)))
```

#### stream-axiom-1

```
	forall dt u_0 u_1,(ite list_member(dt,u_0)
    (
     list_order(dt,u_1,u_0) \/
     (
      (u_0==u_1) \/
      ~list_head(dt,u_1)
     )
    )
    (
     ~list_head(dt,u_0) /\
     (
      ~list_order(dt,u_0,u_1) /\
      (
       ~list_order(dt,u_1,u_0) /\
       (
        ~list_head(dt,u_1) \/
        list_member(dt,u_1)
       )
      )
     )
    ))
```

#### stream-assertion-2

```
Reverse(l1,l2,l3):=
forall u v,(
 (
  (
   list_order(l1,u,v) \/
   list_order(l2,v,u) \/
   (list_member(l2,u) /\ list_member(l1,v))
  ) =>
  list_order(l3,u,v)
 ) /\
 (list_member(l3,u) <=>
  (list_member(l1,u) \/ list_member(l2,u)))
)
```

#### stream-axiom-2

```
	forall dt u_0 u_1,(ite list_member(dt,u_0)
    (
     list_member(dt,u_1) \/
     ~list_order(dt,u_0,u_1)
    )
    (
     ~list_head(dt,u_0) /\
     (
      ~list_order(dt,u_1,u_0) /\
      (
       list_member(dt,u_1) \/
       (
        (u_0==u_1) \/
        (
         ~list_head(dt,u_1) /\
         ~list_order(dt,u_0,u_1)
        )
       )
      )
     )
    ))
```
