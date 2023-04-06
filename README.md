<!--
author:   André Dietrich

email:    LiaScript@web.de

version:  0.0.10

language: en

narrator: US English Male

comment:  A set of macros for plotting diagrams with plantUML in LiaScript. See
          also https://plantuml.com for more information.

script:   https://cdn-0.plantuml.com/synchro2.min.js

@onload

window.cleanDitaa = function(code) {
  const pattern = /@startuml\nditaa/g 
  if(code.match(pattern)) {
    return code.replace(pattern, "@startditaa").replace(/@enduml/g, "@endditaa")
  }
  return code
}

@end

@plantUML: @plantUML.exec(svg,```@0```)

@plantUML.svg: @plantUML.exec(svg,```@0```)

@plantUML.png: @plantUML.exec(png,```@0```)

@plantUML.exec
<script run-once modify="false">
function draw(type, code, counter = 10) {
  try {
    let s = unescape(encodeURIComponent(window.cleanDitaa(code)));
    var arr = [];
    for (let i = 0; i < s.length; i++) {
      arr.push(s.charCodeAt(i));
    }
    let compressor = new Zopfli.RawDeflate(arr);
    let compressed = compressor.compress();
    let dest = "https://www.plantuml.com/plantuml/" + type + "/" + encode64_(compressed);

    setTimeout(function() {
      send.lia("HTML: <img style='max-width: 100%' src='" + dest + "' onclick='window.LIA.img.click(\"" + dest + "\")'>")
    }, 100)
  } catch(e) {
    if (counter > 0) {
      setTimeout(function() {
        draw(type, code, counter - 1)
      }, 500)
    } else {
      send.lia("LIA: stop")
    }
  }
}


draw("@0", `@1`)
</script>

<span>
<img id="plant@0" src="@0" style="display:none">
</span>

@end


@plantUML.eval
<script>
function draw(type, code, counter = 10) {
  try {
    let s = unescape(encodeURIComponent(window.cleanDitaa(code)));
    var arr = [];
    for (let i = 0; i < s.length; i++) {
      arr.push(s.charCodeAt(i));
    }
    let compressor = new Zopfli.RawDeflate(arr);
    let compressed = compressor.compress();
    let dest = "https://www.plantuml.com/plantuml/" + type + "/" + encode64_(compressed);

    console.html("<img style='max-width: 100%' src='" + dest + "' onclick='window.LIA.img.click(\"" + dest + "\")'>")
    console.log(dest)

    send.lia("LIA: stop")
  } catch(e) {
    if (counter > 0) {
      setTimeout(function() {draw(type, code, counter - 1)}, 100)
    } else {
      send.lia("LIA: stop")
    }
  }
}


draw("@0", `@input`)
""
</script>
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

   to import the latest version, but the API might change in the future, to load
   this specific version load:

   `import: https://github.com/LiaTemplates/plantUML/blob/0.0.10/README.md`

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

The default image type is `svg`, however, you can overwrite this or make it
explicit, by calling either `@plantUML.png` or `@plantUML.svg`. SVG might cause
some problems in some browsers.

```text @plantUML.png
@startuml
ditaa
             Daten                 Daten
               |                     |
               |                     |
+--------------|---------------+     |
| +------+     |               |     |     Funktionsauswahl
| |      |     |               :     |        F_0 F_1 F_2
| |      V     V               V     V         |   |   | Zielregister
| | +----+-----+-----+    +----+-----+-----+   |   |   |  auswahl
| | |cBFB Register A |    |cBFB Register B |   |   |   |     Z
| | +---+------------+    +---------+------+   |   |   |     |
| |     |                           |          |   |   |     |
| |     |        +------------------+          |   |   |     |
| |     |        |                  |          |   |   |     |
| |     +--------+-----------+      |          |   |   |     |
| |     |        |           |      |          |   |   |     |
| |     V        V           V      V          |   |   |     |
| | +----------------+    +----------------+   |   |   |     |
| | |c808            |<-  |c808            |<--+   |   |     |
| | | Demultiplexer  |<-  | Demultiplexer  |<------+   |     |
| | |                |<-  |                |<----------+     |
| | ++-+-+-+-+-+-+-+-+    ++-+-+-+-+-+-+-+-+                 |
| |  | | | | | | | |       | | | | | | | |                   |
| |  | | | | | | | |       V V V V V V V V                   |
| |  | | | | | | | +----------------------------------+      |
| |  | | | | | | +-----------------------------+      |      |
| |  | | | | | +------------------------+      |      |      |
| |  | | | | +-------------------+      |      |      |      |
| |  | | | +--------------+      |      |      |      |      |
| |  | | +---------+      |      |      |      |      |      |
| |  | +----+      |      |      |      |      |      |      |
| |  |      |      |      |      |      |      |      |      |
| |  |  |   |  |   |  |   |  |   |  |   |  |   |  |   |  |   |
| |  V  V   V  V   V  V   V  V   V  V   V  V   V  V   V  V   |
| | +----+ +----+ +----+ +----+ +----+ +----+ +----+ +----+  |
| | | 000| | 001| | 010| | 011| | 100| | 101| | 110| | 111|  |
| | |cFF4| |cFF4| |cFF4| |cFF4| |cFF4| |cFF4| |cFF4| |cFF4|  |
| | | OR | |AND | |EXOR| |ADD | |SUB | |MUL | |DIV | | SL |  |
| | +-+--+ +-+--+ +-+--+ +-+--+ +-+--+ +-+--+ +-+--+ +-+--+  |
| |   :      :      :      :      :      :      :      :     |
| |   +------+------+------+---+--+------+------+------+     |
| |                            |                             |
| |                            V                             |
| |                 +----------+----------+                  |
| |                 |cCCB DeMuxer/Selektor|<-----------------+
| |                 +---+-+---------------+
| |                     : :                     |      |
| +---------------------+ |                   --+---+--+
|                         |                         | Status
+-------------------------+                         v S
@enduml
```




## `@plantUML.eval`

                         --{{0}}--
Use a code-block with plantUML code and add the macro `@plantUML.eval` to the
end of that block to get and editor, which contains the code and by clicking
onto play, you trigger its evaluation. You have to pass the parameter `svg` or
`png` to tell service, wheather you want to have an svg or png image back. The
displayed URL is also the source of the image.


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
@plantUML.eval(png)

## Implementation

                         --{{0}}--
The two main-macros make use of `@plantUML.exec` which receives two parameters.
The first one is a unique id and the second one contains the code.

````html
script:   https://cdn-0.plantuml.com/synchro2.min.js

@onload

window.cleanDitaa = function(code) {
  const pattern = /@startuml\nditaa/g 
  if(code.match(pattern)) {
    return code.replace(pattern, "@startditaa").replace(/@enduml/g, "@endditaa")
  }
  return code
}

@end

@plantUML: @plantUML.exec(svg,```@0```)

@plantUML.svg: @plantUML.exec(svg,```@0```)

@plantUML.png: @plantUML.exec(png,```@0```)

@plantUML.exec
<script run-once modify="false">
function draw(type, code, counter = 10) {
  try {
    let s = unescape(encodeURIComponent(window.cleanDitaa(code)));
    var arr = [];
    for (let i = 0; i < s.length; i++) {
      arr.push(s.charCodeAt(i));
    }
    let compressor = new Zopfli.RawDeflate(arr);
    let compressed = compressor.compress();
    let dest = "https://www.plantuml.com/plantuml/" + type + "/" + encode64_(compressed);

    setTimeout(function() {
      send.lia("HTML: <img style='max-width: 100%' src='" + dest + "' onclick='window.LIA.img.click(\"" + dest + "\")'>")
    }, 100)
  } catch(e) {
    if (counter > 0) {
      setTimeout(function() {
        draw(type, code, counter - 1)
      }, 500)
    } else {
      send.lia("LIA: stop")
    }
  }
}


draw("@0", `@1`)
</script>

<span>
<img id="plant@0" src="@0" style="display:none">
</span>

@end


@plantUML.eval
<script>
function draw(type, code, counter = 10) {
  try {
    let s = unescape(encodeURIComponent(window.cleanDitaa(code)));
    var arr = [];
    for (let i = 0; i < s.length; i++) {
      arr.push(s.charCodeAt(i));
    }
    let compressor = new Zopfli.RawDeflate(arr);
    let compressed = compressor.compress();
    let dest = "https://www.plantuml.com/plantuml/" + type + "/" + encode64_(compressed);

    console.html("<img style='max-width: 100%' src='" + dest + "' onclick='window.LIA.img.click(\"" + dest + "\")'>")
    console.log(dest)

    send.lia("LIA: stop")
  } catch(e) {
    if (counter > 0) {
      setTimeout(function() {draw(type, code, counter - 1)}, 100)
    } else {
      send.lia("LIA: stop")
    }
  }
}


draw("@0", `@input`)
""
</script>
@end
````

                         --{{1}}--
If you want to minimize loading effort in your LiaScript project, you can also
copy this code and paste it into your main comment header, see the code in the
raw file of this document.

{{1}} https://raw.githubusercontent.com/liascript-templates/plantUML/master/README.md
