## Selector.js Workshop Review

#### SelectorTypeMatcher Function
```
function selectorTypeMatcher(selector) {
  // If the string starts with '#', it's an id selector
  if (selector[0] === '#') {
    return 'id';
  } else if (selector[0] === '.') {
    return 'class';
  // if the string starts with '.', it's a class selector
  } else {
    // Here it is either tag or tag.class - if it contains a '.', it's a tag class, otherwise, it's a tag
    if (selector.indexOf('.') !== -1) {
      return 'tag.class'
    } else {
      return 'tag'
    }
  }
}
```

#### MatchFunctionMaker Function
```
function matchFunctionMaker(selector) {
  var selectorType = selectorTypeMatcher(selector);
  var matchFunction;
  if (selectorType === 'id') {
    matchFunction = function(element) {
      return element.id === selector.slice(1);
    }
  } else if (selectorType === 'class') {
    matchFunction = function(element) {
      var classes = element.className.split(" ")
      return classes.indexOf(selector.slice(1)) !== -1;
    }
  } else if (selectorType === 'tag.class') {
    matchFunction = function(element) {
      var tag = selector.split('.')[0];
      var theClass = selector.split('.')[1];
      var classes = element.className.split(" ")
      return classes.indexOf(theClass) !== -1 
        && element.tagName.toLowerCase() === tag.toLowerCase();
    }
  } else if (selectorType === 'tag') {
    matchFunction = function(element) {
      return element.tagName.toLowerCase() === selector.toLowerCase();
    }
  }
  return matchFunction;
}
```

#### Selector Function
Given a start element and a match function, you want to check every element to see if it matches the selector.
```
function traverseDomAndCollectElements(matchFunc, startEl){
  var resultSet = [];
  
  if (typeOf startEl === 'undefined') {
    startEl = document.body;
  }

  if (matchFunc(startEl)) {
    resultSet.push(startEl);
  }

  if (startEl.children.length) {
    [].slice.call(startEl.children).forEach(function(child) {
      var matchingElementsStartingAtChild = traverseDomAndCollectElements(matchFunc,child);
      resultSet = resultSet.concat(matchingElementsStartingAtChild);
    })
  }

  return resultSet;
}
```
```
function $(selector) {
  var selectorMatchFunc = matchFunctionMaker(selector);
  var elements = traverseDomAndCollectElements(selectorMatchFunc);
  return elements;
};
```