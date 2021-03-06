# CSS 实现类似原生效果的 1px 边框

- `border`属性实现效果如下：

```CSS
.border-1px {
    border-width: 1px 0;
    border-style: solid;
    border-color: #333;
}
```

在手机上`border`无法达到想要的效果。
这是因为 devicePixelRatio 特性导致，`iPhone`的`devicePixelRatio==2`，而`border-width: 1px`描述的是设备独立像素，所以，border 被放大到物理像素 2px 显示，在 iPhone 上就显得较粗。

- 使用`border-image`属性实现物理 1px

通常手机端的页面设计稿都是放大一倍的，如：为适应`iphone retina`，设计稿会设计成 640\*960 的分辨率，图片按照 2 倍大小切出来，在手机端看着就不会虚化，非常清晰。

同样，在使用`border-image`时，将 border 设计为物理 1px

样式设置：

```CSS
.border-image-1px {
    border-width: 1px 0;
    -webkit-border-image: url("xxx.png") 2 0 stretch;
    border-image: url("xxx.png") 2 0 stretch;
}
```

- 使用 CSS3 transform 实现

```CSS
.border-1px {
    position: relative;
}
.border-1px:after {
    position: absolute;
    content: '';
    top: -50%;
    bottom: -50%;
    left: -50%;
    right: -50%;
    -webkit-transform: scale(0.5);
    transform: scale(0.5);
    border-top: 1px solid #666;
    border-bottom: 1px solid #666;
}
```
