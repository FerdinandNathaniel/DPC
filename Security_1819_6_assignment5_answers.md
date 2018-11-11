# Security
## Assignment 5 - answers
### Monday, November 5 2018
##### Fabian Kok (s4474333)
##### Teaching assistant: Simona Samardjiska

## Exercise 1
<sub><sup>_Disclaimer: within the slides it says a man-in-the-middle attack is a 'special case' of a replay attack, but this does not follow from the assignments. After asking resident TA's I was adviced to only look at replay attacks, no man-in-the-middle attacks._</sup></sub>

1. Replay attack is possible, Eve can catch the message Alice sends to Bob ($A \to B : A, K_{AB}\{A\}$) and use that to authenticate herself. Meaning;<br>
   1. $E(A) \to B : m$
   2. $B \to E(A) : B, K_{AB}\{B\}$
   3. $E(A) \to B : A, K_{AB}\{A\}$

2. As Alice and Bob do not maintain a list of used nonces allowing duplicates to surface, Eve can catch the message Alice sends to Bob ($K_{AB}\{A,B,N_{B}+1\}$) and replay it when/if Bob uses the same N_{B} again. Meaning;
    1. $E(A) \to B : A, N_{A}$
    2. $B \to E(A) : B, N_{B}, K_{AB}\{B, N_{A} - 1\}$
    3. $E(A) \to B : K_{AB}\{A,B,N_{B}+1\}$

3. As Alice and Bob do not maintain a list of used nonces allowing duplicates to surface, Eve can catch the message Bob sends to Alice ($N_{A}, K_{AB}$) and replay it when Alice sends the same $K_{AB}\{N_{A}\}$ again, meaning;
    1. $A \to B : K_{AB}\{N_{A}\}$
    2. $E(B) \to A : N_{A}, K_{AB}$
    3. $A \to E(B) : K_{AB}\{N_{B} + 1\}$

4. Eve can catch the message Bob sends to Alice ($N_{B}, K_{AB}\{N_{B}\}$) and use this to impersonate Bob, meaning;
    1. $A \to B : N_{A}, K_{AB}\{N_{A}\}$
    2. $E(B) \to A : N_{B}, K_{AB}\{N_{B}\}$
    3. $A \to E(B) : K_{AB}\{A,B,N_{A}\}$


## Exercise 2

1. 
    1. session 1: $E(A) \to B : A, N_{A}$
   
    2. session 1: $B \to E(A) : N_{B}, K_{AB}\{N_{A} + 3\}$
   
    3. **session 2**: $E(A) \to B : A, N_{B} + 3$
   
    4. **session 2**: $B \to E(A) : N_{X}, K_{AB}\{N_{B} + 6\}$
   
    5. session 1: $E(A) \to B: K_{AB}\{N_{B} + 6\}$

2. 
    1. $A \to B : A, N_{A}$
   
    2. $B \to A : N_{B}, K_{AB}\{N_{A} + 3\}$
   
    3. $A \to B : K_{BA}\{N_{B} + 6\}$

3.  <br>
    Eve can copy both replies of Alice and send them to Bob to authenticate herself as Alice.<br>
    
    1. $E(A) \to B : A, K_{AB}\{N_{A} - 1\}$ 
    
    2. $B \to E(A) : N_{A}, L_{AB}\{N_{B}\}$
    
    3. $E(A) \to B : K_{AB}\{A, B, N_{A}\}$
   
4. 
    1. $A \to B : A, K_{AB}\{N_{A} -1\}$
    
    2. $B \to A : N_{A}, L_{AB}\{N_{B}\}$
    
    3. $A \to B : K_{AB}\{A, B, N_{B}\}$

## Exercise 3

1. Include a timestamp $t$ within $K_{B}\{A, K_{AB}\}$ as nonce $N_{t}$ with a sufficiently safe (but feasible) short interval as authentication-threshold, changing the original to: $K_{B}\{A, K_{AB}, N_{t}\}$

2. Include a unique Tag in step 2 within $K_{B}\{A, K_{AB}\}$ as $K_{B}\{A, K_{AB}, T\}$ which is used to encapsulate the ticket, as $T\{K_{A}\{B, K_{AB}\}\}$ i.e., $T\{ticket\}$. The ticket is then sent unpacked with the given Tag $T$.

3. Include a nonce shared with Alice by the TTP to verify the validity of the sent ticket.
    1. $B \to TTP :$ _'Hi, I'm Bob and wish to talk to Alice'_
    2. $TTP \to A :$ _'Hi Alice, Bob wishes to talk to you'_, $K_{A}\{N\}$
    3. $TTP \to B : K_{B}\{A, K_{AB}\}, ticket = K_{A}\{B, K_{AB}, N\}$
    4. $B \to A : ticket, K_{AB}\{message\}$
