# Build doc for ca65 syntax (and C language)

Thoses scripts are used to generate markdown from source code with ca65 syntax.

## Doc

Each pattern starts with ';;@'. In that case, each line will be handled by the script ca65tomd.py.

'.proc' and '.endproc' are detected. '.proc' parameter will define the function and it will be displayed as a new markdown function.

### Script example in order to build doc

#!/bin/bash
VERSION=`cat VERSION`
mkdir -p docs/code/$VERSION
echo "# Assembly" > docs/code/assembly.md
echo "" >> docs/code/assembly.md

echo "# Assembly" > docs/code/$VERSION/assembly.md
echo "" >> docs/code/$VERSION/assembly.md

echo $VERSION

for I in `ls src/*.s`; do
echo $I
cat  $I | python3 docs/ca65todoc.py >> docs/code/$VERSION/assembly.md
cat  $I | python3 docs/ca65todoc.py >> docs/code/assembly.md
done

### @brief

Describe the function. Must be on *one line*.

### @modifyA

Defines that A accumulator is modifyed.

### @modifyY

Defines that Y register is modifyed.

### @modifyX

Defines that X register is modifyed.

### @returnsA

Defines that A accumulator returns something. It requires one parameter, a string explaning define what A returns.

### @returnsX

Defines that X register returns something. It requires one parameter, a string explaning define what X returns.

### @returnsY

Defines that A accumulator returns something. It requires one parameter, a string explaning define what Y returns.

### @inputMEM_

Describes what the routine needs to work with a parameter from memory

Ex :

;;@inputMEM_RES String ptr

### @inputA

Describes what the routine needs to work with a parameter from A

### @inputX

Describes what the routine needs to work with a parameter from X

### @inputY

Describes what the routine needs to work with a parameter from Y

### @bug

Explain a bug

### @note

Display a note

### Example

```
.proc twil_lib_version
    ;;@brief Return twil lib version
    ;;@modifyA
    ;;@returnsA twillib version
    ;;@```ca65
    ;;@`  jsr       twil_lib_version
    ;;@`  cmp       #TWIL_LIB_VERSION_2024_1
    ;;@`  beq       @is_twillib2024.1
    ;;@`  rts
    ;;@`  @is_twillib2024_1:
    ;;@`  ; code here
    ;;@`  rts
    ;;@```
    lda     #TWIL_LIB_VERSION_2024_1
    rts
.endproc
```