# RASP_functions

This repo contains several functions and s-ops designed to work in the esolang
RASP to do a bunch of unrelated things. I used these to get a better
understanding of how the language (and transformers) work

An S-Op that contains the indices in reverse order

```
reverse_indices = length - indices - 1
```

```
>> reverse_indices;
     s-op: reverse_indices
         Example: reverse_indices("hello") = [4, 3, 2, 1, 0] (ints)
```

A function that rotates a sequence by a given number of characters

```
def rotate(seq, num) {
    _indices = (indices + num) % length;
    return aggregate(select(_indices, indices, ==), seq);
}
```

```
>> rotate(tokens, 2);
     s-op: out
         Example: out("hello") = [l, o, h, e, l] (strings)
```

A function that swaps every element in the sequence with its neighbor

```
def _swap(seq) {
    return aggregate(select(indices, indices + 1, ==), seq, "$") 
    if indices % 2 == 0 
    else aggregate(select(indices, indices-1, ==), seq, "$");
}
def swap(seq) {
    return aggregate(select(indices, indices, ==), seq, "$")
    if length % 2 == 1 and indices == length - 1
    else _swap(seq);
}
```

```
>> swap(tokens);
     s-op: out
         Example: out("hello") = [e, h, l, l, o] (strings)
```


A function that returns the max token in a sequence

```
def max_token(seq) {
    select_earlier_in_sorted = 
        select(seq,seq,<) or (select(seq,seq,==) and select(indices,indices,<));
    target_position = 
        selector_width(select_earlier_in_sorted);
    select_new_val = 
        select(target_position,indices,==);
    return aggregate(select_new_val,seq)[-1][0];
}
```

```
>> max_token(tokens);
     s-op: out
         Example: out("hello") = [o]*5 (strings)
```

A function that performs sequence reversal autogeneratively

```
def autogen(seq) {
    dollar_sign_location = select_from_first(seq, "$");
    dollar_sign_index = aggregate(dollar_sign_location, indices);
    return aggregate((select(indices, dollar_sign_index, <=) and select(indices, indices, ==)) or (select(indices, dollar_sign_index, <) and select(indices, dollar_sign_index*2 - indices, ==)), seq, "");
}
```

```
>> autogen(tokens);
     s-op: out
         Example: out("hello$     ") = [h, e, l, l, o, $, o, l, l, e, h] (strings)
```

A function that counts the number of times a certain token appears in the input 
sequence.

```
def count(seq,atom) {
	return round(
		length * aggregate(
			full_s, indicator(seq==atom)));
}
```

```
count(tokens, "l")

Example: out("hello") = [2]*5 (ints)
```
A function that counts the number of times a certain token has appeared in the 
input sequence so far.

```
def howmany(seq, n) {
    return round((indices+1) * aggregate(select(indices, indices, <=), indicator(seq==n)));
}
```

```
howmany(tokens, "l");
     s-op: out
         Example: out("hello") = [0, 0, 1, 2, 2] (ints)
```

A function that draws a smiley face!


```
set example "1234567890ABCDEFGHIJ" 
# Only thing that matters here is that your example is at least 20 chars long
def smiley_face() {
    def draw_point(x, y) {
        return select(indices, x, ==) and select(y, indices, ==);
        }
    return (draw_point(6, 6) or draw_point(7, 6) or draw_point(12, 6) or draw_point(13, 6) or draw_point(7, 12) or draw_point(8, 13) or draw_point(9, 13) or draw_point(10, 13) or draw_point(11, 12) or draw_point(2, 10) or draw_point(2, 11) or draw_point(2, 12) or draw_point(3, 5) or draw_point(3, 15) or draw_point(4, 4) or draw_point(4, 16) or draw_point(5, 3) or draw_point(5, 17) or draw_point(6, 2) or draw_point(6, 18) or draw_point(7, 2) or draw_point(7, 18) or draw_point(8, 2) or draw_point(8, 18) or draw_point(9, 2) or draw_point(9, 18) or draw_point(10, 2) or draw_point(10, 18) or draw_point(11, 2) or draw_point(11, 18) or draw_point(12, 2) or draw_point(12, 18) or draw_point(13, 2) or draw_point(13, 18) or draw_point(14, 3) or draw_point(14, 17) or draw_point(15, 4) or draw_point(15, 16) or draw_point(16, 5) or draw_point(16, 15) or draw_point(17, 10) or draw_point(17, 11) or draw_point(17, 12));
}
```
```
>> smiley_face();
     selector: out
         Example:
                             1 2 3 4 5 6 7 8 9 0 A B C D E F G H I J
                         1 |                                        
                         2 |                                        
                         3 |             1 1 1 1 1 1 1 1            
                         4 |           1                 1          
                         5 |         1                     1        
                         6 |       1                         1      
                         7 |             1 1         1 1            
                         8 |                                        
                         9 |                                        
                         0 |                                        
                         A |     1                             1    
                         B |     1                             1    
                         C |     1         1       1           1    
                         D |                 1 1 1                  
                         E |                                        
                         F |       1                         1      
                         G |         1                     1        
                         H |           1                 1          
                         I |             1 1 1 1 1 1 1 1            
                         J |                                        

```
In case you want to see it in blue!

<img src=/smiley_face.png width=1200px alt="A smiley face drawing using rasp"/>