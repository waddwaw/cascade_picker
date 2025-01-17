# cascade_picker

级联选择器

## Usage

#### 1\. Depend

Add this to you package's `pubspec.yaml` file:

```yaml
dependencies:
  cascade_picker: ^0.0.7
```

or

```yaml
cascade_picker:
    git:
      url: git://github.com/xionghaoo/cascade_picker.git
```

#### 2\. Install

Run command:

```bash
$ flutter packages get
```

#### 3\. Import

Import in Dart code:

```dart
import 'package:cascade_picker/cascade_picker.dart';
```

#### 4\. Show

选择项前面的图标位置：[***images/ic_select_mark.png***][3]

```dart
/// CascadePicker的page是ListView，没有约束的情况下它的高度是无限的，
/// 因此需要约束高度。
///
/// final _cascadeController = CascadeController();
///
/// initialPageData: 第一页的数据
/// nextPageData: 下一页的数据，点击当前页的选择项后调用该方法加载下一页
///   - pageCallback: 用于传递下一页的数据给CascadePicker
///   - currentPage: 当前是第几页
///   - selectIndex: 当前选中第几项
/// controller: 控制器，用于获取已选择的数据
/// maxPageNum: 最大页数
/// selectedIcon(可选): 已选中选项前面的图标，flutter package不能放本地资源文件，因此需要从外部传入，
/// 图标在images文件夹下面
///
/// Expand(
///   child: CascadePicker(
///     initialPageData: ['a', 'b', 'c', 'd'],
///     nextPageData: (pageCallback, currentPage, selectIndex) async {
///       pageCallback(['one', 'two', 'three'])
///     },
///     controller: _cascadeController,
///     maxPageNum: 4,
///     selectedIcon: Image.asset("images/ic_select_mark.png", width: 10, height: 10, color: Colors.redAccent,),
/// )
///
/// InkBox(
///   child: Container(...)
///   onTap: () {
///     /// 判断是否完成选择
///     if (_cascadeController.isCompleted()) {
///       List<String> selectedTitles = _cascadeController.selectedTitles;
///       List<int> selectedIndexes = _cascadeController.selectedIndexes;
///     }
///   }
/// )
```

## Demo代码
> 可以直接用[test_page.dart][4]进行测试

demo使用了以下树形结构数据：
```dart
class Item {
  String? name;
  String? code;
  String? fatherCode;
  String? remark;
  List<Item>? children;
}
```
需要自己提取数据中要显示的标题。页数从1开始，选中项从0开始。
```dart
nextPageData: (pageCallback, currentPage, selectIndex) async {
    if (currentPage == 1) {
        // 在第一页选中，返回第二页列表数据
        List<String>? nextPageData = items[selectIndex]
            .children?.map((e) => e.name!).toList();
        if (nextPageData != null) pageCallback(nextPageData);
    } else if (currentPage == 2) {
        // 在第二页选中，返回第三页列表数据
        // 先获取已选中的序号
        List<int> selectedIndexes = _cascadeController.selectedIndexes;
        // 根据已选中的序号在items中获取下一级页面的列表数据
        List<String>? nextPageData = items[selectedIndexes[0]]
            .children?[selectIndex]
            .children?.map((e) => e.name!).toList();
        if (nextPageData != null) pageCallback(nextPageData);
    }
},
```

<img src="https://github.com/xionghaoo/cascade_picker/blob/master/demo/demo.jpeg" width="400"/><br/>

## 效果

| 1 | 2 |
| --- | --- |
| ![Demo 1][1] | ![demo 2][2]

[1]:https://github.com/xionghaoo/assets/blob/master/cascade_picker_1.png?raw=true
[2]:https://github.com/xionghaoo/assets/blob/master/cascade_picker_2.gif?raw=true
[3]:https://github.com/xionghaoo/cascade_picker/blob/master/images/ic_select_mark.png?raw=true
[4]:https://github.com/xionghaoo/cascade_picker/blob/master/demo/test_page.dart
[5]:https://github.com/xionghaoo/cascade_picker/blob/master/demo/demo.jpeg?raw=true