# css功能样式收集

## 一、不超过某行，超出位置以省略号显示

不超过一行，超出位置以省略号的形式显示：

```css
.ellipsis {
    display: block;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
}
```

不超过两行，超出位置以省略号的形式显示：

```css
.ellipsis-x {
    overflow: hidden;
    text-overflow: ellipsis;
    display: block;
    display: -webkit-box;
    -webkit-box-orient: vertical;
}

.ellipsis-2 {
    @extend .ellipsis-x;
    -webkit-line-clamp: 2;
}
```

不超过三行，超出位置以省略号的形式显示：

```css
.ellipsis-x {
    overflow: hidden;
    text-overflow: ellipsis;
    display: block;
    display: -webkit-box;
    -webkit-box-orient: vertical;
}

.ellipsis-3 {
    @extend .ellipsis-x;
    -webkit-line-clamp: 3;
}
```

不超过四行，超出位置以省略号的形式显示：

```css
.ellipsis-x {
    overflow: hidden;
    text-overflow: ellipsis;
    display: block;
    display: -webkit-box;
    -webkit-box-orient: vertical;
}

.ellipsis-4 {
    @extend .ellipsis-x;
    -webkit-line-clamp: 4;
}
```

