# hot-calendar
- 本人开发的一款uni-app前端插件
- https://ext.dcloud.net.cn/plugin?id=10642

## 插件属性:
|  属性名  |  类型  |  默认值  |  说明  |
|  ----  | ----  |  ----  | ----  |
|  title  |  String  |  ''  |  热力图标题，默认显示最新年份  |
|  cellWidth  |  String  |  '1.64em'  |  支持px，em，rpx，%，正方形单元格的边长，需要扣除2px cell border  |
|  cellRadius  |  String  |  '0'  |  支持px，em，rpx，单元格的圆角  |
|  cellMargin  |  String  |  '0.21em'  |  支持px，em，rpx，%，单元格纵向间距，需要扣除2px cell border  |
|  cellData  |  Array  |  []  |  必填，单元格的数量值，着色的重要指标，默认0为空白格，100为无色不可点击格  |
|  showDay  |  Boolean  |  false  |  日历是否显示日期  |
|  showEarly  |  Boolean  |  false  |  早于cellData的日历是否显示  |
|  level1  |  Number  |  1  |  着色等级，最低级  |
|  level2  |  Number  |  3  |  着色等级  |
|  level3  |  Number  |  6  |  着色等级  |
|  level4  |  Number  |  10  |  着色等级，最高级  |
|  levelUnlimit  |  Number  |  100  |  cellData里用于设置无色不可点击格的参数  |
|  pointDay  |  String  |  ''  |  格式为'Y-m-d'，着重框所在的日期，默认是当天  |

## 插件事件：
|  事件名  |  说明  |  返回值  |
|  ----  | ----  |  ----  |
|  @cellTap  |  单元格点击事件  |  String，格式为'Y-m-d'，单元格日期  |

## 示例：
```
<hot-calendar cellWidth="46rpx" cellMargin="6rpx" :cellData="hotCalendarArr" :showDay="true"
					:showEarly="true" :pointDay="dateStr" @cellTap="hotCalendarClick"></hot-calendar>
```

## cellData格式样例：
```
[
	[[0],[0],[0],[0],[1],[0],[0]],
	[[0],[1],[0],[0],[1],[1],[0]],
	[[0],[0],[0],[0],[1],[1],[1]],
	[[0],[1],[1],[1],[1],[1],[0]],
	[[0],[0],[0],[0],[0],[0],[0]],
	[[0],[1],[1],[1],[1],[0],[0]],
	[[0],[0],[0],[0],[0],[0],[0]],
	[[0],[1],[1],[1],[1],[100],[100]]
]
```
          
## 其它说明：
- 本插件可搭配scroll-view，实现左右灵活拖动
- 欢迎大家下载使用，多提宝贵意见和建议！
- 如遇问题，请加qq 63846152
