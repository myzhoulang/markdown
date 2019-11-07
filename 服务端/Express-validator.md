# Express-validator

## 校验

* check([field, message]) 从body、Cookie、headers、param、query 中找字段

* body([field, message]) 从body 中找

* cookie([field, message])从cookie 中找

* headers([field, message])从headers 中找

* param([field, message])从param 中找

* query([field, message])从query 中找

  

* 校验不能为空

  check('userName').not().isEmpty()

* 校验长度

  check('text').isLength({min: 5, max: 10})

* 校验是否是某一种类型

  check('weekday').isIn(['a', 'b'])

* 可选

  check('weekday').optional()

* 校验是否是邮箱

  check('email').isEmail()

* 是否是信用卡

  check('card').isCreditCard()

* 自定义错误信息

  1. check('email', 错误信息)

  2. check('email').withMessage(错误信息)

     

## 格式化数据

* 将邮件地址规范化

  check('email').isEmail().normalizeEmail()

* 转义特殊字符

  check('message').escape()

* 删除空格

  check('message').trim()

## 自定义验证器

* check('confirmPassword', '两次输入密码不一致' ).custom()(value, {req}) => (value === req.body.password)))