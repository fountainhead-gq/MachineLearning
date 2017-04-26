
## String operations


```python
from __future__ import print_function
import numpy as np
```


```python
np.__version__
```




    '1.11.3'



Q1. Concatenate x1 and x2.


```python
x1 = np.array(['Hello', 'Say'], dtype=np.str)
x2 = np.array([' world', ' something'], dtype=np.str)
out = np.char.add(x1, x2)
print(out)
```

    ['Hello world' 'Say something']
    

Q2. Repeat x three time element-wise.


```python
x = np.array(['Hello ', 'Say '], dtype=np.str)
out = np.char.multiply(x, 3)
print(out)
```

    ['Hello Hello Hello ' 'Say Say Say ']
    

Q3-1. Capitalize the first letter of x element-wise.<br/>
Q3-2. Lowercase x element-wise.<br/>
Q3-3. Uppercase x element-wise.<br/>
Q3-4. Swapcase x element-wise.<br/>
Q3-5. Title-case x element-wise.<br/>


```python
x = np.array(['heLLo woRLd', 'Say sOmething'], dtype=np.str)
capitalized = np.char.capitalize(x)
lowered = np.char.lower(x)
uppered = np.char.upper(x)
swapcased = np.char.swapcase(x)
titlecased = np.char.title(x)
print("capitalized =", capitalized)
print("lowered =", lowered)
print("uppered =", uppered)
print("swapcased =", swapcased)
print("titlecased =", titlecased)
```

    capitalized = ['Hello world' 'Say something']
    lowered = ['hello world' 'say something']
    uppered = ['HELLO WORLD' 'SAY SOMETHING']
    swapcased = ['HEllO WOrlD' 'sAY SoMETHING']
    titlecased = ['Hello World' 'Say Something']
    

Q4. Make the length of each element 20 and the string centered / left-justified / right-justified with paddings of `_`.


```python
x = np.array(['hello world', 'say something'], dtype=np.str)
centered = np.char.center(x, 20, fillchar='_')
left = np.char.ljust(x, 20, fillchar='_')
right = np.char.rjust(x, 20, fillchar='_')

print("centered =", centered)
print("left =", left)
print("right =", right)
```

    centered = ['____hello world_____' '___say something____']
    left = ['hello world_________' 'say something_______']
    right = ['_________hello world' '_______say something']
    

Q5. Encode x in cp500 and decode it again.


```python
x = np.array(['hello world', 'say something'], dtype=np.str)
encoded = np.char.encode(x, 'cp500')
decoded = np.char.decode(encoded,'cp500')
print("encoded =", encoded)
print("decoded =", decoded)
```

    encoded = [b'\x88\x85\x93\x93\x96@\xa6\x96\x99\x93\x84'
     b'\xa2\x81\xa8@\xa2\x96\x94\x85\xa3\x88\x89\x95\x87']
    decoded = ['hello world' 'say something']
    

Q6. Insert a space between characters of x.


```python
x = np.array(['hello world', 'say something'], dtype=np.str)
out = np.char.join(" ", x)
print(out)
```

    ['h e l l o   w o r l d' 's a y   s o m e t h i n g']
    

Q7-1. Remove the leading and trailing whitespaces of x element-wise.<br/>
Q7-2. Remove the leading whitespaces of x element-wise.<br/>
Q7-3. Remove the trailing whitespaces of x element-wise.


```python
x = np.array(['   hello world   ', '\tsay something\n'], dtype=np.str)
stripped = np.char.strip(x)
lstripped = np.char.lstrip(x)
rstripped = np.char.rstrip(x)
print("stripped =", stripped)
print("lstripped =", lstripped)
print("rstripped =", rstripped)
```

    stripped = ['hello world' 'say something']
    lstripped = ['hello world   ' 'say something\n']
    rstripped = ['   hello world' '\tsay something']
    

Q8. Split the element of x with spaces.


```python
x = np.array(['Hello my name is John'], dtype=np.str)
out = np.char.split(x)
print(out)
```

    [['Hello', 'my', 'name', 'is', 'John']]
    

Q9. Split the element of x to multiple lines.


```python
x = np.array(['Hello\nmy name is John'], dtype=np.str)
out = np.char.splitlines(x)
print(out)
```

    [['Hello', 'my name is John']]
    

Q10. Make x a numeric string of 4 digits with zeros on its left.


```python
x = np.array(['34'], dtype=np.str)
out = np.char.zfill(x, 4)
print(out)
```

    ['0034']
    

Q11. Replace "John" with "Jim" in x.


```python
x = np.array(['Hello nmy name is John'], dtype=np.str)
out = np.char.replace(x, "John", "Jim")
print(out)
```

    ['Hello nmy name is Jim']
    

## Comparison

Q12. Return x1 == x2, element-wise.


```python
x1 = np.array(['Hello', 'my', 'name', 'is', 'John'], dtype=np.str)
x2 = np.array(['Hello', 'my', 'name', 'is', 'Jim'], dtype=np.str)
out = np.char.equal(x1, x2)
print(out)
```

    [ True  True  True  True False]
    

Q13. Return x1 != x2, element-wise.


```python
x1 = np.array(['Hello', 'my', 'name', 'is', 'John'], dtype=np.str)
x2 = np.array(['Hello', 'my', 'name', 'is', 'Jim'], dtype=np.str)
out = np.char.not_equal(x1, x2)
print(out)
```

    [False False False False  True]
    

## String information

Q14. Count the number of "l" in x, element-wise.


```python
x = np.array(['Hello', 'my', 'name', 'is', 'Lily'], dtype=np.str)
out = np.char.count(x, "l")
print(out)
```

    [2 0 0 0 1]
    

Q15. Count the lowest index of "l" in x, element-wise.


```python
x = np.array(['Hello', 'my', 'name', 'is', 'Lily'], dtype=np.str)
out = np.char.find(x, "l")
print(out)

# compare
# print(np.char.index(x, "l"))
# => This raises an error!
```

    [ 2 -1 -1 -1  2]
    

Q16-1. Check if each element of x is composed of digits only.<br/>
Q16-2. Check if each element of x is composed of lower case letters only.<br/>
Q16-3. Check if each element of x is composed of upper case letters only.


```python
x = np.array(['Hello', 'I', 'am', '20', 'years', 'old'], dtype=np.str)
out1 = np.char.isdigit(x)
out2 = np.char.islower(x)
out3 = np.char.isupper(x)
print("Digits only =", out1)
print("Lower cases only =", out2)
print("Upper cases only =", out3)
```

    Digits only = [False False False  True False False]
    Lower cases only = [False False  True False  True  True]
    Upper cases only = [False  True False False False False]
    

Q17. Check if each element of x starts with "hi".


```python
x = np.array(['he', 'his', 'him', 'his'], dtype=np.str)
out = np.char.startswith(x, "hi")
print(out)
```

    [False  True  True  True]
    


```python

```
