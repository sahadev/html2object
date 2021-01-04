# html2object

This library is used to parse html into javascript object, or to convert javascript object into html.

## How to use

### Install
```npm
npm install html2object
```
and in your code:
```js
const { html2Object, object2Html } = require("html2object");

html2Object("<div><span>This is span</span></div>")
  .then((object) => {
    const json = JSON.stringify(object);
    console.info(json);
    // output:
    // {
    //     "root": {
    //         "__children": [
    //             {
    //                 "div": {
    //                     "__children": [
    //                         {
    //                             "span": {
    //                                 "__children": [],
    //                                 "__text__": "This is span"
    //                             }
    //                         }
    //                     ]
    //                 }
    //             }
    //         ]
    //     }
    // }

    // modify object property
    const anotherSpanNode = {
      span: { __text__: "This is another span" },
    };
    object.root.__children[0].div.__children.push(anotherSpanNode);

    return object2Html(object.root);
  })
  .then((html) => {
    console.info(html);
    // output:
    // <div>
    //     <span>
    //         This is span 
    //     </span>
    //     <span>
    //         This is another span 
    //     </span>
    // </div>
  });
```

## Note

1. The each node is builded as this construct:
```
{
    tagName: {
        attribute1Key: attribute1Value,
        attribute2Key: attribute2Value,
        ...
        __children: [
            // children node is here.
        ]
    }
}
```
2. The root Node is always wraped by 'root' node. Like this:
```
{
    "root": {
        "__children": [
            {
                // Your actual Node is here.
            }
        ]
    }
}
```

so you should be access your actual node by this: ```jsobject.root.__children[0]```.

> ↑ That's really stupid.

3. This library consists of two parts.

 - The html to object part is implemented by 'htmlparser2'.
 - The object to html part is implemented by 'fast-xml-parser'.

Because of the library 'fast-xml-parser' is focus to parse xml. Which parse html have a lot of problem. So I have to change the implement of html parse. 

> Write in 2021-01-04：I found a very powerful library that very similar with html2object in today, I'm very puzzled why I did't find this library in start. This powerful library is parse5. Please try it in deed.