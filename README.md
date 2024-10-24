# RASP_functions

This repo contains several functions and s-ops designed to work in the esolang
RASP to do a bunch of unrelated things. I used these to get a better
understanding of how the language (and transformers) work

An S-Op that contains the indices in reverse order

```
reverse_indices = length - indices - 1
```
 
A function that rotates a sequence by a given number of characters

```
def rotate(seq, num) {
    _indices = (indices + num) % length;
    return aggregate(select(_indices, indices, ==), seq);
}
```

A function that swaps every element in the sequence with its neighbor

```
def _swap(seq) {
    return aggregate(select(indices, indices + 1, ==), seq, "$") 
    if indices ;% 2 == 0
    else aggregate(select(indices, indices-1, ==), seq, "$")
}
def swap(seq) {
    return aggregate(select(indices, indices, ==), seq, "$")
    if length % 2 == 1 and indices == length - 1
    else _swap(seq);
}
```


A function that returns the max token in a sequence

```
sorted = sort(tokens, tokens);
def max_token(seq) {
    return load_from_location(sorted, select_from_last);
}
```

A function that performs sequence reversal autogeneratively

```
def autogen(seq) {
    dollar_sign_location = select_from_first(seq, "$");
    dollar_sign_index = aggregate(dollar_sign_location, indices);
    return aggregate((select(indices, dollar_sign_index, <=) and select(indices, indices, ==)) or (select(indices, dollar_sign_index, <) and select(indices, dollar_sign_index*2 - indices, ==)), seq, "");
}
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


A function that counts the number of times a certain token has appeared in the 
input sequence so far.

```
def howmany(seq, n) {
    return round((indices+1) * aggregate(select(indices, indices, <=), indicator(seq==n)));
}
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
<img src=/smiley_face.png width=300px />
```


