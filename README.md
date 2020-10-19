# react-calendar-timeline的中文文档翻译

来自google翻译，主要是为了方便自己了解api，大神勿喷

[react-calendar-timeline](https://www.npmjs.com/package/react-calendar-timeline#timeline-headers)

Version
0.27.0


# React Calendar Timeline

A modern and responsive React timeline component.

![calendar demo](https://raw.githubusercontent.com/namespace-ee/react-calendar-timeline/master/demo.gif)

Checkout the [examples here](https://github.com/namespace-ee/react-calendar-timeline/tree/master/examples)!

# 内容

- [入门](#入门)
- [用法](#用法)
- [API](#api)
- [Timeline Markers](#timeline-markers)
- [Timeline Headers](#timeline-headers)
- [FAQ](#faq)
- [Contribute](#contribute)

# 入门

```bash
# via yarn
yarn add react-calendar-timeline

# via npm
npm install --save react-calendar-timeline
```

`react-calendar-timeline` 有 [react](https://reactjs.org/), [react-dom](https://reactjs.org/docs/react-dom.html), [`moment`](http://momentjs.com/) and [`interactjs`](http://interactjs.io/docs/) 依赖.

# 用法

简单使用:

```jsx
import Timeline from 'react-calendar-timeline'
// make sure you include the timeline stylesheet or the timeline will not be styled
import 'react-calendar-timeline/lib/Timeline.css'
import moment from 'moment'

const groups = [{ id: 1, title: 'group 1' }, { id: 2, title: 'group 2' }]

const items = [
  {
    id: 1,
    group: 1,
    title: 'item 1',
    start_time: moment(),
    end_time: moment().add(1, 'hour')
  },
  {
    id: 2,
    group: 2,
    title: 'item 2',
    start_time: moment().add(-0.5, 'hour'),
    end_time: moment().add(0.5, 'hour')
  },
  {
    id: 3,
    group: 1,
    title: 'item 3',
    start_time: moment().add(2, 'hour'),
    end_time: moment().add(3, 'hour')
  }
]

ReactDOM.render(
  <div>
    Rendered by react!
    <Timeline
      groups={groups}
      items={items}
      defaultTimeStart={moment().add(-12, 'hour')}
      defaultTimeEnd={moment().add(12, 'hour')}
    />
  </div>,
  document.getElementById('root')
)
```

# API

_注意!_ 所有道具都必须是不变的。 例如，这意味着如果您想更改其中一项的标题，请传入一个全新的项目数组，而不是在旧数组中更改标题. [更多信息](http://reactkungfu.com/2015/08/pros-and-cons-of-using-immutability-with-react-js/)

该组件可以带很多参数:

## groups

该参数应该是一个vanilla JS array 或者是一个 immutableJS array，
由具有以下属性的对象组成:

```js
{
  id: 1,
  title: 'group 1',
  rightTitle: 'title in the right sidebar',
  stackItems?: true,
  height?: 30
}
```

如果你要使用右侧栏，则可以在此处传递可选的`rightTitle`属性。

如果您想用自定义高度覆盖计算出的高度，则可以在此处传递一个`height`属性设置高度像素。 这对于分类的组非常有用。

## items

期望使用包含以下属性的对象组成的普通JS数组或immutableJS数组：

```js
{
  id: 1,
  group: 1,
  title: 'Random title',
  start_time: 1457902922261,
  end_time: 1457902922261 + 86400000,
  canMove: true,
  canResize: false,
  canChangeGroup: false,
  itemProps: {
    // these optional attributes are passed to the root <div /> of each item as <div {...itemProps} />
    'data-custom-attribute': 'Random content',
    'aria-hidden': true,
    onDoubleClick: () => { console.log('You clicked double!') },
    className: 'weekend',
    style: {
      background: 'fuchsia'
    }
  }
}
```


首选的（响应最快的）选项是为start_time和end_time提供Unix时间戳（以毫秒为单位）。 start_time和end_time转换为（JavaScript`Date`或`moment（）`）也可以使用，但是页面相应会慢很多。

## defaultTimeStart and defaultTimeEnd

除非被“ visibleTimeStart”和“ visibleTimeEnd”覆盖，否则请指定日历的开始位置和结束位置。 此参数需要一个Date或moment对象。

## visibleTimeStart and visibleTimeEnd

日历的确切视口。 当指定了这些选项时，必须通过onTimeChange函数来安排日历的滚动。 此参数需要Unix时间戳（以毫秒为单位）。

**请注意，您需要提供“ defaultTimeStart / End”或“ visibleTimeStart / End”以确保时间轴能正常运行**

## selected

ID对应于项目中ID的数组（“ item.id”）。 如果设置了此属性，则必须在onItemSelect处理程序中自行管理所选项目，以使用新ID更新属性，并使用onItemDeselect处理程序清除选择。 这将覆盖单击时选择一项的默认行为

## keys

An array specifying keys in the `items` and `groups` objects. Defaults to

一个数组中用于默认指定“ items”和“ groups”对象中的键。

```js
{
  groupIdKey: 'id',
  groupTitleKey: 'title',
  groupRightTitleKey: 'rightTitle',
  itemIdKey: 'id',
  itemTitleKey: 'title',    // key for item div content
  itemDivTitleKey: 'title', // key for item div title (<div title="text"/>)
  itemGroupKey: 'group',
  itemTimeStartKey: 'start_time',
  itemTimeEndKey: 'end_time',
}
```

## className

用于设置Timeline根元素的className。

## sidebarWidth

边栏的宽度（以像素为单位）。 如果设置为“ 0”，则不渲染边栏。 默认为150。

## sidebarContent

此处传递的所有内容都将显示在左侧边栏上方。 默认为null。

## rightSidebarWidth

右侧边栏的宽度（以像素为单位）。 如果设置为“ 0”，则不会渲染右侧边栏。 默认为0。

## rightSidebarContent

此处传递的所有内容都将显示在右侧栏上方。 用它来显示小的过滤器。 默认为null。

## dragSnap

拖动项目时捕捉单元。 默认值为“ 15 * 60 * 1000”或15分钟。 这样做时，拖动时项目将间隔15分钟。

## minResizeWidth

可以调整大小的时间线条目的最小宽度（以像素为单位）。 如果未达到，则必须放大以调整更多尺寸。 默认为20。

## lineHeight

日历中一行的高度（以像素为单位）。 默认值30

## itemHeightRatio

item占线的百分比是多少？ 默认值`0.65`

## minZoom

日历可以放大的最短时间（以毫秒为单位）。 默认值`60 * 60 * 1000`（1小时）

## maxZoom

日历可以放大的最长时间（以毫秒为单位）。 默认`5 * 365.24 * 86400 * 1000`（5年）

## clickTolerance

我们可以拖动背景多少像素，以将其算作对背景的点击。 默认值3

## canMove

可以拖动item吗？ 可以在`items`数组中覆盖。 默认为true

## canChangeGroup

可以在groups之间移动item吗？ 可以在`items`数组中覆盖。 默认为true

## canResize

可以调整大小吗？ 可以在`items`数组中覆盖。 接受的值：`“false”`，`“left”`，`“right”`，`“both”`。 默认为`“ right”`。 如果您通过`true`，它将被视为`"right"`，以免破坏与0.9及更低版本的兼容性。

## useResizeHandle

在元素上附加一个特殊的`.rct-drag-right`手柄，仅在从那里拖动时才调整大小。 默认为`false`

### stackItems

将项目相互堆叠，因此时间冲突时不会出现视觉重叠。 可以在`groups`数组中覆盖。 默认为`false`。 需要毫秒或“ Moment”时间戳，而不是原生JavaScript“ Date”对象。

## traditionalZoom

向上/向下 滚动鼠标时放大。 默认为`false`

## itemTouchSendsClick

通常，点击（触摸）一个项目即可将其选中。 如果将其设置为true，则点击将具有与第一次单击选择然后再次单击以打开并发送onItemClick事件相同的效果。 默认为`false`。

## timeSteps

用什么step显示不同的units。 例如。 `minute` `15` 表示仅显示0、15、30和45分钟。

Default:

```js
{
  second: 1,
  minute: 1,
  hour: 1,
  day: 1,
  month: 1,
  year: 1
}
```

## scrollRef

Ref回调，它获取对滚动体元素的DOM引用。 对以编程方式滚动很有用。


## onItemDrag(itemDragObject)

当项目正在移动或调整大小时调用。返回具有以下属性的对象：

| property           | type     | description                                                            |
| ------------------ | -------- | ---------------------------------------------------------------------- |
| `eventType`        | `string` | 返回 `move` 或者`resize`   |
| `itemId`           | `number` | 正在移动或调整大小的项目的ID  |
| `time`             | `number` | UNIX时间戳（毫秒）  |
| `edge`             | `string` | 当 `“resize”` 时，返回 `“left”` 或  `“right”` 值`
| `newGroupOrder`    | `number` | 当“move”时，该项要移动到的新组的索引位置  |


## onItemMove(itemId, dragTime, newGroupOrder)

移动项目时回调。返回
  - item的ID
  - 新的开始时间
  - groups数组中新组的索引。

## onItemResize(itemId, time, edge)

调整项目大小时回调。返回
 - item的ID
 - item的新开始或结束时间
 - 拖动的edge（`left`或`right`）

## onItemSelect(itemId, e, time)

在选择项目时调用。这是在第一次单击项目时发送的。`time`是与您在时间线中单击/选择项目的位置相对应的时间。

## onItemDeselect(e)

取消选择项目时调用。用于清除控制选定的道具。

## onItemClick(itemId, e, time)

在点击item时调用。注意：在单击该项之前必须选择它。。。除非是触摸事件并且启用了“itemTouchSendsClick”。`time`是与您在时间线中单击该项的位置相对应的时间。

## onItemDoubleClick(itemId, e, time)

双击项目时调用。`time`是与双击时间线中的项目的位置相对应的时间

## onItemContextMenu(itemId, e, time)

当鼠标右键单击项目时调用。`time`是与您在时间线中单击该项的位置相对应的时间。注意：如果设置了此属性，则不会显示默认上下文菜单。

## onCanvasClick(groupId, time, e)

当单击画布上的空白点时调用。获取组ID和时间作为参数。例如，在此之后打开一个“new item”窗口。

## onCanvasDoubleClick(groupId, time, e)

双击画布上的空白点时调用。获取组ID和时间作为参数。

## onCanvasContextMenu(groupId, time, e)

当鼠标右键单击画布时调用。注意：如果设置了此属性，则不会显示默认上下文菜单

## onZoom(timelineContext)

在缩放时间线时调用，通过鼠标/收缩缩放或单击标题更改时间线单位

## moveResizeValidator(action, itemId, time, resizeEdge)

This function is called when an item is being moved or resized. It's up to this function to return a new version of `change`, when the proposed move would violate business logic.

在移动或调整项目大小时调用此函数。当提议的移动违反业务逻辑时，由此函数返回“change”的新版本。

参数 `action` 是 `move` 或 `resize`中的一个


参数`“resizeEdge”` 在调整“left”或“right”的大小返回。

参数“time”描述项目的开始时间（用于移动）或开始或结束时间（用于调整大小）的建议新时间。

函数必须以毫秒为单位返回新的unix时间戳。。。或者，如果提议的新时间不干扰业务逻辑，则只需“time”。

例如，要防止项目移到过去，但要保持15分钟的间隔，请使用以下代码：

```js
function (action, item, time, resizeEdge) {
  if (time < new Date().getTime()) {
    var newTime = Math.ceil(new Date().getTime() / (15*60*1000)) * (15*60*1000);
    return newTime;
  }

  return time
}
```


## onTimeChange(visibleTimeStart, visibleTimeEnd, updateScrollCanvas)

当用户试图滚动时的回调函数。使用更新的visibleTimeStart和visibleTimeEnd调用传递的`“updateScrollCanvas（start，end）”`，更改滚动行为，例如限制滚动。


下面是一个示例，它将时间线限制为只显示从现在起6个月到6个月结束的日期。

```js
// this limits the timeline to -6 months ... +6 months
const minTime = moment().add(-6, 'months').valueOf()
const maxTime = moment().add(6, 'months').valueOf()

function (visibleTimeStart, visibleTimeEnd, updateScrollCanvas) {
  if (visibleTimeStart < minTime && visibleTimeEnd > maxTime) {
    updateScrollCanvas(minTime, maxTime)
  } else if (visibleTimeStart < minTime) {
    updateScrollCanvas(minTime, minTime + (visibleTimeEnd - visibleTimeStart))
  } else if (visibleTimeEnd > maxTime) {
    updateScrollCanvas(maxTime - (visibleTimeEnd - visibleTimeStart), maxTime)
  } else {
    updateScrollCanvas(visibleTimeStart, visibleTimeEnd)
  }
}
```

## onBoundsChange(canvasTimeStart, canvasTimeEnd)

当日历画布中的边界更改时调用。例如，可以使用它来加载要显示的新数据。（见下文“Behind the scenes”）。`canvatimestart`和`'canvatimeend'`是以毫秒为单位的unix时间戳。

## itemRenderer

渲染属性函数用于渲染自定义项。函数提供了多个可用于呈现每个项的参数。

提供给函数的参数有两种类型：具有item和timeline状态的上下文参数和prop getters函数

#### Render props params

##### context

* `item` 将我们传递的item作为日历的参数。

* `timelineContext`

| property           | type     | description                                          |
| ------------------ | -------- | ---------------------------------------------------- |
| `timelineWidth`    | `number` | 返回timeline的宽度.              |
| `visibleTimeStart` | `number` | 返回日历视图端口的确切起始位置 |
| `visibleTimeEnd`   | `number` | 返回日历视图端口的确切结尾。  |
| `canvasTimeStart`  | `number` | 表示画布时间线的开始时间（毫秒）  |
| `canvasTimeEnd`    | `number` | 表示画布时间线的结束时间（毫秒）   |

* `itemContext`

| property          | type            | description                                                                                                                                                               |
| ----------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `dimensions`      | `object`        | 返回item的相关 的 `collisionLeft`, `collisionWidth`, `height`, `isDragging`, `left`, `order`, `originalLeft`, `stack`, `top`, and `width` |
| `useResizeHandle` | `boolean`       | 返回calendar根组件属性  `“useResizeHandle“`                                                                                               |
| `title`           | `string`        | 返回要在内容元素中呈现的标题。                                                                                                                        |
| `canMove`         | `boolean`       | 返回项是否可移动。                                                                                                             |
| `canResizeLeft`   | `boolean`       | 如果可以调整left大小，则返回该项                                                                                                  |
| `canResizeRight`  | `boolean`       | 返回项是否可以从右侧调整大小。                                                                                                                       |
| `selected`        | `boolean`       | 返回是否选中该项。                                                                                                          |
| `dragging`        | `boolean`       | 如果正在拖动项，则返回                                                   |
| `dragStart`       | `object`        | 返回项的开始拖动点的x和y。                                                                            |
| `dragTime`       | `number`        |  当前drag时间                                                                                                             |
| `dragGroupDelta`  | `number`        | returns number of groups the item moved. if negative, moving was to top. If positive, moving was to down                                                                  |
| `resizing`        | `boolean`       | 返回项目移动的组数。如果是负数，则移动到顶部。如果是肯定的，那就是向下移动                                                                                                      |
| `resizeEdge`      | `left`, `right` | 正在调整组件大小的一侧窗体                                                                                  |
| `resizeStart`     | `number`        | 返回组件开始移动的x值                |
| `resizeTime`     | `number`        | 当前调整大小时间                                                                              |
| `width`           | `boolean`       | 返回项的宽度（与维度中的宽度相同）                                                                                        |

##### prop getters functions

这些function用于将props应用于渲染的元素。这为您提供了最大的灵活性来渲染您喜欢的内容、时间和地点。

而不是自己在元素上应用props，以避免你的props被覆盖（或覆盖返回的props）。您可以将对象传递给prop getters以避免任何问题。此对象将只接受组件管理的某些属性，以便组件确保正确组合它们。


| property         | type                 | description                                                                                                                               |
| ---------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `getItemProps`   | `function(props={})` | 返回应应用于根项元素的属性。                                            |
| `getResizeProps` | `function(props={})` | 如果将“useResizeHandle”prop设置为true，则返回要应用于“left”和“right”元素的两组属性，作为调整大小的元素 |

* `getItemProps` 返回应用于根项元素的props。返回的props是:

  * key: item id
  * ref: function to get item reference
  * className: classnames to be applied to the item
  * onMouseDown: event handler
  * onMouseUp: event handler
  * onTouchStart: event handler
  * onTouchEnd: event handler
  * onDoubleClick: event handler
  * onContextMenu: event handler
  * style: inline object


  \*\* _给定样式将仅覆盖不需要定位项目的样式。其他样式，如`color`、`raduis`和其他样式_

  可以使用prop参数和properties重写这些属性:

  * className: class names to be added
  * onMouseDown: event handler will be called after the component's event handler
  * onMouseUp: event handler will be called after the component's event handler
  * onTouchStart: event handler will be called after the component's event handler
  * onTouchEnd: event handler will be called after the component's event handler
  * onDoubleClick: event handler will be called after the component's event handler
  * onContextMenu: event handler will be called after the component's event handler
  * style: extra inline styles to be applied to the component



`getResizeProps`当`useResizeHandle`设置为true时才返回应用于左右调整处理程序的props。返回的对象在props“left”下有left元素的属性，在“right”下有要应用于right元素的属性` :

  * left
    * ref: function to get element reference
    * style: style to be applied to the left element
    * className: class names to be applied to left className
  * right
    * ref: function to get element reference
    * style: style to be applied to the right element
    * className: class names to be applied to left className

These properties can be override using the prop argument with properties:

* leftStyle: style to be added to left style
* rightStyle: style to be added to right style
* leftClassName: classes to be added to left handler
* rightClassName: classes to be added to right handler

example

```jsx
let items = [
  {
    id: 1,
    group: 1,
    title: 'Title',
    tip: 'additional information',
    color: 'rgb(158, 14, 206)',
    selectedBgColor: 'rgba(225, 166, 244, 1)',
    bgColor : 'rgba(225, 166, 244, 0.6)',
    ...
  }
]

itemRenderer: ({
  item,
  itemContext,
  getItemProps,
  getResizeProps
}) => {
  const { left: leftResizeProps, right: rightResizeProps } = getResizeProps()
  return (
    <div {...getItemProps(item.itemProps)}>
      {itemContext.useResizeHandle ? <div {...leftResizeProps} /> : ''}

      <div
        className="rct-item-content"
        style={{ maxHeight: `${itemContext.dimensions.height}` }}
      >
        {itemContext.title}
      </div>

      {itemContext.useResizeHandle ? <div {...rightResizeProps} /> : ''}
    </div>
  )}

}
```

## groupRenderer

将用于呈现侧边栏。将作为道具传递给“group”和“isRightSidebar”。
反应组分工作渲染

```jsx
let groups = [
  {
    id: 1,
    title: 'Title',
    tip: 'additional information'
  }
]

groupRenderer = ({ group }) => {
  return (
    <div className="custom-group">
      <span className="title">{group.title}</span>
      <p className="tip">{group.tip}</p>
    </div>
  )
}
```

## resizeDetector

该组件会自动检测何时调整了窗口的大小。 （可选）您还可以检测何时调整了组件的DOM元素的大小.

为此，传递一个“ resizeDetector”。 由于默认情况下捆绑它会添加〜18kb的最小化JS，因此您需要像这样选择加入
:

```jsx
import containerResizeDetector from 'react-calendar-timeline/lib/resize-detector/container'

<Timeline resizeDetector={containerResizeDetector} ... />
```

## verticalLineClassNamesForTime(start, end)

渲染垂直线时将调用此函数。 “ start”和“ end”是当前列的unix时间戳（以毫秒为单位）。 该函数应返回包含应应用于列的className的字符串数组。 这使得可以在视觉上突出显示例如 公共假期或办公时间。
一个示例可能看起来像（请参阅：demo / vertical-classes）:

```jsx
verticalLineClassNamesForTime = (timeStart, timeEnd) => {
  const currentTimeStart = moment(timeStart)
  const currentTimeEnd = moment(timeEnd)

  for (let holiday of holidays) {
    if (
      holiday.isSame(currentTimeStart, 'day') &&
      holiday.isSame(currentTimeEnd, 'day')
    ) {
      return ['holiday']
    }
  }
}
```


请注意，此功能应尽可能优化性能，因为它将在时间轴的每个渲染上都将被调用（即，重置画布，缩放等时）

## horizontalLineClassNamesForGroup(group)

渲染水平线时调用此函数。 “ group”是将呈现到当前行中的组。 该函数应返回包含应应用于行的className的字符串数组。 这样可以在视觉上突出显示类别或重要项目。
一个例子可能看起来像:

```jsx
horizontalLineClassNamesForGroup={(group) => group.root ? ["row-root"] : []}
```

# Timeline Markers

时间轴标记是在画布上特定数据点上覆盖的标记

## Overview

通过将标记声明为“Timeline”组件的“children”，可以将其放置在时间轴中：

```jsx
import Timeline, {
  TimelineMarkers,
  CustomMarker,
  TodayMarker,
  CursorMarker
} from 'react-calendar-timeline'

<Timeline>
  <TimelineMarkers>
    <TodayMarker />
    <CustomMarker date={today} />
    <CustomMarker date={tomorrow}>
      {/* custom renderer for this marker */}
      {({ styles, date }) => {
        const customStyles = {
          ...styles,
          backgroundColor: 'deeppink',
          width: '4px'
        }
        return <div style={customStyles} onClick={someCustomHandler} />
      }}
    </CustomMarker>
    <CursorMarker />
  </TimelineMarkers>
</Timeline>
```

每个标记都允许通过函数作为子组件（ [function as a child component](https://medium.com/merrickchristensen/function-as-child-components-5f3920a9ace9)）传入自定义渲染器。. 这允许用户呈现他们想要的任何内容（事件处理程序、自定义样式等）。此自定义渲染器接收具有两个属性的对象：

> styles: {position: 'absolute', top: 0, bottom: 0, left: number}

此对象_必须_传递给根组件的“style”属性才能正确呈现。请注意，您可以将此对象与任何其他属性合并

> date: number

此标记的unix时间戳中的日期。这可以用来更改标记的渲染方式（或者如果它被渲染的话）

## TimelineMarkers

你想要呈现的时间线标记的warpper

## TodayMarker

放置在当前日期/时间上的标记。

> interval: number | default: 10000

How often the TodayMarker refreshes. Value represents milliseconds.

> children: function({styles: object, date: number}) => JSX.Element

此标记的自定义渲染器。确保始终将“styles”传递给根组件的“style”属性，因为此对象包含标记的位置

```jsx
// custom interval
const twoSeconds = 2000

<TodayMarker interval={twoSeconds} />

//custom renderer

<TodayMarker>
  {({ styles, date }) =>
    // date is value of current date. Use this to render special styles for the marker
    // or any other custom logic based on date:
    // e.g. styles = {...styles, backgroundColor: isDateInAfternoon(date) ? 'red' : 'limegreen'}
    <div style={styles} />
  }
</TodayMarker>
```

## CustomMarker

放置在当前日期/时间上的标记。

> date: number | required

在时间线上放置标记的位置。`date`值是unix时间戳。

> children: function({styles: object, date: number}) => JSX.Element

此标记的自定义渲染器。确保始终将“styles”传递给根组件的“style”属性，因为此对象包含标记的位置。


```jsx
const today = Date.now()
<CustomMarker date={today} />

//custom renderer
<CustomMarker date={today}>
  {({ styles, date }) => <div style={styles} />}
</CustomMarker>

// multiple CustomMarkers
const markerDates = [
  {date: today, id: 1,},
  {date: tomorrow, id: 2,},
  {date: nextFriday, id: 3,},
]

<TimelineMarkers>
  {markerDates.map(marker => <CustomMarker key={marker.date} date={marker.date}/> )}
</TimelineMarkers>
```

## CursorMarker

光标悬停在时间线上时显示的标记，与光标所在位置相匹配。

> children: function({styles: object, date: number}) => JSX.Element

此标记的自定义渲染器。确保始终将“styles”传递给根组件的“style”属性，因为此对象包含标记的位置。

```jsx
// render default marker for Cursor
<CursorMarker />

//custom renderer
<CursorMarker>
  {({ styles, date }) =>
    // date is value of current date. Use this to render special styles for the marker
    // or any other custom logic based on date:
    // e.g. styles = {...styles, backgroundColor: isDateInAfternoon(date) ? 'red' : 'limegreen'}
    <div style={styles} />
  }
</CursorMarker>
```

# Timeline Headers


时间线标题是时间线上方的部分，它包括两个主要部分：第一，日历头，它是一个包含日历日期的可折叠div，称为“DateHeader”。第二，是边栏的标题，称为“SidebarHeader”，左边的，也可以选择右边的

## Default usage

对于默认情况，在时间线上方呈现两个“DateHeader”，一个“primary”和“secondary”。次要事件的日期单位与时间线相同，而“主要”的日期单位比时间线单位大一倍。



对于“SidebarHeader”，左侧将呈现一个空的“SidebarHeader”，如果存在“rightSidebarWith”，则可以选择呈现空的右侧边栏标题。

## Overview

为“DateHeader”或“SidebarHeader”提供任何自定义标头。您需要提供基本用法才能提供任何自定义标头。这些自定义标头应始终包含在组件子级的“TimelineHeaders”组件中。

```jsx
import Timeline, {
  TimelineHeaders,
  SidebarHeader,
  DateHeader
} from 'react-calendar-timeline'

<Timeline>
  <TimelineHeaders>
    <SidebarHeader>
      {({ getRootProps }) => {
        return <div {...getRootProps()}>Left</div>
      }}
    </SidebarHeader>
    <DateHeader unit="primaryHeader" />
    <DateHeader />
  </TimelineHeaders>
<Timeline>
```
## Components

Custom headers are implemented through a set of component with mostly [function as a child component](https://medium.com/merrickchristensen/function-as-child-components-5f3920a9ace9) pattern, designed to give the user the most control on how to render the headers.
自定义标头是通过一组组件实现的，这些组件的主要功能是作为子组件模式的，旨在为用户提供有关如何呈现标头的最大控制权。

### `TimelineHeader`

自定义头部 的核心组件warpper组件

#### props

| Prop          | type            | description                                                                                                                                                               |
| ----------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `style`| `object`| applied to the root component of headers |
| `className` | `string`| applied to the root component of the headers|
| `calendarHeaderStyle`| `object`| applied to the root component of the calendar headers -scrolable div- `DateHeader` and `CustomHeader`)|
| `calendarHeaderClassName`| `string`| applied to the root component of the calendar headers -scrolable div- `DateHeader` and `CustomHeader`)|
| `headerRef` | `function` | used to get the ref of the header element

### `SidebarHeader`

负责呈现左右边栏上方的标题

#### props

| Prop          | type            | description                                                                                                                                                               |
| ----------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `variant`| `left` (default), `right`| renders above the left or right sidebar |
| `children` | `Function`| function as a child component to render the header|
| `headerData` | `any`|  Contextual data to be passed to the item renderer as a data prop |

#### Child function renderer

函数提供了多个可用于呈现侧栏标题的参数

##### Prop getters functions

Rather than applying props on the element yourself and to avoid your props being overridden (or overriding the props returned). You can pass an object to the prop getters to avoid any problems. This object will only accept some properties that our component manage so the component make sure to combine them correctly.

而不是自己在元素上应用道具，以避免你的道具被覆盖（或覆盖返回的道具）。您可以将对象传递给prop getters以避免任何问题。此对象将只接受组件管理的某些属性，以便组件确保正确组合它们。

| property         | type                 | description|
| ---------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `getRootProps`   | `function(props={})` | returns the props you should apply to the root div element.|
| `data`   | `any` | Contextual data passed by `headerData` prop|

* `getRootProps` The returned props are:

  * style: inline object style

  These properties can be override using the prop argument with properties:

  * style: extra inline styles to be applied to the component

#### example

```jsx
import Timeline, {
  TimelineHeaders,
  SidebarHeader,
  DateHeader
} from 'react-calendar-timeline'

<Timeline>
  <TimelineHeaders>
    <SidebarHeader>
      {({ getRootProps }) => {
        return <div {...getRootProps()}>Left</div>
      }}
    </SidebarHeader>
    <SidebarHeader variant="right" headerData={{someData: 'extra'}}>
      {({ getRootProps, data }) => {
        return <div {...getRootProps()}>Right {data.someData}</div>
      }}
    </SidebarHeader>
    <DateHeader unit="primaryHeader" />
    <DateHeader />
  </TimelineHeaders>
<Timeline>
```

_注意：为了方便起见，子函数呈现器可以是组件或函数

### `DateHeader`

负责呈现时间线日历部分上方的标题。由按列划分标题的时间间隔组成

#### props

| Prop          | type            | description|
| ----------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `style`| `object`| applied to the root of the header |
| `className` | `string`| applied to the root of the header|
| `unit`| `second`, `minute`, `hour`, `day`, `week`, `month`, `year` or `primaryHeader` | intervals between columns |
| `labelFormat` | `Function` or `string`| controls the how to format the interval label |
| `intervalRenderer`| `Function`| render prop to render each interval in the header
| `headerData` | `any`|  Contextual data to be passed to the item renderer as a data prop |
| `height` | `number` default (30)|  height of the header in pixels |


_注意_ ：将“primaryHeader”传递给unit标题将作为主标题，间隔单位比时间线单位大1倍

#### Interval unit

intervals are decided through the prop: `unit`. By default, the unit of the intervals will be the same the timeline.

If `primaryHeader` is passed to unit, it will override the unit with a unit a unit larger by 1 of the timeline unit.

If `unit` is set, the unit of the header will be the unit passed though the prop and can be any `unit of time` from `momentjs`.

#### Label format

To format each interval label you can use 2 types of props to format which are:

- `string`: if a string was passed it will be passed to `startTime` method `format` which is a `momentjs` object  .


- `Function`: This is the more powerful method and offers the most control over what is rendered. The returned `string` will be rendered inside the interval

  ```typescript
    type Unit = `second` | `minute` | `hour` | `day` | `month` | `year`
  ([startTime, endTime] : [Moment, Moment], unit: Unit, labelWidth: number, formatOptions: LabelFormat = defaultFormat ) => string
  ```
##### Default format

by default we provide a responsive format for the dates based on the label width. it follows the following rules:

  The `long`, `mediumLong`, `medium` and `short` will be be decided through the `labelWidth` value according to where it lays upon the following scale:

  ```
  |-----`short`-----50px-----`medium`-----100px-----`mediumLong`-----150px--------`long`-----
  ```


  ```typescript
  // default format object
  const format : LabelFormat = {
  year: {
    long: 'YYYY',
    mediumLong: 'YYYY',
    medium: 'YYYY',
    short: 'YY'
  },
  month: {
    long: 'MMMM YYYY',
    mediumLong: 'MMMM',
    medium: 'MMMM',
    short: 'MM/YY'
  },
  week: {
    long: 'w',
    mediumLong: 'w',
    medium: 'w',
    short: 'w'
  },
  day: {
    long: 'dddd, LL',
    mediumLong: 'dddd, LL',
    medium: 'dd D',
    short: 'D'
  },
  hour: {
    long: 'dddd, LL, HH:00',
    mediumLong: 'L, HH:00',
    medium: 'HH:00',
    short: 'HH'
  },
  minute: {
    long: 'HH:mm',
    mediumLong: 'HH:mm',
    medium: 'HH:mm',
    short: 'mm',
  }
}
  ```

_注意_：这只是函数param的一个实现。你自己可以很容易地做到这一点


#### intervalRenderer

用于渲染自定义间隔的Render prop函数。该函数提供了多个可用于渲染每个间隔的参数。



提供给函数的参数有两种类型：具有item和timeline状态的上下文参数和prop getters函数



_注意_：为了方便起见，renderProp可以是组件或函数

##### interval context

对象包含以下属性：

| property           | type     | description                                          |
| ------------------ | -------- | ---------------------------------------------------- |
| `interval`    | `object : {startTime, endTime, labelWidth, left}` | an object containing data related to the interval|
| `intervalText` | `string` | the string returned from `labelFormat` prop |


##### Prop getters functions

而不是自己在元素上应用道具，以避免你的道具被覆盖（或覆盖返回的道具）。您可以将对象传递给prop getters以避免任何问题。此对象将只接受组件管理的某些属性，以便组件确保正确组合它们。

| property         | type                 | description|
| ---------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `getIntervalProps`   | `function(props={})` | returns the props you should apply to the root div element.|

* `getIntervalProps` The returned props are:

  * style: inline object style
  * onClick: event handler
  * key

  These properties can be extended using the prop argument with properties:

  * style: extra inline styles to be applied to the component
  * onClick: extra click handler added to the normal `showPeriod callback`

##### data

data passed through headerData

#### example

```jsx
import Timeline, {
  TimelineHeaders,
  SidebarHeader,
  DateHeader
} from 'react-calendar-timeline'

<Timeline>
  <TimelineHeaders>
    <SidebarHeader>
      {({ getRootProps }) => {
        return <div {...getRootProps()}>Left</div>
      }}
    </SidebarHeader>
    <DateHeader unit="primaryHeader" />
    <DateHeader />
    <DateHeader
      unit="day"
      labelFormat="MM/DD"
      style={{ height: 50 }}
      data={{someData: 'example'}}
      intervalRenderer={({ getIntervalProps, intervalContext, data }) => {
        return <div {...getIntervalProps()}>
          {intervalContext.intervalText}
          {data.example}
        </div>
      }}
    />
  </TimelineHeaders>
</Timeline>
```

### `CustomHeader`

负责呈现时间线日历部分上方的标题。这是“DateHeader”的基本组件，提供了更多的控制和更少的功能。

#### props

| Prop          | type            | description|
| ----------------- | --------------- | ---|
| `unit`| `second`, `minute`, `hour`, `day`, `week`, `month`, `year` (default `timelineUnit`) | intervals |
| `children` | `Function`| function as a child component to render the header|
| `headerData` | `any`|  Contextual data to be passed to the item renderer as a data prop |
| `height` | `number` default (30)|  height of the header in pixels |

#### unit


标题的单位将是通过道具的单位，可以是“momentjs”中的任何“时间单位”。unit的默认值是`timelineUnit`

#### Children

Function as a child component to render the header

Paramters provided to the function has three types: context params which have the state of the item and timeline, prop getters functions and helper functions.

_Note_ : the Child function renderer can be a component or a function for convenience

```
({
  timelineContext: {
    timelineWidth,
    visibleTimeStart,
    visibleTimeEnd,
    canvasTimeStart,
    canvasTimeEnd
  },
  headerContext: {
    unit,
    intervals: this.state.intervals
  },
  getRootProps: this.getRootProps,
  getIntervalProps: this.getIntervalProps,
  showPeriod,
  //contextual data passed through headerData
  data,
})=> React.Node
```

##### context

An object contains context for `timeline` and `header`:


###### Timeline context

| property           | type     | description                                          |
| ------------------ | -------- | ---------------------------------------------------- |
| `timelineWidth`    | `number` | width of timeline|
| `visibleTimeStart` | `number` | unix milliseconds of start visible time |
| `visibleTimeEnd`    | `number` | unix milliseconds of end visible time|
| `canvasTimeStart` | `number` | unix milliseconds of start buffer time |
| `canvasTimeEnd`    | `number` |unix milliseconds of end buffer time|

###### Header context

| property           | type     | description                                          |
| ------------------ | -------- | ---------------------------------------------------- |
| `intervals`    | `array` | an array with all intervals|
| `unit` | `string` | unit passed or timelineUnit |

** `interval`: `[startTime: Moment, endTime: Moment]`

##### Prop getters functions

Rather than applying props on the element yourself and to avoid your props being overridden (or overriding the props returned). You can pass an object to the prop getters to avoid any problems. This object will only accept some properties that our component manage so the component make sure to combine them correctly.

| property         | type                 | description|
| ---------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `getRootProps`   | `function(props={})` | returns the props you should apply to the root div element.|
| `getIntervalProps`   | `function(props={})` | returns the props you should apply to the interval div element.|

* `getIntervalProps` The returned props are:

  * style: inline object style
  * onClick: event handler
  * key

  These properties can be extended using the prop argument with properties:

  * style: extra inline styles to be applied to the component
  * onClick: extra click handler added to the normal `showPeriod callback`

##### helpers:

| property         | type                 | description|
| ---------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `showPeriod`   | `function(props={})` | returns the props you should apply to the root div element.|

##### data:

pass through the `headerData` prop content

#### example

```jsx
import Timeline, {
  TimelineHeaders,
  SidebarHeader,
  DateHeader
} from 'react-calendar-timeline'

<Timeline>
  <TimelineHeaders>
    <SidebarHeader>
      {({ getRootProps }) => {
        return <div {...getRootProps()}>Left</div>
      }}
    </SidebarHeader>
    <DateHeader unit="primaryHeader" />
    <DateHeader />
    <CustomHeader height={50} headerData={{someData: 'data'}} unit="year">
      {({
        headerContext: { intervals },
        getRootProps,
        getIntervalProps,
        showPeriod,
        data,
      }) => {
        return (
          <div {...getRootProps()}>
            {intervals.map(interval => {
              const intervalStyle = {
                lineHeight: '30px',
                textAlign: 'center',
                borderLeft: '1px solid black',
                cursor: 'pointer',
                backgroundColor: 'Turquoise',
                color: 'white'
              }
              return (
                <div
                  onClick={() => {
                    showPeriod(interval.startTime, interval.endTime)
                  }}
                  {...getIntervalProps({
                    interval,
                    style: intervalStyle
                  })}
                >
                  <div className="sticky">
                    {interval.startTime.format('YYYY')}
                  </div>
                </div>
              )
            })}
          </div>
        )
      }}
    </CustomHeader>
  </TimelineHeaders>
</Timeline>
```

# FAQ

## 我的时间表没有样式

你需要包括`时间线.css`文件，通过静态文件引用或Web包样式表绑定。文件位于`lib/时间线.css`

## How can I have items with different colors?

Now you can use item renderer for rendering items with different colors [itemRenderer](https://github.com/namespace-ee/react-calendar-timeline#itemrenderer).
Please refer to [examples](https://github.com/namespace-ee/react-calendar-timeline/tree/master/examples#custom-item-rendering) for a sandbox example

现在可以使用项目渲染器渲染具有不同颜色的项目[itemRenderer](https://github.com/namespace-ee/react-calendar-timeline#itemrenderer)。请参考[examples](https://github.com/namespace-ee/react-calendar-timeline/tree/master/examples#custom-item-rendering)

## How can I add a sidebar on the right?

The library supports right sidebar.
![right sidebar demo](doc/right-sidebar.png)

To use it, you need to add a props to the `<Timeline />` component:

```jsx
rightSidebarWidth={150}
```

And add `rightTitle` prop to the groups objects:

```js
{
  id: 1,
  title: 'group 1',
  rightTitle: 'additional info about group 1'
}
```

如果使用自定义标题，则需要在“TimelineHeader”下添加“SidebarHeader”组件，并使用variant`right`

## The timeline header doesn't fix to the top of the container when I scroll down.

you need to add sticky to the header like [this example](https://github.com/FoothillSolutions/react-calendar-timeline/tree/dest-build/examples#sticky-header).

## I'm using Babel with Rollup or Webpack 2+ and I'm getting strange bugs with click events

These module bundlers don't use the transpiled (ES5) code of this module. They load the original ES2015+ source. Thus your babel configuration needs to match ours. We recommend adding the [`stage-0` preset](https://babeljs.io/docs/plugins/preset-stage-0/) to your `.babelrc` to make sure everything works as intended.

If that's too experimental, then the minimum you need is to add is the [`transform-class-properties`](https://babeljs.io/docs/plugins/transform-class-properties/) plugin that's in stage-2 and possibly the [`transform-object-rest-spread`](https://babeljs.io/docs/plugins/transform-object-rest-spread/) plugin from stage-3. However in this case it's easier to make sure you have at least [`stage-2`](https://babeljs.io/docs/plugins/preset-stage-2/) enabled.

See [issue 51](https://github.com/namespace-ee/react-calendar-timeline/issues/51) for more details.

Alternatively you may import the transpiled version of the timeline like this:

```js
// import Timeline from 'react-calendar-timeline'  // ESnext version
import Timeline from 'react-calendar-timeline/lib' // ES5 version
```

However doing so you lose on some of the features of webpack 2 and will potentially get a slightly larger bundle.

## It doesn't work with `create-react-app`

It's the same issue as above. See [issue 134](https://github.com/namespace-ee/react-calendar-timeline/issues/134#issuecomment-314215244) for details and options.

## What are the zIndex values for all the elements?

This is useful when using the plugins (that you pass as children to the component). Override the CSS to change:
这在使用插件（作为子级传递给组件）时非常有用。重写CSS以更改：

* Horizontal Lines: 30
* Vertical Lines: 40
* Items: 80-88 (depending on selection, dragging, etc)
* Header: 90

## Behind the scenes

The timeline is built with speed, usability and extensibility in mind.

Speed: The calendar itself is actually a 3x wide scrolling canvas of the screen. All scroll events left and right happen naturally, like scrolling any website. When the timeline has scrolled enough (50% of the invisible surface on one side), we change the "position:absolute;left:{num}px;" variables of each of the visible items and scroll the canvas back. When this happens, the `onBoundsChange` prop is called.

This results in a visually endless scrolling canvas with optimal performance.

Extensibility and usability: While some parameters (`onTimeChange`, `moveResizeValidator`) might be hard to configure, these are design decisions to make it as extensible as possible. If you have recipes for common tasks regarding those parameters, send a PR to add them to this doc.

## Interaction

To interact and navigate within the timeline there are the following options for the user:

```
shift + mousewheel = move timeline left/right
alt + mousewheel = zoom in/out
ctrl + mousewheel = zoom in/out 10× faster
meta + mousewheel = zoom in/out 3x faster (win or cmd + mousewheel)
```

Plus there is a handling for pinch-in and pinch-out zoom gestures (two touch points).
The pinch gesture on a trackpad (not a touch device) works in Chrome and Firefox (v55+) because these browsers map the gesture to `ctrl + mousewheel`.

# Contribute

If you like to improve React Calendar Timeline fork the repo and get started by running the following:

```bash
$ git clone https://github.com/namespace-ee/react-calendar-timeline.git react-calendar-timeline
$ cd react-calendar-timeline
$ yarn
$ yarn start
```

Check http://0.0.0.0:8888/ in your browser and have fun!

Please run `npm run lint` before you send a pull request. `npm run jest` runs the tests.

<!--

If you are core member team to patch npm run:

```bash
npm version patch
```

-->

## License
[MIT licensed](/LICENSE.md).
