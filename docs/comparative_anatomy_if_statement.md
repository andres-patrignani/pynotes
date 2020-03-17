# Comparative Anatomy of If Statements

A short example was coded in several common languages for data science and scientific computing to illustrate syntax similarities of `if statements`.

The aridity index represents the level of dryness/wetness of a climate region and is determined by the ration between the annual precipitation and potential atmospheric demand. Depending on the value of this ratio the climate is typically classified as arid, mid, or humid. There exist finer divisions and classifications, but for the sake of simplicity I only considered the three most prominent categories.


## Python

```python
AI = 0.5 # Aridity Index
    
if AI <= 0.5:
    macroclimate_class = 'Arid'

elif AI > 0.5 and AI <= 0.75:
    macroclimate_class = 'Mid'
    
else:
    macroclimate_class = 'Humid'

print(macroclimate_class)
```

```
Arid
```

---

## Matlab

```matlab
AI = 0.5; % Aridity Index
    
if AI <= 0.5
    macroclimate_class = 'Arid';
    
elseif AI > 0.5 && AI <= 0.75
    macroclimate_class = 'Mid';
    
else
    macroclimate_class = 'Humid';
end

disp(macroclimate_class)

```

**Answer**
```
Arid
```

---

## Javascript

```javascript
AI = 0.5 // Aridity Index
    
if (AI <= 0.5){
    macroclimate_class = 'Arid'
    
} else if (AI > 0.5 && AI <= 0.75){
    macroclimate_class = 'Mid'
    
} else {
    macroclimate_class = 'Humid'
}

console.log(macroclimate_class)
```

**Answer**

```
Arid
```

---

## Julia

```julia
AI = 0.5 # Aridity Index

if AI <= 0.5
    macroclimate_class = "Arid"

elseif AI > 0.5 && AI <= 0.75
    macroclimate_class = "Mid"

else
    macroclimate_class = "Humid"
end

print(macroclimate_class)

```

**Answer**

```
Arid
```

## Synthesis

- All languages follow the syntax `if condition`

- All languages contain syntax for `if`, `else if`, and `else` options

- Boolean operators `==`, `<`, and `>` are pretty much the same across different languages. The operators `and` and `or` may be represented differently but conceptually are the same acroos multiple languages.
