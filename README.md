<!--
author:   André Dietrich

email:    andre.dietrich@ovgu.de

version:  0.0.3

language: en

narrator: US English Female

comment:  A set of macros for plotting diagrams with plantUML in LiaScript.

script:   https://s.plantuml.com/synchro2.min.js

@plantUML: @plantUML.exec(@uid,@0)

@plantUML.eval: @plantUML.exec(@uid,`@input`)

@plantUML.exec
<script>
var draw = function () {
  try {
    let s = unescape(encodeURIComponent(`@1`));
    var arr = [];
    for (let i = 0; i < s.length; i++) {
      arr.push(s.charCodeAt(i));
    }
    let compressor = new Zopfli.RawDeflate(arr);
    let compressed = compressor.compress();
    let dest = "http://www.plantuml.com/plantuml" + "/png/"+encode64_(compressed);

    document.getElementById('plant@0').src = dest;
    document.getElementById('plant@0').hidden = false;

    return dest;
  } catch (e) {
    setTimeout( draw, 100)
  }
};

draw()
</script>

<span>
<img id="plant@0" src="@0" hidden="true">
</span>

@end
-->

# plantUML

                         --{{0}}--

A template for including plantUML diagrams into
[LiaScript](https://liascript.github.io) courses. See the following links for
further information and documentation:

                          {{0-1}}
* See the plantUML docs [here...](http://plantuml.com)
* See the Github version of this document
  [here...](https://github.com/liascript-templates/plantUML)
* See the LiaScript version of this document
  [here...](https://liascript.github.io/course/?https://raw.githubusercontent.com/liascript-templates/plantUML/master/README.md)


                         --{{1}}--
There are three ways to use this template. The easiest way is to use the
`import` statement and the URL of the raw text-file of the master branch or any
other branch or version. But you can also copy the required functionality
directly into the header of your Markdown document, see therefor the
[last slide](#6). And of course, you could also clone this project and change
it, as you wish.

                           {{1}}
1. Load the macros via

   `import: https://raw.githubusercontent.com/liascript-templates/plantUML/master/README.md`

2. Copy the definitions into your Project

3. Clone this repository on GitHub

## `@plantUML`

                         --{{0}}--
Simply call the macro `@plantUML` in block-notation, thus, directly within the
header of the code-block. The code block should contain the code for the UML
diagram. The code is then directly evaluated and translated into a plantUML-URL.

```text @plantUML
@startuml
Bob -> Alice : hello
@enduml
```

## `@plantUML.eval`

                         --{{0}}--

Use a code-block with plantUML code and add the macro `@plantUML.eval` to the
end of that block to get and editor, which contains the code and by clicking
onto play, you trigger its evaluation. The displayed URL is also the source of
the image.


```
@startuml

abstract class AbstractList
abstract AbstractCollection
interface List
interface Collection

List <|-- AbstractList
Collection <|-- AbstractCollection

Collection <|- List
AbstractCollection <|- AbstractList
AbstractList <|-- ArrayList

class ArrayList {
  Object[] elementData
  size()
}

enum TimeUnit {
  DAYS
  HOURS
  MINUTES
}

annotation SuppressWarnings

@enduml
```
@plantUML.eval

## Implementation

                         --{{0}}--
The two main-macros make use of `@plantUML.exec` which receives two parameters.
The first one is a unique id and the second one contains the code.

````html
script:   https://s.plantuml.com/synchro2.min.js

@plantUML: @plantUML.exec(@uid,@0)

@plantUML.eval: @plantUML.exec(@uid,`@input`)

@plantUML.exec
<script>
let s = unescape(encodeURIComponent(`@1`));
var arr = [];
for (let i = 0; i < s.length; i++) {
  arr.push(s.charCodeAt(i));
}

let compressor = new Zopfli.RawDeflate(arr);
let compressed = compressor.compress();
let dest = "http://www.plantuml.com/plantuml" + "/svg/"+encode64_(compressed);

document.getElementById('plant@0').src = dest;

dest;
</script>

<span>
<img id="plant@0" src="@0">
</span>

@end
````

                         --{{1}}--
If you want to minimize loading effort in your LiaScript project, you can also
copy this code and paste it into your main comment header, see the code in the
raw file of this document.

{{1}} https://raw.githubusercontent.com/liascript-templates/plantUML/master/README.md
