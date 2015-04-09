+++
date = "2015-04-09T14:00:53+08:00"
tags = ["x", "y"]
title = "How to use HTML5 attribute"

+++
how to get and set data-* attribute by javascript and css<!--more-->

# HTML5 Data attribute
**html5 data attribute**
```html
<input text='value' data-source='source' data-id='ID123' data-user-name='jane'>
```

## how to get and set data attribute 

### Using `getAttribute()` and `setAttribute()`
```javascript
var $target=document.getElementById('#id');
var user_name=$target.getAttribute('data-user-name');
$target.setAttribute('data-user-name');
```

### Jquery `.data()` function
```javascirpt
var user_name=$target.data('user-name');
$target.data('user-name')=value;
```

### dataset
```javascript
var user_name=$target.dataset.username;
```
IE10 and below IE browser not support dataset, so if using less than IE10 you should use first and second solution.

## Data Attribute used by CSS
- get data value
```css
#target{
    content:@user-name
}
```

- CSS selector

```css
#target[username='']{
    color:red;
}
```