[BootstrapValidator](#bootstrapvalidator)
  - [官方](#官方)
  - [引入](#引入)
  - [html](#html)
  - [验证](#验证)
  - [自定义验证](#自定义验证)
  - [常用事件](#常用事件)
  - [表单提交](#表单提交)

# BootstrapValidator

## 官方

- [官网](http://1000hz.github.io/bootstrap-validator/)
- [下载](https://github.com/nghuuphuoc/bootstrapvalidator/archive/v0.4.5.zip)

## 引入

```html
<link rel="stylesheet" href="./static/css/bootstrap.css">
<link rel="stylesheet" href="./static/css/bootstrapValidator.css">

<script src="./static/js/jquery-1.10.2.min.js"></script>
<script src="./static/js/bootstrap.js"></script>
<script src="./static/js/bootstrapValidator.js"></script>
```

## html

在表单中，若对某一字段想添加验证规则，默认需要以`<div class=”form-group”></div>`包裹
（对应错误提示会根据该class值定位），内部`<input class="form-control" />`标签必须有name属性值，
此值为验证匹配字段。

```html
<form class="form-horizontal">
    <div class="form-group">
        <label class="col-lg-3 control-label">Username</label>
        <div class="col-lg-9">
            <input type="text" class="form-control" name="username" />
        </div>
    </div>
    <div class="form-group">
        <label class="col-lg-3 control-label">Email address</label>
        <div class="col-lg-9">
            <input type="text" class="form-control" name="email" />
        </div>
    </div>
</form>
```

## 验证

### 在 HTML 上

```html
<div class="form-group">
    <label class="col-lg-3 control-label">Username</label>
    <div class="col-lg-5">
        <input type="text" class="form-control" name="username"
            data-bv-message="The username is not valid"
            required
            data-bv-notempty-message="The username is required and cannot be empty"
            pattern="[a-zA-Z0-9]+"
            data-bv-regexp-message="The username can only consist of alphabetical, number" />
    </div>
</div>
```

### 在 JavaScript 上

```js
$(function () {
    $('#form-default').bootstrapValidator({
        /**
         * 指定不验证的情况, 值可设置为以下三种类型:
         * 1、String ':disabled, :hidden, :not(:visible)'
         * 2、Array 默认值 [':disabled', ':hidden', ':not(:visible)']
         * 3、带回调函数
         * [':disabled', ':hidden', function($field, validator) {
         *      // $field 当前验证字段的 dom 节点
         *      // validator 验证实例对象
         *      // 可以再次自定义不要验证的规则
         *      // 必须要 return, return true or false
         *      return !$field.is(':visible');
         * }]
         */
        excluded: [':disabled', ':hidden', ':not(:visible)'],

        /**
         * 指定验证后验证字段的提示字体图标. (默认是 bootstrap 风格)
         * Bootstrap version >= 3.1.0
         * 也可以自定义风格, 只要引入好相关的字体文件即可
         * 
         * 默认样式
         * .form-horizontal .has-feedback .form-control-feedback {
         *      top: 0;
         *      right: 15px;
         * }
         * 
         * 自定义该样式覆盖默认样式
         * .form-horizontal .has-feedback .form-control-feedback {
         *      top: 0;
         *      right: -15px;
         * }
         * .form-horizontal .has-feedback .input-group .form-control-feedback {
         *      top: 0;
         *      right: -30px;
         * }
         */
        feedbackIcons: {
            valid: 'glyphicon glyphicon-ok',
            invalid: 'glyphicon glyphicon-remove',
            validating: 'glyphicon glyphicon-refresh'
        },

        /**
         * 生效规则 (三选一)
         * enabled 字段值有变化就触发验证
         * disabled, submitted 当点击提交验证并展示错误信息
         */
        live: 'enabled',

        /**
         * 为每个字段指定通用的错误提示语 
         */
        message: 'This value is not valid',

        /**
         * 指定提交的按钮, 例如: '.btn-submit' '#btn-submit'
         * 当表单验证不通过时, 该按钮为 disabled
         */
        submitButtons: 'button[type="submit"]',

        /**
         * submitHandler: function (validator, $form, $submitButton) {
         *      // validator: 表单验证实例对象
         *      // $form jQuery 对象, 指定的表单对象
         *      // $submitButton jQuery 对象 指定的提交按钮对象
         *      $.post($form.attr('action'), form.serialize(), function (result) {
         *          // 自定义回调函数
         *      }, 'json')
         * }
         * 在 ajax 提交表单时很实用
         */
        submitHandler: null,

        /**
         * 为每个字段设置统一触发验证方式, 也可以在 fields 中为每个字段单独定义, 
         * 默认是 live 配置的方式, 数据改变就验证, 也可以指定一个或多个,
         * 多个空格隔开 'focus blur keyup'
         */
        trigger: null,

        /**
         * number 类型, 为每个字段设置统一的开始验证情况, 只有当输入字符大于等于该值时才验证
         */
        threshold: null,

        /**
         * 表单域配置
         */
        fields: {
            /**
             * input 标签中的 name 属性值, 如上面的 firstName lastName account password
             */
            firstName: {
                // 隐藏或显示该字段的验证
                enbled: true,

                // 错误提示信息
                message: 'This value is not valid',

                // 定义错误提示位置, 值为 CSS 选择器设置方式
                // 例如: '#firstNameMsg' '.lastNameMsg' '[data-stripe="exp-month"]'
                container: null,

                // 定义验证的节点, CSS 选择器设置方式, 可不必须是 name 值
                // 若是 id, class, name 属性, fieldName 为该属性值
                // 若是其他属性值且有中划线链接，<fieldName>转换为驼峰格式  selector: '[data-stripe="exp-month"]' =>  expMonth
                selector: null,

                // 定义触发验证方式（也可在fields中为每个字段单独定义），默认是live配置的方式，数据改变就改变
                // 也可以指定一个或多个（多个空格隔开） 'focus blur keyup'
                trigger: null,

                // 定义验证规则
                validators: {
                    notEmpty: {
                        message: 'This value is not empty'
                    },
                    
                    stringLength: {
                        min: 6,
                        max: 30,
                        message: 'The firstName must be more than 6 and less than 30 characters long'
                    },
                    
                    regexp: {
                        regexp: /^[0-9A-Za-z]$/,
                        message: 'The firstname can only consist of alphabetical, number'
                    },
                    
                    // 与服务器通信验证
                    // {valid:true/false}
                    remote: {
                        url: '/valid.jsp',
                        message: 'The firstname is not available'
                    },
                    
                    // 如果相同报错
                    different: {
                        field: 'lastName',
                        message: 'The firstName and lastName cannot be the same as each other'
                    },
                    
                    // 如果不相同报错
                    identical: {
                        field: '',
                        message: ''
                    }
                }
            }
        }
    }).on('success.form.bv', function (e) {
        // Prevent form submission
        e.preventDefault();

        // get the form instance
        var $form = $(e.target);

        // get the bootstrap validator instance
        var bv = $form.data('bootstrapValidator');

        // use ajax to submit form data
        $.post($form.attr('action'), $form.serialize(), function (result) {
            console.log(result);
        }, 'json');
    });
})
```

## 自定义验证

该规则是拓展插件的 validators 方法。 
将项目中常用的方法放到了一个单独 js 文件中，也就是上面第一步引用的自定义方法。

```js
(function($) {
    //自定义表单验证规则
    $.fn.bootstrapValidator.validators = {
        <validatorName> : {
            /**
             * @param {BootstrapValidator} 表单验证实例对象
             * @param {jQuery} $field jQuery 对象
             * @param {Object} 表单验证配置项值
             * @returns {boolean}
             */
            validate: function(validator, $field, options) {
                // 表单输入的值
                // var value = $field.val();

                //options为<validatorOptions>对象，直接.获取需要的值

                // 返回true/false
                //也可返回{ valid : true/false, message: 'XXXX'}
                return reg.test( $field.val() );

            }
        },
    };
}(window.jQuery));
```

## 常用事件

### 重置某一单一验证字段验证规则

```js
$(formName).data(“bootstrapValidator”).updateStatus("fieldName",  "NOT_VALIDATED",  null );
```

### 重置表单所有验证规则

```js
$(formName).data("bootstrapValidator").resetForm();
```

### 手动触发表单验证

```js
//触发全部验证
$(formName).data(“bootstrapValidator”).validate();
//触发指定字段的验证
$(formName).data(“bootstrapValidator”).validateField('fieldName');
```

### 获取当前表单验证状态

```js
// flag = true/false 
var flag = $(formName).data(“bootstrapValidator”).isValid();
```

### 根据指定字段名称获取验证对象

```js
// element = jq对象 / null
var element = $(formName).data(“bootstrapValidator”).getFieldElements('fieldName');
```

## 表单提交

### 当提交按钮是普通按钮

手动触发表单验证 

```js
 $("buttonName").on("click", function(){
     //获取表单对象
    var bootstrapValidator = form.data('bootstrapValidator');
        //手动触发验证
        bootstrapValidator.validate();
        if(bootstrapValidator.isValid()){
            //表单提交的方法、比如ajax提交
        }
});
```

### 当提交按钮的[type=”submit”]时 

会在success之前自动触发表单验证

```
var bootstrapValidator = form.data('bootstrapValidator');
bootstrapValidator.on('success.form.bv', function (e) {
    e.preventDefault();
    //提交逻辑
});
```
