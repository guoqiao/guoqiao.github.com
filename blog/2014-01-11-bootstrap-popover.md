how to use bootstrap popover
2014-1-11

## 基本使用

我使用的是 Bootstrap 2.3.2, 这个版本里, bootstrap-tooltip.js 以及 bootstrap-popover.js 已经都编译到 bootstrap.js 单个文件中, 不需要再单独加载.

js 文件包含: 

    ...
    <script src="/path/to/jquery.js"></script>
    <script src="/path/to/bootstrap.js"></script>
    ...

html 代码:

    <li class="pop-over" 
        rel="popover"
        data-original-title="this is popover title" 
        data-content="this is popover content">
        ....
    </li>

js 代码:

    $('.pop-over').popover();

注意类名不要使用 popover, 因为 bootstrap 使用了这个类. 这会导致你的 popover 始终弹不出. 我刚开始就犯了这个错误,因为这个疑惑了好半天:-(
popover 可以作用任何元素上, 通常是 a, button 之类, 我这里以 li 为例. 你可以认为它是一个矩形卡片区域, 例如 [readfree](http://readfree.me)首页的书籍卡片.
有了这些代码, 点击下 li 的任意空白位置, 应该就可以弹出 popover 了. 再次点击则关闭.

## 触发方式
popover 默认的触发方式是 click. 有时候, 你希望改变触发方式, 例如鼠标悬停弹出(hover), 这时候就可以使用 data-trigger 属性.

你可以通过 html 属性来指定:

    <li class="pop-over" 
        rel="popover" data-trigger="hover" 
        data-original-title="this is popover title" 
        data-content="this is popover content">
        ....
    </li>

也可以通过 js 初始化参数指定:

    $('.pop-over').popover({data-trigger: 'hover'});

data-trigger 可选项包括: click | hover | focus | manual

## 关闭方式
当 data-trigger="click" 时, 关闭 popover 的方法是再次 click. 这很自然.
但是, 当触发方式是 hover 时, 关闭方式依然是 click. 这时就有点...不一致. 试想:用户可能无意中将鼠标悬停在某个卡片上, 这时他发现弹出了 popover. 很自然的, 他可能没有意识到要点击关闭它. 当他 立即好奇的移开鼠标到别的卡片上, 前一个 popover 仍然显示在那里. 如果他用鼠标依次移过一系列卡片, 页面上玲琅满目的 popover 就很壮观了:0

很显然, 此时应该是鼠标离开卡片就自动关闭 popover 才比较合理, 很遗憾 bootstrap 没有自动给你做这件事.
你可以用下面的代码实现:

    $('.pop-over').on('mouseleave', function(){
        $(this).popover('hide');
    });

## 弹出位置
popover  通过 placement 参数来指定弹出的位置, 可选项为: top | bottom | left | right, 默认right.
当 popover 作用在最右侧的元素上时, 弹出界面就有点尴尬: 会很窄, 因为右侧没有空间了, 只能往下挤成一长条.
对于最右下角的元素, 下面也没有空间了, 此时甚至会影响页面的布局.

你可能很快想到,可以针对不同位置的元素指定不同的 placement. 
对于固定式的页面,这没有问题,因为每个元素的位置基本固定. 但是, 如果你使用了响应式设计的浮动式布局, 哪个元素会出现在边缘上,就不确定了, 取决于屏幕大小, 方向.

如果有一个 auto 选项该多好呢? 自动调整显示位置, 最左边的元素显示在右边, 最右边的则相反.顶部和底部也一样.
仔细看了下, 很遗憾, 没有这个选项. 但是好消息是, 我们可以给 placement 指定一个函数, 来自动计算它的值:


    $('.pop-over').popover({
        placement: function(tip, element) {
                var offset = $(element).offset();
                height = $(document).outerHeight();
                width = $(document).outerWidth();
                vert = 0.5 * height - offset.top;
                vertPlacement = vert > 0 ? 'bottom' : 'top';
                horiz = 0.5 * width - offset.left;
                horizPlacement = horiz > 0 ? 'right' : 'left';
                placement = Math.abs(horiz) > Math.abs(vert) ?  horizPlacement : vertPlacement;
                return placement;
            }
    });

具体效果可以到 [readfree](http://readfree.me) 去查看. 
考虑到 hover 方式会打扰用户, 我的弹出方式采用的是点击, 而鼠标离开时则会自动关闭.
你可以在不同边界的卡片上分别点击体验下.

## 界面定制
popover 的默认界面是比较简单的, 和 modal 差不多. 怎样定制这个界面呢? 你可以添加类似如下的 CSS 样式:

    .popover-title {
        color: blue;
        font-size: 15px;
    }

    .popover-content {
        color: red;
        font-size: 10px;
    }

## 使用总结
由于对前端不是很熟悉,我第一次使用 popover 时, 因为前面提到的种种问题很有挫败感.
例如使用了 popover 作为类名导致代码显示不正常, 例如以为默认是鼠标悬停就显示, 例如发现要点一下才能关闭, 之类.
今天再次使用, 终于成功. 作为一个前段菜鸟, 特地写下这篇 blog 记录下.

希望能帮助到同样被困扰的开发者.



