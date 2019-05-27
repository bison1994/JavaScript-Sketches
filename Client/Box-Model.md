### What
- 盒模型是关于元素结构大小、定位布局和层叠的一系列规则
- box 分类
  + block
  + inline
  + inline block
  + anonymous
- box-sizing
  + content-box 标准盒模型
  + border-box IE 盒模型

### Positioning schemes
- normal flow
  + Block formatting context
  + Inline formatting contexts
  + Relative positioning
- float
- absolute positioning

### Visual Formatting Context
- 格式化上下文是一块独立的渲染区域，区域内的元素根据相应规则排列，与外部元素互不干扰

### Block Formatting Context

- 创建条件
  + 根元素
  + 浮动 (元素的 float 不为 none)
  + 绝对定位元素 (position 为 absolute 或 fixed)
  + overflow 的值不为 visible 的元素
  + block containers (display: inline-block | table-cell | table-caption | flex | inline-flex)
  + ...
- 基本规则
  + margin-collapse
  + 计算 BFC 的高度时，浮动子元素也参与计算
- 应用（[参考](http://lucybain.com/blog/2015/css-block-formatting-context/)）
  + prevent margin-collapse
  + contain floats，使得包含块获得高度
  + restrain floats，使得行内元素独立展示

> In a block formatting context, each box’s left outer edge touches the left edge of the containing block (for right-to-left formatting, right edges touch). This is true even in the presence of floats (although a box’s line boxes may shrink due to the floats), unless the box establishes a new block formatting context (in which case the box itself may become narrower due to the floats).

> [Visual formatting model](https://www.w3.org/TR/CSS2/visuren.html)