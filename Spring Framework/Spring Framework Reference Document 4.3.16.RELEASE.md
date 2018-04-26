## ApplicationContext的附加功能 ##

### 7.15.1 使用MessageSource国际化 ###

`ApplicationContext` 接口继承自一个叫`MessageSource`的接口，因此提供了国际化(i18n)的功能。

- `String getMessage(String code, Object[] args, String default, Local loc)`：从`MessageSource`中检索一条消息的基本方法。如果指定地方没有找到消息，就使用默认消息。任何传过来的参数，都将使用标准库提供的`MessageFormat`功能变成替换值。

- `String getMessage(String code, Object[] args, Locale loc)`：基本和上一个方法一样，但有一点不同：没有指定默认消息，如果没有找到消息，抛出一个`NoSuchMessageException`。

- `String getMessage(MessageSourceResolvable resolvable, Locale locale)`：前面方法使用的所有特性也被封装在一个叫`MessageSourceResolvable`的类中，你可以使用此方法使用。


