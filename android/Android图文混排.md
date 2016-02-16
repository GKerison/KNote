# Android图文混排
1. 自定义组件，解析字符串，插入文本和图片组件
2. WebView呈现方式
3. TextView支持的Span模式

比较：
- 自定义组件并解析组装View的方法虽然也可以实现复杂的混排，但是操作比较麻烦，应用场景为应用需要特殊定制样式、动画、响应等。
- WebView呈现最为丰富，基本能满足大部分HTML标签，但是消耗资源也相应较大，应用场景一般是渲染Web页面。
- TextView系统自带的组件，利用Span来实现图文混排，小巧方便，应用场景文字聊天，简单文本穿插、替换等。

TextView方式渲染简介：

 默认TextView只能解析常用标签 [br,p,div,strong,b,em,cite,dfn,i,big,small,font,blockquote,tt,a,u,sup,sub,h1-6,img],如果想处理格外标签，可自定义TagHandler处理，一般特别发杂的直接就采用其他方式来处理了，所以此处仅仅介绍img标签配合ImageGetter的使用。

```java
//设置文本
TextView.setText(Html.fromHtml(source,imageGetter,tagHandler));

//方法参数
public static Spanned fromHtml(
  String source,//文本源
  ImageGetter imageGetter,//图片获取器
  TagHandler tagHandler//特殊标签处理器
  )

//简单定义一个ImageGetter
 Html.ImageGetter() {
    @Override
    public Drawable getDrawable(String source) {
      //source为图片的地址，需要返回一个Drawable
      Drawable drawable = convert(source);
      drawable.setBounds(0,0,drawable.getIntrinsicWidth(),drawable.getIntrinsicHeight());
      return drawable;
    }
}
```

注意：
- 如果是异步获取图片，返回前还需要调用TextView的invalidate来重绘。
- 如果提前不知道图片大小，可以重新调用setText自动刷新，避免图文重叠现象。

附录两个处理器的定义：

```java
/**
    * Retrieves images for HTML &lt;img&gt; tags.
    */
   public static interface ImageGetter {
       /**
        * This method is called when the HTML parser encounters an
        * &lt;img&gt; tag.  The <code>source</code> argument is the
        * string from the "src" attribute; the return value should be
        * a Drawable representation of the image or <code>null</code>
        * for a generic replacement image.  Make sure you call
        * setBounds() on your Drawable if it doesn't already have
        * its bounds set.
        */
       public Drawable getDrawable(String source);
   }

   /**
    * Is notified when HTML tags are encountered that the parser does
    * not know how to interpret.
    */
   public static interface TagHandler {
       /**
        * This method will be called whenn the HTML parser encounters
        * a tag that it does not know how to interpret.
        */
       public void handleTag(boolean opening, String tag,
                                Editable output, XMLReader xmlReader);
   }
```
