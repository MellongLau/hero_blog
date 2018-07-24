---
title: Swift中如何把UILabel数字的颜色设置为红色
date: 2017-03-12 08:39:58
tags: Swift
---

这篇文章源于群友的一个问题：如何把**『注：此商品只能整件(12的倍数发货),已选1袋,还差11袋』**这段文字中的数字使用红色在 **UILabel** 中显示？

大概思路是：我们可以使用**UILabel** 的 **attribute string** 属性，通过正则表达式匹配获取数字的范围，然后添加对应的 attribute。

<!-- more -->

下面是实现代码，使用 swift 3.0 编写：


```swift
//根据正则表达式改变文字颜色
func changeTextChange(regex: String, text: String, color: UIColor) -> NSMutableAttributedString
{
    let attributeString = NSMutableAttributedString(string: text)

    do {
        let regexExpression = try NSRegularExpression(pattern: regex, options: NSRegularExpression.Options())
        let result = regexExpression.matches(in: text, options: NSRegularExpression.MatchingOptions(), range: NSMakeRange(0, text.characters.count))
        for item in result {
            attributeString.addAttribute(NSForegroundColorAttributeName, value: color, range: item.range)
        }
    } catch {
        print("Failed with error: \(error)")
    }

    return attributeString
}


let text = "注：此商品只能整件(12的倍数发货),已选1袋,还差11袋"
let renderLabel = UILabel(frame: CGRect(x: 0, y: 0, width: 800, height: 30))
renderLabel.textAlignment = NSTextAlignment.center
renderLabel.backgroundColor = UIColor.lightGray
renderLabel.font = UIFont.boldSystemFont(ofSize: 20)
renderLabel.attributedText = changeTextChange(regex: "\\d+", text: text, color: UIColor.red)
```

可以把以上这段代码放到 playground 里面运行。

当然，这里可以不使用正则表达式，用其他方法也可以做到，但是正则表达式的做法比较灵活，以后如果有新的需求可以直接修改正则表达式就可以实现。